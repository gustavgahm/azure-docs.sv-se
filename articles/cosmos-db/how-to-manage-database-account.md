---
title: Lär dig hur du hanterar databaskonton i Azure Cosmos DB
description: Lär dig hur du hanterar databaskonton i Azure Cosmos DB
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: d3ee6db382f848a029f3e0355abdfe5eecc80656
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54035839"
---
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>Hantera databaskonton i Azure Cosmos DB

Den här artikeln beskriver hur du hanterar Azure Cosmos DB-kontot. Du lär dig att konfigurera flera värdar, lägga till eller ta bort en region, konfigurera flera skrivregioner samt konfigurera redundansprioriteringar. 

## <a name="create-a-database-account"></a>Skapa ett databaskonto

### <a id="create-database-account-via-portal"></a>Azure-portalen

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure CLI

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>Konfigurera klienter för flera värdar

### <a id="configure-clients-multi-homing-dotnet"></a>.NET SDK

```csharp
// Create a new connection policy.
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults.
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to preferred locations.
// The name of the location will match what you see in the portal, etc.
policy.PreferredLocations.Add("East US");
policy.PreferredLocations.Add("North Europe");

// Pass the connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-java-async"></a>Java Async SDK

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setPreferredLocations(Collections.singleton("West US"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-java-sync"></a>Java Sync SDK

```java
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
Collection<String> preferredLocations = new ArrayList<String>();
preferredLocations.add("Australia East");
connectionPolicy.setPreferredLocations(preferredLocations);
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy);
```

### <a id="configure-clients-multi-homing-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Set up the connection policy with your preferred regions.
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["West US", "Australia East"];

// Pass that connection policy to the client.
const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy
});
```

### <a id="configure-clients-multi-homing-python"></a>Python SDK

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.PreferredLocations = ['West US', 'Japan West']
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy)

```

## <a name="addremove-regions-from-your-database-account"></a>Lägga till/ta bort regioner från ditt databaskonto

### <a id="add-remove-regions-via-portal"></a>Azure-portalen

1. Gå till ditt Azure Cosmos DB-konto och öppna menyn **Replikera data globalt**.

2. För att lägga till regioner markerar du sexhörningarna på kartan med den **+**-etikett som motsvarar din önskade region. Lägg till en region genom att välja alternativet **+ Lägg till region** och välja en region i den nedrullningsbara menyn.

3. Om du vill ta bort regioner avmarkerar du en eller flera regioner från kartan genom att välja de blå sexhörningarna med kryssmarkeringar. Eller välj ”papperskorgsikonen” (🗑) intill regionen på höger sida.

4. Spara ändringarna genom att välja **OK**.

   ![Lägga till eller ta bort regionsmenyn](./media/how-to-manage-database-account/add-region.png)

I skrivläge för en enskild region kan du inte ta bort skrivregionen. Du måste redundansväxla till en annan region innan du kan ta bort den aktuella skrivregionen.

I skrivläge för flera regioner kan du lägga till eller ta bort vilka regioner som helst förutsatt att du har minst en region.

### <a id="add-remove-regions-via-cli"></a>Azure CLI

```bash
# Given an account created with 1 region like so
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0'

# Add a new region by adding another region to the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0 westus=1'

# Remove a region by removing a region from the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'westus=0'
```

## <a name="configure-multiple-write-regions"></a>Konfigurera flera skrivregioner

### <a id="configure-multiple-write-regions-portal"></a>Azure-portalen

Se till att inställningen **Multi-region Writes** (Skrivning till flera regioner) är aktiverad när du skapar ett databaskonto.

![Skärmbild av skapande av Azure Cosmos-konto](./media/how-to-manage-database-account/account-create.png)

### <a id="configure-multiple-write-regions-cli"></a>Azure CLI

```bash
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-arm"></a>Resource Manager-mall

Följande JSON-kod är ett exempel på en Azure Resource Manager-mall. Du kan använda den för att distribuera ett Azure Cosmos DB-konto med en konsekvensprincip för begränsad föråldring. Det maximala föråldringsintervallet är inställt på 5 sekunder. Det maximala antalet föråldrade begäranden som godtas är inställt på 100. Du kan läsa om Resource Manager-mallformatet och syntaxen i [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "BoundedStaleness",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```


## <a id="manual-failover"></a>Aktivera manuell redundans för ditt Azure Cosmos DB-konto

### <a id="enable-manual-failover-via-portal"></a>Azure-portalen

1. Gå till ditt Azure Cosmos DB-konto och öppna menyn **Replikera data globalt**.

2. Längst upp på menyn väljer du **Manuell redundans**.

   ![Menyn Replikera data globalt](./media/how-to-manage-database-account/replicate-data-globally.png)

3. På menyn **Manuell redundans** väljer du din nya skrivregion. Markera kryssrutan för att bekräfta att du förstår att det här alternativet ändrar din skrivregion.

4. Utlös redundansväxlingen genom att välja **OK**.

   ![Menyn Manuell redundans i portalen](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```bash
# Given your account currently has regions with priority like so: 'eastus=0 westus=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=1 westus=0'
```

## <a id="automatic-failover"></a>Aktivera automatisk redundans för ditt Azure Cosmos DB-konto

### <a id="enable-automatic-failover-via-portal"></a>Azure-portalen

1. Öppna fönsterrutan **Replikera data globalt** i ditt Azure Cosmos DB-konto. 

2. Längst upp i fönsterrutan väljer du **Automatisk redundans**.

   ![Menyn Replikera data globalt](./media/how-to-manage-database-account/replicate-data-globally.png)

3. I fönsterrutan **Automatisk redundans** ser du till att **Aktivera automatisk redundans** är inställt på **PÅ**. 

4. Välj **Spara**.

   ![Menyn Automatisk redundans i portalen](./media/how-to-manage-database-account/automatic-failover.png)

Du kan även ange redundansprioritet på den här menyn.

### <a id="enable-automatic-failover-via-cli"></a>Azure CLI

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-azure-cosmos-db-account"></a>Ange redundansprioritet för ditt Azure Cosmos DB-konto

### <a id="set-failover-priorities-via-portal"></a>Azure-portalen

1. Öppna fönsterrutan **Replikera data globalt** i ditt Azure Cosmos DB-konto. 

2. Längst upp i fönsterrutan väljer du **Automatisk redundans**.

   ![Menyn Replikera data globalt](./media/how-to-manage-database-account/replicate-data-globally.png)

3. I fönsterrutan **Automatisk redundans** ser du till att **Aktivera automatisk redundans** är inställt på **PÅ**. 

4. Du ändrar redundansprioritet genom att dra läsregionerna via de tre punkterna till vänster om raden som visas när du hovrar över dem. 

5. Välj **Spara**.

   ![Menyn Automatisk redundans i portalen](./media/how-to-manage-database-account/automatic-failover.png)

Du kan inte ändra skrivregionen på den här menyn. Du måste göra en manuell redundans för att ändra skrivregionen manuellt.

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```bash
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies 'eastus=0 westus=2 southcentralus=1'
```

## <a name="next-steps"></a>Nästa steg

Läs mer om hur du hanterar konsekvensnivåer och datakonflikter i Azure Cosmos DB. Se följande artiklar:

* [Hantera konsekvens](how-to-manage-consistency.md)
* [Hantera konflikter mellan regioner](how-to-manage-conflicts.md)

