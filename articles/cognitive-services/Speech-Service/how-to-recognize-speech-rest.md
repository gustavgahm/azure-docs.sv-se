---
title: Känna igen tal med hjälp av REST-API
titleSuffix: Azure Cognitive Services
description: Lär dig hur du använder tal till Text-API i Cognitive Services tal-tjänst.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: 7c7bbaa986a6efdb82a50048c7c218f96cd4014a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220339"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Känna igen tal med hjälp av REST-API

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

REST API kan användas för att identifiera kort yttranden med hjälp av en HTTP POST-begäran.

REST-API: et är det enklaste sättet att känna igen tal om du inte använder ett språk som stöds av den [SDK](speech-sdk.md). Du gör en HTTP POST-begäran till tjänstslutpunkten och skickar hela uttryck i brödtexten i begäran. Du får ett svar som har den tolkade texten.

> [!NOTE]
> Yttranden är begränsade till 15 sekunder eller mindre när du använder REST-API.
> Kolla in den [tal SDK](how-to-recognize-speech-csharp.md) för igenkänning av längre yttranden.

Mer information om den **tal till Text** REST API kan du läsa den [REST API: er](rest-apis.md#speech-to-text-api) artikeln. Visa API: et i praktiken genom att ladda ned den [REST API-exempel](https://github.com/Azure-Samples/SpeechToText-REST) från GitHub.

## <a name="next-steps"></a>Nästa steg

- Se den [REST API-översikt](rest-apis.md).
