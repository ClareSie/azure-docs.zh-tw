---
title: Azure 防火牆規則處理邏輯
description: Azure 防火牆具有 NAT 規則、網路規則和應用程式規則。 規則的處理依據是規則類型。
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/18/2020
ms.author: victorh
ms.openlocfilehash: 01f7aa61d3bfb3c712320bbf138160a7ff8197c7
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "95502190"
---
# <a name="configure-azure-firewall-rules"></a>設定 Azure 防火牆規則
您可以在 Azure 防火牆上設定 NAT 規則、網路規則和應用程式規則。 系統會根據規則類型依優先權順序來處理規則集合，將較低的數位從100到65000。 規則集合名稱只能有字母、數位、底線、句點或連字號。 它必須以字母或數位開頭，並以字母、數位或底線結尾。 最大名稱長度為80個字元。

建議您一開始先將規則集合優先順序數位的100遞增 (100、200、300等) ，以便您可以視需要新增更多規則集合。

> [!NOTE]
> 如果您啟用以威脅情報為基礎的篩選，則這些規則會是最高的優先順序，一律會先進行處理。 威脅情報篩選可能會在任何已設定的規則處理之前拒絕流量。 如需詳細資訊，請參閱以 [Azure 防火牆威脅情報為基礎的篩選](threat-intel.md)。

## <a name="outbound-connectivity"></a>輸出連線能力

### <a name="network-rules-and-applications-rules"></a>網路規則和應用程式規則

如果您設定網路規則和應用程式規則，則會在應用程式規則之前依優先權順序套用網路規則。 之後規則就會終止。 因此，如果在網路規則中找到相符項，則不會處理其他規則。  如果沒有符合的網路規則，而且通訊協定為 HTTP、HTTPS 或 MSSQL，則會依優先順序將封包依應用程式規則進行評估。 如果仍然找不到相符項，則會根據 [基礎結構規則集合](infrastructure-fqdns.md)來評估封包。 如果仍然沒有相符的，則預設會拒絕封包。

#### <a name="network-rule-protocol"></a>網路規則通訊協定

您可以設定 **TCP**、 **UDP**、 **ICMP** 或 **任何** IP 通訊協定的網路規則。 任何 IP 通訊協定都會包含網際網路指派的號碼授權單位中所定義的所有 IP 通訊協定 [ (IANA) 通訊協定編號](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) 檔。 如果已明確設定目的地埠，則會將規則轉譯為 TCP + UDP 規則。

在2020年11月9日之前， **任何** 意思是 **TCP**、 **UDP** 或 **ICMP**。 因此，您可能已在該日期之前設定通訊協定 = Any 的規則，以及目的地埠 = ' * '。 如果您不想要允許目前定義的任何 IP 通訊協定，則請修改規則，以明確地設定您想要 (TCP、UDP 或 ICMP) ) 的通訊協定 (。

## <a name="inbound-connectivity"></a>輸入連線能力

### <a name="nat-rules"></a>NAT 規則

您可以藉由設定目的地網路位址轉譯 (DNAT) 來啟用輸入網際網路連線，如 [教學課程：使用 Azure 入口網站的 Azure 防火牆 DNAT 篩選輸入流量](tutorial-firewall-dnat.md)（機器翻譯）中所述。 NAT 規則會在網路規則之前優先套用。 如果找到相符項目，就會新增隱含的對應網路規則，以允許已轉譯的流量。 若要覆寫這個行為，您可以明確地使用符合已轉譯流量的拒絕規則來新增網路規則集合。

應用程式規則不適用於輸入連接。 因此，如果您想要篩選輸入 HTTP/S 流量，則應該使用 (WAF) 的 Web 應用程式防火牆。 如需詳細資訊，請參閱 [什麼是 Azure Web 應用程式防火牆？](../web-application-firewall/overview.md)

## <a name="examples"></a>範例

下列範例顯示其中一些規則組合的結果。

### <a name="example-1"></a>範例 1

因為有相符的網路規則，所以允許連線至 google.com。

**網路規則**

- 動作：Allow


|NAME  |通訊協定  |來源類型  |來源  |目的地類型  |目的地址  |目的地連接埠|
|---------|---------|---------|---------|----------|----------|--------|
|允許-web     |TCP|IP 位址|*|IP 位址|*|80,443

**應用程式規則**

- 動作：拒絕

|NAME  |來源類型  |來源  |通訊協定:連接埠|目標 Fqdn|
|---------|---------|---------|---------|----------|----------|
|拒絕-google     |IP 位址|*|HTTP：80、HTTPs：443|google.com

**結果**

因為封包符合 *允許的 web* 網路規則，所以允許 google.com 的連接。 規則處理會在此時間點停止。

### <a name="example-2"></a>範例 2

因為優先順序較高的 *拒絕* 網路規則集合封鎖 SSH 流量，所以拒絕 SSH 流量。

**網路規則集合1**

- 名稱：允許-集合
- 優先順序：200
- 動作：Allow

|NAME  |通訊協定  |來源類型  |來源  |目的地類型  |目的地址  |目的地連接埠|
|---------|---------|---------|---------|----------|----------|--------|
|允許-SSH     |TCP|IP 位址|*|IP 位址|*|22

**網路規則集合2**

- 名稱：拒絕-集合
- 優先順序：100
- 動作：拒絕

|NAME  |通訊協定  |來源類型  |來源  |目的地類型  |目的地址  |目的地連接埠|
|---------|---------|---------|---------|----------|----------|--------|
|拒絕-SSH     |TCP|IP 位址|*|IP 位址|*|22

**結果**

SSH 連線遭到拒絕，因為較高優先順序的網路規則集合會封鎖它。 規則處理會在此時間點停止。

## <a name="rule-changes"></a>規則變更

如果您將規則變更為拒絕先前允許的流量，則會捨棄任何相關的現有會話。

## <a name="next-steps"></a>後續步驟

- 瞭解如何 [部署和設定 Azure 防火牆](tutorial-firewall-deploy-portal.md)。