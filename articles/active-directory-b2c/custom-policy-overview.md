---
title: Azure Active Directory B2C 自訂原則 | Microsoft Docs
description: 了解 Azure Active Directory B2C 自訂原則。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: f18f44208b97ab5bc8d9cd9ff01d604c62deb963
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678153"
---
# <a name="custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 中的自訂原則

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

自定義策略是定義 Azure 活動目錄 B2C (Azure AD B2C) 租戶行為的配置檔。 對於最常用的身分識別工作，Azure AD B2C 入口網站中預先定義了使用者流程。 身分識別開發人員可以完全編輯自訂原則，以完成許多不同的工作。

## <a name="comparing-user-flows-and-custom-policies"></a>比較使用者流程與自訂原則

| | 使用者流程 | 自訂原則 |
|-|-------------------|-----------------|
| 目標使用者 | 所有具備或不具備身分識別專業知識的應用程式開發人員。 | 身分識別專業人員、系統整合人員、顧問和內部身分識別小組。 他們熟悉 OpenID Connect 流,並了解標識提供者和基於聲明的身份驗證。 |
| 設定方法 | 有使用者介面 (UI) 方便使用的 Azure 入口網站。 | 直接編輯 XML 檔案，然後上傳至 Azure 入口網站。 |
| UI 自訂 | 完整的 UI 自訂,包括 HTML、CSS 和 JavaScript。<br><br>使用自訂字串支援多語言。 | 相同 |
| 屬性自訂 | 標準和自訂屬性。 | 相同 |
| 權杖和工作階段管理 | 自訂權杖和多個工作階段選項。 | 相同 |
| 識別提供者 | 預先定義的本地或社群提供者和大多數 OIDC 識別提供者,例如與 Azure 活動目錄租戶的聯合。 | 標準式 OIDC、OAUTH 和 SAML。  通過使用與 REST API 的整合,也可以進行身份驗證。 |
| 身分識別工作 | 使用本機或許多社交帳戶來註冊或登入。<br><br>自助式密碼重設。<br><br>設定檔編輯。<br><br>Multi-Factor Authentication。<br><br>自訂權杖和工作階段。<br><br>存取權杖流程。 | 使用自訂識別提供者完成與使用者流程相同的工作，或使用自訂範圍。<br><br>註冊時將使用者帳戶佈建在另一個系統中。<br><br>使用您自己的電子郵件服務提供者傳送歡迎電子郵件。<br><br>使用 Azure AD B2C 外部的使用者存放區。<br><br>使用 API 向受信任的系統驗證使用者所提供的資訊。 |

## <a name="policy-files"></a>原則檔

所使用的三種原則檔案類型如下︰

- **基底檔案** - 包含大部分的定義。 建議您儘可能不要變更這個檔案，才有利於原則的疑難排解和長期維護。
- **擴充檔案** - 保存租用戶的唯一組態變更。
- **信賴憑證者 (RP) 檔案** - 以單一工作為主的檔案，直接由應用程式或服務 (也稱為信賴憑證者) 叫用。 每個唯一的工作需要自己的 RP，而根據品牌需求而定，數目可能是「應用程式總數 x 使用案例總數」。

Azure AD B2C 中的使用者流遵循上面描述的檔模式,但開發人員只看到 RP 檔,而 Azure 門戶在後台對擴展檔進行更改。

儘管有三種類型的策略檔,但您並不僅限於三個檔。 您可能有多個每個檔類型的檔。 例如,如果您不想對副檔名檔進行更改,則可以創建擴展程式2檔以進一步擴展擴展檔。

## <a name="custom-policy-core-concepts"></a>自訂原則的核心概念

Azure 中的客戶身分識別和存取管理 (CIAM) 服務包括：

- 使用者目錄，可藉由使用 Microsoft Graph 來存取，並且會保存本機帳戶和同盟帳戶的使用者資料。
- 存取**識別體驗架構**，協調使用者與實體之間的信任，並在它們之間傳遞宣告，以完成身分識別或存取權管理工作。
- Security Token Service (STS)，發出識別碼權杖、重新整理權杖和存取權杖 (及對等的 SAML 判斷提示)，並驗證它們以保護資源。

Azure AD B2C 會依序與身分識別提供者、使用者、其他系統和本機使用者目錄互動，以達成身分識別工作。 例如讓使用者登入、註冊新的使用者或重設密碼。 識別體驗架構和原則 (也稱為使用者旅程圖或信任架構原則) 會建立多方信任，並明確定義動作項目、動作、通訊協定及完成的步驟順序。

身份體驗框架是一個完全可配置、策略驅動的基於雲的 Azure 平臺,用於協調標準協定格式(如 OpenID Connect、OAuth、SAML)和一些非標準協定格式的實體之間的信任,例如基於 REST API 的系統到系統聲明交換。 此架構會建立方便使用、白色標籤體驗，支援 HTML 和 CSS。

自訂原則以一或多個 XML 格式的檔案表示，各檔案在階層鏈中彼此參考。 XML 元素會定義宣告結構描述、宣告轉換、內容定義、宣告提供者、技術設定檔和使用者旅程圖協調流程步驟等元素。 自訂原則可以一或多個 XML 檔案的形式存取，當信賴憑證者叫用時，會由識別體驗架構執行這些檔案。 設定自訂原則的開發人員必須仔細定義信任關係，以包含中繼資料端點、精確的宣告交換定義，並設定每個識別提供者所需的祕密、金鑰和憑證。

### <a name="inheritance-model"></a>繼承模型

當應用程式呼叫 RP 原則檔時，Azure AD B2C 中的識別體驗架構將先後從基底原則檔、擴充原則檔和 RP 原則檔，新增所有元素，以組合目前生效的原則。  RP 檔案會覆寫擴充原則檔中相同類型和名稱的元素，而擴充原則檔會覆寫基底原則檔。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [開始使用自訂原則](custom-policy-get-started.md)
