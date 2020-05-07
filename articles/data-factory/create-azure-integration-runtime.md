---
title: 在 Azure Data Factory 中建立 Azure 整合執行時間
description: 了解如何在 Azure Data Factory 中建立 Azure 整合執行階段，它可用來複製資料和分派轉換活動。
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/13/2020
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.openlocfilehash: 901761c8a5dc17cd7f00c3f04153c8e9aa99b648
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858024"
---
# <a name="how-to-create-and-configure-azure-integration-runtime"></a>如何建立和設定 Azure 整合執行階段
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

整合執行階段 (IR) 是 Azure Data Factory 所使用的計算基礎結構，可提供跨不同網路環境的資料整合功能。 如需 IR 的詳細資訊，請參閱[整合執行階段](concepts-integration-runtime.md)。

Azure IR 提供完全受控的計算，以原生方式執行資料移動並將資料轉換活動分派到計算服務 (例如 HDInsight)。 它裝載於 Azure 環境中，並且支援連線到具有可公開存取端點之公用網路環境中的資源。

本文件會介紹如何建立和設定 Azure 整合執行階段。 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-azure-ir"></a>預設 Azure IR
根據預設，每個資料處理站的後端都有 Azure IR，可支援在雲端資料存放區和在公用網路內計算服務的作業。 該 Azure IR 的位置是自動解析的。 如果連結的服務定義中並未指定 **connectVia** 屬性，則會使用預設的 Azure IR。 只有在您要明確定義 IR 位置，或是基於管理目的而以虛擬方式將不同 IR 上的活動執行作業分組時，才需要明確地建立 Azure IR。 

## <a name="create-azure-ir"></a>建立 Azure IR

若要建立和設定 Azure IR，您可以使用下列程式。

### <a name="create-an-azure-ir-via-azure-powershell"></a>透過 Azure PowerShell 建立 Azure IR
您可以使用**AzDataFactoryV2IntegrationRuntime** PowerShell Cmdlet 建立 Integration Runtime。 若要建立 Azure IR，您可以在命令中指定名稱、位置和類型。 以下是使用位置設定為 "West Europe" (歐洲西部) 來建立 Azure IR 的範例命令：

```powershell
Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```  
針對 Azure IR，類型必須設定為**受控**。 您不需要指定計算詳細資料，因為它在雲端中以彈性的方式受到完整管理。 當您要建立 Azure-SSIS IR 時，才需要指定例如節點大小和節點計數的計算詳細資料。 如需詳細資訊，請參閱[建立和設定 Azure-SSIS IR](create-azure-ssis-integration-runtime.md)。

您可以使用 AzDataFactoryV2IntegrationRuntime PowerShell Cmdlet 來設定現有的 Azure IR 來變更其位置。 如需 Azure IR 位置的詳細資訊，請參閱[整合執行階段簡介](concepts-integration-runtime.md)。

### <a name="create-an-azure-ir-via-azure-data-factory-ui"></a>透過 Azure Data Factory UI 建立 Azure IR
使用下列步驟，使用 Azure Data Factory UI 來建立 Azure IR。

1. 在 Azure Data Factory UI 的 **[開始使用] 頁面上**，選取左窗格中的 [**作者**] 索引標籤。

   ![首頁作者按鈕](media/doc-common-process/get-started-page-author-button.png)

1. 選取左窗格底部**的 [** **連接**]，然後選取 [連線] 視窗中的 [**整合運行**時間]。 選取 [ **+ 新增**]。

   ![建立整合執行階段](media/create-azure-integration-runtime/new-integration-runtime.png)

1. 在 [**整合執行時間設定**] 頁面上，選取 [ **Azure]、[自我**裝載]，然後選取 [**繼續**]。 

1. 在下列頁面上，選取 [ **Azure** ] 以建立 Azure IR，然後選取 [**繼續**]。
   ![建立整合執行階段](media/create-azure-integration-runtime/new-azure-integration-runtime.png)

1. 輸入 Azure IR 的名稱，然後選取 [**建立**]。
   ![建立 Azure IR](media/create-azure-integration-runtime/create-azure-integration-runtime.png)

1. 建立完成時，您會看到快顯通知。 在 [**整合運行**時間] 頁面上，確認您在清單中看到新建立的 IR。

## <a name="use-azure-ir"></a>使用 Azure IR

Azure IR 建立之後，您可以在「已連結的服務」定義中參考它。 以下是從 Azure 儲存體連結的服務參考上述所建立之 Azure 整合執行階段的方法範例：

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=myaccountname;AccountKey=..."
      },
      "connectVia": {
        "referenceName": "MySampleAzureIR",
        "type": "IntegrationRuntimeReference"
      }   
    }
}

```

## <a name="next-steps"></a>後續步驟
若要了解如何建立其他類型的整合執行階段，請參閱下列文章：

- [建立自我裝載整合執行時間](create-self-hosted-integration-runtime.md)
- [建立 Azure-SSIS 整合執行階段](create-azure-ssis-integration-runtime.md)
 
