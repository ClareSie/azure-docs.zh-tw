---
title: 檢查是否有集區和節點錯誤
description: 此文章涵蓋可能發生的背景作業、要檢查的錯誤，以及如何在建立集區和節點時加以避免。
author: mscurrell
ms.author: markscu
ms.date: 08/23/2019
ms.topic: how-to
ms.openlocfilehash: 519b357e4e5fde30221f7dc804bb848ecec9704c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85979912"
---
# <a name="check-for-pool-and-node-errors"></a>檢查是否有集區和節點錯誤

當您建立和管理 Azure Batch 集區時，某些作業將會立即執行。 不過，某些作業則會以非同步方式在背景中執行，這需要數分鐘時間才能完成。

要偵測立即執行的作業是否失敗並不難，因為 API、CLI 或 UI 會立即傳回所有失敗。

本文說明可能為集區和集區節點執行的背景作業。 文中將闡明如何偵測和避免失敗。

## <a name="pool-errors"></a>集區錯誤

### <a name="resize-timeout-or-failure"></a>調整逾時或失敗

在建立新集區或調整現有集區的大小時，您會指定目標節點數目。  建立或調整大小作業會立即完成，但新節點的實際配置或現有節點的移除可能需要數分鐘才能完成。  您可以在[建立](/rest/api/batchservice/pool/add)或[調整大小](/rest/api/batchservice/pool/resize) API 中指定調整逾時。 如果 Batch 無法在調整大小逾時期間取得目標節點數目，則集區會進入穩定狀態，並回報調整大小錯誤。

最新評估的 [ResizeError](/rest/api/batchservice/pool/get#resizeerror) 屬性會列出已發生的錯誤。

調整大小錯誤的常見原因包括：

- 調整逾時太短
  - 在一般情況下，預設的逾時 15 分鐘通常即足以進行集區節點的配置或移除。
  - 如果您將配置大量的節點，建議您將調整逾時設為 30 分鐘。 例如，當您要從 Azure Marketplace 映像將大小調整為 1000 個以上的節點，或從自訂 VM 映像調整為 300 個以上的節點。
- 核心配額不足
  - Batch 帳戶可配置給所有集區的核心數目有其限制。 達到該配額之後，Batch 就會停止配置節點。 您[可以增加](./batch-quota-limit.md)核心配額，使 Batch 能夠配置更多節點。
- 當[集區位於虛擬網路中](./batch-virtual-network.md)時，子網路 IP 不足
  - 虛擬網路子網路必須有足夠的未指派 IP 位址可配置給每個要求的集區節點。 否則，就會無法建立節點。
- 當[集區位於虛擬網路中](./batch-virtual-network.md)時，資源不足
  - 您可以在與 Batch 帳戶相同的訂用帳戶中建立資源，例如負載平衡器、公用 IP 和網路安全性群組。 請確認訂用帳戶配額足夠供這些資源使用。
- 使用自訂 VM 映像的大型集區
  - 使用自訂 VM 映像的大型集區可能需要較長的時間進行配置，因此可能會發生調整逾時。  如需限制和設定的建議，請參閱[使用共用映像庫建立集區](batch-sig-images.md)。

### <a name="automatic-scaling-failures"></a>自動調整失敗

您也可以設定 Azure Batch，使其自動調整集區中的節點數目。 您必須為[集區的自動調整公式](./batch-automatic-scaling.md)定義參數。 Batch 服務會使用此公式定期評估集區中的節點數目，並設定新的目標數目。 以下是可能偵測到的問題類型：

- 自動調整評估失敗。
- 產生的調整大小作業失敗並逾時。
- 自動調整公式的問題會導致不正確的節點目標值。 調整大小可能會成功或逾時。

您可以使用 [autoScaleRun](/rest/api/batchservice/pool/get#autoscalerun) 屬性取得上次自動調整評估的相關資訊。 此屬性會報告評估時間、值和結果，以及任何效能錯誤。

[集區調整大小完成事件](./batch-pool-resize-complete-event.md)會擷取所有評估的相關資訊。

### <a name="delete"></a>刪除

當您刪除包含節點的集區時，Batch 會先刪除節點。 然後它會刪除集區物件本身。 刪除這些節點可能需要幾分鐘的時間。

在刪除過程中，Batch 會將[集區狀態](/rest/api/batchservice/pool/get#poolstate)設為**刪除中**。 呼叫端應用程式可以使用 **state** 和 **stateTransitionTime** 屬性來偵測集區刪除作業是否耗費太多時間。

## <a name="pool-compute-node-errors"></a>集區計算節點錯誤

即使 Batch 在集區中成功配置了節點，仍有各種問題可能導致某些節點狀況不良且無法執行工作。 這些節點仍會產生費用，因此請務必偵測問題，以避免對無法使用的節點付費。 除了常見的節點錯誤之外，了解目前的[作業狀態](/rest/api/batchservice/job/get#jobstate)對於疑難排解很有用。

### <a name="start-task-failures"></a>開始工作失敗

您可以為集區指定選擇性的[開始工作](/rest/api/batchservice/pool/add#starttask)。 與任何工作一樣，您可以指定命令列和要從儲存體下載的資源檔案。 每個節點啟動後，系統即會分別執行其「開始工作」。 **waitForSuccess** 屬性會指定 Batch 是否應等到「開始工作」順利完成後，再為節點排定任何工作。

如果您已設定要讓節點等待「開始工作」順利完成，但開始工作卻失敗，該如何處理？ 在這種情況下，節點將無法使用，但仍會產生費用。

您可以使用最上層 [startTaskInfo](/rest/api/batchservice/computenode/get#starttaskinformation) 節點屬性的 [result](/rest/api/batchservice/computenode/get#taskexecutionresult) 和 [failureInfo](/rest/api/batchservice/computenode/get#taskfailureinformation) 屬性，來偵測失敗的「開始工作」。

失敗的開始工作也會導致 Batch 將節點[狀態](/rest/api/batchservice/computenode/get#computenodestate)設定為 **starttaskfailed**，但前提是 **waitForSuccess** 已設定為 **true**。

與任何工作一樣，開始工作失敗的原因可能有許多個。  若要進行疑難排解，請檢查 stdout、stderr 及任何進一步的工作特定記錄檔。

開始工作必須可以重新進入，因為在相同的節點上可能會多次執行開始工作；當重新製作節點映像或重新開機時，就會執行開始工作。 在罕見的情況下，開始工作將會在事件導致節點重新開機之後執行，其中其中一個作業系統或暫時磁碟已重新製作映像，其他作業系統或暫時磁碟則沒有。 Batch 開始工作 (例如所有 Batch 工作) 是從暫時磁碟執行的，因此這通常不是問題，但在某些情況下，開始工作會將應用程式安裝到作業系統磁碟，並將其他資料保留在暫時磁碟上，這可能會造成問題，因為一切都不同步。如果您同時使用這兩個磁碟，請適當地保護您的應用程式。

### <a name="application-package-download-failure"></a>應用程式套件下載失敗

您可以為集區指定一或多個應用程式套件。 Batch 會將指定的套件檔案下載至每個節點，並在節點啟動之後、排定工作之前將這些檔案解壓縮。 將開始工作命令列與應用程式套件搭配使用是常見的做法。 例如，用來將檔案複製到不同的位置或執行安裝程式。

節點的 [errors](/rest/api/batchservice/computenode/get#computenodeerror) 屬性會回報下載和解壓縮應用程式套件失敗；節點狀態會設定為**無法使用**。

### <a name="container-download-failure"></a>容器下載失敗

您可以在集區上指定一或多個容器參考。 Batch 會將指定的容器下載到每個節點。 節點 [errors](/rest/api/batchservice/computenode/get#computenodeerror) 屬性會回報下載容器失敗，並將節點狀態設定為**無法使用**。

### <a name="node-in-unusable-state"></a>處於無法使用狀態的節點

Azure Batch 將[節點狀態](/rest/api/batchservice/computenode/get#computenodestate)設為**無法使用**的原因有很多。 當節點狀態設為**無法使用**時，將無法為節點排定工作，但節點仍將產生費用。

處於**無法使用**狀態，但沒有[錯誤](/rest/api/batchservice/computenode/get#computenodeerror)的節點表示 Batch 無法與 VM 進行通訊。 在此情況下，Batch 一律會嘗試復原 VM。 Batch 不會自動嘗試復原無法安裝應用程式套件或容器的 VM，即使其狀態為**無法使用**也一樣。

如果 Batch 可判斷出原因，節點的 [errors](/rest/api/batchservice/computenode/get#computenodeerror) 屬性會加以報告。

導致節點**無法使用**的其他原因包括：

- 自訂 VM 映像無效。 例如，未正確備妥的映像。

- VM 因基礎結構失敗或低層級升級而移動。 Batch 會復原節點。

- VM 映像已部署在不支援 VM 映像的硬體上。 例如，嘗試在 [Standard_D1_v2](../virtual-machines/dv2-dsv2-series.md) VM 上執行 CentOS HPC 映像。

- VM 位於 [Azure 虛擬網路](batch-virtual-network.md)，且流量已封鎖到主要連接埠。

- VM 位於虛擬網路中，但對 Azure 儲存體的連出流量遭到封鎖。

- VM 位於具有客戶 DNS 設定的虛擬網路中，且 DNS 伺服器無法解析 Azure 儲存體。

### <a name="node-agent-log-files"></a>節點代理程式記錄檔

在每個集區節點上執行的 Batch 代理程式處理序可提供記錄檔，在您需要連絡支援人員來解決集區節點問題時，可能有所幫助。 節點的記錄檔可透過 Azure 入口網站、Batch Explorer 或 [API](/rest/api/batchservice/computenode/uploadbatchservicelogs) 來上傳。 上傳並儲存記錄檔，將對您有幫助。 其後，您可以刪除節點或集區，以節省執行節點的成本。

### <a name="node-disk-full"></a>節點磁碟已滿

集區節點 VM 的暫存磁碟機是由 Batch 用於作業檔案、工作檔案和共用檔案。

- 應用程式套件檔案
- 工作資源檔
- 應用程式專用的檔案已下載至其中一個 Batch 資料夾
- 每個工作應用程式執行的 Stdout 和 Stderr 檔案
- 應用程式專用的輸出檔

這些檔案中的某些檔案只會在建立集區節點時寫入一次，例如集區應用程式套件或集區開始工作資源檔。 即使在建立節點時只寫入一次，如果這些檔案太大，可能會填滿暫存磁碟機。

在節點上執行的每個工作都會寫出其他檔案，例如 Stdout 和 Stderr。 如果在相同的節點上執行大量工作，和/或工作檔案太大，則可能會填滿暫存磁碟機。

暫存磁碟機的大小取決於 VM 大小。 挑選 VM 大小時的一個考量是要確保暫存磁碟機擁有足夠的空間。

- 在 Azure 入口網站中新增集區時，可以顯示 VM 大小的完整清單，而且有 [資源磁碟大小] 資料行。
- 描述所有 VM 大小的發行項都有包含 [暫存儲存體] 資料行的資料表；例如[計算最佳化 VM 大小](../virtual-machines/sizes-compute.md)

針對每個工作所寫出的檔案，可以指定每個工作的保留時間，以決定工作檔案在自動清除之前要保留的時間長度。 保留時間可以減少，以降低儲存需求。


如果暫存磁碟空間不足 (或非常接近空間不足)，節點將會移至[無法使用](/rest/api/batchservice/computenode/get#computenodestate) 狀態，而且系統會回報節點錯誤，指出磁碟已滿。

### <a name="what-to-do-when-a-disk-is-full"></a>磁碟已滿時該怎麼辦

判斷磁碟已滿的原因：如果您不確定是什麼佔用了節點上的空間，建議您從遠端移至該節點，並手動調查空間消失的位置。 您也可以使用 [Batch 清單檔案 API](/rest/api/batchservice/file/listfromcomputenode) 檢查 Batch 受管理資料夾中的檔案 (例如，工作輸出)。 請注意，此 API 僅會列出 Batch 受管理目錄中的檔案，而且如果您的工作在其他位置建立了檔案，將不會看到這些檔案。

請確認您需要的任何資料都已經從節點擷取，或者已上傳到長期存放區。 磁碟已滿問題的全部緩解措施包括刪除資料以釋出空間。

### <a name="recovering-the-node"></a>復原節點

1. 如果您的集區為 [C.loudServiceConfiguration](/rest/api/batchservice/pool/add#cloudserviceconfiguration) 集區，可以透過 [Batch 重新製作映像 API](/rest/api/batchservice/computenode/reimage)，重新製作節點的映像。這將清理整個磁碟。 [VirtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration) 集區目前不支援重新製作映像。

2. 如果您的集區為 [VirtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration)，可以使用[移除節點 API](/rest/api/batchservice/pool/removenodes)，從集區移除節點。 接著，您可以再次擴增集區，將錯誤的節點取代為全新的節點。

3.  刪除其工作資料仍在節點上的舊的已完成作業或舊的已完成工作。 如需有關在節點上有哪些作業/工作資料的提示，您可以查詢節點上的 [RecentTasks 集合](/rest/api/batchservice/computenode/get#taskinformation)，或[節點上的檔案](/rest/api/batchservice/file/listfromcomputenode)。 刪除作業將會刪除作業中的所有工作，而刪除作業中的工作將會在節點上的工作目錄中觸發要刪除的資料，因而釋出空間。 釋出足夠的空間之後，請重新啟動節點，其應該會移出 [無法使用] 狀態並重新進入 [閒置] 狀態。

## <a name="next-steps"></a>後續步驟

請確認您已設定應用程式以實作完整的錯誤檢查，特別是針對非同步作業。 這可能是能否立即偵測並診斷問題的關鍵。
