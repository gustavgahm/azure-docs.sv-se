---
title: 'Självstudier: Använda Apache Kafka Streams-API – Azure HDInsight '
description: Lär dig hur du använder Apache Kafka Streams-API med Kafka på HDInsight. Detta API kan du utföra för att strömma bearbetningen mellan ämnen i Kafka.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 11/06/2018
ms.openlocfilehash: cb959bd74322534573f83c2b3258ff28d4c324ff
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53584164"
---
# <a name="tutorial-apache-kafka-streams-api"></a>Självstudier: Apache Kafka Streams-API

Lär dig hur du skapar ett program som använder Apache Kafka Streams-API:et och kör det med Kafka i HDInsight. 

Programmet som används i den här självstudien är en strömmande ordräkning. Den läser textdata från ett Kafka-ämne, extraherar enskilda ord och lagrar sedan ordet och antalet till ett annat Kafka-ämne.

> [!NOTE]  
> Kafka-strömningsbearbetning görs ofta med Apache Spark eller Apache Storm. Kafka version 0.10.0 (i HDInsight 3.5 och 3.6) introducerade Kafka Streams-API. Med detta API kan du transformera dataströmmar mellan indata- och utdata-ämnen. I vissa fall kan detta vara ett alternativ till att skapa en strömmande Spark- eller Storm-lösning. 
>
> Mer information om Kafka Streams finns i dokumentationen [Intro to Streams](https://kafka.apache.org/10/documentation/streams/) (Introduktion till Streams) på Apache.org.

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Ställt in din utvecklingsmiljö
> * Förstå koden
> * Skapa och distribuera programmet
> * Konfigurera Kafka-ämnen
> * Kör koden

## <a name="prerequisites"></a>Nödvändiga komponenter

* En Kafka på HDInsight 3.6-klustret. Information om hur du skapar en Kafka på ett HDInsight-kluster finns i dokumentet [Starta med Apache Kafka i HDInsight](apache-kafka-get-started.md).

* Utför stegen i dokumentet [Apache Kafka-konsument- och producent-API](apache-kafka-producer-consumer-api.md). Stegen i det här dokumentet använder exempelprogram och avsnitt som skapats i den här kursen.

## <a name="set-up-your-development-environment"></a>Ställt in din utvecklingsmiljö

Du måste ha följande komponenter installerade i utvecklingsmiljön:

* [Java JDK 8](https://aka.ms/azure-jdks) eller motsvarande, till exempel OpenJDK.

* [Apache Maven](https://maven.apache.org/)

* En SSH-klient och kommandot `scp` . Mer information finns i dokumentet [Använda SSH med HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>Förstå koden

Exempelprogrammet finns på [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) i underkatalogen `Streaming`. Programmet består av två filer:

* `pom.xml`: Den här filen definierar projektberoenden, Java-version och paketeringsmetoder.
* `Stream.java`: Den här filen implementerar strömningslogiken.

### <a name="pomxml"></a>Pom.xml

Viktiga saker att förstå i `pom.xml`-filen är:

* Beroenden: Det här projektet använder Kafka Streams-API, som tillhandahålls av `kafka-clients`-paketet. Följande XML-kod definierar detta beroende:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    > [!NOTE]  
    > `${kafka.version}`-posten har deklarerats i `<properties>..</properties>`-avsnittet i `pom.xml` och är konfigurerad till Kafka-versionen av HDInsight-klustret.

* Plugin-program: Maven-plugin-program tillhandahåller flera olika funktioner. I det här projektet används följande plugin-program:

    * `maven-compiler-plugin`: Används för att ange att den Java-version som används av projektet är 8. Java 8 krävs av HDInsight 3.6.
    * `maven-shade-plugin`: Används för att generera en uber-jar som innehåller det här programmet samt eventuella beroenden. Den används också för att ange startpunkten för programmet, så att du kan köra Jar-filen direkt utan att behöva ange huvudklassen.

### <a name="streamjava"></a>Stream.java

Filen [Stream.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Streaming/src/main/java/com/microsoft/example/Stream.java) använder Streams-API för att implementera ett ordantalprogram. Den läser data från ett Kafka-ämne som heter `test` och skriver ordräkningen till ett ämne som heter `wordcounts`.

Följande kod definierar ordräkningsprogrammet:

```java
package com.microsoft.example;

import org.apache.kafka.common.serialization.Serde;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.KeyValue;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.KStream;
import org.apache.kafka.streams.kstream.KStreamBuilder;

import java.util.Arrays;
import java.util.Properties;

public class Stream
{
    public static void main( String[] args ) {
        Properties streamsConfig = new Properties();
        // The name must be unique on the Kafka cluster
        streamsConfig.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-example");
        // Brokers
        streamsConfig.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, args[0]);
        // SerDes for key and values
        streamsConfig.put(StreamsConfig.KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        streamsConfig.put(StreamsConfig.VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());

        // Serdes for the word and count
        Serde<String> stringSerde = Serdes.String();
        Serde<Long> longSerde = Serdes.Long();

        KStreamBuilder builder = new KStreamBuilder();
        KStream<String, String> sentences = builder.stream(stringSerde, stringSerde, "test");
        KStream<String, Long> wordCounts = sentences
                .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
                .map((key, word) -> new KeyValue<>(word, word))
                .countByKey("Counts")
                .toStream();
        wordCounts.to(stringSerde, longSerde, "wordcounts");

        KafkaStreams streams = new KafkaStreams(builder, streamsConfig);
        streams.start();

        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
    }
}
```


## <a name="build-and-deploy-the-example"></a>Skapa och distribuera exemplet

För att skapa och distribuera projektet till Kafka p HDInsight-klustret utför du följande steg:

1. Hämta exempel från [ https://github.com/Azure-Samples/hdinsight-kafka-java-get-started ](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Ändra katalogerna till katalogen `Streaming` och använd sedan följande kommando för att skapa ett jar-paket:

    ```bash
    mvn clean package
    ```

    Det här kommandot skapar paketet på `target/kafka-streaming-1.0-SNAPSHOT.jar`.

3. Använd följande kommando för att kopiera filen `kafka-streaming-1.0-SNAPSHOT.jar` till ditt HDInsight-kluster:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar sshuser@clustername-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Ersätt `sshuser` med SSH-användare för klustret och ersätt `clustername` med namnet på klustret. Ange lösenordet för SSH-användarkontot om du uppmanas till det. Mer information om hur du använder `scp` med HDInsight finns i [Använda SSH med HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-apache-kafka-topics"></a>Skapa Apache Kafka-ämnen

1. Använd följande kommando för att öppna en SSH-anslutning till klustret:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Ersätt `sshuser` med SSH-användare för klustret och ersätt `clustername` med namnet på klustret. Ange lösenordet för SSH-användarkontot om du uppmanas till det. Mer information om hur du använder `scp` med HDInsight finns i [Använda SSH med HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Spara klustrets namn som en variabel och installera ett JSON-parsningsverktyget (`jq`), med följande kommandon. Ange Kafka-klustrets namn när du uppmanas till detta:

    ```bash
    sudo apt -y install jq
    read -p 'Enter your Kafka cluster name:' CLUSTERNAME
    ```

3. Använd följande kommandon för att hämta värdar för Apache Kafka-meddelandeköer och Apache Zookeeper. När du blir ombedd anger du lösenordet till klusterinloggningskontot (admin). Du uppmanas att ange lösenordet två gånger.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
    ```

4. Skapa ämnen som används av strömningen med följande kommandon:

    > [!NOTE]  
    > Du kan få ett felmeddelande om att ämnet `test` redan finns. Det är OK eftersom det kan ha skapats i självstudien Producent- och konsument-API.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

    Ämnena använda för följande ändamål:

    * `test`: I det här ämnet tas poster emot. Strömningsprogrammet läses härifrån.
    * `wordcounts`: I det här ämnet lagrar strömningsprogrammet sina utdata.
    * `RekeyedIntermediateTopic`: Det här ämnet används för att partitionera om data när antalet uppdateras av operatorn `countByKey`.
    * `wordcount-example-Counts-changelog`: Det är ämnet är ett tillståndslager som används av åtgärden `countByKey`

    > [!IMPORTANT]  
    > Kafka på HDInsight kan också konfigureras för att automatiskt skapa ämnen. Mer information finns i dokumentet [Configure automatic topic creation](apache-kafka-auto-create-topics.md) (Konfigurera automatiskt skapande av ämne).

## <a name="run-the-code"></a>Kör koden

1. Om du vill starta strömningsprogrammet som en bakgrundsprocess använder du följande kommando:

    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS &
    ```

    > [!NOTE]  
    > Du får kanske en varning om Apache log4j. Du kan ignorera den.

2. För att skicka poster till ämnet `test` använder du följande kommando för att starta producentprogrammet:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

3. När producenten är klar använder du följande kommando för att visa informationen som är lagrad i ämnet `wordcounts`:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --from-beginning
    ```

    > [!NOTE]  
    > Parametrarna `--property` uppmanar konsolkonsumenten att skriva ut nyckeln (ordet) tillsammans med antalet (värdet). Den här parametern konfigurerar även funktionen för avserialisering som ska användas vid läsning av dessa värden från Kafka.

    De utdata som genereras liknar följande text:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
   
    > [!NOTE]  
    > Parametern `--from-beginning` konfigurerar konsumenten för att börja vid början av posterna i ämnet. Antalet ökar varje gång ett ord påträffas, så det här ämnet innehåller flera poster för varje ord med ett ökande antal.

7. Använd __Ctrl + C__ om du vill avsluta producenten. Fortsätt att använda __Ctrl + C__ för att avsluta programmet och konsumenten.

## <a name="next-steps"></a>Nästa steg

I det här dokumentet har du lärt dig använda Apache Kafka Streams-API med Kafka i HDInsight. Använd följande för att lära dig mer om att arbeta med Kafka:

* [Analysera Apache Kafka-loggar](apache-kafka-log-analytics-operations-management.md)
* [Replikera data mellan Apache Kafka-kluster](apache-kafka-mirroring.md)
