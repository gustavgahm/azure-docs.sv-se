---
title: Konfigurera ett anpassat domännamn för din Azure API Management-instans | Microsoft Docs
description: Det här avsnittet beskriver hur du konfigurerar ett anpassat domännamn för din Azure API Management-instans.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 12/14/2017
ms.author: apimpm
ms.openlocfilehash: f613995dbdd787d0a031cb2c24d67c682b2d7cec
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446388"
---
# <a name="configure-a-custom-domain-name"></a>Konfigurera ett anpassat domännamn 

När du skapar en API Management (APIM)-instans, Azure tilldelar den till en underdomän till azure-api.net (till exempel `apim-service-name.azure-api.net`). Men du kan lägga upp din APIM-slutpunkter med ditt eget domännamn, till exempel **contoso.com**. Den här självstudien visar hur du mappar ett befintligt anpassat DNS-namn till slutpunkter som exponeras av en Azure API Management-instans.

> [!WARNING]
> Kunder som vill använda certifikat fästa för att förbättra säkerheten för sina program måste använda ett anpassat domännamn > och certifikat som de hanterar inte standardcertifikatet. Kunder som fästa standardcertifikatet i stället att > tar ett fast beroende på egenskaperna för det certifikat som de inte har kontroll över, vilket inte är en rekommendation.

## <a name="prerequisites"></a>Förutsättningar

Om du vill utföra stegen som beskrivs i den här artikeln, måste du ha:

+ En aktiv Azure-prenumeration.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ En APIM-instansen. Mer information finns i [skapa en Azure API Management-instans](get-started-create-service-instance.md).
+ Ett anpassat domännamn som ägs av dig. Det anpassade domännamnet som du vill använda, måste skaffat separat och finns på en DNS-server. Det här avsnittet ger inte instruktioner om hur du vara värd för ett anpassat domännamn.
+ Du måste ha ett giltigt certifikat med en offentlig och privat nyckel (. PFX). Ämnet eller alternativt namn för certifikatmottagare (SAN) måste matcha domännamnet (Detta möjliggör APIM att exponera URL: er på ett säkert sätt via SSL).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Använd Azure portal för att ange ett anpassat domännamn

1. Navigera till APIM-instansen i den [Azure-portalen](https://portal.azure.com/).
2. Välj **anpassade domäner och SSL**.
    
    Det finns ett antal slutpunkter som du kan tilldela ett anpassat domännamn. För närvarande finns följande slutpunkter: 
    + **Proxy** (standardvärdet är: `<apim-service-name>.azure-api.net`), 
    + **Portalen** (standardvärdet är: `<apim-service-name>.portal.azure-api.net`),     
    + **Hantering av** (standardvärdet är: `<apim-service-name>.management.azure-api.net`), 
    + **SCM** (standardvärdet är: `<apim-service-name>.scm.azure-api.net`).

    >[!NOTE]
    > Du kan uppdatera alla slutpunkter eller några av dem. Ofta, kunder uppdatera **Proxy** (den här URL: en används för att anropa API exponeras via API Management) och **Portal** (utvecklarportalen URL: en). **Hantering av** och **SCM** slutpunkter används internt av APIM-kunder och därmed tilldelas mindre ofta ett anpassat domännamn.
3. Välj den slutpunkt som du vill uppdatera. 
4. I fönstret till höger klickar du på **anpassad**.

    + I den **anpassade domännamn**, anger du namnet som du vill använda. Till exempel `api.contoso.com`. <br/>Domännamn med jokertecken (till exempel *. domain.com) stöds också.
    + I den **certifikat**, ange ett giltigt. PFX-fil som du vill ladda upp. 
    + Om certifikatet har ett lösenord, anger du den i den **lösenord** fält.
1. Klicka på tillämpa.

    >[!NOTE]
    >Processen för att ge certifikatet kan ta 15 minuter eller mer beroende på storleken på distributionen. Developer SKU har driftstopp, grundläggande och högre SKU: er har inte driftstopp.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Nästa steg

[Uppgradera och skala din tjänst](upgrade-and-scale.md)
