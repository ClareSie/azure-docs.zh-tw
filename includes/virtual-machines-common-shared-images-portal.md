---
title: 包含檔案
description: 包含檔案
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/06/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 1af80f208c72af9434d596f2c1219c08c0e3f719
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96015978"
---
## <a name="create-an-image-gallery"></a>建立映像資源庫

映像資源庫是用於啟用映像共用的主要資源。 資源庫名稱允許的字元為大寫或小寫字母、數字、點和句點。 資源庫名稱不能包含連字號。  資源庫名稱在您的訂用帳戶內必須是唯一的。 

下列範例會在 *myGalleryRG* 資源群組中建立名為 *myGallery* 的資源庫。

1. 在 https://portal.azure.com 登入 Azure 入口網站。
1. 使用 [搜尋] 方塊中的 [類型 **共用映射庫** ]，然後在結果中選取 [ **共用映射庫** ]。
1. 在 [ **共用映射庫** ] 頁面中，按一下 [ **新增**]。
1. 在 [ **建立共用映射庫** ] 頁面上，選取正確的訂用帳戶。
1. 在 [ **資源群組**] 中，選取 [ **建立新** 的]，並輸入 *myGalleryRG* 作為 [名稱]。
1. 在 [ **名稱**] 中，輸入 *>mygalleryrg* 作為資源庫的名稱。
1. 保留 [ **區域**] 的預設值。
1. 您可以輸入資源庫的簡短描述，例如 *用於測試的影像庫。* 然後按一下 [ **審核 + 建立**]。
1. 通過驗證之後，請選取 [ **建立**]。
1. 部署完成之後，請選取 [移至資源]。


## <a name="create-an-image-definition"></a>建立映像定義 

映像定義會建立映像的邏輯群組。 並且可用來管理在其中建立的映像版本相關資訊。 映像定義名稱可以由大寫或小寫字母、數字、點、虛線和句點組成。 若要深入了解您可以為映像定義指定哪些值，請參閱[映像定義](../articles/virtual-machines/windows/shared-image-galleries.md#image-definitions)。

在您的資源庫內建立資源庫映射定義。 在此範例中，資源庫影像的名稱是 *myImageDefinition*。

1. 在新映射庫的頁面上，選取頁面頂端的 [新增 **映射定義** ]。 
1. 在 [ **將新的映射定義新增至共用映射庫**] 中，針對 [ **區域**] 選取 [ *美國東部*]。
1. 在 [ **映射定義名稱**] 中，輸入 *myImageDefinition*。
1. 針對 [ **作業系統**]，請根據您的來源 VM 選取正確的選項。  
1. 針對 **vm 產生**，請根據您的來源 VM 選取選項。 在大部分的情況下，這會是 *Gen 1*。 如需詳細資訊，請參閱 [第2代 Vm 支援](../articles/virtual-machines/generation-2.md)。
1. 針對 [ **作業系統狀態**]，選取以您的來源 VM 為基礎的選項。 如需詳細資訊，請參閱 [一般化和特製](../articles/virtual-machines/linux/shared-image-galleries.md#generalized-and-specialized-images)化。
1. 若為 **發行者**，請輸入 *myPublisher*。 
1. 在 [ **供應** 專案] 中輸入 *myOffer*。
1. 針對 [ **SKU**]，輸入 *mySKU*。
1. 完成時，選取 [ **審核 + 建立**]。
1. 映射定義通過驗證之後，請選取 [ **建立**]。
1. 部署完成之後，請選取 [移至資源]。


## <a name="create-an-image-version"></a>建立映像版本

從受控映射建立映射版本。 在此範例中，映像版本為 *1.0.0*，且它會被複寫到「美國中西部」和「美國中南部」資料中心。 選擇要複寫的目的地區域時，請記住，您也必須包含 *來源* 區域做為複寫目標。

映像版本允許的字元是數字及句點。 數字必須在 32 位元整數的範圍內。 格式：*MajorVersion*.*MinorVersion*.*Patch*。

根據來源是一般化映射或特製化 VM 的快照集，建立映射版本的步驟會稍有不同。 

### <a name="option-generalized"></a>選項：一般化

1. 在您映射定義的頁面中，選取頁面頂端的 [ **新增版本** ]。
1. 在 [ **區域**] 中，選取您的受控映射儲存所在的區域。 映射版本必須建立在與建立它們的受控映射相同的區域中。
1. 針對 [ **名稱**]，輸入 *1.0.0*。 映射版本名稱應遵循 *主要*。*次要*。使用整數的 *patch* 格式。 
1. 在 [ **來源映射**] 中，從下拉式清單中選取您的來源受控映射。
1. **從最新** 的 [排除] 中，保留預設值 [*否*]。
1. 在 [ **生命循環結束日期**] 中，從日曆選取未來幾個月的日期。
1. 在 **[複寫] 中，** 保留 **預設複本計數** 為1。 您必須複寫至來源區域，因此請將第一個複本保留為預設值，然後挑選第二個複本區域作為 *美國東部*。
1. 當您完成時，選取 [檢閱 + 建立]。 Azure 會驗證設定。
1. 當映射版本通過驗證時，請選取 [ **建立**]。
1. 部署完成之後，請選取 [移至資源]。

將映像複寫到所有目的地區域可能需要一些時間。

### <a name="option-specialized"></a>選項：特製化

1. 在您映射定義的頁面中，選取頁面頂端的 [ **新增版本** ]。
1. 在 [ **區域**] 中，選取儲存快照集的區域。 映射版本必須建立在與建立來源來源相同的區域中。
1. 針對 [ **名稱**]，輸入 *1.0.0*。 映射版本名稱應遵循 *主要*。*次要*。使用整數的 *patch* 格式。 
1. 在 [ **作業系統磁片快照**] 中，從下拉式清單中選取來源 VM 的快照集。 如果您的來源 VM 具有您想要包含的資料磁片，請從下拉式清單中選取正確的 **LUN** 編號，然後選取資料 **磁片快照** 集的資料磁片快照集。 
1. **從最新** 的 [排除] 中，保留預設值 [*否*]。
1. 在 [ **生命循環結束日期**] 中，從日曆選取未來幾個月的日期。
1. 在 **[複寫] 中，** 保留 **預設複本計數** 為1。 您必須複寫至來源區域，因此請將第一個複本保留為預設值，然後挑選第二個複本區域作為 *美國東部*。
1. 當您完成時，選取 [檢閱 + 建立]。 Azure 會驗證設定。
1. 當映射版本通過驗證時，請選取 [ **建立**]。
1. 部署完成之後，請選取 [移至資源]。

## <a name="share-the-gallery"></a>共用資源庫

我們建議您在映像庫層級上共用存取權。 下列步驟將逐步引導您共用剛才建立的資源庫。

1. 開啟 [Azure 入口網站](https://portal.azure.com)。
1. 在左側功能表中，選取 [ **資源群組**]。 
1. 在資源群組清單中，選取 [ **myGalleryRG**]。 資源群組的分頁隨即開啟。
1. 在 [ **myGalleryRG** ] 頁面左側的功能表中，選取 [ **存取控制] (IAM)**。 
1. 在 [ **新增角色指派**] 底下，選取 [ **新增**]。 [ **新增角色指派** ] 窗格隨即開啟。 
1. 在 [ **角色**] 底下，選取 [ **讀取** 者]。
1. 在 [ **指派存取權**] 底下，保留 **Azure AD 使用者、群組或服務主體** 的預設值。
1. 在 [ **選取**] 下，輸入您想要邀請之人員的電子郵件地址。
1. 如果使用者在您的組織外部，您將會看到訊息 **：此使用者將會收到一封電子郵件，讓他們能夠與 Microsoft 共同作業。** 選取具有電子郵件地址的使用者，然後按一下 [ **儲存**]。

如果使用者在您的組織外部，他們會收到加入組織的電子郵件邀請。 使用者必須接受邀請，然後才能在其資源清單中看到資源庫和所有影像定義和版本。