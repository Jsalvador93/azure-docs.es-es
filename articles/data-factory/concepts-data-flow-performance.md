---
title: Guía de optimización y rendimiento de la asignación de instancias de Data Flow en Azure Data Factory | Microsoft Docs
description: Conozca los factores clave que afectan al rendimiento de los flujos de datos en Azure Data Factory cuando se usa la asignación de instancias de Data Flow.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 09/22/2019
ms.openlocfilehash: e4b3e08c0cc7fc1ead2aed551c228c6a1165c3b6
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71180859"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Guía de optimización y rendimiento de la asignación de instancias de Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

La asignación de instancias de Data Flow de Azure Data Factory proporciona una interfaz de explorador sin código para diseñar, implementar y orquestar transformaciones de datos a escala.

> [!NOTE]
> Si no está familiarizado con la asignación de instancias de Data Flow de ADF, vea [Información general de la asignación de instancias de Data Flow](concepts-data-flow-overview.md) antes de leer este artículo.
>

> [!NOTE]
> Al diseñar y probar flujos de datos desde la interfaz de usuario de ADF, asegúrese de activar el conmutador de depuración para poder ejecutar los flujos de datos en tiempo real sin esperar a que se prepare un clúster.
>

![Botón Depurar](media/data-flow/debugb1.png "Depurar")

## <a name="monitor-data-flow-performance"></a>Supervisión del rendimiento de flujo de datos

Al diseñar los flujos de datos de asignación en el explorador, puede realizar pruebas unitarias de cada transformación individual si hace clic en la pestaña de vista previa de datos en el panel de configuración de la parte inferior de cada transformación. El siguiente paso que debe realizar consiste en probar el flujo de datos de un extremo a otro en el Diseñador de canalizaciones. Agregue una actividad de ejecución de Data Flow y use el botón de depuración para probar el rendimiento del flujo de datos. En el panel inferior de la ventana de la canalización, verá un icono de gafas bajo "acciones":

![Supervisión de Data Flow](media/data-flow/mon002.png "Monitor 2 de Data Flow")

Al hacer clic en ese icono se mostrará el plan de ejecución y el perfil de rendimiento siguiente del flujo de datos. Puede usar esta información para calcular el rendimiento del flujo de datos en orígenes de datos de diferente tamaño. Tenga en cuenta que puede suponer un minuto de tiempo de configuración de ejecución del trabajo de clúster en los cálculos de rendimiento general y, si usa la instancia predeterminada de Azure Integration Runtime, es posible que también tenga que agregar cinco minutos de tiempo de desarrollo del clúster.

![Supervisión de Data Flow](media/data-flow/mon003.png "Monitor 3 de Data Flow")

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Optimización para Azure SQL Database y Azure SQL Data Warehouse

![Elemento de origen](media/data-flow/sourcepart3.png "Source Part")

### <a name="partition-your-source-data"></a>Partición de los datos de origen

* Vaya a "Optimizar" y seleccione "Origen". Establezca una columna de una tabla específica o un tipo en una consulta.
* Si ha elegido "columna", seleccione la columna de partición.
* Además, establezca el número máximo de conexiones a la instancia de Azure SQL DB. Puede probar un valor más alto para obtener conexiones en paralelo a la base de datos. Pero algunos casos pueden provocar un rendimiento más rápido con un número limitado de conexiones.
* No es necesario crear particiones de las tablas de la base de datos de origen.
* Si se establece una consulta en la transformación de origen que coincida con el esquema de creación de particiones de la tabla de base de datos, el motor de base de datos de origen podrá aprovechar la eliminación de particiones.
* Si todavía no se han creado particiones en el origen, ADF seguirá usando la creación de particiones en el entorno de transformación de Spark en función de la clave que seleccione en la transformación de origen.

### <a name="set-batch-size-and-query-on-source"></a>Establecimiento del tamaño de lote y la consulta en el origen

![Origen](media/data-flow/source4.png "Source")

* Al establecer el tamaño de lote se indicará a ADF que almacene los datos en conjuntos en memoria en lugar de hacerlo fila a fila. Es una configuración opcional y es posible que se quede sin recursos en los nodos de proceso si no tienen el tamaño correcto.
* El establecimiento de una consulta puede permitir filtrar las filas directamente en el origen incluso antes de que lleguen a Data Flow para su procesamiento, lo que puede acelerar la adquisición de datos inicial.
* Si usa una consulta, puede agregar sugerencias de consulta opcionales para Azure SQL DB, como READ UNCOMMITTED.

### <a name="set-isolation-level-on-source-transformation-settings-for-sql-datasets"></a>Establecimiento del nivel de aislamiento en la configuración de la transformación de origen para conjuntos de datos SQL

* La lectura sin confirmar proporcionará resultados de consulta más rápidos en la transformación de origen.

![Nivel de aislamiento](media/data-flow/isolationlevel.png "Isolation Level")

### <a name="set-sink-batch-size"></a>Establecimiento del tamaño de lote del receptor

![Receptor](media/data-flow/sink4.png "Sink")

* Para evitar el procesamiento fila por fila de los flujos de datos, establezca el "Tamaño del lote" en la configuración del receptor de Azure SQL DB. Esto indicará a ADF que procese las operaciones de escritura de base de datos en lotes según el tamaño especificado.

### <a name="set-partitioning-options-on-your-sink"></a>Establecimiento de opciones de creación de particiones en el receptor

* Incluso si no ha creado particiones de los datos en las tablas de Azure SQL DB de destino, vaya a la pestaña Optimizar y establezca la creación de particiones.
* A menudo, el mero hecho de indicar a ADF que use la creación de particiones Round Robin en los clústeres de ejecución de Spark genera una carga de datos mucho más rápida, en lugar de forzar todas las conexiones desde una sola partición o un solo nodo.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Aumento del tamaño del motor de proceso en Azure Integration Runtime

![Nuevo IR](media/data-flow/ir-new.png "New IR")

* Aumente el número de núcleos, lo que incrementará el número de nodos y le proporcionará más capacidad de procesamiento para consultar la instancia de Azure SQL DB y escribir en ella.
* Pruebe las opciones "Optimizado para proceso" y "Optimizado para memoria" para aplicar más recursos a los nodos de proceso.

### <a name="unit-test-and-performance-test-with-debug"></a>Pruebas unitarias y de rendimiento con la depuración

* Al realizar pruebas unitarias de los flujos de datos, establezca el botón "Data Flow Debug" (Depuración de flujo de datos) en ON.
* En el Diseñador de Data Flow, use la pestaña Data Preview (Vista previa de los datos) en las transformaciones para ver los resultados de la lógica de transformación.
* Para realizar pruebas unitarias de los flujos de datos desde el diseñador de canalizaciones, coloque una actividad de Data Flow en el lienzo de diseño de la canalización y use el botón "Debug" (Depurar) para realizar la prueba.
* La realización de pruebas en modo de depuración funcionará en un entorno de clúster preparado activo, sin necesidad de esperar al desarrollo just-in-time del clúster.
* Durante la depuración de la vista previa de datos dentro de la experiencia del diseñador de Data Flow, puede limitar la cantidad de datos que se prueban para cada origen. Para ello, establezca el límite de filas en el vínculo de configuración de depuración en la interfaz de usuario del diseñador del flujo de datos. Tenga en cuenta que primero debe activar el modo de depuración.

![Configuración de depuración](media/data-flow/debug-settings.png "Debug Settings")

* Al probar los flujos de datos desde una ejecución de depuración de canalización, puede limitar el número de filas que se usan para las pruebas estableciendo el tamaño de muestreo en cada uno de los orígenes. Asegúrese de deshabilitar el muestreo al programar las canalizaciones según una programación operativa normal.

![Muestreo de filas](media/data-flow/source1.png "Row Sampling")

### <a name="disable-indexes-on-write"></a>Deshabilitación de índices durante la escritura
* Use una actividad de procedimiento almacenado de canalización de ADF antes de la actividad de Data Flow que deshabilita los índices en las tablas de destino en las que se escribe desde el receptor.
* Después de la actividad de Data Flow, agregue otra actividad de procedimiento almacenado que habilite estos índices.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Aumento del tamaño de la instancia de Azure SQL DB
* Programe un cambio de tamaño del origen y el receptor de Azure SQL DB antes de ejecutar la canalización para aumentar el rendimiento y minimizar la limitación de Azure una vez que se alcancen los límites de DTU.
* Una vez completada la ejecución de la canalización, puede cambiar el tamaño de las bases de datos a la velocidad de ejecución normal.

## <a name="optimizing-for-azure-sql-data-warehouse"></a>Optimización para Azure SQL Data Warehouse

### <a name="use-staging-to-load-data-in-bulk-via-polybase"></a>Uso de almacenamiento provisional para cargar datos de forma masiva a través de Polybase

* Para el procesamiento fila por fila de los flujos de datos, establezca la opción "Staging" (Almacenamiento provisional) en la configuración del receptor para que ADF pueda aprovechar Polybase para evitar las inserciones fila por fila en el almacenamiento de datos. Esto indicará a ADF que use Polybase con el fin de poder cargar los datos de forma masiva.
* Al ejecutar la actividad de flujo de datos desde una canalización, con el almacenamiento provisional activado, tendrá que seleccionar la ubicación del almacén de blobs de los datos de almacenamiento provisional para la carga de forma masiva.

### <a name="increase-the-size-of-your-azure-sql-dw"></a>Aumento del tamaño de la instancia de Azure SQL DW

* Programe un cambio de tamaño del origen y el receptor de Azure SQL DW antes de ejecutar la canalización para aumentar el rendimiento y minimizar la limitación de Azure una vez que se alcancen los límites de DWU.

* Una vez completada la ejecución de la canalización, puede cambiar el tamaño de las bases de datos a la velocidad de ejecución normal.

## <a name="optimize-for-files"></a>Optimización para archivos

* Puede controlar cuántas particiones va a usar esa instancia de ADF. En cada transformación de origen y receptor, así como en cada transformación individual, puede establecer un esquema de creación de particiones. Para los archivos más pequeños, comprobará que seleccionar "Partición única" en ocasiones funciona mejor y más rápido que solicitar a Spark que cree particiones de los archivos pequeños.
* Si no tiene información suficiente sobre los datos de origen, puede elegir la creación de particiones "Round Robin" y establecer el número de particiones.
* Si examina los datos y descubre que tiene columnas que pueden ser claves hash correctas, use la opción de creación de particiones por hash.
* Al depurar en la vista previa de datos y la depuración de la canalización, tenga en cuenta que el límite y los tamaños de muestreo de los conjuntos de datos de origen basados en archivos solo se aplican al número de filas devueltas, no al número de filas leídas. Es importante tener esto en cuenta, ya que puede afectar al rendimiento de sus ejecuciones de depuración y hacer, posiblemente, hacer que se produzca un error en el flujo.
* Recuerde que los clústeres de depuración son pequeños clústeres de un solo nodo de forma predeterminada, de modo que use archivos pequeños temporales para la depuración. Vaya a Configuración de depuración y seleccione un pequeño subconjunto de sus datos mediante un archivo temporal.

![Configuración de depuración](media/data-flow/debugsettings3.png "Debug Settings")

### <a name="file-naming-options"></a>Opciones de nombre de archivo

* La naturaleza predeterminada de la escritura de datos transformados en la asignación de instancias de Data Flow de ADF consiste en escribir en un conjunto de datos que tiene un blob o un servicio vinculado de ADLS. Debe establecer ese conjunto de datos para que apunte a una carpeta o un contenedor, no a un archivo con nombre.
* Las instancias de Data Flow usan Spark de Azure Databricks para la ejecución, lo que significa que la salida se dividirá en varios archivos, en función de la creación de particiones de Spark predeterminada o del esquema de creación de particiones que haya elegido de forma explícita.
* Una operación muy común en las instancias de Data Flow de ADF consiste en elegir "Output to single file" (Salida a un único archivo) para que todos los archivos PART de salida se combinen en uno solo.
* Pero esta operación requiere que la salida se reduzca a una única partición en un único nodo de clúster.
* Téngalo en cuenta al elegir esta opción habitual. Si combina muchos archivos de origen grandes en una única partición de archivo de salida puede quedarse sin recursos de nodo de clúster.
* Para evitar que se agoten los recursos de nodo de proceso, puede mantener el esquema de creación de particiones predeterminado explícito en ADF, que optimiza el rendimiento, y después agregar una actividad de copia posterior en la canalización que combine todos los archivos PART de la carpeta de salida en un único archivo nuevo. En esencia, esta técnica separa la acción de transformación de la combinación de los archivos y consigue el mismo resultado que la configuración de "salida a un archivo único".

### <a name="looping-through-file-lists"></a>Bucles a través de listas de archivos

En la mayoría de los casos, los flujos de datos de ADF se ejecutarán mejor desde una canalización que permita la transformación del origen de Data Flow para iteración en varios archivos. En otras palabras, es preferible usar caracteres comodines o listas de archivos dentro del origen en Data Flow que iterar a través de una lista grande de archivos con ForEach en la canalización, llamando a una actividad de ejecución de Data Flow en cada iteración. El proceso de Data Flow se ejecutará más rápido si se permiten bucles dentro de Data Flow.

Por ejemplo, si tengo una lista de archivos de datos de julio de 2019 que quiero procesar en una carpeta en Blob Storage, sería más eficaz llamar a una actividad de ejecución de Data Flow una vez desde la canalización y usar un carácter comodín en el origen, como este:

```DateFiles/*_201907*.txt```

Esto funcionará mejor que una búsqueda contra el almacén de blobs en una canalización que luego itera en todos los archivos coincidentes con ForEach que incluya una actividad de ejecución de Data Flow.

### <a name="increase-the-size-of-your-debug-cluster"></a>Aumento del tamaño del clúster de depuración

De forma predeterminada, al activar la depuración se usará el entorno Azure Integration Runtime predeterminado que se crea automáticamente para cada factoría de datos. Este Azure IR predeterminado se establece para 8 núcleos, 4 para un nodo de controlador y 4 para un nodo de trabajo, mediante propiedades de proceso generales. A medida que pruebe con datos de mayor tamaño, puede aumentar el tamaño del clúster de depuración mediante la creación de un nuevo Azure IR con configuraciones más grandes y elegir este nuevo Azure IR al activar la depuración. Esto indicará a ADF que use este Azure IR para la vista previa de los datos y la depuración de la canalización con flujos de datos.

## <a name="next-steps"></a>Pasos siguientes

Vea el resto de artículos sobre Data Flow relacionados con el rendimiento:

- [Pestaña Optimizar de la transformación de Mapping Data Flow](concepts-data-flow-optimize-tab.md)
- [Actividad Data Flow](control-flow-execute-data-flow-activity.md)
- [Supervisión del rendimiento de flujo de datos](concepts-data-flow-monitoring.md)
