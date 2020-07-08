---
title: 啟用單次密碼（OTP）驗證
titleSuffix: Azure AD B2C
description: 瞭解如何使用 Azure AD B2C 自訂原則來設定一次性密碼（OTP）案例。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9592afbf74e65bcb2fe9319da764bf06d8d4eb6c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85385717"
---
# <a name="define-a-one-time-password-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>在 Azure AD B2C 自訂原則中定義一次性密碼技術設定檔

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C （Azure AD B2C）提供管理單次密碼的產生和驗證的支援。 使用技術設定檔來產生程式碼，然後稍後再驗證該程式碼。

一次性密碼技術設定檔也可以在程式碼驗證期間傳回錯誤訊息。 使用**驗證技術設定檔**，設計與一次性密碼的整合。 驗證技術設定檔會呼叫一次性密碼技術設定檔來驗證程式代碼。 在使用者旅程圖繼續進行之前，驗證技術設定檔會驗證使用者提供的資料。 使用驗證技術設定檔時，會在自我判斷頁面上顯示錯誤訊息。

## <a name="protocol"></a>通訊協定

**Protocol** 元素的 **Name** 屬性必須設定為 `Proprietary`。 **Handler**屬性必須包含 Azure AD B2C 所使用之通訊協定處理常式元件的完整名稱：

```xml
Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

下列範例顯示一次性密碼技術設定檔：

```xml
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Validate user input verification code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="generate-code"></a>產生程式碼

此技術設定檔的第一個模式是產生程式碼。 以下是可在此模式中設定的選項。

### <a name="input-claims"></a>輸入宣告

**InputClaims**元素包含要傳送給一次性密碼通訊協定提供者所需的宣告清單。 您也可以將宣告的名稱對應到下面定義的名稱。

| ClaimReferenceId | 必要 | 說明 |
| --------- | -------- | ----------- |
| 識別碼 (identifier) | Yes | 識別稍後需要驗證程式代碼之使用者的識別碼。 它通常用來做為程式碼傳遞目標目的地的識別碼，例如電子郵件地址或電話號碼。 |

**InputClaimsTransformations**元素可能包含**InputClaimsTransformation**元素的集合，這些專案是用來修改輸入宣告，或在傳送至一次性密碼通訊協定提供者之前產生新的宣告。

### <a name="output-claims"></a>輸出宣告

**OutputClaims**元素包含一次性密碼通訊協定提供者所產生的宣告清單。 您也可以將宣告的名稱對應到下面定義的名稱。

| ClaimReferenceId | 必要 | 說明 |
| --------- | -------- | ----------- |
| otpGenerated | Yes | 產生的程式碼，其會話由 Azure AD B2C 管理。 |

**OutputClaimsTransformations** 元素可能含有 **OutputClaimsTransformation** 的集合，用於修改輸出宣告或產生新的輸出宣告。

### <a name="metadata"></a>中繼資料

下列設定可用於設定程式碼產生模式：

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| CodeExpirationInSeconds | No | 程式碼到期前的時間，以秒為單位。 最小值： `60` ;最大值： `1200` ;預設值： `600` 。 |
| CodeLength | No | 程式碼的長度。 預設值為 `6`。 |
| CharacterSet | No | 針對在正則運算式中使用的程式碼所設定的字元集。 例如，`a-z0-9A-Z`。 預設值為 `0-9`。 字元集必須在指定的集合中包含至少10個不同的字元。 |
| NumRetryAttempts | No | 將程式碼視為無效之前的驗證嘗試次數。 預設值為 `5`。 |
| 操作 | Yes | 要執行的作業。 可能的值： `GenerateCode` 。 |
| ReuseSameCode | No | 是否應指定重複的程式碼，而不是在指定的程式碼尚未過期且仍然有效時產生新的程式碼。 預設值為 `false`。 |

### <a name="example"></a>範例

下列範例 `TechnicalProfile` 是用來產生程式碼：

```xml
<TechnicalProfile Id="GenerateCode">
  <DisplayName>Generate Code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="Operation">GenerateCode</Item>
    <Item Key="CodeExpirationInSeconds">600</Item>
    <Item Key="CodeLength">6</Item>
    <Item Key="CharacterSet">0-9</Item>
    <Item Key="NumRetryAttempts">5</Item>
    <Item Key="ReuseSameCode">false</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpGenerated" />
  </OutputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>驗證碼

此技術設定檔的第二個模式是驗證程式代碼。 以下是可在此模式中設定的選項。

### <a name="input-claims"></a>輸入宣告

**InputClaims**元素包含要傳送給一次性密碼通訊協定提供者所需的宣告清單。 您也可以將宣告的名稱對應到下面定義的名稱。

| ClaimReferenceId | 必要 | 說明 |
| --------- | -------- | ----------- |
| 識別碼 (identifier) | Yes | 識別先前已產生程式碼之使用者的識別碼。 它通常用來做為程式碼傳遞目標目的地的識別碼，例如電子郵件地址或電話號碼。 |
| otpToVerify | Yes | 使用者所提供的驗證碼。 |

**InputClaimsTransformations**元素可能包含**InputClaimsTransformation**元素的集合，這些專案是用來修改輸入宣告，或在傳送至一次性密碼通訊協定提供者之前產生新的宣告。

### <a name="output-claims"></a>輸出宣告

在此通訊協定提供者的程式碼驗證期間，沒有提供輸出宣告。

**OutputClaimsTransformations** 元素可能含有 **OutputClaimsTransformation** 的集合，用於修改輸出宣告或產生新的輸出宣告。

### <a name="metadata"></a>中繼資料

下列設定可用於驗證模式的程式碼：

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| 操作 | Yes | 要執行的作業。 可能的值： `VerifyCode` 。 |


### <a name="ui-elements"></a>UI 元素

下列中繼資料可用來設定程式碼驗證失敗時所顯示的錯誤訊息。 應該在[自我](self-asserted-technical-profile.md)判斷技術設定檔中設定中繼資料。 錯誤訊息可以[當地語系化](localization-string-ids.md#one-time-password-error-messages)。

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| UserMessageIfSessionDoesNotExist | No | 如果程式碼驗證會話已過期，要向使用者顯示的訊息。 這可能是程式碼已過期，或從未針對指定的識別碼產生程式碼。 |
| UserMessageIfMaxRetryAttempted | No | 當使用者已超過允許的驗證嘗試次數上限時，要對其顯示的訊息。 |
| UserMessageIfInvalidCode | No | 如果提供了不正確程式碼，要向使用者顯示的訊息。 |
| UserMessageIfVerificationFailedRetryAllowed | No | 如果使用者提供了不正確程式碼，就會向使用者顯示訊息，且允許使用者提供正確的程式碼。  |
|UserMessageIfSessionConflict|No| 如果無法驗證程式代碼，要向使用者顯示的訊息。|

### <a name="example"></a>範例

下列範例 `TechnicalProfile` 是用來驗證程式代碼：

```xml
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Verify Code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="Operation">VerifyCode</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
    <InputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpToVerify" />
  </InputClaims>
</TechnicalProfile>
```

## <a name="next-steps"></a>後續步驟

如需搭配自訂電子郵件驗證使用單次密碼技術設定檔的範例，請參閱下列文章：

- Azure Active Directory B2C 中的自訂電子郵件驗證（[Mailjet](custom-email-mailjet.md)、 [SendGrid](custom-email-sendgrid.md)）

