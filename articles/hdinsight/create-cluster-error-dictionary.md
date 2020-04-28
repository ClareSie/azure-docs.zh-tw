---
title: Azure HDInsight 建立叢集-錯誤字典
description: 瞭解如何針對建立 Azure HDInsight 叢集時所發生的錯誤進行疑難排解
author: karkrish
ms.author: v-todmc
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/19/2019
ms.openlocfilehash: 39179c9b6d02d810561485f6a4af0102711ad0ef
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82186629"
---
# <a name="azure-hdinsight-cluster-creation-errors"></a>Azure HDInsight：叢集建立錯誤

本文說明您在建立叢集時可能會遇到之錯誤的解決方式。

> [!NOTE]
> 本文中所述的前三個錯誤是驗證錯誤。 當 Azure HDInsight 產品使用**CsmDocument_2_0**類別時，就可能發生這種情況。

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>錯誤碼： DeploymentDocument ' CsmDocument_2_0 ' 無法通過驗證

### <a name="error"></a>錯誤

「無法存取腳本動作位置 URI：\<腳本動作 URL」\>

#### <a name="error-message"></a>錯誤訊息

「遠端伺服器傳回錯誤：（404）找不到」。

### <a name="cause"></a>原因

HDInsight 服務無法存取您在建立叢集要求中提供的腳本動作 URL。 當服務嘗試存取腳本動作時，會收到先前的錯誤訊息。

### <a name="resolution"></a>解決方法

- 若為 HTTP 或 HTTPS URL，請嘗試從 incognito 瀏覽器視窗中移至 URL，以確認它。
- 針對 WASB URL，請確定您在要求中提供的儲存體帳戶中有該腳本。 也請確定此儲存體帳戶的儲存體金鑰是正確的。
- 針對 ADLS URL，請確定此腳本存在於儲存體帳戶中。

---

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>錯誤碼： DeploymentDocument ' CsmDocument_2_0 ' 無法通過驗證

### <a name="error"></a>錯誤

「無法存取腳本動作位置 URI： \<SCRIPT_ACTION_URL」\>

#### <a name="error-message"></a>錯誤訊息

「指定的腳本 URI \<SCRIPT_URI\>在 ADLS 中，但此叢集沒有 data lake storage 主體」

### <a name="cause"></a>原因

HDInsight 服務無法存取您在建立叢集要求中提供的腳本動作 URL。 當服務嘗試存取腳本動作時，會收到先前的錯誤訊息。

### <a name="resolution"></a>解決方法

將對應的 Azure Data Lake Storage Gen 1 帳戶新增至叢集。 此外，將存取 Data Lake Storage Gen 1 帳戶的服務主體新增至叢集。

---

## <a name="error-code-deploymentdocument-csmdocument_2_0-failed-the-validation"></a>錯誤碼： DeploymentDocument ' CsmDocument_2_0 ' 無法通過驗證

### <a name="error"></a>錯誤

在要求中提供\<的\>「VM 大小」 CUSTOMER_SPECIFIED_VM_SIZE ' 無效，或角色「\<角色\>」不支援。 有效的值為\<：\>VALID_VM_SIZE_FOR_ROLE」。

### <a name="cause"></a>原因

角色不允許您指定的虛擬機器大小。 發生此錯誤的原因可能是 VM 大小值未如預期般運作，或不適用於電腦角色。

### <a name="resolution"></a>解決方法

錯誤訊息會列出 VM 大小的有效值。 選取其中一個值，然後重試建立叢集要求。

---

## <a name="error-codeinvalidvirtualnetworkid"></a>錯誤碼： InvalidVirtualNetworkId  

### <a name="error"></a>錯誤

「VirtualNetworkId 無效。 VirtualNetworkId '\<USER_VIRTUALNETWORKID\>' * "

### <a name="cause"></a>原因

您在叢集建立期間指定的**VirtualNetworkId**值格式不正確。

### <a name="resolution"></a>解決方法

請確定**VirtualNetworkId**和子網值的格式正確。 若要取得**VirtualNetworkId**值：

1. 移至 Azure 入口網站。
1. 選取您的虛擬網路。
1. 選取 [**屬性**] 功能表項目。 **ResourceID**屬性值是**VirtualNetworkId**值。

以下是虛擬網路識別碼的範例：

"/subscriptions/c15fd9b8-e2b8-1d4e-aa85-2e668040233b/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet"

---

## <a name="error-code-customizationfailederrorcode"></a>錯誤碼： CustomizationFailedErrorCode

### <a name="error"></a>錯誤

「叢集部署因自訂腳本動作發生錯誤而失敗。 失敗的動作\<：\>SCRIPT_NAME，請移至 Ambari UI 以進一步偵測失敗。」

### <a name="cause"></a>原因

您在建立叢集要求期間所提供的自訂腳本，會在成功部署叢集之後執行。 這個錯誤碼表示在執行名為\<SCRIPT_NAME\>的自訂腳本期間，發生錯誤。

### <a name="resolution"></a>解決方法

由於腳本是您的自訂腳本，因此建議您針對問題進行疑難排解，並視需要重新執行腳本。 若要針對腳本失敗進行疑難排解，請檢查/var/lib/ambari-agent/* 資料夾中的記錄檔。 或者，在 Ambari UI 中開啟 [**作業**] 頁面，然後選取**run_customscriptaction**操作以查看錯誤詳細資料。

---

## <a name="error-codeinvaliddocumenterrorcode"></a>錯誤碼： InvalidDocumentErrorCode

### <a name="error"></a>錯誤

「資料庫\< \> \<DATABASE_NAME\>中的 META_STORE_TYPE 中繼存放區\>架構版本\<METASTORE_MAJOR_VERSION 與叢集版本\<CLUSTER_VERSION\>不相容」

### <a name="cause"></a>原因

自訂中繼存放區與選取的 HDInsight 叢集版本不相容。 目前，HDInsight 4.0 叢集僅支援中繼存放區3.0 版和更新版本，而 HDInsight 3.6 叢集不支援中繼存放區版本3.0 和更新版本。

### <a name="resolution"></a>解決方法

僅使用 HDInsight 叢集版本所支援的中繼存放區版本。 如果您未指定自訂中繼存放區，HDInsight 會在內部建立中繼存放區，然後在刪除叢集時刪除它。

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>錯誤碼： FailedToConnectWithClusterErrorCode 

### <a name="error"></a>錯誤

「無法連接到叢集管理端點來執行調整作業。 確認網路安全性規則不會封鎖對叢集的外部存取，而且可以成功存取叢集管理員（Ambari） UI。」

### <a name="cause"></a>原因

網路安全性群組（NSG）上的防火牆規則會封鎖與重要 Azure 健康狀態和管理服務的叢集通訊。

### <a name="resolution"></a>解決方法

如果您打算使用網路安全性群組來控制網路流量，請在安裝 HDInsight 之前，採取下列動作：

- 識別您要用於 HDInsight 的 Azure 區域。
- 識別 HDInsight 所需的 IP 位址。 如需詳細資訊，請參閱 [HDInsight 管理 IP 位址](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)。
  - 建立或修改您打算安裝 HDInsight 之子網的網路安全性群組。
  - 針對 [網路安全性群組]，允許埠443上來自 IP 位址的輸入流量。 此設定可確保 HDInsight 管理服務可以從虛擬網路外部連線到叢集。

---

## <a name="error-code-storagepermissionsblockedformsi"></a>錯誤碼： StoragePermissionsBlockedForMsi

### <a name="error"></a>錯誤

「受控識別沒有儲存體帳戶的許可權。 請確認「儲存體 Blob 資料擁有者」角色已指派給儲存體帳戶的受控識別。 儲存體：/subscriptions/ \<訂用\>帳戶\<識別碼/ResourceGroups/資源\>組\<名/providers/Microsoft.Storage/storageAccounts/儲存體\>帳戶名稱，受控識別\<：/Subscriptions/\>訂用\<帳戶識別碼/resourceGroups/\> / \<資源組名/providers/Microsoft.ManagedIdentity/userAssignedIdentities/\>使用者受控識別名稱 "

### <a name="cause"></a>原因

您未提供管理身分識別所需的許可權。 使用者指派的受控識別在 Azure Data Lake Storage Gen2 儲存體帳戶上沒有「Blob 儲存體參與者」角色。

### <a name="resolution"></a>解決方法

1. 開啟 Azure 入口網站。
1. 移至您的儲存體帳戶。
1. 查看**存取控制（IAM）** 底下。
1. 請確定使用者已指派儲存體 Blob 資料參與者角色或儲存體 Blob 資料擁有者角色。

如需詳細資訊，請參閱在[Data Lake Storage Gen2 帳戶上設定受控識別的許可權](hdinsight-hadoop-use-data-lake-storage-gen2.md)。

---

## <a name="error-code-invalidnetworksecuritygroupsecurityrules"></a>錯誤碼：出現 invalidnetworksecuritygroupsecurityrules

### <a name="error"></a>錯誤

「網路\<安全性群組中的安全性規則/Subscriptions/SubscriptionID\>/resourceGroups/<資源組名\>預設/提供者/Microsoft。網路/networkSecurityGroups/\<網路安全性群組名稱\>設定了子網\</Subscriptions/\>SubscriptionID\</resourceGroups/資源組\>名 RG-westeurope-vnet-tomtom-default/providers/Microsoft。 network/virtualNetworks\</虛擬網路\>name\</subnets/子\>網名稱不允許必要的輸入和/或輸出連線。 如需詳細資訊，請造訪[規劃虛擬網路以取得 Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment)或聯絡支援。」

### <a name="cause"></a>原因

如果網路安全性群組或使用者定義路由（Udr）控制對您 HDInsight 叢集的輸入流量，請確定您的叢集可以與重要的 Azure 健康情況和管理服務進行通訊。

### <a name="resolution"></a>解決方法

如果您打算使用網路安全性群組來控制網路流量，請在安裝 HDInsight 之前，採取下列動作：

- 識別您打算用於 HDInsight 的 Azure 區域，並為您的區域建立安全的 IP 位址清單。 如需詳細資訊，請參閱[健全狀況和管理服務：特定區域](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses#health-and-management-services-specific-regions)。
- 識別 HDInsight 所需的 IP 位址。 如需詳細資訊，請參閱 [HDInsight 管理 IP 位址](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)。
- 建立或修改您打算安裝 HDInsight 之子網的網路安全性群組。 針對 [網路安全性群組]，允許埠443上來自 IP 位址的輸入流量。 此設定可確保 HDInsight 管理服務可以從虛擬網路外部連線到叢集。

---

## <a name="error-code-cluster-setup-failed-to-install-components-on-one-or-more-hosts"></a>錯誤碼：叢集安裝程式無法在一或多部主機上安裝元件

### <a name="error"></a>錯誤

「叢集安裝程式無法在一或多部主機上安裝元件。 請重試您的要求。」

### <a name="cause"></a>原因 

通常，當發生暫時性問題或 Azure 中斷時，就會產生此錯誤。

### <a name="resolution"></a>解決方法

檢查[azure 狀態](https://status.azure.com)頁面中可能會影響叢集部署的任何 azure 中斷。 如果沒有中斷，請重試叢集部署。

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>錯誤碼： FailedToConnectWithClusterErrorCode

### <a name="error"></a>錯誤

無法連接到叢集管理端點。 請稍後重試。

### <a name="cause"></a>原因

當您嘗試建立叢集時，HDInsight 服務無法連線到您的叢集

### <a name="resolution"></a>解決方法

如果您使用自訂 VNet 網路安全性群組（Nsg）和使用者定義路由（Udr），請確定您的叢集可以與 HDInsight 管理服務通訊。 如需其他資訊，請參閱[HDInsight 管理 IP 位址](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)。

---

## <a name="error-code-deployments-failed-due-to-policy-violation-resource-resource-uri-was-disallowed-by-policy-policy-identifiers-policyassignmentnamepolicy-name-idprovidersmicrosoftmanagementmanagementgroupsmanagement-group-name-providersmicrosoftauthorizationpolicyassignmentspolicy-namepolicydefinition-policy-definition"></a>錯誤碼：部署因原則違規而失敗：原則不允許「<Resource URI>資源」。 原則識別碼： ' [{"policyAssignment"： {"name"： "<Policy Name> "，"id"： "/providers/Microsoft.Management/managementGroups/<Management Group Name> Provider/Microsoft. Authorization/policyAssignments/<Policy Name>"}，"policyDefinition"：<Policy Definition>

### <a name="cause"></a>原因

以訂用帳戶為基礎的 Azure 原則可以拒絕建立公用 IP 位址。 建立 HDInsight 叢集需要兩個公用 IP。

下列原則通常會影響叢集建立：

* 防止在訂用帳戶內建立 IP 位址或負載平衡器的原則。
* 防止建立儲存體帳戶的原則。
* 防止刪除網路資源的原則，例如 IP 位址或負載平衡器。

### <a name="resolution"></a>解決方法

在建立 HDInsight 叢集時，刪除或停用以訂用帳戶為基礎的 Azure 原則指派。

---

## <a name="next-steps"></a>後續步驟

如需有關疑難排解叢集建立錯誤的詳細資訊，請參閱[使用 Azure HDInsight 針對叢集建立失敗進行疑難排解](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-troubleshoot-cluster-creation-fails)。
