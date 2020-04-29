---
title: Azure 監視器中的警示架構定義
description: 瞭解 Azure 監視器的一般警示架構定義
author: ofirmanor
ms.topic: conceptual
ms.subservice: alerts
ms.date: 03/14/2019
ms.openlocfilehash: 62b2738324f4c728cd4b5959c04c93649c156afb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81114450"
---
# <a name="common-alert-schema-definitions"></a>常見的警示結構描述定義

本文說明 Azure 監視器的[常見警示架構定義](https://aka.ms/commonAlertSchemaDocs)，包括適用于 webhook、Azure Logic Apps、Azure Functions 和 Azure 自動化 runbook 的。 

任何警示實例都會描述受影響的資源，以及警示的原因。 這些實例會在下列各節的一般架構中說明：
* **基本**資訊：一組標準化的欄位，通用於所有警示類型，其中描述警示所在的資源，以及其他常見的警示中繼資料（例如，嚴重性或描述）。 
* **警示內容**：描述警示原因的一組欄位，其中的欄位會根據警示類型而有所不同。 例如，計量警示會在警示內容中包含類似計量名稱和計量值的欄位，而活動記錄警示則包含產生警示之事件的相關資訊。 

**範例警示承載**
```json
{
  "schemaId": "azureMonitorCommonAlertSchema",
  "data": {
    "essentials": {
      "alertId": "/subscriptions/<subscription ID>/providers/Microsoft.AlertsManagement/alerts/b9569717-bc32-442f-add5-83a997729330",
      "alertRule": "WCUS-R2-Gen2",
      "severity": "Sev3",
      "signalType": "Metric",
      "monitorCondition": "Resolved",
      "monitoringService": "Platform",
      "alertTargetIDs": [
        "/subscriptions/<subscription ID>/resourcegroups/pipelinealertrg/providers/microsoft.compute/virtualmachines/wcus-r2-gen2"
      ],
      "originAlertId": "3f2d4487-b0fc-4125-8bd5-7ad17384221e_PipeLineAlertRG_microsoft.insights_metricAlerts_WCUS-R2-Gen2_-117781227",
      "firedDateTime": "2019-03-22T13:58:24.3713213Z",
      "resolvedDateTime": "2019-03-22T14:03:16.2246313Z",
      "description": "",
      "essentialsVersion": "1.0",
      "alertContextVersion": "1.0"
    },
    "alertContext": {
      "properties": null,
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Percentage CPU",
            "metricNamespace": "Microsoft.Compute/virtualMachines",
            "operator": "GreaterThan",
            "threshold": "25",
            "timeAggregation": "Average",
            "dimensions": [
              {
                "name": "ResourceId",
                "value": "3efad9dc-3d50-4eac-9c87-8b3fd6f97e4e"
              }
            ],
            "metricValue": 7.727
          }
        ]
      }
    }
  }
}
```

## <a name="essentials"></a>基本資訊

| 欄位 | 描述|
|:---|:---|
| alertId | 唯一識別警示實例的 GUID。 |
| alertRule | 產生警示實例之警示規則的名稱。 |
| Severity | 警示的嚴重性。 可能的值： Sev0、Sev1、Sev2、Sev3 或 Sev4。 |
| signalType | 識別已定義警示規則的信號。 可能的值： [計量]、[記錄] 或 [活動記錄]。 |
| monitorCondition | 當警示引發時，警示的監視條件會設定為 [已**引發**]。 當引發警示的基礎條件清除時，監視條件會設定為 [**已解決**]。   |
| monitoringService | 產生警示的監視服務或解決方案。 警示內容的欄位是由監視服務所決定。 |
| alertTargetIds | 受影響警示目標之 Azure Resource Manager 識別碼的清單。 針對 Log Analytics 工作區或 Application Insights 實例上定義的記錄警示，這是各自的工作區或應用程式。 |
| originAlertId | 警示實例的識別碼，由產生它的監視服務所產生。 |
| firedDateTime | 在國際標準時間（UTC）中引發警示實例的日期和時間。 |
| resolvedDateTime | 警示實例的監視條件設定為 [以 UTC**解決**] 的日期和時間。 目前僅適用于計量警示。|
| description | [描述]，如警示規則中所定義。 |
|essentialsVersion| [基本] 區段的版本號碼。|
|alertCoNtextVersion | `alertContext`區段的版本號碼。 |

**範例值**
```json
{
  "essentials": {
    "alertId": "/subscriptions/<subscription ID>/providers/Microsoft.AlertsManagement/alerts/b9569717-bc32-442f-add5-83a997729330",
    "alertRule": "Contoso IT Metric Alert",
    "severity": "Sev3",
    "signalType": "Metric",
    "monitorCondition": "Fired",
    "monitoringService": "Platform",
    "alertTargetIDs": [
      "/subscriptions/<subscription ID>/resourceGroups/aimon-rg/providers/Microsoft.Insights/components/ai-orion-int-fe"
    ],
    "originAlertId": "74ff8faa0c79db6084969cf7c72b0710e51aec70b4f332c719ab5307227a984f",
    "firedDateTime": "2019-03-26T05:25:50.4994863Z",
    "description": "Test Metric alert",
    "essentialsVersion": "1.0",
    "alertContextVersion": "1.0"
  }
}
```

## <a name="alert-context"></a>警示內容

### <a name="metric-alerts"></a>度量警示

#### <a name="monitoringservice--platform"></a>`monitoringService` = `Platform`

**範例值**
```json
{
  "alertContext": {
      "properties": null,
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Percentage CPU",
            "metricNamespace": "Microsoft.Compute/virtualMachines",
            "operator": "GreaterThan",
            "threshold": "25",
            "timeAggregation": "Average",
            "dimensions": [
              {
                "name": "ResourceId",
                "value": "3efad9dc-3d50-4eac-9c87-8b3fd6f97e4e"
              }
            ],
            "metricValue": 31.1105
          }
        ],
        "windowStartTime": "2019-03-22T13:40:03.064Z",
        "windowEndTime": "2019-03-22T13:45:03.064Z"
      }
    }
}
```

### <a name="log-alerts"></a>記錄警示

> [!NOTE]
> 針對已定義自訂電子郵件主旨和/或 JSON 內容的記錄警示，啟用通用架構會將電子郵件主旨和（或）承載架構還原成如下所述的內容。 啟用通用架構的警示，其大小上限為每個警示 256 KB。 如果搜尋結果導致警示大小超過此閾值，則不會內嵌在記錄警示承載中。 您可以藉由檢查旗`IncludeSearchResults`標來判斷這一點。 如果未包含搜尋結果，您應該搭配使用搜尋查詢與[Log ANALYTICS API](https://docs.microsoft.com/rest/api/loganalytics/query/get)。 

#### <a name="monitoringservice--log-analytics"></a>`monitoringService` = `Log Analytics`

**範例值**
```json
{
  "alertContext": {
    "SearchQuery": "search * \n| where Type == \"Heartbeat\" \n| where Category == \"Direct Agent\" \n| where TimeGenerated > ago(30m) ",
    "SearchIntervalStartTimeUtc": "3/22/2019 1:36:31 PM",
    "SearchIntervalEndtimeUtc": "3/22/2019 1:51:31 PM",
    "ResultCount": 2,
    "LinkToSearchResults": "https://portal.azure.com#@72f988bf-86f1-41af-91ab-2d7cd011db47/blade/Microsoft_OperationsManagementSuite_Workspace/AnalyticsBlade/initiator/AnalyticsShareLinkToQuery/isQueryEditorVisible/true/scope/%7B%22resources%22%3A%5B%7B%22resourceId%22%3A%22%2Fsubscriptions%<subscription ID>%2FresourceGroups%2Fpipelinealertrg%2Fproviders%2FMicrosoft.OperationalInsights%2Fworkspaces%2FINC-OmsAlertRunner%22%7D%5D%7D/query/search%20%2A%20%0A%7C%20where%20Type%20%3D%3D%20%22Heartbeat%22%20%0A%7C%20where%20Category%20%3D%3D%20%22Direct%20Agent%22%20%0A%7C%20where%20TimeGenerated%20%3E%20%28datetime%282019-03-22T13%3A51%3A31.0000000%29%20-%2030m%29%20%20/isQuerybase64Compressed/false/timespanInIsoFormat/2019-03-22T13%3a36%3a31.0000000Z%2f2019-03-22T13%3a51%3a31.0000000Z",
    "SeverityDescription": "Warning",
    "WorkspaceId": "2a1f50a7-ef97-420c-9d14-938e77c2a929",
    "SearchIntervalDurationMin": "15",
    "AffectedConfigurationItems": [
      "INC-Gen2Alert"
    ],
    "SearchIntervalInMinutes": "15",
    "Threshold": 10000,
    "Operator": "Less Than",
    "SearchResults": {
      "tables": [
        {
          "name": "PrimaryResult",
          "columns": [
            {
              "name": "$table",
              "type": "string"
            },
            {
              "name": "Id",
              "type": "string"
            },
            {
              "name": "TimeGenerated",
              "type": "datetime"
            }
          ],
          "rows": [
            [
              "Fabrikam",
              "33446677a",
              "2018-02-02T15:03:12.18Z"
            ],
            [
              "Contoso",
              "33445566b",
              "2018-02-02T15:16:53.932Z"
            ]
          ]
        }
      ],
      "dataSources": [
        {
          "resourceId": "/subscriptions/a5ea27e2-7482-49ba-90b3-60e7496dd873/resourcegroups/nrt-tip-kc/providers/microsoft.operationalinsights/workspaces/nrt-tip-kc",
          "tables": [
            "Heartbeat"
          ]
        }
      ]
    },
    "IncludeSearchResults": "True",
    "AlertType": "Number of results"
  }
}
```

#### <a name="monitoringservice--application-insights"></a>`monitoringService` = `Application Insights`

**範例值**
```json
{
  "alertContext": {
    "SearchQuery": "search *",
    "SearchIntervalStartTimeUtc": "3/22/2019 1:36:33 PM",
    "SearchIntervalEndtimeUtc": "3/22/2019 1:51:33 PM",
    "ResultCount": 2,
    "LinkToSearchResults": "https://portal.azure.com#@72f988bf-86f1-41af-91ab-2d7cd011db47/blade/Microsoft_OperationsManagementSuite_Workspace/AnalyticsBlade/initiator/AnalyticsShareLinkToQuery/isQueryEditorVisible/true/scope/%7B%22resources%22%3A%5B%7B%22resourceId%22%3A%22%2Fsubscriptions%<subscription ID>%2FresourceGroups%2FPipeLineAlertRG%2Fproviders%2Fmicrosoft.insights%2Fcomponents%2FWEU-AIRunner%22%7D%5D%7D/query/search%20%2A/isQuerybase64Compressed/false/timespanInIsoFormat/2019-03-22T13%3a36%3a33.0000000Z%2f2019-03-22T13%3a51%3a33.0000000Z",
    "SearchIntervalDurationMin": "15",
    "SearchIntervalInMinutes": "15",
    "Threshold": 10000,
    "Operator": "Less Than",
    "ApplicationId": "8e20151d-75b2-4d66-b965-153fb69d65a6",
    "SearchResults": {
      "tables": [
        {
          "name": "PrimaryResult",
          "columns": [
            {
              "name": "$table",
              "type": "string"
            },
            {
              "name": "Id",
              "type": "string"
            },
            {
              "name": "TimeGenerated",
              "type": "datetime"
            }
          ],
          "rows": [
            [
              "Fabrikam",
              "33446677a",
              "2018-02-02T15:03:12.18Z"
            ],
            [
              "Contoso",
              "33445566b",
              "2018-02-02T15:16:53.932Z"
            ]
          ]
        }
      ],
      "dataSources": [
        {
          "resourceId": "/subscriptions/a5ea27e2-7482-49ba-90b3-60e7496dd873/resourcegroups/nrt-tip-kc/providers/microsoft.operationalinsights/workspaces/nrt-tip-kc",
          "tables": [
            "Heartbeat"
          ]
        }
      ]
    },
    "IncludeSearchResults": "True",
    "AlertType": "Number of results"
  }
}
```

### <a name="activity-log-alerts"></a>活動記錄警示

#### <a name="monitoringservice--activity-log---administrative"></a>`monitoringService` = `Activity Log - Administrative`

**範例值**
```json
{
  "alertContext": {
      "authorization": {
        "action": "Microsoft.Compute/virtualMachines/restart/action",
        "scope": "/subscriptions/<subscription ID>/resourceGroups/PipeLineAlertRG/providers/Microsoft.Compute/virtualMachines/WCUS-R2-ActLog"
      },
      "channels": "Operation",
      "claims": "{\"aud\":\"https://management.core.windows.net/\",\"iss\":\"https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/\",\"iat\":\"1553260826\",\"nbf\":\"1553260826\",\"exp\":\"1553264726\",\"aio\":\"42JgYNjdt+rr+3j/dx68v018XhuFAwA=\",\"appid\":\"e9a02282-074f-45cf-93b0-50568e0e7e50\",\"appidacr\":\"2\",\"http://schemas.microsoft.com/identity/claims/identityprovider\":\"https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/\",\"http://schemas.microsoft.com/identity/claims/objectidentifier\":\"9778283b-b94c-4ac6-8a41-d5b493d03aa3\",\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\":\"9778283b-b94c-4ac6-8a41-d5b493d03aa3\",\"http://schemas.microsoft.com/identity/claims/tenantid\":\"72f988bf-86f1-41af-91ab-2d7cd011db47\",\"uti\":\"v5wYC9t9ekuA2rkZSVZbAA\",\"ver\":\"1.0\"}",
      "caller": "9778283b-b94c-4ac6-8a41-d5b493d03aa3",
      "correlationId": "8ee9c32a-92a1-4a8f-989c-b0ba09292a91",
      "eventSource": "Administrative",
      "eventTimestamp": "2019-03-22T13:56:31.2917159+00:00",
      "eventDataId": "161fda7e-1cb4-4bc5-9c90-857c55a8f57b",
      "level": "Informational",
      "operationName": "Microsoft.Compute/virtualMachines/restart/action",
      "operationId": "310db69b-690f-436b-b740-6103ab6b0cba",
      "status": "Succeeded",
      "subStatus": "",
      "submissionTimestamp": "2019-03-22T13:56:54.067593+00:00"
    }
}
```

#### <a name="monitoringservice--activity-log---policy"></a>`monitoringService` = `Activity Log - Policy`

**範例值**
```json
{
  "alertContext": {
    "authorization": {
      "action": "Microsoft.Resources/checkPolicyCompliance/read",
      "scope": "/subscriptions/<GUID>"
    },
    "channels": "Operation",
    "claims": "{\"aud\":\"https://management.azure.com/\",\"iss\":\"https://sts.windows.net/<GUID>/\",\"iat\":\"1566711059\",\"nbf\":\"1566711059\",\"exp\":\"1566740159\",\"aio\":\"42FgYOhynHNw0scy3T/bL71+xLyqEwA=\",\"appid\":\"<GUID>\",\"appidacr\":\"2\",\"http://schemas.microsoft.com/identity/claims/identityprovider\":\"https://sts.windows.net/<GUID>/\",\"http://schemas.microsoft.com/identity/claims/objectidentifier\":\"<GUID>\",\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\":\"<GUID>\",\"http://schemas.microsoft.com/identity/claims/tenantid\":\"<GUID>\",\"uti\":\"Miy1GzoAG0Scu_l3m1aIAA\",\"ver\":\"1.0\"}",
    "caller": "<GUID>",
    "correlationId": "<GUID>",
    "eventSource": "Policy",
    "eventTimestamp": "2019-08-25T11:11:34.2269098+00:00",
    "eventDataId": "<GUID>",
    "level": "Warning",
    "operationName": "Microsoft.Authorization/policies/audit/action",
    "operationId": "<GUID>",
    "properties": {
      "isComplianceCheck": "True",
      "resourceLocation": "eastus2",
      "ancestors": "<GUID>",
      "policies": "[{\"policyDefinitionId\":\"/providers/Microsoft.Authorization/policyDefinitions/<GUID>/\",\"policySetDefinitionId\":\"/providers/Microsoft.Authorization/policySetDefinitions/<GUID>/\",\"policyDefinitionReferenceId\":\"vulnerabilityAssessmentMonitoring\",\"policySetDefinitionName\":\"<GUID>\",\"policyDefinitionName\":\"<GUID>\",\"policyDefinitionEffect\":\"AuditIfNotExists\",\"policyAssignmentId\":\"/subscriptions/<GUID>/providers/Microsoft.Authorization/policyAssignments/SecurityCenterBuiltIn/\",\"policyAssignmentName\":\"SecurityCenterBuiltIn\",\"policyAssignmentScope\":\"/subscriptions/<GUID>\",\"policyAssignmentSku\":{\"name\":\"A1\",\"tier\":\"Standard\"},\"policyAssignmentParameters\":{}}]"
    },
    "status": "Succeeded",
    "subStatus": "",
    "submissionTimestamp": "2019-08-25T11:12:46.1557298+00:00"
  }
}
```

#### <a name="monitoringservice--activity-log---autoscale"></a>`monitoringService` = `Activity Log - Autoscale`

**範例值**
```json
{
  "alertContext": {
    "channels": "Admin, Operation",
    "claims": "{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn\":\"Microsoft.Insights/autoscaleSettings\"}",
    "caller": "Microsoft.Insights/autoscaleSettings",
    "correlationId": "<GUID>",
    "eventSource": "Autoscale",
    "eventTimestamp": "2019-08-21T16:17:47.1551167+00:00",
    "eventDataId": "<GUID>",
    "level": "Informational",
    "operationName": "Microsoft.Insights/AutoscaleSettings/Scaleup/Action",
    "operationId": "<GUID>",
    "properties": {
      "description": "The autoscale engine attempting to scale resource '/subscriptions/d<GUID>/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachineScaleSets/testVMSS' from 9 instances count to 10 instances count.",
      "resourceName": "/subscriptions/<GUID>/resourceGroups/voiceassistancedemo/providers/Microsoft.Compute/virtualMachineScaleSets/alexademo",
      "oldInstancesCount": "9",
      "newInstancesCount": "10",
      "activeAutoscaleProfile": "{\r\n  \"Name\": \"Auto created scale condition\",\r\n  \"Capacity\": {\r\n    \"Minimum\": \"1\",\r\n    \"Maximum\": \"10\",\r\n    \"Default\": \"1\"\r\n  },\r\n  \"Rules\": [\r\n    {\r\n      \"MetricTrigger\": {\r\n        \"Name\": \"Percentage CPU\",\r\n        \"Namespace\": \"microsoft.compute/virtualmachinescalesets\",\r\n        \"Resource\": \"/subscriptions/<GUID>/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachineScaleSets/testVMSS\",\r\n        \"ResourceLocation\": \"eastus\",\r\n        \"TimeGrain\": \"PT1M\",\r\n        \"Statistic\": \"Average\",\r\n        \"TimeWindow\": \"PT5M\",\r\n        \"TimeAggregation\": \"Average\",\r\n        \"Operator\": \"GreaterThan\",\r\n        \"Threshold\": 0.0,\r\n        \"Source\": \"/subscriptions/<GUID>/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachineScaleSets/testVMSS\",\r\n        \"MetricType\": \"MDM\",\r\n        \"Dimensions\": [],\r\n        \"DividePerInstance\": false\r\n      },\r\n      \"ScaleAction\": {\r\n        \"Direction\": \"Increase\",\r\n        \"Type\": \"ChangeCount\",\r\n        \"Value\": \"1\",\r\n        \"Cooldown\": \"PT1M\"\r\n      }\r\n    }\r\n  ]\r\n}",
      "lastScaleActionTime": "Wed, 21 Aug 2019 16:17:47 GMT"
    },
    "status": "Succeeded",
    "submissionTimestamp": "2019-08-21T16:17:47.2410185+00:00"
  }
}
```

#### <a name="monitoringservice--activity-log---security"></a>`monitoringService` = `Activity Log - Security`

**範例值**
```json
{
  "alertContext": {
    "channels": "Operation",
    "correlationId": "<GUID>",
    "eventSource": "Security",
    "eventTimestamp": "2019-08-26T08:34:14+00:00",
    "eventDataId": "<GUID>",
    "level": "Informational",
    "operationName": "Microsoft.Security/locations/alerts/activate/action",
    "operationId": "<GUID>",
    "properties": {
      "threatStatus": "Quarantined",
      "category": "Virus",
      "threatID": "2147519003",
      "filePath": "C:\\AlertGeneration\\test.eicar",
      "protectionType": "Windows Defender",
      "actionTaken": "Blocked",
      "resourceType": "Virtual Machine",
      "severity": "Low",
      "compromisedEntity": "testVM",
      "remediationSteps": "[\"No user action is necessary\"]",
      "attackedResourceType": "Virtual Machine"
    },
    "status": "Active",
    "submissionTimestamp": "2019-08-26T09:28:58.3019107+00:00"
  }
}
```

#### <a name="monitoringservice--servicehealth"></a>`monitoringService` = `ServiceHealth`

**範例值**
```json
{
  "alertContext": {
    "authorization": null,
    "channels": 1,
    "claims": null,
    "caller": null,
    "correlationId": "f3cf2430-1ee3-4158-8e35-7a1d615acfc7",
    "eventSource": 2,
    "eventTimestamp": "2019-06-24T11:31:19.0312699+00:00",
    "httpRequest": null,
    "eventDataId": "<GUID>",
    "level": 3,
    "operationName": "Microsoft.ServiceHealth/maintenance/action",
    "operationId": "<GUID>",
    "properties": {
      "title": "Azure SQL DW Scheduled Maintenance Pending",
      "service": "SQL Data Warehouse",
      "region": "East US",
      "communication": "<MESSAGE>",
      "incidentType": "Maintenance",
      "trackingId": "<GUID>",
      "impactStartTime": "2019-06-26T04:00:00Z",
      "impactMitigationTime": "2019-06-26T12:00:00Z",
      "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"East US\"}],\"ServiceName\":\"SQL Data Warehouse\"}]",
      "impactedServicesTableRows": "<tr>\r\n<td align='center' style='padding: 5px 10px; border-right:1px solid black; border-bottom:1px solid black'>SQL Data Warehouse</td>\r\n<td align='center' style='padding: 5px 10px; border-bottom:1px solid black'>East US<br></td>\r\n</tr>\r\n",
      "defaultLanguageTitle": "Azure SQL DW Scheduled Maintenance Pending",
      "defaultLanguageContent": "<MESSAGE>",
      "stage": "Planned",
      "communicationId": "<GUID>",
      "maintenanceId": "<GUID>",
      "isHIR": "false",
      "version": "0.1.1"
    },
    "status": "Active",
    "subStatus": null,
    "submissionTimestamp": "2019-06-24T11:31:31.7147357+00:00",
    "ResourceType": null
  }
}
```
#### <a name="monitoringservice--resource-health"></a>`monitoringService` = `Resource Health`

**範例值**
```json
{
  "alertContext": {
    "channels": "Admin, Operation",
    "correlationId": "<GUID>",
    "eventSource": "ResourceHealth",
    "eventTimestamp": "2019-06-24T15:42:54.074+00:00",
    "eventDataId": "<GUID>",
    "level": "Informational",
    "operationName": "Microsoft.Resourcehealth/healthevent/Activated/action",
    "operationId": "<GUID>",
    "properties": {
      "title": "This virtual machine is stopping and deallocating as requested by an authorized user or process",
      "details": null,
      "currentHealthStatus": "Unavailable",
      "previousHealthStatus": "Available",
      "type": "Downtime",
      "cause": "UserInitiated"
    },
    "status": "Active",
    "submissionTimestamp": "2019-06-24T15:45:20.4488186+00:00"
  }
}
```


## <a name="next-steps"></a>後續步驟

- 深入瞭解常見的[警示架構](https://aka.ms/commonAlertSchemaDocs)。
- 瞭解[如何建立邏輯應用程式，以使用一般警示架構來處理您的所有警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-integrations)。 

