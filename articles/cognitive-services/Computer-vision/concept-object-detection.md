---
title: Objektidentifiering - visuellt innehåll
titleSuffix: Azure Cognitive Services
description: Begrepp för objektidentifiering med hjälp av den API för visuellt innehåll.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 93ce86a438fca47100a34da2524515b46bcad574
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567170"
---
# <a name="object-detection"></a>Objektidentifiering

Objektidentifiering liknar [taggning](concept-tagging-images.md), men API: et returnerar de omgivande koordinaterna för avgränsningsfält (i bildpunkter) för varje objekt hittades. Om en bild exempelvis innehåller en hund, en katt och en person, kommer identifieringsåtgärden visa en lista över dessa objekt tillsammans med deras koordinater i bilden. Du kan använda den här funktionen för att bearbeta relationerna mellan objekt i en bild. Du kan även se om det finns flera instanser av samma tagg i en bild.

Identifiera API: et gäller taggar baserat på antalet objekt eller en levande saker som identifierats i avbildningen. Observera att nu finns det ingen formell relation mellan taxonomi för taggning och taxonomi som används för objektidentifiering av. På en konceptuell nivå hittar API: et identifiera endast objekt och levande saker, medan tagg-API kan även inkludera sammanhangsberoende termer som ”inom”, som inte går att lokalisera med avgränsar rutorna.

## <a name="object-detection-example"></a>Exempel för identifiering av objekt

Följande JSON-svar visar vad för visuellt innehåll returnerar när du söker efter objekt i exempelbild.

![En kvinna som använder en Microsoft Surface-enhet i en se](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="limitations"></a>Begränsningar

Det är viktigt att notera begränsningarna i funktionen för identifiering av objekt så att du kan undvika eller minimera effekterna av FALSKT negativ (missade objekt) och begränsad information.
* Objekt identifieras vanligtvis inte om de är mycket liten (mindre än 5% av bilden).
* Objekt identifieras vanligtvis inte om de är placerade mycket bra tillsammans (en stack med nivåer, till exempel).
* Objekt särskiljs inte med hjälp av varumärke eller produkt-namn (olika typer av sodavatten på en store-hylla, till exempel). Men du kan få varumärke information från en avbildning med hjälp av den [varumärken identifiering](concept-brand-detection.md) funktionen.

## <a name="use-the-api"></a>Använda API: et
Funktionen för identifiering av objekt är en del av den [analysera bild](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API. Du kan anropa detta API via en intern SDK eller REST-anrop. När du får det fullständiga JSON-svaret helt enkelt parsa strängen för innehållet i den `"objects"` avsnittet.

* [Snabbstart: Analysera en bild (.NET SDK)](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Snabbstart: Analysera en bild (REST API)](./quickstarts/csharp-analyze.md)