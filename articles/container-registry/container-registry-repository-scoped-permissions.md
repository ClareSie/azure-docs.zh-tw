---
title: Azure Container Registry 中存放庫的許可權
description: 建立權杖，其許可權範圍限於登錄中的特定存放庫，以提取或推送映射，或執行其他動作
ms.topic: article
ms.date: 02/13/2020
ms.openlocfilehash: 9004c45401833d3070266055dd7eb99a2bb43bde
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "80618835"
---
# <a name="create-a-token-with-repository-scoped-permissions"></a>建立具有存放庫範圍許可權的權杖

本文說明如何建立權杖和範圍對應，以管理容器登錄中的存放庫範圍許可權。 藉由建立權杖，登錄擁有者可以提供具有限定範圍、限時存取存放庫的使用者或服務，以提取或推送映射，或執行其他動作。 權杖提供更細微的許可權，而不是其他登錄[驗證選項](container-registry-authentication.md)，其範圍是整個登錄的許可權。 

建立權杖的案例包括：

* 允許具有個別權杖的 IoT 裝置從存放庫提取映射
* 提供具有特定存放庫許可權的外部組織 
* 將存放庫存取限制為您組織中的不同使用者群組。 例如，針對建立以特定存放庫為目標之影像的開發人員，以及從這些存放庫部署之小組的讀取存取權，提供寫入和讀取權限。

> [!IMPORTANT]
> 此功能目前在預覽階段，但[有某些限制](#preview-limitations)。 若您同意[補充的使用規定][terms-of-use]即可取得預覽。 在公開上市 (GA) 之前，此功能的某些領域可能會變更。

## <a name="preview-limitations"></a>預覽限制

* 這項功能僅適用于**Premium**容器登錄。 如需登錄服務層和限制的相關資訊，請參閱[Azure Container Registry sku](container-registry-skus.md)。
* 您目前無法將存放庫範圍的許可權指派給 Azure Active Directory 身分識別，例如服務主體或受控識別。
* 您無法在已啟用[匿名提取存取](container-registry-faq.md#how-do-i-enable-anonymous-pull-access)的登錄中建立範圍對應。

## <a name="concepts"></a>概念

若要設定存放庫範圍的許可權，您可以使用相關聯的*範圍對應*來建立*權杖*。 

* **權杖**連同產生的密碼可讓使用者向登錄進行驗證。 您可以設定權杖密碼的到期日，或隨時停用權杖。  

  使用權杖進行驗證之後，使用者或服務可以執行一或多個範圍設定為一個或多個存放庫的*動作*。

  |動作  |描述  | 範例 |
  |---------|---------|--------|
  |`content/delete`    | 從存放庫移除資料  | 刪除存放庫或資訊清單 |
  |`content/read`     |  讀取存放庫中的資料 |  提取成品 |
  |`content/write`     |  將資料寫入存放庫     | 搭配使用`content/read`以推送成品 |
  |`metadata/read`    | 讀取存放庫中的中繼資料   | 列出標記或資訊清單 |
  |`metadata/write`     |  將中繼資料寫入存放庫  | 啟用或停用讀取、寫入或刪除作業 |

* **範圍對應**會將您套用至權杖的存放庫許可權分組，並可重新套用至其他權杖。 每個權杖都與單一範圍對應相關聯。 

   使用範圍對應：

    * 使用與一組存放庫相同的許可權來設定多個權杖
    * 當您新增或移除範圍對應中的存放庫動作，或套用不同的範圍對應時，更新權杖許可權 

  Azure Container Registry 也提供數個您可以套用的系統定義範圍對應，以及所有存放庫的固定許可權。

下圖顯示權杖與範圍對應之間的關聯性。 

![登錄權杖和範圍對應](media/container-registry-repository-scoped-permissions/token-scope-map-concepts.png)

## <a name="prerequisites"></a>先決條件

* **Azure CLI** Azure CLI 用來建立和管理權杖的命令，可在 Azure CLI 版本2.0.76 或更新版本中取得。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI](/cli/azure/install-azure-cli)。
* **Docker** -若要向登錄進行驗證以提取或推送映射，您需要本機 Docker 安裝。 Docker 提供 [macOS](https://docs.docker.com/docker-for-mac/)、[Windows](https://docs.docker.com/docker-for-windows/) 和 [Linux](https://docs.docker.com/engine/installation/#supported-platforms) 系統的安裝指示。
* **Container registry** -如果您沒有，請在 Azure 訂用帳戶中建立 Premium 容器登錄，或升級現有的登錄。 例如，使用 [Azure 入口網站](container-registry-get-started-portal.md)或 [Azure CLI](container-registry-get-started-azure-cli.md)。 

## <a name="create-token---cli"></a>建立權杖-CLI

### <a name="create-token-and-specify-repositories"></a>建立權杖並指定存放庫

使用[az acr token create][az-acr-token-create]命令來建立權杖。 建立權杖時，您可以在每個存放庫上指定一或多個儲存機制和相關聯的動作。 存放庫還不需要在登錄中。 若要指定現有的範圍對應來建立權杖，請參閱下一節。

下列範例會在登錄*myregistry*中建立具有存放庫下列許可權`samples/hello-world`的權杖： `content/write`和。 `content/read` 根據預設，此命令會將預設的權杖狀態`enabled`設定為，但您可以隨時將`disabled`狀態更新為。

```azurecli
az acr token create --name MyToken --registry myregistry \
  --repository samples/hello-world \
  content/write content/read
```

輸出會顯示權杖的詳細資料，包括兩個產生的密碼。 建議您將密碼儲存在安全的位置，以供稍後用來進行驗證。 無法再次抓取密碼，但可以產生新的密碼。

```console
{
  "creationDate": "2020-01-18T00:15:34.066221+00:00",
  "credentials": {
    "certificates": [],
    "passwords": [
      {
        "creationTime": "2020-01-18T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password1",
        "value": "uH54BxxxxK7KOxxxxRbr26dAs8JXxxxx"
      },
      {
        "creationTime": "2020-01-18T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password2",
        "value": "kPX6Or/xxxxLXpqowxxxxkA0idwLtmxxxx"
      }
    ],
    "username": "MyToken"
  },
  "id": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/tokens/MyToken",
  "name": "MyToken",
  "objectId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "scopeMapId": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/scopeMaps/MyToken-scope-map",
  "status": "enabled",
  "type": "Microsoft.ContainerRegistry/registries/tokens"
```

輸出會包含建立命令之範圍對應的詳細資料。 您可以使用此處命名`MyToken-scope-map`的範圍對應，將相同的儲存機制動作套用至其他權杖。 或者，稍後更新範圍對應，以變更相關聯權杖的許可權。

### <a name="create-token-and-specify-scope-map"></a>建立權杖並指定範圍對應

建立權杖的另一種方法是指定現有的範圍對應。 如果您還沒有範圍對應，請先指定存放庫和相關聯的動作來建立一個。 然後，在建立權杖時指定範圍對應。 

若要建立範圍對應，請使用[az acr scope-map create][az-acr-scope-map-create]命令。 下列命令會在先前使用的`samples/hello-world`存放庫上建立具有相同許可權的範圍對應。 

```azurecli
az acr scope-map create --name MyScopeMap --registry myregistry \
  --repository samples/hello-world \
  content/write content/read \
  --description "Sample scope map"
```

執行[az acr token create][az-acr-token-create]來建立權杖，並指定*MyScopeMap*範圍對應。 如先前範例所示，此命令會將預設的權杖狀態`enabled`設定為。

```azurecli
az acr token create --name MyToken \
  --registry myregistry \
  --scope-map MyScopeMap
```

輸出會顯示權杖的詳細資料，包括兩個產生的密碼。 建議您將密碼儲存在安全的位置，以供稍後用來進行驗證。 無法再次抓取密碼，但可以產生新的密碼。

## <a name="create-token---portal"></a>建立權杖-入口網站

您可以使用 Azure 入口網站來建立權杖和範圍對應。 如同`az acr token create` CLI 命令，您可以套用現有的範圍對應，或藉由指定一或多個存放庫和相關聯的動作，在建立權杖時建立範圍對應。 存放庫還不需要在登錄中。 

下列範例會建立權杖，並在存放`samples/hello-world`庫上建立具有下列許可權的範圍對應： `content/write`和。 `content/read`

1. 在入口網站中，流覽至您的 container registry。
1. 在 [**服務**] 下，選取 **[權杖（預覽）] > + 新增**]。
  ![在入口網站中建立權杖](media/container-registry-repository-scoped-permissions/portal-token-add.png)
1. 輸入權杖名稱。
1. 在 [**範圍對應**] 底下，選取 [**新建**]。
1. 設定範圍對應：
    1. 輸入範圍對應的 [名稱] 和 [描述]。 
    1. 在 [**存放庫** `samples/hello-world`] 底下輸入，然後在`content/read` [ `content/write`**許可權**] 下選取 [和]。 然後選取 [ **+ 新增**]。  
    ![在入口網站中建立範圍對應](media/container-registry-repository-scoped-permissions/portal-scope-map-add.png)

    1. 新增存放庫和許可權之後，請選取 [**新增**] 以新增範圍對應。
1. 接受預設權杖**狀態**為 [**已啟用**]，然後選取 [**建立**]。

驗證並建立權杖之後，權杖詳細資料會出現在 [**權杖**] 畫面中。

### <a name="add-token-password"></a>新增權杖密碼

建立權杖後產生密碼。 若要使用登錄進行驗證，必須啟用權杖並具有有效的密碼。

您可以產生一或兩個密碼，並為每個密碼設定一個到期日。 

1. 在入口網站中，流覽至您的 container registry。
1. 在 [**服務**] 底下，選取 **[權杖（預覽）**]，然後選取權杖。
1. 在 [權杖詳細資料] 中，選取 [ **password1** ] 或 [ **password2**]，然後選取 [產生] 圖示。
1. 在 [密碼] 畫面中，選擇性地設定密碼的到期日，然後選取 [**產生**]。
1. 產生密碼之後，請複製並將它儲存到安全的位置。 您無法在關閉畫面後取出產生的密碼，但您可以產生新的密碼。

    ![在入口網站中建立權杖密碼](media/container-registry-repository-scoped-permissions/portal-token-password.png)

## <a name="authenticate-with-token"></a>使用權杖進行驗證

當使用者或服務使用權杖來向目標登錄進行驗證時，它會提供權杖名稱做為使用者名稱和其中一個產生的密碼。 驗證方法取決於已設定的動作或與權杖相關聯的動作。

|動作  |如何驗證  |
  |---------|---------|
  |`content/delete`    | `az acr repository delete`在 Azure CLI |
  |`content/read`     |  `docker login`<br/><br/>`az acr login`在 Azure CLI  |
  |`content/write`     |  `docker login`<br/><br/>`az acr login`在 Azure CLI     |
  |`metadata/read`    | `az acr repository show`<br/><br/>`az acr repository show-tags`<br/><br/>`az acr repository show-manifests`在 Azure CLI   |
  |`metadata/write`     |  `az acr repository untag`<br/><br/>`az acr repository update`在 Azure CLI |

## <a name="examples-use-token"></a>範例：使用 token

下列範例會使用本文稍早建立的權杖來執行存放庫的一般作業：推送和提取映射、刪除映射，以及列出存放庫標記。 此權杖最初是以`content/write` `content/read` `samples/hello-world`儲存機制上的推播許可權（和動作）來設定。

### <a name="pull-and-tag-test-images"></a>提取和標記測試影像

針對下列範例，請從 Docker `hello-world` Hub `alpine`提取和映射，並為您的登錄和存放庫標記它們。

```bash
docker pull hello-world
docker pull alpine
docker tag hello-world myregistry.azurecr.io/samples/hello-world:v1
docker tag hello-world myregistry.azurecr.io/samples/alpine:v1
```

### <a name="authenticate-using-token"></a>使用權杖進行驗證

執行`docker login`以向登錄進行驗證、提供權杖名稱做為使用者名稱，並提供其中一個密碼。 權杖必須具有`Enabled`狀態。

下列範例會針對 bash shell 進行格式化，並使用環境變數來提供值。

```bash
TOKEN_NAME=MyToken
TOKEN_PWD=<token password>

echo $TOKEN_PWD | docker login --username $TOKEN_NAME --password-stdin myregistry.azurecr.io
```

輸出應該會顯示成功的驗證：

```console
Login Succeeded
```

### <a name="push-images-to-registry"></a>將映像推送到登錄

成功登入之後，嘗試將標記的映射推送至登錄。 因為權杖具有將映射推送至存放`samples/hello-world`庫的許可權，所以下列推送會成功：

```bash
docker push myregistry.azurecr.io/samples/hello-world:v1
```

權杖沒有存放庫的許可權`samples/alpine` ，因此下列的推送嘗試會失敗，並出現類似`requested access to the resource is denied`下列的錯誤：

```bash
docker push myregistry.azurecr.io/samples/alpine:v1
```

### <a name="change-pushpull-permissions"></a>變更推送/提取許可權

若要更新權杖的許可權，請更新相關聯範圍對應中的許可權。 更新的範圍對應會立即套用至所有相關聯的權杖。 

例如，更新`MyToken-scope-map`存放`content/write` `content/read` `samples/alpine`庫上的 with 和動作，並移除存放`content/write` `samples/hello-world`庫上的動作。  

若要使用 Azure CLI，請執行[az acr scope map update][az-acr-scope-map-update]來更新範圍對應：

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/alpine content/write content/read \
  --remove samples/hello-world content/write 
```

在 Azure 入口網站中：

1. 流覽至您的 container registry。
1. 在 [**服務**] 底下，選取 [**範圍對應（預覽）**]，然後選取要更新的範圍對應。
1. 在 [**存放庫** `samples/alpine`] 底下輸入，然後在`content/read` [ `content/write`**許可權**] 下選取 [和]。 然後選取 [ **+ 新增**]。
1. 在 [**存放庫**] 底下，選取`samples/hello-world` [ `content/write`**許可權**] 底下的 []。 然後選取 [儲存]  。

更新範圍對應之後，下列推送會成功：

```bash
docker push myregistry.azurecr.io/samples/alpine:v1
```

因為範圍對應僅具有存放`content/read` `samples/hello-world`庫的許可權，所以對存放庫的推送嘗試`samples/hello-world`現在會失敗：
 
```bash
docker push myregistry.azurecr.io/samples/hello-world:v1
```

從這兩個存放庫提取映射都會成功，因為範圍`content/read`對應會提供這兩個存放庫的許可權：

```bash
docker pull myregistry.azurecr.io/samples/alpine:v1
docker pull myregistry.azurecr.io/samples/hello-world:v1
```
### <a name="delete-images"></a>刪除映像

藉由將`content/delete`動作新增至存放`alpine`庫來更新範圍對應。 此動作可讓您刪除存放庫中的映射，或刪除整個存放庫。

為求簡潔，我們只會顯示[az acr scope-map update][az-acr-scope-map-update]命令來更新範圍對應：

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/alpine content/delete
``` 

若要使用入口網站更新範圍對應，請參閱上一節。

使用下列[az acr repository delete][az-acr-repository-delete]命令來刪除存放`samples/alpine`庫。 若要刪除映射或存放庫，權杖不會`docker login`通過驗證。 相反地，請將權杖的名稱和密碼傳遞給命令。 下列範例會使用稍早在本文中建立的環境變數：

```azurecli
az acr repository delete \
  --name myregistry --repository samples/alpine \
  --username $TOKEN_NAME --password $TOKEN_PWD
```

### <a name="show-repo-tags"></a>顯示存放庫標記 

藉由將`metadata/read`動作新增至存放`hello-world`庫來更新範圍對應。 此動作可讓您讀取存放庫中的資訊清單和標記資料。

為求簡潔，我們只會顯示[az acr scope-map update][az-acr-scope-map-update]命令來更新範圍對應：

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/hello-world metadata/read 
```  

若要使用入口網站更新範圍對應，請參閱上一節。

若要讀取存放`samples/hello-world`庫中的中繼資料，請執行[az acr repository show-資訊清單][az-acr-repository-show-manifests]或[az acr repository show-tags][az-acr-repository-show-tags]命令。 

若要讀取中繼資料，權杖不會`docker login`通過驗證。 相反地，請將權杖的名稱和密碼傳遞至任一命令。 下列範例會使用稍早在本文中建立的環境變數：

```azurecli
az acr repository show-tags \
  --name myregistry --repository samples/hello-world \
  --username $TOKEN_NAME --password $TOKEN_PWD
```

範例輸出：

```console
[
  "v1"
]
```
## <a name="manage-tokens-and-scope-maps"></a>管理權杖和範圍對應

### <a name="list-scope-maps"></a>列出範圍對應

使用 [ [az acr scope-map list][az-acr-scope-map-list] ] 命令，或入口網站中的 [**範圍對應（預覽）** ] 畫面，列出登錄中設定的所有範圍對應。 例如：

```azurecli
az acr scope-map list \
  --registry myregistry --output table
```

輸出會顯示您所定義的範圍對應，以及您可以用來設定權杖的數個系統定義範圍對應：

```
NAME                 TYPE           CREATION DATE         DESCRIPTION
-------------------  -------------  --------------------  ------------------------------------------------------------
_repositories_admin  SystemDefined  2020-01-20T09:44:24Z  Can perform all read, write and delete operations on the ...
_repositories_pull   SystemDefined  2020-01-20T09:44:24Z  Can pull any repository of the registry
_repositories_push   SystemDefined  2020-01-20T09:44:24Z  Can push to any repository of the registry
MyScopeMap           UserDefined    2019-11-15T21:17:34Z  Sample scope map
```

### <a name="show-token-details"></a>顯示權杖詳細資料

若要查看權杖的詳細資料，例如其狀態和密碼到期日，請執行[az acr token show][az-acr-token-show]命令，或在入口網站的 [**權杖（預覽）** ] 畫面中選取權杖。 例如：

```azurecli
az acr scope-map show \
  --name MyScopeMap --registry myregistry
```

使用[az acr token list][az-acr-token-list]命令，或入口網站中的 [**權杖（預覽）** ] 畫面，列出登錄中設定的所有權杖。 例如：

```azurecli
az acr token list --registry myregistry --output table
```

### <a name="generate-passwords-for-token"></a>產生權杖的密碼

如果您沒有權杖密碼，或您想要產生新的密碼，請執行[az acr token credential 產生][az-acr-token-credential-generate]命令。 

下列範例會針對*MyToken*權杖產生 password1 的新值，並在30天內到期。 它會將密碼儲存在環境變數`TOKEN_PWD`中。 這個範例會針對 bash shell 進行格式化。

```azurecli
TOKEN_PWD=$(az acr token credential generate \
  --name MyToken --registry myregistry --days 30 \
  --password1 --query 'passwords[0].value' --output tsv)
```

若要使用 Azure 入口網站來產生權杖密碼，請參閱本文稍早的[建立權杖-入口網站](#create-token---portal)中的步驟。

### <a name="update-token-with-new-scope-map"></a>使用新的範圍對應更新權杖

如果您想要使用不同的範圍對應來更新權杖，請執行[az acr token update][az-acr-token-update] ，並指定新的範圍對應。 例如：

```azurecli
az acr token update --name MyToken --registry myregistry \
  --scope-map MyNewScopeMap
```

在入口網站的 [**權杖（預覽）** ] 畫面上，選取權杖，然後在 [**範圍對應**] 底下，選取不同的範圍對應。

> [!TIP]
> 使用新的範圍對應來更新權杖之後，您可能會想要產生新的權杖密碼。 使用[az acr token credential [產生][az-acr-token-credential-generate]] 命令，或在 Azure 入口網站中重新產生權杖密碼。

## <a name="disable-or-delete-token"></a>停用或刪除權杖

您可能需要暫時停用使用者或服務的權杖認證。 

使用 Azure CLI，執行[az acr token update][az-acr-token-update]命令，將設定`status`為： `disabled`

```azurecli
az acr token update --name MyToken --registry myregistry \
  --status disabled
```

在入口網站中，選取 [**權杖（預覽）** ] 畫面中的權杖，然後選取 [**狀態**] 下的 [**停用**]。

若要刪除權杖以永久讓任何人使用其認證來使存取失效，請執行[az acr token delete][az-acr-token-delete]命令。 

```azurecli
az acr token delete --name MyToken --registry myregistry
```

在入口網站中，選取 [**權杖（預覽）** ] 畫面中的權杖，然後選取 [**捨棄**]。

## <a name="next-steps"></a>後續步驟

* 若要管理範圍對應和權杖，請使用[az acr 範圍對應][az-acr-scope-map]和[az acr token][az-acr-token]命令群組中的其他命令。
* 請參閱[驗證概述](container-registry-authentication.md)，以瞭解使用 Azure container registry 進行驗證的其他選項，包括使用 Azure Active Directory 身分識別、服務主體或系統管理員帳戶。


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-repository]: /cli/azure/acr/repository/
[az-acr-repository-show-tags]: /cli/azure/acr/repository/#az-acr-repository-show-tags
[az-acr-repository-show-manifests]: /cli/azure/acr/repository/#az-acr-repository-show-manifests
[az-acr-repository-delete]: /cli/azure/acr/repository/#az-acr-repository-delete
[az-acr-scope-map]: /cli/azure/acr/scope-map/
[az-acr-scope-map-create]: /cli/azure/acr/scope-map/#az-acr-scope-map-create
[az-acr-scope-map-list]: /cli/azure/acr/scope-map/#az-acr-scope-map-show
[az-acr-scope-map-show]: /cli/azure/acr/scope-map/#az-acr-scope-map-list
[az-acr-scope-map-update]: /cli/azure/acr/scope-map/#az-acr-scope-map-update
[az-acr-scope-map-list]: /cli/azure/acr/scope-map/#az-acr-scope-map-list
[az-acr-token]: /cli/azure/acr/token/
[az-acr-token-show]: /cli/azure/acr/token/#az-acr-token-show
[az-acr-token-list]: /cli/azure/acr/token/#az-acr-token-list
[az-acr-token-delete]: /cli/azure/acr/token/#az-acr-token-delete
[az-acr-token-create]: /cli/azure/acr/token/#az-acr-token-create
[az-acr-token-update]: /cli/azure/acr/token/#az-acr-token-update
[az-acr-token-credential-generate]: /cli/azure/acr/token/credential/#az-acr-token-credential-generate
