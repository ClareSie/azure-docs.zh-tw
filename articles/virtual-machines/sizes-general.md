---
title: Azure VM 大小-一般用途 |Microsoft Docs
description: 列出 Azure 中適用于虛擬機器的不同一般用途大小。 列出個 vcpu、資料磁片和 Nic 數目的相關資訊，以及此系列中大小的儲存體輸送量和網路頻寬。
services: virtual-machines
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/20/2020
ms.author: mimckitt
ms.openlocfilehash: fc263eb6fbe6c6402aaf529229bb7025f070b8d9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81269664"
---
# <a name="general-purpose-virtual-machine-sizes"></a>一般用途的虛擬機器大小

一般用途的虛擬機器大小可讓 CPU 與記憶體比例達到平均。 適用於測試和開發、小型至中型資料庫，以及低至中流量 Web 伺服器。 本文提供一般目的計算的供應專案相關資訊。

- [Av2 系列](av2-series.md)vm 可以部署在各種不同的硬體類型和處理器上。 A 系列 VM 的 CPU 效能及記憶體設定最適合初階的工作負載，例如開發及測試， 根據硬體節流大小，為執行中的執行個體提供一致的處理器效能，不論硬體部署的位置。 若要判斷此大小部署所在的實體硬體，請從虛擬機器內查詢虛擬硬體。 使用案例範例包括開發與測試伺服器、低流量網頁伺服器、中小型資料庫、概念證明以及程式碼存放庫。

  > [!NOTE]
  > A8 – A11 Vm 已規劃于3/2021 淘汰。 如需詳細資訊，請參閱[HPC 遷移指南](https://azure.microsoft.com/resources/hpc-migration-guide/)。

- [B 系列高載](sizes-b-series-burstable.md)Vm 適用于不需要持續完整 CPU 效能的工作負載，例如 web 伺服器、小型資料庫和開發和測試環境。 這些工作負載通常具有高載的效能需求。 B 系列讓這些客戶所購買到的 VM 大小具有價格公道的基準效能，可在 VM 利用低於其基底效能時，讓 VM 執行個體累積點數。 當 VM 累積點數時，VM 可以在您的應用程式需要較高的 CPU 效能時，使用最多 100% 的 CPU 來高載高於 VM 基準。

- [Dav4 和 Dasv4 系列](dav4-dasv4-series.md)是利用 AMD 的2.35 版 Ghz EPYC<sup>TM</sup> 7452 處理器的新大小，在多執行緒設定中，最多可256將 8 mb 的 l3 快取專門提供給每8個核心，而增加客戶選擇以執行其一般用途的工作負載。 Dav4 系列和 Dasv4 系列具有與 D & Dsv3 系列相同的記憶體和磁片設定。

- [DCv2 系列](dcv2-series.md)可協助保護您的資料和程式碼在公用雲端中進行處理時的機密性和完整性。 這些機器是以最新一代的 Intel 2288G 處理器和 SGX 技術為後盾。 使用 Intel Turbo 加速技術，這些機器最多可達 5.0 GHz。 DCv2 系列實例可讓客戶建立安全的記憶體保護區型應用程式，以在使用中時保護其程式碼和資料。

- [Dv2 和 Dsv2 系列](dv2-dsv2-series.md)Vm 是原始 D 系列的後續操作，具備更強大的 CPU 和最佳的 CPU 對記憶體設定，使其適用于大部分的生產工作負載。 Dv2 系列的速度比 D 系列快35%。 Dv2 系列執行于 Intel®的® 8171M 2.1 GHz （Skylake）、Intel®的® E5-2673 v4 2.3 GHz （Broadwell），或 Intel®的® E5-2673 v3 2.4 g h z （Haswell）處理器與 Intel Turbo 加速技術2.0。 Dv2 系列的記憶體和磁碟組態和 D 系列一樣。

- [Dv3 和 Dsv3 系列](dv3-dsv3-series.md)Vm 執行于 Intel®® 8171M 2.1 GHz （Skylake）、Intel®的2673® E5-v4 2.3 g h z （Broadwell），或超執行緒設定中的 Intel®高達® E5-2673 v3 2.4 GHz （Haswell）處理器，為大多數一般用途的工作負載提供更好的價值主張。 除了記憶體已擴充 (從 ~3.5 GiB/vCPU 到 4 GiB/vCPU)，磁碟和網路限制也已就個別核心進行調整，以符合移轉至超執行緒的需求。 Dv3 系列不再具有 D/Dv2 系列的高記憶體 VM 大小，而是已移至記憶體優化[Ev3 和 Esv3 系列](ev3-esv3-series.md)。

D 系列使用案例的範例包括企業級應用程式、關係資料庫、記憶體內部快取及分析。

## <a name="other-sizes"></a>其他大小

- [計算最佳化](sizes-compute.md)
- [記憶體最佳化](sizes-memory.md)
- [儲存體最佳化](sizes-storage.md)
- [GPU 最佳化](sizes-gpu.md)
- [高效能計算](sizes-hpc.md)
- [上一代](sizes-previous-gen.md)

## <a name="next-steps"></a>後續步驟

深入了解 [Azure 計算單位 (ACU)](acu.md) 如何協助您比較各個 Azure SKU 的計算效能。
