---
title: 待用資料的內容仲裁加密
titleSuffix: Azure Cognitive Services
description: 內容仲裁的待用資料加密。
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 38fc21ee45db25f015a6b8b534b0d922efa636f2
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310541"
---
# <a name="content-moderator-encryption-of-data-at-rest"></a>待用資料的內容仲裁加密

內容仲裁會在保存到雲端時自動加密您的資料，以協助符合您的組織安全性和合規性目標。

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> 客戶管理的金鑰僅適用于 E0 定價層。 若要要求能夠使用客戶管理的金鑰，請填寫並提交 [[內容仲裁客戶管理的金鑰要求] 表單](https://aka.ms/cogsvc-cmk)。 大約需要3-5 個工作天的時間，才會收到要求的狀態。 視需求而定，您可能會放在佇列中，並已核准為可用空間。 一旦核准使用 CMK 與內容仲裁服務，您將需要建立新的內容仲裁資源，並選取 [E0] 作為 [定價層]。 一旦建立具有 E0 定價層的內容仲裁資源之後，您就可以使用 Azure Key Vault 來設定受控識別。

[!INCLUDE [cognitive-services-cmk](../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="enable-data-encryption-for-your-content-moderator-team"></a>為您的內容仲裁小組啟用資料加密

若要為您的內容仲裁審查小組啟用資料加密，請參閱[快速入門：在 web 上試用內容仲裁](quick-start.md#create-a-review-team)。  

> [!NOTE]
> 您必須提供具有「內容仲裁」 E0 定價層的_資源識別碼_。

## <a name="next-steps"></a>後續步驟

* 如需支援 CMK 之服務的完整清單，請參閱[客戶管理的金鑰，以取得認知服務](../encryption/cognitive-services-encryption-keys-portal.md)
* [什麼是 Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)？
* [認知服務客戶管理的金鑰要求表單](https://aka.ms/cogsvc-cmk)

