---
title: Övervakning av data som samlas in av Azure Monitor | Microsoft Docs
description: Övervakning av data som samlas in av Azure Monitor är uppdelad i mått som är lätt och kan stödja scenarier i nästan realtid och loggar som används för avancerad analys.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/05/2018
ms.author: bwren
ms.openlocfilehash: efc5fb022d117caeaec9da014252b501f2d06769
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54450036"
---
# <a name="monitoring-data-collected-by-azure-monitor"></a>Övervakning av data som samlas in av Azure Monitor
[Azure Monitor](../overview.md) är en tjänst som hjälper dig att övervaka dina program och resurser som de förlitar sig på. Centrala för den här funktionen är lagring av telemetri och andra data från övervakade resurser. Den här artikeln innehåller en fullständig beskrivning av hur dessa data lagras och används av Azure Monitor.

Alla data som samlas in av Azure Monitor passar in i en av två grundläggande typer [mått](#metrics) och [loggar](#logs). Mått är numeriska värden som beskriver någon aspekt av ett system vid en viss tidpunkt. De är enkel och kan stödja scenarier i nästan realtid. Loggar innehålla olika typer av data ordnas i poster med olika uppsättningar med egenskaper för varje typ. Telemetri, till exempel händelser och spårningar lagras som loggar dessutom till prestandadata så att den kan alla kombineras för analys.

![Översikt över Azure Monitor](media/data-collection/overview.png)

## <a name="metrics"></a>Mått
Mått är numeriska värden som beskriver någon aspekt av ett system vid en given tidpunkt. De är enkel och kan stödja scenarier i nästan realtid. Mätvärden samlas in med jämna mellanrum oavsett värdet ändras. Det är användbart för avisering eftersom de kan samlas in ofta och en avisering kan vara drar igång snabbt med relativt enkla logik.

Exempel: du kan samla in processorbelastning från en virtuell dator på varje minut eller antalet användare som loggat in på ditt program var tionde minut. Du kan starta en avisering när någon av de insamlade värdena eller även skillnaden mellan två värden överskrider ett angivet tröskelvärde.

Följande: specifika attribut för mått i Azure

* Samlas in med en minut frekvens om inget annat anges i definitionen för det mått.
* Identifieras unikt genom ett Måttnamn och ett namnområde som fungerar som en kategori.
* Lagras i 93 dagar. Du kan kopiera mått till loggar för långsiktiga trender.

Varje måttvärde har följande egenskaper:
* Tiden samlades in värdet.
* Typ av mått värdet representerar.
* Resursen värdet är associerad med.
* Själva värdet.
* Vissa mått kan ha flera dimensioner som beskrivs i nästa avsnitt. Anpassade mått kan ha upp till 10 dimensioner.

### <a name="multi-dimensional-metrics"></a>Flerdimensionella mått
Storleken på ett mått är namn / värde-par som innehåller ytterligare data för att beskriva mätvärdet. Till exempel ett mått _tillgängligt diskutrymme_ kan ha en dimension som kallas _enhet_ med värden _C:_, _D:_, vilket gör att visa antingen finns tillräckligt med diskutrymme på alla enheter eller för varje enhet individuellt.

Exemplet nedan visar två datauppsättningar för ett hypotetiskt mått som kallas _Nätverksdataflöde_. Den första datauppsättningen har inga dimensioner. Andra datauppsättningen visas värden med två dimensioner _IP-adress_ och _riktning_:

### <a name="network-throughput"></a>Nätverkets dataflöde

| Tidsstämpel     | Måttvärde |
| ------------- |:-------------|
| 8/9/2017 8:14 | 1,331.8 kbit/s |
| 8/9/2017 8:15 | 1,141.4 kbit/s |
| 8/9/2017 8:16 | 1,110.2 kbit/s |

Den här nolldimensionella mått kan bara svar grundläggande frågor som ”vad var min dataflöde i nätverket vid en given tidpunkt”?

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Nätverkets genomflöde + två dimensioner (”IP” och ”riktning”)

| Tidsstämpel     | Dimensionen ”IP”   | Dimensionen ”riktning” | Måttvärde|
| ------------- |:-----------------|:------------------- |:-----------|
| 8/9/2017 8:14 | IP = ”192.168.5.2” | Riktning = ”skicka”    | 646.5 kbit/s |
| 8/9/2017 8:14 | IP = ”192.168.5.2” | Riktning = ”ta emot” | 420.1 kbit/s |
| 8/9/2017 8:14 | IP = ”10.24.2.15”  | Riktning = ”skicka”    | 150.0 kbit/s |
| 8/9/2017 8:14 | IP = ”10.24.2.15”  | Riktning = ”ta emot” | 115,2 kbit/s |
| 8/9/2017 8:15 | IP = ”192.168.5.2” | Riktning = ”skicka”    | 515.2 kbit/s |
| 8/9/2017 8:15 | IP = ”192.168.5.2” | Riktning = ”ta emot” | 371.1 kbit/s |
| 8/9/2017 8:15 | IP = ”10.24.2.15”  | Riktning = ”skicka”    | 155.0 kbit/s |
| 8/9/2017 8:15 | IP = ”10.24.2.15”  | Riktning = ”ta emot” | 100.1 kbit/s |

Det här måttet kan besvara frågor som ”vad var nätverksgenomflöde för varje IP-adress”? och ”hur mycket data som har skickats och tagits emot”? Flerdimensionella mått utföra ytterligare analys och diagnostik värde som jämförs med nolldimensionella mått.

### <a name="value-of-metrics"></a>Värdet för mått
Enskilda mått ger vanligtvis lite information på egen hand. De ger ett enstaka värde utan kontext än jämförelse med ett enkelt tröskelvärde. Det är värdefullt när de kombineras med andra mått för att identifiera mönster och trender eller i kombination med loggar som fungerar som kontext runt specifika värden.

Till exempel kan ett visst antal användare på ditt program vid en given tidpunkt visa lite om hälsotillståndet för programmet. Men en plötslig försämring användare, som anges med flera värden med samma mått kan indikera ett problem. Långa undantag som utlösts av programmet och anges med ett separat mått kan identifiera ett problem med programmet som orsakar i listrutan. Händelser som programmet skapar för att identifiera fel i dess komponenter kan hjälpa dig att identifiera orsaken.

### <a name="sources-of-metric-data"></a>Källor för måttdata
Det finns tre grundläggande källor till mätvärden som samlats in från Azure Monitor. Alla de här måtten är tillgängliga i mått store där de kan utvärderas tillsammans oavsett källan.

**Plattformen mått** skapas av Azure-resurser och ger dig insyn i deras hälsotillstånd och prestanda. Varje typ av resurs skapar en [distinkt uppsättning med mått](../../azure-monitor/platform/metrics-supported.md) utan konfiguration krävs.

**Mätvärden för Application** skapas av Application Insights för övervakade program och hjälper dig att identifiera problem med prestanda och spåra trender i hur ditt program används. Detta inkluderar sådana värden som _serversvarstid_ och _webbläsarundantag_.

**Anpassade mått** är mått som du definierar förutom standard mått som är automatiskt tillgängliga. Anpassade mått måste skapas mot en enskild resurs i samma region som den här resursen. Du kan skapa anpassade mått med hjälp av följande metoder:
- [Definiera anpassade mått i ditt program](../../azure-monitor/app/api-custom-events-metrics.md) som övervakas av Application Insights. Dessa anges utöver standard av programmått.
- Publicera anpassade mått från dina Windows-datorer med hjälp av [Windows diagnostik tillägget SÄKERHETSSPECIFIKA](../../azure-monitor/platform/diagnostics-extension-overview.md).
- Publicera anpassade mått från din Linux-datorer med hjälp av [InfluxData Telegraf agenten](https://www.influxdata.com/time-series-platform/telegraf/).
- Skriva anpassade mått från en Azure-tjänst med hjälp av den anpassade måtten-API.

![Översikt över mått](media/data-collection/metrics-overview.png)

### <a name="what-can-you-do-with-metrics"></a>Vad kan du göra med mått?
Följande: uppgifter som du kan utföra med mått

- Använd [måttutforskaren](../../azure-monitor/platform/metrics-charts.md) att analysera insamlade mätvärdena och rita ut dem i ett diagram. Spåra prestanda för en resurs (till exempel en virtuell dator, webbplats eller logic app) genom att fästa diagram för en [Azure-instrumentpanelen](../../azure-portal/azure-portal-dashboards.md).
- Konfigurera en [måttaviseringsregel](alerts-metric.md) som skickar ett meddelande eller tar [automatisk åtgärd](action-groups.md) när måtten överskrider ett tröskelvärde.
- Använd [Autoskala](../../azure-monitor/platform/autoscale-overview.md) att öka eller minska resurser baserat på ett mått som korsar ett tröskelvärde.
- Vägens mått till Log Analytics att analysera måttdata tillsammans med loggdata och för att lagra måttvärden under längre tid än 93 dagar.
- Stream mått för att en [Händelsehubb](../../azure-monitor/platform/stream-monitoring-data-event-hubs.md) att dirigera dem till [Azure Stream Analytics](../../stream-analytics/stream-analytics-introduction.md) eller till externa system.
- [Arkivera](../../azure-monitor/learn/tutorial-archive-data.md) prestanda eller hälsotillstånd historiken för dina resurser för efterlevnad, granskning eller rapportering offline.
- Komma åt mätvärden värden från en kommandorad eller anpassade program med hjälp av [PowerShell-cmdletar](https://docs.microsoft.com/powershell/module/azurerm.insights/?view=azurermps-6.7.0) eller [REST API](../../azure-monitor/platform/rest-api-walkthrough.md).

### <a name="viewing-metrics"></a>Visa mått
Mått i Azure Monitor lagras i en tidsserie databas som är optimerat för snabb hämtnings- och lagrar måttvärden 93 dagar. Kopiera mått till loggar för långsiktig analys och trender.

Måttdata används i en mängd olika sätt enligt beskrivningen ovan. Använd [måttutforskaren](../../azure-monitor/platform/metrics-charts.md) att direkt analysera data i din mått store och skapa diagram över värdena för flera mått med tiden. Du kan visa diagrammen interaktivt eller fästa dem på en instrumentpanel för att visa dem med andra visualiseringar. Du kan också hämta mått med hjälp av den [Azure monitoring REST API](../../azure-monitor/platform/rest-api-walkthrough.md).

![Metrics Explorer](media/data-collection/metrics-explorer.png)

## <a name="logs"></a>Logs
Loggar innehålla olika typer av data ordnas i poster med olika uppsättningar med egenskaper för varje typ. Loggar kan innehålla numeriska värden, till exempel mått, men vanligtvis innehåller textdata med detaljerade beskrivningar. De ytterligare skiljer sig från mått genom att de kan variera i deras struktur och ofta inte har samlats in med jämna mellanrum.

En vanlig typ av loggpost är en händelse som samlas in sporadiskt. De skapas av programmet eller tjänsten och inkluderar vanligtvis tillräcklig information för att tillhandahålla slutförd kontexten på egen hand. En händelse kan till exempel ange att en viss resurs har skapats eller ändrats, en ny värd igång som svar på ökad trafik, eller ett fel upptäcktes i ett program.

Loggarna är särskilt användbart för att kombinera data från olika källor, komplexa analyser och trender över tid. Eftersom formatet för data kan variera kan program skapa anpassade loggar med hjälp av strukturen som de behöver. Mått replikeras även i loggarna kan kombinera dem med andra övervakningsdata för trender och andra dataanalys.

### <a name="sources-of-log-data"></a>Källor för loggdata
Azure Monitor kan samla in loggdata från olika källor båda i Azure och lokala resurser. Källor för loggdata som följande:

- [Aktivitetsloggar](collect-activity-logs.md) från Azure-resurser med information om sina konfigurationer och hälsa och [diagnostikloggar](../../azure-monitor/platform/diagnostic-logs-stream-log-store.md) som ger insikt i deras funktion.
- Agenter på [Windows](agent-windows.md) och [Linux](../learn/quick-collect-linux-computer.md) virtuella datorer som skickar telemetri från gästoperativsystem och program till Azure Monitor enligt [datakällor](data-sources.md) som du konfigurerar.
- Programdata som samlas in av [Application Insights](https://docs.microsoft.com/azure/application-insights/).
- Data som ger insikter om ett visst program eller tjänst från [övervakningslösningar](../insights/solutions.md) eller funktioner som Behållareinsikter, VM insikter eller Resource Group insikter.
- Säkerhetsdata som samlas in av [Azure Security Center](https://docs.microsoft.com/azure/security-center/).
- [Mått](#metrics) från Azure-resurser. På så sätt kan du kan lagra mätvärden under längre tid än 93 dagar och analysera dem med andra loggdata.
- Telemetri som skrivs till [Azure Storage](azure-storage-iis-table.md).
- Anpassade data från alla REST API-klient som använder den [HTTP Data Collector API](data-collector-api.md) klienten eller från en [Azure Logic App](https://docs.microsoft.com/azure/logic-apps/) arbetsflöde.

![Översikt över loggar](media/data-collection/logs-overview.png)

### <a name="what-can-you-do-with-logs"></a>Vad kan du göra med loggar?
Uppgifter som du kan utföra med loggar som följande:

- Använd [Log Analytics](../log-query/get-started-portal.md) i Azure portal för att skriva frågor som analyserar loggdata. Fästa resultatet renderas som tabeller eller diagram för en [Azure-instrumentpanelen](../../azure-portal/azure-portal-dashboards.md).
- Konfigurera en [loggvarningsregler](alerts-log.md) som skickar ett meddelande eller tar [automatisk åtgärd](action-groups.md) när frågans resultat matchar ett visst resultat.
- Skapa ett arbetsflöde baserat på log data med hjälp av [Logikappar](~/articles/logic-apps/index.yml).
- Exportera resultatet av en fråga till [Power BI](powerbi.md) att använda olika visualiseringar och dela med användare utanför Azure.
- Komma åt mätvärden värden från en kommandorad eller anpassade program med hjälp av [PowerShell-cmdletar](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/?view=azurermps-6.8.1) eller [REST API](https://dev.loganalytics.io/).

### <a name="viewing-log-data"></a>Visa loggdata
Alla loggdata i Azure Monitor hämtas med hjälp av en [loggfråga](../log-query/log-query-overview.md) skrivna med den [Datautforskaren frågespråk](../log-query/get-started-queries.md), där du kan snabbt hämta, konsolidera och analysera insamlade data. Använd [Log Analytics](../log-query/portals.md) att skriva och testa frågor i Azure-portalen. Du kan arbeta interaktivt med resultat eller fästa dem på en instrumentpanel för att visa dem med andra visualiseringar. Du kan också hämta loggar med hjälp av den [Azure monitoring REST API](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

> [!IMPORTANT]
> Data från Application Insights lagras i en separat partition än andra loggdata i Azure Monitor. Det ger stöd för samma funktioner som andra loggdata, men du måste använda den [Application Insights-konsolen](../app/analytics.md) eller [Application Insights API](https://dev.applicationinsights.io/) åt dessa data. Du kan använda en [mellan resurser fråga](../log-query/cross-workspace-query.md) att analysera programdata tillsammans med andra loggdata.

![Logs](media/data-collection/logs.png)

## <a name="convert-monitoring-data"></a>Konvertera övervakningsdata

### <a name="metrics-to-logs"></a>Mått till loggar
Du kan kopiera mått till loggar för att utföra avancerade analyser med andra datatyper med hjälp av Azure Monitor funktionsrikt frågespråk. Du kan även spara loggdata under längre perioder än mätvärden, där du kan utföra resursanvändningstrender. Använda mått för att stödja nästan i realtid analys och varning när du använder loggar för trender och göra analyser med andra data.

Du kan få vägledning för att samla in mått från Azure-resurser på [samla in Azure-tjänsteloggar och mått för användning i Azure Monitor](collect-azure-metrics-logs.md). Få vägledning för att samla in resurser mått från Azure PaaS-resurser på [konfigurera insamling av mätvärden för Azure PaaS-resurs med Azure Monitor](collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Loggar till mått
Enligt beskrivningen ovan, är mått snabbare än loggar, så du kan skapa aviseringar med kortare svarstider och till en lägre kostnad. En betydande mängd numeriska data lagras som loggar som skulle vara lämplig för mått, men inte lagras som mått i Azure Monitor. Ett vanligt exempel är prestandadata som samlats in från agenter och lösningar för hantering. Vissa av dessa värden kan kopieras till mått, när de är tillgängliga för aviseringar och analys med Metrics Explorer.

Förklaring av den här funktionen är tillgänglig på [skapa aviseringar för mått för loggar i Azure Monitor](../../azure-monitor/platform/alerts-metric-logs.md). Listan över värden support finns på [stöds mått med Azure Monitor](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces).

## <a name="stream-data-to-external-systems"></a>Stream-data till externa system
Förutom att använda verktygen i Azure för att analysera övervakningsdata, kan du ha ett krav att vidarebefordra det till ett externt verktyg, till exempel en säkerhetsinformations- och event management (SIEM)-produkt. Den här vidarebefordran normalt görs direkt från övervakade resurser via [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/).

Du kan få vägledning för de olika typerna av övervakningsdata på [Stream Azure-övervakningsdata till en händelsehubb för användning av något externt verktyg](../../azure-monitor/platform/stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om den [övervakningsdata är tillgängliga](data-sources.md) för olika resurser i Azure.
