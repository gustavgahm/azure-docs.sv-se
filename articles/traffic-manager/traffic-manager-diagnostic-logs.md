---
title: Aktivera Diagnostisk loggning i Azure Traffic Manager
description: Lär dig hur du aktiverar diagnostikloggning för Traffic Manager-profilen och komma åt de loggfiler som skapas som ett resultat.
services: traffic-manager
author: KumudD
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: kumud
ms.openlocfilehash: abdc50d6d3d27ab7611994089345a997afc72cae
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55082534"
---
# <a name="enable-diagnostic-logging-in-azure-traffic-manager"></a>Aktivera Diagnostisk loggning i Azure Traffic Manager

Den här artikeln beskriver hur du aktiverar loggning och åtkomst till loggar med diagnostikdata för en Traffic Manager-profil.

Diagnostikloggar för Azure Traffic Manager kan ge insikter i beteendet för Traffic Manager profilen resursen. Du kan till exempel använda loggen profildata för att avgöra varför enskilda avsökningar tidsgräns har mot en slutpunkt.

## <a name="enable-diagnostic-logging"></a>Aktivera diagnostikloggning

Du kan köra kommandon i den [Azure Cloud Shell](https://shell.azure.com/powershell), eller genom att köra PowerShell från datorn. Azure Cloud Shell är ett interaktivt gränssnitt. Den har vanliga Azure-verktyg förinstallerat och har konfigurerats för användning med ditt konto. Om du kör PowerShell från datorn, måste den *AzureRM* PowerShell-modulen, 6.13.1 eller senare. Du kan köra `Get-Module -ListAvailable AzureRM` att hitta den installerade versionen. Om du behöver installera eller uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/azurerm/install-azurerm-ps) (Installera Azure PowerShell-modul). Om du kör PowerShell lokalt måste du också behöva köra `Login-AzureRmAccount` att logga in på Azure.

1. **Hämta Traffic Manager-profilen:**

    Om du vill aktivera Diagnostisk loggning, behöver du ID för en Traffic Manager-profil. Hämta Traffic Manager-profilen som du vill aktivera Diagnostisk loggning för med [Get-AzureRmTrafficManagerProfile](/powershell/module/AzureRM.TrafficManager/Get-AzureRmTrafficManagerProfile). Utdata innehåller information för Traffic Manager-profil-ID.

    ```azurepowershell-interactive
    Get-AzureRmTrafficManagerProfile -Name <TrafficManagerprofilename> -ResourceGroupName <resourcegroupname>
    ```

2. **Aktivera Diagnostisk loggning för Traffic Manager-profilen:**

    Aktivera Diagnostisk loggning för Traffic Manager-profil med hjälp av ID som hämtades i föregående steg med [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/set-azurermdiagnosticsetting?view=latest). Följande kommando lagrar utförliga loggar för Traffic Manager-profilen till en angiven Azure Storage-konto. 

      ```azurepowershell-interactive
    Set-AzureRmDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId> -StorageAccountId <storageAccountId> -Enabled $true
      ``` 
3. **Kontrollera inställningarna för diagnostik:**

      Kontrollera diagnostikinställningar för Traffic Manager-profil med [Get-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermdiagnosticsetting?view=latest). Följande kommando visar de kategorier som har loggats för en resurs.

     ```azurepowershell-interactive
     Get-AzureRmDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId>
     ```  
      Kontrollera att alla logga kategorier som har associerats med Traffic Manager profilen resurs visas som aktiverat. Kontrollera också att lagringskontot är rätt inställd.

## <a name="access-log-files"></a>Loggfiler för åtkomst
1. Logga in på [Azure Portal](https://portal.azure.com). 
1. Gå till Azure Storage-kontot i portalen.
2. På den **översikt** sidan för ditt Azure storage-konto, under **Services** Välj **Blobar**.
3. För **behållare**väljer **insights-logs-probehealthstatusevents**, och navigera till filen pt1h.JSON och klicka på **hämta** att hämta och spara en kopia av den här loggfilen filen.

    ![Åtkomst till loggfiler för din Traffic Manager-profil från en blob storage](./media/traffic-manager-logs/traffic-manager-logs.png)


## <a name="traffic-manager-log-schema"></a>Schema för Traffic Manager-loggen

Alla diagnostikloggar som är tillgängliga i Azure Monitor delar ett gemensamt översta schema med flexibilitet för varje tjänst att generera unika egenskaper för sina egna händelser. Översta diagnostikloggar schema, se [tjänster, scheman och kategorier som stöds för diagnostikloggar för Azure](../azure-monitor/platform/tutorial-dashboards.md).

I följande tabell innehåller loggarna schema specifika till resursen i Azure Traffic Manager-profil.

|||||
|----|----|---|---|
|**Fältnamn**|**Fälttyp**|**Definition**|**Exempel**|
|EndpointName|Sträng|Namnet på Traffic Manager-slutpunkten vars hälsostatus registreras.|*myPrimaryEndpoint*|
|Status|Sträng|Traffic Manager-slutpunkt som har avlästes hälsostatus. Status kan antingen vara **upp** eller **ned**.|**Upp**|
|||||

## <a name="next-steps"></a>Nästa steg

* Läs mer om [Traffic Manager-övervakning](traffic-manager-monitoring.md)

