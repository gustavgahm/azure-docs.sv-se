---
title: 'Snabbstart: Känna igen tal, C++ (Linux) – Speech Services'
titleSuffix: Azure Cognitive Services
description: Lär dig hur du känner igen tal i C++ på Linux med hjälp av Speech Service SDK
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: wolfma
ms.openlocfilehash: 914f86e8705916af4927aa192a41fc7636b80f3a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228703"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>Snabbstart: Känna igen tal i C++ på Linux med hjälp av Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

I den här artikeln får skapa du ett C++-konsolprogram för Ubuntu Linux 16.04 eller 18.04. Du använder Cognitive Services [Speech SDK](speech-sdk.md) för att transkribera tal till text i realtid från datorns mikrofon. Programmet har skapat med [Speech SDK för Linux](https://aka.ms/csspeech/linuxbinary) och Linux-distributionens C++-kompilerare (till exempel `g++`).

## <a name="prerequisites"></a>Nödvändiga komponenter

Du behöver en prenumerationsnyckel för Speech-tjänsten för att slutföra den här snabbstarten. Du kan skaffa en utan kostnad. Mer information finns i [Prova Speech Service kostnadsfritt](get-started.md).

## <a name="install-speech-sdk"></a>Installera Speech SDK

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Den aktuella versionen av Cognitive Services Speech SDK är `1.2.0`.

Speech SDK för Linux kan användas för att bygga både 64-bitars och 32-bitars program. De nödvändiga biblioteken och huvudfilerna kan laddas ned som en tar-fil från https://aka.ms/csspeech/linuxbinary.

Ladda ned och installera SDK enligt följande:

1. Kontrollera att SDK-beroenden är installerade.

   ```sh
   sudo apt-get update
   sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
   ```

1. Välj en katalog som Speech SDK-filerna ska extraheras till och ange miljövariabeln `SPEECHSDK_ROOT` så att den pekar på den katalogen. Den här variabeln gör det enkelt att referera till katalogen i framtida kommandon. Exempel: Om du vill använda katalogen `speechsdk` i arbetskatalogen använder du ett kommando som liknar följande:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Skapa katalogen om den inte redan finns.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Ladda ned och extrahera arkivet `.tar.gz` som innehåller Speech SDK-binärfilerna:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Validera innehållet i toppnivåkatalogen i det extraherade paketet:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Kataloglistan ska innehålla meddelande från tredje part och licensfiler, samt en `include`-katalog som innehåller huvudfiler (`.h`) och en `lib`-katalog som innehåller bibliotek.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-sample-code"></a>Lägg till exempelkod

1. Skapa en C++-källfil med namnet `helloworld.cpp` och klistra in följande kod i den.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Ersätt strängen `YourSubscriptionKey` i den nya filen med din prenumerationsnyckel för Speech-tjänsten.

1. Ersätt strängen `YourServiceRegion` med den [region](regions.md) som är associerad med din prenumeration (till exempel `westus` för en kostnadsfri provprenumeration).

## <a name="build-the-app"></a>Skapa appen

> [!NOTE]
> Se till att ange kommandon nedan som en _enda kommandorad_. Det enklaste sättet att göra det är att kopiera kommandot med hjälp av **kopieringsknappen** bredvid varje kommando och klistra in den i gränssnittsprompten.

* Kör följande kommando för att skapa programmet i ett **x64**-system (64-bitars).

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Kör följande kommando för att skapa programmet i ett **x86**-system (32-bitars).

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-app"></a>Kör appen

1. Konfigurera inläsarens bibliotekssökväg så att den pekar på Speech SDK-biblioteket.

   * På ett **x64**-system (64-bitars) anger du följande kommando.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * På ett **x86**-system (32-bitars) anger du följande kommando.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Kör appen.

   ```sh
   ./helloworld
   ```

1.  I konsolfönstret visas en uppmaning som ber dig säga något. Säg en engelsk fras eller en mening. Ditt tal överförs till Speech-tjänsten och transkriberas till text som visas i samma fönster.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Utforska C++-exempel på GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Se även

- [Anpassa akustiska modeller](how-to-customize-acoustic-models.md)
- [Anpassa språkmodeller](how-to-customize-language-model.md)
