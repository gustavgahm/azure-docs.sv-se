---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: b6e0e57881154f5885e9f518363eda3c5b1169a0
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52331501"
---
### <a name="install-via-composer"></a>Installera via Composer
1. Skapa en fil med namnet **composer.json** i roten av projektet och Lägg till följande kod:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage": "*"
      }
    }
    ```
2. Ladda ned **[composer.phar] [ composer-phar]** i projektroten.
3. Öppna en kommandotolk och kör följande kommando i projektroten
   
    ```
    php composer.phar install
    ```

Du kan också gå till den [Azure Storage PHP-klientbibliotek] [ php-sdk-github] på GitHub för att klona källkoden.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
