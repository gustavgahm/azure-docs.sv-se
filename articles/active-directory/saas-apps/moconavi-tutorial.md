---
title: 'Självstudier: Azure Active Directory-integrering med moconavi | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och moconavi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 3009cb42ac477b18d45ab5968d6f5793ce1cd36c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165905"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Självstudier: Azure Active Directory-integrering med moconavi

I den här självstudien får du lära dig hur du integrerar moconavi med Azure Active Directory (AD Azure).

Integrera moconavi med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till moconavi.
- Du kan aktivera användarna att automatiskt få loggat in på moconavi (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure-portalen.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med moconavi, behöver du följande objekt:

- En Azure AD-prenumeration
- En moconavi enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö.
Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till moconavi från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-moconavi-from-the-gallery"></a>Att lägga till moconavi från galleriet
För att konfigurera integrering av moconavi i Azure AD, som du behöver lägga till moconavi från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till moconavi från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]

3. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Knappen Nytt program][3]

4. I sökrutan skriver **moconavi**väljer **moconavi** resultatet panelen klickar **Lägg till** för att lägga till programmet.

    ![moconavi i resultatlistan](./media/moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa enkel inloggning med Azure AD

I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med moconavi baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i moconavi är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i moconavi upprättas.

Om du vill konfigurera och testa Azure AD enkel inloggning med moconavi, måste du utföra följande byggblock:

1. **[Konfigurera enkel inloggning med Azure AD](#configure-azure-ad-single-sign-on)** – så att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)** – för att testa enkel inloggning med Azure AD med Britta Simon.
3. **[Skapa en testanvändare moconavi](#create-a-moconavi-test-user)**  – du har en motsvarighet för Britta Simon i moconavi som är länkad till en Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändaren](#assign-the-azure-ad-test-user)** – så att Britta Simon kan använda enkel inloggning med Azure AD.
5. **[Testa enkel inloggning](#test-single-sign-on)** – för att verifiera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera enkel inloggning med Azure AD

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt moconavi program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med moconavi:**

1. I Azure-portalen på den **moconavi** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera länk för enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.

    ![Enkel inloggning för dialogrutan](./media/moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. På den **moconavi domän och URL: er** avsnittet, utför följande steg:

    ![moconavi domän och URL: er enkel inloggning för information](./media/moconavi-tutorial/tutorial_moconavi_url.png)

    a. I textrutan **Inloggnings-URL** anger du en URL med följande mönster: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. I textrutan **Identifierare** anger du en URL med följande mönster: `https://<yourserverurl>/moconavi-saml2`

    C. I textrutan **Svars-URL** skriver du en URL med följande mönster: `https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera de här värdena med den faktiska inloggnings-URL:en, identifieraren och svars-URL:en. Kontakta [moconavi klienten supportteamet](mailto:support@recomot.co.jp) att hämta dessa värden.

4. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.

    ![Länk för hämtning av certifikat](./media/moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning – knappen Spara](./media/moconavi-tutorial/tutorial_general_400.png)

6. Att konfigurera enkel inloggning på **moconavi** sida, som du behöver skicka de hämtade **XML-Metadata för** till [moconavi supportteamet](mailto:support@recomot.co.jp). De anger inställningen så att SAML SSO-anslutningen ställs in korrekt på båda sidorna.

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en Azure AD-testanvändare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen, i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/moconavi-tutorial/create_aaduser_01.png)

2. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare”-länkar](./media/moconavi-tutorial/create_aaduser_02.png)

3. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/moconavi-tutorial/create_aaduser_03.png)

4. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/moconavi-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** skriver användarens Britta Simon e-postadress.

    c. Välj den **visa lösenord** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** box.

    d. Klicka på **Skapa**.

### <a name="create-a-moconavi-test-user"></a>Skapa en moconavi testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i moconavi. Arbeta med [moconavi supportteamet](mailto:support@recomot.co.jp) att lägga till användare i moconavi-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till moconavi.

![Tilldela rollen][200]

**Om du vill tilldela Britta Simon moconavi, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201]

2. I listan med program väljer **moconavi**.

    ![Länken moconavi i listan med program](./media/moconavi-tutorial/tutorial_moconavi_app.png)

3. I menyn till vänster, klickar du på **användare och grupper**.

    ![Länken ”användare och grupper”][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

5. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.

### <a name="test-single-sign-on"></a>Testa enkel inloggning

1. Installera moconavi från Microsoft store.

2. Starta moconavi.

3. Klicka på **ansluta inställningen** knappen.

    ![Testa enkel inloggning](./media/moconavi-tutorial/testing1.png)

4. Ange `https://mcs-admin.moconavi.biz/gateway` till **Anslut till URL: en** textrutan och klicka sedan på **klar** knappen.

    ![Testa enkel inloggning](./media/moconavi-tutorial/testing2.png)

5. Utför följande steg på följande skärmbild:

    ![Testa enkel inloggning](./media/moconavi-tutorial/testing3.png)

    a. Ange **indata autentiseringsnyckeln**:`azureAD` till **indata autentiseringsnyckeln** textrutan.

    b. Ange **indata användar-ID**: `your ad account` till **indata användar-ID** textrutan.

    c. Klicka på **inloggning**.

6. Ange din Azure AD-lösenord till **lösenord** textrutan och klicka sedan på **inloggning** knappen.

    ![Testa enkel inloggning](./media/moconavi-tutorial/testing4.png)

7. Azure AD-autentisering har lyckats när menyn visas.

    ![Testa enkel inloggning](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/moconavi-tutorial/tutorial_general_01.png
[2]: ./media/moconavi-tutorial/tutorial_general_02.png
[3]: ./media/moconavi-tutorial/tutorial_general_03.png
[4]: ./media/moconavi-tutorial/tutorial_general_04.png

[100]: ./media/moconavi-tutorial/tutorial_general_100.png

[200]: ./media/moconavi-tutorial/tutorial_general_200.png
[201]: ./media/moconavi-tutorial/tutorial_general_201.png
[202]: ./media/moconavi-tutorial/tutorial_general_202.png
[203]: ./media/moconavi-tutorial/tutorial_general_203.png

