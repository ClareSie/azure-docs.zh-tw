---
title: 設定 keychain
titleSuffix: Microsoft identity platform
description: 瞭解如何設定 keychain，讓您的應用程式可以在 keychain 中快取權杖。
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 06e197a6e445c7dc1179be696318905f2132ee36
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85477731"
---
# <a name="configure-keychain"></a>設定 keychain

當 [適用于 iOS 和 macOS 的 Microsoft 驗證程式庫](msal-overview.md) (MSAL) 登入使用者，或重新整理權杖時，它會嘗試在 keychain 中快取權杖。 在 keychain 中快取權杖可讓 MSAL 在多個由相同 Apple 開發人員散發的應用程式之間，提供無訊息的單一登入 (SSO) 。 SSO 可透過 keychain 存取群組功能來達成。 如需詳細資訊，請參閱 Apple 的 [Keychain 專案檔案](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc)。

本文涵蓋如何設定應用程式權利，讓 MSAL 可以將快取權杖寫入至 iOS 和 macOS keychain。

## <a name="default-keychain-access-group"></a>預設 keychain 存取群組

### <a name="ios"></a>iOS

IOS 上的 MSAL 預設會使用 `com.microsoft.adalcache` 存取群組。 這是 MSAL 和 Azure AD 驗證程式庫 (ADAL) Sdk 所使用的共用存取群組，可確保在相同發行者的多個應用程式之間獲得最佳的單一登入 (SSO) 體驗。

在 iOS 上，將 `com.microsoft.adalcache` keychain 群組新增至應用程式在 XCode 的**專案設定**  >  **功能**  >  **keychain 共用**下的許可權

### <a name="macos"></a>macOS

MacOS 上的 MSAL `com.microsoft.identity.universalstorage` 預設會使用存取群組。

由於 macOS keychain 限制的緣故，MSAL 的並 `access group` 不會直接轉譯為 keychain access group 屬性 (請參閱 kSecAttrAccessGroup 10.14 及更早版本的 [macOS](https://developer.apple.com/documentation/security/ksecattraccessgroup?language=objc)) 。 不過，它的行為類似于 SSO 的觀點，因為藉由確保相同 Apple 開發人員所散發的多個應用程式可以具有無訊息 SSO。

在 macOS 10.15 (macOS Catalina) 之後，MSAL 會使用 keychain access group 屬性來達成無訊息 SSO，類似于 iOS。

## <a name="custom-keychain-access-group"></a>自訂 keychain 存取群組

如果您想要使用不同的 keychain 存取群組，在建立之前，您可以傳遞自訂群組 `MSALPublicClientApplicationConfig` `MSALPublicClientApplication` ，如下所示：

# <a name="objective-c"></a>[Objective-C](#tab/objc)

```objc
MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"your-client-id"
                                                                                            redirectUri:@"your-redirect-uri"
                                                                                              authority:nil];
    
config.cacheConfig.keychainSharingGroup = @"custom-group";
    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:nil];
    
// Now call acquiretoken. 
// Tokens will be saved into the "custom-group" access group
// and only shared with other applications declaring the same access group
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
let config = MSALPublicClientApplicationConfig(clientId: "your-client-id",
                                            redirectUri: "your-redirect-uri",
                                              authority: nil)
config.cacheConfig.keychainSharingGroup = "custom-group"
        
do {
  let application = try MSALPublicClientApplication(configuration: config)
  // continue on with application          
} catch let error as NSError {
  // handle error here
}       
```

---

## <a name="disable-keychain-sharing"></a>停用 keychain 共用

如果您不想要在多個應用程式之間共用 SSO 狀態，或使用任何 keychain 存取群組，請將應用程式套件組合識別碼作為 keychainGroup 來停用 keychain 共用：

# <a name="objective-c"></a>[Objective-C](#tab/objc)

```objc
config.cacheConfig.keychainSharingGroup = [[NSBundle mainBundle] bundleIdentifier];
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
if let bundleIdentifier = Bundle.main.bundleIdentifier {
    config.cacheConfig.keychainSharingGroup = bundleIdentifier
}
```

---

## <a name="handle--34018-error-failed-to-set-item-into-keychain"></a>處理-34018 錯誤 (無法將專案設定為 keychain) 

錯誤-34018 通常表示 keychain 未正確設定。 確定已在 MSAL 中設定的 keychain 存取群組符合 [權利] 中設定的群組。

## <a name="ensure-your-application-is-properly-signed"></a>確定您的應用程式已正確簽署

在 macOS 上，應用程式可以在不需要由開發人員簽署的情況下執行。 雖然 MSAL 的大部分功能都將繼續運作，但透過 keychain 存取的 SSO 需要簽署應用程式。 如果您遇到多個 keychain 提示，請確定您應用程式的簽章有效。

## <a name="next-steps"></a>接下來的步驟

深入瞭解 Apple 的 [共用存取權，以在應用程式集合中 Keychain 專案](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) 的 keychain 存取群組。
