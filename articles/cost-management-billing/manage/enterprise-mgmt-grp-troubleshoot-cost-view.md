---
title: 針對 Azure 企業成本檢視進行疑難排解
description: 了解如何解決 Azure 入口網站中與組織成本檢視有關的任何問題。
author: bandersmsft
ms.reviewer: amberb
ms.service: cost-management-billing
ms.subservice: enterprise
ms.topic: troubleshooting
ms.date: 08/20/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: bd0abadb624bced7f603fcbd112f7c422e88fc0c
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2020
ms.locfileid: "91371842"
---
# <a name="troubleshoot-enterprise-cost-views"></a>為企業成本檢視疑難排解

在企業註冊內，有多個設定可能導致註冊內的使用者無法檢視成本。  這些設定由註冊管理員負責管理。 或者，如果並非直接經由 Microsoft 購買註冊，則會由合作夥伴管理設定。  本文可協助您了解設定，以及這些設定如何影響註冊。 這些設定與 Azure 角色無關。

## <a name="enable-access-to-costs"></a>能夠存取成本

您查看成本資訊時，是否看見「未經授權」訊息或「成本檢視在您的註冊中停用」  ？
![訂用帳戶的「目前成本」欄位顯示「未授權」的螢幕擷取畫面。](./media/enterprise-mgmt-grp-troubleshoot-cost-view/unauthorized.png)

這可能是下列其中一個原因所造成：

1. 您是經由企業夥伴購買 Azure，而該夥伴尚未發佈價格。 請連絡夥伴更新[企業版入口網站](https://ea.azure.com)內的價格設定。
2. 如果您是 EA Direct 客戶，有幾種可能：
    * 您是帳戶擁有者，且您的註冊管理員已停用 [AO 檢視費用]  設定。  
    * 您是部門管理員，且您的註冊管理員已停用 [DA 檢視費用]  設定。
    * 請連絡您的註冊管理員以取得存取權。 註冊管理員可以更新[企業版入口網站](https://ea.azure.com/manage/enrollment)中的設定。

      ![顯示企業版入口網站檢視費用設定的螢幕擷取畫面。](./media/enterprise-mgmt-grp-troubleshoot-cost-view/ea-portal-settings.png)

## <a name="asset-is-unavailable"></a>資產無法使用

如果您在嘗試存取訂用帳戶或管理群組時收到「此資產無法使用」  錯誤訊息，則表示您不具備可檢視此項目的正確角色。  

![顯示「資產無法使用」訊息的螢幕擷取畫面。](./media/enterprise-mgmt-grp-troubleshoot-cost-view/asset-not-found.png)

向 Azure 訂用帳戶或管理群組系統管理員要求存取。 如需詳細資訊，請參閱[使用 Azure 入口網站新增或移除 Azure 角色指派](../../role-based-access-control/role-assignments-portal.md)。

## <a name="next-steps"></a>後續步驟
- 如果您有問題或需要協助，請[建立支援要求](https://go.microsoft.com/fwlink/?linkid=2083458)。
