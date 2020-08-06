---
title: Azure CLI 指令碼範例 - 使用 NLB 建立 Linux VM
description: Azure CLI 指令碼範例 - 使用 NLB 建立 Linux VM
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 15d5a11d91a2ffcea98fd8a3f5c7bc3a08f32613
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87479518"
---
# <a name="create-a-highly-available-vm"></a>建立高可用性 VM

此指令碼範例會建立所需的一切，以執行數部依據高可用性和負載平衡組態所設定的 Ubuntu 虛擬機器。 執行指令碼之後，您將擁有三部已加入至 Azure 可用性設定組並可透過 Azure Load Balancer 存取的虛擬機器。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>範例指令碼

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>清除部署

執行下列命令來移除資源群組、VM 和所有相關資源。

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>指令碼說明

此指令碼使用下列命令來建立資源群組、虛擬機器、可用性設定組、負載平衡器和所有相關資源。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [az group create](/cli/azure/group) | 建立用來存放所有資源的資源群組。 |
| [az network vnet create](/cli/azure/network/vnet) | 建立 Azure 虛擬網路和子網路。 |
| [az network public-ip create](/cli/azure/network/public-ip) | 建立具有靜態 IP 位址和相關聯 DNS 名稱的公用 IP 位址。 |
| [az network lb create](/cli/azure/network/lb) | 建立 Azure 網路負載平衡器 (NLB)。 |
| [az network lb probe create](/cli/azure/network/lb/probe) | 建立 NLB 探查。 NLB 探查可用來監視 NLB 集合中的每部 VM。 如有任何 VM 變得無法存取，就不會將流量路由至該 VM。 |
| [az network lb rule create](/cli/azure/network/lb/rule) | 建立 NLB 規則。 在此範例中，會為連接埠 80 建立規則。 當 HTTP 流量抵達 NLB 時，就會路由到 NLB 集合中某個 VM 的連接埠 80。 |
| [az network lb inbound-nat-rule create](/cli/azure/network/lb/inbound-nat-rule) | 建立 NLB 網路位址轉譯 (NAT) 規則。  NAT 規則會將 NLB 的連接埠對應到 VM 上的連接埠。 在此範例中，會為 NLB 集合的每部 VM 所收到的 SSH 流量建立 NAT 規則。  |
| [az network nsg create](/cli/azure/network/nsg) | 建立網路安全性群組 (NSG)，做為網際網路和虛擬機器之間的安全性界限。 |
| [az network nsg rule create](/cli/azure/network/nsg/rule) | 建立允許輸入流量的 NSG 規則。 在此範例中，會開放連接埠 22 供 SSH 流量使用。 |
| [az network nic create](/cli/azure/network/nic) | 建立虛擬網路卡，並將它連接至虛擬網路、子網路及 NSG。 |
| [az vm availability-set create](/cli/azure/network/lb/rule) | 建立可用性設定組。 可用性設定組可將虛擬機器分散到各個實體資源，讓整個集合不致受到萬一發生的失敗所影響，藉此來確保應用程式運作時間。 |
| [az vm create](/cli/azure/vm/availability-set) | 建立虛擬機器，並將它連線到網路卡、虛擬網路、子網路及 NSG。 此命令也會指定要使用的虛擬機器映像和管理認證。  |
| [az group delete](/cli/azure/vm/extension) | 刪除資源群組，包括所有的巢狀資源。 |

## <a name="next-steps"></a>後續步驟

如需 Azure CLI 的詳細資訊，請參閱 [Azure CLI 文件](/cli/azure)。

您可以在 [Azure Linux VM 文件](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)中找到其他的虛擬機器 CLI 指令碼範例。
