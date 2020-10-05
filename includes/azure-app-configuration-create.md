---
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: include
ms.date: 1/31/2020
ms.openlocfilehash: c8044ca5a52bb3acf1e154a7b835471d09072083
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91372320"
---
1. 若要建立新的應用程式組態存放區，請登入 [Azure 入口網站](https://portal.azure.com)。 在首頁的左上角，選取 [建立資源]  。 在 [搜尋 Marketplace] 方塊中輸入*應用程式組態*，然後選取 <kbd>Enter</kbd> 鍵。

    ![搜尋應用程式設定](media/azure-app-configuration-create/azure-portal-search.png)

1. 從搜尋結果中選取 [應用程式設定]  ，然後選取 [建立]  。

    ![選取 [建立]](media/azure-app-configuration-create/azure-portal-app-configuration-create.png)

1. 在 [建立應用程式組態] 窗格上，輸入下列設定：

    | 設定 | 建議的值 | 描述 |
    |---|---|---|
    | **訂用帳戶** | 您的訂用帳戶 | 選取您要用來測試應用程式設定的 Azure 訂用帳戶。 如果您的帳戶僅有一個訂用帳戶，則會自動加以選取，而且不會顯示 [訂用帳戶]**** 清單。 |
    | **資源群組** | *AppConfigTestResources* | 為應用程式組態存放區資源選取或建立資源群組。 此群組可用於組織多個資源，以便能夠藉由刪除資源群組來同時刪除多個資源。 如需詳細資訊，請參閱[使用資源群組管理您的 Azure 資源](/azure/azure-resource-manager/resource-group-overview)。 |
    | **資源名稱** | 全域唯一的名稱 | 輸入要對應用程式組態存放區資源使用的唯一資源名稱。 名稱必須是介於 5 到 50 個字元的字串，且只能包含數字、字母和 `-` 字元。 名稱的開頭或結尾不可以是 `-` 字元。 |
    | **位置** | *美國中部* | 使用 [位置]  來指定裝載應用程式設定存放區所在的地理位置。 為了獲得最佳效能，請將資源建立在與應用程式其他元件相同的區域內。 |
    | **定價層** | *免費* | 選取所需的定價層。 如需詳細資訊，請參閱[應用程式組態定價頁面](https://azure.microsoft.com/pricing/details/app-configuration)。 |

1. 選取 [檢閱 + 建立]  以驗證您的設定。

1. 選取 [建立]  。 部署可能需要幾分鐘的時間。

1. 部署完成之後，瀏覽至應用程式組態資源。 選取 [設定] > [存取金鑰]。 請記下連接字串的主要唯讀索引鍵。 稍後，您會使用此連接字串來設定您的應用程式，使其與您建立的應用程式組態存放區進行通訊。
