---
title: '使用 c # 管理 GA 環境中的參考資料-Azure 時間序列深入解析 |Microsoft Docs'
description: '瞭解如何藉由建立以 c # 撰寫的自訂應用程式，來管理 GA 環境的參考資料。'
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/15/2020
ms.custom: seodec18
ms.openlocfilehash: f0ce0f7d90540274d24a7e0248e6f197b74033a1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416974"
---
# <a name="manage-ga-reference-data-for-an-azure-time-series-insights-environment-using-c"></a>使用 C 管理 Azure 時間序列深入解析環境的 GA 參考資料#

本文示範如何結合 c #、 [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)和 Azure Active Directory，將程式設計 api 要求提供給 Azure 時間序列深入解析 GA[參考資料管理 api](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api)。

> [!TIP]
> 查看 GA c # 程式碼[https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample)範例，網址為。

## <a name="summary"></a>[摘要]

下面的範例程式碼示範下列功能：

* 使用[MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) **PublicClientApplication**取得存取權杖。
* 針對 GA[參考資料管理 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api)的順序建立、讀取、更新和刪除作業。
* 常見的回應碼，包括[一般錯誤碼](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api#validation-and-error-handling)。
    
    參考資料管理 API 會個別處理每個專案，而有一個專案的錯誤則無法防止其他人成功完成。 例如，如果您的要求有100個專案，而其中一個專案發生錯誤，則會寫入99個專案，而其中一個會遭到拒絕。

## <a name="prerequisites-and-setup"></a>必要條件和設定

編譯及執行範例程式碼之前，您必須先完成下列步驟：

1. 布建[GA Azure 時間序列深入解析](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started
)環境。

1. 在您的環境中[建立參考資料集](time-series-insights-add-reference-data-set.md)。 使用下列參考資料配置：

   | 索引鍵名稱 | 類型 |
   | --- | --- |
   | uuid | 字串 | 

1. 如[驗證和授權](time-series-insights-authentication-and-authorization.md)中所述，設定您的 Azure 時間序列深入解析環境以進行 Azure Active Directory。 使用`http://localhost:8080/`做為重新**導向 URI**。

1. 安裝必要的專案相依性。

1. 以適當的環境識別碼取代每個 **#PLACEHOLDER #** ，以編輯下面的範例程式碼。

1. 在`dotnet run`專案的根目錄中執行。 出現提示時，請使用您的使用者設定檔來登入 Azure。 

## <a name="project-dependencies"></a>專案相依性

建議使用最新版本的 Visual Studio 和**NETCore**：

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) -版本 16.4.2 +
* [NETCore. app](https://www.nuget.org/packages/Microsoft.NETCore.App/2.2.8) -版本 v2.2.8-preview

範例程式碼有兩個必要的相依性：

* MSAL.NET [4.7.1 套件。](https://www.nuget.org/packages/Microsoft.Identity.Client/)
* [Newtonsoft. Json](https://www.nuget.org/packages/Newtonsoft.Json) -12.0.3 package。

使用[NuGet 2.12](https://www.nuget.org/)新增封裝 +：

* `dotnet add package Newtonsoft.Json --version 12.0.3`
* `dotnet add package Microsoft.Identity.Client --version 4.7.1`

或：

1. `csharp-tsi-msal-ga-sample.csproj`宣告檔案：

    ```XML
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <LangVersion>latest</LangVersion>
        <RootNamespace>csharp-tsi-msal-ga-sample</RootNamespace>
        <RunWorkingDirectory>$(MSBuildThisFileDirectory)</RunWorkingDirectory>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Identity.Client" Version="4.7.1.0" Culture="neutral" PublicKeyToken="0a613f4dd989e8ae" />
        <PackageReference Include="Newtonsoft.Json" Version="12.0.3" />
      </ItemGroup>
    </Project>
    ```
1. 然後執行 `dotnet restore`。

## <a name="c-sample-code"></a>C# 範例程式碼

```csharp
// Copyright (c) Microsoft Corporation.  All rights reserved.

namespace CsharpTsiMsalGaSample
{
    using Microsoft.Identity.Client;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    public static class Program
    {
        /**
         * Review the product documentation for detailed configuration steps or skip ahead and configure your environment settings.
         * 
         * https://docs.microsoft.com/azure/time-series-insights/time-series-insights-authentication-and-authorization
         */

        // Azure Time Series Insights environment configuration
        internal static string EnvironmentFqdn = "#PLACEHOLDER#.env.timeseries.azure.com";
        internal static string EnvironmentReferenceDataSetName = "#PLACEHOLDER#";

        // Azure Active Directory application configuration
        internal static string AadClientApplicationId = "#PLACEHOLDER#";
        internal static string[] AadScopes = new string[] { "https://api.timeseries.azure.com//user_impersonation" };
        internal static string AadRedirectUri = "http://localhost:8080/";
        internal static string AadTenantName = "#PLACEHOLDER#";
        internal static string AadAuthenticationAuthority = "https://login.microsoftonline.com/" + AadTenantName + ".onmicrosoft.com/oauth2/authorize?resource=https://api.timeseries.azure.com/";

        private static async Task<string> AcquireAccessTokenAsync()
        {
            if (AadClientApplicationId == "#PLACEHOLDER#" || AadScopes.Length == 0 || AadRedirectUri == "#PLACEHOLDER#" || AadTenantName.StartsWith("#PLACEHOLDER#"))
            {
                throw new Exception($"Use the link {"https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started"} to update the values of 'AadClientApplicationId', 'AadScopes', 'AadRedirectUri', and 'AadAuthenticationAuthority'.");
            }

            /**
             * MSAL.NET configuration. Review the product documentation for more information about MSAL.NET authentication options.
             * 
             * https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/
             */

            IPublicClientApplication app = PublicClientApplicationBuilder
                .Create(AadClientApplicationId)
                .WithRedirectUri(AadRedirectUri)
                .WithAuthority(AadAuthenticationAuthority)
                .Build();

            AuthenticationResult result = await app
                .AcquireTokenInteractive(AadScopes)
                .ExecuteAsync();

            Console.WriteLine("MSAL Authentication Token Acquired: {0}", result.AccessToken);
            Console.WriteLine("");
            return result.AccessToken;
        }

        // System.Net.HttpClient helper to wrap HTTP POST made to the GA Reference Data API
        private static async Task<HttpResponseMessage> AsyncHttpPostRequestHelper(HttpClient httpClient, string input)
        {
             if (EnvironmentFqdn.StartsWith("#PLACEHOLDER#") || EnvironmentReferenceDataSetName == "#PLACEHOLDER#")
             {
                throw new Exception($"Use the link {"https://docs.microsoft.com/azure/time-series-insights/time-series-insights-authentication-and-authorization"} to update the values of 'EnvironmentFqdn' and 'EnvironmentReferenceDataSetName'.");
             }

             Console.WriteLine("HTTP JSON Request Body: {0}", input);
             Console.WriteLine("");
             HttpContent requestBody = new StringContent(input, Encoding.UTF8, "application/json");

             Uri uri = new UriBuilder("https", EnvironmentFqdn)
             {
                Path = $"referencedatasets/{EnvironmentReferenceDataSetName}/$batch",
                Query = "api-version=2016-12-12"
             }.Uri;
                
             Console.WriteLine("Making HTTP POST to URI: {0}", uri);
             Console.WriteLine("");

             HttpResponseMessage response = await httpClient.PostAsync(uri, requestBody);
             if (response.IsSuccessStatusCode)
             {
                var jsonString = await response.Content.ReadAsStringAsync();
                var jsonStringTransferObject = JsonConvert.DeserializeObject<object>(jsonString);
                Console.WriteLine("HTTP JSON Response Body: {0}", jsonStringTransferObject);
                Console.WriteLine("");
                return response;
             }

             return null;
        }

        private static async Task TsiMsalGaSample()
        {
            Console.WriteLine("Beginning demo...");
            Console.WriteLine("");
            Console.WriteLine("The following samples assume a single Key Name (uuid) with String type...");
            Console.WriteLine("");

            string accessToken = await AcquireAccessTokenAsync();
            var httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer " + accessToken);

            {   
                // CREATE reference data
                Console.WriteLine("CREATE reference data example...");
                Console.WriteLine("");
                string createInput = @"
                    {
                        ""put"": [
                            {
                                ""uuid"": ""Fan1""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""color"": ""White"",
                                ""floor"": 2
                            },
                            {
                                ""uuid"": ""Fan3"",
                                ""color"": ""Red"",
                                ""maxSpeed"": 5
                            }
                        ]
                    }";


                var createResponse = await AsyncHttpPostRequestHelper(httpClient, createInput);

                // READ reference data
                Console.WriteLine("READ reference data example...");
                Console.WriteLine("");
                string readInput = @"
                    {
                        ""get"": [
                            {
                                ""uuid"": ""Fan1""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""color"": ""White"",
                                ""desc"": ""example""
                            },
                            {
                                ""uuid"": ""Fan3"",
                                ""color"": ""Red"",
                                ""maxSpeed"": 5
                            }
                        ]
                    }";

                var readResponse = await AsyncHttpPostRequestHelper(httpClient, readInput);

                // UPDATE reference data
                Console.WriteLine("UPDATE reference data example...");
                Console.WriteLine("");
                string updateInput = @"
                    {
                        ""patch"": [
                            {
                                ""uuid"": ""Fan1"",
                                ""color"": ""Red""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""color"": ""Orange""
                            },
                            {
                                ""uuid"": ""Fan3"",
                                ""desc"": ""Example""
                            }
                        ]
                    }";

                var inputResponse = await AsyncHttpPostRequestHelper(httpClient, updateInput);

                // DELETE reference data
                Console.WriteLine("DELETE reference data example...");
                Console.WriteLine("");
                string deleteInput = @"
                    {
                        ""delete"": [
                            {
                                ""uuid"": ""Fan1""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""color"": ""Orange""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""desc"": ""Blue""
                            }
                        ]
                    }";

                var deleteResponse = await AsyncHttpPostRequestHelper(httpClient, deleteInput);
            }
        }

        internal static void Main(string[] args)
        {
            Task.Run(async () => await TsiMsalGaSample()).Wait();
        }
    }
}
```

## <a name="next-steps"></a>後續步驟

- 閱讀 GA[參考資料管理 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api)參考檔。
