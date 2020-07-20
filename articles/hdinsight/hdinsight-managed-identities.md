---
title: Azure HDInsight 中的受控識別
description: 提供 Azure HDInsight 中受控識別的執行總覽。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/15/2020
ms.openlocfilehash: 1081865a2e138af38ba171197719f08dedf6ffdb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "81408930"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Azure HDInsight 中的受控識別

受控識別是在 Azure Active Directory （Azure AD）中註冊的身分識別，其認證是由 Azure 管理。 使用受控識別時，您不需要在 Azure AD 中註冊服務主體。 或維護認證，例如憑證。

受控識別會在 Azure HDInsight 中用來存取 Azure AD 網域服務，或在需要時存取 Azure Data Lake Storage Gen2 中的檔案。

受控識別有兩種類型：使用者指派和系統指派。 Azure HDInsight 僅支援使用者指派的受控識別。 HDInsight 不支援系統指派的受控識別。 使用者指派的受控識別會建立為獨立的 Azure 資源，您可以將其指派給一或多個 Azure 服務實例。 相反地，系統指派的受控識別會建立在 Azure AD 中，然後直接在特定 Azure 服務實例上啟用。 系統指派的受控識別的生命週期接著會系結至其啟用所在之服務實例的生命週期。

## <a name="hdinsight-managed-identity-implementation"></a>HDInsight 受控識別執行

在 Azure HDInsight 中，受管理的身分識別會布建在叢集的每個節點上。 不過，這些身分識別元件僅供 HDInsight 服務使用。 目前沒有任何支援的方法可使用安裝在 HDInsight 叢集節點上的受控識別來產生存取權杖。 針對某些 Azure 服務，受控識別會使用可供您用來取得存取權杖的端點來執行。 使用權杖與您自己的其他 Azure 服務互動。

## <a name="create-a-managed-identity"></a>建立受控識別

您可以使用下列任何方法來建立受控識別：

* [Azure 入口網站](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

設定受控識別的其餘步驟取決於將使用它的案例。

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Azure HDInsight 中的受控識別案例

受控識別會在多個案例的 Azure HDInsight 中使用。 如需詳細的安裝和設定指示，請參閱相關檔：

* [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-assigned-managed-identity) \(部分機器翻譯\)
* [企業安全性套件](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [客戶管理的金鑰磁碟加密](disk-encryption.md)

## <a name="faq"></a>常見問題集

### <a name="what-happens-if-i-delete-the-managed-identity-after-the-cluster-creation"></a>如果我在叢集建立之後刪除受控識別，會發生什麼事？

當需要受控識別時，您的叢集會遇到問題。 在建立叢集之後，目前沒有任何方法可更新或變更受控識別。 因此我們的建議是要確保在叢集執行時間期間不會刪除受控識別。 或者，您可以重新建立叢集並指派新的受控識別。

## <a name="next-steps"></a>後續步驟

* [什麼是適用於 Azure 資源的受控識別？](../active-directory/managed-identities-azure-resources/overview.md)
