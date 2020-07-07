---
title: 如何跨區域移動您的服務資源
titleSuffix: Azure Cognitive Search
description: 本文將說明如何在 Azure 雲端中，將您的 Azure 認知搜尋資源從一個區域移至另一個區域。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 03/24/2020
ms.openlocfilehash: 00f16d11f7a9cd276772eda5e91d6e117ada8c9f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "80246296"
---
# <a name="move-your-azure-cognitive-search-service-to-another-azure-region"></a>將您的 Azure 認知搜尋服務移至另一個 Azure 區域

客戶偶爾會詢問如何將搜尋服務移至另一個區域。 目前沒有任何內建機制或工具可協助執行這項工作，但本文可協助您瞭解達到相同結果的手動步驟。

> [!NOTE]
> 在 Azure 入口網站中，所有服務都有一個 [**匯出範本**] 命令。 在 Azure 認知搜尋的案例中，此命令會產生服務的基本定義（名稱、位置、層級、複本和分割區計數），但不會辨識服務的內容，也不會包含金鑰、角色或記錄。 雖然此命令存在，但我們不建議您使用它來移動搜尋服務。

## <a name="guidance-for-moving-a-service"></a>移動服務的指引

1. 識別相依性和相關服務，以瞭解重新放置服務的完整影響，以防您需要移動的不只是 Azure 認知搜尋。

   Azure 儲存體用於記錄、建立知識存放區，而且是常用的外部資料源，可供 AI 擴充和編制索引。 認知服務是 AI 擴充中的相依性。 如果您使用 AI 擴充，認知服務和您的搜尋服務都必須位於相同區域中。

1. 建立服務上所有物件的清查，讓您知道要移動的內容：索引、同義字對應、索引子、資料來源、技能集。 如果您啟用記錄功能，請建立並封存歷程記錄所需的任何報表。

1. 檢查新區域中的定價和可用性，以確保 Azure 認知搜尋的可用性以及新區域中的任何相關服務。 大部分的功能都適用于所有區域，但部分預覽功能的可用性有限。

1. 在新區域中建立服務，並從原始程式碼重新發佈任何現有的索引、同義字對應、索引子、資料來源和技能集。 請記住，服務名稱必須是唯一的，因此您無法重複使用現有的名稱。 檢查每個技能集，以查看與認知服務的連線是否仍然有效，而不是在相同區域的需求方面。 此外，如果建立了知識存放區，請檢查 Azure 儲存體的連接字串（如果您使用不同的服務）。

1. 重載索引和知識存放區（如果適用的話）。 您將使用應用程式程式碼將 JSON 資料推送至索引，或重新執行索引子以從外部來源提取檔。 

1. 啟用記錄功能，如果您使用它們，請重新建立安全性角色。

1. 更新用戶端應用程式和測試套件，以使用新的服務名稱和 API 金鑰，並測試所有應用程式。

1. 一旦新的服務經過完整測試和運作，請刪除舊的服務。

## <a name="next-steps"></a>後續步驟

下列連結可協助您在完成上面所述的步驟時，尋找詳細資訊。

+ [Azure 認知搜尋定價和區域](https://azure.microsoft.com/pricing/details/search/)
+ [選擇階層。](search-sku-tier.md)
+ [建立搜尋服務](search-create-service-portal.md)
+ [載入搜尋檔](search-what-is-data-import.md)
+ [啟用記錄](search-monitor-logs.md)


<!-- To move your Azure Cognitive Service account from one region to another, you will create an export template to move your subscription(s). After moving your subscription, you will need to move your data and recreate your service.

In this article, you'll learn how to:

> [!div class="checklist"]
> * Export a template.
> * Modify the template: adding the target region, search and storage account names.
> * Deploy the template to create the new search and storage accounts.
> * Verify your service status in the new region
> * Clean up resources in the source region.

## Prerequisites

- Ensure that the services and features that your account uses are supported in the target region.

- For preview features, ensure that your subscription is whitelisted for the target region. For more information about preview features, see [knowledge stores](https://docs.microsoft.com/azure/search/knowledge-store-concept-intro), [incremental enrichment](https://docs.microsoft.com/azure/search/cognitive-search-incremental-indexing-conceptual), and [private endpoint](https://docs.microsoft.com/azure/search/service-create-private-endpoint).

## Assessment and planning

When you move your search service to the new region, you will need to [move your data to the new storage service](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#configure-the-new-storage-account) and then rebuild your indexes, skillsets and knowledge stores. You should record current settings and copy json files to make the rebuilding of your service easier and faster.

## Moving your search service's resources

To start you will export and then modify a Resource Manager template.

### Export a template

1. Sign in to the [Azure portal](https://portal.azure.com).

2. Go to your Resource Group page.

> [!div class="mx-imgBorder"]
> ![Resource Group page example](./media/search-move-resource/export-template-sample.png)

3. Select **All resources**.

3. In the left hand navigation menu select **Export template**.

4. Choose **Download** in the **Export template** page.

5. Locate the .zip file that you downloaded from the portal, and unzip that file to a folder of your choice.

The zip file contains the .json files that comprise the template and scripts to deploy the template.

### Modify the template

You will modify the template by changing the search and storage account names and regions. The names must follow the rules for each service and region naming conventions. 

To obtain region location codes, see [Azure Locations](https://azure.microsoft.com/global-infrastructure/locations/).  The code for a region is the region name with no spaces, **Central US** = **centralus**.

1. In the Azure portal, select **Create a resource**.

2. In **Search the Marketplace**, type **template deployment**, and then press **ENTER**.

3. Select **Template deployment**.

4. Select **Create**.

5. Select **Build your own template in the editor**.

6. Select **Load file**, and then follow the instructions to load the **template.json** file that you downloaded and unzipped in the previous section.

7. In the **template.json** file, name the target search and storage accounts by setting the default value of the search and storage account names. 

8. Edit the **location** property in the **template.json** file to the target region for both your search and storage services. This example sets the target region to `centralus`.

```json
},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Search/searchServices",
            "apiVersion": "2020-03-13",
            "name": "[parameters('searchServices_target_region_search_name')]",
            "location": "centralus",
            "sku": {
                "name": "standard"
            },
            "properties": {
                "replicaCount": 1,
                "partitionCount": 1,
                "hostingMode": "Default"
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[parameters('storageAccounts_tagetstorageregion_name')]",
            "location": "centralus",
            "sku": {
                "name": "Standard_RAGRS",
                "tier": "Standard"
            },
```

### Deploy the template

1. Save the **template.json** file.

2. Enter or select the property values:

- **Subscription**: Select an Azure subscription.

- **Resource group**: Select **Create new** and give the resource group a name.

- **Location**: Select an Azure location.

3. Click the **I agree to the terms and conditions stated above** checkbox, and then click the **Select Purchase** button.

## Verifying your services' status in new region

To verify the move, open the new resource group and your services will be listed with the new region.

To move your data from your source region to the target region, please see this article's guidelines for [moving your data to the new storage account](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#move-data-to-the-new-storage-account).

## Clean up resources in your original region

To commit the changes and complete the move of your service account, delete the source service account.

## Next steps

[Create an index](https://docs.microsoft.com/azure/search/search-get-started-portal)

[Create a skillset](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob)

[Create a knowledge store](https://docs.microsoft.com/azure/search/knowledge-store-create-portal) -->