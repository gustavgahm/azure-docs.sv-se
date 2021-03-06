---
title: Arbeta med Azure Cosmos DB-konton
description: Den här artikeln beskrivs hur skapar och använder Azure Cosmos DB-konton
author: dharmas-cosmos
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: cbc11e2fc54ecffbea22a66354f334f3562e3339
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459221"
---
# <a name="work-with-azure-cosmos-account"></a>Arbeta med Azure Cosmos-konto

Azure Cosmos DB är en fullständigt hanterad platform-as-a-service (PaaS). För att börja använda Azure Cosmos DB, bör du först skapa ett Azure Cosmos-konto i Azure-prenumerationen. Ditt Azure Cosmos-konto innehåller ett unikt DNS-namn och du kan hantera ett konto med hjälp av Azure-portalen, Azure CLI eller genom att använda olika språkspecifika SDK: er. Mer information finns i [så här hanterar du ditt Azure Cosmos-konto](how-to-manage-database-account.md).

Azure Cosmos-kontot är den grundläggande enheten på global distribution och hög tillgänglighet. För att distribuera globalt dina data och dataflöde över flera Azure-regioner, kan du lägga till och ta bort Azure-regioner till ditt Azure Cosmos-konto när som helst. Du kan konfigurera ditt Azure Cosmos-konto om du vill ha en enda eller flera Skriv-regioner. Mer information finns i [att lägga till och ta bort Azure-regioner till ditt Azure Cosmos-konto](how-to-manage-database-account.md). Du kan konfigurera den [standardkonsekvens](consistency-levels.md) nivå på Azure Cosmos-konto. Azure Cosmos DB tillhandahåller omfattande serviceavtal som omfattar genomflöde, svarstid på 99: e percentilen, konsekvens och hög tillgänglighet. Mer information finns i [Azure Cosmos DB-serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

För att på ett säkert sätt hantera åtkomst till alla data i ditt Azure Cosmos-konto, kan du använda huvudnycklar som är associerat med ditt konto. Att ytterligare skydda åtkomsten till dina data kan du konfigurera en tjänstslutpunkt för virtuellt nätverk och IP-brandvägg på ditt Azure Cosmos-konto. 

## <a name="elements-in-an-azure-cosmos-account"></a>Element i ett Azure Cosmos-konto

Azure Cosmos DB-behållare är den grundläggande körningsenheten på skalbarhet. Du kan i princip har ett obegränsat dataflöde (RU/s) och lagring på en behållare. Azure Cosmos DB partitionerar transparent behållaren med hjälp av logiska Partitionsnyckeln som du anger för att skala ditt dataflöde och lagring. Mer information finns i [arbetar med Azure Cosmos-behållare och objekt](databases-containers-items.md).

För närvarande kan du skapa upp till 100 Azure Cosmos-konton i en Azure-prenumeration. Ett enda Azure Cosmos-konto hantera i stort sett obegränsad mängd data och etablerade dataflöden. För att hantera dina data och dataflöde, kan du skapa en eller flera Azure-Cosmos-databaser under ditt konto och i den här databasen kan du skapa en eller flera behållare. Följande bild visar hierarkin av element i ett Azure Cosmos-konto:

![Hierarki med en Azure Cosmos-konto](./media/account-overview/hierarchy.png)

## <a name="next-steps"></a>Nästa steg

Du kan nu fortsätta att lära dig hur du hanterar din Azure Cosmos-konto och se andra begrepp som är associerade med Azure Cosmos DB:

* [Anvisningar: hantera ditt Azure Cosmos-konto](how-to-manage-database-account.md)
* [Global distribution](distribute-data-globally.md)
* [Konsekvensnivåer](consistency-levels.md)
* [Arbeta med Azure Cosmos-behållare och objekt](databases-containers-items.md)
* [VNET-tjänstslutpunkt för ditt Azure Cosmos-konto](vnet-service-endpoint.md)
* [IP-brandväggen för ditt Azure Cosmos-konto](firewall-support.md)
* [Anvisningar: lägga till och ta bort Azure-regioner till ditt Azure Cosmos-konto](how-to-manage-database-account.md)
* [Azure Cosmos DB-serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
