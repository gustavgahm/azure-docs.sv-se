---
title: Spåra användning av ett labb i Azure Lab Services | Microsoft Docs
description: I den här självstudien spårar du som labbskapare/ägare användningen av ditt labb.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 93d7a6e884cf02fa41838d4a07644c122a43823b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823835"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Självstudier: Spåra användning av ett labb i Azure Lab Services
Den här kursen visar hur en labbskapare/ägare kan spåra användningen av ett labb.

I de här självstudierna gör du följande:

> [!div class="checklist"]
> * Visa användare som har registrerats med ditt labb
> * Visa användningen av virtuella datorer i labbet
> * Hantera virtuella studentdatorer 


## <a name="view-users-registered-with-the-lab"></a>Visa användare som har registrerats med labbet

1. Gå till [webbplatsen för Azure Lab Services](https://labs.azure.com). 
2. Välj **Logga in** och ange dina autentiseringsuppgifter. Azure Lab Services har stöd för organisationskonton och Microsoft-konton.
3. På sidan **My labs** (Mina labb) väljer du det labb som du vill spåra användningen för. 
4. Välj **Användare** på den vänstra menyn eller i panelen **Användare**. Du kan se studenter som har registrerats med ditt labb. Välj **Registreringslänk**, kopiera länken och skicka den till nya studenter som ännu inte har registrerats med ditt labb. 

    ![Registrerade användare](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Visa användningen av virtuella datorer i labbet 

1. Välj **Virtuella datorer** på menyn till vänster. 
2. Kontrollera att du ser status för virtuella datorer och det antal timmar som de virtuella datorerna har körts. Den tid som en labbägare tillbringar på en virtuell studentdator räknas inte mot den användningstid som visas i den sista kolumnen. 

    ![VM-användning](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Hantera virtuella studentdatorer 
När du hovrar musen över en rad i listan med virtuella datorer ser du kontroller för att utföra följande uppgifter (enligt bilden i föregående avsnitt): 

- Ansluta till en virtuell dator
- Starta en virtuell dator
- Stoppa en virtuell dator
- Ta bort en virtuell dator

Du kan även använda verktygsfältsknappar för att starta, stoppa eller ta bort en virtuell dator. 



## <a name="next-steps"></a>Nästa steg
Mer information om klassrumslabb finns i artiklarna under [Instruktionsguider](how-to-manage-lab-accounts.md).
