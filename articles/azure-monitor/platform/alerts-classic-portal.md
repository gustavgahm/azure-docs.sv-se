---
title: Skapa, visa och hantera klassiska måttaviseringar med Azure Monitor
description: Lär dig hur du använder Azure portal, CLI eller Powershell för att skapa, visa och hantera klassiska måttaviseringsregler.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.openlocfilehash: 6b74d83de0495e3436c9bef623a827e8a1496767
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343315"
---
# <a name="create-view-and-manage-classic-metric-alerts-using-azure-monitor"></a>Skapa, visa och hantera klassiska måttaviseringar med Azure Monitor

Klassiska måttaviseringar i Azure Monitor är ett sätt att få ett meddelande när en av dina mått överskrider ett tröskelvärde. Klassiska måttaviseringar är en äldre funktion som gör att aviseringar endast på nolldimensionella mått. Det finns en befintlig nyare funktion som kallas måttaviseringar, som har förbättrade funktioner över klassiska måttaviseringar. Du kan läsa mer om de nya funktionerna för måttaviseringar i [översikt över aviseringar i mått](../../azure-monitor/platform/alerts-metric-overview.md). I den här artikeln beskriver vi hur du skapar, visa och hantera klassiska måttaviseringsregler via Azure-portalen, Azure CLI och Powershell.

## <a name="with-azure-portal"></a>Med Azure portal

1. I den [portal](https://portal.azure.com/), leta upp den resurs som du vill övervaka och välj den sedan.

2. I den **övervakning** väljer **aviseringar (klassisk)**. Text och ikonen kan variera något mellan olika resurser. Om du inte hittar **aviseringar (klassisk)** här, det kan vara i **aviseringar** eller **Varningsregler**.

    ![Övervakning](media/alerts-classic-portal/AlertRulesButton.png)

3. Välj den **Lägg till måttavisering (klassisk)** kommandot och sedan fylla i fälten.

    ![Lägg till avisering](media/alerts-classic-portal/AddAlertOnlyParamsPage.png)

4. **Namn på** din varningsregeln. Välj sedan en **beskrivning**, som även visas i e-postaviseringar.

5. Välj den **mått** som du vill övervaka. Välj sedan en **villkor** och **tröskelvärdet** värdet för måttet. Också välja den **Period** tid som måttregel måste vara uppfyllda innan aviseringen utlösare. Om du använder perioden ”under de senaste 5 minuterna” och aviseringen söker efter en processor över 80 procent, utlöser aviseringen till exempel när Processorn har varit konsekvent ovan 80% i 5 minuter. Efter först inträffar utlösaren utlöser igen när Processorn är mindre än 80% i 5 minuter. CPU metriska måttenheter sker varje minut.

6. Välj **e-ägare...**  om du vill att administratörer och medadministratörer för att ta emot e-postaviseringar när aviseringen utlöses.

7. Om du vill skicka meddelanden till ytterligare e-postadresser när aviseringen utlöses, lägga till dem i den **administratören email(s)** fält. Avgränsa flera e-postmeddelanden med semikolon, i följande format:  *email@contoso.com;email2@contoso.com*

8. Placera i en giltig URI i den **Webhook** om du vill att den ska anropas när aviseringen utlöses.

9. Om du använder Azure Automation kan välja du en runbook som ska köras när aviseringen utlöses.

10. Skapa aviseringen genom att välja **OK**.

Inom några minuter, aviseringen är aktiv och utlöser som det beskrivits.

När du skapar en avisering kan du markera den och gör något av följande uppgifter:

* Visa ett diagram som visar tröskelvärde för mått och de faktiska värdena från föregående dag.
* Redigera eller ta bort den.
* **Inaktivera** eller **aktivera** det om du vill att tillfälligt stoppa eller återuppta ta emot meddelanden för den här aviseringen.

## <a name="with-azure-cli"></a>Med Azure CLI

I föregående avsnitt beskrivs hur du skapar, visa och hantera måttaviseringsregler med hjälp av Azure portal. Det här avsnittet beskrivs hur du gör samma sak med hjälp av plattformsoberoende [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest). Snabbaste sättet att börja använda Azure CLI är via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest).

### <a name="get-all-classic-metric-alert-rules-in-a-resource-group"></a>Hämta alla klassiska måttaviseringsregler i en resursgrupp

```azurecli
az monitor alert list --resource-group <group name>
```

### <a name="see-details-of-a-particular-classic-metric-alert-rule"></a>Se information om en viss klassiska måttaviseringsregel

```azurecli
az monitor alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-classic-metric-alert-rule"></a>Skapa en klassisk måttaviseringsregel

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-a-classic-metric-alert-rule"></a>Ta bort en klassiska måttaviseringsregel

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="with-powershell"></a>Med PowerShell

Detta avsnitt visar hur du använder PowerShell kommandon skapa, visa och hantera klassiska måttaviseringar. Exemplen i den här artikeln visar hur du kan använda Azure Monitor-cmdletar för klassiska måttaviseringar.

1. Om du inte redan gjort konfigurera PowerShell att köra på datorn. Mer information finns i [installera och konfigurera PowerShell](/powershell/azure/overview). Du kan också granska hela listan med Azure Monitor PowerShell-cmdlets på [Azure Monitor (insikter) Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.insights).

2. Logga först in på Azure-prenumerationen.

    ```PowerShell
    Connect-AzureRmAccount
    ```

3. Du ser ett tecken på skärmen. När du loggar in ditt konto, TenantID, och standard prenumerations-ID visas. Alla Azure-cmdlets fungerar i kontexten för din Standardprenumeration. Om du vill visa en lista över prenumerationer som du har åtkomst till, använder du följande kommando:

    ```PowerShell
    Get-AzureRmSubscription
    ```

4. Om du vill ändra den fungerande kontexten till en annan prenumeration, använder du följande kommando:

    ```PowerShell
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

5. Du kan hämta alla klassiska måttaviseringsregler på en resursgrupp:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup montest
    ```

6. Du kan visa information om en klassisk måttaviseringsregel

    ```PowerShell
    Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
    ```

7. Du kan hämta alla Varningsregler för en målresurs. Till exempel ange alla Varningsregler på en virtuell dator.

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
    ```

8. Klassiska Varningsregler kan inte längre skapas via PowerShell. Skapa en varningsregel som du behöver använda den nya [”Lägg till AzureRmMetricAlertRule'](https://docs.microsoft.com/powershell/module/azurerm.insights/add-azurermmetricalertrule?view=azurermps-6.13.0) kommando.

## <a name="next-steps"></a>Nästa steg

- [Skapa en klassisk måttavisering med en Resource Manager-mall](../../azure-monitor/platform/alerts-enable-template.md).
- [Har en klassisk måttavisering meddela en icke-Azure-system med en webhook](../../azure-monitor/platform/alerts-webhooks.md).
