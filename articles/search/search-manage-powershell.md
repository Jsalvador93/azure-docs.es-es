---
title: 'Scripts de PowerShell con el módulo Az.Search: Azure Search'
description: Cree y configure un servicio Azure Search con PowerShell. Puede escalar o reducir un servicio verticalmente, gestionar la administración y las claves de API de consulta y consultar información del sistema.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: heidist
ms.openlocfilehash: 6090881cc2b94fa42fdac22220c858a0153ccc5c
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648090"
---
# <a name="manage-your-azure-search-service-with-powershell"></a>Administración del servicio Azure Search con PowerShell
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [API DE REST](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Puede ejecutar los scripts y cmdlets de PowerShell en o Windows, Linux, o en [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) para crear y configurar Azure Search. El módulo **Az.Search** extiende Azure PowerShell con paridad completa a las [API REST de administración de Azure Search](https://docs.microsoft.com/rest/api/searchmanagement). Con Azure PowerShell y **Az.Search**, puede realizar las siguientes tareas:

> [!div class="checklist"]
> * [Obtener la lista de todos los servicios de búsqueda de su suscripción](#list-search-services)
> * [Obtener información sobre un servicio de búsqueda específico](#get-search-service-information)
> * [Crear o eliminar un servicio](#create-or-delete-a-service)
> * [Regenerar claves de API de administración](#regenerate-admin-keys)
> * [Crear o eliminar claves de API de consulta](#create-or-delete-query-keys)
> * [Escalar un servicio aumentando o reduciendo las réplicas y particiones](#scale-replicas-and-partitions)

PowerShell no se puede usar para cambiar el nombre, la región o el nivel de su servicio. Cuando se crea un servicio, se asignan recursos dedicados. El cambio del hardware subyacente (tipo de nodo o ubicación), requiere un nuevo servicio. No hay herramientas o API para transferir contenido de un servicio a otro. Toda la administración de contenido se realiza a través de API [REST](https://docs.microsoft.com/rest/api/searchservice/) o [.NET](https://docs.microsoft.com/dotnet/api/?term=microsoft.azure.search), y si desea mover los índices, tendrá que volver a crearlos y cargarlos en un nuevo servicio. 

Aunque no haya comandos de PowerShell dedicados para la administración de contenido, puede escribir el script de PowerShell que llama a REST o .NET para crear y cargar índices. El módulo **Az.Search** por sí solo no proporciona estas operaciones.

Otras tareas que no se admiten a través de PowerShell o ninguna otra API (solo portal) incluyen:
+ [Asociar un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md) para [indexación enriquecida de inteligencia artificial](cognitive-search-concept-intro.md). Un servicio cognitivo se asocia a un conjunto de aptitudes, no a una suscripción o servicio.
+ [Soluciones de supervisión de complementos](search-monitor-usage.md#add-on-monitoring-solutions) o [análisis de tráfico de búsqueda](search-traffic-analytics.md) usados para la supervisión de Azure Search.

<a name="check-versions-and-load"></a>

## <a name="check-versions-and-load-modules"></a>Comprobación de versiones y carga de módulos

Los ejemplos de este artículo son interactivos y requieren permisos elevados. Azure PowerShell (el módulo **Az**) debe estar instalado. Para más información, vea [Instalar Azure PowerShell](/powershell/azure/overview).

### <a name="powershell-version-check-51-or-later"></a>Comprobación de la versión de PowerShell (5.1 o posterior)

La versión de PowerShell local de be ser la 5.1 o una versión posterior, en cualquier sistema operativo admitido.

```azurepowershell-interactive
$PSVersionTable.PSVersion
```

### <a name="load-azure-powershell"></a>Carga de Azure PowerShell

Si no sabe si **Az** está instalado, ejecute el siguiente comando como un paso de verificación. 

```azurepowershell-interactive
Get-InstalledModule -Name Az
```

Algunos sistemas no cargan los módulos automáticamente. Si se produce un error en el comando anterior, intente cargar el módulo, y si se vuelve a producir un error, regrese a las instrucciones de instalación para ver si falta algún paso.

```azurepowershell-interactive
Import-Module -Name Az
```

### <a name="connect-to-azure-with-a-browser-sign-in-token"></a>Conexión a Azure con un token de inicio de sesión de explorador

Puede usar sus credenciales de inicio de sesión del portal para conectarse a una suscripción en PowerShell. También puede [autenticarse de forma no interactiva con una entidad de servicio](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

```azurepowershell-interactive
Connect-AzAccount
```

Si mantiene varias suscripciones de Azure, establezca la suscripción de Azure. Para ver una lista de las suscripciones actuales, ejecute este comando.

```azurepowershell-interactive
Get-AzSubscription | sort SubscriptionName | Select SubscriptionName
```

Para especificar la suscripción, ejecute el siguiente comando. En el ejemplo siguiente, el nombre de la suscripción es `ContosoSubscription`.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName ContosoSubscription
```

<a name="list-search-services"></a>

## <a name="list-all-azure-search-services-in-your-subscription"></a>Obtención de una lista de todos los servicios de Azure Search de su suscripción

Los siguientes comandos son de [**Az.Resources**](https://docs.microsoft.com/powershell/module/az.resources/?view=azps-1.4.0#resources), que devuelven información sobre los recursos existentes y servicios ya aprovisionados en su suscripción. Si no sabe cuántos servicios de búsqueda ya se han creado, estos comandos devuelven esa información, lo que le ahorra un viaje al portal.

El primer comando devuelve todos los servicios de búsqueda.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Search/searchServices | ft
```

De la lista de servicios, devuelva información sobre un recurso específico.

```azurepowershell-interactive
Get-AzResource -ResourceName <service-name>
```

Los resultados deben tener un aspecto similar a la siguiente salida.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : westus
ResourceId        : /subscriptions/<alpha-numeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="import-azsearch"></a>Importación de Az.Search

Los comandos de [**Az.Search**](https://docs.microsoft.com/powershell/module/az.search/?view=azps-1.4.0#search) no están disponibles hasta que cargue el módulo.

```azurepowershell-interactive
Install-Module -Name Az.Search
```

### <a name="list-all-azsearch-commands"></a>Lista de todos los comandos de Az.Search

Como paso de verificación, devuelva una lista de comandos proporcionados en el módulo.

```azurepowershell-interactive
Get-Command -Module Az.Search
```

Los resultados deben tener un aspecto similar a la siguiente salida.

```
CommandType     Name                                Version    Source
-----------     ----                                -------    ------
Cmdlet          Get-AzSearchAdminKeyPair            0.7.1      Az.Search
Cmdlet          Get-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          Get-AzSearchService                 0.7.1      Az.Search
Cmdlet          New-AzSearchAdminKey                0.7.1      Az.Search
Cmdlet          New-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          New-AzSearchService                 0.7.1      Az.Search
Cmdlet          Remove-AzSearchQueryKey             0.7.1      Az.Search
Cmdlet          Remove-AzSearchService              0.7.1      Az.Search
Cmdlet          Set-AzSearchService                 0.7.1      Az.Search
```

## <a name="get-search-service-information"></a>Obtención de información del servicio de búsqueda

Después de que **Az.Search** se importe y sepa el grupo de recursos que contiene el servicio de búsqueda, ejecute [Get-AzSearchService](https://docs.microsoft.com/powershell/module/az.search/get-azsearchservice?view=azps-1.4.0) para devolver la definición del servicio, incluido el nombre, la región, el nivel y los recuentos de réplicas y particiones.

```azurepowershell-interactive
Get-AzSearchService -ResourceGroupName <resource-group-name>
```

Los resultados deben tener un aspecto similar a la siguiente salida.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : West US
Sku               : Standard
ReplicaCount      : 1
PartitionCount    : 1
HostingMode       : Default
ResourceId        : /subscriptions/<alphanumeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="create-or-delete-a-service"></a>Creación o eliminación de un servicio

[**New-AzSearchService**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) se usa para [crear un nuevo servicio de búsqueda](search-create-service-portal.md).

```azurepowershell-interactive
New-AzSearchService -ResourceGroupName "demo-westus" -Name "my-demo-searchapp" -Sku "Standard" -Location "West US" -PartitionCount 3 -ReplicaCount 3
``` 
Los resultados deben tener un aspecto similar a la siguiente salida.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Id                : /subscriptions/<alphanumeric-subscription-ID>/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 3
PartitionCount    : 3
HostingMode       : Default
Tags
```     

## <a name="regenerate-admin-keys"></a>Regeneración de claves de administración

[**New-AzSearchAdminKey**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) se usa para sustituir las [claves de API](search-security-api-keys.md) de administración. Se crean dos claves de administración con cada servicio para el acceso autenticado. Las claves son necesarias en cada solicitud. Ambas claves de administración son funcionalmente equivalentes, concediendo acceso completo de escritura a un servicio de búsqueda con la capacidad de recuperar cualquier información, o crear y eliminar cualquier objeto. Existen dos claves, de forma que puede usar una mientras reemplaza la otra. 

Solo puede regenerar una en cada momento, especificada como clave `primary` o `secondary`. Para un servicio ininterrumpido, no olvide actualizar todo el código de cliente para usar una clave secundaria mientras sustituye la clave principal. Evite cambiar las claves mientras haya operaciones en tránsito.

Como puede imaginar, si regenera las claves sin actualizar el código de cliente, se producirá un error en las solicitudes que usan la clave antigua. La regeneración de todas las claves nuevas no lo bloquea permanentemente para acceder al servicio y puede seguir accediendo a este a través del portal. Después de regenerar las claves principal y secundaria, puede actualizar el código de cliente para utilizar las nuevas claves y las operaciones se reanudarán en consecuencia.

El servicio genera los valores de las claves de API. No puede proporcionar una clave personalizada para que la use Azure Search. De forma similar, no hay ningún nombre definido por el usuario para las claves de API de administración. Las referencias a la clave son cadenas fijas, ya sea `primary` o `secondary`. 

```azurepowershell-interactive
New-AzSearchAdminKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -KeyKind Primary
```

Los resultados deben tener un aspecto similar a la siguiente salida. Se devuelven las dos claves aunque solo cambie una a la vez.

```
Primary                    Secondary
-------                    ---------
<alphanumeric-guid>        <alphanumeric-guid>  
```

## <a name="create-or-delete-query-keys"></a>Creación o eliminación de claves de consulta

[**New-AzSearchQueryKey**](https://docs.microsoft.com/powershell/module/az.search/new-azsearchquerykey?view=azps-1.4.0) se usa para crear [claves de API](search-security-api-keys.md) de consulta para el acceso de solo lectura desde las aplicaciones cliente hasta un índice de Azure Search. Las claves de consulta se utilizan para autenticarse en un índice específico con el fin de recuperar los resultados de búsqueda. Las claves de consulta no conceden acceso de solo lectura a otros elementos en el servicio, como un índice, origen de datos o indizador.

No puede proporcionar una clave para que la use Azure Search. El servicio genera las claves de API.

```azurepowershell-interactive
New-AzSearchQueryKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -Name <query-key-name> 
```

## <a name="scale-replicas-and-partitions"></a>Escalado de réplicas y particiones

[**Set-AzSearchService**](https://docs.microsoft.com/powershell/module/az.search/set-azsearchservice?view=azps-1.4.0) se utiliza para [aumentar o reducir las réplicas y particiones](search-capacity-planning.md) para reajustar los recursos facturables dentro del servicio. El aumento de las réplicas o particiones se agrega a la factura, que tiene cargos fijos y variables. Si tiene una necesidad temporal de potencia de procesamiento adicional, puede aumentar las réplicas y particiones para controlar la carga de trabajo. El área de supervisión de la página Introducción del portal tiene iconos en la latencia de consulta, consultas por segundo y limitación, que indican si la capacidad actual es adecuada.

La incorporación o retirada de recursos puede tardar un unos minutos. Los ajustes de capacidad se producen en segundo plano, lo que permite que las cargas de trabajo existentes continúen. La capacidad adicional se usa para las solicitudes entrantes en cuanto esté lista, sin ninguna configuración adicional necesaria. 

La retirada de la capacidad puede ser perjudicial. Para evitar la eliminación de solicitudes, es recomendable detener todos los trabajos de indexación y del indexador antes de reducir la capacidad. Si esto no es factible, podría plantearse reducir la capacidad de forma incremental, una réplica y partición a la vez, hasta que se alcancen los nuevos niveles marcados como objetivo.

Una vez que envíe el comando, no hay forma de terminarlo a la mitad. Tendrá que esperar hasta que el comando finalice antes de revisar los recuentos.

```azurepowershell-interactive
Set-AzSearchService -ResourceGroupName <resource-group-name> -Name <search-service-name> -PartitionCount 6 -ReplicaCount 6
```

Los resultados deben tener un aspecto similar a la siguiente salida.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 6
PartitionCount    : 6
HostingMode       : Default
Id                : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```


## <a name="next-steps"></a>Pasos siguientes

Cree un [índice](search-what-is-an-index.md) y [consulte un índice](search-query-overview.md) mediante el portal, API REST o el SDK de .NET.

* [Creación de un índice de Azure Search en Azure Portal](search-create-index-portal.md)
* [Configuración de un indexador para cargar datos desde otros servicios](search-indexer-overview.md)
* [Consulta de un índice de Azure Search mediante el Explorador de búsqueda de Azure Portal](search-explorer.md)
* [Cómo usar Azure Search en .NET](search-howto-dotnet-sdk.md)