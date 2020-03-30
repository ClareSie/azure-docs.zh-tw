---
title: 使用 Power BI 將 Apache Hive 資料視覺化 - Azure HDInsight
description: 了解如何使用 Microsoft Power BI 將 Azure HDInsight 所處理的 Hive 資料視覺化。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/02/2020
ms.openlocfilehash: d9b64785dbd82842479eb3f313b8394f9f25b40b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79369993"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>在 Azure HDInsight 中使用 ODBC 藉由 Microsoft Power BI 將 Apache Hive 資料視覺化

瞭解如何使用 ODBC 將 Microsoft Power BI 桌面連接到 Azure HDInsight 並視覺化 Apache Hive 資料。

> [!IMPORTANT]
> 您可以利用 Hive ODBC 驅動程式，透過 Power BI Desktop 中的一般 ODBC 連接器執行匯入作業。 不過，對於 BI 工作負載則不建議使用此方式，因為 Hive 查詢引擎具有非互動式的本質。 為發揮其效能，[HDInsight 互動式查詢連接器](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)和 [HDInsight Spark 連接器](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect)會是較好的選擇。

在本文中，您將資料從`hivesampletable`Hive 表載入到 Power BI。 Hive 資料表包含某些行動電話使用量資料。 然後您可在世界地圖上繪製使用量資料：

![HDInsight Power BI 地圖報告](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

此資訊也適用於新的[互動式查詢](../interactive-query/apache-interactive-query-get-started.md)叢集類型。 若要了解如何使用直接查詢來連線到 HDInsight 互動式查詢，請參閱[在 Azure HDInsight 中使用直接查詢藉由 Microsoft Power BI 將互動式查詢 Hive 資料視覺化](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)。

## <a name="prerequisites"></a>Prerequisites

在閱讀本文之前，您必須有下列各項：

* HDInsight 叢集。 此叢集可以是含有 Hive 的 HDInsight 叢集或新發行的互動式查詢叢集。 如需建立叢集，請參閱[建立叢集](apache-hadoop-linux-tutorial-get-started.md)。

* [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)。 您可以從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=45331)下載一份。

## <a name="create-hive-odbc-data-source"></a>建立 Hive ODBC 資料來源

請參閱[建立 Hive ODBC 資料來源](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source)。

## <a name="load-data-from-hdinsight"></a>從 HDInsight 載入資料

**蜂巢表**附帶所有 HDInsight 群集。

1. 啟動 Power BI Desktop。

1. 從頂部功能表，導航到**主頁** > **獲取資料** > **更多...**

    ![HDInsight Excel 電源 BI 開放資料](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)

1. 從 **"獲取資料"** 對話方塊中，從左側選擇 **"其他**"，從右側選擇**ODBC，** 然後在底部選擇 **"連接**"。

1. 從 **"從 ODBC"** 對話方塊中，從下拉清單中選擇您在最後一節中創建的資料來源名稱。 然後選擇 **"確定**"。

1. 對於首次使用，將打開**一個 ODBC 驅動程式**對話方塊。 從左側功能表中選擇 **"預設"或"自訂**"。 然後選擇 **"連接到**打開**的導航器**"。

1. 從 **"導航器"** 對話方塊中，展開**ODBC > HIVE >預設**，選擇**hive 樣板**，然後選擇 **"載入**"。

## <a name="visualize-data"></a>顯現資料

從上一個程序繼續進行。

1. 在"視覺化"窗格中，選擇 **"地圖**"，它是一個地球圖示。

    ![HDInsight Power BI 自訂報告](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)

1. 從 **"欄位"** 窗格中，選擇**國家/地區****和設備製造**。 您可以看到地圖上繪製的資料。

1. 展開地圖。

## <a name="next-steps"></a>後續步驟

在本文中，您已了解如何使用 Power BI 將 HDInsight 中的資料視覺化。  如需詳細資訊，請參閱下列文章：

* [使用 Apache Zepelin 在 Azure HDInsight 中運行 Apache Hive 查詢](../interactive-query/hdinsight-connect-hive-zeppelin.md)。
* [使用 Microsoft Hive ODBC 驅動程式將 Excel 連線到 HDInsight](./apache-hadoop-connect-excel-hive-odbc-driver.md)。
* [使用 Power Query 將 Excel 連線到 Apache Hadoop](apache-hadoop-connect-excel-power-query.md)。
* [使用 Data Lake Tools for Visual Studio 連線至 Azure HDInsight 及執行 Apache Hive 查詢](apache-hadoop-visual-studio-tools-get-started.md)。
* [使用 Azure HDInsight 工具進行視覺化工作室代碼](../hdinsight-for-vscode.md)。
* [將資料上傳至 HDInsight](./../hdinsight-upload-data.md)。
