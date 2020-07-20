---
title: 彈性查詢總覽
description: 彈性查詢可讓您執行跨多個資料庫的 Transact-SQL 查詢。
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: sstein
ms.date: 12/05/2019
ms.openlocfilehash: 0497356e536632d179f4f64a6a9df8551a7a1897
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85557864"
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>Azure SQL Database 彈性查詢概觀 (預覽)
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

彈性查詢功能 (預覽) 可讓您在 Azure SQL Database 中執行跨多個資料庫的 Transact-SQL 查詢。 它可讓您執行跨資料庫查詢以存取遠端資料表，以及將 Microsoft 和第三方工具 (Excel、Power BI、Tableau 等) 連接到具有多個資料庫的資料層。 使用這項功能，您可以將查詢向外延展至大型資料層，並將商業智慧（BI）報表中的結果視覺化。

## <a name="why-use-elastic-queries"></a>為何要使用彈性查詢

### <a name="azure-sql-database"></a>Azure SQL Database

Azure SQL Database 完全在 T-sql 中跨資料庫進行查詢。 這可讓您進行遠端資料庫的唯讀查詢，並為目前的 SQL Server 客戶提供選項，以使用三個部分的名稱或連結的伺服器，將應用程式遷移至 SQL Database。

### <a name="available-on-standard-tier"></a>適用於標準層

標準和進階服務層級上都支援彈性查詢。 請參閱下面＜預覽限制＞一節中較低服務層級的效能限制。

### <a name="push-parameters-to-remote-databases"></a>將參數推送到遠端資料庫

彈性查詢現在可以將 SQL 參數發送至遠端資料庫以供執行。

### <a name="stored-procedure-execution"></a>預存程序執行

使用 [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) 執行遠端預存程序呼叫或遠端函式。

### <a name="flexibility"></a>彈性

具有彈性查詢的外部資料表可以參考具有不同結構描述或資料表名稱的遠端資料表。

## <a name="elastic-query-scenarios"></a>彈性查詢案例

目標是協助查詢案例便利進行，其中由多個資料庫提供資料列給單一整體結果。 查詢可以由使用者或應用程式直接撰寫，或透過連接到資料庫的工具來間接撰寫。 使用商業 BI 或資料整合工具 (或無法變更的任何應用程式) 建立報告時，這特別有用。 透過彈性查詢，您可以在 Excel、Power BI、Tableau 或 Cognos 等工具中使用熟悉的 SQL Server 連線體驗，以查詢多個資料庫。
彈性查詢可讓您透過 SQL Server Management Studio 或 Visual Studio 所發出的查詢，輕鬆存取整個資料庫集合，並協助更方便從 Entity Framework 或其他 ORM 環境執行跨資料庫查詢。 圖 1 顯示的案例中，現有的雲端應用程式 (使用 [彈性資料庫用戶端程式庫](elastic-database-client-library.md)) 根據相應放大的資料層建置，而彈性查詢用於跨資料庫報告。

**圖 1** 相應放大的資料層上使用的彈性查詢

![相應放大的資料層上使用的彈性查詢][1]

彈性查詢的客戶案例可依下列拓撲區分特性：

* **垂直資料分割 - 跨資料庫查詢** (拓撲 1)：資料會在資料層中的幾個資料庫之間以垂直方式分割。 一般而言，不同的資料表集位於不同的資料庫。 這表示不同資料庫的結構描述不同。 比方說，庫存的所有資料表都位於一個資料庫上，而所有會計相關資料表則位於另一個資料庫上。 此拓撲的常見使用案例會要求使用者跨多個資料庫中的資料表進行查詢或編譯報表。
* **水平資料分割 - 分區化** (拓撲 2)：資料會以水平方式分割，以將資料列分散在相應放大的資料層中。 使用此方法時，所有參與資料庫的結構描述都相同。 這個方法也稱為「分區化」。 使用 (1) 彈性資料庫工具程式庫或 (2) 自行分區化可以執行和管理分區化。 彈性查詢用於查詢或編譯跨多個分區的報表。 分區通常是彈性集區中的資料庫。 只要資料庫共用通用架構，您就可以將彈性查詢視為一次查詢彈性集區的所有資料庫的有效方式。

> [!NOTE]
> 彈性查詢最適合可在外部來源端執行大部分處理 (篩選、彙總) 的報告案例。 它不適用於有大量資料要從遠端資料庫傳輸的 ETL 作業。 針對繁重的報告工作負載或具有更複雜查詢的資料倉儲案例，也請考慮使用[Azure Synapse 分析](https://azure.microsoft.com/services/synapse-analytics)。
>  

## <a name="vertical-partitioning---cross-database-queries"></a>垂直資料分割 - 跨資料庫查詢

若要開始撰寫程式碼，請參閱 [開始使用跨資料庫查詢 (垂直資料分割)](elastic-query-getting-started-vertical.md)。

彈性查詢可用來將資料庫中的資料放在 SQL Database 可供 SQL Database 中的其他資料庫使用。 這可讓某個資料庫的查詢在 SQL Database 中參考任何其他遠端資料庫中的資料表。 第一個步驟是定義每個遠端資料庫的外部資料來源。 外部資料來源已定義於本機資料庫中，您想要從中取得遠端資料庫上資料表的存取權。 遠端資料庫不需要進行任何變更。 在不同資料庫有不同結構描述的典型垂直資料分割案例中，彈性查詢可用來實作常見使用案例，例如存取參考資料和跨資料庫查詢。

> [!IMPORTANT]
> 您必須具備 ALTER ANY EXTERNAL DATA SOURCE 權限。 這個權限包含在 ALTER DATABASE 權限中。 需有 ALTER ANY EXTERNAL DATA SOURCE 權限，才能參考基礎資料來源。
>

**參考資料**：拓撲是用來管理參考資料。 在下圖中，包含參考資料的兩個資料表 (T1 和 T2) 會保留在專用的資料庫上。 利用彈性查詢，您現在可以在遠端從其他資料庫存取資料表 T1 和 T2，如圖所示。 如果參考資料表很小或參考資料表的遠端查詢有選擇性述詞，則使用拓撲 1。

**圖 2** 垂直資料分割 - 使用彈性查詢來查詢參考資料

![ 垂直資料分割 - 使用彈性查詢來查詢參考資料][3]

**跨資料庫查詢**：彈性查詢可啟用需要在 SQL Database 中跨多個資料庫進行查詢的使用案例。 圖 3 顯示四個不同的資料庫：CRM、庫存、HR 和產品。 在其中一個資料庫中執行的查詢也需要存取另一個或其他所有資料庫。 利用彈性查詢，您可以在上述每個資料庫上執行一些簡單的 DDL 陳述式，針對此案例設定您的資料庫。 進行此一次性設定之後，存取遠端資料表就像從 T-SQL 查詢或從 BI 工具參考本機資料表一樣簡單。 如果遠端查詢未傳回大量結果，則建議使用這個方法。

**圖 3** 垂直資料分割 - 使用彈性查詢來查詢各種資料庫

![ 垂直資料分割 - 使用彈性查詢來查詢各種資料庫][4]

下列步驟會針對垂直資料分割案例設定彈性資料庫查詢，這些案例需要存取位於具有相同架構之 SQL Database 中遠端資料庫上的資料表：

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource (類型為 **RDBMS**
* [CREATE/DROP EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) mytable

執行 DDL 陳述式之後，您可以存取遠端資料表 "mytable"，就像存取本機資料表一樣。 Azure SQL Database 會自動開啟遠端資料庫的連線、處理您對遠端資料庫的要求，以及傳回結果。

## <a name="horizontal-partitioning---sharding"></a>水平資料分割 - 分區化

使用彈性查詢在分區化 (即水平分割) 的資料層執行報告工作時，需要[彈性資料庫分區對應](elastic-scale-shard-map-management.md)來代表資料層的資料庫。 一般而言，這種情節中只會使用單一分區對應，並以具有彈性查詢功能 (前端節點) 的專用資料庫作為報告查詢的進入點。 只有這個專用的資料庫需要存取分區對應。 圖 4 說明此拓撲及其彈性查詢資料庫和分區對應的組態。 如需有關彈性資料庫用戶端程式庫和建立分區對應的詳細資訊，請參閱 [分區對應管理](elastic-scale-shard-map-management.md)。

**圖 4** 水平資料分割 - 使用彈性查詢來報告分區化資料層

![ 水平資料分割 - 使用彈性查詢來報告分區化資料層][5]

> [!NOTE]
> 彈性查詢資料庫 (前端節點) 可以是個別的資料庫，或是裝載分區對應的相同資料庫。
> 無論您選擇哪一個設定，都應該確定該資料庫的服務層級和計算大小夠高，可處理預期的登入/查詢要求數量。

下列步驟會針對水準資料分割案例設定彈性資料庫查詢，這些案例需要存取（通常） SQL Database 中的數個遠端資料庫上的一組資料表：

* [CREATE MASTER KEY](https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql) mycredential
* 使用彈性資料庫用戶端程式庫，建立代表您的資料層的 [分區對應](elastic-scale-shard-map-management.md) 。
* [CREATE/DROP EXTERNAL DATA SOURCE](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql) mydatasource (類型為 **SHARD_MAP_MANAGER**)
* [CREATE/DROP EXTERNAL TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) mytable

執行這些步驟後，您即可存取水平分割的資料表 "mytable"，就像存取本機資料表一樣。 Azure SQL Database 會自動開啟遠端資料庫 (實際儲存資料表的位置) 的多個平行連線、處理對於遠端資料庫的要求，以及傳回結果。
如需水平資料分割案例所需步驟的詳細資訊，請參閱 [水平資料分割的彈性查詢](elastic-query-horizontal-partitioning.md)。

若要開始撰寫程式碼，請參閱[開始使用彈性查詢進行水平資料分割 (分區化)](elastic-query-getting-started.md)。

> [!IMPORTANT]
> 在大量資料庫上成功執行彈性查詢，會在查詢執行期間，高度依賴每個資料庫的可用性。 如果其中一個資料庫無法使用，整個查詢將會失敗。 如果您打算一次查詢數百個或數千個資料庫，請確定您的用戶端應用程式已內嵌重試邏輯，或考慮利用[彈性資料庫作業](https://docs.microsoft.com/azure/sql-database/sql-database-job-automation-overview#elastic-database-jobs-preview)（預覽）並查詢較小的資料庫子集，並將每個查詢的結果合併成單一目的地。

## <a name="t-sql-querying"></a>T-SQL 查詢

一旦您已定義外部資料來源和外部資料表，您可以使用一般 SQL Server 連接字串來連接到您定義外部資料表的資料庫。 您可以接著對該連線上的外部資料表執行 T-SQL 陳述式，其限制如下所述。 您可以[水平資料分割](elastic-query-horizontal-partitioning.md)和[垂直資料分割](elastic-query-vertical-partitioning.md)文件主題中找到 T-SQL 查詢範例的詳細資訊。

## <a name="connectivity-for-tools"></a>工具的連線能力

您可以使用一般 SQL Server 連接字串，將您的應用程式、BI 或資料整合工具連接到具有外部資料表的資料庫。 請確定 SQL Server 可支援做為您的工具的資料來源。 連線之後，請參考彈性查詢資料庫和該資料庫中的外部資料表，就如同您會使用您的工具連接的任何其他 SQL Server 資料庫一樣。

> [!IMPORTANT]
> 目前不支援使用 Azure Active Directory 與彈性查詢進行驗證。

## <a name="cost"></a>成本

彈性查詢包含在 Azure SQL Database 的成本中。 請注意，支援遠端資料庫與彈性查詢端點位於不同資料中心的拓撲，但從遠端資料庫輸出的資料以一般 [Azure 費率](https://azure.microsoft.com/pricing/details/data-transfers/)收費。

## <a name="preview-limitations"></a>預覽限制

* 在標準服務層級上執行第一個彈性查詢最多可能需要幾分鐘的時間。 需要這些時間才能載入彈性查詢功能；較高的服務層級和計算大小可改善載入效能。
* 尚未支援來自 SSMS 或 SSDT 的外部資料來源或外部資料表的指令碼。
* SQL Database 的匯入/匯出尚未支援外部資料源和外部資料表。 如果您需要使用匯入/匯出，請在匯出前卸除這些物件，然後在匯入後予以重新建立。
* 彈性查詢目前僅支援以唯讀方式存取外部資料表。 不過，您可以在定義外部資料表的資料庫上使用完整的 T-SQL 功能。 例如，在使用 SELECT <column_list> INTO <local_table> (舉例而言) 保存暫存結果時，或在彈性查詢資料庫上定義預存程序來參考外部資料表時，這可能有其效用。
* 除了 Nvarchar （max）以外，外部資料表定義中不支援 LOB 類型（包括空間類型）。 若要解決此問題，您可以在將 LOB 類型轉型成 nvarchar (max) 的遠端資料庫上建立檢視表、透過此檢視表而非基底資料表定義外部資料表，然後在查詢中將它轉換回原始的 LOB 類型。
* 結果集中屬於 nvarchar(max) 資料類型的資料行會停用彈性查詢實作中使用的批次處理術語，且可能對查詢造成一個數量級的影響，或甚至在要將大量非彙總的資料視為查詢結果進行傳輸的非標準使用案例中，造成兩個數量級的影響。
* 目前不支援外部資料表的資料行統計資料。 支援資料表統計資料，但必須以手動方式建立。
* 彈性查詢僅適用于 Azure SQL Database。 您無法使用它來查詢 SQL Server 實例。

## <a name="share-your-feedback"></a>分享您的意見反應

請在以下MSDN 論壇或 Stack Overflow 上，與我們分享您在彈性查詢體驗方面的意見。 我們很樂意接受關於服務的各種意見 (缺失、不完善、功能落差)。

## <a name="next-steps"></a>後續步驟

* 若要開始撰寫程式碼，請參閱 [開始使用跨資料庫查詢 (垂直資料分割)](elastic-query-getting-started-vertical.md)。
* 如需垂直資料分割之資料的語法和範例查詢，請參閱[查詢垂直資料分割的資料](elastic-query-vertical-partitioning.md)
* 如需水平資料分割 (分區化) 教學課程，請參閱[開始使用彈性查詢進行水平資料分割 (分區化)](elastic-query-getting-started.md)。
* 如需水平資料分割之資料的語法和範例查詢，請參閱[查詢水平資料分割的資料](elastic-query-horizontal-partitioning.md)
* 如需會在單一遠端 Azure SQL Database 或一組在水平資料分割配置中作為分區之資料庫上執行 Transact-SQL 陳述式的預存程序，請參閱 [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714)。

<!--Image references-->
[1]: ./media/elastic-query-overview/overview.png
[2]: ./media/elastic-query-overview/topology1.png
[3]: ./media/elastic-query-overview/vertpartrrefdata.png
[4]: ./media/elastic-query-overview/verticalpartitioning.png
[5]: ./media/elastic-query-overview/horizontalpartitioning.png

<!--anchors-->