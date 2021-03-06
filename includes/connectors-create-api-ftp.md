---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 65f1e6d2489775a17ba2dacef0623706364fffab
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091774"
---
### <a name="prerequisites"></a>Förutsättningar
* En [FTP](https://wikipedia.org/wiki/File_Transfer_Protocol) konto  

Innan du kan använda din FTP-konto i en logikapp, måste du godkänna logikappen att ansluta till FTP-konto. Som tur är kan göra du det enkelt från i logikappen på Azure Portal.  

Här följer stegen för att auktorisera din logikapp för att ansluta till FTP-konto:  

1. Om du vill skapa en anslutning till FTP, i logic appdesigner väljer **visa Microsoft hanterade API: er** i nedrullningsbara listan anger *FTP* i sökrutan. Välj utlösaren eller åtgärden som du kommer att tycka att använda:  
   ![FTP-anslutning, skapa steg](./media/connectors-create-api-ftp/ftp-1.png)  
2. Om du inte skapat några anslutningar till FTP innan du kan hämta uppmanas du att ange din FTP-autentiseringsuppgifter. Dessa autentiseringsuppgifter används för att auktorisera din logikapp för att ansluta till och få åtkomst till data för din FTP-konto:  
   ![FTP-anslutning, skapa steg](./media/connectors-create-api-ftp/ftp-2.png)  
3. Lägg märke till anslutningen har skapats och du kan nu fortsätta med andra steg i logikappen:  
   ![FTP-anslutning, skapa steg](./media/connectors-create-api-ftp/ftp-3.png)  

