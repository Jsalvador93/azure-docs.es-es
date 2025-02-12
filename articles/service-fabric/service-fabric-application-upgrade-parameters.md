---
title: 'Actualización de la aplicación: parámetros de actualización | Microsoft Docs'
description: Describe los parámetros relacionados con la actualización de una aplicación de Service Fabric, incluida la realización de comprobaciones de estado y directivas para deshacer automáticamente la actualización.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/08/2018
ms.author: subramar
ms.openlocfilehash: 9a93c0993ee45e72b11b023982dfbbe8c6528272
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60614393"
---
# <a name="application-upgrade-parameters"></a>Parámetros de actualización de la aplicación
En este artículo se describen los distintos parámetros que se aplican durante la actualización de una aplicación de Azure Service Fabric. Los parámetros de actualización de la aplicación controlan los tiempos de espera y las comprobaciones de estado que se aplican durante la actualización y especifican las directivas que deben aplicarse cuando se produce un error en una actualización. Los parámetros de la aplicación se aplican en las actualizaciones mediante:
- PowerShell
- Visual Studio
- SFCTL
- [REST](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-startapplicationupgrade)

Las actualizaciones de aplicaciones se inician a través de uno de estos tres modos de actualización que el usuario puede seleccionar. Cada modo tiene su propio conjunto de parámetros de la aplicación:
- Supervisado
- Automático no supervisado
- Manual no supervisado

Los parámetros obligatorios y opcionales aplicables se describen en las secciones correspondientes a continuación.

## <a name="visual-studio-and-powershell-parameters"></a>Parámetros de Visual Studio y PowerShell

Las actualizaciones de aplicaciones de Service Fabric con PowerShell usan el comando [Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade). Para seleccionar el modo de actualización, debe pasar el parámetro **Monitored**, **UnmonitoredAuto** o **UnmonitoredManual** a [ Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade).

Los parámetros de actualización de la aplicación Service Fabric de Visual Studio se establecen mediante el cuadro de diálogo de configuración de actualización de Visual Studio. Para seleccionar el modo de actualización de Visual Studio, use el cuadro desplegable **Actualizar modo** para **Monitored**, **UnmonitoredAuto** o **UnmonitoredManual**. Para obtener más información, consulte [Configuración de la actualización de una aplicación de Service Fabric en Visual Studio](service-fabric-visualstudio-configure-upgrade.md).

### <a name="required-parameters"></a>Parámetros obligatorios
(PS = PowerShell, VS = Visual Studio)

| Parámetro | Se aplica a | DESCRIPCIÓN |
| --- | --- | --- |
ApplicationName |PS| Nombre de la aplicación que se actualiza. Ejemplos: fabric:/VisualObjects, fabric:/ClusterMonitor. |
ApplicationTypeVersion|PS|La versión del tipo de aplicación al que se dirige la actualización. |
FailureAction |PS, VS|Los valores permitidos son **Rollback**, **Manual** y **Invalid**. Acción de compensación que se debe realizar cuando una actualización *supervisada* detecta infracciones de directivas de mantenimiento o supervisión. <br>**Rollback** especifica que la actualización se revertirá automáticamente a la versión previa a la actualización. <br>**Manual** indica que la actualización se cambiará al modo de actualización *UnmonitoredManual*. <br>**Invalid** indica que la acción de error no es válida.|
Monitored |PS|Indica que se supervisa el modo de actualización. Después de que el cmdlet finalice una actualización para un dominio de actualización, si el estado de mantenimiento del dominio de actualización y del clúster cumple las directivas de mantenimiento que defina, Service Fabric actualiza el siguiente dominio de actualización. Si el dominio de actualización o el clúster no cumplen las directivas de mantenimiento, la actualización produce un error y Service Fabric revierte la actualización para el dominio de actualización o vuelve al modo manual por la directiva especificada. Este es el modo recomendado para las actualizaciones de aplicaciones en un entorno de producción. |
UpgradeMode | VS | Los valores permitidos son **Monitored** (valor predeterminado), **UnmonitoredAuto** o **UnmonitoredManual**. Vea los parámetros de PowerShell para cada modo de este artículo para obtener más información. |
UnmonitoredAuto | PS | Indica que el modo de actualización no se supervisa de forma automática. Después de que Service Fabric actualice un dominio de actualización, actualiza el siguiente dominio de actualización, independientemente del estado de mantenimiento de la aplicación. Este modo no se recomienda para entornos de producción y solo es útil durante el desarrollo de una aplicación. |
UnmonitoredManual | PS | Indica que el modo de actualización no se supervisa de forma manual. Después de que Service Fabric actualice un dominio de actualización, espera a que el usuario actualice el siguiente dominio de actualización mediante el uso del cmdlet *Resume-ServiceFabricApplicationUpgrade*. |

### <a name="optional-parameters"></a>Parámetros opcionales

Los parámetros de evaluación de estado son opcionales. Si no se especifican los criterios de evaluación de estado al iniciar una actualización, Service Fabric usa las directivas de mantenimiento de aplicaciones especificadas en ApplicationManifest.xml de la instancia de aplicación.

Utilice la barra de desplazamiento horizontal de la parte inferior de la tabla para ver el campo de descripción completo.

(PS = PowerShell, VS = Visual Studio)

| Parámetro | Se aplica a | DESCRIPCIÓN |
| --- | --- | --- |
| ApplicationParameter |PS, VS| Especifica las invalidaciones de los parámetros de aplicación.<br>Los parámetros de aplicaciones de PowerShell se especifican como pares de nombre/valor de la tabla hash. Por ejemplo, @{ "VotingData_MinReplicaSetSize" = "3"; "VotingData_PartitionCount" = "1" }.<br>Se pueden especificar parámetros de aplicaciones de Visual Studio en el cuadro de diálogo Publicación de la aplicación de Service Fabric en el campo **Archivo de parámetros de aplicación**.
| Confirm |PS| Los valores permitidos son **True** y **False**. Solicita confirmación antes de ejecutar el cmdlet. |
| ConsiderWarningAsError |PS, VS |Los valores permitidos son **True** y **False**. El valor predeterminado es **False**. Trate los eventos de estado de advertencia para la aplicación como errores al evaluar el estado de la aplicación durante la actualización. De forma predeterminada, Service Fabric no evalúa los eventos de estado de advertencia en los que se producen errores, por lo que puede continuar la actualización incluso si hay eventos de advertencia. |
| DefaultServiceTypeHealthPolicy | PS, VS |Especifica la directiva de mantenimiento del tipo de servicio predeterminado que se usará para la actualización supervisada con el formato MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices. Por ejemplo, 5,10,15 indica los valores siguientes: MaxPercentUnhealthyPartitionsPerService = 5, MaxPercentUnhealthyReplicasPerPartition = 10, MaxPercentUnhealthyServices = 15. |
| Force | PS, VS | Los valores permitidos son **True** y **False**. Indica que el proceso de actualización omite el mensaje de advertencia y fuerza la actualización incluso si no ha cambiado el número de versión. Esto es útil para las pruebas locales, pero no se recomienda para su uso en un entorno de producción, ya que requiere la eliminación de la implementación existente, lo que provoca un tiempo de inactividad y una posible pérdida de datos. |
| ForceRestart |PS, VS |Si actualiza una configuración o un paquete de datos sin actualizar el código del servicio, el servicio se reinicia solo si la propiedad ForceRestart se establece en **True**. Una vez completada la actualización, Service Fabric notifica al servicio que un nuevo paquete de configuración o datos está disponible. El servicio es responsable de aplicar los cambios. Si es necesario, el servicio puede reiniciarse automáticamente. |
| HealthCheckRetryTimeoutSec |PS, VS |La duración (en segundos) empleada por Service Fabric para continuar realizando la evaluación de estado antes de declarar que se ha producido un error en la actualización. El valor predeterminado es 600 segundos. Esta duración se inicia tras alcanzarse el valor de *HealthCheckWaitDurationSec*. Dentro de *HealthCheckRetryTimeout*, Service Fabric puede realizar varias comprobaciones de estado del estado de la aplicación. El valor predeterminado es 10 minutos y se recomienda una personalización apropiada para su aplicación. |
| HealthCheckStableDurationSec |PS, VS |La duración (en segundos) para comprobar la estabilidad de la aplicación antes de pasar al siguiente dominio de actualización o completar la actualización. Esta duración de espera se emplea para evitar cambios de estado no detectados justo después de la realización de la comprobación de estado. El valor predeterminado es 120 segundos y se recomienda una personalización apropiada para su aplicación. |
| HealthCheckWaitDurationSec |PS, VS | El tiempo de espera (en segundos) una vez finalizada la actualización en el dominio de actualización antes de que Service Fabric evalúe el estado de la aplicación. Esta duración también puede considerarse como el tiempo que debe tardar en ejecutarse una aplicación antes de que se pueda considerar correcta. Si se supera la comprobación de estado, el proceso de actualización pasa al siguiente dominio de actualización.  Si se produce un error en la comprobación de estado, Service Fabric espera un intervalo [UpgradeHealthCheckInterval](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-fabric-settings#clustermanager) antes de reintentar la comprobación de estado de nuevo hasta que se alcance el valor de *HealthCheckRetryTimeoutSec*. El valor predeterminado y recomendado es 0 segundos. |
| MaxPercentUnhealthyDeployedApplications|PS, VS |El valor predeterminado y recomendado es 0. Especifique el número máximo de aplicaciones implementadas (consulte la [sección Estado](service-fabric-health-introduction.md)) que puede ser incorrecto antes de que la aplicación se considere incorrecta y provoque un error en la actualización. Este parámetro define el estado de la aplicación en el nodo y ayuda a detecta problemas durante la actualización. Normalmente, la carga de las réplicas de la aplicación se equilibrará en el otro nodo, lo que permite que la aplicación figure como correcta y, por lo tanto, continúe la actualización. Al especificar un estado de mantenimiento *MaxPercentUnhealthyDeployedApplications* estricto, Service Fabric puede encontrar un problema con el paquete de aplicación de forma rápida y hacer que la actualización fracase para responder rápido a los errores. |
| MaxPercentUnhealthyServices |PS, VS |Un parámetro para *DefaultServiceTypeHealthPolicy* y *ServiceTypeHealthPolicyMap*. El valor predeterminado y recomendado es 0. Especifique el número máximo de servicios en la instancia de aplicación que puede ser incorrecto antes de que la aplicación se considere incorrecta y haga que se produzca un error en la actualización. |
| MaxPercentUnhealthyPartitionsPerService|PS, VS |Un parámetro para *DefaultServiceTypeHealthPolicy* y *ServiceTypeHealthPolicyMap*. El valor predeterminado y recomendado es 0. Especifique el número máximo de particiones en un servicio que puede ser incorrecto antes de que el servicio se considere incorrecto. |
| MaxPercentUnhealthyReplicasPerPartition|PS, VS |Un parámetro para *DefaultServiceTypeHealthPolicy* y *ServiceTypeHealthPolicyMap*. El valor predeterminado y recomendado es 0. Especifique el número máximo de réplicas en la partición que puede ser incorrecto antes de que esta se considere incorrecta. |
| ServiceTypeHealthPolicyMap | PS, VS | Representa la directiva de mantenimiento que se usa para evaluar el mantenimiento de los servicios que pertenecen a un tipo de servicio. Toma una entrada de la tabla hash en el siguiente formato: @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}. Por ejemplo: @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }. |
| TimeoutSec | PS, VS | Especifica el período de tiempo de espera en segundos para la operación. |
| UpgradeDomainTimeoutSec |PS, VS |Tiempo máximo (en segundos) para actualizar un solo dominio de actualización. Si se alcanza este tiempo de espera, la actualización se detiene y continúa en función del valor de *FailureAction*. El valor predeterminado es Never (infinito) y debe personalizarse correctamente para su aplicación. |
| UpgradeReplicaSetCheckTimeoutSec |PS, VS |Se mide en segundos.<br>**Servicio sin estado**: dentro de un único dominio de actualización, Service Fabric intenta garantizar la disponibilidad de instancias adicionales del servicio. Si el recuento de instancias de destino es superior a uno, Service Fabric espera la disponibilidad de más de una instancia, hasta un valor de tiempo de espera máximo. Este tiempo de espera se especifica mediante la propiedad *UpgradeReplicaSetCheckTimeoutSec*. Si se agota el tiempo de expiración, Service Fabric continúa con la actualización, independientemente del número de instancias de servicio. Si el recuento de instancias de destino es uno, Service Fabric no espera e inmediatamente continúa con la actualización.<br><br>**Servicio con estado**: dentro de un único dominio de actualización, Service Fabric intenta garantizar que el conjunto de réplicas tenga un cuórum. Service Fabric espera la disponibilidad de un cuórum, hasta un valor de tiempo de espera máximo (especificado por la propiedad *UpgradeReplicaSetCheckTimeoutSec*). Si se agota el tiempo de expiración, Service Fabric continúa con la actualización, independientemente del cuórum. Este valor se establece como Nunca (infinito) si avanza, y en 1200 segundos si retrocede. |
| UpgradeTimeoutSec |PS, VS |Un tiempo de expiración (en segundos) que se aplica a toda la actualización. Si se agota el tiempo de espera, la actualización se detiene y se desencadena *FailureAction*. El valor predeterminado es Never (infinito) y debe personalizarse correctamente para su aplicación. |
| WhatIf | PS | Los valores permitidos son **True** y **False**. Muestra lo que sucedería si se ejecutara el cmdlet. El cmdlet no se ejecuta. |

Los criterios *MaxPercentUnhealthyServices*, *MaxPercentUnhealthyPartitionsPerService* y *MaxPercentUnhealthyReplicasPerPartition* se pueden especificar por tipo de servicio para una instancia de aplicación. Establecer estos parámetros por servicio permite que una aplicación contenga diferentes tipos de servicios con diferentes directivas de evaluación. Por ejemplo, un tipo de servicio de puerta de enlace sin estado puede tener un valor de *MaxPercentUnhealthyPartitionsPerService* distinto al de un tipo de servicio del motor con estado para una instancia de aplicación determinada.

## <a name="sfctl-parameters"></a>Parámetros de SFCTL

Las actualizaciones de aplicaciones de Service Fabric mediante la CLI de Service Fabric usan el comando [sfctl application upgrade](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade) junto con los siguientes parámetros obligatorios y opcionales.

### <a name="required-parameters"></a>Parámetros obligatorios

| Parámetro | DESCRIPCIÓN |
| --- | --- |
| application-id  |Identificador de la aplicación que se actualiza. <br> Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric:" A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric:/myapp/app1", la identidad de la aplicación sería "myapp\~app1' in 6.0+ and 'myapp/app1" en las versiones anteriores.|
application-version |La versión del tipo de aplicación al que se dirige la actualización.|
parameters  |Una lista codificada en JSON de reemplazos de parámetros de aplicación que se aplicarán al actualizar la aplicación.|

### <a name="optional-parameters"></a>Parámetros opcionales

| Parámetro | DESCRIPCIÓN |
| --- | --- |
default-service-health-policy | Especificación codificada en [JSON](https://docs.microsoft.com/rest/api/servicefabric/sfclient-model-servicetypehealthpolicy) de la directiva de mantenimiento que se usa de forma predeterminada para evaluar el estado de un tipo de servicio. La asignación está vacía de forma predeterminada. |
failure-action | Los valores permitidos son **Rollback**, **Manual** y **Invalid**. Acción de compensación que se debe realizar cuando una actualización *supervisada* detecta infracciones de directivas de mantenimiento o supervisión. <br>**Rollback** especifica que la actualización se revertirá automáticamente a la versión previa a la actualización. <br>**Manual** indica que la actualización se cambiará al modo de actualización *UnmonitoredManual*. <br>**Invalid** indica que la acción de error no es válida.|
force-restart | Si actualiza una configuración o un paquete de datos sin actualizar el código del servicio, el servicio se reinicia solo si la propiedad ForceRestart se establece en **True**. Una vez completada la actualización, Service Fabric notifica al servicio que un nuevo paquete de configuración o datos está disponible. El servicio es responsable de aplicar los cambios. Si es necesario, el servicio puede reiniciarse automáticamente. |
health-check-retry-timeout | La cantidad de tiempo que se reintentan las evaluaciones de mantenimiento cuando la aplicación o el clúster no tienen un estado correcto antes de que se ejecute *FailureAction*. En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. Valor predeterminado: PT0H10M0S. |
health-check-stable-duration | La cantidad de tiempo que la aplicación o el clúster deben tener un estado correcto antes de que la actualización continúe con el siguiente dominio de actualización. En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. Valor predeterminado: PT0H2M0S. |
health-check-wait-duration | La cantidad de tiempo de espera después de completar un dominio de actualización antes de aplicar directivas de mantenimiento. En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. Valor predeterminado: 0.|
max-unhealthy-apps | El valor predeterminado y recomendado es 0. Especifique el número máximo de aplicaciones implementadas (consulte la [sección Estado](service-fabric-health-introduction.md)) que puede ser incorrecto antes de que la aplicación se considere incorrecta y provoque un error en la actualización. Este parámetro define el estado de la aplicación en el nodo y ayuda a detecta problemas durante la actualización. Normalmente, la carga de las réplicas de la aplicación se equilibrará en el otro nodo, lo que permite que la aplicación figure como correcta y, por lo tanto, continúe la actualización. Al especificar un estado de mantenimiento *max-unhealthy-apps* estricto, Service Fabric puede encontrar un problema con el paquete de aplicación de forma rápida y hacer que la actualización fracase para responder rápido a los errores. Se representa como un número entre 0 y 100. |
modo | Los valores permitidos son **Monitored**, **UpgradeMode**, **UnmonitoredAuto** y **UnmonitoredManual**. El valor predeterminado es **UnmonitoredAuto**. Consulte la sección *Parámetros obligatorios* de Visual Studio y PowerShell para las descripciones de estos valores.|
replica-set-check-timeout |Se mide en segundos. <br>**Servicio sin estado**: dentro de un único dominio de actualización, Service Fabric intenta garantizar la disponibilidad de instancias adicionales del servicio. Si el recuento de instancias de destino es superior a uno, Service Fabric espera la disponibilidad de más de una instancia, hasta un valor de tiempo de espera máximo. Este tiempo de espera se especifica mediante la propiedad *replica-set-check-timeout*. Si se agota el tiempo de expiración, Service Fabric continúa con la actualización, independientemente del número de instancias de servicio. Si el recuento de instancias de destino es uno, Service Fabric no espera e inmediatamente continúa con la actualización.<br><br>**Servicio con estado**: dentro de un único dominio de actualización, Service Fabric intenta garantizar que el conjunto de réplicas tenga un cuórum. Service Fabric espera la disponibilidad de un cuórum, hasta un valor de tiempo de expiración máximo (especificado por la propiedad *replica-set-check-timeout*). Si se agota el tiempo de expiración, Service Fabric continúa con la actualización, independientemente del cuórum. Este valor se establece como Nunca (infinito) si avanza, y en 1200 segundos si retrocede. |
service-health-policy | Mapa codificado en JSON con una directiva de mantenimiento de tipo de servicio por cada nombre de tipo de servicio. El mapa está vacío de forma predeterminada. [Formato JSON del parámetro](https://docs.microsoft.com/rest/api/servicefabric/sfclient-model-applicationhealthpolicy#servicetypehealthpolicymap). Contiene el JSON de la parte "Value" **MaxPercentUnhealthyServices**, **MaxPercentUnhealthyPartitionsPerService** y **MaxPercentUnhealthyReplicasPerPartition**. Consulte la sección Parámetros opcionales de Visual Studio y PowerShell para las descripciones de estos parámetros.
timeout | Especifica el período de tiempo de espera en segundos para la operación. Valor predeterminado: 60. |
upgrade-domain-timeout | El período de tiempo del que dispone cada dominio de actualización para completarse antes de la ejecución de *FailureAction*. En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. El valor predeterminado es Never (infinito) y debe personalizarse correctamente para su aplicación. Valor predeterminado: P10675199DT02H48M05.4775807S. |
upgrade-timeout | El período de tiempo del que dispone cada dominio de actualización para completarse antes de la ejecución de *FailureAction*. En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. El valor predeterminado es Never (infinito) y debe personalizarse correctamente para su aplicación. Valor predeterminado: P10675199DT02H48M05.4775807S.|
warning-as-error | Los valores permitidos son **True** y **False**. El valor predeterminado es **False**. Puede pasarse como una marca. Trate los eventos de estado de advertencia para la aplicación como errores al evaluar el estado de la aplicación durante la actualización. De forma predeterminada, Service Fabric no evalúa los eventos de estado de advertencia en los que se producen errores, por lo que puede continuar la actualización incluso si hay eventos de advertencia. |

## <a name="next-steps"></a>Pasos siguientes
[actualización de aplicaciones usando Visual Studio](service-fabric-application-upgrade-tutorial.md) ofrece información para actualizar una aplicación mediante Visual Studio.

[Actualización de aplicaciones de Service Fabric con PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) se explica en detalle lo que tiene que hacer para actualizar una aplicación mediante PowerShell.

[Actualización de la aplicación mediante la CLI de Service Fabric en Linux](service-fabric-application-lifecycle-sfctl.md#upgrade-application) le lleva por los pasos para actualizar una aplicación mediante la CLI de Service Fabric.

[Actualización de la aplicación mediante el complemento de Eclipse para Service Fabric](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Consiga que sus actualizaciones de aplicaciones sean compatibles aprendiendo a usar la [serialización de datos](service-fabric-application-upgrade-data-serialization.md).

Aprenda a usar funcionalidades avanzadas para actualizar una aplicación. Para ello, consulte los [temas avanzados](service-fabric-application-upgrade-advanced.md).

Solucione problemas habituales en las actualizaciones de aplicaciones consultando los pasos que figuran en [Solución de problemas de las actualizaciones de aplicaciones](service-fabric-application-upgrade-troubleshooting.md).
