---
title: Ansluta till Azure Stack med PowerShell som operatör | Microsoft Docs
description: Lär dig hur du ansluter till Azure Stack med PowerShell som operatör
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: 47d6b336a031f4233bebb7af0b0c57dd8f643dac
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55452490"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>Ansluta till Azure Stack med PowerShell som operatör

*Gäller för: Integrerade Azure Stack-system och Azure Stack Development Kit*

Du kan konfigurera Azure Stack för att använda PowerShell för att hantera resurser, t.ex skapa erbjudanden, planer, kvoter och aviseringar. Det här avsnittet hjälper dig att konfigurera miljön för operatorn.

## <a name="prerequisites"></a>Förutsättningar

Kör följande från den [Utvecklingskit](./asdk/asdk-connect.md#connect-with-rdp) eller från en Windows-baserad externa klient om du är [är anslutna till ASDK via VPN](./asdk/asdk-connect.md#connect-with-vpn). 

 - Installera [Azure Stack-kompatibla Azure PowerShell-moduler](azure-stack-powershell-install.md).  
 - Ladda ned den [verktyg som krävs för att arbeta med Azure Stack](azure-stack-powershell-download.md).  

## <a name="connect-with-azure-ad"></a>Ansluta till Azure AD

Konfigurera miljön för Azure Stack-operatör med PowerShell. Kör något av följande skript: Ersätt tenantName för Azure Active Directory (Azure AD) och värden för Azure Resource Manager-slutpunkten med din egen miljökonfiguration. <!-- GraphAudience endpoint -->

```PowerShell  
    # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external"

    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackAdmin").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>Ansluta med AD FS

Anslut till Azure Stack-operator-miljön med PowerShell med Azure Active Directory Federation Services (Azure AD FS). För Azure Stack development kit den här Azure Resource Manager-slutpunkten är inställt på `https://adminmanagement.local.azurestack.external`. Kontakta tjänstleverantören om du vill ha Azure Resource Manager-slutpunkten för integrerade Azure Stack-system.

<!-- GraphAudience endpoint -->

  ```PowerShell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
  Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external"

  # Sign in to your environment

  $cred = get-credential

  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -Credential $cred
  ```

> [!Note]  
> AD FS har endast stöd för interaktiv autentisering med användaridentiteter. Om ett autentiseringsuppgiftobjekt krävs måste du använda ett huvudnamn för tjänsten (SPN). Mer information om hur du skapar ett huvudnamn för tjänsten med Azure Stack och AD FS som identity management-tjänsten finns i [Hantera tjänstens huvudnamn för AD FS](azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

## <a name="test-the-connectivity"></a>Testa anslutningen

Nu när du har allt installation, Använd PowerShell för att skapa resurser i Azure Stack. Du kan till exempel skapa en resursgrupp för ett program och lägga till en virtuell dator. Använd följande kommando för att skapa en resursgrupp med namnet **MyResourceGroup**.

```PowerShell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Nästa steg

 - [Utveckla mallar för Azure Stack](user/azure-stack-develop-templates.md)
 - [Distribuera mallar med PowerShell](user/azure-stack-deploy-template-powershell.md)
