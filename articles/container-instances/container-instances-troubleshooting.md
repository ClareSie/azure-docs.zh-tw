---
title: 針對常見問題進行疑難排解
description: 瞭解如何針對您部署、執行或管理 Azure 容器實例時所遇到的常見問題進行疑難排解
ms.topic: article
ms.date: 06/25/2020
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: d8e7fb85e369f5f278436370944eafeb1fb6a50e
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2020
ms.locfileid: "96779510"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>在 Azure 容器執行個體中針對常見問題進行疑難排解

本文說明如何針對管理或將容器部署到 Azure 容器執行個體的常見問題，進行疑難排解。 [另請參閱常見問題。](container-instances-faq.md)

如果您需要其他支援，請參閱 [Azure 入口網站](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)中的可用說明 **+ 支援** 選項。

## <a name="issues-during-container-group-deployment"></a>容器群組部署期間的問題
### <a name="naming-conventions"></a>命名規範

定義您的容器規格時，特定參數需要遵循命名限制。 以下資料表具有容器群組屬性的特定需求。 如需詳細資訊，請參閱 Azure 架構中心中的 [命名慣例][azure-name-restrictions] ，以及 [Azure 資源的命名規則和限制][naming-rules]。

| 影響範圍 | 長度 | 大小寫 | 有效字元 | 建議模式 | 範例 |
| --- | --- | --- | --- | --- | --- |
| 容器名稱<sup>1</sup> | 1-63 |小寫 | 除了第一個或最後一個字元以外，都可以使用英數字元和連字號 |`<name>-<role>-container<number>` |`web-batch-container1` |
| 容器連接埠 | 介於 1 到 65535 之間 |整數 |介於 1 到 65535 之間的整數 |`<port-number>` |`443` |
| DNS 名稱標籤 | 5-63 |不區分大小寫 |除了第一個或最後一個字元以外，都可以使用英數字元和連字號 |`<name>` |`frontend-site1` |
| 環境變數 | 1-63 |不區分大小寫 |除了第一個或最後一個字元以外，都可以使用英數字元和底線 (_) |`<name>` |`MY_VARIABLE` |
| 磁碟區名稱 | 5-63 |小寫 |除了第一個或最後一個字元以外的任何位置的英數位元和連字號。 不能包含兩個連續連字號。 |`<name>` |`batch-output-volume` |

<sup>1</sup>如果容器實例未獨立指定，例如使用命令部署，則也會限制容器組名 `az container create` 。

### <a name="os-version-of-image-not-supported"></a>不支援映像的 OS 版本

如果您指定 Azure 容器執行個體不支援的映像，則會傳回 `OsVersionNotSupported` 錯誤。 錯誤會類似下面內容，其中 `{0}` 是您嘗試部署的映像名稱：

```json
{
  "error": {
    "code": "OsVersionNotSupported",
    "message": "The OS version of image '{0}' is not supported."
  }
}
```

當部署以 Semi-Annual 通道版本1709或1803（不受支援）為基礎的 Windows 映像時，最常遇到此錯誤。 如需 Azure 容器實例中支援的 Windows 映像，請參閱 [常見問題](container-instances-faq.md#what-windows-base-os-images-are-supported)。

### <a name="unable-to-pull-image"></a>無法提取映像

如果 Azure 容器執行個體一開始無法提取您的映像，它會重試一段時間。 如果映像提取作業持續發生失敗，ACI 最終會停止部署，而且您可能會看到 `Failed to pull image` 錯誤。

若要解決此問題，請刪除容器執行個體並重試您的部署。 請確定此映像存在在登錄中，而且您已輸入正確的映像名稱。

如果無法提取映像，系統便會顯示如 [az container show][az-container-show] 輸出中所示的事件：

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"mcr.microsoft.com/azuredocs/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```
### <a name="resource-not-available-error"></a>資源無法使用錯誤

Azure 中有各種不同的地區資源負載，因此您在嘗試部署容器執行個體時，可能會收到下列錯誤訊息：

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

此錯誤訊息表示您嘗試執行部署產品的地區負載過重，因此當時無法配置您為容器指定的資源。 執行下列一或多個風險降低步驟有助於解決問題。

* 確認容器部署設定是否位於 [Azure Container Instances 地區可用性](container-instances-region-availability.md)所定義的參數範圍內
* 為容器指定較低階的 CPU 和記憶體設定
* 部署至其他 Azure 地區
* 過一段時間再部署

## <a name="issues-during-container-group-runtime"></a>容器群組執行時間期間的問題
### <a name="container-continually-exits-and-restarts-no-long-running-process"></a>容器不斷結束又重新啟動 (沒有長時間執行的程序)

容器群組的[重新啟動原則](container-instances-restart-policy.md)預設為 [一律]，因此容器群組中的群組在執行完成後一律會重新啟動。 如果您要執行以工作為基礎的容器，則可能需要將此設定變更為 [OnFailure] 或 [永不]。 如果指定 **OnFailure** 後仍持續重新啟動，可能是容器中執行的應用程式或指令碼的問題。

如果執行的容器群組不含長時間執行的程序，您可能會看到 Ubuntu 或 Alpine 之類的映像重複地結束並重新啟動。 透過 [EXEC](container-instances-exec.md) 連線是不可行的，因為容器沒有任何程序可維持其存留狀態。 若要解決此問題，請在容器群組部署中包含如下所示的啟動命令，讓容器保持執行狀態。

```azurecli-interactive
## Deploying a Linux container
az container create -g MyResourceGroup --name myapp --image ubuntu --command-line "tail -f /dev/null"
```

```azurecli-interactive 
## Deploying a Windows container
az container create -g myResourceGroup --name mywindowsapp --os-type Windows --image mcr.microsoft.com/windows/servercore:ltsc2019
 --command-line "ping -t localhost"
```

容器執行個體 API 和 Azure 入口網站包含 `restartCount` 屬性。 若要檢查容器的重新啟動次數，可以在 Azure CLI 中使用 [az container show][az-container-show] 命令。 在下列範例輸出中 (為簡潔起見已截斷畫面)，您可以在輸出的結尾看到 `restartCount` 屬性。

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Linux 散發套件的大部分容器映像都會設定殼層 (例如 bash) 來作為預設命令。 因為殼層本身不是長時間執行的服務，當設定為預設的 **Always** 重新啟動原則時，這些容器會立即結束並落入重新啟動迴圈。

### <a name="container-takes-a-long-time-to-start"></a>容器要等很久才會啟動

在 Azure 容器實例中參與容器啟動時間的三個主要因素如下：

* [映像大小](#image-size)
* [映像位置](#image-location)
* [快取的影像](#cached-images)

Windows 映像會有[其他考量](#cached-images)。

#### <a name="image-size"></a>映像大小

如果您的容器要等很久才會啟動，但最終還是會啟動成功，請先看看您的容器映像大小。 因為 Azure Container Instances 會視需要來提取您的容器映像，因此啟動時間的長短會與其大小直接相關。

您可以在 Docker CLI 中使用 `docker images` 命令來檢視容器映像的大小：

```console
$ docker images
REPOSITORY                                    TAG       IMAGE ID        CREATED          SIZE
mcr.microsoft.com/azuredocs/aci-helloworld    latest    7367f3256b41    15 months ago    67.6MB
```

讓映像不會變得太大的關鍵在於，確保最終的映像不會包含執行階段所不需要的任何項目。 若要做到這一點，有一種方式是使用[多階段建置][docker-multi-stage-builds]。 多階段建置可讓您輕鬆地確保最終映像只包含應用程式所需的構件，而不會包含建置階段所需的任何額外內容。

#### <a name="image-location"></a>映像位置

另一種可在容器啟動階段降低對於映像提取作業影響的方式，是在您想要部署容器執行個體的相同區域中，將容器映像裝載在 [Azure Container Registry](../container-registry/index.yml) 中。 這種方式會縮短容器映像需要經過的網路路徑，從而大幅縮短下載時間。

#### <a name="cached-images"></a>快取的影像

Azure 容器實例會使用快取機制，以協助加速建立于通用 [Windows 基礎映射](container-instances-faq.md#what-windows-base-os-images-are-supported)（包括、和）之映射的容器啟動時間 `nanoserver:1809` `servercore:ltsc2019` `servercore:1809` 。 常用的 Linux 映射（例如 `ubuntu:1604` 和） `alpine:3.6` 也會快取。 針對 Windows 和 Linux 映射，請避免使用 `latest` 標記。 請參閱 Container Registry 的 [映射標記最佳做法](../container-registry/container-registry-image-tag-version.md) ，以取得指導方針。 如需快取映射和標記的最新清單，請使用列出快取 [影像][list-cached-images] API。

> [!NOTE]
> 在 Azure 容器執行個體中使用以 Windows Server 2019 為基礎的映像是預覽功能。

#### <a name="windows-containers-slow-network-readiness"></a>Windows 容器會降低網路整備速度

在初始建立時，Windows 容器可能最多 30 秒 (或更長，但很罕見) 沒有任何輸入或輸出連線。 如果您的容器應用程式需要網際網路連線，請新增延遲，然後重試邏輯，以允許有 30 秒的時間來建立網際網路連線。 初始安裝之後，容器網路應該就可以正常繼續運作。

### <a name="cannot-connect-to-underlying-docker-api-or-run-privileged-containers"></a>無法連線到基礎 Docker API 或執行具有特殊權限的容器

Azure 容器執行個體不會公開基礎結構 (其中裝載容器群組) 的直接存取。 這包括 Docker API 的存取權，Docker API 可在容器主機上執行，並且可執行具有特殊權限的容器。 如果您需要 Docker 互動，請查閱 [REST 參考文件](/rest/api/container-instances/)，以了解 ACI API 支援的內容。 如果有遺漏的項目，請在 [ACI 意見反應論壇](https://aka.ms/aci/feedback)上提交要求。

### <a name="container-group-ip-address-may-not-be-accessible-due-to-mismatched-ports"></a>容器群組 IP 位址可能因為連接埠不相符而無法存取

Azure 容器實例尚未支援像是一般 docker 設定的埠對應。 如果您認為容器群組的 IP 位址無法存取，當您認為它應該是時，請確定您已將容器映射設定為使用屬性來接聽您在容器群組中公開的相同埠 `ports` 。

如果您想要確認 Azure 容器實例可以接聽您在容器映射中設定的埠，請測試 `aci-helloworld` 公開端口的映射部署。 也請執行 `aci-helloworld` 應用程式，讓它在埠上接聽。 `aci-helloworld` 接受選擇性的環境變數 `PORT` ，以覆寫其接聽的預設埠80。 例如，若要測試埠9000，請在建立容器群組時設定 [環境變數](container-instances-environment-variables.md) ：

1. 設定容器群組以公開端口9000，並將埠號碼傳遞為環境變數的值。 此範例會針對 Bash shell 進行格式化。 如果您偏好使用 PowerShell 或命令提示字元之類的另一個 shell，您將需要據以調整變數指派。
    ```azurecli
    az container create --resource-group myResourceGroup \
    --name mycontainer --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --ip-address Public --ports 9000 \
    --environment-variables 'PORT'='9000'
    ```
1. 在的命令輸出中，尋找容器群組的 IP 位址 `az container create` 。 尋找 **ip** 的值。 
1. 成功布建容器之後，請在瀏覽器中流覽至容器應用程式的 IP 位址和埠，例如： `192.0.2.0:9000` 。 

    您應該會看到「歡迎使用 Azure 容器實例！」 web 應用程式所顯示的訊息。
1. 當您使用完容器之後，請使用下列命令將它移除 `az container delete` ：

    ```azurecli
    az container delete --resource-group myResourceGroup --name mycontainer
    ```

## <a name="next-steps"></a>後續步驟

瞭解如何 [取得容器記錄和事件](container-instances-get-logs.md) ，以協助您進行容器的調試。

<!-- LINKS - External -->
[azure-name-restrictions]: /azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#naming-and-tagging-resources
[naming-rules]: ../azure-resource-manager/management/resource-name-rules.md
[windows-sac-overview]: /windows-server/get-started/semi-annual-channel-overview
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/_/microsoft-windows-servercore
[docker-hub-windows-nano]: https://hub.docker.com/_/microsoft-windows-nanoserver

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az-container-show
[list-cached-images]: /rest/api/container-instances/location/listcachedimages
