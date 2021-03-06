---
title: Azure API 管理術語 | Microsoft Docs
description: 本文提供 API 管理專用的詞彙定義。
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/11/2017
ms.author: apimpm
ms.openlocfilehash: 5bc76d2526c5585071a240af36b8a31e3de5708f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87024923"
---
# <a name="azure-api-management-terminology"></a>Azure API 管理術語

本文提供 API 管理專用的詞彙定義 (APIM)。

## <a name="term-definitions"></a>詞彙定義

* **後端 API** -  實作您 API 和其作業的 HTTP 服務。 
* **前端 API** /**APIM API** -APIM API 不會裝載 api，它會為您的 api 建立 facade，以便根據您的需求自訂外觀，而不需要觸及後端 API。 如需詳細資訊，請參閱[匯入和發佈 API](import-and-publish.md)。
* **APIM 產品** -  產品包含一或多個 API 以及使用量配額與使用規定。 您可以包括數個 API，並透過開發人員入口網站將它們提供給開發人員。 如需詳細資訊，請參閱[建立和發佈產品](api-management-howto-add-products.md)。
* **APIM API 作業** -  每個 APIM API 都代表可供開發人員使用的一組作業。 每個 APIM API 都包含會實作 API 之後端服務的參考，而其作業會與後端服務實作的作業相對應。 如需詳細資訊，請參閱[模擬 API 回應](mock-api-responses.md)。
* **版本** - 有時您想要發佈新的或不同 API 功能給某些使用者，而其他人想要堅持使用目前適用的 API。 如需詳細資訊，請參閱[發佈多個 API 版本](api-management-get-started-publish-versions.md)。
* **修訂** - 當您的 API 已準備就緒，而且開發人員也已準備好使用時，對該 API 的變更通常需要特別小心，同時避免中斷 API 的呼叫者作業。 這對於讓開發人員了解您所做的更改也相當有用。 如需詳細資訊，請參閱[使用修訂](api-management-get-started-revise-api.md)。
* **開發人員入口網站**您的客戶 (開發人員) 應使用開發人員入口網站來存取您的 API。 您可以自訂開發人員入口網站。 如需詳細資訊，請參閱[自訂開發人員入口網站](api-management-customize-styles.md)。

## <a name="next-steps"></a>接下來的步驟

> [!div class="nextstepaction"]
> [建立執行個體](get-started-create-service-instance.md)

