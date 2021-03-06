---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 11/20/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: fb5f6dbb410a15bf88fccc1ed3f9c3ca0f830c70
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "96296077"
---
|名稱<br /><sub>(Azure 入口網站)</sub> |描述 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Synapse 工作區應使用客戶管理的金鑰來加密待用資料](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff7d52b2d-e161-4dfa-a82b-55e564167385) |使用客戶管理的金鑰來控制 Azure Synapse 工作區中儲存之資料的待用加密。 客戶管理的金鑰提供雙重加密，方法是使用服務管理的金鑰，在預設加密之上新增第二層加密。 |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceCMK_Audit.json) |
|[應移除 Azure Synapse 工作區上的 IP 防火牆規則](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F56fd377d-098c-4f02-8406-81eb055902b8) |移除所有 IP 防火牆規則可確保您的 Azure Synapse 工作區只能從私人端點存取，藉以改善安全性。 此設定會審核建立允許在工作區上進行公用網路存取的防火牆規則。 |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceFirewallRules_Audit.json) |
|[應啟用 Azure Synapse 工作區上的受控工作區虛擬網路](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2d9dbfa3-927b-4cf0-9d0f-08747f971650) |啟用受控工作區虛擬網路可確保您的工作區與其他工作區的網路隔離。 在此虛擬網路中部署的資料整合和 Spark 資源也提供 Spark 活動的使用者層級隔離。 |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceManagedVnet_Audit.json) |
|[應啟用 Azure Synapse 工作區上的私人端點連線](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F72d11df1-dd8a-41f7-8925-b05b960ebafc) |私人端點可以設定為私下連線至 Azure Synapse 工作區。 這是用來對 Azure Synapse 工作區強制執行安全通道。 |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceUsePrivateLinks_Audit.json) |
|[Synapse 受控私人端點應該只連接到已核准 Azure Active Directory 租使用者中的資源](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3a003702-13d2-4679-941b-937e58c443f0) |只允許 Azure Active Directory (Azure AD) 租使用者中的資源連線，以保護您的 Synapse 工作區。 核准的 Azure AD 租使用者可以在原則指派期間定義。 |稽核、停用、拒絕 |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/Workspace_DataExfiltrationPrevention_Deny.json) |
