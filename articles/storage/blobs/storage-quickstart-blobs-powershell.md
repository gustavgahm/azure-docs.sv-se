---
title: Azure snabbstart – Skapa en blob i objektlagring med hjälp av PowerShell | Microsoft Docs
description: I den här snabbstarten använder du Azure PowerShell i objektlagring (Blob). Sedan använder du PowerShell och laddar upp en blob till Azure Storage, laddar ned en blob och listar blobarna i en container.
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: rogarana
ms.openlocfilehash: f85d404df37d34f7363114fbbf34ceec3bbe7c0f
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042809"
---
# <a name="quickstart-upload-download-and-list-blobs-by-using-azure-powershell"></a>Snabbstart: Ladda upp, ladda ned och lista blobar med Azure PowerShell

Använd Azure PowerShell-modulen för att skapa och hantera Azure-resurser. Du kan skapa och hantera Azure-resurser via PowerShell-kommandoraden eller i skript. I den här guiden beskrivs hur du använder PowerShell för att överföra filer mellan en lokal disk och Azure Blob Storage.

## <a name="prerequisites"></a>Nödvändiga komponenter

Du behöver en Azure-prenumeration för att få åtkomst till Azure Storage. Om du inte redan har en prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Den här snabbstarten kräver Azure PowerShell-modulen Az version 0.7 eller senare. Kör `Get-Module -ListAvailable Az` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/install-Az-ps) (Installera Azure PowerShell-modul).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Skapa en container

Blobar laddas alltid upp till en container. Du kan ordna grupper av blobar på samma sätt som du ordnar filer i mappar på datorn.

Ange containernamnet och skapa sedan containern med hjälp av [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer). Ange behörigheten för `blob` att tillåta offentlig åtkomst av filerna. Containerns namn i det här exemplet är *quickstartblobs*.

```powershell
$containerName = "quickstartblobs"
new-azurestoragecontainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>Ladda upp blobar i containern

Blob Storage stöder blockblobar, tilläggsblobar och sidblobar. VHD-filer som stöder virtuella IaaS-datorer är sidblobar. Använd tilläggsblobar för loggning, till exempel när du vill skriva till en fil och sedan fortsätta att lägga till mer information. De flesta filer som lagras i Blob Storage är blockblobar. 

Om du vill ladda upp en fil till en blockblob ska du hämta en referens för containern och sedan hämta en referens för blockbloben i den containern. När du har blobreferensen kan du ladda upp data till den med hjälp av [set-azurestorageblobcontent](/powershell/module/azure.storage/set-azurestorageblobcontent). Den här åtgärden skapar bloben om den inte finns eller skriver över bloben om den finns.

I följande exempel laddas *Image001.jpg* och *Image002.png* upp från mappen *D:\\_TestImages* på den lokala disken till containern som du skapade.

```powershell
# upload a file
set-azurestorageblobcontent -File "D:\_TestImages\Image001.jpg" `
  -Container $containerName `
  -Blob "Image001.jpg" `
  -Context $ctx 

# upload another file
set-azurestorageblobcontent -File "D:\_TestImages\Image002.png" `
  -Container $containerName `
  -Blob "Image002.png" `
  -Context $ctx
```

Ladda upp så många filer som du vill innan du fortsätter.

## <a name="list-the-blobs-in-a-container"></a>Visa en lista över blobarna i en container

Hämta en lista över blobar i containern med hjälp av [get-azurestorageblob](/powershell/module/azure.storage/get-azurestorageblob). Det här exemplet visar bara namnen på de blobar som har laddats upp.

```powershell
get-azurestorageblob -Container $ContainerName -Context $ctx | select Name
```

## <a name="download-blobs"></a>Ladda ned blobbar

Ladda ned blobarna till den lokala disken. För varje blob du vill ladda ned anger du namnet och anropar [get-azurestorageblobcontent](/powershell/module/azure.storage/get-azurestorageblobcontent).

I det här exemplet laddas blobarna ned till *D:\\_TestImages\Downloads* på den lokala disken. 

```powershell
# download first blob
get-azurestorageblobcontent -Blob "Image001.jpg" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 

# download another blob
get-azurestorageblobcontent -Blob "Image002.png" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx
```

## <a name="data-transfer-with-azcopy"></a>Dataöverföring med AzCopy

Verktyget [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) är ett annat alternativ för högpresterande, skriptbar dataöverföring för Azure Storage. Använd AzCopy för att överföra data till och från Blob, File och Table Storage.

Som ett enkelt exempel ser du här AzCopy-kommandot för att ladda upp en fil med namnet *myfile.txt* till containern *mystoragecontainer* inifrån ett PowerShell-fönster.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Rensa resurser

Ta bort alla resurser som du har skapat. Det enklaste sättet att ta bort tillgångarna är för att ta bort resursgruppen. Om du tar bort resursgruppen tas även alla resurser bort som ingår i gruppen. När du i följande exempel tar bort resursgruppen tas lagringskontot och själva resursgruppen bort.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du överfört filer mellan en lokal disk och Azure Blob Storage. Lär dig mer om att arbeta med Blob-lagring med hjälp av PowerShell genom att fortsätta till avsnittet om att använda Azure PowerShell med Azure Storage.

> [!div class="nextstepaction"]
> [Använd Azure PowerShell med Azure Storage](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Referens för Microsoft Azure PowerShell Storage-cmdletar

* [Storage PowerShell cmdletar](/powershell/module/az.storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer

* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) är en kostnadsfri, fristående app från Microsoft som gör det möjligt att arbeta visuellt med Azure Storage-data i Windows, macOS och Linux.
