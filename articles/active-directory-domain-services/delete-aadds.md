---
title: 刪除 Azure Active Directory Domain Services |Microsoft Docs
description: 瞭解如何使用 Azure 入口網站來停用或刪除 Azure Active Directory Domain Services 受控網域
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 07/06/2020
ms.author: iainfou
ms.openlocfilehash: bece22fa0b9d41f29f8829d47a26f4ae2075feac
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86040107"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>使用 Azure 入口網站刪除 Azure Active Directory Domain Services 受控網域

如果您不再需要 Azure Active Directory Domain Services （Azure AD DS）受控網域，您可以將它刪除。 沒有任何選項可關閉或暫時停用 Azure AD DS 受控網域。 刪除受控網域不會刪除或對 Azure AD 租使用者造成不良影響。

本文說明如何使用 Azure 入口網站來刪除受控網域。

> [!WARNING]
> **刪除是永久的，無法復原。**
> 
> 當您刪除受控網域時，會發生下列步驟：
>   * 受控網域的網域控制站會解除佈建，並從虛擬網路中移除。
>   * 受控網域上的資料會永久刪除。 此資料包括您所建立的自訂 Ou、Gpo、自訂 DNS 記錄、服務主體、Gmsa 等等。
>   * 聯結至受控網域的機器會失去與網域間的信任關係，且必須從網域中解除聯結。
>       * 您無法使用公司 AD 認證登入這些電腦。 相反地，您必須使用電腦的本機系統管理員認證。

## <a name="delete-the-managed-domain"></a>刪除受控網域

若要刪除受控網域，請完成下列步驟：

1. 在 [Azure 入口網站中，搜尋並選取 [ **Azure AD Domain Services**]。
1. 選取受控網域的名稱，例如*aaddscontoso.com*。
1. 在 [概觀] 頁面上，按一下 [刪除]。 若要確認刪除，請再次輸入受控網域的功能變數名稱，然後選取 [**刪除**]。

可能需要15-20 分鐘或更久的時間來刪除受控網域。

## <a name="next-steps"></a>後續步驟

請考慮針對您想要在 Azure AD DS 中看到的功能，[分享意見][feedback]反應。

如果您想要再次開始使用 Azure AD DS，請參閱[建立和設定 Azure Active Directory Domain Services 受控網域][create-instance]。

<!-- INTERNAL LINKS -->
[feedback]: contact-us.md
[create-instance]: tutorial-create-instance.md
