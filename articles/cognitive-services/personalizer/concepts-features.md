---
title: 功能：動作和內容-個人化工具
titleSuffix: Azure Cognitive Services
description: 個人化工具會使用特性 (即動作和內容的相關資訊) 來提出更好的排名建議。 特性可能是所有項目通用的，也可能為某個項目所特有。
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/14/2019
ms.openlocfilehash: edd1549ddabef0ae1ba37150ad75a371ac6e6d85
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2020
ms.locfileid: "94365511"
---
# <a name="features-are-information-about-actions-and-context"></a>特性是動作和內容的相關資訊

個人化工具服務會藉由學習應用程式在給定內容中應該對使用者顯示什麼內容，來發揮其作用。

個人化工具會使用 **特性** ，也就是用來選擇最佳 **動作** 的 **目前內容** 相關資訊。 特性代表您認為可能有助於提供個人化服務 (以便獲得更好的報酬) 的所有資訊。 特性可能是所有項目通用的，也可能為某個項目所特有。 

例如，您可能會有關於下列項目的 **特性** ：

* _使用者角色_ ，例如 `Sports_Shopper` 。 這應該不是個別的使用者識別碼。 
* 內容，例如某段影片是 `Documentary`、`Movie` 還是 `TV Series`，或是商店內是否有某個零售商品。
* 當前的一段時間，例如一週內的哪一天。

個人化工具不規定、限制或修正您可以針對動作和內容傳送哪些功能：

* 您可以針對某些動作傳送某些特性，至於其他您未擁有的動作，則不傳送。 例如，電視影集便可能沒有電影所擁有的屬性。
* 您可能會擁有某些只會在某些時候才出現的特性。 例如，行動應用程式可能會提供比網頁還要多的資訊。 
* 一段時間後，您可能會新增和移除關於內容和動作的特性。 個人化工具會繼續從可用資訊來學習。
* 內容必須至少有一個特性。 個人化工具不支援空白內容。 如果您每次只會傳送固定的內容，個人化工具便只會針對關於動作中特性的排名來選擇動作。
* 針對類別功能，您不需要定義可能的值，也不需要預先定義數值的範圍。

## <a name="supported-feature-types"></a>支援的特性類型

個人化工具支援字串、數字和布林值類型的特性。

### <a name="how-choice-of-feature-type-affects-machine-learning-in-personalizer"></a>功能類型的選擇如何影響個人化工具中的 Machine Learning

* **字串** ：對於字串類型，每個索引鍵和值的組合都會在個人化工具機器學習模型中建立新的權數。 
* **數值** ：當數位應依比例影響個人化結果時，您應該使用數值。 這與案例相依。 在簡化的範例中，例如將零售體驗個人化時，NumberOfPetsOwned 可能是數位的一項功能，因為您可能會想要讓具有2或3個寵物的人員影響個人化結果兩次或三次，最多可有1個寵物。 以數位單位為基礎，但意義不是線性的功能（例如年齡、溫度或個人高度）最適合編碼為字串，而且功能品質通常可以使用範圍來改善。 例如，Age 可能會編碼為 "Age"： "0-5"、"Age"： "6-10" 等等。
* 以值 "false" 傳送的 **布林** 值，就像它們根本沒有傳送一樣。

要求中應該省略不存在的特性。 請避免傳送 Null 值的特性，因為系統在將模型定型時會將其視為既存特性來進行處理，且認為其值為「Null」。

## <a name="categorize-features-with-namespaces"></a>使用命名空間來分類特性

個人化工具會將特性組織到各個命名空間。 您要決定應用程式中是否要使用命名空間，以及這些命名空間應當是什麼內容。 命名空間可用來群組關於類似主題的特性，也可以用來群組來自特定來源的特性。

以下是應用程式所用特性命名空間的範例：

* User_Profile_from_CRM
* 時間
* Mobile_Device_Info
* http_user_agent
* VideoResolution
* UserDeviceInfo
* 天氣
* Product_Recommendation_Ratings
* current_time
* NewsArticle_TextAnalytics

您可以遵循您自己的慣例來為特性命名空間命名，但前提是這些特性命名空間必須是有效的 JSON 索引鍵。 命名空間是用來將功能組織成不同的集合，以及區分具有類似名稱的功能。 您可以將命名空間視為新增至功能名稱的「前置詞」。 命名空間無法進行嵌套。


在下列 JSON 中，`user`、`state` 和 `device` 便是特性命名空間。 

> [!Note]
> 目前，我們強烈建議使用以 UTF-8 為基礎並以不同字母開頭的功能命名空間名稱。 例如，、 `user` `state` 和 `device` 開頭為 `u` 、 `s` 和 `d` 。 目前擁有具有相同第一個字元的命名空間，可能會導致用於機器學習的索引發生衝突。

JSON 物件可以包括嵌套的 JSON 物件和簡單的屬性/值。 只有當陣列專案是數位時，才可以包含陣列。 

```JSON
{
    "contextFeatures": [
        { 
            "user": {
                "profileType":"AnonymousUser",
                "latlong": [47.6, -122.1]
            }
        },
        {
            "state": {
                "timeOfDay": "noon",
                "weather": "sunny"
            }
        },
        {
            "device": {
                "mobile":true,
                "Windows":true
            }
        }
    ]
}
```

### <a name="restrictions-in-character-sets-for-namespaces"></a>命名空間的字元集限制

您用來命名命名空間的字串必須遵循一些限制： 
* 它不能是 unicode。
* 您可以使用部分可列印的符號，搭配代碼 < 256 作為命名空間名稱。 
* 您無法使用具有代碼的符號 < 32 (無法列印) 、32 (空間) 、58 (冒號) 、124 (管道) 和126–140。
* 它不能以底線 "_" 開頭，否則將會忽略該功能。

## <a name="how-to-make-feature-sets-more-effective-for-personalizer"></a>如何讓特性集合在個人化工具中發揮更好的效果

良好的特性集合可協助個人化工具學習如何預測可帶來最高報酬的動作。 

請考慮將遵循下列建議的特性傳送給個人化工具排名 API：

* 有足夠的特性可促成個人化。 如需更加精確的針對性內容，就需要提供更多的特性。

* 特性數量多到足以實現多樣化「密度」。 如果有許多項目群組到少數幾個貯體，便代表該特性的「密度」夠高。 例如，您可以將數千部影片分類為「長時間」(長度超過 5 分鐘) 和「短時間」(長度不到 5 分鐘)。 這會是「密度非常高」的特性。 另一方面，您也可以讓同樣的數千個項目具有稱為「標題」的屬性，如此一來，這些項目幾乎就不會有彼此相同的值。 這種特性的密度就非常低 (或是「疏鬆」)。  

具有高密度特性可協助個人化工具不斷類推地學習每一個項目。 但如果特性數量不多且密度過高，個人化工具就會嘗試在只有幾個貯體可供選擇的情況下，精確地鎖定內容。

### <a name="improve-feature-sets"></a>改善特性集合 

請執行離線評估來分析使用者行為。 這麼做可讓您查看過去的資料，以了解哪些特性對正面報酬的貢獻較大，哪些特性的貢獻較少。 您可以了解哪些特性有幫助，然後由您和您的應用程式尋找更好的特性來傳送給個人化工具，更進一步地改善結果。

以下幾節是為了改善傳送給個人化工具的功能所能進行的常見做法。

#### <a name="make-features-more-dense"></a>提供密度更高的特性

您可以藉由編輯特性集合，讓其規模變得更大以及讓密度變得更高或更低，來改善您的特性集合。

例如，單位計算至秒的時間戳記，便是非常疏鬆的特性。 將時間分類成「早晨」、「中午」、「下午」等等，可以讓特性變得更密集 (更有效)。

位置資訊通常也受益于建立更廣泛的分類。 例如，Latitude-Longitude 座標（例如 Lat：47.67402 ° N、Long：122.12154 ° W）太精確，而且會強制模型學習以相異維度表示的緯度和經度。 當您嘗試根據位置資訊來個人化時，它有助於將位置資訊群組在較大的磁區中。 簡單的方法是為 Lat-Long 的數位選擇適當的舍入精確度，並將緯度和經度變成一個字串，以將緯度和經度合併到「區域」中。 例如，在大約幾公里寬度的區域中表示 47.67402 N、Long：122.12154 ° W 的好方法是 "location"： "34.3，12.1"。


#### <a name="expand-feature-sets-with-extrapolated-information"></a>使用類推的資訊擴大特性集合

您也可以想想有沒有尚未探索到的屬性可以從您已經擁有的資訊來衍生，從而獲得更多的特性。 例如，在虛構的電影清單個人化中，週末與工作日是否可以 elicits 使用者的不同行為？ 您可以擴充時間來獲得「週末」或「平日」屬性。 與文化有關的國定假日是否會讓人們關注某些電影類型？ 例如，「萬聖節」屬性在與其相關的地點便很有用。 雨天是否可能會對許多人在選擇電影時造成重大影響？ 使用時間和地點，天氣服務便可以提供該資訊，您也可以將其新增為額外的特性。 

#### <a name="expand-feature-sets-with-artificial-intelligence-and-cognitive-services"></a>使用人工智慧和認知服務來擴大特性集合

人工智慧和已可運行的認知服務對個人化工具很有幫助。 

藉由使用人工智慧服務來前置處理您的項目，您就可以自動擷取出可能與個人化有所關聯的資訊。

例如：

* 您可以透過[影片索引器](https://azure.microsoft.com/services/media-services/video-indexer/)來執行電影檔案，以擷取場景元素、文字、情緒和其他眾多屬性。 然後讓這些屬性變得更密集，以反映原始項目的中繼資料所沒有的特徵。 
* 可以透過偵測物件來執行影像，透過情緒來執行臉部等等。
* 可以藉由擷取實體和情緒、使用 Bing 知識圖表來擴大實體等等，來增強文字中的資訊。

您還可以使用其他幾個 [Azure 認知服務](https://www.microsoft.com/cognitive-services)，例如

* [實體連結](../text-analytics/index.yml)
* [文字分析](../text-analytics/overview.md)
* [表情](../face/overview.md)
* [電腦視覺](../computer-vision/overview.md)

## <a name="actions-represent-a-list-of-options"></a>動作代表選項清單

每個動作：

* 具有 _事件_ 識別碼。 如果您已經有事件識別碼，您應該提交該識別碼。 如果您沒有事件識別碼，請不要傳送，個人化工具會為您建立一個，並在排名要求的回應中傳回。 識別碼與排名事件相關聯，而不是使用者。 如果您建立識別碼，GUID 的效果最佳。 
* 都有特性清單。
* 特性清單可以很大 (數百個項目)，但建議您評估各個特性是否有效，以移除對獲得報酬沒有用的特性。 
* **動作** 中的特性不一定會與個人化工具所用 **內容** 中的特性有任何關聯。
* 動作的特性可能會存在於某些動作中，但不存在於其他動作中。 
* 特定動作識別碼的特性可能只存在一天，之後就無法取得。 

個人化工具的機器學習演算法在擁有穩定的特性集合時，會表現得比較好，但如果特性集合會隨著時間而變化，排名呼叫就不會失敗。

請勿在為動作排名時傳入超過 50 個動作。 這些動作可能每一次都是相同的 50 個動作，也可能會改變。 例如，如果您的電子商務應用程式有內含 10,000 個項目的產品目錄，您就可以使用推薦或篩選引擎來判斷客戶可能喜歡的前 40 個項目，然後用個人化工具來找出目前內容下會產生最大報酬 (例如，使用者會新增到購物籃) 的項目。


### <a name="examples-of-actions"></a>動作的範例

您傳送給排名 API 的動作取決於您要嘗試個人化的項目。

以下是一些範例：

|用途|動作|
|--|--|
|將要在新聞網站上選出的條目個人化。|每個動作都是潛在的新聞條目。|
|將廣告放在網站上最佳的位置。|每個動作都會是版面配置或用來建立廣告版面配置 (例如，在頂端、在右邊、小圖、大圖) 的規則。|
|在購物網站上顯示個人化的推薦項目排名。|每個動作都是特定的產品。|
|建議使用者介面項目，例如要套用至特定相片的篩選條件。|每個動作可能是不同的篩選條件。|
|為聊天 Bot 選擇要用來釐清使用者意圖或提出建議動作的回應。|每個動作都是如何解讀回應的選項。|
|選擇要在搜尋結果清單頂端顯示的項目|每個動作都是排名最高的幾個搜尋結果其中之一。|


### <a name="examples-of-features-for-actions"></a>動作特性的範例

以下是動作特性的良好範例。 這些範例極大程度上會因為每個應用程式而有所不同。

* 具有動作特徵的特性。 例如，是電影還是電視影集？
* 關於使用者過去可能如何與此動作互動的特性。 例如，這部電影的觀看人數在人口統計 A 還是 B 中最多，這部電影一般不會播放超過一次。
* 與使用者如何「看到」動作的特徵有關的特性。 例如，縮圖中顯示的電影海報是否包含臉部、車輛或景色？

### <a name="load-actions-from-the-client-application"></a>從用戶端應用程式載入動作

一般來說，動作中的特性可能來自內容管理系統、目錄和推薦系統。 應用程式會負責從您擁有的相關資料庫和系統載入動作的相關資訊。 如果動作不會變更或是每次都載入動作會對效能造成不必要的影響，您便可以在應用程式中新增邏輯來快取這項資訊。

### <a name="prevent-actions-from-being-ranked"></a>防止動作進行排名

在某些情況下，您可能會有不想對使用者顯示的動作。 若要防止某個動作成為排名第一的項目，您最好不要將其以首位納入到要傳送給排名 API 的動作清單。

在某些情況下，您的商務邏輯只能在後面才判斷是否要對使用者顯示排名 API 呼叫所產生的「動作」。 針對這些情況，請使用「非作用中事件」。

## <a name="json-format-for-actions"></a>動作的 JSON 格式

在呼叫排名時，您會傳送多個動作以供選擇：

JSON 物件可以包括嵌套的 JSON 物件和簡單的屬性/值。 只有當陣列專案是數位時，才可以包含陣列。 

```json
{
    "actions": [
    {
      "id": "pasta",
      "features": [
        {
          "taste": "salty",
          "spiceLevel": "medium",
          "grams": [400,800]
        },
        {
          "nutritionLevel": 5,
          "cuisine": "italian"
        }
      ]
    },
    {
      "id": "ice cream",
      "features": [
        {
          "taste": "sweet",
          "spiceLevel": "none",
          "grams": [150, 300, 450]
        },
        {
          "nutritionalLevel": 2
        }
      ]
    },
    {
      "id": "juice",
      "features": [
        {
          "taste": "sweet",
          "spiceLevel": "none",
          "grams": [300, 600, 900]
        },
        {
          "nutritionLevel": 5
        },
        {
          "drink": true
        }
      ]
    },
    {
      "id": "salad",
      "features": [
        {
          "taste": "salty",
          "spiceLevel": "low",
          "grams": [300, 600]
        },
        {
          "nutritionLevel": 8
        }
      ]
    }
  ]
}
```

## <a name="examples-of-context-information"></a>內容資訊的範例

「內容」的資訊取決於每個應用程式和使用案例，但一般來說，可能包含的資訊如下：

* 關於使用者的人口統計和設定檔資訊。
* 從 HTTP 標頭中擷取的資訊 (例如，使用者代理程式)，或衍生自 HTTP 資訊的資訊 (例如，根據 IP 位址的反向地理對應)。
* 當前時間的相關資訊，例如星期幾、是否為週末、上午或下午、是否為假期等等。
* 從行動應用程式擷取的資訊，例如位置、移動或電量。
* 使用者行為的歷史彙總，例如這位使用者之前最常看的電影內容類型。

應用程式會負責從您可能擁有的相關資料庫、感應器和系統載入內容的相關資訊。 如果內容資訊不會變更，您便可以在應用程式中新增邏輯來快取這項資訊，再將資訊傳送給排名 API。

## <a name="json-format-for-context"></a>內容的 JSON 格式 

內容會以傳送給排名 API 的 JSON 物件來表示：

JSON 物件可以包括嵌套的 JSON 物件和簡單的屬性/值。 只有當陣列專案是數位時，才可以包含陣列。 

```JSON
{
    "contextFeatures": [
        { 
            "user": {
                "name":"Doug"
            }
        },
        {
            "state": {
                "timeOfDay": "noon",
                "weather": "sunny"
            }
        },
        {
            "device": {
                "mobile":true,
                "Windows":true,
                "screensize": [1680,1050]
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>後續步驟

[增強式學習](concepts-reinforcement-learning.md)