---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: f771b6b0416c5777c1ebde7e2cf2c4ffc6f375ff
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155297"
---
Los errores que se producen en una instancia de Azure Functions pueden provenir de cualquiera de los orígenes siguientes:

- Uso de [desencadenadores y enlaces](..\articles\azure-functions\functions-triggers-bindings.md) de Azure Functions integrados
- Llamadas a las API de los servicios de Azure subyacentes
- Llamadas a puntos de conexión REST
- Llamadas a bibliotecas de cliente, paquetes o API de terceros

Seguir las siguientes prácticas de control de errores sólidas es importantes para evitar la pérdida de datos o mensajes perdidos. Entre las prácticas recomendadas de control de errores se incluyen las acciones siguientes:

- [Habilitación de Application Insights](../articles/azure-functions/functions-monitoring.md)
- [Uso del control de errores estructurado](#use-structured-error-handling)
- [Diseño para idempotencia](../articles/azure-functions/functions-idempotent.md)
- Implementar directivas de reintentos (si procede)

### <a name="use-structured-error-handling"></a>Uso del control de errores estructurado

La captura y publicación de los errores es fundamental para supervisar el estado de la aplicación. El nivel superior de cualquier código de función debe incluir un bloque try/catch. En el bloque catch, puede capturar y publicar errores.

### <a name="retry-support"></a>Compatibilidad con los reintentos

Los siguientes desencadenadores admiten el reintento integrado:

* [Almacenamiento de blobs de Azure](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure Queue Storage](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Service Bus (cola/tema)](../articles/azure-functions/functions-bindings-service-bus.md)

De forma predeterminada, estos desencadenadores reintentan las solicitudes hasta cinco veces. Después del quinto reintento, ambos desencadenadores escriben un mensaje en una [cola de mensajes dudosos](..\articles\azure-functions\functions-bindings-storage-queue.md#trigger---poison-messages).

Debe implementar manualmente las directivas de reintento para cualquier otro desencadenador o tipo de enlace. Las implementaciones manuales pueden incluir la escritura de información de error en una [cola de mensajes dudosos](..\articles\azure-functions\functions-bindings-storage-blob.md#trigger---poison-blobs). Al escribir en una cola de mensajes dudosos, tiene la oportunidad de reintentar las operaciones más adelante. El desencadenador de Blob Storage usa este mismo enfoque.
