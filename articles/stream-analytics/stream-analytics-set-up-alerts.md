---
title: Konfigurera övervakning av aviseringar för Azure Stream Analytics-jobb
description: Den här artikeln beskriver hur du använder Azure-portalen för att konfigurera övervakning och aviseringar för Azure Stream Analytics-jobb.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/19/2019
ms.custom: seodec18
ms.openlocfilehash: 4c0d32a201da5befbc8b68148f0b051e283ec289
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/19/2019
ms.locfileid: "54412402"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Konfigurera aviseringar för Azure Stream Analytics-jobb

Det är viktigt att övervaka dina Azure Stream Analytics-jobb för att säkerställa att jobbet körs kontinuerligt utan problem. Den här artikeln beskriver hur du ställer in aviseringar för vanliga scenarier som bör övervakas. 

Regler kan ställas in på mått via portalen och kan konfigureras [programmässigt](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) Åtgärdsloggar data.

## <a name="set-up-alerts-in-the-azure-portal"></a>Konfigurera aviseringar i Azure portal

I följande exempel visar hur du ställer in aviseringar för när dina jobb försätts i ett felaktigt tillstånd. Den här aviseringen rekommenderas för alla jobb.

1. I Azure-portalen öppnar du ett Stream Analytics-jobb som du vill skapa en avisering för.

2. På den **jobbet** sidan, gå till den **övervakning** avsnittet.  

3. Välj **mått**, och klicka sedan på **ny aviseringsregel**.

   ![Azure Stream Analytics aviseringar portalinställningarna](./media/stream-analytics-set-up-alerts/stream-analytics-set-up-alerts.png)  

4. Ditt Stream Analytics-jobbets namn visas automatiskt **RESOURCE**. Klicka på **Lägg till villkor**, och välj **alla administrativa åtgärder** under **konfigurera signallogiken**.

   ![Välj Signalnamn för Stream Analytics-avisering](./media/stream-analytics-set-up-alerts/stream-analytics-condition-signal.png)  

5. Under **konfigurera signallogiken**, ändra **Händelsenivå** till **alla** och ändra **Status** till **misslyckades** . Lämna **händelsen initieras av** tomt och klicka på **klar**.

   ![Konfigurera signallogiken för Stream Analytics-avisering](./media/stream-analytics-set-up-alerts/stream-analytics-configure-signal-logic.png) 

6. Välj en befintlig åtgärdsgrupp eller skapa en ny grupp. I det här exemplet är en ny åtgärdsgrupp som heter **TIDashboardGroupActions** skapades med en **e-postmeddelanden** åtgärd som skickar ett e-postmeddelande till användare med den **ägare** Azure-resurs Hanterarroll.

   ![Ställa in en avisering för Azure Streaming Analytics-jobb](./media/stream-analytics-set-up-alerts/stream-analytics-add-group-email-action.png)

7. Den **RESOURCE**, **villkor**, och **ÅTGÄRDSGRUPPER** måste var och en post.

   ![Skapa aviseringsregel för Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule-2.png)

   Lägg till en **varningsregelns namn**, **beskrivning**, och din **resursgrupp** till den **AVISERINGSINFORMATION** och klicka på **skapa avisering regeln** att skapa regel för ditt Stream Analytics-jobb.

   ![Skapa aviseringsregel för Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule.png)

## <a name="scenarios-to-monitor"></a>Scenarier för att övervaka

Följande aviseringar rekommenderas för övervakning av prestanda i ditt Stream Analytics-jobb. De här måtten bör utvärderas varje minut under de senaste 5-minutersperioden. Om jobbet har prestandaproblem, kan du använda frågeparallellisering blir mer optimala och prova att öka antalet enheter för strömning.

|Mått|Tillstånd|Tidsmängd|Tröskelvärde|Korrigerande åtgärder|
|-|-|-|-|-|
|SU % utnyttjande|Större än|Maximal|80|Det finns flera faktorer som ökar SU % utnyttjande. Du kan skala med frågeparallellisering och öka antalet enheter för strömning. Mer information finns i [Utnyttja frågeparallellisering i Azure Stream Analytics](stream-analytics-parallelization.md).|
|Körningsfel|Större än|Totalt|0|Granska aktivitet eller diagnostikloggar och gör lämpliga ändringar i indata, fråga eller utdata.|
|Fördröjning för vattenstämpel|Större än|Maximal|När medelvärdet för det här måttet under de senaste 15 minuterna är större än sent ankomst tolerans (i sekunder). Om du inte har ändrat sent ankomst toleransen anges standard till 5 sekunder.|Försök att öka antalet SUs eller parallellisera din fråga. Läs mer på SUs [förstå och justera Direktuppspelningsenheter](stream-analytics-streaming-unit-consumption.md#how-many-sus-are-required-for-a-job). Läs mer på parallellisera frågan [utnyttja frågeparallellisering i Azure Stream Analytics](stream-analytics-parallelization.md).|
|Fel vid deserialisering av indata|Större än|Totalt|0|Granska aktivitet eller diagnostikloggar och gör lämpliga ändringar för indata. Läs mer på diagnostikloggar [felsöka Azure Stream Analytics med hjälp av diagnostikloggar](stream-analytics-job-diagnostic-logs.md)|

## <a name="get-help"></a>Få hjälp

Mer information om hur du konfigurerar aviseringar i Azure-portalen finns i [Varningsaviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

För mer hjälp kan du prova vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion till Azure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-get-started.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

