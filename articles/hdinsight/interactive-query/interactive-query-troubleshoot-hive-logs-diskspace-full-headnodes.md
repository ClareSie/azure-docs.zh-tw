---
title: Apache Hive 記錄填滿磁碟空間-Azure HDInsight
description: Apache Hive 記錄檔會填滿 Azure HDInsight 中前端節點的磁碟空間。
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 03/05/2020
ms.openlocfilehash: d843b942702d335065a5f3798572e34c71b4cd0e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "78943959"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>案例： Apache Hive 記錄檔會填滿前端節點上的磁碟空間 Azure HDInsight

本文針對 Azure HDInsight 叢集前端節點上沒有足夠磁碟空間的相關問題，說明其疑難排解步驟和可能的解決方式。

## <a name="issue"></a>問題

在 Apache Hive/LLAP 叢集上，不需要的記錄會佔用前端節點上的整個磁碟空間。 因此，可能會出現下列問題。

1. 因為前端節點上沒有空間，所以 SSH 存取失敗。
2. Ambari 提供*HTTP 錯誤：503服務無法使用*。

當`ambari-agent`問題發生時，記錄會顯示下列各項。
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>原因

在 advanced Hive-log4j 設定中，會省略參數*log4j. 附加器. RFA. MaxBackupIndex* 。 它會造成無止盡的記錄檔產生。

## <a name="resolution"></a>解決方案

1. 流覽至 Ambari 入口網站上的 [Hive 元件摘要`Configs` ]，然後按一下 [索引標籤]。

2. 移至 [ `Advanced hive-log4j`高級設定] 中的區段。

3. 將`log4j.appender.RFA`參數設定為 RollingFileAppender。 

4. 設定`log4j.appender.RFA.MaxFileSize` ， `log4j.appender.RFA.MaxBackupIndex`如下所示。

```
log4jhive.log.maxfilesize=1024MB
log4jhive.log.maxbackupindex=10

log4j.appender.RFA=org.apache.log4j.RollingFileAppender
log4j.appender.RFA.File=${hive.log.dir}/${hive.log.file}
log4j.appender.RFA.MaxFileSize=${log4jhive.log.maxfilesize}
log4j.appender.RFA.MaxBackupIndex=${log4jhive.log.maxbackupindex}
log4j.appender.RFA.layout=org.apache.log4j.PatternLayout
log4j.appender.RFA.layout.ConversionPattern=%d{ISO8601} %-5p [%t] %c{2}: %m%n
```
5. 將`hive.root.logger`設定`INFO,RFA`為，如下所示。 預設設定為 DEBUG，這會使記錄檔變得非常大。

```
# Define some default values that can be overridden by system properties
hive.log.threshold=ALL
hive.root.logger=INFO,RFA
hive.log.dir=${java.io.tmpdir}/${user.name}
hive.log.file=hive.log
```

6. 儲存並重新啟動必要的元件。

## <a name="next-steps"></a>後續步驟

如果您沒有看到您的問題，或無法解決您的問題，請瀏覽下列其中一個管道以取得更多支援：

* 透過[Azure 社區支援](https://azure.microsoft.com/support/community/)取得 azure 專家的解答。

* 連接[@AzureSupport](https://twitter.com/azuresupport) -官方 Microsoft Azure 帳戶，藉由將 Azure 社區連接至適當的資源來改善客戶體驗：解答、支援及專家。

* 如果您需要更多協助，您可以從[Azure 入口網站](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支援要求。 從功能表列選取 [**支援**]，或開啟 [說明 **+ 支援**] 中樞。 如需詳細資訊，請參閱[如何建立 Azure 支援要求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)。 您的 Microsoft Azure 訂用帳戶包含訂用帳戶管理和帳單支援的存取權，而技術支援則透過其中一項[Azure 支援方案](https://azure.microsoft.com/support/plans/)提供。
