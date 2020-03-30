---
title: Apache Kafka 提高級別 - Azure HDInsight
description: 了解如何設定在 Azure HDInsight 上 Apache Kafka 叢集的受控磁碟來提高延展性。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/09/2019
ms.openlocfilehash: 56c25b7c77809a5cb7f4e539cff8e1815cd9976f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "77031695"
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>在 HDInsight 上設定 Apache Kafka 的儲存體和延展性

瞭解如何配置[Apache Kafka](https://kafka.apache.org/)在 HDInsight 上使用的託管磁片數。

HDInsight 上的 Kafka 會在 HDInsight 叢集中使用虛擬機器的本機磁碟。 由於 Kafka 的 I/O 非常大量，因此會使用 [Azure 受控磁碟](../../virtual-machines/windows/managed-disks-overview.md)來提供高輸送量，並提供每個節點更多儲存空間。 如果將傳統的虛擬硬碟 (VHD) 用於 Kafka，每個節點就會限制為 1 TB。 使用受控磁碟時，您可以利用多個磁碟在叢集中的每個節點達到 16 TB。

下圖提供 HDInsight 上的 Kafka 採用受控磁碟之前與 HDInsight 上的 Kafka 採用受控磁碟之間的比較：

![卡夫卡具有託管磁片架構](./media/apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>設定受控磁碟：Azure 入口網站

1. 請遵循[建立 HDInsight 叢集](../hdinsight-hadoop-create-linux-clusters-portal.md)中的步驟，了解使用入口網站建立叢集的一般步驟。 不要完成門戶創建過程。

2. 在 **"配置&定價**"部分中，使用 __"節點數"__ 欄位配置磁片數。

    > [!NOTE]  
    > 受控磁碟的類型可以是__標準__ (HDD) 或__進階__ (SSD)。 進階磁碟會與 DS 和 GS 系列搭配使用。 所有其他的 VM 類型是使用標準磁碟。

    ![每個輔助節點突出顯示磁片的群集大小部分](./media/apache-kafka-scalability/azure-portal-cluster-configuration-pricing-kafka-disks.png)

## <a name="configure-managed-disks-resource-manager-template"></a>設定受控磁碟：Resource Manager 範本

若要控制背景工作角色節點在 Kafka 叢集中所使用的磁碟數目，請使用下列區段的範本：

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

您可以找到一個完整的範本，演示如何在 配置[https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json)託管磁片。

## <a name="next-steps"></a>後續步驟

如需使用 HDInsight 上 Apache Kafka 的詳細資訊，請參閱下列文件：

* [使用 MirrorMaker 在 HDInsight 上建立 Apache Kafka 複本](apache-kafka-mirroring.md)
* [在 HDInsight 上搭配使用 Apache Storm 與 Apache Kafka](../hdinsight-apache-storm-with-kafka.md)
* [在 HDInsight 上使用阿帕奇火花與阿帕奇卡夫卡](../hdinsight-apache-spark-with-kafka.md)
* [透過 Azure 虛擬網路連線到 Apache Kafka](apache-kafka-connect-vpn-gateway.md)

* [使用 Apache Kafka 之受控磁碟的 HDInsight 部落格](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)
