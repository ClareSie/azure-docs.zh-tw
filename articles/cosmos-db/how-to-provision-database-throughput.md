---
title: 在 Azure Cosmos DB 中佈建資料庫輸送量
description: 瞭解如何使用 Azure 入口網站、CLI、PowerShell 和各種其他 Sdk，在 Azure Cosmos DB 的資料庫層級布建輸送量。
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: 0ba86d6955805fc97743a1a51aa586b179d82235
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2020
ms.locfileid: "82869862"
---
# <a name="provision-throughput-on-a-database-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中佈建資料庫的輸送量

本文說明如何在 Azure Cosmos DB 中佈建資料庫的輸送量。 您可以布建單一[容器](how-to-provision-container-throughput.md)或資料庫的輸送量，並在其中的容器之間共用輸送量。 若要瞭解使用容器層級和資料庫層級輸送量的時機，請參閱在[容器和資料庫上布建輸送量的使用案例](set-throughput.md)一文。 您可以使用 Azure 入口網站或 Azure Cosmos DB SDK 來佈建資料庫層級的輸送量。

## <a name="provision-throughput-using-azure-portal"></a>使用 Azure 入口網站佈建輸送量

### <a name="sql-core-api"></a><a id="portal-sql"></a>SQL (Core) API

1. 登入 [Azure 入口網站](https://portal.azure.com/)。

1. [建立新的 Azure Cosmos 帳戶](create-sql-api-dotnet.md#create-account)，或選取現有 Azure Cosmos 帳戶。

1. 開啟 [資料總管]**** 窗格，然後選取 [新增資料庫]****。 提供下列詳細資料：

   * 輸入資料庫識別碼。
   * 選取 [佈建輸送量]****。
   * 輸入輸送量 (例如 1000 RU)。
   * 選取 [確定]  。

    ![[新增資料庫] 對話方塊的螢幕擷取畫面](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli-or-powershell"></a>使用 Azure CLI 或 PowerShell 布建輸送量

若要建立具有共用輸送量的資料庫，請參閱

* [使用 Azure CLI 建立資料庫](manage-with-cli.md#create-a-database-with-shared-throughput)
* [使用 Powershell 建立資料庫](manage-with-powershell.md#create-db-ru)

## <a name="provision-throughput-using-net-sdk"></a>使用 .NET SDK 佈建輸送量

> [!Note]
> 您可以使用適用於 SQL API 的 Cosmos SDK 為所有 API 佈建輸送量。 您也可以選擇性地對 Cassandra API 使用下列範例。

### <a name="all-apis"></a><a id="dotnet-all"></a>所有 API

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 500
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

---

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra API

類似的命令可以透過任何 CQL 相容的驅動程式執行。

```csharp
// Create a Cassandra keyspace and provision throughput of 400 RU/s
session.Execute("CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=400");
```
 
## <a name="next-steps"></a>後續步驟

請參閱下列文章，了解如何 Azure Cosmos DB 中佈建的輸送量：

* [全域調整佈建的輸送量](scaling-throughput.md)
* [在容器和資料庫中佈建輸送量](set-throughput.md)
* [如何佈建容器的輸送量](how-to-provision-container-throughput.md)
* [Azure Cosmos DB 中的要求單位和輸送量](request-units.md)
