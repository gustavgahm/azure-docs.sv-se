---
title: 'Självstudier: Azure Active Directory-integrering med Anaplan | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Anaplan.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4a9c2914-6c8c-4a88-93e3-3753afb40e6b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jeedes
ms.openlocfilehash: 0cb169b3400e32144428c7b4bd65a6943a0a1865
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175408"
---
# <a name="tutorial-azure-active-directory-integration-with-anaplan"></a>Självstudier: Azure Active Directory-integrering med Anaplan

I den här självstudien får du lära dig hur du integrerar Anaplan med Azure Active Directory (AD Azure).

Integrera Anaplan med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Anaplan
- Du kan aktivera användarna att automatiskt få loggat in på Anaplan (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Anaplan, behöver du följande objekt:

- En Azure AD-prenumeration
- En Anaplan enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du skaffa en månads utvärderingsperiod [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Anaplan från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-anaplan-from-the-gallery"></a>Att lägga till Anaplan från galleriet
För att konfigurera integrering av Anaplan i Azure AD, som du behöver lägga till Anaplan från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Anaplan från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Appar][2]
    
3. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Appar][3]

4. I sökrutan skriver **Anaplan**.

    ![Skapa en Azure AD-användare för testning](./media/anaplan-tutorial/tutorial_anaplan_search.png)

5. I resultatpanelen väljer **Anaplan**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/anaplan-tutorial/tutorial_anaplan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med Anaplan baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i Anaplan är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i Anaplan upprättas.

I Anaplan, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med Anaplan, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Anaplan](#creating-an-anaplan-test-user)**  – du har en motsvarighet för Britta Simon i Anaplan som är länkad till en Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Anaplan program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Anaplan:**

1. I Azure-portalen på den **Anaplan** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/anaplan-tutorial/tutorial_anaplan_samlbase.png)

3. På den **Anaplan domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/anaplan-tutorial/tutorial_anaplan_url.png)

    a. I textrutan **Inloggnings-URL** anger du en URL med följande mönster: `https://sdp.anaplan.com/frontdoor/saml/<tenant name>`

    b. I textrutan **Identifierare** anger du en URL med följande mönster: `https://<subdomain>.anaplan.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med faktisk inloggnings-URL och identifierare. Kontakta [Anaplan klienten supportteamet](mailto:support@anaplan.com) att hämta dessa värden. 
 
4. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.

    ![Konfigurera enkel inloggning](./media/anaplan-tutorial/tutorial_anaplan_certificate.png) 

5. Klicka på knappen **Spara**.

    ![Konfigurera enkel inloggning](./media/anaplan-tutorial/tutorial_general_400.png)

6. På den **Anaplan Configuration** klickar du på **konfigurera Anaplan** att öppna **konfigurera inloggning** fönster. Kopiera den **URL för utloggning, SAML entitets-ID och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Konfigurera enkel inloggning](./media/anaplan-tutorial/tutorial_anaplan_configure.png) 

7. Att konfigurera enkel inloggning på **Anaplan** sida, som du behöver skicka de hämtade **XML-Metadata för**, **SAML entitets-ID**, **SAML enkel inloggning för tjänst-URL** och **URL: en för utloggning** till [Anaplan supportteamet](mailto:support@anaplan.com). De anger inställningen så att SAML SSO-anslutningen ställs in korrekt på båda sidorna.

> [!TIP]
> Nu kan du läsa en kortare version av instruktionerna i [Azure Portal](https://portal.azure.com), samtidigt som du konfigurerar appen!  När du har lagt till appen från avsnittet **Active Directory > Företagsprogram**, behöver du bara klicka på fliken **Enkel inloggning**. Du kommer då till den inbäddade dokumentationen via avsnittet **Konfiguration** längst ned. Du kan läsa mer om funktionen för inbäddad dokumentation här: [Inbäddad Azure AD-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/anaplan-tutorial/create_aaduser_01.png) 

2. Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/anaplan-tutorial/create_aaduser_02.png) 

3. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/anaplan-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/anaplan-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-anaplan-test-user"></a>Skapa en Anaplan testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i Anaplan. Kontakta [Anaplan supportteamet](mailto:support@anaplan.com) att lägga till användare i Anaplan-plattformen.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Anaplan.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon Anaplan, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

2. I listan med program väljer **Anaplan**.

    ![Konfigurera enkel inloggning](./media/anaplan-tutorial/tutorial_anaplan_app.png) 

3. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen Anaplan i åtkomstpanelen du bör få automatiskt loggat in på ditt Anaplan program.

Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/anaplan-tutorial/tutorial_general_01.png
[2]: ./media/anaplan-tutorial/tutorial_general_02.png
[3]: ./media/anaplan-tutorial/tutorial_general_03.png
[4]: ./media/anaplan-tutorial/tutorial_general_04.png

[100]: ./media/anaplan-tutorial/tutorial_general_100.png

[200]: ./media/anaplan-tutorial/tutorial_general_200.png
[201]: ./media/anaplan-tutorial/tutorial_general_201.png
[202]: ./media/anaplan-tutorial/tutorial_general_202.png
[203]: ./media/anaplan-tutorial/tutorial_general_203.png

