---
title: .NET-服務對服務驗證-Data Lake Storage Gen1
description: 了解如何使用 .NET SDK 搭配 Azure Active Directory 向 Azure Data Lake Storage Gen1 完成服務對服務驗證
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 19b4ac619ec3e72c787efc8e9f043f42dbd8b09b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85505149"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>使用 .NET SDK 向 Azure Data Lake Storage Gen1 進行服務對服務驗證
> [!div class="op_single_selector"]
> * [使用 Java](data-lake-store-service-to-service-authenticate-java.md)
> * [使用 .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [使用 Python](data-lake-store-service-to-service-authenticate-python.md)
> * [使用 REST API](data-lake-store-service-to-service-authenticate-rest-api.md)
>
>

在此文章中，您會了解如何使用 .NET SDK 向 Azure Data Lake Storage Gen1 進行服務對服務驗證。 針對使用 .NET SDK 向 Data Lake Storage Gen1 進行使用者驗證，請參閱[使用 .NET SDK 向 Data Lake Storage Gen1 進行使用者驗證](data-lake-store-end-user-authenticate-net-sdk.md)。

## <a name="prerequisites"></a>Prerequisites
* **Visual Studio 2013 或**更新版本。 下列指示使用 Visual Studio 2019。

* **Azure 訂用帳戶**。 請參閱[取得 Azure 免費試用](https://azure.microsoft.com/pricing/free-trial/)。

* **建立 Azure Active Directory "Web" 應用程式**。 您必須已經完成[使用 Azure Active Directory 向 Data Lake Storage Gen1 進行服務對服務驗證](data-lake-store-service-to-service-authenticate-using-active-directory.md)中的步驟。

## <a name="create-a-net-application"></a>建立 .NET 應用程式
1. 在 Visual Studio 中，依序 **選取 [檔案** ] 功能表、[ **新增**] 和 [ **專案**]。
2. 選擇 [ **主控台應用程式 ( .NET Framework) **]，然後選取 **[下一步]**。
3. 在 [專案名稱]**** 中，輸入 `CreateADLApplication`，然後選取 [建立]****。

4. 將 NuGet 套件新增至您的專案。

   1. 在方案總管中以滑鼠右鍵按一下專案名稱，然後按一下 [ **管理 NuGet 封裝**]。
   2. 在 [ **NuGet 封裝管理員** ] 索引標籤中，確定已選取 [ **套件來源** ] 設定為 [ **nuget.org** ]，並已選取 [ **包含發行** 前版本] 核取方塊。
   3. 搜尋並安裝下列 NuGet 封裝：

      * `Microsoft.Azure.Management.DataLake.Store` - 本教學課程使用 v2.1.3-preview。
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - 本教學課程使用 v2.2.12。

        ![新增 NuGet 來源](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "建立新的 Azure Data Lake 帳戶")
   4. 關閉 **NuGet 封裝管理員**。

5. 開啟 **Program.cs**，刪除現有的程式碼，然後納入下列陳述式以新增命名空間的參考。

```csharp
using System;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading;
using System.Collections.Generic;
using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

## <a name="service-to-service-authentication-with-client-secret"></a>使用用戶端祕密進行服務對服務驗證
在 .NET 用戶端應用程式中加入這個程式碼片段。 以從 Azure AD Web 應用程式 (列為必要元件) 中擷取的值取代預留位置值。 這個程式碼片段可讓您使用適用於 Azure AD Web 應用程式的用戶端祕密/金鑰，以**非互動方式**向 Data Lake Storage Gen1 驗證應用程式。

```csharp
private static void Main(string[] args)
{
    // Service principal / application authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    string TENANT = "<AAD-directory-domain>";
    string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
    System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
    System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
    string secret_key = "<AAD_WEB_APP_SECRET_KEY>";
    var armCreds = GetCreds_SPI_SecretKey(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, secret_key);
    var adlCreds = GetCreds_SPI_SecretKey(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, secret_key);
}
```

前述程式碼片段會使用協助程式函式 `GetCreds_SPI_SecretKey`。 從 [GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_secretkey) 即可取得此協助程式函式的程式碼。

## <a name="service-to-service-authentication-with-certificate"></a>使用憑證進行服務對服務驗證

在 .NET 用戶端應用程式中加入這個程式碼片段。 以從 Azure AD Web 應用程式 (列為必要元件) 中擷取的值取代預留位置值。 這個程式碼片段可讓您使用適用於 Azure AD Web 應用程式的憑證，以**非互動方式**向 Data Lake Storage Gen1 驗證應用程式。 如需有關如何建立 Azure AD 應用程式的指示，請參閱[使用憑證建立服務主體](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate)。

```csharp
private static void Main(string[] args)
{
    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    string TENANT = "<AAD-directory-domain>";
    string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
    System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
    System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
    var cert = new X509Certificate2(@"d:\cert.pfx", "<certpassword>");
    var armCreds = GetCreds_SPI_Cert(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, cert);
    var adlCreds = GetCreds_SPI_Cert(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, cert);
}
```

前述程式碼片段會使用協助程式函式 `GetCreds_SPI_Cert`。 從 [GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_cert) 即可取得此協助程式函式的程式碼。

## <a name="next-steps"></a>接下來的步驟
在此文章中，您已了解如何使用 .NET SDK 透過服務對服務驗證向 Data Lake Storage Gen1 進行驗證。 您現在可以看看下列文章，了解如何使用 .NET SDK 搭配 Data Lake Storage Gen1。

* [使用.NET SDK 在 Data Lake Storage Gen1 上進行帳戶管理作業](data-lake-store-get-started-net-sdk.md)
* [使用 .NET SDK 對 Data Lake Storage Gen1 進行資料作業](data-lake-store-data-operations-net-sdk.md)
