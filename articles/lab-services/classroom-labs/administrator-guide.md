---
title: Azure 實驗室服務-系統管理員指南 |Microsoft Docs
description: 本指南可協助使用 Azure 實驗室服務建立及管理實驗室帳戶的系統管理員。
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2019
ms.author: spelluru
ms.openlocfilehash: 307ca08e733417efc9496415a09a0898fe10393e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82183461"
---
# <a name="azure-lab-services---administrator-guide"></a>Azure 實驗室服務-系統管理員指南
管理大學雲端資源的資訊技術（IT）系統管理員通常負責為其學校設定實驗室帳戶。 設定實驗室帳戶之後，系統管理員或教師會建立包含在實驗室帳戶中的教室實驗室。 本文提供相關 Azure 資源的高階總覽，以及建立它們的指引。

![實驗室帳戶中 Azure 資源的高層級觀點](../media/administrator-guide/high-level-view.png)

- 教室實驗室裝載于 Azure 實驗室服務所擁有的 Azure 訂用帳戶內。
- 實驗室帳戶、共用映射資源庫和映射版本都裝載于您的訂用帳戶內。
- 您可以將您的實驗室帳戶和共用映射庫放在相同的資源群組中。 在此圖中，它們位於不同的資源群組中。 

## <a name="subscription"></a>訂用帳戶
您的大學有一或多個 Azure 訂用帳戶。 訂用帳戶可用來管理其中所使用之所有 Azure resources\services 的計費和安全性，包括實驗室帳戶。

實驗室帳戶和其訂用帳戶之間的關聯性很重要，因為：

- 帳單是透過包含實驗室帳戶的訂用帳戶來回報。
- 您可以為訂用帳戶的 Azure Active Directory （AD）租使用者提供 Azure 實驗室服務的存取權。 您可以將使用者新增為實驗室帳戶 owner\contributor、教室實驗室建立者或教室實驗室擁有者。

在 Azure 實驗室服務所擁有的訂用帳戶中，會為您管理及託管教室實驗室及其虛擬機器（Vm）。

## <a name="resource-group"></a>資源群組
訂用帳戶包含一或多個資源群組。 資源群組是用來建立 Azure 資源的邏輯分組，以在相同的解決方案中一起使用。  

當您建立實驗室帳戶時，您必須設定包含實驗室帳戶的資源群組。 

建立[共用映射庫](#shared-image-gallery)時，也需要資源群組。 您可以選擇將您的實驗室帳戶和共用映射庫放在兩個不同的資源群組中，如果您打算跨不同的解決方案共用映射庫，這是一般情況。 或者，您可以選擇將它們放在相同的資源群組中。

當您建立實驗室帳戶時，您可以同時自動建立和附加共用映射資源庫。  此選項會導致實驗室帳戶和共用映射資源庫在不同的資源群組中建立。 使用本教學課程中所述的步驟時，您會看到這項行為：在[建立實驗室帳戶時設定共用映射資源庫](how-to-attach-detach-shared-image-gallery.md#configure-at-the-time-of-lab-account-creation)。 本文頂端的影像也會使用此設定。 

我們建議您事先投入時間來規劃您的資源群組結構，因為一旦建立實驗室帳戶或共用映射庫的資源群組後，就*無法*變更它。 如果您需要變更這些資源的資源群組，您必須刪除並重新建立您的實驗室帳戶和/或共用映射資源庫。

## <a name="lab-account"></a>實驗室帳戶

實驗室帳戶可作為一或多個教室實驗室的容器。 開始使用 Azure 實驗室服務時，通常只有一個實驗室帳戶。 當您的實驗室使用方式調整規模時，您稍後可以選擇建立更多的實驗室帳戶。

下列清單將重點放在多個實驗室帳戶可能有所説明的案例：

- **跨教室實驗室管理不同的原則需求**

    當您設定實驗室帳戶時，您可以設定套用至實驗室帳戶下*所有*教室實驗室的原則，例如：
    - 具有教室實驗室可存取之共用資源的 Azure 虛擬網路。 例如，您可能有一組教室實驗室需要存取虛擬網路中的共用資料集。
    - 課堂實驗室可用來建立 Vm 的虛擬機器（VM）映射。 例如，您可能有一組需要存取 Linux Marketplace 映射[資料科學 VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804)的教室實驗室。

    如果您的教室實驗室有彼此之間的獨特原則需求，則建立個別的實驗室帳戶來分別管理這些教室實驗室可能會很有説明。

- **依實驗室帳戶區分預算**
  
    除了透過單一實驗室帳戶回報所有教室實驗室成本，您可能需要更清楚的預算。 例如，您可以為大學的數學部門、電腦科學部門等建立實驗室帳戶，以區隔各部門的預算。  接著，您可以使用[Azure 成本管理](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)來查看每個個別實驗室帳戶的成本。

- **隔離 active\production labs 的試驗實驗室**
  
    在某些情況下，您可能會想要試驗實驗室帳戶的原則變更，而不會影響 active\production labs。 在這種情況下，為試驗目的建立個別的實驗室帳戶，可讓您隔離變更。 

## <a name="classroom-lab"></a>教室實驗室

教室實驗室包含每個指派給單一學生的虛擬機器（Vm）。  一般來說，您可以預期：

- 針對每個類別都有一個教室實驗室。
- 每半年建立一組新的教室實驗室（或針對您的類別所提供的每個時間範圍）。 通常針對具有相同映射需求的類別，您應該使用[共用映射資源庫](#shared-image-gallery)，跨實驗室和學期重複使用映射。

在決定如何結構您的教室實驗室時，請考慮下列幾點：

- **教室實驗室中的所有 Vm 都會使用已發佈的相同映射進行部署**

    因此，如果您的類別需要同時發佈不同的實驗室映射，則必須為每個課程建立個別的教室實驗室。
  
- **使用量配額是在實驗室層級設定，並適用于實驗室內的所有使用者**

    若要為使用者設定不同的配額，您必須建立個別的教室實驗室。 不過，在您設定配額之後，可以將更多小時加入特定的使用者。
  
- **啟動或關閉排程是在實驗室層級設定，並適用于實驗室內的所有 Vm**

    類似于上一個點，如果您需要為使用者設定不同的排程，您必須建立個別的教室實驗室。

根據預設，每個教室實驗室都會有自己的虛擬網路。  如果您已啟用 vnet 對等互連，每個教室實驗室將會有自己的子網對等互連至指定的虛擬網路。

## <a name="shared-image-gallery"></a>共用映射資源庫

共用映射資源庫會附加至實驗室帳戶，並做為儲存映射的中央儲存機制。 當教育者選擇從教室實驗室的範本虛擬機器（VM）匯出時，映射會儲存在圖庫中。 每次教育者變更範本 VM 和匯出時，會儲存新版本的映射，同時維持舊版。

講師可以在建立新的教室實驗室時，從共用映射庫發佈映射版本。 雖然圖庫會儲存影像的多個版本，但教師只能在建立實驗室時選取最新版本。

共用映射庫是一種選擇性的資源，您可能不需要在只有幾個教室實驗室的情況之下立即開始使用。 不過，使用「共用映射庫」有許多優點，可在您調整以擁有更多教室實驗室時有所説明：

- **可讓您儲存及管理範本 VM 映射的版本**

    建立自訂映射或對公用 Marketplace 資源庫中的映射進行變更（軟體、設定等等）非常有用。  例如，教育工作者通常需要安裝不同的 software\tooling。 不同版本的範本 VM 映射可以匯出至共用映射資源庫，而不需要學生自行手動安裝這些必要條件。 這些映射版本可以在建立新的教室實驗室時使用。
- **啟用跨教室實驗室的範本 VM 映射 sharing\reuse**

    您可以儲存並重複使用映射，如此一來，您就不需要在每次建立新的教室實驗室時從頭設定映射。 例如，如果提供的多個類別需要相同的映射，則此映射只需要建立一次，並匯出至共用映射資源庫，才能在教室實驗室中共用。
- **透過複寫來確保映射可用性**

    當您從教室實驗室儲存到共用映射資源庫時，您的影像會自動複寫到[相同地理位置內](https://azure.microsoft.com/global-infrastructure/regions/)的其他區域。 在某個區域發生中斷的情況下，將映射發佈到您的教室實驗室並不會受到影響，因為可以使用來自另一個區域的映射複本。  從多個複本發佈 Vm 也有助於提高效能。

若要以邏輯方式將共用映射分組，您有幾個選項：

- 建立多個共用映射資源庫。 每個實驗室帳戶只能連接到一個共用映射資源庫，因此，此選項也會要求您建立多個實驗室帳戶。
- 或者，您可以使用由多個實驗室帳戶共用的單一共用映射資源庫。 在此情況下，每個實驗室帳戶只能啟用適用于其所包含之教室實驗室的映射。

## <a name="naming"></a>命名

當您開始使用 Azure 實驗室服務時，我們建議您為資源群組、實驗室帳戶、教室實驗室和共用映射資源庫建立命名慣例。 雖然您所建立的命名慣例會因貴組織的需求而不同，但下表概述一般方針。

| 資源類型 | [角色] | 建議模式 | 範例 |
| ------------- | ---- | ----------------- | -------- | 
| 資源群組 | 包含一或多個實驗室帳戶，以及一或多個共用映射資源庫 | \<組織簡短名稱\>-\<環境\>-rg<ul><li>**組織簡短名稱**會識別資源群組支援的組織名稱</li><li>**環境**會識別資源的環境，例如試驗或生產</li><li>**Rg**代表資源類型：資源群組。</li></ul> | contosouniversitylabs-rg<br/>contosouniversitylabs-試驗-rg<br/>contosouniversitylabs-生產-rg |
| 實驗室帳戶 | 包含一或多個實驗室 | \<組織簡短名稱\>-\<環境\>-la<ul><li>**組織簡短名稱**會識別資源群組支援的組織名稱</li><li>**環境**會識別資源的環境，例如試驗或生產</li><li>**La**代表資源類型：實驗室帳戶。</li></ul> | contosouniversitylabs-la<br/>mathdeptlabs-la<br/>sciencedeptlabs-試驗-la<br/>sciencedeptlabs-生產-la |
| 教室實驗室 | 包含一或多個 Vm |\<類別名稱\>-\<時間\>-範圍\<教育者識別碼\><ul><li>[**類別名稱**] 可識別實驗室支援的類別名稱。</li><li>**時間範圍**可識別提供類別的時間範圍。</li>**教育識別碼**可識別擁有實驗室的教育者。</li></ul> | CS1234-fall2019-johndoe<br/>CS1234-spring2019-johndoe |
| 共用映射資源庫 | 包含一或多個 VM 映射版本 | \<組織簡短名稱\>資源庫 | contosouniversitylabsgallery |

如需有關命名其他 Azure 資源的詳細資訊，請參閱[Azure 資源的命名慣例](/azure/architecture/best-practices/naming-conventions)。

## <a name="regionslocations"></a>Regions\locations

設定 Azure 實驗室服務的資源時，您必須提供將裝載資源之資料中心的區域（或位置）。 以下是區域如何影響與設定實驗室相關的每個資源的詳細資料。

### <a name="resource-group"></a>資源群組

區域會指定儲存資源群組相關資訊的資料中心。 包含在資源群組中的 Azure 資源可以位於與其父系不同的區域中。

### <a name="lab-account"></a>實驗室帳戶

實驗室帳戶的位置會指出此資源所在的區域。  

### <a name="classroom-lab"></a>教室實驗室

教室實驗室存在的位置會根據下列因素而有所不同：

  - **實驗室帳戶已對等互連至虛擬網路（VNet）**
  
    當您的實驗室帳戶位於相同區域時[，可以使用 VNet 進行對等互連](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network)。  使用 VNet 對等互連實驗室帳戶時，會在與實驗室帳戶和 VNet 相同的區域中自動建立教室實驗室。

    > [!NOTE]
    > 使用 VNet 對等互連實驗室帳戶時，會停用 [**允許實驗室建立者選擇實驗室位置**] 的設定。 如需此設定的詳細資訊，請參閱下列文章：[允許實驗室建立者挑選實驗室的位置](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location)。
    
  - **不會對等互連任何 VNet ***，且***不允許實驗室建立者挑選實驗室位置**
  
    當沒有具有實驗室帳戶**的 VNet 對等互連** *，而且* [**不**允許實驗室建立者挑選實驗室位置](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location)時，會在具有可用 VM 容量的區域中自動建立教室實驗室。  具體而言，Azure 實驗室服務[會在與實驗室帳戶位於相同地理位置的區域](https://azure.microsoft.com/global-infrastructure/regions)中尋找可用性。

  - **不會對等互連任何 VNet ***，而且***可以選擇實驗室建立者來挑選實驗室位置**
       
    當**沒有 VNet 對等互連**，而且有[實驗室建立者允許選擇實驗室位置](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location)時，實驗室建立者可以選取的位置會以可用容量為基礎。

> [!NOTE]
> 為協助確保區域有足夠的 VM 容量，請務必先透過實驗室帳戶或在建立實驗室時要求容量。

一般規則是將資源的區域設定為最接近其使用者的地區。 對於教室實驗室而言，這表示要建立最接近您的學生的教室實驗室。 針對所有學生都位於世界各地的線上課程，您必須使用最佳的判斷來建立集中的教室實驗室。 或者，根據您的學生所在地區，將一個類別分割成多個教室實驗室。

### <a name="shared-image-gallery"></a>共用映射資源庫

區域會指出第一個映射版本儲存所在的來源區域，然後才會自動複寫至目的地區域。

## <a name="vm-sizing"></a>VM 大小調整

當系統管理員或實驗室建立者建立教室實驗室時，他們可以根據其教室的需求，從下列 VM 大小中選擇。 請記住，可用的計算大小取決於您的實驗室帳戶所在的區域：

| 大小 | 規格 | 數列 | 建議使用 |
| ---- | ----- | ------ | ------------- |
| 小型| <ul><li>2 個核心</li><li>3.5 GB 的 RAM</li> | [Standard_A2_v2](https://docs.microsoft.com/azure/virtual-machines/av2-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | 此大小最適合用於命令列、開啟網頁瀏覽器、低流量網頁伺服器、小型至中型資料庫。 |
| 中 | <ul><li>4核心</li><li>7 GB RAM</li> | [Standard_A4_v2](https://docs.microsoft.com/azure/virtual-machines/av2-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | 此大小最適合用於關係資料庫、記憶體內部快取及分析。 |
| 中（嵌套虛擬化） | <ul><li>4核心</li><li>16 GB RAM</li></ul> | [Standard_D4s_v3](https://docs.microsoft.com/azure/virtual-machines/dv3-dsv3-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#dsv3-series) | 此大小最適合用於關係資料庫、記憶體內部快取及分析。  此大小也支援嵌套虛擬化。 |
| 大型 | <ul><li>8 核心</li><li>32 GB 的 RAM</li></ul>  | [Standard_DC8_v2](https://docs.microsoft.com/azure/virtual-machines/dcv2-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | 此大小最適合需要更快的 Cpu、更佳的本機磁片效能、大型資料庫、大型記憶體快取的應用程式。  此大小也支援嵌套虛擬化。 |
| 小型 GPU （視覺效果） | <ul><li>6個核心</li><li>56 GB 的 RAM</li>  | [Standard_NV6](https://docs.microsoft.com/azure/virtual-machines/nv-series) | 此大小最適合用於使用 OpenGL 和 DirectX 這類架構的遠端視覺效果、串流、遊戲、編碼。 |
| 小型 GPU （計算） | <ul><li>6個核心</li><li>56 GB 的 RAM</li></ul>  | [Standard_NC6](https://docs.microsoft.com/azure/virtual-machines/nc-series) |此大小最適合電腦密集型應用程式，例如人工智慧和深度學習。 |
| 中 GPU （視覺效果） | <ul><li>12個核心</li><li>112 GB 的 RAM</li></ul>  | [Standard_NC12](https://docs.microsoft.com/azure/virtual-machines/nc-series) | 此大小最適合用於使用 OpenGL 和 DirectX 這類架構的遠端視覺效果、串流、遊戲、編碼。 |

## <a name="manage-identity"></a>管理身分識別

使用[Azure 的角色型存取控制](https://docs.microsoft.com/azure/role-based-access-control/overview)，您可以指派下列角色來授與實驗室帳戶和教室實驗室的存取權：

- **實驗室帳戶擁有者**

    建立實驗室帳戶的系統管理員會自動新增至實驗室帳戶的 [**擁有**者] 角色。  獲指派**擁有**者角色的系統管理員可以：
     - 變更實驗室帳戶的設定。
     - 將實驗室帳戶的存取權授與其他系統管理員，作為擁有者或參與者。
     - 將教室實驗室的存取權授與開發人員、擁有者或參與者。
     - 建立及管理實驗室帳戶中的所有教室實驗室。

- **實驗室帳戶參與者**

    獲指派**參與者**角色的系統管理員可以：
    - 變更實驗室帳戶的設定。
    - 建立及管理實驗室帳戶中的所有教室實驗室。

    不過，他們*無法*將實驗室帳戶或教室實驗室的存取權授與其他使用者。

- **教室實驗室建立者**

    若要在實驗室帳戶中建立教室實驗室，教師必須是**實驗室建立者**角色的成員。  當教師建立教室實驗室時，系統會自動將他們新增為實驗室的擁有者。  請參閱教學課程，以瞭解如何[將使用者新增至**實驗室建立者**角色](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role)。 

- **教室實驗室 owner\contributor**
  
    當教師是實驗室的**擁有**者或**參與者**角色的成員時，可以查看和變更教室實驗室的設定;他們也必須是實驗室帳戶的 [**讀者**] 角色的成員。

    實驗室的**擁有**者和**參與者**角色之間的主要差異，在於參與者*無法*授與其他使用者存取權來管理實驗室專用的擁有者，可以讓其他使用者存取管理實驗室。

    此外，教師也*無法*建立新的教室實驗室，除非他們也是「實驗室建立**者**」角色的成員。

- **共用映射資源庫**

    當您將共用映射資源庫連結至實驗室帳戶時，[實驗室帳戶 owners\contributors] 和 [實驗室 creators\owners\contributors] 會自動獲得存取權，以在資源庫中查看和儲存影像。

以下是協助指派角色的一些秘訣：
   - 通常，只有系統管理員才應該是實驗室帳戶的**擁有**者或**參與者**角色的成員;您可能會有一個以上的 owner\contributor。
   - 讓教師能夠建立新的課堂實驗室並管理他們所建立的實驗室;您只需要將存取權指派給 [**實驗室建立者**] 角色。
   - 讓教師能夠管理特定的課堂實驗室，但*不*能建立新的實驗室;您應該為他們所管理的每個教室實驗室指派**擁有**者或**參與者**角色的存取權。  例如，您可能會想要讓教授和教學助理共同擁有教室實驗室。  請參閱指南，以瞭解如何[將使用者新增為教室實驗室的擁有](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-add-user-lab-owner)者。

## <a name="pricing"></a>定價

### <a name="azure-lab-services"></a>Azure 實驗室服務

下列文章說明 Azure 實驗室服務的定價： [Azure 實驗室服務定價](https://azure.microsoft.com/pricing/details/lab-services/)。

如果您打算使用共用映射資源庫來儲存和管理映射版本，您也需要考慮此定價。 

### <a name="shared-image-gallery"></a>共用映射資源庫

建立共用映射資源庫並將其附加至您的實驗室帳戶是免費的。 在您將映射版本儲存至資源庫之前，不會產生成本。 一般而言，使用共用映射資源庫的定價非常微不足道，但請務必瞭解它的計算方式，因為它不包含在 Azure 實驗室服務的定價中。  

#### <a name="storage-charges"></a>儲存體費用

若要儲存映射版本，共用映射庫會使用標準 HDD 受控磁片。 所使用之 HDD 受控磁片的大小取決於所儲存之映射版本的大小。 請參閱下列文章以瞭解定價：[受控磁片定價](https://azure.microsoft.com/pricing/details/managed-disks/)。

#### <a name="replication-and-network-egress-charges"></a>複寫和網路輸出費用

當您使用教室實驗室的範本虛擬機器（VM）儲存映射版本時，Azure 實驗室服務會先將其儲存在來源區域中，然後自動將來源映射版本複寫至一或多個目的地區域。 請務必注意，Azure 實驗室服務會自動將來源映射版本複寫至教室實驗室所在[地理位置中](https://azure.microsoft.com/global-infrastructure/regions/)的所有目的地區域。 例如，如果您的教室實驗室位於美國地理位置，則映射版本會複寫到美國境內的八個區域中的每一個

當映射版本從來源區域複寫到其他目的地區域時，就會發生網路輸出費用。 當映射的資料一開始從來源區域傳出時，費用會以映射版本的大小為基礎。  如需定價詳細資料，請參閱下列文章：[頻寬定價詳細資料](https://azure.microsoft.com/pricing/details/bandwidth/)。

[教育解決方案](https://www.microsoft.com/licensing/licensing-programs/licensing-for-industries?rtc=1&activetab=licensing-for-industries-pivot:primaryr3)客戶可能會放棄支付輸出費用。 請與您的帳戶管理員交流，以深入瞭解。  如需詳細資訊，請參閱連結檔中**的常見問題**一節，特別是「學術客戶有哪些資料傳輸計畫？如何限定？」的問題。

#### <a name="pricing-example"></a>定價範例

為了回顧上面所述的定價，讓我們來看一個將範本 VM 映射儲存到共用映射資源庫的範例。 假設有下列案例：

- 您有一個自訂 VM 映射。
- 您正在儲存兩個版本的映射。
- 您的實驗室為美國，總共有八個地區。
- 每個映射版本的大小為 32 GB;因此，HDD 管理的磁片價格是每月 $1.54。

總成本預估為：

影像數目×個版本×個複本數目×個受控磁片價格

在此範例中，成本為：

1個自訂映射（32 GB） x 2 版本 x 8 個美國地區 x $1.54 = 每月 $24.64

#### <a name="cost-management"></a>成本管理

請務必定期從資源庫刪除不必要的映射版本，以讓實驗室帳戶管理員管理成本。 

您不應該刪除特定區域的複寫，以降低成本（此選項存在於共用映射資源庫中）。 複寫變更可能會對 Azure 實驗室服務從儲存在共用映射資源庫中的映射發佈 Vm 的能力造成負面影響。

## <a name="next-steps"></a>後續步驟

如需建立實驗室帳戶和實驗室的逐步指示，請參閱教學課程：[設定指南](tutorial-setup-lab-account.md)
