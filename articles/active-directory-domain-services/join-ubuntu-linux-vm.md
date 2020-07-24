---
title: 將 Ubuntu VM 加入 Azure AD Domain Services |Microsoft Docs
description: 瞭解如何設定 Ubuntu Linux 虛擬機器並將其加入 Azure AD Domain Services 受控網域。
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 07/13/2020
ms.author: iainfou
ms.openlocfilehash: 7eaf8b6b5cddc8a01b59cda0cafc819e06a5ec7c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87004999"
---
# <a name="join-an-ubuntu-linux-virtual-machine-to-an-azure-active-directory-domain-services-managed-domain"></a>將 Ubuntu Linux 虛擬機器加入 Azure Active Directory Domain Services 受控網域

若要讓使用者能夠使用一組認證來登入 Azure 中的虛擬機器（Vm），您可以將 Vm 加入 Azure Active Directory Domain Services （Azure AD DS）受控網域。 當您將 VM 加入 Azure AD DS 受控網域時，可以使用網域中的使用者帳戶和認證來登入和管理伺服器。 也會套用來自受控網域的群組成員資格，讓您控制對 VM 上檔案或服務的存取。

本文說明如何將 Ubuntu Linux VM 加入受控網域。

## <a name="prerequisites"></a>必要條件

若要完成此教學課程，您需要下列資源和權限：

* 有效的 Azure 訂用帳戶。
    * 如果您沒有 Azure 訂用帳戶，請先[建立帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
* 與您的訂用帳戶相關聯的 Azure Active Directory 租用戶，可與內部部署目錄或僅限雲端的目錄同步。
    * 如果需要，請[建立 Azure Active Directory 租用戶][create-azure-ad-tenant]或[將 Azure 訂用帳戶與您的帳戶建立關聯][associate-azure-ad-tenant]。
* 已在您的 Azure AD 租用戶中啟用並設定 Azure Active Directory Domain Services 受控網域。
    * 如有需要，第一個教學課程會引導您[建立並設定 Azure Active Directory Domain Services 受控網域][create-azure-ad-ds-instance]。
* 屬於受控網域一部分的使用者帳戶。

## <a name="create-and-connect-to-an-ubuntu-linux-vm"></a>建立並連接至 Ubuntu Linux VM

如果您在 Azure 中有現有的 Ubuntu Linux VM，請使用 SSH 連接到它，然後繼續進行下一個步驟以[開始設定 VM](#configure-the-hosts-file)。

如果您需要建立 Ubuntu Linux VM，或想要建立要與本文搭配使用的測試 VM，您可以使用下列其中一種方法：

* [Azure 入口網站](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

當您建立 VM 時，請注意虛擬網路設定，以確保 VM 可以與受控網域進行通訊：

* 將 VM 部署到已啟用 Azure AD Domain Services 的相同或對等互連虛擬網路中。
* 將 VM 部署到與您的 Azure AD Domain Services 受控網域不同的子網。

部署 VM 之後，請遵循使用 SSH 連接到 VM 的步驟。

## <a name="configure-the-hosts-file"></a>設定 hosts 檔案

為確保受控網域已正確設定 VM 主機名稱，請編輯 */etc/hosts*檔案，並設定主機名稱：

```console
sudo vi /etc/hosts
```

在*hosts*檔案中，更新*localhost*位址。 在下例中︰

* *aaddscontoso.com*是受控網域的 DNS 功能變數名稱。
* *ubuntu*是您要加入受控網域之 ubuntu VM 的主機名稱。

使用您自己的值來更新這些名稱：

```console
127.0.0.1 ubuntu.aaddscontoso.com ubuntu
```

完成時，請使用編輯器的命令來儲存並結束*hosts*檔案 `:wq` 。

## <a name="install-required-packages"></a>安裝必要的套件

VM 需要一些額外的套件，才能將 VM 加入受控網域。 若要安裝及設定這些封裝，請使用來更新和安裝網域聯結工具`apt-get`

在 Kerberos 安裝期間， *krb5 使用者*套件會以全部大寫的方式來提示領域名稱。 例如，如果您的受控網功能變數名稱稱是*aaddscontoso.com*，請輸入*AADDSCONTOSO.COM*作為領域。 安裝會 `[realm]` `[domain_realm]` 在 */etc/krb5.conf*設定檔中寫入和區段。 請確定您將領域全部指定為大寫：

```console
sudo apt-get update
sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
```

## <a name="configure-network-time-protocol-ntp"></a>設定網路時間通訊協定（NTP）

若要讓網域通訊正確運作，您的 Ubuntu VM 的日期和時間必須與受控網域同步處理。 將受控網域的 NTP 主機名稱新增至 */etc/ntp.conf*檔案。

1. 使用編輯器開啟*ntp*檔案：

    ```console
    sudo vi /etc/ntp.conf
    ```

1. 在*ntp*檔案中，建立一行來新增受控網域的 DNS 名稱。 在下列範例中，會新增*aaddscontoso.com*的專案。 使用您自己的 DNS 名稱：

    ```console
    server aaddscontoso.com
    ```

    完成時，請使用編輯器的命令來儲存並結束*ntp*檔案。 `:wq`

1. 為確保 VM 與受控網域同步，需要執行下列步驟：

    * 停止 NTP 伺服器
    * 更新受控網域中的日期和時間
    * 啟動 NTP 服務

    執行下列命令來完成這些步驟。 搭配命令使用您自己的 DNS 名稱 `ntpdate` ：

    ```console
    sudo systemctl stop ntp
    sudo ntpdate aaddscontoso.com
    sudo systemctl start ntp
    ```

## <a name="join-vm-to-the-managed-domain"></a>將 VM 加入受控網域

現在已將必要的套件安裝在 VM 上，並已設定 NTP，請將 VM 加入受控網域。

1. 使用 `realm discover` 命令來探索受控網域。 下列範例會探索領域*AADDSCONTOSO.COM*。 以大寫指定您自己的受控功能變數名稱：

    ```console
    sudo realm discover AADDSCONTOSO.COM
    ```

   如果 `realm discover` 命令找不到您的受控網域，請參閱下列疑難排解步驟：

    * 請確定可從 VM 連線到該網域。 請嘗試 `ping aaddscontoso.com` 查看是否傳回正面回復。
    * 檢查 VM 是否部署到可使用受控網域的相同或對等互連虛擬網路。
    * 確認虛擬網路的 DNS 伺服器設定已更新，以指向受控網域的網域控制站。

1. 現在使用命令初始化 Kerberos `kinit` 。 指定屬於受控網域的使用者。 如有需要，請[將使用者帳戶新增至 Azure AD 中的群組](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md)。

    同樣地，必須以全部大寫輸入受控功能變數名稱。 在下列範例中，會使用名為的帳戶 `contosoadmin@aaddscontoso.com` 來初始化 Kerberos。 輸入屬於受控網域的使用者帳戶：

    ```console
    kinit contosoadmin@AADDSCONTOSO.COM
    ```

1. 最後，使用命令將 VM 加入受控網域 `realm join` 。 使用與您在上一個命令中指定的受控網域之一部分相同的使用者帳戶 `kinit` ，例如 `contosoadmin@AADDSCONTOSO.COM` ：

    ```console
    sudo realm join --verbose AADDSCONTOSO.COM -U 'contosoadmin@AADDSCONTOSO.COM' --install=/
    ```

將 VM 加入受控網域需要幾分鐘的時間。 下列範例輸出顯示 VM 已成功加入受控網域：

```output
Successfully enrolled machine in realm
```

如果您的 VM 無法順利完成網域加入程式，請確定 VM 的網路安全性群組允許 TCP + UDP 埠464上的輸出 Kerberos 流量連到受控網域的虛擬網路子網。

如果您收到錯誤*未指定的 GSS 失敗。 次要程式碼可能會提供詳細資訊（在 Kerberos 資料庫中找不到伺服器）*、開啟檔案 */etc/krb5.conf* ，並在區段中新增下列程式碼， `[libdefaults]` 然後再試一次：

```console
rdns=false
```

## <a name="update-the-sssd-configuration"></a>更新 SSSD 設定

上一個步驟中安裝的其中一個封裝是系統安全性服務 Daemon （SSSD）。 當使用者嘗試使用網域認證登入 VM 時，SSSD 會將要求轉送至驗證提供者。 在此案例中，SSSD 會使用 Azure AD DS 來驗證要求。

1. 使用編輯器開啟*sssd*檔案：

    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

1. 批註掉*use_fully_qualified_names*的程式程式碼，如下所示：

    ```console
    # use_fully_qualified_names = True
    ```

    完成時，請使用編輯器的命令來儲存並結束*sssd*檔案。 `:wq`

1. 若要套用變更，請重新開機 SSSD 服務：

    ```console
    sudo service sssd restart
    ```

## <a name="configure-user-account-and-group-settings"></a>設定使用者帳戶和群組設定

當 VM 加入受控網域並設定進行驗證時，有幾個使用者設定選項需要完成。 這些設定變更包括允許以密碼為基礎的驗證，以及在網域使用者第一次登入時自動建立本機 VM 上的主目錄。

### <a name="allow-password-authentication-for-ssh"></a>允許 SSH 的密碼驗證

根據預設，使用者只能使用 SSH 公用金鑰驗證來登入 VM。 以密碼為基礎的驗證失敗。 當您將 VM 加入受控網域時，這些網域帳戶必須使用密碼型驗證。 將 SSH 設定更新為允許以密碼為基礎的驗證，如下所示。

1. 使用編輯器開啟*sshd_conf*檔案：

    ```console
    sudo vi /etc/ssh/sshd_config
    ```

1. 將*passwordauthentication 開頭*的行更新為 *[是]*：

    ```console
    PasswordAuthentication yes
    ```

    完成時，請使用編輯器的命令儲存並結束*sshd_conf*檔案 `:wq` 。

1. 若要套用變更並讓使用者使用密碼登入，請重新開機 SSH 服務：

    ```console
    sudo systemctl restart ssh
    ```

### <a name="configure-automatic-home-directory-creation"></a>設定自動主目錄建立

若要在使用者第一次登入時啟用主目錄的自動建立，請完成下列步驟：

1. 在編輯器中開啟 */etc/pam.d/common-session*檔案：

    ```console
    sudo vi /etc/pam.d/common-session
    ```

1. 在此檔案中的行下方，新增下列程式 `session optional pam_sss.so` 程式碼：

    ```console
    session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
    ```

    完成時，請使用編輯器的命令來儲存和結束*一般會話*檔案 `:wq` 。

### <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>授與 'AAD DC Administrators' 群組 sudo 權限

若要授與*AAD DC Administrators*群組的成員 Ubuntu VM 上的系統管理許可權，您可以在 */etc/sudoers*中新增一個專案。 新增之後， *AAD DC 系統管理員*群組的成員就可以在 `sudo` Ubuntu VM 上使用命令。

1. 開啟*sudoers*檔案進行編輯：

    ```console
    sudo visudo
    ```

1. 將下列專案新增至 */etc/sudoers*檔案的結尾：

    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

    完成時，請使用命令儲存並結束編輯器 `Ctrl-X` 。

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>使用網域帳戶登入 VM

若要確認 VM 已成功加入受控網域，請使用網域使用者帳戶啟動新的 SSH 連線。 確認已建立主目錄，並已套用網域的群組成員資格。

1. 從您的主控台建立新的 SSH 連線。 使用屬於受控網域的網域帳戶（ `ssh -l` 例如）， `contosoadmin@aaddscontoso.com` 然後輸入您 VM 的位址，例如*ubuntu.aaddscontoso.com*。 如果您使用 Azure Cloud Shell，請使用 VM 的公用 IP 位址，而不是內部 DNS 名稱。

    ```console
    ssh -l contosoadmin@AADDSCONTOSO.com ubuntu.aaddscontoso.com
    ```

1. 當您成功連線到 VM 時，請確認已正確初始化主目錄：

    ```console
    pwd
    ```

    您應該會在 */home*目錄中，其中包含符合使用者帳戶的專屬目錄。

1. 現在檢查是否已正確解析群組成員資格：

    ```console
    id
    ```

    您應該會看到來自受控網域的群組成員資格。

1. 如果您已以*AAD DC 系統管理員*群組的成員身分登入 VM，請檢查您是否可以正確地使用 `sudo` 命令：

    ```console
    sudo apt-get update
    ```

## <a name="next-steps"></a>接下來的步驟

如果您在將 VM 連線到受控網域或使用網域帳戶登入時發生問題，請參閱針對[網域加入問題進行疑難排解](join-windows-vm.md#troubleshoot-domain-join-issues)。

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
