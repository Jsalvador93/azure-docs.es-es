---
title: Conceptos de arquitectura en Azure IoT Central | Microsoft Docs
description: En este artículo se presentan conceptos clave relacionados con la arquitectura de Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 43357bdeb444fed20f29107d10dc31a61857fccf
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877503"
---
# <a name="azure-iot-central-architecture"></a>Arquitectura de Azure IoT Central

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

En este artículo se proporciona información general sobre la arquitectura de Microsoft Azure IoT Central.

![Arquitectura de nivel superior](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Dispositivos

Los dispositivos intercambian datos con la aplicación Azure IoT Central. Un dispositivo puede:

- Enviar las mediciones como telemetría.
- Sincronizar la configuración con la aplicación.

En Azure IoT Central, los datos que un dispositivo puede intercambiar con la aplicación se especifican en una plantilla de dispositivo. Para obtener más información acerca de las plantillas de dispositivo, consulte [Metadata management](#metadata-management) (Administración de metadatos).

Para obtener más información sobre cómo se conectan los dispositivos a la aplicación de Azure IoT Central, consulte [Device connectivity](concepts-connectivity.md) (Conectividad de dispositivos).

## <a name="cloud-gateway"></a>Puerta de enlace en la nube

Azure IoT Central usa Azure IoT Hub como una puerta de enlace en la nube que permite la conectividad del dispositivo. IoT Hub permite lo siguiente:

- Ingesta de datos a escala en la nube.
- Administración de dispositivos.
- Conectividad de dispositivos segura.

Para más información acerca de IoT Hub, consulte [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

Para obtener más información sobre la conectividad de dispositivos en Azure IoT Central, consulte [Device connectivity](concepts-connectivity.md) (Conectividad de dispositivos).

## <a name="data-stores"></a>Almacenes de datos

Azure IoT Central almacena los datos de la aplicación en la nube. Entre los datos de aplicación almacenados están los siguientes:

- Plantillas del dispositivo.
- Identidades del dispositivo.
- Metadatos del dispositivo.
- Datos de usuario y rol.

Azure IoT Central utiliza un almacén en serie temporal para los datos de medición enviados desde los dispositivos. Los datos de serie temporal de dispositivos usados por el servicio de análisis.

## <a name="analytics"></a>Análisis

El servicio de análisis es responsable de generar los datos de informes personalizados que muestra la aplicación. Un operador puede [personalizar el análisis](howto-create-analytics.md) mostrado en la aplicación. El servicio de análisis se basa en [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) y procesa los datos de medición enviados desde los dispositivos.

## <a name="rules-and-actions"></a>Reglas y acciones

Las [reglas y acciones](howto-create-telemetry-rules.md) trabajan conjuntamente para automatizar las tareas dentro de la aplicación. Un generador puede definir reglas basadas en la telemetría de dispositivo, como la temperatura que supera un umbral definido. Azure IoT Central usa un procesador de secuencias para determinar cuando se cumplen las condiciones de la regla. Si se cumple una condición de regla, desencadena una acción definida por el generador. Por ejemplo, una acción puede enviar un correo electrónico para notificar a un ingeniero que la temperatura en un dispositivo es demasiado alta.

## <a name="metadata-management"></a>Administración de metadatos

En una aplicación de Azure IoT Central, las plantillas de dispositivo definen el comportamiento y la capacidad de los tipos de dispositivo. Por ejemplo, una plantilla de dispositivo de un refrigerador especifica la telemetría que un refrigerador envía a la aplicación.

![Arquitectura de plantillas](media/concepts-architecture/template_architecture.png)

En la plantilla de un dispositivo:

- Las **medidas** especifican la telemetría que el dispositivo envía a la aplicación.
- La **configuración** especifica los parámetros que puede establecer un operador.
- Las **propiedades** especifican los metadatos que puede establecer un operador.
- Las **reglas** automatizan el comportamiento de la aplicación en función de los datos enviados desde un dispositivo.
- Los **paneles** son vistas personalizables de un dispositivo en la aplicación.

Una aplicación puede tener uno o varios dispositivos simulados y reales basados en cada plantilla de dispositivo.

## <a name="data-export"></a>Exportación de datos

En una aplicación de Azure IoT Central, puede [exportar continuamente los datos](howto-export-data-event-hubs-service-bus.md) a sus propias instancias de Azure Event Hubs y de Azure Service Bus. También puede exportar los datos periódicamente a la cuenta de Azure Blob Storage. IoT Central puede exportar medidas, dispositivos y plantillas de dispositivo.

## <a name="batch-device-updates"></a>Actualizaciones de dispositivos en lote

En una aplicación de Azure IoT Central, puede [crear y ejecutar trabajos](howto-run-a-job.md) para administrar los dispositivos conectados. Estos trabajos le permiten realizar actualizaciones masivas de los comandos, las configuraciones y propiedades de los dispositivos. Por ejemplo, puede crear un trabajo para aumentar la velocidad del ventilador de varias máquinas expendedoras refrigeradas.

## <a name="role-based-access-control-rbac"></a>Control de acceso basado en roles (RBAC)

Un [administrador puede definir reglas de acceso](howto-administer.md) para una aplicación de Azure IoT Central con los roles predefinidos. Un administrador puede asignar usuarios a roles que determinan a qué áreas de la aplicación tiene acceso el usuario.

## <a name="security"></a>Seguridad

Entre las características de seguridad de Azure IoT Central están las siguientes:

- Los datos se cifran en tránsito y en reposo.
- La autenticación se proporciona por Azure Active Directory o la cuenta de Microsoft. Se admite la autenticación de dos factores.
- Aislamiento de inquilino completo.
- Seguridad a nivel de dispositivo.

## <a name="ui-shell"></a>Shell de IU

El shell de la interfaz de usuario es una a aplicación moderna, con capacidad de respuesta y basada en un explorador HTML5.
Un administrador puede personalizar la interfaz de usuario de la aplicación mediante la aplicación de temas personalizados y la modificación de los vínculos de ayuda para que apunten a sus propios recursos de ayuda personalizados. Para más información sobre la interfaz de usuario personalizada, consulte el artículo [Personalizar la interfaz de usuario de Azure IoT Central](howto-customize-ui.md).

Un operador puede crear paneles de aplicaciones personalizados. Puede tener varios paneles que muestren datos diferentes y cambiar entre ellos.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido sobre la arquitectura de Azure IoT Central, el siguiente paso sugerido es obtener información sobre [la conectividad de dispositivos](concepts-connectivity.md) en Azure IoT Central.