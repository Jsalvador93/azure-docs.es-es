---
title: Incorporación de tolerancia a errores en la actividad de copia de Azure Data Factory a través de la omisión de filas incompatibles | Microsoft Docs
description: Obtenga información sobre cómo agregar tolerancia a errores en la actividad de copia de Azure Data Factory a través de la omisión de filas incompatibles durante la copia
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 3a255b21e8bfd7d78954603e9aa6e5ca39cee95b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60566078"
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Incorporación de tolerancia a errores en la actividad de copia a través de la omisión de filas incompatibles

> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
> * [Versión 1](data-factory-copy-activity-fault-tolerance.md)
> * [Versión 2 (versión actual)](../copy-activity-fault-tolerance.md)

> [!NOTE]
> Este artículo se aplica a la versión 1 de Data Factory. Si usa la versión actual del servicio Data Factory, consulte el artículo acerca de la [tolerancia a errores en la actividad de copia de Data Factory](../copy-activity-fault-tolerance.md).

La [actividad de copia](data-factory-data-movement-activities.md) de Azure Data Factory ofrece dos maneras para tratar con filas incompatibles cuando se copian datos entre almacenes de datos de origen y de receptor:

- Puede anular y omitir la actividad de copia cuando se encuentren datos incompatibles (comportamiento predeterminado).
- Se puede continuar copiando todos los datos a través de la incorporación de tolerancia a errores y la omisión de filas de datos incompatibles. Además, puede registrar las filas incompatibles en Azure Blob Storage. Luego puede examinar el registro para obtener información sobre la causa del error, corregir los datos en el origen de datos y reintentar la actividad de copia.

## <a name="supported-scenarios"></a>Escenarios admitidos
La actividad de copia admite tres escenarios para detectar, omitir y registrar datos incompatibles:

- **Incompatibilidad entre el tipo de datos de origen y el tipo nativo de receptor**

    Por ejemplo:  Copie datos desde un archivo CSV en Blob Storage a una base de datos SQL con una definición de esquema que contenga tres columnas de tipo **INT**. Las filas del archivo CSV que contienen datos numéricos, como `123,456,789`, se copian correctamente en el almacén de receptor. Sin embargo, las filas que contienen valores no numéricos, como `123,456,abc`, se detectan como incompatibles y se omiten.

- **Error de coincidencia en el número de columnas entre el origen y el receptor**

    Por ejemplo:  Copie datos desde un archivo CSV en Blob Storage a una base de datos SQL con una definición de esquema que contenga seis columnas. Las filas del archivo CSV que contiene seis columnas se copian correctamente en el almacén de receptor. Las filas del archivo CSV que contienen más o menos de seis columnas se detectan como incompatibles y se omiten.

- **Infracción de clave principal al escribir en SQL Server, Azure SQL Database o Azure Cosmos DB**

    Por ejemplo:  Copie datos desde un servidor SQL a una base de datos SQL. Se define una clave principal en la base de datos SQL de receptor, pero no se define en el servidor SQL de origen. Las filas duplicadas que existen en el origen no se pueden copiar en el receptor. La actividad de copia solo copia la primera fila de los datos de origen en el receptor. Las filas de origen subsiguientes que contienen el valor de clave principal duplicado se detectan como incompatibles y se omiten.

>[!NOTE]
>Esta característica no se aplica cuando la actividad de copia se configura para invocar el mecanismo de carga de datos externos, incluido [PolyBase en Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) o [Unload en Amazon Redshift](data-factory-amazon-redshift-connector.md#use-unload-to-copy-data-from-amazon-redshift). Para cargar datos en SQL Data Warehouse mediante PolyBase, use la compatibilidad de tolerancia a errores nativa de PolyBase especificando "[polyBaseSettings](data-factory-azure-sql-data-warehouse-connector.md#sqldwsink)" en la actividad de copia.

## <a name="configuration"></a>Configuración
En el ejemplo siguiente se proporciona una definición JSON para configurar la omisión de las filas incompatibles en la actividad de copia:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },
    "enableSkipIncompatibleRow": true,
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Propiedad | DESCRIPCIÓN | Valores permitidos | Obligatorio |
| --- | --- | --- | --- |
| **enableSkipIncompatibleRow** | Habilite si lo desea la omisión de filas incompatibles durante la copia. | True<br/>False (valor predeterminado) | Sin |
| **redirectIncompatibleRowSettings** | Un grupo de propiedades que puede especificarse cuando quiere registrar las filas incompatibles. | &nbsp; | Sin |
| **linkedServiceName** | El servicio vinculado de Azure Storage para almacenar el registro que contiene las filas que se omiten. | El nombre de un servicio vinculado de [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) o [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service), que hace referencia a la instancia de almacenamiento que desea usar para almacenar el archivo de registro. | Sin |
| **path** | La ruta de acceso del archivo de registro que contiene las filas que se omiten. | Especifique la ruta de acceso de Blob Storage que desee usar para registrar los datos incompatibles. Si no se proporciona una ruta de acceso, el servicio creará un contenedor para usted. | Sin |

## <a name="monitoring"></a>Supervisión
Una vez que se completa la ejecución de la actividad de copia, puede ver el número de filas omitidas en la sección de supervisión:

![Supervisión de filas incompatibles omitidas](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Si configura el registro de las filas incompatibles, puede encontrar el archivo de registro en esta ruta de acceso: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` En el archivo de registro, verá las filas que se omitieron y la causa principal de la incompatibilidad.

Los datos originales y el error correspondiente se registran en el archivo. A continuación se muestra un ejemplo del contenido del archivo de registro:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre la actividad de copia de Azure Data Factory, consulte [Movimiento de datos con la actividad de copia](data-factory-data-movement-activities.md).
