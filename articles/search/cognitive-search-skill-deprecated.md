---
title: Föråldrad kognitiva funktioner – Azure Search
description: Den här sidan innehåller en lista över kognitiv sökning färdigheter som betraktas som inaktuella och stöds inte inom en snar framtid.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: c35e4253858d6820d86d7d3e0763a3dcc577d09d
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54387941"
---
# <a name="deprecated-cognitive-search-skills"></a>Föråldrad kognitiv sökning kunskaper

Det här dokumentet beskriver kognitiva kunskaper som betraktas som inaktuella. Använd följande guide för innehållet:

* Kunskapsnamn på: Namnet på den färdighet upphör att gälla, det mappas till den @odata.type attribut.
* Senaste tillgängliga api-version: Den senaste versionen av Azure Sök offentliga API: N via vilken kompetens som innehåller motsvarande föråldrad färdighet kan vara skapats/uppdaterats.
* Support upphör: Den sista dagen efter vilka motsvarande färdighet anses vara stöds inte. Skapat kompetens bör fortfarande att fungera, men användare rekommenderas för att migrera från en föråldrad färdighet.
* Rekommendationer: Migreringsvägen fram emot att använda en färdighet som stöds. Användare bör följa rekommendationerna för att fortsätta att få support.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft.Skills.Text.NamedEntityRecognitionSkill

### <a name="last-available-api-version"></a>Senaste tillgängliga api-version

2017-11-11-preview

### <a name="end-of-support"></a>Support upphör

Den 15 februari 2019

### <a name="recommendations"></a>Rekommendationer 

Använd [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) i stället. Den innehåller de flesta av funktionerna i NamedEntityRecognitionSkill med högre kvalitet. Det har även mer omfattande information i dess komplexa utdata-fält.

Att migrera till den [entitet erkännande färdighet](cognitive-search-skill-entity-recognition.md), måste du utföra en eller flera av följande ändringar till din kompetens-definition. Du kan uppdatera en färdighet definition med hjälp av den [uppdatera kompetens API](https://docs.microsoft.com/rest/api/searchservice/update-skillset).

_Obs!_ Förtroendepoäng som ett begrepp som stöds för närvarande inte. Det kommer att stödjas inom en snar framtid. Den `minimumPrecision` parametern finns på den `EntityRecognitionSkill` för framtida användning och för bakåtkompatibilitet kompatibilitet.

1. *(Krävs)*  Ändra den `@odata.type` från `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` till `"#Microsoft.Skills.Text.EntityRecognitionSkill"`.

2. *(Valfritt)*  Om du gör användning av den `entities` utdata, Använd den `namedEntities` komplex samling utdata från den `EntityRecognitionSkill` i stället. Du kan använda den `targetName` i kompetensen definitionen för att mappa den till en anteckning med namnet `entities`.

3. *(Valfritt)*  Om du inte uttryckligen anger den `categories`, `EntityRecognitionSkill` kan returnera olika slags kategorier än de som stöddes av den `NamedEntityRecognitionSkill`. Om det här beteendet är oönskade, se till att uttryckligen ställa in den `categories` parameter `["Person", "Location", "Organization"]`.

    _Exemplet migrering definitioner_

    * Enkel migrering

        _(Före) NamedEntityRecognition färdighet definition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
        _(Efter) EntityRecognition färdighet definition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
    
    * Lite komplicerat migrering

        _(Före) NamedEntityRecognition färdighet definition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "entities"
            }
            ]
        }
        ```
        _(Efter) EntityRecognition färdighet definition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person", "Location", "Organization" ],
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "namedEntities",
                "targetName": "entities"
            }
            ]
        }
        ```

## <a name="see-also"></a>Se också

+ [Fördefinierade kunskaper](cognitive-search-predefined-skills.md)
+ [Hur du definierar en kompetens](cognitive-search-defining-skillset.md)
+ [Entiteten erkännande färdighet](cognitive-search-skill-entity-recognition.md)
