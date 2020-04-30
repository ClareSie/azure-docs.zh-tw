---
title: 管理 Azure 自動化中的變數
description: 變數資產是可用於 Azure 自動化中所有 Runbook 和 DSC 設定的值。  這篇文章說明變數的詳細資料，以及如何以文字式和圖形化編寫形式加以使用。
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4778e9b2c0d3b442b214966ab69810d2f42b70b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732753"
---
# <a name="manage-variables-in-azure-automation"></a>管理 Azure 自動化中的變數

變數資產是可用於自動化帳戶中所有 runbook 和 DSC 設定的值。 您可以從 Azure 入口網站、從 PowerShell、在 runbook 內，或在 DSC 設定中管理它們。

自動化變數對下列案例很實用：

- 在多個 runbook 或 DSC 設定之間共用值。

- 從相同 runbook 或 DSC 設定中的多個工作之間共用值。

- 從入口網站或從 PowerShell 命令列管理 runbook 或 DSC 設定所使用的值。 例如，一組一般設定專案，例如特定的 VM 名稱清單、特定的資源群組、AD 功能變數名稱等。  

Azure 自動化保存變數，並使其可供使用，即使 runbook 或 DSC 設定失敗也是一樣。 此行為可讓一個 runbook 或 DSC 設定設定另一個 runbook 所使用的值，或下一次執行時的相同 runbook 或 DSC 設定。

Azure 自動化會安全地儲存每個加密的變數。 建立變數時，您可以 Azure 自動化做為安全資產來指定其加密和儲存體。 其他安全的資產包括認證、憑證和連線。 Azure 自動化會將這些資產加密，並使用為每個自動化帳戶產生的唯一金鑰加以儲存。 金鑰會儲存在系統管理的 Key Vault 中。 儲存安全資產之前，Azure 自動化會從 Key Vault 載入金鑰，然後使用它來加密資產。 

>[!NOTE]
>本文已更新為使用新的 Azure PowerShell Az 模組。 AzureRM 模組在至少 2020 年 12 月之前都還會持續收到錯誤 (Bug) 修正，因此您仍然可以持續使用。 若要深入了解新的 Az 模組和 AzureRM 的相容性，請參閱[新的 Azure PowerShell Az 模組簡介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 如需有關混合式 Runbook 背景工作角色的 Az 模組安裝指示，請參閱[安裝 Azure PowerShell 模組](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。 針對您的自動化帳戶，您可以使用[如何更新 Azure 自動化中的 Azure PowerShell 模組](../automation-update-azure-modules.md)，將模組更新為最新版本。

## <a name="variable-types"></a>變數型別

當您使用 Azure 入口網站建立變數時，您必須從下拉式清單中指定資料類型，讓入口網站可以顯示適當的控制項來輸入變數值。 以下是 Azure 自動化中可用的變數類型：

* 字串
* 整數
* Datetime
* 布林值
* Null

變數不限於指定的資料類型。 如果您想要指定不同類型的值，則必須使用 Windows PowerShell 來設定變數。 如果您指出`Not defined`，變數的值會設定為 Null。 您必須使用[AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) Cmdlet 或`Set-AutomationVariable`活動來設定此值。

您無法使用 Azure 入口網站來建立或變更複雜變數類型的值。 不過，您可以使用 Windows PowerShell 提供任何類型的值。 複雜類型會以[PSCustomObject](/dotnet/api/system.management.automation.pscustomobject)的形式抓取。

您可以藉由建立陣列或雜湊表，並將它儲存到變數中，來儲存多個值到單一變數。

>[!NOTE]
>VM 名稱變數最多可以有80個字元。 資源群組變數最多可以有90個字元。 請參閱[Azure 資源的命名規則和限制](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-name-rules)。

## <a name="powershell-cmdlets-that-create-and-manage-variable-assets"></a>建立和管理變數資產的 PowerShell Cmdlet

針對 Az 模組，下表中的 Cmdlet 是用來透過 Windows PowerShell 建立和管理自動化變數資產。 它們會隨附于[Az. automation 模組](/powershell/azure/overview)中，可在自動化 RUNBOOK 和 DSC 設定中使用。

| Cmdlet | 描述 |
|:---|:---|
|[AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | 擷取現有變數的值。 您無法使用這個 Cmdlet 來抓取加密變數的值。 若要這麼做，唯一的方法是在`Get-AutomationVariable` RUNBOOK 或 DSC 設定中使用活動。 |
|[新增-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | 建立新的變數並設定其值。|
|[移除-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| 移除現有的變數。|
|[設定-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| 設定現有的變數的值。 |

## <a name="activities-to-access-variables-in-runbooks-and-dsc-configurations"></a>在 runbook 和 DSC 設定中存取變數的活動

下表中的活動是用來存取 runbook 和 DSC 設定中的變數。 這些活動的 Cmdlet 隨附于全域模組`Orchestrator.AssetManagement.Cmdlets`。

| 活動 | 描述 |
|:---|:---|
|`Get-AutomationVariable`|擷取現有變數的值。|
|`Set-AutomationVariable`|設定現有的變數的值。|

> [!NOTE]
> 避免`Get-AutomationVariable`在 RUNBOOK 或 DSC `Name`設定的參數中使用變數。 使用此參數可能會在設計階段將 runbook 或 DSC 設定與自動化變數之間的相依性探索複雜化。

`Get-AutomationVariable`請注意，無法在 PowerShell 中運作，但只能在 RUNBOOK 或 DSC 設定中使用。 例如，若要查看加密變數的值，您可以建立 runbook 來取得該變數，然後將它寫入輸出資料流程：
 
```powershell
$mytestencryptvar = Get-AutomationVariable -Name TestVariable
Write-output "The encrypted value of the variable is: $mytestencryptvar"
```

## <a name="functions-to-access-variables-in-python-2-runbooks"></a>在 Python 2 runbook 中存取變數的函式

下表中的函數可用來存取 Python 2 runbook 中的變數。

|Python 2 函式|描述|
|:---|:---|
|`automationassets.get_automation_variable`|擷取現有變數的值。 |
|`automationassets.set_automation_variable`|設定現有的變數的值。 |

> [!NOTE]
> 您必須在 Python `automationassets` runbook 的頂端匯入模組，才能存取資產功能。

## <a name="working-with-automation-variables"></a>使用 Automation 變數

>[!NOTE]
>如果您想要移除變數的加密，您必須刪除變數，並將它重新建立為未加密。

### <a name="create-a-new-variable-using-the-azure-portal"></a>使用 Azure 入口網站建立新的變數

1. 從您的自動化帳戶，依序按一下 [**資產**] 圖格和 [**資產**] 分頁，然後選取 [**變數**]。
2. 在 [變數]**** 圖格上，選取 [新增變數]****。
3. 完成 [**新增變數**] 分頁上的選項，然後按一下 [**建立**] 以儲存新的變數。

> [!NOTE]
> 儲存加密的變數之後，就無法在入口網站中看到它。 只能更新。

### <a name="create-and-use-a-variable-in-windows-powershell"></a>在 Windows PowerShell 中建立和使用變數

PowerShell 腳本會使用`New-AzAutomationVariable` Cmdlet 或其對等的 AzureRM 模組，建立新的變數並設定其初始值。 如果變數已加密，則呼叫應使用`Encrypted`參數。

腳本會使用[AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0)來抓取變數的值。 如果值是簡單類型，Cmdlet 會抓取相同的類型。 如果它是複雜型別，則`PSCustomObject`會抓取型別。

>[!NOTE]
>PowerShell 腳本無法取得已加密的值。 若要這麼做，唯一的方法是在`Get-AutomationVariable` RUNBOOK 或 DSC 設定中使用活動。

下列範例顯示如何建立 String 類型的變數，然後傳回其值。

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

下列範例示範如何建立具有複雜類型的變數，然後取得其屬性。 在此情況下，會使用來自[update-azvm](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0)的虛擬機器物件。

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

### <a name="create-and-use-a-variable-in-a-runbook-or-dsc-configuration"></a>在 runbook 或 DSC 設定中建立和使用變數

從 runbook 或 DSC 設定中建立新變數的唯一方式，就是使用`New-AzAutomationVariable` Cmdlet 或其對等的 AzureRM 模組。 此腳本會使用此 Cmdlet 來設定變數的初始值。 接著，腳本就可以使用`Get-AzAutomationVariable`來抓取該值。 如果值是簡單類型，則會抓取相同的類型。 如果它是複雜型別，則會`PSCustomObject`抓取型別。

>[!NOTE]
>取得加密值的唯一方法是使用 runbook 或 DSC 設定`Get-AutomationVariable`中的活動。 

### <a name="textual-runbook-samples"></a>文字式 Runbook 範例

#### <a name="set-and-retrieve-a-simple-value-from-a-variable"></a>從變數設定和取出簡單的值

下列範例命令顯示如何設定及擷取文字式 Runbook 中的變數。 這個範例假設建立名為`NumberOfIterations`和`NumberOfRunnings`的整數變數，以及名為`SampleMessage`的字串變數。

```powershell
$NumberOfIterations = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)
```

#### <a name="set-and-retrieve-a-variable-in-a-python-2-runbook"></a>在 Python 2 runbook 中設定和取出變數

下列範例示範如何使用變數、設定變數，以及處理 Python 2 runbook 中不存在之變數的例外狀況。

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("nonexisting variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>圖形化 Runbook 範例

在圖形化 runbook 中，您可以加入`Get-AutomationVariable`或`Set-AutomationVariable`活動。 只要以滑鼠右鍵按一下圖形化編輯器 [程式庫] 窗格中的變數，然後選取您想要的活動即可。

![加入變數至畫布](../media/variables/runbook-variable-add-canvas.png)

#### <a name="set-values-in-a-variable"></a>設定變數中的值

下圖顯示的範例活動會在圖形化 Runbook 中使用簡單值更新變數。 在此範例中`Get-AzVM` ，會抓取單一 Azure 虛擬機器，並將電腦名稱稱儲存至現有的自動化字串變數。 [連結是否為管線或序列，是因為程式](../automation-graphical-authoring-intro.md#links-and-workflow)代碼在輸出中只預期單一物件。

![設定簡單變數](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>後續步驟

- 若要深入瞭解如何在圖形化編寫中連接活動，請參閱[圖形化編寫中的連結](../automation-graphical-authoring-intro.md#links-and-workflow)。
- 若要開始使用圖形化 runbook，請參閱[我的第一個圖形化 runbook](../automation-first-runbook-graphical.md)。
