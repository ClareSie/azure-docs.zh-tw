---
title: 在 Azure 門戶 - Azure AD 中查看託管標識的服務主體
description: 逐步指導您如何在 Azure 入口網站中檢視受控識別的服務主體。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: c12f15cc79d5329d028239ade4e18a853000bf01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298592"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>在 Azure 入口網站中檢視受控識別的服務主體

Azure 資源受控識別會在 Azure Active Directory 中為 Azure 服務提供自動受控識別。 您可以使用此身分識別來向任何支援 Azure AD 驗證的服務進行驗證，不需要任何您程式碼中的認證。 

在本文中，您會了解如何使用 Azure 入口網站來檢視受控識別的服務主體。

 > [!NOTE] 
 > 服務主體是企業應用程式。 

## <a name="prerequisites"></a>Prerequisites

- 如果您不熟悉 Azure 資源的受控識別，請參閱[概觀一節](overview.md)。
- 如果您還沒有 Azure 帳戶，請[註冊免費帳戶](https://azure.microsoft.com/free/)。
- 啟用[虛擬機器](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity)或[應用程式](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity)上的系統指派身分識別。

## <a name="view-the-service-principal"></a>檢視服務主體

此程序示範如何檢視已啟用系統指派身分識別的 VM 所具有的服務主體 (應用程式適用同樣的步驟)。

1. 按一下 [Azure Active Directory]****，然後按一下 [企業應用程式]****。
2. 在 **"應用程式類型"** 下，選擇 **"所有應用程式**"，然後按一下"**應用**"。
3. 在搜尋篩選方塊中，輸入已啟用受控識別的 VM 或應用程式所具有的名稱，或從顯示的清單中選出。

   ![在入口網站中檢視受控識別服務主體](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>後續步驟

[適用於 Azure 資源的受控識別](/azure/active-directory/managed-identities-azure-resources/overview)

