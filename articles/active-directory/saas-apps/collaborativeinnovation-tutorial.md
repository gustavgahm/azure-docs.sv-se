---
title: 'Självstudier: Azure Active Directory-integrering med samarbetsfunktioner Innovation | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och samarbetsfunktioner Innovation.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: f8578ee37754a411b496df93f8af54398ece9f40
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55151422"
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a>Självstudier: Azure Active Directory-integrering med samarbetsfunktioner Innovation

Lär dig hur du integrerar samarbetsfunktioner Innovation med Azure Active Directory (AD Azure) i den här självstudien.

Integrera samarbetsfunktioner Innovation med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har tillgång till samarbetsfunktioner Innovation
- Du kan aktivera användarna att automatiskt få loggat in på samarbetsfunktioner Innovation (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med samarbetsfunktioner Innovation, behöver du följande objekt:

- En Azure AD-prenumeration
- En Samarbetsbaserad Innovation enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Du bör följa de här rekommendationerna när du testar stegen i självstudien:

- Använd inte din produktionsmiljö om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du skaffa en månads utvärderingsperiod [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till samarbetsfunktioner Innovation från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-collaborative-innovation-from-the-gallery"></a>Att lägga till samarbetsfunktioner Innovation från galleriet
För att konfigurera integrering av samarbetsfunktioner Innovation i Azure AD, som du behöver lägga till samarbetsfunktioner Innovation från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till samarbetsfunktioner Innovation från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Appar][2]
    
1. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Appar][3]

1. I sökrutan skriver **samarbetsfunktioner Innovation**.

    ![Skapa en Azure AD-användare för testning](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

1. I resultatpanelen väljer **samarbetsfunktioner Innovation**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med samarbetsfunktioner Innovation baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i samarbetsfunktioner Innovation är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i samarbetsfunktioner Innovation upprättas.

I samarbetsfunktioner Innovation och tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med samarbetsfunktioner Innovation, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en testanvändare samarbetsfunktioner Innovation](#creating-a-collaborative-innovation-test-user)**  – du har en motsvarighet för Britta Simon i samarbetsfunktioner Innovation som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program med samarbetsfunktioner Innovation.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med samarbetsfunktioner Innovation:**

1. I Azure-portalen på den **samarbetsfunktioner Innovation** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

1. På den **gemensam Innovation domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    a. I textrutan **Inloggnings-URL** anger du en URL med följande mönster: `https://<instancename>.foundry.<companyname>.com/`

    b. I textrutan **Identifierare** anger du en URL med följande mönster: `https://<instancename>.foundry.<companyname>.com`
    
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med faktisk inloggnings-URL och identifierare. Kontakta [samarbetsfunktioner Innovation klienten supportteamet](https://www.unilever.com/contact/) att hämta dessa värden.  

1. Samarbetsfunktioner Innovation program som förväntar SAML-intyg i ett visst format. Konfigurera följande anspråk för det här programmet. Du kan hantera värdena för dessa attribut från den ”**användarattribut**” på sidan för integrering av program. Följande skärmbild visar ett exempel på detta.
    
    ![Konfigurera enkel inloggning](./media/collaborativeinnovation-tutorial/attribute.png)
    
1. Klicka på **visa och redigera alla andra användarattribut** kryssrutan i den **användarattribut** avsnitt för att expandera attribut. Utför följande steg på varje visas attribut-

    | Attributnamn | Attributvärde |
    | ---------------| --------------- |    
    | givenName | user.givenname |
    | Efternamn | user.surname |
    | emailaddress | user.userprincipalname |
    | namn | user.userprincipalname |

    a. Klicka på ett attribut för att öppna den **redigera attributet** fönster.

    ![Konfigurera enkel inloggning](./media/collaborativeinnovation-tutorial/url_update.png)

    b. Ta bort URL-värdet från den **Namespace**.
    
    c. Klicka på **Ok** att spara inställningen.

1. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.

    ![Konfigurera enkel inloggning](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

1. Klicka på knappen **Spara**.

    ![Konfigurera enkel inloggning](./media/collaborativeinnovation-tutorial/tutorial_general_400.png)

1. Att konfigurera enkel inloggning på **samarbetsfunktioner Innovation** sida, som du behöver skicka de hämtade **XML-Metadata för** till [samarbetsfunktioner Innovation supportteamet](https://www.unilever.com/contact/). De anger inställningen så att SAML SSO-anslutningen ställs in korrekt på båda sidorna.

> [!TIP]
> Nu kan du läsa en kortare version av instruktionerna i [Azure Portal](https://portal.azure.com), samtidigt som du konfigurerar appen!  När du har lagt till appen från avsnittet **Active Directory > Företagsprogram**, behöver du bara klicka på fliken **Enkel inloggning**. Du kommer då till den inbäddade dokumentationen via avsnittet **Konfiguration** längst ned. Du kan läsa mer om funktionen för inbäddad dokumentation här: [Inbäddad Azure AD-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/collaborativeinnovation-tutorial/create_aaduser_01.png) 

1. Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/collaborativeinnovation-tutorial/create_aaduser_02.png) 

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/collaborativeinnovation-tutorial/create_aaduser_03.png) 

1. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/collaborativeinnovation-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-collaborative-innovation-test-user"></a>Skapa en testanvändare samarbetsfunktioner Innovation

Om du vill aktivera Azure AD-användare att logga in på samarbetsfunktioner Innovation, måste de etableras i samarbetsfunktioner Innovation.  

Vid det här programmet är etablering automatisk som programmet stöder just-in-time användaretablering. Det finns så du behöver inte utföra några steg här.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till samarbetsfunktioner Innovation.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon samarbetsfunktioner Innovation, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **samarbetsfunktioner Innovation**.

    ![Konfigurera enkel inloggning](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

Du bör få inloggningssidan i samarbetsfunktioner Innovation programmet när du klickar på panelen samarbetsfunktioner Innovation i åtkomstpanelen.
Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/collaborativeinnovation-tutorial/tutorial_general_203.png

