---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/12/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1c2525b352c25f470814ce909a8d10ff821d9e32
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2020
ms.locfileid: "70961606"
---
產生 CA 憑證。

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```

列印 base64 格式的 CA 憑證。 這是 Azure 支援的格式。 您會在[P2S 設定步驟](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)中，將此憑證上傳至 Azure。

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```

產生使用者憑證。

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```

產生包含使用者憑證的 p12 組合。 在後續使用用戶端組態檔的步驟中將會用到此組合。

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```