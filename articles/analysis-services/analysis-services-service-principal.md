---
title: 使用服務主體讓 Azure Analysis Services 工作自動化 | Microsoft Docs
description: 了解如何建立服務主體來自動化 Azure Analysis Services 管理工作。
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b04b9ababfe0e4c2a60d14044b9d3ee120837dc5
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96491038"
---
# <a name="automation-with-service-principals"></a>使用服務主體進行自動化

服務主體是您在租用戶內建立的 Azure Active Directory 應用程式資源，用來執行自動資源和服務層級作業。 它們是種唯一的「使用者身分識別」，具有應用程式識別碼和密碼或憑證。 服務主體只有執行角色所定義工作所需的權限，以及為此所指派給服務主體的權限。 

在 Analysis Services 中，服務主體會與 Azure 自動化、PowerShell 自動模式、自訂用戶端應用程式和 Web 應用程式搭配使用，讓一般工作自動化。 例如，佈建伺服器、部署模型、資料重新整理、擴大/縮減，以及暫停/繼續都可以藉由使用服務主體來進行自動化。 權限會透過角色成員資格指派給服務主體；這與一般 Azure AD UPN 帳戶的做法很像。

Analysis Services 也支援使用服務主體的受控識別所執行的作業。 若要深入了解，請參閱[適用於 Azure 資源的受控識別](../active-directory/managed-identities-azure-resources/overview.md)和[支援 Azure AD 驗證的 Azure 服務](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-analysis-services)。    

## <a name="create-service-principals"></a>建立服務主體
 
服務主體可以在 Azure 入口網站中建立，或是使用 PowerShell 建立。 若要深入了解，請參閱：

[建立服務主體 - Azure 入口網站](../active-directory/develop/howto-create-service-principal-portal.md)   
[建立服務主體 - PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>在 Azure 自動化中儲存認證和憑證資產

服務主體認證和憑證可以安全地儲存在 Azure 自動化中，以進行 Runbook 作業。 若要深入了解，請參閱：

[Azure 自動化中的認證資產](../automation/shared-resources/credentials.md)   
[Azure 自動化中的憑證資產](../automation/shared-resources/certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>將服務主體新增至伺服器管理員角色

在您針對 Analysis Services 伺服器管理作業使用服務主體之前，必須將其新增至伺服器管理員角色。 服務主體必須直接新增至伺服器管理員角色。 將服務主體新增至安全性群組，然後將該安全性群組新增至伺服器管理員角色並不受支援。 若要深入了解，請參閱[將服務主體新增至伺服器管理員角色](analysis-services-addservprinc-admins.md)。

## <a name="service-principals-in-connection-strings"></a>連接字串中的服務主體

服務主體 appID 和密碼或憑證可以在連接字串中使用，這點與 UPN 非常類似。

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

#### <a name="using-azanalysisservices-module"></a><a name="azmodule"></a>使用 Az.AnalysisServices 模組

透過 [Az.AnalysisServices](/powershell/module/az.analysisservices) 模組針對資源管理作業使用服務主體時，請使用 `Connect-AzAccount` Cmdlet。 

在下列範例中，會使用 appID 和密碼來執行控制平面作業，以同步處理唯讀複本並擴大/擴增：

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force
$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

# Connect using Az module
Connect-AzAccount -Credential $Credential -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx"

# Syncronize a database for query scale out
Sync-AzAnalysisServicesInstance -Instance "asazure://westus.asazure.windows.net/testsvr" -Database "testdb"

# Scale up the server to an S1, set 2 read-only replicas, and remove the primary from the query pool. The new replicas will hydrate from the synchronized data.
Set-AzAnalysisServicesServer -Name "testsvr" -ResourceGroupName "testRG" -Sku "S1" -ReadonlyReplicaCount 2 -DefaultConnectionMode Readonly
```

#### <a name="using-sqlserver-module"></a>使用 SQLServer 模組

在下列範例中，appID 和密碼可用來執行模型資料庫重新整理作業：

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full" -ServicePrincipal -ApplicationId $AppId -TenantId $TenantId -Credential $Credential
```

### <a name="amo-and-adomd"></a>AMO 和 ADOMD 

與用戶端應用程式和 Web 應用程式連線時，來自 NuGet 的 [AMO 和 ADOMD 用戶端程式庫](/analysis-services/client-libraries?view=azure-analysis-services-current&preserve-view=true) 15.0.2 版及更新版本的可安裝套件，會使用下列語法在連接字串中支援服務主體：`app:AppID` 和密碼或 `cert:thumbprint`。 

在下列範例中，`appID` 和 `password` 可用來執行模型資料庫重新整理作業：

```csharp
string appId = "xxx";
string authKey = "yyy";
string connString = $"Provider=MSOLAP;Data Source=asazure://westus.asazure.windows.net/<servername>;User ID=app:{appId};Password={authKey};";
Server server = new Server();
server.Connect(connString);
Database db = server.Databases.FindByName("adventureworks");
Table tbl = db.Model.Tables.Find("DimDate");
tbl.RequestRefresh(RefreshType.Full);
db.Model.SaveChanges();
```

## <a name="next-steps"></a>後續步驟
[使用 Azure PowerShell 登入](/powershell/azure/authenticate-azureps)   
[使用 Logic Apps 重新整理](analysis-services-refresh-logic-app.md)  
[使用 Azure 自動化重新整理](analysis-services-refresh-azure-automation.md)  
[將服務主體新增至伺服器管理員角色](analysis-services-addservprinc-admins.md)  
[使用服務主體將 Power BI Premium 工作區和資料集工作自動化](/power-bi/admin/service-premium-service-principal)