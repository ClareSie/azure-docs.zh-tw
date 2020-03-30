---
title: Azure 批次處理池創建事件
description: Batch 池的引用創建事件，創建池後發出該事件。 記錄檔內容將公開集區的一 般資訊。
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: dea025b274278aa5fed2900c95b4a274541ffef9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022184"
---
# <a name="pool-create-event"></a>集區建立事件

 一旦集區建立完成，就會發出此事件。 記錄檔內容將公開集區的一 般資訊。 請注意，如果集區的目標大小大於 0 個計算節點，則集區調整開始事件將緊接在此事件之後。

 下面的示例顯示了使用`CloudServiceConfiguration`屬性創建的池創建事件的池的正文。

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Standard_F1s",
    "imageType": "VirtualMachineConfiguration",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "virtualMachineConfiguration": {
          "imageReference": {
            "publisher": " ",
            "offer": " ",
            "sku": " ",
            "version": " "
          },
          "nodeAgentId": " "
        },
    "resizeTimeout": "300000",
    "targetDedicatedNodes": 2,
    "targetLowPriorityNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|元素|類型|注意|
|-------------|----------|-----------|
|`id`|String|池的 ID。|
|`displayName`|String|集區顯示名稱。|
|`vmSize`|String|集區中虛擬機器的大小。 集區中所有虛擬機器的大小相同。 <br/><br/> 如需雲端服務集區 (使用 cloudServiceConfiguration 建立的集區) 的虛擬機器可用大小相關資訊，請參閱[雲端服務的大小](https://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/)。 Batch 支援 `ExtraSmall` 以外的所有雲端服務 VM 大小。<br/><br/> 如需使用 Virtual Machines Marketplace 中映像的集區 (使用 virtualMachineConfiguration 建立的集區) 其可用 VM 大小的相關資訊，請參閱[虛擬機器大小](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (英文) (Linux) 或 [Azure 中 Windows 虛擬機器的大小](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows)。 除了 `STANDARD_A0` 和進階儲存體的大小 (`STANDARD_GS`、`STANDARD_DS` 和 `STANDARD_DSV2` 系列) 以外，Batch 支援所有的 Azure VM 大小。|
|`imageType`|String|映射的部署方法。 支援的值是`virtualMachineConfiguration`或`cloudServiceConfiguration`|
|[`cloudServiceConfiguration`](#bk_csconf)|複雜類型|集區的雲端服務組態。|
|[`virtualMachineConfiguration`](#bk_vmconf)|複雜類型|集區的虛擬機器組態。|
|[`networkConfiguration`](#bk_netconf)|複雜類型|集區的網路組態。|
|`resizeTimeout`|Time|將計算節點配置至為集區上最後一次調整大小作業所指定的集區逾時。  (建立集區時的初始大小視為一次調整大小。)|
|`targetDedicatedNodes`|Int32|為池請求的專用計算節點數。|
|`targetLowPriorityNodes`|Int32|為池請求的低優先順序計算節點數。|
|`enableAutoScale`|Bool|指定集區大小是否隨著時間自動調整。|
|`enableInterNodeCommunication`|Bool|指定是否針對節點間的直接通訊設定集區。|
|`isAutoPool`|Bool|指定是否已透過作業的 AutoPool 機制建立集區。|
|`maxTasksPerNode`|Int32|可在集區內單一計算節點上並行執行的工作數目上限。|
|`vmFillType`|String|定義 Batch 服務如何在集區中的計算節點間散發工作。 有效值為 Spread 或 Pack。|

###  <a name="cloudserviceconfiguration"></a><a name="bk_csconf"></a>雲服務配置

|元素名稱|類型|注意|
|------------------|----------|-----------|
|`osFamily`|String|安裝在集區中虛擬機器上的 Azure 客體 OS 系列。<br /><br /> 可能的值包括：<br /><br /> **2** – OS 系列 2，相當於 Windows Server 2008 R2 SP1。<br /><br /> **3** – OS 系列 3，相當於 Windows Server 2012。<br /><br /> **4** – OS 系列 4，相當於Windows Server 2012 R2。<br /><br /> 如需詳細資訊，請參閱[客體 ​OS 發佈新聞](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases)。|
|`targetOSVersion`|String|安裝在集區中虛擬機器上的 Azure 客體 OS 版本。<br /><br /> 預設值為**\*** 指定指定族的最新作業系統版本。<br /><br /> 如需其他允許的值，請參閱[客體 OS 發佈新聞](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases)。|

###  <a name="virtualmachineconfiguration"></a><a name="bk_vmconf"></a> virtualMachineConfiguration

|元素名稱|類型|注意|
|------------------|----------|-----------|
|[`imageReference`](#bk_imgref)|複雜類型|指定要使用的平台或 Marketplace 映像的相關資訊。|
|`nodeAgentId`|String|佈建在計算節點上的 Batch 節點代理程式其 SKU。|
|[`windowsConfiguration`](#bk_winconf)|複雜類型|指定虛擬機器上的 Windows 作業系統設定。 如果 imageReference 正在參照 Linux 作業映像，則不得指定此屬性。|

###  <a name="imagereference"></a><a name="bk_imgref"></a>圖像參考

|元素名稱|類型|注意|
|------------------|----------|-----------|
|`publisher`|String|映像的發行者。|
|`offer`|String|映像的供應項目。|
|`sku`|String|映像的 SKU。|
|`version`|String|映像的版本。|

###  <a name="windowsconfiguration"></a><a name="bk_winconf"></a>視窗配置

|元素名稱|類型|注意|
|------------------|----------|-----------|
|`enableAutomaticUpdates`|Boolean|指出是否已針對自動更新啟用虛擬機器。 如果未指定此屬性，則預設值為 true。|

###  <a name="networkconfiguration"></a><a name="bk_netconf"></a>網路設定

|元素名稱|類型|注意|
|------------------|--------------|----------|
|`subnetId`|String|指定建立集區的計算節點所在的子網路其資源識別碼。|
