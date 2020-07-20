---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 10/24/2019
ms.openlocfilehash: a3340eb9e53afa83c35109bad7d22f81413dd644
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80234151"
---
## <a name="prepare-an-iot-hub"></a>準備 IoT 中樞

您的 Azure 訂用帳戶中也必須要有 Azure IoT 中樞，才能完成本快速入門。 如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。 如果您沒有 IoT 中樞，請依照[下列指示建立一個](../articles/iot-hub/iot-hub-create-using-cli.md)。

如果您要在本機使用 Azure CLI，請先使用 `az login` 登入您的 Azure 訂用帳戶。 如果您要在 Azure Cloud Shell 中執行這些命令，則會自動登入。

如果您在本機使用 Azure CLI，`az` 版本應為 **2.0.73** 或更新版本；Azure Cloud Shell 會使用最新版本。 使用 `az --version` 命令檢查電腦上所安裝的版本。

執行下列命令，將適用於 Azure CLI 的 Microsoft Azure IoT 擴充功能新增至您的執行個體：

```azurecli-interactive
az extension add --name azure-iot
```

執行下列命令，在 IoT 中樞建立裝置身分識別。 將 **YourIoTHubName** 和 **YourDeviceID** 預留位置取代為您自行選擇的「IoT 中樞名稱」  和「裝置識別碼」  。

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

執行下列命令，以針對您剛註冊的裝置取得「裝置連接字串」  (請記下以供後續使用)：

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```
