---
title: Azure VM 大小-儲存體 |Microsoft Docs
description: 列出 Azure 中適用于虛擬機器的不同儲存體優化大小。 列出 vCPU 數目、資料磁碟和 NIC 的相關資訊，以及此服務中各種大小之儲存體輸送量和網路頻寬的相關資訊。
ms.subservice: sizes
documentationcenter: ''
author: sasha-melamed
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 038433e3b83ba936c14142053b86e0a7b0ed9dc6
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87291064"
---
# <a name="storage-optimized-virtual-machine-sizes"></a>儲存體最佳化的虛擬機器大小

儲存體優化的 VM 大小提供高磁片輸送量和 IO，適用于大型資料、SQL、NoSQL 資料庫、資料倉儲和大型交易資料庫。  範例包括 Cassandra、MongoDB、Cloudera 和 Redis。 本文提供個 vcpu、資料磁片和 Nic 的數目，以及每個優化大小的本機儲存體輸送量和網路頻寬的相關資訊。

[Lsv2 系列](lsv2-series.md)具有高輸送量、低延遲、直接對應的本機 NVMe 儲存體，在[AMD EPYC<sup>TM</sup> 7551 處理器](https://www.amd.com/en/products/epyc-7000-series)上執行，且所有核心提升為 2.55 ghz，最大加速為 3.0 ghz。 Lsv2 系列 VM 提供採用同時多執行緒設定且具有 8 到 80 個 vCPU 的大小。  每一 vCPU 有 8 GiB 記憶體，每 8 個 vCPU 有一個 1.92 TB NVMe SSD M.2 裝置，在 L80s v2 上最高可達 19.2 TB (10 x 1.92 TB)。

## <a name="other-sizes"></a>其他大小

- [一般用途](sizes-general.md)
- [計算最佳化](sizes-compute.md)
- [記憶體最佳化](sizes-memory.md)
- [GPU 最佳化](sizes-gpu.md)
- [高效能計算](sizes-hpc.md)
- [前幾代](sizes-previous-gen.md)

## <a name="next-steps"></a>後續步驟

深入了解 [Azure 計算單位 (ACU)](acu.md) 如何協助您比較各個 Azure SKU 的計算效能。

瞭解如何在適用于[Windows](windows/storage-performance.md)或[Linux](linux/storage-performance.md)的 Lsv2 系列虛擬機器上優化效能。
