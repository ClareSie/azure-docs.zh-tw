---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 95bd83575809f6ecda716ff751b47b7bb499cae3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85073242"
---
## <a name="configure-an-environment-variable-for-authentication"></a>設定用於驗證的環境變數

應用程式需要驗證其所使用之認知服務的存取權。 若要進行驗證，建議您建立環境變數來儲存 Azure 資源的金鑰。 

當您擁有金鑰之後，請將它寫入至執行應用程式的本機電腦上的新環境變數。 若要設定環境變數，請開啟主控台視窗，並遵循您的作業系統所適用的指示。 取代為 `your-key` 您的資源的其中一個金鑰。

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx COGNITIVE_SERVICE_KEY "your-key"
```

新增環境變數之後，您可能需要重新啟動任何需要讀取環境變數的執行中程式，包括主控台視窗。 例如，如果您使用 Visual Studio 做為編輯器，請在執行範例前重新啟動 Visual Studio。

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

新增環境變數之後，從主控台視窗執行 `source ~/.bashrc`，讓變更生效。

#### <a name="macos"></a>[macOS](#tab/unix)

編輯 .bash_profile，然後新增環境變數：

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

新增環境變數之後，從主控台視窗執行 `source ~/.bash_profile`，讓變更生效。

***
