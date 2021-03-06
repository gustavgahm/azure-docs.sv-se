---
title: Översikt över distribution - Avere vFXT för Azure
description: Översikt över distribution Avere vFXT för Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 1be11fff7139b250e85fe15cec9082a2c85cf857
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55298542"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT för Azure – distributionsöversikt

Den här artikeln ger en översikt över de steg som krävs för att hämta en Avere vFXT för Azure-kluster vara igång och körs.

Flera aktiviteter som krävs innan och när du har skapat klustret vFXT från Azure Marketplace. Att ha en tydlig uppfattning om hur start till slut kan du begränsa det arbete som behövs. 

## <a name="deployment-steps"></a>Distributionssteg

Efter [planera systemet](avere-vfxt-deploy-plan.md), kan du börja skapa Avere vFXT klustret. 

En Azure Resource Manager-mall i Azure Marketplace samlar in nödvändig information och distribuerar automatiskt hela klustret. 

När vFXT klustret är igång kan du vill veta hur du ansluter klienter till den vid behov, hur du flyttar dina data till den nya Blob storage-behållaren.  

Här är en översikt över alla steg.

1. Konfigurera krav 

   Innan du skapar en virtuell dator måste du skapa en ny prenumeration för Avere vFXT projektet, konfigurera äganderätten till prenumerationen, kontrollera kvoter och begära en ökning om det behövs och acceptera villkoren för att använda Avere vFXT programvara. Läs [förberedelser för att skapa Avere vFXT](avere-vfxt-prereqs.md) detaljerade anvisningar.

1. Skapa en åtkomst för klusternoderna

   Azure använder [rollbaserad åtkomstkontroll](../role-based-access-control/index.yml) (RBAC) för att auktorisera klusternodens virtuella datorer att utföra vissa uppgifter. Till exempel måste klusternoderna kunna tilldela eller omtilldelar IP-adresser till andra noder i klustret. Innan du skapar klustret måste du definiera en roll som ger dem behörighet.

   Läs [skapa klusterrollen noden åtkomst](avere-vfxt-prereqs.md#create-the-cluster-node-access-role) anvisningar.

   Kontrollanten klustret använder också en åtkomstroll, men du kan acceptera standardroll, ägare, istället för att skapa dina egna. Om du vill skapa en anpassad roll för kluster-styrenheten läsa [anpassad åtkomst kontrollantrollen](avere-vfxt-controller-role.md). 

1. Skapa Avere vFXT kluster 

   Använd Azure Marketplace för att skapa Avere vFXT för Azure-kluster. En mall samlar in informationen som krävs och kör skript för att skapa den slutgiltiga produkten.

   Skapa kluster omfattar följande steg som utförs av marketplace-mall: 

   * Skapa nytt nätverk infrastruktur och resursgrupper, om det behövs
   * Skapa en *kluster controller*  

     Kluster-styrenheten är en enkel virtuell dator som finns i samma virtuella nätverk som Avere vFXT klustret och anpassad programvara som behövs för att skapa och hantera klustret. Kontrollanten skapar vFXT noderna och formulär klustret och det ger också ett kommandoradsgränssnitt för att hantera klustret under dess livslängd.

     Om du konfigurerar din kontrollant med en offentlig IP-adress måste kan den också fungera som en jump-värd för att ansluta till klustret från utanför det virtuella nätverket.

   * Klustret skapas noden virtuella datorer
   * Konfigurera virtuella datorer i klusternoden som klustret
   * Du kan också skapa en ny blobbehållare och konfigurera den som backend-lagring för klustret

1. Konfigurera klustret 

   Anslut till Avere vFXT Konfigurationsgränssnittet (Avere Kontrollpanelen) att anpassa inställningar för klustrets. Anmäla dig till stöd för övervakning och Lägg till lagringssystemet om du använder ett lokalt datacenter.

   * [Åtkomst till vFXT klustret](avere-vfxt-cluster-gui.md)
   * [Aktivera stöd för](avere-vfxt-enable-support.md)
   * [Konfigurera lagring](avere-vfxt-add-storage.md) (vid behov)

1. Montera klienter

   Följ riktlinjerna i [montera Avere vFXT klustret](avere-vfxt-mount-clients.md) vill veta mer om belastningsutjämning och hur klientdatorer ska ansluta till klustret.

1. Lägg till data (vid behov)

   Eftersom Avere vFXT är en skalbar flera cache, är det bästa sättet att flytta data till en ny backend-storage-behållare med flera klienten, flertrådat fil kopia strategi. Läs [flytta data till klustret vFXT](avere-vfxt-data-ingest.md) mer information.

## <a name="next-steps"></a>Nästa steg

Fortsätta att [förberedelser för att skapa Avere vFXT](avere-vfxt-prereqs.md) utföra preliminär åtgärder för att distribuera Avere vFXT för Azure. 