---
title: Snabbstart för att skapa en Azure IoT Edge-enhet i Linux | Microsoft Docs
description: I den här snabbstarten får du lära dig att skapa en IoT Edge-enhet och sedan fjärrdistribuera färdig kod från Azure Portal.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/31/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: dcfbc014eaa191c7992a2da195f9bcd10b44194f
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54191492"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Snabbstart: Distribuera din första IoT Edge-modul till en Linux x64-enhet

Azure IoT Edge flyttar kraften i molnet till IoT-enheter. I den här snabbstarten får du lära dig hur du använder molngränssnittet för att fjärrdistribuera färdig kod till en IoT Edge-enhet.

I den här snabbstarten lär du dig att:

1. Skapa en IoT Hub.
2. Registrera en IoT Edge-enhet till din IoT Hub.
3. Installera och starta IoT Edge-körningen på enheten.
4. Fjärrdistribuera en modul till en IoT Edge-enhet.

![Diagram – Snabbstart av arkitektur för enhet och moln](./media/quickstart-linux/install-edge-full.png)

Denna snabbstart gör din Linux-dator eller virtuella dator till en IoT Edge-enhet. Du kan sedan distribuera en modul från Azure Portal till din enhet. Modulen som du distribuerar i den här snabbstarten är en simulerad sensor som genererar temperatur-, fuktighets- och lufttrycksdata. De andra självstudierna i Azure IoT Edge bygger vidare på det arbete som du gör här, genom att distribuera moduler som analyserar simulerade data för verksamhetsinsyn.

Om du inte har en aktiv Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Du kommer använda Azure CLI för att slutföra många av stegen i den här snabbstarten och Azure IoT har ett tillägg för att aktivera ytterligare funktioner. 

Lägg till Azure IoT-tillägget till Cloud Shell-instansen.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Nödvändiga komponenter

Molnresurser: 

* En resursgrupp som du använder för att hantera alla resurser i den här snabbstarten. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

IoT Edge-enhet:

* En Linux-enhet eller en virtuell dator som ska fungera som din IoT Edge-enhet. Om du vill skapa en virtuell dator i Azure använder du följande kommando för att snabbt komma igång:

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```

   Det kan ta några minuter att skapa och starta den nya virtuella datorn. 

   När du skapar en ny virtuell dator noterar du **publicIpAddress**, som anges som en del av utdata från kommandot för att skapa. Du använder den här offentliga IP-adressen för att ansluta till den virtuella datorn senare i snabbstarten.

## <a name="create-an-iot-hub"></a>Skapa en IoT Hub

Starta snabbstarten genom att skapa en IoT-hubb med Azure CLI.

![Diagram – Skapa en IoT-hubb i molnet](./media/quickstart-linux/create-iot-hub.png)

Den kostnadsfria nivån för IoT Hub fungerar för den här snabbstarten. Om du har använt IoT Hub tidigare och redan har skapat en kostnadsfri hubb kan du använda den. Varje prenumeration kan bara ha en kostnadsfri IoT Hub. 

Följande kod skapar en kostnadsfri **F1**-hubb i resursgruppen **IoTEdgeResources**. Ersätt *{hub_name}* med ett unikt namn för din IoT Hub.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Om du får ett felmeddelande eftersom det redan finns en kostnadsfri hubb i din prenumeration ändrar du SKU till **S1**. Om du får ett felmeddelande om att IoT Hub-namnet inte är tillgängligt innebär det att någon annan redan har en hubb med det namnet. Prova med ett nytt namn. 

## <a name="register-an-iot-edge-device"></a>Registrera en IoT Edge-enhet

Registrera en IoT Edge-enhet med IoT-hubben som du nyss skapade.
![Diagram – Registrera en enhet med en IoT Hub-identitet](./media/quickstart-linux/register-device.png)

Skapa en enhetsidentitet för den simulerade enheten så att den kan kommunicera med din IoT Hub. Enhetsidentiteten finns i molnet, och du använder en unik enhetsanslutningssträng för att associera en fysisk enhet med en enhetsidentitet. 

Eftersom IoT Edge-enheter fungerar och kan hanteras på annat sätt än typiska IoT-enheter deklarerar du den här identiteten till att höra till en IoT Edge-enhet med flaggan `--edge-enabled`. 

1. Ange följande kommando i Azure Cloud Shell för att skapa en enhet med namnet **myEdgeDevice** i din hubb.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

   Om du får ett felmeddelande om iothubowner-principnycklar kontrollerar du att CloudShell kör den senaste versionen av tillägget azure-cli-iot-ext. 

2. Hämta anslutningssträngen för din enhet som länkar den fysiska enheten med dess identitet i IoT Hub. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. Kopiera anslutningssträngen från JSON-utdata och spara den. Du behöver det här värdet för att konfigurera IoT Edge-körningen i nästa avsnitt.

   ![Hämta anslutningssträngen från CLI-utdata](./media/quickstart/retrieve-connection-string.png)

## <a name="install-and-start-the-iot-edge-runtime"></a>Installera och starta IoT Edge-körningen

Installera och starta Azure IoT Edge-körningen på din IoT Edge-enhet. 
![Diagram – Starta körningen på enheten](./media/quickstart-linux/start-runtime.png)

IoT Edge-körningen distribueras på alla IoT Edge-enheter. Den har tre komponenter. **IoT Edge säkerhetsdaemon** startas varje gång en Edge-enhet startar. Enheten startas genom att IoT Edge-agenten startas. **IoT Edge-agenten** underlättar distribution och övervakning av moduler på IoT Edge-enheten, inklusive IoT Edge-hubb. **IoT Edge-hubben** hanterar kommunikationen mellan moduler på IoT Edge-enheten, samt mellan enheten och IoT Hub. 

Under körningskonfigurationen anger du en enhetsanslutningssträng. Använd den sträng som du hämtade från Azure CLI. Den här strängen associerar den fysiska enheten med IoT Edge-enhetsidentiteten i Azure. 

### <a name="connect-to-your-iot-edge-device"></a>Ansluta till din IoT Edge-enhet

Alla steg i det här avsnittet sker på din IoT Edge-enhet. Om du använder din egen dator som IoT Edge-enhet så kan du fortsätta till nästa avsnitt. Om du använder en virtuell dator eller sekundär maskinvara bör du ansluta till den datorn nu. 

Om du har skapat en virtuell Azure-dator för den här snabbstarten hämtar du den offentliga IP-adress som var utdata för kommandot för att skapa. Du hittar även den offentliga IP-adressen på den virtuella datorns översiktssida i Azure-portalen. Använd följande kommando för att ansluta till din virtuella dator. Ersätt **{publicIpAddress}** med adressen för din dator. 

```azurecli-interactive
ssh azureuser@{publicIpAddress}
```

### <a name="register-your-device-to-use-the-software-repository"></a>Registrera din enhet för att använda programvarudatabasen

De paket som du behöver för IoT Edge-körningen hanteras i en programvarudatabas. Konfigurera din IoT Edge-enhet för att få åtkomst till den här databasen. 

Stegen i det här avsnittet gäller x64-enheter som kör **Ubuntu 16.04**. För att komma åt programvarudatabasen i andra versioner av Linux eller i enhetsarkitekturer kan du läsa [Installera Azure IoT Edge-körning på Linux (x64)](how-to-install-iot-edge-linux.md) eller [Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md).

1. Installera databaskonfigurationen på den dator du använder som IoT Edge-enhet.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Installera en offentlig nyckel för att komma åt databasen.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Installera en körmiljö för containrar

IoT Edge-körning är en uppsättning containrar, och den logik som du distribuerar till IoT Edge-enheten är paketerad som containrar. Förbered din enhet för dessa komponenter genom att installera en körmiljö för containrar.

1. Uppdatera **apt-get**.

   ```bash
   sudo apt-get update
   ```

2. Installera **Moby**, en körmiljö för containrar.

   ```bash
   sudo apt-get install moby-engine
   ```

3. Installera CLI-kommandona för Moby. 

   ```bash
   sudo apt-get install moby-cli
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>Installera och konfigurera IoT Edge säkerhetsdaemon

Denna säkerhetsdaemon installeras som en systemtjänst så att IoT Edge-körningen startar varje gång enheten startas. Installationen innehåller även en version av **hsmlib** som gör att säkerhetsdaemon kan interagera med enhetens maskinvarusäkerhet. 

1. Ladda ned och installera IoT Edge säkerhetsdaemon. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Öppna konfigurationsfilen för IoT Edge. Filen är skyddad, så du kan behöva använda utökade privilegier för att komma åt den.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Lägg till IoT Edge-enhetens anslutningssträng. Hitta variabeln **device_connection_string** och uppdatera dess värde med den sträng som du kopierade efter att du registrerade enheten. Den här anslutningssträngen associerar den fysiska enheten med enhetsidentiteten som du skapade i Azure.

4. Spara och stäng filen. 

   `CTRL + X`, `Y`, `Enter`

5. Starta om daemon för IoT Edge-säkerhet för att tillämpa ändringarna.

   ```bash
   sudo systemctl restart iotedge
   ```

### <a name="view-the-iot-edge-runtime-status"></a>Visa status för IoT Edge-körningen

Kontrollera att körningen har installerats och konfigurerats korrekt.

>[!TIP]
>Förhöjd behörighet krävs för att köra `iotedge`-kommandon. När du loggar ut från datorn och sedan loggar in igen för första gången efter installationen av IoT Edge-körningen, så uppdateras dina behörigheter automatiskt. Tills dess kan du använda **sudo** framför kommandona. 

1. Kontrollera att Edge-säkerhetsdaemon körs som en systemtjänst.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Kontrollera att Edge Daemon körs som en systemtjänst](./media/quickstart-linux/iotedged-running.png)

2. Hämta tjänstloggar om du behöver felsöka tjänsten. 

   ```bash
   journalctl -u iotedge
   ```

3. Visa de moduler som körs på enheten. 

   ```bash
   sudo iotedge list
   ```

   ![Visa en modul på din enhet](./media/quickstart-linux/iotedge-list-1.png)

Din IoT Edge-enhet har nu konfigurerats. Den är redo att köra molndistribuerade moduler. 

## <a name="deploy-a-module"></a>Distribuera en modul

Hantera din Azure IoT Edge-enhet från molnet för att distribuera en modul som ska skicka telemetridata till IoT Hub.
![Diagram – Distribuera en modul från ett moln till en enhet](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Visa genererade data

I den här snabbstarten skapade du en ny IoT Edge-enhet och installerade IoT Edge-körningsmiljön på den. Sedan använde du Azure-portalen för att distribuera en IoT Edge-modul som ska köras på enheten utan att några ändringar krävs på själva enheten. 

I det här fallet skapar modulen som du distribuerade exempeldata som du kan använda för testning. Modulen för en simulerad temperatursensor genererar miljödata som du kan använda för testning. Den simulerade sensorn övervakar både en dator och miljön runt datorn. Den här sensorn kan till exempel finnas i ett serverrum, på fabriksgolvet eller på en vindturbin. Meddelandet innehåller rumstemperatur och fuktighet, maskintemperatur och tryck samt en tidsstämpel. I IoT Edge-självstudierna används de data som skapas av den här modulen som testdata för analyser.

Öppna kommandotolken på IoT Edge-enheten igen. Kontrollera att modulen distribuerades från molnet som körs på IoT Edge-enheten:

   ```bash
   sudo iotedge list
   ```

   ![Visa tre moduler på enheten](./media/quickstart-linux/iotedge-list-2.png)

Visa de meddelanden som skickas från temperatursensor-modulen:

   ```bash
   sudo iotedge logs SimulatedTemperatureSensor -f
   ```

   >[!TIP]
   >IoT Edge-kommandon är skiftlägeskänsliga när det gäller modulnamnen.

   ![Visa data från modulen](./media/quickstart-linux/iotedge-logs.png)

Du kan även se när meddelandena tas emot av din IoT-hubb med hjälp av [Azure IoT Hub-tillägget för Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (tidigare Azure IoT Toolkit-tillägget). 

## <a name="clean-up-resources"></a>Rensa resurser

Om du vill fortsätta med IoT Edge-självstudierna kan du använda enheten du registrerade och konfigurerade i den här snabbstarten. I annat fall kan du ta bort de Azure-resurser som du skapade och ta bort IoT Edge-körningen från enheten.

### <a name="delete-azure-resources"></a>Ta bort Azure-resurser

Om du skapade den virtuella datorn och IoT-hubben i en ny resursgrupp kan du ta bort den gruppen och alla associerade resurser. Kontrollera att det inte finns något du vill behålla i innehållet i resursgruppen. Om du inte vill ta bort hela gruppen kan du ta bort enskilda resurser i stället.

Ta bort gruppen **IoTEdgeResources**.

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

### <a name="remove-the-iot-edge-runtime"></a>Ta bort IoT Edge-körningen

Om du vill ta bort installationerna från din enhet kan du använda följande kommandon.  

Ta bort IoT Edge-körningen.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Ta bort körmiljön för containern.

   ```bash
   sudo apt-get remove --purge moby-cli
   sudo apt-get remove --purge moby-engine
   ```

## <a name="next-steps"></a>Nästa steg

Den här snabbstarten är en förutsättning för alla andra IoT Edge-självstudier. Du kan fortsätta med någon av de andra självstudierna om du vill lära dig mer om hur Azure IoT Edge kan förvandla dina data till affärsinsikter.

> [!div class="nextstepaction"]
> [Filtrera sensordata med hjälp av en Azure-funktion](tutorial-deploy-function.md)
