---
title: ta med fil
description: ta med fil
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 46b588d6977aa6ffeb9d473e6bfe149cde7147ba
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478760"
---
Filen **utterances.json** med exempelyttranden har ett visst format. 

Fältet `text` innehåller texten för exempelyttrandet. Fältet `intentName` måste motsvara namnet på en befintlig avsikt i LUIS-appen. Fältet `entityLabels` är obligatoriskt. Om du inte vill märka ut några entiteter kan du ange en tom matris.

Om matrisen entityLabels inte är tom måste `startCharIndex` och `endCharIndex` märka ut den entitet som anges i fältet `entityName`. Indexet är nollbaserat, vilket innebär att 6 i det översta exemplet avser ”S” i Seattle och inte blanksteget innan stora S. Om du börjar eller slutar etiketten vid ett blanksteg i texten misslyckas API-anropet om att lägga till yttrandet.

```JSON
[
  {
    "text": "go to Seattle today",
    "intentName": "BookFlight",
    "entityLabels": [
      {
        "entityName": "Location::LocationTo",
        "startCharIndex": 6,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "purple dogs are difficult to work with",
    "intentName": "BookFlight",
    "entityLabels": []
  }
]
```
