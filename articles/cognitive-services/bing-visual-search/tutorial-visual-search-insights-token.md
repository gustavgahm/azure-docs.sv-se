---
title: Hitta liknande bilder från tidigare sökningar med ImageInsightsToken - Bing Visual Search
titlesuffix: Azure Cognitive Services
description: 'Använd Bing Visual Search SDK för att hämta URL: er för bilder som anges av ImageInsightsToken.'
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: article
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 540c4132789d0d1ab666d452b5d6fb220025d5dc
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55157388"
---
# <a name="find-similar-images-from-previous-searches-using-imageinsightstoken"></a>Hitta liknande bilder från tidigare sökningar med ImageInsightsToken

Visual Search SDK gör det möjligt för dig att hitta avbildningar online från tidigare sökningar som returnerar en `ImageInsightsToken`.  Det här programmet hämtar ett `ImageInsightsToken` och använder token i en efterföljande sökning. Den skickar sedan den `ImageInsightsToken` till Bing och returnerar resultat som innehåller URL: er för Bing-sökning och URL: er av liknande bilder som finns online.

Den fullständiga källkoden för det här exemplet finns med ytterligare felhantering och kommentarer på [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchInisghtsTokens.cs).

## <a name="prerequisites"></a>Förutsättningar

* Valfri version av [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Om du använder Linux/Mac OS kan det här programmet köras med [Mono](http://www.mono-project.com/).
* NuGet Visual Search och bildsökning paket. 
    - Från Solution Explorer i Visual Studio högerklickar du på projektet och väljer `Manage NuGet Packages` på menyn. Installera den `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketet, och `Microsoft.Azure.CognitiveServices.Search.ImageSearch` paketet. Installering av NuGet-paketet installerar även följande:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="get-the-imageinsightstoken-from-the-bing-image-search-sdk"></a>Hämta ImageInsightsToken från SDK för Bing-bildsökning

Det här programmet använder ett `ImageInsightsToken` erhålls via den [Bing bild Search SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart). I en ny C# konsolapp, skapa en klient för att anropa API med hjälp av `ImageSearchAPI()`. Använd sedan `SearchAsync()` med din fråga.

```csharp
var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
Console.WriteLine("Search images for query \"canadian rockies\"");
```

Den första sökningen resultat med Store `imageResults.Value.First()`, och sedan lagra den avbildning insight `ImageInsightsToken`. 

```csharp
String insightTok = "None";
if (imageResults.Value.Count > 0)
{
    var firstImageResult = imageResults.Value.First();
    insightTok = firstImageResult.ImageInsightsToken;
}
else
{
    insightTok = "None found";
    Console.WriteLine("Couldn't find image results!");
}
```

Detta `ImageInsightsToken` kommer att skickas till Bing Visual Search i en begäran.

## <a name="add-the-imageinsightstoken-to-a-visual-search-request"></a>Lägg till ImageInsightsToken i en begäran om Visual Search

Ange den `ImageInsightsToken` Visual Search begäran genom att skapa en `ImageInfo` objekt från den `ImageInsightsToken` i svar från Bing Visual Search. 

```csharp
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-bing-visual-search-to-find-images-from-an-imageinsightstoken"></a>Använd Bing Visual Search för att hitta bilder från en ImageInsightsToken

Den `VisualSearchRequest` objekt innehåller information om avbildningen i `ImageInfo` som ska sökas igenom. `VisualSearchMethodAsync()`-metoden hämtar resultatet. Du behöver att ge en bild-binär, enligt bilden representeras av token.

```csharp
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;

```

## <a name="iterate-through-the-visual-search-results"></a>Gå igenom de Visual Search-resultat

Resultaten från Visuell sökning är `ImageTag`-objekt.  Varje tagg innehåller en lista med `ImageAction`-objekt.  Varje `ImageAction` innehåller en `Data` fält som är en lista med värden som beror på vilken typ av åtgärd. Du kan gå igenom den `ImageTag` objekt i `visualSearchResults.Tags`för instansen och hämta den `ImageAction` taggen i den. Exemplet nedan skriver ut information om `PagesIncluding` åtgärder.

```csharp
if (visualSearchResults.Tags.Count > 0)
{
    // List of tags
    foreach (ImageTag t in visualSearchResults.Tags)
    {
        foreach (ImageAction i in t.Actions)
        {
            Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " WebSearchURL: " + i.WebSearchUrl);

            if (i.ActionType == "PagesIncluding")
            {
                foreach (ImageObject o in (i as ImageModuleAction).Data.Value)
                {
                    Console.WriteLine("ContentURL: " + o.ContentUrl);
                }
            }
        }
    }
}
```

### <a name="pagesincluding-actiontypes"></a>PagesIncluding ActionTypes

De faktiska bild-URL: er från åtgärdstyper kräver en typkonvertering som läser en `ActionType` som `ImageModuleAction`, som innehåller en `Data` element med en lista med värden.  Varje värde är URL:en till en bild.  Nedanstående omvandlar `PagesIncluding`-åtgärdstypen till `ImageModuleAction` och läser värdena.

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

Mer information om dessa datatyper finns i [Bilder – Visuell sökning](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).


## <a name="returned-urls"></a>Returnerade URL: er

Hela appen returnerar följande webbadresser:

|Åtgärdstyp  |URL  | |
|---------|---------|---------|
|MoreSizes -> WebSearchUrl     |         |         
|VisualSearch -> WebSearchUrl     |         |         
|ImageById -> WebSearchUrl    |         |         
|RelatedSearches -> WebSearchUrl:    |         |         
|DocumentLevelSuggestions -> WebSearchUrl:     |         |         
|TopicResults -> WebSearchUrl    | https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1       |         
|ImageResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1       |         

Enligt ovan, den `TopicResults` och `ImageResults` typer innehåller frågor om relaterade bilder. Länken URL: er till Bing-sökresultat.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Skapa en enkelsidig webbapp](tutorial-bing-visual-search-single-page-app.md)
