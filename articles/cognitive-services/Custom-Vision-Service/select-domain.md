---
title: 選取自訂視覺專案的網域-電腦視覺
titleSuffix: Azure Cognitive Services
description: 本文將說明如何在自訂視覺服務中選取專案的網域。
services: cognitive-services
author: shonohs
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: shono
ms.openlocfilehash: 1fb30cc0634224213dc9a188a16902e07d379904
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127764"
---
# <a name="select-a-domain-for-a-custom-vision-project"></a>選取自訂視覺專案的網域

從自訂視覺專案的 [設定] 分頁中，您可以選取專案的網域。 選擇最接近您案例的網域。

## <a name="image-classification"></a>影像分類

|網域|用途|
|---|---|
|__泛型__| 已針對廣泛的影像分類工作進行最佳化。 如果沒有其他適用的領域，或您不確定要選擇哪一個領域，請選取「泛型」領域。|
|__Food__|已針對菜餚相片進行最佳化，如同您在餐廳菜單上看見的一樣。 如果您想要將個別水果或蔬菜的相片分類，請使用「食物」領域。|
|__地標__|已針對可辨識的地標 (包括自然和人工) 進行最佳化。 地標在相片中清楚顯示時，此領域的效果最佳。 即使地標前面的人稍微阻擋到該地標，此領域還是能夠運作。|
|__零售__|已針對在購物目錄或購物網站上找到的影像進行最佳化。 如果您想在連衣裙、褲子和襯衫之間進行高精確度的分類，請使用此領域。|
|__精簡領域__| 已針對邊緣裝置上的即時分類條件約束進行優化。|

## <a name="object-detection"></a>物件偵測

|網域|用途|
|---|---|
|__一般__| 已針對廣泛的物件偵測工作進行最佳化。 如果沒有其他適用的領域，或您不確定要選擇哪一個領域，請選取「泛型」領域。|
|__標誌__|已針對尋找影像中的品牌標誌進行最佳化。|
|__貨架上的產品__|已針對在架子上偵測和分類產品進行優化。|
|__精簡領域__| 已針對邊緣裝置上的即時物件偵測條件約束進行優化。|

## <a name="compact-domains"></a>精簡領域

精簡領域所產生的模型可以匯出到本機執行。 模型效能會因選取的定義域而異。 在下表中，我們會報告 Intel 桌上型電腦 CPU 和 NVidia GPU \[1\]上的模型大小和推斷時間。 

> [!NOTE]
> 這些數位不包括前置處理和後處理時間。

|工作|網域|模型大小|CPU 推斷時間|GPU 推斷時間|
|---|---|---|---|---|
|分類|一般 (精簡)|5 MB|13 毫秒|5 毫秒|
|物件偵測|一般 (精簡)|45 MB|35毫秒|5 毫秒|
|物件偵測|一般（compact） [S1]|14 MB|27毫秒|7 毫秒|

## <a name="vaidk-vision-ai-dev-kit"></a>VAIDK （視覺 AI 開發工具組）

選取 compact 網域時，會提供額外的「匯出功能」選項，讓您能夠區別「基本平臺」和「視覺 AI 開發工具組」。

在 [_匯出功能_] 下，兩個選項為：

- 基本平臺（Tensorflow、CoreML、ONNX 等）
- 視覺 AI 開發工具組。

當您選取 [_視覺 AI 開發工具組_] 時，[一般] _（精簡）_ 和 _[一般] （compact） [S1]_ 可用於影像_分類，但_不能使用 [一般]、[_地標_] 和 [_零售_]，而非 [_食物_] compact 網域。

>[!NOTE]
>物件偵測的__一般（精簡）__ 網域需要特殊的後處理邏輯。 如需詳細資訊，請參閱匯出的 zip 套件中的範例腳本。 如果您需要不具後處理邏輯的模型，請使用__一般（compact） [S1]__。

>[!IMPORTANT]
>不保證匯出的模型會提供與雲端上的預測 API 完全相同的結果。 執行中平臺或前置處理的實作為些許差異，可能會在模型輸出中產生較大的差異。 如需前置處理邏輯的詳細資料，請參閱[這份檔](quickstarts/image-classification.md)。

\[1\] Intel 強式 E5-2690 CPU 和 NVIDIA Tesla M60
