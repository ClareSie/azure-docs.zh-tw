---
title: 快速入門：建立 Linux Java 應用程式
description: 藉由將您的第一個 Java 應用程式部署至 App Service 中的 Linux 容器，在 Azure App Service 上開始使用 Linux 應用程式。
keywords: Azure, App Service, Web 應用程式, Linux, Java, Maven, 快速入門
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/27/2019
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 2192260c3106a6b8d3d3ffd0e063ee945734f308
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "79136307"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-linux"></a>快速入門：在 Linux 上的 Azure App Service 中建立 Java 應用程式

[Linux 上的 App Service](app-service-linux-intro.md) 使用 Linux 作業系統提供可高度擴充、自我修復的 Web 主機服務。 本快速入門說明如何使用 [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) 搭配[適用於 Maven 的 Azure Web 應用程式外掛程式](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)，在 Linux 作業系統上部署 Java Web 封存 (WAR) 檔案。

> [!NOTE]
>
> 使用 IntelliJ、Eclipse 和 VS Code 等熱門 IDE 也可以執行相同的動作。 請參閱 [Azure Toolkit for IntelliJ 快速入門](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app)、[Azure Toolkit for Eclipse 快速入門](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app)或 [VS Code 快速入門](https://code.visualstudio.com/docs/java/java-webapp)中的類似文件。
>
![在 Azure App Service 中執行的範例應用程式](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>建立 Java 應用程式

在 Cloud Shell 提示字元中執行下列 Maven 命令，以建立名為 `helloworld` 的新應用程式：

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp"
```
然後將您的工作目錄變更為專案資料夾：

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>設定 Maven 外掛程式

部署到 Azure App Service 的程序從 Azure CLI 使用帳戶認證。 繼續執行前，請[使用 Azure CLI 登入](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)。

```azurecli
az login
```

您可以設定部署，在命令提示字元中執行 maven 命令，並按 **ENTER** 鍵使用預設組態，直到您看到**確認 (是/否)** 提示，然後按 **'y'** 完成設定。 
```cmd
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.0:config
```
範例程序如下所示：

```cmd
~@Azure:~/helloworld$ mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.0:config
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------------< example.demo:helloworld >-----------------------
[INFO] Building helloworld Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.9.0:config (default-cli) @ helloworld ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. Java 11
2. Java 8 [*]
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. TOMCAT 8.5 [*]
Enter index to use:
Please confirm webapp properties
AppName : helloworld-1558400876966
ResourceGroup : helloworld-1558400876966-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : TOMCAT 8.5-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

> [!NOTE]
> 在本文中，我們只會使用封裝在 WAR 檔案中的 JAVA 應用程式。 此外掛程式也支援 JAR Web 應用程式，請瀏覽[將 Java SE JAR 檔案部署至 Linux 上的 App Service](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)，並加以試用。

再次瀏覽至 `pom.xml` 以查看外掛程式組態是否已更新，如有需要，您可以直接在 pom 檔案中修改 App Service 的其他組態，以下列出一些常見的組態：

 屬性 | 必要 | 描述 | 版本
---|---|---|---
`<schemaVersion>` | false | 指定組態結構描述的版本。 支援的值包括：`v1`、`v2`。 | 1.5.2
`<resourceGroup>` | true | Web 應用程式的 Azure 資源群組。 | 0.1.0+
`<appName>` | true | 您的 Web 應用程式名稱。 | 0.1.0+
`<region>` | true | 指定將裝載 Web 應用程式的區域；預設值為 **westeurope**。 所有有效的區域皆列於[支援的區域](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)一節。 | 0.1.0+
`<pricingTier>` | false | 您 Web 應用程式的定價層。 預設值為 **P1V2**。| 0.1.0+
`<runtime>` | true | 執行階段環境組態，您可以在[此處](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)查看詳細資料。 | 0.1.0+
`<deployment>` | true | 部署組態，您可以在[此處](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)查看詳細資料。 | 0.1.0+

> [!div class="nextstepaction"]
> [我遇到問題](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=config)

## <a name="deploy-the-app"></a>部署應用程式

使用下列命令將您的 Java 應用程式部署至 Azure：

```bash
mvn package azure-webapp:deploy
```

完成部署後，在網頁瀏覽器中使用下列 URL，瀏覽至已部署的應用程式，例如 `http://<webapp>.azurewebsites.net`。 

![在 Azure App Service 中執行的範例應用程式](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**恭喜！** 您已將第一個 Java 應用程式部署至 Linux 上的 App Service。

> [!div class="nextstepaction"]
> [我遇到問題](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=deploy)

## <a name="clean-up-resources"></a>清除資源

在前述步驟中，您在資源群組中建立了 Azure 資源。 如果您在未來不需要這些資源，請從入口網站刪除資源群組，或在 Cloud Shell 中執行下列命令：

```azurecli-interactive
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

此命令可能會花一分鐘執行。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [使用 Java 連線至 Azure SQL 資料庫](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [使用 Java 連線至適用於 MySQL 的 Azure DB](/azure/mysql/connect-java?toc=/azure/java/toc.json)

> [!div class="nextstepaction"]
> [使用 Java 連線至適用於 PostgreSQL 的 Azure DB](/azure/postgresql/connect-java?toc=/azure/java/toc.json)

> [!div class="nextstepaction"]
> [設定 Java 應用程式](configure-custom-container.md)

> [!div class="nextstepaction"]
> [使用 Jenkins 的 CI/CD](/azure/jenkins/deploy-jenkins-app-service-plugin)

> [!div class="nextstepaction"]
> [適用於 Java 開發人員的其他 Azure 資源](/java/azure/)

> [!div class="nextstepaction"]
> [深入了解 Azure 的 Maven 外掛程式](https://github.com/microsoft/azure-maven-plugins)
