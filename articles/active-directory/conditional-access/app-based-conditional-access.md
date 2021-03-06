---
title: Så här kräver godkända klientappar för åtkomst till molnet appen med villkorlig åtkomst i Azure Active Directory | Microsoft Docs
description: Lär dig hur du kräver godkända klientappar för åtkomst till molnet appen med villkorlig åtkomst i Azure Active Directory.
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
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 53ee1beb1c5fc50acd0710eea475415f71662492
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076680"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>Hur: Kräv godkända klientappar för cloud app-åtkomst med villkorlig åtkomst 

Dina anställda använder mobila enheter för både personliga och arbetsrelaterade uppgifter. Att se till att dina anställda kan vara produktiva, vill du också förhindra förlust av data. Med villkorlig åtkomst för Azure Active Directory (AD Azure), kan du begränsa åtkomsten till dina appar i molnet på godkända klientappar som kan skydda företagets data.  

Det här avsnittet förklarar hur du konfigurerar villkor åtkomstprinciper som kräver godkända klientprogram.

## <a name="overview"></a>Översikt

Med [Azure AD villkorsstyrd åtkomst](overview.md), du kan finjustera hur behöriga användare kan komma åt dina resurser. Du kan exempelvis begränsa åtkomsten till dina molnappar till betrodda enheter.

Du kan använda [Intunes appskyddsprinciper](https://docs.microsoft.com/intune/app-protection-policy) för att skydda företagets data. Intunes appskyddsprinciper behöver inte lösning för hantering av mobila enheter (MDM), där du kan skydda företagets data eller inte registrerar enheter i en enhetshanteringslösning.

Azure Active Directory villkorlig åtkomst kan du begränsa åtkomsten till dina appar i molnet på klientappar som stöder Intunes appskyddsprinciper. Du kan exempelvis begränsa åtkomsten till Exchange Online för Outlook-appen.

Med villkorlig åtkomst-terminologin dessa klientappar kallas **godkända klientappar**.  


![Villkorlig åtkomst](./media/app-based-conditional-access/05.png)


En lista över godkända klientappar finns i [godkända kravet på klienten app](technical-reference.md#approved-client-app-requirement).


Du kan kombinera principer för appbaserad villkorlig åtkomst med andra principer som [principer för enhetsbaserad villkorlig åtkomst](require-managed-devices.md) att skapa flexibilitet i hur du skyddar data för både personliga och företagsägda enheter.

 


## <a name="before-you-begin"></a>Innan du börjar

Det här avsnittet förutsätter att du är bekant med:

- Den [godkända kravet på klienten app](technical-reference.md#approved-client-app-requirement) Teknisk referens.


- Grundläggande begrepp för [villkorlig åtkomst i Azure Active Directory](overview.md).

- Så här [konfigurerar principer för villkorlig åtkomst](app-based-mfa.md).

- Den [migrering av principer för villkorlig åtkomst](best-practices.md#policy-migration).
 

## <a name="prerequisites"></a>Förutsättningar

Om du vill skapa en princip för appbaserad villkorlig åtkomst måste du ha en Enterprise Mobility + Security eller Azure Active Directory premium-prenumeration och användarna måste ha licens för EMS eller Azure AD. 


## <a name="exchange-online-policy"></a>Exchange Online-princip 

Det här scenariot består av en princip för appbaserad villkorlig åtkomst för åtkomst till Exchange Online.


### <a name="scenario-playbook"></a>Scenariot spelbok

Det här scenariot förutsätter att en användare:

- Konfigurerar e-post med en intern e-postprogrammet på iOS eller Android för att ansluta till Exchange

- Tar emot ett e-postmeddelande som anger att åtkomst är bara tillgänglig i Outlook-appen

- Hämtar programmet med länken

- Öppnar Outlook-programmet och loggar in med autentiseringsuppgifter för Azure AD

- Uppmanas att installera Authenticator (iOS) eller Företagsportalen (Android) för att fortsätta

- Installerar program och kan gå tillbaka till Outlook-appen och fortsätt

- Uppmanas att registrera en enhet

- Komma åt e-post

Alla Intune-appskyddsprinciper är aktiverad när den åtkomst till företagets data och kan uppmana användaren att starta om programmet, Använd en ytterligare PIN-kod osv (om konfigurerad för programmet och plattformen).

### <a name="configuration"></a>Konfiguration 

**Steg 1 – konfigurera en Azure AD-princip för villkorlig åtkomst för Exchange Online**

För principen för villkorlig åtkomst i det här steget måste du konfigurera följande komponenter:

![Villkorlig åtkomst](./media/app-based-conditional-access/01.png)

1. Den **namn** av principer för villkorlig åtkomst.

2. **Användare och grupper**: Varje princip för villkorlig åtkomst måste ha minst en användare eller grupp valdes.

3. **Appar i molnet:** Som molnappar, måste du välja **Office 365 Exchange Online**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/07.png)

4. **Villkor:** Som **villkor**, måste du konfigurera **enhetsplattformar** och **klientappar**:

    a. Som **enhetsplattformar**väljer **Android** och **iOS**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/03.png)

    b. Som **klientappar (förhandsgranskning)** väljer **mobilappar och skrivbordsappar** och **moderna autentiseringsklienter**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/91.png)

5. Som **åtkomstkontroller**, måste du ha **Kräv godkänd klientapp (förhandsversion)** valda.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/05.png)
 

**Steg 2 – konfigurera en Azure AD-princip för villkorlig åtkomst för Exchange Online med Active Sync (EAS)**

För principen för villkorlig åtkomst i det här steget måste du konfigurera följande komponenter:

![Villkorlig åtkomst](./media/app-based-conditional-access/06.png)

1. Den **namn** av principer för villkorlig åtkomst.

2. **Användare och grupper**: Varje princip för villkorlig åtkomst måste ha minst en användare eller grupp valdes.


3. **Appar i molnet:** Som molnappar, måste du välja **Office 365 Exchange Online**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/07.png)

4. **Villkor:** Som **villkor**, måste du konfigurera **klientappar (förhandsgranskning)**. 

    a. Som **klientappar (förhandsgranskning)** väljer **mobilappar och skrivbordsklienter** och **Exchange ActiveSync-klienter**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/92.png)

    b. Som **åtkomstkontroller**, måste du ha **Kräv godkänd klientapp (förhandsversion)** valda.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/05.png)


**Steg 3 – Konfigurera Intunes appskyddsprincip för iOS och Android-klientprogram**


![Villkorlig åtkomst](./media/app-based-conditional-access/09.png)

Se [skydda data och appar med Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) för mer information.


## <a name="exchange-online-and-sharepoint-online-policy"></a>Exchange Online och SharePoint Online-princip

Det här scenariot består av en villkorlig åtkomst med mam-princip för åtkomst till Exchange Online och SharePoint Online med godkända appar.

### <a name="scenario-playbook"></a>Scenariot spelbok

Det här scenariot förutsätter att en användare:

- Försöker använda SharePoint-app att ansluta och visa företagets webbplatser

- Försök att logga in med samma autentiseringsuppgifter som autentiseringsuppgifter för Outlook-appen

- Behöver inte registrera igen och kan få åtkomst till resurser


### <a name="configuration"></a>Konfiguration

**Steg 1 – konfigurera en Azure AD-princip för villkorlig åtkomst för Exchange Online och SharePoint Online**

För principen för villkorlig åtkomst i det här steget måste du konfigurera följande komponenter:

![Villkorlig åtkomst](./media/app-based-conditional-access/71.png)

1. Den **namn** av principer för villkorlig åtkomst.

2. **Användare och grupper**: Varje princip för villkorlig åtkomst måste ha minst en användare eller grupp valdes.


3. **Appar i molnet:** Som molnappar, måste du välja **Office 365 Exchange Online** och **Office 365 SharePoint Online**. 

    ![Villkorlig åtkomst](./media/app-based-conditional-access/02.png)

4. **Villkor:** Som **villkor**, måste du konfigurera **enhetsplattformar** och **klientappar**:

    a. Som **enhetsplattformar**väljer **Android** och **iOS**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/03.png)

    b. Som **klientappar (förhandsgranskning)** väljer **mobilappar och skrivbordsklienter** och **moderna autentiseringsklienter**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/91.png)

5. Som **åtkomstkontroller**, måste du ha **Kräv godkänd klientapp (förhandsversion)** valda.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/05.png)




**Steg 2 – konfigurera en Azure AD-princip för villkorlig åtkomst för Exchange Online med Active Sync (EAS)**

För principen för villkorlig åtkomst i det här steget måste du konfigurera följande komponenter:

![Villkorlig åtkomst](./media/app-based-conditional-access/06.png)

1. Den **namn** av principer för villkorlig åtkomst.

2. **Användare och grupper**: Varje princip för villkorlig åtkomst måste ha minst en användare eller grupp valdes.

3. **Appar i molnet:** Som molnappar, måste du välja **Office 365 Exchange Online**. Online 

    ![Villkorlig åtkomst](./media/app-based-conditional-access/07.png)

4. **Villkor:** Som **villkor**, måste du konfigurera **klientappar**:

    a. Som **klientappar (förhandsgranskning)** väljer **mobilappar och skrivbordsklienter** och **Exchange ActiveSync-klienter**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/92.png)

    b. Som **åtkomstkontroller**, måste du ha **Kräv godkänd klientapp (förhandsversion)** valda.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/05.png)




**Steg 3 – Konfigurera Intunes appskyddsprincip för iOS och Android-klientprogram**


![Villkorlig åtkomst](./media/app-based-conditional-access/09.png)

Se [skydda data och appar med Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) för mer information.


## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>App-baserade eller kompatibla enhetsprincip för Exchange Online och SharePoint Online

Det här scenariot består av en princip för villkorlig åtkomst för app-baserade eller kompatibla enheter för åtkomst till Exchange Online.


### <a name="scenario-playbook"></a>Scenariot spelbok

Det här scenariot förutsätter att:
 
- Vissa användare har redan registrerats (med eller utan företagets enheter)

- Användare som inte har registrerats i och registrerad med Azure AD med en app skyddade program behöver registrera en enhet för att få åtkomst till resurser

- Registrerade användare med hjälp av app som skyddas programmet behöver inte registrera enheten


### <a name="configuration"></a>Konfiguration

**Steg 1 – konfigurera en Azure AD-princip för villkorlig åtkomst för Exchange Online och SharePoint Online**

För principen för villkorlig åtkomst i det här steget måste du konfigurera följande komponenter:

![Villkorlig åtkomst](./media/app-based-conditional-access/62.png)

1. Den **namn** av principer för villkorlig åtkomst.

2. **Användare och grupper**: Varje princip för villkorlig åtkomst måste ha minst en användare eller grupp valdes.

3. **Appar i molnet:** Som molnappar, måste du välja **Office 365 Exchange Online** och **Office 365 SharePoint Online**. 

     ![Villkorlig åtkomst](./media/app-based-conditional-access/02.png)

4. **Villkor:** Som **villkor**, måste du konfigurera **enhetsplattformar** och **klientappar**. 
 
    a. Som **enhetsplattformar**väljer **Android** och **iOS**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/03.png)

    b. Som **klientappar (förhandsgranskning)** väljer **mobilappar och skrivbordsklienter** och **moderna autentiseringsklienter**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/91.png)

5. Som **åtkomstkontroller**, måste du ha följande valda:

    - **Kräv att enheten är markerad som kompatibel**

    - **Kräv godkänd klientapp (förhandsversion)**

    - **Begär en av de valda kontrollerna**   
 
    ![Villkorlig åtkomst](./media/app-based-conditional-access/11.png)



**Steg 2 – konfigurera en Azure AD-princip för villkorlig åtkomst för Exchange Online med Active Sync (EAS)**

För principen för villkorlig åtkomst i det här steget måste du konfigurera följande komponenter:

![Villkorlig åtkomst](./media/app-based-conditional-access/61.png)

1. Den **namn** av principer för villkorlig åtkomst.

2. **Användare och grupper**: Varje princip för villkorlig åtkomst måste ha minst en användare eller grupp valdes.

3. **Appar i molnet:** Som molnappar, måste du välja **Office 365 Exchange Online**. 

    ![Villkorlig åtkomst](./media/app-based-conditional-access/07.png)

4. **Villkor:** Som **villkor**, måste du konfigurera **klientappar**. 

    Som **klientappar (förhandsgranskning)** väljer **mobilappar och skrivbordsklienter** och **Exchange ActiveSync-klienter**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/91.png)

5. Som **åtkomstkontroller**, måste du ha **Kräv godkänd klientapp (förhandsversion)** valda.
 
    ![Villkorlig åtkomst](./media/app-based-conditional-access/11.png)




**Steg 3 – Konfigurera Intunes appskyddsprincip för iOS och Android-klientprogram**


![Villkorlig åtkomst](./media/app-based-conditional-access/09.png)

Se [skydda data och appar med Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) för mer information.





## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>App-baserade och kompatibel enhetsprincip för Exchange Online och SharePoint Online

Det här scenariot består av en princip för villkorlig åtkomst för app-baserade och kompatibla enheter för åtkomst till Exchange Online.


### <a name="scenario-playbook"></a>Scenariot spelbok

Det här scenariot förutsätter att en användare:
 
-   Konfigurerar e-post med en intern e-postprogrammet på iOS eller Android för att ansluta till Exchange
-   Tar emot ett e-postmeddelande som anger att åtkomst kräver att enheten registreras
-   Hämtar Företagsportalen och loggar in på Företagsportalen
-   Kontrollerar e-post och uppmanas att använda Outlook-appen
-   Hämtar Outlook-appen
-   Öppnar Outlook-appen och anger de autentiseringsuppgifter som används i registreringen
-   Användaren kan komma åt e-post

Alla Intunes appskyddsprinciper aktiveras vid tidpunkten för åtkomst till företagets data och kan uppmana användaren att starta om programmet, Använd en ytterligare PIN-kod osv (om konfigurerad för programmet och plattform)


### <a name="configuration"></a>Konfiguration

**Steg 1 – konfigurera en Azure AD-princip för villkorlig åtkomst för Exchange Online och SharePoint Online**

För principen för villkorlig åtkomst i det här steget måste du konfigurera följande komponenter:

![Villkorlig åtkomst](./media/app-based-conditional-access/62.png)

1. Den **namn** av principer för villkorlig åtkomst.

2. **Användare och grupper**: Varje princip för villkorlig åtkomst måste ha minst en användare eller grupp valdes.

3. **Appar i molnet:** Som molnappar, måste du välja **Office 365 Exchange Online** och **Office 365 SharePoint Online**. 

     ![Villkorlig åtkomst](./media/app-based-conditional-access/02.png)

4. **Villkor:** Som **villkor**, måste du konfigurera **enhetsplattformar** och **klientappar**. 
 
    a. Som **enhetsplattformar**väljer **Android** och **iOS**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/03.png)

    b. Som **klientappar (förhandsgranskning)** väljer **mobilappar och skrivbordsappar** och **moderna autentiseringsklienter**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/91.png)

5. Som **åtkomstkontroller**, måste du ha följande valda:

    - **Kräv att enheten är markerad som kompatibel**

    - **Kräv godkänd klientapp (förhandsversion)**

    - **Kräv de valda kontrollerna**   
 
    ![Villkorlig åtkomst](./media/app-based-conditional-access/13.png)



**Steg 2 – konfigurera en Azure AD-princip för villkorlig åtkomst för Exchange Online med Active Sync (EAS)**

För principen för villkorlig åtkomst i det här steget måste du konfigurera följande komponenter:

![Villkorlig åtkomst](./media/app-based-conditional-access/61.png)

1. Den **namn** av principer för villkorlig åtkomst.

2. **Användare och grupper**: Varje princip för villkorlig åtkomst måste ha minst en användare eller grupp valdes.

3. **Appar i molnet:** Som molnappar, måste du välja **Office 365 Exchange Online**. 

    ![Villkorlig åtkomst](./media/app-based-conditional-access/07.png)

4. **Villkor:** Som **villkor**, måste du konfigurera **klientappar (förhandsgranskning)**. 

    Som **klientappar (förhandsgranskning)** väljer **mobilappar och skrivbordsklienter** och **Exchange ActiveSync-klienter**.

    ![Villkorlig åtkomst](./media/app-based-conditional-access/92.png)

5. Som **åtkomstkontroller**, måste du ha följande valda:

    - **Kräv att enheten är markerad som kompatibel**

    - **Kräv godkänd klientapp (förhandsversion)**

    - **Kräv de valda kontrollerna**   
 
    ![Villkorlig åtkomst](./media/app-based-conditional-access/64.png)




**Steg 3 – Konfigurera Intunes appskyddsprincip för iOS och Android-klientprogram**


![Villkorlig åtkomst](./media/app-based-conditional-access/09.png)

Se [skydda data och appar med Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) för mer information.






## <a name="next-steps"></a>Nästa steg

Om du vill veta hur du konfigurerar principer för villkorlig åtkomst finns i [kräver MFA för specifika appar med villkorlig åtkomst i Azure Active Directory](app-based-mfa.md).

Om du är redo att konfigurera principer för villkorsstyrd åtkomst för din miljö kan du läsa sidan om [metodtips för villkorsstyrd åtkomst i Azure Active Directory](best-practices.md). 
