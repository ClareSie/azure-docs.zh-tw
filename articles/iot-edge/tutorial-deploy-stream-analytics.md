---
title: 教學課程 - 使用 Azure IoT Edge 進行邊緣上的串流分析
description: 在本教學課程中，您會將 Azure 串流分析作為模組部署至 IoT Edge 裝置
author: kgremban
ms.author: kgremban
ms.date: 07/29/2020
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: c259e913a8ee5181bc58aea651af62324cf01fcb
ms.sourcegitcommit: 14bf4129a73de2b51a575c3a0a7a3b9c86387b2c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2020
ms.locfileid: "87439411"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module"></a>教學課程：將 Azure 串流分析部署為 IoT Edge 模組

許多 IoT 解決方案皆使用分析服務，來深入了解資料從 IoT 裝置送達雲端時的相關情形。 透過 Azure IoT Edge，您可以採取 [Azure 串流分析](https://docs.microsoft.com/azure/stream-analytics/)邏輯並將其運用於裝置本身。 藉由處理 Edge 上的遙測資料流，您將可減少上傳的資料量，並縮短將見解付諸行動的所需時間。

Azure IoT Edge 和 Azure 串流分析已整合，可簡化您的工作負載開發。 您可以在 Azure 入口網站中建立 Azure 串流分析作業，然後將其部署為 IoT Edge 模組 (不含額外程式碼)。  

Azure 串流分析針對雲端中或 IoT Edge 上的資料，提供了豐富結構化的查詢語法。 如需詳細資訊，請參閱 [Azure 串流分析文件](../stream-analytics/stream-analytics-edge.md)。

本教學課程中的串流分析模組會計算 30 秒滾動時段的平均溫度。 當平均溫度達到 70 度時，模組就會傳送警示，提醒裝置採取因應動作。 在此案例中，該動作為重設模擬溫度感應器。 在生產環境中，您可能會使用此功能用來關閉機器，或在溫度達到危險程度時採取預防措施。

在本教學課程中，您會了解如何：
> [!div class="checklist"]
>
> * 建立 Azure 串流分析作業以在邊緣上處理資料。
> * 將新的 Azure 串流分析作業與其他 IoT Edge 模組連結。
> * 從 Azure 入口網站將 Azure 串流分析作業部署至 IoT Edge 裝置。

<center>

![圖表 - 教學課程架構：階段和部署 ASA 作業](./media/tutorial-deploy-stream-analytics/asa-architecture.png)
</center>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>必要條件

Azure IoT Edge 裝置：

* 您可以遵循 [Linux](quickstart-linux.md) 或 [Windows 裝置](quickstart.md)快速入門中的步驟，使用 Azure 虛擬機器作為 IoT Edge 裝置。

雲端資源：

* Azure 中的免費或標準層 [IoT 中樞](../iot-hub/iot-hub-create-through-portal.md)。

## <a name="create-an-azure-stream-analytics-job"></a>建立 Azure 串流分析作業

在這一節中，您會建立可執行下列步驟的 Azure 串流分析作業：

* 從您的 IoT Edge 裝置接收資料。
* 查詢設定範圍外之值的遙測資料。
* 根據查詢結果對 IoT Edge 裝置採取動作。

### <a name="create-a-storage-account"></a>建立儲存體帳戶

當您建立要在 IoT Edge 裝置上執行的 Azure 串流分析作業時，該作業必須以可從裝置呼叫的方式儲存。 您可以使用現有的 Azure 儲存體帳戶，或於此時建立新帳戶。

1. 在 Azure 入口網站中，移至 [建立資源] > [儲存體] > [儲存體帳戶]。

1. 提供下列值以建立您的儲存體帳戶：

   | 欄位 | 值 |
   | ----- | ----- |
   | 訂用帳戶 | 選擇與您的 IoT 中樞相同的訂用帳戶。 |
   | 資源群組 | 建議您對 IoT Edge 快速入門和教學課程中的所有測試資源，使用相同的資源群組。 例如 **IoTEdgeResources**。 |
   | 名稱 | 提供儲存體帳戶的唯一名稱。 |
   | Location | 選擇接近您的位置。 |

1. 將其他欄位保留為預設值，然後選取 [檢閱 + 建立]。

1. 檢閱您的設定，然後選取 [建立]。

### <a name="create-a-new-job"></a>建立新作業

1. 在 Azure 入口網站中，移至 [建立資源] > [物聯網] > [串流分析作業]。

1. 提供下列值以建立您的作業：

   | 欄位 | 值 |
   | ----- | ----- |
   | 作業名稱 | 為您的作業提供名稱。 例如 **IoTEdgeJob** |
   | 訂用帳戶 | 選擇與您的 IoT 中樞相同的訂用帳戶。 |
   | 資源群組 | 建議您對於在 IoT Edge 快速入門和教學課程中建立的所有測試資源，使用相同的資源群組。 例如 **IoTEdgeResources**。 |
   | Location | 選擇接近您的位置。 |
   | 裝載環境 | 選取 [邊緣]。 |

1. 選取 [建立]。

### <a name="configure-your-job"></a>設定您的作業

在 Azure 入口網站中建立您的串流分析作業後，您可以為其設定輸入、輸出和查詢，以對通過的資料執行。

本節將使用輸入、輸出和查詢這三項元素，建立會從 IoT Edge 裝置接收溫度資料的作業。 它會以為期 30 秒的滾動時段分析該項資料。 如果該時段中的平均溫度超過 70 度，則會有警示傳送至 IoT Edge 裝置。 您在下一節中部署作業時，將會明確指定資料的來處和去向。  

1. 在 Azure 入口網站中瀏覽至您的串流分析作業。

1. 在 [作業拓撲] 下方選取 [輸入]，然後選取 [新增串流輸入]。

   ![Azure 串流分析 - 新增輸入](./media/tutorial-deploy-stream-analytics/asa-input.png)

1. 從下拉式清單中選擇 [Edge 中樞]。

1. 在 [新增輸入] 窗格中，輸入 [溫度] 作為輸入的別名。

1. 將其他欄位保留為預設值，然後選取 [儲存]。

1. 在 [作業拓撲] 下方開啟 [輸出]，然後選取 [新增]。

   ![Azure 串流分析 - 新增輸出](./media/tutorial-deploy-stream-analytics/asa-output.png)

1. 從下拉式清單中選擇 [Edge 中樞]。

1. 在 [新增輸出] 窗格中，輸入 [警示] 作為輸出別名。

1. 將其他欄位保留為預設值，然後選取 [儲存]。

1. 在 [作業拓撲] 下方，選取 [查詢]。

1. 將預設文字取代為下列查詢。 如果 30 秒時段中的機器平均溫度達到 70 度，SQL 程式碼即會將重設命令傳送至輸出警示。 重設命令已預先編寫到感應器的程式中，作為可採取的動作。

    ```sql
    SELECT  
        'reset' AS command
    INTO
       alert
    FROM
       temperature TIMESTAMP BY timeCreated
    GROUP BY TumblingWindow(second,30)
    HAVING Avg(machine.temperature) > 70
    ```

1. 選取 [儲存佇列]。

### <a name="configure-iot-edge-settings"></a>設定 IoT Edge 設定

若要準備將您的串流分析作業部署到 IoT Edge 裝置上，您必須將該作業與儲存體帳戶中的容器產生關聯。 當您部署作業時，作業定義即會匯出至儲存體容器。

1. 在 [設定] 底下選取 [儲存體帳戶設定]，然後選取 [新增儲存體帳戶]。

   ![Azure 串流分析 - 新增儲存體帳戶](./media/tutorial-deploy-stream-analytics/add-storage-account.png)

1. 從下拉式功能表中，選取您在本教學課程開頭所建立的**儲存體帳戶**。

1. 針對 [容器] 欄位選取 [新建]，並提供儲存體容器的名稱。

1. 選取 [儲存]。

## <a name="deploy-the-job"></a>部署作業

現在您已經準備好要在 IoT Edge 裝置上部署 Azure 串流分析作業。

在本節中，您會使用 Azure 入口網站中的 [設定模組] 精靈建立*部署資訊清單*。 部署資訊清單是一個 JSON 檔案，用以說明將部署到裝置的所有模組、用來儲存模組映像的容器登錄、管理模組的方式，以及模組彼此通訊的方式。 IoT Edge 裝置會從 IoT 中樞擷取其部署資訊清單，然後使用其中的資訊來部署和設定它所有已指派的模組。

在本教學課程中，您會部署兩個模組。 第一個是 **SimulatedTemperatureSensor**，這是模擬溫度和溼度感應器的模組。 第二個是您的串流分析作業。 感應器模組會提供您的作業查詢所將分析的資料流。

1. 在 Azure 入口網站中，瀏覽至您的 IoT 中樞。

1. 移至 [IoT Edge]，然後開啟 IoT Edge 裝置的詳細資料頁面。

1. 選取 [設定模組]。  

1. 如果您先前在此裝置上部署過 SimulatedTemperatureSensor 模組，它可能會自動填入。 若非如此，請以下列步驟新增模組：

   1. 按一下 [新增]，然後選取 [IoT Edge 模組]。
   1. 針對名稱，輸入 **SimulatedTemperatureSensor**。
   1. 針對映像 URI，輸入 **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0**。
   1. 其他設定保留不變，然後選取 [新增]。

1. 執行下列步驟，以新增 Azure 串流分析 Edge 作業：

   1. 按一下 [新增]，然後選取 [Azure 串流分析模組]。
   1. 選取訂用帳戶與您建立的 Azure 串流分析 Edge 作業。
   1. 選取 [儲存]。

   儲存變更之後，您串流分析作業的詳細資料就會發佈到您所建立的儲存體容器。

1. 當串流分析模組新增至模組清單時，請選取其名稱以查看其結構，並在 [更新 IoT Edge 模組] 頁面上更新其設定。

   [模組設定] 索引標籤具有指向標準 Azure 串流分析映像的**映像 URI**。 這個映像會用於部署至 IoT Edge 裝置的每個串流分析模組。

   [模組對應項設定] 索引標籤會顯示 JSON，這個 JSON 會定義名為 **ASAJobInfo** 的 Azure 串流分析 (ASA) 屬性。 該屬性的值會指向儲存體容器中的作業定義。 此屬性表示串流分析映像如何以您特定的作業詳細資料進行設定。

   根據預設，串流分析模組的名稱會與其所依據的作業相同。 您可以視需要變更此頁面上的模組名稱，但這不是必要的。

1. 選取 [更新] 或 [取消]。

1. 記下您的串流分析模組名稱 (因為您在下一個步驟將會用到此名稱)。 然後，選取 [下一步:路由] 以繼續。

1. 在 [路由] 索引標籤上，請定義要用來在模組與 IoT 中樞之間傳遞訊息的方式。 訊息會使用名稱/值組來構成。 以下表所示的配對來取代預設的 `route` 和 `upstream` 名稱和值，下列名稱/值組會以 Azure 串流分析模組的名稱取代 {moduleName} 的執行個體。

    | 名稱 | 值 |
    | --- | --- |
    | `telemetryToCloud` | `FROM /messages/modules/SimulatedTemperatureSensor/* INTO $upstream` |
    | `alertsToCloud` | `FROM /messages/modules/{moduleName}/* INTO $upstream` |
    | `alertsToReset` | `FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint("/modules/SimulatedTemperatureSensor/inputs/control")` |
    | `telemetryToAsa` | `FROM /messages/modules/SimulatedTemperatureSensor/* INTO BrokeredEndpoint("/modules/{moduleName}/inputs/temperature")`|

    您在此處宣告的路由，會定義通過 IoT Edge 裝置的資料流程。 來自 SimulatedTemperatureSensor 的遙測資料會傳送至 IoT 中樞，以及先前在串流分析作業中設定的**溫度**輸入。 **警示**輸出訊息會傳送至 IoT 中樞和 SimulatedTemperatureSensor 模組，以觸發重設命令。

1. 完成時，選取 [下一步:檢閱 + 建立]。

1. 在 [檢閱 + 建立] 索引標籤中，您可以看到您在精靈中提供的資訊如何轉換成 JSON 部署資訊清單。 檢閱資訊清單後，請選取 [建立]。

1. 您會回到 [裝置詳細資料] 頁面。 選取 [重新整理]。  

    您應該會看到新的串流分析模組正在與 IoT Edge 代理程式和 IoT Edge 中樞模組一起執行。 可能需要幾分鐘的時間，資訊才會到達您的 IoT Edge 裝置，然後才會啟動新的模組。 如果您沒有立即看到模組執行，請繼續重新整理頁面。

    ![依裝置報告的 SimulatedTemperatureSensor 和 ASA 模組](./media/tutorial-deploy-stream-analytics/module-output2.png)

## <a name="view-data"></a>檢視資料

現在您可以移至您的 IoT Edge 裝置，確認 Azure 串流分析模組和 SimulatedTemperatureSensor 模組之間的互動。

1. 檢查是否所有模組皆在 Docker 中執行：

   ```cmd/sh
   iotedge list  
   ```

1. 檢視所有系統記錄和計量資料。 使用串流分析模組名稱：

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

1. 藉由檢視感應器記錄，檢視影響 SimulatedTemperatureSensor 的重設命令：

   ```cmd/sh
   iotedge logs SimulatedTemperatureSensor
   ```

   您可以看到機器的溫度逐漸上升，直到在 30 秒內到達 70 度。 接著串流分析模組會觸發程序重設，且機器溫度降回為 21 度。

   ![將命令輸出重設到模組記錄中](./media/tutorial-deploy-stream-analytics/docker_log.png)

## <a name="clean-up-resources"></a>清除資源

如果您打算繼續閱讀下一篇建議的文章，則可以保留您所建立的資源和組態，並加以重複使用。 您可以也繼續使用相同的 IoT Edge 裝置作為測試裝置。

否則，可以刪除您在本文中使用的本機設定和 Azure 資源，以避免產生費用。

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>後續步驟

在本教學課程中，您設定了 Azure 串流分析作業，以分析來自 IoT Edge 裝置的資料。 接著，您載入將此 Azure 串流分析模組載入至 IoT Edge 裝置，以處理及因應本機的溫度上升，以及將彙總的資料流傳送至雲端。 若要進一步了解 Azure IoT Edge 可如何為您的企業建立解決方案，請繼續閱讀其他教學課程。

> [!div class="nextstepaction"]
> [將 Azure Machine Learning 模型部署為模組](tutorial-deploy-machine-learning.md)
