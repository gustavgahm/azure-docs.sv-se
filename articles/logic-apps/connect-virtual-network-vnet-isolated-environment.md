---
title: Ansluta till virtuella Azure-nätverk från Azure Logic Apps via en integration service-miljö (ISE)
description: Skapa en integration service-miljö (ISE) så att logic apps och integrationskonton kan komma åt Azure-nätverk (Vnet), utan att förlora privata och isolerade från offentligt eller ”global” Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 12/06/2018
ms.openlocfilehash: 31f3cf9bd8f83c5da32569ed370de1ed35299749
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/07/2019
ms.locfileid: "54062391"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-through-an-integration-service-environment-ise"></a>Ansluta till virtuella Azure-nätverk från Azure Logic Apps via en integration service-miljö (ISE)

> [!NOTE]
> Den här funktionen är i *privat förhandsgranskning*. Att begära åtkomst, [skapa din begäran om att ansluta till här](https://aka.ms/iseprivatepreview).

För scenarier där dina logic apps och integrationskonton behöver åtkomst till en [Azure-nätverk](../virtual-network/virtual-networks-overview.md), skapa en [ *integreringstjänstmiljön* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). En ISE är en privat och isolerad miljö som använder dedikerade lagring och andra resurser åtskilda från den offentliga eller ”global” Logic Apps-tjänsten. Den här separationen minskar också påverkas som andra Azure-klienter kan ha på din apps prestanda. Din ISE *in* till till din Azure-nätverk, som sedan distribuerar Logic Apps-tjänsten till ditt virtuella nätverk. När du skapar ett logic app eller varje konto, Välj den här ISE som deras plats. Ditt logic app eller varje konto kan sedan direkt åtkomst till resurser, till exempel virtuella datorer (VM), servrar, system och tjänster i ditt virtuella nätverk. 

![Välj integreringstjänstmiljö](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Den här artikeln visar hur du utför dessa uppgifter:

* Konfigurera behörigheter på Azure-nätverk så att den privata Logic Apps-instansen kan komma åt det virtuella nätverket.

* Skapa din integration service-environment (ISE). 

* Skapa en logikapp som kan köras i din ISE. 

* Skapa ett integrationskonto för logikappar i din ISE.

Läs mer om integreringstjänstmiljöer [åtkomst till Azure Virtual Network-resurser från Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Förutsättningar

* En Azure-prenumeration. Om du heller inte har någon Azure-prenumeration kan du <a href="https://azure.microsoft.com/free/" target="_blank">registrera ett kostnadsfritt Azure-konto</a>. 

  > [!IMPORTANT]
  > Logic apps, inbyggda åtgärder och kopplingar som körs i din ISE kan du använda en annan prisplanen inte förbrukningsbaserad prisplanen. Mer information finns i [Logic Apps-priser](../logic-apps/logic-apps-pricing.md).

* En [Azure-nätverk](../virtual-network/virtual-networks-overview.md). Om du inte har ett virtuellt nätverk kan du lära dig hur du [skapa en Azure-nätverk](../virtual-network/quick-create-portal.md). 

* Ge dina logikappar direkt åtkomst till Azure-nätverk, [konfigurera rollbaserad åtkomstkontroll (RBAC) behörigheter](#vnet-access) så att Logic Apps-tjänsten har behörigheter för åtkomst till ditt virtuella nätverk. 

* Grundläggande kunskaper om [hur du skapar logikappar](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Ange behörigheter för virtuella nätverk

När du skapar en integration service-miljö (ISE) kan du välja ett Azure-nätverk var du *mata in* din miljö. Men innan du kan välja ett virtuellt nätverk för din miljö måste ställa du in behörigheter för rollbaserad åtkomstkontroll (RBAC) i det virtuella nätverket. Om du vill konfigurera behörigheter att tilldela dessa specifika roller till Azure Logic Apps-tjänsten:

1. I den [Azure-portalen](https://portal.azure.com), hitta och välj ditt virtuella nätverk. 

1. I det virtuella nätverket menyn väljer **åtkomstkontroll (IAM)**. 

1. Under **åtkomstkontroll (IAM)**, Välj **Lägg till rolltilldelning**. 

   ![Lägg till roller](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. På den **Lägg till rolltilldelning** fönstret lägga till rollen krävs i Azure Logic Apps-tjänsten enligt beskrivningen. 

   1. Under **rollen**väljer **Nätverksdeltagare**. 
   
   1. Under **tilldela åtkomst till**väljer **Azure AD-användare, grupp eller tjänstens huvudnamn**.

   1. Under **Välj**, ange **Azure Logic Apps**. 

   1. När listan visas, väljer **Azure Logic Apps**. 

      > [!TIP]
      > Om du inte hittar den här tjänsten kan du ange Logic Apps-tjänsten app-ID: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` 
   
   1. När du är klar väljer du **Spara**.

   Exempel:

   ![Lägg till rolltilldelning](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

Mer information finns i [behörigheter för åtkomst till virtuellt nätverk](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Skapa din ISE

Följ dessa steg för att skapa din integration service-environment (ISE):

1. I den [Azure-portalen](https://portal.azure.com), på Azure-huvudmenyn väljer **skapa en resurs**.

   ![Skapa ny resurs](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. I sökrutan anger du ”integreringstjänstmiljön” som filter.
Listan med resultat väljer **Integreringstjänstmiljön (förhandsversion)**, och välj sedan **skapa**.

   ![Välj ”Integreringstjänstmiljön”](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Välj ”Skapa”](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Ange följande information för din miljö och välj sedan **granska + skapa**, till exempel:

   ![Ange information om miljön](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Egenskap  | Krävs | Värde | Beskrivning |
   |----------|----------|-------|-------------|
   | **Prenumeration** | Ja | <*Azure-prenumerationsnamn*> | Azure-prenumeration för din miljö | 
   | **Resursgrupp** | Ja | <*Azure-resource-group-name*> | Azure-resursgrupp där du vill skapa en miljö |
   | **Namn på integreringstjänstmiljö** | Ja | <*miljö-name*> | Namn för att ge din miljö | 
   | **Plats** | Ja | <*Azure-datacenterregion*> | Azure-datacenterregion var du vill distribuera din miljö | 
   | **Ytterligare kapacitet** | Ja | 0, 1, 2, 3 | Antalet enheter för den här ISE-resursen | 
   | **Virtuellt nätverk** | Ja | <*Azure-virtual-network-name*> | Azure-nätverket där du vill att mata in din miljö så att logic apps i denna miljö kan komma åt det virtuella nätverket. Om du inte har ett nätverk kan du skapa en här. <p>**Viktiga**: Du kan *endast* utföra den här inmatning när du skapar din ISE. Men innan du kan skapa den här relationen, se till att du redan [konfigurera rollbaserad åtkomstkontroll i ditt virtuella nätverk för Azure Logic Apps](#vnet-access). | 
   | **Undernät** | Ja | <*undernät resurslistan*> | En ISE kräver fyra *tom* undernät för att skapa resurser i din miljö. Se till dessa undernät *inte delegerad* till alla tjänster. Du *kan inte ändra* undernätsadresserna när du har skapat din miljö. <p><p>Att skapa varje undernät, [att följa stegen i den här tabellen](#create-subnet). Varje undernät måste uppfylla följande kriterier: <p>-Måste vara tom. <br>-Använder ett namn som inte börjar med ett tal eller ett bindestreck. <br>-Använder den [Classless Inter-Domain Routing CIDR-format](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) och en klass B-adressutrymmet. <br>-Innehåller minst en `/27` i adressutrymmet så undernätet hämtar minst 32 adresser. Läs om hur du beräknar antalet adresser i [IPv4 CIDR-block som](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks). Exempel: <p>- `10.0.0.0/24` har 256 adresser eftersom 2<sup>(32-24)</sup> är 2<sup>8</sup> eller 256. <br>- `10.0.0.0/27` har 32 adresser eftersom 2<sup>(32-27)</sup> är 2<sup>5</sup> eller 32. <br>- `10.0.0.0/28` har bara 16 adresser eftersom 2<sup>(32-28)</sup> är 2<sup>4</sup> eller 16. |
   |||||

   <a name="create-subnet"></a>

   **Skapa undernät**

   1. Under den **undernät** väljer **hantera undernätskonfiguration**.

      ![Hantera konfiguration av undernät](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. På den **undernät** fönstret Välj **undernät**.

      ![Lägga till undernät](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. På den **Lägg till undernät** fönstret anger den här informationen.

      * **Namn**: Namn för ditt undernät
      * **Adressintervall (CIDR-block)**: Intervall för ditt undernät i det virtuella nätverket och i CIDR-format

      ![Lägg till information om undernät](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. När du är klar väljer du **OK**.

   1. Upprepa dessa steg för tre flera undernät.

1. När Azure verifierar har ISE-information, väljer **skapa**, till exempel:

   ![Välj ”Skapa” efter lyckad validering](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure börjar distribuera din miljö, men den här processen *kan* ta upp till två timmar innan du avslutar. 
   Välj meddelandeikonen, vilket öppnar meddelandefönstret för att kontrollera Distributionsstatus i din Azure verktygsfältet.

   ![Kontrollera Distributionsstatus för](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Om distributionen har slutförts visas det här meddelandet i Azure:

   ![Distribueringen lyckades](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   > [!NOTE]
   > Om distributionen misslyckas eller om du ta bort din ISE Azure *kan* ta upp till en timme innan du publicerar dina undernät. Därför kanske du måste vänta innan du återanvänder dessa undernät i ett annat ISE.

1. Om du vill visa din miljö, Välj **gå till resurs** om Azure inte automatiskt går till din miljö när distributionen är klar.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Skapa logikapp – ISE

För att skapa logikappar som använder din integration service-environment (ISE), följer du stegen i [så här skapar du en logikapp](../logic-apps/quickstart-create-first-logic-app-workflow.md) men med följande skillnader: 

* När du skapar din logikapp under den **plats** egenskapen, Välj din ISE från den **integreringstjänstmiljöer** avsnittet, till exempel:

  ![Välj integreringstjänstmiljö](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Du kan använda samma inbyggda utlösare och åtgärder, till exempel HTTP, som körs i samma ISE som din logikapp. Kopplingar med den **ISE** märka också köras i samma ISE som din logikapp. Anslutningar utan den **ISE** etikett som körs i tjänsten för global Logic Apps.

  ![Välj ISE-tjänster](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* När du mata in din ISE i Azure-nätverk, logikappar i din ISE direkt åtkomst till resurser i det virtuella nätverket. För lokala system som är anslutna till ett virtuellt nätverk, att mata in en ISE i nätverket så att dina logikappar har direkt åtkomst dessa system med någon av dessa objekt: 

  * ISE-anslutning för systemet, till exempel SQL Server
  
  * HTTP-åtgärd 
  
  * Anpassad anslutningsapp

  För lokala system som inte är i ett virtuellt nätverk eller inte har ISE kopplingar först [konfigurera den lokala datagatewayen](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Skapa integrationskonto – ISE

Om du vill använda en integrering med logic apps i en integreringstjänstmiljö (ISE) måste det integrationskontot måste använda den *samma miljö* som logic apps. Logic apps i en ISE kan referera till integrationskonton i samma ISE. 

Om du vill skapa ett integrationskonto som använder en ISE, följer du stegen i [hur du skapar konton för logikappsintegration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) undantag för den **plats** egenskapen där den **integreringstjänstmiljöer**  avsnittet visas nu. Välj i stället dina ISE i stället för en region, till exempel:

![Välj integreringstjänstmiljö](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Få support

* Om du har frågor kan du besöka <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">forumet för Azure Logic Apps</a>.
* Om du vill skicka in eller rösta på förslag på funktioner besöker du <a href="https://aka.ms/logicapps-wish" target="_blank">webbplatsen för Logic Apps-användarfeedback</a>.

## <a name="next-steps"></a>Nästa steg

* Läs mer om [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Lär dig mer om [virtual network-integration för Azure-tjänster](../virtual-network/virtual-network-for-azure-services.md)
