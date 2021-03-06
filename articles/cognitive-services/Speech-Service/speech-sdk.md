---
title: Om Taltjänst-SDK - Taltjänster
titleSuffix: Azure Cognitive Services
description: Tal tjänsten Software Development Kit (SDK) får dina program inbyggda funktioner i tjänsten tal, vilket gör det enklare att utveckla programvara. Den här artikeln innehåller mer information om SDK för Windows, Linux och Android.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 1b58ef5f8254ef81eceefa277e5f146531fa2947
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221801"
---
# <a name="about-the-speech-service-sdk"></a>Om Taltjänst-SDK

Tal tjänsten Software Development Kit (SDK) får dina program inbyggda funktioner i tjänsten tal, vilket gör det enklare att utveckla programvara. För närvarande SDK ger åtkomst till **tal till Text**, **Talöversättning**, och **avsikt erkännande**.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Hämta SDK

### <a name="windows"></a>Windows

För Windows stöder vi följande språk:

* C#UWP och .NET, C++: Du kan referera till och använder den senaste versionen av vår tal SDK NuGet-paketet. Paketet innehåller 32-bitars och 64-bitars klientbibliotek och hanterade (.NET)-bibliotek. SDK: N kan installeras i Visual Studio med hjälp av NuGet. Sök efter **Microsoft.CognitiveServices.Speech**.

* Java: Du kan referera till och använder den senaste versionen av vår tal SDK Maven-paketet som stöder endast Windows x64. I Maven-projekt lägger du till `https://csspeechstorage.blob.core.windows.net/maven/` som ytterligare lagringsplats och referens `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` som ett beroende.

### <a name="linux"></a>Linux

> [!NOTE]
> För närvarande kan stöder vi endast Ubuntu 16.04 och 18.04 på en dator (x86 eller x64 för utveckling med C++ och x64 för .NET Core och Java).

Kontrollera att du har de nödvändiga kompilatorn och bibliotek som är installerade genom att köra följande kommandon för shell:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

* C#: Du kan referera till och använder den senaste versionen av vår tal SDK NuGet-paketet. Lägg till följande paketreferens i projektet om du vill referera till SDK:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.2.0" />
  ```

* Java: Du kan referera till och använder den senaste versionen av vår tal SDK Maven-paketet. I Maven-projekt lägger du till `https://csspeechstorage.blob.core.windows.net/maven/` som ytterligare lagringsplats och referens `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` som ett beroende.

* C++: Hämta SDK: N som en [.tar-paketet](https://aka.ms/csspeech/linuxbinary) och packa upp filerna i en katalog med ditt val. I följande tabell visas mappstrukturen SDK:

  |Sökväg|Beskrivning|
  |-|-|
  |`license.md`|Licens|
  |`ThirdPartyNotices.md`|Meddelanden från tredje part|
  |`include`|Header-filer för C och C++|
  |`lib/x64`|Interna x64 bibliotek för att länka med ditt program|
  |`lib/x86`|Interna x86 bibliotek för att länka med ditt program|

  Om du vill skapa ett program, kopiera eller flytta nödvändiga binärfiler (och bibliotek) i din utvecklingsmiljö. Lägga till dem som krävs i Byggprocessen.

### <a name="android"></a>Android

Java SDK för Android kommer som ett [AAR (Android-biblioteket)](https://developer.android.com/studio/projects/android-library), som innehåller de nödvändiga bibliotek och behörigheter som krävs för Android. Den finns i ett Maven-centrallager på `https://csspeechstorage.blob.core.windows.net/maven/` som paket `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0`.

Om du vill använda paketet från Android Studio-projekt gör du följande ändringar:

* I projektet på servernivå build.gradle-filen lägger du till följande för att den `repository` avsnittet:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* I modul på servernivå build.gradle-filen lägger du till följande för att den `dependencies` avsnittet:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.2.0'
  ```

Java SDK är också en del av den [tal Devices SDK](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Nästa steg

* [Hämta en kostnadsfri utvärderingsprenumeration på Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Se hur du kan känna igen tal i C#](quickstart-csharp-dotnet-windows.md)
