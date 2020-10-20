---
title: Azure Active Directory B2C 程式碼範例 | Microsoft Docs
description: Azure Active Directory B2C 行動裝置、傳統型、Web 和單頁應用程式的程式碼範例。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/02/2020
ms.custom: mvc
ms.topic: sample
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 63a79df8e540e88d0f1e3f4c5b8d339ff03c877f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91854285"
---
# <a name="azure-active-directory-b2c-code-samples"></a>Azure Active Directory B2C 程式碼範例

下表提供 iOS、Android、.NET 和 Node.js 等應用程式的範例連結。

## <a name="mobile-and-desktop-apps"></a>行動和傳統型應用程式

| 範例 | 描述 |
|--------| ----------- |
| [ios-swift-native-msal](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal) | 以 Swift 撰寫的 iOS 範例，其使用 OAuth 2.0 驗證 Azure AD B2C 使用者和呼叫 API |
| [android-native-msal](https://github.com/Azure-Samples/ms-identity-android-java#b2cmodefragment-class) | 簡單的 Android 應用程式，其展示如何透過 Azure Active Directory B2C 使用 MSAL 來驗證使用者，以及使用所產生的權杖 MSAL 來存取 Web API。 |
| [ios-native-appauth](https://github.com/Azure-Samples/active-directory-b2c-ios-native-appauth) | 此範例示範如何使用第三方程式庫以 Objective-C 建置 iOS 應用程式，而該應用程式會向我們的 Azure AD B2C 識別服務驗證 Microsoft 身分識別使用者。 |
| [android-native-appauth](https://github.com/Azure-Samples/active-directory-b2c-android-native-appauth) | 此範例示範如何使用第三方程式庫建置 Android 應用程式，而該應用程式會向我們的 B2C 識別服務驗證 Microsoft 身分識別使用者並使用 OAuth 2.0 存取權杖呼叫 Web API。 |
| [dotnet-desktop](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) | 此範例示範 Windows Desktop .NET (WPF) 應用程式如何使用 Azure AD B2C 登入使用者、使用 MSAL.NET 取得存取權杖，以及呼叫 API。 |
| [xamarin-native](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | 簡單的 Xamarin Forms 應用程式，其展示如何透過 Azure Active Directory B2C 使用 MSAL 來驗證使用者，以及使用所產生的權杖 MSAL 來存取 Web API。 |

## <a name="web-apps-and-apis"></a>Web Apps 和 API

| 範例 | 描述 |
|--------| ----------- |
| [dotnet-webapp-and-webapi](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) | .NET Web 應用程式的合併範例，該應用程式會呼叫 .NET Web API (兩者均使用 Azure AD B2C 保護)。 |
| [dotnetcore-webapp-openidconnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-5-B2C) | 使用 OpenID Connect 在 Azure AD B2C 中登入使用者的 ASP.NET Core Web 應用程式。 |
| [dotnetcore-webapp-msal-api](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/4-WebApp-your-API/4-2-B2C) | ASP.NET Core Web 應用程式，該應用程式可以使用 Azure AD B2C 登入使用者、使用 MSAL.NET 取得存取權杖，以及呼叫 API。 |
| [openidconnect-nodejs](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS) | Node.js 應用程式，該應用程式可供輕鬆快速地使用 OpenID Connect 設定 Web 應用程式與 Express。 |
| [javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | 適用於 Azure AD B2C 的小型 Node.js Web API，示範如何使用 passport.js 保護您的 Web API 並接受 B2C 存取權杖。 |
| [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/master/README_B2C.md) | 示範如何整合 Microsoft 身分識別平台的 B2C 與 Python Web 應用程式。  |

## <a name="single-page-apps"></a>單一頁面應用程式

| 範例 | 描述 |
|--------| ----------- |
| [javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) | 呼叫 Web API 的單頁應用程式 (SPA)。 運用 MSAL.js 完成 Azure AD B2C 驗證。 |

## <a name="saml-test-application"></a>SAML 測試應用程式

| 範例 | 描述 |
|--------| ----------- |
| [saml-sp-tester](https://github.com/azure-ad-b2c/saml-sp-tester/tree/master/source-code) | SAML 測試應用程式，用來測試設定為 SAML 識別提供者的 Azure AD B2C。 |

## <a name="api-connectors"></a>API 連接器

下列表格提供程式碼範例的連結，這些程式碼範例可讓您使用 [API 連接器](api-connectors-overview.md)，在您的使用者流程中運用 Web API。

## <a name="api-connector-azure-function-quickstarts"></a>API 連接器 Azure Function 快速入門

| 範例                                                                                                                          | 描述                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-api-connector-azure-function-validate) | 此 .NET Core Azure Function 範例示範如何將註冊限制為特定的電子郵件網域，並驗證使用者提供的資訊。 |
| [Node.js](https://github.com/Azure-Samples/active-directory-nodejs-external-identities-api-connector-azure-function-validate)   | 此 Node.js Azure Function 範例示範如何將註冊限制為特定的電子郵件網域，並驗證使用者提供的資訊。  |
| [Python](https://github.com/Azure-Samples/active-directory-python-external-identities-api-connector-azure-function-validate)    | 此 Python Azure Function 範例示範如何將註冊限制為特定的電子郵件網域，並驗證使用者提供的資訊。    |