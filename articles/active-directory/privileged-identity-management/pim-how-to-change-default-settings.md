---
title: 在 PIM 中設定 Azure AD 角色設定-Azure AD |Microsoft Docs
description: 瞭解如何在 Azure AD Privileged Identity Management （PIM）中設定 Azure AD 角色設定。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 02/28/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 334f239ea8382a67add9655d4a25f98d254567d3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743944"
---
# <a name="configure-azure-ad-role-settings-in-privileged-identity-management"></a>在 Privileged Identity Management 中設定 Azure AD 角色設定

特殊權限角色管理員可以在其 Azure Active Directory （Azure AD）組織中自訂 Privileged Identity Management （PIM），包括變更啟用合格角色指派之使用者的體驗。

## <a name="determine-your-version-of-pim"></a>判斷您的 PIM 版本

從2019年11月開始，Privileged Identity Management 的 Azure AD 角色部分更新為符合 Azure 資源角色體驗的新版本。 這會建立額外的功能，以及[現有 API 的變更](azure-ad-roles-features.md#api-changes)。 推出新版本時，您在本文中遵循的程式取決於您目前擁有的 Privileged Identity Management 版本。 請依照本節中的步驟來判斷您擁有的 Privileged Identity Management 版本。 知道您的 Privileged Identity Management 版本之後，您可以選取本文中符合該版本的程式。

1. 以[特殊權限角色管理員](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)角色的使用者身分登入[Azure 入口網站](https://portal.azure.com/)。
1. 開啟**Azure AD Privileged Identity Management**。 如果您在 [總覽] 頁面頂端有橫幅，請遵循本文的 [**新版本**] 索引標籤中的指示。 否則，請依照 [**先前版本**] 索引標籤中的指示進行。

  [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

請遵循這篇文章中的步驟，核准或拒絕 Azure AD 角色的要求。

# <a name="new-version"></a>[新版本](#tab/new)

## <a name="open-role-settings"></a>開啟角色設定

請依照下列步驟開啟 Azure AD 角色的設定。

1. 以特殊[許可權角色管理員](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)角色中的使用者身分登入[Azure 入口網站](https://portal.azure.com/)。
gt
1. 開啟**Azure AD Privileged Identity Management** &gt; **Azure AD 角色**] &gt; **角色設定**。

    ![列出 Azure AD 角色的角色設定頁面](./media/pim-how-to-change-default-settings/role-settings.png)

1. 選取您要設定其設定的角色。

    ![列出數個指派和啟用設定的角色設定詳細資料頁面](./media/pim-how-to-change-default-settings/role-settings-page.png)

1. 選取 [編輯]**** 以開啟 [角色設定] 頁面。

    ![[編輯角色設定] 頁面，其中包含更新指派和啟用設定的選項](./media/pim-how-to-change-default-settings/role-settings-edit.png)

    在每個角色的 [角色設定] 窗格上，有許多您可以設定的設定。

## <a name="assignment-duration"></a>指派持續時間

當您進行角色的設定時，每個指派類型 (合格和有效) 都有兩個指派持續時間選項可選擇。 在 Privileged Identity Management 中將使用者指派給角色時，這些選項會變成預設的最大持續時間。

您可以選擇下列其中一個**合格**的指派持續時間選項：

| | |
| --- | --- |
| **允許永久合格的指派** | 全域管理員和特殊許可權角色管理員可以指派永久的合格指派。 |
| **合格指派的到期時間** | 全域管理員和特殊許可權角色管理員可以要求所有合格指派都具有指定的開始和結束日期。 |

此外，您可以從下列**有效**指派持續時間選項中選擇一個：

| | |
| --- | --- |
| **允許永久有效的指派** | 全域管理員和特殊許可權角色管理員可以指派永久的主動指派。 |
| **有效指派的到期時間** | 全域管理員和特殊許可權角色管理員可以要求所有作用中的指派都具有指定的開始和結束日期。 |

> [!NOTE]
> 具有指定結束日期的所有指派都可以由全域管理員和特殊許可權角色管理員進行更新。 此外，使用者也可以起始自助要求，以[延長或更新角色指派](pim-resource-roles-renew-extend.md)。

## <a name="require-multi-factor-authentication"></a>需要多重要素驗證

Privileged Identity Management 可針對兩個不同的案例選擇性地強制執行 Azure Multi-Factor Authentication。

### <a name="require-multi-factor-authentication-on-active-assignment"></a>要求為有效指派進行 Multi-Factor Authentication

在某些情況下，您可能會想要在短時間內將使用者指派給某個角色（例如一天）。 在此情況下，指派的使用者不需要要求啟用。 在此案例中，當使用者使用其角色指派時，Privileged Identity Management 無法強制執行多重要素驗證，因為它們已在角色中從指派的時間開始生效。

若要確保系統管理員滿足指派，您可以藉由勾選 [在作用中**指派時需要多重要素驗證**] 方塊，對作用中的指派強制執行多重要素驗證。

### <a name="require-multi-factor-authentication-on-activation"></a>啟用時需要 Multi-Factor Authentication

您可以要求具有角色資格的使用者，證明他們使用 Azure 多重要素驗證的物件，然後才能啟用。 多重要素驗證可確保使用者的身分為合理的確定性。 在使用者帳戶可能受到危害的情況下，強制執行這個選項可保護重要資源。

若要在啟用前要求多重要素驗證，請在 [**編輯角色設定**] 的 [指派] 索引標籤中，核取 [**啟用時需要多重要素驗證**] 方塊。

如需詳細資訊，請參閱[多重要素驗證和 Privileged Identity Management](pim-how-to-require-mfa.md)。

## <a name="activation-maximum-duration"></a>啟用持續時間上限

使用 [啟用持續時間上限]**** 滑桿，可設定角色在到期前維持作用中狀態的最長時間 (以小時為單位)。 此值可以是1到24小時。

## <a name="require-justification"></a>需要理由

您可以要求使用者在啟動時輸入業務理由。 若要要求提供理由，請核取 [進行有效指派時需要提供理由]**** 方塊，或核取 [啟用時需要提供理由]**** 方塊。

## <a name="require-approval-to-activate"></a>需要核准才可啟用

如果設定多個核准者，則核准會在其中一項核准或拒絕時立即完成。 您不能要求至少有兩位使用者的核准。 若要要求核准以啟用角色，請遵循下列步驟。

1. 請核取 [需要核准才可啟用]**** 核取方塊。

1. 選取 [**選取核准者**]。

    ![選取使用者或群組窗格以選取核准者](./media/pim-resource-roles-configure-role-settings/resources-role-settings-select-approvers.png)

1. 選取至少一位使用者，然後按一下 [**選取**]。 您必須至少選取一個核准者。 沒有任何預設核准者。

    您的選項將出現在所選取的核准者清單中。

1. 當您指定所有角色設定後，請選取 [**更新**] 以儲存變更。

# <a name="previous-version"></a>[先前的版本](#tab/previous)

## <a name="open-role-settings"></a>開啟角色設定

請依照下列步驟開啟 Azure AD 角色的設定。

1. 開啟**Azure AD Privileged Identity Management**。

1. 選取 [ **Azure AD 角色**]。

1. 選取 [設定]。

    ![Azure AD 角色-設定](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. 選取 [角色]****。

1. 選取您要設定其設定的角色。

    ![Azure AD 角色-設定角色](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    在每個角色的設定頁面上，有許多您可以設定的設定。 這些設定只會影響身為**合格**指派 (而不是**永久**指派) 的使用者。

## <a name="activations"></a>啟用

使用 [啟用]**** 滑桿，設定角色在到期前維持作用中狀態的最長時間 (以小時為單位)。 此值可介於 1 到 72 小時。

## <a name="notifications"></a>通知

使用 [通知]**** 參數來指定啟動角色時，系統管理員是否會收到電子郵件通知。 此通知有助於偵測未經授權或不合法的啟用。

設定為 [已啟用]**** 時，就會傳送通知給：

- 特殊權限角色管理員
- 安全性系統管理員
- 全域管理員

如需詳細資訊，請參閱[Privileged Identity Management 中的電子郵件通知](pim-email-notifications.md)。

## <a name="incidentrequest-ticket"></a>事件/要求票證

使用「**事件/要求票證**」參數，要求合格系統管理員在啟用其角色時包含票證號碼。 這種作法可讓角色存取審核更有效率。

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

使用 [Multi-Factor Authentication]**** 的切換，指定是否要要求使用者在啟用其角色之前，先以 MFA 驗證其身分識別。 他們只需要在每個會話中驗證其身分識別一次，而不是每次啟動角色。 啟用 MFA 時，需要記住兩個秘訣：

- 擁有 Microsoft 帳戶作為其電子郵件地址（通常是 @outlook.com ，但不一定）的使用者無法註冊 Azure 多重要素驗證。 如果您想要將角色指派給具有 Microsoft 帳戶的使用者，您應該將其設為永久系統管理員或停用該角色的多重要素驗證。
- 您無法針對 Azure AD 和 Office 365 的高特殊許可權角色停用 Azure 多重要素驗證。 此安全性功能可協助保護下列角色：  
  
  - Azure 資訊保護管理員
  - 計費管理員
  - 雲端應用程式系統管理員
  - 規範管理員
  - 條件式存取管理員
  - Dynamics 365 管理員
  - 客戶 LockBox 存取核准者
  - 目錄寫入器
  - Exchange 系統管理員
  - 全域管理員
  - Intune 管理員
  - Power BI 管理員
  - 特殊權限角色管理員
  - 安全性系統管理員
  - SharePoint 管理員
  - 商務用 Skype 的管理員
  - 使用者管理員

如需詳細資訊，請參閱[多重要素驗證和 Privileged Identity Management](pim-how-to-require-mfa.md)。

## <a name="require-approval"></a>需要核准

如果您想要委派所需的核准來啟用角色，請遵循下列步驟。

1. 將 [需要核准]**** 切換為 [啟用]****。 展開的窗格會有選取核准者的選項。

    ![Azure AD 角色-設定-需要核准](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    如果您未指定任何核准者，特殊權限角色管理員會變成預設核准者，然後需要核准此角色的所有啟用要求。

1. 若要新增核准者，請按一下 [**選取核准者**]。

    ![Azure AD 角色-設定-需要核准](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. 除了特殊權限角色管理員以外，請選取一或多個核准者，然後按一下 [**選取**]。 我們建議您至少新增兩個核准者。 即使您將自己新增為核准者，也無法自行核准角色啟用。 您的選項將出現在所選取的核准者清單中。

1. 在您指定所有角色設定之後，請選取 [**儲存**] 以儲存變更。

---

## <a name="next-steps"></a>後續步驟

- [在 Privileged Identity Management 中指派 Azure AD 角色](pim-how-to-add-role-to-user.md)
- [在 Privileged Identity Management 中設定 Azure AD 角色的安全性警示](pim-how-to-configure-security-alerts.md)
