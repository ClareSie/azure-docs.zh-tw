---
title: CLI 指令碼 - 下載慢速查詢記錄 - 適用於 MySQL 的 Azure 資料庫
description: 此範例 Azure CLI 指令碼示範如何啟用並下載「適用於 MySQL 的 Azure 資料庫」伺服器的伺服器記錄。
author: savjani
ms.author: pariks
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 28dbf51fa58dfdfc375bf8f44964a6b66db008e8
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2020
ms.locfileid: "94539153"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>使用 Azure CLI 啟用並下載「適用於 MySQL 的 Azure 資料庫」伺服器的伺服器慢速查詢記錄
此範例 CLI 指令碼會啟用並下載單一「適用於 MySQL 的 Azure 資料庫」伺服器的慢速查詢記錄。

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- 本文需要 2.0 版或更新版本的 Azure CLI。 如果您是使用 Azure Cloud Shell，就已安裝最新版本。 

## <a name="sample-script"></a>範例指令碼
在此範例指令碼中，請編輯醒目提示的命令列，將系統管理員使用者名稱和密碼更新為您自己的使用者名稱和密碼。 請以您自己的伺服器記錄檔名稱取代 `az monitor` 命令中的 &lt;log_file_name&gt;。
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>清除部署
執行指令碼之後，請使用下列命令來移除資源群組及其相關聯的所有資源。 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>指令碼說明
此指令碼會使用下表中簡述的命令：

| **命令** | **注意事項** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | 建立用來存放所有資源的資源群組。 |
| [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create) | 建立主控資料庫的 MySQL 伺服器。 |
| [az mysql server configuration list](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) | 列出伺服器的設定值。 |
| [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) | 更新伺服器的設定。 |
| [az mysql server-logs list](/cli/azure/mysql/server-logs#az-mysql-server-logs-list) | 列出伺服器的記錄檔。 |
| [az mysql server-logs download](/cli/azure/mysql/server-logs#az-mysql-server-logs-download) | 下載記錄檔。 |
| [az group delete](/cli/azure/group#az-group-delete) | 刪除資源群組，包括所有的巢狀資源。 |

## <a name="next-steps"></a>後續步驟
- 了解 Azure CLI 的詳細資訊：[Azure CLI 文件](/cli/azure)。
- 嘗試額外的指令碼：[Azure Database for MySQL 的 Azuer CLI 範例](../sample-scripts-azure-cli.md)
