---
title: 'Självstudier: Azure Active Directory-integrering med IBM OpenPages | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och IBM OpenPages.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 74d5dea5-2c48-464a-a7d0-cdd481c429d7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jeedes
ms.openlocfilehash: a9efdc918340d127c554e84d29db2b2df784a4ce
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181630"
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-openpages"></a>Självstudier: Azure Active Directory-integrering med IBM OpenPages

I den här självstudien får du lära dig hur du integrerar IBM OpenPages med Azure Active Directory (AD Azure).

Integrera IBM OpenPages med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till IBM OpenPages.
- Du kan aktivera användarna att automatiskt få loggat in på IBM OpenPages (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure-portalen.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med IBM OpenPages, behöver du följande objekt:

- En Azure AD-prenumeration
- En IBM OpenPages enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till IBM OpenPages från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-ibm-openpages-from-the-gallery"></a>Att lägga till IBM OpenPages från galleriet
För att konfigurera integrering av IBM OpenPages i Azure AD, som du behöver lägga till IBM OpenPages från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till IBM OpenPages från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]
    
1. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Knappen Nytt program][3]

1. I sökrutan skriver **IBM OpenPages**väljer **IBM OpenPages** resultatet panelen klickar **Lägg till** för att lägga till programmet.

    ![IBM OpenPages i resultatlistan](./media/ibmopenpages-tutorial/tutorial_ibmopenpage_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa enkel inloggning med Azure AD

I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med IBM OpenPages utifrån en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i IBM OpenPages är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i IBM OpenPages upprättas.

I IBM OpenPages tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med IBM OpenPages, måste du utföra följande byggblock:

1. **[Konfigurera enkel inloggning med Azure AD](#configure-azure-ad-single-sign-on)** – så att användarna kan använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)** – för att testa enkel inloggning med Azure AD med Britta Simon.
1. **[Skapa en testanvändare för IBM OpenPages](#create-an-ibm-openpages-test-user)**  – du har en motsvarighet för Britta Simon i IBM OpenPages som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändaren](#assign-the-azure-ad-test-user)** – så att Britta Simon kan använda enkel inloggning med Azure AD.
1. **[Testa enkel inloggning](#test-single-sign-on)** – för att verifiera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera enkel inloggning med Azure AD

I det här avsnittet ska du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt OpenPages för IBM-program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med IBM OpenPages:**

1. I Azure-portalen på den **IBM OpenPages** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera länk för enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Enkel inloggning för dialogrutan](./media/ibmopenpages-tutorial/tutorial_ibmopenpage_samlbase.png)

1. På den **IBM OpenPages domän och URL: er** avsnittet, utför följande steg:

    ![IBM OpenPages domän och URL: er med enkel inloggning för information](./media/ibmopenpages-tutorial/tutorial_ibmopenpage_url.png)

    a. I textrutan **Identifierare** anger du en URL med följande mönster: `http://<subdomain>.ibm.com:<ID>/openpages`

    b. I textrutan **Svars-URL** skriver du en URL med följande mönster: `https://<subdomain>.ibm.com:<ID>/samlsps/op`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med den faktiska identifieraren och svars-URL. Kontakta [IBM OpenPages klienten supportteamet](https://www.ibm.com/support/home/) att hämta dessa värden. 
 
1. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.

    ![Länk för hämtning av certifikat](./media/ibmopenpages-tutorial/tutorial_ibmopenpage_certificate.png) 

1. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning – knappen Spara](./media/ibmopenpages-tutorial/tutorial_general_400.png)

1. Att konfigurera enkel inloggning på **IBM OpenPages** sida, som du behöver skicka de hämtade **XML-Metadata för** till [IBM OpenPages supportteamet](https://www.ibm.com/support/home/). De anger inställningen så att SAML SSO-anslutningen ställs in korrekt på båda sidorna.

> [!TIP]
> Nu kan du läsa en kortare version av instruktionerna i [Azure Portal](https://portal.azure.com), samtidigt som du konfigurerar appen!  När du har lagt till appen från avsnittet **Active Directory > Företagsprogram**, behöver du bara klicka på fliken **Enkel inloggning**. Du kommer då till den inbäddade dokumentationen via avsnittet **Konfiguration** längst ned. Du kan läsa mer om funktionen för inbäddad dokumentation här: [Inbäddad Azure AD-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en Azure AD-testanvändare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen, i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/ibmopenpages-tutorial/create_aaduser_01.png)

1. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare”-länkar](./media/ibmopenpages-tutorial/create_aaduser_02.png)

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/ibmopenpages-tutorial/create_aaduser_03.png)

1. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/ibmopenpages-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** skriver användarens Britta Simon e-postadress.

    c. Välj den **visa lösenord** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** box.

    d. Klicka på **Skapa**.
 
### <a name="create-an-ibm-openpages-test-user"></a>Skapa en IBM OpenPages testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i IBM OpenPages. Arbeta med [IBM OpenPages supportteamet](https://www.ibm.com/support/home/) att lägga till användare i IBM OpenPages-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till IBM OpenPages.

![Tilldela rollen][200] 

**Om du vill tilldela IBM OpenPages Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **IBM OpenPages**.

    ![IBM OpenPages länken i listan med program](./media/ibmopenpages-tutorial/tutorial_ibmopenpage_app.png)  

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Länken ”användare och grupper”][202]

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen IBM OpenPages i åtkomstpanelen du bör få automatiskt loggat in på ditt OpenPages för IBM-program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ibmopenpages-tutorial/tutorial_general_01.png
[2]: ./media/ibmopenpages-tutorial/tutorial_general_02.png
[3]: ./media/ibmopenpages-tutorial/tutorial_general_03.png
[4]: ./media/ibmopenpages-tutorial/tutorial_general_04.png

[100]: ./media/ibmopenpages-tutorial/tutorial_general_100.png

[200]: ./media/ibmopenpages-tutorial/tutorial_general_200.png
[201]: ./media/ibmopenpages-tutorial/tutorial_general_201.png
[202]: ./media/ibmopenpages-tutorial/tutorial_general_202.png
[203]: ./media/ibmopenpages-tutorial/tutorial_general_203.png

