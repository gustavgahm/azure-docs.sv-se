---
title: Problem vid inloggning i ett egenutvecklat program | Microsoft Docs
description: Vanliga rrors som kan vara orsaken du inte kunna logga in på ett program som du har utvecklat med Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: b776fab035a169132a18866c100370da12b3febb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197542"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Problem vid inloggning i ett egenutvecklat program

Det finns flera fel som kan vara orsaken du inte kunna logga in på en app. Den största orsaken personer inträffar det här problemet är felkonfigurerad appar.

## <a name="errors-related-to--misconfigured-apps"></a>Fel som rör felkonfigurerad appar

* Kontrollera båda konfigurationerna i portalen matchar vad du har i din app. Mer specifikt jämför klienter/program-ID, svars-URL, klient hemligheter/nycklar och App-ID-URI.

* Jämför den resurs som du ska få åtkomst till i kod med behörigheterna som är konfigurerade i den **krävs resurser** flik för att se till att du bara begära resurser som du har konfigurerat.

* Se [Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory) för alla liknande fel eller problem.

## <a name="next-steps"></a>Nästa steg

[Utvecklarguide för Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Medgivande och integrera appar i Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Medgivande och ge behörighet till för Azure AD v2.0 konvergerat appar](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
