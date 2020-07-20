---
title: 在 Azure Active Directory B2C 中定義自訂屬性 | Microsoft Docs
description: 在 Azure Active Directory B2C 中定義您應用程式的自訂屬性，以收集您客戶的相關資訊。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d02a7acf1d59c628e3b83161ab9cabb4a19ca41a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85384986"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中定義自訂屬性

 每個客戶面向的應用程式對於需要收集的資訊都有不同的需求。 您的 Azure Active Directory B2C （Azure AD B2C）租使用者隨附一組儲存在屬性中的內建資訊，例如名字、姓氏、城市和郵遞區號。 Azure AD B2C 可讓您擴充儲存在每個客戶帳戶上的屬性組合。

 您可以在 [Azure 入口網站](https://portal.azure.com/) 中建立自訂屬性，並在您的註冊使用者流程、註冊或登入使用者流程，或設定檔編輯使用者流程中使用這些屬性。 您也可以使用 [Microsoft Graph API](manage-user-accounts-graph-api.md) 來讀取和寫入這些屬性。

## <a name="create-a-custom-attribute"></a>建立自訂屬性

1. 以 Azure AD B2C 租用戶的全域管理員身分登入 [Azure 入口網站](https://portal.azure.com/)。
2. 在 Azure 入口網站的右上角切換到您的 Azure AD B2C 租用戶，確定您使用的目錄包含該租用戶。 選取您的訂用帳戶資訊，然後選取 [切換目錄]****。

    ![切換為您的 Azure AD B2C 租用戶](./media/user-flow-custom-attributes/switch-directories.png)

    選擇包含您租用戶的目錄。

    ![在目錄和訂用帳戶篩選中反白顯示 B2C 租使用者](./media/user-flow-custom-attributes/select-directory.PNG)

3. 選擇 Azure 入口網站左上角的 [所有服務]，搜尋並選取 [Azure AD B2C]。
4. 選取 [使用者屬性]****，然後選取 [新增]****。
5. 提供自訂屬性的**名稱** (例如 "ShoeSize")
6. 選擇 [資料類型]****。 只有**字串**、**布林值**，以及 **Int** 可供使用。
7. 選擇性地輸入 [描述]****，以供參考。
8. 按一下 [建立]。

自訂屬性現在會在**使用者屬性**清單中提供，並在使用者流程中使用。 自訂屬性只會在第一次用於任何使用者流程中時建立，而不是在您將它加入至**使用者屬性**清單時建立。

## <a name="use-a-custom-attribute-in-your-user-flow"></a>在您的使用者流程中使用自訂屬性

1. 在 Azure AD B2C 租用戶中，選取 [使用者流程]****。
1. 選取您的原則 (例如「B2C_1_SignupSignin」) 以開啟之。
1. 選取 [使用者屬性]****，然後選取自訂屬性 (例如 "ShoeSize")。 按一下 [檔案] 。
1. 選取 [應用程式宣告]****，然後選取自訂屬性。
1. 按一下 [檔案] 。

一旦您使用新建立的自訂屬性來建立新的使用者，就可以在[Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)中查詢該物件。 或者，您可以在使用者流程上使用 [[執行使用者流程](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-user-flows)] 功能，以驗證客戶體驗。 現在您應該會在註冊期間收集的屬性清單中看到 **ShoeSize** ，其亦會在傳回至您應用程式的權杖中出現。
