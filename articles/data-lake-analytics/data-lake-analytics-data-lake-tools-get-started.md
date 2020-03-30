---
title: 查詢 Azure 資料湖分析 - 視覺化工作室
description: 了解如何安裝適用於 Visual Studio 的 Data Lake 工具，如何開發和測試 U-SQL 指令碼。
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: dacce0d4f40f077b5da6221000192a4398da99e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260341"
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>使用適用於 Visual Studio 的 Data Lake 工具來開發 U-SQL 指令碼

[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure Data Lake 和串流分析工具包含兩項 Azure 服務、Azure Data Lake Analytics 和 Azure 串流分析相關的功能。 有關 Azure 流分析方案的詳細資訊，請參閱[視覺化工作室的 Azure 流分析工具](../stream-analytics/stream-analytics-tools-for-visual-studio-install.md)。

本文介紹如何使用視覺化工作室創建 Azure 資料湖分析帳戶。 您可以在[U-SQL](data-lake-analytics-u-sql-get-started.md)中定義作業，並將作業提交到資料湖分析服務。 有關資料湖分析的詳細資訊，請參閱[Azure 資料湖分析概述](data-lake-analytics-overview.md)。

> [!IMPORTANT]
> 我們建議您升級到 Azure 資料湖工具，適用于 Visual Studio 版本 2.3.3000.4 或更高版本。 舊版目前已淘汰，不再提供下載。
>
> 1. 請檢查您是否使用比 2.3.3000.4 版 Azure Data Lake Tools for Visual Studio 還要舊的版本。
>
>    ![檢查工具版本](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-about-data-lake.png)
>
> 1. 如果您的版本比 2.3.3000.4 版還要舊，請藉由造訪下載中心來更新您的 Azure Data Lake Tools for Visual Studio：
>    - [適用于視覺工作室 2017 和 2019](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools)
>    - [針對 Visual Studio 2013 和 2015](https://www.microsoft.com/en-us/download/details.aspx?id=49504)

## <a name="prerequisites"></a>Prerequisites

* **Visual Studio**：支援 Express 以外的所有版本。

  * Visual Studio 2019
  * Visual Studio 2017
  * Visual Studio 2015
  * Visual Studio 2013

* **Microsoft Azure SDK for .NET** 2.7.1 版或更新版本。 使用 [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) 來進行安裝。
* **資料湖分析**帳戶。 如需建立帳戶，請參閱[使用 Azure 入口網站開始使用 Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md)。

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>安裝 Azure Data Lake Tools for Visual Studio

要進行本教學課程，就必須安裝 Data Lake Tools for Visual Studio。 有關詳細資訊，請參閱[為視覺化工作室安裝資料湖工具](data-lake-analytics-data-lake-tools-install.md)。

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>連線至 Azure Data Lake Analytics 帳戶

1. 開啟 Visual Studio。

1. 通過選擇**視圖** > **伺服器資源管理器**打開**伺服器資源管理器**。

1. 按右鍵**Azure**，然後選擇"**連接到 Microsoft Azure 訂閱**"。 登錄**您的帳戶**，請按照說明操作。

1. 在**伺服器資源管理器中**，選擇**Azure** > **資料湖分析**。 您會看到 Data Lake Analytics 帳戶的清單。

## <a name="write-your-first-u-sql-script"></a>撰寫第一個 U-SQL 指令碼

下列文字是簡單的 U-SQL 指令碼。 它會定義小型資料集，並將該資料集寫入預設 Data Lake Store 中，作為名為 `/data.csv` 的檔案。

```sql
USE DATABASE master;
USE SCHEMA dbo;
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-data-lake-analytics-job"></a>提交資料湖分析作業

1. 在視覺化工作室中，選擇 **"檔** > **新專案** > **"。**

1. 選擇**U-SQL 專案**類型，然後選擇 **"下一步**"。 在 **"配置新專案"** 中，選擇 **"創建**"。

   Visual Studio 創建包含**腳本.usql**檔的解決方案。

1. 將第[一個 U-SQL 腳本](#write-your-first-u-sql-script)中的腳本粘貼到**腳本.usql**視窗中。

1. 在**解決方案資源管理器**中，按右鍵**腳本.usql，** 然後選擇 **"提交腳本**"。

1. 在 **"提交作業"** 中，選擇您的資料湖分析帳戶並選擇"**提交**"。

   ![提交 U-SQL Visual Studio 專案](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-submit-job-vs2019.png)

提交作業之後，[作業檢視]**** 索引標籤隨即開啟，並顯示作業進度。

* **作業摘要**會顯示作業的摘要。
* **作業圖形**會以視覺化方式檢視作業的進度。
* **中繼資料作業**會顯示 U-SQL 目錄上所執行的所有動作。
* **資料**會顯示所有的輸入和輸出。
* [狀態歷程記錄]**** 會顯示時間軸和狀態的詳細資料。
* **AU 分析**顯示作業中使用的 AU 數量，並探索不同 AU 分配策略的類比。
* **診斷**會提供作業執行和效能最佳化的進階分析。

![U SQL Visual Studio Data Lake Analytics 工作效能圖表](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

要查看最新的作業狀態並刷新螢幕，請選擇"**刷新**"。

## <a name="check-job-status"></a>檢查作業狀態

1. 在**伺服器資源管理器中**，選擇**Azure** > **資料湖分析**。

1. 展開 Data Lake Analytics 帳戶名稱。

1. 按兩下 [作業]****。

1. 選取您先前提交的作業。

## <a name="see-the-job-output"></a>查看作業輸出

1. 在**伺服器資源管理器中**，流覽到您提交的作業。

1. 按一下 **[資料]** 索引標籤。

1. 在 [作業輸出]**** 索引標籤中，選取 `"/data.csv"` 檔案。

## <a name="next-steps"></a>後續步驟

* [在您自己的工作站上執行 U-SQL 指令碼進行測試和偵錯](data-lake-analytics-data-lake-tools-local-run.md)
* [使用 Azure Data Lake Tools for Visual Studio Code 執行 U-SQL 作業的 C# 程式碼偵錯](data-lake-tools-for-vscode-local-run-and-debug.md)
* [使用 Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
