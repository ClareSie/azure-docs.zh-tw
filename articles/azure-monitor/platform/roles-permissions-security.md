---
title: Azure 監視器中的角色、許可權和安全性
description: 了解如何使用 Azure 監視器的內建角色和權限來限制存取監視資源。
author: johnkemnetz
services: azure-monitor
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 81309f0b5781e6302887a5b079ed359e70659834
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658960"
---
# <a name="roles-permissions-and-security-in-azure-monitor"></a>Azure 監視器中的角色、許可權和安全性

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

許多團隊需要嚴格規範對監視資料及設定的存取。 例如，如果您有專門負責監視的團隊成員（支援工程師、DevOps 工程師），或者如果您使用託管服務提供者，則可能需要授予他們僅訪問監視資料的許可權，同時限制他們創建、修改或刪除資源。 本文說明如何在 Azure 中快速將內建的監視 RBAC 角色套用到使用者，或針對需要有限監視權限的使用者建置您自己的自訂角色。 接著會討論 Azure 監視器相關資源的安全性考量，以及如何限制對這些資源所包含的資料進行存取。

## <a name="built-in-monitoring-roles"></a>內建的監視角色
Azure 監視器的內建角色是專為協助限制存取訂用帳戶中的資源所設計，同時仍可讓負責監視基礎結構的人員取得及設定他們所需的資料。 Azure 監視器提供兩個現成的角色︰監視讀取器和監視參與者。

### <a name="monitoring-reader"></a>監視讀取器
受指派監視讀取器角色的人員可以檢視訂用帳戶中所有的監視資料，但無法修改任何資源或編輯與監視資源相關的任何設定。 這個角色適用於組織中的使用者，例如支援或作業工程師，這些人員必須能夠︰

* 在入口網站中檢視監視儀表板，並建立自己的私人監視儀表板。
* 檢視 [Azure 警示](alerts-overview.md)中定義的警示規則
* 使用 [Azure 監視器 REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx)、[PowerShell cmdlets](powershell-quickstart-samples.md) 或[跨平台 CLI](cli-samples.md) 查詢度量。
* 使用入口網站、Azure 監視器 REST API、PowerShell Cmdlets 或跨平台 CLI 查詢活動記錄檔。
* 檢視用於資源的 [診斷設定](diagnostic-settings.md) 。
* 檢視用於訂用帳戶的 [記錄檔設定檔](activity-log-export.md) 。
* 檢視自動調整設定。
* 檢視警示活動和設定。
* 存取 Application Insights 資料，並檢視 AI 分析中的資料。
* 搜尋 Log Analytics 工作區資料，包括工作區的使用狀況資料。
* 檢視 Log Analytics 管理群組。
* 檢索日誌分析工作區中的搜索架構。
* 在日誌分析工作區中列出監視包。
* 在日誌分析工作區中檢索並執行保存的搜索。
* 檢索日誌分析工作區存儲配置。

> [!NOTE]
> 此角色不會對已串流至事件中樞或儲存在儲存體帳戶中的記錄檔資料授予讀取權限。 [請參閱下方](#security-considerations-for-monitoring-data) 以取得設定存取這些資源的相關資訊。
> 
> 

### <a name="monitoring-contributor"></a>監視參與者
受指派監視參與者角色的人員可以檢視訂用帳戶中所有的監視資料，並建立或修改監視設定，但無法修改任何其他資源。 此角色是監視讀取者角色的超集，且適用於組織的監視團隊成員或受控服務提供者，這些服務提供者除了上述的權限之外，也必須能夠︰

* 將監視儀表板發佈為共用儀表板。
* 設置資源的[診斷設置](diagnostic-settings.md)。\*
* 設置訂閱的[日誌設定檔](activity-log-export.md)。\*
* 透過 [Azure 警示](alerts-overview.md)設定警示規則活動和設定。
* 建立 Application Insights web 測試和元件。
* 列出 Log Analytics 工作區共用金鑰。
* 在日誌分析工作區中啟用或禁用監視包。
* 在日誌分析工作區中創建、刪除和執行保存的搜索。
* 創建並刪除日誌分析工作區存儲配置。

\*還必須單獨授予使用者目標資源（存儲帳戶或事件中心命名空間）的 ListKeys 許可權，以設置日誌設定檔或診斷設置。

> [!NOTE]
> 此角色不會對已串流至事件中樞或儲存在儲存體帳戶中的記錄檔資料授予讀取權限。 [請參閱下方](#security-considerations-for-monitoring-data) 以取得設定存取這些資源的相關資訊。
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>監視權限和自訂的 RBAC 角色
如果上述的內建角色不符合您團隊的確切需求，您可以使用更精確的權限 [建立自訂的 RBAC 角色](../../role-based-access-control/custom-roles.md) 。 以下是一般 Azure 監視器 RBAC 作業及其說明。

| 作業 | 描述 |
| --- | --- |
| Microsoft.Insights/ActionGroups/[Read, Write, Delete] |讀取/寫入/刪除動作群組。 |
| Microsoft.Insights/ActivityLogAlerts/[Read, Write, Delete] |讀取/寫入/刪除活動記錄警示。 |
| Microsoft.Insights/AlertRules/[讀取、寫入、刪除] |(從傳統警示) 讀取/寫入/刪除警示規則。 |
| Microsoft.Insights/AlertRules/Incidents/Read |列出警示規則的事件 (觸發的警示規則歷程記錄)。 這僅適用於入口網站。 |
| Microsoft.Insights/AutoscaleSettings/[讀取、寫入、刪除] |讀取/寫入/刪除自動調整設定。 |
| Microsoft.Insights/DiagnosticSettings/[讀取、寫入、刪除] |讀取/寫入/刪除診斷設定。 |
| Microsoft.Insights/EventCategories/Read |列舉「活動記錄」中所有可能的類別。 「Azure 入口網站」所使用。 |
| Microsoft.Insights/eventtypes/digestevents/Read |此為使用者需要透過入口網站存取活動記錄時所需的權限。 |
| Microsoft.Insights/eventtypes/values/Read |列出訂用帳戶中的活動記錄檔事件 (管理事件)。 此權限適用於以程式設計方式存取和入口網站存取活動記錄檔。 |
| Microsoft.Insights/ExtendedDiagnosticSettings/[Read, Write, Delete] | 讀取/寫入/刪除網路流量記錄的診斷設定。 |
| Microsoft.Insights/LogDefinitions/Read |此為使用者需要透過入口網站存取活動記錄時所需的權限。 |
| Microsoft.Insights/LogProfiles/[Read, Write, Delete] |讀取/寫入/刪除記錄設定檔 (將「活動記錄」串流至事件中樞或儲存體帳戶)。 |
| Microsoft.Insights/MetricAlerts/[Read, Write, Delete] |讀取/寫入/刪除近乎即時的計量警示 |
| Microsoft.Insights/MetricDefinitions/Read |讀取度量定義 (可用資源的度量類型清單)。 |
| Microsoft.Insights/Metrics/Read |讀取資源的度量。 |
| Microsoft.Insights/Register/Action |註冊「Azure 監視器」資源提供者。 |
| Microsoft.Insights/ScheduledQueryRules/[讀取、寫入、刪除] |讀取/寫入/刪除 Azure 監視器中的記錄警示。 |



> [!NOTE]
> 存取警示、診斷設定和資源的度量需要使用者具有資源類型和該資源範圍的讀取權限。 建立 (「寫入」) 封存至儲存體帳戶或串流至事件中樞的診斷設定或記錄檔設定檔的使用者也需要在目標資源上擁有 ListKeys 權限。
> 
> 

例如，您可以使用上述資料表針對「活動記錄檔讀取器」建立如下的自訂 RBAC 角色︰

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>監視資料的安全性考量
監視資料 (尤其是記錄檔)，可以包含機密資訊，例如 IP 位址或使用者名稱。 來自 Azure 的監視資料有三種基本形式︰

1. 活動記錄檔，會描述您 Azure 訂用帳戶上所有的控制層面動作。
2. 資源日誌，即資源發出的日誌。
3. 度量，是由資源發出。

這三種資料類型都可以儲存在儲存體帳戶或串流到事件中樞，兩者都是一般用途的 Azure 資源。 由於這些是一般用途的資源，因此對其進行建立、刪除及存取是保留給系統管理員的特殊權限作業。 我們建議您對監視相關的資源使用下列作法以防止誤用︰

* 針對監視資料使用單一、專用的儲存體帳戶。 如果您需要將監視資料分成多個儲存體帳戶，切勿在監視及非監視資料之間分享儲存體帳戶的使用情況，因為這可能會不小心讓只需要存取監視資料 (例如，第三方 SIEM) 的人員存取非監視資料。
* 以上述相同的原因在所有的診斷設定中使用單一、專用的服務匯流排或事件中樞命名空間。
* 將存取監視相關的儲存體帳戶或事件中樞保存在不同的資源群組中，以限制存取它們，並在監視角色上 [使用範圍](../../role-based-access-control/overview.md#scope) 來限制只能存取該資源群組。
* 當使用者只需要存取監視資料時，切勿針對訂用帳戶範圍內的儲存體帳戶或事件中樞授與 ListKeys 權限。 反之，對資源或資源群組 (如果您有專用的監視資源群組) 範圍內的使用者授與這些權限。

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>限制存取監控相關的儲存體帳戶
當使用者或應用程式需要存取儲存體帳戶中的監視資料時，您應該在包含具有 Blob 儲存體服務層級唯讀存取權的監視資料儲存體帳戶上 [產生帳戶 SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx) 。 在 PowerShell 中，它看起來應該如下所示：

```powershell
$context = New-AzStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

接著，您可將權杖提供給需要從該儲存體帳戶進行讀取的實體，且它可以從該儲存體帳戶中的所有 blob 進行列出並讀取。

或者，如果您需要使用 RBAC 控制此權限，可以在該特定儲存體帳戶上對該實體授與 Microsoft.Storage/storageAccounts/listkeys/action 權限。 對於需要設定診斷設定或記錄檔設定檔以封存至儲存體帳戶的使用者而言，這是必要的。 例如，針對只需要從一個儲存體帳戶進行讀取的使用者或應用程式，您可以建立下列自訂 RBAC 角色︰

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzRoleDefinition -Role $role 
```

> [!WARNING]
> ListKeys 權限可讓使用者列出主要和次要的儲存體帳戶金鑰。 在該儲存體帳戶中所有已簽署的服務 (blob、佇列、資料表、檔案) 中，這些金鑰會對使用者授與所有已簽署的權限 (讀取、寫入、建立 blob、刪除 blob 等)。 我們建議盡可能使用上述的帳戶 SAS。
> 
> 

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>限制存取監控相關的事件中樞
可以使用事件中樞採用類似的模式，但您必須先建立專用的接聽授權規則。 如果您要對僅需要接聽監視相關事件中樞的應用程式授與存取權，請執行下列作業︰

1. 在針對只有接聽宣告的串流監視資料所建立的事件中樞上建立共用存取原則。 這可以在入口網站中完成。 例如，您可能會將它稱為 “monitoringReadOnly”。 可能的話，您會直接將該金鑰提供給取用者，並略過下一個步驟。
2. 如果消費者需要能夠獲取金鑰臨時，則向使用者授予該事件中心的 ListKeys 操作。 對於需要設定診斷設定或記錄檔設定檔以串流至事件中樞的使用者而言，這也是必要的。 例如，您可能會建立 RBAC 規則︰
   
   ```powershell
   $role = Get-AzRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzRoleDefinition -Role $role 
   ```

## <a name="monitoring-within-a-secured-virtual-network"></a>在受保護虛擬網路內監視

Azure 監視器需要存取您的 Azure 資源，才能提供您啟用的服務。 如果您想要監視 Azure 資源，同時防止其存取公用網際網路，您可以啟用下列設定。

### <a name="secured-storage-accounts"></a>受保護的儲存體帳戶 

監視資料通常會寫入到儲存體帳戶。 您可能想要確定未經授權的使用者無法存取複製到儲存體帳戶的資料。 為了增加安全性，您可以限制儲存體帳戶使用「選取的網路」來鎖定網路存取權，只允許已授權的資源與信任的 Microsoft 服務存取儲存體帳戶。
![Azure 存儲設置](./media/roles-permissions-security/secured-storage-example.png)對話方塊 Azure 監視器被視為這些"受信任的 Microsoft 服務"之一 如果允許受信任的 Microsoft 服務訪問安全存儲，Azure 監視器將有權訪問安全存儲帳戶;如果允許受信任的 Microsoft 服務訪問安全存儲，則 Azure 監視器將有權訪問您的安全存儲帳戶。在這些受保護的條件下，啟用將 Azure 監視器資源日誌、活動日誌和指標寫入存儲帳戶。 這也會讓 Log Analytics 讀取來自受保護儲存體的記錄。   


如需詳細資訊，請參閱[網路安全性與 Azure 儲存體](../../storage/common/storage-network-security.md)

## <a name="next-steps"></a>後續步驟
* [深入了解 RBAC 和 Resource Manager 中的權限](../../role-based-access-control/overview.md)
* [閱讀 Azure 中的監視概觀](../../azure-monitor/overview.md)


