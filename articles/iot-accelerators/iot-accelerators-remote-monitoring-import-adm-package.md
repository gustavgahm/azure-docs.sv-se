---
title: Remote Monitoring solution Importera paket ADM - Azure | Microsoft Docs
description: Den här artikeln beskrivs hur du importerar ett hanteringspaket för automatisk enheten till lösningsacceleratorn för fjärrövervakning
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 8fd6e733f3e80ba2a3ec632c088d070252e260cc
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52684994"
---
# <a name="import-an-automatic-device-management-package-into-your-remote-monitoring-solution-accelerator"></a>Importera ett paket för hantering av enheter till lösningsacceleratorn för fjärrövervakning

En automatisk enhetshanteringskonfigurationen definierar konfigurationsändringar för att distribuera till en grupp av enheter. Den här artikeln förutsätter att en utvecklare i din organisation redan har skapat en automatisk enhetshanteringskonfigurationen. Mer information om hur utvecklare skapar en konfiguration, ser du något av följande instruktionsartiklar för IoT Hub:

- [Konfigurera och övervaka IoT-enheter i stor skala med Azure portal](../iot-hub/iot-hub-auto-device-config.md)
- [Konfigurera och övervaka IoT-enheter i stor skala med Azure CLI](../iot-hub/iot-hub-auto-device-config-cli.md)

En utvecklare som skapar och testar en automatisk enhetshanteringskonfigurationen i en utvecklingsmiljö. När du är klar kan importera du konfigurationen till lösningsacceleratorn för fjärrövervakning.

## <a name="export-a-configuration"></a>Exportera en konfiguration

Använd Azure-portalen för att exportera konfigurationen för en automatisk enheten från din utvecklingsmiljö:

1. Navigera till IoT-hubben som du använder för att utveckla och testa dina IoT-enheter i Azure-portalen. Klicka på **IoT enhetskonfiguration**:

    [![Konfiguration för IoT-enhet](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-expanded.png#lightbox)

1. Klicka på den konfiguration som du vill använda. Den **information om enhetens konfiguration** visas:

    [![Konfigurationsdetaljer för IoT-enhet](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-expanded.png#lightbox)
1. Klicka på **ladda ned konfigurationsfilen**:

    [![Ladda ned konfigurationsfilen](./media/iot-accelerators-remote-monitoring-import-adm-package/download-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/download-expanded.png#lightbox)

1. Spara JSON-filen som en lokal fil med namnet **configuration.json**.

Nu har du en fil som innehåller automatisk enhetshanteringskonfigurationen. I nästa avsnitt importerar du den här konfigurationen som ett paket till lösningen för fjärrövervakning.

## <a name="import-a-package"></a>Importera ett paket

Följ stegen nedan för att importera en automatisk enhetshanteringskonfigurationen som ett paket i din lösning:

1. Navigera till den **paket** sidan fjärrövervakning webbläsaren: ![paket sidan](media/iot-accelerators-remote-monitoring-import-adm-package/packagepage.png)

1. Klicka på **+ nytt paket**, Välj **Configuration** som pakettyp och klicka på **Bläddra** att välja den **configuration.json** fil som du sparade i föregående avsnitt:

    ![Välj konfiguration](media/iot-accelerators-remote-monitoring-import-adm-package/uploadpackage.png)

1. Klicka på **överför** att lägga till paketet i lösningen för fjärrövervakning:

    ![Överförda paketet](media/iot-accelerators-remote-monitoring-import-adm-package/uploadedpackage.png)

Nu har du laddat ner en automatisk enhetshanteringskonfigurationen som ett paket. På den **distributioner** kan du distribuera det här paketet till dina anslutna enheter.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur du skapar ett konfigurationspaket och importerar den till från lösningen för fjärrövervakning, nästa steg är att lära dig hur du [hantera enheter som är anslutna till fjärrövervakning gruppvis](iot-accelerators-remote-monitoring-bulk-configuration-update.md).
