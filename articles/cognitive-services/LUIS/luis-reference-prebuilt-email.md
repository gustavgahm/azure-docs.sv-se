---
title: LUIS förskapade entiteter e-referens – Azure | Microsoft Docs
titleSuffix: Azure
description: Den här artikeln innehåller e-post fördefinierade entitetsinformation i Språkförståelse (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 080c4d08348265ea891dd02ff3aa2fe8ba2ad2f1
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221495"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>E-fördefinierade entitet för en LUIS-app
Extrahering av e-post innehåller hela e-postadressen från ett uttryck. Eftersom den här entiteten har redan tränats, behöver du inte lägga till exempel yttranden som innehåller e-postmeddelande till programmet avsikter. E-entitet stöds i `en-us` kultur endast. 

## <a name="resolution-for-prebuilt-email"></a>Lösning för fördefinierade e-post
I följande exempel visas av lösningen på den **builtin.email** entitet.

```json
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55
    }
  ]
}
```

## <a name="next-steps"></a>Nästa steg

Lär dig mer om den [nummer](luis-reference-prebuilt-number.md), [ordningstal](luis-reference-prebuilt-ordinal.md), och [procent](luis-reference-prebuilt-percentage.md). 
