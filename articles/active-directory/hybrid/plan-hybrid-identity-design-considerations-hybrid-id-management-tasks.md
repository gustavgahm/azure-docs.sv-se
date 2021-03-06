---
title: Hybridutformning identity - administrationsuppgifter Azure | Microsoft Docs
description: Med villkorlig åtkomstkontroll kontrollerar de specifika villkor som du väljer när du autentiserar användaren och innan åtkomst ges till programmet i Azure Active Directory. När dessa villkor är uppfyllda, användaren autentiseras och får åtkomst till programmet.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: f6e5ca57eaa4a4a1975c0295933fc91e5726ae4c
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55487939"
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Planera för Hybrid Identitetslivscykel
Identitet är en av grundpelarna i enterprise mobility och programmet åtkomststrategi. Om du loggar din mobila enhet eller SaaS-app, är din identitet på för att få åtkomst till allt. På den högsta nivån omfattar en lösning för Identitetshantering förena och synkronisering mellan din identitet lagringsplatser, bland annat automatisera och centralisera att etablera resurser. ID-lösning bör vara en centraliserad identitet i både lokalt och i molnet och även använda någon form av identitetsfederation att underhålla centraliserad autentisering och på ett säkert sätt dela och samarbeta med externa användare och företag. Resurser mellan operativsystem och program till personer i eller tillhör en organisation. Organisationsstruktur kan ändras för att hantera etablering principer och procedurer.

Det är också viktigt att du har en identitetslösning som är avsedda för att underlätta för dina användare genom att låta dem via självbetjäning upplevelser för att hålla dem produktiva. ID-lösning är mer robust om det möjliggör enkel inloggning för användare i alla resurser som de behöver åtkomst. Administratörer på alla nivåer kan använda standardiserad procedurer för att hantera användarens autentiseringsuppgifter. Vissa nivåer av administration kan minskas eller elimineras, beroende på bredden av etablering hanteringslösningen. Dessutom kan du säkert distribuera administrationsfunktioner, manuellt eller automatiskt, mellan olika organisationer. Till exempel kan en domänadministratör fungera endast personer och resurser i domänen. Den här användaren kan utföra uppgifter för administration och etablering, men har inte behörighet att göra konfigurationsåtgärder, till exempel skapa arbetsflöden.

## <a name="determine-hybrid-identity-management-tasks"></a>Determine Hybrid Identity Management Tasks
Distribuera administrativa uppgifter i din organisation bättre precision och effektivitet för administration och förbättrar balansen för arbetsbelastningen för en organisation. Följande är pivoteringsmöjligheter som definierar en robust identitetshanteringssystem.

 ![](./media/plan-hybrid-identity-design-considerations/Identity_management_considerations.png)

För att definiera hybrid identity-hanteringsuppgifter, måste du förstå några grundläggande egenskaper för den organisation som kommer börja använda hybrid-identitet. Det är viktigt att förstå de aktuella lagringsplatser som används för identitet källor. Genom att känna till dessa kärnelement, du har grundläggande krav och baserat på att du måste ställa mer detaljerade frågor som leder dig till ett bättre beslut för din lösning för Identitetshantering.  

När du definierar dessa krav, se till att minst följande frågor besvaras

* Alternativ för etablering: 
  
  * Stöder hybrididentitetslösning en robust konto åtkomsthantering och etablering system?
  * Hur är användare, grupper och lösenord som ska hanteras?
  * Är identitetslivcykelhantering dynamiskt? 
    * Hur lång tid tar lösenord uppdateringar konto inaktiveringen?
* Licenshantering: 
  
  * Fungerar licenshantering för hybrid identity lösningen hanterar?
    * Om Ja, vilka funktioner är tillgängliga?
* Hanterar lösningen licenshantering av gruppbaserad? 
  
      - Om Ja, är det möjligt att tilldela en grupp? 
       - Om Ja, kommer molnkatalogen automatiskt tilldela licenser till alla medlemmar i gruppen? 
        - Vad händer om en användare är därefter läggs till eller tas bort från gruppen, kommer en licens automatiskt tilldelas eller tas bort efter behov? 
* Integrering med andra identitetsleverantörer för från tredje part:
* Kan den här kombinerade lösningen integreras med identitetsleverantörer från tredje part att implementera enkel inloggning?
* Är det möjligt att förena de olika identitetsprovidrarna i ett sammanhängande identitetssystem?
* Om Ja, hur och som är de och vilka funktioner är tillgängliga?

## <a name="synchronization-management"></a>Hantering av datasynkronisering
Ett av målen med en identity manager för att kunna ta med alla identitetsleverantörer och hålla dem synkroniserade. Du behåller synkronisera data utifrån en auktoritativ master identitetsprovider. I hybrididentitetsscenario med synkroniserade management-modellen kan hantera alla användar- och identiteter i en lokal server och synkronisera de konton och eventuellt lösenord till molnet. Användaren anger de samma lösenord lokalt som i molnet och vid inloggning, lösenordet verifieras av ID-lösning. Den här modellen använder en katalogsynkroniseringsverktyget.

![](./media/plan-hybrid-identity-design-considerations/Directory_synchronization.png) Synkronisering av din hybrididentitetslösning till rätt design Kontrollera att följande frågor besvaras: • vilka är de sync-lösningarna som är tillgängliga för hybrididentitetslösning?
• Vilka är funktionerna för enkel inloggning?
• Alternativ för identitetsfederation mellan B2B och B2C?

## <a name="next-steps"></a>Nästa steg
[Fastställa införandestrategin för hybrid identity](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](plan-hybrid-identity-design-considerations-overview.md)

