---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: fdcb5bb57afe145b57cb1223242d4d5625a2dfb7
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400674"
---
需要語音資源訂用帳戶金鑰和區域，才能建立語音設定物件。 需要設定物件，才能具現化語音辨識器物件。

辨識器執行個體會公開多種辨識語音的方式。 在此範例中，語音會辨識出一次。 此功能可讓語音服務知道您要傳送單一片語以進行辨識，且一旦識別出該片語即停止辨識語音。 產生結果後，程式碼就會將辨識原因寫入至主控台。

> [!TIP]
> 語音 SDK 會預設為使用 `en-us` 來辨識語言，如需選擇來源語言的詳細資訊，請參閱[指定語音轉換文字的來源語言](../../../how-to-specify-source-language.md)。