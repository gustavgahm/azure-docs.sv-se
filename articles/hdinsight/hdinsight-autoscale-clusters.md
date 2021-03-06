---
title: Skala automatiskt Azure HDInsight-kluster (förhandsversion)
description: Använda HDInsight Autoscale-funktionen för att automatiskt skala kluster
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: hrasheed
ms.openlocfilehash: bd1ffcfd915fe9ece683ec88d27f54b3a9214621
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55475693"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Skala automatiskt Azure HDInsight-kluster (förhandsversion)

Azure HDInsight-kluster Autoskala funktionen skalas automatiskt antalet arbetarnoder i ett kluster upp eller ner baserat på belastning inom ett fördefinierat intervall. När du skapar ett nytt HDInsight-kluster, kan du ange ett lägsta och högsta antal arbetsnoder. Automatisk skalning och Övervakare resurskraven för analyser läsa in och skalar antalet arbetarnoder sedan upp eller ned i enlighet med detta. Det finns ingen extra kostnad för den här funktionen.

## <a name="getting-started"></a>Komma igång

### <a name="create-a-cluster-with-the-azure-portal"></a>Skapa ett kluster med Azure-portalen

> [!Note]
> Automatisk skalning är för närvarande stöds endast för Azure HDInsight Hive, MapReduce och Spark-kluster version 3.6.

Om du vill aktivera funktionen för automatisk skalning, gör du följande som en del av normal klusterskapningsprocessen:

1. Välj **anpassad (storlek, inställningar, appar)** snarare än **Snabbregistrering**.
2. På **anpassade** steg 5 (**klusterstorlek**) kontrollera den **Worker noden Autoskala** kryssrutan.
3. Ange de önskade värdena för:  

    * Inledande **många av arbetsnoder**.  
    * **Minsta** antalet arbetsnoder.  
    * **Maximal** antalet arbetsnoder.  

![Aktivera alternativet för automatisk skalning av worker noden](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

Det inledande antalet arbetarnoder måste ligga mellan lägsta och högsta, inklusive. Det här värdet anger den ursprungliga storleken för klustret när den skapas. Det minsta antalet arbetarnoder måste vara större än noll.

När du har valt VM-typ för varje nodtyp kommer du att kunna se de uppskattade kostnaden för hela klustret. Du kan sedan justera inställningarna så att de passar din budget.

Din prenumeration har en kapacitet kvot för varje region. Det totala antalet kärnor i din huvudnoder som kombineras med det maximala antalet arbetsnoder får inte överskrida kvoten för kapacitet. Den här kvoten är dock en mjuk gräns. Du kan alltid skapa ett supportärende för att få ökad enkelt.

> [!Note]  
> Om du överskrider den totala grundkvoten får du ett felmeddelande om ”maximalt antal nod överskrider de tillgängliga kärnorna i den här regionen, Välj en annan region eller kontakta supporten för att öka kvoten”.

Mer information om HDInsight-kluster med Azure portal finns i [skapa Linux-baserade kluster i HDInsight med Azure portal](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Skapa ett kluster med en Resource Manager-mall

När du skapar ett HDInsight-kluster med en Azure Resource Manager-mall till en `autoscale` noden till den `computeProfile`  >  `workernode` avsnittet med egenskaper `minInstanceCount` och `maxInstanceCount` som visas i json-kodfragmentet nedan.

```json
{                            
    "name": "workernode",
    "targetInstanceCount": 4,
    "autoscale": {
        "minInstanceCount": 2,
        "maxInstanceCount": 10
    },
    "hardwareProfile": {
        "vmSize": "Standard_D13_V2"
    },
    "osProfile": {
        "linuxOperatingSystemProfile": {
            "username": "[parameters('sshUserName')]",
            "password": "[parameters('sshPassword')]"
        }
    },
    "virtualNetworkProfile": null,
    "scriptActions": []
}
```

För mer information om hur du skapar kluster med Resource Manager-mallar, se [skapa Apache Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md).  

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Aktivera och inaktivera automatisk skalning för ett kluster som körs

Aktiverar automatisk skalning för ett aktivt kluster stöds inte för privata förhandsversionen. Det måste vara aktiverat när klustret skapas.

Inaktivera automatisk skalning eller ändra inställningarna för automatisk skalning för ett aktivt kluster stöds inte i privat förhandsvisning. Du måste ta bort klustret och skapa ett nytt kluster för att ta bort eller ändra inställningarna.

## <a name="monitoring"></a>Övervakning

Du kan visa klustret skala upp och skala ned historik som en del av klustermått. Du kan också visa alla åtgärder för skalning över tidigare dag, vecka eller längre tidsperiod.

## <a name="how-it-works"></a>Hur det fungerar

### <a name="metrics-monitoring"></a>Övervaka mått

Automatisk skalning kontinuerligt övervakar klustret och samlar in följande mått:

1. **Totalt antal väntande CPU**: Det totala antalet kärnor som krävs för att starta körning av alla väntande behållare.
2. **Totalt antal väntande minne**: Den totala mängden minne (i MB) krävs för att starta körning av alla väntande behållare.
3. **Totalt antal kostnadsfria CPU**: Summan av alla oanvända kärnor på aktiva arbetsnoderna.
4. **Totalt ledigt minne**: Summan av oanvänt minne (i MB) på de aktiva arbetsnoderna.
5. **Använt minne per nod**: Belastningen på en underordnad nod. En underordnad nod där 10 GB minne används, anses under högre belastning än en worker med 2 GB minne som används.
6. **Antal program huvudservrar per nod**: Antal program Master (AM) behållare som körs på en underordnad nod. En underordnad nod som är värd för 2 AM behållare anses vara viktigare än en underordnad nod som är värd för 0 AM-behållare.

Mått som ovan kontrolleras var 60: e sekund. Automatisk skalning kommer utgör skala och skala ned beslut baserat på de här måtten.

### <a name="cluster-scale-up"></a>Klustret skala upp

När följande villkor har identifierats, utfärdar automatisk skalning en skala upp begäran:

* Totalt antal väntande CPU är större än totalt ledigt CPU i mer än 1 minut.
* Totalt antal väntande minne är större än totala lediga minnet i mer än 1 minut.

Vi beräknar att ett visst antal nya arbetsnoderna behövs för att uppfylla kraven på aktuella CPU och minne och utfärda en skala upp begäran som lägger till det antalet nya arbetsnoderna.

### <a name="cluster-scale-down"></a>Klustret skala ned

När följande villkor har identifierats, utfärdar automatisk skalning en skala ned begäran:

* Totalt antal väntande CPU är mindre än totalt ledigt CPU i mer än 10 minuter.
* Totalt antal väntande minne är mindre än totala lediga minnet i mer än 10 minuter.

Baserat på antalet AM behållare per nod samt kraven på aktuella CPU och minne, utfärdar automatisk skalning en begäran att ta bort ett visst antal noder, anger vilka noder som är möjliga kandidater för borttagning. Som standard tas två noder bort i en cykel.

## <a name="next-steps"></a>Nästa steg

* Läs mer om bästa praxis för att skala kluster manuellt i [skalning Metodtips](hdinsight-scaling-best-practices.md)
