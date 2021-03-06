---
title: 'Azure AD Connect-synkronisering:  Ändra AD DS-kontolösenordet | Microsoft Docs'
description: Det här avsnittet dokumentet beskriver hur du uppdaterar Azure AD Connect när lösenordet för AD DS-kontot har ändrats.
services: active-directory
keywords: AD DS-konto, Active Directory-konto, lösenord
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: f4e23f05d2b9ee8396826e0321a00d95cc13bd99
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55486120"
---
# <a name="changing-the-ad-ds-account-password"></a>Ändra AD DS-kontolösenordet
AD DS-kontot refererar till det användarkonto som används av Azure AD Connect för att kommunicera med lokala Active Directory. Om du ändrar lösenordet för AD DS-kontot, måste du uppdatera Azure AD Connect-synkroniseringstjänsten med det nya lösenordet. I annat fall synkronisering kan inte längre synkronisera korrekt med en lokal Active Directory och du kommer märka följande fel:

* I hanteraren för synkroniseringstjänsten, alla import eller export igen med en lokal AD misslyckas med **inga start autentiseringsuppgifter** fel.

* Under Windows Loggboken, programmets händelselogg innehåller ett fel med **händelse-ID 6000** och meddelandet **”hanteringsagenten” contoso.com ”misslyckades att köra eftersom autentiseringsuppgifterna var ogiltiga”**.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>Hur du uppdaterar synkroniseringstjänsten med nytt lösenord för AD DS-konto
Så här uppdaterar synkroniseringstjänsten med det nya lösenordet:

1. Starta hanteraren för synkroniseringstjänsten (START → Synchronization Service).
</br>![Sync Service Manager](./media/how-to-connect-sync-change-addsacct-pass/startmenu.png)  

2. Gå till den **kopplingar** fliken.

3. Välj den **AD Connector** som motsvarar AD DS-kontot som dess lösenord har ändrats.

4. Under **åtgärder**väljer **egenskaper**.

5. I popup-dialogrutan Välj **Anslut till Active Directory-skog**:

6. Ange det nya lösenordet för AD DS-konto i den **lösenord** textrutan.

7. Klicka på **OK** att spara det nya lösenordet och Stäng popup.

8. Starta om Azure AD Connect under Windows Service Control Manager-synkroniseringstjänsten. Detta är att säkerställa att alla referenser till det gamla lösenordet tas bort från cacheminnet.

## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](how-to-connect-sync-whatis.md)

* [Integrera dina lokala identiteter med Azure Active Directory](whatis-hybrid-identity.md)
