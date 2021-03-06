---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 11/22/2020
ms.topic: include
ms.openlocfilehash: 30de9181fd23a29b28973d01899f0b23fca3ae89
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96014565"
---
## <a name="availability"></a>可用性

|層面|詳細資料|
|----|:----|
|版本狀態：|正式上市 (GA)|
|定價：|**適用於容器登錄的 Azure Defender** 的計費方式如 [定價頁面](../articles/security-center/security-center-pricing.md) 所示|
|支援的登錄和映像：|ACR 登錄中的 Linux 映像可透過殼層存取從公用網際網路來存取|
|不支援的登錄和映像：|Windows 映像<br>「私人」登錄<br>具有存取權的登錄會受到防火牆、服務端點或私人端點 (例如 Azure Private Link) 的限制<br>極簡映像，例如 [Docker scratch](https://hub.docker.com/_/scratch/) 映像，或只包含應用程式及其執行階段相依性，但不含套件管理員、shell 或 OS 的「Distroless」映像|
|必要的角色和權限：|**安全性讀取者** 和 [Azure Container Registry 角色和權限](../articles/container-registry/container-registry-roles.md)|
|雲端：|:::image type="icon" source="../articles/security-center/media/icons/yes-icon.png" border="false"::: 商業雲端<br>:::image type="icon" source="../articles/security-center/media/icons/yes-icon.png" border="false"::: US Gov - 目前僅支援「推送時進行掃描」功能。 深入了解[何時會掃描映像？](../articles/security-center/defender-for-container-registries-introduction.md#when-are-images-scanned)<br>:::image type="icon" source="../articles/security-center/media/icons/no-icon.png" border="false":::中國 Gov、其他 Gov|
|||