---
title: 'Självstudier: Azure Active Directory-integrering med ADP Globalview | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ADP Globalview.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: f5d11dd1ed8e23414eed5c9d7cf5c248b58aaa8a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163729"
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a>Självstudier: Azure Active Directory-integrering med ADP Globalview

I den här självstudien får du lära dig hur du integrerar ADP Globalview med Azure Active Directory (AD Azure).

Integrera ADP Globalview med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till ADP Globalview
- Du kan aktivera användarna att automatiskt få loggat in på ADP Globalview (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med ADP Globalview, behöver du följande objekt:

- En Azure AD-prenumeration
- En ADP Globalview enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du skaffa en månads utvärderingsperiod [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till ADP Globalview från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-adp-globalview-from-the-gallery"></a>Att lägga till ADP Globalview från galleriet
För att konfigurera integrering av ADP Globalview i Azure AD, som du behöver lägga till ADP Globalview från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till ADP Globalview från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Appar][2]
    
3. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Appar][3]

4. I sökrutan skriver **ADP Globalview**.

    ![Skapa en Azure AD-användare för testning](./media/adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. I resultatpanelen väljer **ADP Globalview**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med ADP Globalview baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i ADP Globalview är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i ADP Globalview upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i ADP Globalview.

Om du vill konfigurera och testa Azure AD enkel inloggning med ADP Globalview, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare ADP Globalview](#creating-an-adp-globalview-test-user)**  – du har en motsvarighet för Britta Simon i ADP Globalview som är länkad till en Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt ADP Globalview program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med ADP Globalview:**

1. I Azure-portalen på den **ADP Globalview** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. På den **ADP Globalview domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_adpglobalview_url.png)

     I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster: `https://<subdomain>.globalview.adp.com/federate` eller `https://<subdomain>.globalview.adp.com/federate2`

    > [!NOTE] 
    > Värdet är inte verkligt. Uppdatera värdet med den faktiska identifieraren. Kontakta [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) att hämta värdet.
 
4. På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. ADP GlobalView programmet förväntar sig SAML-intyg i ett visst format, vilket kräver att du kan lägga till anpassade attributmappningar i SAML-tokenattribut konfigurationen. 

6. Följande skärmbild visar ett exempel för den. De anspråk som alltid är **”PersonImmutableID”** och värdet som vi har mappats till ExtensionAttribute2 som innehåller EmployeeID för användaren. Användarmappning från Azure AD till ADP GlobalView görs här på EmployeeID men du kan mappa den till ett annat värde även baserat på dina inställningar för programmet. Du kan arbeta med ADP GlobalView teamet först att använda rätt identifierare för en användare och mappa värdet med den **”PersonImmutableID”** anspråk. Du kan också mappa anspråket e-post och användar-ID som visas i bilden.

    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. I den **användarattribut** avsnittet på den **enkel inloggning** dialogrutan Konfigurera SAML-token attributet som visas i bilden och utför följande steg:
    
    | Attributnamn | Attributvärde |
    | ------------------- | -------------------- |    
    | personalimmutableid | user.extensionattribute2 |
    | e-post               | user.mail |
    | användar-ID              | user.userprincipalname|
    
    a. Klicka på **Lägg till attribut** att öppna den **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_attribute_05.png)

    b. I textrutan **Namn** skriver du det attributnamn som visas för den raden.

    c. Från den **värdet** anger attributvärdet som visas för den raden.
    
    d. Klicka på **OK**.

    > [!NOTE] 
    > Innan du kan konfigurera SAML-kontroll, måste du kontakta din [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) och begära värdet för attributet för unik identifierare för din klient. Du behöver det här värdet för att konfigurera det anpassade anspråket för ditt program. 

8. På den **ADP Globalview Configuration** klickar du på **konfigurera ADP Globalview** att öppna **konfigurera inloggning** fönster. Kopiera den **URL för utloggning, SAML entitets-ID och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. Klicka på knappen **Spara**.

    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_general_400.png)

10. Att konfigurera enkel inloggning på **ADP Globalview** sida, som du behöver skicka de hämtade **certifikat (Base64)**, **URL för utloggning, SAML entitets-ID och SAML enkel inloggning för tjänst-URL** till [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx).

> [!TIP]
> Nu kan du läsa en kortare version av instruktionerna i [Azure Portal](https://portal.azure.com), samtidigt som du konfigurerar appen!  När du har lagt till appen från avsnittet **Active Directory > Företagsprogram**, behöver du bara klicka på fliken **Enkel inloggning**. Du kommer då till den inbäddade dokumentationen via avsnittet **Konfiguration** längst ned. Du kan läsa mer om funktionen för inbäddad dokumentation här: [Inbäddad Azure AD-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/adglobalview-tutorial/create_aaduser_01.png) 

2.  Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/adglobalview-tutorial/create_aaduser_02.png) 

3. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/adglobalview-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/adglobalview-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-adp-globalview-test-user"></a>Skapa en testanvändare ADP Globalview

Målet med det här avsnittet är att skapa en användare som kallas Britta Simon i ADP GlobalView. Arbeta med [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) att lägga till användare i ADP GlobalView-konto. 

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ADP Globalview.

![Tilldela användare][200] 

**Om du vill tilldela ADP Globalview Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

2. I listan med program väljer **ADP Globalview**.

    ![Konfigurera enkel inloggning](./media/adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Målet med det här avsnittet är att testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.  

När du klickar på panelen ADP GlobalView i åtkomstpanelen du bör få automatiskt loggat in på ditt ADP GlobalView program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/adglobalview-tutorial/tutorial_general_203.png

