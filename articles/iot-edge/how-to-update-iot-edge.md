---
title: 更新裝置上的 IoT Edge 版本 - Azure IoT Edge | Microsoft Docs
description: 如何更新 IoT Edge 裝置以執行最新版的安全性精靈和 IoT Edge 執行階段
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/22/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ee00425da89391e5228f2d48b49ca85426066f1e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85299002"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>更新 IoT Edge 安全性精靈和執行階段

當 IoT Edge 服務發行新版本時，您會想要更新您的 IoT Edge 裝置，以取得最新的功能和安全性改進。 本文提供在新版本發行時如何更新 IoT Edge 裝置的相關資訊。

如果您想要更新至較新的版本，IoT Edge 裝置將有兩個元件需要更新。 第一個元件是在裝置上執行，並在裝置啟動時啟動執行階段模組的安全性精靈。 目前，安全性精靈只能從裝置本身進行更新。 第二個元件是由 IoT Edge 中樞和 IoT Edge 代理程式模組組成的執行階段。 根據您建構部署的方式，執行階段可以從裝置或從遠端更新。

若要尋找最新版的 Azure IoT Edge，請參閱 [Azure IoT Edge 版本](https://github.com/Azure/azure-iotedge/releases)。

## <a name="update-the-security-daemon"></a>更新安全性精靈

IoT Edge 安全性精靈是一項原生元件，必須在 IoT Edge 裝置上使用套件管理員進行更新。

請使用 `iotedge version` 命令檢查在您的裝置上執行的安全性精靈版本。

### <a name="linux-devices"></a>Linux 裝置

在 Linux x64 裝置上，請使用 apt 取得或適當的套件管理員，將安全性守護程式更新為最新版本。

從 Microsoft 取得最新的存放庫設定：

* **Ubuntu Server 16.04**：

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Ubuntu Server 18.04**：

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Raspbian 延展**：

   ```bash
   curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
   ```

複製產生的清單。

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

安裝 Microsoft GPG 公開金鑰。

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

更新 apt。

   ```bash
   sudo apt-get update
   ```

檢查以查看有哪些可用的 IoT Edge 版本。

   ```bash
   apt list -a iotedge
   ```

如果您想要更新至最新版本的安全性背景程式，請使用下列命令，這也會將**libiothsm-std**更新為最新版本：

   ```bash
   sudo apt-get install iotedge
   ```

如果您想要更新至特定版本的安全性守護程式，請從 apt 清單輸出中指定版本。 當**iotedge**更新時，它會自動嘗試將**libiothsm-std**封裝更新為最新版本，這可能會造成相依性衝突。 如果您不打算使用最新版本，請務必以相同版本的兩個套件為目標。 例如，下列命令會安裝特定版本的1.0.9 版本：

   ```bash
   sudo apt-get install iotedge=1.0.9-1 libiothsm-std=1.0.9-1
   ```

如果您要安裝的版本無法透過 apt 取得，您可以使用捲曲的目標，從[IoT Edge 版本](https://github.com/Azure/azure-iotedge/releases)存放庫中的任何版本。 針對您想要安裝的版本，找出適用于您裝置的**libiothsm-std**和**iotedge**檔案。 針對每個檔案，在檔案連結上按一下滑鼠右鍵，並複製連結位址。 使用連結位址來安裝這些元件的特定版本：

```bash
curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
```

### <a name="windows-devices"></a>Windows 裝置

在 Windows 裝置上，請使用 PowerShell 腳本來更新安全性守護程式。 腳本會自動提取最新版本的安全性守護程式。

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

執行 IoTEdge 命令會從您的裝置移除和更新安全性背景工作，以及兩個執行時間容器映射。 Yaml 檔案會保留在裝置上，以及來自 Moby 容器引擎（如果您使用的是 Windows 容器）的資料。 保留設定資訊表示您不需要在更新程式期間，再次提供裝置的連接字串或裝置布建服務資訊。

如果您想要更新至特定版本的安全性守護程式，請從[IoT Edge](https://github.com/Azure/azure-iotedge/releases)版本中尋找您要設為目標的版本。 在該版本中，下載**Microsoft-Azure-IoTEdge.cab**檔案。 然後，使用 `-OfflineInstallationPath` 參數指向本機檔案位置。 例如：

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux> -OfflineInstallationPath <absolute path to directory>
```

>[!NOTE]
>`-OfflineInstallationPath`參數會在提供的目錄中尋找名為**Microsoft-Azure-IoTEdge.cab**的檔案。 從 IoT Edge 版本 1.0.9-rc4 開始，有兩個 .cab 檔案可供使用，一個用於 AMD64 裝置，另一個用於 ARM32。 請為您的裝置下載正確的檔案，然後將檔案重新命名，以移除架構尾碼。

如需更新選項的詳細資訊，請使用命令 `Get-Help Update-IoTEdge -full` 或參閱[所有安裝參數](how-to-install-iot-edge-windows.md#all-installation-parameters)。

## <a name="update-the-runtime-containers"></a>更新執行階段容器

更新 IoT Edge 代理程式和 IoT Edge 中樞容器的方式，取決於您是在部署中使用輪流標記（例如1.0）還是特定標記（例如1.0.7）。

使用 `iotedge logs edgeAgent` 或 `iotedge logs edgeHub` 命令，檢查您裝置上目前的 IoT Edge 代理程式和 IoT Edge 中樞模組版本。

  ![在記錄中尋找容器版本](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>了解 IoT Edge 標記

IoT Edge 代理程式和 IoT Edge 中樞映像會標示與其相關聯的 IoT Edge 版本。 標記可透過兩種不同的使用方式用於執行階段映像上：

* **累積標記** - 僅使用版本號碼的前兩個值來取得符合這些數字的最新映像。 例如，有新版本指向最新的 1.0.x 版時，就會更新 1.0。 如果 IoT Edge 裝置的容器執行階段重新提取映像，執行階段模組就會更新為最新版本。 進行開發時建議使用此方法。 從 Azure 入口網站執行的部署預設為累積標記。

* **特定標記** - 版本號碼的三個值全都會使用，以明確設定映像版本。 例如，1.0.7 在其初始版本之後不會變更。 當您準備好要更新時，您可以在部署資訊清單中宣告新的版本號碼。 建議將此方法用於生產用途。

### <a name="update-a-rolling-tag-image"></a>更新累積標記映像

如果您在部署中使用累積標記 (例如 mcr.microsoft.com/azureiotedge-hub:**1.0**)，您就必須在裝置上強制執行容器執行階段以提取映像的最新版本。

從您的 IoT Edge 裝置中刪除映像的本機版本。 在 Windows 機器上，解除安裝安全性精靈也會移除執行階段映像，因此您不需要再採取此步驟。

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

您可能需要使用強制 `-f` 旗標將映像移除。

IoT Edge 服務將會提取執行階段映像的最新版本，並自動在您的裝置上將其重新啟動。

### <a name="update-a-specific-tag-image"></a>更新特定標記映像

如果您在部署中使用特定標記（例如，mcr.microsoft.com/azureiotedge-hub：**1.0.8**），則您只需要更新部署資訊清單中的標記，並將變更套用至您的裝置。

1. 在 [Azure 入口網站的 [IoT 中樞中，選取您的 IoT Edge 裝置，然後選取 [**設定模組**]。

1. 在 [ **IoT Edge 模組**] 區段中，選取 [**執行時間設定**]。

   ![設定執行時間設定](./media/how-to-update-iot-edge/configure-runtime.png)

1. 在 [**執行時間設定**] 中，以所需的版本更新**Edge 中樞**的**映射**值。 請先不要選取 [**儲存**]。

   ![更新 Edge 中樞映射版本](./media/how-to-update-iot-edge/runtime-settings-edgehub.png)

1. 折迭**Edge 中樞**設定，或向下展開，並以相同的所需版本更新**edge 代理程式**的**影像**值。

   ![更新 Edge 中樞代理程式版本](./media/how-to-update-iot-edge/runtime-settings-edgeagent.png)

1. 選取 [儲存]。

1. 選取 [審核] [ **+ 建立**]、[檢查部署]，然後選取 [**建立**]。

## <a name="update-offline-or-to-a-specific-version"></a>離線或特定版本的更新

如果您想要離線更新裝置，或更新至特定版本的 IoT Edge 而不是最新版本，您可以使用參數來執行此動作 `-OfflineInstallationPath` 。

有兩個元件可用來更新 IoT Edge 裝置：

* PowerShell 腳本，其中包含安裝指示
* Microsoft Azure IoT Edge cab，其中包含 IoT Edge security daemon （iot）、Moby 容器引擎和 Moby CLI

1. 如需最新的 IoT Edge 安裝檔案和舊版，請參閱[Azure IoT Edge 版本](https://github.com/Azure/azure-iotedge/releases)。

2. 尋找您要安裝的版本，並從版本資訊的 [**資產**] 區段將下列檔案下載到您的 IoT 裝置：

   * IoTEdgeSecurityDaemon.ps1
   * 從1.0.9 或更新版本，或從1.0.8 和更舊版本發行的 Microsoft-Azure-IoTEdge.cab Microsoft-Azure-IoTEdge-amd64.cab。

   從1.0.9 開始也提供 Microsoft-Azure-IotEdge-arm32.cab，僅供測試之用。 Windows ARM32 裝置目前不支援 IoT Edge。

   請務必使用與您所使用之 .cab 檔案相同版本中的 PowerShell 腳本，因為功能會變更以支援每個版本的功能。

3. 如果您下載的 .cab 檔案中有架構尾碼，請將檔案重新命名為，只**Microsoft-Azure-IoTEdge.cab**。

4. 若要以離線元件更新，請使用[點來源](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing)作為 PowerShell 腳本的本機複本。 然後，使用 `-OfflineInstallationPath` 參數作為命令的一部分 `Update-IoTEdge` ，並提供檔案目錄的絕對路徑。 例如，

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Update-IoTEdge -OfflineInstallationPath <path>
   ```

## <a name="update-to-a-release-candidate-version"></a>更新為發行候選版本

Azure IoT Edge 定期發行 IoT Edge 服務的新版本。 在每個穩定版本之前，都有一或多個候選版（RC）版本。 RC 版本包含發行的所有規劃功能，但仍在進行測試和驗證。 如果您想要及早測試新功能，您可以安裝 RC 版本，並透過 GitHub 提供意見反應。

發行候選版本遵循相同的版本編號慣例，但具有 **-rc**加上結尾附加的遞增編號。 您可以在與穩定版本相同的[Azure IoT Edge 版本](https://github.com/Azure/azure-iotedge/releases)清單中看到發行候選版本。 例如，尋找**1.0.9 rc5**和**1.0.9 rc6**，這是**1.0.9**之前的兩個候選版本。 您也可以看到 RC 版本是以**發行前**標籤標示。

IoT Edge 代理程式和中樞模組具有以相同慣例標記的 RC 版本。 例如， **mcr.microsoft.com/azureiotedge-hub:1.0.9-rc6**。

做為預覽，發行候選版本不會包含為一般安裝程式目標的最新版本。 相反地，您需要手動將您想要測試的 RC 版本資產設為目標。 在大部分的情況下，安裝或更新至 RC 版本與將任何其他特定版本的 IoT Edge 做為目標相同。

使用本文中的各節，瞭解如何將 IoT Edge 裝置更新為特定版本的安全性背景程式或執行時間模組。

如果您要在新電腦上安裝 IoT Edge，請使用下列連結來瞭解如何根據您的裝置作業系統來安裝特定版本：

* [Linux](how-to-install-iot-edge-linux.md#install-runtime-using-release-assets)
* [Windows](how-to-install-iot-edge-windows.md#offline-or-specific-version-installation)

## <a name="next-steps"></a>後續步驟

檢視最新的 [Azure IoT Edge 版本](https://github.com/Azure/azure-iotedge/releases)。

在[物聯網的 blog](https://azure.microsoft.com/blog/topics/internet-of-things/)中隨時掌握最新的更新和公告
