---
title: 如何調試 Udf-Azure 數位 Twins |Microsoft Docs
description: 瞭解在 Azure 數位 Twins 中用來偵測使用者定義函數的建議方法。
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/21/2020
ms.custom: seodec18
ms.openlocfilehash: 518383488aa878dab75aec7ad5da664332b62ad0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "76511632"
---
# <a name="how-to-debug-user-defined-functions-in-azure-digital-twins"></a>如何為 Azure Digital Twins 中的使用者定義函式偵錯

本文摘要說明如何診斷和偵測 Azure 數位 Twins 中的使用者定義函數。 接著，它會找出偵錯這些函式時的一些最常見案例。

>[!TIP]
> 若要深入了解如何在 Azure Digital Twins 中使用活動記錄、診斷記錄和 Azure 監視器來設定偵錯工具，請閱讀[如何設定監視和記錄](./how-to-configure-monitoring.md)。

## <a name="debug-issues"></a>為問題偵錯

瞭解如何診斷 Azure 數位 Twins 內的問題，可讓您有效地分析問題、找出問題的原因，並為他們提供適當的解決方案。

提供各種記錄、分析和診斷工具給該端。

### <a name="enable-logging-for-your-instance"></a>為您的實例啟用記錄

Azure Digital Twins 支援強固的記錄、監視與分析功能。 解決方案開發人員可以使用 Azure 監視器記錄、診斷記錄、活動記錄和其他服務，以支援 IoT 應用程式的複雜監視需求。 您可以合併記錄選項以查詢或檢視跨多的服務的記錄，並提供許多服務的精細記錄涵蓋範圍。

* 如需 Azure 數位 Twins 特有的記錄設定，請閱讀[如何設定監視和記錄](./how-to-configure-monitoring.md)。
* 請參閱[Azure 監視器](../azure-monitor/overview.md)總覽，以瞭解透過 Azure 監視器啟用的強大記錄檔設定。
* 請參閱[從 azure 資源收集和取用記錄資料](../azure-monitor/platform/platform-logs-overview.md)一文，以透過 Azure 入口網站、Azure CLI 或 PowerShell 設定 Azure 數位 Twins 中的診斷記錄設定。

設定好之後，您將能夠選取所有記錄類別、計量，並使用功能強大的 Azure 監視器 log analytics 工作區來支援您的偵錯工具。

### <a name="trace-sensor-telemetry"></a>追蹤感應器遙測

若要追蹤感應器遙測，請確認已針對您的 Azure Digital Twins 執行個體啟用診斷設定。 然後，請確定已選取所有需要的記錄檔類別。 最後，確認所需的記錄會傳送至 Azure 監視器記錄。

若要使感應器遙測訊息與其各自的記錄相符，您可以針對所傳送的事件資料指定相互關聯識別碼。 若要這樣做，將 `x-ms-client-request-id` 屬性設為 GUID。

傳送遙測之後，請使用 [設定相互關聯識別碼] 開啟 Azure 監視器 log analytics 來查詢記錄：

```Kusto
AzureDiagnostics
| where CorrelationId == 'YOUR_CORRELATION_IDENTIFIER'
```

| 查詢值 | 更換為 |
| --- | --- |
| YOUR_CORRELATION_IDENTIFIER | 針對事件資料指定的相互關聯識別碼 |

若要讀取所有最近的遙測記錄查詢：

```Kusto
AzureDiagnostics
| order by CorrelationId desc
```

如果您為使用者定義函數啟用記錄，這些記錄會顯示在您的 log analytics 實例中，其`UserDefinedFunction`類別目錄為。 若要取得它們，請在 log analytics 中輸入下列查詢準則：

```Kusto
AzureDiagnostics
| where Category == 'UserDefinedFunction'
```

如需有關功能強大之查詢作業的詳細資訊，請閱讀[開始使用查詢](../azure-monitor/log-query/get-started-queries.md)。

## <a name="identify-common-issues"></a>找出常見問題

為解決方案進行疑難排解時，診斷並找出常見問題很重要。 以下各小節摘要說明開發使用者定義函式時常遇到的數個問題。

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="check-if-a-role-assignment-was-created"></a>請檢查是否已建立角色指派

如果沒有在管理 API 中建立的角色指派，使用者定義函式不會具備執行任何動作的存取權，例如傳送通知、擷取中繼資料，以及在拓撲內設定計算的值。

透過管理 API 檢查使用者定義函式是否存在角色指派：

```URL
GET YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=Down&objectId=YOUR_USER_DEFINED_FUNCTION_ID
```

| 參數值 | 更換為 |
| --- | --- |
| YOUR_USER_DEFINED_FUNCTION_ID | 擷取其角色指派之使用者定義函式的識別碼|

如果沒有任何角色指派存在，請了解[如何為使用者定義函式建立角色指派](./how-to-user-defined-functions.md)。

### <a name="check-if-the-matcher-works-for-a-sensors-telemetry"></a>檢查比對器是否適用於感應器的遙測

如果針對 Azure Digital Twins 執行個體的管理 API 進行下列呼叫，您可以判斷指定的比對器是否適用於指定的感應器。

```URL
GET YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_IDENTIFIER/evaluate/YOUR_SENSOR_IDENTIFIER?enableLogging=true
```

| 參數 | 更換為 |
| --- | --- |
| *YOUR_MATCHER_IDENTIFIER* | 您想要評估之比對器的識別碼 |
| *YOUR_SENSOR_IDENTIFIER* | 您想要評估之感應器的識別碼 |

回應：

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-triggers"></a>檢查感應器觸發的項目

如果針對 Azure Digital Twins 管理 API 進行下列呼叫，您可以判斷所指定感應器內送遙測資料觸發之使用者定義函式的識別碼：

```URL
GET YOUR_MANAGEMENT_API_URL/sensors/YOUR_SENSOR_IDENTIFIER/matchers?includes=UserDefinedFunctions
```

| 參數 | 更換為 |
| --- | --- |
| *YOUR_SENSOR_IDENTIFIER* | 要傳送遙測的感應器識別碼 |

回應：

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>接收通知的問題

當您未收到所觸發使用者定義函式的通知時，請確認您的拓撲物件類型參數符合要使用的識別碼類型。

**不正確**實例

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

當指定的拓撲物件類型為 `Space` 時，所使用的識別碼會參考感應器，因此會發生這種情況。

**正確**實例

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

避免此問題最簡單的方式是針對中繼資料物件使用 `Notify` 方法。

範例：

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>常見診斷例外狀況

如果您啟用診斷設定，可能會遇到下列常見的例外狀況：

1. **節流**：如果使用者定義函式超出[服務限制](./concepts-service-limits.md)一文中所述的執行速度限制，便會發生節流情況。 在節流限制到期之前，不需要進行任何進一步的作業，就可以成功執行。

1. **找不到資料**：如果使用者定義函式嘗試存取不存在的的中繼資料，作業會失敗。

1. **未獲授權**：如果使用者定義函式還未設定角色指派，或缺少足夠的權限存取拓撲中特定的中繼資料，作業會失敗。

## <a name="next-steps"></a>後續步驟

- 了解如何在 Azure Digital Twins 中啟用[監視和記錄](./how-to-configure-monitoring.md)。

- 如需更多 Azure 記錄選項，請參閱[Azure 活動記錄的總覽一](../azure-monitor/platform/platform-logs-overview.md)文。
