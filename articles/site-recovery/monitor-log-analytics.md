---
title: Supervisión de Azure Site Recovery con registros de Azure Monitor (Log Analytics)
description: Aprenda a supervisar Azure Site Recovery con registros de Azure Monitor (Log Analytics)
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: raynew
ms.openlocfilehash: 4eb88658437d3b29cc55d24bb83f73b660daea43
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718231"
---
# <a name="monitor-site-recovery-with-azure-monitor-logs"></a>Supervisión de Site Recovery con registros de Azure Monitor

En este artículo se describe cómo supervisar las máquinas replicadas que haya replicado Azure [Site Recovery](site-recovery-overview.md), mediante los [registros de Azure Monitor](../azure-monitor/platform/data-platform-logs.md) y [log Analytics](../azure-monitor/log-query/log-query-overview.md).

Los registros de Azure Monitor proporcionan una plataforma de datos de registro que recopila registros de actividad y diagnóstico, junto con otros datos de supervisión. En los registros de Azure Monitor, debe usar Log Analytics para escribir y probar consultas de registro y para analizar interactivamente esos datos de registro. Puede visualizar y consultar los resultados del registro y configurar alertas para realizar acciones basadas en los datos supervisados.

En cuanto a Site Recovery, puede usar los registros de Azure Monitor para hacer lo siguiente:

- **Supervisar el estado de Site Recovery**. Por ejemplo, puede supervisar el estado de la replicación, probar el estado de conmutación por error, los eventos de Site Recovery, los objetivos del punto de recuperación (RPO) para máquinas protegidas y las velocidades de cambio de disco o datos.
- **Configurar alertas para Site Recovery**. Por ejemplo, puede configurar alertas relacionadas con el estado de la máquina, probar el estado de conmutación por error o el estado del trabajo de Site Recovery.

El uso de los registro de Azure Monitor con Site Recovery es compatible para la replicación de Azure a Azure y con la VM de VMware o el servidor físico a la replicación de Azure.
## <a name="before-you-start"></a>Antes de comenzar

Esto es lo que necesita:

- Al menos una máquina protegida en un almacén de Recovery Services.
- Un área de trabajo de Log Analytics para almacenar los registros de Site Recovery. [Obtenga información](../azure-monitor/learn/quick-create-workspace.md) sobre cómo configurar un área de trabajo.
- Tener una noción básica de cómo escribir, ejecutar y analizar consultas de registros en Log Analytics. [Más información](../azure-monitor/log-query/get-started-portal.md).

Le recomendamos que revise las [preguntas de supervisión más comunes](monitoring-common-questions.md) antes de comenzar.

## <a name="configure-site-recovery-to-send-logs"></a>Configurar Site Recovery para enviar registros

1. En el almacén, haga clic en **Diagnostic settings** > **Add diagnostic setting** (Configuración de diagnóstico > Agregar configuración de diagnóstico).

    ![Seleccionar el registro de diagnóstico](./media/monitoring-log-analytics/add-diagnostic.png)

2. En **Diagnostic settings** (Configuración de diagnóstico), especifique un nombre para la acción de registro y seleccione **Send to Log Analytics** (Enviar a Log Analytics).
3. Seleccione la suscripción de los registros de Azure Monitor y el área de trabajo de Log Analytics.
4. En la lista de registros, seleccione todos los registros con el prefijo **AzureSiteRecovery**. A continuación, haga clic en **Aceptar**.

    ![Selección del área de trabajo](./media/monitoring-log-analytics/select-workspace.png)

Los registros de Site Recovery comienzan a obtener datos en una tabla (**AzureDiagnostics**) en el área de trabajo seleccionada.


## <a name="query-the-logs---examples"></a>Consultar los registros: ejemplos

Puede recuperar datos de los registros mediante consultas de registro escritas con el [lenguaje de consulta Kusto](../azure-monitor/log-query/get-started-queries.md). En esta sección se proporcionan algunos ejemplos de consultas comunes que puede usar para la supervisión de Site Recovery.

> [!NOTE]
> Algunos de los ejemplos usan **replicationProviderName_s** establecido en **A2A**. Esta opción recupera las VM de Azure que se replican en una región secundaria de Azure mediante Site Recovery. En estos ejemplos, puede reemplazar **A2A** con **InMageAzureV2**, si quiere recuperar las VM de VMware locales o los servidores físicos que se replican en Azure mediante Site Recovery.


### <a name="query-replication-health"></a>Estado de la replicación de consulta

Esta consulta traza un gráfico circular del estado actual de la replicación de todas las VM de Azure protegidas, y está dividido en tres estados: Normal, Advertencia o Crítico.

```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s  
| project name_s , replicationHealth_s  
| summarize count() by replicationHealth_s  
| render piechart   
```
### <a name="query-mobility-service-version"></a>Consultar la versión de Mobility Service

Esta consulta traza un gráfico circular de las VM de Azure replicadas con Site Recovery, y lo desglosa en función de la versión del agente de Mobility que estén ejecutando las máquinas.

```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s  
| project name_s , agentVersion_s  
| summarize count() by agentVersion_s  
| render piechart 
```

### <a name="query-rpo-time"></a>Consultar la hora de RPO

Esta consulta traza un gráfico de barras de VM de Azure replicadas con Site Recovery, y lo desglosa en función del objetivo de punto de recuperación (RPO): Menos de 15 minutos; entre 15 y 30 minutos; más de 30 minutos.

```
AzureDiagnostics 
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)  
| extend RPO = case(rpoInSeconds_d <= 900, "<15Min",   
rpoInSeconds_d <= 1800, "15-30Min", ">30Min")  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s  
| project name_s , RPO  
| summarize Count = count() by RPO  
| render barchart 
```

![Consultar RPO](./media/monitoring-log-analytics/example1.png)

### <a name="query-site-recovery-jobs"></a>Consultar trabajos de Site Recovery

Esta consulta recupera todos los trabajos de Site Recovery (para todos los escenarios de recuperación ante desastres) activados en las últimas 72 horas y su estado de finalización.

```
AzureDiagnostics  
| where Category == "AzureSiteRecoveryJobs"  
| where TimeGenerated >= ago(72h)   
| project JobName = OperationName , VaultName = Resource , TargetName = affectedResourceName_s, State = ResultType  
```

### <a name="query-site-recovery-events"></a>Consultar eventos de Site Recovery

Esta consulta recupera todos los eventos de Site Recovery (para todos los escenarios de recuperación ante desastres) generados en las últimas 72 horas, junto con su gravedad. 

```
AzureDiagnostics   
| where Category == "AzureSiteRecoveryEvents"   
| where TimeGenerated >= ago(72h)   
| project AffectedObject=affectedResourceName_s , VaultName = Resource, Description_s = healthErrors_s , Severity = Level  
```

### <a name="query-test-failover-state-pie-chart"></a>Consultar el estado de conmutación por error de prueba (gráfico circular)

Esta consulta traza un gráfico circular del estado de conmutación por error de prueba de las VM de Azure replicadas con Site Recovery.

```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)  
| where isnotempty(failoverHealth_s) and isnotnull(failoverHealth_s)  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s  
| project name_s , Resource, failoverHealth_s  
| summarize count() by failoverHealth_s  
| render piechart 
```

### <a name="query-test-failover-state-table"></a>Consultar el estado de conmutación por error de prueba (tabla)

Esta consulta traza una tabla del estado de conmutación por error de prueba de las VM de Azure replicadas con Site Recovery.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)   
| where isnotempty(failoverHealth_s) and isnotnull(failoverHealth_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project VirtualMachine = name_s , VaultName = Resource , TestFailoverStatus = failoverHealth_s 
```

### <a name="query-machine-rpo"></a>Consultar el RPO de la máquina

Esta consulta traza un gráfico de tendencias que realiza un seguimiento del RPO de una VM de Azure específica (ContosoVM123) durante las últimas 72 horas.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where TimeGenerated > ago(72h)  
| where isnotempty(name_s) and isnotnull(name_s)   
| where name_s == "ContosoVM123"  
| project TimeGenerated, name_s , RPO_in_seconds = rpoInSeconds_d   
| render timechart 
```
![Consultar el RPO de la máquina](./media/monitoring-log-analytics/example2.png)

### <a name="query-data-change-rate-churn-for-a-vm"></a>Consultar la velocidad de cambio de datos (renovación) de una VM

Esta consulta traza un gráfico de tendencias de una VM de Azure específica (ContosoVM123), que realiza el seguimiento de la velocidad de cambio de datos (bytes de escritura por segundo) y la velocidad de carga de datos. Esta información solo está disponible para VM de Azure replicadas en una región secundaria de Azure.

```
AzureDiagnostics   
| where Category in ("AzureSiteRecoveryProtectedDiskDataChurn", "AzureSiteRecoveryReplicationDataUploadRate")   
| extend CategoryS = case(Category contains "Churn", "DataChurn",   
Category contains "Upload", "UploadRate", "none")  
| extend InstanceWithType=strcat(CategoryS, "_", InstanceName_s)   
| where TimeGenerated > ago(24h)   
| where InstanceName_s startswith "ContosoVM123"   
| project TimeGenerated , InstanceWithType , Churn_MBps = todouble(Value_s)/1048576   
| render timechart  
```
![Consultar el cambio de datos](./media/monitoring-log-analytics/example3.png)

### <a name="query-disaster-recovery-summary-azure-to-azure"></a>Consultar el resumen de recuperación ante desastres (Azure a Azure)

Esta consulta traza una tabla de resumen para VM de Azure replicadas en una región secundaria de Azure.  Muestra el nombre de la VM, el estado de replicación y protección, el RPO, el estado de conmutación por error de prueba, la versión del agente de Mobility, cualquier error de replicación activa y la ubicación de origen.

```
AzureDiagnostics 
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project VirtualMachine = name_s , Vault = Resource , ReplicationHealth = replicationHealth_s, Status = protectionState_s, RPO_in_seconds = rpoInSeconds_d, TestFailoverStatus = failoverHealth_s, AgentVersion = agentVersion_s, ReplicationError = replicationHealthErrors_s, SourceLocation = primaryFabricName_s 
```

### <a name="query-disaster-recovery-summary-vmwarephysical-servers"></a>Consultar el resumen de recuperación ante desastres (VMware / servidores físicos)

Esta consulta traza una tabla de resumen para VM de VMware y servidores físicos replicados en Azure.  Muestra el nombre de la máquina, el estado de replicación y protección, el RPO, el estado de conmutación por error de prueba, la versión del agente de Mobility, cualquier error de replicación activa y el servidor de proceso relevante.

```
AzureDiagnostics  
| where replicationProviderName_s == "InMageAzureV2"   
| where isnotempty(name_s) and isnotnull(name_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project VirtualMachine = name_s , Vault = Resource , ReplicationHealth = replicationHealth_s, Status = protectionState_s, RPO_in_seconds = rpoInSeconds_d, TestFailoverStatus = failoverHealth_s, AgentVersion = agentVersion_s, ReplicationError = replicationHealthErrors_s, ProcessServer = processServerName_g  
```

## <a name="set-up-alerts---examples"></a>Configurar alertas: ejemplos

Puede configurar alertas de Site Recovery basadas en datos de Azure Monitor. [Obtenga más información](../azure-monitor/platform/alerts-log.md#managing-log-alerts-from-the-azure-portal) sobre cómo configurar alertas de registro. 

> [!NOTE]
> Algunos de los ejemplos usan **replicationProviderName_s** establecido en **A2A**. Esto establece alertas en las VM de Azure que se replican en una región secundaria de Azure. En estos ejemplos, puede reemplazar **A2A** con **InMageAzureV2** si quiere establecer alertas para VM de VMware locales o para servidores físicos replicados en Azure.

### <a name="multiple-machines-in-a-critical-state"></a>Varias máquinas en estado crítico

Configure una alerta si más de 20 VM de Azure replicadas entran en estado crítico.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where replicationHealth_s == "Critical"  
| where isnotempty(name_s) and isnotnull(name_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| summarize count() 
```
Para la alerta, establezca el **valor del umbral** en 20.

### <a name="single-machine-in-a-critical-state"></a>Máquina individual en estado crítico

Configure una alerta si una VM de Azure replicada específica entra en un estado crítico.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where replicationHealth_s == "Critical"  
| where name_s == "ContosoVM123"  
| where isnotempty(name_s) and isnotnull(name_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| summarize count()  
```
Para la alerta, establezca el **valor del umbral** en 1.

### <a name="multiple-machines-exceed-rpo"></a>Varias máquinas superan el RPO

Configure una alerta si el RPO de más de 20 VM de Azure supera los 30 minutos.
```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)   
| where rpoInSeconds_d > 1800  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project name_s , rpoInSeconds_d   
| summarize count()  
```
Para la alerta, establezca el **valor del umbral** en 20.

### <a name="single-machine-exceeds-rpo"></a>La máquina individual supera el RPO

Configure una alerta si el RPO de una sola VM de Azure supera los 30 minutos.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)   
| where name_s == "ContosoVM123"  
| where rpoInSeconds_d > 1800  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project name_s , rpoInSeconds_d   
| summarize count()  
```
Para la alerta, establezca el **valor del umbral** en 1.

### <a name="test-failover-for-multiple-machines-exceeds-90-days"></a>La conmutación por error de prueba de varias máquinas supera los 90 días

Configure una alerta si la última conmutación por error de prueba correcta se realizó hace más de 90 días, en más de 20 VM. 

```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where Category == "AzureSiteRecoveryReplicatedItems"  
| where isnotempty(name_s) and isnotnull(name_s)   
| where lastSuccessfulTestFailoverTime_t <= ago(90d)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| summarize count()  
```
Para la alerta, establezca el **valor del umbral** en 20.

### <a name="test-failover-for-single-machine-exceeds-90-days"></a>La conmutación por error de prueba de una sola máquina supera los 90 días

Configure una alerta si la última conmutación por error de prueba correcta de una VM específica se realizó hace más de 90 días.
```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where Category == "AzureSiteRecoveryReplicatedItems"  
| where isnotempty(name_s) and isnotnull(name_s)   
| where lastSuccessfulTestFailoverTime_t <= ago(90d)   
| where name_s == "ContosoVM123"  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| summarize count()  
```
Para la alerta, establezca el **valor del umbral** en 1.

### <a name="site-recovery-job-fails"></a>Error en el trabajo de Site Recovery

Configure una alerta si durante el último día se produce un error en un trabajo de Site Recovery (en este caso, el trabajo Reprotect) que se encuentre en cualquier escenario de Site Recovery. 
```
AzureDiagnostics   
| where Category == "AzureSiteRecoveryJobs"   
| where OperationName == "Reprotect"  
| where ResultType == "Failed"  
| summarize count()  
```

Para la alerta, establezca el **valor del umbral** en 1 y el **período** en 1440 minutos, para comprobar los errores del último día.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga más información](site-recovery-monitor-and-troubleshoot.md) sobre la supervisión de Site Recovery integrada.
