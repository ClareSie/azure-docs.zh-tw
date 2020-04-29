---
title: 使用外部中繼資料存放區 - Azure HDInsight
description: 搭配 Azure HDInsight 叢集使用外部中繼資料存放區。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/03/2020
ms.openlocfilehash: e53164d1e25f8a8d0a14d21c0544d95cf912fe9f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81313951"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>在 Azure HDInsight 中使用外部中繼資料存放區

HDInsight 可讓您利用外部資料存放區來控制您的資料和中繼資料。 這項功能適用于[Apache Hive 中繼存放區](#custom-metastore)、 [apache Oozie 中繼存放區](#apache-oozie-metastore)和[apache Ambari 資料庫](#custom-ambari-db)。

HDInsight 中的 Apache Hive 中繼存放區是 Apache Hadoop 架構不可或缺的一部分。 中繼存放區是中央架構存放庫。 中繼存放區是由其他海量資料存取工具（例如 Apache Spark、互動式查詢（LLAP）、Presto 或 Apache Pig）使用。 HDInsight 使用 Azure SQL Database 作為 Hive 中繼存放區。

![HDInsight Hive 中繼資料存放區架構](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

您有兩種方式可以為您的 HDInsight 叢集設定中繼存放區：

* [預設中繼存放區](#default-metastore)
* [自訂中繼存放區](#custom-metastore)

## <a name="default-metastore"></a>預設中繼存放區

根據預設，HDInsight 會以每個叢集類型來建立中繼存放區。 您可以改為指定自訂中繼存放區。 預設中繼存放區包含下列考量：

* 沒有其他成本。 HDInsight 會以每個叢集類型來建立中繼存放區，但不會對您產生額外成本。

* 每個預設中繼存放區是叢集生命週期的一部分。 當您刪除叢集時，相應的中繼存放區和中繼資料會一併刪除。

* 您無法與其他叢集共用預設中繼存放區。

* 預設中繼存放區會使用基本的 Azure SQL DB，它具有五個 DTU (資料庫交易單位) 限制。
此預設中繼存放區通常用於相對簡單的工作負載。 不需要多個叢集，且不需要保留超過叢集生命週期的中繼資料的工作負載。

## <a name="custom-metastore"></a>自訂中繼存放區

HDInsight 也支援自訂中繼存放區，這是針對生產叢集建議的中繼存放區：

* 您將自己的 Azure SQL Database 指定為中繼存放區。

* 中繼存放區的生命週期不會系結至叢集生命週期，因此您可以建立和刪除叢集，而不會遺失中繼資料。 即使您刪除並重新建立 HDInsight 叢集之後，中繼資料 (例如您的 Hive 結構描述) 仍會保存。

* 自訂中繼存放區可讓您將多個叢集與叢集類型連結至該中繼存放區。 例如，單一中繼存放區可以在 HDInsight 中的互動式查詢、Hive 和 Spark 叢集之間共用。

* 您需要根據您選擇的效能層級支付中繼存放區 (Azure SQL DB) 的成本。

* 您可以視需要相應增加中繼存放區。

* 叢集和外部中繼存放區必須裝載在相同的區域中。

![HDInsight Hive 中繼資料存放區使用案例](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

### <a name="create-and-config-azure-sql-database-for-the-custom-metastore"></a>建立和設定自訂中繼存放區的 Azure SQL Database

在設定 HDInsight 叢集的自訂 Hive 中繼存放區之前，建立或擁有現有的 Azure SQL Database。  如需詳細資訊，請參閱[快速入門：在 AZURE SQL DB 中建立單一資料庫](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started?tabs=azure-portal)。

建立叢集時，HDInsight 服務必須連接到外部中繼存放區並驗證您的認證。 設定 Azure SQL Database 防火牆規則，以允許 Azure 服務和資源存取伺服器。 選取 [**設定伺服器防火牆**]，在 Azure 入口網站中啟用此選項。 然後，在 [**拒絕公用網路存取**] 底下選取 [**否**]，然後在 [**允許 Azure 服務和資源存取此伺服器**以取得 Azure SQL Database 伺服器或資料庫 **]** 底下。 如需詳細資訊，請參閱[建立和管理 IP 防火牆規則](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#use-the-azure-portal-to-manage-server-level-ip-firewall-rules)

![[設定伺服器防火牆] 按鈕](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall1.png)

![允許 azure 服務存取](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall2.png)

### <a name="select-a-custom-metastore-during-cluster-creation"></a>在叢集建立期間選取自訂中繼存放區

您可以隨時將叢集指向先前建立的 Azure SQL Database。 若要透過入口網站建立叢集，請從**儲存體 > 中繼存放區設定**指定選項。

![HDInsight Hive 中繼資料存放區 Azure 入口網站](./media/hdinsight-use-external-metadata-stores/azure-portal-cluster-storage-metastore.png)

## <a name="hive-metastore-guidelines"></a>Hive 中繼存放區方針

* 盡可能使用自訂中繼存放區，這樣有助於個別計算資源 (您的執行中叢集) 和中繼資料 (儲存在中繼存放區)。

* 從 S2 層開始，該層提供 50 個 DTU 和 250 GB 的儲存體。 如果您看到瓶頸，您可以相應增加資料庫。

* 如果您想要讓多個 HDInsight 叢集存取不同的資料，請針對每個叢集上的中繼存放區使用不同資料庫。 如果您在多個 HDInsight 叢集間共用中繼存放區，則表示這些叢集會使用相同的中繼資料和基礎使用者資料檔案。

* 定期備份您的自訂中繼存放區。 Azure SQL Database 會自動產生備份，但備份保留時間範圍有所不同。 如需詳細資訊，請參閱[了解自動 SQL Database 備份](../sql-database/sql-database-automated-backups.md)。

* 在相同的區域中找出您的中繼存放區和 HDInsight 叢集。 此設定會提供最高效能和最低的網路輸出費用。

* 使用 Azure SQL Database 監視工具或 Azure 監視器記錄，監視中繼存放區的效能和可用性。

* 針對現有的自訂中繼存放區資料庫建立新的較高版本的 Azure HDInsight 時，系統會升級中繼存放區的架構。 升級無法復原，而不需要從備份還原資料庫。

* 如果您在多個叢集間共用中繼存放區，請確定所有叢集都是相同的 HDInsight 版本。 不同的 Hive 版本會使用不同的中繼存放區資料庫結構描述。 例如，您無法在 Hive 2.1 和 Hive 3.1 版本的叢集間共用中繼存放區。

* 在 HDInsight 4.0 中，Spark 和 Hive 會使用獨立目錄來存取 SparkSQL 或 Hive 資料表。 Spark 所建立的資料表存在於 Spark 目錄中。 Hive 所建立的資料表存在於 Hive 目錄中。 此行為不同于 HDInsight 3.6，其中 Hive 和 Spark 共用通用目錄。 HDInsight 4.0 中的 hive 和 Spark 整合依賴 Hive 倉儲連接器（HWC）。 HWC 可做為 Spark 與 Hive 之間的橋樑。 [瞭解 Hive 倉儲連接器](../hdinsight/interactive-query/apache-hive-warehouse-connector.md)。

## <a name="apache-oozie-metastore"></a>Apache Oozie 中繼存放區

Apache Oozie 是一個可管理 Hadoop 作業的工作流程協調系統。 Oozie 支援 Apache MapReduce、Pig、Hive 等等的 Hadoop 作業。  Oozie 會使用中繼存放區來儲存工作流程的相關詳細資料。 為提升使用 Oozie 時的效能，您可以使用 Azure SQL Database 作為自訂中繼存放區。 中繼存放區會在您刪除叢集之後，提供 Oozie 作業資料的存取權。

如需有關使用 Azure SQL Database 來建立 Oozie 中繼存放區的指示，請參閱[使用 Apache Oozie 來處理工作流程](hdinsight-use-oozie-linux-mac.md)。

## <a name="custom-ambari-db"></a>自訂 Ambari DB

若要使用您自己的外部資料庫搭配 Apache Ambari on HDInsight，請參閱[自訂 Apache Ambari 資料庫](hdinsight-custom-ambari-db.md)。

## <a name="next-steps"></a>後續步驟

* [使用 Apache Hadoop、Apache Spark、Apache Kafka 及其他工具在 HDInsight 中設定叢集](./hdinsight-hadoop-provision-linux-clusters.md)
