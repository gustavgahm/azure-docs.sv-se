---
title: Microsoft Graph-API:er för Azure AD Privileged Identity Management (PIM) (förhandsversion) | Microsoft Docs
description: Ger information om hur du använder Microsoft Graph-API:er för Azure Active Directory Privileged Identity Management (PIM) (förhandsversion).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 11/13/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 77c7eb2d57fcbe6676be82ee68effeae4b1ae615
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55189314"
---
# <a name="microsoft-graph-apis-for-pim-preview"></a>Microsoft Graph-API:er för PIM (förhandsversion)

De flesta av de uppgifter som du kan utföra i Azure AD Privileged Identity Management (PIM) med Azure-portalen kan du även utföra med hjälp av [Microsoft Graph-API:erna](https://developer.microsoft.com/graph/docs/concepts/overview). Den här artikeln beskriver några viktiga begrepp vid användning av Microsoft Graph-API:er för PIM. Mer information om Microsoft Graph-API:er finns i [referensen för Azure AD Privileged Identity Management API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root).

> [!IMPORTANT]
> API:er under /betaversionen i Microsoft Graph är i förhandsversion och kan komma att ändras. Användning av dessa API:er i produktionsprogram stöds inte.

## <a name="required-permissions"></a>Nödvändiga behörigheter

För att anropa Microsoft Graph API:er för PIM måste du ha **en eller flera** av följande behörigheter:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>Ställa in behörigheter

För att program ska kunna anropa Microsoft Graph-API:er för PIM måste de ha nödvändiga behörigheter. Det enklaste sättet att ange nödvändiga behörigheter är att använda [Azure AD-ramverket för medgivande](../develop/consent-framework.md).

### <a name="set-permissions-in-graph-explorer"></a>Ställa in behörigheter i Graph Explorer

Om du använder Graph Explorer för att testa anrop kan du ange behörigheterna i verktyget.

1. Logga in på [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) som global administratör.

1. Klicka på **ändra behörigheter**.

    ![Graph Explorer – ändra behörigheter](./media/pim-apis/graph-explorer.png)

1. Lägg till bockar intill de behörigheter som du vill inkludera. `PrivilegedAccess.ReadWrite.AzureAD` är ännu inte tillgänglig i Graph Explorer.

    ![Graph Explorer – ändra behörigheter](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Klicka på **Ändra behörigheter** för att tillämpa behörighetsändringarna.

## <a name="next-steps"></a>Nästa steg

- [Referens för Azure AD Privileged Identity Management API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
