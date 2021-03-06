---
title: Granskning och rapportering av en användare för Azure Active Directory B2B-samarbete | Microsoft Docs
description: Egenskaper för gäst-användare kan konfigureras i Azure Active Directory B2B-samarbete
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 06622c093ca90b3873365e6c93c40fc7221a6398
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55075201"
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a>Granskning och rapportering av en användare för B2B-samarbete
Med gästanvändare har du granskningsfunktioner liknar med användare. 

## <a name="access-reviews"></a>Åtkomstgranskningar
Du kan använda åtkomstgranskningar regelbundet kontrollerar om gästanvändare fortfarande behöver åtkomst till dina resurser. Den **Åtkomstgranskningar** funktionen är tillgänglig i **Azure Active Directory** under **hantera** > **organisationens relationer**. (Du kan också söka efter ”åtkomstgranskningar” från **alla tjänster** i Azure-portalen.) Läs hur du använder åtkomstgranskningar i [hantera gäståtkomst med Azure AD åtkomst går igenom](../governance/manage-guest-access-with-access-reviews.md).

## <a name="audit-logs"></a>Granskningsloggar

Azure AD-audit-loggarna ger poster i system- och aktiviteter, inklusive aktiviteter som initierats av gästanvändare. Åtkomst till granskningsloggar, på **Azure Active Directory**under **övervakning**väljer **granskningsloggar**. Här är ett exempel på inbjudan och inlösen historiken för inbjudens Sam Oogle:

![granskningslogg](./media/auditing-and-reporting/audit-log.png)

Du kan fördjupa dig i var och en av dessa händelser för att hämta information. Exempelvis kan du nu ska vi titta detaljinformationen godkännande.

![information om datoraktivitet](./media/auditing-and-reporting/activity-details.png)

Du kan också exportera dessa loggar från Azure AD och använda Rapporteringsverktyg föredrar för att hämta anpassade rapporter.

### <a name="next-steps"></a>Nästa steg

- [Användaregenskaper för B2B-samarbete](user-properties.md)

