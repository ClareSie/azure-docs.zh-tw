---
title: 準備模型進行部署
titleSuffix: ML Studio (classic) - Azure
description: 如何通過將機器學習工作室（經典）培訓實驗轉換為預測實驗，準備經過培訓的模型以部署為 Web 服務。
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 03/28/2017
ms.openlocfilehash: 061c340f8c4952d5a0f2a3873f7475e4f733c290
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204506"
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio-classic"></a>如何準備模型以部署在 Azure 機器學習工作室（經典）

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure 機器學習工作室（經典版）為您提供了開發預測分析模型，然後將其部署為 Azure Web 服務來操作所需的工具。

為此，您可以使用 Studio（經典）創建一個實驗（稱為*訓練實驗*），在其中訓練、評分和編輯模型。 滿意之後，即備妥模型以進行部署，方法是將訓練實驗轉換成「預測實驗」**，並將後者設定成給使用者資料評分。

您可以在教程 1 中看到此過程的示例[：預測信用風險](tutorial-part1-credit-risk.md)。

本文所深入探討的詳細資料是有關如何將訓練實驗轉換成預測實驗，以及預測實驗的部署方式。 了解這些詳細資料，即可了解如何設定您已部署的模型，以讓它更具效率。



## <a name="overview"></a>總覽 

將訓練實驗轉換為預測實驗包含三個步驟：

1. 將機器學習演算法模組取代為定型模型。
2. 將訓練調整為僅適用於評分所需的模組。 訓練實驗包含訓練所需但模型受過訓練之後，就不再需要的多個模組。
3. 定義模型如何接受來自 Web 服務使用者的資料，以及將傳回的資料。

> [!TIP]
> 在訓練實驗中，您曾考慮使用您自己的資料來訓練和評分模型。 但部署之後，使用者就會將新的資料傳送至您的模型，而且會傳回預測結果。 因此，將訓練實驗轉換成預測實驗以準備好進行部署時，請記住其他人如何使用模型。
> 
> 

## <a name="set-up-web-service-button"></a>設定 Web 服務按鈕
執行實驗之後 (按一下實驗畫布底端的 [執行]****)，請按一下 [設定 Web 服務]**** 按鈕 (選取 [預測 Web 服務]**** 選項)。 [設定 Web 服務]**** 會執行三個步驟，以將訓練實驗轉換為預測實驗：

1. 它會將定型模型儲存到實驗畫布左側之模組選擇區的 [定型模型]**** 區段中。 它接著會將機器學習演算法和[定型模型][train-model]模組取代為所儲存的定型模型。
2. 它會分析您的實驗，並移除已清楚僅用於定型且不再需要的模組。
3. 它會將 _Web 服務輸入_和_輸出_模組插入實驗中的預設位置 (這些模組會接受並傳回使用者資料)。

例如，以下實驗會使用範例人口普查資料，訓練二元推進式決策樹模型：

![訓練實驗](./media/convert-training-experiment-to-scoring-experiment/figure1.png)

這項實驗中的模組基本上執行是四個不同的功能：

![模組功能](./media/convert-training-experiment-to-scoring-experiment/figure2.png)

當您將這個訓練實驗轉換為預測實驗時，就不再需要其中一些模組，或它們現在有不同的用途：

* **資料** - 在評分期間，不會使用此範例資料集中的資料，因為 Web 服務的使用者會提供要評分的資料。 不過，定型模型會使用此資料集中的中繼資料，例如，資料類型。 因此，您必須將資料集保留在預測實驗中，讓它能夠提供這項中繼資料。

* **準備** - 根據要提交用於評分的使用者資料而定，這些模組不一定需要處理傳入的資料。 [設定 Web 服務]**** 按鈕並未接觸這些項目，您需要決定要如何處理它們。
  
    例如，在此範例中，範例資料集可能會有遺漏的值，因此，會包含[清除遺漏資料][clean-missing-data]模型來處理它們。 而且，範例資料集會包括培訓模型不需要的資料行。 因此，包括[選取資料集中的資料行][select-columns]模組，以將那些額外的資料行從資料流程中排除。 如果您知道透過 Web 服務提交用於評分的資料不會有遺漏的值，則您可以移除[清除遺漏的資料][clean-missing-data]模組。 不過，因為[選取資料集中的資料行][select-columns]模組有助於定義定型模型所預期的資料行，所以必須保留該模組。

* **訓練** - 這些模組可用來訓練模型。 當您按一下 [設定 Web 服務]**** 時，這些模組都會取代為包含定型模型的單一模組。 這個新模組會儲存在模組調色盤的 [定型模型]**** 區段。

* **評分** - 在此範例中，[分割資料][split]模組是用來將資料流分割成測試資料和訓練資料。 在預測實驗中，我們不會再進行訓練，因此可以移除[分割資料][split]。 同樣地，第二個[評分模型][score-model]模組和[評估模型][evaluate-model]模組會用來比較測試資料的結果，因此在預測實驗中不需要這些模組。 但其餘的[評分模型][score-model]模組需要透過 Web 服務傳回評分結果。

以下是按一下 [設定 Web 服務] **** 之後，我們的範例外觀：

![已轉換的預測實驗](./media/convert-training-experiment-to-scoring-experiment/figure3.png)

[設定 Web 服務]**** 所完成的工作可能足以準備您的實驗，以當做 Web 服務部署。 不過，您可能會想要執行一些您的實驗專屬的額外工作。

### <a name="adjust-input-and-output-modules"></a>調整輸入和輸出模組
在訓練實驗中，您使用一組訓練資料進行一些處理，以取得 Machine Learning 演算法所需格式的資料。 如果您預期透過 Web 服務收到的資料不需要這項處理，則可以略過它︰將 **Web 服務輸入模組**的輸出連線至實驗中的不同模組。 使用者的資料現在會送到模型的這個位置。

例如，[設定 Web 服務]**** 預設會將 [Web 服務輸入]**** 模組放在資料流程的頂端，如上圖所示。 但我們可以手動將 **Web 服務輸入**放在資料處理模組之後：

![移動 Web 服務輸入](./media/convert-training-experiment-to-scoring-experiment/figure4.png)

透過 Web 服務提供的輸入資料現在會直接傳入 [評分模型] 模組，而不需要任何前置處理。

同樣地， **設定 Web 服務** 會將 Web 服務輸出模組放在資料流程的底部。 在此範例中，Web 服務會將[評分模型][score-model]模組的輸出傳回給使用者，其中包括完整的輸入資料向量以及評分結果。
不過，如果您偏好傳回其他內容，則可以在 **Web 服務輸出**模組之前新增其他模組。 

例如，若只要傳回評分結果，而不傳回整個輸入資料向量，請新增[選取資料集中的資料行][select-columns]模組，以排除評分結果以外的所有資料行。 接著，將 **Web 服務輸出**模組移到[選取資料集中的資料行][select-columns]模組的輸出。 實驗看起來如下：

![移動 Web 服務輸出](./media/convert-training-experiment-to-scoring-experiment/figure5.png)

### <a name="add-or-remove-additional-data-processing-modules"></a>新增或移除其他資料處理模組
如果在您的實驗中還有您知道在評分期間不需要的模組，則可以移除這些模組。 例如，由於我們已經將 **Web 服務輸入**模組移到資料處理模組之後的某個點，因此我們可以從預測實驗中移除[清除遺漏的資料][clean-missing-data]模組。

我們的預測實驗現在看起來像這樣：

![移除額外的模組](./media/convert-training-experiment-to-scoring-experiment/figure6.png)


### <a name="add-optional-web-service-parameters"></a>新增選用的 Web 服務參數
在某些情況下，您可能需要讓 Web 服務的使用者變更存取服務時的模組行為。 *Web 服務參數* 可讓您執行這項操作。

常見的範例是設定[匯入資料][import-data]模組，讓已部署之 Web 服務的使用者可以在存取 Web 服務時，指定不同的資料來源。 或者，設定[匯出資料][export-data]模組，以便能夠指定不同的目的地。

您可以定義 Web 服務參數，並使其與一個或多個模組參數產生關聯，而且您可以指定它們是必要還是選用參數。 Web 服務的使用者會在服務受到存取時提供這些參數的值，並據此修改模組動作。

如需 Web 服務參數和其用法的詳細資訊，請參閱[使用 Azure Machine Learning Web 服務參數][webserviceparameters]。

[webserviceparameters]: web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>將預測實驗部署為 Web 服務
既然已經為預測實驗做好充分的準備，您可以將它當做 Azure Web 服務部署。 使用者可以使用 Web 服務，將料傳送到您的模型，模型就會傳回其預測。

如需完整部署流程的詳細資訊，請參閱[部署 Azure Machine Learning Web 服務][deploy]

[deploy]: deploy-a-machine-learning-web-service.md

<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
