---
title: Supervisión de las métricas de rendimiento y almacenamiento en Azure Cosmos DB
description: Obtenga información sobre cómo supervisar la cuenta de Azure Cosmos DB para aplicar métricas de rendimiento, como solicitudes y errores de servidor, y métricas de uso, como consumo de almacenamiento.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: d8e80594e0c56f57527d1703b0cf1323571cf351
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855706"
---
# <a name="monitor-performance-and-storage-metrics-in-azure-cosmos-db"></a>Supervisión de las métricas de rendimiento y almacenamiento en Azure Cosmos DB

Puede supervisar las cuentas de Azure Cosmos DB en [Azure Portal](https://portal.azure.com/). Para cada cuenta de Azure Cosmos DB, está disponible un conjunto completo de métricas para supervisar el rendimiento, almacenamiento, disponibilidad, latencia y coherencia.

Las métricas pueden revisarse en la página Cuenta, en la nueva página Métricas o en Azure Monitor.

## <a name="view-performance-metrics-on-the-metrics-page"></a>Visualización de las métricas de rendimiento en la página Métricas
1. En [Azure Portal](https://portal.azure.com/), haga clic en **Todos los servicios**, desplácese a **Bases de datos**, y haga clic en **Azure Cosmos DB** y en el nombre de la cuenta de Cosmos DB de la que quiere ver las métricas de rendimiento.
2. Cuando se cargue la nueva página, en el menú de recursos, en **Supervisión**, haga clic en **Métricas**.
3. Cuando se abre la página Métricas, seleccione la colección para revisar desde la lista desplegable **Colecciones**.

   Azure Portal muestra el conjunto disponible de métricas de la colección. Tenga en cuenta que se proporcionan las métricas de rendimiento, almacenamiento, disponibilidad, latencia y coherencia en pestañas independientes. Para obtener detalles adicionales de las métricas proporcionadas, haga clic en la flecha doble en la parte superior derecha de cada panel de métricas.

   ![Captura de pantalla de la lente de supervisión que muestra el conjunto de métricas](./media/monitor-accounts/metrics-suite.png)

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Visualice las métricas de rendimiento mediante el uso de supervisión de Azure
1. En [Azure Portal](https://portal.azure.com/), haga clic en **Supervisar** en la barra izquierda.
2. En el menú de recursos, haga clic en **Métricas**.
3. En la ventana **Supervisar: métricas**, en el menú desplegable **Grupo de recursos**, seleccione el grupo de recursos asociado a la cuenta de Azure Cosmos DB que le gustaría supervisar. 
4. En el menú de lista desplegable **Recursos**, seleccione la base de datos de la cuenta que desee supervisar.
5. En la lista de **Métricas disponibles**, seleccione las métricas que desee mostrar. Utilice la tecla CTRL para seleccionar varios elementos. 

## <a name="view-performance-metrics-on-the-account-page"></a>Visualización de métricas de rendimiento en la página Cuenta
1. En [Azure Portal](https://portal.azure.com/), haga clic en **Todos los servicios**, desplácese a **Bases de datos**, y haga clic en **Azure Cosmos DB** y en el nombre de la cuenta de Cosmos DB de la que quiere ver las métricas de rendimiento.
2. La lente **Supervisión** muestra los iconos siguientes de forma predeterminada:
   
   * El total de solicitudes del día actual.
   * Almacenamiento utilizado.
   
   ![Captura de pantalla de la lente de supervisión que muestra las solicitudes y el uso de almacenamiento](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Al hacer clic en la flecha doble en la parte superior derecha del icono **Solicitudes** se abre la página de detalle **Métrica**.
4. La página **Métrica** muestra los detalles sobre el total de solicitudes. 

## <a name="set-up-alerts-in-the-portal"></a>Configuración de alertas en el portal

> [!NOTE]
> Las alertas clásicas en Azure Monitor se retirarán próximamente. Azure Portal ofrece una [herramienta de migración](../azure-monitor/platform/alerts-using-migration-tool.md) para migrar las reglas de alertas clásicas. Aun así, no se pueden migrar todas las alertas clásicas de métricas de Azure Cosmos DB, ya que hay algunas excepciones. Consulte el artículo de [Azure Monitor](../azure-monitor/platform/alerts-understand-migration.md#cosmos-db-metrics) para obtener una lista de las alertas clásicas que no se pueden migrar. 

1. En [Azure Portal](https://portal.azure.com/), haga clic en **Todos los servicios**, **Azure Cosmos DB** y en el nombre de la cuenta de Azure Cosmos DB para la que quiere configurar las alertas de las métricas de rendimiento.
2. En el menú de recursos, haga clic en **Reglas de alerta** para abrir la página Reglas de alerta.  
   ![Captura de pantalla de la parte de reglas de alerta seleccionada](./media/monitor-accounts/madocdb10.5.png)
3. En la página **Reglas de alerta**, haga clic en **Agregar alerta**.  
   ![Captura de pantalla de la página Reglas de alerta, con el botón Agregar alerta resaltado](./media/monitor-accounts/madocdb11.png)
4. En la página **Agregar una regla de alerta**, especifique:
   
   * El nombre de la regla de alerta que va a configurar.
   * Una descripción de la nueva regla de alerta.
   * La métrica de la regla de alerta.
   * La condición, el umbral y el período que determinan cuándo se activa la alerta. Por ejemplo, un número de errores de servidor mayor que cinco durante los últimos 15 minutos.
   * Si se envía un correo electrónico al administrador del servicio y a los coadministradores cuando la alerta de dispara.
   * Direcciones de correo electrónico adicionales para las notificaciones de alerta.  
     ![Captura de pantalla de la página Agregar una regla de alerta](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programmatically"></a>Supervisión de Azure Cosmos DB mediante programación
Las métricas de nivel de cuenta disponibles en el portal, como el uso de almacenamiento de cuenta y el total de solicitudes, no están disponibles mediante las API de SQL. Sin embargo, puede recuperar datos de uso en el nivel de colección mediante las API de SQL. Para recuperar datos de nivel de colección, haga lo siguiente:

* Para usar la API de REST, [ejecute una operación GET en la colección](https://msdn.microsoft.com/library/mt489073.aspx). La información de cuota y uso de la colección se devuelve en los encabezados x-ms-resource-quota y x-ms-resource-usage de la respuesta.
* Para usar el SDK de .NET, use el método [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx), que devuelve un objeto [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) que contiene varias propiedades de uso, como **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage** y otras más.

Para acceder a métricas adicionales, use el [SDK de Azure Monitor](https://www.nuget.org/packages/Microsoft.Azure.Insights). Se pueden recuperar definiciones de métricas mediante la llamada a:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Las consultas para recuperar métricas individuales utilizan el siguiente formato:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Para obtener más información, consulte [Retrieving Resource Metrics via the Azure Monitor API](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/)(Recuperación de métricas de recursos mediante la API de Azure Monitor). Tenga en cuenta que se ha cambiado el nombre "Azure Insights" a "Azure Monitor".  Esta entrada de blog hace referencia al nombre anterior.

## <a name="next-steps"></a>Pasos siguientes
Para conocer más sobre el planeamiento de la capacidad de Azure Cosmos DB, consulte la [calculadora del planificador de capacidad de Azure Cosmos DB](https://www.documentdb.com/capacityplanner).

