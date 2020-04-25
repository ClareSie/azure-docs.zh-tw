---
title: Apache ZooKeeper server 無法在 Azure HDInsight 中形成仲裁
description: Apache ZooKeeper server 無法在 Azure HDInsight 中形成仲裁
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/20/2019
ms.openlocfilehash: 41ac109e5c5379e6085dd57a3fcd8119915558fb
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133279"
---
# <a name="apache-zookeeper-server-fails-to-form-a-quorum-in-azure-hdinsight"></a>Apache ZooKeeper server 無法在 Azure HDInsight 中形成仲裁

本文說明與 Azure HDInsight 叢集互動時，問題的疑難排解步驟和可能的解決方法。

## <a name="issue"></a>問題

Apache ZooKeeper 伺服器狀況不良，徵兆可能包括：資源管理員/名稱節點處於待命模式、簡單 HDFS 作業無法運作、 `zkFailoverController`已停止且無法啟動、Yarn/Spark/Livy 作業因 ZooKeeper 錯誤而失敗。 LLAP 守護程式在安全 Spark 或互動式 Hive 叢集上可能也無法啟動。 您可能會看到類似下列的錯誤訊息：

```
19/06/19 08:27:08 ERROR ZooKeeperStateStore: Fatal Zookeeper error. Shutting down Livy server.
19/06/19 08:27:08 INFO LivyServer: Shutting down Livy server.
```

在 Zookeeper 伺服器登入/var/log/zookeeper/zookeeper-zookeeper-server-\*的任何 Zookeeper 主機時，您可能也會看到下列錯誤：

```
2020-02-12 00:31:52,513 - ERROR [CommitProcessor:1:NIOServerCnxn@178] - Unexpected Exception:
java.nio.channels.CancelledKeyException
```

## <a name="cause"></a>原因

當快照集檔案的數量很大或快照集檔案損毀時，ZooKeeper 伺服器將無法形成仲裁，這會導致 ZooKeeper 相關的服務狀況不良。 ZooKeeper 伺服器不會從其資料目錄中移除舊的快照集檔案，而是定期執行的工作，供使用者維護 ZooKeeper 的資料軌。 如需詳細資訊，請參閱[ZooKeeper 的優點與限制](https://zookeeper.apache.org/doc/r3.3.5/zookeeperAdmin.html#sc_strengthsAndLimitations)。

## <a name="resolution"></a>解決方法

檢查 ZooKeeper 資料目錄`/hadoop/zookeeper/version-2` ， `/hadoop/hdinsight-zookeeper/version-2`並找出快照集檔案大小是否龐大。 如果有大型快照集，請採取下列步驟：

1. 請檢查相同仲裁中其他 ZooKeeper 伺服器的狀態，以確定它們可與命令 "`echo stat | nc {zk_host_ip} 2181 (or 2182)`" 正常運作。  

1. 在和`/hadoop/zookeeper/version-2` `/hadoop/hdinsight-zookeeper/version-2`中，登入有問題的 ZooKeeper 主機、備份快照集和交易記錄檔，然後在兩個目錄中清除這些檔案。 

1. 在 Ambari 或 ZooKeeper 主機中重新開機有問題的 ZooKeeper 伺服器。 然後重新開機有問題的服務。

## <a name="next-steps"></a>後續步驟

如果您沒有看到您的問題，或無法解決您的問題，請瀏覽下列其中一個管道以取得更多支援：

- 透過[Azure 社區支援](https://azure.microsoft.com/support/community/)取得 azure 專家的解答。

- 連接[@AzureSupport](https://twitter.com/azuresupport) -官方 Microsoft Azure 帳戶，以改善客戶體驗。 將 Azure 社區連接到正確的資源：解答、支援和專家。

- 如果您需要更多協助，您可以從[Azure 入口網站](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支援要求。 從功能表列選取 [**支援**]，或開啟 [說明 **+ 支援**] 中樞。 如需詳細資訊，請參閱[如何建立 Azure 支援要求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)。 您的 Microsoft Azure 訂用帳戶包含訂用帳戶管理和帳單支援的存取權，而技術支援則透過其中一項[Azure 支援方案](https://azure.microsoft.com/support/plans/)提供。
