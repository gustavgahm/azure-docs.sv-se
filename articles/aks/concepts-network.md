---
title: Begrepp - nätverk i Azure Kubernetes-tjänster (AKS)
description: Läs mer om nätverk i Azure Kubernetes Service (AKS), inklusive grundläggande och avancerade nätverk, ingående domänkontrollanter, belastningsutjämnare och statiska IP-adresser.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: iainfou
ms.openlocfilehash: 62ba98f221041d5bbf9bb095a02d052218eb0fd0
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381276"
---
# <a name="network-concepts-for-applications-in-azure-kubernetes-service-aks"></a>Nätverkskoncept för program i Azure Kubernetes Service (AKS)

I en behållarbaserad mikrotjänster metod för utveckling av måste programkomponenter samarbeta för att bearbeta sina uppgifter. Kubernetes tillhandahåller olika resurser som gör det här programmets kommunikation. Du kan ansluta till och exponera program internt eller externt. Om du vill skapa program med hög tillgänglighet kan du läsa in balansera dina program. Mer komplexa program kan kräva konfiguration av inkommande trafik för SSL/TLS-avslutning eller Routning av flera komponenter. Av säkerhetsskäl kan du också behöva begränsa flödet av nätverkstrafik till eller mellan poddar och noder.

Den här artikeln innehåller grundläggande begrepp som tillhandahåller nätverk till dina program i AKS:

- [Tjänster](#services)
- [Azure-nätverk](#azure-virtual-networks)
- [Ingress-styrenheter](#ingress-controllers)
- [Nätverksprinciper](#network-policies)

## <a name="kubernetes-basics"></a>Grundläggande om Kubernetes

För att tillåta åtkomst till dina program eller om programkomponenter som ska kommunicera med varandra, tillhandahåller Kubernetes ett Abstraktionslager för virtuella nätverk. Kubernetes-noderna är anslutna till ett virtuellt nätverk och kan tillhandahålla inkommande och utgående anslutningar till poddar. Den *kube-proxy* komponenten körs på alla noder att tillhandahålla dessa nätverks-funktioner.

I Kubernetes, *Services* gruppera poddarna för direkt åtkomst via en IP-adress eller DNS-namn och på en viss port. Du kan även distribuera trafik med hjälp av en *belastningsutjämnare*. Mer komplex routning av programtrafik kan även uppnås med *Ingress-styrenheter*. Säkerhet och filtrering av nätverkstrafik för poddar som är möjligt med Kubernetes *nätverksprinciper*.

Azure-plattformen hjälper också till att förenkla virtuella nätverk för AKS-kluster. När du skapar en belastningsutjämnare för Kubernetes är underliggande Azure belastningsutjämningsresursen skapas och konfigureras. När du öppnar nätverksportar till poddar konfigureras de motsvarande Azure reglerna för nätverkssäkerhetsgrupper. För HTTP-routning för program, Azure kan också konfigurera *externa DNS* som ny inkommande vägar har konfigurerats.

## <a name="services"></a>Tjänster

För att förenkla nätverkskonfiguration för programarbetsbelastningar Kubernetes använder *Services* logiskt kan gruppera en uppsättning poddar och ange nätverksanslutning. Följande typer av tjänsten är tillgängliga:

- **Kluster-IP** -skapar en intern IP-adress för användning i AKS-klustret. Bra för interna program som har stöd för andra arbetsbelastningar i klustret.

    ![Diagram över klustrets IP-trafikflödet i ett AKS-kluster][aks-clusterip]

- **NodePort** -skapar en portmappning på den underliggande nod som gör att program som kan nås direkt med noden IP-adress och port.

    ![Diagram över NodePort trafikflödet i ett AKS-kluster][aks-nodeport]

- **LoadBalancer** – skapar en Azure load balancer-resurs, konfigurerar en extern IP-adress och ansluter de begärda poddarna till belastningsutjämnarens serverdelspool. Om du vill tillåta för kunder att skapas räckvidd programmet, regler för belastningsutjämning på önskad portar. 

    ![Diagram över belastningsutjämnare trafikflödet i ett AKS-kluster][aks-loadbalancer]

    För ytterligare kontroll och routning av inkommande trafik, kan du i stället använda en [Ingress-kontrollant](#ingress-controllers).

- **ExternalName** -skapar en specifik DNS-post för enklare programåtkomst.

IP-adressen för belastningsutjämnare och tjänster kan tilldelas dynamiskt eller ange en befintlig statisk IP-adress som du använder. Både interna och externa statiska IP-adresser kan tilldelas. Den här befintlig statisk IP-adress är ofta kopplat till en DNS-post.

Båda *interna* och *externa* belastningsutjämnare kan skapas. Interna belastningsutjämnare endast har tilldelats en privat IP-adress, så kan inte nås från Internet.

## <a name="azure-virtual-networks"></a>Azures virtuella nätverk

I AKS, kan du distribuera ett kluster som använder någon av följande två modeller:

- *Grundläggande* nätverket - nätverket resurser skapas och konfigureras enligt AKS-klustret distribueras.
- *Avancerade* nätverk – The AKS-kluster är ansluten till befintliga virtuella nätverksresurser och konfigurationer.

### <a name="basic-networking"></a>Grundläggande nätverk

Den *grundläggande* nätverk alternativet är standardkonfigurationen för att skapa för AKS-kluster. Azure-plattformen hanterar nätverkskonfiguration och poddar. Grundläggande nätverk är lämplig för distributioner som inte kräver konfiguration av anpassade virtuellt nätverk. Du kan inte definiera nätverkskonfiguration, till exempel namn på undernät eller IP-adressintervall som tilldelats AKS-kluster med grundläggande nätverk.

Noder i ett AKS-kluster som har konfigurerats för användning med grundläggande nätverk den [kubenet] [ kubenet] Kubernetes-plugin-programmet.

Grundläggande nätverk tillhandahåller följande funktioner:

- Exponera en Kubernetes-tjänst externt eller internt via Azure Load Balancer.
- Poddar kan komma åt resurser på Internet.

### <a name="advanced-networking"></a>Avancerade nätverk

*Avancerade* nätverk placerar poddarna i Azure-nätverk som du konfigurerar. Det här virtuella nätverket tillhandahåller automatisk anslutning till andra Azure-resurser och integrering med en omfattande uppsättning funktioner. Avancerade nätverksfunktioner är lämplig för distributioner som kräver specifika konfigurationer av virtuella, till exempel för att använda ett befintligt undernät och anslutning. Du kan definiera dessa namn på undernät och IP-adressintervall med avancerade nätverk.

Noder i ett AKS-kluster som har konfigurerats för användning av avancerad nätverk den [Azure behållare nätverk gränssnitt (CNI)] [ cni-networking] Kubernetes-plugin-programmet.

![Diagram över två noder med bryggor ansluta dem till ett enda Azure virtuellt nätverk][advanced-networking-diagram]

Avancerade nätverk tillhandahåller följande funktioner över grundläggande nätverk:

- Distribuera din AKS-kluster till ett befintligt Azure virtuellt nätverk eller skapa ett nytt virtuellt nätverk och undernät för klustret.
- Varje pod i klustret tilldelas en IP-adress i det virtuella nätverket. Poddarna kan kommunicera direkt med andra poddar i klustret och andra noder i det virtuella nätverket.
- En pod kan ansluta till andra tjänster i ett peer-kopplade virtuella nätverk, inklusive till lokala nätverk via ExpressRoute och plats-till-plats (S2S) VPN-anslutningar. Poddar kan även nås från en lokal plats.
- Poddar i ett undernät som har aktiverat Tjänsteslutpunkter kan på ett säkert sätt ansluta till Azure-tjänster, till exempel Azure Storage och SQL DB.
- Du kan skapa användardefinierade vägar (UDR) för att dirigera trafik från poddar till en virtuell nätverksenhet.

Mer information finns i [konfigurera avancerade nätverk för ett AKS-kluster][aks-configure-advanced-networking].

## <a name="ingress-controllers"></a>Ingress-styrenheter

När du skapar en LoadBalancer typ Service skapas en underliggande Azure belastningsutjämningsresursen. Belastningsutjämnaren har konfigurerats för att distribuera trafik till poddarna i din tjänst på en viss port. LoadBalancer-fungerar bara på layer 4 - tjänsten är inte medveten om faktiska program och kan inte göra några ytterligare överväganden för routning.

*Ingress-styrenheter* fungerar på lager 7 och smartare regler kan använda för att distribuera trafik. Ett vanligt användningsområde för Ingress-kontrollant är att dirigera HTTP-trafik till olika program baserat på inkommande URL.

![Diagram över ingående trafikflödet i ett AKS-kluster][aks-ingress]

I AKS, kan du skapa en Ingress-resurs med något som NGINX eller använda funktionen AKS HTTP programmet routning. När du aktiverar HTTP programmet routning för ett AKS-kluster kan Azure-plattformen skapar Ingress-kontrollanten och en *externt DNS* controller. Då nya Ingress-resurser skapas i Kubernetes, skapas de nödvändiga DNS A-posterna i en klusterspecifika DNS-zon. Mer information finns i [distribuera routning av HTTP-programmet][aks-http-routing].

En annan vanlig funktion för ingång är SSL/TLS-avslutning. I stora webbprogram som kan nås via HTTPS, kan TLS-avslutning hanteras av Ingress-resursen inte själva programmet. Du kan konfigurera Ingress-resurs för att använda leverantörer som krypterar vi för att tillhandahålla automatisk generering av TLS-certifiering och konfiguration. Läs mer om hur du konfigurerar en NGINX-Ingress-kontrollant med ska vi kryptera [Ingångshändelser och TLS][aks-ingress-tls].

## <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper

En säkerhet grupp filter nätverkstrafik för virtuella datorer, till exempel AKS-noder. När du skapar tjänster, till exempel en LoadBalancer konfigurerar Azure-plattformen automatiskt alla regler för nätverkssäkerhetsgrupper som krävs. Inte manuellt konfigurera regler för nätverkssäkerhetsgrupper för att filtrera trafik för poddar i ett AKS-kluster. Definiera alla nödvändiga portar och vidarebefordran som en del av Kubernetes Service-manifest och låt Azure-plattformen skapa eller uppdatera lämpliga regler.

Nätverkssäkerhetsgrupp finns regler för trafik som SSH som standard. Det är dessa standardregler för klusterhantering och felsöka åtkomst. Ta bort dessa standardregler kan orsaka problem med hantering av AKS och delar upp servicenivåmål (SLO).

## <a name="next-steps"></a>Nästa steg

Om du vill komma igång med AKS nätverk finns i [skapa och konfigurera avancerade nätverk för ett AKS-kluster][aks-configure-advanced-networking].

Mer information om core Kubernetes och AKS-begrepp finns i följande artiklar:

- [Kubernetes / AKS-kluster och arbetsbelastningar][aks-concepts-clusters-workloads]
- [Kubernetes / AKS åtkomst och identitet][aks-concepts-identity]
- [Kubernetes / AKS-säkerhet][aks-concepts-security]
- [Kubernetes / AKS-lagring][aks-concepts-storage]
- [Kubernetes / AKS skala][aks-concepts-scale]

<!-- IMAGES -->
[aks-clusterip]: ./media/concepts-network/aks-clusterip.png
[aks-nodeport]: ./media/concepts-network/aks-nodeport.png
[aks-loadbalancer]: ./media/concepts-network/aks-loadbalancer.png
[advanced-networking-diagram]: ./media/concepts-network/advanced-networking-diagram.png
[aks-ingress]: ./media/concepts-network/aks-ingress.png

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[aks-http-routing]: http-application-routing.md
[aks-ingress-tls]: ingress.md
[aks-configure-advanced-networking]: configure-advanced-networking.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md