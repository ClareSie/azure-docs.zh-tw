---
title: Azure API 管理範本資料模型參考 | Microsoft Docs
description: 了解在 Azure API 管理的開發人員入口網站範本資料模型中，所使用之一般項目的實體和類型表示法。
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 5625ff7e4fc51b9b6b894698719247902a480f44
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84690026"
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API 管理範本資料模型參考
本主題說明在 Azure API 管理的開發人員入口網站範本資料模型中，所使用之一般項目的實體和類型表示法。  
  
 如需有關使用範本的詳細資訊，請參閱[如何使用範本自訂 API 管理開發人員入口網站](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/)。  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="reference"></a>參考

-   [API](#API)  
-   [API 摘要](#APISummary)  
-   [應用程式](#Application)  
-   [Attachment](#Attachment)  
-   [程式碼範例](#Sample)  
-   [註解](#Comment)  
-   [篩選](#Filtering)  
-   [標頭](#Header)  
-   [HTTP 要求](#HTTPRequest)  
-   [HTTP 回應](#HTTPResponse)  
-   [問題](#Issue)  
-   [運算](#Operation)  
-   [作業功能表](#Menu)  
-   [作業功能表項目](#MenuItem)  
-   [分頁](#Paging)  
-   [參數](#Parameter)  
-   [產品](#Product)  
-   [提供者](#Provider)  
-   [表示法](#Representation)  
-   [訂用帳戶](#Subscription)  
-   [訂用帳戶摘要](#SubscriptionSummary)  
-   [使用者帳戶資訊](#UserAccountInfo)  
-   [使用者登入](#UseSignIn)  
-   [使用者註冊](#UserSignUp)  
  
##  <a name="api"></a><a name="API"></a>API  
 `API` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`id`|字串|資源識別碼。 可唯一識別目前 API 管理服務執行個體內的 API。 其值為 `apis/{id}` 格式的有效相對 URL，其中 `{id}` 是 API 識別碼。 這個屬性是唯讀的。|  
|`name`|字串|API 的名稱。 不能空白。 長度上限是 100 個字元。|  
|`description`|字串|API 的描述。 不能空白。 可包含 HTML 格式標籤。 長度上限是 1000 個字元。|  
|`serviceUrl`|字串|實作此 API 之後端服務的絕對 URL。|  
|`path`|字串|可在 API 管理服務執行個體內唯一識別此 API 及其所有資源路徑的相對 URL。 此路徑會附加至在服務執行個體建立期間所指定的 API 端點基底 URL，以形成此 API 的公用 URL。|  
|`protocols`|數字陣列|描述此 API 中的作業可在哪些通訊協定上叫用。 允許的值為 `1 - http` 和 `2 - https`或兩者兼具。|  
|`authenticationSettings`|[授權伺服器驗證設定](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|此 API 中所包含之驗證設定的集合。|  
|`subscriptionKeyParameterNames`|物件 (object)|選擇性屬性，可用來指定包含訂用帳戶金鑰之查詢和/或標頭參數的自訂名稱。 此屬性若存在，其中就至少必須包含下列兩個屬性的其中之一。<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="api-summary"></a><a name="APISummary"></a>API 摘要  
 `API summary` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`id`|字串|資源識別碼。 可唯一識別目前 API 管理服務執行個體內的 API。 其值為 `apis/{id}` 格式的有效相對 URL，其中 `{id}` 是 API 識別碼。 這個屬性是唯讀的。|  
|`name`|字串|API 的名稱。 不能空白。 長度上限是 100 個字元。|  
|`description`|字串|API 的描述。 不能空白。 可包含 HTML 格式標籤。 長度上限是 1000 個字元。|  
  
##  <a name="application"></a><a name="Application"></a>應用程式  
 `application` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Id`|字串|應用程式的唯一識別碼。|  
|`Title`|字串|應用程式的標題。|  
|`Description`|字串|應用程式的描述。|  
|`Url`|URI|應用程式的 URI。|  
|`Version`|字串|應用程式的版本資訊。|  
|`Requirements`|字串|應用程式需求的描述。|  
|`State`|number|應用程式的目前狀態。<br /><br /> - 0 - 已註冊<br /><br /> - 1 - 已提交<br /><br /> - 2 - 已發佈<br /><br /> - 3 - 已拒絕<br /><br /> - 4 - 已解除發佈|  
|`RegistrationDate`|Datetime|應用程式的註冊日期和時間。|  
|`CategoryId`|number|應用程式的類別 (金融、娛樂等)|  
|`DeveloperId`|字串|提交應用程式之開發人員的唯一識別碼。|  
|`Attachments`|[附件](#Attachment)實體的集合。|應用程式的任何附件，例如螢幕擷取畫面或圖示。|  
|`Icon`|[Attachment](#Attachment)|應用程式的圖示。|  
  
##  <a name="attachment"></a><a name="Attachment"></a>附件  
 `attachment` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`UniqueId`|字串|附件的唯一識別碼。|  
|`Url`|字串|資源的 URL。|  
|`Type`|字串|附件的類型。|  
|`ContentType`|字串|附件的媒體類型。|  
  
##  <a name="code-sample"></a><a name="Sample"></a>程式碼範例  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`title`|字串|作業的名稱。|  
|`snippet`|字串|此屬性已過時而不應使用。|  
|`brush`|字串|在顯示程式碼範例時所要使用的程式碼語法著色範本。 允許的值為 `plain`、`php``java`、`xml``objc`、`python`、`ruby` 和 `csharp`。|  
|`template`|字串|此程式碼範例範本的名稱。|  
|`body`|字串|程式碼片段之程式碼範例部分的預留位置。|  
|`method`|字串|作業的 HTTP 方法。|  
|`scheme`|字串|作業要求所要使用的通訊協定。|  
|`path`|字串|作業的路徑。|  
|`query`|字串|具有已定義參數的查詢字串範例。|  
|`host`|字串|包含此作業之 API 的 API 管理服務閘道 URL。|  
|`headers`|[標頭](#Header)實體的集合。|此作業的標頭。|  
|`parameters`|[參數](#Parameter)實體的集合。|針對這項作業所定義的參數。|  
  
##  <a name="comment"></a><a name="Comment"></a>加以  
 `API` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Id`|number|批註的識別碼。|  
|`CommentText`|字串|註解的本文。 可包含 HTML。|  
|`DeveloperCompany`|字串|開發人員的公司名稱。|  
|`PostedOn`|Datetime|註解的公佈日期和時間。|  
  
##  <a name="issue"></a><a name="Issue"></a>問題  
 `issue` 實體具有下列屬性。  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Id`|字串|問題的唯一識別碼。|  
|`ApiID`|字串|回報此問題之 API 的識別碼。|  
|`Title`|字串|問題的標題。|  
|`Description`|字串|問題的描述。|  
|`SubscriptionDeveloperName`|字串|報告問題之開發人員的名字。|  
|`IssueState`|字串|問題的目前狀態。 可能的值為已提議、已開啟、已關閉。|  
|`ReportedOn`|Datetime|問題的報告日期和時間。|  
|`Comments`|[註解](#Comment)實體的集合。|此問題的註解。|  
|`Attachments`|[附件](api-management-template-data-model-reference.md#Attachment)實體的集合。|問題的任何附件。|  
|`Services`|[API](#API) 實體的集合。|提出問題之使用者所訂閱的 API。|  
  
##  <a name="filtering"></a><a name="Filtering"></a>濾除  
 `filtering` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Pattern`|字串|目前的搜尋詞彙；或者，如果沒有搜尋詞彙，則為 `null`。|  
|`Placeholder`|字串|未指定搜尋詞彙時要在搜尋方塊中顯示的文字。|  
  
##  <a name="header"></a><a name="Header"></a>標頭  
 本區段描述 `parameter` 表示法。  
  
|屬性|類型|Description|  
|--------------|-----------------|----------|  
|`name`|字串|參數名稱。|  
|`description`|字串|參數描述。|  
|`value`|字串|標頭值。|  
|`typeName`|字串|標頭值的資料類型。|  
|`options`|字串|選項。|  
|`required`|boolean|標頭是否為必要。|  
|`readOnly`|boolean|標頭是否為唯讀。|  
  
##  <a name="http-request"></a><a name="HTTPRequest"></a>HTTP 要求  
 本區段描述 `request` 表示法。  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`description`|字串|作業要求描述。|  
|`headers`|[標頭](#Header)實體的陣列。|要求標頭。|  
|`parameters`|[參數](#Parameter)的陣列|作業要求參數的集合。|  
|`representations`|[表示法](#Representation)的陣列|作業要求表示法的集合。|  
  
##  <a name="http-response"></a><a name="HTTPResponse"></a>HTTP 回應  
 本區段描述 `response` 表示法。  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`statusCode`|正整數|作業回應狀態碼。|  
|`description`|字串|作業回應描述。|  
|`representations`|[表示法](#Representation)的陣列|作業回應表示法的集合。|  
  
##  <a name="operation"></a><a name="Operation"></a>運營  
 `operation` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`id`|字串|資源識別碼。 可唯一識別目前 API 管理服務執行個體內的作業。 其值為 `apis/{aid}/operations/{id}` 格式的有效相對 URL，其中 `{aid}` 是 API 識別碼，`{id}` 是作業識別碼。 這個屬性是唯讀的。|  
|`name`|字串|作業名稱。 不能空白。 長度上限是 100 個字元。|  
|`description`|字串|作業的描述。 不能空白。 可包含 HTML 格式標籤。 長度上限是 1000 個字元。|  
|`scheme`|字串|描述此 API 中的作業可在哪些通訊協定上叫用。 允許的值為 `http`、`https` 或 `http` 和 `https` 兼具。|  
|`uriTemplate`|字串|可識別此作業之目標資源的相對 URL 範本。 可包含參數。 範例：`customers/{cid}/orders/{oid}/?date={date}`|  
|`host`|字串|裝載 API 的 API 管理閘道 URL。|  
|`httpMethod`|字串|作業 HTTP 方法。|  
|`request`|[HTTP 要求](#HTTPRequest)|包含要求詳細資料的實體。|  
|`responses`|[HTTP 回應](#HTTPResponse)的陣列|作業 [HTTP 回應](#HTTPResponse)實體的陣列。|  
  
##  <a name="operation-menu"></a><a name="Menu"></a>作業功能表  
 `operation menu` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`ApiId`|字串|目前 API 的識別碼。|  
|`CurrentOperationId`|字串|目前作業的識別碼。|  
|`Action`|字串|功能表類型。|  
|`MenuItems`|[作業功能表項目](#MenuItem)實體的集合。|目前 API 的作業。|  
  
##  <a name="operation-menu-item"></a><a name="MenuItem"></a>作業功能表項目  
 `operation menu item` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Id`|字串|作業的識別碼。|  
|`Title`|字串|作業的描述。|  
|`HttpMethod`|字串|作業的 HTTP 方法。|  
  
##  <a name="paging"></a><a name="Paging"></a>速度  
 `paging` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Page`|number|目前的頁碼。|  
|`PageSize`|number|要在單一頁面中顯示的結果數上限。|  
|`TotalItemCount`|number|要顯示的項目數。|  
|`ShowAll`|boolean|是否要在單一頁面中顯示所有結果。|  
|`PageCount`|number|結果的頁數。|  
  
##  <a name="parameter"></a><a name="Parameter"></a>實參  
 本區段描述 `parameter` 表示法。  
  
|屬性|類型|Description|  
|--------------|-----------------|----------|  
|`name`|字串|參數名稱。|  
|`description`|字串|參數描述。|  
|`value`|字串|參數值。|  
|`options`|字串陣列|針對查詢參數值所定義的值。|  
|`required`|boolean|指定參數是否為必要。|  
|`kind`|number|此參數為路徑參數 (1) 還是查詢字串參數 (2)。|  
|`typeName`|字串|參數類型。|  
  
##  <a name="product"></a><a name="Product"></a>基礎  
 `product` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Id`|字串|資源識別碼。 可唯一識別目前 API 管理服務執行個體內的產品。 其值為 `products/{pid}` 格式的有效相對 URL，其中 `{pid}` 是產品識別碼。 這個屬性是唯讀的。|  
|`Title`|字串|產品的名稱。 不能空白。 長度上限是 100 個字元。|  
|`Description`|字串|產品的描述。 不能空白。 可包含 HTML 格式標籤。 長度上限是 1000 個字元。|  
|`Terms`|字串|產品使用規定。 嘗試訂閱產品的開發人員將會看到且必須接受這些使用規定，才能完成訂閱程序。|  
|`ProductState`|number|指定產品是否已發佈。 開發人員可在開發人員入口網站中找到已發佈的產品。 未發佈的產品則只有系統管理員能看見。<br /><br /> 產品狀態的允許值為︰<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|`AllowMultipleSubscriptions`|boolean|指定使用者是否可以同時對此產品擁有多個訂閱。|  
|`MultipleSubscriptionsCount`|number|允許使用者同時擁有此產品的訂用帳戶數目上限。|  
  
##  <a name="provider"></a><a name="Provider"></a>那裡  
 `provider` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Properties`|字串字典|此驗證提供者的屬性。|  
|`AuthenticationType`|字串|提供者類型  (Azure Active Directory、Facebook 登入、Google 帳戶、Microsoft 帳戶、Twitter)。|  
|`Caption`|字串|提供者的顯示名稱。|  
  
##  <a name="representation"></a><a name="Representation"></a>表示  
 本區段描述 `representation`。  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`contentType`|字串|為此表示法指定已註冊或自訂內容類型，例如 `application/xml`。|  
|`sample`|字串|表示法的範例。|  
  
##  <a name="subscription"></a><a name="Subscription"></a> 訂閱  
 `subscription` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Id`|字串|資源識別碼。 可唯一識別目前 API 管理服務執行個體內的訂用帳戶。 其值為 `subscriptions/{sid}` 格式的有效相對 URL，其中 `{sid}` 是訂用帳戶識別碼。 這個屬性是唯讀的。|  
|`ProductId`|字串|已訂閱之產品的產品資源識別碼。 其值為 `products/{pid}` 格式的有效相對 URL，其中 `{pid}` 是產品識別碼。|  
|`ProductTitle`|字串|產品的名稱。 不能空白。 長度上限是 100 個字元。|  
|`ProductDescription`|字串|產品的描述。 不能空白。 可包含 HTML 格式標籤。 長度上限是 1000 個字元。|  
|`ProductDetailsUrl`|字串|產品詳細資料的相對 URL。|  
|`state`|字串|訂用帳戶的狀態。 可能的狀態為：<br /><br /> - `0 - suspended` – 訂用帳戶已遭封鎖，而且訂閱者無法呼叫產品的任何 API。<br /><br /> - `1 - active` – 訂用帳戶是作用中狀態。<br /><br /> - `2 - expired` – 訂用帳戶已達到期日，因此已停用。<br /><br /> - `3 - submitted` – 開發人員已提出訂用帳戶要求，但尚未進行核准或拒絕。<br /><br /> - `4 - rejected` – 系統管理員已拒絕訂用帳戶要求。<br /><br /> - `5 - cancelled` – 開發人員或系統管理員已取消訂用帳戶。|  
|`DisplayName`|字串|訂用帳戶的顯示名稱。|  
|`CreatedDate`|dateTime|訂用帳戶的建立日期，採用 ISO 8601 格式︰`2014-06-24T16:25:00Z`。|  
|`CanBeCancelled`|boolean|目前的使用者是否可取消訂用帳戶。|  
|`IsAwaitingApproval`|boolean|訂用帳戶是否正在等待核准。|  
|`StartDate`|dateTime|訂用帳戶的開始日期，採用 ISO 8601 格式︰`2014-06-24T16:25:00Z`。|  
|`ExpirationDate`|dateTime|訂用帳戶的到期日期，採用 ISO 8601 格式︰`2014-06-24T16:25:00Z`。|  
|`NotificationDate`|dateTime|訂用帳戶的通知日期，採用 ISO 8601 格式︰`2014-06-24T16:25:00Z`。|  
|`primaryKey`|字串|主要訂用帳戶金鑰。 長度上限是 256 個字元。|  
|`secondaryKey`|字串|次要訂用帳戶金鑰。 長度上限是 256 個字元。|  
|`CanBeRenewed`|boolean|目前的使用者是否可以更新訂用帳戶。|  
|`HasExpired`|boolean|訂用帳戶是否已到期。|  
|`IsRejected`|boolean|是否已拒絕訂用帳戶要求。|  
|`CancelUrl`|字串|要取消訂用帳戶的相對 Url。|  
|`RenewUrl`|字串|要更新訂用帳戶的相對 Url。|  
  
##  <a name="subscription-summary"></a><a name="SubscriptionSummary"></a>訂用帳戶摘要  
 `subscription summary` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Id`|字串|資源識別碼。 可唯一識別目前 API 管理服務執行個體內的訂用帳戶。 其值為 `subscriptions/{sid}` 格式的有效相對 URL，其中 `{sid}` 是訂用帳戶識別碼。 這個屬性是唯讀的。|  
|`DisplayName`|字串|訂用帳戶的顯示名稱|  
  
##  <a name="user-account-info"></a><a name="UserAccountInfo"></a>使用者帳戶資訊  
 `user account info` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`FirstName`|字串|名字。 不能空白。 長度上限是 100 個字元。|  
|`LastName`|字串|姓氏。 不能空白。 長度上限是 100 個字元。|  
|`Email`|字串|電子郵件地址。 不能空白，且在服務執行個體內必須是唯一的。 長度上限是 254 個字元。|  
|`Password`|字串|使用者帳戶密碼。|  
|`NameIdentifier`|字串|帳戶識別碼，與使用者的電子郵件相同。|  
|`ProviderName`|字串|驗證提供者名稱。|  
|`IsBasicAccount`|boolean|如果此帳戶是使用電子郵件和密碼進行註冊，則為 true；如果帳戶是使用提供者進行註冊，則為 false。|  
  
##  <a name="user-sign-in"></a><a name="UseSignIn"></a>使用者登入  
 `user sign in` 實體具有下列屬性：  
  
|屬性|類型|Description|  
|--------------|----------|-----------------|  
|`Email`|字串|電子郵件地址。 不能空白，且在服務執行個體內必須是唯一的。 長度上限是 254 個字元。|  
|`Password`|字串|使用者帳戶密碼。|  
|`ReturnUrl`|字串|使用者按下登入時所在頁面的 URL。|  
|`RememberMe`|boolean|是否要儲存目前使用者的資訊。|  
|`RegistrationEnabled`|boolean|是否啟用註冊。|  
|`DelegationEnabled`|boolean|是否啟用委派的登入。|  
|`DelegationUrl`|字串|委派的登入 url (如果已啟用)。|  
|`SsoSignUpUrl`|字串|使用者的單一登入 URL (如果有的話)。|  
|`AuxServiceUrl`|字串|如果目前的使用者是系統管理員，這是 Azure 入口網站中服務執行個體的連結。|  
|`Providers`|[提供者](#Provider)實體的集合|此使用者的驗證提供者。|  
|`UserRegistrationTerms`|字串|使用者必須先同意才能登入的條款。|  
|`UserRegistrationTermsEnabled`|boolean|是否啟用條款。|  
  
##  <a name="user-sign-up"></a><a name="UserSignUp"></a>使用者註冊  
 `user sign up` 實體具有下列屬性：  
  
|屬性|類型|描述|  
|--------------|----------|-----------------|  
|`PasswordConfirm`|boolean|[註冊](api-management-page-controls.md#sign-up)註冊控制項所使用的值。|  
|`Password`|字串|使用者帳戶密碼。|  
|`PasswordVerdictLevel`|number|[註冊](api-management-page-controls.md#sign-up)註冊控制項所使用的值。|  
|`UserRegistrationTerms`|字串|使用者必須先同意才能登入的條款。|  
|`UserRegistrationTermsOptions`|number|[註冊](api-management-page-controls.md#sign-up)註冊控制項所使用的值。|  
|`ConsentAccepted`|boolean|[註冊](api-management-page-controls.md#sign-up)註冊控制項所使用的值。|  
|`Email`|字串|電子郵件地址。 不能空白，且在服務執行個體內必須是唯一的。 長度上限是 254 個字元。|  
|`FirstName`|字串|名字。 不能空白。 長度上限是 100 個字元。|  
|`LastName`|字串|姓氏。 不能空白。 長度上限是 100 個字元。|  
|`UserData`|字串|[註冊](api-management-page-controls.md#sign-up)控制項所使用的值。|  
|`NameIdentifier`|字串|[註冊](api-management-page-controls.md#sign-up)註冊控制項所使用的值。|  
|`ProviderName`|字串|驗證提供者名稱。|

## <a name="next-steps"></a>後續步驟
如需有關使用範本的詳細資訊，請參閱[如何使用範本自訂 API 管理開發人員入口網站](api-management-developer-portal-templates.md)。
