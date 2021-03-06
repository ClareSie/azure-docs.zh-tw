---
title: 範例模型
description: 列出範例模型的來源。
author: florianborn71
ms.author: flborn
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: 8e5dcb6c9dfa08efc0889fcab779d6cb333d2330
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "88507530"
---
# <a name="sample-models"></a>範例模型

本文列出一些可用於測試 Azure 遠端轉譯服務的範例資料資源。

## <a name="built-in-sample-model"></a>內建範例模型

我們提供內建範例模型，其一律可以使用 URL **builtin://Engine** 載入

![範例模型](./media/sample-model.png "範例模型")

模型統計資料：

| 名稱 | 值 |
|-----------|:-----------|
| [需要的伺服器大小](../how-tos/session-rest-api.md#create-a-session) | 標準 |
| 三角形數目 | 1870 萬 |
| 可移動的組件數目 | 2073 |
| 材質數目 | 94 |

## <a name="third-party-data"></a>第三方資料

Khronos 群組會維護一組用於測試的 glTF 範例模型。 ARR 支援文字 ( *.gltf*) 和二進位 ( *.glb*) 表單的 glTF 格式。 我們建議使用 PBR 模型來取得最佳的視覺效果：

* [glTF 範例模型](https://github.com/KhronosGroup/glTF-Sample-Models)

## <a name="next-steps"></a>後續步驟

* [快速入門：使用 Unity 轉譯模型](../quickstarts/render-model.md)
* [快速入門：轉換模型以進行轉譯](../quickstarts/convert-model.md)
