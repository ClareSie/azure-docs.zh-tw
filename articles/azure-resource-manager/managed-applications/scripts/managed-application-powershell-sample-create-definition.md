---
title: 建立受控應用程式定義 - Azure PowerShell
description: 提供 Azure PowerShell 指令碼範例，以在 Azure 訂用帳戶中建立受控應用程式定義。
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: b605d63db09b81bfe73eca8f4abd7a38d2325004
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86056016"
---
# <a name="create-a-managed-application-definition-with-powershell"></a>使用 PowerShell 建立受控應用程式定義

此指令碼會將受控應用程式定義發佈到服務類別目錄。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>範例指令碼

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/create-definition/create-definition.ps1 "Create definition")]


## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令來建立受控應用程式定義。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [New-AzManagedApplicationDefinition](/powershell/module/az.resources/new-azmanagedapplicationdefinition) | 建立受控應用程式定義。 提供內含所需檔案的套件。 |


## <a name="next-steps"></a>後續步驟

* 如需受控應用程式的簡介，請參閱 [Azure 受控應用程式概觀](../overview.md)。
* 如需 PowerShell 的詳細資訊，請參閱 [Azure PowerShell 文件](/powershell/azure/get-started-azureps)。
