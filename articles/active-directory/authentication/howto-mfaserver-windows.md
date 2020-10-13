---
title: Windows 驗證和 Azure MFA 伺服器 Azure Active Directory
description: 部署 Windows 驗證與 Azure Multi-Factor Authentication Server。
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b945b984210dc3fc3338da8a5670a7813198e40
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "88919466"
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows 驗證與 Azure Multi-Factor Authentication Server

使用 Azure Multi-Factor Authentication Server 的 [Windows 驗證] 區段來啟用及設定應用程式的 Windows 驗證。 在設定 Windows 驗證之前，請注意下列清單︰

* 設定之後，重新啟動 Azure Multi-Factor Authentication，「終端機服務」才會生效。
* 如果核取 [需要 Azure Multi-Factor Authentication 使用者比對]，但您不在使用者清單中，您將無法在重新開機後登入電腦。
* 信任的 IP 取決於應用程式是否可以提供用於驗證的用戶端 IP。 目前僅支援終端機服務。  

> [!IMPORTANT]
> 從2019年7月1日起，Microsoft 不再為新的部署提供 MFA Server。 想要在登入事件期間 (MFA) 要求多重要素驗證的新客戶應該使用雲端式 Azure Multi-Factor Authentication。
>
> 若要開始使用雲端式 MFA，請參閱 [教學課程：使用 Azure 保護使用者登入事件 Multi-Factor Authentication](tutorial-enable-azure-mfa.md)。
>
> 在2019年7月1日前啟用 MFA Server 的現有客戶，可以下載最新版本、未來的更新，並照常產生啟用認證。

> [!NOTE]
> 在 Windows Server 2012 R2 上，不支援這項功能來保護終端機服務。

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>若要使用 Windows 驗證來保護應用程式，請使用下列程式

1. 在 Azure Multi-Factor Authentication Server 中，按一下 [Windows 驗證] 圖示。
   ![MFA Server 中的 Windows 驗證](./media/howto-mfaserver-windows/windowsauth.png)
2. 核取 [啟用 Windows 驗證]**** 核取方塊。 預設不核取此方塊。
3. [應用程式] 索引標籤可讓系統管理員設定一或多個應用程式要經過 Windows 驗證。
4. 選取伺服器或應用程式 – 指定是否啟用伺服器/應用程式。 按一下 [確定]  。
5. 按一下 [**新增 ...** ]
6. [信任的 IP] 索引標籤可讓您針對來自特定 IP 的 Windows 工作階段，略過 Azure Multi-Factor Authentication。 例如，如果員工從辦公室和家裡使用應用程式，您可能決定當他們在辦公室時不要響起 Azure Multi-Factor Authentication 的電話。 為此，您可以將辦公室子網路指定為信任的 IP 項目。
7. 按一下 [**新增 ...** ]
8. 如果您想要略過單一 IP 位址，請選取 [ **單一 ip** ]。
9. 如果您想要跳過整個 IP 範圍，請選取 [ **Ip 範圍** ]。 範例：10.63.193.1-10.63.193.100。
10. 如果您想要使用子網標記法指定 Ip 範圍，請選取 [ **子網** ]。 輸入子網路的起始 IP，並從下拉式清單中挑選適當的網路遮罩。
11. 按一下 [確定]  。

## <a name="next-steps"></a>接下來的步驟

- [設定 Azure MFA Server 的協力廠商 VPN 應用裝置](howto-mfaserver-nps-vpn.md)

- [利用 Azure MFA 的 NPS 擴充功能強化現有的驗證基礎結構驗證](howto-mfa-nps-extension.md)
