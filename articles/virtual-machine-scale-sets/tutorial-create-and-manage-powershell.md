---
title: Självstudie – Skapa och hantera en VM-skalningsuppsättning i Azure | Microsoft Docs
description: Läs hur du använder Azure PowerShell för att skapa en VM-skalningsuppsättning, tillsammans med vissa vanliga hanteringsuppgifter, till exempel att starta och stoppa en instans, eller ändra kapaciteten för en skalningsuppsättning.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/18/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 0aa4b8fd606c45f2dea702140c34fc93bcd4c5a4
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54885379"
---
# <a name="tutorial-create-and-manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Självstudier: Skapa och hantera en VM-skalningsuppsättning med Azure PowerShell
Med en VM-skalningsuppsättning kan du distribuera och hantera en uppsättning identiska, virtuella datorer med automatisk skalning. Under livscykeln för en VM-skalningsuppsättning kan du behöva köra en eller flera hanteringsuppgifter. I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa och anslut till en VM-skalningsuppsättning
> * Välja och använda VM-avbildningar
> * Visa och använd specifika virtuella datorstorlekar
> * Skala en skalningsuppsättning automatiskt
> * Utför vanliga hanteringsåtgärder för skalningsuppsättningar

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Om du väljer att installera och använda PowerShell lokalt krävs version 6.0.0 eller senare av Azure PowerShell-modulen i den här självstudiekursen. Kör `Get-Module -ListAvailable AzureRM` för att hitta versionen. Om du behöver uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/azurerm/install-azurerm-ps) (Installera Azure PowerShell-modul). Om du kör PowerShell lokalt måste du också köra `Connect-AzureRmAccount` för att skapa en anslutning till Azure. 


## <a name="create-a-resource-group"></a>Skapa en resursgrupp
En Azure-resursgrupp är en logisk container där Azure-resurser distribueras och hanteras. En resursgrupp måste skapas före en VM-skalningsuppsättning. Skapa en resursgrupp med kommandot [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). I det här exemplet skapas en resursgrupp med namnet *myResourceGroup* i regionen *EastUS*. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Location "EastUS"
```
Resursgruppens namn anges när du skapar eller ändrar en skalningsuppsättning under den här självstudien.


## <a name="create-a-scale-set"></a>Skapa en skalningsuppsättning
Först anger du ett administratörsanvändarnamn och lösenord för virtuella datorer med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Skapa nu en skalningsuppsättning för en virtuell dator med [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss). För att distribuera trafik till flera virtuella datorinstanser så skapas även en lastbalanserare. Lastbalanseraren innehåller regler för att distribuera trafik på TCP-port 80 och för att tillåta trafik för fjärrskrivbordet på TCP-port 3389 och PowerShell-fjärrkommunikation på TCP-port 5985:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -Location "EastUS" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -Credential $cred
```

Det tar några minuter att skapa och konfigurera alla skalningsuppsättningsresurser och virtuella datorinstanser.


## <a name="view-the-vm-instances-in-a-scale-set"></a>Visa de virtuella datorinstanserna i en skalningsuppsättning
Om du vill visa en lista med virtuella datorinstanser i en skalningsuppsättning, använder du [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) på följande sätt:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Följande exempelutdata visar två virtuella datorinstanser i skalningsuppsättningen:

```powershell
ResourceGroupName         Name Location             Sku InstanceID ProvisioningState
-----------------         ---- --------             --- ---------- -----------------
MYRESOURCEGROUP   myScaleSet_0   eastus Standard_DS1_v2          0         Succeeded
MYRESOURCEGROUP   myScaleSet_1   eastus Standard_DS1_v2          1         Succeeded
```

Om du vill visa mer information om en specifik VM-instans, lägger du till parametern `-InstanceId` till [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). Följande exempel visar information om den virtuella datorinstansen *1*:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


## <a name="list-connection-information"></a>Lista anslutningsinformation
En offentlig IP-adress är tilldelad till lastbalanseraren som dirigerar trafik till de enskilda virtuella datorinstanserna. Som standard läggs NAT (Network Address Translation)-regler till Azure-lastbalanseraren som vidarebefordrar fjärranslutningstrafik till varje virtuell dator på en viss port. Om du vill ansluta till de virtuella datorinstanserna i en skalningsuppsättning, kan du skapa en fjärranslutning till en tilldelad offentlig IP-adress och portnummer.

Om du vill visa en lista med NAT-portar för anslutning till VM-instanser i en skalningsuppsättning, hämta först lastbalanseringsobjektet med [Get-AzureRmLoadBalancer](/powershell/module/AzureRM.Network/Get-AzureRmLoadBalancer). Visa sedan de ingående NAT-reglerna med [Get-AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/AzureRM.Network/Get-AzureRmLoadBalancerInboundNatRuleConfig):

```azurepowershell-interactive
# Get the load balancer object
$lb = Get-AzureRmLoadBalancer -ResourceGroupName "myResourceGroup" -Name "myLoadBalancer"

# View the list of inbound NAT rules
Get-AzureRmLoadBalancerInboundNatRuleConfig -LoadBalancer $lb | Select-Object Name,Protocol,FrontEndPort,BackEndPort
```

Följande exempelutdata visar instansnamnet, den offentliga IP-adressen för lastbalanseraren och portnummer som NAT-regler vidarebefordrar trafik till:

```powershell
Name             Protocol FrontendPort BackendPort
----             -------- ------------ -----------
myScaleSet3389.0 Tcp             50001        3389
myScaleSet5985.0 Tcp             51001        5985
myScaleSet3389.1 Tcp             50002        3389
myScaleSet5985.1 Tcp             51002        5985
```

*Namnet* på regeln överensstämmer med namnet på den virtuella datorinstansen som det visas i ett tidigare [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm)-kommando. Till exempel för att ansluta till den virtuella datorinstansen *0*, använder du *myScaleSet3389.0* och ansluter till porten *50001*. För att ansluta till den virtuella datorinstansen *1*, använder du värdet från *myScaleSet3389.1* och ansluter till porten *50002*. Om du vill använda PowerShell-fjärrkommunikation, ansluter du till lämplig virtuell datorinstansregel för *TCP* port *5985*.

Visa den offentliga IP-adressen för lastbalanseraren med [Get-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/Get-AzureRmPublicIpAddress):

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" -Name "myPublicIPAddress" | Select IpAddress
```

Exempel på utdata:

```powershell
IpAddress
---------
52.168.121.216
```

Skapa en fjärranslutning till din första virtuella datorinstans. Ange din offentliga IP-adress och portnummer för nödvändiga virtuella datorinstanser som de visas i föregående kommandon. När du uppmanas, anger du de autentiseringsuppgifter som användes när du skapade skalningsuppsättningen (som standard i exempelkommandona är de *azureuser* och *P@ssw0rd!*). Om du använder Azure Cloud Shell, utför du den här åtgärden från en lokal PowerShell-kommandotolk eller klienten för fjärrskrivbord. Följande exempel ansluter till den virtuella datorinstansen *1*:

```powershell
mstsc /v 52.168.121.216:50001
```

Efter att du loggat in på den virtuella datorinstansen kan du utföra vissa manuella konfigurationsändringar efter behov. Stäng fjärranslutningen för tillfället.


## <a name="understand-vm-instance-images"></a>Förstå avbildningar av virtuella datorinstanser
Azures marknadsplats inkluderar flera avbildningar som kan användas för att skapa virtuella datorinstanser. Använd kommandot [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) för att få en lista med tillgängliga utgivare.

```azurepowershell-interactive
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Du kan visa en lista över avbildningar för en viss utgivare med [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku). Listan med avbildningar kan även filtreras efter `-PublisherName` eller `–Offer`. I följande exempel filtreras listan för alla avbildningar med utgivarnamnet *MicrosoftWindowsServer* och ett erbjudande som matchar *WindowsServer*:

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

Följande exempelutdata visar alla tillgängliga Windows Server-avbildningar:

```powershell
Skus                                  Offer         PublisherName          Location
----                                  -----         -------------          --------
2008-R2-SP1                           WindowsServer MicrosoftWindowsServer eastus
2008-R2-SP1-smalldisk                 WindowsServer MicrosoftWindowsServer eastus
2012-Datacenter                       WindowsServer MicrosoftWindowsServer eastus
2012-Datacenter-smalldisk             WindowsServer MicrosoftWindowsServer eastus
2012-R2-Datacenter                    WindowsServer MicrosoftWindowsServer eastus
2012-R2-Datacenter-smalldisk          WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter                       WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-Server-Core           WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-Server-Core-smalldisk WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-smalldisk             WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-with-Containers       WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-with-RDSH             WindowsServer MicrosoftWindowsServer eastus
2016-Nano-Server                      WindowsServer MicrosoftWindowsServer eastus
```

När du skapade en skalningsuppsättning i början av självstudien, angavs en standard virtuell datoravbildning på *Windows Server 2016 DataCenter* för de virtuella datorinstanserna. Du kan ange en annan virtuell datorinstans baserat på utdata från [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku). Följande exempel skapar en skalningsuppsättning som anges med parametern `-ImageName` för att ange en virtuell datoravbildning på *MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest*. Eftersom det tar några minuter att skapa och konfigurera alla skalningsuppsättningsresurser och virtuella datorinstanser så behöver du inte distribuera följande skalningsuppsättning:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup2" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet2" `
  -VirtualNetworkName "myVnet2" `
  -SubnetName "mySubnet2" `
  -PublicIpAddressName "myPublicIPAddress2" `
  -LoadBalancerName "myLoadBalancer2" `
  -UpgradePolicyMode "Automatic" `
  -ImageName "MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest" `
  -Credential $cred
```


## <a name="understand-vm-instance-sizes"></a>Förstå storlekar för virtuella datorinstanser
Storleken på en virtuell datorinstans, eller *SKU*, fastställer mängden beräkningsresurser som CPU, GPU och minne som görs tillgängliga för den virtuella datorinstansen. Virtuella datorinstanser i en skalningsuppsättning måste ha lämplig storlek för förväntad arbetsbelastning.

### <a name="vm-instance-sizes"></a>Storlekar på virtuella datorinstanser
Följande tabell kategoriserar vanliga virtuella datorstorlekar i användningsfall.

| Typ                     | Normala storlekar           |    Beskrivning       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Generellt syfte](../virtual-machines/windows/sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Balanserat förhållande mellan processor och minne. Perfekt för utveckling eller test samt små till medelstora lösningar för program och data.  |
| [Beräkningsoptimerad](../virtual-machines/windows/sizes-compute.md)   | Fs, F             | Högt förhållande mellan processor och minne. Bra för program med medelhög trafik, nätverkstillämpningar och batchprocesser.        |
| [Minnesoptimerad](../virtual-machines/windows/sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Högt förhållande mellan minne och kärna. Utmärkt för relationsdatabaser, mellanstora till stora cacheminnen och minnesinterna analyser.                 |
| [Lagringsoptimerad](../virtual-machines/windows/sizes-storage.md)      | Ls                | Högt diskgenomflöde och I/O. Perfekt för stordata, SQL- och NoSQL-databaser.                                                         |
| [GPU](../virtual-machines/windows/sizes-gpu.md)          | NV, NC            | Virtuella specialdatorer som är avsedda för tung grafisk rendering och videoredigering.       |
| [Höga prestanda](../virtual-machines/windows/sizes-hpc.md) | H, A8-11          | Virtuella datorer med de kraftfullaste processorerna och nätverksgränssnitt för stora dataflöden (RDMA). 

### <a name="find-available-vm-instance-sizes"></a>Hitta tillgängliga storlekar för virtuella datorinstanser
Om du vill se en lista med storlekar på virtuella datorinstanser som är tillgängliga i en viss region kan du använda kommandot [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize). 

```azurepowershell-interactive
Get-AzureRmVMSize -Location "EastUS"
```

Utdata liknar följande komprimerade exempel som visar resurserna som tilldelats varje virtuell datorstorlek:

```powershell
Name                   NumberOfCores MemoryInMB MaxDataDiskCount OSDiskSizeInMB ResourceDiskSizeInMB
----                   ------------- ---------- ---------------- -------------- --------------------
Standard_DS1_v2                    1       3584                4        1047552                 7168
Standard_DS2_v2                    2       7168                8        1047552                14336
[...]
Standard_A0                        1        768                1        1047552                20480
Standard_A1                        1       1792                2        1047552                71680
[...]
Standard_F1                        1       2048                4        1047552                16384
Standard_F2                        2       4096                8        1047552                32768
[...]
Standard_NV6                       6      57344               24        1047552               389120
Standard_NV12                     12     114688               48        1047552               696320
```

När du skapade en skalningsuppsättning i början av självstudien angavs en standard-VM-SKU på *Standard_DS1_v2* för de virtuella datorinstanserna. Du kan ange en annan virtuell datorinstans-storlek baserat på utdata från [Get-AzurerRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize). Följande exempel skapar en skalningsuppsättning med parametern `-VmSize` för att ange en virtuell datorinstans-storlek på *Standard_F1*. Eftersom det tar några minuter att skapa och konfigurera alla skalningsuppsättningsresurser och virtuella datorinstanser så behöver du inte distribuera följande skalningsuppsättning:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup3" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet3" `
  -VirtualNetworkName "myVnet3" `
  -SubnetName "mySubnet3" `
  -PublicIpAddressName "myPublicIPAddress3" `
  -LoadBalancerName "myLoadBalancer3" `
  -UpgradePolicyMode "Automatic" `
  -VmSize "Standard_F1" `
  -Credential $cred
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ändra kapaciteten för en skalningsuppsättning
När du har skapat en skalningsuppsättning, begärde du två virtuella datorinstanser. Om du vill öka eller minska antalet virtuella datorinstanser i din befintliga skalningsuppsättning, kan du manuellt ändra kapaciteten. Skalningsuppsättningen skapar eller tar bort antalet virtuella datorinstanser som krävs och konfigurerar sedan lastbalanseraren att distribuera trafiken.

Skapa först ett skalningsuppsättningsobjekt med [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) och ange sedan ett nytt värde för `sku.capacity`. Om du vill tillämpa kapacitetsändringen, använder du [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). Följande exempel anger antalet virtuella datorinstanser i din skalningsuppsättning till *3*:

```azurepowershell-interactive
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 3
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss 
```

Det tar några minuter att uppdatera kapaciteten för din skalningsuppsättning. Om du vill se antalet instanser som du har i skalningsuppsättningen för tillfället, använder du [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

```azurepowershell-interactive
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Följande exempelutdata visar att kapaciteten för skalningsuppsättningen nu är *3*:

```powershell
Sku        :
  Name     : Standard_DS2
  Tier     : Standard
  Capacity : 3
```


## <a name="common-management-tasks"></a>Vanliga administrativa uppgifter
Du kan nu skapa en skalningsuppsättning, lista anslutningsinformationen och ansluta till virtuella datorinstanser. Du har lärt dig hur du kan använda en annan OS-avbildning för dina virtuella datorinstanser, välja en annan virtuell datorstorlek eller manuellt skala antalet instanser manuellt. Som en del av den dagliga hanteringen, kan du behöva stoppa, starta eller starta om de virtuella datorinstanserna i en skalningsuppsättning.

### <a name="stop-and-deallocate-vm-instances-in-a-scale-set"></a>Stoppa och frigöra virtuella datorinstanser i en skalningsuppsättning
Om du vill stoppa en eller flera virtuella datorer i en skalningsuppsättning, använder du [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss). Parametern `-InstanceId` låter dig ange en eller flera virtuella datorer att stoppa. Om du inte anger ett instans-ID, stoppas alla virtuella datorer i skalningsuppsättningen. Följande exempel stoppar instansen *1*:

```azurepowershell-interactive
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```

Som standard frigörs stoppade virtuella datorer och uppbär inga beräkningskostnader. Om du vill att den virtuella datorn är kvar i etablerat tillstånd när den stoppats, lägger du till parametern `-StayProvisioned` till det föregående kommandot. Stoppade virtuella datorer som fortsätter att vara etablerade, kostar vanliga beräkningsavgifter.

### <a name="start-vm-instances-in-a-scale-set"></a>Starta virtuella datorinstanser i en skalningsuppsättning
Om du vill starta en eller flera virtuella datorer i en skalningsuppsättning, använder du [Start-AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss). Parametern `-InstanceId` låter dig ange en eller flera virtuella datorer att starta. Om du inte anger ett instans-ID, startas alla virtuella datorer i skalningsuppsättningen. Följande exempel startar instansen *1*:

```azurepowershell-interactive
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```

### <a name="restart-vm-instances-in-a-scale-set"></a>Starta om virtuella datorinstanser i en skalningsuppsättning
Om du vill starta om en eller flera virtuella datorer i en skalningsuppsättning, använder du [Restart-AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss). Parametern `-InstanceId` låter dig ange en eller flera virtuella datorer att starta om. Om du inte anger ett instans-ID, startas alla virtuella datorer i skalningsuppsättningen om. Följande exempel startar om instansen *1*:

```azurepowershell-interactive
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


## <a name="clean-up-resources"></a>Rensa resurser
Om en resursgrupp tas bort, tas även alla resurser som ingår i gruppen bort, som de virtuella datorinstanserna, det virtuella nätverket och diskarna tas också bort. Parametern `-Force` bekräftar att du vill ta bort resurserna utan att tillfrågas ytterligare en gång. Parametern `-AsJob` återför kontrollen till kommandotolken utan att vänta på att uppgiften slutförs.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Nästa steg
I den här självstudien fick du lära dig hur du utför vissa grundläggande skapande och hanteringsåtgärder för skalningsuppsättningar med Azure PowerShell:

> [!div class="checklist"]
> * Skapa och anslut till en VM-skalningsuppsättning
> * Välja och använda VM-avbildningar
> * Visa och använda specifika VM-storlekar
> * Skala en skalningsuppsättning automatiskt
> * Utför vanliga hanteringsåtgärder för skalningsuppsättningar

Gå vidare till nästa självstudie om du vill lära dig mer om diskar i skalningsuppsättningar.

> [!div class="nextstepaction"]
> [Använda datadiskar med skalningsuppsättningar](tutorial-use-disks-powershell.md)
