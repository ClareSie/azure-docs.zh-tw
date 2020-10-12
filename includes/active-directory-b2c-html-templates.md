---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: d43b879057001d62ea72bd2e011ad52957d47470
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "78189004"
---
## <a name="sample-templates"></a>範例範本
您可以在此找到 UI 自訂的範例範本：

```bash
git clone https://github.com/Azure-Samples/Azure-AD-B2C-page-templates
```

此專案包含下列範本：
- [海洋藍](https://github.com/Azure-Samples/Azure-AD-B2C-page-templates/tree/master/ocean_blue)
- [石板灰色](https://github.com/Azure-Samples/Azure-AD-B2C-page-templates/tree/master/slate_gray)

若要使用範例：

1. 在本機電腦上複製存放庫。 選擇範本資料夾 `/ocean_blue` 或 `/slate_gray` 。
1. 將範本資料夾和資料夾下的所有檔案上傳 `/assets` 至 Blob 儲存體（如上一節所述）。
1. 接下來，開啟 `\*.html` 或的根目錄中的每個檔案 `/ocean_blue` `/slate_gray` ，將相對 url 的所有實例取代為您在步驟2中上傳的 css、影像和字型檔案的 url。 例如：
    ```html
    <link href="./css/assets.css" rel="stylesheet" type="text/css" />
    ```

    收件者
    ```html
    <link href="https://your-storage-account.blob.core.windows.net/your-container/css/assets.css" rel="stylesheet" type="text/css" />
    ```
1. 儲存檔案 `\*.html` ，並將它們上傳至 Blob 儲存體。
1. 現在修改原則，指向您的 HTML 檔案（如先前所述）。
1. 如果您看到遺漏字型、影像或 CSS，請檢查延伸模組原則和 .html 檔案中的參考 \* 。
