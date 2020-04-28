---
title: 從 Azure IoT Central 規則執行多個動作 |Microsoft Docs
description: 從單一 IoT Central 規則執行多個動作，並建立可重複使用的動作群組，讓您可以從多個規則執行。
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 12/06/2019
ms.topic: how-to
ms.service: iot-central
manager: philmea
ms.openlocfilehash: b447f44d0c95693e560fd5bbfbff8c8daeec964e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "80157682"
---
# <a name="group-multiple-actions-to-run-from-one-or-more-rules"></a>從一或多個規則將多個動作分組以執行

*本文適用于構建者和系統管理員。*

在 Azure IoT Central 中，您會建立規則，以在符合條件時執行動作。 規則是以裝置遙測或事件為基礎。 例如，您可以在裝置溫度超過閾值時通知操作員。 本文說明如何使用[Azure 監視器](../../azure-monitor/overview.md)*動作群組*，將多個動作附加至 IoT Central 規則。 您可以將動作群組附加到多個規則。 [動作群組](../../azure-monitor/platform/action-groups.md)是由 Azure 訂用帳戶的擁有者所定義的通知喜好設定集合。

## <a name="prerequisites"></a>Prerequisites

- 使用標準定價方案建立的應用程式
- 用來建立和管理 Azure 監視器動作群組的 Azure 帳戶和訂用帳戶

## <a name="create-action-groups"></a>建立動作群組

您可以在 Azure 入口網站中，或使用[Azure Resource Manager 範本](../../azure-monitor/platform/action-groups-create-resource-manager-template.md)來[建立和管理動作群組](../../azure-monitor/platform/action-groups.md)。

動作群組可以：

- 傳送電子郵件、SMS 或發出語音通話等通知。
- 執行動作，例如呼叫 webhook。

下列螢幕擷取畫面顯示傳送電子郵件和 SMS 通知並呼叫 webhook 的動作群組：

![動作群組](media/howto-use-action-groups/actiongroup.png)

若要在 IoT Central 規則中使用動作群組，動作群組必須與 IoT Central 應用程式位於相同的 Azure 訂用帳戶中。

## <a name="use-an-action-group"></a>使用動作群組

若要在 IoT Central 應用程式中使用動作群組，請先建立規則。 當您將動作新增至規則時，請選取 [ **Azure 監視器動作群組**]：

![選擇動作](media/howto-use-action-groups/chooseaction.png)

從您的 Azure 訂用帳戶中選擇動作群組：

![選擇動作群組](media/howto-use-action-groups/chooseactiongroup.png)

選取 [儲存]  。 動作群組現在會出現在觸發規則時要執行的動作清單中：

![已儲存動作群組](media/howto-use-action-groups/savedactiongroup.png)

下表摘要說明傳送至支援的動作類型的資訊：

| 動作類型 | 輸出格式 |
| ----------- | -------------- |
| 電子郵件       | 標準 IoT Central 電子郵件範本 |
| sms         | Azure IoT Central 警示： $ {applicationName}-"$ {ruleName}" 已于 $ {triggerDate} $ {triggerTime} 于 "$ {deviceName}" 上觸發 |
| 語音       | Azure triggerDate} $ {triggerTime}，在應用程式 $ {applicationName} 中，已于裝置 "$ {deviceName}" 上觸發規則 "$ {ruleName}" |
| Webhook     | {"schemaId"： "AzureIoTCentralRuleWebhook"，"data"： {[一般 webhook](howto-create-webhooks.md#payload)承載}} |

下列文字是來自動作群組的 SMS 訊息範例：

`iotcentral: Azure IoT Central alert: Contoso - "Low pressure alert" triggered on "Motion sensor 2" at March 20, 2019 10:12 UTC`

## <a name="next-steps"></a>後續步驟

既然您已瞭解如何使用動作群組搭配規則，建議的下一個步驟是瞭解如何[管理您的裝置](howto-manage-devices.md)。
