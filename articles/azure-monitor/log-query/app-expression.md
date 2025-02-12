---
title: Expresión app() de las consultas de registros de Azure Monitor | Microsoft Docs
description: La expresión app se usa en una consulta de registro de Azure Monitor para recuperar datos de una aplicación de Application Insights específica en el mismo grupo de recursos, en otro grupo de recursos o en otra suscripción.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/25/2019
ms.author: bwren
ms.openlocfilehash: a1a605bc733597430f64dceeb6c485db0abf657b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60589250"
---
# <a name="app-expression-in-azure-monitor-query"></a>Expresión app() de la consulta de Azure Monitor

La expresión `app` se usa en una consulta de Azure Monitor para recuperar datos de una aplicación de Application Insights específica en el mismo grupo de recursos, en otro grupo de recursos o en otra suscripción. Resulta útil para incluir datos de aplicación en una consulta de registros de Azure Monitor y para consultar datos a través de varias aplicaciones en una consulta de Application Insights.



## <a name="syntax"></a>Sintaxis

`app(`*Identificador*`)`


## <a name="arguments"></a>Argumentos

- *Identificador*: Identifica la aplicación con uno de los formatos de la tabla siguiente.

| Identificador | DESCRIPCIÓN | Ejemplo
|:---|:---|:---|
| Nombre de recurso | Nombre legible de la aplicación (también denominado "nombre del componente") | app("fabrikamapp") |
| Nombre completo | Nombre completo de la aplicación en el formato siguiente: "subscriptionName/resourceGroup/componentName" | app('AI-Prototype/Fabrikam/fabrikamapp') |
| ID | GUID de la aplicación | app("988ba129-363e-4415-8fe7-8cbab5447518") |
| Id. de recurso de Azure | Identificador del recurso de Azure |app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp") |


## <a name="notes"></a>Notas

* Debe tener acceso de lectura a la aplicación.
* En la identificación de una aplicación por su nombre, se da por supuesto que este es único en todas las suscripciones accesibles. Si tiene varias aplicaciones con el nombre especificado, la consulta producirá un error debido a la ambigüedad. En este caso debe usar uno de los otros identificadores.
* Utilice la expresión relacionada [área de trabajo](workspace-expression.md) para hacer consultas entre áreas de trabajo de Log Analytics.
* Actualmente, la expresión app() actualmente no se admite en la consulta de búsqueda cuando se usa Azure Portal para crear un [regla de alerta de búsqueda de registros personalizada](../platform/alerts-log.md), salvo que se use una aplicación de Application Insights se usa como recurso para la regla de alertas.

## <a name="examples"></a>Ejemplos

```Kusto
app("fabrikamapp").requests | count
```
```Kusto
app("AI-Prototype/Fabrikam/fabrikamapp").requests | count
```
```Kusto
app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count
```
```Kusto
app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [expresión workspace](workspace-expression.md) para hacer referencia al área de trabajo de Log Analytics.
- Obtenga información sobre cómo se almacenan los [datos de Azure Monitor](../../azure-monitor/log-query/log-query-overview.md).
- Acceda a toda la documentación del [lenguaje de consulta de Kusto](/azure/kusto/query/).
