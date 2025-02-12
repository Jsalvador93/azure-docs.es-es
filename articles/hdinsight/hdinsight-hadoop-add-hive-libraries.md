---
title: Incorporación de bibliotecas de Apache Hive durante la creación de clústeres de HDInsight (Azure)
description: Aprenda a agregar bibliotecas de Apache Hive (archivos JAR) a un clúster de HDInsight durante la creación del clúster.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: c3ef5362c4d97b8d805212f9cf813c7bc9c8c18c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059447"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>Incorporación de bibliotecas personalizadas de Apache Hive al crear el clúster de HDInsight

Obtenga información sobre cómo cargar previamente las bibliotecas de [Apache Hive](https://hive.apache.org/) en HDInsight. Este documento contiene información sobre el uso de una acción de script para cargar previamente bibliotecas durante la creación del clúster. Las bibliotecas que se agregan mediante los pasos de este documento están disponibles globalmente en Hive: no hace falta usar [ADD JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) para cargarlos.

## <a name="how-it-works"></a>Cómo funciona

Al crear un clúster, puede usar una acción de script para modificar los nodos de clúster creados. El script de este documento acepta un único parámetro, que es la ubicación de las bibliotecas. Esta ubicación debe estar en una cuenta de Azure Storage, y las bibliotecas deben almacenarse como archivos jar.

Durante la creación del clúster, el script enumera los archivos, los copia en el directorio `/usr/lib/customhivelibs/` de los nodos principal y de trabajo y luego los agrega a la propiedad `hive.aux.jars.path` en el archivo `core-site.xml`. En los clústeres basados en Linux, también actualiza el archivo `hive-env.sh` con la ubicación de los archivos.

> [!NOTE]  
> El uso de las acciones de script de este artículo permite que las bibliotecas estén disponibles en las situaciones siguientes:
>
> * **HDInsight basado en Linux**: cuando se usa un cliente de Hive, **WebHCat** y **HiveServer2**.
> * **HDInsight basado en Windows**: cuando se usa el cliente de Hive y **WebHCat**.

## <a name="the-script"></a>La secuencia de comandos

**Ubicación del script**

Para **clústeres basados en Linux**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Para **clústeres basados en Windows**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

**Requisitos**

* Las scripts deben aplicarse tanto a los **nodos principales** como a los **nodos de trabajo**.

* Los archivos JAR que se van a instalar deben almacenarse en Azure Blob Storage en un **único contenedor**.

* La cuenta de almacenamiento que contiene la biblioteca de archivos JAR **debe** vincularse al clúster de HDInsight durante la creación. Debe ser la cuenta de almacenamiento predeterminada o una agregada a través de la __configuración opcional__.

* La ruta de acceso de WASB al contenedor debe especificarse como un parámetro para la acción de script. Por ejemplo, si los archivos JAR se almacenan en un contenedor llamado **libs** en una cuenta de almacenamiento llamada **mystorage**, el parámetro sería **wasb://libs\@mystorage.blob.core.windows.net/** .

  > [!NOTE]  
  > En este documento se supone que ha creado ya una cuenta de almacenamiento, contenedora de blobs, y ha cargado los archivos en ella.
  >
  > Si no ha creado una cuenta de almacenamiento, puede hacerlo a través de [Azure Portal](https://portal.azure.com). Después, puede usar una utilidad como el [Explorador de Azure Storage](https://storageexplorer.com/) para crear un contenedor en la cuenta y cargar archivos en él.

## <a name="create-a-cluster-using-the-script"></a>Creación de un clúster mediante el script

> [!NOTE]  
> Al seguir estos pasos, se crea un clúster de HDInsight basado en Linux. Para crear un clúster basado en Windows, seleccione **Windows** como sistema operativo del clúster en el momento de su creación y use el script de Windows (PowerShell) en lugar del script de Bash.
>
> También puede usar Azure PowerShell o el SDK de .NET para HDInsight para crear un clúster mediante este script. Para obtener más información sobre el uso de estos métodos, consulte [Personalización de clústeres de HDInsight mediante acciones de script](hdinsight-hadoop-customize-cluster-linux.md).

1. Inicie el aprovisionamiento de un clúster siguiendo los pasos que se describen en [Aprovisionamiento de clústeres de HDInsight en Linux](hdinsight-hadoop-provision-linux-clusters.md), pero no complete la operación.

2. En la sección **Configuración opcional**, seleccione **Acciones de script** y proporcione la información siguiente:

   * **NOMBRE**: escriba un nombre descriptivo para la acción de script.

   * **URI DE SCRIPT**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh.

   * **PRINCIPAL**: active esta opción.

   * **TRABAJO**: active esta opción.

   * **ZOOKEEPER**: Déjelo en blanco.

   * **PARÁMETROS**: escriba la dirección WASB que dirige al contenedor y la cuenta de almacenamiento que contiene los archivos JAR. Por ejemplo, **wasb://libs\@mystorage.blob.core.windows.net/** .

3. En la parte inferior de **Acciones de scripts**, use el botón **Seleccionar** para guardar la configuración.

4. En la sección **Configuración opcional**, seleccione **Cuentas de almacenamiento vinculadas** y luego el vínculo **Agregar una clave de almacenamiento**. Seleccione la cuenta de almacenamiento que contenga los archivos jar. Después, use los botones de **selección** para guardar la configuración y volver a **Configuración opcional**.

5. Para guardar la configuración opcional, use el botón **Seleccionar** situado en la parte inferior de la sección **Configuración opcional**.

6. Continúe aprovisionando el clúster tal como se describe en [Aprovisionamiento de clústeres de HDInsight en Linux](hdinsight-hadoop-provision-linux-clusters.md).

Una vez finalizada la creación del clúster, podrá utilizar los archivos JAR agregados a través de este script desde Hive sin tener que utilizar la instrucción `ADD JAR`.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca del uso con Hive, consulte [Uso de Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md)
