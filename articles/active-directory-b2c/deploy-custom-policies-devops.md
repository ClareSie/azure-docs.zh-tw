---
title: 使用 Azure Pipelines 部署自訂原則
titleSuffix: Azure AD B2C
description: 瞭解如何在 Azure DevOps Services 中使用 Azure Pipelines，在 CI/CD 管線中部署 Azure AD B2C 自訂原則。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/14/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 411fa207323a9bff6cfcc3b17769203c444dd844
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85388675"
---
# <a name="deploy-custom-policies-with-azure-pipelines"></a>使用 Azure Pipelines 部署自訂原則

藉由使用您在[Azure Pipelines][devops-pipelines]中設定的持續整合與傳遞（CI/CD）管線，您可以將 Azure AD B2C 自訂原則納入軟體傳遞和程式碼控制自動化中。 當您部署到不同的 Azure AD B2C 環境（例如開發、測試和生產）時，建議您移除手動程式，並使用 Azure Pipelines 來執行自動化測試。

啟用 Azure Pipelines 以管理 Azure AD B2C 內的自訂原則時，需要執行三個主要步驟：

1. 在您的 Azure AD B2C 租使用者中建立 web 應用程式註冊
1. 設定 Azure 存放庫
1. 設定 Azure 管線

> [!IMPORTANT]
> 使用 Azure 管線管理 Azure AD B2C 自訂原則目前會使用 Microsoft Graph API 端點上提供的**預覽**作業 `/beta` 。 不支援在生產應用程式中使用這些 API。 如需詳細資訊，請參閱[Microsoft Graph REST API Beta 版端點參考](https://docs.microsoft.com/graph/api/overview?toc=./ref/toc.json&view=graph-rest-beta)。

## <a name="prerequisites"></a>必要條件

* [Azure AD B2C 租](tutorial-create-tenant.md)使用者，以及具有[B2C IEF 原則系統管理員](../active-directory/users-groups-roles/directory-assign-admin-roles.md#b2c-ief-policy-administrator)角色之目錄中使用者的認證
* 已上傳至您租使用者的[自訂原則](custom-policy-get-started.md)
* 已在您的租使用者中註冊的[管理應用程式](microsoft-graph-get-started.md)，其中包含 Microsoft Graph API 許可權*原則。 ReadWrite. TrustFramework*
* [Azure 管線](https://azure.microsoft.com/services/devops/pipelines/)和[Azure DevOps Services 專案][devops-create-project]的存取權

## <a name="client-credentials-grant-flow"></a>用戶端認證授與流程

此處所述的案例會使用 OAuth 2.0[用戶端認證授與流程](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md)，在 Azure Pipelines 和 Azure AD B2C 之間使用服務對服務呼叫。 此授與流程可讓 Azure Pipelines （機密用戶端）之類的 web 服務使用自己的認證，而不是在呼叫另一個 web 服務（在此案例中為 Microsoft Graph API）時，模擬使用者以進行驗證。 Azure Pipelines 以非互動方式取得權杖，然後對 Microsoft Graph API 提出要求。

## <a name="register-an-application-for-management-tasks"></a>註冊管理工作的應用程式

如[必要條件](#prerequisites)中所述，您需要應用程式註冊，您的 PowerShell 腳本（由 Azure Pipelines 執行）可用於存取您租使用者中的資源。

如果您已經有用於自動化工作的應用程式註冊，請確定它已被授與在**Microsoft Graph**  >  **Policy**  >  應用程式註冊之**API 許可權**內的 Microsoft Graph 原則**TrustFramework**許可權。

如需註冊管理應用程式的指示，請參閱[使用 Microsoft Graph 管理 Azure AD B2C](microsoft-graph-get-started.md)。

## <a name="configure-an-azure-repo"></a>設定 Azure 存放庫

註冊管理應用程式之後，您就可以設定原則檔案的存放庫。

1. 登入您的 Azure DevOps Services 組織。
1. [建立新的專案][devops-create-project]，或選取現有的專案。
1. 在您的專案中，流覽至**存放庫**，然後**選取 [檔案**] 頁面。 選取現有的存放庫，或在此練習中建立一個。
1. 建立名為*B2CAssets*的資料夾。 為所需的預留位置檔案命名*README.md*並**認可**檔案。 您稍後可以視需要移除此檔案。
1. 將您的 Azure AD B2C 原則檔案新增至*B2CAssets*資料夾。 這包括*TrustFrameworkBase.xml*、 *TrustFrameWorkExtensions.xml*、 *SignUpOrSignin.xml*、 *ProfileEdit.xml*、 *PasswordReset.xml*，以及您所建立的其他任何原則。 記錄每個 Azure AD B2C 原則檔案的檔案名，以便在稍後的步驟中使用（用來做為 PowerShell 腳本引數）。
1. 在存放庫的根目錄中建立名為*Scripts*的資料夾，將預留位置檔案命名為*DeployToB2c.ps1*。 此時不要認可檔案，您將在稍後的步驟中執行此動作。
1. 將下列 PowerShell 腳本貼入*DeployToB2c.ps1*，然後**認可**檔案。 腳本會從 Azure AD 取得權杖，並呼叫 Microsoft Graph API，將*B2CAssets*資料夾內的原則上傳至您的 Azure AD B2C 租使用者。

    ```PowerShell
    [Cmdletbinding()]
    Param(
        [Parameter(Mandatory = $true)][string]$ClientID,
        [Parameter(Mandatory = $true)][string]$ClientSecret,
        [Parameter(Mandatory = $true)][string]$TenantId,
        [Parameter(Mandatory = $true)][string]$PolicyId,
        [Parameter(Mandatory = $true)][string]$PathToFile
    )

    try {
        $body = @{grant_type = "client_credentials"; scope = "https://graph.microsoft.com/.default"; client_id = $ClientID; client_secret = $ClientSecret }

        $response = Invoke-RestMethod -Uri https://login.microsoftonline.com/$TenantId/oauth2/v2.0/token -Method Post -Body $body
        $token = $response.access_token

        $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
        $headers.Add("Content-Type", 'application/xml')
        $headers.Add("Authorization", 'Bearer ' + $token)

        $graphuri = 'https://graph.microsoft.com/beta/trustframework/policies/' + $PolicyId + '/$value'
        $policycontent = Get-Content $PathToFile
        $response = Invoke-RestMethod -Uri $graphuri -Method Put -Body $policycontent -Headers $headers

        Write-Host "Policy" $PolicyId "uploaded successfully."
    }
    catch {
        Write-Host "StatusCode:" $_.Exception.Response.StatusCode.value__

        $_

        $streamReader = [System.IO.StreamReader]::new($_.Exception.Response.GetResponseStream())
        $streamReader.BaseStream.Position = 0
        $streamReader.DiscardBufferedData()
        $errResp = $streamReader.ReadToEnd()
        $streamReader.Close()

        $ErrResp

        exit 1
    }

    exit 0
    ```

## <a name="configure-your-azure-pipeline"></a>設定您的 Azure 管線

當您的存放庫已初始化並填入您的自訂原則檔案之後，您就可以開始設定發行管線。

### <a name="create-pipeline"></a>建立管線

1. 登入您的 Azure DevOps Services 組織，然後流覽至您的專案。
1. 在您的專案中，選取 [**管線**] [  >  **釋放**  >  **新管線**]。
1. 在 [**選取範本**] 底下，選取 [**空白作業**]。
1. 輸入 [**階段名稱**] （例如*DeployCustomPolicies*），然後關閉窗格。
1. 選取 [**新增**成品]，然後在 [**來源類型**] 下選取 [ **Azure 存放庫**]。
    1. 選擇包含您以 PowerShell 腳本填入之*腳本*資料夾的來源存放庫。
    1. 選擇**預設分支**。 如果您在上一節中建立了新的存放庫，則預設分支為*master*。
    1. 將*預設分支中*的 [**預設版本**] 設定保留為 [最新]。
    1. 輸入存放庫的**來源別名**。 例如， *policyRepo*。 不要在別名名稱中包含任何空格。
1. 選取 [新增]
1. 重新命名管線以反映其意圖。 例如，*部署自訂原則管線*。
1. 選取 [**儲存**] 以儲存管線設定。

### <a name="configure-pipeline-variables"></a>設定管線變數

1. 選取 [**變數**] 索引標籤。
1. 在 [**管線變數**] 底下新增下列變數，並依照指定的方式設定其值：

    | Name | 值 |
    | ---- | ----- |
    | `clientId` | 您先前註冊之應用程式的**應用程式（用戶端）識別碼**。 |
    | `clientSecret` | 您稍早建立的**用戶端密碼**值。 <br /> 將變數類型變更為 [**秘密**] （選取 [鎖定] 圖示）。 |
    | `tenantId` | `your-b2c-tenant.onmicrosoft.com`，其中*您的 b2c 租*使用者是 Azure AD B2C 租使用者的名稱。 |

1. 選取 [**儲存**] 以儲存變數。

### <a name="add-pipeline-tasks"></a>新增管線工作

接下來，新增用來部署原則檔案的工作。

1. 選取 [**工作**] 索引標籤。
1. 選取 [**代理程式作業**]，然後選取加號（ **+** ），將工作新增至代理程式作業。
1. 搜尋並選取 [ **PowerShell**]。 請勿選取 [Azure PowerShell]、[目的機器上的 PowerShell] 或另一個 PowerShell 專案。
1. 選取新增的**PowerShell 腳本**工作。
1. 針對 PowerShell 腳本工作輸入下列值：
    * 工作**版本**： 2. *
    * **顯示名稱**：此工作應上傳的原則名稱。 例如， *B2C_1A_TrustFrameworkBase*。
    * **類型**：檔案路徑
    * **腳本路徑**：選取省略號（***...***），流覽至 [*腳本*] 資料夾，然後選取 [ *DeployToB2C.ps1* ] 檔案。
    * **引數:**

        為**引數**輸入下列值。 將取代 `{alias-name}` 為您在上一節中指定的別名。

        ```PowerShell
        # Before
        -ClientID $(clientId) -ClientSecret $(clientSecret) -TenantId $(tenantId) -PolicyId B2C_1A_TrustFrameworkBase -PathToFile $(System.DefaultWorkingDirectory)/{alias-name}/B2CAssets/TrustFrameworkBase.xml
        ```

        例如，如果您指定的別名是*policyRepo*，則引數行應該是：

        ```PowerShell
        # After
        -ClientID $(clientId) -ClientSecret $(clientSecret) -TenantId $(tenantId) -PolicyId B2C_1A_TrustFrameworkBase -PathToFile $(System.DefaultWorkingDirectory)/policyRepo/B2CAssets/TrustFrameworkBase.xml
        ```

1. 選取 [**儲存**] 以儲存代理程式作業。

您剛剛新增的工作會將*一個*原則檔案上傳到 Azure AD B2C。 在繼續之前，請手動觸發作業（**建立發行**），以確保它在建立其他工作之前成功完成。

如果工作順利完成，請針對每個自訂原則檔案執行上述步驟，以新增部署工作。 修改 `-PolicyId` `-PathToFile` 每個原則的和引數值。

`PolicyId`是在 TrustFrameworkPolicy 節點中的 XML 原則檔案開頭找到的值。 例如， `PolicyId` 下列原則 XML 中的*B2C_1A_TrustFrameworkBase*：

```xml
<TrustFrameworkPolicy
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:xsd="http://www.w3.org/2001/XMLSchema"
xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
PolicySchemaVersion="0.3.0.0"
TenantId="contoso.onmicrosoft.com"
PolicyId= "B2C_1A_TrustFrameworkBase"
PublicPolicyUri="http://contoso.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
```

執行代理程式並上傳原則檔案時，請確定它們是依下列順序上傳：

1. *TrustFrameworkBase.xml*
1. *TrustFrameworkExtensions.xml*
1. *SignUpOrSignin.xml*
1. *ProfileEdit.xml*
1. *PasswordReset.xml*

Identity Experience Framework 會強制執行此順序，因為檔案結構是建立在階層式鏈上。

## <a name="test-your-pipeline"></a>測試您的管線

若要測試您的發行管線：

1. 依序選取 [**管線**] 和 [**發行**]。
1. 選取您稍早建立的管線，例如*DeployCustomPolicies*。
1. 選取 [**建立發行**]，然後選取 [**建立**] 以將發行排入佇列。

您應該會看到一則通知橫幅，指出發行已排入佇列。 若要查看其狀態，請選取通知橫幅中的連結，或在 [**發行**] 索引標籤上的清單中加以選取。

## <a name="next-steps"></a>後續步驟

深入了解：

* [使用用戶端認證的服務對服務呼叫](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)
* [Azure DevOps Services](https://docs.microsoft.com/azure/devops/user-guide/?view=azure-devops)

<!-- LINKS - External -->
[devops]: https://docs.microsoft.com/azure/devops/?view=azure-devops
[devops-create-project]:  https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops
[devops-pipelines]: https://docs.microsoft.com/azure/devops/pipelines
