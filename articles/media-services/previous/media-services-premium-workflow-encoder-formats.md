---
title: 媒體編碼器高階工作流程格式和轉碼器 | Microsoft Docs
description: 本主題提供 Media Encoder Premium Workflow 格式和轉碼器的概觀
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: 87cd7c63939331190530a46071a6b4c40480562f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84705095"
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>媒體編碼器高階工作流程格式和轉碼器

> [!NOTE]
> 本主題中討論的媒體編碼器高階工作流程媒體處理器無法在中國使用。 

本文包含 **Media Encoder Premium Workflow** 編碼器公開預覽版本支援的輸入與輸出檔案格式以及轉碼器清單。

[媒體編碼器高階工作流程輸入格式和編解碼器](#input_formats)

媒體編碼器高階工作流程輸出格式和轉碼器

**Media Encoder Premium Workflow** 支援 [本](#closed_captioning) 章節所述的隱藏式字幕。 

## <a name="media-encoder-premium-workflow-input-formats-and-codecs"></a><a id="input_formats"></a>Media Encoder Premium Worflow 輸入格式和轉碼器

下節列出此媒體處理器支援做為輸入的轉碼器和檔案格式。

### <a name="input-containerfile-formats"></a>輸入容器/檔案格式

* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2 傳輸資料流
* MPEG-2 程式資料流
* MPEG-4/MP4
* Windows Media/ASF
* AVI (未壓縮 8 位元/10 位元)

### <a name="input-video-codecs"></a>輸入視訊轉碼器

* AVC 8 位元/10 位元，高達 4:2:2，包括 AVCIntra
* Avid DNxHD (使用 MXF)
* DVCPro/DVCProHD (使用 MXF)
* HEVC/H.265、主要和主要 10 設定檔
* JPEG2000
* MPEG-2 (高達 422 Profile 和 High Level，包括 XDCAM、XDCAM HD、XDCAM IMX、CableLabs ® 和 D10 等變種)
* MPEG-1
* Windows Media 視訊/VC-1

### <a name="input-audio-codecs"></a>輸入音訊轉碼器

* AES (SMPTE 331M 和 302M，AES3-2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC、AAC-HE 和 AAC-HEv2；高達 5.1)
* MPEG Layer 2
* MP3 (MPEG-1 音訊層 3)
* Windows Media 音訊
* WAV/PCM

## <a name="media-encoder-premium-workflow-output-formats-and-codecs"></a><a id="output_format"></a>Media Encoder Premium Worflow 輸出格式和轉碼器

下節列出此媒體處理器支援做為輸出的轉碼器和檔案格式。

### <a name="output-containerfile-formats"></a>輸出容器/檔案格式

* Adobe® Flash® F4V
* MXF (OP1a、XDCAM 和 AS02)
* DPP (包括 AS11)
* GXF
* MPEG-4/MP4
* Windows Media/ASF
* AVI (未壓縮 8 位元/10 位元)
* Smooth Streaming 檔案格式 (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>輸出視訊轉碼器

* AVC (H.264；8 位元；高達 High Profile、Level 5.2；4K Ultra HD；AVC Intra)
* Avid DNxHD (使用 MXF)
* DVCPro/DVCProHD (使用 MXF)
* MPEG-2 (高達 422 Profile 和 High Level，包括 XDCAM、XDCAM HD、XDCAM IMX、CableLabs ® 和 D10 等變種)
* MPEG-1
* Windows Media 視訊/VC-1
* JPEG 縮圖建立
* HEVC (H.265、8 位元和 10 位元、主要和主要 10 設定檔)


### <a name="output-audio-codecs"></a>輸出音訊轉碼器

* AES (SMPTE 331M 和 302M，AES3-2003)
* Dolby® Digital (AC3)
* Dolby® Digital Plus (E-AC3) 高達 7.1
* AAC (AAC-LC、AAC-HE 和 AAC-HEv2；高達 5.1)
* MPEG Layer 2
* MP3 (MPEG-1 音訊層 3)
* Windows Media 音訊

>[!NOTE]
>如果編碼成 Dolby® Digital (AC3)，則輸出只能寫入到 ISO MP4 檔案。

## <a name="support-for-closed-captioning"></a><a id="closed_captioning"></a>支援隱藏式字幕

內嵌時， **Media Encoder Premium Workflow** 支援：

1. SCC 檔案
2. SMPTE-TT 檔案
3. CEA-608/CEA-708 – 以使用者資料 (H.264 基礎資料流、ATSC/53、SCTE20 的 SEI 訊息) 的形式傳送，或以 MXF/GXF 檔案中的輔助資料形式傳送
4. STL 字幕檔案

輸出時，可以使用下列選項：

1. CEA-608 至 CEA-708 翻譯
2. CEA-608/CEA-708 傳遞 (內嵌在 H.264 基礎資料流的 SEI 訊息，或執行為 MXF 檔案中的輔助資料)
3. SCC
4. SMPTE 計時文字 (來自來源 CEA-608 經由 SMPTE RP2052，包括建立 DFXP 檔案)
5. SRT 副標題檔案
6. DVB 副標題資料流

> [!NOTE]
> Azure 媒體服務並不支援透過串流傳遞上述所有的輸出格式。

## <a name="known-issues"></a>已知問題

如果您的輸入視訊不包含隱藏式字幕，輸出資產仍然會包含空白 TTML 檔案。 

## <a name="need-help"></a>需要協助嗎？

您可以藉由瀏覽至[新增支援要求](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)來開啟支援票證
## <a name="media-services-learning-paths"></a>媒體服務學習路徑

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>提供意見反應

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

