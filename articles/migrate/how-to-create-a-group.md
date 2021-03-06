---
title: Gruppera datorer för utvärdering med Azure Migrate | Microsoft Docs
description: Beskriver hur du kan gruppera datorer innan du kör en utvärdering med Azure Migrate-tjänsten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/28/2018
ms.author: raynew
ms.openlocfilehash: c11d2f22fa08417107b0eecdd902b4521410b358
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53252430"
---
# <a name="group-machines-for-assessment"></a>Gruppera datorer för utvärdering

Den här artikeln beskriver hur du skapar en grupp av datorer för utvärdering av [Azure Migrate](migrate-overview.md). Azure Migrate utvärderar datorer i gruppen för att kontrollera om de är lämplig för migrering till Azure och ger uppskattningar för storlek och kostnad för att köra datorn i Azure. Du kan manuellt skapa gruppen i Azure Migrate med följande metod om du vet att de datorer som behöver migreras tillsammans. Om du inte är mycket säker om de datorer som behöver grupperas tillsammans kan du använda beroendevisualiseringsfunktionen i Azure Migrate för att skapa grupper. [Läs mer.](how-to-create-group-machine-dependencies.md)

> [!NOTE]
> Beroendevisualiseringsfunktionen är inte tillgänglig i Azure Government.

## <a name="create-a-group"></a>Skapa en grupp

1. I den **översikt** i Azure Migrate-projektet, under hantera, klickar du på **grupper** > **+ grupp**, och ange ett gruppnamn.
2. Lägg till en eller flera datorer i gruppen och klicka på **skapa**.
3. Du kan också välja för att köra en ny utvärdering för gruppen.

    ![Skapa en grupp](./media/how-to-create-a-group/create-group.png)

När gruppen har skapats kan du ändra det genom att välja gruppen på den **grupper** sidan och sedan lägga till eller ta bort datorer.

## <a name="next-steps"></a>Nästa steg

- Lär dig hur du använder [datorberoende mappning](how-to-create-group-machine-dependencies.md) att skapa grupper med hög exakthet.
- [Läs mer](concepts-assessment-calculation.md) om hur utvärderingar beräknas.
