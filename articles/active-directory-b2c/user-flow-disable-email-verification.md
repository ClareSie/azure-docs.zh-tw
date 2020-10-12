---
title: 在客戶註冊期間停用電子郵件驗證
titleSuffix: Azure AD B2C
description: 瞭解如何在 Azure Active Directory B2C 中的客戶註冊期間停用電子郵件驗證。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 10613bd2d6219272248f882e45ae1c33d2cc9d61
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85384204"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中的客戶註冊期間停用電子郵件驗證

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

請遵循下列步驟來停用電子郵件驗證：

1. 登入 [Azure 入口網站](https://portal.azure.com)
1. 使用上方功能表中的 [ **目錄 + 訂** 用帳戶] 篩選，來選取包含您 Azure AD B2C 租使用者的目錄。
1. 在左側功能表中，選取 [Azure AD B2C]。 或者，選取 [所有服務]，然後搜尋並選取 [Azure AD B2C]。
1. 選取 [使用者流程]****。
1. 選取您要停用電子郵件驗證的使用者流程。 例如， *B2C_1_signinsignup*。
1. 選取 [ **頁面配置**]。
1. 選取 **本機帳戶註冊頁面**。
1. 在 [ **使用者屬性**] 下，選取 [ **電子郵件地址**]。
1. 在 [ **需要驗證** ] 下拉式清單中，選取 [ **否**]。
1. 選取 [儲存]****。 此使用者流程的電子郵件驗證現在已停用。

## <a name="next-steps"></a>後續步驟

- 瞭解如何 [在 Azure Active Directory B2C 中自訂使用者介面](customize-ui-overview.md)

