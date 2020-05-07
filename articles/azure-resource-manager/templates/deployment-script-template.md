---
title: 在範本中使用部署腳本 |Microsoft Docs
description: 在 Azure Resource Manager 範本中使用部署腳本。
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: jgao
ms.openlocfilehash: 14663e71126d8c201015996e3e4dc76976128bcc
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610797"
---
# <a name="use-deployment-scripts-in-templates-preview"></a>在範本中使用部署腳本（預覽）

瞭解如何在 Azure 資源範本中使用部署腳本。 使用者可以使用名`Microsoft.Resources/deploymentScripts`為的新資源類型，在範本部署中執行部署腳本，並檢查執行結果。 這些腳本可以用來執行自訂步驟，例如：

- 將使用者新增至目錄
- 執行資料平面作業，例如複製 blob 或種子資料庫
- 查詢和驗證授權金鑰
- 建立自我簽署憑證
- 在 Azure AD 中建立物件
- 從自訂系統查詢 IP 位址區塊

部署腳本的優點：

- 容易編寫程式碼、使用和 debug。 您可以在慣用的開發環境中開發部署腳本。 這些腳本可以內嵌在範本或外部腳本檔案中。
- 您可以指定指令碼語言和平臺。 目前支援在 Linux 環境上 Azure PowerShell 和 Azure CLI 部署腳本。
- 允許指定用來執行腳本的身分識別。 目前僅支援[Azure 使用者指派的受控識別](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)。
- 允許將命令列引數傳遞至腳本。
- 可以指定腳本輸出，並將它們傳回至部署。

部署腳本資源僅適用于可使用 Azure 容器實例的區域。  請參閱 azure[區域中 Azure 容器實例的資源可用性](../../container-instances/container-instances-region-availability.md)。

> [!IMPORTANT]
> 執行腳本和疑難排解時，需要儲存體帳戶和容器實例。 您可以選擇指定現有的儲存體帳戶，否則腳本服務會自動建立儲存體帳戶和容器實例。 當部署腳本在終端機狀態中執行時，腳本服務通常會刪除這兩個自動建立的資源。 在資源刪除之前，您需支付資源費用。 若要深入瞭解，請參閱[清理部署腳本資源](#clean-up-deployment-script-resources)。

## <a name="prerequisites"></a>Prerequisites

- **使用者指派的受控識別，具有對目標資源群組的參與者角色**。 此身分識別會用來執行部署指令碼。 若要在資源群組外部執行作業，您必須授與其他許可權。 例如，如果您想要建立新的資源群組，請將身分識別指派給訂用帳戶層級。

  > [!NOTE]
  > 腳本服務會建立儲存體帳戶（除非您指定現有的儲存體帳戶）和背景中的容器實例。  如果訂用帳戶尚未註冊 Azure 儲存體帳戶（Microsoft 儲存體）和 Azure 容器實例（Microsoft.containerinstance）資源提供者，則需要使用者指派的受控識別（在訂用帳戶層級具有參與者的角色）。

  若要建立身分識別，請參閱使用 Azure 入口網站或[使用 Azure CLI](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)[建立使用者指派的受控識別](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)，或使用[Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)。 您在部署範本時將需要身分識別的識別碼。 此身分識別的格式為：

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  藉由提供資源組名和識別名稱，使用下列 CLI 或 PowerShell 腳本來取得識別碼。

  # <a name="cli"></a>[CLI](#tab/CLI)

  ```azurecli-interactive
  echo "Enter the Resource Group name:" &&
  read resourceGroupName &&
  echo "Enter the managed identity name:" &&
  read idName &&
  az identity show -g jgaoidentity1008rg -n jgaouami --query id
  ```

  # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name $idName).Id
  ```

  ---

- **Azure PowerShell**或**Azure CLI**。 如需支援的 Azure PowerShell 版本清單，請參閱[這裡](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)。如需支援的 Azure CLI 版本清單，請參閱[這裡](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)。

    >[!IMPORTANT]
    > 部署腳本會使用 Microsoft Container Registry （MCR）中的可用 CLI 映射。 需要約一個月的時間來認證適用于部署腳本的 CLI 映射。 請勿使用在30天內發行的 CLI 版本。 若要尋找映射的發行日期，請參閱[Azure CLI 版本](https://docs.microsoft.com/cli/azure/release-notes-azure-cli?view=azure-cli-latest)資訊。 如果使用不支援的版本，則錯誤訊息會列出支援的版本。

    您不需要這些版本來部署範本。 但在本機測試部署腳本需要這些版本。 請參閱[安裝 Azure PowerShell 模組](/powershell/azure/install-az-ps)。 您可以使用預先設定的 Docker 映射。  請參閱[設定開發環境](#configure-development-environment)。

## <a name="sample-templates"></a>範例範本

下列 json 是一個範例。  您可以在[這裡](/azure/templates/microsoft.resources/deploymentscripts)找到最新的範本架構。

```json
{
  "type": "Microsoft.Resources/deploymentScripts",
  "apiVersion": "2019-10-01-preview",
  "name": "runPowerShellInline",
  "location": "[resourceGroup().location]",
  "kind": "AzurePowerShell", // or "AzureCLI"
  "identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
      "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID": {}
    }
  },
  "properties": {
    "forceUpdateTag": 1,
    "containerSettings": {
      "containerGroupName": "mycustomaci"
    },
    "storageAccountSettings": {
      "storageAccountName": "myStorageAccount",
      "storageAccountKey": "myKey"
    },
    "azPowerShellVersion": "3.0",  // or "azCliVersion": "2.0.80"
    "arguments": "[concat('-name ', parameters('name'))]",
    "environmentVariables": [
      {
        "name": "someSecret",
        "secureValue": "if this is really a secret, don't put it here... in plain text..."
      }
    ],
    "scriptContent": "
      param([string] $name)
      $output = 'Hello {0}' -f $name
      Write-Output $output
      $DeploymentScriptOutputs = @{}
      $DeploymentScriptOutputs['text'] = $output
    ", // or "primaryScriptUri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
    "supportingScriptUris":[],
    "timeout": "PT30M",
    "cleanupPreference": "OnSuccess",
    "retentionInterval": "P1D"
  }
}
```

> [!NOTE]
> 此範例適用于示範用途。  **scriptContent**和**primaryScriptUris**不能並存于範本中。

屬性值詳細資料：

- 身分**識別**：部署腳本服務會使用使用者指派的受控識別來執行腳本。 目前只支援使用者指派的受控識別。
- **kind**：指定指令碼的類型。 目前，Azure PowerShell 和 Azure CLI 腳本都是支援的。 值為**azurepowershell 來**和**AzureCLI**。
- **forceUpdateTag**：在範本部署之間變更此值會強制部署腳本重新執行。 使用需要設定為參數之 defaultValue 的 newGuid （）或 utcNow （）函數。 若要深入了解，請參閱[執行指令碼多次](#run-script-more-than-once)。
- **containerSettings**：指定自訂 Azure 容器實例的設定。  **containerGroupName**是用來指定容器組名。  如果未指定，則會自動產生組名。
- **storageAccountSettings**：指定要使用現有儲存體帳戶的設定。 如果未指定，則會自動建立儲存體帳戶。 請參閱[使用現有的儲存體帳戶](#use-an-existing-storage-account)。
- **azPowerShellVersion**/**azCliVersion**：指定要使用的模組版本。 如需支援的 PowerShell 和 CLI 版本清單，請參閱[必要條件](#prerequisites)。
- **arguments**：指定參數值。 多個值應以空格分隔。
- **environmentVariables**：指定要傳遞至腳本的環境變數。 如需詳細資訊，請參閱[開發部署腳本](#develop-deployment-scripts)。
- **scriptContent**：指定指令碼內容。 若要執行外部腳本，請`primaryScriptUri`改用。 如需範例，請參閱[使用內嵌腳本](#use-inline-scripts)和[使用外部腳本](#use-external-scripts)。
- **primaryScriptUri**：將可公開存取的 Url 指定給具有支援副檔名的主要部署腳本。
- **supportingScriptUris**：指定可公開存取的 url 陣列，以支援在`ScriptContent`或`PrimaryScriptUri`中呼叫的檔案。
- **timeout**：指定以 [ISO 8601 格式](https://en.wikipedia.org/wiki/ISO_8601)指定的指令碼執行時間允許上限。 預設值為 **P1D**。
- **cleanupPreference**。 指定當腳本執行處於終止狀態時，清除部署資源的喜好設定。 預設設定**一律**為，這表示刪除資源時（儘管終端機狀態為 [成功]、[失敗]、[已取消]）。 若要深入了解，請參閱[清除部署指令碼資源](#clean-up-deployment-script-resources)。
- **retentionInterval**：指定服務在部署腳本執行達到結束狀態之後，保留部署腳本資源的間隔。 當此持續時間到期時，將會刪除部署腳本資源。 持續時間是以[ISO 8601 模式](https://en.wikipedia.org/wiki/ISO_8601)為基礎。 預設值為**P1D**，這表示七天。 當 cleanupPreference 設定為 *OnExpiration* 時，就會使用此屬性。 目前未啟用*OnExpiration*屬性。 若要深入了解，請參閱[清除部署指令碼資源](#clean-up-deployment-script-resources)。

### <a name="additional-samples"></a>其他範例

- [建立憑證並將其指派給金鑰保存庫](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json)

- [建立使用者指派的受控識別，並將其指派給資源群組，並執行部署腳本](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault-mi.json)。

> [!NOTE]
> 建議您建立使用者指派的身分識別，並事先授與許可權。 如果您在執行部署腳本的相同範本中建立身分識別並授與許可權，您可能會收到登入和許可權相關的錯誤。 需要一些時間，許可權才會生效。

## <a name="use-inline-scripts"></a>使用內嵌腳本

下列範本有一個以`Microsoft.Resources/deploymentScripts`類型定義的資源。 反白顯示的部分是內嵌腳本。

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json" range="1-54" highlight="34-40":::

> [!NOTE]
> 因為內嵌部署指令碼會以雙引號括住，所以部署指令碼內的字串必須以單引號括住。 PowerShell 的逸出字元是 **&#92;** 。 您也可以考慮使用字串替代，如先前的 JSON 範例所示。 請參閱 name 參數的預設值。

腳本接受一個參數，並輸出參數值。 **DeploymentScriptOutputs**是用來儲存輸出。  在 [輸出] 區段中，[**值**] 行會顯示如何存取儲存的值。 `Write-Output`用於偵錯工具。 若要瞭解如何存取輸出檔，請參閱[調試腳本](#debug-deployment-scripts)。  如需屬性描述，請參閱[範例範本](#sample-templates)。

若要執行腳本，請選取 [**試試看**] 開啟 Cloud Shell，然後將下列程式碼貼入 [Shell] 窗格中。

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to be created"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$id = Read-Host -Prompt "Enter the user-assigned managed identity ID"

New-AzResourceGroup -Name $resourceGroupName -Location $location

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json" -identity $id

Write-Host "Press [ENTER] to continue ..."
```

輸出看起來如下：

![Resource Manager 範本部署腳本 hello world 輸出](./media/deployment-script-template/resource-manager-template-deployment-script-helloworld-output.png)

## <a name="use-external-scripts"></a>使用外部腳本

除了內嵌腳本之外，您也可以使用外部腳本檔案。 僅支援具有**ps1**副檔名的主要 PowerShell 腳本。 針對 CLI 腳本，只要腳本是有效的 bash 腳本，主要腳本就可以有任何延伸模組（或不含擴充功能）。 若要使用外部腳本檔案， `scriptContent`請`primaryScriptUri`將取代為。 例如：

```json
"primaryScriptURI": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
```

若要查看範例，請選取[這裡](https://github.com/Azure/azure-docs-json-samples/blob/master/deployment-script/deploymentscript-helloworld-primaryscripturi.json)。

外部腳本檔案必須可供存取。  若要保護儲存在 Azure 儲存體帳戶中的腳本檔案，請參閱[使用 SAS 權杖部署私人 ARM 範本](./secure-template-with-sas-token.md)。

您必須負責確保部署腳本（ **PrimaryScriptUri**或**SupportingScriptUris**）所參考的腳本完整性。  只參考您信任的腳本。

## <a name="use-supporting-scripts"></a>使用支援的腳本

您可以將複雜的邏輯分隔成一個或多個支援的腳本檔案。 `supportingScriptURI`屬性可讓您視需要將 uri 陣列提供給支援的腳本檔案：

```json
"scriptContent": "
    ...
    ./Create-Cert.ps1
    ...
"

"supportingScriptUris": [
  "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/create-cert.ps1"
],
```

您可以從內嵌腳本和主要腳本檔案呼叫支援的腳本檔案。 支援的腳本檔案對副檔名沒有任何限制。

支援的檔案會在執行時間複製到 azscripts/azscriptinput。 使用相對路徑來參考內嵌腳本和主要腳本檔案中的支援檔案。

## <a name="work-with-outputs-from-powershell-script"></a>使用 PowerShell 腳本的輸出

下列範本顯示如何在兩個 deploymentScripts 資源之間傳遞值：

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic.json" range="1-84" highlight="39-40,66":::

在第一個資源中，您會定義名為 **$DeploymentScriptOutputs**的變數，並使用它來儲存輸出值。 若要從範本內的另一個資源存取輸出值，請使用：

```json
reference('<ResourceName>').output.text
```

## <a name="work-with-outputs-from-cli-script"></a>使用 CLI 腳本的輸出

不同于 PowerShell 部署腳本，CLI/bash 支援不會公開用來儲存腳本輸出的一般變數，而是會有稱為**AZ_SCRIPTS_OUTPUT_PATH**的環境變數，以儲存腳本輸出檔案所在的位置。 如果部署腳本是從 Resource Manager 範本執行，Bash shell 會自動為您設定此環境變數。

部署腳本輸出必須儲存在 AZ_SCRIPTS_OUTPUT_PATH 位置，而且輸出必須是有效的 JSON 字串物件。 檔案的內容必須儲存為索引鍵/值組。 例如，字串的陣列會儲存為 {"MyResult"： ["foo"，"bar"]}。  只儲存陣列結果（例如 ["foo"，"bar"]）是不正確。

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic-cli.json" range="1-44" highlight="32":::

[jq](https://stedolan.github.io/jq/)是在上一個範例中使用。 它隨附容器映射。 請參閱[設定開發環境](#configure-development-environment)。

## <a name="develop-deployment-scripts"></a>開發部署腳本

### <a name="handle-non-terminating-errors"></a>處理非終止錯誤

您可以使用部署腳本中的[**$ErrorActionPreference**](/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-7#erroractionpreference
)變數，控制 PowerShell 如何回應非終止錯誤。 腳本服務不會設定/變更此值。  儘管您為 $ErrorActionPreference 設定的值，部署腳本會在腳本遇到錯誤時，將資源布建狀態設定為 [*失敗*]。

### <a name="pass-secured-strings-to-deployment-script"></a>將安全的字串傳遞至部署腳本

在您的容器實例中設定環境變數（EnvironmentVariable），可讓您提供由容器執行之應用程式或腳本的動態設定。 部署腳本會以與 Azure 容器實例相同的方式來處理不安全且安全的環境變數。 如需詳細資訊，請參閱[在容器實例中設定環境變數](../../container-instances/container-instances-environment-variables.md#secure-values)。

## <a name="debug-deployment-scripts"></a>Debug 部署腳本

腳本服務會建立[儲存體帳戶](../../storage/common/storage-account-overview.md)（除非您指定現有的儲存體帳戶）和用於執行腳本的[容器實例](../../container-instances/container-instances-overview.md)。 如果這些資源是由腳本服務自動建立，則這兩個資源在資源名稱中會有**azscripts**尾碼。

![Resource Manager 範本部署腳本資源名稱](./media/deployment-script-template/resource-manager-template-deployment-script-resources.png)

使用者腳本、執行結果和 stdout 檔案會儲存在儲存體帳戶的檔案共用中。 有一個名為**azscripts**的資料夾。 在資料夾中，輸入和輸出檔案有兩個資料夾： **azscriptinput**和**azscriptoutput**。

輸出檔案夾包含 **executionresult.json** 和指令碼輸出檔案。 您可以在**executionresult**中看到腳本執行錯誤訊息。 只有在腳本執行成功時，才會建立輸出檔案。 輸入資料夾包含系統 PowerShell 指令檔和使用者部署指令檔。 您可以使用已修改的檔案來取代使用者部署腳本檔案，然後從 Azure 容器實例重新執行部署腳本。

您可以使用 REST API，在資源群組層級和訂用帳戶層級取得部署腳本資源部署資訊：

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>?api-version=2019-10-01-preview
```

```rest
/subscriptions/<SubscriptionID>/providers/microsoft.resources/deploymentScripts?api-version=2019-10-01-preview
```

下列範例會使用[ARMClient](https://github.com/projectkudu/ARMClient)：

```azurepowershell
armclient login
armclient get /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourcegroups/myrg/providers/microsoft.resources/deploymentScripts/myDeployementScript?api-version=2019-10-01-preview
```

輸出如下：

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-status.json" range="1-37" highlight="15,34":::

輸出會顯示部署狀態，以及部署腳本資源識別碼。

下列 REST API 會傳回記錄檔：

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

只有在刪除部署腳本資源之前，才可運作。

若要在入口網站中查看 deploymentScripts 資源，請選取 [**顯示隱藏的類型**]：

![Resource Manager 範本部署腳本、顯示隱藏的類型、入口網站](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="use-an-existing-storage-account"></a>使用現有儲存體帳戶

執行腳本和疑難排解時，需要儲存體帳戶和容器實例。 您可以選擇指定現有的儲存體帳戶，否則腳本服務會自動建立儲存體帳戶和容器實例。 使用現有儲存體帳戶的需求：

- 支援的儲存體帳戶種類包括：一般用途 v2 帳戶、一般用途 v1 帳戶和 fileStorage 帳戶。 如需詳細資訊，請參閱[儲存體帳戶的類型](../../storage/common/storage-account-overview.md)。
- 必須關閉儲存體帳戶防火牆規則。 請參閱[設定 Azure 儲存體防火牆和虛擬網路](../../storage/common/storage-network-security.md)
- 部署腳本的使用者指派受控識別必須具有管理儲存體帳戶的許可權，其中包括讀取、建立、刪除檔案共用。

若要指定現有的儲存體帳戶，請將下列 json 新增至的屬性`Microsoft.Resources/deploymentScripts`元素：

```json
"storageAccountSettings": {
  "storageAccountName": "myStorageAccount",
  "storageAccountKey": "myKey"
},
```

如需完整`Microsoft.Resources/deploymentScripts`的定義範例，請參閱[範例範本](#sample-templates)。

使用現有的儲存體帳戶時，腳本服務會建立具有唯一名稱的檔案共用。 如需腳本服務如何清除檔案共用的詳細說明，請參閱[清除部署腳本資源](#clean-up-deployment-script-resources)。

## <a name="clean-up-deployment-script-resources"></a>清除部署腳本資源

執行腳本和疑難排解時，需要儲存體帳戶和容器實例。 您可以選擇指定現有的儲存體帳戶，否則腳本服務會自動建立儲存體帳戶和容器實例。 當部署腳本在終端機狀態中執行時，腳本服務會刪除這兩個自動建立的資源。 在資源刪除之前，您需支付資源費用。 如需價格資訊，請參閱[容器實例定價](https://azure.microsoft.com/pricing/details/container-instances/)和[Azure 儲存體定價](https://azure.microsoft.com/pricing/details/storage/)。

這些資源的生命週期是由範本中的下列屬性所控制：

- **cleanupPreference**：在腳本執行于結束狀態時清除喜好設定。 支援的值為：

  - **Always**：一旦腳本執行進入結束狀態，即刪除自動建立的資源。 如果使用現有的儲存體帳戶，腳本服務會刪除在儲存體帳戶中建立的檔案共用。 由於 deploymentScripts 資源可能會在資源清除後仍存在，因此腳本服務會在刪除資源之前保存腳本執行結果，例如 stdout、輸出、傳回值等等。
  - **OnSuccess**：只有在腳本執行成功時，才刪除自動建立的資源。 如果使用現有的儲存體帳戶，腳本服務只會在腳本執行成功時，才會移除檔案共用。 您仍然可以存取資源來尋找 debug 資訊。
  - **OnExpiration**：只有在**retentionInterval**設定過期時，才刪除自動資源。 如果使用現有的儲存體帳戶，腳本服務會移除檔案共用，但會保留儲存體帳戶。

- **retentionInterval**：指定將保留腳本資源的時間間隔，之後將會過期和刪除。

> [!NOTE]
> 不建議將部署腳本資源用於其他用途。

## <a name="run-script-more-than-once"></a>執行腳本超過一次

部署腳本執行是等冪運算。 如果未變更任何 deploymentScripts 資源屬性（包括內嵌腳本），則當您重新部署範本時，將不會執行此腳本。 部署腳本服務會比較範本中的資源名稱與相同資源群組中的現有資源。 如果您想要多次執行相同的部署腳本，有兩個選項：

- 變更 deploymentScripts 資源的名稱。 例如，使用[utcNow](./template-functions-date.md#utcnow)範本函式做為資源名稱，或做為資源名稱的一部分。 變更資源名稱會建立新的 deploymentScripts 資源。 這很適合用來保存腳本執行的歷程記錄。

    > [!NOTE]
    > UtcNow 函數只能用於參數的預設值。

- 在 [ `forceUpdateTag`範本] 屬性中指定不同的值。  例如，使用 utcNow 作為值。

> [!NOTE]
> 撰寫具有等冪性的部署腳本。 這可確保如果不小心再次執行，則不會造成系統變更。 例如，如果部署腳本是用來建立 Azure 資源，請先確認資源不存在再建立，否則腳本將會成功，否則您就不會重新建立資源。

## <a name="configure-development-environment"></a>設定開發環境

您可以使用預先設定的 docker 容器映射作為部署腳本開發環境。 下列程式說明如何在 Windows 上設定 docker 映射。 針對 Linux 和 Mac，您可以在網際網路上找到相關資訊。

1. 在您的開發電腦上安裝[Docker Desktop](https://www.docker.com/products/docker-desktop) 。
1. 開啟 [Docker Desktop]。
1. 從 [工作列] 選取 [Docker 桌面] 圖示，然後選取 [**設定**]。
1. 選取 [**共用磁片磁碟機**]，選取您要提供給容器的本機磁片磁碟機，**然後選取 [** 套用]

    ![Resource Manager 範本部署腳本 docker 磁片磁碟機](./media/deployment-script-template/resource-manager-deployment-script-docker-setting-drive.png)

1. 在提示字元中輸入您的 windows 認證。
1. 開啟終端機視窗，不論是命令提示字元或 Windows PowerShell （請勿使用 PowerShell ISE）。
1. 將部署腳本容器映射提取到本機電腦：

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    此範例使用 PowerShell 版本2.7.0。

    若要從 Microsoft Container Registry （MCR）提取 CLI 映射：

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    這個範例會使用版本 CLI 2.0.80。 部署腳本會使用[此處](https://hub.docker.com/_/microsoft-azure-cli)找到的預設 CLI 容器映射。

1. 在本機執行 docker 映射。

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    以共用磁片磁碟機上的現有資料夾取代** &lt;主機驅動程式代號>** 和** &lt;主機目錄名稱>** 。  它會將資料夾對應至容器中的 **/data**資料夾。 如需範例，請對應 D:\docker：

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **-這**表示讓容器映射保持運作狀態。

    CLI 範例：

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. 當您收到提示時，請選取 [**共用**]。
1. 下列螢幕擷取畫面顯示如何在 d:\docker 資料夾中有 helloworld 檔案時，執行 PowerShell 腳本。

    ![Resource Manager 範本部署腳本 docker cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

成功測試腳本之後，您可以使用它作為部署腳本。

## <a name="next-steps"></a>後續步驟

在本文中，您已瞭解如何使用部署腳本。 若要逐步執行部署腳本教學課程：

> [!div class="nextstepaction"]
> [教學課程：在 Azure Resource Manager 範本中使用部署腳本](./template-tutorial-deployment-script.md)