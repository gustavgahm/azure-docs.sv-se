---
title: Få ut mer av Azure Application Insights | Microsoft Docs
description: Efter att komma igång med Application Insights, är här en sammanfattning av de funktioner som du kan utforska.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: b81a555111ff49fcf2e14a75afdce81835d151bb
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54038557"
---
# <a name="more-telemetry-from-application-insights"></a>Mer telemetri från Application Insights
När du har [lagt till Application Insights för ASP.NET-koden](../../azure-monitor/app/asp-net.md), det finns några saker som du kan göra för att få ännu mer telemetri. 

| Åtgärd | Vad du får|
|---|---|
|(IIS-servrar) [Installera Status Monitor](https://go.microsoft.com/fwlink/?LinkId=506648) på varje server-datorn.<br/>(Azure-webbappar) Öppna Application Insights-bladet i Azure på Kontrollpanelen för webbappen.| [**Prestandaräknare**](../../azure-monitor/app/performance-counters.md)<br/>[**Undantag** ](asp-net-exceptions.md) – detaljerad Stacka spårningar<br/>[**Beroenden**](../../azure-monitor/app/asp-net-dependencies.md)|
|[Lägg till JavaScript-kodavsnitt till dina webbsidor](../../azure-monitor/app/javascript.md)|[Sidan prestanda](../../azure-monitor/app/usage-overview.md), webbläsarundantag, prestanda för AJAX. Anpassad telemetri för klientsidan.|
|[Skapa webbtester för tillgänglighet](../../azure-monitor/app/monitor-web-app-availability.md)|Få aviseringar om din webbplats blir otillgänglig|
|[Se till att buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) genereras av MSBuild|[Skapa anteckningar i måttdiagram](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Skriva anpassade händelser och mått](../../azure-monitor/app/api-custom-events-metrics.md)|Antal företag händelser och mått, spåra detaljerad användning och mycket mer.|
|[Profilera live-webbplatsen](https://aka.ms/AIProfilerPreview)|Detaljerad funktionen tidsinställningar från live-webbapp|






