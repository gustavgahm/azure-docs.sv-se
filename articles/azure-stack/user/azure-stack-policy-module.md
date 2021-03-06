---
title: Använda principmodul Azure Stack | Microsoft Docs
description: Lär dig att begränsa en Azure-prenumeration till att fungera som en Azure Stack-prenumeration
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: sethm
ms.lastreviewed: 11/29/2018
ms.openlocfilehash: 2e1b7257e7ffc4460d86018a6318e33f95e01700
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246272"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Hantera Azure policy som använder Azure Stack-principmodulen

*Gäller för: Integrerade Azure Stack-system och Azure Stack Development Kit*

Principmodulen för Azure Stack kan du konfigurera en Azure-prenumeration med samma versionshantering och tjänsttillgänglighet som Azure Stack. Modulen använder den [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) cmdlet för att skapa en Azure-princip som begränsar de typer av resurser och tjänster som är tillgängliga i en prenumeration. Du sedan skapa en principtilldelning inom det aktuella området med hjälp av den [New-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) cmdlet. Du kan använda din Azure-prenumeration när du har konfigurerat principen för att utveckla appar som är mål för Azure Stack.

## <a name="install-the-module"></a>Installera modulen

1. Installera rätt version av AzureRM PowerShell-modulen enligt beskrivningen i steg 1 av [installera PowerShell för Azure Stack](azure-stack-powershell-install.md).
2. [Ladda ned Azure Stack-verktyg från GitHub](azure-stack-powershell-download.md).
3. [Konfigurera PowerShell för användning med Azure Stack](azure-stack-powershell-configure-user.md).
4. Importera modulen AzureStack.Policy.psm1:

    ```PowerShell
    Import-Module .\Policy\AzureStack.Policy.psm1
    ```

## <a name="apply-policy-to-azure-subscription"></a>Tillämpa principen på Azure-prenumeration

Du kan använda följande kommando för att tillämpa en standardprincip för Azure Stack mot din Azure-prenumeration. Ersätt innan du kör det här kommandot `Azure Subscription Name` med namnet på din Azure-prenumeration.

```PowerShell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "Azure Subscription Name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Tillämpa principen på en resursgrupp

Du kanske vill tillämpa principer som är mer detaljerad. Du kan exempelvis ha andra resurser som körs i samma prenumeration. Du kan begränsa det princip för programmet till en specifik resursgrupp som du kan testa dina appar för Azure Stack med hjälp av Azure-resurser. Innan du kör följande kommando ersätter `Azure Subscription Name` med namnet på din Azure-prenumeration.

```PowerShell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "Azure Subscription Name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName
```

## <a name="policy-in-action"></a>Principen fungerar i praktiken

När du har distribuerat Azure policy kan få du felmeddelanden när du försöker distribuera en resurs som är förbjuden enligt principen.

![Resultatet av resursen misslyckades på grund av principbegränsningen](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Nästa steg

* [Distribuera mallar med PowerShell](azure-stack-deploy-template-powershell.md)
* [Distribuera mallar med Azure CLI](azure-stack-deploy-template-command-line.md)
* [Distribuera mallar med Visual Studio](azure-stack-deploy-template-visual-studio.md)
