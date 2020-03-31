---
title: Azure 監視器中的線資料解決方案 |微軟文檔
description: 連線資料是來自內有 Log Analytics 代理程式電腦的網路和效能的彙總資料。 網路資料結合記錄資料可協助您將資料相互關聯。
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/03/2018
ms.openlocfilehash: ee7a2f49641eb0cfe1f8a4bffb44c7f8642408fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670639"
---
# <a name="wire-data-20-preview-solution-in-azure-monitor"></a>Azure 監視器中的線資料 2.0（預覽）解決方案

![Wire Data 符號](media/wire-data/wire-data2-symbol.png)

連線資料是彙總的網路和效能資料，使用 Log Analytics 代理程式從 Windows 連線和 Linux 連線的電腦收集，包括您的環境中由 Operations Manager 監視的資料。 網路資料結合其他記錄資料可協助您將資料相互關聯。

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

除了 Log Analytics 代理程式，Wire Data 解決方案還會使用您在 IT 基礎結構的電腦上所安裝的 Microsoft 相依性代理程式。 相依性代理程式會監視往返於您電腦傳送的網路資料 (屬於 [OSI 模型](https://en.wikipedia.org/wiki/OSI_model)中的網路層級 2-3)，包括使用的各種通訊協定和連接埠。 然後，使用代理將資料發送到 Azure 監視器。  

>[!NOTE]
>如果您已部署服務映射，或者正在考慮為 VM 部署服務映射或[Azure 監視器](../../azure-monitor/insights/vminsights-overview.md)，則在 Azure 監視器中收集和存儲新的連接指標資料集，該資料集提供與有線資料類似的資訊。

預設情況下，Azure 監視器從 Windows 和 Linux 中內置的計數器以及您可以指定的其他效能計數器中記錄 CPU、記憶體、磁片和網路效能資料的資料。 針對每個代理程式，都是即時收集網路和其他資料，包括電腦使用的子網路和應用程式層級通訊協定。  Wire Data 會查看應用程式層級的網路資料，而不會往下查看 TCP 傳輸層。 解決方案不會查看個別的 ACK 和 SYN。 交握完成後，即會將它視為即時連接，並且標示為已連接。 只要兩端同意通訊端為開啟，該連接會維持為即時，且可以來回傳遞資料。 一旦任一方關閉連接，它被標記為"已中斷連線"。  因此，它只會計算已成功完成封包的頻寬，而不會回報重新傳送或失敗的封包。

如果您曾經使用 [sFlow](http://www.sflow.org/) 或其他軟體來搭配 [Cisco NetFlow 通訊協定](https://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html)，則會很熟悉從連線資料傳回的統計資料和資料。

幾種內建的記錄搜尋查詢包括︰

- 提供連線資料的代理程式
- 提供連線資料的代理程式的 IP 位址
- 依 IP 位址的輸出通訊
- 應用程式通訊協定傳送的位元組數目
- 應用程式服務傳送的位元組數目
- 不同通訊協定接收的位元組數目
- IP 版本傳送及接收的位元組總數
- 可靠測量的連線平均延遲
- 起始或接收網路流量的電腦處理程序
- 處理程序的網路流量

當您使用連線資料來搜尋時，您可以篩選和分組資料，以檢視最常用的前幾個代理程式和通訊協定的相關資訊。 或者，您可以檢視某些電腦 (IP 位址/MAC 位址) 何時彼此通訊、持續時間，以及已傳送的資料量；基本上，就是在檢視以搜尋為基礎之網路流量的相關中繼資料。

不過，因為是檢視中繼資料，在深入的疑難排解中不見得實用。 Azure 監視器中的線資料不是網路資料的完整捕獲。  不適用於封包層級的深入疑難排解。 與其他收集方法相比，使用代理的優點是，您不必安裝設備、重新配置網路交換器或執行複雜的配置。 連線資料是以代理程式為基礎—在電腦上安裝代理程式，它就會監視自己的網路流量。 另一個優點是您想要監視雲端提供者、主機服務提供者或 Microsoft Azure 中執行的工作負載，而其中使用者未擁有網狀架構層級。

## <a name="connected-sources"></a>連接的來源

Wire Data 會從 Microsoft 相依性代理程式取得其資料。 依賴項代理依賴于日誌分析代理連接到 Azure 監視器。 這表示，伺服器必須先安裝 Log Analytics 代理程式，並設定 Dependency Agent。 下表描述 Wire Data 解決方案支援的連線來源。

| **連接源** | **支援** | **描述** |
| --- | --- | --- |
| Windows 代理程式 | 是 | Wire Data 會分析並收集來自 Windows 代理程式電腦的資料。 <br><br> 除了 Windows 的[日誌分析代理](../platform/agent-windows.md)外，Windows 代理還需要 Microsoft 依賴項代理。 有關作業系統版本的完整清單，請參閱[支援的作業系統](vminsights-enable-overview.md#supported-operating-systems)。 |
| Linux 代理程式 | 是 | Wire Data 會分析並收集來自 Linux 代理程式電腦的資料。<br><br> 除了 Linux 的[日誌分析代理](../learn/quick-collect-linux-computer.md)外，Linux 代理還需要微軟依賴項代理。 有關作業系統版本的完整清單，請參閱[支援的作業系統](vminsights-enable-overview.md#supported-operating-systems)。 |
| System Center Operations Manager 管理群組 | 是 | Wire Data 會在連線的 [System Center Operations Manager 管理群組](../platform/om-agents.md)中，分析並收集來自 Windows 和 Linux 代理程式的資料。 <br><br> 需要從系統中心操作管理器代理電腦到 Azure 監視器的直接連接。 |
| Azure 儲存體帳戶 | 否 | Wire Data 會收集來自代理程式電腦的資料，因此沒有要從 Azure 儲存體收集的資料。 |

在 Windows 上，系統中心操作管理器和 Azure 監視器都使用 Microsoft 監視代理 （MMA） 來收集和發送資料。 視內容而定，此代理程式可稱為 System Center Operations Manager 代理程式、Log Analytics 代理程式、MMA 或直接代理程式。 系統中心操作管理器和 Azure 監視器提供略有不同的 MMA 版本。 這些版本可以每個版本向系統中心操作管理器、Azure 監視器或兩者報告。

在 Linux 上，Linux 的日誌分析代理收集資料並將其發送到 Azure 監視器。 可以在具有直接連接到 Azure 監視器的代理的伺服器上使用有線資料，也可以在通過系統中心操作管理組連接到 Azure 監視器的伺服器上使用有線資料。

依賴項代理本身不傳輸任何資料，並且不需要對防火牆或埠進行任何更改。 線資料中的資料始終由日誌分析代理直接或通過日誌分析閘道傳輸到 Azure 監視器。

![代理程式圖表](./media/wire-data/agents.png)

如果您是系統中心操作管理器使用者，管理組連接到 Azure 監視器：

- 當系統中心操作管理器代理可以訪問 Internet 以連接到 Azure 監視器時，無需其他配置。
- 當系統中心操作管理器代理無法通過 Internet 訪問 Azure 監視器時，您需要配置日誌分析閘道以使用系統中心操作管理器。

如果 Windows 或 Linux 電腦無法直接連接到服務，則需要配置日誌分析代理以使用日誌分析閘道連接到 Azure 監視器。 您可以從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=52666)下載 Log Analytics 閘道。

## <a name="prerequisites"></a>Prerequisites

- 需要[洞察力與分析](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing)解決方案供應項目。
- 如果您使用舊版 Wire Data 解決方案，則必須先將它移除。 不過，您仍然可以在 Wire Data 2.0 和記錄搜尋中使用透過原始 Wire Data 解決方案擷取的所有資料。
- 安裝或卸載依賴項代理需要管理員許可權。
- 依賴項代理必須安裝在具有 64 位作業系統的電腦上。

### <a name="operating-systems"></a>作業系統

以下各節列出了依賴項代理的支援作業系統。 Wire Data 不支援任何 32 位元架構的作業系統。

#### <a name="windows-server"></a>Windows Server

- Windows Server 2019
- Windows Server 2016 1803
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Windows 桌面

- Windows 10 1803
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="supported-linux-operating-systems"></a>支援的 Linux 作業系統
以下各節列出了 Linux 上依賴項代理的支援作業系統。  

- 只支援預設版本和 SMP Linux 核心版本。
- 所有 Linux 散發套件皆不支援非標準的核心版本 (例如 PAE 和 Xen)。 舉例來說，版本字串為「2.6.16.21-0.8-xen」的系統就不受支援。
- 不支援自訂核心，包括重新編譯的標準核心。

##### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| 作業系統版本 | 核心版本 |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

##### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| 作業系統版本 | 核心版本 |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

##### <a name="centosplus"></a>CentOSPlus
| 作業系統版本 | 核心版本 |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

##### <a name="ubuntu-server"></a>Ubuntu Server

| 作業系統版本 | 核心版本 |
|:--|:--|
| Ubuntu 18.04 | 內核 4.15.\*<br>4.18* |
| Ubuntu 16.04.3 | 核心 4.15.* |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

##### <a name="suse-linux-11-enterprise-server"></a>SUSE Linux 11 Enterprise Server

| 作業系統版本 | 核心版本
|:--|:--|
| 11 SP4 | 3.0.* |

##### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| 作業系統版本 | 核心版本
|:--|:--|
| 12 SP2 | 4.4.* |
| 12 SP3 | 4.4.* |

### <a name="dependency-agent-downloads"></a>Dependency Agent 下載

| 檔案 | OS | 版本 | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |



## <a name="configuration"></a>組態

執行下列步驟來設定您工作區的 Wire Data 解決方案。

1. 從[Azure 市場](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview)啟用活動日誌分析解決方案，或使用[解決方案庫中的添加監視解決方案](../../azure-monitor/insights/solutions.md)中描述的過程。
2. 在要獲取資料的每台電腦上安裝依賴項代理。 依賴項代理可以監視與近鄰的連接，因此您可能不需要每台電腦上的代理。

> [!NOTE]
> 您無法將舊版 Wire Data 解決方案新增至新的工作區。 如果您已啟用原始 Wire Data 解決方案，您可以繼續使用。 不過，若要使用 Wire Data 2.0，您必須先移除原始版本。
> 
 
### <a name="install-the-dependency-agent-on-windows"></a>在 Windows 上安裝 Dependency Agent

必須有系統管理員權限，以便安裝或解除安裝代理程式。

依賴項代理安裝在通過安裝依賴項代理-Windows.exe 運行 Windows 的電腦上。 如果您在執行此可執行檔時不使用任何選項，則會啟動以互動方式安裝的精靈。

使用以下步驟在運行 Windows 的每台電腦上安裝依賴項代理：

1. 依照[為混合式環境中的 Windows 電腦設定 Log Analytics 代理程式](../../azure-monitor/platform/agent-windows.md)一文中的步驟，安裝 Log Analytics 代理程式。
2. 使用上一節中的連結下載 Windows 依賴項代理，然後使用以下命令運行它：`InstallDependencyAgent-Windows.exe`
3. 遵循精靈來安裝代理程式。
4. 如果 Dependency Agent 無法啟動，請檢查記錄以取得詳細的錯誤資訊。 針對 Windows 代理程式，記錄的目錄是 %Programfiles%\Microsoft Dependency Agent\logs。

#### <a name="windows-command-line"></a>Windows 命令列

使用下表中的選項以從命令列安裝。 若要查看安裝旗標的清單，請如下所示使用 /? 旗標執行安裝程式 如下所示。

InstallDependencyAgent-Windows.exe /?

| **國旗** | **描述** |
| --- | --- |
| <code>/?</code> | 取得命令列選項的清單。 |
| <code>/S</code> | 執行無訊息安裝，不會出現任何使用者提示。 |

預設情況下，Windows 依賴項代理的檔放置在 C：*程式檔_Microsoft 依賴項代理中。

### <a name="install-the-dependency-agent-on-linux"></a>在 Linux 上安裝 Dependency Agent

必須有 root 權限，以便安裝或設定代理程式。

依賴項代理通過安裝依賴代理-Linux64.bin 安裝在 Linux 電腦上，這是一個具有自提取二進位檔案的 shell 腳本。 您可以使用 _sh_ 來執行檔案，或對檔案本身新增執行權限。

請使用下列步驟在每一部 Linux 電腦上安裝 Dependency Agent：

1. 依照[為混合式環境中的 Linux 電腦設定 Log Analytics 代理程式](../../azure-monitor/learn/quick-collect-linux-computer.md#obtain-workspace-id-and-key)一文中的步驟，安裝 Log Analytics 代理程式。
2. 使用上一節中的連結來下載 Linux 相依性代理程式，然後使用下列命令將它安裝為根目錄：sh InstallDependencyAgent-Linux64.bin
3. 如果 Dependency Agent 無法啟動，請檢查記錄以取得詳細的錯誤資訊。 在 Linux 代理程式上，記錄檔的目錄是 /var/opt/microsoft/dependency-agent/log。

若要查看安裝旗標的清單，請如下所示以 `-help` 旗標執行安裝程式。

```
InstallDependencyAgent-Linux64.bin -help
```

| **國旗** | **描述** |
| --- | --- |
| <code>-help</code> | 取得命令列選項的清單。 |
| <code>-s</code> | 執行無訊息安裝，不會出現任何使用者提示。 |
| <code>--check</code> | 檢查權限和作業系統，但不會安裝代理程式。 |

Dependency Agent 的檔案位於下列目錄：

| **檔** | **位置** |
| --- | --- |
| 核心檔案 | /opt/microsoft/dependency-agent |
| 記錄檔 | /var/opt/microsoft/dependency-agent/log |
| 組態檔 | /etc/opt/microsoft/dependency-agent/config |
| 服務可執行檔 | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br><br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| 二進位儲存體檔案 | /var/opt/microsoft/dependency-agent/storage |

### <a name="installation-script-examples"></a>安裝指令碼範例

要一次輕鬆地將依賴項代理部署到許多伺服器上，使用腳本很有説明。 您可以使用以下腳本示例在 Windows 或 Linux 上下載和安裝依賴項代理。

#### <a name="powershell-script-for-windows"></a>適用於 Windows 的 PowerShell 指令碼

```powershell

Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>適用於 Linux 的殼層指令碼

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>Desired State Configuration

要通過所需的狀態配置部署依賴項代理，可以使用 xPS 希望狀態配置模組和類似以下內容的代碼位：

```powershell
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"



Node $NodeName

{

    # Download and install the Dependency agent

    xRemoteFile DAPackage

    {

        Uri = "https://aka.ms/dependencyagentwindows"

        DestinationPath = $DAPackageLocalPath

        DependsOn = "[Package]OI"

    }

    xPackage DA

    {

        Ensure = "Present"

        Name = "Dependency Agent"

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = ""

        InstalledCheckRegKey = "HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"

        InstalledCheckRegValueName = "DisplayName"

        InstalledCheckRegValueData = "Dependency Agent"

    }

}

```

### <a name="uninstall-the-dependency-agent"></a>卸載依賴項代理

使用以下部分可説明您刪除依賴項代理。

#### <a name="uninstall-the-dependency-agent-on-windows"></a>在 Windows 上卸載依賴項代理

系統管理員可透過控制台解除安裝 Windows Dependency Agent。

系統管理員也可以執行 %Programfiles%\Microsoft Dependency Agent\Uninstall.exe，以解除安裝 Dependency Agent。

#### <a name="uninstall-the-dependency-agent-on-linux"></a>在 Linux 上卸載依賴項代理

要從 Linux 完全卸載依賴項代理，必須刪除代理本身和連接器，該連接器將自動與代理一起安裝。 您可以使用下列單一命令同時解除安裝這兩者：

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>管理組件

在 Log Analytics 工作區中啟動 Wire Data 時，會將 300 KB 的管理組件傳送至該工作區中的所有 Windows 伺服器。 如果您是在[連線的管理群組](../platform/om-agents.md)中使用 System Center Operations Manager 代理程式，則會從 System Center Operations Manager 部署相依性監視管理組件。 如果代理直接連接，Azure 監視器將提供管理包。

管理組件名稱為 Microsoft.IntelligencePacks.ApplicationDependencyMonitor。 它會寫入至 %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs。 管理組件所使用的資料來源是 %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources&lt;自動產生的識別碼&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll。

## <a name="using-the-solution"></a>使用解決方案

請使用下列資訊來安裝和設定方案。

- 連線資料方案會從執行 Windows Server 2012 R2、Windows 8.1 和更新版本作業系統的電腦取得資料。
- 您想要取得連線資料的來源電腦上需要有 Microsoft.NET Framework 4.0 或更新版本。
- 使用[解決方案庫中的添加監視解決方案](solutions.md)中描述的過程將線資料解決方案添加到日誌分析工作區。 不需要進一步的組態。
- 如果您想要檢視特定解決方案的連線資料，必須先將此解決方案新增至您的工作區。

依序安裝代理程式和解決方案之後，Wire Data 2.0 磚會出現在您的工作區中。

![Wire Data 磚](./media/wire-data/wire-data-tile.png)

## <a name="using-the-wire-data-20-solution"></a>使用 Wire Data 2.0 解決方案

在 Azure 入口網站 Log Analytics 工作區的 [概觀]**** 頁面中，按一下 [Wire Data 2.0]**** 磚以開啟 [Wire Data] 儀表板。 此儀表板包含下表中的刀鋒視窗。 每個刀鋒視窗最多會列出 10 個與該刀鋒視窗中指定範圍和時間範圍的準則相符的項目。 您可以按一下刀鋒視窗底部的 [查看全部]****，或按一下刀鋒視窗標頭，以執行記錄搜尋來傳回所有記錄。

| **刀鋒視窗** | **描述** |
| --- | --- |
| 擷取網路流量的代理程式數 | 顯示擷取網路流量的代理程式數，並列出擷取最多流量的前 10 部電腦。 按一下此數字可執行記錄搜尋，以搜尋 <code>WireData \| summarize sum(TotalBytes) by Computer \| take 500000</code>。 按一下清單中的電腦可執行記錄搜尋，以傳回擷取的位元組總數。 |
| 區域子網路數 | 顯示代理程式探索到的區域子網路數。  按一下此數字可執行記錄搜尋，以搜尋 <code>WireData \| summarize sum(TotalBytes) by LocalSubnet</code>，其中列出所有子網路及透過每個子網路傳送的位元組數目。 按一下清單中的子網路可執行記錄搜尋，以傳回透過子網路傳送的位元組總數。 |
| 應用程式層級通訊協定數 | 顯示代理程式探索到的使用中應用程式層級通訊協定數。 按一下此數字可執行記錄搜尋，以搜尋 <code>WireData \| summarize sum(TotalBytes) by ApplicationProtocol</code>。 按一下通訊協定可執行記錄搜尋，以傳回使用該通訊協定傳送的位元組總數。 |

![Wire Data 儀表板](./media/wire-data/wire-data-dash.png)

您可以使用 [擷取網路流量的代理程式數]**** 刀鋒視窗，來判斷電腦所耗用的網路頻寬量。 此刀鋒視窗可協助您輕鬆找到環境中「通訊量最大」__ 的電腦。 這類電腦可能超載、運作異常，或使用的網路資源不尋常地過多。

![記錄搜尋範例](./media/wire-data/log-search-example01.png)

同樣地，您可以使用 [區域子網路數]**** 刀鋒視窗，來判斷透過子網路移動的網路流量。 使用者通常會為其應用程式定義重要區域周圍的子網路。 此刀鋒視窗會提供這些區域的檢視。

![記錄搜尋範例](./media/wire-data/log-search-example02.png)

[應用程式層級通訊協定數]**** 刀鋒視窗可協助了解使用中的通訊協定，因此很有用。 例如，您可能預期 SSH 不會用於網路環境中。 藉由檢視刀鋒視窗中可用的資訊，即可快速確認或否認您的預期。

![記錄搜尋範例](./media/wire-data/log-search-example03.png)

它也有助於了解通訊協定流量是否隨時間增加或減少。 例如，如果應用程式所傳輸的資料量增加，可能有應該注意或值得關注的問題。

## <a name="input-data"></a>輸入資料

連線資料會使用您已啟用的代理程式，來收集有關網路流量的中繼資料。 每個代理程式約每隔 15 秒傳送一次資料。

## <a name="output-data"></a>輸出資料

對於每種類型的輸入資料，系統會建立 _WireData_ 類型的記錄。 WireData 記錄具有下表所示的屬性：

| 屬性 | 描述 |
|---|---|
| 電腦 | 收集資料所在的電腦名稱 |
| TimeGenerated | 記錄的時間 |
| LocalIP | 本機電腦的 IP 位址 |
| SessionState | 已連線或已中斷連線 |
| ReceivedBytes | 接收的位元組數目 |
| ProtocolName | 使用的網路通訊協定名稱 |
| IPVersion | IP 版本 |
| 方向 | 輸入或輸出 |
| MaliciousIP | 已知惡意來源的 IP 位址 |
| Severity | 可疑惡意程式碼嚴重性 |
| RemoteIPCountry | 遠端 IP 位址的國家/地區 |
| ManagementGroupName | Operations Manager 管理群組的名稱 |
| SourceSystem | 收集資料所在的來源 |
| SessionStartTime | 工作階段的開始時間 |
| SessionEndTime | 工作階段的結束時間 |
| LocalSubnet | 收集資料所在的子網路 |
| LocalPortNumber | 本機連接埠號碼 |
| RemoteIP | 遠端電腦所使用的遠端 IP 位址 |
| RemotePortNumber | 遠端 IP 位址所使用的連接埠號碼 |
| SessionID | 識別兩個 IP 位址之間通訊工作階段的唯一值 |
| SentBytes | 傳送的位元組數目 |
| TotalBytes | 工作階段期間傳送的位元組總數 |
| ApplicationProtocol | 使用的網路通訊協定類型   |
| ProcessID | Windows 處理序識別碼 |
| ProcessName | 處理序的路徑和檔案名稱 |
| RemoteIPLongitude | IP 經度值 |
| RemoteIPLatitude | IP 緯度值 |

## <a name="next-steps"></a>後續步驟

- [搜尋記錄](../../azure-monitor/log-query/log-query-overview.md)以檢視詳細的連線資料搜尋記錄。
