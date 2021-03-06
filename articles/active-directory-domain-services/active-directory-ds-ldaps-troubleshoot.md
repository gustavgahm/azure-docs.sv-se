---
title: Felsöka säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs
description: Felsöka säkert LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: ergreenl
ms.openlocfilehash: 97a2ee23435d0b29565bc3bf1dcb426e9e83fd31
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55185229"
---
# <a name="troubleshoot-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Felsöka säkert LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän

## <a name="connection-issues"></a>Anslutningsproblem
Om du har problem med att ansluta till den hanterade domänen med säkert LDAP:

* Utfärdaren kedja av certifikatet för säkert LDAP måste vara betrott på klienten. Du kan välja att lägga till rotcertifikatutfärdaren i arkivet Betrodda rotcertifikat på klienten att upprätta förtroendet.
* Kontrollera att LDAP-klient (till exempel ldp.exe) ansluter till säker LDAP-slutpunkten med hjälp av ett DNS-namn, inte IP-adress.
* Kontrollera DNS-namnet LDAP-klienten ansluter till. Det måste matcha den offentliga IP-adressen för säkert LDAP på den hanterade domänen.
* Kontrollera att certifikatet för säkert LDAP för din hanterade domän har DNS-namnet i ett ämne eller Alternativt ämnesnamn-attributet.
* NSG-inställningarna för det virtuella nätverket måste tillåta trafik till port 636 från internet. Det här steget gäller endast om du har aktiverat åtkomst med säkert LDAP via internet.


## <a name="need-help"></a>Behöver du hjälp?
Om du fortfarande har problem med att ansluta till den hanterade domänen med säkert LDAP [kontakta produktteamet](active-directory-ds-contact-us.md) om du behöver hjälp. Inkludera följande information för att diagnosticera problemet bättre:
* En skärmbild av ldp.exe anslutningen och misslyckas.
* En Azure AD-klient-ID och DNS-domännamnet för den hanterade domänen.
* Exakta användarnamnet som du försöker binda som.


## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services – komma igång-guiden](active-directory-ds-getting-started.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
* [Grunderna i LDAP-fråga](https://technet.microsoft.com/library/aa996205.aspx)
* [Administrera Grupprincip i en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-group-policy.md)
* [Nätverkssäkerhetsgrupper](../virtual-network/security-overview.md)
* [Skapa en Nätverkssäkerhetsgrupp](../virtual-network/tutorial-filter-network-traffic.md)
