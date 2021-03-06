---
title: 'Självstudier: Azure Active Directory-integrering med SAML SSO för bambu resolution GmbH | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SAML SSO för bambu resolution GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 027f5a9f02a0580fce61091e8be9ece9069fb34f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156190"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Självstudier: Azure Active Directory-integrering med SAML SSO för bambu resolution GmbH

I den här självstudien får du lära dig hur du integrerar SAML SSO för bambu resolution GmbH med Azure Active Directory (AD Azure).

Integrera SAML SSO för bambu resolution GmbH med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till SAML SSO för bambu resolution GmbH.
- Du kan aktivera användarna att automatiskt få loggat in på SAML SSO för bambu resolution GmbH (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure-portalen.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med SAML SSO för bambu resolution GmbH, behöver du följande objekt:

- En Azure AD-prenumeration
- En SAML SSO för bambu av upplösning GmbH, enkel inloggning på aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till SAML SSO för bambu resolution GmbH från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>Att lägga till SAML SSO för bambu resolution GmbH från galleriet
För att konfigurera integrering av SAML SSO för bambu resolution GmbH i Azure AD, som du behöver lägga till SAML SSO för bambu resolution GmbH från galleriet i din lista över hanterade SaaS-appar.

**Om du vill lägga till SAML SSO för bambu resolution GmbH från galleriet, utför du följande steg:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]
    
1. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Knappen Nytt program][3]

1. I sökrutan skriver **SAML SSO för bambu resolution GmbH**väljer **SAML SSO för bambu resolution GmbH** resultatet panelen klickar **Lägg till** för att lägga till den programmet.

    ![SAML SSO för bambu av lösningen GmbH i listan med resultat](./media/bamboo-tutorial/tutorial_bamboo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa enkel inloggning med Azure AD

I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med SAML SSO för bambu resolution GmbH baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vilka motsvarande användaren i SAML SSO för bambu resolution GmbH är att en användare i Azure AD. Med andra ord en länk relationen mellan en Azure AD-användare och relaterade användaren i SAML SSO för bambu resolution GmbH måste upprättas.

I SAML SSO för bambu resolution GmbH, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med SAML SSO för bambu resolution GmbH, måste du utföra följande byggblock:

1. **[Konfigurera enkel inloggning med Azure AD](#configure-azure-ad-single-sign-on)** – så att användarna kan använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)** – för att testa enkel inloggning med Azure AD med Britta Simon.
1. **[Skapa en SAML SSO för bambu av lösning GmbH testanvändare](#create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user)**  – du har en motsvarighet för Britta Simon i SAML SSO för bambu resolution GmbH som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändaren](#assign-the-azure-ad-test-user)** – så att Britta Simon kan använda enkel inloggning med Azure AD.
1. **[Testa enkel inloggning](#test-single-sign-on)** – för att verifiera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera enkel inloggning med Azure AD

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din SAML SSO för bambu resolution GmbH program.

**Om du vill konfigurera Azure AD enkel inloggning med SAML SSO för bambu resolution GmbH, utför du följande steg:**

1. I Azure-portalen på den **SAML SSO för bambu resolution GmbH** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera länk för enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Enkel inloggning för dialogrutan](./media/bamboo-tutorial/tutorial_bamboo_samlbase.png)

1. På den **SAML SSO bambu resolution GmbH domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i IDP-initierad läge:

    ![SAML SSO för bambu resolution GmbH domän och URL: er med enkel inloggning för information](./media/bamboo-tutorial/tutorial_bamboo_url.png)

    a. I textrutan **Identifierare** anger du en URL med följande mönster: `https://<server-base-url>/plugins/servlet/samlsso`

    b. I textrutan **Svars-URL** skriver du en URL med följande mönster: `https://<server-base-url>/plugins/servlet/samlsso`

1. Kontrollera **visa avancerade URL-inställningar** och utföra följande steg om du vill konfigurera programmet i **SP** initierade läge:

    ![SAML SSO för bambu resolution GmbH domän och URL: er med enkel inloggning för information](./media/bamboo-tutorial/tutorial_bamboo_url1.png)

    I textrutan **Inloggnings-URL** anger du en URL med följande mönster: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera de här värdena med den faktiska identifieraren, svars-URL och inloggnings-URL. Kontakta [SAML SSO för bambu resolution GmbH klienten supportteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) att hämta dessa värden. 

1. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.

    ![Länk för hämtning av certifikat](./media/bamboo-tutorial/tutorial_bamboo_certificate.png) 

1. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning – knappen Spara](./media/bamboo-tutorial/tutorial_general_400.png)

1. Inloggning till SAML SSO för bambu av lösning GmbH företagets plats som administratör.

1. Till höger i verktygsfältet klickar du på **inställningar** > **tillägg**.

    ![Inställningarna](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. Gå till säkerhets-avsnittet, klickar du på **SAML SingleSignOn** på menyraden.

    ![Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. På den **konfigurationssidan för SAML SIngleSignOn plugin-programmet**, klickar du på **lägga till idp**. 

    ![Den lägger till idp](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. På den **väljer SAML-identitetsprovider** kan utföra följande steg:

    ![Identitetsprovidern](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. Välj **Idp typ** som **AZURE AD**.

    b. I den **namn** textrutan skriver du namnet.

    c. I den **beskrivning** textrutan anger du beskrivningen.

    d. Klicka på **Nästa**.

1. På den **identitet providerkonfigurationen** klickar du på **nästa**.

    ![Identity-config](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1.  På den **importera SAML Idp Metadata** , klickar du på **Läs in fil** att ladda upp den **XML-METADATA för** fil som du har hämtat från Azure-portalen.

    ![Idpmetadata](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. Klicka på **Nästa**.

1. Klicka på **Spara inställningar**.

    ![Spara](./media/bamboo-tutorial/tutorial_bamboo_save.png)
    
> [!TIP]
> Nu kan du läsa en kortare version av instruktionerna i [Azure Portal](https://portal.azure.com), samtidigt som du konfigurerar appen!  När du har lagt till appen från avsnittet **Active Directory > Företagsprogram**, behöver du bara klicka på fliken **Enkel inloggning**. Du kommer då till den inbäddade dokumentationen via avsnittet **Konfiguration** längst ned. Du kan läsa mer om funktionen för inbäddad dokumentation här: [Inbäddad Azure AD-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en Azure AD-testanvändare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen, i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/bamboo-tutorial/create_aaduser_01.png)

1. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare”-länkar](./media/bamboo-tutorial/create_aaduser_02.png)

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/bamboo-tutorial/create_aaduser_03.png)

1. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/bamboo-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** skriver användarens Britta Simon e-postadress.

    c. Välj den **visa lösenord** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** box.

    d. Klicka på **Skapa**.
 
### <a name="create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>Skapa en SAML SSO för bambu av lösning GmbH testanvändare

Målet med det här avsnittet är att skapa en användare som kallas Britta Simon i SAML SSO för bambu resolution GmbH. SAML SSO för bambu resolution GmbH har stöd för just-in-time-etablering och även användare kan skapas manuellt, kontakta [SAML SSO för bambu resolution GmbH klienten supportteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) enligt dina behov.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SAML SSO för bambu resolution GmbH.

![Tilldela rollen][200] 

**Om du vill tilldela SAML SSO för bambu Britta Simon resolution GmbH, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **SAML SSO för bambu resolution GmbH**.

    ![SAML SSO för bambu per lösning GmbH länk i listan med program](./media/bamboo-tutorial/tutorial_bamboo_app.png)  

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Länken ”användare och grupper”][202]

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på SAML SSO för bambu av lösning GmbH panel i åtkomstpanelen du bör få automatiskt loggat in på ditt SAML SSO för bambu resolution GmbH program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bamboo-tutorial/tutorial_general_01.png
[2]: ./media/bamboo-tutorial/tutorial_general_02.png
[3]: ./media/bamboo-tutorial/tutorial_general_03.png
[4]: ./media/bamboo-tutorial/tutorial_general_04.png

[100]: ./media/bamboo-tutorial/tutorial_general_100.png

[200]: ./media/bamboo-tutorial/tutorial_general_200.png
[201]: ./media/bamboo-tutorial/tutorial_general_201.png
[202]: ./media/bamboo-tutorial/tutorial_general_202.png
[203]: ./media/bamboo-tutorial/tutorial_general_203.png

