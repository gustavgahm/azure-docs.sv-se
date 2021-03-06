---
title: Så här visar du tjänstens huvudnamn för en hanterad identitet i Azure portal
description: Stegvisa instruktioner för att visa tjänstens huvudnamn för en hanterad identitet i Azure-portalen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: priyamo
ms.openlocfilehash: 7d4d37ebb224a8420dd0201be1bc7a8e37d244bf
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195264"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>Visa tjänstens huvudnamn för en hanterad identitet i Azure portal

Hanterade identiteter för Azure-resurser tillhandahåller Azure-tjänster med en automatiskt hanterad identitet i Azure Active Directory. Du kan använda den här identiteten för att autentisera till en tjänst som stöder Azure AD-autentisering utan autentiseringsuppgifter i din kod. 

I den här artikeln får lära du att visa tjänstens huvudnamn för en hanterad identitet med hjälp av Azure portal.

## <a name="prerequisites"></a>Förutsättningar

- Om du är bekant med hanterade identiteter för Azure-resurser kan du kolla den [översiktsavsnittet](overview.md).
- Om du inte redan har ett Azure-konto, [registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/).
- Aktivera [systemtilldelade identiteter på en virtuell dator](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) eller [program](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).

## <a name="view-the-service-principal"></a>Visa tjänstens huvudnamn

Den här proceduren visar hur du kan visa tjänstens huvudnamn för en virtuell dator med systemtilldelade identiteter aktiverat (samma steg som gäller för ett program).

1. Klicka på **Azure Active Directory** och klicka sedan på **företagsprogram**.
2. Under **programtyp**, Välj **alla program**.
3. I rutan Sök filter skriver du namnet på den virtuella datorn eller program som har hanterad identitet aktiverad eller välj i listan som visas.

   ![Visa hanterad identitet tjänstens huvudnamn i portalen](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Nästa steg

[Hanterade identiteter för Azure-resurser](/azure/active-directory/managed-identities-azure-resources/overview)

