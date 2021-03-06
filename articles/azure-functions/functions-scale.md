---
title: Azure Functions skalning och värdtjänster | Microsoft Docs
description: Lär dig hur du väljer mellan Azure Functions-förbrukningsplanen och App Service-plan.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, funktion, förbrukningsplan, app service-plan, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 08/09/2018
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08897b2085c2a8f0eafb90b77486d60a0edce190
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359875"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure Functions skalar och som är värd för

Azure Functions som körs i två olika lägen: Förbrukningsplan och Azure App Service-plan. Med förbrukningsplanen beräkningskraften automatiskt när koden körs. Din app är skalade ut vid behov för att hantera belastningen och skalades när koden inte körs. Du behöver inte betala för virtuella datorer eller reserverad kapacitet i förväg.

> [!NOTE]  
> Förbrukningsplan för Linux är [nu som offentlig förhandsversion](https://azure.microsoft.com/updates/azure-functions-consumption-plan-for-linux-preview/).

Om du inte är bekant med Azure Functions finns i den [översikt över Azure Functions](functions-overview.md).

När du skapar en funktionsapp kan välja du värdplanen för funktioner i appen. I antingen plan, en instans av den *Azure Functions värden* körs funktionerna. Typ av plan kontroller:

* Hur ha instanser skalas ut.
* Resurser som är tillgängliga för varje värd.

> [!IMPORTANT]
> Du måste välja vilken typ av värdplan under genereringen av funktionsappen. Du kan inte ändra efteråt.

Du kan skala mellan nivåerna för att allokera olika mängd resurser på en App Service-plan. I förbrukningsplanen hanterar Azure Functions automatiskt alla resursallokering. 

## <a name="consumption-plan"></a>Förbrukningsplan

När du använder en förbrukningsplan är dynamiskt instanser av Azure Functions-värden har lagts till och tas bort baserat på antalet inkommande händelser. Den här serverlösa planen skalas automatiskt och du debiteras för beräkningsresurser bara när dina funktioner körs. I en förbrukningsplan gör körning av en funktion timeout när en konfigurerbar tidsperiod.

> [!NOTE]
> Standardvärdet för timeout för funktioner i en förbrukningsplan är 5 minuter. Det kan du öka värdet för Funktionsappen upp till 10 minuter genom att ändra egenskapen `functionTimeout` i den [host.json](functions-host-json.md#functiontimeout) projektfilen.

Fakturering baseras på antalet körningar, körningstid och minne som används. Fakturering slås samman över alla funktioner i en funktionsapp. Mer information finns i den [Azure Functions-priserna sidan].

Med förbrukningsplanen är standard värdplan och har följande fördelar:

* Betala endast när dina funktioner körs.
* Skala ut automatiskt, även under perioder med hög att läsa in.

## <a name="app-service-plan"></a>App Service-plan

I den dedikerade App Service-planen kör dina funktionsappar på dedikerade virtuella datorer på Basic, Standard, Premium och isolerade SKU: er, vilket är samma som andra App Service-appar. Dedikerade virtuella datorer allokeras till din funktionsapp, vilket innebär att functions-värden kan vara [alltid körs](#always-on). App Service-planer har stöd för Linux.

Överväg en App Service-plan i följande fall:

* Du har befintliga, underutnyttjade virtuella datorer som redan kör andra App Service-instanser.
* Dina funktionsappar kör kontinuerligt eller nästan kontinuerligt. I det här fallet kan en App Service Plan vara mer kostnadseffektivt.
* Du behöver fler alternativ än vad som tillhandahålls på förbrukningsplanen som CPU eller minne.
* Din kod måste köras längre än den högsta körningstiden som tillåts i förbrukningsplan som är upp till 10 minuter.
* Du behöver funktioner som endast är tillgängliga i en App Service-plan, till exempel stöd för App Service Environment, virtuellt nätverk/VPN-anslutning och större storlekar för Virtuella datorer.
* Du vill köra funktionsappen på Linux eller vill du ange en anpassad avbildning som du vill köra dina funktioner.

En virtuell dator frikopplar kostnaden från antalet körningar, körningstid och minne som används. Därför kan betalar du inte mer än kostnaden för den VM-instans som du tilldelar. Mer information om hur App Service-planen fungerar finns i den [Azure App Service-planer djupgående översikt över](../app-service/overview-hosting-plans.md). 

Du kan skala ut manuellt genom att lägga till flera VM-instanser med en App Service-plan eller du kan aktivera automatisk skalning. Mer information finns i [skala instansantalet manuellt eller automatiskt](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Du kan även skala upp genom att välja en annan App Service-plan. Mer information finns i [skala upp en app i Azure](../app-service/web-sites-scale.md). 

När du kör JavaScript-funktioner i en App Service-plan, bör du välja en plan med färre virtuella processorer. Mer information finns i den [väljer App Service-planer med enkel kärna](functions-reference-node.md#considerations-for-javascript-functions).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
### <a name="always-on"></a>Alltid på

Om du kör i en App Service-plan, bör du aktivera den **alltid på** så att din funktionsapp körs korrekt. I en App Service-plan går att funktionskörningen inaktiv efter ett par minuter av inaktivitet, så att endast HTTP-utlösare ska ”aktivera” dina funktioner. Alltid är på endast tillgänglig på en App Service plan. I en förbrukningsplan aktiverar plattformen funktionsappar automatiskt.

## <a name="what-is-my-hosting-plan"></a>Vad är min värdplanen

Information om värdplanen som används av funktionsappen finns **App Service-plan / prisnivå** i den **översikt** fliken för funktionsappen i den [Azure-portalen](https://portal.azure.com). För App Service-planer anges också prisnivån. 

![Visa skalning plan i portalen](./media/functions-scale/function-app-overview-portal.png)

Du kan också använda Azure CLI för att fastställa planen, enligt följande:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

När utdata från det här kommandot är `dynamic`, funktionsappen är i förbrukningsplanen. Alla andra värden ange nivån för en App Service plan.

Även med alltid aktiverad, tidsgränsen för körningar för enskilda funktioner styrs av den `functionTimeout` i den [host.json](functions-host-json.md#functiontimeout) projektfilen.

## <a name="storage-account-requirements"></a>Krav för lagringskonto

En funktionsapp kräver ett allmänt Azure Storage-konto som har stöd för Azure Blob, kö, filer och tabell för lagring på en förbrukningsplan eller en App Service plan. Detta beror på funktioner som förlitar sig på Azure Storage för åtgärder som att hantera utlösare och logga funktionskörningar, men vissa lagringskonton stöder inte köer och tabeller. Dessa konton, bland annat endast blob storage-konton (inklusive premiumlagring) och lagringskonton med replikering för zonredundant lagring, är filtrerat ut från din befintliga **Lagringskonto** val när du skapar en funktionsapp.

<!-- JH: Does using a Premium Storage account improve perf? -->

Mer information om lagringskontotyper finns [introduktion till Azure Storage-tjänsterna](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Hur förbrukningsplanen fungerar

I förbrukningsplan kan skalas kontrollanten skala automatiskt Processorn och minnesresurser genom att lägga till ytterligare instanser för Functions-värden, baserat på antalet händelser som utlöses på dess funktioner. Varje instans av Functions-värden är begränsad till 1,5 GB minne.  En instans av värden är funktionsappen, vilket innebär att alla funktioner i en funktionen app-resurs i en instans och skala på samma gång. Funktionsappar som delar samma förbrukningsplan skalas oberoende av varandra.  

När du använder Förbrukningsvärdplanen lagras funktionen kodfiler på Azure-filresurser på funktionens huvudlagringskontot. När du tar bort huvudlagringskontot av funktionsappen kodfiler funktionen tas bort och kan inte återställas.

> [!NOTE]
> När du använder en blob-utlösare i en förbrukningsplan, kan det vara upp till en 10 minuters fördröjning vid bearbetningen av nya blobbar. Den här fördröjningen uppstår när en funktionsapp är inaktiv. När appen körs behandlas BLOB-objekt direkt. För att undvika den här kallstart fördröjningen kan använda en App Service-plan med **Always On** aktiverad, eller använda Event Grid-utlösaren. Mer information finns i [referensartikeln för blob-utlösare bindning](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Runtime skalning

Azure Functions använder en komponent som kallas den *skala controller* att övervaka frekvensen för händelser och avgöra om du vill skala ut eller skala in. Skala controller använder heuristisk sökning för varje Utlösartyp av. Till exempel när du använder en Azure Queue storage-utlösare, skalas utifrån kölängden och åldern på äldsta kömeddelandet.

Skala är funktionsappen. När funktionsappen har skalats ut, tilldelas ytterligare resurser för att köra flera instanser av Azure Functions-värden. Eftersom beräknings-efterfrågan minskar, tar däremot funktionen ha instanser bort av kontrollanten skala. Antalet instanser så småningom att skalas ned till noll när inga funktioner som körs inom en funktionsapp.

![Skala controller tjänsthändelser och skapa instanser](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Förstå skalningsbeteenden

Skalning kan variera på ett antal faktorer och skala på olika sätt beroende på utlösaren och den valda språket. Det finns dock några aspekter av skalning som finns i systemet idag:

* En enda funktionsapp skalar bara upp till högst 200 instanser. En enda instans kan bearbeta flera meddelande eller begäran i taget dock så det finns en set-gränsen för antalet samtidiga körningar.
* Nya instanser allokeras endast högst en gång var tionde sekund.

Olika utlösare kan också ha olika skalningsgränser samt dokumenterade nedan:

* [Händelsehubb](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Bästa praxis och mönster för skalbara appar

Det finns många aspekter av en funktionsapp som påverkar hur väl den skalas, inklusive konfiguration av värdserver, runtime fotavtryck och Resursanvändning.  Mer information finns i den [skalbarhet i prestanda överväganden artikeln](functions-best-practices.md#scalability-best-practices). Du bör också känna till hur anslutningar ska fungera med ditt funktionen app. Mer information finns i [hur du hanterar anslutningar i Azure Functions](manage-connections.md).

### <a name="billing-model"></a>Faktureringsmodell

Faktureringen för förbrukningsplanen beskrivs i detalj på den [Azure Functions-priserna sidan]. Användning sammanställs på funktionen app-nivå och räknar bara den tid som Funktionskoden körs. Här följer några enheter för fakturering:

* **Resursförbrukning i gigabyte-sekunder (GB-s)**. Beräknad som en kombination av minnesstorlek och körningstid för alla funktioner i en funktionsapp. 
* **Körningar**. Räknas varje gång en funktion körs som svar på en händelseutlösare.

[Azure Functions-priserna sidan]: https://azure.microsoft.com/pricing/details/functions
