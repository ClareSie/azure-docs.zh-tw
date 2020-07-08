---
title: Azure Service Fabric CLI-sfctl 混亂
description: 深入瞭解 sfctl，這是 Azure Service Fabric 命令列介面。 包含用來管理混亂的命令清單。
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 9bc7a5405309e35a36b15f44a1b136b899afbb55
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84119327"
---
# <a name="sfctl-chaos"></a>sfctl chaos
啟動、停止及報告 chaos 測試服務。

## <a name="subgroups"></a>子群組
|子群組|Description|
| --- | --- |
| [任務](service-fabric-sfctl-chaos-schedule.md) | 取得並設定 chaos 排程。 |
## <a name="commands"></a>命令

|Command|描述|
| --- | --- |
| 活動 | 根據接續權杖或時間範圍取得 Chaos 事件的下一個區段。 |
| get | 取得 Chaos 的狀態。 |
| start | 在叢集中啟動 Chaos。 |
| stop | 如果 Chaos 正在叢集中執行，便將它停止，並讓 Chaos 排程處於已停止狀態。 |

## <a name="sfctl-chaos-events"></a>sfctl chaos events
根據接續權杖或時間範圍取得 Chaos 事件的下一個區段。

若要取得 Chaos 事件的下一個區段，您可以指定 ContinuationToken。 若要取得 Chaos 事件新區段的開頭，您可以透過 StartTimeUtc 和 EndTimeUtc 來指定時間範圍。 您可以在同一個呼叫中同時指定 ContinuationToken 和時間範圍。 當 Chaos 事件超過 100 個時，系統會以多個區段傳回 Chaos 事件，其中每個區段包含的 Chaos 事件不超過 100 個，而若要取得下一個區段，您可以使用接續權杖來對此 API 發出呼叫。

### <a name="arguments"></a>引數

|引數|說明|
| --- | --- |
| --continuation-token | 接續權杖參數可用來取得下一組結果。 當來自系統的結果無法放入單一回應中時，API 的回應中會包含具有非空白值的接續權杖。 當此值傳遞至下一個 API 呼叫時，API 會傳回下一組結果。 如果沒有任何進一步的結果，接續權杖就不會包含值。 此參數的值不能經過 URL 編碼。 |
| --end-time-utc | 表示要產生 Chaos 報告之時間範圍結束時間的 Windows 檔案時間。 如需詳細資料，請參閱 [DateTime.ToFileTimeUtc 方法](https\://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) \(機器翻譯\)。 |
| --max-results | 分頁式查詢時傳回的最大結果數目。 此參數定義傳回結果數目的上限。 根據設定中所定義的訊息大小限制上限，如果所傳回的結果無法放入訊息中，則它們可以小於指定的結果上限。 如果此參數為零或未指定，則分頁式查詢會在傳回訊息中盡可能包含較多結果。 |
| --start-time-utc | 表示要產生 Chaos 報告之時間範圍開始時間的 Windows 檔案時間。 如需詳細資料，請參閱 [DateTime.ToFileTimeUtc 方法](https\://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) \(機器翻譯\)。 |
| --timeout -t | 執行作業的伺服器超時（以秒為單位）。 這個超時時間會指定用戶端願意等待要求的作業完成的持續期間。 此參數的預設值為60秒。  預設值\: 60。 |

### <a name="global-arguments"></a>全域引數

|引數|說明|
| --- | --- |
| --debug | 增加記錄詳細資訊，以顯示所有偵錯記錄。 |
| --help -h | 顯示此說明訊息並結束。 |
| --output -o | 輸出格式。  允許的值\:json、jsonc、table、tsv。  預設值\:json。 |
| --query | JMESPath 查詢字串。 如需詳細資訊和範例，請參閱 http\://jmespath.org/。 |
| --verbose | 增加記錄詳細資訊。 使用 --debug 來取得完整偵錯記錄。 |

## <a name="sfctl-chaos-get"></a>sfctl chaos get
取得 Chaos 的狀態。

取得指出 Chaos 是否正在執行的 Chaos 狀態、用於執行 Chaos 的 Chaos 參數，以及 Chaos 排程的狀態。

### <a name="arguments"></a>引數

|引數|說明|
| --- | --- |
| --timeout -t | 執行作業的伺服器超時（以秒為單位）。 這個超時時間會指定用戶端願意等待要求的作業完成的持續期間。 此參數的預設值為60秒。  預設值\: 60。 |

### <a name="global-arguments"></a>全域引數

|引數|說明|
| --- | --- |
| --debug | 增加記錄詳細資訊，以顯示所有偵錯記錄。 |
| --help -h | 顯示此說明訊息並結束。 |
| --output -o | 輸出格式。  允許的值\:json、jsonc、table、tsv。  預設值\:json。 |
| --query | JMESPath 查詢字串。 如需詳細資訊和範例，請參閱 http\://jmespath.org/。 |
| --verbose | 增加記錄詳細資訊。 使用 --debug 來取得完整偵錯記錄。 |

## <a name="sfctl-chaos-start"></a>sfctl chaos start
在叢集中啟動 Chaos。

如果 Chaos 尚未在叢集中執行，此命令會使用傳入的 Chaos 參數啟動 Chaos。 如果在進行這個呼叫時 Chaos 已在執行中，則呼叫會失敗，錯誤碼為 FABRIC_E_CHAOS_ALREADY_RUNNING。 如需詳細資訊，請參閱在[Service Fabric 叢集中引發受控制的混亂](https\://docs.microsoft.com/azure/service-fabric/service-fabric-controlled-chaos)一文。

### <a name="arguments"></a>引數

|引數|說明|
| --- | --- |
| --app-type-health-policy-map | 針對特定應用程式類型，具有狀況不良應用程式百分比上限的字典（索引鍵/值）專案的 JSON 編碼陣列。 每個字典專案都會將應用程式類型名稱指定為索引鍵，並將代表用來評估指定應用程式類型應用程式之 MaxPercentUnhealthyApplications 百分比的整數表示為值。 <br><br> 對於特定應用程式類型，定義具有最大健康情況不良應用程式百分比的對應。 應用程式類型健全狀況原則對應可以在叢集健康狀態評估期間使用，以描述個別的應用程式類型。 對應中包含的應用程式類型是針對在對應中指定的百分比進行評估，不包含在叢集健康原則中定義的全域 MaxPercentUnhealthyApplications。 對應中指定之應用程式類型的應用程式不會針對應用程式的全域集區進行計算。 例如，如果某些類型的應用程式很重要，叢集系統管理員可以將專案新增至該應用程式類型的對應，並為其指派0% 的值（不容許任何失敗）。 其他所有應用程式則可以使用設為 20% 的 MaxPercentUnhealthyApplications 來進行評估，以容忍數千個應用程式執行個體發生一些失敗。 只有在叢集資訊清單使用 HealthManager/EnableApplicationTypeHealthEvaluation 的設定項目啟用應用程式類型健康情況評估時，才會使用應用程式類型健康原則對應。 <br><br> 範例 JSON 編碼字串： [{ \" 金鑰 \" ： \" Fabric：/投票 \" ， \" 值 \" ： \" 0 \" }] |
| --chaos-target-filter | 具有兩個字串類型索引鍵的 JSON 編碼字典。 這兩個索引鍵分別是 NodeTypeInclusionList 和 ApplicationInclusionList。 這兩個索引鍵的值都是字串清單。 chaos_target_filter 會針對鎖定的 Chaos 錯誤定義所有篩選，例如，只有特定節點類型或特定應用程式會引發錯誤。 <br><br> 如果未使用 chaos_target_filter，Chaos 會針對所有叢集實體引發錯誤。 如果使用 chaos_target_filter，則 Chaos 只會針對符合 chaos_target_filter 規格的實體引發錯誤。 NodeTypeInclusionList 和 ApplicationInclusionList 只允許集合聯集語意。 您不能指定 NodeTypeInclusionList 和 ApplicationInclusionList 的交集。 例如，您不能指定「只有在此應用程式位於該節點類型上時才對它引發錯誤」。 若 NodeTypeInclusionList 或 ApplicationInclusionList 中包含某個實體，則該實體就不能使用 ChaosTargetFilter 排除。 即使 applicationX 並未出現在 ApplicationInclusionList 中，但在某些混亂反覆項目中也能對 applicationX 引發錯誤，因為它正好位於包括在 NodeTypeInclusionList 中的 nodeTypeY 的節點上。 如果 NodeTypeInclusionList 和 ApplicationInclusionList 都是空白，則會擲回 ArgumentException。 針對這些節點類型的節點，會啟用所有錯誤類型 (重新啟動節點、重新啟動程式碼封裝、移除複本、重新啟動複本、移動主要複本，以及移動次要複本)。 如果某個節點類型 (假設為 NodeTypeX) 未出現在 NodeTypeInclusionList 中，則系統將永遠不會針對 NodeTypeX 的節點啟用節點層級錯誤 (例如 NodeRestart)，但如果 ApplicationInclusionList 中的某個應用程式正好位於 NodeTypeX 的節點上，則仍會針對 NodeTypeX 啟用程式碼套件和複本錯誤。 您最多可在此清單中包含 100 個節點類型名稱，若要增加此數目，則必須針對 MaxNumberOfNodeTypesInChaosEntityFilter 設定進行設定升級。 屬於這些應用程式之服務的所有複本，都適用於由混亂所引發的複本錯誤 (重新啟動複本、移除複本、移動主要複本和移動次要複本)。 只有在程式碼封裝只裝載這些應用程式的複本時，混亂才可以將程式碼封裝重新啟動。 如果某個應用程式未出現在此清單中，系統仍然可能在某些混亂反覆項目中針對它引發錯誤，前提是該應用程式位於某個包含在 NodeTypeInclusionList 中之節點類型的節點上。 不過，如果 applicationX 因位置限制而繫結至 nodeTypeY，且 applicationX 未出現在 ApplicationInclusionList，而且 nodeTypeY 未出現在 NodeTypeInclusionList 中，則系統將永遠不會對 applicationX 引發錯誤。 您最多可在此清單中包含 1000 個應用程式名稱，若要增加此數目，則必須針對 MaxNumberOfApplicationsInChaosEntityFilter 設定進行設定升級。 |
| --context | (string, string) 類型之索引鍵-值組的 JSON 編碼對應。 此對應可用於記錄混亂執行的相關資訊。 此類組合不能超過 100 個，且每個字串 (索引鍵或值) 最多為 4095 個字元長。 此對應由混亂執行的起始者設定，以選擇性地儲存特定執行的相關內容。 |
| --disable-move-replica-faults | 停用移動主要錯誤和移動次要錯誤。 |
| --max-cluster-stabilization | 等待所有叢集實體變成穩定且狀況良好的時間長度上限。  預設值\: 60。 <br><br> Chaos 會在反覆項目中執行，並且在每個反覆項目開始時它會驗證叢集實體的健康情況。 在驗證期間，如果叢集實體在 MaxClusterStabilizationTimeoutInSeconds 內不穩定且狀況不良，Chaos 就會產生驗證失敗事件。 |
| --max-concurrent-faults | 每個反覆運算引發的最大並行錯誤數。 Chaos 會在反覆項目中執行，而兩個連續的反覆項目之間會以驗證階段分隔。 並行程度越高，就越容易發生插入錯誤的情況 -- 導致一系列更複雜的狀態而更不易找出錯誤 (bug)。 建議從值為 2 或 3 開始，上調此值時請小心謹慎。  預設值\: 1。 |
| --max-percent-unhealthy-apps | 在 Chaos 期間評估叢集健康情況時，於回報錯誤之前，可允許的狀況不良應用程式百分比上限。 <br><br> 在報告錯誤之前，允許健康情況不良應用程式的最大百分比。 例如，若要允許 10% 的應用程式健康情況不良，這個值會是 10。 百分比表示在系統將叢集視為錯誤之前，容許應用程式健康情況不良的最大百分比。 如果未到達此百分比，但至少有一個健康情況不良的應用程式，則健康情況會評估為 Warning。 此計算是將健康情況不良的應用程式數目除以叢集中應用程式執行個體的總數而得，但不包括 ApplicationTypeHealthPolicyMap 所包含之應用程式類型的應用程式。 針對少量的應用程式數目，計算會四捨五入以容許一個失敗。 預設百分比是零。 |
| --max-percent-unhealthy-nodes | 在 Chaos 期間評估叢集健康情況時，於回報錯誤之前，可允許的狀況不良節點百分比上限。 <br><br> 在報告錯誤之前，允許健康情況不良節點的最大百分比。 例如，若要允許 10% 的節點健康情況不良，這個值會是 10。 百分比表示在叢集被視為處於錯誤狀態之前，容許節點健康情況不良的最大百分比。 如果未到達此百分比，但至少有一個健康情況不良的節點，則健康情況會評估為 Warning。 百分比是將健康情況不良節點數目除以叢集中的節點總數計算而得。 針對較少的節點數目，計算會四捨五入以容許一個失敗。 預設百分比是零。 在大型叢集中，永遠都有一些節點會關閉或需要修復，因此應設定此百分比來容許這種情形。 |
| --time-to-run | Chaos 在自動停止之前，將執行的總時間 (秒)。 可允許的最大值為 4,294,967,295 (System.UInt32.MaxValue)。  預設值\: 4294967295。 |
| --timeout -t | 預設值\: 60。 |
| --wait-time-between-faults | 單一反覆項目內連續錯誤之間的等候時間 (秒)。  預設值\: 20。 <br><br> 值越大，錯誤之間的重疊越低，且叢集歷經的狀態轉換順序會越簡單。 建議從 1 到 5 之間的值來開始，上調此值時請小心謹慎。 |
| --wait-time-between-iterations | Chaos 的兩個連續反覆項目之間的時間區隔 (秒)。 值越大，錯誤插入率越低。  預設值\: 30。 |
| --warning-as-error | 可指示是否將具有相同嚴重性的警告視為錯誤。 |

### <a name="global-arguments"></a>全域引數

|引數|說明|
| --- | --- |
| --debug | 增加記錄詳細資訊，以顯示所有偵錯記錄。 |
| --help -h | 顯示此說明訊息並結束。 |
| --output -o | 輸出格式。  允許的值\:json、jsonc、table、tsv。  預設值\:json。 |
| --query | JMESPath 查詢字串。 如需詳細資訊和範例，請參閱 http\://jmespath.org/。 |
| --verbose | 增加記錄詳細資訊。 使用 --debug 來取得完整偵錯記錄。 |

## <a name="sfctl-chaos-stop"></a>sfctl chaos stop
如果 Chaos 正在叢集中執行，便將它停止，並讓 Chaos 排程處於已停止狀態。

讓 Chaos 停止執行新的錯誤。 進行中的錯誤會繼續執行，直到完成為止。 目前的 Chaos 排程會進入已停止狀態。 停止排程之後，排程就會停留在已停止狀態，而不會用於 Chaos 排程來進行新的 Chaos 回合。 必須設定新的 Chaos 排程，才能繼續執行排程。

### <a name="arguments"></a>引數

|引數|說明|
| --- | --- |
| --timeout -t | 執行作業的伺服器超時（以秒為單位）。 這個超時時間會指定用戶端願意等待要求的作業完成的持續期間。 此參數的預設值為60秒。  預設值\: 60。 |

### <a name="global-arguments"></a>全域引數

|引數|說明|
| --- | --- |
| --debug | 增加記錄詳細資訊，以顯示所有偵錯記錄。 |
| --help -h | 顯示此說明訊息並結束。 |
| --output -o | 輸出格式。  允許的值\:json、jsonc、table、tsv。  預設值\:json。 |
| --query | JMESPath 查詢字串。 如需詳細資訊和範例，請參閱 http\://jmespath.org/。 |
| --verbose | 增加記錄詳細資訊。 使用 --debug 來取得完整偵錯記錄。 |


## <a name="next-steps"></a>後續步驟
- [設定](service-fabric-cli.md) Service Fabric CLI。
- 了解如何使用[範例指令碼](/azure/service-fabric/scripts/sfctl-upgrade-application)來使用 Service Fabric CLI。
