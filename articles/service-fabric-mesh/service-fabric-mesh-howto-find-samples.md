---
title: Hitta Azure Service Fabric Mesh-exempel | Microsoft Docs
description: Lär dig hur du hittar olika Service Fabric Mesh-exempelprogram.
services: service-fabric-mesh
keywords: ''
author: v-vasuke
ms.author: v-vasuke
ms.date: 12/03/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: 6dac55e69761067cf9700e10f449af27ca659f20
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000333"
---
# <a name="find-service-fabric-mesh-samples"></a>Hitta Service Fabric Mesh-exempel

I den här tabellen beskrivs tillgängliga Service Fabric Mesh-exempelprogram. Källkoden i de här exemplen visar hur du uppnår ett visst scenario med hjälp av Service Fabric-resursmodellen.

Mer information om hur du distribuerar mallar direkt till Azure finns på [Github-sidan med exempelmallar](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/README.md).


|Exempelmall|Scenariobeskrivning|Källkod|Utvecklarverktyg|
|------------|--------------------|----------|----------------------|
| [Hello World-app](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/helloworld) | Statisk webbsida i en container. För Linux används nginx, för Windows används IIS | [Källkod](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) | Inga krav |
| [Räknarapp för Azure-filvolymer](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter) | Lagra tillstånd genom att montera Azure Files-baserad volym i containern. <br><br> **Obs!** Den här mallen kräver att en Azure Files-filresurs redan har etablerats [Instruktioner](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share) | [Källkod](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Visual Studio Mesh-verktyg |
| [TodoListApp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/todolist) | Skapa ett program med en klientdels- och serverdelstjänst som använder DNS-baserad matchning. Används som en självstudie [här](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-tutorial-create-dotnetcore) | [Källkod](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/todolistapp) | Visual Studio Mesh-verktyg |
| [Visual Objects-app](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/visualobjects) | Skala och uppgradera mikrotjänster i ett program. | [Källkod](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) |  Visual Studio Mesh-verktyg |
| [Röstningsapp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/voting) | Skapa ett program med en klientdels- och serverdelstjänst som använder DNS-baserad matchning | [Källkod](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) | Visual Studio Mesh-verktyg för Windows-versionen, VS Code/dotnet cli kan användas för Linux-versionen |
| [Räknarapp för Service Fabric Reliable Disk-volymer](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter)| Lagra tillstånd genom att montera Service Fabric Reliable Disk-baserad volym i containern.| [Källkod](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Visual Studio Mesh-verktyg |
