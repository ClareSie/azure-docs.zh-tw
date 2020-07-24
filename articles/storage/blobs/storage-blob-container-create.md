---
title: 使用 .NET 建立或刪除 blob 容器-Azure 儲存體
description: 瞭解如何使用 .NET 用戶端程式庫，在您的 Azure 儲存體帳戶中建立或刪除 blob 容器。
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/22/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: abf4cb33fa953ec9a257397551b3d17752fe67f5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87070730"
---
# <a name="create-or-delete-a-container-in-azure-storage-with-net"></a>使用 .NET 建立或刪除 Azure 儲存體中的容器

Azure 儲存體中的 blob 會組織成容器。 在您可以上傳 blob 之前，您必須先建立容器。 本文說明如何使用[適用于 .net 的 Azure 儲存體用戶端程式庫](/dotnet/api/overview/azure/storage?view=azure-dotnet)來建立和刪除容器。

## <a name="name-a-container"></a>為容器命名

容器名稱必須是有效的 DNS 名稱，因為它會形成用來定址容器或其 blob 的唯一 URI 的一部分。 為容器命名時，請遵循下列規則：

- 容器名稱的長度可以介於3到63個字元之間。
- 容器名稱必須以字母或數位開頭，且只能包含小寫字母、數位和虛線（-）字元。
- 容器名稱中不允許有兩個或多個連續的虛線字元。

容器的 URI 採用下列格式：

`https://myaccount.blob.core.windows.net/mycontainer`

## <a name="create-a-container"></a>建立容器

若要建立容器，請呼叫下列其中一個方法：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

- [建立](/dotnet/api/azure.storage.blobs.blobcontainerclient.create)
- [CreateAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.createasync)
- [CreateIfNotExists](/dotnet/api/azure.storage.blobs.blobcontainerclient.createifnotexists)
- [CreateIfNotExistsAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.createifnotexistsasync)

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

- [建立](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.create)
- [CreateAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createasync)
- [CreateIfNotExists](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createifnotexists)
- [CreateIfNotExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createifnotexistsasync)
---

如果已有相同名稱的容器， **Create**和**CreateAsync**方法就會擲回例外狀況。

**CreateIfNotExists**和**CreateIfNotExistsAsync**方法會傳回布林值，指出是否已建立容器。 如果已經存在相同名稱的容器，這些方法會傳回**False** ，表示未建立新的容器。

容器會在儲存體帳戶下方直接建立。 您無法將某個容器巢於另一個容器下方。

下列範例會以非同步方式建立容器：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Containers.cs" id="CreateSampleContainerAsync":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
private static async Task<CloudBlobContainer> CreateSampleContainerAsync(CloudBlobClient blobClient)
{
    // Name the sample container based on new GUID, to ensure uniqueness.
    // The container name must be lowercase.
    string containerName = "container-" + Guid.NewGuid();

    // Get a reference to a sample container.
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    try
    {
        // Create the container if it does not already exist.
        bool result = await container.CreateIfNotExistsAsync();
        if (result == true)
        {
            Console.WriteLine("Created container {0}", container.Name);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
    }

    return container;
}
```
---

## <a name="create-the-root-container"></a>建立根容器

根容器會做為儲存體帳戶的預設容器。 每個儲存體帳戶可能會有一個根容器，其必須命名為 *$root*。 根容器必須明確地建立或刪除。

您可以參考儲存在根容器中的 blob，而不需要包含根容器名稱。 根容器可讓您在儲存體帳戶階層的最上層參考 blob。 例如，您可以透過下列方式參考根容器中的 blob：

`https://myaccount.blob.core.windows.net/default.html`

下列範例會以同步方式建立根容器：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Containers.cs" id="CreateRootContainer":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
private static void CreateRootContainer(CloudBlobClient blobClient)
{
    try
    {
        // Create the root container if it does not already exist.
        CloudBlobContainer container = blobClient.GetContainerReference("$root");
        if (container.CreateIfNotExists())
        {
            Console.WriteLine("Created root container.");
        }
        else
        {
            Console.WriteLine("Root container already exists.");
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
    }
}
```
---

## <a name="delete-a-container"></a>刪除容器

若要刪除 .NET 中的容器，請使用下列其中一種方法：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

- [刪除](/dotnet/api/azure.storage.blobs.blobcontainerclient.delete)
- [DeleteAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.deleteasync)
- [DeleteIfExists](/dotnet/api/azure.storage.blobs.blobcontainerclient.deleteifexists)
- [Cloudblobcontainer.deleteifexistsasync](/dotnet/api/azure.storage.blobs.blobcontainerclient.deleteifexistsasync)

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

- [刪除](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.delete)
- [DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteifexists)
- [Cloudblobcontainer.deleteifexistsasync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteifexistsasync)
---

如果容器不存在， **Delete**和**DeleteAsync**方法會擲回例外狀況。

**DeleteIfExists**和**cloudblobcontainer.deleteifexistsasync**方法會傳回布林值，指出容器是否已刪除。 如果指定的容器不存在，則這些方法會傳回**False** ，表示容器並未刪除。

刪除容器之後，您就無法使用相同名稱建立*至少*30 秒的容器。 嘗試建立具有相同名稱的容器將會失敗，並出現 HTTP 錯誤碼409（衝突）。 容器上的任何其他作業或其包含的 blob 將會失敗，並出現 HTTP 錯誤碼404（找不到）。

下列範例會刪除指定的容器，並在容器不存在時處理例外狀況：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Containers.cs" id="DeleteSampleContainerAsync":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
private static async Task DeleteSampleContainerAsync(CloudBlobClient blobClient, string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    try
    {
        // Delete the specified container and handle the exception.
        await container.DeleteAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```
---

下列範例示範如何刪除以指定前置詞開頭的所有容器。

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Containers.cs" id="DeleteContainersWithPrefixAsync":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
private static async Task DeleteContainersWithPrefixAsync(CloudBlobClient blobClient, string prefix)
{
    Console.WriteLine("Delete all containers beginning with the specified prefix");
    try
    {
        foreach (var container in blobClient.ListContainers(prefix))
        {
            Console.WriteLine("\tContainer:" + container.Name);
            await container.DeleteAsync();
        }

        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```
---

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>另請參閱

- [建立容器作業](/rest/api/storageservices/create-container)
- [刪除容器作業](/rest/api/storageservices/delete-container)
