---
title: 設定 Linux VM 的 DHCPv6
titleSuffix: Azure Load Balancer
description: 在本文中，您將瞭解如何設定 Linux Vm 的 DHCPv6。
services: load-balancer
documentationcenter: na
author: asudbring
keywords: ipv6, azure load balancer, 雙重堆疊, 公用 ip, 原生 ipv6, 行動, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2019
ms.author: allensu
ms.openlocfilehash: d8bd62bab627beb70a8fcba276bf8c2eca309c45
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96006741"
---
# <a name="configure-dhcpv6-for-linux-vms"></a>設定 Linux VM 的 DHCPv6


Azure Marketplace 中的一些 Linux 虛擬機器映像沒有預設的動態主機設定通訊協定版本 6 (DHCPv6) 設定。 若要支援 IPv6，在您使用的 Linux OS 散發套件中必須設定 DHCPv6。 各種 Linux 發行套件會以各種不同的方式設定 DHCPv6，因為它們使用不同的套件。

> [!NOTE]
> Azure Marketplace 中最新的 SUSE Linux 和 CoreOS 映像已有預先設定 DHCPv6。 使用這些映像不需要再進行額外的變更。

本文件說明如何啟用 DHCPv6 使您的 Linux 虛擬機器取得 IPv6 位址。

> [!WARNING]
> 不當編輯網路組態檔可能會使您失去 VM 的網路存取權。 我們建議您先在非生產系統上測試組態變更。 本文中的指示已經過在 Azure Marketplace 中最新版 Linux 映像上測試過。 如需更詳細的指示，請參閱您所用 Linux 版本的文件。

## <a name="ubuntu"></a>Ubuntu

1. 編輯 */etc/dhcp/dhclient6.conf* 檔案，並新增下行：

    ```config
    timeout 10;
    ```

2. 編輯下列組態的 eth0 介面網路組態︰

   * 在 **Ubuntu 12.04 和 14.04** 上，編輯 */etc/network/interfaces.d/eth0.cfg* 檔案。 
   * 在 **Ubuntu 16.04** 上，編輯 */etc/network/interfaces.d/50-cloud-init.cfg* 檔案。

    ```config
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. 更新 IPv6 位址︰

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

從 Ubuntu 17.10 開始，預設的網路設定機制是 [NETPLAN]( https://netplan.io)。  在安裝/具現化時，NETPLAN 會從位於下列位置的 YAML 設定檔讀取網路設定：/{lib、etc、run}/netplan/*. YAML。

請在您的設定中為每個 ethernet 介面包含 *dhcp6： true* 語句。  例如：

```config
network:
  version: 2
  ethernets:
    eno1:
      dhcp6: true
```

在早期開機期間，netplan 「網路轉譯器」會將設定寫入/run，以將裝置的控制權交給指定的網路背景程式，以取得有關 NETPLAN 的參考資訊，請參閱 https://netplan.io/reference 。
 
## <a name="debian"></a>Debian

1. 編輯 */etc/dhcp/dhclient6.conf* 檔案，並新增下行：

    ```config
    timeout 10;
    ```

2. 編輯 */etc/network/interfaces* 檔案，並新增下列組態：

    ```config
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. 更新 IPv6 位址︰

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-centos-and-oracle-linux"></a>RHEL、CentOS 和 Oracle Linux

1. 編輯 */etc/sysconfig/network* 檔案，並新增下列參數：

    ```config
    NETWORKING_IPV6=yes
    ```

2. 編輯 */etc/sysconfig/network-scripts/ifcfg-eth0* 檔案，並新增下列兩個參數：

    ```config
    IPV6INIT=yes
    DHCPV6C=yes
    ```

3. 更新 IPv6 位址︰

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-and-opensuse-13"></a>SLES 11 和 openSUSE 13

在 Azure 中最新的 SUSE Linux Enterprise Server (SLES) 和 openSUSE 映像已預先設定 DHCPv6。 使用這些映像不需要再進行額外的變更。 如果您的 VM 是以較舊或自訂 SUSE 映像建置而成，請執行下列步驟︰

1. 如有需要，安裝 `dhcp-client` 套件：

    ```bash
    sudo zypper install dhcp-client
    ```

2. 編輯 */etc/sysconfig/network/ifcfg-eth0* 檔案，並新增下列參數：

    ```config
    DHCLIENT6_MODE='managed'
    

3. Renew the IPv6 address:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 和 openSUSE Leap

在 Azure 中最新的 SLES 和 openSUSE 映像已預先設定 DHCPv6。 使用這些映像不需要再進行額外的變更。 如果您的 VM 是以較舊或自訂 SUSE 映像建置而成，請執行下列步驟︰

1. 編輯 */etc/sysconfig/network/ifcfg-eth0* 檔案，並以下列值取代 `#BOOTPROTO='dhcp4'` 參數：

    ```config
    BOOTPROTO='dhcp'
    ```

2. 對 */etc/sysconfig/network/ifcfg-eth0* 檔案，新增下列參數：

    ```config
    DHCLIENT6_MODE='managed'
    ```

3. 更新 IPv6 位址︰

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

在 Azure 中最新的 SLES 映像已預先設定 DHCPv6。 使用這些映像不需要再進行額外的變更。 如果您的 VM 是以較舊或自訂 CoreOS 映像建置而成，請執行下列步驟︰

1. 編輯 */etc/systemd/network/10_dhcp.network* 檔案：

    ```config
    [Match]
    eth0

    [Network]
    DHCP=ipv6
    ```

2. 更新 IPv6 位址︰

    ```bash
    sudo systemctl restart systemd-networkd
    ```
