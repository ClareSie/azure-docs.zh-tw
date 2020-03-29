---
title: Azure Active Directory 中的 Web 應用程式
description: 說明何謂 Web 應用程式，以及此應用程式類型的通訊協定流程、註冊與權杖到期等基本概念。
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: e4a7fb72d40f5db65e8e30264e9d68b2727749e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154401"
---
# <a name="web-apps"></a>Web 應用程式

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Web 應用程式是向 Web 應用程式驗證 Web 瀏覽器中使用者的應用程式。 在這種情況下，Web 應用程式指示使用者的瀏覽器將其登錄到 Azure AD。 Azure AD 通過使用者的瀏覽器返回登錄回應，該瀏覽器包含安全權杖中有關使用者的聲明。 此案例支援使用 OpenID Connect、SAML 2.0 和 WS-Federation 通訊協定來登入。

## <a name="diagram"></a>圖表

![瀏覽器到 Web 應用程式的驗證流程](./media/authentication-scenarios/web-browser-to-web-api.png)

## <a name="protocol-flow"></a>通訊協定流程

1. 當使用者造訪應用程式且需要登入時，他們透過登入要求而重新導向 Azure AD 中的驗證端點。
1. 使用者在登入頁面上登入。
1. 如果身份驗證成功，Azure AD 將創建身份驗證權杖，並返回對在 Azure 門戶中配置的應用程式 Reply URL 的登錄回應。 對於實際執行應用程式，此回覆 URL 應該為 HTTPS。 傳回的權杖包含應用程式驗證權杖所需的使用者與 Azure AD 宣告。
1. 應用程式會使用 Azure AD 的同盟中繼資料文件可用的公開簽署金鑰和簽發者資訊來驗證權杖。 應用程式會在驗證權杖之後，對使用者啟動新的工作階段。 此工作階段可讓使用者存取應用程式，直到過期為止。

## <a name="code-samples"></a>程式碼範例

請參閱「Web 瀏覽器到 Web 應用程式」案例的程式碼範例。 歡迎您經常回來看看，因為我們會隨時新增新的範例。

## <a name="app-registration"></a>應用程式註冊

要註冊 Web 應用，請參閱[註冊應用](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)。

* 單個租戶 - 如果只為組織構建應用程式，則必須使用 Azure 門戶在公司目錄中註冊該應用程式。
* 多租戶 - 如果要構建一個應用程式，該應用程式可以由組織外部的使用者使用，則必須在公司的目錄中註冊，但也必須在將使用該應用程式的每個組織的目錄中註冊。 若要讓您的應用程式出現在他們的目錄中，您可以為客戶加上註冊程序，讓他們同意應用程式。 當他們註冊您的應用程式時，他們會看到對話方塊顯示應用程式所需的權限，以及是否同意的選項。 根據所需的權限，其他組織的系統管理員可能必須同意。 當使用者或系統管理員同意時，應用程式就會註冊在他們的目錄中。

## <a name="token-expiration"></a>權杖到期

當 Azure AD 頒發的權杖的存留期過期時，使用者的會話將過期。 如有需要，您的應用程式可以縮短這段時間，例如因為一段時間沒有活動而將使用者登出。 當工作階段到期時，會提示使用者重新登入。

## <a name="next-steps"></a>後續步驟

* 深入了解其他[應用程式類型和案例](app-types.md)
* 瞭解 Azure AD[身份驗證基礎知識](v1-authentication-scenarios.md)
