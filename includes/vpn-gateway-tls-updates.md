---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e4d20cd39d2a843ee1ab57a412ac668b3495fdb1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186139"
---
>[!NOTE]
>A partir del 1 de julio de 2018, se elimina la compatibilidad con TLS 1.0 y 1.1 de Azure VPN Gateway. VPN Gateway será solo compatible con TLS 1.2. Para mantener la compatibilidad, consulte las [actualizaciones para habilitar la compatibilidad con TLS 1.2](#tls1).

Además, los siguientes algoritmos heredados también pasarán a estar en desuso para TLS a partir del 1 de julio de 2018:

* RC4 (Rivest Cipher 4)
* DES (Algoritmo de cifrado de datos)
* 3DES (Triple algoritmo de cifrado de datos)
* MD5 (Código hash 5)

### <a name="tls1"></a>¿Cómo se habilita la compatibilidad con TLS 1.2 en Windows 7 y Windows 8.1?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
