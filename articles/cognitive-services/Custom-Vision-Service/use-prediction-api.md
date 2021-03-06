---
title: 'Exempel: Använd förutsägelseslutpunkt för att programmatiskt testa bilder med klassificerare – Custom Vision'
titlesuffix: Azure Cognitive Services
description: Lär dig hur du använder API:et för att programmatiskt testa bilder med din klassificerare för Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: sample
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 3a81f3cef6aaeb5c98022d9fc93f4d84f3f58a6e
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363657"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Använd förutsägelseslutpunkten för att testa bilder programmatiskt med en klassificerare för Custom Vision Service

När du tränar din modell kan du testa bilder programmatiskt genom att skicka dem till förutsägelse-API:et. 

> [!NOTE]
> Det här dokumentet visar hur du använder C# för att skicka en bild till förutsägelse-API:et. Mer information och exempel på användning av API:et finns i [Referens för förutsägelse-API](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>Hämta URL och förutsägelsenyckel

Från [Custom Vision-webbsidan](https://customvision.ai), markera projektet och välj sedan fliken __prestanda__. För att visa information om hur du använder förutsägelse-API, inklusive __förutsägelsenyckel__ väljer du __förutsägelse-URL__. För projekt som är kopplade till en Azure-resurs kan din __förutsägelsenyckel__ också hittas i [Azure Portal](https://portal.azure.com) för associerad Azure-resurs under __nycklar__. Kopiera följande information för användning i programmet:

* __URL__ för att använda en __bildfil__.
* Värde för __Förutsägelsenyckel__.

> [!TIP]
> Om du har flera iterationer kan du styra vilken som används genom att ange den som standard. Välj iteration från avsnittet __iterationer__ och välj sedan __ange som standard__ överst på sidan.

![Prestandafliken visas med en röd rektangel runt förutsägelse-URL:en.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>Skapa programmet

1. Skapa ett nytt C#-konsolprogram från Visual Studio.

2. Använd följande kod som brödtext i filen __Program.cs__.

    > [!IMPORTANT]
    > Ändra följande information:
    >
    > * Ange __namnområdet__ till namnet på ditt projekt.
    > * Ange det värde för __förutsägelsenyckeln__ som du fick tidigare på raden som börjar med `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Ange det värde för __URL__ som du fick tidigare på raden som börjar med `string url =`.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CSPredictionSample
    {
        static class Program
        {
            static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }

            static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid subscription key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

                // Prediction URL - replace this example URL with your valid prediction URL.
                string url = "http://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/prediction/d16e136c-5b0b-4b84-9341-6a3fff8fa7fe/image?iterationId=f4e573f6-9843-46db-8018-b01d034fd0f2";

                HttpResponseMessage response;

                // Request body. Try this sample with a locally stored image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (var content = new ByteArrayContent(byteData))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                    response = await client.PostAsync(url, content);
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }
        }
    }
    ```

## <a name="use-the-application"></a>Använd programmet

När du kör programmet, ange sökvägen till en bildfil. Bilden skickas till API:et och resultaten returneras som ett JSON-dokument. Följande JSON är ett exempel på svaret

```json
{
    "Id":"3f76364c-b8ae-4818-a2b2-2794cfbe377a",
    "Project":"2277aca4-7aff-4742-8afb-3682e251c913",
    "Iteration":"84105bfe-73b5-4fcc-addb-756c0de17df2",
    "Created":"2018-05-03T14:15:22.5659829Z",
    "Predictions":[
        {"TagId":"35ac2ad0-e3ef-4e60-b81f-052a1057a1ca","Tag":"dog","Probability":0.102716163},
        {"TagId":"28e1a872-3776-434c-8cf0-b612dd1a953c","Tag":"cat","Probability":0.02037274}
    ]
}
```

## <a name="next-steps"></a>Nästa steg

[Exportera modellen för mobilanvändning](export-your-model.md)
