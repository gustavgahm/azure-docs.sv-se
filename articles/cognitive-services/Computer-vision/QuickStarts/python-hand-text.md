---
title: 'Snabbstart: Extrahera handskriven text – REST, Python'
titleSuffix: Azure Cognitive Services
description: I den här snabbstarten ska du extrahera handskriven text från en bild med hjälp av API för visuellt innehåll med Python.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 69a2c457b7971cb856507393851317769c99a5e2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209204"
---
# <a name="quickstart-extract-handwritten-text-using-the-rest-api-and-python-in-computer-vision"></a>Snabbstart: Extrahera handskriven text med hjälp av REST-API och Python i Visuellt innehåll

I den här snabbstarten extraherar du handskriven text från en bild med hjälp av REST API för visuellt innehåll. Med hjälp av metoderna [Recognize Text](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) (Identifiera text) och [Get Recognize Text Operation Result](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) (Hämta resultat för åtgärden Identifiera text) kan du identifiera handskriven text i en bild och sedan extrahera identifierade tecken till en teckenström som kan användas på en dator.

> [!IMPORTANT]
> Till skillnad från metoden [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) så körs metoden [Identifiera text](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) asynkront. Den här metoden returnerar inte någon information i en svarsbrödtext. I stället returnerar metoden Identifiera text en URI i värdet i svarsrubrikfältet `Operation-Content`. Du kan sedan anropa denna URI, som representerar metoden [Hämta resultat för åtgärden Identifiera text](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201), så att du både kan kontrollera status och returnera resultaten för metodanropet Identifiera text.

Du kan köra den här snabbstarten steg för steg med hjälp av en Jupyter-anteckningsbok på [MyBinder](https://mybinder.org). Starta Binder med den här knappen:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) innan du börjar.

## <a name="prerequisites"></a>Nödvändiga komponenter

- Du måste ha [Python](https://www.python.org/downloads/) installerat om du vill köra exemplet lokalt.
- Du måste ha en prenumerationsnyckel för Visuellt innehåll. Du kan skaffa en prenumerationsnyckel genom att följa anvisningarna i [Skaffa prenumerationsnycklar](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample"></a>Skapa och köra exemplet

Så här skapar du och kör exemplet:

1. Kopiera följande kod till en textredigerare.
1. Gör nedanstående ändringar i koden där det behövs:
    1. Ersätt värdet för `subscription_key` med din prenumerationsnyckel.
    1. Ersätt värdet för `vision_base_url` med slutpunktens API för visuellt innehåll i den Azure-region där du fick dina prenumerationsnycklar, om så behövs.
    1. Du kan också ersätta värdet för `image_url` med webbadressen till en annan bild från vilken du vill extrahera handskriven text.
1. Spara koden som en fil med tillägget `.py`. Till exempel `get-handwritten-text.py`.
1. Öppna ett kommandotolksfönster.
1. Kör exemplet i kommandotolken med kommandot `python`. Till exempel `python get-handwritten-text.py`.

```python
import requests
import time
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Polygon
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the "westus" region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

text_recognition_url = vision_base_url + "recognizeText"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" + \
    "Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
# Note: The request parameter changed for APIv2.
# For APIv1, it is 'handwriting': 'true'.
params  = {'mode': 'Handwritten'}
data    = {'url': image_url}
response = requests.post(
    text_recognition_url, headers=headers, params=params, json=data)
response.raise_for_status()

# Extracting handwritten text requires two API calls: One call to submit the
# image for processing, the other to retrieve the text found in the image.

# Holds the URI used to retrieve the recognized text.
operation_url = response.headers["Operation-Location"]

# The recognized text isn't immediately available, so poll to wait for completion.
analysis = {}
poll = True
while (poll):
    response_final = requests.get(
        response.headers["Operation-Location"], headers=headers)
    analysis = response_final.json()
    time.sleep(1)
    if ("recognitionResult" in analysis):
        poll= False 
    if ("status" in analysis and analysis['status'] == 'Failed'):
        poll= False

polygons=[]
if ("recognitionResult" in analysis):
    # Extract the recognized text, with bounding boxes.
    polygons = [(line["boundingBox"], line["text"])
        for line in analysis["recognitionResult"]["lines"]]

# Display the image and overlay it with the extracted text.
plt.figure(figsize=(15, 15))
image = Image.open(BytesIO(requests.get(image_url).content))
ax = plt.imshow(image)
for polygon in polygons:
    vertices = [(polygon[0][i], polygon[0][i+1])
        for i in range(0, len(polygon[0]), 2)]
    text     = polygon[1]
    patch    = Polygon(vertices, closed=True, fill=False, linewidth=2, color='y')
    ax.axes.add_patch(patch)
    plt.text(vertices[0][0], vertices[0][1], text, fontsize=20, va="top")
_ = plt.axis("off")
```

## <a name="examine-the-response"></a>Granska svaret

Ett svar som anger att åtgärden lyckades returneras i JSON. Exempelwebbsidan parsar och visar ett lyckat svar i kommandotolkens fönster enligt följande exempel:

```json
{
  "status": "Succeeded",
  "recognitionResult": {
    "lines": [
      {
        "boundingBox": [
          2,
          52,
          65,
          46,
          69,
          89,
          7,
          95
        ],
        "text": "dog",
        "words": [
          {
            "boundingBox": [
              0,
              59,
              63,
              43,
              77,
              86,
              3,
              102
            ],
            "text": "dog"
          }
        ]
      },
      {
        "boundingBox": [
          6,
          2,
          771,
          13,
          770,
          75,
          5,
          64
        ],
        "text": "The quick brown fox jumps over the lazy",
        "words": [
          {
            "boundingBox": [
              0,
              4,
              92,
              5,
              77,
              71,
              0,
              71
            ],
            "text": "The"
          },
          {
            "boundingBox": [
              74,
              4,
              189,
              5,
              174,
              72,
              60,
              71
            ],
            "text": "quick"
          },
          {
            "boundingBox": [
              176,
              5,
              321,
              6,
              306,
              73,
              161,
              72
            ],
            "text": "brown"
          },
          {
            "boundingBox": [
              308,
              6,
              387,
              6,
              372,
              73,
              293,
              73
            ],
            "text": "fox"
          },
          {
            "boundingBox": [
              382,
              6,
              506,
              7,
              491,
              74,
              368,
              73
            ],
            "text": "jumps"
          },
          {
            "boundingBox": [
              492,
              7,
              607,
              8,
              592,
              75,
              478,
              74
            ],
            "text": "over"
          },
          {
            "boundingBox": [
              589,
              8,
              673,
              8,
              658,
              75,
              575,
              75
            ],
            "text": "the"
          },
          {
            "boundingBox": [
              660,
              8,
              783,
              9,
              768,
              76,
              645,
              75
            ],
            "text": "lazy"
          }
        ]
      },
      {
        "boundingBox": [
          2,
          84,
          783,
          96,
          782,
          154,
          1,
          148
        ],
        "text": "Pack my box with five dozen liquor jugs",
        "words": [
          {
            "boundingBox": [
              0,
              86,
              94,
              87,
              72,
              151,
              0,
              149
            ],
            "text": "Pack"
          },
          {
            "boundingBox": [
              76,
              87,
              164,
              88,
              142,
              152,
              54,
              150
            ],
            "text": "my"
          },
          {
            "boundingBox": [
              155,
              88,
              243,
              89,
              222,
              152,
              134,
              151
            ],
            "text": "box"
          },
          {
            "boundingBox": [
              226,
              89,
              344,
              90,
              323,
              154,
              204,
              152
            ],
            "text": "with"
          },
          {
            "boundingBox": [
              336,
              90,
              432,
              91,
              411,
              154,
              314,
              154
            ],
            "text": "five"
          },
          {
            "boundingBox": [
              419,
              91,
              538,
              92,
              516,
              154,
              398,
              154
            ],
            "text": "dozen"
          },
          {
            "boundingBox": [
              547,
              92,
              701,
              94,
              679,
              154,
              525,
              154
            ],
            "text": "liquor"
          },
          {
            "boundingBox": [
              696,
              94,
              800,
              95,
              780,
              154,
              675,
              154
            ],
            "text": "jugs"
          }
        ]
      }
    ]
  }
}
```

## <a name="clean-up-resources"></a>Rensa resurser

Ta bort filen när den inte längre behövs.

## <a name="next-steps"></a>Nästa steg

Utforska ett Python-program som använder Visuellt innehåll för att utföra optisk teckenläsning (OCR), skapa miniatyrbilder med smart beskärning och identifiera, kategorisera, tagga och beskriv visuella funktioner, inklusive ansikten, i en bild. Du kan experimentera med API för visuellt innehåll i [Open API-testkonsolen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Självstudie: API för visuellt innehåll med Python](../Tutorials/PythonTutorial.md)
