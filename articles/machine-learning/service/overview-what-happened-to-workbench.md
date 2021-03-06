---
title: Vad hände med Machine Learning Workbench?
titleSuffix: Azure Machine Learning service
description: Läs mer om vad som hände med Machine Learning Workbench-programmet, vad som har ändrats i tjänsten Azure Machine Learning Service och vad supporttidslinjen är.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 01/09/2019
ms.custom: seodec18
ms.openlocfilehash: e4e4c9ae764e1278251c01d41a0617ed2e88e439
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242322"
---
# <a name="what-happened-to-azure-machine-learning-workbench"></a>Vad hände med Azure Machine Learning Workbench?

Azure Machine Learning Workbench-programmet och vissa andra tidiga funktioner togs bort och ersattes i versionen från september 2018 för att frigöra plats för en förbättrad [arkitektur](concept-azure-machine-learning-architecture.md). 

För att förbättra upplevelsen innehåller versionen många viktiga uppdateringar som baseras på feedback från kunder. Grundläggande funktioner från experimentkörningar till modelldistribution har inte förändrats. Men du kan nu använda den robusta <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> och [Azure CLI](reference-azure-machine-learning-cli.md) för att utföra dina maskininlärningsaktiviteter och pipelines.  

De flesta av de artefakter som skapades i den tidigare versionen av Azure Machine Learning Service lagras i din egen lokala lagring eller molnlagring. Dessa artefakter försvinner aldrig.

I den här artikeln får lära du dig om vad som ändrats och hur det påverkar ditt befintliga arbete med Azure Machine Learning Workbench och dess API:er.

>[!Warning]
>Den här artikeln är inte avsedd för Azure Machine Learning Studio-användare. Den är till för Azure Machine Learning-tjänstkunder som har installerat Workbench-programmet (förhandsversionen) och/eller har konton för experimentering och förhandsgranskning av modellhantering.


## <a name="what-changed"></a>Vad har ändrats?

Den senaste versionen av Azure Machine Learning Service innehåller följande funktioner:
+ En [förenklad Azure-resursmodell](concept-azure-machine-learning-architecture.md).
+ Ett [nytt användargränssnitt för portalen](how-to-track-experiments.md) för att hantera experiment och beräkningsmål.
+ Ett nytt och mer omfattande Python <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>.
+ Det nya, utökade [Azure CLI-tillägget](reference-azure-machine-learning-cli.md) för maskininlärning.

[Arkitekturen](concept-azure-machine-learning-architecture.md) har gjorts om för enklare användning. I stället för flera Azure-resurser och konton behöver du bara en [Azure Machine Learning-tjänstarbetsyta](concept-azure-machine-learning-architecture.md#workspace). Du kan skapa arbetsytor snabbt i [Azure-portalen](quickstart-get-started.md). Genom att använda en arbetsyta kan flera användare lagra tränings- och distributionsberäkningsmål, modellexperiment, Docker-avbildningar, distribuerade modeller och så vidare.

Även om det finns nya förbättrade CLI- och SDK-klienter i den aktuella versionen, har själva Workbench-skrivbordsprogrammet tagits ur bruk. Experiment kan hanteras i [instrumentpanelen för arbetsytan i Azure-portalen](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Använd instrumentpanelen för att hämta din experimenthistorik, hantera beräkningsmål som är kopplade till din arbetsyta, hantera modeller och Docker-avbildningar och även distribuera webbtjänster.

<a name="timeline"></a>

## <a name="support-timeline"></a>Supporttidslinje

Den 9 januari 2019 upphörde stödet för Machine Learning Workbench-, Azure Machine Learning-experimenterings- och modellhanteringskonton, och associerade SDK och CLI har avslutats. 

Alla nya funktioner är tillgängliga med hjälp av detta <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, [kommandoradsgränssnittet](reference-azure-machine-learning-cli.md) och [portalen](quickstart-get-started.md).

## <a name="what-about-run-histories"></a>Vad händer med körhistorik?

Äldre körhistorik är inte längre tillgänglig, men du kan fortfarande visa dina körningar i den senaste versionen. 

Körhistorik kallas nu **experiment**. Du kan samla in modellens experiment och utforska dem med hjälp av SDK, CLI eller Azure Portal.

Portalens instrumentpanel för arbetsytor stöds endast i webbläsarna Microsoft Edge, Chrome och Firefox:

[ ![Onlineportalen](./media/overview-what-happened-to-workbench/image001.png) ] (./media/overview-what-happened-to-workbench/image001.png#lightbox)

Börja träna dina modeller och spåra körhistorik med hjälp av nya CLI och SDK. Hur du gör visas i [Självstudie: träna modeller med Azure Machine Learning-tjänsten](tutorial-train-models-with-aml.md).

## <a name="can-i-still-prep-data"></a>Går det fortfarande att förbereda data?

Dina befintliga dataförberedelsefiler kan inte överföras till den senaste versionen eftersom vi inte längre har Machine Learning Workbench. Däremot kan du fortfarande förbereda datauppsättningar av valfri storlek för modellering.   

Med datamängder oavsett storlek kan du använda [Azure Machine Learning-SDK för dataförberedelse](http://aka.ms/data-prep-sdk) för att snabbt förbereda dina data före modellering genom att skriva Python-kod. 

Du kan följa [den här självstudien](tutorial-data-prep.md) för att lära dig mer om hur du använder Azure Machine Learning SDK för dataförberedelse.

## <a name="will-projects-persist"></a>Kommer projekt att bevaras?

Du förlorar inte någon kod eller något arbete. I den äldre versionen är projekt molnentiteter med en lokal katalog. I den senaste versionen kopplar du lokala kataloger till Azure Machine Learning Service-arbetsytan med hjälp av en lokal konfigurationsfil. Se ett [diagram över den senaste arkitekturen](concept-azure-machine-learning-architecture.md).

Mycket av projektinnehållet finns redan på den lokala datorn. Så du behöver bara skapa en konfigurationsfil i denna katalog och referera till den i koden för att ansluta till din arbetsyta. Vill du fortsätta att använda den lokala katalog som innehåller filer och skript anger du katalogens namn i Python-kommandot ['experiment.submit'](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), eller så använder du CLI-kommandot 'az ml project attach'.  Exempel:
```python
run = exp.submit(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
```

Lär dig hur du kommer igång i [Python med huvudsaklig SDK](quickstart-create-workspace-with-python.md) med hjälp av [Azure Portal](quickstart-get-started.md).

## <a name="what-about-my-registered-models-and-images"></a>Vad händer med mina registrerade modeller och bilder?
 
De modeller som du registrerade i ditt gamla modellregister måste migreras till din nya arbetsyta om du vill fortsätta att använda dem. För att migrera dina modeller hämtar du modellerna och registrerar dem på nytt i den nya arbetsytan. 

Bilder som du skapade i ditt gamla avbildningsregister måste återskapas i den nya arbetsytan om du vill fortsätta att använda dem. Du kan återskapa dessa bilder genom att följa avsnittet [Konfigurera och skapa avbildning](how-to-deploy-and-where.md#configureimage). 

## <a name="what-about-deployed-web-services"></a>Vad händer med distribuerade webbtjänster?

När nu stödet för det äldre kommandoradsgränssnittet har upphört kan du inte längre distribuera om modeller eller hantera webbtjänster som du ursprungligen har distribuerat med ditt modellhanteringskonto. Dessa webbtjänster kommer dock fortsätta att fungera så länge det finns stöd för Azure Container Service (ACS).

I den senaste versionen distribueras modeller som webbtjänster till Azure Container Instances (ACI) eller Azure Kubernetes Service-kluster (AKS). Du kan även distribuera till FPGA och till Azure IoT Edge. 

Mer information finns i följande artiklar:
+ [Var och hur du distribuerar modeller](how-to-deploy-and-where.md)
+ [Självstudier: Distribuera modeller i Azure Machine Learning-tjänsten](tutorial-deploy-models-with-aml.md)

## <a name="what-about-the-old-sdk-and-cli"></a>Vad gäller för den gamla SDK:n och CLI?

Ja, de fortsätter att fungera fram till januari. Se föregående [tidslinje](#timeline). Vi rekommenderar att du börjar skapa dina nya experiment och modeller med den senaste SDK:n eller CLI.

Genom att använda den nya Python SDK:n i den senaste versionen kan du interagera med Azure Machine Learning Service i alla Python-miljöer. Lär dig hur du installerar den senast <a href="https://aka.ms/aml-sdk" target="_blank">SDK:n</a>. Du kan också använda det uppdaterade [Azure Machine Learning-tillägget](reference-azure-machine-learning-cli.md) med den omfattande uppsättningen `az ml`-kommandon för att interagera med tjänsten i valfri kommandoradmiljö, inklusive Azure Cloud Shell.

## <a name="what-about-visual-studio-code-tools-for-ai"></a>Vad gäller för Visual Studio Code Tools för AI?

I den senaste versionen har tillägget bytt namn till Azure Machine Learning för Visual Studio Code. Det har även utökats och förbättrats för att fungera med föregående nya funktioner.

[ ![Azure Machine Learning för Visual Studio Code](./media/overview-what-happened-to-workbench/vscode.png)] (./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Vad gäller för domänpaket?

Domänpaketen för visuellt innehåll, textanalys och prognostisering kan inte användas med den senaste versionen av Azure Machine Learning. Du kan dock fortfarande skapa och träna modeller för visuellt innehåll, textanalys och prognostisering med den senaste Azure Machine Learning Python-<a href="https://aka.ms/aml-sdk" target="_blank">SDK:n</a>. Om du vill lära dig mer om att migrera befintliga modeller som skapats med visuellt innehåll-, textanalys- och prognostiseringspaket, kontakta[AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Nästa steg

Lär dig mer om den [senaste arkitekturen för Azure Machine Learning Service](concept-azure-machine-learning-architecture.md). 

En översikt över tjänsten finns i [Vad är tjänsten Azure Machine Learning?](overview-what-is-azure-ml.md)

Söker du en snabbstart som visar hur du skapar en arbetsyta, skapar ett projekt, kör ett skript och utforskar körningshistoriken för skriptet med den senaste versionen av Azure Machine Learning-tjänsten kan du prova med [Komma igång med Azure Machine Learning-tjänsten](quickstart-get-started.md).

För en mer ingående beskrivning av det här arbetsflödet kan du följa den [fullständiga självstudien](tutorial-train-models-with-aml.md), som innehåller detaljerade anvisningar för att träna och distribuera modeller med Azure Machine Learning-tjänsten. 
