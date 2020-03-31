---
title: Azure AD Connect：後續步驟及如何管理 Azure AD Connect | Microsoft Docs
description: 了解如何針對 Azure AD Connect 擴充預設組態和作業工作。
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c204029557a73dc3f02015afb92c0fdbf0d4d50e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261290"
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>接下來的步驟，以及如何管理 Azure AD Connect
使用本文中的操作程序來自訂 Azure Active Directory (Azure AD) Connect，以符合您組織的需要和需求。  

## <a name="add-additional-sync-admins"></a>新增額外的同步處理管理員
根據預設，只有執行安裝的使用者和本機系統管理員，才能管理已安裝的同步處理引擎。 如想讓其他人存取及管理同步處理引擎，請找到本機伺服器上名為 ADSyncAdmins 的群組，將這些人加入這個群組中。

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>將授權指派給 Azure AD Premium 和 Enterprise Mobility Suite 使用者
既然您的使用者已同步到雲端，現在您必須為他們指派授權，他們才能開始使用雲端應用程式 (例如 Office 365)。

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>指派 Azure AD Premium 或 Enterprise Mobility Suite 授權

1. 以系統管理員身分登入 Azure 入口網站。
2. 選取左側的 [Active Directory]****。
3. 在 [Active Directory]**** 頁面上，在有您想要設定之使用者的目錄上按兩下。
4. 在目錄頁面頂端，選取 [授權] ****。
5. 在 [授權]**** 頁面上，選取 [Active Directory Premium]**** 或 [Enterprise Mobility Suite]****，然後按一下 [指派]****。
6. 在對話方塊中，選取您要對其指派授權的使用者，然後按一下核取記號圖示，以儲存變更。

## <a name="verify-the-scheduled-synchronization-task"></a>確認已排定的同步處理工作
使用 Azure 入口網站來檢查同步處理的狀態。

### <a name="to-verify-the-scheduled-synchronization-task"></a>確認已排程的同步處理工作
1. 以系統管理員身分登入 Azure 入口網站。
2. 選取左側的 [Active Directory]****。
3. 在左側，選擇**Azure AD 連接**
4. 在頁面頂部，記下最後一次同步。

![目錄同步處理時間](./media/how-to-connect-post-installation/verify2.png)

## <a name="start-a-scheduled-synchronization-task"></a>啟動已排定的同步處理工作
如果需要運行同步任務，可以通過以下操作執行此操作：

1. 按兩下 Azure AD 連接桌面快捷方式以啟動嚮導。
2. 按一下 [設定]****。
3. 在任務螢幕上，選擇 **"自訂同步"選項**，然後按一下"**下一步"**
4. 輸入您的 Azure AD 認證
5. 按 [下一步]****。 按 [下一步]****。  按 [下一步]****。
5.  在"**準備配置"** 螢幕上，確保選擇**配置完成後啟動同步過程**框。
6.  按一下 [設定]****。

有關 Azure AD 連接同步計畫程式的詳細資訊，請參閱[Azure AD 連接計畫程式](how-to-connect-sync-feature-scheduler.md)。

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Azure AD Connect 中可用的其他工作
在初始 Azure AD Connect 的安裝之後，您隨時可以從 Azure AD Connect 開始頁面或桌面捷徑啟動精靈。  您會發現將精靈整個再執行一遍時，會以其他工作的形式提供一些新的選項。  

下表提供這些工作的摘要及個別工作的簡短描述。

![其他工作清單](./media/how-to-connect-post-installation/addtasks2.png)

| 其他工作 | 描述 |
| --- | --- |
|**隱私權設定**|查看正在與 Microsoft 共用的遙測資料。|
|**檢視目前的組態**|檢視目前的 Azure AD Connect 解決方案。  這包括一般設定、同步處理的目錄，以及同步處理設定。 |
| **自訂同步處理選項** |變更目前的組態，像是將其他 Active Directory 樹系新增到組態中或啟用同步處理選項 (例如使用者、群組、裝置或密碼回寫)。 |
|**設定裝置選項**|可用於同步的設備選項|
|**重新整理目錄結構描述**|允許您為同步添加新的本地目錄物件|
|**配置暫存模式** |預備不會立即同步處理及匯出到 Azure AD 或內部部署 Active Directory 的資訊。  此功能可讓您在進行同步處理之前，先進行預覽。 |
|**更改使用者登錄**|更改使用者用於登錄的身份驗證方法|
|**管理聯合**|管理 AD FS 基礎結構、續訂證書和添加 AD FS 伺服器|
|**疑難排解**|説明解決 Azure AD 連接問題|

## <a name="next-steps"></a>後續步驟
詳細瞭解[將本地標識與 Azure 活動目錄集成](whatis-hybrid-identity.md)。
