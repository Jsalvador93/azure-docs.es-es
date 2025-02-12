---
title: Programaciones de mantenimiento de Azure (versión preliminar) | Microsoft Docs
description: La programación de mantenimiento permite a los clientes planear los eventos de mantenimiento programados necesarios que el servicio de Azure SQL Data Warehouse usa para implementar nuevas características, revisiones, actualizaciones.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 07/16/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 3875106e8c6301c95bc8d0fbce6a1c0400d07f78
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278125"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Uso de programaciones de mantenimiento para administrar las actualizaciones del servicio y el mantenimiento

Los programas de mantenimiento están ahora disponibles en todas las regiones de Azure SQL Data Warehouse. Esta característica integra las notificaciones de mantenimiento planeado de Service Health, el monitor de comprobación de Resource Health y el servicio de programación de mantenimiento de Azure SQL Data Warehouse.

Use programación de mantenimiento para elegir un período de tiempo en el que sea conveniente recibir las nuevas características, actualizaciones y revisiones. Elija una ventana de mantenimiento principal y una secundaria en un período de siete días. Un ejemplo es una ventana principal de sábado a las 22:00 a domingo a la 01:00, y una ventana secundaria el miércoles de 19:00 a 22:00. Si SQL Data Warehouse no puede realizar el mantenimiento durante la ventana de mantenimiento principal, se intentará nuevamente durante la ventana de mantenimiento secundaria. El mantenimiento del servicio puede producirse durante la ventana principal o la secundaria. Para garantizar una finalización rápida de las operaciones de mantenimiento, DW400(c) y los niveles más bajos de almacenamiento de datos pueden completar el mantenimiento fuera de una ventana de mantenimiento designada.

Todos las instancias de Azure SQL Data Warehouse recién creadas tendrán una programación de mantenimiento definida por el sistema aplicada durante la implementación. La programación se podrá editar tan pronto como se complete la implementación.

Cada ventana de mantenimiento puede ser de tres a ocho horas. El mantenimiento puede producirse en cualquier momento dentro de la ventana. Cuando se inicie el mantenimiento, se cancelarán todas las sesiones activas y se revertirán las transacciones no confirmadas. Experimentará varias pérdidas breves de conectividad a medida que el servicio implementa el nuevo código en el almacenamiento de datos. Se le notificará inmediatamente después de que se haya completado el mantenimiento en el almacenamiento de datos.

Para utilizar esta característica, deberá identificar una ventana principal y una secundaria dentro de intervalos de días separados. Todas las operaciones de mantenimiento deben finalizar dentro de las ventanas de mantenimiento programadas. Sin notificación previa, no se llevará a cabo mantenimiento fuera de las ventanas de mantenimiento especificadas. Si el almacenamiento de datos se pausa durante un período de mantenimiento programado, se actualizará durante la operación de reanudación.  

## <a name="alerts-and-monitoring"></a>Supervisión y alertas

Una integración con las notificaciones de Service Health y el monitor de comprobación de Resource Health permite a los clientes mantenerse informados de la actividad de mantenimiento inminente. La automatización nueva aprovecha las ventajas de Azure Monitor. Puede decidir cómo recibir notificaciones de eventos de mantenimiento inminentes. Asimismo, puede decidir qué flujos automatizados pueden ayudarle a administrar el tiempo de inactividad y minimizar el impacto en sus operaciones.

A todos los eventos de mantenimiento les precede una notificación con 24 horas de adelanto, con la excepción actual de DW400c y los niveles más bajos. Para minimizar el tiempo de inactividad de la instancia, debe asegurarse de que no hay transacciones de larga ejecución en el almacenamiento de datos antes del comienzo del período de mantenimiento seleccionado.

> [!NOTE]
> En caso de que sea necesario implementar una actualización donde el tiempo es fundamental, los tiempos de las notificaciones avanzadas pueden reducirse significativamente.

Si recibió una notificación anticipada indicándole que se llevará a cabo el mantenimiento, pero SQL Data Warehouse no puede realizarlo en ese momento, recibirá una notificación de cancelación. Después se reanudará el mantenimiento durante el siguiente período de mantenimiento programado.

Todos los eventos de mantenimiento activos se muestran en la sección **Service Health: mantenimiento planeado**. El historial de Service Health incluye un registro completo de eventos anteriores. Puede supervisar el mantenimiento mediante el panel del portal de comprobación de Azure Service Health durante un evento activo.

### <a name="maintenance-schedule-availability"></a>Disponibilidad de la programación de mantenimiento

Aunque Programación de mantenimiento no esté aún disponible en la región seleccionada, puede ver y editar la programación de mantenimiento en cualquier momento. Cuando Programación de mantenimiento esté disponible en su región, la programación identificada pasará a estar activa inmediatamente en el almacenamiento de datos.

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga más información](viewing-maintenance-schedule.md) sobre cómo ver una programación de mantenimiento.
- [Obtenga más información](changing-maintenance-schedule.md) sobre cómo cambiar una programación de mantenimiento.
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) sobre cómo crear, ver y administrar alertas mediante Azure Monitor.
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre las acciones del webhook para las reglas de alerta de registro.
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) sobre la creación y administración de grupos de acciones.
- [Obtenga más información](https://docs.microsoft.com/azure/service-health/service-health-overview) acerca de Azure Service Health.
