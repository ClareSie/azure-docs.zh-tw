---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 14a6703b3e256d33ab3b18e1821587cc3eb293db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "79381564"
---
對於運行的應用程式，確保虛擬機器 （VM） 的安全非常重要。 保護 VM 可包含一或多項 Azure 服務和功能，其中涵蓋保護 VM 的存取權及保護資料的儲存體。 本文提供可讓您保護 VM 和應用程式的資訊。

## <a name="antimalware"></a>反惡意程式碼

現今雲端環境的威脅型態非常多變，因此為了符合法規和達到安全性需求，在維護有效保護機制方面增加許多壓力。 [適用於 Azure 的 Microsoft Antimalware](../articles/security/fundamentals/antimalware.md) 是即時保護功能，有助於識別和移除病毒、間諜軟體和其他惡意軟體。 您可設定警示，在已知惡意或非必要軟體嘗試自行安裝或在您的 VM 上執行時通知您。 運行 Linux 或 Windows 伺服器 2008 的 VM 不支援它。

## <a name="azure-security-center"></a>Azure 資訊安全中心

[Azure 資訊安全中心](../articles/security-center/security-center-intro.md)可協助您保護 VM、偵測威脅並採取相應的措施。 資訊安全中心能提供您 Azure 訂用帳戶之間的整合式安全性監視和原則管理，協助偵測原先可能會忽略的威脅，且適用於廣泛的安全性解決方案生態系統。

安全中心的及時訪問可以應用於 VM 部署，以鎖定 Azure VM 的入站流量，減少攻擊風險，同時在需要時輕鬆訪問 VM。 已啟用 Just-In-Time 且使用者要求存取 VM 時，資訊安全中心會檢查使用者擁有此 VM 的哪些權限。 如果其擁有正確權限，則要求會通過核准，且資訊安全中心會將網路安全性群組 (NSG) 自動設定為在有限的時間內允許輸入流量進入選取的連接埠。 時間到期之後，資訊安全中心會將 NSG 還原為其先前的狀態。 

## <a name="encryption"></a>加密

為託管磁片提供了兩種加密方法。 作業系統級別的加密（即 Azure 磁片加密）和平臺級別的加密（即伺服器端加密）。

### <a name="server-side-encryption"></a>伺服器端加密

預設情況下，Azure 託管磁片在將資料保存到雲中時自動加密資料。 伺服器端加密可保護您的資料，並説明您履行組織安全性和合規性承諾。 Azure 託管磁片中的資料使用 256 位[AES 加密](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)進行透明加密，這是可用的最強的塊密碼之一，並且符合 FIPS 140-2 標準。

加密不會影響託管磁片的性能。 加密沒有額外費用。

您可以依賴平臺管理的金鑰來加密託管磁片，也可以使用自己的金鑰管理加密。 如果選擇使用自己的金鑰管理加密，則可以指定*客戶託管金鑰*，用於加密和解密託管磁片中的所有資料。 

要瞭解有關伺服器端加密的詳細資訊，請參閱[Windows](../articles/virtual-machines/windows/disk-encryption.md)或[Linux](../articles/virtual-machines/linux/disk-encryption.md)的文章。

### <a name="azure-disk-encryption"></a>Azure 磁碟加密

如需強化 [Windows VM](../articles/virtual-machines/windows/encrypt-disks.md) 和 [Linux VM](../articles/virtual-machines/linux/disk-encryption-overview.md) 安全性與合規性，可以將 Azure 中的虛擬磁碟加密。 Windows VM 上的虛擬磁碟在待用時是使用 BitLocker 進行加密。 Linux VM 上的待用虛擬磁碟會使用 dm-crypt 進行加密。 

將 Azure 中的虛擬磁碟加密完全免費。 密碼編譯金鑰會使用軟體保護功能儲存在 Azure 金鑰保存庫中，或者您可以在 FIPS 140-2 第 2 級標準認證的硬體安全性模組 (HSM) 中匯入或產生金鑰。 這些密碼編譯金鑰用來加密及解密連接到 VM 的虛擬磁碟。 您可保留這些密碼編譯金鑰的控制權，並可稽核其使用情況。 Azure Active Directory 服務主體會提供一個安全機制，以便在 VM 開機或關機時發出這些密碼編譯金鑰。

## <a name="key-vault-and-ssh-keys"></a>Key Vault 和 SSH 金鑰

祕密和憑證均可模型化為資源並由 [Key Vault](../articles/key-vault/key-vault-whatis.md) 提供。 您可以使用 Azure PowerShell 來建立適用於 [Windows VM](../articles/virtual-machines/windows/key-vault-setup.md) 的金鑰保存庫以及適用於 [Linux VM](../articles/virtual-machines/linux/key-vault-setup.md) 的 Azure CLI。 您也可以建立用於加密的金鑰。

金鑰保存庫存取原則可分別授與金鑰、祕密和憑證的權限。 例如，您可以只提供金鑰存取權給使用者，但不提供密碼的權限。 不過，金鑰、密碼或憑證的存取權限是在保存庫層級。 換句話說，[金鑰保存庫訪問策略](../articles/key-vault/key-vault-secure-your-key-vault.md)不支持對象級別許可權。

當您連線到 VM 時，您應該使用公開金鑰加密來提供更安全的登入方式。 此程序包括使用安全殼層 (SSH) 命令 (而非使用者名稱和密碼) 來驗證自己的公用和私密金鑰交換。 密碼很容易遭受暴力密碼破解攻擊，尤其是在網際網路對向 VM (例如 Web 伺服器) 上。 您可以利用安全殼層 (SSH) 金鑰組，在 Azure 上建立使用 SSH 金鑰來進行驗證的 [Linux VM](../articles/virtual-machines/linux/mac-create-ssh-keys.md)，進而免除登入密碼的需求。 您也可以使用 SSH 金鑰從 [Windows VM](../articles/virtual-machines/linux/ssh-from-windows.md) 連線至 Linux VM。

## <a name="managed-identities-for-azure-resources"></a>適用於 Azure 資源的受控識別

建置雲端應用程式常見的難題是如何管理程式碼中的認證，以向雲端服務進行驗證。 保護好這些認證是相當重要的工作。 在理想情況下，這些認證永不會出現在開發人員工作站且簽入原始程式碼控制。 Azure Key Vault 可安全地儲存認證、祕密和其他金鑰，但是您的程式碼必須向 Key Vault 進行驗證，才可擷取這些項目。 

Azure Active Directory (Azure AD) 中適用於 Azure 資源的受控識別功能可解決此問題。 這項功能會在 Azure AD 中將自動受控識別提供給 Azure 服務。 您可以使用此身分識別來完成任何支援 Azure AD 驗證的服務驗證 (包括 Key Vault)，不需要您程式碼中的任何認證。  您在 VM 上執行的程式碼可以向僅可從 VM 內存取的兩個端點要求權杖。 如需關於這項服務的詳細資訊，請檢閱 [Azure 資源的受控識別](../articles/active-directory/managed-identities-azure-resources/overview.md)概觀頁面。   

## <a name="policies"></a>原則

[Azure 原則](../articles/azure-policy/azure-policy-introduction.md)可用來為您組織的 [Windows VM](../articles/virtual-machines/windows/policy.md) 和 [Linux VM](../articles/virtual-machines/linux/policy.md) 定義所要的行為。 藉由使用原則，組織可以強制執行整個企業的各種慣例和規則。 強制執行所要的行為有助於降低風險，同時促進組織的成功。

## <a name="role-based-access-control"></a>角色型存取控制

使用[角色型存取控制 (RBAC)](../articles/role-based-access-control/overview.md)，可讓您區隔小組內的職責，而僅授予使用者在 VM 上執行作業所需的存取權。 您不需為每個人授與 VM 的權限，而是只允許執行特定的動作。 您可以使用 [Azure CLI](https://docs.microsoft.com/cli/azure/role) 或 [Azure PowerShell](../articles/role-based-access-control/role-assignments-powershell.md)，在 [Azure 入口網站](../articles/role-based-access-control/role-assignments-portal.md)中設定 VM 的存取控制。


## <a name="next-steps"></a>後續步驟
- 逐步執行步驟，以使用 Azure 資訊安全中心來監視 [Linux](../articles/security/fundamentals/overview.md) 或 [Windows](../articles/virtual-machines/windows/tutorial-azure-security.md) 虛擬機器安全性。
