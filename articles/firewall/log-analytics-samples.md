---
title: Azure 防火牆的 Azure 監視器記錄
description: Azure 監視器記錄可以用來分析您的 Azure 防火牆。 範例檔案是 Azure 監視器中的 [View Designer] 內建。
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 09/11/2020
ms.author: victorh
ms.openlocfilehash: 86538f6d0467eb15e549179166ca957902a2d0c3
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94659551"
---
# <a name="azure-monitor-logs-for-azure-firewall"></a>Azure 防火牆的 Azure 監視器記錄

您可以使用下列 Azure 監視器記錄範例來分析您的 Azure 防火牆記錄。 範例檔案是 Azure 監視器中的 [View Designer]，Azure 監視器文章中的 [View designer](../azure-monitor/platform/view-designer.md) 有關于視圖設計概念的詳細資訊。

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="azure-monitor-logs-view"></a>Azure 監視器記錄檔視圖

以下是您可以設定範例 Azure 監視器記錄視覺效果的方式。 您可以從 [azure-docs-json-samples](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-firewall/AzureFirewall.omsview) \(英文\) 存放庫下載該範例視覺效果。 最簡單的方法是以滑鼠右鍵按一下本頁面上的超連結，然後選擇 [另存新檔]，並提供如 **AzureFirewall.omsview** 的名稱。 

執行下列步驟，將此視圖新增至 Log Analytics 工作區：

1. 在 Azure 入口網站中開啟 Log Analytics 工作區。
2. 開啟位於 [一般] 底下的 [檢視設計工具]。
3. 按一下 [匯入]  。
4. 瀏覽並選取您之前所下載的 **AzureFirewall.omsview** 檔案。
5. 按一下 [檔案] 。

以下是針對應用程式規則記錄資料的檢視外觀：

![應用程式規則記錄資料](./media/log-analytics-samples/azurefirewall-applicationrulelogstats.png)

而以下是網路規則記錄資料的外觀：

![網路規則記錄資料]( ./media/log-analytics-samples/azurefirewall-networkrulelogstats.png)

Azure 防火牆會將下列資料記錄在 AzureDiagnostics，並將類別列為 **>azurefirewallapplicationrule** 或 **AzureFirewallNetworkRule**。 包含詳細資料的資料會儲存在 msg_s 欄位中。 使用 [parse](/azure/kusto/query/parseoperator) \(英文\) 運算子，我們便可以從 msg_s 欄位中擷取各種有趣的屬性。 下列查詢會同時從兩個類別中擷取資訊。

## <a name="application-rules-log-data-query"></a>應用程式規則記錄資料查詢

下列查詢會剖析應用程式規則記錄資料。 各個不同的註解行中提供有建置查詢之方式的指導：

```Kusto
AzureDiagnostics
| where Category == "AzureFirewallApplicationRule"
//using :int makes it easier to pars but later we'll convert to string as we're not interested to do mathematical functions on these fields
//this first parse statement is valid for all entries as they all start with this format
| parse msg_s with Protocol " request from " SourceIP ":" SourcePortInt:int " " TempDetails
//case 1: for records that end with: "was denied. Reason: SNI TLS extension was missing."
| parse TempDetails with "was " Action1 ". Reason: " Rule1
//case 2: for records that end with
//"to ocsp.digicert.com:80. Action: Allow. Rule Collection: RC1. Rule: Rule1"
//"to v10.vortex-win.data.microsoft.com:443. Action: Deny. No rule matched. Proceeding with default action"
| parse TempDetails with "to " FQDN ":" TargetPortInt:int ". Action: " Action2 "." *
//case 2a: for records that end with:
//"to ocsp.digicert.com:80. Action: Allow. Rule Collection: RC1. Rule: Rule1"
| parse TempDetails with * ". Rule Collection: " RuleCollection2a ". Rule:" Rule2a
//case 2b: for records that end with:
//for records that end with: "to v10.vortex-win.data.microsoft.com:443. Action: Deny. No rule matched. Proceeding with default action"
| parse TempDetails with * "Deny." RuleCollection2b ". Proceeding with" Rule2b
| extend 
SourcePort = tostring(SourcePortInt)
|extend
TargetPort = tostring(TargetPortInt)
| extend
//make sure we only have Allowed / Deny in the Action Field
Action1 = case(Action1 == "Deny","Deny","Unknown Action")
| extend
    Action = case(Action2 == "",Action1,Action2),
    Rule = case(Rule2a == "",case(Rule1 == "",case(Rule2b == "","N/A", Rule2b),Rule1),Rule2a), 
    RuleCollection = case(RuleCollection2b == "",case(RuleCollection2a == "","No rule matched",RuleCollection2a),RuleCollection2b),
    FQDN = case(FQDN == "", "N/A", FQDN),
    TargetPort = case(TargetPort == "", "N/A", TargetPort)
| project TimeGenerated, msg_s, Protocol, SourceIP, SourcePort, FQDN, TargetPort, Action ,RuleCollection, Rule
```

以下是以更精簡方式呈現的相同查詢：

```Kusto
AzureDiagnostics
| where Category == "AzureFirewallApplicationRule"
| parse msg_s with Protocol " request from " SourceIP ":" SourcePortInt:int " " TempDetails
| parse TempDetails with "was " Action1 ". Reason: " Rule1
| parse TempDetails with "to " FQDN ":" TargetPortInt:int ". Action: " Action2 "." *
| parse TempDetails with * ". Rule Collection: " RuleCollection2a ". Rule:" Rule2a
| parse TempDetails with * "Deny." RuleCollection2b ". Proceeding with" Rule2b
| extend SourcePort = tostring(SourcePortInt)
| extend TargetPort = tostring(TargetPortInt)
| extend Action1 = case(Action1 == "Deny","Deny","Unknown Action")
| extend Action = case(Action2 == "",Action1,Action2),Rule = case(Rule2a == "", case(Rule1 == "",case(Rule2b == "","N/A", Rule2b),Rule1),Rule2a), 
RuleCollection = case(RuleCollection2b == "",case(RuleCollection2a == "","No rule matched",RuleCollection2a), RuleCollection2b),FQDN = case(FQDN == "", "N/A", FQDN),TargetPort = case(TargetPort == "", "N/A", TargetPort)
| project TimeGenerated, msg_s, Protocol, SourceIP, SourcePort, FQDN, TargetPort, Action ,RuleCollection, Rule
```

## <a name="network-rules-log-data-query"></a>網路規則記錄資料查詢

下列查詢會剖析網路規則記錄資料。 各個不同的註解行中提供有建置查詢之方式的指導：

```Kusto
AzureDiagnostics
| where Category == "AzureFirewallNetworkRule"
//using :int makes it easier to pars but later we'll convert to string as we're not interested to do mathematical functions on these fields
//case 1: for records that look like this:
//TCP request from 10.0.2.4:51990 to 13.69.65.17:443. Action: Deny//Allow
//UDP request from 10.0.3.4:123 to 51.141.32.51:123. Action: Deny/Allow
//TCP request from 193.238.46.72:50522 to 40.119.154.83:3389 was DNAT'ed to 10.0.2.4:3389
| parse msg_s with Protocol " request from " SourceIP ":" SourcePortInt:int " to " TargetIP ":" TargetPortInt:int *
//case 1a: for regular network rules
//TCP request from 10.0.2.4:51990 to 13.69.65.17:443. Action: Deny//Allow
//UDP request from 10.0.3.4:123 to 51.141.32.51:123. Action: Deny/Allow
| parse msg_s with * ". Action: " Action1a
//case 1b: for NAT rules
//TCP request from 193.238.46.72:50522 to 40.119.154.83:3389 was DNAT'ed to 10.0.2.4:3389
| parse msg_s with * " was " Action1b " to " NatDestination
//case 2: for ICMP records
//ICMP request from 10.0.2.4 to 10.0.3.4. Action: Allow
| parse msg_s with Protocol2 " request from " SourceIP2 " to " TargetIP2 ". Action: " Action2
| extend
SourcePort = tostring(SourcePortInt),
TargetPort = tostring(TargetPortInt)
| extend 
    Action = case(Action1a == "", case(Action1b == "",Action2,Action1b), Action1a),
    Protocol = case(Protocol == "", Protocol2, Protocol),
    SourceIP = case(SourceIP == "", SourceIP2, SourceIP),
    TargetIP = case(TargetIP == "", TargetIP2, TargetIP),
    //ICMP records don't have port information
    SourcePort = case(SourcePort == "", "N/A", SourcePort),
    TargetPort = case(TargetPort == "", "N/A", TargetPort),
    //Regular network rules don't have a DNAT destination
    NatDestination = case(NatDestination == "", "N/A", NatDestination)
| project TimeGenerated, msg_s, Protocol, SourceIP,SourcePort,TargetIP,TargetPort,Action, NatDestination
```

以下是以更精簡方式呈現的相同查詢：

```Kusto
AzureDiagnostics
| where Category == "AzureFirewallNetworkRule"
| parse msg_s with Protocol " request from " SourceIP ":" SourcePortInt:int " to " TargetIP ":" TargetPortInt:int *
| parse msg_s with * ". Action: " Action1a
| parse msg_s with * " was " Action1b " to " NatDestination
| parse msg_s with Protocol2 " request from " SourceIP2 " to " TargetIP2 ". Action: " Action2
| extend SourcePort = tostring(SourcePortInt),TargetPort = tostring(TargetPortInt)
| extend Action = case(Action1a == "", case(Action1b == "",Action2,Action1b), Action1a),Protocol = case(Protocol == "", Protocol2, Protocol),SourceIP = case(SourceIP == "", SourceIP2, SourceIP),TargetIP = case(TargetIP == "", TargetIP2, TargetIP),SourcePort = case(SourcePort == "", "N/A", SourcePort),TargetPort = case(TargetPort == "", "N/A", TargetPort),NatDestination = case(NatDestination == "", "N/A", NatDestination)
| project TimeGenerated, msg_s, Protocol, SourceIP,SourcePort,TargetIP,TargetPort,Action, NatDestination
```

## <a name="threat-intelligence-log-data-query"></a>威脅情報記錄資料查詢

下列查詢會剖析威脅情報規則記錄資料：

```Kusto
AzureDiagnostics
| where OperationName  == "AzureFirewallThreatIntelLog"
| parse msg_s with Protocol " request from " SourceIP ":" SourcePortInt:int " to " TargetIP ":" TargetPortInt:int *
| parse msg_s with * ". Action: " Action "." Message
| parse msg_s with Protocol2 " request from " SourceIP2 " to " TargetIP2 ". Action: " Action2
| extend SourcePort = tostring(SourcePortInt),TargetPort = tostring(TargetPortInt)
| extend Protocol = case(Protocol == "", Protocol2, Protocol),SourceIP = case(SourceIP == "", SourceIP2, SourceIP),TargetIP = case(TargetIP == "", TargetIP2, TargetIP),SourcePort = case(SourcePort == "", "N/A", SourcePort),TargetPort = case(TargetPort == "", "N/A", TargetPort)
| sort by TimeGenerated desc | project TimeGenerated, msg_s, Protocol, SourceIP,SourcePort,TargetIP,TargetPort,Action,Message
```

## <a name="sample-logs"></a>範例記錄檔

下列記錄範例顯示記錄專案中包含的資料。

:::image type="content" source="media/log-analytics-samples/log1.png" alt-text="記錄專案的螢幕擷取畫面。可以看到多個值，例如時間戳記、通訊協定、埠號碼、動作、規則集合和規則。" border="false":::

:::image type="content" source="media/log-analytics-samples/log2.png" alt-text="記錄專案的螢幕擷取畫面。顯示多個值，例如時間戳記、通訊協定、來源和目標 I P 位址，以及動作。" border="false":::

:::image type="content" source="media/log-analytics-samples/log3.png" alt-text="記錄專案的螢幕擷取畫面。顯示多個值，例如時間戳記、通訊協定、來源和目標 I P 位址和埠，以及訊息。" border="false":::
## <a name="next-steps"></a>後續步驟

若要了解 Azure 防火牆監視與診斷，請參閱[教學課程：監視 Azure 防火牆記錄和計量](./firewall-diagnostics.md)。