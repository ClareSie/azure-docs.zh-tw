---
title: 平衡多個實例之間的分割區負載-Azure 事件中樞 |Microsoft Docs
description: 描述如何使用事件處理器和 Azure 事件中樞 SDK，在應用程式的多個實例之間平衡分割區負載。
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: d5db1e877c1bfa6fac177e1ff8ed137e0301b709
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85314994"
---
# <a name="balance-partition-load-across-multiple-instances-of-your-application"></a>平衡應用程式多個實例之間的分割區負載
若要調整您的事件處理應用程式，您可以執行應用程式的多個實例，並讓它在負載之間取得平衡。 在較舊的版本中， [EventProcessorHost](event-hubs-event-processor-host.md)允許您在接收時，平衡程式的多個實例與檢查點事件之間的負載。 在較新的版本（5.0）中， **EventProcessorClient** （.Net 和 JAVA）或**EventHubConsumerClient** （Python 和 JavaScript）可讓您執行相同的動作。 使用事件可讓開發模型變得更簡單。 您可以藉由註冊事件處理常式來訂閱您感興趣的事件。

本文說明使用多個實例從事件中樞讀取事件的範例案例，然後提供事件處理器用戶端功能的詳細資料，讓您一次從多個分割區接收事件，並與其他使用相同事件中樞和取用者群組的取用者進行負載平衡。

> [!NOTE]
> 分割取用者概念是針對事件中樞調整大小的關鍵。 與[競爭取用者](https://msdn.microsoft.com/library/dn568101.aspx)模式相比，分割取用者模式可藉由消除爭奪瓶頸，以及加速端對端平行處理原則來具有高度縮放能力。

## <a name="example-scenario"></a>範例案例

針對範例案例，請設想有一家監視 100,000 戶家庭的住家安全公司。 每隔一分鐘，就會從各種感應器（例如動作偵測器、門/視窗開啟感應器、玻璃中斷偵測器等等）取得資料，並在每個首頁中安裝。 該公司會提供網站給住戶，讓他們可近乎即時地監視房子內的活動。

每個感應器會將資料推送至事件中樞。 事件中樞已設定 16 個分割區。 在取用端，您需要一種機制來讀取這些事件、將其合併（篩選、匯總等等），然後將匯總傾印到儲存體 blob，然後將其投射到方便使用的網頁。

## <a name="write-the-consumer-application"></a>撰寫取用者應用程式

在分散式環境中設計取用者時，案例必須處理下列需求：

1. **縮放：** 建立多個取用者，而每個取用者都會負責從幾個事件中樞分割區讀取資料。
2. **負載平衡：** 以動態方式增加或減少取用者。 例如，對每個家庭新增感應器類型 (例如一氧化碳偵測器) 時，事件數目就會增加。 在此情況下，操作人員會增加取用者執行個體的數目。 然後，取用者集區可以重新平衡他們擁有的分割區數目，讓新增的取用者共同分攤負載。
3. **失敗時順暢地繼續：** 如果取用者（取用者**a**）發生失敗（例如，裝載取用者的虛擬機器突然損毀），則其他取用者可以挑選取用**者 a**所擁有的分割區，然後繼續進行。 此外，接續點 (稱為「檢查點」** 或「位移」**) 應剛好位在**取用者 A** 失敗的位置，或在此稍微前面一點的位置。
4. 取用**事件：** 雖然前三個點會處理取用者的管理，但必須有程式碼來取用事件並對其執行有用的工作。 例如，將它匯總並上傳至 blob 儲存體。

## <a name="event-processor-or-consumer-client"></a>事件處理器或取用者用戶端

您不需要建立自己的解決方案，就能滿足這些需求。 Azure 事件中樞 Sdk 會提供這種功能。 在 .NET 或 JAVA Sdk 中，您可以使用事件處理器用戶端（EventProcessorClient），在 Python 和 JavaScript Sdk 中，您可以使用 EventHubConsumerClient。 在舊版的 SDK 中，它是支援這些功能的事件處理器主機（EventProcessorHost）。

在大部分的生產環境案例中，我們建議您使用事件處理器用戶端來讀取和處理事件。 處理器用戶端的目的是提供健全的經驗，讓您以高效能且容錯的方式處理事件中樞所有分割區的事件，同時提供方法來檢查其進度。 事件處理器用戶端也能夠在指定事件中樞的取用者群組內容中，以合作方式運作。 當實例可供使用或無法供群組使用時，用戶端會自動管理工作的散發和平衡。

## <a name="partition-ownership-tracking"></a>分割區的擁有權追蹤

事件處理器實例通常會擁有並處理一或多個資料分割中的事件。 資料分割的擁有權會平均分散于與事件中樞和取用者群組組合相關聯的所有作用中事件處理器實例之間。 

藉由新增或更新檢查點存放區中的專案，為每個事件處理器提供唯一識別碼，並宣告資料分割的擁有權。 所有事件處理器實例會定期與此存放區進行通訊，以更新其本身的處理狀態，並瞭解其他作用中的實例。 然後，此資料會用來平衡使用中處理器之間的負載。 新的實例可以加入處理集區，以相應增加規模。 當實例因失敗或相應減少而停止時，分割區擁有權會正常地傳輸到其他使用中的處理器。

檢查點存放區中的資料分割擁有權記錄會追蹤事件中樞的命名空間、事件中樞名稱、取用者群組、事件處理器識別碼（也稱為擁有者）、資料分割識別碼和上次修改時間。



| 事件中樞命名空間               | 事件中樞名稱 | **取用者群組** | 擁有者                                | 資料分割識別碼 | 上次修改時間  |
| ---------------------------------- | -------------- | :----------------- | :----------------------------------- | :----------- | :------------------ |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 3be3f9d3-9d9e-4c50-9491-85ece8334ff6 | 0            | 2020-01-15T01：22：15 |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | f5cc5176-ce96-4bb4-bbaa-a0e3a9054ecf | 1            | 2020-01-15T01：22：17 |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 72b980e9-2efc-4ca7-ab1b-ffd7bece8472 | 2            | 2020-01-15T01：22：10 |
|                                    |                | :                  |                                      |              |                     |
|                                    |                | :                  |                                      |              |                     |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 844bd8fb-1f3a-4580-984d-6324f9e208af | 15           | 2020-01-15T01：22：00 |

每個事件處理器實例都會取得分割區的擁有權，並開始處理最後已知[檢查點](# Checkpointing)的分割區。 如果處理器失敗（VM 關機），則其他實例會藉由查看上次修改時間來偵測此情況。 其他實例會嘗試取得非使用中實例先前擁有之分割區的擁有權，而檢查點存放區則保證只有其中一個實例會成功宣告資料分割的擁有權。 因此，在任何指定的時間點，最多隻會有一個處理器從分割區接收事件。

## <a name="receive-messages"></a>接收訊息

當您建立事件處理器時，會指定將處理事件和錯誤的函式。 每次呼叫處理事件的函式時，都會從特定的分割區傳遞單一事件。 您必須負責處理這個事件。 如果您想要確保取用者至少處理每個訊息一次，您必須使用重試邏輯來撰寫自己的程式碼。 但請留意有害訊息。

我們建議您執行相當快速的動作。 也就是說，儘量少處理。 如果您需要寫入儲存體並進行某些路由，最好使用兩個取用者群組，並有兩個事件處理器。

## <a name="checkpointing"></a>檢查點

*檢查點*是事件處理器用來標記或認可資料分割內上次成功處理事件之位置的進程。 標記檢查點通常會在處理事件的函式中完成，並在取用者群組內以每個分割區為基礎發生。 

如果事件處理器與分割區中斷連接，另一個實例可以繼續處理該資料分割在該取用者群組中最後一個處理器所認可的檢查點。 當處理器連接時，它會將位移傳遞給事件中樞，以指定要開始讀取的位置。 如此一來，您可以使用檢查點，將事件標示為「完成」，由下游應用程式，並在事件處理器關閉時提供復原功能。 從這個檢查點處理程序中指定較低的位移，即可回到較舊的資料。 

當執行檢查點以將事件標示為已處理時，檢查點存放區中的專案會以事件的位移和序號來新增或更新。 使用者應該決定更新檢查點的頻率。 在每個成功處理的事件之後更新，會在觸發基礎檢查點存放區的寫入作業時，產生效能和成本的影響。 此外，每個單一事件的檢查點都是指佇列訊息模式，其中服務匯流排佇列可能是比事件中樞更好的選項。 事件中樞背後的構想是，在大型規模中您能取得「至少一次」的傳遞。 藉由讓您的下游系統具有等冪性，即可輕鬆地從失敗中復原，或以接收多次的相同事件重新啟動該結果。

> [!NOTE]
> 如果您在支援儲存體 Blob SDK 不同版本的環境中使用 Azure Blob 儲存體做為檢查點存放區，而不是在 Azure 上提供，您必須使用程式碼，將儲存體服務 API 版本變更為該環境所支援的特定版本。 例如，如果您在[Azure Stack Hub 2002 版上執行事件中樞](https://docs.microsoft.com/azure-stack/user/event-hubs-overview)，儲存體服務的最高可用版本是2017-11-09 版。 在此情況下，您必須使用程式碼，將儲存體服務 API 版本的目標設為2017-11-09。 如需如何以特定儲存體 API 版本為目標的範例，請參閱 GitHub 上的下列範例： 
> - [.Net](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample10_RunningWithDifferentStorageVersion.cs)。 
> - [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/)
> - [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript)或[TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/typescript)
> - [Python](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/)

## <a name="thread-safety-and-processor-instances"></a>執行緒安全性和處理器執行個體

根據預設，會針對指定的資料分割，順序呼叫處理事件的函式。 後續事件，並從相同的分割區佇列呼叫此函式，因為事件抽取會繼續在其他執行緒的背景中執行。 請注意，來自不同分割區的事件可以同時處理，而且跨分割區存取的任何共用狀態都必須同步。

## <a name="next-steps"></a>後續步驟
請參閱下列快速入門：

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](get-started-python-send-v2.md)
- [JavaScript](get-started-node-send-v2.md)
