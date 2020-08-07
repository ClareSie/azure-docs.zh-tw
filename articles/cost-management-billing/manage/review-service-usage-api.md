---
title: 使用 REST API 檢閱 Azure 服務資源使用量
description: 了解如何使用 Azure REST API 檢閱 Azure 服務資源使用量。 建立成本管理報告，並篩選特定的資源類型。
author: lleonard-msft
ms.service: cost-management-billing
ms.topic: reference
ms.date: 03/31/2020
ms.author: banders
ms.openlocfilehash: 89767ff64da0473feef9e1d340ee4046cd4a1373
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87461709"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>使用 REST API 檢閱 Azure 資源使用量

Azure 成本管理 API 可協助您檢閱及管理 Azure 資源的耗用量。

在本文中，您將了解如何建立每日報告，以產生包含每小時使用量資訊的逗號分隔值文件；以及了解如何使用篩選器自訂報告，以便您查詢 Azure 資源群組中虛擬機器、資料庫，及標記資源的使用量。

>[!NOTE]
> 成本管理 API 目前僅供私人預覽。

## <a name="create-a-basic-cost-management-report"></a>建立基本的成本管理報告

使用成本管理 API 中的 `reports` 作業來定義如何產生成本報告，以及報告將發佈至何處。

```http
https://management.azure.com/subscriptions/{subscriptionGuid}/providers/Microsoft.CostManagement/reports/{reportName}?api-version=2018-09-01-preview
Content-Type: application/json   
Authorization: Bearer
```

`{subscriptionGuid}` 參數為必要，且應包含可使用 API 權杖中提供認證來讀取的訂用帳戶識別碼。 

`{reportName}` 參數會指定報告的名稱。 若要取得報表名稱的清單，您可以使用 Reports_List 作業來取得清單：`/subscriptions/{subscriptionId}/providers/Microsoft.CostManagement/reports`。 在 [GitHub](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/cost-management/resource-manager/Microsoft.CostManagement/preview/2018-08-01-preview/examples/ReportList.json) 上檢視範例輸出。

以下是必要標頭：

|要求標頭|描述|  
|--------------------|-----------------|  
|*Content-Type:*| 必要。 設定為 `application/json`。 |  
|*Authorization:*| 必要。 設定為有效的 `Bearer` 權杖。 |

在 HTTP 要求本文中設定報告參數。 在下列範例中，報告設定為每日作用中時產生，其為寫入 Azure 儲存體 blob 容器的 CSV 檔案，且包含資源群組 `westus` 中所有資源每小時的成本資訊。

```json
{
    "properties": {
        "schedule": {
            "status": "Inactive",
            "recurrence": "Daily",
            "recurrencePeriod": {
                "from": "2018-08-21",
                "to": "2019-10-31"
            }
        },
        "deliveryInfo": {
            "destination": {
                "resourceId": "/subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
                "container": "MyReportContainer",
                "rootFolderPath": "MyScheduleTest"
            }
        },
        "format": "Csv",
        "definition": {
            "type": "Usage",
            "timeframe": "MonthToDate",
            "dataSet": {
                "granularity": "Hourly",
                "filter": {
                    "dimensions": {
                        "name": "ResourceLocation",
                        "operator": "In",
                        "values": [
                            "westus"
                        ]
                    }
                }
            }
        }
    }
}
```

此

## <a name="filtering-reports"></a>篩選報告

建立報告時要求本文的 `filter` 和 `dimensions` 區段可讓您專注在特定資源類型的成本。 先前的要求本文會示範在區域中如何依照所有資源進行篩選。

### <a name="get-all-compute-usage"></a>取得所有計算使用量

使用 `ResourceType` 維度來報告所有區域中訂用帳戶的 Azure 虛擬機器成本。

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.ClassicCompute/virtualMachines",
                "Microsoft.Compute/virtualMachines"
        ]
    }
}
```

### <a name="get-all-database-usage"></a>取得所有資料庫使用量

使用 `ResourceType` 維度來報告所有區域中訂用帳戶的 Azure SQL Database 成本。

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.Sql/servers"
        ]
    }
}
```

### <a name="report-on-specific-instances"></a>報告特定的執行個體

`Resource` 維度可讓您報告特定資源的成本。

```json
"filter": {
    "dimensions": {
        "name": "Resource",
        "operator": "In",
        "values": [
            "subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroup}/providers/Microsoft.ClassicCompute/virtualMachines/{ResourceName}"
        ]
    }
}
```

### <a name="changing-timeframes"></a>變更時間範圍

將 `timeframe` 定義設定為 `Custom`，以設定除了內建選項當週到現在，以及月初到現在以外的時間範圍。

```json
"timeframe": "Custom",
"timePeriod": {
    "from": "2017-12-31T00:00:00.000Z",
    "to": "2018-12-30T00:00:00.000Z"
}
```

## <a name="next-steps"></a>後續步驟
- [Get started with Azure REST API](https://docs.microsoft.com/rest/api/azure/) (開始使用 Azure REST API)   
