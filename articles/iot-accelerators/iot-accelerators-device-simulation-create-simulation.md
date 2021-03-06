---
title: Skapa en ny IoT-simulering – Azure | Microsoft Docs
description: I den här självstudien använder du enhetssimulering för att skapa och köra en ny simulering.
author: troyhopwood
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: tutorial
ms.custom: mvc
ms.date: 10/25/2018
ms.author: troyhop
ms.openlocfilehash: 268efa35f63ac8b9e0c6ea3f22a71242a2984726
ms.sourcegitcommit: 3dcb1a3993e51963954194ba2a5e42260d0be258
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50756847"
---
# <a name="tutorial-create-and-run-an-iot-device-simulation"></a>Självstudie: Skapa och köra en IoT-enhetssimulering

I den här självstudien använder du enhetssimulering för att skapa och köra en ny IoT-simulering med hjälp av en eller flera simulerade enheter.

I den här kursen får du göra följande:

>[!div class="checklist"]
> * Visa alla aktiva och historiska simuleringar
> * Skapa och köra en ny simulering
> * Visa mått för en simulering
> * Stoppa en simulering

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="prerequisites"></a>Nödvändiga komponenter

För att följa den här självstudien behöver du en distribuerad instans av enhetssimuleringen i din Azure-prenumeration.

Om du inte har distribuerat enhetssimulering ännu bör du slutföra snabbstarten [Distribuera och köra en IoT-enhetssimulering i Azure](quickstart-device-simulation-deploy.md).

## <a name="open-device-simulation"></a>Öppna enhetssimulering

För att köra enhetssimulering i webbläsaren navigerar du först till [Microsoft Azure IoT-lösningsacceleratorer](https://www.azureiotsolutions.com). 

Du kan bli ombedd att logga in med dina autentiseringsuppgifter för Azure-prenumeration.

Klicka sedan på **Starta** på den panel för enhetssimulering som du distribuerade i [snabbstarten](quickstart-device-simulation-deploy.md).

## <a name="view-simulations"></a>Visa simuleringar

Välj **Simuleringar** på menyraden. Sidan **Simuleringar** visar information om alla tillgängliga simuleringar. Från den här sidan kan du se simuleringar som för närvarande körs och dem som kördes tidigare. Om du vill köra en tidigare simulering igen klickar du på simuleringspanelen så att simuleringsinformationen öppnas:

![Simuleringar](media/iot-accelerators-device-simulation-create-simulation/dashboard.png)

## <a name="create-a-simulation"></a>Skapa en simulering

Skapa en simulering genom att klicka på **+ Ny simulering** i det övre högra hörnet.

En simulering består av en eller flera enhetsmodeller. Enhetsmodellen definierar beteendet, telemetrin och meddelandeformatet för den enhet som ska simuleras.

Lägg till en enhetsmodell genom att klicka på **+ Lägg till en enhetstyp** och välja enhetsmodellen i listan. Simuleringen kan ha fler än en enhetsmodell. Varje enhetsmodell kan ha olika antal enheter och meddelandefrekvens.

När formuläret för den nya simuleringen är klart klickar du på **Starta simulering** för att starta simuleringen.

Beroende på antalet simulerade enheter kan det ta några minuter för simuleringen att konfigureras och startas:

![Ny simulering](media/iot-accelerators-device-simulation-create-simulation/newsimulation.png)

## <a name="stop-a-simulation"></a>Stoppa en simulering

När du klickar på **Starta simulering** visas sidan med simuleringsinformation. Den här sidan visar simuleringsstatistik och -mått live från IoT Hub. Du kan även navigera till den här sidan genom att klicka på simuleringspanelen på sidan **Simuleringar**.

Om du vill stoppa en simulering klickar du på **Stoppa simulering** i åtgärdsfältet längst uppe till höger.

![Stoppa simulering](media/iot-accelerators-device-simulation-create-simulation/simulationdetails.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudien lärde du dig att skapa, köra och stoppa en simulering. Du lärde dig även hur du visar simuleringsinformation. Om du vill veta mer om hur du kör simuleringar kan du fortsätta till nästa självstudie:

> [!div class="nextstepaction"]
> [Skapa en anpassad simulerad enhet](iot-accelerators-device-simulation-create-custom-device.md)
