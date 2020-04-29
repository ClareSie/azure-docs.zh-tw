---
title: Azure HDInsight 管理 IP 位址
description: 瞭解您必須允許輸入流量的 IP 位址，以便適當地設定網路安全性群組和使用者定義的路由，以使用 Azure HDInsight 的虛擬網路。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/03/2020
ms.openlocfilehash: 5e8f39b58f258742108fe323d9395efd87bc288f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "78271820"
---
# <a name="hdinsight-management-ip-addresses"></a>HDInsight 管理 IP 位址

> [!Important]
> 在大部分情況下，您現在可以使用網路安全性群組的[服務標記](hdinsight-service-tags.md)功能，而不是手動新增 IP 位址。 只有服務標籤才會新增新的區域，而靜態 IP 位址最後會被取代。

如果您使用網路安全性群組（Nsg）或使用者定義的路由（Udr）來控制 HDInsight 叢集的輸入流量，您必須確定您的叢集可以與重要的 Azure 健康狀態和管理服務進行通訊。  這些服務的某些 IP 位址是特定區域，而其中一些會套用到所有 Azure 區域。 如果您不是使用自訂 DNS，您可能也需要允許來自 Azure DNS 服務的流量。

下列各節將討論必須允許的特定 IP 位址。

## <a name="azure-dns-service"></a>Azure DNS 服務

如果您使用的是 Azure 提供的 DNS 服務，允許從埠53上的__168.63.129.16__進行存取。 如需詳細資訊，請參閱[vm 和角色實例的名稱解析](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)檔。 如果您使用的是自訂 DNS，請略過此步驟。

## <a name="health-and-management-services-all-regions"></a>健全狀況和管理服務：所有區域

允許來自下列 IP 位址的流量，以進行 Azure HDInsight 健康狀態和管理服務（適用于所有 Azure 區域）：

| 來源 IP 位址 | Destination  | 方向 |
| ---- | ----- | ----- |
| 168.61.49.99 | \*：443 | 輸入 |
| 23.99.5.239 | \*：443 | 輸入 |
| 168.61.48.131 | \*：443 | 輸入 |
| 138.91.141.162 | \*：443 | 輸入 |

## <a name="health-and-management-services-specific-regions"></a>健全狀況和管理服務：特定區域

允許在資源所在的特定 Azure 區域中，針對 Azure HDInsight 健康狀態和管理服務列出的 IP 位址進行流量：

> [!IMPORTANT]  
> 如果未列出您使用的 Azure 區域，請使用網路安全性群組的[服務](hdinsight-service-tags.md)標籤功能。

| Country | 區域 | 允許的來源 IP 位址 | 允許的目的地 | 方向 |
| ---- | ---- | ---- | ---- | ----- |
| Asia | 東亞 | 23.102.235.122</br>52.175.38.134 | \*：443 | 輸入 |
| &nbsp; | 東南亞 | 13.76.245.160</br>13.76.136.249 | \*：443 | 輸入 |
| 澳大利亞 | 澳大利亞東部 | 104.210.84.115</br>13.75.152.195 | \*：443 | 輸入 |
| &nbsp; | 澳大利亞東南部 | 13.77.2.56</br>13.77.2.94 | \*：443 | 輸入 |
| 巴西 | 巴西南部 | 191.235.84.104</br>191.235.87.113 | \*：443 | 輸入 |
| Canada | 加拿大東部 | 52.229.127.96</br>52.229.123.172 | \*：443 | 輸入 |
| &nbsp; | 加拿大中部 | 52.228.37.66</br>52.228.45.222 |\*：443 | 輸入 |
| 中國 | 中國北部 | 42.159.96.170</br>139.217.2.219</br></br>42.159.198.178</br>42.159.234.157 | \*：443 | 輸入 |
| &nbsp; | 中國東部 | 42.159.198.178</br>42.159.234.157</br></br>42.159.96.170</br>139.217.2.219 | \*：443 | 輸入 |
| &nbsp; | 中國北部 2 | 40.73.37.141</br>40.73.38.172 | \*：443 | 輸入 |
| &nbsp; | 中國東部 2 | 139.217.227.106</br>139.217.228.187 | \*：443 | 輸入 |
| 歐洲 | 北歐 | 52.164.210.96</br>13.74.153.132 | \*：443 | 輸入 |
| &nbsp; | 西歐| 52.166.243.90</br>52.174.36.244 | \*：443 | 輸入 |
| 法國 | 法國中部| 20.188.39.64</br>40.89.157.135 | \*：443 | 輸入 |
| 德國 | 德國中部 | 51.4.146.68</br>51.4.146.80 | \*：443 | 輸入 |
| &nbsp; | 德國東北部 | 51.5.150.132</br>51.5.144.101 | \*：443 | 輸入 |
| 印度 | 印度中部 | 52.172.153.209</br>52.172.152.49 | \*：443 | 輸入 |
| &nbsp; | 印度南部 | 104.211.223.67<br/>104.211.216.210 | \*：443 | 輸入 |
| 日本 | 日本東部 | 13.78.125.90</br>13.78.89.60 | \*：443 | 輸入 |
| &nbsp; | 日本西部 | 40.74.125.69</br>138.91.29.150 | \*：443 | 輸入 |
| 南韓 | 南韓中部 | 52.231.39.142</br>52.231.36.209 | \*：443 | 輸入 |
| &nbsp; | 南韓南部 | 52.231.203.16</br>52.231.205.214 | \*：443 | 輸入
| United Kingdom | 英國西部 | 51.141.13.110</br>51.141.7.20 | \*：443 | 輸入 |
| &nbsp; | 英國南部 | 51.140.47.39</br>51.140.52.16 | \*：443 | 輸入 |
| 美國 | 美國中部 | 13.89.171.122</br>13.89.171.124 | \*：443 | 輸入 |
| &nbsp; | 美國東部 | 13.82.225.233</br>40.71.175.99 | \*：443 | 輸入 |
| &nbsp; | 美國中北部 | 157.56.8.38</br>157.55.213.99 | \*：443 | 輸入 |
| &nbsp; | 美國中西部 | 52.161.23.15</br>52.161.10.167 | \*：443 | 輸入 |
| &nbsp; | 美國西部 | 13.64.254.98</br>23.101.196.19 | \*：443 | 輸入 |
| &nbsp; | 美國西部 2 | 52.175.211.210</br>52.175.222.222 | \*：443 | 輸入 |
| &nbsp; | 阿拉伯聯合大公國北部 | 65.52.252.96</br>65.52.252.97 | \*：443 | 輸入 |

如需用於 Azure Government 之 IP 位址的資訊，請參閱 [Azure Government Intelligence + Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) 文件。

如需詳細資訊，請參閱[控制網路流量](hdinsight-plan-virtual-network-deployment.md#networktraffic)一節。

如果您使用的是使用者定義的路由（Udr），您應該指定路由，並允許從虛擬網路到上述 Ip 的輸出流量，並將下一個躍點設為「網際網路」。

## <a name="next-steps"></a>後續步驟

* [建立 Azure HDInsight 叢集的虛擬網路](hdinsight-create-virtual-network.md)
* [Azure HDInsight 的網路安全性群組（NSG）服務標記](hdinsight-service-tags.md)
