---
title: Integrera Azure Automation med Event Grid | Microsoft Docs
description: Lär dig hur du lägger till en tagg automatiskt när en ny virtuell dator skapas och skicka ett meddelande till Microsoft Teams.
keywords: automation, runbook, teams, event grid, virtual machine, VM
services: automation
author: eamonoreilly
manager: ''
ms.service: automation
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: eamono
ms.openlocfilehash: d7113842aff4d6d9ec66439d1d43e5e24333d3f1
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467578"
---
# <a name="tutorial-integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Självstudie: Integrera Azure Automation med Event Grid och Microsoft Teams

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Importera en Event Grid-exempel-runbook.
> * Skapa en valfri Microsoft Teams-webhook.
> * Skapa en webhook för runbooken.
> * Skapa en Event Grid-prenumeration.
> * Skapa en virtuell dator som utlöser runbooken.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="prerequisites"></a>Nödvändiga komponenter

För slutföra den här självstudien krävs ett [Azure Automation-konto](../automation/automation-offering-get-started.md) för att lagra den runbook som utlöses från Azure Event Grid-prenumerationen.

* Modulen `AzureRM.Tags` måste läsas in på ditt Automation-konto. Se sidan om [hur du importerar moduler i Azure Automation](../automation/automation-update-azure-modules.md) för att lära dig hur du importerar moduler till Azure Automation.

## <a name="import-an-event-grid-sample-runbook"></a>Importera en Event Grid-exempel-runbook

1. Välj ditt Automation-konto och välj sidan **Runbooks**.

   ![Välja runbooks](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. Välj knappen **Sök i galleri**.

3. Sök efter **Event Grid** och välj **Integrera Azure Automation med Event Grid**.

    ![Importera galleri-runbook](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Välj **Importera** och ge den namnet **Watch-VMWrite**.

5. När den har importerats väljer du **Redigera** för att visa runbook-källan. Välj sedan knappen **Publicera**.

> [!NOTE]
> Rad 74 i skriptet måste ha få raden ändrad till `Update-AzureRmVM -ResourceGroupName $VMResourceGroup -VM $VM -Tag $Tag | Write-Verbose`. Parametern `-Tags` är nu `-Tag`.

## <a name="create-an-optional-microsoft-teams-webhook"></a>Skapa en valfri Microsoft Teams-webhook

1. I Microsoft Teams väljer du **Fler alternativ** intill kanalnamnet, och sedan väljer du **Anslutningsappar**.

    ![Microsoft Teams-anslutningar](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Bläddra igenom listan över anslutningsappar till **Inkommande Webhook** och välj **Lägg till**.

3. Ange **AzureAutomationIntegration** som namn och välj **Skapa**.

4. Kopiera webhooken till Urklipp och spara den. Webhooks-URL:en används för att skicka information till Microsoft Teams.

5. Välj **Klar** för att spara webhooken.

## <a name="create-a-webhook-for-the-runbook"></a>Skapa en webhook för runbooken

1. Öppna runbooken Watch-VMWrite.

2. Välj **Webhooks** och välj knappen **Lägg till Webhook**.

3. Ange **WatchVMEventGrid** som namn. Kopiera URL:en till Urklipp och spara den.

    ![Konfigurera namn på webhook](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. Välj **Konfigurera parametrar och kör inställningar**, och ange en Microsoft Teams-webhook-URL för **CHANNELURL**. Lämna **WEBHOOKDATA** tomt.

    ![Konfigurera webhook-parametrar](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Välj **Skapa** för att skapa webhooken för Automation-runbook.

## <a name="create-an-event-grid-subscription"></a>Skapa en Event Grid-prenumeration

1. På översiktssidan **Automation-konto** väljer du **Event Grid**.

    ![Välja Event Grid](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. Klicka på **+ Händelseprenumeration**.

3. Konfigurera prenumerationen med följande information:

   * Som **Typ av ämne** väljer du **Azure-prenumerationer**.
   * Avmarkera kryssrutan **Prenumerera på alla händelsetyper**.
   * Ange **AzureAutomation** som namn.
   * I listrutan **Definierade händelsetyper** avmarkerar du alla alternativ utom **Resource Write Success** (Resursskrivning lyckades).
   * Som **Typ av slutpunkt** väljer du **Webhook**.
   * Klicka på **Välj en slutpunkt**. På sidan **Välj webhook** som öppnas klistrar du in webhook-URL:en du har skapat för Watch-VMWrite-runbooken.
   * Under **FILTER** anger du den prenumeration och den resursgrupp där du vill leta efter de nya virtuella datorerna som skapats. Det bör se ut så här: `/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`

4. Välj **Skapa** för att spara Event Grid-prenumerationen.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Skapa en virtuell dator som utlöser runbooken

1. Skapa en ny virtuell dator i den resursgrupp som du angav i prefixfiltret för Event Grid-prenumerationen.

2. Runbooken Watch-VMWrite ska anropas, och en ny tagg ska läggas till i den virtuella datorn.

    ![VM-tagg](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Ett nytt meddelande skickas till Microsoft Teams-kanalen.

    ![Microsoft Teams-meddelande](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudien konfigurerar du integrering mellan Event Grid och Automation. Du har lärt dig att:

> [!div class="checklist"]
> * Importera en Event Grid-exempel-runbook.
> * Skapa en valfri Microsoft Teams-webhook.
> * Skapa en webhook för runbooken.
> * Skapa en Event Grid-prenumeration.
> * Skapa en virtuell dator som utlöser runbooken.

> [!div class="nextstepaction"]
> [Skapa och dirigera anpassade händelser med Event Grid](../event-grid/custom-event-quickstart.md)
