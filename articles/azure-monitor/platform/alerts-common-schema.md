---
title: Esquema de alertas comunes para las alertas de supervisión de Azure
description: Descripción del esquema de alertas comunes, por qué debería usarlo y cómo habilitarlo
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: 9b142e00543d425b73c4102914bba2dd92c75b8b
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71702922"
---
# <a name="common-alert-schema"></a>Esquema de alertas comunes

Este artículo describe qué es el esquema de alertas comunes, las ventajas de usarlo y cómo habilitarlo.

## <a name="what-is-the-common-alert-schema"></a>¿Qué es el esquema de alertas comunes?

El esquema de alertas comunes estandariza hoy en día la experiencia de consumo para las notificaciones de alerta en Azure. Históricamente, los tres tipos de alerta en Azure actuales (métrica, registro y registro de actividad) han tenido sus propias plantillas de correo electrónico, esquemas de webhook, etc. Con el esquema de alerta comunes, ahora puede recibir notificaciones de alertas con un esquema coherente.

Cualquier instancia de alerta describe **el recurso que resultó afectado** y **la causa de la alerta**, y estas instancias se describen en el esquema común en las secciones siguientes:
* **Información esencial**: Un conjunto de **campos estandarizados** común a todos los tipos de alertas, que describen en **qué recurso** se encuentra la alerta junto con otros metadatos de alerta comunes (por ejemplo, gravedad o descripción). 
* **Contexto de alerta**: Un conjunto de campos que describen la **causa de la alerta**, con campos que varían **según el tipo de alerta**. Por ejemplo, una alerta de métrica tendría campos como el nombre de la métrica y el valor de la métrica en el contexto de la alerta, mientras que una alerta de registro de actividad tendría información sobre el evento que generó la alerta. 

Entre los escenarios de integración típicos que nos cuentan nuestros clientes está el enrutamiento de la instancia de alerta al equipo correspondiente según alguna variable dinámica (por ejemplo, grupo de recursos), después de que el equipo responsable empiece a trabajar en ella. Con el esquema de alertas comunes, puede tener una lógica de enrutamiento estandarizada con tipos de alerta al utilizar los campos esenciales, dejando los campos de contexto tal cual para que los equipos afectados los investiguen en profundidad.

Esto significa que puede tener potencialmente menos integraciones, con lo que el proceso de administrarlas y mantenerlas será una tarea _mucho_ más sencilla. Además, los enriquecimientos de carga de alertas futuras (por ejemplo, personalización, enriquecimiento de diagnóstico, etc.) solo se detectarán en el esquema común.

## <a name="what-enhancements-does-the-common-alert-schema-bring"></a>¿Qué mejoras aporta el esquema de alertas comunes?

El esquema de alertas comunes se manifestará principalmente en las notificaciones de alertas. Las mejoras que verá son las siguientes:

| . | Mejoras|
|:---|:---|
| sms | Una plantilla SMS coherente para todos los tipos de alerta. |
| Email | Una plantilla de correo electrónico detallada y coherente, que le permite diagnosticar fácilmente los problemas de un vistazo. Vínculos profundos insertados en la instancia de alerta en el portal y los recursos afectados garantizan que pueda pasar rápidamente al proceso de corrección. |
| Webhook/Logic App/Azure Functions/Runbook de Automation | Una estructura JSON coherente para todos los tipos de alerta, que le permite crear fácilmente integraciones entre los diferentes tipos de alerta. |

El nuevo esquema también permitirá una experiencia de consumo de alertas más rica tanto en Azure Portal como en Azure Mobile App en el futuro inmediato. 

[Más información sobre las definiciones de esquema para webhooks/Logic Apps/Azure Functions/runbook de Automation.](https://aka.ms/commonAlertSchemaDefinitions)

> [!NOTE]
> Las siguientes acciones no admiten el esquema de alertas comunes: Conector ITSM.

## <a name="how-do-i-enable-the-common-alert-schema"></a>¿Cómo habilito el esquema de alertas comunes?

Puede participar o dejar de participar en el esquema de alertas comunes a través de grupos de acciones, en el portal y la API REST. El botón de alternancia para cambiar al nuevo esquema existe en el nivel de acción. Por ejemplo, debe expresar por separado la intención de recibir una acción de correo electrónico y una acción de webhook.

> [!NOTE]
> 1. Los siguientes tipos de alerta admiten el esquema común de forma predeterminada (no es necesario expresar la participación):
>     * Alertas de detección inteligente
> 1. Los siguientes tipos de alerta no admiten actualmente el esquema común:
>     * Las alertas generadas por [Azure Monitor para VM](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)
>     * Las alertas generadas por [Azure Cost Management](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario)

### <a name="through-the-azure-portal"></a>Mediante Azure Portal

![Participación en el esquema de alertas comunes](media/alerts-common-schema/portal-opt-in.png)

1. Abra una acción existente o una nueva en un grupo de acciones. 
1. Seleccione "Sí" para que el botón de alternancia habilite el esquema de alerta comunes, como se muestra.

### <a name="through-the-action-groups-rest-api"></a>Mediante la API REST de grupos de acciones

También puede usar la [API de grupos de acciones](https://docs.microsoft.com/rest/api/monitor/actiongroups) para participar en el esquema de alertas comunes. Al realizar la llamada de [creación o actualización](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) de la API REST, puede establecer la marca "useCommonAlertSchema" en "true" (para participar) o "false" (para dejar de participar) para cualquiera de las siguientes acciones: correo electrónico/webhook/Logic Apps/función de Azure Functions/runbook de Automation.

Por ejemplo, el siguiente cuerpo de la solicitud realizado en la API REST de [creación o actualización](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) hará lo siguiente:

* Habilitar el esquema de alertas comunes para la acción de correo electrónico "John Doe's email"
* Deshabilitar el esquema de alertas comunes para la acción de correo electrónico "Jane Smith's email"
* Habilitar el esquema de alertas comunes para la acción de webhook "Sample webhook"

```json
{
  "properties": {
    "groupShortName": "sample",
    "enabled": true,
    "emailReceivers": [
      {
        "name": "John Doe's email",
        "emailAddress": "johndoe@email.com",
        "useCommonAlertSchema": true
      },
      {
        "name": "Jane Smith's email",
        "emailAddress": "janesmith@email.com",
        "useCommonAlertSchema": false
      }
    ],
    "smsReceivers": [
      {
        "name": "John Doe's mobile",
        "countryCode": "1",
        "phoneNumber": "1234567890"
      },
      {
        "name": "Jane Smith's mobile",
        "countryCode": "1",
        "phoneNumber": "0987654321"
      }
    ],
    "webhookReceivers": [
      {
        "name": "Sample webhook",
        "serviceUri": "http://www.example.com/webhook",
        "useCommonAlertSchema": true
      }
    ]
  },
  "location": "Global",
  "tags": {}
}
```





## <a name="next-steps"></a>Pasos siguientes

- [Definiciones del esquema de alertas comunes para webhooks/Logic Apps/Azure Functions/runbook de Automation.](https://aka.ms/commonAlertSchemaDefinitions)
- [Obtenga información sobre cómo crear una aplicación lógica que aproveche el esquema común de alertas para controlar todas las alertas.](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-integrations) 



