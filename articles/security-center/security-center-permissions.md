---
title: Azure 資訊安全中心的權限 | Microsoft Docs
description: 本文說明 Azure 資訊安全中心如何使用角色型存取控制，將權限指派給使用者，並識別每個角色允許的動作。
services: security-center
cloud: na
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 6e61571400930d4a781d6d67647bd662a7f2d350
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82106214"
---
# <a name="permissions-in-azure-security-center"></a>Azure 資訊安全中心的權限

Azure 資訊安全中心會使用[角色型存取控制 (RBAC)](../role-based-access-control/role-assignments-portal.md)，以提供可在 Azure 中指派給使用者、群組與服務的[內建角色](../role-based-access-control/built-in-roles.md)。

資訊安全中心會評估資源的組態，以識別安全性問題與弱點。 在「資訊安全中心」中，當您獲指派為資源所屬的訂用帳戶或資源群組「擁有者」、「參與者」或「讀取者」角色時，您只會看到與資源相關的項目。

除了這些角色，有兩個特定的資訊安全中心角色：

* **安全性讀取者**：屬於此角色的使用者有檢視資訊安全中心的權限。 使用者可以檢視建議、警示、安全性原則和安全性狀態，但無法進行變更。
* **安全性系統管理員**：屬於此角色的使用者具有與安全性讀取者相同的許可權，而且也可以更新安全性原則，並關閉警示和建議。

> [!NOTE]
> 安全性角色、安全性讀取者和安全性系統管理員只能在資訊安全中心中存取。 上述安全性角色無法存取 Azure 的其他服務區域，例如儲存體、Web 和行動或物聯網。
>
>

## <a name="roles-and-allowed-actions"></a>角色和允許的動作

下表會顯示資訊安全中心的角色和允許的動作。

| [角色] | 編輯安全性原則 | 針對資源套用安全性建議</br> （包含「快速修正！」） | 關閉警示和建議 | 檢視警示和建議 |
|:--- |:---:|:---:|:---:|:---:|
| 訂用帳戶擁有者 | ✔ | ✔ | ✔ | ✔ |
| 訂用帳戶參與者 | -- | ✔ | ✔ | ✔ |
| 資源群組擁有者 | -- | ✔ | -- | ✔ |
| 資源群組參與者 | -- | ✔ | -- | ✔ |
| 讀取者 | -- | -- | -- | ✔ |
| 安全性系統管理員 | ✔ | -- | ✔ | ✔ |
| 安全性讀取者 | -- | -- | -- | ✔ |

> [!NOTE]
> 我們建議您指派所需的最寬鬆角色，以便使用者完成其工作。 例如，將「讀取者」角色指派給只需要檢視資源安全性狀態的相關資訊，但不採取行動的使用者，例如套用建議或編輯原則。
>
>

## <a name="next-steps"></a>後續步驟
本文說明資訊安全中心如何使用 RBAC，將權限指派給使用者，並識別每個角色允許的動作。 現在，您已熟悉監視您的訂用帳戶的安全性狀態所需的角色指派，編輯安全性原則和套用建議，接著了解如何︰

- [在資訊安全中心設定安全性原則](tutorial-security-policy.md)
- [管理資訊安全中心的安全性建議](security-center-recommendations.md)
- [監視您的 Azure 資源的安全性健全狀況](security-center-monitoring.md)
- [管理和回應資訊安全中心中的安全性警示](security-center-managing-and-responding-alerts.md)
- [監視合作夥伴安全性解決方案](security-center-partner-solutions.md)
