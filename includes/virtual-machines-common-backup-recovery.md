---
title: ta med fil
description: ta med fil
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 3dfc72ff0347a93c6c6dce0e7ec763dd8241c55b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/16/2018
ms.locfileid: "29958817"
---
## <a name="azure-backup"></a>Azure Backup

Använda Azure Backup för att säkerhetskopiera virtuella Azure-datorer kör produktionsarbetsbelastningar. Azure Backup stöder programkonsekvent säkerhetskopiering för både Windows och Linux virtuella datorer. Med Azure Backup skapas återställningspunkter som lagras i geo-redundanta återställningsvalv. När du återställer från en återställningspunkt kan du återställa hela den virtuella datorn eller bara specifika filer. 

En enkel, praktiska introduktion till Azure Backup för virtuella Azure-datorer finns i ”Säkerhetskopiera virtuella datorer i Azure” självstudierna för [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) eller [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).

Mer information om hur Azure Backup fungerar finns [planera din infrastruktur för VM-säkerhetskopiering i Azure](../articles/backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery skyddar dina virtuella datorer från ett scenario med en större katastrof när en hela region påträffar ett avbrott på grund av större naturkatastrof eller omfattande driftstopp. Du kan konfigurera Azure Site Recovery för din virtuella dator så att du kan återställa ditt program med en enda klickning i frågan minuter. Du kan replikera till en Azure-region önskat, är inte begränsat till parad regioner. 

Du kan köra katastrofåterställning flyttar med redundanstestningar på begäran, utan att påverka dina produktionsarbetsbelastningar eller pågående replikering. Skapa återställningsplaner för att samordna redundans och återställning efter fel för hela programmet körs på flera virtuella datorer. Funktionen återställning plan är integrerad med Azure automation-runbooks.

Du kan komma igång med [replikering av virtuella datorer](https://aka.ms/a2a-getting-started). 

## <a name="managed-snapshots"></a>Hanterad ögonblicksbilder 

I utvecklings- och testmiljöer ger ögonblicksbilder en snabb och enkel alternativet för att säkerhetskopiera virtuella datorer som använder hanterade diskar. En hanterad ögonblicksbild är en skrivskyddad fullständig kopia av hanterade diskar. Ögonblicksbilder finns oberoende av källdisken och kan användas för att skapa nya hanterade diskar för att återskapa en virtuell dator. De debiteras baserat på den använda delen av disken. Om du till exempel skapar en ögonblicksbild av en hanterad disk med etablerad kapacitet på 64 GB och faktisk använd datastorlek på 10 GB så debiteras ögonblicksbilden endast för den använda datastorleken på 10 GB.  

Mer information om hur du skapar ögonblicksbilder finns:

* [Skapa kopia av en virtuell hårddisk som lagras som en hanterad disk med hjälp av ögonblicksbilder i Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Skapa kopia av en virtuell hårddisk som lagras som en hanterad disk med hjälp av ögonblicksbilder i Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Nästa steg
Du kan prova att använda Azure Backup genom att följa ”säkerhetskopiering av Windows-datorer kursen” i [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) eller [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).
