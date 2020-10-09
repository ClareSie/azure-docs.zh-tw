---
title: 顯示控制項參考
titleSuffix: Azure AD B2C
description: Azure AD B2C 顯示控制項的參考。 使用顯示控制項自訂自訂原則中定義的使用者旅程。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 131ecd010cba55f08199f713654792c0844a47e1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85202291"
---
# <a name="display-controls"></a>顯示控制項

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**顯示控制項**是具有特殊功能的使用者介面元素，並與 Azure Active Directory B2C (Azure AD B2C) 後端服務互動。 它可讓使用者在頁面上執行動作，該頁面會在後端叫用 [驗證技術設定檔](validation-technical-profile.md) 。 顯示控制項會顯示在頁面上，並由自我判斷 [技術設定檔](self-asserted-technical-profile.md)參考。

下圖說明自我判斷的註冊頁面，其中包含兩個可驗證主要和次要電子郵件地址的顯示控制項。

![範例轉譯的顯示控制項](media/display-controls/display-control-email.png)

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Prerequisites

 在[自我判斷技術設定檔](self-asserted-technical-profile.md)的[中繼資料](self-asserted-technical-profile.md#metadata)區段中，參考的[ContentDefinition](contentdefinitions.md)必須 `DataUri` 設定為頁面合約版本2.0.0 或更高版本。 例如：

```xml
<ContentDefinition Id="api.selfasserted">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:2.0.0</DataUri>
  ...
```

## <a name="defining-display-controls"></a>定義顯示控制項

[ **控制項** ] 元素包含下列屬性：

| 屬性 | 必要 | 描述 |
| --------- | -------- | ----------- |
| Id | 是 | 用於顯示控制項的識別碼。 您可以 [參考](#referencing-display-controls)它。 |
| UserInterfaceControlType | 是 | 顯示控制項的類型。 目前支援 [VerificationControl](display-control-verification.md) |

[ **控制項** ] 元素包含下列元素：

| 元素 | 發生次數 | 描述 |
| ------- | ----------- | ----------- |
| InputClaims | 0:1 | **InputClaims** 用來預先填入要從使用者收集的宣告值。 |
| DisplayClaims | 0:1 | **DisplayClaims** 可用來代表要從使用者收集的宣告。 |
| OutputClaims | 0:1 | **OutputClaims** 可用來代表要暫時 **儲存此顯示的宣告**。 |
| 動作 | 0:1 | **動作** 可用來列出驗證技術設定檔，以針對前端發生的使用者動作叫用。 |

### <a name="input-claims"></a>輸入宣告

在顯示控制項中，您可以使用 **InputClaims** 專案預先填入要在頁面上從使用者收集的宣告值。 任何 **InputClaimsTransformations** 都可以在參考此顯示控制項的自我判斷提示技術設定檔中定義。

下列範例會會預先填入電子郵件地址，以使用已存在的位址進行驗證。

```xml
<DisplayControl Id="emailControl" UserInterfaceControlType="VerificationControl">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailAddress" />
  </InputClaims>
  ...
```

### <a name="display-claims"></a>顯示宣告

每種類型的顯示控制項都需要一組不同的顯示宣告、 [輸出宣告](#output-claims)，以及要執行的 [動作](#display-control-actions) 。

類似于[自我判斷技術設定檔](self-asserted-technical-profile.md#display-claims)中定義的**顯示宣告**，顯示宣告代表要從使用者在顯示控制項中收集的宣告。 參考的 **ClaimType** 元素必須針對 Azure AD B2C 所支援的使用者輸入類型指定 **>userinputtype** 元素，例如 `TextBox` 或 `DropdownSingleSelect` 。 如果 **動作**需要顯示宣告值，請將 **必要** 的屬性設定為， `true` 以強制使用者提供該特定顯示宣告的值。

特定的顯示控制項類型需要特定的顯示宣告。 例如， **VerificationControl**類型的顯示控制項需要**VerificationCode** 。 您可以使用屬性 **ControlClaimType** 來指定所需的宣告所指定的 DisplayClaim。 例如：

```xml
<DisplayClaim ClaimTypeReferenceId="otpCode" ControlClaimType="VerificationCode" Required="true" />
```

### <a name="output-claims"></a>輸出宣告

顯示控制項的 **輸出宣告** 不會傳送到下一個協調流程步驟。 它們只會暫時儲存在目前的顯示控制項會話中。 您可以在相同顯示控制項的不同動作之間共用這些暫存宣告。

若要將輸出宣告升階到下一個協調流程步驟，請使用參考此顯示控制項之實際自我判斷技術設定檔的 **OutputClaims** 。

### <a name="display-control-actions"></a>顯示控制項動作

顯示控制項的 **動作** 就是當使用者在用戶端上執行特定動作時，會在 Azure AD B2C 後端執行的程式 (瀏覽器) 。 例如，當使用者選取頁面上的按鈕時所要執行的驗證。

動作會定義 **驗證技術設定檔**的清單。 它們是用來驗證顯示控制項的部分或全部顯示宣告。 驗證技術設定檔會驗證使用者輸入，並可能會將錯誤傳回給使用者。 您可以在顯示控制項動作中使用 **ContinueOnError**、 **ContinueOnSuccess**和 **前置條件** ，類似于在自我判斷技術設定檔中的 [驗證技術配置](validation-technical-profile.md) 檔中使用它們的方式。

下列範例會根據使用者選取的 **mfaType** 宣告，在電子郵件或 SMS 中傳送程式碼。

```xml
<Action Id="SendCode">
  <ValidationClaimsExchange>
    <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AzureMfa-SendSms">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>mfaType</Value>
          <Value>email</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
        </Precondition>
      </Preconditions>
    </ValidationClaimsExchangeTechnicalProfile>
    <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AadSspr-SendEmail">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>mfaType</Value>
          <Value>phone</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
        </Precondition>
      </Preconditions>
    </ValidationClaimsExchangeTechnicalProfile>
  </ValidationClaimsExchange>
</Action>
```

## <a name="referencing-display-controls"></a>參考顯示控制項

顯示控制項是在[自我判斷技術設定檔](self-asserted-technical-profile.md)的[顯示宣告](self-asserted-technical-profile.md#display-claims)中參考。

例如：

```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
  ...
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim DisplayControlReferenceId="PhoneVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
```
