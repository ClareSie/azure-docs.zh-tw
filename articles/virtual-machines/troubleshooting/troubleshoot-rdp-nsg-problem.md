---
title: 因 NSG 中未啟用 RDP 連接埠而無法連線到 Azure VM | Microsoft Docs
description: 了解如何在 Azure 入口網站中排解因 NSG 設定而導致 RDP 失敗的問題 | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 6c877690cf27edd73f1d828b8a1dda6f4f34e780
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "77918167"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>因 NSG 中未啟用 RDP 連接埠而無法從遠端連線到 VM

本文將說明如何解決因網路安全性群組 (NSG) 中未啟用遠端桌面通訊協定 (RDP) 連接埠，而無法連線到 Azure Windows 虛擬機器 (VM) 的問題。


## <a name="symptom"></a>徵狀

您無法對 Azure 中的 VM 建立 RDP 連線，因為網路安全性群組中未開啟 RDP 連接埠。

## <a name="solution"></a>解決方案 

當您建立新 VM 時，根據預設，系統會封鎖所有來自網際網路的流量。 

若要啟用 NSG 中的 RDP 連接埠，請遵循下列步驟：
1. 登入[Azure 入口網站](https://portal.azure.com)。
2. 在 [虛擬機器] **** 中，選取有此問題的 VM。 
3. 在 [設定]  中，選取 [網路]  。 
4. 在 [輸入連接埠規則]**** 中，檢查是否已正確設定 RDP 的連接埠。 以下是設定範例： 

    **優先順序**：300 </br>
    **名稱**：Port_3389 </br>
    **埠（目的地）**：3389 </br>
    **通訊協定**： TCP </br>
    **來源**：任何 </br>
    **目的地**：任何 </br>
    **動作**：允許 </br>

如果您指定來源 IP 位址，此設定會只允許來自特定 IP 位址或 IP 位址範圍的流量連線至 VM。 請確定您用來啟動 RDP 工作階段的電腦在該範圍內。

如需有關 NSG 的詳細資訊，請參閱[網路安全性群組](../../virtual-network/security-overview.md)。

> [!NOTE]
> RDP 連接埠 3389 已公開至網際網路。 因此，我們建議您只將此連接埠用於測試。 針對生產環境，我們建議您使用 VPN 或私人連線。

## <a name="next-steps"></a>後續步驟

如果 NSG 中已啟用 RDP 連接埠，請參閱[對 Azure VM 中的一般 RDP 錯誤進行疑難排解](./troubleshoot-rdp-general-error.md)。



