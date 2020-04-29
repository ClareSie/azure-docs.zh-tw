---
title: Microsoft 身分識別平臺的應用程式類型 |Azure
description: Microsoft 身分識別平臺（v2.0）端點所支援的應用程式和案例類型。
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: def92071496716f90b24158a50e4a5233e93c994
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81677994"
---
# <a name="application-types-for-microsoft-identity-platform"></a>Microsoft 身分識別平臺的應用程式類型

Microsoft 身分識別平台 (v2.0) 端點支援各種新式應用程式架構的驗證，它們全都以產業標準通訊協定 [OAuth 2.0 或 OpenID Connect](active-directory-v2-protocols.md) 為基礎。 本文說明您可以使用 Microsoft 身分識別平臺來建立的應用程式類型，不論您偏好的語言或平臺為何。 這些資訊是設計來協助您在開始使用程式[代碼](v2-overview.md#getting-started)之前，先瞭解高階案例。

## <a name="the-basics"></a>基本概念

您必須在新的[應用程式註冊入口網站](https://go.microsoft.com/fwlink/?linkid=2083908)中註冊每個使用 Microsoft 身分識別平臺端點的應用程式。 應用程式註冊程序會為您的應用程式收集和指派下列值：

* 可唯一識別您應用程式的**應用程式（用戶端）識別碼**
* 可用來將回應導回到應用程式的「重新導向 URI」****
* 一些其他案例特定的值，例如支援的帳戶類型

如需詳細資訊，請了解如何[註冊應用程式](quickstart-register-app.md)。

註冊應用程式之後，應用程式會將要求傳送至端點，以與 Microsoft 身分識別平臺通訊。 我們提供開放原始碼架構，以及可處理這些要求詳細資料的程式庫。 您也可以選擇建立對這些端點的要求，來自行實作驗證邏輯：

```HTTP
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

## <a name="single-page-apps-javascript"></a>單頁應用程式 (JavaScript)

許多新式應用程式都有一個單頁應用程式前端，主要是以 JavaScript 撰寫。 通常是使用像是「角度」、「反應」或 Vue 的架構來撰寫。 Microsoft 身分識別平臺端點會使用[OAuth 2.0 隱含流程](v2-oauth2-implicit-grant-flow.md)來支援這些應用程式。

在此流程中，應用程式會直接從 Microsoft 身分識別平臺授權端點接收權杖，而不需要任何伺服器對伺服器交換。 所有驗證邏輯和工作階段處理都完全在 JavaScript 用戶端中進行，而不需要執行額外的頁面重新導向。

![顯示隱含驗證流程](./media/v2-app-types/convergence-scenarios-implicit.svg)

若要查看此案例的實際運作情況，請在[Microsoft 身分識別平臺開始](v2-overview.md#getting-started)使用一節中，試用其中一個單頁應用程式程式碼範例。

## <a name="web-apps"></a>Web 應用程式

針對使用者透過瀏覽器存取的 Web 應用程式 (.NET、PHP、Java、Ruby、Python、Node)，您可以使用 [OpenID Connect](active-directory-v2-protocols.md) 來執行使用者登入。 在 OpenID Connect 中，Web 應用程式會收到識別碼權杖。 識別碼權杖是一個安全性權杖，可驗證使用者的身分識別並以宣告形式提供使用者的相關資訊：

```JSON
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

如需 Microsoft 身分識別平臺端點中所用不同權杖類型的詳細資訊，請參閱[存取權杖](access-tokens.md)參考和[id_token 參考](id-tokens.md)

在 Web 伺服器應用程式中，登入驗證流程會採用下列概要步驟：

![顯示 web 應用程式驗證流程](./media/v2-app-types/convergence-scenarios-webapp.svg)

您可以使用從 Microsoft 身分識別平臺端點接收的公開簽署金鑰來驗證識別碼權杖，以確保使用者的身分識別。 系統會設定工作階段 Cookie，這可在後續的頁面要求上用來識別使用者。

若要查看此案例的實際運作情況，請嘗試[Microsoft 身分識別平臺](v2-overview.md#getting-started)開始使用一節中的其中一個 web 應用程式登入程式碼範例。

除了簡易登入之外，Web 伺服器應用程式可能也需要存取其他 Web 服務，例如 REST API。 在此情況下，Web 伺服器應用程式可以使用 [OAuth 2.0 授權碼流程](active-directory-v2-protocols.md)，參與結合了 OpenID Connect 與 OAuth 2.0 的流程。 如需此案例的詳細資訊，請參閱[開始使用 web apps 和 Web api](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)。

## <a name="web-apis"></a>Web API

您可以使用 Microsoft 身分識別平臺端點來保護 web 服務，例如應用程式的 RESTful Web API。 Web Api 可以在多種平臺和語言中執行。 也可以使用 Azure Functions 中的 HTTP 觸發程式來執行。 Web API 使用 OAuth 2.0 存取權杖來保護其資料及驗證連入要求，而不是識別碼權杖和會話 cookie。 Web API 的呼叫端會在 HTTP 要求的授權標頭中附加存取權杖，如下所示：

```HTTP
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API 使用存取權杖來驗證 API 呼叫端的身分識別，並從在存取權杖中編碼的宣告中，將呼叫端的相關資訊解壓縮。 如需 Microsoft 身分識別平臺端點中所用不同權杖類型的詳細資訊，請參閱[存取權杖](access-tokens.md)參考和[id_token](id-tokens.md)參考。

Web API 可以藉由公開許可權（也稱為[範圍](v2-permissions-and-consent.md)），讓使用者能夠加入宣告或退出特定的功能或資料。 為了讓發出呼叫的應用程式取得範圍的權限，使用者必須在流程中對範圍表示同意。 Microsoft 身分識別平臺端點會要求使用者提供許可權，然後在 Web API 收到的所有存取權杖中記錄許可權。 Web API 會驗證它在每次呼叫時所收到的存取權杖，並執行授權檢查。

Web API 可以從所有類型的應用程式接收存取權杖，包括 web 伺服器應用程式、桌面和行動應用程式、單一頁面應用程式、伺服器端守護程式，甚至是其他 web Api。 Web API 的高階流程如下所示：

![顯示 Web API 驗證流程](./media/v2-app-types/convergence-scenarios-webapi.svg)

若要瞭解如何使用 OAuth2 存取權杖來保護 Web API，請參閱[Microsoft 身分識別平臺開始](v2-overview.md#getting-started)使用一節中的 Web API 程式碼範例。

在許多情況下，web Api 也需要向 Microsoft 身分識別平臺所保護的其他下游 web Api 發出輸出要求。 若要這麼做，web Api 可以利用代理者**流程，** 這可讓 Web API 交換連入存取權杖，以用於輸出要求中的另一個存取權杖。 如需詳細資訊，請參閱[Microsoft 身分識別平臺和 OAuth 2.0](v2-oauth2-on-behalf-of-flow.md)代理者流程。

## <a name="mobile-and-native-apps"></a>行動和原生應用程式

裝置安裝的應用程式（例如行動和桌面應用程式）通常需要存取後端服務或 web Api，以儲存資料並代表使用者執行功能。 這些應用程式可以使用 [OAuth 2.0 授權碼流程](v2-oauth2-auth-code-flow.md)，將登入和授權新增至後端服務。

在此流程中，當使用者登入時，應用程式會從 Microsoft 身分識別平臺端點接收授權碼。 授權碼代表應用程式具備權限，可代表登入的使用者呼叫後端服務。 應用程式可以在背景中以授權碼交換 OAuth 2.0 存取權杖和重新整理權杖。 應用程式可以使用存取權杖，在 HTTP 要求中向 web Api 進行驗證，並使用重新整理權杖，在較舊的存取權杖過期時取得新的存取權杖。

![顯示原生應用程式驗證流程](./media/v2-app-types/convergence-scenarios-native.svg)

## <a name="daemons-and-server-side-apps"></a>精靈和伺服器端應用程式

具有長時間執行程式或在沒有與使用者互動的情況下運作的應用程式，也需要方法來存取受保護的資源，例如 web Api。 這些應用程式可以使用應用程式的身分識別 (而非使用者委派的身分識別) 搭配 OAuth 2.0 用戶端認證流程，來驗證及取得權杖。 您可以使用用戶端密碼或憑證來提供應用程式的身分識別。 如需詳細資訊，請參閱[使用 Microsoft 身分識別平臺的 .Net Core daemon 主控台應用程式](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2)。

在此流程中，應用程式會直接與`/token`端點互動以取得存取權：

![顯示 daemon 應用程式驗證流程](./media/v2-app-types/convergence-scenarios-daemon.svg)

若要建置精靈應用程式，請參閱[用戶端認證文件](v2-oauth2-client-creds-grant-flow.md)，或試試 [.NET 範例應用程式](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)。
