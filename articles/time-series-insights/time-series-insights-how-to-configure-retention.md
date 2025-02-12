---
title: Configuración de la retención en el entorno de Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo configurar la retención en el entorno de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/09/2019
ms.custom: seodec18
ms.openlocfilehash: e5cc1489af1dce3a9a57b96a3240c63e0395c33a
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947242"
---
# <a name="configuring-retention-in-time-series-insights"></a>Configuración de la retención en Time Series Insights

En este artículo se describe cómo configurar el **Tiempo de retención de datos** y el **Comportamiento correspondiente a un exceso del límite de almacenamiento** en Azure Time Series Insights.

## <a name="summary"></a>Resumen

Cada entorno de Time Series Insights (TSI) tiene una opción para configurar el **tiempo de retención de datos**. El valor abarca de 1 a 400 días. Los datos se eliminan según la capacidad de almacenamiento del entorno o la duración de la retención (1-400), lo que se agote antes.

Cada entorno de TSI tiene una configuración adicional de **Comportamiento correspondiente a un exceso del límite de almacenamiento**. Esta configuración controla el comportamiento de entrada y de purga cuando se alcanza la capacidad máxima de un entorno. Hay dos comportamientos para elegir:

- **Purgar datos antiguos** (valor predeterminado)
- **Pausar entradas**

Para obtener información detallada para comprender mejor estas opciones de configuración, consulte [Understanding retention in Time Series Insights](time-series-insights-concepts-retention.md) (Información sobre la retención en Time Series Insights).  

## <a name="configure-data-retention"></a>Configuración de la retención de datos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Busque su entorno de Time Series Insights existente. Seleccione **Todos los recursos** en el menú izquierdo de Azure Portal. Seleccione el entorno de Time Series Insights.

1. En el encabezado **CONFIGURACIÓN**, haga clic en **Configurar**.

1. Seleccione el **tiempo de retención de datos** para configurar el período de retención mediante la barra del control deslizante o escriba un número en el cuadro de texto.

1. Tenga en cuenta la opción de configuración **Capacidad**, ya que este valor afecta a la cantidad máxima de eventos de datos y la capacidad de almacenamiento total para almacenar los datos.

1. Alterne la opción de configuración **Comportamiento correspondiente a un exceso del límite de almacenamiento**. Seleccione el comportamiento **Purgar datos antiguos** o **Pausar entradas**.

1. Seleccione **Guardar** para configurar los cambios.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información, consulte [Understanding retention in Time Series Insights](time-series-insights-concepts-retention.md) (Información sobre la retención en Time Series Insights).