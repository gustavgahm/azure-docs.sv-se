---
title: Snabbstart – skicka en fråga till den lokala företag i Bing med Python | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Använd den här artikeln för att börja använda Bing-API för sökning av lokala företag i Python.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 5966f7bf635a0de00fb841cdf22af9cea0698d0e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176258"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Snabbstart: Skicka en fråga till den lokala företag i Bing i Python

Använd den här snabbstarten för att börja skicka begäranden till den lokala företag i Bing, vilket är en Azure Cognitive Service. Medan detta enkla program är skrivet i Python är API: et en RESTful-webb-tjänst som är kompatibel med alla programmeringsspråk som HTTP-förfrågningar och JSON-parsning.

Det här exempelprogrammet hämtar lokala svarsdata från API: et för sökfrågan `hotel in Bellevue`.

## <a name="prerequisites"></a>Förutsättningar

* [Python](https://www.python.org/) 2.x eller 3.x
 
Du måste ha en [Cognitive Services API-konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) med Bing-API: er. Det räcker med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) för den här snabbstarten. Använd åtkomstnyckel som tillhandahållits av den kostnadsfria utvärderingsversionen.  Se även [Priser för Cognitive Services – API för Bing-sökning](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-complete-application"></a>Kör hela appen

Följande kod hämtar lokaliserade resultat. Den implementeras i följande steg:
1. Deklarera variabler för att specificera slutpunkten med hjälp av värd och sökväg.
2. Ange Frågeparametern. 
3. Definiera sökfunktionen som skapar begäran och lägger till Ocp-Apim-Subscription-Key-huvudet.
4. Ange huvudet Ocp-Apim-Subscription-Key. 
5. Kontrollera anslutningen och skicka begäran.
6. Skriv ut JSON-resultaten.

Här följer den fullständiga koden för demon:

```
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'api.cognitive.microsoft.com/bing'
path = '/v7.0/localbusinesses/search'

query = 'restaurant in Bellevue'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

```

## <a name="next-steps"></a>Nästa steg
- [Lokala företag Search Java Snabbstart](local-search-java-quickstart.md)
- [Sök i lokala företag C# Snabbstart](local-quickstart.md)
- [Lokala företag Search Node-Quickstart](local-search-node-quickstart.md)
