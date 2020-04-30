---
title: 如何在 Microsoft 合作夥伴中心管理商業 marketplace 帳戶
description: 瞭解如何在 Microsoft 合作夥伴中心管理商業 marketplace 帳戶。
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 54b45825f2322dc127d5db818b7abe52e4a98967
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82208392"
---
# <a name="manage-your-commercial-marketplace-account-in-partner-center"></a>在合作夥伴中心管理您的商業 marketplace 帳戶

[建立合作夥伴中心帳戶](./create-account.md)之後，您可以使用 [[商業 marketplace] 儀表板](https://partner.microsoft.com/dashboard/commercial-marketplace/overview)來管理您的帳戶和供應專案。

在本文中，我們將深入探討如何管理您的合作夥伴中心帳戶，包括如何：

- [存取您的合作夥伴中心帳戶設定](#access-your-account-settings)
- [尋找您的發行者識別碼、Symantec 識別碼、賣方識別碼、使用者識別碼、MPN 識別碼和 Azure AD 租使用者](#account-details)
- [更新連絡人資訊](#contact-info)
- [設定用於監視客戶使用量的追蹤 Guid](#tracking-guids)
- [管理使用者](#manage-users)
- [管理群組](#manage-groups)
- [管理 Azure AD 應用程式](#manage-azure-ad-applications)
- [定義使用者角色和權限](#define-user-roles-and-permissions)
- [管理 Azure AD 租使用者（工作帳戶）](#manage-tenants)
- [管理合作夥伴中心協定](#agreements)

## <a name="access-your-account-settings"></a>存取您的帳戶設定

如果您尚未這麼做，則您（或您組織的系統管理員）應存取合作夥伴中心帳戶的[帳戶設定](https://partner.microsoft.com/dashboard/account/management)，以進行下列動作：
- 檢查公司的帳戶驗證狀態
- 確認您的 Symantec 識別碼、賣方識別碼、Microsoft 合作夥伴網路（MPN）識別碼、發行者識別碼和連絡人資訊，包括公司核准者和賣方連絡人
- 為將在合作夥伴中心使用您商務帳戶的任何人建立使用者帳戶

### <a name="open-developer-settings"></a>開啟開發人員設定

**帳戶設定**位於 [合作夥伴中心] 的 [[商業 marketplace] 儀表板](https://partner.microsoft.com/dashboard/commercial-marketplace)右上角。 選取齒輪圖示（靠近儀表板右上角），然後選取 [ **開發人員設定**]。

![合作夥伴中心的帳戶設定功能表](./media/dashboard-developer-settings.png)

在 [**帳戶設定**] 中，您將能夠看到您的：
- **帳戶詳細資料**：帳戶類型和帳戶狀態
- **發行者**識別碼：賣方識別碼、使用者識別碼、發行者識別碼、Azure AD 租使用者等等。
- **連絡人資訊**：發行者顯示名稱、賣方連絡人名稱、電子郵件、電話和位址
- **追蹤 guid**：與您的帳戶相關聯的任何追蹤 guid

### <a name="account-details"></a>帳戶詳細資料

在 [帳戶詳細資料] 區段中，您可以看到基本資訊，例如您的**帳戶類型**（公司或個人）和帳戶的**驗證狀態**。 在您的帳戶驗證過程中，這些設定會顯示所需的每個步驟，包括電子郵件驗證、雇用驗證和商務驗證。 您也可以在這裡更新您的電子郵件，並視需要重新傳送驗證。

### <a name="publisher-ids"></a>發行者識別碼

在 [發行者識別碼] 區段中，您可以看到您的**SYMANTEC 識別碼**、**賣方識別碼**、**使用者識別碼**、 **MPN 識別碼**和**Azure AD**租使用者。 Microsoft 會指派這些值來唯一識別您的開發人員帳戶，而且無法編輯。

### <a name="contact-info"></a>連絡資訊

在 [連絡人資訊] 區段中，您可以看到您的**發行者顯示名稱**、**賣方連絡人資訊**（公司賣方的連絡人名稱、電子郵件、電話號碼和位址），以及**公司核准者**（擁有核准公司決策之人員的姓名、電子郵件和電話號碼）。

#### <a name="payout-account"></a>支付帳戶

付款帳戶是從您的銷售送往的銀行帳戶。 此銀行帳戶必須與您註冊合作夥伴中心帳戶的國家/地區相同。

若要設定您的支出帳戶：

1. 前往合作夥伴中心的[商業 marketplace 總覽頁面](https://partner.microsoft.com/dashboard/commercial-marketplace/overview)。
2. 在 [設定檔] 區段下的 [付款**設定檔**] 旁，選取 [**更新**]。
3. **選擇付款方法： [** 銀行帳戶] 或 [PayPal]。
4. **新增付款資訊**：這可能包括選擇帳戶類型（支票或儲蓄）、輸入帳戶持有人名稱、帳戶號碼，以及路由號碼、帳單位址、電話號碼或 PayPal 電子郵件地址。 * 如需使用 PayPal 作為帳戶付款條件，並瞭解您的市場區域是否支援該方法的詳細資訊，請參閱[PayPal 資訊](https://docs.microsoft.com/windows/uwp/publish/setting-up-your-payout-account-and-tax-forms#paypal-info)。

> [!IMPORTANT]
> 變更您的付款帳戶會延遲您的付款最多一次付款週期。 發生此延遲的原因是因為我們需要驗證帳戶變更，就像我們在第一次設定付款帳戶時所做的一樣。 您仍然會在帳戶確認之後收到全額款項。當期付款週期未付的款項會新增至下一期。  

#### <a name="tax-profile"></a>稅金設定檔

檢查您目前的稅務設定檔狀態，並確認是否顯示正確的**實體類型**和**稅務憑證資訊**。 選取 [**編輯**] 以更新或完成任何必要的表單。

若要建立您的稅務狀態，您必須指定您的居住和公民國家/地區，並填寫與您的國家/地區相關聯的適當稅務表單。

無論您居住的國家/地區或公民，都必須填寫美國稅務形式，以透過 Microsoft 銷售任何供應專案。 滿足特定美國常駐需求的合作夥伴，必須填寫一個 IRS 9 的表單。 美國以外的其他合作夥伴必須填寫 IRS 的8個表單。 完成稅金設定檔之後，您就可以在線上填寫這些表單。

向 Microsoft 收到付款或要求稅務協定優惠時不需要美國個人納稅識別號碼 (或 ITIN)。

您可以在合作夥伴中心以電子方式完成並提交您的稅務表單;在大部分的情況下，您不需要列印和郵寄任何表單。

不同國家和地區的稅金要求不同。 您必須支付的確切金額取決於銷售供應專案的國家和地區。 Microsoft 免除銷售，並代表您在某些國家/地區使用稅金。 這些國家/地區將在列出您供應專案的過程中識別。 在其他國家/地區，視您註冊的位置而定，您可能需要匯款銷售，並將您的銷售直接使用於當地的稅務機關。 此外，您的銷售繼續進行時，可能會以收入為准。 我們強烈建議您洽詢您所在國家或地區的相關授權單位，其最能協助您判斷 Microsoft 銷售交易的正確稅務資訊。

##### <a name="withholding-rates"></a>扣繳率
您在納稅申報表中送出的資訊將決定適當的稅金扣繳率。 扣繳率只適用於您在美國完成的銷售；非美國地區的銷售不適用扣繳率。 扣繳率視情況而有不同，但對於在美國以外地區註冊的大多數開發人員而言，預設扣繳率為 30%。 如果您的國家/地區和美國已簽訂所得稅協定，您可以選擇降低此扣繳率。

##### <a name="tax-treaty-benefits"></a>稅務協定優惠
如果您在美國以外的地區，可能可以享有稅務協定優惠。 這些權益會因國家/地區而異，並可讓您減少 Microsoft withholds 的稅金。 您可以完成 W-8BEN 表單的 Part II 來要求稅務協定優惠。 建議您與您國家或地區的適當資源聯繫，以確認您是否適用這些優惠。

[深入瞭解 Windows 應用程式/遊戲開發人員和 Azure Marketplace 發行者的稅務詳細資料](https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps)。

#### <a name="payout-hold-status"></a>支付保留狀態

根據預設，Microsoft 會每月傳送付款。 不過，您可以選擇讓您的支出保持在最上面，這會防止傳送付款給您的帳戶。 如果您選擇保留支出，我們會繼續記錄您所獲得的任何收益，並在您的**支出摘要**中提供詳細資料。 不過，除非您移除保存，否則我們不會傳送任何付款給您的帳戶。 

若要保留付款，請移至 [**帳戶設定**]。 在 [**財務詳細資料**] 的 [**支付保留狀態**] 區段中，將滑桿切換到 [**開啟**]。 您隨時都可以變更您的支付保留狀態，但是請注意您決定將會影響後續的每月支付。 例如，如果您想要保留4月的支出，請務必將您的付款保留狀態設定為**在**三月份結束之前。

將支付保留狀態設為 [**開啟**] 之後，除非將滑桿切換回 [**關閉**]，否則會保留所有支付。 當您這麼做時，您將會在下一個月付款週期中包含（前提是已符合任何適用的付款閾值）。 例如，如果您已支出，但想要在六月產生支出，則請務必在5月底之前，將支出保留狀態切換為**關閉**。

> [!NOTE]
> 您的付款**保留狀態**選擇適用于透過 Microsoft 合作夥伴中心付費的**所有**收益來源，包括 Azure Marketplace、AppSource、Microsoft Store、廣告等等）。 您無法針對個別收入來源選取不同保留狀態。

### <a name="devices"></a>裝置

裝置管理設定只適用于 UWP 發行。 [深入了解](https://docs.microsoft.com/windows/uwp/publish/manage-account-settings-and-profile#additional-settings-and-info)。

### <a name="tracking-guids"></a>追蹤 Guid

全域唯一識別碼（Guid）是唯一的參考編號（包含32個十六進位數位），可用來追蹤您的 Azure 使用量。 

若要建立 GUID 來進行追蹤，您應該使用 GUID 產生器。 Azure 儲存體小組已建立 [GUID 產生器表單](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR3i8TQB_XnRAsV3-7XmQFpFUMVRVVFFLTDFLS0E2QzNYSkFZR1U3WVJCTSQlQCN0PWcu)，其會透過電子郵件傳送正確格式的 GUID 給您，且可跨不同追蹤系統重複使用。

建議您為每個產品的每個供應項目與散發通道建立唯一的 GUID。 如果您不想要分割報告，您可以讓產品的多個散發通道使用單一 GUID。

如果您使用範本部署產品，而且在 Azure Marketplace 和 GitHub 上都有提供，您可以建立並註冊兩個不同的 Guid：

*    Azure Marketplace 中的產品 A
*    GitHub 上的產品 A

報告是由合作夥伴值 (Microsoft 合作夥伴 ID) 與 GUID 所完成。 您也可以在更細微的層級追蹤 Guid，以符合供應專案內的每個計畫。

如需詳細資訊，請參閱[使用 Guid 追蹤 Azure 客戶使用量常見問題](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution#faq)。

## <a name="create-a-billing-profile"></a>建立帳單設定檔

如果您要發佈[Dynamics 365 For Customer Engagement & Power Apps](./create-new-customer-engagement-offer.md)或[Dynamics 365 For Operations](./create-new-operations-offer.md)供應專案，您需要完成**帳單設定檔**。

系統會從您的法律實體預先填入帳單位址，您稍後可以更新此位址。 [稅務] 和 [加值稅] 識別碼欄位是選擇性的。  無法編輯國家/地區名稱和公司名稱。

## <a name="multi-user-account-management"></a>多使用者帳戶管理

合作夥伴中心會利用[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) （Azure AD）進行多使用者帳戶存取和管理。 貴組織的 Azure AD 會在註冊過程中自動與您的合作夥伴中心帳戶相關聯。

## <a name="manage-users"></a>管理使用者

[合作夥伴中心] 的 [**使用者**] 區段（在 [**帳戶設定**] 下）可讓您使用 Azure AD 來管理可存取合作夥伴中心帳戶的使用者、群組和 Azure AD 應用程式。 您的帳戶必須具有您要在其中新增或編輯使用者的[工作帳戶（Azure AD 租使用者）](./company-work-accounts.md)的[**管理員層級**](#define-user-roles-and-permissions)許可權。 若要管理不同工作帳戶/租使用者中的使用者，您必須登出，然後以具有該工作帳戶/租使用者之**管理員**許可權的使用者身分登入。

當您使用公司帳戶（Azure AD 租使用者）登入之後，您可以：

- [新增或移除使用者](#add-existing-users)
- [變更使用者密碼。](#change-a-user-password)
- [新增或移除群組](#manage-groups)
- [新增或移除 Azure AD 應用程式](#add-new-azure-ad-applications)
- [管理 Azure AD 應用程式的金鑰](#manage-keys-for-an-azure-ad-application)
- [定義使用者角色和權限](#define-user-roles-and-permissions)

請記住，所有合作夥伴中心使用者（包括群組和 Azure AD 應用程式）在與您的合作夥伴中心帳戶相關聯的[Azure AD 租](#manage-tenants)使用者中都必須有作用中的工作帳戶。

### <a name="add-existing-users"></a>新增現有使用者

若要將使用者新增至您公司的[工作帳戶（Azure AD 租使用者）](./company-work-accounts.md)中已存在的合作夥伴中心帳戶：

1. 移至 [**使用者**] （在 [**帳戶設定**] 底下），然後選取 [**新增使用者**]。
2. 從出現的清單中選取一個或多個使用者。 您可以使用搜尋方塊來搜尋特定的使用者。
* 如果您選取一個以上的使用者來新增至您的合作夥伴中心帳戶，您必須為他們指派相同的角色或一組自訂許可權。 若要新增具有不同角色/許可權的多個使用者，請針對每個角色或一組自訂許可權重複這些步驟。
3. 當您完成選擇使用者時，請按一下 [**加入選取**的]。
4. 在 **\[角色\]** 區段中，指定所選使用者的角色或自訂權限。
5. 選取 [儲存]  。

### <a name="create-new-users"></a>建立新的使用者

若要建立全新的使用者帳戶，您必須擁有具有[**全域管理員**](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)許可權的帳戶。 

1. 移至 [**使用者**] （在 [**帳戶設定**] 底下），選取 [**新增使用者**]，然後選擇 [**建立新使用者**]。
1. 為每個新使用者輸入名字、姓氏和使用者名稱。 
1. 如果您想要讓新使用者擁有組織目錄中的全域系統管理員帳戶，請核取 [**讓此使用者成為您 Azure AD 中的全域管理員] 方塊，並完全控制所有目錄資源**。 這將可讓使用者完整存取您公司的 Azure AD 中的所有系統管理功能。 除非您將適當的角色/許可權授與帳戶，否則他們將能夠在貴組織的工作帳戶（Azure AD 租使用者）中新增及管理使用者，而不是在合作夥伴中心內。
1. 如果您核取 [將**此使用者設為全域管理員**] 方塊，則必須提供**密碼復原電子郵件**，供使用者在必要時復原其密碼。
1. 在 [**群組成員資格**] 區段中，選取您要讓新使用者所屬的任何群組。
1. 在 **\[角色\]** 區段中，指定使用者的角色或自訂權限。
1. 選取 [儲存]  。

在合作夥伴中心建立新的使用者，也會在您登入的工作帳戶（Azure AD 租使用者）中建立該使用者的帳戶。 對合作夥伴中心內的使用者名稱進行變更，將會在您組織的工作帳戶（Azure AD 租使用者）中進行相同的變更。

### <a name="invite-new-users-by-email"></a>透過電子郵件邀請新使用者

若要透過電子郵件邀請目前不屬於您公司工作帳戶（Azure AD 租使用者）的使用者，您必須擁有具有[**全域管理員**](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)許可權的帳戶。

1. 前往 [**使用者**] （在 [**帳戶設定**] 下），選取 [**新增使用者**]，然後選擇 [**以電子郵件邀請使用者**]。
2. 輸入一個或多個電子郵件地址 (最多十個)，以逗號或分號分隔。
3. 在 **\[角色\]** 區段中，指定使用者的角色或自訂權限。
4. 選取 [儲存]  。

您邀請的使用者將會收到電子郵件邀請，以加入您的合作夥伴中心帳戶。 將會在您的工作帳戶（Azure AD 租使用者）中建立新的來賓使用者帳戶。 使用者必須接受邀請才能存取您的帳戶。

如果您需要重新傳送邀請，請造訪 [**使用者**] 頁面、在使用者清單中尋找邀請、選取其電子郵件地址（或顯示 [*邀請待決*] 的文字）。 然後，在頁面底部選取 [**重新傳送邀請**]。

> [!NOTE]
> 如果您的組織使用[目錄整合](https://go.microsoft.com/fwlink/p/?LinkID=724033)來同步處理內部部署目錄服務與您的 Azure AD，您將無法在合作夥伴中心建立新的使用者、群組或 Azure AD 應用程式。 您（或您內部部署目錄中的其他系統管理員）必須直接在內部部署目錄中建立它們，才能夠在合作夥伴中心看到並新增它們。

### <a name="remove-a-user"></a>移除使用者

若要從您的工作帳戶（Azure AD 租使用者）中移除使用者，請移至 [**使用者**] （在 [**帳戶設定**] 下），然後使用最右側資料行中的核取方塊來選取您想要移除的使用者，然後從可用的動作中選擇 [**移除**]。 隨即會出現一個快顯視窗，供您確認是否要移除選取的使用者。

### <a name="change-a-user-password"></a>變更使用者密碼。

如果您的其中一個使用者需要變更其密碼，則如果您在建立使用者帳戶時提供了**密碼復原電子郵件**，他們就可以自行執行此動作。 您也可以依照下列步驟來更新使用者的密碼。 若要在公司工作帳戶（Azure AD 租使用者）中變更使用者的密碼，您必須以具有[**全域管理員**](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)許可權的帳戶登入。 請注意，這會在您的 Azure AD 租使用者中變更使用者的密碼，以及用來存取合作夥伴中心的密碼。

1. 從 [**使用者**] 頁面（在 [**帳戶設定**] 下），選取您要編輯之使用者帳戶的名稱。
2. 選取頁面底部的 [**重設密碼**] 按鈕。
3. [確認] 頁面隨即出現，其中會顯示使用者的登入資訊，包括暫時密碼。 請務必列印或複製此資訊，並將它提供給該使用者，因為您在離開此頁面之後將無法存取該暫時密碼。

## <a name="manage-groups"></a>管理群組

群組可讓您同時控制多個使用者角色和許可權。

### <a name="add-an-existing-group"></a>新增現有的群組

若要將組織工作帳戶（Azure AD 租使用者）中已存在的群組新增至您的合作夥伴中心帳戶：

1. 從 [**使用者**] 頁面（在 [**帳戶設定**] 下）選取 [**新增群組**]。
2. 從出現的清單中選取一個或多個群組。 您可以使用搜尋方塊來搜尋特定的群組。
如果您選取多個群組以新增至您的合作夥伴中心帳戶，您必須將相同的角色或一組自訂許可權指派給他們。 若要新增具有不同角色/許可權的多個群組，請針對每個角色或一組自訂許可權重複這些步驟。
3. 當您完成選擇群組時，請按一下 [**加入選取**的]。
4. 在 **\[角色\]** 區段中，指定所選群組的角色或自訂權限。 不論與個別帳戶相關聯的角色和許可權為何，群組的所有成員都可以存取您的合作夥伴中心帳戶，以及您套用至群組的許可權。
5. 選取 [儲存]  。

當您新增現有的群組時，屬於該群組成員的每個使用者都將能夠存取您的合作夥伴中心帳戶，以及與該群組指派角色相關聯的許可權。

### <a name="add-a-new-group"></a>新增新的群組

若要將全新的群組新增至您的合作夥伴中心帳戶：

1. 從 [**使用者**] 頁面（在 [**帳戶設定**] 下）選取 [**新增群組**]。
2. 在下一個頁面上，選取 [**新增群組**]。
3. 輸入新群組的顯示名稱。
4. 指定群組的角色或自訂權限。 不論與個別帳戶相關聯的角色/許可權為何，群組的所有成員都可以存取您在此處套用的許可權給您的合作夥伴中心帳戶。
5. 從顯示的清單中選取新群組的 [使用者]。 您可以使用搜尋方塊來搜尋特定的使用者。
6. 完成選取使用者時，按一下 **\[新增選取的項目\]**，將其新增到新群組。
7. 選取 [儲存]  。

請注意，這個新群組也會建立在您組織的工作帳戶（Azure AD 租使用者）中，而不只是在您的合作夥伴中心帳戶中。

### <a name="remove-a-group"></a>移除群組

若要從您的工作帳戶（Azure AD 租使用者）中移除群組，請移至 [**使用者**] （在 [**帳戶設定**] 下），然後使用最右側資料行中的核取方塊來選取您想要移除的群組，然後從可用的動作中選擇 [**移除**]。 隨即會出現一個快顯視窗，供您確認是否要移除選取的群組。

## <a name="manage-azure-ad-applications"></a>管理 Azure AD 應用程式

您可以允許隸屬于公司 Azure AD 的應用程式或服務存取您的合作夥伴中心帳戶。

### <a name="add-existing-azure-ad-applications"></a>新增現有的 Azure AD 應用程式

若要新增公司 Azure Active Directory 中現有的應用程式：

1. 從 [**使用者**] 頁面（在 [**帳戶設定**] 下）選取 [**新增 Azure AD 應用程式**]。
2. 從出現的清單中選取一個或多個 Azure AD 應用程式。 您可以使用搜尋方塊來搜尋特定的 Azure AD 應用程式。 如果您選取一個以上的 Azure AD 應用程式來新增至您的合作夥伴中心帳戶，則必須為他們指派相同的角色或一組自訂許可權。 若要新增多個具有不同角色/許可權的 Azure AD 應用程式，請針對每個角色或一組自訂許可權重複這些步驟。
3. 當您完成選取 Azure AD 應用程式時，請按一下 [**新增選取**的]。
4. 在 **\[角色\]** 區段中，指定所選 Azure AD 應用程式的角色或自訂權限。
5. 選取 [儲存]  。

### <a name="add-new-azure-ad-applications"></a>加入新的 Azure AD 應用程式

如果您想要將合作夥伴中心存取權授與全新的 Azure AD 應用程式帳戶，您可以在 [**使用者**] 區段中建立一個。 請注意，這會在您的公司工作帳戶（Azure AD 租使用者）中建立新的帳戶，而不只是在您的合作夥伴中心帳戶中。 如果您主要是使用此 Azure AD 應用程式進行合作夥伴中心驗證，而且不需要使用者直接存取，您可以為 [**回復 URL** ] 和 [**應用程式識別碼 URI**] 輸入任何有效的位址，前提是您目錄中的任何其他 Azure AD 應用程式都不會使用這些值。

1. 從 [**使用者**] 頁面（在 [**帳戶設定**] 下）選取 [**新增 Azure AD 應用程式**]。
2. 在下一個頁面上，選取 [**新增 Azure AD 應用程式**]。
3. 輸入新 Azure AD 應用程式的 [**回復 URL** ]。 使用者可以從此 URL 登入並使用您的 Azure AD 應用程式 (也稱為 App URL 或登入 URL)。 **回覆 URL** 長度不可超過 256 個字元，而且在您的目錄中必須是唯一的。
4. 輸入新 Azure AD 應用程式的 **\[應用程式識別碼 URI\]**。 這是將單一登入要求傳送至 Azure AD 時，所顯示 Azure AD 應用程式的邏輯識別碼。 請注意，**應用程式識別碼 URI**對於您目錄中的每個 Azure AD 應用程式必須是唯一的。 此識別碼的長度不能超過256個字元。 如需應用程式識別碼 URI 的詳細資訊，請參閱[整合應用程式與 Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-modify-supported-accounts#change-the-application-registration-to-support-different-accounts)。
5. 在 **\[角色\]** 區段中，指定 Azure AD 應用程式的角色或自訂權限。
6. 選取 [儲存]  。

新增或建立 Azure AD 應用程式之後，您可以返回 **\[使用者\]** 區段，然後選取應用程式名稱以檢閱應用程式的設定，包括租用戶識別碼、用戶端識別碼、回覆 URL，以及 App 識別碼 URI。

### <a name="remove-an-azure-ad-application"></a>移除 Azure AD 應用程式

若要從您的工作帳戶（Azure AD 租使用者）中移除應用程式，請移至 [**使用者**] （在 [**帳戶設定**] 下），然後使用最右側資料行中的核取方塊來選取您想要移除的應用程式，然後從可用的動作中選擇 [**移除**]。 隨即會出現一個快顯視窗，供您確認是否要移除選取的應用程式。

### <a name="manage-keys-for-an-azure-ad-application"></a>管理 Azure AD 應用程式的金鑰

如果您的 Azure AD 應用程式會在 Microsoft Azure AD 讀取或寫入資料，則它需要金鑰。 您可以在合作夥伴中心編輯其資訊，以建立 Azure AD 應用程式的金鑰。 您也可以移除不再需要的金鑰。

1. 從 [**使用者**] 頁面（在 [**帳戶設定**] 下）中，選取 Azure AD 應用程式的名稱。 您會看到 Azure AD 應用程式的所有作用中金鑰，包括金鑰的建立日期和到期時間。 
2. 若要移除不再需要的金鑰，請選取 [**移除**]。
3. 若要加入新的金鑰，請選取 [**新增金鑰**]。
4. 您會看到顯示**用戶端識別碼**和**金鑰值**的畫面。 請務必列印或複製這份資訊，因為您離開此頁面之後將無法再存取。
5. 如果您想要建立更多金鑰，請選取 [**新增另一個金鑰**]。

## <a name="define-user-roles-and-permissions"></a>定義使用者角色和權限

貴公司的使用者可獲指派下列角色和合作夥伴中心的商業 Marketplace 方案許可權：

- **Manager**
  - 可以存取所有 Microsoft 帳戶的功能，但稅務和付款設定除外
  - 可以管理使用者、角色和工作帳戶（租使用者）
- **開發人員**
  - 可以管理及發佈供應專案
  - 可以查看一些發行者報表

> [!NOTE]
> 若為商業 Marketplace 方案，則不會使用全域管理員、商務參與者、財務參與者和市場行銷人員角色。 將這些角色指派給使用者不會有任何作用。 只有管理員和開發人員角色會授與許可權給使用者。

如需有關在合作夥伴中心的其他區域中管理角色和許可權的詳細資訊，例如 Azure Active Directory （AD）、雲端解決方案提供者（CSP）、控制台廠商（CPV）、來賓使用者或 Microsoft 合作夥伴網路（MPN），請參閱[在合作夥伴中心指派使用者角色和許可權](https://docs.microsoft.com/partner-center/permissions-overview)。

## <a name="manage-tenants"></a>管理租使用者

在這份檔中，Azure Active Directory （AD）租使用者也稱為您的「工作帳戶」，是您組織在 Azure 入口網站中設定的標記法，可協助您管理內部和外部使用者的特定 Microsoft 雲端服務實例。 如果您的組織訂閱了 Microsoft 雲端服務（例如 Azure、Microsoft Intune 或 Office 365），就會為您建立 Azure AD 租使用者。

您可以設定多個租使用者以搭配合作夥伴中心使用。 在合作夥伴中心帳戶中具有**管理員**角色的任何使用者，都可以選擇從帳戶新增和移除 Azure AD 的租使用者。  

### <a name="add-an-existing-tenant"></a>新增現有的租使用者

若要將另一個 Azure AD 租使用者與您的合作夥伴中心帳戶產生關聯：

1. 從 [租使用者 **] 頁面（在 [** **帳戶設定**] 下）選取 [**將另一個 Azure AD 租使用者建立關聯**]
2. 輸入您想要建立關聯之租用戶的 Azure AD 認證。
3. 檢閱 Azure AD 租用戶的組織和網域名稱。 若要完成關聯，請選取 **\[確認\]**。

如果關聯成功，您就可以開始在合作夥伴中心的 [**使用者**] 區段中新增和管理帳戶使用者。

### <a name="create-a-new-tenant"></a>建立新的租使用者

若要使用您的合作夥伴中心帳戶建立全新的 Azure AD 租使用者：

1. 從 [租使用者 **] 頁面（在 [** **帳戶設定**] 下），選取 [**建立新的 Azure AD 租**使用者]。
2. 輸入新 Azure AD 的目錄資訊︰
    - **功能變數名稱**：我們將用於 Azure AD 網域的唯一名稱，以及 "onmicrosoft.com"。 例如，如果您輸入「範例」，您的 Azure AD 網域會是 "example.onmicrosoft.com"。
    - **連絡人電子郵件**：我們可以在需要時針對帳戶相關資訊與您連絡的電子郵件地址。
    - **全域管理員使用者帳戶資訊**：您想要針對新的全域管理員帳戶使用的名字、姓氏、使用者名稱及密碼。
3. 選取 [**建立**] 以確認新的網域和帳戶資訊。
4. 使用新的 Azure AD 全域管理員使用者名稱和密碼登入，以開始[新增及管理使用者](#manage-users)。

如需在 Azure 入口網站內建立新租使用者的詳細資訊，而不是透過合作夥伴中心入口網站，請參閱[在 Azure Active Directory 中建立新的租使用者一](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)文。

### <a name="remove-a-tenant"></a>移除租使用者

若要從**您的合作夥伴**中心帳戶移除租使用者，請在 [租使用者] 頁面上尋找其名稱（在 [**帳戶設定**] 中），然後選取 [**移除**]。 系統會提示您確認是否要移除租使用者。 一旦您這麼做，該租使用者中的任何使用者都將無法登入合作夥伴中心帳戶，而且將會移除您為這些使用者設定的任何許可權。

當您移除租使用者時，從該租使用者新增至合作夥伴中心帳戶的所有使用者，將無法再登入該帳戶。

> [!TIP]
> 如果您目前已使用相同租使用者中的帳戶登入合作夥伴中心，則無法移除租使用者。 若要移除租使用者，您必須以**管理員**的身分登入合作夥伴中心，以取得與該帳戶相關聯的另一個租使用者。 如果只有一個租用戶關聯至帳戶，則必須以開啟該帳戶的 Microsoft 帳戶登入後，才能移除該租用戶。

## <a name="agreements"></a>合約

[合作夥伴中心] 的 [**協定**] 區段（在 [**帳戶設定**] 下）可讓您查看已授權的發佈合約清單。 這些合約會根據名稱和版本號碼列出，包括接受日期和接受合約的使用者名稱。

如果有需要注意的合約更新，則**需要的動作**可能會出現在此頁面的頂端。 若要接受已更新的合約，請先閱讀連結的合約版本，然後選取 [**接受合約**]。
