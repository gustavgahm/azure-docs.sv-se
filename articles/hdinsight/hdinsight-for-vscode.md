---
title: Azure HDInsight Tools - använder Visual Studio Code för Hive, LLAP eller PySpark | Microsoft Docs
description: Lär dig hur du använder Azure HDInsight Tools för Visual Studio Code för att skapa och skicka frågor och skript.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/15/2018
ms.openlocfilehash: 230c5a50efe762b8f27e6a0a84c36c74403b8ecc
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663884"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Använd Azure HDInsight Tools för Visual Studio Code

Lär dig hur du använder Azure HDInsight Tools för Visual Studio Code (VS Code) för att skapa och skicka Apache Hive-batchjobb, interaktiva Hive-frågor och PySpark-skript för Apache Spark. Först kommer vi att beskriva hur du installerar HDInsight-verktygen i VS Code och sedan vi gå igenom hur du skickar in jobb för att Hive och Spark. 

Azure HDInsight-verktyg kan installeras på de plattformar som stöds av VS Code, inklusive Windows, Linux och macOS. Nedan hittar du de nödvändiga förutsättningarna för olika plattformar.


## <a name="prerequisites"></a>Förutsättningar

Följande krävs för att slutföra stegen i den här artikeln:

- Ett HDInsight-kluster. För att skapa ett kluster, se [Kom igång med HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono är endast krävs för Linux och macOS.
- VSCode [tillägget Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

## <a name="install-the-hdinsight-tools"></a>Installera HDInsight-verktyg
   
När du har installerat kraven kan installera du Azure HDInsight Tools för VS Code. 

### <a name="to-install-azure-hdinsight-tools"></a>Installera Azure HDInsight-verktyg

1. Öppna Visual Studio Code.

2. I den vänstra rutan väljer **tillägg**. I sökrutan anger **HDInsight**.

3. Bredvid **Azure HDInsight Tools**väljer **installera**. Efter några sekunder i **installera** knappen ändras till **Reload**.

4. Välj **Reload** att aktivera den **Azure HDInsight Tools** tillägget.

5. Välj **Reload Window** att bekräfta. Du kan se **Azure HDInsight Tools** i den **tillägg** fönstret.

   ![HDInsight Visual Studio Code Python-installation](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-work-folder"></a>Öppna arbetsmapp för HDInsight

Skapa en arbetsmapp i VS Code innan du kan ansluta till Azure.

### <a name="to-open-a-work-folder"></a>Öppna en arbetsmapp

1. På den **filen** menyn och välj **Öppna mapp**. Sedan ange en befintlig mapp som mappen arbete eller skapa en ny. Mappen visas i den vänstra rutan.

2. I det vänstra fönstret, Välj den **ny fil** ikonen bredvid Arbetsmappens.

   ![Ny fil](./media/hdinsight-for-vscode/new-file.png)

3. Namnge den nya filen med filnamnstillägget .py (Spark-skriptet) eller .hql (Hive-frågor). 

## <a name="connect-to-hdinsight-cluster"></a>Ansluta till HDInsight-kluster

Innan du kan skicka skript till HDInsight-kluster från VS Code, måste du ansluta till ditt Azure-konto eller länka ett kluster (med hjälp av Ambari användarnamn/lösenord eller domänanslutna konto).

### <a name="to-connect-to-azure"></a>Att ansluta till Azure

1. Skapa en ny arbetsmapp och en ny skriptfil om inte redan har.

2. Högerklicka på Skriptredigeraren och klicka sedan på snabbmenyn väljer **HDInsight: Logga in**. Du kan också ange **Ctrl + Skift + P**, och ange sedan **HDInsight: Logga in**.

    ![HDInsight Tools för Visual Studio Code-inloggning](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. För att logga in, följer du anvisningarna logga in i den **utdata** fönstret.
    + För globala Azure-miljön, **HDInsight: Logga in** kommandot utlöser **logga in på Azure** åtgärd i HDInsight explorer och vice versa.

        ![Inloggningsinstruktioner för azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + För andra miljöer, följer du instruktionerna för inloggning.

        ![Inloggningsinstruktioner för andra miljöer](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

    När du är ansluten, visas namnet på ditt Azure i statusfältet längst ned till vänster i VS Code-fönstret. 

    > [!NOTE]  
    > På grund av ett problem med kända Azure-autentisering måste du öppna en webbläsare i privat eller inkognito läge. Om ditt Azure-konto har två faktorer som är aktiverad, rekommenderar vi att använda phone autentisering i stället för PIN-autentisering.
  

4. Högerklicka på Skriptredigeraren för att öppna snabbmenyn. På snabbmenyn kan du utföra följande uppgifter:

    - Logga ut
    - En lista över kluster
    - Ange standard-kluster
    - Skicka interaktiva Hive-frågor
    - Skicka Hive-batchskript
    - Köra en interaktiv PySpark-fråga
    - Skicka PySpark-skript för batch
    - Ange konfiguration

<h3 id="linkcluster">Länka ett kluster</h3>

Du kan länka ett normalt kluster med hjälp av en [Apache Ambari](https://ambari.apache.org/) hanteras användarnamn eller länka ett Enterprise Security Pack säker Hadoop-kluster med hjälp av en domän-användarnamn (t.ex: user1@contoso.com).
1. Öppna kommandopaletten genom att välja **CTRL + SKIFT + P**, och ange sedan **HDInsight: Länka ett kluster**.

   ![länken cluster kommando](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Ange HDInsight-kluster-URL > indata Username -> lösenordet -> Välj typ av kluster -> den visar lyckad info om verifiering.
   
   ![länka kluster dialog](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]  
   > Länkade användarnamn och lösenord används om klustret både loggas i Azure-prenumeration och länkad ett kluster. 
   
3. Du kan se ett länkade-kluster med hjälp av kommandot **listan kluster**. Nu kan du skicka ett skript för att den här länkade kluster.

   ![länkade kluster](./media/hdinsight-for-vscode/linked-cluster.png)

4. Du kan också Avlänka ett kluster genom att mata in **HDInsight: Ta bort länken till ett kluster** från kommandopaletten.


### <a name="to-link-a-generic-apache-livy-endpoint"></a>Länka en allmän Apache Livy-slutpunkt

1. Öppna kommandopaletten genom att välja **CTRL + SKIFT + P**, och ange sedan **HDInsight: Länka ett kluster**.
2. Välj **allmän Livy Endpoint**.
3. Ange allmänna Livy slutpunkten, till exempel: http\:/ / 10.172.41.42:18080.
4. Välj **grundläggande** när behöver auktorisering för allmän Livy slutpunkten, annars väljer **ingen**.
5. Inkommande användarnamn när du väljer **grundläggande** i step4.
6. Lösenordet när du väljer **grundläggande** i step4.
7. Allmän livy slutpunkten länkat.

   ![länkade Allmänt livy-kluster](./media/hdinsight-for-vscode/link-cluster-process-generic-livy.png)

## <a name="list-hdinsight-clusters"></a>Lista över HDInsight-kluster

Om du vill testa anslutningen, kan du lista dina HDInsight-kluster:

### <a name="to-list-hdinsight-clusters-under-your-azure-subscription"></a>Visa en lista över HDInsight-kluster under din Azure-prenumeration
1. Öppna en arbetsmapp och sedan ansluta till Azure. Mer information finns i [öppna HDInsight fungerar mappen](#open-hdinsight-work-folder) och [Anslut till Azure](#connect-to-hdinsight-cluster).

2. Högerklicka på skriptredigeraren och välj sedan **HDInsight: Klustret visas** på snabbmenyn. 

3. HDInsight-kluster som visas i den **utdata** fönstret.

    ![Ange en standardkonfiguration för kluster](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Ange ett standardkluster
1. Öppna en arbetsmapp och Anslut till Azure. Se [öppna HDInsight arbetsmapp](#open-hdinsight-work-folder) och [Anslut till Azure](#connect-to-hdinsight-cluster).

2. Högerklicka på skriptredigeraren och välj sedan **HDInsight: Ange Standardkluster**. 

3. Välj ett kluster som standardklustret för den aktuella skriptfilen. Verktygen automatiskt uppdatera konfigurationsfilen **. VSCode\settings.json**. 

   ![Ställ in standardkonfiguration för kluster](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Ange Azure-miljön
1. Öppna kommandopaletten genom att välja **CTRL + SKIFT + P**.

2. Ange **HDInsight: Ange Azure-miljön**.

3. Välj en miljö, till exempel ”Azure” eller ”AzureChina” som din standardalternativet för inloggning.

4. Under tiden kan verktyget redan har sparats din inloggning standardposten i **. VSCode\settings.json**. Du uppdatera också direkt den i den här konfigurationsfilen. 

   ![Ange standard inloggningen post konfiguration](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Skicka interaktiva Hive-frågor, Hive kommandoskript

Med HDInsight Tools för VS Code, kan du skicka interaktiva Hive-frågor, Hive batch skript till HDInsight-kluster.

1. Skapa en ny arbetsmapp och en ny fil för Hive-skript om du inte redan har gjort det.

2. Ansluta till dina Azure-konto eller länk-kluster.

3. Sedan kopierar du och klistrar in följande kod i din Hive-fil och sparar den.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
4. Högerklicka på Skriptredigeraren, Välj **HDInsight: Hive interaktiv** skicka frågan eller använder genvägen **Ctrl + Alt + I**. Välj **HDInsight: Hive Batch** skicka skriptet eller använder genvägen **Ctrl + Alt + H**. 

5. Välj klustret om du inte har angett ett standardkluster. Med verktygen kan du dessutom skicka ett kodblock istället för hela skriptfilen med snabbmenyn. Efter en liten stund visas resultatet av frågan i en ny flik.

   ![Interaktiva Hive-resultat](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Panelen **RESULTAT**: Du kan spara hela resultatet som en CSV-, JSON- eller Excel-fil till en lokal sökväg, eller markera flera rader.

    - Panelen **MEDDELANDEN**: När du väljer **Rad**nummer, leder det till den första raden i det aktuella skriptet.

## <a name="submit-interactive-pyspark-queries"></a>Köra en interaktiv PySpark-fråga

### <a name="to-submit-interactive-pyspark-queries-to-hdinsight-spark-clusters"></a>Att skicka interaktiv PySpark-frågor till HDInsight Spark-kluster.

1. Skapa en ny arbetsmapp för och en ny .py-fil om du inte redan har.

    > [!NOTE]
    > VSCode rekommenderar att du installerar Python-tillägg för .py-fil. Du kan installera tillägget eller stänga dialogrutan.
    > 
    >![HDInsight Visual Studio Code Python-installation](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

2. Ansluta till ditt Azure-konto om du inte gjort det ännu.

3. Kopiera och klistra in följande kod i skriptfilen:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```
4. Installera Python-miljö om du inte gjort det, se [konfigurera interaktiv PySpark-miljö för Visual Studio Code](set-up-pyspark-interactive-environment.md).

5. Markera det här skriptet. Högerklicka Skriptredigeraren och välj **HDInsight: Interaktiv PySpark**, eller använder genväg **Ctrl + Alt + I**.

6. Välja ett kluster som du vill skicka din PySpark-fråga. Snart därefter visas frågeresultatet i den nya rätta fliken:

   ![Skicka resultat för Python-jobb](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
7. Verktyget stöder också den **SQL-satsen** fråga.

   ![Skicka Python jobb resultatet](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) status för ansökan visas till vänster i nedre statusfältet när du kör frågor. Inte skicka andra frågor när statusen är **PySpark-Kernel (upptagen)**. 

>[!NOTE]  
>Klustren kan underhålla sessionsinformation. Definierad variabel, funktionen och motsvarande värden hålls i sessionen, så att de kan refereras över flera tjänstanrop för samma kluster. 

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 stöds inte med Spark2.2/2.3

PySpark3 stöds inte längre med Apache Spark 2.2 och Spark2.3 klustret, endast ”PySpark” stöds för Python. Det är känt problem som skickas till spark 2.2/2.3 misslyckas med Python3.

   ![Skicka till fel vid hämtning av python3](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Följ anvisningarna för att använda Python2.x: 

1. Installera Python 2.7 till lokala datorn och lägger till den i systemsökvägen.

2. Starta om VSCode.

3. Växla till Python 2 genom att klicka på den **Python XXX** enhetens status menyraden och väljer sedan målet Python.

   ![Välj python-version](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)

## <a name="submit-pyspark-batch-job"></a>Skicka PySpark batch-jobb

1. Skapa en ny arbetsmapp och en ny skriptfil med filnamnstillägget .py om inte redan har.

2. Ansluta till ditt Azure-konto om du inte redan gjort det.

3. Kopiera och klistra in följande kod i skriptfilen:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```
4. Högerklicka på skriptredigeraren och välj sedan **HDInsight: PySpark Batch**, eller använder genväg **Ctrl + Alt + H**. 

5. Välja ett kluster som du vill skicka PySpark-jobb. 

   ![Skicka resultat för Python-jobb](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

När du skickar in ett Python-jobb, skicka loggarna visas i den **utdata** fönster i VS Code. Den **Spark Användargränssnittet URL** och **Yarn UI URL** visas också. Du kan öppna URL: en i en webbläsare för att visa jobbstatusen.

## <a name="apache-livy-configuration"></a>Apache Livy konfiguration

[Apache Livy](https://livy.incubator.apache.org/) konfiguration stöds, kan ställas in på den **. VSCode\settings.json** i arbetsmappar utrymme. Livy-konfigurationen stöder för närvarande endast Python-skriptet. Mer information finns i [Livy README](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Hur du utlöser livy konfiguration**
   
Du hittar på **filen** menyn och välj **inställningar**, och välj **inställningar** på snabbmenyn. Klicka på **ARBETSYTEINSTÄLLNINGARNA** fliken kan du börja ange livy-konfiguration.

Du kan också skicka en fil, läggs Observera mappen .vscode automatiskt till mappen arbete. Du kan hitta livy-konfigurationen genom att klicka på **.vscode\settings.json**.

+ Projektinställningar:

    ![Livy-konfiguration](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>För inställningar **driverMomory** och **executorMomry**, anger du värdet med enhet, till exempel 1 g eller 1 024 m. 

+ De Livy-konfigurationerna som stöds:   

    **POST /batches**   
    Begärandetext

    | namn | beskrivning | typ | 
    | :- | :- | :- | 
    | fil | Fil som innehåller programmet ska köras | sökvägen (krävs) | 
    | proxyUser | Användaren att personifiera när jobbet körs | sträng | 
    | Klassnamn | Programmet Java/Spark-huvudklass | sträng |
    | args | Kommandoradsargument för programmet | lista med strängar | 
    | JAR-filer | JAR-filer som ska användas i den här sessionen | Lista över strängar | 
    | pyFiles | Python-filer som ska användas i den här sessionen | Lista över strängar |
    | filer | filer som ska användas i den här sessionen | Lista över strängar |
    | driverMemory | Mängden minne som ska användas för processen för drivrutinen | sträng |
    | driverCores | Antalet kärnor som ska användas för processen för drivrutinen | int |
    | executorMemory | Mängden minne för att använda per körningsprocess | sträng |
    | executorCores | Antalet kärnor som ska användas för varje executor | int |
    | numExecutors | Antal executors att starta för den här sessionen | int |
    | Arkiv | Arkiv som ska användas i den här sessionen | Lista över strängar |
    | kö | Namnet på YARN-kö som har skickats | sträng |
    | namn | Namnet på den här sessionen | sträng |
    | conf | Egenskaper för Spark-konfiguration | Karta över nyckel = värde |

    Svarstext   
    Det Batch-objektet.

    | namn | beskrivning | typ | 
    | :- | :- | :- | 
    | id | Sessions-id | int | 
    | appId | Program-id för den här sessionen |  String |
    | appInfo | Detaljerad programinformation | Karta över nyckel = värde |
    | log | Log-rader | lista med strängar |
    | state |   Batch-tillstånd | sträng |

>[!NOTE]
>Tilldelade livy konfig visas i utdatafönstret när skicka skript.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integrera med Azure HDInsight i Utforskaren

Azure HDInsight har lagts till den vänstra panelen. Du kan bläddra och hantera klustret direkt.

1. Expandera den **AZURE HDINSIGHT**, om inte logga in, visas **logga in på Azure...**  länk.

    ![Logga in länkbild](./media/hdinsight-for-vscode/hid-azure-hdinsight-sign-in.png)

2. Klicka på **logga in på Azure**, detta alternativ inloggning länken och koden längst ned.

    ![Inloggningsinstruktioner för andra miljöer](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin-code.png)

3. Klicka på **Kopiera & Öppna** knappen öppnar webbläsaren, klistra in koden, klickar du på **Fortsätt** knappen, kan du se tips om inloggning har.

4. När du har loggat in, tillgängliga prenumerationer och -kluster (Spark, Hadoop och HBase stöds) visas i **AZURE HDINSIGHT**. 

   ![Azure HDInsight-prenumeration](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

5. Expandera klustret om du vill visa hive-metadata-databasen och tabellschemat.

   ![Azure HDInsight-kluster](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)

## <a name="additional-features"></a>Ytterligare funktioner

HDInsight för VS Code har stöd för följande funktioner:

- **IntelliSense automatisk komplettering**. Förslag för nyckelord, metoder, variabler och så vidare. Olika ikoner representerar olika typer av objekt.

    ![HDInsight Tools för Visual Studio Code, IntelliSense objekttyper](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense fel markör**. Tjänsten språk understryker redigering fel för Hive-skriptet.     
- **Syntax höjdpunkter**. Språk-tjänsten använder olika färger för att skilja variabler, nyckelord, datatyp, funktioner och så vidare. 

    ![HDInsight Tools för Visual Studio Code syntax höjdpunkter](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Nästa steg

### <a name="demo"></a>Demo
* HDInsight för VS Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Verktyg och tillägg

* [Felsöka Apache Spark-program via fjärranslutning via VPN med hjälp av Azure Toolkit för IntelliJ](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Felsöka Apache Spark-program via fjärranslutning via SSH med hjälp av Azure Toolkit för IntelliJ](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Använda HDInsight Tools för IntelliJ med begränsat Hortonworks-läge](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Använda HDInsight-verktygen i Azure Toolkit för Eclipse för att skapa Apache Spark-program](spark/apache-spark-eclipse-tool-plugin.md)
* [Använda Apache Zeppelin-anteckningsböcker med Apache Spark-kluster på HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter notebook i Apache Spark-kluster för HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualisera Apache Hive-data med Microsoft Power BI i Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Visualisera Interactive Query Hive-data med Power BI i Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Ställ in interaktiv PySpark-miljö för Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Använda Apache Zeppelin för att köra Apache Hive-frågor i Azure HDInsight ](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Scenarier
* [Apache Spark med BI: Utföra interaktiv dataanalys med Spark i HDInsight med BI-verktyg](spark/apache-spark-use-bi-tools.md)
* [Apache Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark med Machine Learning: Använda Spark i HDInsight för att förutse matinspektionsresultat](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Webbplatslogganalys med Apache Spark i HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](spark/apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Apache Spark-kluster med hjälp av Apache Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för Apache Spark-klustret i Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](spark/apache-spark-job-debugging.md)



