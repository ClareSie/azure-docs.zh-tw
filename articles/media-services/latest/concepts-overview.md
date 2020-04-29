---
title: 媒體服務術語和概念
titleSuffix: Azure Media Services
description: 瞭解 Azure 媒體服務的術語和概念。
services: media-servicesgit
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/18/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 02d0897774261a25a2fccb70a31d0f264c458740
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "79500098"
---
# <a name="media-services-terminology-and-concepts"></a>媒體服務術語和概念

本主題提供 Azure 媒體服務術語和概念的簡要概述。 本文也會提供文章的連結，其中包含媒體服務 v3 概念和功能的深入說明。

在開始開發之前，應該先檢查這些主題中所述的基本概念。

> [!NOTE]
> 目前，您可以使用[Azure 入口網站](https://portal.azure.com/)來：管理媒體服務 V3[即時事件](live-events-outputs-concept.md)、查看（不管理） v3[資產](assets-concept.md)，以及[取得存取 api 的相關資訊](access-api-portal.md)。
> 針對所有其他管理工作（例如，[轉換和作業](transforms-jobs-concept.md)和[內容保護](content-protection-overview.md)），請使用[REST API](https://aka.ms/ams-v3-rest-ref)、 [CLI](https://aka.ms/ams-v3-cli-ref)或其中一個支援的[sdk](media-services-apis-overview.md#sdks)。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="media-services-v3-terminology"></a>媒體服務 v3 術語

|詞彙|描述|
|---|---|
|實況活動|**實況活動**代表內嵌、轉碼（選擇性），以及封裝影片、音訊和即時中繼資料的即時串流的管線。<br/><br/>針對從媒體服務 v2 Api 進行遷移的客戶，**即時事件**會取代 v2 中的**通道**實體。 如需詳細資訊，請參閱[從 V2 遷移至 v3](migrate-from-v2-to-v3.md)。|
|串流端點/封裝/來源|**串流端點**代表動態（即時）封裝和原始服務，可將您的即時和隨選內容直接傳遞至用戶端播放機應用程式。 它會使用其中一種常見的串流處理媒體通訊協定（HLS 或破折號）。 此外，**串流端點**也提供動態（及時）加密給領先業界的數位版權管理系統（drm）。<br/><br/>在媒體串流處理產業中，這項服務通常稱為封裝工具**或****來源**。  此功能的產業中的其他常見詞彙包括 JITP （僅限時間封裝程式）或 JITE （及時加密）。

## <a name="media-services-v3-concepts"></a>媒體服務 v3 概念

|概念|描述|連結|
|---|---|---|
|資產和上傳內容|若要開始在 Azure 中管理、加密、編碼、分析和串流處理媒體內容，您需要建立媒體服務帳戶，並將數位檔案上傳到**資產**。|[雲端上傳和儲存體](storage-account-concept.md)<br/><br/>[資產概念](assets-concept.md)|
|內容編碼|將高品質的數位媒體檔案上傳到資產之後，您就可以將它們編碼成可在各種不同的瀏覽器和裝置上播放的格式。 <br/><br/>若要使用媒體服務 v3 進行編碼，您需要建立**轉換**和**作業**。|[轉換與工作](transforms-jobs-concept.md)<br/><br/>[使用媒體服務編碼](encoding-concept.md)|
|分析內容 (影片索引器)|媒體服務 v3 可讓您使用媒體服務 v3 預設值，從您的影片和音訊檔案中摘錄見解。 若要使用媒體服務 v3 預設值來分析您的內容，您需要建立**轉換**和**作業**。<br/><br/>如果您想要更詳細的深入解析，請直接使用[影片索引子](https://docs.microsoft.com/azure/media-services/video-indexer/)。|[分析視訊和音訊檔案](analyzing-video-audio-files-concept.md)|
|封裝和交付|一旦您的內容經過編碼，您就可以利用**動態封裝**。 在媒體服務中，**串流端點**是用來將媒體內容傳遞給用戶端播放者的動態封裝服務。 若要讓輸出資產中的影片可供用戶端播放，您必須建立**串流定位器**，然後建立串流 url。 <br/><br/>建立**串流定位器**時，除了資產的名稱之外，您還需要指定**串流原則**。 **串流原則**可讓您定義**串流定位器**的串流通訊協定和加密選項（如果有的話）。 無論您是即時或隨選串流內容，都可以使用動態封裝。 <br/><br/>您可以使用媒體服務**動態資訊清單**，只串流特定的轉譯或影片 subclips。|[動態封裝](dynamic-packaging-overview.md)<br/><br/>[串流端點](streaming-endpoint-concept.md)<br/><br/>[串流定位器](streaming-locators-concept.md)<br/><br/>[串流原則](streaming-policy-concept.md)<br/><br/>[動態資訊清單](filters-dynamic-manifest-overview.md)<br/><br/>[篩選器](filters-concept.md)|
|內容保護|有了媒體服務，您就可以使用進階加密標準（AES-128）或/和三個主要 DRM 系統（Microsoft PlayReady、Google Widevine 和 Apple FairPlay）的任何一種，以動態方式傳遞您的即時和隨選內容。 媒體服務也提供服務，可傳遞 AES 金鑰和 DRM (PlayReady、Widevine 和 FairPlay) 授權給授權用戶端。 <br/><br/>如果您在串流上指定加密選項，請建立**內容金鑰原則**，並將它與您的**串流定位器**建立關聯。 **內容金鑰原則**可讓您設定如何將內容金鑰傳遞給終端用戶端。<br/><br/> 請在需要相同的選項時，嘗試重複使用原則。| [內容金鑰原則](content-key-policy-concept.md)<br/><br/>[內容保護](content-protection-overview.md)|
|即時串流|媒體服務可讓您在 Azure 雲端上將實況活動傳遞給客戶。 **實況活動**負責內嵌和處理即時視訊摘要。 當您建立**即時事件**時，系統會建立輸入端點，讓您用來從遠端編碼器傳送即時信號。 當資料流程流入**即時事件**之後，您就可以藉由建立**資產**、**即時輸出**和**串流定位器**來開始串流事件。 **即時輸出**會將串流封存到**資產**中，並透過**串流端點**將它提供給檢視器。 即時事件可設定為*傳遞* (內部部署即時編碼器會傳送多重位元速率串流) 或*即時編碼* (內部部署即時編碼器會傳送單一位元速率串流)。 |[即時串流概觀](live-streaming-overview.md)<br/><br/>[即時事件與即時輸出](live-events-outputs-concept.md)|
|使用事件方格監視|若要查看作業的進度，請使用**事件方格**。 媒體服務也會發出即時事件種類。 透過事件方格，您的應用程式幾乎可以從所有 Azure 服務和自訂來源接聽及回應事件。 |[處理事件方格事件](reacting-to-media-services-events.md)<br/><br/>[結構描述](media-services-event-schemas.md)|
|使用 Azure 監視器進行監視|監視計量和診斷記錄，以協助您瞭解如何使用 Azure 監視器來執行應用程式。|[計量和診斷記錄](media-services-metrics-diagnostic-logs.md)<br/><br/>[診斷記錄結構描述](media-services-diagnostic-logs-schema.md)|
|播放程式用戶端|您可以使用 Azure 媒體播放機，在各種不同的瀏覽器和裝置上播放媒體服務串流的媒體內容。 Azure 媒體播放機使用 HTML5、媒體來源延伸模組（MSE）和加密媒體延伸（EME）等業界標準來提供豐富的彈性串流體驗。 |[Azure 媒體播放器概觀](use-azure-media-player.md)|

## <a name="ask-questions-give-feedback-get-updates"></a>提出問題、提供意見反應、取得更新

請參閱 [Azure 媒體服務社群](media-services-community.md)文章，以了解詢問問題、提供意見反應及取得媒體服務相關更新的不同方式。

## <a name="next-steps"></a>後續步驟

* [遠端檔案編碼和串流影片 - REST](stream-files-tutorial-with-rest.md)
* [已上傳檔案編碼和串流影片 - REST](stream-files-tutorial-with-api.md)
* [即時串流 - .NET](stream-live-tutorial-with-api.md)
* [分析影片 - .NET](analyze-videos-tutorial-with-api.md)
* [AES-128 動態加密 - .NET](protect-with-aes128.md)
* [透過多重 DRM 進行動態加密 - .NET](protect-with-drm.md)
