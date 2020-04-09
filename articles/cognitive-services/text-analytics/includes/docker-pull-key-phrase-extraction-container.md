---
title: 鍵短語提取容器的 Docker 拉拔
titleSuffix: Azure Cognitive Services
description: 鍵短文提取容器的 Docker 拉取指令
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: db5d0cfecde32d3dd6afe181d67a6ce6c0826eda
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876974"
---
#### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>鍵短語提取容器的 Docker 拉拔

使用[`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)命令從 Microsoft 容器註冊表下載容器映射。

有關文字分析容器可用標記的完整說明,請參閱 Docker Hub 上的[「關鍵短語提取」](https://go.microsoft.com/fwlink/?linkid=2018757)容器。

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```
