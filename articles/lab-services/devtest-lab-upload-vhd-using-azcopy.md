---
title: 使用 AzCopy 將 VHD 檔案上傳到 Azure DevTest Labs | Microsoft Docs
description: 本文提供了一個演練，用於使用 AzCopy 命令列實用程式將 VHD 檔上載到 Azure DevTest 實驗室中的實驗室存儲帳戶。
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 49dc70788bf2a44b6925c5f3f8226fdadab8768c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "76757417"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a>使用 AzCopy 將 VHD 檔案上傳到實驗室的儲存體帳戶

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

在 Azure DevTest Labs 中，可以使用 VHD 檔案來建立自訂映像，這些映像可用來佈建虛擬機器。 下列步驟將逐步引導您使用 AzCopy 命令列公用程式，將 VHD 檔案上傳到實驗室的儲存體帳戶。 在您上傳 VHD 檔案之後，[後續步驟](#next-steps)一節會列出一些說明如何從所上傳的 VHD 檔案建立自訂映像的文章。 如需 Azure 中磁碟和 VHD 的詳細資訊，請參閱[受控磁碟簡介](../virtual-machines/linux/managed-disks-overview.md)

> [!NOTE] 
>  
> AzCopy 是一個僅適用於 Windows 的命令列公用程式。

## <a name="step-by-step-instructions"></a>逐步指示

下列步驟將逐步引導您使用 [AzCopy](https://aka.ms/downloadazcopy) 將 VHD 檔案上傳到 Azure DevTest Labs。 

1. 使用 Azure 入口網站來取得實驗室的儲存體帳戶名稱：

1. 登錄到 Azure[門戶](https://go.microsoft.com/fwlink/p/?LinkID=525040)。

1. 選擇**所有服務**，然後從清單中選擇**DevTest 實驗室**。

1. 從實驗室清單中，選取所需的實驗室。  

1. 在實驗室的刀鋒視窗上，選取 [組態] ****。 

1. 在實驗室的 [組態]**** 刀鋒視窗上，選取 [自訂映像 (VHD)]****。

1. 在 **"自訂圖像**"邊欄選項卡上，選擇 **"添加**"。 

1. 在 [自訂映像]**** 刀鋒視窗上，選取 [VHD]****。

1. 在 [VHD]**** 刀鋒視窗上，選取 [使用 PowerShell 上傳 VHD]****。

    ![使用 PowerShell 上傳 VHD](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. [使用 PowerShell 上傳映像]**** 刀鋒視窗會顯示一個對 **Add-AzureVhd** Cmdlet 的呼叫。 第一個參數 (*Destination*) 包含 Blob 容器 (*uploads*) 的 URI，其格式如下：

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. 記下完整的 URI，因為在稍後的步驟中將會用到。

1. 使用 AzCopy 來上傳 VHD 檔案：
 
1. [下載並安裝最新版的 AzCopy](https://aka.ms/downloadazcopy)。

1. 開啟命令視窗，然後瀏覽至 AzCopy 安裝目錄。 您可以視需要將 AzCopy 安裝位置新增到您的系統路徑。 AzCopy 預設會安裝到下列目錄：

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. 使用儲存體帳戶金鑰和 Blob 容器 URI，在命令提示字元中執行下列命令。 *vhdFileName* 值必須包含在引號中。 視 VHD 檔案大小及您的連線速度而定，上傳 VHD 檔案的程序可能會相當長。   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>後續步驟

- [使用 Azure 入口網站在 Azure DevTest Labs 中從 VHD 檔案建立自訂映像](devtest-lab-create-template.md)
- [使用 PowerShell 在 Azure DevTest Labs 中從 VHD 檔案建立自訂映像](devtest-lab-create-custom-image-from-vhd-using-powershell.md)