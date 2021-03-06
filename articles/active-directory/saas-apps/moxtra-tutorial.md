---
title: 'Självstudier: Azure Active Directory-integrering med Moxtra | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Moxtra.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: e0674c0bd3e5244b76d35e05057aee3b75249703
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197117"
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Självstudier: Azure Active Directory-integrering med Moxtra

I den här självstudien får du lära dig hur du integrerar Moxtra med Azure Active Directory (AD Azure).

Integrera Moxtra med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Moxtra
- Du kan aktivera användarna att automatiskt få loggat in på Moxtra (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Moxtra, behöver du följande objekt:

- En Azure AD-prenumeration
- En Moxtra enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du skaffa en månads utvärderingsperiod [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Moxtra från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-moxtra-from-the-gallery"></a>Att lägga till Moxtra från galleriet
För att konfigurera integrering av Moxtra i Azure AD, som du behöver lägga till Moxtra från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Moxtra från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Appar][2]
    
1. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Appar][3]

1. I sökrutan skriver **Moxtra**.

    ![Skapa en Azure AD-användare för testning](./media/moxtra-tutorial/tutorial_moxtra_search.png)

1. I resultatpanelen väljer **Moxtra**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med Moxtra baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i Moxtra är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i Moxtra upprättas.

I Moxtra, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med Moxtra, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en testanvändare Moxtra](#creating-a-moxtra-test-user)**  – du har en motsvarighet för Britta Simon i Moxtra som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Moxtra program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Moxtra:**

1. I Azure-portalen på den **Moxtra** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_samlbase.png)

1. På den **Moxtra domän och URL: er** avsnittet, utföra följande steg:

    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_url.png)

    I den **inloggnings-URL** textrutan anger du ett URL: en som: `https://www.moxtra.com/service/#login`

1. Moxtra program som förväntar SAML-intyg i ett visst format. Konfigurera följande anspråk för det här programmet. Du kan hantera värdena för dessa attribut från den ”**användarattribut**” på sidan för integrering av program. Följande skärmbild visar ett exempel för den här konfigurationen. 

    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_attributes.png)
    
1. I den **användarattribut** avsnittet på den **enkel inloggning** dialogrutan Konfigurera SAML-token attributet som visas i bilden och utför följande steg:
    
    | Attributnamn | Attributvärde |
    | ------------------- | -------------------- |    
    | förnamn | user.givenname |
    | lastname | user.surname |
    | idpid    | < SAML entitets-ID > 

    > [!Note]
    > Värdet för **idpid** attributet inte är verkliga. Du kan få det faktiska värdet från **Snabbreferens** avsnittet **Moxtra Configuration**.
    
    a. Klicka på **Lägg till attribut** att öppna den **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_attribute_04.png)

    b. I textrutan **Namn** skriver du det attributnamn som visas för den raden.

    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_attribute_05.png)

    c. Från den **värdet** anger attributvärdet som visas för den raden.

    d. Klicka på **OK**.
    
1. På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_certificate.png) 

1. Klicka på knappen **Spara**.

    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_general_400.png)

1. På den **Moxtra Configuration** klickar du på **konfigurera Moxtra** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML entitets-ID och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_configure.png) 

1. I ett nytt webbläsarfönster inloggning till webbplatsen Moxtra företag som administratör.

1. I verktygsfältet till vänster, klickar du på **-administratörskonsolen > SAML enkel inloggning**, och klicka sedan på **New**.
   
    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_06.png) 

1. På den **SAML** utför följande steg:
   
    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. I den **namn** textrutan anger du ett namn för din konfiguration (t.ex.: *SAML*). 
  
    b. I den **IdP entitets-ID** textrutan klistra in värdet för **SAML entitets-ID** som du har kopierat från Azure-portalen. 
 
    c. I **inloggnings-URL** textrutan klistra in värdet för **SAML inloggnings-tjänst-URL för enkel** som du har kopierat från Azure-portalen. 
 
    d. I den **AuthnContextClassRef** textrutan typ **urn: oasis: namn: tc: SAML:2.0:ac:classes:Password**. 
 
    e. I den **NameID-Format** textrutan typ **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**. 
 
    f. Öppna certifikat som du har hämtat från Azure-portalen i anteckningar, kopiera innehållet och klistra in den i den **certifikat** textrutan.    
 
    g. Skriv din e-postdomän SAML i textrutan SAML e-domän.    
  
    >[!NOTE]
    >Om du vill se stegen för att verifiera domänen, klickar du på den ”**jag**” nedan.

    h. Klicka på **Uppdatera**.

> [!TIP]
> Nu kan du läsa en kortare version av instruktionerna i [Azure Portal](https://portal.azure.com), samtidigt som du konfigurerar appen!  När du har lagt till appen från avsnittet **Active Directory > Företagsprogram**, behöver du bara klicka på fliken **Enkel inloggning**. Du kommer då till den inbäddade dokumentationen via avsnittet **Konfiguration** längst ned. Du kan läsa mer om funktionen för inbäddad dokumentation här: [Inbäddad Azure AD-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/moxtra-tutorial/create_aaduser_01.png) 

1. Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/moxtra-tutorial/create_aaduser_02.png) 

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/moxtra-tutorial/create_aaduser_03.png) 

1. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/moxtra-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-moxtra-test-user"></a>Skapa en Moxtra testanvändare

Målet med det här avsnittet är att skapa en användare som kallas Britta Simon i Moxtra.

**Om du vill skapa en användare som kallas Britta Simon i Moxtra, utför du följande steg:**

1. Logga in på webbplatsen Moxtra företag som administratör.

1. I verktygsfältet till vänster, klickar du på **-administratörskonsolen > Användarhantering**, och sedan **Lägg till användare**.
   
    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_10.png) 

1. På den **Lägg till användare** dialogrutan utför följande steg:
  
    a. I den **Förnamn** textrutan typ **Britta**.
  
    b. I den **efternamn** textrutan typ **Simon**.
  
    c. I den **e-post** textrutan typ Brittas e-postadressen samma som på Azure-portalen.
  
    d. I den **Division** textrutan typ **Dev**.
  
    e. I den **avdelning** textrutan typ **IT**.
  
    f. Välj **administratör**.
  
    g. Klicka på **Lägg till**.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Moxtra.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon Moxtra, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **Moxtra**.

    ![Konfigurera enkel inloggning](./media/moxtra-tutorial/tutorial_moxtra_app.png) 

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen Moxtra i åtkomstpanelen du bör få automatiskt loggat in på ditt Moxtra program.
Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/moxtra-tutorial/tutorial_general_01.png
[2]: ./media/moxtra-tutorial/tutorial_general_02.png
[3]: ./media/moxtra-tutorial/tutorial_general_03.png
[4]: ./media/moxtra-tutorial/tutorial_general_04.png

[100]: ./media/moxtra-tutorial/tutorial_general_100.png

[200]: ./media/moxtra-tutorial/tutorial_general_200.png
[201]: ./media/moxtra-tutorial/tutorial_general_201.png
[202]: ./media/moxtra-tutorial/tutorial_general_202.png
[203]: ./media/moxtra-tutorial/tutorial_general_203.png

