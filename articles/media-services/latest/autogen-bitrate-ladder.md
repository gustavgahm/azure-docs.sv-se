---
title: Koda videor med Standard-kodare i Media Services – Azure | Microsoft Docs
description: Det här avsnittet visar hur du använder Standard-kodare i Media Services för att koda indata video med en automatiskt genererad bithastighetsstege, baserat på inkommande upplösning och bithastighet.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 976e1ea013c6a30615bdc742ed79e76ec35258a1
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53132814"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Koda med en automatiskt genererad bithastighetsstege

## <a name="overview"></a>Översikt

Den här artikeln förklarar hur du använder Standard-kodare i Media Services för att koda ett indatavideon till en automatiskt genererad bithastighetsstege (bithastighet upplösning par) baserat på inkommande upplösning och bithastighet. Den här inbyggda encoder inställning eller förinställning, kan inte överstiga inkommande upplösning och bithastighet. Till exempel om indata är 720p med 3 Mbit/s, utdata förblir 720p i bästa och börjar avgifterna lägre än 3 Mbit/s.

### <a name="encoding-for-streaming"></a>Kodning för direktuppspelning

När du använder den **AdaptiveStreaming** förinställda i **transformera**, du får utdata som passar för leverans via strömningsprotokoll som HLS och DASH. När du använder den här förinställning tjänsten smart avgör hur många video lager för att generera och på vilka bithastighet och upplösning. Utdatainnehållet finns där AAC-kodad ljud och video H.264-kodad inte är överlagrad MP4-filer.

Om du vill se ett exempel på hur denna inställning används, se [Stream en fil](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Resultat

Det här avsnittet visar tre exempel på utdata video lagren produceras av Media Services encoder till följd av kodning med den **AdaptiveStreaming** förinställda. I samtliga fall innehåller resultatet en ljuddata MP4-fil med stereo ljud kodade med 128 kbit/s.

### <a name="example-1"></a>Exempel 1
Källan med höjd ”1080” och ramhastighet ”29.970” ger 6 video lager:

|Lager|Höjd|Bredd|Bithastighet (kbit/s)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Exempel 2
Källan med höjd ”720” och ramhastighet ”23.970” producerar 5 video lager:

|Lager|Höjd|Bredd|Bithastighet (kbit/s)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Exempel 3
Källan med höjd ”360” och ramhastighet ”29.970” ger 3 video lager:

|Lager|Höjd|Bredd|Bithastighet (kbit/s)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Strömma en fil](stream-files-dotnet-quickstart.md)
