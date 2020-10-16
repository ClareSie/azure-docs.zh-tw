---
title: 適用于 Azure Functions 的 Azure 事件方格系結
description: 了解如何在 Azure Functions 中處理 Event Grid 事件。
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.custom: fasttrack-edit
ms.openlocfilehash: 115ff1b59ecbe3f4fdb089c2bd61da955fba3984
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2020
ms.locfileid: "92104541"
---
# <a name="azure-event-grid-bindings-for-azure-functions"></a>適用于 Azure Functions 的 Azure 事件方格系結

本參考會說明如何處理 Azure Functions 中的 [事件方格](../event-grid/overview.md) 事件。 如需如何在 HTTP 端點中處理事件方格訊息的詳細資訊，請參閱將 [事件接收到 HTTP 端點](../event-grid/receive-events.md)。

Event Grid 是一項 Azure 服務，會傳送 HTTP 要求通知您「發行者」** 中發生的事件。 發行者是產生事件的服務或資源。 例如，Azure Blob 儲存體帳戶即為發行者，而 [Blob 的上傳或刪除是事件](../storage/blobs/storage-blob-event-overview.md)。 部分 [Azure 服務內建有將事件發佈至 Event Grid 的支援](../event-grid/overview.md#event-sources)。

事件*處理常式*會接收及處理事件。 Azure Functions 是數個[有內建 Event Grid 事件處理支援的 Azure 服務](../event-grid/overview.md#event-handlers)之一。 在此參考中，您將瞭解如何使用事件方格觸發程式，在從事件方格接收事件時叫用函式，以及使用輸出系結將事件傳送至 [事件方格自訂主題](../event-grid/post-to-custom-topic.md)。

如果您想要的話，可以使用 HTTP 觸發程式來處理事件方格事件;請參閱 [將事件接收到 HTTP 端點](../event-grid/receive-events.md)。 目前，當事件在 [CloudEvents 架構](../event-grid/cloudevents-schema.md#azure-functions)中傳遞時，您無法使用 Azure Functions 應用程式的事件方格觸發程式。 相反地，請使用 HTTP 觸發程序。

| 動作 | 類型 |
|---------|---------|
| 在事件方格事件分派時執行函數 | [觸發程序](./functions-bindings-event-grid-trigger.md) |
| 傳送事件方格事件 |[輸出系結](./functions-bindings-event-grid-output.md) |

此參考中的程式碼預設為 .NET Core 語法，用於函數2.x 版和更新版本中。 如需 1.x 語法的資訊，請參閱 [1.x 函式範本](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates)。

## <a name="add-to-your-functions-app"></a>新增至您的函數應用程式

### <a name="functions-2x-and-higher"></a>Functions 2.x 和更新版本

使用觸發程式和系結會要求您參考適當的封裝。 NuGet 套件適用于 .NET 類別庫，而擴充功能配套則用於所有其他應用程式類型。

| 語言                                        | 加入者 .。。                                   | 備註 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | 安裝 [NuGet 套件]2.x 版 | |
| C # 腳本、JAVA、JavaScript、Python、PowerShell | 註冊[延伸]模組套件組合          | 建議搭配使用 [Azure Tools 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) 功能與 Visual Studio Code。 |
| C # 腳本 (online-僅適用于 Azure 入口網站)          | 新增系結                            | 若要更新現有的系結延伸模組，而不需要重新發佈函數應用程式，請參閱 [更新您的延伸]模組。 |

[core tools]: ./functions-run-local.md
[延伸模組套件組合]: ./functions-bindings-register.md#extension-bundles
[NuGet 套件]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid
[更新您的延伸模組]: ./functions-bindings-register.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

函式1.x 應用程式會自動 [參考 Nuget.exe NuGet](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) 套件2.x 版。

## <a name="next-steps"></a>後續步驟
* [在事件方格事件分派時執行函數](./functions-bindings-event-grid-trigger.md)
* [分派事件方格事件](./functions-bindings-event-grid-trigger.md)