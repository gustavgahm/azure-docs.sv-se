---
title: Starta om Azure Database för MySQL med Azure-portalen
description: Den här artikeln beskrivs hur du kan starta om en Azure Database for MySQL-server med Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 11/16/2018
ms.openlocfilehash: 83e862aea5b1f2de5a3f80970c2331fc9d81704e
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540286"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Starta om Azure Database för MySQL med Azure-portalen
Det här avsnittet beskrivs hur du kan starta om en Azure Database for MySQL-server. Du kan behöva starta om servern för underhåll orsaker, vilket medför ett kort avbrott som servern utför åtgärden.

Starta om servern kommer att blockeras om tjänsten är upptagen. Tjänsten kan till exempel behandlar tidigare åtgärden, till exempel skala virtuella kärnor.

Den tid som krävs för att slutföra en omstart är beroende av MySQL-återställningsprocessen. Om du vill minska tiden för omstart, rekommenderar vi du minska mängden aktiviteten på server före omstarten.

## <a name="prerequisites"></a>Förutsättningar
Du behöver följande för att slutföra den här guiden:
- En [Azure Database for MySQL-server och databas](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Utföra omstart av servern

Starta följande steg om MySQL-server:

1. Välj din Azure Database for MySQL-server i Azure-portalen.

2. I verktygsfältet på serverns **översikt** klickar du på **starta om**.

   ![Azure Database for MySQL - översikt – starta](./media/howto-restart-server-portal/2-server.png)

3. Klicka på **Ja** att bekräfta att starta om servern. 

   ![Azure Database för MySQL - omstart bekräfta ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Observera att Serverstatusen ändras till ”startar om”.

   ![Azure Database för MySQL - status för omstart ](./media/howto-restart-server-portal/4-restarting-status.png)

5. Bekräfta omstart av servern är klar.

   ![Azure Database för MySQL - omstart lyckades ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Nästa steg

[Snabbstart: Skapa Azure Database for MySQL-server med Azure-portalen](./quickstart-create-mysql-server-database-using-azure-portal.md)