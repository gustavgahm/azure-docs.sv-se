---
title: Använda Azure Event Grid till att automatisera ändra storlek på uppladdade bilder | Microsoft Docs
description: Azure Event Grid kan utlösas vid blob-överföringar i Azure Storage. Du kan använda det här till att skicka bildfiler som laddats upp till Azure Storage till andra tjänster, som Azure Functions, för storleksändring och andra förbättringar.
services: event-grid, functions
author: spelluru
manager: jpconnoc
editor: ''
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/19/2019
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: 4a7e6189914728fac24e51f3b2dee66cc0bd8a05
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54463719"
---
# <a name="tutorial-automate-resizing-uploaded-images-using-event-grid"></a>Självstudie: Automatisera storleksändring av överförda bilder med Event Grid

[Azure Event Grid](overview.md) är en händelsetjänst för molnet. Med Event Grid kan du skapa prenumerationer på händelser som genereras av Azure-tjänster eller resurser från tredje part.  

Den här självstudien är del två i en serie med Storage-självstudier. Den bygger vidare på [föregående Storage-självstudie][previous-tutorial] och lägger till serverfri och automatisk generering av miniatyrbilder med Azure Event Grid och Azure Functions. Event Grid gör att [Azure Functions](../azure-functions/functions-overview.md) kan svara på [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)-händelser och generera miniatyrbilder av bilder som laddats upp. En händelseprenumeration skapas mot skapandehändelsen i Blob Storage. När en blob läggs till i en viss Blob-lagringscontainer anropas en funktionsslutpunkt. Data som skickas till funktionsbindningen från Event Grid används till att få åtkomst till bloben och generera miniatyrbilden.

Du kan använda Azure CLI och Azure-portalen till att lägga till funktionen för storleksändring i en befintlig app för uppladdning av bilder.

![Publicerad webbapp i webbläsaren Microsoft Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Skapa ett allmänt Azure Storage-konto
> * Distribuera serverfri kod med Azure Functions
> * Skapa en prenumeration på en Blob Storage-händelse i Event Grid

## <a name="prerequisites"></a>Nödvändiga komponenter

För att slutföra den här självstudien behöver du:

Du måste ha slutfört den föregående Blob Storage-självstudien: [Ladda upp avbildningsdata i molnet med Azure Storage][previous-tutorial].

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Om du inte redan har registrerat resursprovidern Event Grid i din prenumeration kontrollerar du att den är registrerad.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt måste du ha Azure CLI version 2.0.14 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI]( /cli/azure/install-azure-cli). 

Om du inte använder Cloud Shell måste du först logga in med `az login`.

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure Storage-konto

För Azure Functions krävs ett allmänt lagringskonto. Skapa ett separat allmänt lagringskonto i resursgruppen med hjälp av kommandot [az storage account create](/cli/azure/storage/account#az-storage-account-create).

Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. 

I följande kommando infogar du ditt globalt unika lagringskontonamn på det allmänna lagringskontot istället för platshållaren `<general_storage_account>`. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Skapa en funktionsapp  

Du måste ha en funktionsapp som värd för körning av funktionen. Funktionsappen är en miljö för serverfri körning av funktionskoden. Skapa en funktionsapp med kommandot [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

I följande kommando infogar du ditt unika funktionsappnamn istället för platshållaren `<function_app>`. Funktionsappens namn används som DNS-standarddomän för funktionsappen. Därför måste namnet vara unikt bland alla appar i Azure. För `<general_storage_account>` ersätter du namnet på det allmänna lagringskonto du skapade.

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Nu måste du konfigurera funktionsappen så att den ansluts till Blob Storage-kontot du skapade i [föregående självstudie][previous-tutorial].

## <a name="configure-the-function-app"></a>Konfigurera funktionsappen

Funktionen behöver anslutningssträngen för att ansluta till bloblagringskontot. Funktionskoden som du distribuerar till Azure i följande steg letar efter anslutningssträngen i appinställningen myblobstorage_STORAGE, och den letar efter miniatyrbildens behållarnamn i appinställningen myContainerName. Visa anslutningssträngen med kommandot [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Ange programinställningar med kommandot [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set).

I följande CLI-kommandon är `<blob_storage_account>` namnet på det bloblagringskonto du skapade i föregående självstudie.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbnails FUNCTIONS_EXTENSION_VERSION=~2
```

Inställningen `FUNCTIONS_EXTENSION_VERSION=~2` får funktionsappen att köra version 2.x av Azure Functions-körningen.

Nu kan du distribuera ett funktionskodprojekt till den här funktionsappen.

## <a name="deploy-the-function-code"></a>Distribuera funktionskoden 

# <a name="nettabdotnet"></a>[\..NET](#tab/dotnet)

Storleksändringen för C#-exempelskriptet (.csx) är tillgängligt på [GitHub](https://github.com/Azure-Samples/function-image-upload-resize). Distribuera funktionskodprojektet till funktionsappen med kommandot [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config). 

I följande kommando är `<function_app>` namnet på funktionsappen som du skapade tidigare.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
Storleksändringsfunktionen för exemplet Node.js är tillgängligt på [GitHub](https://github.com/Azure-Samples/storage-blob-resize-function-node). Distribuera funktionskodprojektet till funktionsappen med kommandot [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config).

I följande kommando är `<function_app>` namnet på funktionsappen som du skapade tidigare.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-resize-function-node
```
---

Funktionen för att ändra bildstorlek utlöses av HTTP-förfrågningar som skickats till den från tjänsten Event Grid. Du kan ange för Event Grid att du vill hämta meddelandena via funktionens URL genom att skapa en händelseprenumeration. För den här självstudien prenumererar du på blob-skapade händelser.

Data som skickas till funktionen från Event Grid-meddelandet inkluderar blobens URL. URL:en skickas sedan till indatabindningen för att hämta den uppladdade avbildningen från Blob Storage. Funktionen genererar en miniatyrbild och skriver den resulterande dataströmmen till en separat container i Blob Storage. 

I det här projektet används `EventGridTrigger` som typ av utlösare. Det är bättre att använda Event Grid-utlösaren än någon allmän HTTP-utlösare. Event Grid verifierar automatiskt Event Grid Function-utlösare. Med allmänna HTTP-utlösare måste du implementera [verifieringssvaret](security-authentication.md#webhook-event-delivery).

Mer information om den här funktionen finns i [filerna function.json och run.csx](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
Funktionsprojektkoden distribueras direkt från den offentliga exempeldatabasen. Mer information om distributionsalternativen för Azure Functions finns i [Kontinuerlig distribution för Azure Functions](../azure-functions/functions-continuous-deployment.md).

## <a name="create-an-event-subscription"></a>Skapa en händelseprenumeration

En händelseprenumeration anger vilka provider-genererade händelser du vill skicka till en viss slutpunkt. I det här fallet exponeras slutpunkten av din funktion. Använd följande steg till att skapa en händelseprenumeration som skickar meddelanden till din funktion i Azure-portalen: 

1. Öppna [Azure-portalen](https://portal.azure.com) och klicka på pilen längst ned till vänster för att expandera alla tjänster, skriv *functions* (funktioner) i fältet **Filter** och välj sedan **Funktionsappar**. 

    ![Bläddra till Funktionsappar i Azure-portalen](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Expandera din funktionsapp, välj funktionen **imageresizerfunc** och välj sedan **Lägg till Event Grid-prenumeration**.

    ![Bläddra till Funktionsappar i Azure-portalen](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Använd de inställningar för händelseprenumerationen som anges i tabellen.
    
    ![Skapa händelseprenumeration från funktionen i Azure-portalen](./media/resize-images-on-storage-blob-upload-event/event-subscription-create.png)

    | Inställning      | Föreslaget värde  | Beskrivning                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Typ av ämne** |  Lagringskonton | Välj händelseprovidern för Storage-kontot. | 
    | **Prenumeration** | Din Azure-prenumeration | Som standard välj den aktuella Azure-prenumerationen.   |
    | **Resursgrupp** | myResourceGroup | Välj **Använd befintlig** och välj den resursgrupp du har använt i den här självstudien.  |
    | **Resurs** |  Ditt Blob Storage-konto |  Välj det Blob Storage-konto du skapade. |
    | **Händelsetyper** | Blob skapas | Avmarkera alla typer utom **Blob skapas**. Det är bara händelsetyper för `Microsoft.Storage.BlobCreated` som skickas till funktionen.| 
    | **Typ av prenumerant** |  genereras automatiskt |  Fördefinieras som Web Hook. |
    | **Slutpunkt för prenumerant** | genereras automatiskt | Använd den slutpunktsadress som genereras åt dig. | 
    | **Namn** | imageresizersub | Namn som identifierar din nya händelseprenumeration. | 
4. *Valfritt:* Om du vill skapa ytterligare containrar i samma bloblagring för andra ändamål i framtiden kan du använda **funktionerna för ämnesfiltrering** på fliken **Filter**. Då får du mer detaljerad styrning för blobhändelser, och kan se till att din funktionsapp endast anropas när blobar läggs till i **avbildningscontainern**. 
5. Klicka på **Skapa** för att lägga till händelseprenumerationen. Då skapas en händelseprenumeration som utlöser `imageresizerfunc` när en blob läggs till i containern *images*. Funktionen återställer bildstorleken och lägger till dem i containern med *miniatyrer*.

Nu när tjänsterna på serversidan har konfigurerats ska du testa funktionen för storleksändring i exempelwebbappen. 

## <a name="test-the-sample-app"></a>Testa exempelappen

När du ska testa storleksändring i webbappen bläddrar du till URL-adressen för din publicerade app. Standardwebbadressen för webbappen är `https://<web_app>.azurewebsites.net`.

Klicka på regionen **Upload photos** (Ladda upp foton) för att välja och ladda upp en fil. Du kan också dra ett foto till den här regionen. 

Observera att en kopia av uppladdade bilden visas i karusellen **Generated thumbnails** (Genererade miniatyrer) när den uppladdade bilden försvinner. Den här bildens storlek ändrades av funktionen. Därefter lades den till i containern med *miniatyrer* och laddades ned av webbklienten.

![Publicerad webbapp i webbläsaren Microsoft Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa ett allmänt Azure Storage-konto
> * Distribuera serverfri kod med Azure Functions
> * Skapa en prenumeration på en Blob Storage-händelse i Event Grid

Gå vidare till del tre i Storage-självstudien om du vill lära dig om säker åtkomst till lagringskontot.

> [!div class="nextstepaction"]
> [Säker åtkomst till programdata i molnet](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

+ Mer information om Event Grid finns i [En introduktion till Azure Event Grid](overview.md). 
+ Om du vill prova en annan självstudie om Azure Functions kan du läsa [Skapa en funktion som kan integreras med Azure Logic Apps](../azure-functions/functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md
