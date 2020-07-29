---
title: Azure IoT 中樞調整 | Microsoft Docs
description: 如何調整 IoT 中樞以支援您預期的訊息輸送量和所需功能。 包含每個層級支援的輸送量和分區化選項的摘要。
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: wesmc
ms.custom:
- amqp
- mqtt
- 'Role: Cloud Development'
- 'Role: Operations'
ms.openlocfilehash: bfd4f783c250a7bf0ec28cff903e4c59d8570d89
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87307517"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>選擇適合您解決方案的 IoT 中樞層

IoT 解決方案各不相同，因此 Azure IoT 中樞提供了幾個以定價和級別為基礎的選項。 本文旨在協助您評估 IoT 中樞需求。 如需有關 IoT 中樞層的定價資訊，請參閱 [IoT 中樞定價](https://azure.microsoft.com/pricing/details/iot-hub)。

若要決定適合您解決方案的 IoT 中樞層，請問自己兩個問題：

**我打算使用哪些功能？**

Azure IoT 中樞提供基本和標準兩個層級，兩者所支援的功能數目不同。 如果您的 IoT 解決方案是以收集資料裝置再集中分析為基礎，則基本層可能最適合您。 如果您想要使用更進階的組態，以便從遠端控制 IoT 裝置或將您的某些工作負載分散到各個裝置本身，則請考慮使用標準層。 如需這兩個層級各自包含之功能的詳細明細，請繼續閱讀[基本和標準層](#basic-and-standard-tiers)。

**我打算每日移動多少資料？**

每個 IoT 中樞層均提供三種大小，並以在任何指定日所能處理的資料輸送量為基礎。 這三種大小會加上數字 1、2 和 3 以供識別。 例如，每單位的層級 1 IoT 中樞一天可以處理 40 萬個訊息，每單位的層級 3 則可處理 3 億個。 如需資料指引的詳細資訊，請繼續閱讀[訊息輸送量](#message-throughput)。

## <a name="basic-and-standard-tiers"></a>基本和標準層

標準層的 IoT 中樞可啟用所有功能，想要使用雙向通訊功能的 IoT 解決方案都必須使用此層級。 基本層則可啟用一小部分功能，可供只需要從裝置到雲端進行單向通訊的 IoT 解決方案使用。 這兩個層級均提供相同的安全性和驗證功能。

每個 IoT 中樞只能選擇層次內的一個[版本](https://azure.microsoft.com/pricing/details/iot-hub/)類型。 例如，所建立的 IoT 中樞可具有多個 S1 單位，但不能具有來自不同版本 (例如 S1 和 S2) 的混合單位。

| 功能 | 基本層 | 免費/標準層 |
| ---------- | ---------- | ------------- |
| [裝置到雲端的遙測](iot-hub-devguide-messaging.md) | 是 | 是 |
| [每部裝置身分識別](iot-hub-devguide-identity-registry.md) | 是 | 是 |
| [訊息路由](iot-hub-devguide-messages-read-custom.md)、[訊息擴充](iot-hub-message-enrichments-overview.md)和[事件方格整合](iot-hub-event-grid.md) | 是 | 是 |
| [HTTP、AMQP 和 MQTT 通訊協定](iot-hub-devguide-protocols.md) | 是 | 是 |
| [裝置佈建服務](../iot-dps/about-iot-dps.md) | 是 | 是 |
| [監視和診斷](iot-hub-monitor-resource-health.md) | 是 | 是 |
| [雲端到裝置傳訊](iot-hub-devguide-c2d-guidance.md) |   | 是 |
| [裝置對應項](iot-hub-devguide-device-twins.md)、[模組對應項](iot-hub-devguide-module-twins.md)和[裝置管理](iot-hub-device-management-overview.md) |   | 是 |
| [裝置串流 (預覽)](iot-hub-device-streams-overview.md) |   | 是 |
| [Azure IoT Edge](../iot-edge/about-iot-edge.md) |   | 是 |
| [IoT 隨插即用預覽版](../iot-pnp/overview-iot-plug-and-play.md) |   | 是 |

IoT 中樞也會提供免費層供您測試和評估。 其具有標準層的所有功能，但允許的傳訊量有限。 您無法從免費層升級至基本或標準層。

## <a name="partitions"></a>資料分割

Azure IoT 中樞包含 [Azure 事件中樞](../event-hubs/event-hubs-features.md)的多個核心元件，包括[分割區](../event-hubs/event-hubs-features.md#partitions)。 IoT 中樞的事件資料流通常會填入由各種不同 IoT 裝置報告的內送遙測資料。 事件資料流的分割可用來減少同時讀取和寫入事件資料流時所發生的爭用。

分割區限制是在 IoT 中樞建立時所選擇的，無法變更。 基本層 IoT 中樞和標準層 IoT 中樞的分割區上限為 32。 大部分的 IoT 中樞只需要 4 個分割區。 如需關於決定分割區的詳細資訊，請參閱事件中樞的常見問題集：[我需要多少個分割區？](../event-hubs/event-hubs-faq.md#how-many-partitions-do-i-need)

## <a name="tier-upgrade"></a>層級升級

在建立 IoT 中樞後，您不需要中斷現有作業就可以從基本層升級到標準層。 如需詳細資訊，請參閱[如何升級 IoT 中樞](iot-hub-upgrade.md)。

當您從基本層移轉至標準層時，分割區組態會保持不變。

> [!NOTE]
> 免費層不支援升級至基本或標準層。

## <a name="iot-hub-rest-apis"></a>IoT 中樞 REST API

IoT 中樞的基本和標準層之間的支援功能差異，代表某些 API 呼叫無法與基本層中樞搭配運作。 下表顯示可用的 API：

| API | 基本層 | 免費/標準層 |
| --- | ---------- | ------------- |
| [刪除裝置](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/deletedevice) | 是 | 是 |
| [取得裝置](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/getdevice) | 是 | 是 |
| [刪除模組](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/deletemodule) (機器翻譯) | 是 | 是 |
| [取得模組](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/getmodule) (機器翻譯) | 是 | 是 |
| [取得登錄統計資料](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/getdevicestatistics) | 是 | 是 |
| [取得服務統計資料](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/getservicestatistics) | 是 | 是 |
| [建立或更新裝置](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/createorupdatedevice) (機器翻譯) | 是 | 是 |
| [建立或更新模組](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/createorupdatemodule) (機器翻譯) | 是 | 是 |
| [查詢 IoT 中樞](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/queryiothub) | 是 | 是 |
| [建立檔案上傳 SAS URI](https://docs.microsoft.com/rest/api/iothub/device/createfileuploadsasuri) | 是 | 是 |
| [接收裝置繫結通知](https://docs.microsoft.com/rest/api/iothub/device/receivedeviceboundnotification) | 是 | 是 |
| [傳送裝置事件](https://docs.microsoft.com/rest/api/iothub/device/senddeviceevent) | 是 | 是 |
| 傳送模組事件 | 僅限 AMQP 和 MQTT | 僅限 AMQP 和 MQTT |
| [更新檔案上傳狀態](https://docs.microsoft.com/rest/api/iothub/device/updatefileuploadstatus) | 是 | 是 |
| [大量裝置作業](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/bulkdevicecrud) | 是，IoT Edge 功能除外 | 是 |
| [取消匯入匯出作業](https://docs.microsoft.com/rest/api/iothub/service/jobclient/cancelimportexportjob) (機器翻譯) | 是 | 是 |
| [建立匯入匯出作業](https://docs.microsoft.com/rest/api/iothub/service/jobclient/createimportexportjob) (機器翻譯) | 是 | 是 |
| [取得匯入匯出作業](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjob) | 是 | 是 |
| [取得匯入匯出作業](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjobs) (機器翻譯) | 是 | 是 |
| [清除命令佇列](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/purgecommandqueue) |   | 是 |
| [取得裝置對應項](https://docs.microsoft.com/rest/api/iothub/service/twin/getdevicetwin) |   | 是 |
| [取得模組對應項](https://docs.microsoft.com/rest/api/iothub/service/twin/getmoduletwin) (機器翻譯) |   | 是 |
| [叫用裝置方法](https://docs.microsoft.com/rest/api/iothub/service/devicemethod/invokedevicemethod) |   | 是 |
| [更新裝置對應項](https://docs.microsoft.com/rest/api/iothub/service/twin/updatedevicetwin) |   | 是 |
| [更新模組對應項](https://docs.microsoft.com/rest/api/iothub/service/twin/updatemoduletwin) (機器翻譯) |   | 是 |
| [放棄裝置繫結通知](https://docs.microsoft.com/rest/api/iothub/device/abandondeviceboundnotification) |   | 是 |
| [完成裝置繫結通知](https://docs.microsoft.com/rest/api/iothub/device/completedeviceboundnotification) |   | 是 |
| [取消作業](https://docs.microsoft.com/rest/api/iothub/service/jobclient/canceljob) |   | 是 |
| [建立作業](https://docs.microsoft.com/rest/api/iothub/service/jobclient/createjob) |   | 是 |
| [取得作業](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getjob) |   | 是 |
| [查詢作業](https://docs.microsoft.com/rest/api/iothub/service/jobclient/queryjobs) |   | 是 |

## <a name="message-throughput"></a>訊息輸送量

若要評估每個單位的流量，最佳方式為調整 IoT 中樞解決方案的大小。 尤其要考慮以下類別的作業所需的尖峰輸送量：

* 裝置到雲端的訊息
* 雲端到裝置的訊息
* 身分識別登錄作業

IoT 中樞流量的測量是以單位為基礎。 當建立 IoT 中樞時，請選擇其階層和版本，並設定可用的單位數。 您最多可針對 B1、B2、S1 或 S2 版本購買 200 個單位，或針對 B3 或 S3 版本購買 10 個單位。 建立 IoT 中樞之後，即可變更其版本內可用的單位數、在其階層內的版本之間升級或降級 (B1 到 B2)，或從基本層升級至標準層 (B1 到 S1)，而不需要中斷現有的作業。 如需詳細資訊，請參閱[如何升級 IoT 中樞](iot-hub-upgrade.md)。  

以每一層的流量功能為例，裝置到雲端訊息會遵循下列持續輸送量指引：

| 階層版本 | 持續的輸送量 | 持續的傳送速率 |
| --- | --- | --- |
| B1, S1 |每個單位最多 1111 KB/分鐘<br/>(1.5 GB/天/單位) |每個單位平均 278 個訊息/分鐘<br/>(400,000 個訊息/天/單位) |
| B2, S2 |每個單位最多 16 MB/分鐘<br/>(22.8 GB/天/單位) |每個單位平均 4,167 個訊息/分鐘<br/>(600 萬個訊息/天/單位) |
| B3, S3 |每個單位最多 814 MB/分鐘<br/>(1144.4 GB/天/單位) |每個單位平均 208,333 個訊息/分鐘<br/>(3 億個訊息/天/單位) |

裝置到雲端輸送量只是在設計 IoT 解決方案時必須考慮的其中一個計量。 如需更完整的資訊，請參閱 [IoT 中樞配額和節流](iot-hub-devguide-quotas-throttling.md)。

### <a name="identity-registry-operation-throughput"></a>身分識別登錄作業輸送量

大部分的 IoT 中樞識別登錄作業都與裝置佈建有關，應該都不是執行階段作業。

如需特定高載效能數據，請參閱 [IoT 中樞配額與節流](iot-hub-devguide-quotas-throttling.md)。

## <a name="auto-scale"></a>自動調整規模

如果即將達到 IoT 中樞上允許的訊息限制，則可使用這些[步驟來自動調整](https://azure.microsoft.com/resources/samples/iot-hub-dotnet-autoscale/)，以增加相同 IoT 中樞層的 IoT 中樞單位。

## <a name="next-steps"></a>後續步驟

* 如需 IoT 中樞功能和效能詳細資料的詳細資訊，請參閱 [IoT 中樞定價](https://azure.microsoft.com/pricing/details/iot-hub)或 [IoT 中樞配額和節流](iot-hub-devguide-quotas-throttling.md)。

* 若要變更 IoT 中樞層，請遵循[升級 IoT 中樞](iot-hub-upgrade.md)內的步驟。
