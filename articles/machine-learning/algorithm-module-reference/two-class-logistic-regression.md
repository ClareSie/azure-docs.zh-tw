---
title: 雙類別羅吉斯回歸：模組參考
titleSuffix: Azure Machine Learning
description: 瞭解如何在 Azure Machine Learning 中使用兩個類別的羅吉斯回歸模組來建立羅吉斯回歸模型，以用來預測兩個（而且只有兩個）結果。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/22/2020
ms.openlocfilehash: d746b942f624fc83515ba29b0a092c2e592f1f25
ms.sourcegitcommit: 1ed0230c48656d0e5c72a502bfb4f53b8a774ef1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82137615"
---
# <a name="two-class-logistic-regression-module"></a>雙類別羅吉斯回歸模組

本文說明 Azure Machine Learning 設計工具（預覽）中的模組。

您可以使用此模組來建立羅吉斯回歸模型，以用來預測兩個（而且只有兩個）結果。 

羅吉斯回歸是知名的統計技術，可用來建立許多種類問題的模型。 此演算法是一種受*監督的學習*方法; 因此，您必須提供已包含結果的資料集來定型模型。  

### <a name="about-logistic-regression"></a>關於羅吉斯回歸  

羅吉斯回歸是統計資料中知名的方法，用來預測結果的機率，而且特別適合分類工作。 此演算法會藉由將資料與羅吉斯函式進行調整，來預測事件發生的機率。
  
在此課程模組中，分類演算法已針對二分或 binary 變數優化。 如果您需要分類多個結果，請使用[多元羅吉斯回歸](./multiclass-logistic-regression.md)模組。

##  <a name="how-to-configure"></a>如何設定  

若要定型此模型，您必須提供包含標籤或類別資料行的資料集。 因為此模組適用于雙類別的問題，所以標籤或類別資料行必須剛好包含兩個值。 

例如，[標籤] 資料行可能是 [投票]，可能的值為 [是] 或 [否]。 或者，它可能是 [信用風險]，可能的值為「高」或「低」。 
  
1.  將**兩個類別的羅吉斯回歸**模組新增至您的管線。  
  
2.  藉由設定 [**建立定型模式]** 選項，指定您要如何訓練模型。  
  
    -   **單一參數**：如果您知道要如何設定模型，可以提供一組特定值做為引數。  

    -   **參數範圍**：如果您不確定最佳參數，您可以使用[微調模型超參數](tune-model-hyperparameters.md)模組來尋找最佳參數。 您會提供某個範圍的值，而講師會逐一查看設定的多個組合，以判斷產生最佳結果的值組合。
  
3.  針對 [**優化容錯**]，指定優化模型時要使用的臨界值。 如果反覆運算之間的改進低於指定的臨界值，則會將演算法視為在解決方案上具有交集，而定型會停止。  
  
4.  針對 [ **l1 正規化權數**] 和 [ **l2 正規化權數**]，輸入要用於正規化參數 L1 和 l2 的值。 非零值建議用於兩者。  
     *正規化*是一種防止過度學習的方法，penalizing 具有極端係數值的模型。 正規化的運作方式是將與係數值相關聯的負面影響加入假設的錯誤中。 因此，具有極端係數值的精確模型會懲罰更多，但具有較保守值的較不精確模型會懲罰較少。  
  
     L1 與 L2 regularization 有不同的效果，並使用。  
  
    -   L1 可以套用到疏鬆的模型，使用高維度資料時，這是很有用。  
  
    -   相較之下，L2 regularization 是不是疏鬆的資料。  
  
     這個演算法支援 L1 和 L2 正規化值的線性組合：也就是說，如果<code>x = L1</code>和<code>y = L2</code>，則<code>ax + by = c</code>會定義正規化詞彙的線性範圍。  
  
    > [!NOTE]
    >  想要深入瞭解 L1 和 L2 正規化嗎？ 下列文章會討論 L1 和 L2 正規化的差異，以及它們如何影響模型的調整，並提供用於羅吉斯回歸和類神經網路模型的程式碼範例： [Machine Learning 的 l1 和 L2 正規化](https://msdn.microsoft.com/magazine/dn904675.aspx)  
    >
    > 已為羅吉斯回歸模型設計了 L1 和 L2 詞彙的不同線性組合：例如，[彈性網路正規化](https://wikipedia.org/wiki/Elastic_net_regularization)。 我們建議您參考這些組合，以定義在您的模型中有效的線性組合。
      
5.  針對**l-BFGS 的記憶體大小**，指定用於*l BFGS*優化的記憶體數量。  
  
     L BFGS 代表「有限的記憶體 Broyden-Fletcher-Goldfarb-Shanno」。 這是常用於參數估計的最佳化演算法。 此參數指出要儲存的通過位置和梯度數目，以計算下一個階梯。  
  
     此最佳化參數限制用來計算下一個步驟和方向的記憶體數量。 當您指定較少的記憶體時，訓練是更快，但較不精確。  
  
6.  針對 [**亂數字種子**]，輸入整數值。 如果您想要在相同管線的多個回合之間重現結果，則定義種子值是很重要的。  
  
  
8. 將加上標籤的資料集新增至管線，並將模型定型：

    + 如果您將 [**建立定型模式**] 設定為 [**單一參數**]，請連接已標記的資料集和 [[訓練模型](train-model.md)] 模組。  
  
    + 如果您將 [**建立定型模式**] 設定為 [**參數範圍**]，請連接已加上標籤的資料集，並使用[微調模型超參數](tune-model-hyperparameters.md)來定型模型。  
  
    > [!NOTE]
    > 
    > 如果您將參數範圍傳遞至[定型模型](train-model.md)，它只會使用單一參數清單中的預設值。  
    > 
    > 如果您將一組參數值傳遞至[微調模型超參數](tune-model-hyperparameters.md)模組，當它預期每個參數的設定範圍時，就會忽略這些值，並使用學習模組的預設值。  
    > 
    > 如果您選取 [**參數範圍**] 選項，並輸入任何參數的單一值，則會在整個清除中使用您指定的單一值，即使其他參數在某個範圍的值之間變更也是如此。  
  
9. 提交管線。  
  
## <a name="results"></a>結果

定型完成後：
 
  
+ 若要對新資料進行預測，請使用定型的模型和新的資料做為[評分模型](./score-model.md)模組的輸入。 


## <a name="next-steps"></a>後續步驟

請參閱可用來 Azure Machine Learning 的[模組集合](module-reference.md)。 