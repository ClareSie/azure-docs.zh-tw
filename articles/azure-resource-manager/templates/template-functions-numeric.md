---
title: 範本函數 - 數位
description: 描述 Azure Resource Manager 範本中用來使用數值的函式。
ms.topic: conceptual
ms.date: 11/08/2017
ms.openlocfilehash: 2ca5c539036d002b83b8141132a0ebf2530dc6af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156339"
---
# <a name="numeric-functions-for-arm-templates"></a>ARM 範本的數位函數

資源管理器提供了以下函數，用於在 Azure 資源管理器 （ARM） 範本中處理整數：

* [新增](#add)
* [複製索引](#copyindex)
* [div](#div)
* [浮動](#float)
* [Int](#int)
* [麥克斯](#max)
* [分鐘](#min)
* [國防部](#mod)
* [mul](#mul)
* [sub](#sub)

<a id="add" />

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="add"></a>add
`add(operand1, operand2)`

傳回兩個所提供整數加總後的數字。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
|operand1 |是 |int |要新增的第一個數字。 |
|operand2 |是 |int |要新增的第二個數字。 |

### <a name="return-value"></a>傳回值

整數，其中包含參數的總和。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json)會新增兩個參數。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| addResult | Int | 8 |

若要使用 Azure CLI 部署此範例範本，請使用：

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json
```

若要使用 PowerShell 部署此範例範本，請使用：

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json
```

<a id="copyindex" />

## <a name="copyindex"></a>copyIndex
`copyIndex(loopName, offset)`

傳回反覆項目迴圈的索引。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| loopName | 否 | 字串 | 用來取得反覆項目的迴圈名稱。 |
| Offset |否 |int |要加入到以零為起始之反覆項目值的數字。 |

### <a name="remarks"></a>備註

這個函式一律搭配 **copy** 物件使用。 如果未針對 **offset** 提供任何值，則會傳回目前的反覆項目值。 反覆項目值是從零開始。 定義資源或變數時，您可以使用反覆項目迴圈。

**LoopName** 屬性可讓您指定 copyIndex 要參考資源的反覆項目還是屬性的反覆項目。 如果未提供任何 **loopName** 的值，就會使用目前的資源類型反覆項目。 逐一查看屬性時，請提供 **loopName** 的值。

如需如何使用 **copyIndex**的完整範例，請參閱 [在 Azure Resource Manager 中建立資源的多個執行個體](copy-resources.md)。

如需使用 **copyIndex** 的範例，請參閱[變數](template-syntax.md#variables)。

### <a name="example"></a>範例

下列範例顯示複製迴圈以及名稱中所包含的索引值。

```json
"resources": [
  {
    "name": "[concat('examplecopy-', copyIndex())]",
    "type": "Microsoft.Web/sites",
    "copy": {
      "name": "websitescopy",
      "count": "[parameters('count')]"
    },
    ...
  }
]
```

### <a name="return-value"></a>傳回值

整數，代表目前的反覆項目索引。

<a id="div" />

## <a name="div"></a>div
`div(operand1, operand2)`

傳回兩個所提供整數相除後的商。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| operand1 |是 |int |被除數。 |
| operand2 |是 |int |除數。 不能為 0。 |

### <a name="return-value"></a>傳回值

代表除法的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json)會使用一個參數除以另一個參數。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| divResult | Int | 2 |

若要使用 Azure CLI 部署此範例範本，請使用：

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json
```

若要使用 PowerShell 部署此範例範本，請使用：

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json
```

<a id="float" />

## <a name="float"></a>FLOAT
`float(arg1)`

將值轉換為浮點數。 您只會在將自訂參數傳遞給應用程式 (例如邏輯應用程式) 時使用這個函式。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |字串或整數 |要轉換為浮點數的值。 |

### <a name="return-value"></a>傳回值
浮點數。

### <a name="example"></a>範例

下列範例顯示如何使用 float 將參數傳遞給邏輯應用程式：

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
            "custom1": {
                "value": "[float('3.0')]"
            },
            "custom2": {
                "value": "[float(3)]"
            },
```

<a id="int" />

## <a name="int"></a>int
`int(valueToConvert)`

將指定的值轉換成整數。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| valueToConvert |是 |字串或整數 |要轉換成整數的值。 |

### <a name="return-value"></a>傳回值

轉換值的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json)會將使用者提供的參數值轉換為整數。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": {
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| intResult | Int | 4 |

若要使用 Azure CLI 部署此範例範本，請使用：

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

若要使用 PowerShell 部署此範例範本，請使用：

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

<a id="max" />

## <a name="max"></a>max
`max (arg1)`

傳回整數陣列的最大值，或以逗號分隔的整數清單。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |整數的陣列，或以逗號分隔的整數清單 |要用來取得最大值的集合。 |

### <a name="return-value"></a>傳回值

整數，代表集合中的最大值。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json)顯示如何搭配使用 max 與陣列和整數清單：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

若要使用 Azure CLI 部署此範例範本，請使用：

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

若要使用 PowerShell 部署此範例範本，請使用：

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

<a id="min" />

## <a name="min"></a>Min
`min (arg1)`

傳回整數陣列的最小值，或以逗號分隔的整數清單。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |整數的陣列，或以逗號分隔的整數清單 |要用來取得最小值的集合。 |

### <a name="return-value"></a>傳回值

整數，代表集合中的最小值。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json)顯示如何搭配使用 min 與陣列和整數清單：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

若要使用 Azure CLI 部署此範例範本，請使用：

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

若要使用 PowerShell 部署此範例範本，請使用：

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

<a id="mod" />

## <a name="mod"></a>mod
`mod(operand1, operand2)`

傳回兩個所提供整數相除後的餘數。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| operand1 |是 |int |被除數。 |
| operand2 |是 |int |除數，不能為 0。 |

### <a name="return-value"></a>傳回值
代表餘數的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json)傳回的是一個參數除以另一個參數的餘數。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| modResult | Int | 1 |

若要使用 Azure CLI 部署此範例範本，請使用：

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

若要使用 PowerShell 部署此範例範本，請使用：

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

<a id="mul" />

## <a name="mul"></a>mul
`mul(operand1, operand2)`

傳回兩個所提供整數相乘後的數字。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| operand1 |是 |int |要相乘的第一個數字。 |
| operand2 |是 |int |要相乘的第二個數字。 |

### <a name="return-value"></a>傳回值

代表乘法的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json)會使用一個參數乘以另一個參數。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

若要使用 Azure CLI 部署此範例範本，請使用：

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

若要使用 PowerShell 部署此範例範本，請使用：

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

<a id="sub" />

## <a name="sub"></a>sub
`sub(operand1, operand2)`

傳回兩個所提供整數相減後的數字。

### <a name="parameters"></a>參數

| 參數 | 必要 | 類型 | 描述 |
|:--- |:--- |:--- |:--- |
| operand1 |是 |int |減數。 |
| operand2 |是 |int |被減數。 |

### <a name="return-value"></a>傳回值
代表減法的整數。

### <a name="example"></a>範例

下列[範例範本](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json)會使用一個參數乘以另一個參數。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer to subtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

上述範例中具有預設值的輸出如下：

| 名稱 | 類型 | 值 |
| ---- | ---- | ----- |
| subResult | Int | 4 |

若要使用 Azure CLI 部署此範例範本，請使用：

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

若要使用 PowerShell 部署此範例範本，請使用：

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

## <a name="next-steps"></a>後續步驟
* 有關 Azure 資源管理器範本中部分的說明，請參閱[創作 Azure 資源管理器範本](template-syntax.md)。
* 要合併多個範本，請參閱[使用 Azure 資源管理器使用連結範本](linked-templates.md)。
* 要反覆運算創建資源類型時指定的次數，請參閱[在 Azure 資源管理器中創建多個資源實例](copy-resources.md)。
* 要查看如何部署已創建的範本，請參閱使用 Azure[資源管理器範本部署應用程式](deploy-powershell.md)。

