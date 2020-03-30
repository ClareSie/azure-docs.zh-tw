---
title: 受控身分識別
description: 瞭解託管標識在 Azure 應用服務和 Azure 函數中的工作方式，以及如何配置託管標識並為後端資源生成權杖。
author: mattchenderson
ms.topic: article
ms.date: 03/04/2020
ms.author: mahender
ms.reviewer: yevbronsh
ms.openlocfilehash: 6e3169f2bfcba0a02af1490f875cbab8a14d02f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280023"
---
# <a name="how-to-use-managed-identities-for-app-service-and-azure-functions"></a>如何使用 App Service 和 Azure Functions 的受控身分識別

> [!Important] 
> 如果您跨越訂用帳戶/租用戶移轉應用程式，App Service 和 Azure Functions 的受控身分識別將無法正常運作。 應用程式將必須取得新的身分識別，這可透過停用並重新啟用功能來實現。 請參閱下方的[移除身分識別](#remove)。 下游資源也必須更新存取原則，才能使用新的身分識別。

此主題示範如何為 App Service 和 Azure Functions 應用程式建立受控應用程式身分識別，以及如何使用它來存取其他資源。 Azure 活動目錄 （AAD） 的託管標識允許應用輕鬆訪問其他受 AAD 保護的資源，如 Azure 金鑰保存庫。 身分識別由 Azure 平台負責管理，因此您不需要佈建或輪替任何密碼。 如需有關 ADD 中受控身分識別的詳細資訊，請參閱 [Azure 資源的受控身分識別](../active-directory/managed-identities-azure-resources/overview.md)。

您的應用程式可以授與兩種類型的身分識別： 
- **系統指派的身分識別**會繫結至您的應用程式，如果您的應用程式已刪除，則會被刪除。 應用程式只能有一個系統指派的身分識別。
- **使用者分配的標識**是可分配給應用的獨立 Azure 資源。 應用程式可以有多個使用者指派的身分識別。

## <a name="add-a-system-assigned-identity"></a>添加系統分配的標識

若要建立採用系統指派的身分識別的應用程式，您必須在應用程式上設定額外的屬性。

### <a name="using-the-azure-portal"></a>使用 Azure 入口網站

若要在入口網站中設定受控身分識別，您需要先像平常一樣建立應用程式，然後再啟用此功能。

1. 像平常一樣在入口網站中建立應用程式。 在入口網站中瀏覽至該應用程式。

2. 如果您使用函式應用程式，請瀏覽至 [平台功能]****。 若使用類型的應用程式，請在左側導覽列中向下捲動到 [設定]****。

3. 選擇**標識**。

4. 在 [系統指派]**** 索引標籤內，將 [狀態]**** 切換為 [開啟]****。 按一下 [儲存]****。

    ![App Service 中的受控身分識別](media/app-service-managed-service-identity/system-assigned-managed-identity-in-azure-portal.png)

### <a name="using-the-azure-cli"></a>使用 Azure CLI

若要使用 Azure CLI 設定受控身分識別，您必須對現有的應用程式使用 `az webapp identity assign` 命令。 有三個選項可供您執行本節中的範例︰

- 從 Azure 入口網站使用 [Azure Cloud Shell](../cloud-shell/overview.md)。
- 通過位於下面每個代碼塊右上角的"試用"按鈕使用嵌入的 Azure 雲外殼。
- 如果您偏好使用本機 CLI 主控台，請[安裝最新版的 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 或更新版本)。 

下列步驟將逐步引導您建立 web 應用程式，並使用 CLI 指派身分識別給它：

1. 如果您要在本機主控台中使用 Azure CLI，請先使用 [az login](/cli/azure/reference-index#az-login) 登入 Azure。 使用與 Azure 訂閱關聯的帳戶，您可以在該訂閱下部署應用程式：

    ```azurecli-interactive
    az login
    ```
2. 使用 CLI 建立 web 應用程式。 如需如何使用 CLI 搭配 App Service 的相關範例，請參閱 [App Service CLI 範例](../app-service/samples-cli.md)：

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myPlan --resource-group myResourceGroup --sku S1
    az webapp create --name myApp --resource-group myResourceGroup --plan myPlan
    ```

3. 執行 `identity assign` 命令來建立此應用程式的身分識別：

    ```azurecli-interactive
    az webapp identity assign --name myApp --resource-group myResourceGroup
    ```

### <a name="using-azure-powershell"></a>使用 Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

下列步驟將逐步引導您建立 Web 應用程式，並使用 Azure PowerShell 指派身分識別給它：

1. 如果需要，請使用 Azure PowerShell 指南中的說明安裝 Azure [PowerShell，](/powershell/azure/overview)然後運行`Login-AzAccount`以創建與 Azure 的連接。

2. 使用 Azure PowerShell 建立 Web 應用程式。 如需更多如何使用 Azure PowerShell 搭配 App Service 的相關範例，請參閱 [App Service PowerShell 範例](../app-service/samples-powershell.md)：

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzResourceGroup -Name myResourceGroup -Location $location
    
    # Create an App Service plan in Free tier.
    New-AzAppServicePlan -Name $webappname -Location $location -ResourceGroupName myResourceGroup -Tier Free
    
    # Create a web app.
    New-AzWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName myResourceGroup
    ```

3. 執行 `Set-AzWebApp -AssignIdentity` 命令來建立此應用程式的身分識別：

    ```azurepowershell-interactive
    Set-AzWebApp -AssignIdentity $true -Name $webappname -ResourceGroupName myResourceGroup 
    ```

### <a name="using-an-azure-resource-manager-template"></a>使用 Azure Resource Manager 範本

您可以使用 Azure Resource Manager 範本來將 Azure 資源的部署自動化。 若要深入了解如何部署到 App Service 和 Functions，請參閱[在 App Service 中將資源部署自動化](../app-service/deploy-complex-application-predictably.md)和[在 Azure Functions 中將資源部署自動化](../azure-functions/functions-infrastructure-as-code.md)。

對於所有 `Microsoft.Web/sites` 型別的資源來說，您可以在資源定義中加入以下屬性，以建立採用身分識別的資源：
```json
"identity": {
    "type": "SystemAssigned"
}    
```

> [!NOTE] 
> 應用程式可以同時具有系統指派的身分識別和使用者指派的身分識別。 在此情況下，`type` 屬性將會是 `SystemAssigned,UserAssigned`

新增系統指派的類型能告訴 Azure 該如何建立及管理應用程式的身分識別。

例如，Web 應用程式可能與下圖中的範例相似：
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

當網站建立時，它擁有以下額外屬性：
```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

tenantId 屬性能辨識身分識別所隸屬的 AAD 租用戶。 principalId 是應用程式新身分識別的唯一識別碼。 在 AAD 內，服務主體的名稱與您提供給 App Service 或 Azure Functions 執行個體的名稱相同。


## <a name="add-a-user-assigned-identity"></a>添加使用者分配的標識

利用使用者指派的身分識別建立應用程式會需要您建立身分識別，然後將其資源識別碼新增到您的應用程式設定中。

### <a name="using-the-azure-portal"></a>使用 Azure 入口網站

首先，您必須建立使用者指派的身分識別資源。

1. 根據[這些指示](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)建立使用者指派的受控識別資源。

2. 像平常一樣在入口網站中建立應用程式。 在入口網站中瀏覽至該應用程式。

3. 如果您使用函式應用程式，請瀏覽至 [平台功能]****。 若使用類型的應用程式，請在左側導覽列中向下捲動到 [設定]****。

4. 選擇**標識**。

5. 在 **"使用者分配**"選項卡中，按一下"**添加**"。

6. 搜尋您之前建立的身分識別，並加以選取。 按一下 **[新增]**。

    ![App Service 中的受控身分識別](media/app-service-managed-service-identity/user-assigned-managed-identity-in-azure-portal.png)

### <a name="using-an-azure-resource-manager-template"></a>使用 Azure Resource Manager 範本

您可以使用 Azure Resource Manager 範本來將 Azure 資源的部署自動化。 若要深入了解如何部署到 App Service 和 Functions，請參閱[在 App Service 中將資源部署自動化](../app-service/deploy-complex-application-predictably.md)和[在 Azure Functions 中將資源部署自動化](../azure-functions/functions-infrastructure-as-code.md)。

對於所有 `Microsoft.Web/sites` 型別的資源來說，您可以在資源定義中加入以下區塊，以所需之身分識別的資源識別碼取代 `<RESOURCEID>`，來建立採用身分識別的資源：
```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {}
    }
}    
```

> [!NOTE] 
> 應用程式可以同時具有系統指派的身分識別和使用者指派的身分識別。 在此情況下，`type` 屬性將會是 `SystemAssigned,UserAssigned`

添加使用者分配的類型告訴 Azure 使用為應用程式指定的使用者分配的標識。

例如，Web 應用程式可能與下圖中的範例相似：
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]": {}
        }
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
    ]
}
```

當網站建立時，它擁有以下額外屬性：
```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {
            "principalId": "<PRINCIPALID>",
            "clientId": "<CLIENTID>"
        }
    }
}
```

主體 Id 是用於 AAD 管理的標識的唯一識別碼。 clientId 是應用程式新標識的唯一識別碼，用於指定在運行時調用期間要使用的標識。


## <a name="obtain-tokens-for-azure-resources"></a>獲取 Azure 資源的權杖

應用可以使用其託管標識獲取權杖來訪問受 AAD 保護的其他資源，如 Azure 金鑰保存庫。 這些權杖代表存取資源的應用程式，而不是任何特定的應用程式使用者。 

您可能需要設定目標資源，讓應用程式得以存取。 例如，如果請求權杖訪問金鑰保存庫，則需要確保添加了包含應用程式標識的訪問策略。 否則即使呼叫含有權杖，依然會遭到拒絕。 若要深入了解哪些資源支援 Azure Active Directory 權杖，請參閱[支援 Azure AD 驗證的 Azure 服務](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)。

> [!IMPORTANT]
> 託管標識的後端服務可為每個資源 URI 保持大約 8 小時的緩存。 如果更新特定目標資源的訪問策略並立即檢索該資源的權杖，則可以繼續獲取具有過時許可權的緩存權杖，直到該權杖過期。 當前無法強制刷新權杖。

有一個簡單的 REST 通訊協定可用來在 App Service 和 Azure Functions 中取得權杖。 這可用於所有應用程式和語言。 對於 .NET 和 JAVA，Azure SDK 在此協定上提供了抽象，並有助於本地開發體驗。

### <a name="using-the-rest-protocol"></a>使用 REST 通訊協定

採用受控身分識別的應用程式有兩個已定義的環境變數：

- MSI_ENDPOINT - 本機權杖服務的 URL。
- MSI_SECRET - 用於協助減輕伺服器端要求偽造 (SSRF) 攻擊的標頭。 值會由平台旋轉。

**MSI_ENDPOINT** 是應用程式要求權杖的來源本機 URL。 若要取得資源的權杖，請向該端點提出包含以下參數的 HTTP GET 要求：

> |參數名稱|在|描述|
> |-----|-----|-----|
> |resource|查詢|資源的 AAD 資源 URI，也就是要取得權杖的目標資源。 這可能是其中一個[支援 Azure AD 驗證的 Azure 服務](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)，或任何其他資源 URI。|
> |api-version|查詢|要使用的權杖 API 版本。 目前唯一支援的版本為 "2017-09-01"。|
> |secret|頁首|MSI_SECRET 環境變數的值。 此標頭用來協助減輕伺服器端要求偽造 (SSRF) 攻擊。|
> |clientid|查詢|（可選，除非使用者分配）要使用的使用者分配標識的 ID。 如果省略，則使用系統指派的身分識別。|

> [!IMPORTANT]
> 如果嘗試獲取使用者分配標識的權杖，則必須包括 該`clientid`屬性。 否則，權杖服務將嘗試獲取系統分配的標識的權杖，該權杖可能存在，也可能不存在。

成功的 200 OK 回應包括含以下屬性的 JSON 本文：

> |屬性名稱|描述|
> |-------------|----------|
> |access_token|所要求的存取權杖。 呼叫端 Web 服務可以使用此權杖來向接收端 Web 服務進行驗證。|
> |expires_on|存取權杖的到期時間。 日期會表示為從 1970-01-01T0:0:0Z UTC 至到期時間的秒數。 這個值用來判斷快取權杖的存留期。|
> |resource|接收端 Web 服務的應用程式識別碼 URI。|
> |token_type|表示權杖類型值。 Azure AD 唯一支援的類型是 Bearer。 有關無記名權杖的詳細資訊，請參閱[OAuth 2.0 授權框架：無記名權杖使用 （RFC 6750）。](https://www.rfc-editor.org/rfc/rfc6750.txt)|

該回應與 [AAD 服務對服務存取權杖要求的回應](../active-directory/develop/v2-oauth2-client-creds-grant-flow.md#get-a-token)。

> [!NOTE]
> 環境變數會在程序初次啟動時進行設定，因此您在應用程式上啟用受控身分識別前，可能需要重新啟動應用程式或重新部署其程式碼，才可讓您的程式碼使用 `MSI_ENDPOINT` 和 `MSI_SECRET`。

### <a name="rest-protocol-examples"></a>REST 通訊協定範例

範例要求如下所示：

```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```

範例回應如下所示：

```
HTTP/1.1 200 OK
Content-Type: application/json

{
    "access_token": "eyJ0eXAi…",
    "expires_on": "09/14/2017 00:00:00 PM +00:00",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer"
}
```

### <a name="code-examples"></a>程式碼範例

# <a name="net"></a>[.NET](#tab/dotnet)

> [!TIP]
> 對於 .NET 語言，您也可以使用 [Microsoft.Azure.Services.AppAuthentication](#asal) 而不需要自行製作要求。

```csharp
private readonly HttpClient _client;
// ...
public async Task<HttpResponseMessage> GetToken(string resource)  {
    var request = new HttpRequestMessage(HttpMethod.Get, 
        String.Format("{0}/?resource={1}&api-version=2017-09-01", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource));
    request.Headers.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await _client.SendAsync(request);
}
```

# <a name="javascript"></a>[JAVAscript](#tab/javascript)

```javascript
const rp = require('request-promise');
const getToken = function(resource, cb) {
    let options = {
        uri: `${process.env["MSI_ENDPOINT"]}/?resource=${resource}&api-version=2017-09-01`,
        headers: {
            'Secret': process.env["MSI_SECRET"]
        }
    };
    rp(options)
        .then(cb);
}
```

# <a name="python"></a>[Python](#tab/python)

```python
import os
import requests

msi_endpoint = os.environ["MSI_ENDPOINT"]
msi_secret = os.environ["MSI_SECRET"]

def get_bearer_token(resource_uri):
    token_auth_uri = f"{msi_endpoint}?resource={resource_uri}&api-version=2017-09-01"
    head_msi = {'Secret':msi_secret}

    resp = requests.get(token_auth_uri, headers=head_msi)
    access_token = resp.json()['access_token']

    return access_token
```

# <a name="powershell"></a>[電源外殼](#tab/powershell)

```powershell
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:MSI_ENDPOINT + "?resource=$resourceURI&api-version=2017-09-01"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"Secret"="$env:MSI_SECRET"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

---

### <a name="using-the-microsoftazureservicesappauthentication-library-for-net"></a><a name="asal"></a>將 Microsoft.Azure.Services.AppAuthentication 程式庫運用在 .NET 上

對於 .NET 應用程式和函式來說，採用受控身分識別最簡單的方法就是透過 Microsoft.Azure.Services.AppAuthentication 套件。 該程式庫還能讓您使用來自 Visual Studio、[Azure CLI](/cli/azure) 或 Active Directory 整合式驗證的使用者帳戶，在部署機器上以本機方式測試程式碼。 如需使用此程式庫之本機開發選項的詳細資訊，請參閱 [Microsoft.Azure.Services.AppAuthentication 參考]。 本節示範如何在您的程式碼中開始使用程式庫。

1. 將對 [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) 及任何其他必要 NuGet 套件的參考新增到您的應用程式。 以下範例也使用 [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)。

2. 將下列程式碼新增至您的應用程式，將其目標修改成正確的資源。 此範例示範兩種使用 Azure Key Vault 的方式：

    ```csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;
    // ...
    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
    // OR
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
    ```

若要深入了解 Microsoft.Azure.Services.AppAuthentication 和它公開的作業，請參閱 [Microsoft.Azure.Services.AppAuthentication 參考]與[採用 MSI 的 App Service 和 KeyVault .NET 範例](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)。

### <a name="using-the-azure-sdk-for-java"></a>使用 JAVA 的 Azure SDK

對於 JAVA 應用程式和函數，使用託管標識的最簡單方法是通過 JAVA[的 Azure SDK。](https://github.com/Azure/azure-sdk-for-java) 本節示範如何在您的程式碼中開始使用程式庫。

1. 添加對 Azure [SDK 庫](https://mvnrepository.com/artifact/com.microsoft.azure/azure)的引用。 對於 Maven 專案，您可以將此程式碼片段添加到`dependencies`專案的 POM 檔部分：

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure</artifactId>
        <version>1.23.0</version>
    </dependency>
    ```

2. 使用物件`AppServiceMSICredentials`進行身份驗證。 此示例演示如何使用此機制使用 Azure 金鑰保存庫：

    ```java
    import com.microsoft.azure.AzureEnvironment;
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.keyvault.Vault
    //...
    Azure azure = Azure.authenticate(new AppServiceMSICredentials(AzureEnvironment.AZURE))
            .withSubscription(subscriptionId);
    Vault myKeyVault = azure.vaults().getByResourceGroup(resourceGroup, keyvaultName);

    ```


## <a name="remove-an-identity"></a><a name="remove"></a>刪除標識

您可用建立身分識別的相同方式，使用入口網站、PowerShell 或 CLI 停用功能，來將系統指派的身分識別移除。 使用者指派的身分識別可以個別移除。 要刪除所有標識，請將類型設置為[ARM 範本](#using-an-azure-resource-manager-template)中的"無"：

```json
"identity": {
    "type": "None"
}
```

以這種方式將系統指派的身分識別移除，也會從 AAD 刪除它。 當您刪除應用程式資源時，系統指派的身分識別會自動從 AAD 移除。

> [!NOTE]
> 還可以設定另一個應用程式設定：WEBSITE_DISABLE_MSI，這只會停用本機權杖服務。 不過，系統會將身分識別留在原地，且工具仍會將受控身分識別顯示為「開啟」或「已啟用」。 因此，不建議使用這個設定。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [使用受控身分識別安全地存取 SQL Database](app-service-web-tutorial-connect-msi.md)

[Microsoft.Azure.Services.AppAuthentication 參考]: https://go.microsoft.com/fwlink/p/?linkid=862452
