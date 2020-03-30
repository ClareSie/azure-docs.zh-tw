---
title: 使用 Azure 資料工廠從 ODBC 源複製資料
description: 了解如何使用 Azure Data Factory 管線中的複製活動，將資料從 OData 來源複製到支援的接收資料存放區。
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: jingwang
ms.openlocfilehash: 6513cfc5432e969fc53aa72b075af194a064d178
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244364"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>使用 Azure Data Factory 從 ODBC 資料存放區複製資料及將資料複製到處
> [!div class="op_single_selector" title1="選取您目前使用的 Data Factory 服務版本："]
> * [版本 1](v1/data-factory-odbc-connector.md)
> * [當前版本](connector-odbc.md)

本文概述如何使用 Azure Data Factory 中的「複製活動」，從 ODBC 資料存放區複製資料及將資料複製到該處。 本文是根據[複製活動概觀](copy-activity-overview.md)一文，該文提供複製活動的一般概觀。

## <a name="supported-capabilities"></a>支援的功能

此 ODBC 連接器支援用於以下活動：

- 使用[支援的源/接收器矩陣](copy-activity-overview.md)[複製活動](copy-activity-overview.md)
- [查找活動](control-flow-lookup-activity.md)

您可以將資料從 ODBC 來源複製到任何支援的接收資料存放區，或從任何支援的來源資料存放區複製到 ODBC 接收器。 如需複製活動所支援作為來源/接收器的資料存放區清單，請參閱[支援的資料存放區](copy-activity-overview.md#supported-data-stores-and-formats)表格。

具體而言，這個 ODBC 連接器支援使用 **Basic** (基本) 或 **Anonymous** (匿名) 驗證，從**任何 ODBC 相容的資料存放區**複製資料或將資料複製到該處。 需要**64 位 ODBC 驅動程式**。

## <a name="prerequisites"></a>Prerequisites

若要使用這個 ODBC 連接器，您必須：

- 設定一個「自我裝載 Integration Runtime」。 有關詳細資訊[，請參閱自託管集成運行時](create-self-hosted-integration-runtime.md)文章。
- 為集成運行時電腦上的資料存儲安裝 64 位 ODBC 驅動程式。

## <a name="getting-started"></a>開始使用

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

下列各節提供屬性的相關詳細資料，這些屬性是用來定義 ODBC 連接器專屬的 Data Factory 實體。

## <a name="linked-service-properties"></a>連結服務屬性

以下是針對 ODBC 已連結服務支援的屬性：

| 屬性 | 描述 | 必要 |
|:--- |:--- |:--- |
| type | 類型屬性必須設定為：**Odbc** | 是 |
| connectionString | 不包含認證部分的連接字串。 您可以用 `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"` 模式指定連接字串，或使用您在 Integration Runtime 電腦上以 `"DSN=<name of the DSN on IR machine>;"` 設定的系統 DSN (資料來源名稱) (仍需要據此指定連結的服務中的認證部分)。<br>您還可以在 Azure 金鑰保存庫中輸入密碼， `password` 並將配置從連接字串中拔出。有關詳細資訊，請參閱 [在 Azure 金鑰保存庫中](store-credentials-in-key-vault.md) 存儲憑據。| 是 |
| authenticationType | 用來連接到 ODBC 資料存放區的驗證類型。<br/>允許的值為：**Basic** (基本) 和 **Anonymous** (匿名)。 | 是 |
| userName | 如果您要使用 Basic 驗證，請指定使用者名稱。 | 否 |
| 密碼 | 指定您為 userName 指定之使用者帳戶的密碼。 將此欄位標記為 SecureString，將它安全地儲存在 Data Factory 中，或[參考 Azure Key Vault 中儲存的祕密](store-credentials-in-key-vault.md)。 | 否 |
| 認證 (credential) | 以驅動程式特定「屬性-值」格式指定之連接字串的存取認證部分。 範例： `"RefreshToken=<secret refresh token>;"`. 請將此欄位標示為 SecureString。 | 否 |
| connectVia | 用來連線到資料存放區的 [Integration Runtime](concepts-integration-runtime.md)。 如[必要條件](#prerequisites)所述，必須要有一個「自我裝載 Integration Runtime」。 |是 |

**範例 1：使用 Basic (基本) 驗證**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**範例 2：使用 Anonymous (匿名) 驗證**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>資料集屬性

有關可用於定義資料集的節和屬性的完整清單，請參閱[資料集](concepts-datasets-linked-services.md)一文。 本節提供 ODBC 資料集所支援的屬性清單。

要將資料從/複製到與 ODBC 相容的資料存儲，支援以下屬性：

| 屬性 | 描述 | 必要 |
|:--- |:--- |:--- |
| type | 資料集的類型屬性必須設置為 **：OdbcTable** | 是 |
| tableName | ODBC 資料存放區中資料表的名稱。 | 就來源而言為非必要 (如果已指定活動來源中的「查詢」)；<br/>就接收器而言為必要 |

**範例**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "OdbcTable",
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

如果使用`RelationalTable`類型化資料集，則仍支援該資料集，同時建議您今後使用新資料集。

## <a name="copy-activity-properties"></a>複製活動屬性

如需可用來定義活動的區段和屬性完整清單，請參閱[管線](concepts-pipelines-activities.md)一文。 本節提供 ODBC 來源所支援的屬性清單。

### <a name="odbc-as-source"></a>ODBC 作為來源

要從與 ODBC 相容的資料存儲複製資料，複製活動**源**部分支援以下屬性：

| 屬性 | 描述 | 必要 |
|:--- |:--- |:--- |
| type | 必須將複製活動源的類型屬性設置為 **：OdbcSource** | 是 |
| 查詢 | 使用自訂 SQL 查詢來讀取資料。 例如：`"SELECT * FROM MyTable"`。 | 否 (如果已指定資料集中的 "tableName") |

**例子：**

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OdbcSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

如果使用`RelationalSource`類型化源，它仍然支援按"當前"，同時建議您今後使用新的源。

### <a name="odbc-as-sink"></a>ODBC 作為接收器

若要將資料複製到 ODBC 相容的資料存放區，請將複製活動中的接收器類型設定為 **OdbcSink**。 複製活動**接收器**部分支援以下屬性：

| 屬性 | 描述 | 必要 |
|:--- |:--- |:--- |
| type | 複製活動接收器的類型屬性必須設定為：**OdbcSink** | 是 |
| writeBatchTimeout |在逾時前等待批次插入作業完成的時間。<br/>允許的值為：時間範圍。 範例：“00:30:00” (30 分鐘)。 |否 |
| writeBatchSize |當緩衝區大小達到 writeBatchSize 時，將資料插入 SQL 資料表中<br/>允許的值為：整數 (資料列數目)。 |No (預設值為 0 - 自動偵測) |
| preCopyScript |指定一個供「複製活動」在每次執行時將資料寫入到資料存放區前執行的 SQL 查詢。 您可以使用此屬性來清除預先載入的資料。 |否 |

> [!NOTE]
> 針對 "writeBatchSize"，如果未設定 (自動偵測)，複製活動會先偵測驅動程式是否支援批次作業，如果支援，就將它設定為 10000，如果不支援，則將它設定為 1。 如果您將此值明確設定為 0 以外的值，複製作業會採用該值，並於驅動程式不支援批次作業時，在執行階段發生失敗。

**例子：**

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="sap-hana-sink"></a>SAP HANA 接收器

>[!NOTE]
>若要從 SAP HANA 資料存放區複製資料，請參考原生 [SAP HANA 連接器](connector-sap-hana.md)。 若要將資料複製到 SAP HANA，請依照此指示來使用 ODBC 連接器。 請注意，SAP HANA 連接器和 ODBC 連接器的已連接服務具有不同的類型，因此無法重複使用。
>

您可以使用一般的 ODBC 連接器從 IBM Informix 資料庫複製資料。

請在電腦上設定一個能夠存取您資料存放區的「自我裝載 Integration Runtime」。 此 Integration Runtime 會使用 SAP HANA 的 ODBC 驅動程式來連線到資料存放區。 因此，如果尚未在該相同電腦上安裝該驅動程式，請安裝該驅動程式。 如需詳細資料，請參閱[必要條件](#prerequisites)一節。

在 Data Factory 解決方案中使用 SAP HANA 接收之前，請先依照[針對連線問題進行疑難排解](#troubleshoot-connectivity-issues)一節中的指示，確認 Integration Runtime 是否能夠連線到資料存放區。

建立 ODBC 已連結的服務來將 SAP HANA 資料存放區連線至 Azure Data Factory，如以下範例所示︰

```json
{
    "name": "SAPHANAViaODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "Driver={HDBODBC};servernode=<HANA server>.clouddatahub-int.net:30015",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

如需在複製作業中使用 ODBC 資料存放區作為來源/接收資料存放區的詳細概觀，請從頭閱讀本文。

## <a name="lookup-activity-properties"></a>查找活動屬性

要瞭解有關屬性的詳細資訊，請檢查[查找活動](control-flow-lookup-activity.md)。


## <a name="troubleshoot-connectivity-issues"></a>疑難排解連線問題

若要針對連線問題進行疑難排解，請使用 [整合執行階段組態管理員]**** 的 [診斷]**** 索引標籤。

1. 啟動 [整合執行階段組態管理員]****。
2. 切換至 [診斷] **** 索引標籤。
3. 在 [測試連線] 區段底下，選取資料存放區的 [類型]**** (已連結的服務)。
4. 指定用來連線到資料存放區的 [連接字串]****，選擇 [驗證]****，然後輸入 [使用者名稱]****、[密碼]**** 和/或 [認證]****。
5. 按一下 [測試連線] **** 以測試資料存放區連線。

## <a name="next-steps"></a>後續步驟
如需 Azure Data Factory 中的複製活動所支援作為來源和接收器的資料存放區清單，請參閱[支援的資料存放區](copy-activity-overview.md#supported-data-stores-and-formats)。
