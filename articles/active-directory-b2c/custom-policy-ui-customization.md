---
title: 使用自訂政策自訂應用程式的使用者介面
titleSuffix: Azure AD B2C
description: 深入了解在 Azure Active Directory B2C 中使用自訂原則來自訂使用者介面。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e50d6d0623e87dfa68a7cc9744c3f595ff0179c6
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396373"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中使用自訂原則來自訂應用程式的使用者介面

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

通過完成本文中的步驟,您可以創建一個註冊和登錄自定義策略與您的品牌和外觀。 使用 Azure Active Directory B2C (Azure AD B2C)，幾乎可完全掌控對使用者呈現的 HTML 和 CSS 內容。 使用自訂原則時，您會以 XML 設定 UI 自訂，而不是在 Azure 入口網站中使用控制項。

## <a name="prerequisites"></a>Prerequisites

完成[開始使用自訂原則](custom-policy-get-started.md)中的步驟。 您應該有一個使用本機帳戶來註冊和登入的有效自訂原則。

[!INCLUDE [active-directory-b2c-html-how-to](../../includes/active-directory-b2c-html-how-to.md)]

### <a name="4-modify-the-extensions-file"></a>4. 修改副檔名檔案

要設定 UI 自訂,請將**ContentDefinition**及其子元素從基本檔案複製到擴展檔。

1. 開啟原則的基底檔案。 例如, <em> `SocialAndLocalAccounts/` </em>. 此基本檔案是自訂策略初學者套件中包含的策略檔之一,您應該在先決條件中取得,[從自訂策略開始](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom)。
1. 搜尋 **ContentDefinitions** 元素的完整內容並且複製。
1. 開啟擴充檔案。 例如 TrustFrameworkExtensions.xml**。 搜尋 **BuildingBlocks** 元素。 如果此元素不存在，請加以新增。
1. 貼上您複製的 **ContentDefinitions** 元素完整內容，作為 **BuildingBlocks** 元素的子項目。
1. 在您複製的 XML 中，搜尋包含 `Id="api.signuporsignin"` 的 **ContentDefinition** 元素。
1. 將 **LoadUri** 的值變更為您上傳至儲存體的 HTML 檔案 URL。 例如： `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html` 。

    自訂策略應類似於以下代碼段:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

1. 儲存擴充檔案。

### <a name="5-upload-and-test-your-updated-custom-policy"></a>5. 上傳並測試更新的自訂策略

#### <a name="51-upload-the-custom-policy"></a>5.1 上傳自訂原則

1. 選取頂端功能表中的 [目錄 + 訂用帳戶]**** 篩選，然後選擇包含您租用戶的目錄，以確定您使用的是包含 Azure AD B2C 租用戶的目錄。
1. 搜尋並選擇**Azure AD B2C**。
1. 在 **「政策」** 下,選擇**身份體驗框架**。
1. 選擇 **「上載自定義策略**」。
1. 上傳您先前變更的擴充檔案。

#### <a name="52-test-the-custom-policy-by-using-run-now"></a>5.2 使用 **「立即執行」** 測試自訂政策

1. 選擇您上載的策略,然後選擇 **「立即執行**」。
1. 您應該可以使用電子郵件地址註冊。

[!INCLUDE [active-directory-b2c-html-templates](../../includes/active-directory-b2c-html-templates.md)]

## <a name="configure-dynamic-custom-page-content-uri"></a>設定動態自訂頁面內容 URI

通過使用 Azure AD B2C 自訂策略,可以在 URL 路徑或查詢字串中發送參數。 將參數傳遞至您的 HTML 端點，即可動態變更網頁內容。 例如，視您從 Web 或行動裝置應用程式傳遞的參數而定，您可以變更 Azure AD B2C 註冊或登入背景影像。 參數可以是任何[宣告解析器](claim-resolver-overview.md),如應用程式 ID、語言 ID 或自訂查詢字串`campaignId`參數,如 。

### <a name="sending-query-string-parameters"></a>傳送查詢字串參數

要發送查詢字串參數,在[依賴方策略](relyingparty.md)中,添加如下`ContentDefinitionParameters`所示的元素。

```XML
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <UserJourneyBehaviors>
    <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        <Parameter Name="lang">{Culture:LanguageName}</Parameter>
        <Parameter Name="appId">{OIDC:ClientId}</Parameter>
    </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    ...
</RelyingParty>
```

在內容定義中,將的值`LoadUri`變更為`https://<app_name>.azurewebsites.net/home/unified`。 自訂策略`ContentDefinition`應類似於以下代碼段:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://<app_name>.azurewebsites.net/home/unified</LoadUri>
  ...
</ContentDefinition>
```

當 Azure AD B2C 載入網頁時,它會呼叫 Web 伺服器終結點:

```http
https://<app_name>.azurewebsites.net/home/unified?campaignId=123&lang=fr&appId=f893d6d3-3b6d-480d-a330-1707bf80ebea
```

### <a name="dynamic-page-content-uri"></a>動態頁面內容 URI

內容可以根據所使用的參數從不同位置提取。 在啟用 CORS 的終結點中,設置一個資料夾結構以承載內容。 例如,您可以在以下結構中組織內容。 *根資料夾/資料夾每種語言/您的html檔案*。 例如,自定義頁面 URI 可能如下所示:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://contoso.blob.core.windows.net/{Culture:LanguageName}/myHTML/unified.html</LoadUri>
  ...
</ContentDefinition>
```

Azure AD B2C 為法`fr`語傳送兩個字母的 ISO 代碼:

```http
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="next-steps"></a>後續步驟

有關可自訂的 UI 元素的詳細資訊,請參閱[使用者流的 UI 自訂參考指南](customize-ui-overview.md)。
