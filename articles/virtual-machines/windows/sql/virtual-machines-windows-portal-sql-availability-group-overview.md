---
title: Tillgänglighet för SQL Server grupperar - Azure-datorer – översikt | Microsoft Docs
description: Den här artikeln introducerar SQL Server-Tillgänglighetsgrupper på virtuella Azure-datorer.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: 5f8ae6d9138a7413b0cca4cca7bcc47c13212674
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358059"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Introduktion till SQL Server Always On-Tillgänglighetsgrupper på virtuella Azure-datorer #

Den här artikeln ger en introduktion av SQL Server-Tillgänglighetsgrupper på Azure Virtual Machines. 

Always On-Tillgänglighetsgrupper på Azure Virtual Machines liknar ständigt aktiverade Tillgänglighetsgrupper lokalt. Mer information finns i [Always On-Tillgänglighetsgrupper (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

Diagrammet visar delarna av en fullständig SQL Server-tillgänglighetsgrupp i Azure Virtual Machines.

![Tillgänglighetsgrupp](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Den viktigaste skillnaden för en tillgänglighetsgrupp i Azure Virtual Machines är att Azure-datorer, kräver en [belastningsutjämnare](../../../load-balancer/load-balancer-overview.md). Belastningsutjämnaren innehåller IP-adresser för tillgänglighetsgruppens lyssnare. Om du har mer än en tillgänglighetsgrupp måste varje grupp en lyssnare. En belastningsutjämnare har stöd för flera lyssnare.

Dessutom på en virtuell Azure IaaS-dator gästredundanskluster rekommenderar vi ett enda nätverkskort per server (klusternoden) och ett enda undernät. Azure-nätverk har fysiska redundans, vilket gör ytterligare nätverkskort och undernät onödiga på ett gästkluster för virtuella Azure IaaS-datorer. Även om verifieringsrapporten utfärdar en varning att noderna är endast kan nås i ett enda nätverk, kan den här varningen ignoreras på Azure IaaS VM-gästredundanskluster. 

När du är redo att skapa en tillgänglighetsgrupp för SQL Server på Azure Virtual Machines, referera till de här självstudierna.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Automatiskt skapa en tillgänglighetsgrupp från en mall

[Konfigurera Always On-tillgänglighetsgrupp i Azure VM automatiskt – Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Manuellt skapa en tillgänglighetsgrupp i Azure portal

Du kan också skapa virtuella datorer själv utan mallen. Först slutföra förutsättningarna och sedan skapa tillgänglighetsgruppen. Finns i följande avsnitt: 

- [Konfigurera krav för SQL Server Always On-Tillgänglighetsgrupper på Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Skapa Always On Availability Group att förbättra tillgänglighet och katastrofåterställning](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Nästa steg

[Konfigurera en SQLServer Always On-tillgänglighetsgrupp på virtuella Azure-datorer i olika regioner](virtual-machines-windows-portal-sql-availability-group-dr.md).
