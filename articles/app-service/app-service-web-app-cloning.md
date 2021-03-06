---
title: Klona app med PowerShell - Azure App Service
description: Lär dig mer om att klona din App Service-app till en ny app med hjälp av PowerShell.
services: app-service\web
documentationcenter: ''
author: ahmedelnably
manager: stefsch
editor: ''
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2016
ms.author: aelnably
ms.custom: seodec18
ms.openlocfilehash: 17ea8545855cd926a393e9e40d3eccaabd6dba53
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54886534"
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service-App kloning med hjälp av PowerShell
Med denna version av Microsoft Azure PowerShell version 1.1.0 är ett nytt alternativ har lagts till `New-AzureRMWebApp` som hjälper dig att klona en befintlig App Service-app till en nyligen skapad app i en annan region eller i samma region. Det här alternativet gör att kunderna kan distribuera ett antal appar över olika regioner snabbt och enkelt.

Appkloning är för närvarande endast stöd för premium-nivån app service-planer. Den nya funktionen använder samma begränsningar som säkerhetskopieringsfunktionen i App Service, se [säkerhetskopiera en app i Azure App Service](manage-backup.md).

## <a name="cloning-an-existing-app"></a>Klona en befintlig app
Scenario: En befintlig app i regionen södra centrala USA och du vill klona innehållet till en ny app i norra centrala USA. Detta kan inträffa med Azure Resource Manager-versionen av PowerShell-cmdleten för att skapa en ny app med den `-SourceWebApp` alternativet.

Att känna till resursgruppens namn som innehåller källappen, du kan använda följande PowerShell-kommando för att hämta information om appen källa (i det här fallet med namnet `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Du kan använda för att skapa en ny App Service-Plan, `New-AzureRmAppServicePlan` kommandot som i följande exempel

```PowerShell
New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Med hjälp av den `New-AzureRmWebApp` kommandot kan du skapa den nya appen i regionen USA, norra centrala och koppla dem till en premium-nivån som App Service-Plan. Du kan dessutom använda samma resursgrupp som källappen eller definiera en ny resursgrupp som du ser i följande kommando:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Om du vill klona en befintlig app, inklusive alla tillhörande distributionsplatser, måste du använda den `IncludeSourceWebAppSlots` parametern. Följande PowerShell-kommando visar hur du använder parametern med det `New-AzureRmWebApp` kommando:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Om du vill klona en befintlig app i samma region, måste du skapa en ny resursgrupp och en ny app service plan i samma region och sedan använda följande PowerShell-kommando för att klona appen:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klona en befintlig App som en App Service Environment
Scenario: En befintlig app i regionen södra centrala USA och du vill klona innehållet till en ny app till en befintlig App Service Environment (ASE).

Att känna till resursgruppens namn som innehåller källappen, du kan använda följande PowerShell-kommando för att hämta information om appen källa (i det här fallet med namnet `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Lita på ASE-namn och resursgruppens namn som ASE tillhör, kan du skapa den nya appen i den befintliga ASE som visas i följande kommando:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

Den `Location` parametern krävs på grund av äldre orsak, men ignoreras när du skapar appen i en ase-miljö. 

## <a name="cloning-an-existing-app-slot"></a>Klona en befintlig App-plats
Scenario: Du vill klona en befintlig distributionsplats för en app till antingen en ny app eller en ny plats. Den nya appen kan vara i samma region som den ursprungliga appen platsen eller i en annan region.

Att känna till resursgruppens namn som innehåller källappen, du kan använda följande PowerShell-kommando för att hämta information om källa app fack (i det här fallet med namnet `source-appslot`) är kopplad till `source-app`:

```PowerShell
$srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-app -Slot source-appslot
```

Följande kommando visar hur du skapar en klon av källappen till en ny app:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-app -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Konfigurera Traffic Manager vid kloning av en app
Skapa appar för flera regioner och konfigurera Azure Traffic Manager för att dirigera trafik till de här apparna är ett viktigt scenario för att säkerställa att kunders appar med hög tillgänglighet. När du klonar en befintlig app, har du möjlighet att ansluta båda apparna till en ny traffic manager-profil eller en befintlig. Endast Azure Resource Manager-versionen av Traffic Manager stöds.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Skapa en ny Traffic Manager-profil när Kloningen av en app
Scenario: Du vill klona en app till en annan region när du konfigurerar en Azure Resource Manager traffic manager-profil som innehåller båda apparna. Följande kommando visar hur du skapar en klon av källappen till en ny app när du konfigurerar en ny Traffic Manager-profil:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-app-to-an-existing-traffic-manager-profile"></a>Lägger till nya klonade appen i en befintlig Traffic Manager-profil
Scenario: Du redan har en Azure Resource Manager traffic manager-profil och vill lägga till båda apparna som slutpunkter. Du vill göra det måste du först Assemblera befintlig traffic manager-profil-ID. Du behöver prenumerations-ID, resursgruppens namn och det befintliga profilnamnet för traffic manager.

```PowerShell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

När traffic Manager-ID, visar följande kommando skapar en klon av källappen till en ny app när du lägger till dem i en befintlig Traffic Manager-profil:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Aktuella begränsningar
Här följer kända begränsningarna med appkloning:

* Inställningar för automatisk skalning klonas inte
* Inställningarna för Säkerhetskopieringsschemat klonas inte
* VNET-inställningarna klonas inte
* App Insights ställs inte automatiskt in på mål-app
* Enkelt autentiseringsinställningarna klonas inte
* Kudu-tillägget inte klonas
* Tips regler klonas inte
* Databasinnehåll klona inte
* Utgående IP-adresser ändras om Kloningen till en annan skalningsenhet

### <a name="references"></a>Referenser
* [App Service kloning](app-service-web-app-cloning.md)
* [Säkerhetskopiera en app i Azure App Service](manage-backup.md)
* [Azure Resource Manager-support för förhandsversionen av Azure Traffic Manager](../traffic-manager/traffic-manager-powershell-arm.md)
* [Introduktion till App Service-miljöer](environment/intro.md)
* [Använda Azure PowerShell med Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

