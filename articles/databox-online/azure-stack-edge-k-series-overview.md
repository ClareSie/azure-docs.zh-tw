---
title: Azure Stack Edge Mini R 概觀 | Microsoft Docs
description: 說明 Azure Stack Edge Mini R，這是軍事應用程式的儲存體解決方案，使用具有電池的便攜實體裝置，透過 WI-FI 傳輸到 Azure。
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 09/23/2020
ms.author: alkohli
ms.openlocfilehash: 23ec79b7e3989c9749edf98d930ad8305324c9da
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2020
ms.locfileid: "96454015"
---
# <a name="what-is-the-azure-stack-edge-mini-r"></a>Azure Stack Edge Mini R 是什麼？

Azure Stack Edge Mini R 是專為在惡劣環境中使用而設計的，是非常便於攜帶且堅固的邊緣計算裝置。 Azure Stack Edge Mini R 以硬體即服務解決方案的形式提供。 Microsoft 可為您提供作為網路儲存體閘道的雲端管理裝置，並具有可加速 AI 推斷的內建視覺處理單元 (VPU)。

本文提供您 Azure Stack Edge Mini R 解決方案的概觀、主要功能，以及適合部署此裝置的案例。


## <a name="key-capabilities"></a>主要功能

Azure Stack Edge Mini R 具有下列功能：

|功能 |描述  |
|---------|---------|
|堅固的硬體| 專為最嚴峻的環境設計的堅固硬體。|
|便攜| 便於攜帶，使用電池的板型規格。|
|雲端管理|裝置與服務是透過 Azure 入口網站來管理的。|
|Edge 計算工作負載|允許對資料進行分析、處理、篩選。<br>支援 VM 和容器化工作負載。 |
|加速 AI 推斷| 由 Intel Movidius Myriad X VPU 啟用。|
|支援有線與無線使用 | 允許以有線和無線方式進行資料轉送。|
|資料存取     | 能使用雲端 API 直接從 Azure 儲存體 Blob 與 Azure 檔案存取資料，以在雲端中進行額外的資料處理。 裝置上的本機快取可用來快速存取最近使用的檔案。|
|中斷連線模式|  使用者可以選擇透過 Azure Stack Hub 管理裝置和服務。 以離線模式部署、執行和管理應用程式。 <br> 中斷連線模式支援離線上傳案例。|
|支援的檔案傳輸通訊協定      |支援使用標準 SMB、NFS 與 REST 通訊協定擷取資料。 <br> 如需支援版本的詳細資訊，請移至 [ 系統需求](azure-stack-edge-gpu-system-requirements.md)。|
|資料重新整理     | 能夠以來自雲端的最新檔案對本機檔案進行重新整理。|
|雙重加密    | 使用自我加密磁碟提供第一層加密。 VPN 提供第二層加密。 支援使用 BitLocker 在本機加密資料，並透過 *https* 保護對雲端的資料傳輸。|
|頻寬節流設定| 節流以限制尖峰時間的頻寬使用量。|

## <a name="use-cases"></a>使用案例

以下是 Azure Stack Edge Mini R 可用於該邊緣的快速 Machine Learning (ML) 推斷，並在將資料傳送到 Azure 之前先進行預先處理的各種案例。

- **使用 Azure Machine Learning 進行推斷**：透過 Azure Stack Edge Mini R，您可以執行 ML 模型，以便在將資料傳送到雲端之前快速取得可據以採取動作的結果。 完整資料集可以選擇性地進行傳輸，以繼續重新定型並改善您的 ML 模型。 如需如何在 Azure Stack Edge Mini R 裝置上使用 Azure ML 硬體加速模型的詳細資訊，請參閱[在 Azure Stack Edge Mini R 上部署 Azure ML 硬體加速模型](../machine-learning/how-to-deploy-fpga-web-service.md#deploy-to-a-local-edge-server) \(部分機器翻譯\)。

- **前置處理資料** - 先透過計算選項 (例如容器化工作負載和虛擬機器) 轉換資料，再將資料傳送至 Azure，以建立更容易操作的資料集。 預先處理功能可用來：

    - 彙總資料。
    - 修改資料，例如移除個人資料。
    - 子集資料可將儲存體和頻寬最佳化，或用於進一步分析。
    - 分析並對 IoT 事件做出反應。

- **透過網路將資料傳輸到 Azure**：使用 Azure Stack Edge Mini R，輕鬆且快速地將資料傳輸到 Azure，以進行進一步的運算和分析，或基於封存目的使用。

## <a name="components"></a>元件

Azure Stack Edge Mini R 解決方案由 Azure Stack Edge 資源、Azure Stack Edge Mini R 堅固便攜的實體裝置及本機 Web UI 所組成。

* **Azure Stack Edge Mini R 實體裝置** - 由 Microsoft 提供的計算和儲存裝置，非常便於攜帶且堅固。 裝置具有內建電池，重量不到 7 磅。

    ![Azure Stack Edge Mini R 裝置](media/azure-stack-edge-k-series-overview/perspective-view-1.png)

* **Azure Stack Edge 資源** - Azure 入口網站中的資源，可讓您從不同的地理位置存取 Web 介面來管理堅固的 Azure Stack Edge Mini R 裝置。 使用 Azure Stack Edge 資源來建立和管理資源、檢視和管理裝置與警示，以及管理共用。  

* **Azure Stack Edge Mini R 本機 Web UI** - Azure Stack Edge Mini R 裝置上的瀏覽器型本機使用者介面，主要是用於裝置的初始設定。 也可使用本機 Web UI 對 Azure Stack Edge Pro 裝置執行診斷、關機和重新啟動、檢視複製記錄，以及連絡 Microsoft 支援服務以提出服務要求。


## <a name="region-availability"></a>區域可用性

Azure Stack Edge Mini R 實體裝置、Azure 資源，以及您要傳輸資料的目標儲存體帳戶，不需全都位於相同區域。

- **資源可用性**：如需 Azure Stack Edge 適用區域的完整清單，請移至 [依區域提供的 Azure 產品](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)。 

- **裝置可用性** - 如需適用 Azure Stack Edge Mini R 裝置的所有國家/地區的清單，請移至 [Azure Stack Edge Mini R 定價](https://azure.microsoft.com/pricing/details/azure-stack/edge/#azureStackEdgeMiniR)，參閱 [Azure Stack Edge Mini R] 索引標籤中的 [可用性] 區段。

- **目的地儲存體帳戶** - 儲存資料的儲存體帳戶在所有 Azure 區域都可用。 若要獲得最佳效能，儲存 Azure Stack Edge Mini R 資料的儲存體帳戶應位於接近裝置所在的位置。 儲存體帳戶的位置若離裝置遙遠，將會導致較長的延遲時間與較緩慢的效能。


## <a name="next-steps"></a>後續步驟

- 檢閱 [ 系統需求](azure-stack-edge-gpu-system-requirements.md)。