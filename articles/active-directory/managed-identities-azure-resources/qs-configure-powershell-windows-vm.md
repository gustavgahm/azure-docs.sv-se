---
title: Hur du konfigurerar hanterade identiteter för Azure-resurser på en Azure-dator med hjälp av PowerShell
description: Steg för steg-instruktioner för att konfigurera hanterade identiteter för Azure-resurser på en Azure-dator med hjälp av PowerShell.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: priyamo
ms.openlocfilehash: a92e543ea8a633d4d7dfd1b276fadc0224696153
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169985"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>Konfigurera hanterade identiteter för Azure-resurser på en Azure-dator med hjälp av PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Hanterade identiteter för Azure-resurser tillhandahåller Azure-tjänster med en automatiskt hanterad identitet i Azure Active Directory. Du kan använda den här identiteten för att autentisera till en tjänst som stöder Azure AD-autentisering utan autentiseringsuppgifter i din kod. 

I den här artikeln med hjälp av PowerShell, du lära dig hur du utför följande hanterade identiteter för Azure-resurser på en Azure VM.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Förutsättningar

- Om du är bekant med hanterade identiteter för Azure-resurser kan du kolla den [översiktsavsnittet](overview.md). **Se till att granska den [skillnaden mellan en hanterad identitet systemtilldelade och användartilldelade](overview.md#how-does-it-work)**.
- Om du inte redan har ett Azure-konto [registrerar du dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du fortsätter.
- Installera [den senaste versionen av Azure PowerShell](/powershell/azure/install-az-ps) om du inte redan har gjort.

## <a name="system-assigned-managed-identity"></a>Systemtilldelade hanterad identitet

I det här avsnittet får du lära dig hur du aktiverar och inaktiverar det systemtilldelade hanterad identitet med hjälp av Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Aktivera systemtilldelade hanterad identitet under skapandet av en Azure-dator

Om du vill skapa en Azure-dator med systemtilldelade hanterade identiteten aktiverat ditt konto måste den [virtuell Datordeltagare](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rolltilldelning.  Inga ytterligare Azure AD directory rolltilldelningar krävs.

1. Se något av följande Azure VM Snabbstarter, Slutför bara de nödvändiga avsnitt (”logga in på Azure”, ”skapa resursgruppen”, ”skapa nätverk grupp”, ”skapa den virtuella datorn”).
    
    När du kommer till avsnittet ”Skapa VM” gör en liten ändring i [New AzVMConfig](/powershell/module/az.compute/new-azvm) cmdlet-syntax. Se till att lägga till en `-AssignIdentity:$SystemAssigned` parameter för att etablera den virtuella datorn med identiteten systemtilldelade aktiverat, till exempel:
      
    ```powershell
    $vmConfig = New-AzVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Skapa en Windows-dator med hjälp av PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Skapa en Linux-dator med hjälp av PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Valfritt) Lägg till hanterade identiteter för Azure-resurser VM-tillägget (planerad för utfasning i januari 2019) med den `-Type` parametern på den [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. Du kan skicka ”ManagedIdentityExtensionForWindows” eller ”ManagedIdentityExtensionForLinux” beroende på vilken typ av virtuell dator och namnge den med hjälp av den `-Name` parametern. Den `-Settings` parametern anger den port som används av OAuth-token-slutpunkten för tokenförvärv:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Det här steget är valfritt eftersom du kan använda Azure Instance Metadata Service (IMDS) identitet slutpunkten, för att hämta token samt. Hanterade identiteter för VM-tillägg för Azure-resurser är planerat för utfasning i januari 2019. 

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Aktivera systemtilldelade hanterad identitet på en befintlig Azure VM

Om du vill aktivera systemtilldelade hanterad identitet på en virtuell dator som ursprungligen etablerades utan att det behöver ditt konto måste den [virtuell Datordeltagare](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rolltilldelning.  Inga ytterligare Azure AD directory rolltilldelningar krävs.

1. Logga in på Azure med `Connect-AzAccount`. Använd ett konto som är associerad med Azure-prenumerationen som innehåller den virtuella datorn.

   ```powershell
   Connect-AzAccount
   ```

2. Hämta först virtuella datorns egenskaper med hjälp av den `Get-AzVM` cmdlet. Om du vill aktivera en automatiskt genererad hanterad identitet, Använd den `-AssignIdentity` växla den [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet:

   ```powershell
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Valfritt) Lägg till hanterade identiteter för Azure-resurser VM-tillägget (planerad för utfasning i januari 2019) med den `-Type` parametern på den [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. Du kan skicka ”ManagedIdentityExtensionForWindows” eller ”ManagedIdentityExtensionForLinux” beroende på vilken typ av virtuell dator och namnge den med hjälp av den `-Name` parametern. Den `-Settings` parametern anger den port som används av OAuth-token-slutpunkten för tokenförvärv. Se till att ange rätt `-Location` parameter, matcha platsen för den befintliga virtuella datorn:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Det här steget är valfritt eftersom du kan använda Azure Instance Metadata Service (IMDS) identitet slutpunkten, för att hämta token samt.

### <a name="add-vm-system-assigned-identity-to-a-group"></a>Lägg till systemtilldelad identitet för virtuell dator till en grupp

När du har aktiverat systemtilldelad identitet på en virtuell dator kan du lägga till den till en grupp.  Följande procedur lägger till en virtuell dators systemtilldelade identiteter i en grupp.

1. Logga in på Azure med `Connect-AzAccount`. Använd ett konto som är associerad med Azure-prenumerationen som innehåller den virtuella datorn.

   ```powershell
   Connect-AzAccount
   ```

2. Hämta och anteckna den `ObjectID` (som angetts på den `Id` fältet för returnerade värden) för den Virtuella datorns tjänstens huvudnamn:

   ```powerhshell
   Get-AzADServicePrincipal -displayname "myVM"
   ```

3. Hämta och anteckna den `ObjectID` (som angetts på den `Id` fältet för returnerade värden) i gruppen:

   ```powershell
   Get-AzADGroup -searchstring "myGroup"
   ```

4. Lägg till den Virtuella datorns tjänstens huvudnamn i gruppen:

   ```powershell
   Add-AzureADGroupMember -ObjectId "<objectID of group>" -RefObjectId "<object id of VM service principal>"
   ```

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Inaktivera systemtilldelade hanterad identitet från en Azure virtuell dator

Om du vill inaktivera systemtilldelade hanterad identitet på en virtuell dator, ditt konto måste den [virtuell Datordeltagare](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rolltilldelning.  Inga ytterligare Azure AD directory rolltilldelningar krävs.

Om du har en virtuell dator som inte längre behöver systemtilldelade hanterad identitet, men fortfarande ha användartilldelade hanterade identiteter, använder du följande cmdlet:

1. Logga in på Azure med `Connect-AzAccount`. Använd ett konto som är associerad med Azure-prenumerationen som innehåller den virtuella datorn.

   ```powershell
   Connect-AzAccount
   ```

2. Hämta VM-egenskaper med hjälp av den `Get-AzVM` cmdleten och ange den `-IdentityType` parameter `UserAssigned`:

   ```powershell   
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 
   Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Om du har en virtuell dator som inte längre behöver systemtilldelade hanterad identitet och har inga hanterade användartilldelade identiteter, använder du följande kommandon:

```powershell
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Ta bort hanterade identiteter för Azure-resurser VM-tillägg, användare-Name-växeln med den [Remove-AzVMExtension](/powershell/module/az.compute/remove-azvmextension) cmdlet, att ange samma namn som du använde när du har lagt till tillägget:

   ```powershell
   Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-managed-identity"></a>Användartilldelade hanterad identitet

Du lär dig hur du lägger till och ta bort en hanterad Användartilldelad identitet från en virtuell dator med Azure PowerShell i det här avsnittet.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Tilldela en virtuell dator när du skapar en hanterad Användartilldelad identitet

Om du vill tilldela en Användartilldelad identitet till en virtuell dator, ditt konto måste den [virtuell Datordeltagare](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) och [hanterade Identitetsoperatör](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolltilldelningar. Inga ytterligare Azure AD directory rolltilldelningar krävs.

1. Se något av följande Azure VM Snabbstarter, Slutför bara de nödvändiga avsnitt (”logga in på Azure”, ”skapa resursgruppen”, ”skapa nätverk grupp”, ”skapa den virtuella datorn”). 
  
    När du kommer till avsnittet ”Skapa VM” gör en liten ändring i [ `New-AzVMConfig` ](/powershell/module/az.compute/new-azvm) cmdlet-syntax. Lägg till den `-IdentityType UserAssigned` och `-IdentityID ` parametrar för att etablera den virtuella datorn med en Användartilldelad identitet.  Ersätt `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, och `<USER ASSIGNED IDENTITY NAME>` med dina egna värden.  Exempel:
    
    ```powershell 
    $vmConfig = New-AzVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [Skapa en Windows-dator med hjälp av PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Skapa en Linux-dator med hjälp av PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Valfritt) Lägg till hanterad identitet för Azure-resurser VM en tillägget med hjälp av den `-Type` parametern på den [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. Du kan skicka ”ManagedIdentityExtensionForWindows” eller ”ManagedIdentityExtensionForLinux” beroende på vilken typ av virtuell dator och namnge den med hjälp av den `-Name` parametern. Den `-Settings` parametern anger den port som används av OAuth-token-slutpunkten för tokenförvärv. Se till att ange rätt `-Location` parameter, matcha platsen för den befintliga virtuella datorn:
      > [!NOTE]
    > Det här steget är valfritt eftersom du kan använda Azure Instance Metadata Service (IMDS) identitet slutpunkten, för att hämta token samt. Hanterade identiteter för VM-tillägg för Azure-resurser är planerat för utfasning i januari 2019.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Tilldela en hanterad Användartilldelad identitet till en befintlig Azure VM

Om du vill tilldela en Användartilldelad identitet till en virtuell dator, ditt konto måste den [virtuell Datordeltagare](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) och [hanterade Identitetsoperatör](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolltilldelningar. Inga ytterligare Azure AD directory rolltilldelningar krävs.

1. Logga in på Azure med `Connect-AzAccount`. Använd ett konto som är associerad med Azure-prenumerationen som innehåller den virtuella datorn.

   ```powershell
   Connect-AzAccount
   ```

2. Skapa en Användartilldelad hanterad identitet med hjälp av den [New AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity) cmdlet.  Obs den `Id` i utdata eftersom du behöver det i nästa steg.

   > [!IMPORTANT]
   > Skapa användartilldelade hanterade identiteter stöder endast alfanumeriskt och bindestreck (0-9 eller a-z eller A-Z eller -) tecken. Namnet bör dessutom begränsas till 24 tecken för tilldelning till VM/VMSS ska fungera korrekt. Kom tillbaka om för att få uppdateringar. Mer information finns i [vanliga frågor och kända problem](known-issues.md)

   ```powershell
   New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Hämta VM-egenskaper med hjälp av den `Get-AzVM` cmdlet. Om du vill tilldela en hanterad Användartilldelad identitet för Azure-datorn, Använd den `-IdentityType` och `-IdentityID` växla den [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet.  Värdet för den`-IdentityId` parametern är den `Id` du antecknade i föregående steg.  Ersätt `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, och `<USER ASSIGNED IDENTITY NAME>` med dina egna värden.

   > [!WARNING]
   > Om du vill behålla alla tidigare användartilldelade hanterade identiteter tilldelas den virtuella datorn kan fråga den `Identity` egenskapen för det Virtuella datorobjektet (till exempel `$vm.Identity`).  Om någon användare tilldelas hanterade identiteter returneras, lägga till dem i följande kommando tillsammans med den nya användaren som tilldelats hanterad identitet om du vill tilldela till den virtuella datorn.

   ```powershell
   $vm = Get-AzVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Lägg till hanterad identitet för Azure-resurser VM-tillägget (planerad för utfasning i januari 2019) med den `-Type` parametern på den [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. Du kan skicka ”ManagedIdentityExtensionForWindows” eller ”ManagedIdentityExtensionForLinux” beroende på vilken typ av virtuell dator och namnge den med hjälp av den `-Name` parametern. Den `-Settings` parametern anger den port som används av OAuth-token-slutpunkten för tokenförvärv. Ange rätt `-Location` parameter, matcha platsen för den befintliga virtuella datorn.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Ta bort en hanterad Användartilldelad identitet från en Azure-dator

Om du vill ta bort en Användartilldelad identitet till en virtuell dator måste ditt konto måste den [virtuell Datordeltagare](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rolltilldelning.

Om den virtuella datorn har flera användartilldelade hanterade identiteter, kan du ta bort alla utom den sista som använder följande kommandon. Ersätt parametervärdena `<RESOURCE GROUP>` och `<VM NAME>` med dina egna värden. Den `<USER ASSIGNED IDENTITY NAME>` är användartilldelade hanterade identitetens namnegenskapen som fortfarande på den virtuella datorn. Den här informationen kan hittas genom att fråga den `Identity` egenskapen för det Virtuella datorobjektet.  Till exempel `$vm.Identity`:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Om den virtuella datorn inte har en automatiskt genererad hanterad vill identitet och du ta bort alla användartilldelade hanterade identiteter från den, använder du följande kommando:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Om den virtuella datorn har både systemtilldelade och användartilldelade hanterade identiteter, kan du ta bort alla användartilldelade hanterade identiteter genom att växla mellan att använda systemtilldelade endast till hanterade identiteter.

```powershell 
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Nästa steg

- [Hanterade identiteter för översikt över Azure-resurser](overview.md)
- Fullständig Azure skapandet virtuella datorn Snabbstarter, finns här:
  
  - [Skapa en Windows-dator med PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Skapa en Linux-dator med PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
