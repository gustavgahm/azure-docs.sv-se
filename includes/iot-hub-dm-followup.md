---
title: ta med fil
description: ta med fil
services: iot-hub
ms.service: iot-hub
author: dominicbetts
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 3cdfa74c5f124e57b125399e39b7981b1dfb6d09
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43096360"
---
## <a name="customize-and-extend-the-device-management-actions"></a>Anpassa och utöka enheten hanteringsåtgärder

Dina IoT-lösningar kan utöka definierad uppsättning enhetshanteringsmönster eller Aktivera egna mönster genom att använda enhetstvillingen och moln till enhet metoden primitiver. Andra exempel på åtgärder för hantering av enhet är fabriksåterställning, uppdatering av inbyggd programvara, programuppdateringar, energisparfunktioner, nätverk och hantering och datakryptering.

## <a name="device-maintenance-windows"></a>Underhållsperioder för enhet

Normalt kan du konfigurera enheter för att utföra åtgärder vid en tidpunkt som minimerar avbrott och stilleståndstid. Enheten underhållsperioderna är ett vanligt mönster att definiera den tid när en enhet ska använda för att uppdatera konfigurationen. Backend-lösningar kan använda önskade egenskaper för enhetstvillingen för att definiera och aktivera en princip på enheten som gör att en underhållsperiod. När en enhet tar emot princip för underhåll av fönstret, använda den rapporterad egenskap i enhetstvillingen för att rapportera status för principen. Backend-appen kan sedan använda enhetstvillingsfrågor intyga kompatibiliteten för enheter och varje princip.

## <a name="next-steps"></a>Nästa steg

I den här självstudien använde du en direkt metod för att utlösa en fjärromstart på en enhet. Du används rapporterade egenskaper för att rapportera senast omstart från enheten och frågat enhetstvillingen för att identifiera senast omstart av enheten från molnet.

Om du vill fortsätta att komma igång med IoT Hub och enhetshanteringsmönster som via luften firmware-uppdatering, se:

[Självstudie: Hur du gör en firmware-uppdatering](../articles/iot-hub/tutorial-firmware-update.md)

Läs hur du utökar din IoT-lösning och schema anropar på flera enheter i den [schema och sändningsjobb](../articles/iot-hub/iot-hub-node-node-schedule-jobs.md) självstudien.

Om du vill komma igång med IoT Hub går [komma igång med IoT Edge](../articles/iot-edge/tutorial-simulate-device-linux.md).