---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: f9b1b32775393196a77bef3aa2257f6fb66afdcb
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012276"
---
> [!NOTE]
> Las solicitudes de varias partes normalmente requieren tres elementos:
> * Un encabezado **Content-Type**:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Un elemento **Content-Disposition**:
>   * `form-data; name="metadata"`
> * El contenido del archivo que se va a cargar
>
> **Content-Type** y **Content-Disposition** variarán en función del escenario de uso.

Se pueden realizar solicitudes de varias partes mediante programación (a través de C#), a través de un cliente de REST o de una herramienta como [Postman](https://docs.microsoft.com/azure/digital-twins/how-to-configure-postman#multi). Las herramientas de cliente REST pueden tener distintos niveles de compatibilidad de las solicitudes complejas con varias partes. Las opciones de configuración también pueden variar ligeramente de una herramienta a otra. Compruebe qué herramienta se adapta mejor a sus necesidades.

> [!IMPORTANT]
> Las solicitudes de varias partes realizadas a las API de administración de Azure Digital Twins normalmente tienen dos partes:
> * los metadatos del blob (como un tipo MIME asociado) declarado por **Content-Type** o **Content-Disposition**.
> * Contenido del blob que incluye el contenido no estructurado de un archivo que se cargará
>
> Ninguna de las dos partes es necesaria para las solicitudes **PATCH**. Ambos son necesarios para las operaciones de creación o **POST**.

El [código fuente de inicio rápido de ocupación](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/api/update.cs) contiene completos ejemplos de C# que muestran cómo realizar solicitudes de varias partes en las API de administración de Azure Digital Twins.