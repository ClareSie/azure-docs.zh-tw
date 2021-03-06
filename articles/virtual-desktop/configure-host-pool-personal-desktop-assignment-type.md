---
title: Windows 虛擬桌面個人桌面指派類型 - Azure
description: 如何設定 Windows 虛擬桌面個人桌面主機集區的自動或直接指派。
author: Heidilohr
ms.topic: how-to
ms.date: 07/09/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 446dae3c064f5f23d35cb12b2b24bdfea9e27012
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "88007806"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>設定個人桌面主機集區指派類型

>[!IMPORTANT]
>此內容適用於具有 Azure Resource Manager Windows 虛擬桌面物件的 Windows 虛擬桌面。 如果您使用不含 Azure Resource Manager 物件的 Windows 虛擬桌面 (傳統版)，請參閱[這篇文章](./virtual-desktop-fall-2019/configure-host-pool-personal-desktop-assignment-type-2019.md)。

您可以設定個人桌面主機集區的指派類型來調整您的 Windows 虛擬桌面環境，使其更符合您的需求。 在本主題中，我們將示範如何為您的使用者設定自動或直接指派。

>[!NOTE]
> 本文中的指示僅適用於個人桌面主機集區，而非集區式主機集區，因為集區式主機集區中的使用者不會指派給特定的工作階段主機。

## <a name="prerequisites"></a>必要條件

本文假設您已下載並安裝 Windows 虛擬桌面 PowerShell 模組。 如果您尚未這麼做，請依照 [設定 PowerShell 模組](powershell-module.md)中的指示進行。

## <a name="configure-automatic-assignment"></a>設定自動指派

自動指派是在 Windows 虛擬桌面環境中建立的新個人桌面主機集區，其中的預設指派類型。 自動指派使用者不需要特定的工作階段主機。

若要自動指派使用者，請先將其指派給個人桌面主機集區，使其可以在自己的摘要中看見桌面。 當指派的使用者在自己的摘要中啟動桌面時，如果他們尚未連線到完成指派程式的主機集區，則將宣告可用的工作階段主機。

若要將主機集區設定為自動將使用者指派給 VM，請執行下列 PowerShell Cmdlet：

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -PersonalDesktopAssignmentType Automatic
```

若要將使用者指派給個人桌面主機集區，請執行下列 PowerShell Cmdlet：

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

## <a name="configure-direct-assignment"></a>設定直接指派

不同於自動指派，當您使用直接指派時，必須將使用者指派給個人桌面主機集區與特定的工作階段主機，才能連線到他們的個人桌面。 如果使用者只指派給沒有工作階段主機指派的主機集區，則使用者將無法存取資源。

若要設定主機集區來要求將使用者直接指派給工作階段主機，請執行下列 PowerShell Cmdlet：

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -PersonalDesktopAssignmentType Direct
```

若要將使用者指派給個人桌面主機集區，請執行下列 PowerShell Cmdlet：

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

若要將使用者指派給特定的工作階段主機，請執行下列 PowerShell Cmdlet：

```powershell
Update-AzWvdSessionHost -HostPoolName <hostpoolname> -Name <sessionhostname> -ResourceGroupName <resourcegroupname> -AssignedUser <userupn>
```

若要直接將使用者指派給 Azure 入口網站中的工作階段主機：

1. 在 <https://portal.azure.com> 登入 Azure 入口網站。
2. 在搜尋列中輸入 **Windows 虛擬桌面** 。
3. 在 [ **服務**] 下，選取 [ **Windows 虛擬桌面**]。
4. 在 [Windows 虛擬桌面] 頁面上，移至視窗左側的功能表，然後選取 [ **主機**集區]。
5. 選取您要更新之主機集區的名稱。
6. 接下來，移至視窗左側的功能表，然後選取 [ **應用程式群組**]。
7. 選取您要編輯的桌面應用程式組名，然後選取視窗左側功能表中的 [ **指派** ]。
8. 選取 [ **+ 新增**]，然後選取要發佈此桌面應用程式群組的使用者或使用者群組。
9. 選取資訊列中的 [ **指派 VM** ]，將工作階段主機指派給使用者。
10. 選取您要指派給使用者的工作階段主機，然後選取 [ **指派**]。
11. 從可用的使用者清單中選取您想要指派工作階段主機的使用者。
12. 當您完成時，請選取 [ **選取**]。

## <a name="next-steps"></a>後續步驟

現在您已完成設定個人桌面指派類型，您可以登入 Windows 虛擬桌面用戶端，在使用者工作階段中進行測試。 接下來的兩個操作說明將會說明如何使用您所選擇的用戶端，連線至工作階段：

- [與 Windows 桌面用戶端連線](connect-windows-7-10.md)
- [與 Web 用戶端連線](connect-web.md)
- [與 Android 用戶端連線](connect-android.md)
- [與 iOS 用戶端連線](connect-ios.md)
- [與 macOS 用戶端連線](connect-macos.md)