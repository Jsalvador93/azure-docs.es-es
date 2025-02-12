---
title: Integration Runtime en Azure Data Factory | Microsoft Docs
description: Obtenga más información sobre Integration Runtime en Azure Data Factory.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: abnarain
ms.openlocfilehash: ab82055d99401b0e7bc8aedf247fdf0d779f9f07
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875273"
---
# <a name="integration-runtime-in-azure-data-factory"></a>Integration Runtime en Azure Data Factory
Integration Runtime (IR) es la infraestructura de proceso que usa Azure Data Factory para proporcionar las siguientes capacidades de integración de datos en distintos entornos de red:

- **Data Flow**: ejecute una instancia de [Data Flow](concepts-data-flow-overview.md) en el entorno de proceso de Azure administrado.  
- **Movimiento de datos**: Copie los datos entre almacenes de datos en redes públicas y almacenes de datos en redes privadas (red privada local o virtual). Proporciona compatibilidad para conectores integrados, conversión de formato, asignación de columnas y transferencia de datos escalable y de rendimiento superior.
- **Distribución de actividades**:  distribuya y supervise actividades de transformación que se ejecuten en una gran variedad de servicios de proceso, como Azure Databricks, Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server, etc.
- **Ejecución de paquetes SSIS**: ejecute de forma nativa paquetes de SQL Server Integration Services (SSIS) en un entorno de proceso de Azure administrado.

En Data Factory, una actividad define la acción que se realizará. Un servicio vinculado define un almacén de datos o un servicio de proceso de destino. Una instancia de Integration Runtime proporciona el puente entre la actividad y los servicios vinculados.  La actividad o el servicio vinculado hace referencia a él, y proporciona el entorno de proceso donde se ejecuta la actividad o desde donde se distribuye. De esta manera, la actividad puede realizarse en la región más cercana posible al almacén de datos o servicio de proceso de destino de la manera con mayor rendimiento, a la vez que se satisfacen las necesidades de seguridad y cumplimiento.

## <a name="integration-runtime-types"></a>Tipos de instancias de Integration Runtime
Data Factory ofrece tres tipos de instancias de Integration Runtime, y debe elegir el tipo que atienda mejor las funcionalidades de integración de datos y las necesidades del entorno de red que está buscando.  Estos tres tipos son:

- Azure
- Autohospedado
- SSIS de Azure

En la tabla siguiente se describen las funcionalidades y la compatibilidad de red para cada uno de los tipos de instancias de Integration Runtime:

Tipo de IR | Red pública | Red privada
------- | -------------- | ---------------
Azure | Data Flow<br/>Movimiento de datos<br/>Distribución de actividades | &nbsp;
Autohospedado | Movimiento de datos<br/>Distribución de actividades | Movimiento de datos<br/>Distribución de actividades
SSIS de Azure | Ejecución de paquetes SSIS | Ejecución de paquetes SSIS

En el diagrama siguiente se muestra cómo pueden utilizarse los distintos tipos de instancias de Integration Runtime en combinación para ofrecer funcionalidades de integración de datos enriquecidas y compatibilidad con redes:

![Diferentes tipos de instancias de Integration Runtime](media/concepts-integration-runtime/different-integration-runtimes.png)

## <a name="azure-integration-runtime"></a>Tiempo de ejecución de integración de Azure
Una instancia de Integration Runtime de Azure es capaz de:

- Ejecución de flujos de datos en Azure 
- Ejecutar la actividad de copia entre los almacenes de datos en la nube
- Distribuir las siguientes actividades de transformación en la red pública: actividad Notebook, Jar o Python de Databricks, actividad Hive de HDInsight, actividad Pig de HDInsight, actividad MapReduce de HDInsight, actividad Spark de HDInsight, actividad Streaming de HDInsight, actividad de ejecución de lotes de Machine Learning, actividad de recurso de actualización de Machine Learning, actividad de procedimiento almacenado, actividad de U-SQL de Data Lake Analytics, actividad personalizada de .Net, actividad web, actividad de Búsqueda y actividad de Obtener metadatos.

### <a name="azure-ir-network-environment"></a>Entorno de red de Azure Integration Runtime
Azure Integration Runtime admite la conexión con almacenes de datos y servicios de proceso con puntos de conexión de acceso público. Use Integration Runtime autohospedado para el entorno de Azure Virtual Network.

### <a name="azure-ir-compute-resource-and-scaling"></a>Recursos de proceso y escalado de Azure Integration Runtime
Integration Runtime de Azure proporciona un proceso completamente administrado y sin servidor en Azure.  No tiene que preocuparse del aprovisionamiento de la infraestructura, la instalación de software, la aplicación de revisiones ni el escalado de la capacidad.  Además, solo se paga por la duración de la utilización real.

Integration Runtime de Azure proporciona el proceso nativo para mover datos entre almacenes de datos en la nube de forma segura, confiable y con alto rendimiento.  Puede establecer cuántas unidades de integración de datos se usarán en la actividad de copia, y el tamaño de proceso de Integration Runtime de Azure se escala verticalmente de manera elástica en consecuencia sin tener que ajustar de manera explícita el tamaño de Integration Runtime de Azure. 

La distribución de actividades es una operación ligera para enrutar la actividad al servicio de proceso de destino, por lo que no hay necesidad de escalar verticalmente el tamaño de proceso para este escenario.

Para obtener información sobre cómo crear y configurar una instancia de Integration Runtime de Azure, consulte cómo crear y configurar una instancia de Integration Runtime de Azure en las guías de procedimientos. 

> [!NOTE] 
> Azure Integration Runtime tiene propiedades relacionadas con el flujo de datos en el entorno de ejecución de Data Flow, que define la infraestructura de proceso subyacente que se usaría para ejecutar los flujos de datos. 

## <a name="self-hosted-integration-runtime"></a>Integration Runtime autohospedado
Una instancia de Integration Runtime autohospedado es capaz de:

- Ejecutar la actividad de copia entre los almacenes de datos en la nube y un almacén de datos en la red privada.
- Distribuir las siguientes actividades de transformación frente a los recursos de proceso en la red local o Azure Virtual Network: actividad Hive de HDInsight (BYOC, traiga su propio clúster), actividad Pig de HDInsight (BYOC), actividad MapReduce de HDInsight (BYOC), actividad Spark de HDInsight (BYOC), actividad Streaming de HDInsight (BYOC), actividad de ejecución de lotes de Machine Learning, actividades de recurso de actualización de Machine Learning, actividad de procedimiento almacenado, actividad de U-SQL de Data Lake Analytics, actividad personalizada (se ejecuta en Azure Batch), actividad de Búsqueda y actividad de Obtener metadatos.

> [!NOTE] 
> Use Integration Runtime autohospedado para admitir almacenes de datos que requieran bring-your-own driver (BYOD), como SAP Hana, MySQL, etc.  Para obtener más información, consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats).

### <a name="self-hosted-ir-network-environment"></a>Entorno de red de IR autohospedado
Si desea realizar la integración de datos de manera segura en un entorno de red privada, que no tiene una línea de vista directa desde el entorno de nube pública, puede instalar IR autohospedado en el entorno local detrás del firewall corporativo, o dentro de un red privada virtual.  Integration Runtime autohospedado establece conexiones salientes basadas en HTTP para abrir Internet.

### <a name="self-hosted-ir-compute-resource-and-scaling"></a>Recursos de proceso y escalado de IR autohospedado
IR autohospedado debe instalarse en un equipo local o en una máquina virtual dentro de una red privada. Actualmente, solo se admite ejecutar instancias de Integration Runtime autohospedados en un sistema operativo Windows.  

Para una alta disponibilidad y escalabilidad, puede escalar horizontalmente IR autohospedado mediante la asociación de la instancia lógica con varias máquinas locales en modo activo-activo.  Para más información, consulte el artículo sobre cómo [crear y configurar un IR autohospedado](create-self-hosted-integration-runtime.md) en las guías de procedimientos.

## <a name="azure-ssis-integration-runtime"></a>Integration Runtime de SSIS de Azure
Para levantar y mover la carga de trabajo de SSIS existente, puede crear una instancia de Integration Runtime de SSIS de Azure para ejecutar paquetes SSIS de forma nativa.

### <a name="azure-ssis-ir-network-environment"></a>Entorno de red de una instancia de Integration Runtime para la integración de SSIS en Azure
Integration Runtime de SSIS de Azure se puede aprovisionar en la red pública o la red privada.  El acceso a datos locales se admite mediante la combinación de Integration Runtime de SSIS de Azure a una red virtual que está conectada a la red local.  

### <a name="azure-ssis-ir-compute-resource-and-scaling"></a>Recursos de proceso y escalado de una instancia de Integration Runtime para la integración de SSIS en Azure
Integration Runtime de SSIS de Azure es un clúster totalmente administrado de máquinas virtuales de Azure dedicado a ejecutar los paquetes de SSIS. Puede hacer que su propio servidor de Azure SQL Database o Instancia administrada hospede el catálogo de proyectos y paquetes de SSIS (SSISDB) al que se va a adjuntar. Puede escalar verticalmente la eficacia de los procesos especificando el tamaño de nodo y escalar horizontalmente especificando el número de nodos del clúster. Puede administrar el coste de funcionamiento de Integration Runtime de SSIS de Azure deteniéndolo e iniciándolo según lo considere oportuno.

Para más información, consulte el artículo sobre cómo crear y configurar un IR de SSIS de Azure en las guías de procedimientos.  Una vez creado, puede implementar y administrar los paquetes de SSIS existentes con poco o ningún cambio utilizando herramientas conocidas, como SQL Server Data Tools (SSDT) y SQL Server Management Studio (SSMS), de manera similar a usar SSIS de forma local.

Consulte los siguientes artículos para más información sobre Integration Runtime de SSIS de Azure: 

- [Tutorial: Implementación de paquetes SSIS en Azure](tutorial-create-azure-ssis-runtime-portal.md). En este artículo se proporcionan instrucciones paso a paso para crear una instancia de IR de SSIS de Azure y se usa una base de datos de Azure SQL para hospedar el catálogo de SSIS. 
- [Instrucciones: Creación de una instancia de Integration Runtime de SSIS de Azure](create-azure-ssis-integration-runtime.md). Este artículo amplía el tutorial y proporciona instrucciones acerca del uso de Instancia administrada de Azure SQL Database y cómo unir un entorno de ejecución de integración a una red virtual. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Supervisión de una instancia de Integration Runtime de SSIS de Azure). En este artículo se muestra cómo recuperar información sobre una instancia de IR de SSIS de Azure, junto con descripciones de los estados en la información devuelta. 
- [Administración de Integration Runtime de SSIS de Azure](manage-azure-ssis-integration-runtime.md). En este artículo se muestra cómo detener, iniciar o quitar una instancia de IR de SSIS de Azure. También se muestra cómo escalar horizontalmente IR de SSIS de Azure mediante la adición de más nodos a IR. 
- [Unión de una instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md). En este artículo se ofrece información conceptual sobre cómo unir una instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual de Azure. También se proporcionan los pasos para configurar la red virtual mediante Azure Portal para que se una la instancia de Integration Runtime de SSIS de Azure. 

## <a name="integration-runtime-location"></a>Ubicación de Integration Runtime
La ubicación de Data Factory es donde se almacenan los metadatos de la factoría de datos y desde donde se inicia el desencadenamiento de la canalización. Sin embargo, una factoría de datos puede acceder a almacenes de datos y a servicios de proceso en otras regiones de Azure para mover datos entre los almacenes de datos o para procesar datos mediante servicios de proceso. Este comportamiento se lleva a cabo a través de la instancia de [Integration Runtime disponible globalmente](https://azure.microsoft.com/global-infrastructure/services/) para garantizar el cumplimiento de los datos, la eficacia y los menores costes de salida de la red.

La ubicación de Integration Runtime define la ubicación de su proceso de back-end y, en esencia, la ubicación donde se realizan el movimiento de datos, la distribución de actividades y la ejecución de paquetes de SSIS. La ubicación de Integration Runtime puede ser diferente de la ubicación de la factoría de datos a la que pertenece. 

### <a name="azure-ir-location"></a>Ubicación de Azure Integration Runtime
Puede establecer una ubicación determinada para una instancia de Azure Integration Runtime, en cuyo caso el movimiento de datos y la distribución de actividades se producirá en esa región específica. 

Si opta por utilizar la **resolución automática de Azure Integration Runtime**, que es la opción predeterminada: 

- En la actividad de copia, ADF intentará detectar automáticamente el receptor y el almacén de datos de origen para elegir la mejor ubicación en la misma región si está disponible, o la más cercana en la misma ubicación geográfica, o si esta no se puede detectar, para usar la región de la factoría de datos como alternativa.

- Para la ejecución de las actividades de Búsqueda, Obtener metadatos o Eliminar (también conocidas como actividades de canalización), la distribución de actividades de transformación (también conocidas como actividades externas) y las operaciones de creación (probar conexión, examinar lista de carpetas y lista de tablas, obtener una vista previa de datos), ADF usará IR en la región de la factoría de datos.

- Para Data Flow, ADF usará IR en la región de la factoría de datos. 

  > [!TIP] 
  > Una buena práctica sería asegurarse de que Data Flow se ejecuta en la misma región que los almacenes de datos correspondiente (si es posible). Puede conseguirlo mediante la resolución automática de Azure IR (si la ubicación del almacén de datos coincide con la ubicación de Data Factory), o mediante la creación de una nueva instancia de Azure IR en la misma región que los almacenes de datos y la posterior ejecución del flujo de datos en ella. 

Puede supervisar qué ubicación de IR se usa durante la ejecución de la actividad en la vista de supervisión de actividades de la canalización en la interfaz de usuario, o en la carga de supervisión de actividades.

>[!TIP]
>Si tiene requisitos estrictos de cumplimiento de datos y debe garantizar que los datos no salen de una determinada región geográfica, puede crear explícitamente una instancia de Azure IR en una determinada región y dirigir el servicio vinculado a esta instancia de IR con la propiedad ConnectVia. Por ejemplo, si desea copiar datos de un blob de la región Sur de Reino Unido en SQL DW de esa misma región y desea garantizar que los datos no salen del Reino Unido, cree una instancia de Azure IR en Reino Unido y vincule ambos servicios vinculados a ella.

### <a name="self-hosted-ir-location"></a>Ubicación de IR autohospedado
IR autohospedado se registra lógicamente en Data Factory, y el usuario proporciona el proceso que se usa para admitir sus capacidades. Por lo tanto, no hay ninguna propiedad de ubicación explícita para la instancia de Integration Runtime autohospedado. 

Cuando se utiliza para realizar el movimiento de datos, IR autohospedado extrae datos del origen y los escribe en el destino.

### <a name="azure-ssis-ir-location"></a>Ubicación de la instancia de Integration Runtime para la integración de SSIS en Azure
Al seleccionar la ubicación adecuada para Integration Runtime de SSIS de Azure, es esencial lograr un alto rendimiento en los flujos de trabajo de extracción, transformación y carga (ETL).

- La ubicación de Azure-SSIS Integration Runtime no es preciso que sea la misma que la ubicación de la factoría de datos, pero debe ser la misma que la ubicación de su propio servidor de la Instancia administrada de Azure SQL Database donde se hospedará la SSISDB. De esta manera, Integration Runtime de SSIS de Azure puede acceder fácilmente a SSISDB sin incurrir en un tráfico excesivo entre las distintas ubicaciones.
- Si no tiene un servidor existente de Instancia administrada de Azure SQL Database para hospedar la SSISDB, pero tiene orígenes y destinos de datos locales, debe crear un nuevo servidor de Instancia administrada de Azure SQL Database en la misma ubicación de una red virtual conectada a la red local.  De esta manera, puede crear la instancia de Azure-SSIS Integration Runtime mediante el nuevo servidor de Instancia administrada o Azure SQL Database, y unirse a esa red virtual, todo en la misma ubicación, lo que minimiza de manera eficaz los movimientos de datos entre distintas ubicaciones.
- Si la ubicación de su servidor existente de Instancia administrada de Azure SQL Database donde se hospeda la SSISDB no es la misma que la ubicación de una red virtual conectada a la red local, cree primero la instancia de Azure-SSIS Integration Runtime con un servidor existente de Instancia administrada de Azure SQL Database, únase a otra red virtual en la misma ubicación y, a continuación, configure una conexión entre redes virtuales en distintas ubicaciones.

En el diagrama siguiente se muestra la configuración de la ubicación de Data Factory y sus instancias de Integration Runtime:

![Ubicación de Integration Runtime](media/concepts-integration-runtime/integration-runtime-location.png)

## <a name="determining-which-ir-to-use"></a>Determinar qué instancias de Integration Runtime usar

### <a name="copy-activity"></a>Actividad de copia

Para la actividad de copia, necesita servicios vinculados de origen y receptor para definir la dirección del flujo de datos. Se utiliza la lógica siguiente para determinar qué instancia de Integration Runtime se utiliza para realizar la copia: 

- **Copia entre dos orígenes de datos en la nube**: si el servicio vinculado de origen y el receptor usan Azure IR, ADF usará la instancia regional de Azure IR, si la especificó, o determinará automáticamente una ubicación para Azure IR si decidió usar la opción de resolución automática de IR (la opción predeterminada) como se ha descrito en la sección [Ubicación de Integration Runtime](#integration-runtime-location).
- **Copia de datos entre un origen de datos en la nube y un origen de datos en una red privada**: si el servicio vinculado de origen o de receptor apunta a una instancia de IR autohospedado, la actividad de copia se ejecuta en esa instancia de Integration Runtime autohospedado.
- **Copia entre dos orígenes de datos en una red privada**: el servicio vinculado de origen y de receptor deben apuntar a la misma instancia de Integration Runtime y esa instancia se usa para ejecutar la actividad de copia.

### <a name="lookup-and-getmetadata-activity"></a>Actividad Lookup y GetMetadata

La actividad Lookup y GetMetadata se ejecuta en el entorno de ejecución de integración asociado al servicio vinculado de almacén de datos.

### <a name="transformation-activity"></a>Actividad de transformación

Cada actividad de transformación tiene un destino de servicio vinculado de proceso de destino, que apunta a una instancia de Integration Runtime. Esta instancia de Integration Runtime es desde donde se distribuye la actividad de transformación.

### <a name="data-flow-activity"></a>Actividad Data Flow

La actividad Data Flow se ejecuta en el entorno de ejecución de integración asociado a ella. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Cómo crear y configurar una instancia de Azure Integration Runtime](create-azure-integration-runtime.md)
- [Crear Integration Runtime autohospedado](create-self-hosted-integration-runtime.md)
- [Creación de una instancia de Integration Runtime de SSIS de Azure](create-azure-ssis-integration-runtime.md). Este artículo amplía el tutorial y proporciona instrucciones acerca del uso de Instancia administrada de Azure SQL Database y cómo unir un entorno de ejecución de integración a una red virtual. 
