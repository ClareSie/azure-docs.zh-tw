---
title: 虛擬網路的 Azure CLI 範例
description: 了解您可用來完成 Azure CLI 中工作的各種範例指令碼，包括為多層式應用程式建立虛擬網路。
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: kumud
ms.custom: devx-track-azurecli
ms.openlocfilehash: 539d11205ffead52d7f40526f2c712e8cf8b5cdd
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87501435"
---
# <a name="azure-cli-samples-for-virtual-network"></a>虛擬網路的 Azure CLI 範例

下表包含使用 Azure CLI 命令所建置的 Bash 指令碼連結：

| 指令碼 | 描述 |
|----|----|
| [建立多層式應用程式的虛擬網路](./scripts/virtual-network-cli-sample-multi-tier-application.md) | 建立具有前端和後端子網路的虛擬網路。 傳送到前端子網路的流量會限制為 HTTP 和 SSH，而傳送到後端子網路的流量則限制為 MySQL 且連接埠為 3306。 |
| [對等互連兩個虛擬網路](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | 在相同區域中建立和連線兩個虛擬網路。 |
| [透過網路虛擬設備路由傳送流量](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | 建立具有前端和後端子網路的虛擬網路，以及可在兩個子網路間路由傳送流量的 VM。 |
| [篩選輸入和輸出 VM 網路流量](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | 建立具有前端和後端子網路的虛擬網路。 傳輸至前端子網路的輸入網路流量限制為 HTTP、HTTPS 和 SSH。 不允許從後端子網路傳輸至網際網路的輸出流量。 |
|[設定具有 Basic Load Balancer 的 IPv4 + IPv6 雙重堆疊虛擬網路](./scripts/virtual-network-cli-sample-ipv6-dual-stack.md)|部署包含兩個 VM 和一個具備 IPv4 與 IPv6 公用 IP 位址的 Azure Basic Load Balancer 的雙重堆疊 (IPv4 + IPv6) 虛擬網路。 |
|[設定具有 Standard Load Balancer 的 IPv4 + IPv6 雙重堆疊虛擬網路](./scripts/virtual-network-cli-sample-ipv6-dual-stack-standard-load-balancer.md)|部署包含兩個 VM 和一個具備 IPv4 與 IPv6 公用 IP 位址的 Azure Standard Load Balancer 的雙重堆疊 (IPv4 + IPv6) 虛擬網路。 |
|[教學課程：建立和測試 NAT 閘道 - Azure CLI](../virtual-network/tutorial-create-validate-nat-gateway-cli.md)|使用來源和目的地虛擬機器建立及驗證 NAT 閘道。 |
