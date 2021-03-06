---
ms.openlocfilehash: eb6551a9ec9accd4a7ee270ec47794fdaed91a8a
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "88684159"
---
當您設定 Azure 資源時，系統會將高速公路車流的短片複製到 Azure 中您要作為 IoT Edge 裝置的 Linux VM。 本快速入門會使用影片檔案來模擬即時串流。

開啟應用程式，例如 [VLC media player](https://www.videolan.org/vlc/)。 選取 `Ctrl+N`，然後貼上[高速公路交叉口範例影片](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv)的連結以便開始播放。 您會看到許多車輛在高速公路車流中移動的畫面。

在本快速入門中，您將在 IoT Edge 上使用 Live Video Analytics 以偵測車輛和人員等物件。 您會將相關聯的推斷事件發佈到 IoT Edge 中樞。
