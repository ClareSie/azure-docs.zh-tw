---
title: 傳送 IoT 裝置安全性訊息的 Defender
description: 瞭解如何使用適用于 IoT 的 Defender 來傳送安全性訊息。
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.custom: devx-track-js
ms.openlocfilehash: 5217940a3696bd001db421e61be8313453807c8b
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318512"
---
# <a name="send-security-messages-sdk"></a>傳送安全性訊息 SDK

本操作指南說明當您在不使用 Defender for IoT 代理程式時，選擇收集並傳送裝置安全性訊息時的 Defender for IoT 服務功能，並說明如何進行。

在本指南中，您將了解如何：

> [!div class="checklist"]
> * 使用 Azure IoT C SDK 傳送安全性訊息
> * 使用 Azure IoT c # SDK 傳送安全性訊息
> * 使用 Azure IoT Python SDK 來傳送安全性訊息
> * 使用 Azure IoT Node.js SDK 來傳送安全性訊息
> * 使用 Azure IoT JAVA SDK 來傳送安全性訊息

## <a name="defender-for-iot-capabilities"></a>適用于 IoT 的 Defender 功能

適用于 IoT 的 defender 可以處理及分析任何種類的安全性訊息資料（只要傳送的資料符合 [IoT 架構的 Defender](https://aka.ms/iot-security-schemas) ），訊息就會設定為安全性訊息。

## <a name="security-message"></a>安全性訊息

適用于 IoT 的 Defender 會使用下列準則來定義安全性訊息：

- 如果訊息是使用 Azure IoT SDK 傳送
- 如果訊息符合 [安全性訊息架構](https://aka.ms/iot-security-schemas)
- 如果訊息在傳送之前已設定為安全性訊息

每個安全性訊息都包含寄件者的中繼資料 `AgentId` ，例如、 `AgentVersion` `MessageSchemaVersion` 和安全性事件的清單。
架構會定義包含事件種類之安全性訊息的有效和必要屬性。

> [!NOTE]
> 所傳送的訊息若不符合結構描述，則會遭到忽略。 請務必先確認結構描述再起始資料的傳送，因為系統目前並不會儲存遭到忽略的訊息。

> [!NOTE]
> 使用 Azure IoT SDK 傳送但未設定為安全性訊息的訊息，將不會路由傳送至 Defender for IoT 管線。

## <a name="valid-message-example"></a>有效的訊息範例

下列範例顯示有效的安全性訊息物件。 此範例包含訊息中繼資料和一個 `ProcessCreate` 安全性事件。

一旦設定為安全性訊息並傳送之後，Defender 會針對 IoT 處理此訊息。

```json
"AgentVersion": "0.0.1",
"AgentId": "e89dc5f5-feac-4c3e-87e2-93c16f010c25",
"MessageSchemaVersion": "1.0",
"Events": [
    {
        "EventType": "Security",
        "Category": "Triggered",
        "Name": "ProcessCreate",
        "IsEmpty": false,
        "PayloadSchemaVersion": "1.0",
        "Id": "21a2db0b-44fe-42e9-9cff-bbb2d8fdf874",
        "TimestampLocal": "2019-01-27 15:48:52Z",
        "TimestampUTC": "2019-01-27 13:48:52Z",
        "Payload":
            [
                {
                    "Executable": "/usr/bin/myApp",
                    "ProcessId": 11750,
                    "ParentProcessId": 1593,
                    "UserName": "aUser",
                    "CommandLine": "myApp -a -b"
                }
            ]
    }
]
```

## <a name="send-security-messages"></a>傳送安全性訊息

使用[Azure Iot c 裝置 sdk](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview)、 [Azure iot c # 裝置](https://github.com/Azure/azure-iot-sdk-csharp/tree/preview)sdk、azure iot [Node.js SDK](https://github.com/Azure/azure-iot-sdk-node)、 [Azure IOT Python sdk](https://github.com/Azure/azure-iot-sdk-python)或[azure IoT JAVA sdk](https://github.com/Azure/azure-iot-sdk-java)，在*不*使用 Defender for IoT 代理程式的情況下傳送安全性訊息。

若要從您的裝置傳送裝置資料以供 Defender for IoT 處理，請使用下列其中一個 Api 來標示訊息，以正確地路由傳送至 Defender for IoT 處理管線。

所有傳送的資料（即使以正確的標頭標記）也必須符合 [適用于 IoT 的 Defender 訊息架構](https://aka.ms/iot-security-schemas)。

### <a name="send-security-message-api"></a>傳送安全性訊息 API

**傳送安全性訊息**API 目前以 c 和 c #、Python、Node.js 和 JAVA 提供。

#### <a name="c-api"></a>C API

```c
bool SendMessageAsync(IoTHubAdapter* iotHubAdapter, const void* data, size_t dataSize) {

    bool success = true;
    IOTHUB_MESSAGE_HANDLE messageHandle = NULL;

    messageHandle = IoTHubMessage_CreateFromByteArray(data, dataSize);

    if (messageHandle == NULL) {
        success = false;
        goto cleanup;
    }

    if (IoTHubMessage_SetAsSecurityMessage(messageHandle) != IOTHUB_MESSAGE_OK) {
        success = false;
        goto cleanup;
    }

    if (IoTHubModuleClient_SendEventAsync(iotHubAdapter->moduleHandle, messageHandle, SendConfirmCallback, iotHubAdapter) != IOTHUB_CLIENT_OK) {
        success = false;
        goto cleanup;
    }

cleanup:
    if (messageHandle != NULL) {
        IoTHubMessage_Destroy(messageHandle);
    }

    return success;
}

static void SendConfirmCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback) {
    if (userContextCallback == NULL) {
        //error handling
        return;
    }

    if (result != IOTHUB_CLIENT_CONFIRMATION_OK){
        //error handling
    }
}
```

#### <a name="c-api"></a>C# API

```cs

private static async Task SendSecurityMessageAsync(string messageContent)
{
    ModuleClient client = ModuleClient.CreateFromConnectionString("<connection_string>");
    Message  securityMessage = new Message(Encoding.UTF8.GetBytes(messageContent));
    securityMessage.SetAsSecurityMessage();
    await client.SendEventAsync(securityMessage);
}
```

#### <a name="nodejs-api"></a>Node.js API

```typescript
var Protocol = require('azure-iot-device-mqtt').Mqtt

function SendSecurityMessage(messageContent)
{
  var client = Client.fromConnectionString(connectionString, Protocol);

  var connectCallback = function (err) {
    if (err) {
      console.error('Could not connect: ' + err.message);
    } else {
      var message = new Message(messageContent);
      message.setAsSecurityMessage();
      client.sendEvent(message);
  
      client.on('error', function (err) {
        console.error(err.message);
      });
  
      client.on('disconnect', function () {
        clearInterval(sendInterval);
        client.removeAllListeners();
        client.open(connectCallback);
      });
    }
  };

  client.open(connectCallback);
}
```

#### <a name="python-api"></a>Python API

若要使用 Python API，您需要安裝 [azure iot 裝置](https://pypi.org/project/azure-iot-device/)套件。

使用 Python API 時，您可以使用唯一的裝置或模組連接字串，透過模組或透過裝置來傳送安全性訊息。 當您使用下列 Python 腳本範例時，請使用 **IoTHubDeviceClient**，並使用模組來使用 **IoTHubModuleClient**。

```python
from azure.iot.device.aio import IoTHubDeviceClient, IoTHubModuleClient
from azure.iot.device import Message

async def send_security_message_async(message_content):
    conn_str = os.getenv("<connection_string>")
    device_client = IoTHubDeviceClient.create_from_connection_string(conn_str)
    await device_client.connect()
    security_message = Message(message_content)
    security_message.set_as_security_message()
    await device_client.send_message(security_message)
    await device_client.disconnect()
```

#### <a name="java-api"></a>Java API

```java
public void SendSecurityMessage(string message)
{
    ModuleClient client = new ModuleClient("<connection_string>", IotHubClientProtocol.MQTT);
    Message msg = new Message(message);
    msg.setAsSecurityMessage();
    EventCallback callback = new EventCallback();
    string context = "<user_context>";
    client.sendEventAsync(msg, callback, context);
}
```

## <a name="next-steps"></a>後續步驟

- 閱讀適用于 IoT 的 Defender 服務 [總覽](overview.md)
- 深入瞭解適用于 IoT 的 Defender [架構](architecture.md)
- 啟用[服務](quickstart-onboard-iot-hub.md)
- 閱讀 [常見問題](resources-frequently-asked-questions.md)
- 了解如何存取[未經處理的安全性資料](how-to-security-data-access.md)
- 了解[建議](concept-recommendations.md)
- 了解[警示](concept-security-alerts.md)
