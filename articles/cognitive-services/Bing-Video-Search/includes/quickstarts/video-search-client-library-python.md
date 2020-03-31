---
title: Bing 影片搜尋 Python 用戶端程式庫快速入門
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.openlocfilehash: 7a9fab8ba8bb9d21c9284cbf14bc67226d2ef9d3
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80289904"
---
使用本快速入門，透過適用於 Python 的 Bing 影片搜尋用戶端程式庫開始搜尋新聞。 雖然 Bing 影片搜尋具有與大部分程式設計語言相容的 REST API，但此用戶端程式庫可提供簡單的方法，將服務整合到您的應用程式。 在 [Github](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) 上可找到此範例的完整原始程式碼，以及其他註釋和功能。

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>Prerequisites

- [Python](https://www.python.org/) 2.x 或 3.x
- 適用於 Python 的 Bing 影片搜尋用戶端程式庫

建議您使用 Python [虛擬環境](https://docs.python.org/3/tutorial/venv.html)。 您可以使用 [venv 模組](https://pypi.python.org/pypi/virtualenv)來安裝和初始化虛擬環境。 透過下列方式安裝適用於 Python 2.7 的 virtualenv：

```console
python -m venv mytestenv
```

透過下列方式安裝 Bing 影片搜尋用戶端程式庫：

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>建立應用程式並將其初始化

1. 在您慣用的 IDE 或編輯器中建立新的 Python 專案，以及新增下列匯入陳述式。 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchClient
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. 建立訂用帳戶金鑰的變數。 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    ```

## <a name="create-the-search-client"></a>建立搜尋用戶端

建立 `CognitiveServicesCredentials` 的執行個體，並具現化用戶端：

```python
client = VideoSearchAPI(endpoint, CognitiveServicesCredentials(subscription_key))
```

## <a name="send-a-search-request-and-get-a-response"></a>傳送搜尋要求並取得回應

1. 使用 `client.videos.search()` 與您的搜尋查詢，將要求傳送至 Bing 影片搜尋 API 並取得回應。

    ```python
    video_result = client.videos.search(query="SwiftKey")
    ```

2. 如果回應包含搜尋結果，則取得第一個結果，並列印其識別碼、名稱和 URL。

    ```python
    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")
    ```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [建立單頁 Web 應用程式](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>另請參閱 

- [什麼是 Bing 影片搜尋 API？](../../overview.md)
- [認知服務 .NET SDK 範例](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
