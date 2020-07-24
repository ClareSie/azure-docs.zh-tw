---
title: 如何產生 & 傳輸 HSM 保護的金鑰-BYOK – Azure Key Vault
description: 使用本文來協助您規劃、產生及傳輸您自己的受 HSM 保護的金鑰，以與 Azure Key Vault 搭配使用。 也稱為「整合您自己的金鑰」（BYOK）。
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: conceptual
ms.date: 05/29/2020
ms.author: ambapat
ms.openlocfilehash: 4df934f38a8fb657fa4a8de5922d96197a3d02cc
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87061107"
---
# <a name="import-hsm-protected-keys-to-key-vault-byok"></a>將受 HSM 保護的金鑰匯入 Key Vault （BYOK）

當您使用 Azure Key Vault 時，您可以在硬體安全模組（HSM）中匯入或產生金鑰，以提高保證;金鑰永遠不會離開 HSM 界限。 此案例通常稱為「*整合您自己的金鑰*」（BYOK）。 Key Vault 使用 Hsm 的 nCipher nShield 系列（已驗證 FIPS 140-2 Level 2）來保護您的金鑰。

使用本文中的資訊可協助您規劃、產生及傳輸您自己的受 HSM 保護的金鑰，以與 Azure Key Vault 搭配使用。

> [!NOTE]
> 此功能不適用於 Azure 中國世紀。 
> 
> 此匯入方法僅適用于[支援的 hsm](#supported-hsms)。 

如需詳細資訊，以及開始使用 Key Vault 的教學課程（包括如何為受 HSM 保護的金鑰建立金鑰保存庫），請參閱[什麼是 Azure Key Vault？](../general/overview.md)。

## <a name="overview"></a>概觀

以下是程序概觀。 本文稍後會說明要完成的特定步驟。

* 在 Key Vault 中，產生金鑰（稱為*金鑰交換金鑰*（KEK））。 KEK 必須是只有金鑰操作的 RSA HSM 金鑰 `import` 。 只有 Key Vault Premium SKU 支援 RSA-HSM 金鑰。
* 下載 KEK 公開金鑰做為 pem 檔案。
* 將 KEK 公開金鑰傳送至連線到內部部署 HSM 的離線電腦。
* 在離線電腦中，使用 HSM 廠商提供的 BYOK 工具來建立 BYOK 檔案。 
* 目標金鑰會以 KEK 加密，在傳輸到 Key Vault HSM 之前會保持加密狀態。 只有金鑰的加密版本會離開內部部署 HSM。
* 在 Key Vault HSM 內產生的 KEK 無法匯出。 Hsm 會強制執行規則，指出 Key Vault HSM 以外的 KEK 沒有任何明確的版本存在。
* KEK 必須位於將匯入目標金鑰的相同金鑰保存庫中。
* 將 BYOK 檔案上傳至 Key Vault 時，Key Vault HSM 會使用 KEK 的私密金鑰來解密目標金鑰內容，並將它匯入為 HSM 金鑰。 這種作業完全在 Key Vault HSM 內執行。 目標金鑰一律會保留在 HSM 保護界限內。

## <a name="prerequisites"></a>先決條件

下表列出在 Azure Key Vault 中使用 BYOK 的必要條件：

| 需求 | 詳細資訊 |
| --- | --- |
| Azure 訂用帳戶 |若要在 Azure Key Vault 中建立金鑰保存庫，您需要 Azure 訂用帳戶。 [註冊免費試用版](https://azure.microsoft.com/pricing/free-trial/)。 |
| Key Vault Premium SKU，用以匯入受 HSM 保護的金鑰 |如需 Azure Key Vault 中服務層級和功能的詳細資訊，請參閱[Key Vault 定價](https://azure.microsoft.com/pricing/details/key-vault/)。 |
| 支援的 Hsm 清單中的 HSM，以及 HSM 廠商提供的 BYOK 工具和指示 | 您必須具備 HSM 的許可權，以及如何使用 HSM 的基本知識。 請參閱[支援的 hsm](#supported-hsms)。 |
| Azure CLI 2.1.0 或更新版本 | 請參閱[安裝 Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)。|

## <a name="supported-hsms"></a>支援的 Hsm

|廠商名稱|廠商類型|支援的 HSM 模型|詳細資訊|
|---|---|---|---|
|nCipher|負責<br/>HSM 即服務|<ul><li>Hsm 的 nShield 系列</li><li>nShield 即服務</ul>|[nCipher 新的 BYOK 工具和檔](https://www.ncipher.com/products/key-management/cloud-microsoft-azure)|
|Thales|製造商|<ul><li>Luna HSM 7 系列（含固件版本7.3 或更新版本）</li></ul>| [Luna BYOK 工具和檔](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|
|Fortanix|負責<br/>HSM 即服務|<ul><li>自我保護金鑰管理服務（SDKMS）</li><li>Equinix SmartKey</li></ul>|[將 SDKMS 金鑰匯出至 BYOK 的雲端提供者-Azure Key Vault](https://support.fortanix.com/hc/en-us/articles/360040071192-Exporting-SDKMS-keys-to-Cloud-Providers-for-BYOK-Azure-Key-Vault)|
|Marvell|製造商|所有具有的 LiquidSecurity Hsm<ul><li>2.0.4 版或更新版本的固件</li><li>固件3.2 版或更新版本</li></ul>|[Marvell BYOK 工具和檔](https://www.marvell.com/products/security-solutions/nitrox-hs-adapters/exporting-marvell-hsm-keys-to-cloud-azure-key-vault.html)|
|Cryptomathic|ISV （企業金鑰管理系統）|多個 HSM 品牌和型號，包括<ul><li>nCipher</li><li>Thales</li><li>Utimaco</li></ul>[如需詳細資訊，請參閱 Cryptomathic 網站](https://www.cryptomathic.com/azurebyok)|[Cryptomathic BYOK tool 和檔](https://www.cryptomathic.com/azurebyok)|



## <a name="supported-key-types"></a>支援的金鑰類型

|機碼名稱|金鑰類型|金鑰大小|來源|描述|
|---|---|---|---|---|
|金鑰交換金鑰（KEK）|RSA| 2048位<br />3072位<br />4096位|Azure Key Vault HSM|在 Azure Key Vault 中產生的 HSM 支援的 RSA 金鑰組|
|目標金鑰|RSA|2048位<br />3072位<br />4096位|廠商 HSM|要傳輸至 Azure Key Vault HSM 的金鑰|

## <a name="generate-and-transfer-your-key-to-the-key-vault-hsm"></a>產生金鑰並將其傳輸至 Key Vault HSM

若要產生金鑰並將其傳輸至 Key Vault HSM：

* [步驟1：產生 KEK](#step-1-generate-a-kek)
* [步驟2：下載 KEK 公開金鑰](#step-2-download-the-kek-public-key)
* [步驟3：產生並準備您的金鑰以進行傳輸](#step-3-generate-and-prepare-your-key-for-transfer)
* [步驟4：將您的金鑰傳輸至 Azure Key Vault](#step-4-transfer-your-key-to-azure-key-vault)

### <a name="step-1-generate-a-kek"></a>步驟1：產生 KEK

KEK 是在 Key Vault HSM 中產生的 RSA 金鑰。 KEK 是用來加密您要匯入的金鑰（*目標*金鑰）。

KEK 必須是：
- RSA-HSM 金鑰（2048位; 3072 位或4096位）
- 在您要匯入目標金鑰的相同金鑰保存庫中產生
- 建立時允許的金鑰作業設定為`import`

> [!NOTE]
> KEK 必須有「匯入」做為唯一允許的金鑰作業。 「匯入」與所有其他金鑰作業互斥。

使用[az keyvault key create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create)命令來建立將金鑰作業設定為的 KEK `import` 。 記錄 `kid` 從下列命令傳回的金鑰識別碼（）。 （您將使用 `kid` [步驟 3](#step-3-generate-and-prepare-your-key-for-transfer)中的值）。

```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

### <a name="step-2-download-the-kek-public-key"></a>步驟2：下載 KEK 公開金鑰

使用[az keyvault key 下載](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download)將 KEK 公開金鑰下載至 pem 檔案。 您匯入的目標金鑰會使用 KEK 公開金鑰進行加密。

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

將 KEKforBYOK. publickey. pem 檔案傳送到您的離線電腦。 在下一個步驟中，您將需要此檔案。

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>步驟3：產生並準備您的金鑰以進行傳輸

請參閱 HSM 廠商的檔，以下載並安裝 BYOK 工具。 遵循 HSM 廠商提供的指示來產生目標金鑰，然後建立金鑰傳輸套件（BYOK 檔案）。 BYOK 工具會使用步驟1中所 `kid` 下載的 KEKforBYOK. publickey. pem 檔案，在 BYOK 檔案[Step 2](#step-2-download-the-kek-public-key)中產生加密的目標金鑰。 [Step 1](#step-1-generate-a-kek)

將 BYOK 檔案傳送到已連線的電腦。

> [!NOTE] 
> 不支援匯入 RSA 1024 位金鑰。 目前不支援匯入橢圓曲線（EC）按鍵。
> 
> **已知問題**：只有固件 v7.4.0 或更新版本才支援從 Luna hsm 匯入 RSA 4k 目標金鑰。

### <a name="step-4-transfer-your-key-to-azure-key-vault"></a>步驟4：將您的金鑰傳輸至 Azure Key Vault

若要完成金鑰匯入，請將金鑰傳輸套件（BYOK 檔案）從已中斷連線的電腦傳輸到連接網際網路的電腦。 使用[az keyvault key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import)命令將 BYOK 檔案上傳至 Key Vault HSM。

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

如果上傳成功，Azure CLI 會顯示匯入之金鑰的屬性。

## <a name="next-steps"></a>接下來的步驟

您現在可以在您的金鑰保存庫中使用這個受 HSM 保護的金鑰。 如需詳細資訊，請參閱[此價格與功能比較](https://azure.microsoft.com/pricing/details/key-vault/)。



