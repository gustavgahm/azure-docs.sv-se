---
title: Begrepp för hög tillgänglighet i Azure Database for MySQL
description: Det här avsnittet innehåller information om hög tillgänglighet med Azure Database for MySQL
author: jasonwhowell
ms.author: jasonh
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 70577f32debc526aaccbd79b62dd35e82119e3f9
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548400"
---
# <a name="high-availability-concepts-in-azure-database-for-mysql"></a>Begrepp för hög tillgänglighet i Azure Database for MySQL
Azure Database for MySQL-tjänsten ger garanterat hög tillgänglighet. Ekonomisk servicenivåavtalet (SLA) är 99,99% vid allmän tillgänglighet. Det är praktiskt taget ingen program driftstopp när du använder den här tjänsten.

## <a name="high-availability"></a>Hög tillgänglighet
Modell med hög tillgänglighet (HA) baseras på inbyggda mekanismer för redundans när en nod på servernivå avbrott inträffar. Ett avbrott för noden på servernivå kan inträffa på grund av ett maskinvarufel eller som svar på en tjänstdistribution.

Vid alla tidpunkter uppstå ändringar som gjorts i en Azure Database for MySQL-databasserver i samband med en transaktion. Ändringar sparas synkront i Azure storage när transaktionen genomförs. Om en nod på servernivå avbrott inträffar, databasservern automatiskt skapar en ny nod och lagring av data till den nya noden. Alla aktiva anslutningar tas bort och alla aktiva transaktioner genomförs inte.

## <a name="application-retry-logic-is-essential"></a>Logik för omprövning av program som är viktigt
Det är viktigt att MySQL databasprogram har skapats för att identifiera och försök avbrutna anslutningar och misslyckats transaktioner. När programmet försöker omdirigeras transparent programmets anslutning till den nya instansen, som tar för den felaktiga instansen.

Internt i Azure används en gateway för att omdirigera anslutningar till den nya instansen. Redundansväxla hela processen tar normalt tio sekunder eller vid ett avbrott. Eftersom omdirigeringen hanteras internt av gatewayen, detsamma externa anslutningssträngen för klientprogram.

## <a name="scaling-up-or-down"></a>Skala upp eller ned
Liknar modellen hög tillgänglighet när en Azure Database for MySQL skalas upp eller ned, en ny server-instansen med den angivna storleken skapas. Befintliga datalagring är frånkopplat från den ursprungliga instansen och kopplat till den nya instansen.

Under åtgärden sker ett avbrott i databasanslutningar. Klientprogrammen är frånkopplade och öppna ogenomförda transaktioner har avbrutits. När klientprogrammet försöker ansluta igen, eller gör en ny anslutning, dirigeras gatewayen anslutningen till den nya storlekar instansen. 

## <a name="next-steps"></a>Nästa steg
- En översikt över tjänsten finns i [Azure Database for MySQL-översikt](overview.md)
- En översikt över logik för omprövning, se [hantering av tillfälliga anslutningsfel för Azure Database for MySQL](concepts-connectivity.md)
