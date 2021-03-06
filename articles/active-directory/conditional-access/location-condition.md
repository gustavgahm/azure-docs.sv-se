---
title: Vad är platsvillkor som i Azure Active Directory villkorlig åtkomst? | Microsoft Docs
description: Lär dig hur du använder platsvillkoret för att styra åtkomsten till dina molnappar baserat på användarens nätverksplats.
services: active-directory
keywords: conditional access to apps, conditional access with Azure AD, secure access to company resources, conditional access policies
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/21/2019
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 26721aa0eac69875f6a3704025e6ab71a54a1e31
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078108"
---
# <a name="what-is-the-location-condition-in-azure-active-directory-conditional-access"></a>Vad är platsvillkoret för villkorlig åtkomst i Azure Active Directory? 

Med [villkorlig åtkomst i Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), du kan styra hur behöriga användare kan komma åt dina appar i molnet. Platsvillkoret för en princip för villkorlig åtkomst kan du koppla kontroller åtkomstinställningar till nätverksplatser för dina användare.

Den här artikeln ger dig den information du behöver att konfigurera platsvillkoret. 

## <a name="locations"></a>Platser

Azure AD aktiverar enkel inloggning på enheter, appar och tjänster från var som helst på internet. Du kan styra åtkomsten till dina molnappar baserat på nätverksplats för en användare med platsvillkor. Vanliga användningsområden för platsvillkoret är:

- Att kräva multifaktorautentisering för användare som ansluter till en tjänst när är utanför företagets nätverk  

- Blockera åtkomst för användare som ansluter till en tjänst från vissa länder eller regioner. 

En plats är en etikett för en nätverksplats som antingen representerar en namngiven plats eller multifaktorautentisering tillförlitliga IP-adresser.


## <a name="named-locations"></a>Namngivna platser 

Du kan skapa logiska grupperingar av IP-adressintervall, länder och regioner med namngivna platser. 

Du kan komma åt din sökvägarna i den **hantera** på sidan för villkorlig åtkomst.

![Platser](./media/location-condition/02.png)

 


En namngiven plats har följande komponenter:

![Platser](./media/location-condition/42.png)

- **Namn på** – visningsnamnet för en namngiven plats.

- **IP-intervall** - minst en IPv4-adressintervall i CIDR-format. Finns inte stöd för att ange en Ipv6-adressintervall.

- **Markera som betrodd plats** – en flagga som du kan ange för en namngiven plats att ange en betrodd plats. Betrodda platser är oftast nätverksområden som kontrolleras av IT-avdelningen. Förutom villkorlig åtkomst, betrodda namngivna platser används också av Azure Identity Protection och Azure AD-säkerhetsrapporter för att minska [falska positiva identifieringar](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations-1).

- **Länder / regioner** – det här alternativet kan du välja en eller flera land eller region för att definiera en namngiven plats. 

- **Inkludera okända områden** -vissa IP-adresser mappas inte till ett visst land. Det här alternativet kan du välja om dessa IP-adresser ska ingå i den namngivna platsen. De kan kontrollera när principen med hjälp av den namngivna platsen ska gälla för okända platser.

Antalet namngivna platser som du kan konfigurera begränsas av storleken på det relaterade objektet i Azure AD. Du kan konfigurera:

- En namnet plats med upp till 1200 IP-intervall.

- Högst 90 namngivna platser med en IP-adressintervall som är tilldelade till var och en av dem.




## <a name="trusted-ips"></a>Tillförlitliga IP-adresser

Du kan också konfigurera IP-adressintervall som motsvarar din organisations lokalt intranät i den [multifaktorautentisering tjänstinställningar](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx). Den här funktionen kan du konfigurera upp till 50 IP-adressintervall. Det är de IP-adressintervall i CIDR-format. Mer information finns i [tillförlitliga IP-adresser](../authentication/howto-mfa-mfasettings.md#trusted-ips).  

Om du har tillförlitliga IP-adresser konfigureras, de visas som **MFA tillförlitliga IP-adresser** i listan över platser för platsvillkoret.   

### <a name="skipping-multi-factor-authentication"></a>Hoppar över multifaktorautentisering

På inställningssidan för multi-Factor authentication-tjänsten kan du identifiera företagsintranät användare genom att välja **hoppa över multifaktorautentisering för förfrågningar från federerade användare från mitt intranät**. Den här inställningen anger att innanför företagets nätverk anspråk som utfärdas av AD FS, bör betrodd och används för att identifiera användaren är i företagsnätverket. Mer information finns i [aktivera funktionen tillförlitliga IP-adresser med hjälp av villkorlig åtkomst](../authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access).

När du har kontrollerat det här alternativet, inklusive namngiven plats **MFA tillförlitliga IP-adresser** gäller för alla principer med den här valda.

För mobila och skrivbordsprogram, som har länge sessionen livslängd, är villkorlig åtkomst regelbundet ny utvärdering. Standardvärdet är en gång i timmen. När insidan är i ett företagsnätverkanspråk och endast utfärdats vid tidpunkten för den första autentiseringen, Azure AD inte kan ha en lista över betrodda IP-adressintervall. I det här fallet är det svårare att avgöra om användaren är fortfarande i företagets nätverk:

1. Kontrollera om användarens IP-adress är i något av de betrodda IP-adressintervall.

2. Kontrollera om de tre första oktetterna i användarens IP-adress matchar de första 3 oktetterna i IP-adressen för den första autentiseringen. IP-adressen jämförs med den första autentiseringen eftersom detta är när insidan ett företagsnätverkanspråk ursprungligen utfärdade och platsen som har verifierats.

Om båda stegen misslyckas, anses en användare som inte längre finnas på en tillförlitlig IP-adress.



## <a name="location-condition-configuration"></a>Konfiguration av villkor

När du konfigurerar platsvillkoret har möjlighet att skilja mellan:

- Vilken plats som helst 
- Alla betrodda platser
- Valda platser

![Platser](./media/location-condition/01.png)

### <a name="any-location"></a>Vilken plats som helst

Som standard att välja **valfri plats** orsakar en princip som ska tillämpas på alla IP-adresser, vilket innebär att alla adresser på Internet. Den här inställningen är inte begränsad till IP-adresser som du har konfigurerat som namn. När du väljer **valfri plats**, du kan fortfarande utesluta särskilda platser från en princip. Du kan till exempel använda en princip för att alla platser utom betrodda platser för att ange omfånget till alla platser utom företagets nätverk.

### <a name="all-trusted-locations"></a>Alla betrodda platser

Det här alternativet gäller för:

- Alla platser som har markerats som betrodd plats
- **MFA tillförlitliga IP-adresser** (om konfigurerad)


### <a name="selected-locations"></a>Valda platser

Med det här alternativet kan du välja en eller flera namngivna platser. En användare måste ansluta från någon av de valda platserna för en princip med den här inställningen ska gälla. När du klickar på **Välj** den namngivna nätverk val av kontroll som visar en lista över namngivna nätverk öppnas. I listan visas också om en nätverksplats nedan har markerats som betrodda. Namngiven plats kallas **MFA tillförlitliga IP-adresser** används för att ta med IP-inställningar som kan konfigureras på sidan Inställningar för multi-Factor authentication-tjänsten.

## <a name="what-you-should-know"></a>Det här bör du känna till

### <a name="when-is-a-location-evaluated"></a>När utvärderas en plats?

Principer för villkorlig åtkomst utvärderas när: 

- En användare loggar först in till ett webbprogram för app-, Mobil- eller desktop. 

- Ett program för mobil eller dator som använder modern autentisering använder en uppdateringstoken för att få en ny åtkomsttoken. Som standard är detta en gång i timmen. 

Det innebär för mobila enheter och program med modern autentisering, en ändring på plats så identifieras inom en timme efter att ändra nätverksplatsen. För mobila och skrivbordsprogram som inte använder modern autentisering, tillämpas principen på varje begäran-token. Frekvensen för begäran kan variera beroende på programmet. På samma sätt tillämpas vid första inloggningen principen för webbprogram och är bra för livslängden för sessionen på webbprogrammet. På grund av skillnader i sessionen livslängd för program är varierar tiden mellan principutvärdering. Varje gång programmet begär en ny token logga in tillämpas principen.


Azure AD utfärdar en token timme som standard. När du har flyttat av företagets nätverk, inom en timme tillämpas principen för program som använder modern autentisering.


### <a name="user-ip-address"></a>Användarens IP-adress

IP-adressen som används i principutvärdering är den offentliga IP-adressen för användaren. För enheter i ett privat nätverk inte klientens IP-Adressen för användarens enhet på intranätet, det är den adress som används av nätverket för att ansluta till det offentliga internet. 

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Massinläsning överföring och hämtning av namngivna platser

När du skapar eller uppdaterar kan namngivna platser för massuppdateringar, du överföra eller hämta en CSV-fil med IP-intervall. Ladda upp ersätter IP-adressintervall i listan med de från filen. Varje rad i filen innehåller en IP-adressintervall i CIDR-format. 


### <a name="cloud-proxies-and-vpns"></a>Molnet proxyservrar och VPN-anslutningar 

När du använder en molnvärd proxy eller VPN-lösning använder IP-adressen Azure AD när utvärdering av en princip är IP-adressen för proxyservern. Rubriken X-Forwarded-For (XFF) som innehåller de användare som offentlig IP-adress inte används eftersom det finns ingen validering som den kommer från en betrodd källa, skulle så presentera en metod för faking en IP-adress. 

När en cloud-proxy är på plats eller en princip som används för att kräva en domänansluten enhet kan användas för insidan corpnet anspråk från AD FS.



### <a name="api-support-and-powershell"></a>API-stöd och PowerShell 

API och PowerShell stöds inte ännu för namngivna platser eller för principer för villkorlig åtkomst.





## <a name="next-steps"></a>Nästa steg

- Om du vill veta hur du konfigurerar principer för villkorlig åtkomst finns i [kräver MFA för specifika appar med villkorlig åtkomst i Azure Active Directory](app-based-mfa.md).

- Om du är redo att konfigurera principer för villkorsstyrd åtkomst för din miljö kan du läsa sidan om [metodtips för villkorsstyrd åtkomst i Azure Active Directory](best-practices.md). 
