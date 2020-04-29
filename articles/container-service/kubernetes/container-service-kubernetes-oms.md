---
title: (已淘汰) 監視 Azure Kubernetes 叢集 - Operations Management
description: 使用 Log Analytics 監視 Azure Container Service 中的 Kubernetes 叢集
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 02d04076ccc41d243a493838667f5e8cc6bfa5ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "79371149"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-log-analytics"></a>(已淘汰) 使用 Log Analytics 監視 Azure Container Service 叢集

> [!TIP]
> 如需使用 Azure Kubernetes Service 的此文更新版本，請參閱[適用於容器的 Azure 監視器](../../azure-monitor/insights/container-insights-overview.md)。

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>先決條件
本逐步解說假設您已[使用 Azure Container Service 建立 Kubernetes 叢集](container-service-kubernetes-walkthrough.md)。

同時也假設您已經安裝 `az` Azure cli 和 `kubectl` 工具。

您可以藉由執行下列操作來測試是否已安裝 `az` 工具：

```azurecli
az --version
```

如果您尚未安裝 `az` 工具，[這裡](https://github.com/azure/azure-cli#installation)有指示。
或者，您可以使用 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)，它已為您安裝 `az` Azure cli 和 `kubectl` 工具。

您可以藉由執行下列操作來測試是否已安裝 `kubectl` 工具：

```console
kubectl version
```

如果您尚未安裝 `kubectl`，可以執行︰

```azurecli
az acs kubernetes install-cli
```

若要測試您的 kubectl 工具中是否已安裝 kubernetes 金鑰，您可以執行：

```console
kubectl get nodes
```

如果上述命令發生錯誤，您必須將 kubernetes 叢集金鑰安裝到 kubectl 工具中。 您可以使用下列命令來執行此操作：

```azurecli
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-log-analytics"></a>使用Log Analytics 監視容器

Log Analytics 是 Microsoft 的雲端型 IT 管理解決方案，可協助您管理並保護內部部署和雲端基礎結構。容器解決方案是 Log Analytics 中的解決方案，可協助您在單一位置檢視容器詳細目錄、效能和記錄。 您可以在集中式位置檢視記錄以稽核、對容器進行疑難排解，並尋找壟斷及佔用過量主機資源的容器。

![](media/container-service-monitoring-oms/image1.png)

如需容器解決方案的詳細資訊，請參閱[容器解決方案 Log Analytics](../../azure-monitor/insights/containers.md)。

## <a name="installing-log-analytics-on-kubernetes"></a>在 Kubernetes 上安裝 Log Analytics

### <a name="obtain-your-workspace-id-and-key"></a>取得您的工作區識別碼和金鑰
為了使 Log Analytics 代理程式能與服務溝通，必須使用工作區識別碼和工作區金鑰來設定它。 若要取得工作區識別碼和金鑰，您必須在 <https://mms.microsoft.com> 建立帳戶。
請依步驟指示建立帳戶。 當您完成建立帳戶之後，可以按一下 [Log Analytics]**** 刀鋒視窗，然後按一下您的工作區名稱，來取得識別碼和金鑰。 接著，依序在 [進階設定]****、[連接的來源]**** 及 [Linux 伺服器]**** 下方，您將可找到所需的資訊，如下所示。

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-log-analytics-agent-using-a-daemonset"></a>使用 DaemonSet 安裝 Log Analytics 代理程式
DaemonSet 是 Kubernetes 用來在叢集中每個主機上執行容器的單一執行個體。
它們非常適合用來執行監視代理程式。

以下是[DAEMONSET YAML](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)檔案。 將它儲存到名為 `oms-daemonset.yaml` 的檔案，並將檔案中的 `WSID` 和 `KEY` 預留位置值取代為您的工作區識別碼和金鑰。

當您將工作區識別碼和金鑰新增至 DaemonSet 設定之後，就可以在您的叢集上使用 `kubectl` 命令列工具安裝 Log Analytics 代理程式：

```console
kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-log-analytics-agent-using-a-kubernetes-secret"></a>使用 Kubernetes 秘密安裝 Log Analytics 代理程式
若要保護您的 Log Analytics 工作區識別碼和金鑰，您可以使用 Kubernetes 秘密作為 DaemonSet YAML 檔案的一部分。

- 複製腳本、秘密範本檔案和 DaemonSet YAML 檔案（從存放[庫](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)），並確定它們位於相同的目錄中。
  - 秘密產生指令碼 - secret-gen.sh
  - 祕密範本 - secret-template.yaml
    - DaemonSet YAML 檔案 - omsagent-ds-secrets.yaml
- 執行指令碼。 此指令碼會要求提供 Log Analytics 工作區識別碼和主要金鑰。 插入該資訊，而指令碼會建立秘密 yaml 檔案，以便您執行。

  ```console
  sudo bash ./secret-gen.sh
  ```

  - 執行以下命令來建立祕密 Pod：

     ```console
     kubectl create -f omsagentsecret.yaml
     ```

  - 若要檢查，請執行下列命令：

  ```console
  kubectl get secrets
  ```

  ```output
  NAME                  TYPE                                  DATA      AGE
  default-token-gvl91   kubernetes.io/service-account-token   3         50d
  omsagent-secret       Opaque                                2         1d
  root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
  Name:           omsagent-secret
  Namespace:      default
  Labels:         <none>
  Annotations:    <none>

  Type:   Opaque

  Data
  ====
  WSID:   36 bytes
  KEY:    88 bytes
  ```

  - 執行下列程式來建立您的 omsagent daemon：
  
  ```console
  kubectl create -f omsagent-ds-secrets.yaml
  ```

### <a name="conclusion"></a>結論
這樣就完成了！ 幾分鐘後，您應該可以看到資料流向您的 Log Analytics 儀表板。
