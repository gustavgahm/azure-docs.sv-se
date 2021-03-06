---
title: 'Självstudier: Azure Active Directory-integrering med SD-element | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SD-element.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 0081946ff62e42e1fb601b135c7102dd83e84fe3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158918"
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Självstudier: Azure Active Directory-integrering med SD-element

I den här självstudien får du lära dig hur du integrerar SD-element med Azure Active Directory (AD Azure).

Integrera SD-element med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till SD-element
- Du kan aktivera användarna att automatiskt få loggat in på SD-element (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med SD-element, behöver du följande objekt:

- En Azure AD-prenumeration
- En SD element enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du skaffa en månads utvärderingsperiod [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till SD-element från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sd-elements-from-the-gallery"></a>Att lägga till SD-element från galleriet
För att konfigurera integrering av SD-element i Azure AD, som du behöver lägga till SD-element från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till SD-element från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Appar][2]
    
1. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Appar][3]

1. I sökrutan skriver **SD element**.

    ![Skapa en Azure AD-användare för testning](./media/sd-elements-tutorial/tutorial_sdelements_search.png)

1. I resultatpanelen väljer **SD element**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med SD-element som baseras på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i SD-element som är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i SD-element upprättas.

I SD-element, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med SD-element, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en testanvändare SD element](#creating-a-sd-elements-test-user)**  – du har en motsvarighet för Britta Simon i SD-element som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program för SD-element.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med SD-element:**

1. I Azure-portalen på den **SD element** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_sdelements_samlbase.png)

1. På den **SD element domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_sdelements_url.png)

    a. I textrutan **Identifierare** anger du en URL med följande mönster: `https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. I textrutan **Svars-URL** skriver du en URL med följande mönster: `https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med den faktiska identifieraren och svars-URL. Kontakta [SD element supportteam](mailto:support@sdelements.com) att hämta dessa värden.

1. SD element program som förväntar SAML-intyg i ett visst format. Konfigurera följande anspråk för det här programmet. Du kan hantera värdena för dessa attribut från den **”användarattribut”** fliken av programmet. Följande skärmbild visar ett exempel på detta.

    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_sdelements_attribute.png)

1. I den **användarattribut** avsnittet på den **enkel inloggning** dialogrutan Konfigurera SAML-token attributet som visas i bilden och utför följande steg: 

    | Attributnamn | Attributvärde |
    | --- | --- |
    | e-post |user.mail |
    | förnamn |user.givenname |
    | lastname |user.surname |

    a. Klicka på **Lägg till attribut** att öppna den **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_officespace_05.png)

    b. I textrutan **Namn** skriver du det attributnamn som visas för den raden.

    c. Från den **värdet** anger attributvärdet som visas för den raden.

    d. Klicka på **OK**.
 
1. På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_sdelements_certificate.png) 

1. Klicka på knappen **Spara**.

    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_general_400.png)

1. På den **SD element Configuration** klickar du på **konfigurera SD element** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML entitets-ID och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_sdelements_configure.png)

1. För att få enkel inloggning aktiverat kan du kontakta din [SD element supportteam](mailto:support@sdelements.com) och ge dem hämtade certifikatfilen. 

1. I ett annat webbläsarfönster inloggning till SD-element-klienten som administratör.

1. Klicka på menyn längst upp **System**, och sedan **enkel inloggning**. 
   
    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_sd-elements_09.png) 

1. På den **inställningar för enkel inloggning** dialogrutan utför följande steg:
   
    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Som **SSO typ**väljer **SAML**.
   
    b. I den **identitet providern entitets-ID** textrutan klistra in värdet för **SAML entitets-ID**, som du har kopierat från Azure-portalen. 
   
    c. I den **providern enkel inloggning Identitetstjänst** textrutan klistra in värdet för **SAML enkel inloggning för tjänst-URL**, som du har kopierat från Azure-portalen. 
   
    d. Klicka på **Spara**.

> [!TIP]
> Nu kan du läsa en kortare version av instruktionerna i [Azure Portal](https://portal.azure.com), samtidigt som du konfigurerar appen!  När du har lagt till appen från avsnittet **Active Directory > Företagsprogram**, behöver du bara klicka på fliken **Enkel inloggning**. Du kommer då till den inbäddade dokumentationen via avsnittet **Konfiguration** längst ned. Du kan läsa mer om funktionen för inbäddad dokumentation här: [Inbäddad Azure AD-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/sd-elements-tutorial/create_aaduser_01.png) 

1. Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/sd-elements-tutorial/create_aaduser_02.png) 

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/sd-elements-tutorial/create_aaduser_03.png) 

1. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/sd-elements-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-sd-elements-test-user"></a>Skapa en testanvändare SD-element

Målet med det här avsnittet är att skapa en användare som kallas Britta Simon i SD-element. När det gäller SD-element är skapa SD element användare en manuell aktivitet.

**Utför följande steg för att skapa Britta Simon i SD-element:**

1. I ett webbläsarfönster inloggning till webbplatsen för företagets SD-element som administratör.

1. Klicka på menyn längst upp **Användarhantering**, och sedan **användare**.
   
    ![Skapa en testanvändare SD-element](./media/sd-elements-tutorial/tutorial_sd-elements_11.png) 

1. Klicka på **Lägg till ny användare**.
   
    ![Skapa en testanvändare SD-element](./media/sd-elements-tutorial/tutorial_sd-elements_12.png)
 
1. På den **Lägg till ny användare** dialogrutan utför följande steg:
   
    ![Skapa en testanvändare SD-element](./media/sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. I den **e** textrutan Ange e-postadress för användaren som **brittasimon@contoso.com**.
   
    b. I den **Förnamn** textrutan Ange först namnet på användaren som **Britta**.
   
    c. I den **efternamn** textrutan Ange efternamn för användaren som **Simon**.
   
    d. Som **rollen**väljer **användaren**. 
   
    e. Klicka på **skapa användare**.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SD-element.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon SD-element, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **SD element**.

    ![Konfigurera enkel inloggning](./media/sd-elements-tutorial/tutorial_sdelements_app.png) 

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Målet med det här avsnittet är att prova Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.
  
När du klickar på panelen SD-element i åtkomstpanelen du bör få automatiskt loggat in på programmets SD-element.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/sd-elements-tutorial/tutorial_general_203.png

