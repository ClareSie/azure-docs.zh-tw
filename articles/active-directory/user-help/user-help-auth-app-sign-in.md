---
title: 使用 Microsoft Authenticator 應用程式來登入 - Azure AD
description: 使用 Microsoft Authenticator 應用程式登入公司或學校帳戶，或使用雙因素驗證或手機登入，登入您個人的 Microsoft 和非 Microsoft 帳戶。
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 06/03/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: 4b2d79b24263a848d146bbbe8d42f23ec28f49b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84415877"
---
# <a name="sign-in-to-your-accounts-using-the-microsoft-authenticator-app"></a>使用 Microsoft Authenticator 應用程式登入帳戶

當您使用雙因素驗證時，Microsoft Authenticator 應用程式可協助您登入帳戶。 雙因素驗證有助於您以更安全的方式存取帳戶，尤其是在檢視機密資訊時。 您有可能忘記密碼，或密碼有可能遭竊或遭到破解，而雙因素驗證可作為額外的安全性步驟，協助您保護帳戶而使他人難以入侵。

您可以透過多種方式使用 Microsoft Authenticator 應用程式，包括：

- 在您以使用者名稱和密碼登入後，提供另一個驗證方法的提示。

- 使用您的使用者名稱和採用指紋、臉部或 PIN 的行動裝置提供登入，而無須輸入密碼。

  >[!Important]
  >此手機登入方法僅適用於您的公司或學校以及您的個人 Microsoft 帳戶。 您的帳戶和非 Microsoft 帳戶會要求您使用標準的雙因素驗證程序。

## <a name="prerequisites"></a>Prerequisites

使用 Microsoft Authenticator 應用程式之前，您必須：

 1. 下載並安裝 Microsoft Authenticator 應用程式。 如果您還沒有這麼做，請參閱[下載並安裝應用程式](user-help-auth-app-download-install.md)。

 2. 將您的公司/學校、個人和第三方帳戶新增至 Microsoft Authenticator 應用程式中。 如需詳細步驟，請參閱[新增公司或學校帳戶](user-help-auth-app-add-work-school-account.md)、[新增個人帳戶](user-help-auth-app-add-personal-ms-account.md)，以及[新增的非 Microsoft 帳戶](user-help-auth-app-add-non-ms-account.md)。

## <a name="turn-on-and-use-phone-sign-in-for-your-work-or-school-account"></a>為您的公司或學校帳戶開啟並使用手機登入

手機登入是一種雙步驟驗證類型。 您仍然必須提供您知道的一件事和您擁有的一個東西來確認您的身分識別，但手機登入可讓您略過輸入您的帳戶密碼，並在行動裝置上執行所有身分識別驗證。

開啟手機登入之前，您必須開啟雙因素驗證。 如需有關如何開啟帳戶的雙因素驗證的詳細資訊，請參閱[新增公司或學校帳戶](user-help-auth-app-add-work-school-account.md)和[新增個人帳戶](user-help-auth-app-add-personal-ms-account.md)。

手機登入僅適用於 iOS 以及執行 Android 6.0 或更新版本的 Android 裝置。

### <a name="turn-on-phone-sign-in"></a>開啟手機登入

開啟 Microsoft Authenticator 應用程式、移至您的公司或學校帳戶，然後開啟手機登入。

- **當您按一下 [帳戶] 圖格時**，您會看到該帳戶的全螢幕畫面。 如果您看到**已啟用手機登入**，則表示您已完全設定為不用密碼來登入。 如果您看到 [啟用手機登入]，請加以點選來開啟手機登入。
- **如果您已使用應用程式進行雙因素驗證**，您可以按一下 [帳戶] 磚，以查看帳戶的全螢幕視圖。 然後，點選 [啟用手機登入] 以開啟手機登入。
- 如果在應用程式的 [帳戶] 畫面上**找不到您的公司或學校帳戶**，表示您還沒有將帳戶新增到應用程式中。 遵循[新增您的工作或學校帳戶說明](user-help-auth-app-add-work-school-account.md)中的步驟來新增您的工作或學校帳戶。

開啟手機登入之後，您只能使用 Microsoft Authenticator 應用程式進行登入。 其做法如下：

1. 登入您的公司或學校帳戶。

    輸入您的使用者名稱之後，[核准登入] 畫面隨即出現，其中顯示一個兩位數的號碼，並要求您透過 Microsoft Authenticator 應用程式登入。 如果您不想使用這個登入方法，您可以選取 [改為使用您的密碼]，然後使用您的密碼登入。

    ![電腦上的核准登入方塊](media/user-help-auth-app-sign-in/microsoft-auth-app-sign-in.png)

2. 在裝置上開啟通知或 Microsoft Authenticator 應用程式，然後點選與您在電腦的 [核准登入] 畫面上看到之數字相符的數字。

    ![裝置上的核准登入方塊](media/user-help-auth-app-sign-in/microsoft-auth-app-sign-in-numbers.png)

3. 如果您可以辨識該登入嘗試，請選擇 [核准]。 否則，請選擇 [拒絕]。

4. 使用您手機的 PIN 或生物識別金鑰來完成驗證。

## <a name="turn-on-and-use-phone-sign-in-for-your-personal-microsoft-accounts"></a>為您的個人 Microsoft 帳戶開啟並使用手機登入

您可以針對個人的 Microsoft 帳戶開啟手機登入，例如，您用來登入 Outlook.com、Xbox 或 Skype 的帳戶。

>[!NOTE]
>若要協助保護您的帳戶，Microsoft Authenticator 應用程式在裝置上需要有 PIN 或生物識別鎖定。 如果您讓手機保持解除鎖定狀態，應用程式就會要求您先設定安全鎖定之後，才能開啟手機登入。

### <a name="turn-on-phone-sign-in"></a>開啟手機登入 

開啟 Microsoft Authenticator 應用程式、移至您的公司或學校帳戶，然後開啟手機登入。

- **當您按一下 [帳戶] 圖格時**，您會看到帳戶的全螢幕視圖。 如果您看到**已啟用手機登入**，則表示您已完全設定為不用密碼來登入。 如果您看到 [啟用手機登入]，請加以點選來開啟手機登入。
- **如果您已經使用應用程式進行雙因素驗證**，您可以按一下 [帳戶] 磚，以查看帳戶的全螢幕視圖。 然後，點選 [啟用手機登入] 以開啟手機登入。
- 如果在應用程式的 [帳戶] 畫面上**找不到您的帳戶**，表示您還沒有將帳戶新增到應用程式中。 遵循[新增個人 Microsoft 帳戶](user-help-auth-app-add-personal-ms-account.md)一文中的步驟來新增您個人的 Microsoft 帳戶。

### <a name="sign-in-to-your-account-using-phone-sign-in"></a>使用手機登入來登入您的帳戶

1. 移至您個人 Microsoft 帳戶登入頁面，然後選取 [改為使用 Microsoft Authenticator 應用程式] 連結，而不是輸入您的密碼。

    Microsoft 會傳送通知至您的電話。

2. 核准通知。

## <a name="sign-in-using-two-factor-verification-for-your-account"></a>為您的帳戶使用雙因素驗證登入

標準的雙因素驗證方法會要求您在您登入的裝置中輸入您的使用者名稱和密碼，然後選擇 Microsoft Authenticator 應用程式是否收到通知，或是否要從驗證器應用程式複製驗證碼。 在 Android 裝置上，您可以在 [**帳戶**] 畫面上找到驗證碼。 在 iOS 裝置上，這些驗證碼可以在帳戶的 [**帳戶**] 畫面或 [全螢幕] 視圖中找到，視帳戶類型而定。 當您將帳戶新增至 Microsoft Authenticator 應用程式時，您會開啟帳戶的雙因素驗證。

>[!Note]
>如果您在 Microsoft Authenticator 應用程式的 [帳戶] 畫面上沒有看到您的公司或學校帳戶，或您個人的帳戶，表示您還沒有將該帳戶新增到 Microsoft Authenticator 應用程式。 若要新增您的帳戶，請參閱[新增公司或學校帳戶](user-help-auth-app-add-work-school-account.md)或[新增個人帳戶](user-help-auth-app-add-personal-ms-account.md)。

如需了解使用雙因素驗證的各種方法登入公司或學校或個人帳戶所需的步驟，請參閱[使用雙步驟驗證或安全性資訊登入](user-help-sign-in.md)。

## <a name="frequently-asked-questions"></a>常見問題集

| 問題 | 解決方法 |
| -------- | -------- |
| 使用我的電話登入會如何比輸入密碼更安全？ | 現今大部分的人會使用使用者名稱和密碼登入網站或應用程式。 不幸的是，密碼可能會遺失、遭竊或被駭客猜到。<br><br>設定 Microsoft Authenticator 應用程式之後，它會在您的手機上設定一個金鑰，來將您受到手機 PIN 或生物識別鎖定所保護的帳戶解除鎖定。 這個金鑰接著會在登入時用來證明您的身分識別。<br><br>**重要事項**<br>您的資料只會用來在本機保護您的金鑰。 絕對不會將它傳送到雲端或儲存在雲端。 |
| 手機登入是否會取代雙步驟驗證？ 應該將它關機嗎？ | 手機登入是一種雙步驟驗證，其中兩個步驟會行動裝置上同時進行。 您應該將雙步驟驗證保持在開啟狀態，以協助為您的帳戶提供額外的安全性。 |
| 如果我為帳戶持續開啟了雙步驟驗證，是否必須核准兩個通知呢？ | 否。 使用手機登入您的 Microsoft 帳戶，也會被視為雙步驟驗證，因此不需要第二個核准。 |
| 如果我遺失手機或者沒帶手機，該怎麼辦？ 如何存取我的帳戶？ | 您隨時都可按一下登入頁面上的 [改為使用密碼] 連結，切換回使用您的密碼。 不過，如果您使用雙步驟驗證，您仍需要使用第二個方法來驗證您的身分識別。<br><br>**重要**<br>強烈建議您確定有多個與您帳戶相關聯的最新驗證方法。<br><br>您可以從[安全性設定](https://account.live.com/proofs/manage)頁面來管理個人帳戶的驗證方法。 針對公司或學校帳戶，如果您的系統管理員已開啟安全性資訊，您就可以移至組織的[其他安全性驗證](https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1)頁面或**保護您的帳戶安全**頁面。 如需關於安全性資訊的詳細資訊，請參閱[安全性資訊 (預覽) 概觀](user-help-security-info-overview.md)。<br><br>如果您無法管理驗證方法，就必須連絡您的系統管理員。 |
| 如何停止使用此功能並改回使用我的密碼？ | 對於個人帳戶，在登入期間選取 [改為使用密碼] 連結。 系統會記住您最近的選擇，並在您下次登入時依預設提供該選擇。 如果您想要改回使用手機登入，請在登入期間選取 [改為使用應用程式] 連結。<br><br>針對公司或學校帳戶，您必須從 Microsoft Authenticator 應用程式的 [設定] 頁面將裝置取消註冊，或從設定檔的 [裝置和活動] 區域停用裝置。 如需如何從設定檔停用裝置的詳細資訊，請參閱[從我的應用程式入口網站更新設定檔和帳戶資訊](my-apps-portal-end-user-update-profile.md#view-your-organization-related-profile-information)。 |
| 為什麼不能使用多個公司或學校帳戶進行手機登入？ | 一支手機必須向單一公司或學校帳戶進行註冊。 如果您想要針對不同的公司或學校帳戶開啟手機登入，就必須透過 [設定] 頁面將您在此裝置中的帳戶取消註冊。 |
| 可以使用手機來登入我的電腦嗎？ | 針對您的電腦，我們建議使用 Windows 10 上的 Windows Hello 來登入。 Windows Hello 可讓您使用臉部、指紋或 PIN 來登入。 |

## <a name="next-steps"></a>後續步驟

- 如果您在取得個人 Microsoft 帳戶的驗證碼時遇到問題，請參閱 [Microsoft 帳戶安全性資訊及驗證碼](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes)一文中的＜疑難排解驗證碼問題＞一節。

- 如果您有更多關於應用程式的一般問題，請參閱 [Microsoft Authenticator 常見問題集](user-help-auth-app-faq.md)

- 如果您需要雙步驟驗證的詳細資訊，請參閱[對我的帳戶進行雙步驟驗證設定](multi-factor-authentication-end-user-first-time.md)

- 如果您想要安全性資訊的詳細資訊，請參閱[安全性資訊 (預覽) 概觀](user-help-security-info-overview.md)
