---
title: Stöd för språk
titleSuffix: Azure Cognitive Services
description: LUIS har olika funktioner i tjänsten. Alla funktioner är inte på samma språkparitet. Kontrollera att de funktioner som du är intresserad av stöds i kulturen för språk som du arbetar med. En LUIS-app är kultur-specifika och kan inte ändras när den är inställd.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: 85bd7b1c150eaa23ec5fa438a8fcbe2d5eb66f45
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220425"
---
# <a name="language-and-region-support-for-luis"></a>Stöd för språk och din region för LUIS

LUIS har olika funktioner i tjänsten. Alla funktioner är inte på samma språkparitet. Kontrollera att de funktioner som du är intresserad av stöds i kulturen för språk som du arbetar med. En LUIS-app är kultur-specifika och kan inte ändras när den är inställd.

## <a name="multi-language-luis-apps"></a>Flera language Understanding Intelligent Service-appar

Om du behöver ett klientprogram för flera språk LUIS som en chattrobot finns ett par alternativ. Om LUIS har stöd för alla språk, kan du utveckla en LUIS-app för varje språk. Varje LUIS-app har ett unikt app-ID och slutpunkten log. Om du måste ange språkförståelse för ett språk som LUIS inte stöder, du kan använda [Microsoft Translator API](../Translator/translator-info-overview.md) skicka uttryck till LUIS-slutpunkten för att översätta uttryck till ett språk som stöds, och ta emot den resulterande poäng.

## <a name="languages-supported"></a>Språk som stöds

LUIS förstår yttranden på följande språk:

| Språk |Nationell inställning  |  Fördefinierade domän | Fördefinierade entitet | Fras förslag | **[Textanalys](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)<br>(Känsla och<br>Nyckelord)|
|--|--|:--:|:--:|:--:|:--:|
| Amerikansk engelska |`en-US` | ✔ | ✔  |✔|✔|
| *[Kinesiska](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Nederländska |`nl-NL` |-|  -   |-|✔|
| Franska (Frankrike) |`fr-FR` |-| ✔ |✔ |✔|
| Franska (Kanada) |`fr-CA` |-|   -   |-|✔|
| Tyska |`de-DE` |-| ✔ |✔ |✔|
| Italienska |`it-IT` |-| ✔ |✔|✔|
| *[Japanska](#japanese-support-notes) |`ja-JP` |-| ✔ |✔|Endast diskussionsämne|
| Koreanska |`ko-KR` |-|   -   |-|Endast diskussionsämne|
| Portugisiska (Brasilien) |`pt-BR` |-| ✔ |✔ |inte alla underordnade kulturer|
| Spanska (Spanien) |`es-ES` |-| ✔ |✔|✔|
| Spanska (Mexiko)|`es-MX` |-|  -   |✔|✔|
| turkiska | `tr-TR` |-|-|-|Endast sentiment|


Språkstöd varierar för [förskapade entiteter](luis-reference-prebuilt-entities.md) och [fördefinierade domäner](luis-reference-prebuilt-domains.md).

### <a name="chinese-support-notes"></a>* Kinesiska stöder anteckningar

 - I den `zh-cn` kulturen, LUIS förväntar sig förenklad kinesiska teckenuppsättningen i stället för den traditionella teckenuppsättningen.
 - Namn på avsikter, entiteter, funktioner och reguljära uttryck kan vara i kinesiska eller latinska tecken.
 - Se den [fördefinierade domäner referens ](luis-reference-prebuilt-domains.md) information där fördefinierade domäner som stöds i den `zh-cn` kultur.
<!--- When writing regular expressions in Chinese, do not insert whitespace between Chinese characters.-->

### <a name="japanese-support-notes"></a>* Japanska stöder anteckningar

 - Eftersom LUIS stöder inte syntaktiska analys och kommer inte att förstå skillnaden mellan Keigo och informell japanska, måste du lägga till de olika nivåerna av förfarande utbildning exempel för dina program.
     - でございます är inte samma som です.
     - です är inte samma som だ.

### <a name="text-analytics-support-notes"></a>** Text analytics har stöd för anteckningar
Text analytics innehåller keyPhrase fördefinierade entitets- och attityd-analys. Endast portugisiska stöds för subkulturer: `pt-PT` och `pt-BR`. Andra kulturer som stöds på primära kultur-nivå. Mer information om textanalys [språk som stöds](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages).

### <a name="speech-api-supported-languages"></a>Talspråk för API som stöds
Se tal [språk som stöds](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode) för talspråk diktering läge.

### <a name="bing-spell-check-supported-languages"></a>Stavningskontroll i Bing stöds språk
Se stavningskontroll i Bing [språk som stöds](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) en lista över språk som stöds och status.

## <a name="rare-or-foreign-words-in-an-application"></a>Sällsynta eller främmande ord i ett program
I den `en-us` kulturen, LUIS lär sig att skilja mest svenska ord, inklusive slang. I den `zh-cn` kulturen, LUIS lär sig att skilja mellan de flesta kinesiska tecken. Om du använder ett ovanligt ord i `en-us` eller tecknet i `zh-cn`, och du ser att LUIS verkar det går inte att skilja den ord eller tecken, du kan lägga till den ord eller tecken till en [funktionen frasen lista](luis-how-to-add-features.md). Till exempel ska utanför programmet – det vill säga främmande ord--kultur läggas till en fras-list-funktion. Den här frasen listan bör markeras ej utbytbara, att indikera att uppsättning sällsynta ord utgör en klass som LUIS bör lär sig att känna igen, men de är inte synonymer eller utbytbara med varandra.

### <a name="hybrid-languages"></a>Hybrid-språk
Hybrid språk kombinera ord från två kulturer, till exempel engelska och kinesiska. De här språken stöds inte i LUIS eftersom en app är baserad på en enda kultur.

## <a name="tokenization"></a>Tokenisering
Om du vill utföra maskininlärning LUIS delar ett uttryck i [token](luis-glossary.md#token) baserat på kultur.

|Språk|  varje blanksteg eller specialtecken | tecknet nivå|sammansatta ord|[principfilerna entitet returnerades](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|Kinesiska||✔||✔|
|Nederländska|||✔|✔|
|Engelska (en-us)|✔ ||||
|Franska (fr-FR)|✔||||
|Franska (fr-CA)|✔||||
|Tyska|||✔|✔|
|Italienska|✔||||
|Japanska||||✔|
|Koreanska||✔||✔|
|Portugisiska (Brasilien)|✔||||
|Spanska (es-ES)|✔||||
|Spanska (es-MX)|✔||||
