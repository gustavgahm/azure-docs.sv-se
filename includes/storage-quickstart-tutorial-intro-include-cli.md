---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: c5cb1c5a005265950f8dcd02c8e7675918751dab
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165931"
---
## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en Azure-resursgrupp med kommandot [az group create](/cli/azure/group#az_group_create). En resursgrupp är en logisk container där Azure-resurser distribueras och hanteras.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

Skapa ett allmänt lagringskonto med kommandot [az storage account create](/cli/azure/storage/account#create). Det allmänna lagringskontot kan användas för alla fyra tjänsterna: blobar, filer, tabeller och köer. 

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>Ange autentiseringsuppgifterna för lagringskontot

Azure CLI behöver dina autentiseringsuppgifter för lagringskontot för de flesta kommandona i den här självstudien. Det finns flera alternativ för att göra det, och ett av de enklaste sätten att ange dem är att ställa in miljövariablerna `AZURE_STORAGE_ACCOUNT` och `AZURE_STORAGE_ACCESS_KEY`.

Visa först dina lagringskontonycklar med kommandot [az storage account keys list](/cli/azure/storage/account/keys#list):

```azurecli-interactive
az storage account keys list \
    --account-name mystorageaccount \
    --resource-group myResourceGroup \
    --output table
```

Ange nu miljövariablerna `AZURE_STORAGE_ACCOUNT` och `AZURE_STORAGE_ACCESS_KEY`. Det kan du göra med Bash-gränssnittet med kommandot `export`:

```bash
export AZURE_STORAGE_ACCOUNT="mystorageaccountname"
export AZURE_STORAGE_ACCESS_KEY="myStorageAccountKey"
```