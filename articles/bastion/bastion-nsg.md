---
title: 使用 Azure 防禦中的 Vm 和 Nsg
description: 本文說明如何將 NSG 存取納入 Azure 防禦
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: charwen
ms.openlocfilehash: e4782213b38ad9e265cc66c3073dc5f357c50561
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85321644"
---
# <a name="working-with-nsg-access-and-azure-bastion"></a>使用 NSG 存取和 Azure 防禦

使用 Azure 防禦時，您可以使用網路安全性群組（Nsg）。 如需詳細資訊，請參閱[安全性群組](../virtual-network/security-overview.md)。

:::image type="content" source="./media/bastion-nsg/figure-1.png" alt-text="NSG":::

在此圖表中：

* 防禦主機會部署到虛擬網路。
* 使用者使用任何 HTML5 瀏覽器連線到 Azure 入口網站。
* 使用者會流覽至 Azure 虛擬機器以進行 RDP/SSH。
* 連線整合-在瀏覽器內單鍵按一下 RDP/SSH 會話
* Azure VM 上不需要公用 IP。

## <a name="network-security-groups"></a><a name="nsg"></a>網路安全性群組

本節說明使用者與 Azure 防禦之間的網路流量，以及至虛擬網路中的 Vm 目標：

### <a name="azurebastionsubnet"></a><a name="apply"></a>AzureBastionSubnet

Azure 防禦會特別部署至***AzureBastionSubnet***。

* **輸入流量：**

   * **來自公用網際網路的輸入流量：** Azure 防禦會建立公用 IP，其必須在公用 IP 上啟用埠443，以供輸入流量使用。 不需要在 AzureBastionSubnet 上開啟埠3389/22。
   * **來自 Azure 防禦控制平面的輸入流量：** 針對 [控制平面連線]，啟用埠443從**GatewayManager**服務標記輸入。 這可讓控制平面（也就是閘道管理員）能夠與 Azure 防禦交談。

* **輸出流量：**

   * **目標 vm 的輸出流量：** Azure 防禦會透過私人 IP 到達目標 Vm。 Nsg 需要允許埠3389和22的其他目標 VM 子網的輸出流量。
   * **Azure 中其他公用端點的輸出流量：** Azure 防禦必須能夠連接到 Azure 內的各種公用端點（例如，用於儲存診斷記錄和計量記錄）。 基於這個理由，Azure 防禦需要輸出至443以**AzureCloud**服務標記。

### <a name="target-vm-subnet"></a>目標 VM 子網
這是包含您想要 RDP/SSH 的目標虛擬機器的子網。

   * **來自 Azure 防禦的輸入流量：** Azure 防禦會透過私人 IP 到達目標 VM。 RDP/SSH 埠（分別是埠3389/22）必須在目標 VM 端透過私人 IP 開啟。 最佳做法是，您可以在此規則中新增 Azure 防禦子網 IP 位址範圍，讓防禦能夠在目標 VM 子網的目標 Vm 上開啟這些埠。


## <a name="next-steps"></a>後續步驟

如需 Azure 防禦的詳細資訊，請參閱[常見問題](bastion-faq.md)。
