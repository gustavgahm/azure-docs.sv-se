---
title: Hantera VM-diskar i Azure Stack | Microsoft Docs
description: Skapa diskar för virtuella datorer i Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/18/2019
ms.author: mabrigg
ms.reviewer: jiahan
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 022dd937ae0557ca848a329039703f3b94ad0885
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245362"
---
# <a name="create-virtual-machine-disk-storage-in-azure-stack"></a>Skapa disklagring för virtuella datorer i Azure Stack

*Gäller för: Integrerade Azure Stack-system och Azure Stack Development Kit*

Den här artikeln beskriver hur du skapar disklagring för virtuella datorer med hjälp av Azure Stack-portalen eller med hjälp av PowerShell.

## <a name="overview"></a>Översikt

Från och med version 1808, stöder Azure Stack användning av hanterade diskar och ohanterade diskar på virtuella datorer som både ett operativsystem (OS) och en datadisk. Före version 1808 stöds endast ohanterade diskar. 

**[Hanterade diskar](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#managed-disks)**  förenkla Diskhantering för virtuella Azure IaaS-datorer genom att hantera de lagringskonton som är associerade med de Virtuella diskarna. Du behöver bara ange storleken på disk och Azure Stack skapar och hanterar disken åt dig.

**[Ohanterade diskar](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks)**, kräver att du skapar en [lagringskonto](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) att lagra diskar. Diskar som du skapar kallas VM-diskar och lagras i behållare i lagringskontot.

### <a name="best-practice-guidelines"></a>Riktlinjer för bästa praxis

Om du vill förbättra prestanda och minska övergripande kostnader, rekommenderar vi att du placerar varje VM-disk i en separat behållare. En behållare ska innehålla en OS-disk eller en datadisk, men inte båda på samma gång. (Men det finns inget att hindra dig från att sätta båda typerna av disk i samma behållare.)

Om du lägger till en eller flera datadiskar till en virtuell dator kan du använda ytterligare behållare som en plats för att lagra dessa diskar. OS-disken för ytterligare virtuella datorer bör även finnas i sina egna behållare.

När du skapar virtuella datorer kan återanvända du samma lagringskonto för varje ny virtuell dator. Endast de behållare som du skapar måste vara unika.

### <a name="adding-new-disks"></a>Att lägga till nya diskar

I följande tabell sammanfattas hur du lägger till diskar med hjälp av portalen och med hjälp av PowerShell.

| Metod | Alternativ
|-|-|
|[Användarportalen](#use-the-portal-to-add-additional-disks-to-a-vm)|-Lägga till nya datadiskar till en befintlig virtuell dator. Nya diskar skapas med Azure Stack. </br> </br>-Lägga till en befintlig (.vhd)-diskfil till en tidigare skapad virtuell dator. Om du vill göra detta måste du förbereda VHD-filen och sedan överföra filen till Azure Stack. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | – Skapa en ny virtuell dator med en OS-disk och lägga till en eller flera datadiskar i den virtuella datorn på samma gång. |

## <a name="use-the-portal-to-add-disks-to-a-vm"></a>Använda portalen för att lägga till diskar till en virtuell dator

Som standard när du använder portalen för att skapa en virtuell dator för de flesta marketplace-objekt skapas endast OS-disken.

När du har skapat en virtuell dator kan du använda portalen för att:
* Skapa en ny datadisk och koppla den till den virtuella datorn.
* Ladda upp en befintlig datadisk och koppla den till den virtuella datorn.

Varje ohanterad disk som du lägger till ska placeras i en separat behållare.

>[!NOTE]  
>Diskar skapas och hanteras av Azure kallas [hanterade diskar](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).

### <a name="use-the-portal-to-create-and-attach-a-new-data-disk"></a>Använda portalen för att skapa och koppla en ny datadisk

1.  I portalen, väljer **alla tjänster** > **virtuella datorer**.    
    ![Exempel: VM-instrumentpanelen](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Välj en virtuell dator som tidigare har skapats.   
    ![Exempel: Välj en virtuell dator i instrumentpanelen](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Den virtuella datorn, Välj **diskar** > **Lägg till datadisk**.       
    ![Exempel: Koppla en ny disk till den virtuella datorn](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  För datadisk:
    -  Ange den **LUN**. Den logiska enheten måste vara ett giltigt tal.
    -  Välj **skapa disk**.
    ![Exempel: Koppla en ny disk till den virtuella datorn](media/azure-stack-manage-vm-disks/add-a-data-disk-create-disk.png)

5.  I avsnittet Skapa managed disk-bladet:
    -  Ange den **namn** på disken.
    -  Välj en befintlig eller skapa en ny **resursgrupp**.
    -  Välj den **plats**. Platsen är som standard samma behållare som innehåller OS-disken.
    -  Välj den **kontotyp**. 
        ![Exempel: Koppla en ny disk till den virtuella datorn](media/azure-stack-manage-vm-disks/create-manage-disk.png)

        **Premium SSD**  
        Premium-diskar (SSD) är kompletterade med solid state-hårddiskar och ger prestanda med låg fördröjning. De ger bästa balans mellan pris och prestanda och lämpar sig för I/O-intensiva program och produktionsarbetsbelastningar.
       
        **Standard HDD**  
        Standarddiskar (HHD) är kompletterade med magnetiska enheter och fungerar bättre för program där data används sällan. Zonen redundant diskar backas upp av zonredundant lagring (ZRS) som replikerar data över flera zoner och är tillgängliga även om en enstaka zon ligger nere. 

    -  Välj den **källtyp**.

       Skapa en disk från en ögonblicksbild av en annan disk, en blob i ett lagringskonto eller skapa en tom disk.

        **ögonblicksbild**  
        Välj en ögonblicksbild, om den är tillgänglig. Ögonblicksbilden måste vara i tillgängliga i Virtuellt datorns prenumeration och plats.

        **Lagringsblob**  
        - Lägg till URI för blob Storage som innehåller disk image.  
        - Välj **Bläddra** att öppna bladet Storage-konton. Instruktioner finns i [lägga till en datadisk från ett lagringskonto](#add-a-data-disk-from-a-storage-account).
        - Välj OS-typ av avbildningen, antingen **Windows**, **Linux**, eller **inget (datadisk)**.

        **Inget (tom disk)**

    -  Välj den **storlek (GiB)**.

       Standard diskkostnader ökar beroende på storleken på disken. Premium disk kostnader och prestanda ökar beroende på storleken på disken. Mer information finns i [priser för Managed Disks](https://go.microsoft.com/fwlink/?linkid=843142).

    -  Välj **Skapa**. Azure Stack skapar och validerar den hantera disken.

5.  När Azure Stack skapar disken och kopplar den till den virtuella datorn, den nya disken visas i den virtuella datorns diskinställningarna under **DATADISKAR**.   

    ![Exempel: Visa disk](media/azure-stack-manage-vm-disks/view-data-disk.png)

### <a name="add-a-data-disk-from-a-storage-account"></a>Lägga till en datadisk från ett lagringskonto

Mer information om hur du arbetar med lagringskonton i Azure Stack finns i [introduktion till Azure Stack storage](azure-stack-storage-overview.md).

1. Välj den **lagringskonto** att använda.
2. Välj den **behållare** där du vill att datadisken. Från den **behållare** bladet kan du skapa en ny behållare om du vill. Du kan sedan ändra platsen för den nya disken till en egen behållare. När du använder en separat behållare för varje disk kan distribuera du placeringen av datadisken som kan förbättra prestanda.
3. Välj **Välj** Spara valet.

    ![Exempel: Välj en container](media/azure-stack-manage-vm-disks/select-container.png)

## <a name="attach-an-existing-data-disk-to-a-vm"></a>Koppla en befintlig datadisk till en virtuell dator

1.  [Förbereda en VHD-fil](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) för användning som en datadisk för en virtuell dator. Ladda upp den VHD-filen till ett lagringskonto som du använder med den virtuella datorn som du vill bifoga VHD-filen till.

    Planera att använda en annan behållare för VHD-filen än den behållare som innehåller OS-disken.   
    ![Exempel: Ladda upp en VHD-fil](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  När VHD-filen har överförts, är du redo att koppla den virtuella Hårddisken till en virtuell dator. I menyn till vänster väljer **virtuella datorer**.  
 ![Exempel: Välj en virtuell dator i instrumentpanelen](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Välj den virtuella datorn i listan.

    ![Exempel: Välj en virtuell dator i instrumentpanelen](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  På sidan för den virtuella datorn, väljer **diskar** > **bifoga befintlig**.   

    ![Exempel: Ansluta en befintlig disk](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  I den **bifoga befintlig disk** väljer **VHD-filen**. Den **lagringskonton** öppnas.    

    ![Exempel: Välj en VHD-fil](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  Under **lagringskonton**, Välj kontot som ska användas och välj sedan en behållare som innehåller VHD-filen som du tidigare har laddats upp. Välj den VHD-filen och välj sedan **Välj** Spara valet.    

    ![Exempel: Välj en container](media/azure-stack-manage-vm-disks/select-container2.png)

7.  Under **bifoga befintlig disk**, den markerade filen finns under **VHD-filen**. Uppdatera den **vara värd för cachelagring** inställning på disken och välj sedan **OK** att spara den nya diskkonfigurationen för den virtuella datorn.    

    ![Exempel: Bifoga VHD-filen](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  När Azure Stack skapar disken och kopplar den till den virtuella datorn, den nya disken visas i den virtuella datorns diskinställningarna under **Datadiskar**.   

    ![Exempel: Fullständig bifoga disken](media/azure-stack-manage-vm-disks/complete-disk-attach.png)

## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>Använd PowerShell för att lägga till flera ohanterade diskar till en virtuell dator

Du kan använda PowerShell för att etablera en virtuell dator och lägga till en ny datadisk eller bifoga en befintlig **VHD** filen som en datadisk.

Den **Add-AzureRmVMDataDisk** cmdlet lägger till en datadisk till en virtuell dator. Du kan lägga till en datadisk när du skapar en virtuell dator eller du kan lägga till en datadisk till en befintlig virtuell dator. Ange den **VhdUri** parametern för att distribuera diskarna till olika behållare.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Lägga till datadiskar i en ny virtuell dator
I följande exempel används PowerShell-kommandon för att skapa en virtuell dator med tre datadiskarna, var och en placeras i en annan behållare.

Det första kommandot skapar en VM-objekt och lagrar den i den *$VirtualMachine* variabeln. Kommandot tilldelar namn och storlek för den virtuella datorn.

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                    -VMSize "Standard_A2"
```

Följande tre kommandon tilldelar sökvägar för tre datadiskarna till de *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, och *$DataDiskVhdUri03* variabler. Definiera en annan sökväg i URL: en för att distribuera diskarna till olika behållare.

```powershell
$DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
```

```powershell
$DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
```

```powershell
$DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
```

De sista tre kommandona lägga till datadiskar i den virtuella datorn lagras i *$VirtualMachine*. Varje kommando anger namn, plats och ytterligare egenskaper för disken. URI för varje disk lagras i *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, och *$DataDiskVhdUri03*.

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk1' `
                -Caching 'ReadOnly' -DiskSizeInGB 10 -Lun 0 `
                -VhdUri $DataDiskVhdUri01 -CreateOption Empty
```

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk2' `
                -Caching 'ReadOnly' -DiskSizeInGB 11 -Lun 1 `
                -VhdUri $DataDiskVhdUri02 -CreateOption Empty
```

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk3' `
                -Caching 'ReadOnly' -DiskSizeInGB 12 -Lun 2 `
                -VhdUri $DataDiskVhdUri03 -CreateOption Empty
```

Använd följande PowerShell-kommandon för att lägga till Operativsystemets disk- och konfiguration till den virtuella datorn och starta sedan den nya virtuella datorn.

```powershell
#set variables
$rgName = "myResourceGroup"
$location = "local"
#Set OS Disk
$osDiskUri = "https://contoso.blob.local.azurestack.external/vhds/osDisk.vhd"
$osDiskName = "osDisk"

$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $osDiskName -VhdUri $osDiskUri `
    -CreateOption FromImage -Windows

# Create a subnet configuration
$subnetName = "mySubNet"
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24

# Create a vnet configuration
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet

# Create a public IP
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
    -AllocationMethod Dynamic

# Create a network security group configuration
$nsgName = "myNsg"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule

# Create a NIC configuration
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
-Location $location -SubnetId $vnet.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id -PublicIpAddressId $pip.Id

#Create the new VM
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName VirtualMachine | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $VirtualMachine
```

### <a name="add-data-disks-to-an-existing-virtual-machine"></a>Lägga till datadiskar i en befintlig virtuell dator

I följande exempel används PowerShell-kommandon för att lägga till tre datadiskar till en befintlig virtuell dator.
Det första kommandot hämtar den virtuella datorn med namnet VirtualMachine med hjälp av den **Get-AzureRmVM** cmdlet. Kommandot lagrar den virtuella datorn i den *$VirtualMachine* variabeln.

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                -Name "VirtualMachine"
```
Följande tre kommandon tilldelar sökvägar för tre datadiskarna till $DataDiskVhdUri01 och $DataDiskVhdUri02 $DataDiskVhdUri03 variabler.  Olika sökvägar i vhduri visar olika behållare för disk-placering.
```powershell
$DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
```
```powershell
$DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
```
```powershell
$DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
```


Följande tre kommandon lägga till datadiskar i den virtuella datorn lagras i den *$VirtualMachine* variabeln. Varje kommando anger namn, plats och ytterligare egenskaper för disken. URI för varje disk lagras i *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, och *$DataDiskVhdUri03*.

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" `
                      -VhdUri $DataDiskVhdUri01 -LUN 0 `
                      -Caching ReadOnly -DiskSizeinGB 10 -CreateOption Empty
```

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk2" `
                      -VhdUri $DataDiskVhdUri02 -LUN 1 `
                      -Caching ReadOnly -DiskSizeinGB 11 -CreateOption Empty
```

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk3" `
                      -VhdUri $DataDiskVhdUri03 -LUN 2 `
                      -Caching ReadOnly -DiskSizeinGB 12 -CreateOption Empty
```

Det slutliga kommandot uppdaterar tillståndet för den virtuella datorn lagras i *$VirtualMachine* i -*ResourceGroupName*.

```powershell
Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

## <a name="next-steps"></a>Nästa steg

Läs mer om Azure Stack-datorer i [överväganden för virtuella datorer i Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
