---
title: 使用 Azure PowerShell 新增或移除 Azure 角色指派-Azure RBAC
description: 瞭解如何使用 Azure PowerShell 和 Azure 角色型存取控制，為使用者、群組、服務主體或受控識別授與 azure 資源的存取權 (Azure RBAC) 。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: f3fc54829be301c063440bd3508472287b6db265
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94648296"
---
# <a name="add-or-remove-azure-role-assignments-using-azure-powershell"></a>使用 Azure PowerShell 新增或移除 Azure 角色指派

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] 本文說明如何使用 Azure PowerShell 指派角色。

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>先決條件

若要新增或移除角色指派，您必須具有：

- `Microsoft.Authorization/roleAssignments/write` 和 `Microsoft.Authorization/roleAssignments/delete` 權限，例如[使用者存取系統管理員](built-in-roles.md#user-access-administrator)或[擁有者](built-in-roles.md#owner)
- Azure Cloud Shell 或[Azure PowerShell](/powershell/azure/install-az-ps) [的 PowerShell](../cloud-shell/overview.md)

## <a name="get-object-ids"></a>取得物件識別碼

若要加入或移除角色指派，您可能需要指定物件的唯一識別碼。 識別碼的格式如下：`11111111-1111-1111-1111-111111111111`。 您可以使用 Azure 入口網站或 Azure PowerShell 來取得識別碼。

### <a name="user"></a>User

若要取得 Azure AD 使用者的物件識別碼，您可以使用 [>get-azaduser](/powershell/module/az.resources/get-azaduser)。

```azurepowershell
Get-AzADUser -StartsWith <string_in_quotes>
(Get-AzADUser -DisplayName <name_in_quotes>).id
```

### <a name="group"></a>群組

若要取得 Azure AD 群組的物件識別碼，您可以使用 [remove-azadgroup](/powershell/module/az.resources/get-azadgroup)。

```azurepowershell
Get-AzADGroup -SearchString <group_name_in_quotes>
(Get-AzADGroup -DisplayName <group_name_in_quotes>).id
```

### <a name="application"></a>應用程式

若要取得應用程式) 所使用的 Azure AD 服務主體 (身分識別的物件識別碼，您可以使用 [>get-azadserviceprincipal](/powershell/module/az.resources/get-azadserviceprincipal)。 對於服務主體，請使用物件識別碼，而 **不是** 應用程式識別碼。

```azurepowershell
Get-AzADServicePrincipal -SearchString <service_name_in_quotes>
(Get-AzADServicePrincipal -DisplayName <service_name_in_quotes>).id
```

## <a name="add-a-role-assignment"></a>新增角色指派

在 Azure RBAC 中，若要授與存取權，您可以新增角色指派。

### <a name="user-at-a-resource-group-scope"></a>資源群組範圍的使用者

若要在資源群組範圍新增使用者的角色指派，請使用 [>new-azroleassignment](/powershell/module/az.resources/new-azroleassignment)。

```azurepowershell
New-AzRoleAssignment -SignInName <email_or_userprincipalname> -RoleDefinitionName <role_name> -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> New-AzRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales


RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales/pr
                     oviders/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

### <a name="using-the-unique-role-id"></a>使用唯一角色識別碼

有幾個時候角色名稱可能會變更，例如：

- 您正在使用自己的自訂角色，並決定變更名稱。
- 您使用的預覽角色在名稱中有 **(預覽)** 。 釋放角色時，就會重新命名角色。

> [!IMPORTANT]
> 預覽版本是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 可能不支援特定功能，或可能已經限制功能。
> 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

即使角色已重新命名，角色識別碼也不會變更。 如果您使用腳本或自動化來建立角色指派，最好使用唯一角色識別碼，而不是角色名稱。 因此，如果角色已重新命名，您的腳本可能會更有作用。

若要使用唯一角色識別碼（而非角色名稱）來新增角色指派，請使用 [>new-azroleassignment](/powershell/module/az.resources/new-azroleassignment)。

```azurepowershell
New-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionId <role_id> -Scope <resource_group_name/resource/management groups>
```

下列範例會將「[虛擬機器參與者](built-in-roles.md#virtual-machine-contributor)」角色指派給 *醫藥-sales* 資源群組範圍的 *alain \@ example.com* 使用者。 若要取得唯一角色識別碼，您可以使用 [>get-azroledefinition](/powershell/module/az.resources/get-azroledefinition) ，或查看 [Azure 內建角色](built-in-roles.md)。

```Example
PS C:\> New-AzRoleAssignment -ObjectId 44444444-4444-4444-4444-444444444444 -RoleDefinitionId 9980e02c-c2be-4d73-94e8-173b1dc7cf3c -Scope /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales/providers/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

### <a name="group-at-a-resource-scope"></a>資源範圍中的群組

若要在資源範圍新增群組的角色指派，請使用 [>new-azroleassignment](/powershell/module/az.resources/new-azroleassignment)。 如需如何取得群組之物件識別碼的詳細資訊，請參閱 [取得物件](#get-object-ids)識別碼。

```azurepowershell
New-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -ResourceName <resource_name> -ResourceType <resource_type> -ParentResource <parent resource> -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> Get-AzADGroup -SearchString "Pharma"

SecurityEnabled DisplayName         Id                                   Type
--------------- -----------         --                                   ----
           True Pharma Sales Admins aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa Group

PS C:\> New-AzRoleAssignment -ObjectId aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa -RoleDefinitionName "Virtual Machine Contributor" -ResourceName RobertVirtualNetwork -ResourceType Microsoft.Network/virtualNetworks -ResourceGroupName RobertVirtualNetworkResourceGroup

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork/providers/Microsoft.Authorizat
                     ion/roleAssignments/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork
DisplayName        : Pharma Sales Admins
SignInName         :
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa
ObjectType         : Group
CanDelegate        : False
```

### <a name="application-at-a-subscription-scope"></a>訂用帳戶範圍的應用程式

若要在訂用帳戶範圍中新增應用程式的角色指派，請使用 [>new-azroleassignment](/powershell/module/az.resources/new-azroleassignment)。 如需如何取得應用程式物件識別碼的詳細資訊，請參閱 [取得物件](#get-object-ids)識別碼。

```azurepowershell
New-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -Scope /subscriptions/<subscription_id>
```

```Example
PS C:\> New-AzRoleAssignment -ObjectId 77777777-7777-7777-7777-777777777777 -RoleDefinitionName "Reader" -Scope /subscriptions/00000000-0000-0000-0000-000000000000

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/66666666-6666-6666-6666-666666666666
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
DisplayName        : MyApp1
SignInName         :
RoleDefinitionName : Reader
RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId           : 77777777-7777-7777-7777-777777777777
ObjectType         : ServicePrincipal
CanDelegate        : False
```

### <a name="user-at-a-management-group-scope"></a>管理群組範圍的使用者

若要在管理群組範圍新增使用者的角色指派，請使用 [>new-azroleassignment](/powershell/module/az.resources/new-azroleassignment)。 若要取得管理群組識別碼，您可以在 Azure 入口網站的 [ **管理群組** ] 分頁上找到它，也可以使用 [AzManagementGroup](/powershell/module/az.resources/get-azmanagementgroup)。

```azurepowershell
New-AzRoleAssignment -SignInName <email_or_userprincipalname> -RoleDefinitionName <role_name> -Scope /providers/Microsoft.Management/managementGroups/<group_id>
```

```Example
PS C:\> New-AzRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Billing Reader" -Scope /providers/Microsoft.Management/managementGroups/marketing-group

RoleAssignmentId   : /providers/Microsoft.Management/managementGroups/marketing-group/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
Scope              : /providers/Microsoft.Management/managementGroups/marketing-group
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Billing Reader
RoleDefinitionId   : fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

## <a name="remove-a-role-assignment"></a>移除角色指派

在 Azure RBAC 中，若要移除存取權，您可以使用 [>new-azroleassignment](/powershell/module/az.resources/remove-azroleassignment)移除角色指派。

下列範例會從 *醫藥-sales* 資源群組上的 *alain \@ Example.com* 使用者移除「*虛擬機器參與者*」角色指派：

```Example
PS C:\> Remove-AzRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales
```

下列範例會從訂用帳戶範圍的 <object_id> 中移除 <role_name> 角色。

```azurepowershell
Remove-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -Scope /subscriptions/<subscription_id>
```

下列範例會從管理群組範圍的 <object_id> 中移除 <role_name> 角色。

```azurepowershell
Remove-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -Scope /providers/Microsoft.Management/managementGroups/<group_id>
```

如果您收到錯誤訊息：「提供的資訊未對應至角色指派」，請確定您也指定了 `-Scope` 或 `-ResourceGroupName` 參數。 如需詳細資訊，請參閱 [疑難排解 AZURE RBAC](troubleshooting.md#role-assignments-with-identity-not-found)。

## <a name="next-steps"></a>後續步驟

- [使用 Azure PowerShell 列出 Azure 角色指派](role-assignments-list-powershell.md)
- [教學課程：使用 Azure PowerShell 為群組授與 Azure 資源的存取權](tutorial-role-assignments-group-powershell.md)
- [教學課程：使用 Azure PowerShell 建立 Azure 自訂角色](tutorial-custom-role-powershell.md)
- [使用 Azure PowerShell 管理資源](../azure-resource-manager/management/manage-resources-powershell.md)