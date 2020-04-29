---
title: 在 Azure 資訊安全中心管理安全性警示 | Microsoft Docs
description: 本文件可協助您使用「Azure 資訊安全中心」功能來管理及回應安全性警示。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: 8e44ce594375deeac47f037515d96c57d15c8359
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "80398395"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>在 Azure 資訊安全中心管理和回應安全性警示

本主題說明如何查看並處理您收到的警示，以保護您的資源。 

* 若要瞭解不同類型的警示，請參閱[安全性警示類型](alerts-reference.md)。
* 如需資訊安全中心如何產生警示的總覽，請參閱[Azure 資訊安全中心如何偵測及回應威脅](security-center-alerts-overview.md)。

> [!NOTE]
> 若要啟用進階偵測，請升級至 Azure 資訊安全中心標準。 有免費試用版可用。 若要升級，請選取[安全性原則](tutorial-security-policy.md)中的 [定價層]。 若要深入了解，請參閱 [Azure 資訊安全中心價格](security-center-pricing.md)。

## <a name="what-are-security-alerts"></a>什麼是安全性警示：
資訊安全中心會自動收集、分析及整合您 Azure 資源、網路和已連線的合作夥伴解決方案 (例如防火牆和端點保護解決方案) 的記錄檔資料，來偵測真正的威脅並減少誤判情形。 「資訊安全中心」會顯示優先安全性警示清單，以及需要您快速調查問題的資訊，和如何修復攻擊行為的建議。

> [!NOTE]
> 如需資訊安全中心偵測功能如何發揮作用的詳細資訊，請參閱[Azure 資訊安全中心如何偵測及回應威脅](security-center-alerts-overview.md#detect-threats)。

## <a name="manage-your-security-alerts"></a>管理您的安全性警示

1. 從 [資訊安全中心] 儀表板中，參閱 [**威脅防護**] 磚，以查看並瞭解警示。

    ![資訊安全中心的 [安全性警示] 圖格](./media/security-center-managing-and-responding-alerts/security-center-dashboard-alert.png)

1. 若要查看有關警示的更多詳細資料，請按一下磚。

   ![資訊安全中心內的安全性警示](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. 若要篩選顯示的警示，請按一下 [**篩選**]，然後在開啟的**篩選**器分頁中選取您要套用的篩選選項。 清單會根據選取的篩選準則來進行更新。 篩選可能非常有説明。 例如，您可能想確認在過去 24 小時發生的安全性警示，因為您正在調查系統中可能的入侵行動。

    ![篩選資訊安全中心的警示](./media/security-center-managing-and-responding-alerts/security-center-filter-alerts.png)

## <a name="respond-to-security-alerts"></a>回應安全性警示

1. 從 [**安全性警示**] 清單中，按一下 [安全性警示]。 其中會顯示相關的資源，以及補救攻擊所需採取的步驟。

    ![回應安全性警示](./media/security-center-managing-and-responding-alerts/security-center-alert.png)

1. 查看資訊之後，請按一下受攻擊的資源。

    ![關於如何處理安全性警示的建議](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    [**一般資訊**] 區段可以提供觸發安全性警示的深入解析。 它會顯示如目標資源、來源 IP 位址（如果適用的話）、警示是否仍在作用中的資訊，以及有關如何補救的建議。  

    > [!NOTE]
    >在某些情況下，來源 IP 位址無法使用，某些 Windows 安全性事件記錄檔不會包含 IP 位址。

1. 資訊安全中心會根據安全性警訊，建議您不同的補救步驟。 針對每個警示遵循這些步驟。 

    在某些情況下，為了減輕安全性警示，您可能必須使用其他 Azure 控制項或服務來執行建議的補救措施。 

## <a name="see-also"></a>請參閱

在本文件中，您了解到如何在資訊安全中心設定安全性原則。 如要深入了解資訊安全中心，請參閱下列主題：

- [Microsoft Learn 模組，說明如何使用 Azure 資訊安全中心的警示功能來監看和回應威脅](https://docs.microsoft.com/learn/modules/resolve-threats-with-azure-security-center/)
* [Azure 資訊安全中心中的安全性警示](security-center-alerts-overview.md)。
* [處理安全性事件](security-center-incident.md)