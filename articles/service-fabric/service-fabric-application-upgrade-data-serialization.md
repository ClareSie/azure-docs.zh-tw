---
title: 應用程式升級：資料序列化
description: 資料序列化的最佳作法，以及它如何影響應用程式輪流升級。
ms.topic: conceptual
ms.date: 11/02/2017
ms.openlocfilehash: 9474a0bf8041ac541389f782f60bf1220d690cde
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2020
ms.locfileid: "96575630"
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>資料序列化如何影響應用程式升級
在 [輪流應用程式升級](service-fabric-application-upgrade.md)中，升級會套用至節點的子集，一次一個升級網域。 在此過程中，有些升級網域會有您應用程式的新版本，有些升級網域則有您應用程式的舊版本。 在首度發行期間，新版的應用程式必須能夠讀取舊版的資料，而舊版的應用程式必須能夠讀取新版的資料。 如果資料格式沒有向前及向後相容，升級便可能會失敗，或是發生更糟糕的狀況，像是資料可能會遺失或損毀。 本文將討論您資料格式的構成項目並提供最佳作法，以確保您的資料向前及向後相容。

## <a name="what-makes-up-your-data-format"></a>資料格式的構成項目？
在 Azure Service Fabric 中，保留及複寫的資料來自您的 C# 類別。 針對使用[可靠集合](service-fabric-reliable-services-reliable-collections.md)的應用程式，該資料就是可靠的字典和佇列中的物件。 對於使用 [Reliable Actors](service-fabric-reliable-actors-introduction.md)的應用程式，也就是動作項目的備份狀態。 這些 C# 類別必須是可序列化，以便保存和複寫。 因此，資料格式是由已序列化的欄位和屬性，以及其序列化方式來定義。 例如，在 `IReliableDictionary<int, MyClass>` 中，資料是序列化 `int` 和序列化 `MyClass`。

### <a name="code-changes-that-result-in-a-data-format-change"></a>程式碼變更造成資料格式變更
由於資料格式是由 C# 類別決定，所以類別的變更可能會導致資料格式變更。 請小心確保輪流升級能夠處理資料格式變更。 可能會造成資料格式變更的範例：

* 新增或移除欄位或屬性
* 重新命名欄位或屬性
* 變更欄位或屬性的類型
* 變更類別名稱或命名空間

### <a name="data-contract-as-the-default-serializer"></a>將資料合約做為預設的序列化程式
序列化程式通常負責讀取資料，以及還原序列化為目前版本，即使資料是舊版或 *新* 版。 預設的序列化程式是 [資料合約序列化程式](/dotnet/framework/wcf/feature-details/using-data-contracts)，其具有定義完善的版本控制規則。 可靠的集合允許覆寫序列化程式，但是 Reliable Actors 目前不允許。 資料序列化程式在啟用輪流升級中扮演著重要的角色。 資料合約序列化程式是我們建議用於 Service Fabric 應用程式的序列化程式。

## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>資料格式如何影響輪流升級
輪流升級期間，有兩種序列化程式可能會遇到舊版或 *新* 版資料的主要案例：

1. 節點升級並重新啟動之後，新的序列化程式會載入資料，該資料保存到舊版的磁碟。
2. 輪流升級期間，叢集將會包含舊和新版本程式碼的混合。 由於複本會放在不同升級網域，且複本會互相傳送資料，因此您的新版和/或舊版資料可能會遇到新版和/或舊版序列化程式。

> [!NOTE]
> 此處的「新版」和「舊版」是指您正在執行的程式碼的版本。 「新序列化程式」是指在您的新版應用程式中執行的序列化程式程式碼。 「新資料」是指來自您的新版應用程式的序列化 C# 類別。
> 
> 

兩個版本的程式碼和資料格式必須同時向前及向後相容。 如果它們不相容，輪流升級可能會失敗，或可能會遺失資料。 輪流升級可能會失敗，因為程式碼或序列化程式在遇到其他版本時可能會擲回例外狀況或錯誤。 比方說，如果已加入新屬性，但是舊的序列化程式在還原序列化期間捨棄它，則可能會遺失資料。

資料合約是建議的解決方案，以確保您的資料相容。 它有新增、移除及變更欄位的定義完善版本控制規則。 它也支援處理未知欄位、連結到序列化和還原序列化程序，以及處理類別繼承。 如需詳細資訊，請參閱 [使用資料合約](/dotnet/framework/wcf/feature-details/using-data-contracts)。

## <a name="next-steps"></a>後續步驟
[使用 Visual Studio 升級您的應用程式](service-fabric-application-upgrade-tutorial.md) 會逐步引導您使用 Visual Studio 進行應用程式升級。

[使用 Powershell 升級您的應用程式](service-fabric-application-upgrade-tutorial-powershell.md) 會逐步引導您使用 powershell 來升級應用程式。

使用 [升級參數](service-fabric-application-upgrade-parameters.md)來控制您應用程式的升級方式。

瞭解如何在升級您的應用程式時使用先進的功能，方法是參考 [Advanced 主題](service-fabric-application-upgrade-advanced.md)。

藉由參考針對 [應用程式升級進行疑難排解](service-fabric-application-upgrade-troubleshooting.md)的步驟，修正應用程式升級中常見的問題。
