---
title: Liveuppspelning begrepp i Azure Media Services - Live-händelser och Live utdata | Microsoft Docs
description: Den här artikeln ger en översikt över direktsänd strömning begrepp i Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 02/01/2019
ms.author: juliako
ms.openlocfilehash: db7d47005b2855ffe3e28c43086a2bfa6b22c8f3
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55659525"
---
# <a name="live-events-and-live-outputs"></a>Livehändelser och liveresultat

Azure Media Services kan du leverera händelser till dina kunder på Azure-molnet. Om du vill konfigurera din strömmade direktsändningar av evenemang i Media Services v3, måste du förstå begrepp som diskuteras i den här artikeln:

* [Live-händelser](#live-events)
* [Live händelsetyper](#live-vent-types)
* [Live-händelse typer jämförelse](#live-event-types-comparison)
* [Live-alternativ för skapande av händelse](#live-event-creation-options)
* [Live-händelse mata in URL: er](#live-event-ingest-urls)
* [Live-händelse förhandsgransknings-URL](#live-event-preview-url)
* [Live utdata](#live-outputs).

## <a name="live-events"></a>Livehändelser

[Live-händelser](https://docs.microsoft.com/rest/api/media/liveevents) ansvarar för att mata in och bearbeta direktsänd video feeds. När du skapar en Live-händelse skapas en slutpunkt för indata som du kan använda för att skicka en direktsänd signal från en fjärransluten kodare. Remote livekodare skickar bidraget till som indata slutpunkten med hjälp av antingen den [RTMP](https://www.adobe.com/devnet/rtmp.html) eller [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (fragmenterad MP4)-protokollet. För Smooth Streaming-inmatningsprotokollet, URL-scheman som stöds är `http://` eller `https://`. RTMP-infogningsprotokollet, URL-scheman som stöds är `rtmp://` eller `rtmps://`. 

## <a name="live-event-types"></a>Live händelsetyper

En [direktsänd händelse](https://docs.microsoft.com/rest/api/media/liveevents) kan vara något av två typer: direkt och live encoding. 

### <a name="pass-through"></a>Direkt

![direktautentisering](./media/live-streaming/pass-through.png)

När du använder direkt **direktsänd händelse**, du förlita dig på din lokala livekodare för att generera en videoström för flera bithastigheter och skicka att som bidraget feed på Live-händelsen (med RTMP eller fragmenterad MP4-protokollet). Live-händelsen har sedan via inkommande video strömmar utan vidare bearbetning. En direkt LiveEvent är optimerad för tidskrävande direktsändningar eller 24 x 365 linjär liveuppspelning. När du skapar den här typen av direktsänd händelse kan du ange None (LiveEventEncodingType.None).

Du kan skicka bidraget feed på lösningar upp till 4 K och vid en bildruta andelen 60 bildrutor per sekund, med H.264/AVC eller H.265/HEVC codec för indatavideo och AAC (AAC-LC, HE-AACv1 eller HE-AACv2) ljudcodec.  Se den [direktsänd händelse skriver jämförelse](live-event-types-comparison.md) nedan för mer information.

> [!NOTE]
> Genomströmningsmetoden är det mest ekonomiska sättet för liveuppspelning när du utför flera händelser under en längre tid och du redan har investerat i lokala kodare. Se [prisuppgifter](https://azure.microsoft.com/pricing/details/media-services/).
> 

Se ett .NET-kodexempel i [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![Live encoding](./media/live-streaming/live-encoding.png)

När du använder live encoding med Media Services, kan du konfigurera din lokala livekodare för att skicka en enkel bithastighet video som bidrag till Live-händelse (med RTMP eller fragmenterad Mp4-protokollet). Live-händelsen kodar den inkommande, enkel bithastigheten, strömma till en [flera video bithastighet](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), gör den tillgänglig för leverans för uppspelning av protokoll som MPEG-DASH, HLS och Smooth Streaming-enheter. När du skapar den här typen av direktsänd händelse, ange kodningstyp som **Standard** (LiveEventEncodingType.Standard).

Du kan skicka bidraget feed på upp till 1080 p upplösning vid en bildfrekvens av 30 bilder/sekund, med H.264/AVC video-codec och AAC (AAC-LC, HE-AACv1 eller HE-AACv2) ljudcodec. Se den [direktsänd händelse skriver jämförelse](live-event-types-comparison.md) nedan för mer information.

## <a name="live-event-creation-options"></a>Live-alternativ för skapande av händelse

När du skapar en Live-händelse, kan du ange följande alternativ:

* Strömningsprotokoll för Live-händelse (stöds för närvarande protokollen RTMP och Smooth Streaming).<br/>Du kan inte ändra protokollalternativ när direktsänd händelse eller dess associerade Live utdata körs. Om du behöver olika protokoll, bör du skapa separata direktsänd händelse för varje strömningsprotokoll.  
* IP-begränsningar på infogning och förhandsgranskning. Du kan definiera de IP-adresser som får mata in en video till den här Live-händelsen. Tillåtna IP-adresser kan anges som en enskild IP-adress (till exempel 10.0.0.1), ett IP-intervall med IP-adress och en CIDR-nätmask (till exempel 10.0.0.1/22) eller ett IP-intervall med en IP-adress och en prickad decimalnätmask (till exempel 10.0.0.1(255.255.252.0)).<br/>Om inga IP-adresser har angetts och det saknas regeldefinitioner, kommer ingen IP-adress att tillåtas. Skapa en regel för att tillåta IP-adresser och ange 0.0.0.0/0.<br/>IP-adresserna måste vara i något av följande format: IpV4-adress med 4 siffror, CIDR-adressintervall.
* När du skapar händelsen, kan du ange att den ska startas automatiskt. <br/>När autostart är angett till true (sant) startas live-händelsen efter skapandet. Faktureringen startar när Live-händelsen börjar köras. Du måste explicit anropa Stop på resursen för direktsänd händelse att stoppa ytterligare fakturering. Du kan också starta händelsen när du är redo att börja direktuppspelning. 

    Mer information finns i [direktsänd händelse tillstånd och fakturering](live-event-states-billing.md).

## <a name="live-event-ingest-urls"></a>Live-händelse mata in URL: er

När Live-händelsen har skapats kan du mata in URL: er som du tillhandahåller till kodaren lokalt. Live Encoding använder dessa URL: er för att ange en direktsänd dataström. Mer information finns i [rekommenderas en lokal livekodare](recommended-on-premises-live-encoders.md). 

Du kan antingen använda icke-alternativa URL: er eller alternativa URL: er. 

* Icke-anpassad URL

    Icke-anpassad URL är standardläget i AMS v3. Du eventuellt hämta Live-händelse snabbt men infognings-URL är känd endast när direktsändningen startas. URL-Adressen ändras om du stoppa/starta Live-händelsen. <br/>Icke-anpassad är användbart i situationer när en användare vill strömma använder en app där appen vill skaffa en direktsändning ASAP och har en dynamisk infognings-URL är inte ett problem.
* Anpassad URL

    Anpassad läge föredras av stora media sändningsföretag som använder maskinvara broadcast-kodare och inte vill konfigurera sina kodare när de startar Live-händelsen. De vill ha en förutsägande infognings-URL som inte ändras med tiden.

> [!NOTE] 
> För en inmatning URL: en ska vara förutsägande, måste du använda ”anpassad” läge och skicka din egen åtkomst-token (för att undvika en slumpmässig token i URL: en).

### <a name="live-ingest-url-naming-rules"></a>Liveinmatning regler för namngivning av URL: en

Den *slumpmässiga* strängen nedan är ett 128-bitars hexadecimalt tal (som består av 32 tecken mellan 0 och 9 a – f).<br/>
Den *åtkomsttoken* nedan visas vad du behöver ange för fast URL: en. Det är också 128-bitars hexadecimalt tal.

#### <a name="non-vanity-url"></a>Icke-anpassad URL

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/<access token>`
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/<access token>`
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/<access token>`
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/<access token>`

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<random 128bit hex string>.channel.media.azure.net/<access token>/ingest.isml`
`https://<random 128bit hex string>.channel.media.azure.net/<access token>/ingest.isml`

#### <a name="vanity-url"></a>Anpassad URL

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/<access token>`
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/<access token>`
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/<access token>`
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/<access token>`

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<access token>/ingest.isml`
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<access token>/ingest.isml`

## <a name="live-event-preview-url"></a>Live-händelse förhandsgransknings-URL

När den **direktsänd händelse** börjar ta emot bidrag feed, dess förhandsgranskningsslutpunkten kan använda för att förhandsgranska och validera att du får den direktsända dataströmmen innan du publicerar ytterligare. När du har kontrollerat att förhandsgranska dataströmmen är bra, du kan använda LiveEvent så att den direktsända dataströmmen analysleverans via en eller flera (färdiga) **Strömningsslutpunkter**. För att åstadkomma detta måste du skapa ett nytt [Live utdata](https://docs.microsoft.com/rest/api/media/liveoutputs) på den **direktsänd händelse**. 

> [!IMPORTANT]
> Kontrollera att videon flödar till förhandsgransknings-URL innan du fortsätter!

## <a name="live-outputs"></a>Liveutdata

När du har dataströmmen väl flödar till Live-händelse, kan du påbörja strömningshändelsen genom att skapa en [tillgången](https://docs.microsoft.com/rest/api/media/assets), [Live utdata](https://docs.microsoft.com/rest/api/media/liveoutputs), och [Strömningspositionerare](https://docs.microsoft.com/rest/api/media/streaminglocators). Live utdata kommer arkiverar dataströmmen och gör den tillgänglig för visning via den [Strömningsslutpunkt](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

> [!NOTE]
> Live utdata start vid skapandet och avbryts när tas bort. När du tar bort Live-utdata kan du inte tar bort den underliggande tillgången och innehållet i tillgången. 

Förhållandet mellan en **direktsänd händelse** och dess **Live utdata** är liknar traditionella TV-sändning, där en kanal (**direktsänd händelse**) representerar en konstant strömma video och en inspelning (**Live utdata**) är begränsad till en viss tidpunkt-segment (till exempel kvällar nyheter från 18:30:00 till 19:00:00). Du kan spela TV med hjälp av en Digital Video Recorder (DVR) – den motsvarande funktionen i Live-händelser hanteras via den **ArchiveWindowLength** egenskapen. Det är en ISO 8601-timespan varaktighet (till exempel PTHH:MM:SS) som anger kapaciteten för DVR och kan anges från minst 3 minuter till högst 25 timmar.

Den **Live utdata** objekt som liknar en inspelningar som ska fånga upp och registrera den direktsända dataströmmen till en tillgång i Media Services-kontot. Inspelat innehåll ska behållas i Azure Storage-kontot som hör till ditt konto till den behållare som definieras av tillgången resursen. Den **Live utdata** också låter dig styra vissa egenskaper för den utgående direktsända dataströmmen, till exempel hur mycket av dataströmmen som sparas i arkivet inspelningen (till exempel kapaciteten för moln DVR) och om huruvida användarna kan starta Titta på den direktsända dataströmmen. Arkivet på disken är en cirkulär Arkiv ”fönster” som innehåller endast mängden innehåll som har angetts i den **archiveWindowLength** egenskapen för den **Live utdata**. Innehållet som ligger utanför det här fönstret tas automatiskt bort från storage-behållare och kan inte återställas. Du kan skapa flera **Live utdata** (upp till tre maximalt) på en **direktsänd händelse** med olika Arkiv längd och inställningar.  

Om du har publicerat den **Live utdata**'s **tillgången** med hjälp av en **Strömningspositionerare**, **direktsänd händelse** (upp till DVR fönstret längd) kommer fortsätta att vara synliga tills positionerare för direktuppspelning slutar att gälla eller tas bort, beroende på vilket som inträffar först.

Mer information finns i [med molnbaserade DVR](live-event-cloud-dvr.md).

## <a name="next-steps"></a>Nästa steg

- [Strömma liveevenemang](live-streaming-overview.md)
- [Live direktuppspelning självstudien](stream-live-tutorial-with-api.md)
