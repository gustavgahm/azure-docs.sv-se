---
title: 'Självstudier: Azure Active Directory-integrering med Zoom | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zoom.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: ace02a0cb93cf3e56e4b895524b9e2d35440aecb
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54812993"
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Självstudier: Azure Active Directory-katalogintegrering med Zoom

I den här självstudien får du lära dig hur du integrerar Zoom med Azure Active Directory (AD Azure).
Genom att integrera Zoom med Azure AD får du följande fördelar:

* Du kan i Azure AD styra vem som har åtkomst till Zoom.
* Du kan göra så att dina användare automatiskt loggas in på Zoom (enkel inloggning) med sina Azure AD-konton.
* Du kan hantera dina konton på en central plats – Azure-portalen.

Om du vill ha mer information om SaaS-appintegrering med Azure AD läser du avsnittet om [programåtkomst och enkel inloggning med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Om du inte har en Azure-prenumeration kan du [skapa ett kostnadsfritt konto ](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Nödvändiga komponenter

För att kunna konfigurera Azure AD-integrering med Zoom behöver du följande:

* En Azure AD-prenumeration. Om du inte har någon Azure AD-miljö kan du hämta en månads utvärderingsversion [här](https://azure.microsoft.com/pricing/free-trial/)
* Zoom-prenumeration med enkel inloggning aktiverat

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien konfigurerar och testar du enkel inloggning med Azure AD i en testmiljö.

* Zoom har stöd för **SP**-initierad enkel inloggning

## <a name="adding-zoom-from-the-gallery"></a>Lägga till Zoom från galleriet

För att kunna konfigurera integreringen av Zoom i Azure AD måste du lägga till Zoom från galleriet i din lista över hanterade SaaS-appar.

**Gör följande för att lägga till Zoom från galleriet:**

1. I **[Azure-portalen](https://portal.azure.com)**, i den vänstra navigeringspanelen, klickar du på **Azure Active Directory**-ikonen.

    ![Azure Active Directory-knappen](common/select-azuread.png)

2. Gå till **Företagsprogram** och välj alternativet **Alla program**.

    ![Bladet Företagsprogram](common/enterprise-applications.png)

3. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Knappen Nytt program](common/add-new-app.png)

4. Skriv **Zoom** i sökrutan, välj **Zoom** på resultatpanelen och klicka på knappen **Lägg till** i programmet.

     ![Zoom i listan med resultat](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa enkel inloggning med Azure AD

I det här avsnittet konfigurerar och testar du enkel inloggning med Azure AD med Zoom baserat på en testanvändare med namnet **Britta Simon**.
För att enkel inloggning ska fungera måste en länkrelation mellan en Azure AD-användare och den relaterade användaren i Zoom upprättas.

För att kunna konfigurera och testa enkel inloggning i Azure AD med Zoom måste du slutföra följande byggblock:

1. **[Konfigurera enkel inloggning med Azure AD](#configure-azure-ad-single-sign-on)** – så att användarna kan använda den här funktionen.
2. **[Konfigurera enkel inloggning för Zoom](#configure-zoom-single-sign-on)** – för att konfigurera inställningarna för enkel inloggning på programsidan.
3. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)** – för att testa enkel inloggning med Azure AD med Britta Simon.
4. **[Tilldela Azure AD-testanvändaren](#assign-the-azure-ad-test-user)** – så att Britta Simon kan använda enkel inloggning med Azure AD.
5. **[Skapa Zoom-testanvändare](#create-zoom-test-user)** – så att det finns en motsvarighet till Britta Simon i Zoom som är länkad till Azure AD-representationen av användaren.
6. **[Testa enkel inloggning](#test-single-sign-on)** – för att verifiera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera enkel inloggning med Azure AD

I det här avsnittet aktiverar du enkel inloggning med Azure AD i Azure-portalen.

Konfigurera enkel inloggning med Azure AD med Zoom genom att utföra följande steg:

1. I [Azure Portal](https://portal.azure.com/) går du till sidan för programintegrering för **Zoom** och väljer **Enkel inloggning**.

    ![Konfigurera länk för enkel inloggning](common/select-sso.png)

2. I dialogrutan **Välj en metod för enkel inloggning** väljer du läget **SAML/WS-Fed** för att aktivera enkel inloggning.

    ![Välja läge för enkel inloggning](common/select-saml-option.png)

3. På sidan **Konfigurera enkel inloggning med SAML** klickar du på **redigeringsikonen** för att öppna dialogrutan **Grundläggande SAML-konfiguration**.

    ![Redigera grundläggande SAML-konfiguration](common/edit-urls.png)

4. I avsnittet **Grundläggande SAML-konfiguration** utför du följande steg:

    ![Zoom-domän och information om URL:er för enkel inloggning](common/sp-identifier.png)

    a. I textrutan **Inloggnings-URL** anger du en URL enligt följande mönster: `https://<companyname>.zoom.us`

    b. I textrutan **Identifierare (entitets-ID)** anger du en URL enligt följande mönster: `<companyname>.zoom.us`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera dessa värden med faktisk inloggnings-URL och identifierare. Kontakta [Zooms supportteam](https://support.zoom.us/hc/en-us) för att få dessa värden. Du kan även se mönstren som visas i avsnittet **Grundläggande SAML-konfiguration** i Azure-portalen.

5. Zoom-programmet förväntar sig SAML-intyg i ett visst format. Konfigurera följande anspråk för det här programmet. Du kan hantera värdena för dessa attribut i avsnittet **Användarattribut** på sidan för programintegrering. På sidan **Konfigurera enkel inloggning med SAML** klickar du på knappen **Redigera** för att öppna dialogrutan **Användarattribut**.

    ![image](common/edit-attribute.png)

6. I avsnittet **Användaranspråk** i dialogrutan **Användarattribut** konfigurerar du SAML-tokenattributet på det sätt som visas i bilden ovan och utför följande steg:
    
    | Namn | Namnområde  |  Källattribut|
    | ---------------| --------------- | --------- |
    | E-postadress  | user.mail  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail |
    | Förnamn  | user.givenname  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname |
    | Efternamn  | user.surname  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname |
    | Telefonnummer  | user.telephonenumber  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone |
    | Avdelning  | user.department  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department |

    a. Klicka på **Lägg till nytt anspråk** för att öppna dialogrutan **Hantera användaranspråk**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. I textrutan **Namn** skriver du det attributnamn som visas för den raden.

    c. Välj Källa som **Attribut**.

    d. Från listan över **Källattribut** skriver du det attributvärde som visas för den raden.

    e. Klicka på **Ok**

    f. Klicka på **Spara**.

4. På sidan **Konfigurera enkel inloggning med SAML** går du till avsnittet **SAML-signeringscertifikat**, klickar du på **Ladda ned** för att ladda ned **Certifikat (Base64)** från de angivna alternativen enligt dina behov och sparar det på datorn.

    ![Länk för nedladdning av certifikatet](common/certificatebase64.png)

6. I avsnittet **Konfigurera Zoom** kopierar du lämpliga URL:er enligt dina behov.

    ![Kopiera konfigurations-URL:er](common/copy-configuration-urls.png)

    a. Inloggnings-URL

    b. Azure AD-identifierare

    c. Utloggnings-URL

### <a name="configure-zoom-single-sign-on"></a>Konfigurera Zoom med enkel inloggning

1. Logga in på din Zoom-företagswebbplats som administratör i ett annat webbläsarfönster.

2. Klicka på fliken **Enkel inloggning**.
   
    ![Fliken Enkel inloggning](./media/zoom-tutorial/IC784700.png "Enkel inloggning")

3. Klicka på fliken **Säkerhetskontroll** och gå sedan till inställningarna för **Enkel inloggning**.

4. Gör följande i avsnittet Enkel inloggning:
   
    ![Avsnittet Enkel inloggning](./media/zoom-tutorial/IC784701.png "Enkel inloggning")
   
    a. I textrutan **Sign-in page URL** (Inloggnings-URL) klistrar du in värdet för den **inloggnings-URL** som du har kopierat från Azure-portalen.
   
    b. I textrutan **Signout page URL** (Utloggnings-URL) klistrar du in värdet för den **utloggnings-URL** som du har kopierat från Azure-portalen.
     
    c. Öppna ditt Base64-kodade certifikat i Anteckningar, kopiera innehållet till Urklipp och klistra in det i textrutan **Certifikat för identitetsprovider**.

    d. I textrutan **Issuer**  (Utfärdare) klistrar du in det värde för **Azure AD-identifierare** som du har kopierat från Azure-portalen. 

    e. Klicka på **Spara**.

    > [!NOTE] 
    > För ytterligare information, besök informationen för Zoom [https://zoomus.zendesk.com/hc/articles/115005887566](https://zoomus.zendesk.com/hc/articles/115005887566)

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare 

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

1. Gå till den vänstra rutan i Azure-portalen och välj **Azure Active Directory**, välj **Users** och sedan **Alla användare**.

    ![Länkarna ”Användare och grupper” och ”Alla grupper”](common/users.png)

2. Välj **Ny användare** överst på skärmen.

    ![Knappen Ny användare](common/new-user.png)

3. Genomför följande steg i Användaregenskaper.

    ![Dialogrutan Användare](common/user-properties.png)

    a. I fältet **Namn** anger du **BrittaSimon**.
  
    b. I fältet **Användarnamn** anger du **brittasimon@yourcompanydomain.extension**  
    Till exempel, BrittaSimon@contoso.com

    c. Markera kryssrutan **Visa lösenord** och skriv sedan ned det värde som visas i rutan Lösenord.

    d. Klicka på **Skapa**.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändaren

I det här avsnittet gör du det möjligt för Britta Simon att använda enkel inloggning med Azure genom att bevilja åtkomst till Zoom.

1. I Azure Portal väljer du **Företagsprogram**, **Alla program** och sedan **Zoom**.

    ![Bladet Företagsprogram](common/enterprise-applications.png)

2. I listan över program skriver och väljer du **Zoom**.

    ![Zoom-länken i listan med program](common/all-applications.png)

3. På menyn till vänster väljer du **Användare och grupper**.

    ![Länken ”Användare och grupper”](common/users-groups-blade.png)

4. Klicka på knappen **Lägg till användare** och välj sedan **Användare och grupper** i dialogrutan **Lägg till tilldelning**.

    ![Fönstret Lägg till tilldelning](common/add-assign-user.png)

5. I dialogrutan **Användare och grupper** väljer du **Britta Simon** i listan med användare och klickar på knappen **Välj** längst ned på skärmen.

6. Om du förväntar dig ett rollvärde i SAML-försäkran väljer du i dialogrutan **Välj roll** lämplig roll för användaren i listan och klickar sedan på knappen **Välj** längst ned på skärmen.

7. I dialogrutan **Lägg till tilldelning** klickar du på knappen **Tilldela**.

### <a name="create-zoom-test-user"></a>Skapa Zoom-testanvändare

För att Azure AD-användare ska kunna logga in på Zoom måste de etableras till Zoom. När det gäller Zoom är etablering en manuell aktivitet.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Gör följande för att etablera ett användarkonto:

1. Logga in på din **Zoom**-företagswebbplats som administratör.
 
2. Klicka på fliken **Kontohantering** och klicka sedan på **Användarhantering**.

3. Klicka på **Lägga till användare** under Användarhantering.
   
    ![Användarhantering](./media/zoom-tutorial/IC784703.png "Användarhantering")

4. Gör följande i dialogrutan **Lägg till användare**:
   
    ![Lägga till användare](./media/zoom-tutorial/IC784704.png "Lägga till användare")
   
    a. Som **Användartyp** väljer du **Grundläggande**.

    b. I textrutan **E-post** skriver du in e-postadressen till ett giltigt Azure AD-konto som du vill etablera.

    c. Klicka på **Lägg till**.

> [!NOTE]
> Du kan använda alla verktyg för att skapa ett Zoom-användarkonto eller API: er som tillhandahålls av Zoom för att etablera Azure Active Directory-användarkonton.

### <a name="test-single-sign-on"></a>Testa enkel inloggning 

I det här avsnittet testar du konfigurationen för enkel inloggning Azure AD med hjälp av åtkomstpanelen.

När du klickar på Zoom-panelen i åtkomstpanelen bör du automatiskt loggas in på Zoom som du har konfigurerat enkel inloggning för. Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ytterligare resurser

- [ Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Vad är programåtkomst och enkel inloggning med Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Vad är villkorsstyrd åtkomst i Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

