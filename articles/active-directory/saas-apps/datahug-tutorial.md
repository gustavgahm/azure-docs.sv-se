---
title: 'Självstudier: Azure Active Directory-integrering med Datahug | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Datahug.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: 6f1ba5ed3da451200c944145376a0f8a3b550e71
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55162879"
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a>Självstudier: Azure Active Directory-integrering med Datahug

I den här självstudien får du lära dig hur du integrerar Datahug med Azure Active Directory (AD Azure).

Integrera Datahug med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Datahug
- Du kan aktivera användarna att automatiskt få loggat in på Datahug (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i. [Vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Datahug, behöver du följande objekt:

- En Azure AD-prenumeration
- En Datahug enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du skaffa en månads utvärderingsperiod [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Datahug från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-datahug-from-the-gallery"></a>Att lägga till Datahug från galleriet
För att konfigurera integrering av Datahug i Azure AD, som du behöver lägga till Datahug från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Datahug från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Appar][2]
    
1. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Appar][3]

1. I sökrutan skriver **Datahug**.

    ![Skapa en Azure AD-användare för testning](./media/datahug-tutorial/tutorial_datahug_search.png)

1. I resultatpanelen väljer **Datahug**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med Datahug baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i Datahug är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i Datahug upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Datahug.

Om du vill konfigurera och testa Azure AD enkel inloggning med Datahug, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en testanvändare Datahug](#creating-a-datahug-test-user)**  – du har en motsvarighet för Britta Simon i Datahug som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Datahug program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Datahug:**

1. I Azure-portalen på den **Datahug** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/datahug-tutorial/tutorial_datahug_samlbase.png)

1. På den **Datahug domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/datahug-tutorial/tutorial_datahug_ur1.png)

    a. I textrutan **Identifierare** anger du en URL med följande mönster: `https://apps.datahug.com/identity/<uniqueID>`

    b. I textrutan **Svars-URL** skriver du en URL med följande mönster: `https://apps.datahug.com/identity/<uniqueID>/acs`

1. Kontrollera **visa avancerade URL-inställningar**. Om du vill konfigurera programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/datahug-tutorial/tutorial_datahug_url2.png)

    I den **inloggnings-URL** textrutan anger du ett URL: en som: `https://apps.datahug.com/`
     
    > [!NOTE] 
    > De här värdena är inte verkliga. Uppdatera dessa värden med den faktiska identifieraren och svars-URL. Här rekommenderar vi att du om du vill använda det unika värdet av strängen i identifierare och svars-URL. Kontakta [Datahug klienten supportteamet](http://datahug.com/about/contact-us/) att hämta dessa värden. 

1. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.

    ![Konfigurera enkel inloggning](./media/datahug-tutorial/tutorial_datahug_certificate.png) 

1.  Kontrollera **”visa avancerade inställningar för signering av certifikat”** och utför följande steg:

    ![Konfigurera enkel inloggning](./media/datahug-tutorial/tutorial_datahug_cert.png)

    a. I **signering alternativet**väljer **inloggning SAML-försäkran**.
    
    b. I **signering algoritmen**väljer **SHA1**.
 
1. Klicka på knappen **Spara**.

    ![Konfigurera enkel inloggning](./media/datahug-tutorial/tutorial_general_400.png)
    
1. På den **Datahug Configuration** klickar du på **konfigurera Datahug** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML entitets-ID** och **SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Konfigurera enkel inloggning](./media/datahug-tutorial/tutorial_datahug_configure.png) 

1. Att konfigurera enkel inloggning på **Datahug** sida, som du behöver skicka de hämtade **XML-Metadata för**, **SAML entitets-ID** och **SAML enkel inloggning för tjänst-URL**  till [Datahug support](http://datahug.com/about/contact-us/). De har konfigurerat det här programmet har SAML SSO-anslutningen korrekt inställda på båda sidorna.

> [!TIP]
> Nu kan du läsa en kortare version av instruktionerna i [Azure Portal](https://portal.azure.com), samtidigt som du konfigurerar appen!  När du har lagt till appen från avsnittet **Active Directory > Företagsprogram**, behöver du bara klicka på fliken **Enkel inloggning**. Du kommer då till den inbäddade dokumentationen via avsnittet **Konfiguration** längst ned. Du kan läsa mer om funktionen för inbäddad dokumentation här: [Inbäddad Azure AD-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/datahug-tutorial/create_aaduser_01.png) 

1. Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/datahug-tutorial/create_aaduser_02.png) 

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/datahug-tutorial/create_aaduser_03.png) 

1. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/datahug-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-datahug-test-user"></a>Skapa en Datahug testanvändare

Om du vill aktivera Azure AD-användare att logga in på Datahug, måste de etableras i Datahug.  
När Datahug, etablering är en manuell aktivitet.

**Utför följande steg för att etablera ett användarkonto:**

1. Logga in på webbplatsen Datahug företag som administratör.

1. Hovra över den **kugghjulet** i övre högra hörnet och klicka på **inställningar**
   
   ![Lägga till medarbetare](./media/datahug-tutorial/1.png)

1. Välj **personer** och klicka på den **Lägg till användare** fliken

    ![Lägga till medarbetare](./media/datahug-tutorial/2.png)

1. Skriv e-postadress för den person som du vill skapa ett konto för och klicka på **Lägg till**.

    ![Lägga till medarbetare](./media/datahug-tutorial/3.png)

    > [!NOTE] 
    > Du kan skicka e-post för registrering till användaren genom att välja **skicka välkomstmeddelande** kryssrutan.  
    > Om du skapar ett konto för Salesforce inte skicka välkomstmeddelandet.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Datahug.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon Datahug, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **Datahug**.

    ![Konfigurera enkel inloggning](./media/datahug-tutorial/tutorial_datahug_app.png) 

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.
När du klickar på panelen Datahug i åtkomstpanelen du bör få automatiskt loggat in på ditt Datahug program. Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/datahug-tutorial/tutorial_general_01.png
[2]: ./media/datahug-tutorial/tutorial_general_02.png
[3]: ./media/datahug-tutorial/tutorial_general_03.png
[4]: ./media/datahug-tutorial/tutorial_general_04.png

[100]: ./media/datahug-tutorial/tutorial_general_100.png

[200]: ./media/datahug-tutorial/tutorial_general_200.png
[201]: ./media/datahug-tutorial/tutorial_general_201.png
[202]: ./media/datahug-tutorial/tutorial_general_202.png
[203]: ./media/datahug-tutorial/tutorial_general_203.png

