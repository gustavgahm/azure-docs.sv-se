---
title: Publicera en kunskapsbas
titleSuffix: QnA Maker - Azure Cognitive Services
description: Publicera din kunskapsbas är det sista steget i att göra din kunskapsbas tillgängliga som en slutpunkt för besvara fråga. När du publicerar en kunskapsbas flyttar frågor och svar om innehållet i kunskapsbasen från test-index till ett index för produktion i Azure search.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/11/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 2db8a87f28905ab03fb45d56943ae40d01908527
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225388"
---
# <a name="publish-a-knowledge-base-using-the-qna-maker-portal"></a>Publicera en kunskapsbas med QnA Maker-portalen

Publicera din kunskapsbas är det sista steget i att göra din kunskapsbas tillgängliga som en fråga besvarar slutpunkt för ett klientprogram. 

När du publicerar en kunskapsbas flyttar frågor och svar innehållet i kunskapsbasen från test-index till ett index för produktion i Azure search.

![Publicera prod test index](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar. 

## <a name="publish-a-knowledge-base"></a>Publicera en kunskapsbas

1. När du har gjort ändringar i din KB, Välj **publicera** i det övre navigeringsfältet. Du kan publicera upp till det angivna antalet kunskapsbaser för Azure Search. 

    ![Publicera kunskapsbas](../media/qnamaker-how-to-publish-kb/publish.png)

2. Välj **publicera** igen för att se vilka slutpunkt som kan användas i program / robotar koden.

    ![Publicerades kunskapsbas](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="clean-up-resources"></a>Rensa resurser

När du är klar med kunskapsbasen kan du ta bort den i QnA Maker-portalen.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Få analysfunktioner på din kunskapsbas](./get-analytics-knowledge-base.md)
