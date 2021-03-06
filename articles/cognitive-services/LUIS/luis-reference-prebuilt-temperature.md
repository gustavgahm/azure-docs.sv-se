---
title: Temperatur fördefinierade entitet
titleSuffix: Azure
description: Den här artikeln innehåller temperatur fördefinierade entitetsinformation i Språkförståelse (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 921f2bb2c03a24d5ed269452cdc7f14e7a40c73b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207997"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>Temperatur fördefinierade entitet för en LUIS-app
Temperatur extraherar en mängd olika typer av temperatur. Eftersom den här entiteten har redan tränats, behöver du inte lägga till exempel yttranden som innehåller temperatur till programmet. Temperatur entitet stöds i [många kulturer](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Typer av temperatur
Temperatur hanteras från den [identifierare fulltext](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) GitHub-lagringsplats

## <a name="resolution-for-prebuilt-temperature-entity"></a>Lösning för fördefinierade temperatur entitet
I följande exempel visas av lösningen på den **builtin.temperature** entitet.

```json
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
  "entities": [
    {
      "entity": "30 degrees",
      "type": "builtin.temperature",
      "startIndex": 23,
      "endIndex": 32,
      "resolution": {
        "unit": "Degree",
        "value": "30"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nästa steg

Lär dig mer om den [procent](luis-reference-prebuilt-percentage.md), [nummer](luis-reference-prebuilt-number.md), och [ålder](luis-reference-prebuilt-age.md) entiteter. 
