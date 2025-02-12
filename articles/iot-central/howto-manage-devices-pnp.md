---
title: Administración de dispositivos en una aplicación de Azure IoT Central | Microsoft Docs
description: Como operador, aprenda a administrar dispositivos en la aplicación de Azure IoT Central.
author: sarahhubbard
ms.author: sahubbar
ms.date: 07/17/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 5c39a8190ea91ac5a4306a27c34fef321db7d169
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879264"
---
# <a name="manage-devices-in-your-azure-iot-central-application-preview-features"></a>Administración de dispositivos en una aplicación de Azure IoT Central (características de versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este artículo se describe cómo administrar dispositivos en la aplicación de Azure IoT Central como operador. Como operador, puede hacer lo siguiente:

- Usar la página **Devices** (Dispositivos) para ver, agregar y eliminar los dispositivos conectados a una aplicación de Azure IoT Central.
- Mantener un inventario actualizado de los dispositivos.
- Mantener actualizados los metadatos del dispositivo cambiando los valores almacenados en las propiedades del dispositivo desde sus vistas.
- Controlar el comportamiento de los dispositivos mediante la actualización de una configuración en un dispositivo específico desde sus vistas.

## <a name="view-your-devices"></a>Visualización de los dispositivos

Para ver un dispositivo determinado:

1. Elija **Devices** (Dispositivos) en el menú de navegación de la izquierda. Verá una lista con todos los dispositivos y sus plantillas de dispositivo.

1. Elija una plantilla.

1. En el panel derecho de la página **Devices** (Dispositivos), verá una lista de los dispositivos creados con esa plantilla. Elija un dispositivo para ver la página de detalles de ese dispositivo:

    ![Página Detalles del dispositivo](./media/howto-manage-devices-pnp/devicelist.png)


## <a name="add-a-device"></a>Agregar un dispositivo

Para agregar un dispositivo real a una aplicación de Azure IoT Central:

1. Elija **Devices** (Dispositivos) en el menú de navegación de la izquierda.

1. Elija la plantilla de dispositivo desde la que va a crear un dispositivo.

1. Elija + **Nuevo**.

1. Cambie el botón de alternancia **Simulated** **On** (Activado) u **Off** (Desactivado). Un dispositivo real hace referencia a un dispositivo físico que se conecta a la aplicación de Azure IoT Central. Un dispositivo simulado tiene datos de ejemplo generados por Azure IoT Central, que los pone a su disposición.

1. Haga clic en **Create**(Crear).

1. El dispositivo aparece ahora en la lista de dispositivos de esta plantilla. Selecciónelo para ver su página de detalles, que contiene todas las vistas del dispositivo.

## <a name="import-devices"></a>Importación de dispositivos

Para conectar un gran número de dispositivos a la aplicación, puede importar dispositivos en bloque desde un archivo CSV. El archivo CSV debe tener las siguientes columnas y encabezados:

* **IOTC_DeviceID**: el identificador de dispositivo debe estar en minúscula.
* **IOTC_DeviceName**: esta columna es opcional.

Para registrar dispositivos en la aplicación en bloque:

1. Elija **Devices** (Dispositivos) en el menú de navegación de la izquierda.

1. En el panel izquierdo, elija la plantilla de dispositivo para la que desea crear dispositivos en bloque.

    > [!NOTE]
    > Si aún no tiene una plantilla de dispositivo, puede importar dispositivos en **All devices** (Todos los dispositivos) y registrarlos sin plantilla. Una vez que los dispositivos se han importado, puede migrarlos a una plantilla.

1. Seleccione **Import** (Importar).

    ![Acción de importación](./media/howto-manage-devices-pnp/bulkimport1a.png)


1. Seleccione el archivo CSV que tiene la lista de identificadores de dispositivo que desea importar.

1. La importación de dispositivos se inicia una vez que se ha cargado el archivo. En el panel Device Operations (Operaciones de dispositivo) puede realizar un seguimiento del estado de la importación. Este panel aparece automáticamente cuando se inicia la importación, o bien se puede acceder a él a través del icono de la campana de la esquina superior derecha.

1. Una vez que la importación finalizada, se muestra un mensaje de confirmación en el panel Device Operations (Operaciones de dispositivo).

    ![Importación correcta](./media/howto-manage-devices-pnp/bulkimport3a.png)


Si se produce un error en la operación de importación de dispositivos, aparece un mensaje de error en el panel Device Operations (Operaciones de dispositivo). Se genera un archivo de registro descargable con todos los errores.

**Migración de dispositivos a una plantilla**

Si para registrar dispositivos inicia la importación en **All devices** (Todos los dispositivos), los dispositivos se crean sin asociación con ninguna plantilla. Los dispositivos deben estar asociados con una plantilla para explorar los datos y otros detalles sobre ellos. Siga estos pasos para asociar dispositivos con una plantilla:

1. Elija **Devices** (Dispositivos) en el menú de navegación de la izquierda.

1. Elija **All devices** (Todos los servicios) en el panel izquierdo:

    ![Dispositivos no asociados](./media/howto-manage-devices-pnp/unassociateddevices1a.png)


1. Use el filtro de la cuadrícula para determinar si el valor de la columna **Device Template** (Plantilla de dispositivo) es "Unassociated" (Sin asociar) en cualquiera de los dispositivos.

1. Seleccione los dispositivos que quiere asociar con una plantilla:

1. Seleccione **Migrate** (Migrar):

    ![Asociar dispositivos](./media/howto-manage-devices-pnp/unassociateddevices2a.png)


1. Elija la plantilla en la lista de plantillas disponibles y seleccione **Migrate** (Migrar).

1. Los dispositivos seleccionados están asociados con la plantilla de dispositivo que eligió.


## <a name="export-devices"></a>Exportación de dispositivos

Para conectar un dispositivo real a IoT Central, necesita su cadena de conexión. Puede exportar los detalles del dispositivo de forma masiva para obtener la información que necesita para crear las cadenas de conexión del dispositivo. El proceso de exportación crea un archivo CSV con la identidad, el nombre y las claves de todos los dispositivos seleccionados.

Para realizar la exportación masiva de dispositivos desde la aplicación:

1. Elija **Devices** (Dispositivos) en el menú de navegación de la izquierda.

1. En el panel izquierdo, elija la plantilla de dispositivo para la que desea exportar los dispositivos.

1. Seleccione los dispositivos que desea exportar y la acción **Exportar**.

    ![Exportación](./media/howto-manage-devices-pnp/export1a.png)


1. Se inicia el proceso de exportación. Para realizar un seguimiento del estado, utilice el panel Device Operations (Operaciones de dispositivo).

1. Cuando la exportación finaliza, se muestra un mensaje de confirmación junto con un vínculo para descargar el archivo generado.

1. Seleccione el vínculo **Download File** (Descargar archivo) para descargar el archivo en una carpeta local del disco.

    ![Exportación correcta](./media/howto-manage-devices-pnp/export2a.png)


1. El archivo CSV exportado contiene las siguientes columnas: Id. de dispositivo, nombre del dispositivo, claves de dispositivo y huellas digitales de certificado X509:

    * IOTC_DEVICEID
    * IOTC_DEVICENAME
    * IOTC_SASKEY_PRIMARY
    * IOTC_SASKEY_SECONDARY
    * IOTC_X509THUMBPRINT_PRIMARY
    * IOTC_X509THUMBPRINT_SECONDARY

Para más información sobre las cadenas de conexión y la conexión de dispositivos reales a la aplicación de IoT Central, consulte [Conectividad de dispositivos en Azure IoT Central](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="delete-a-device"></a>Eliminar un dispositivo

Para eliminar ya sea un dispositivo real o simulado de la aplicación de Azure IoT Central:

1. Elija **Devices** (Dispositivos) en el menú de navegación.

1. Elija la plantilla de dispositivo del dispositivo que desea eliminar.

1. Use las herramientas de filtro para filtrar y buscar los dispositivos. Active la casilla situada junto al dispositivo que desea eliminar.

1. Elija **Eliminar**. Puede realizar un seguimiento del estado de la eliminación en el panel Device Operations (Operaciones de dispositivo).

## <a name="change-a-property"></a>Cambio de una propiedad

Las propiedades de la nube son los metadatos de dispositivo asociados al dispositivo, como la ciudad y el número de serie. Las propiedades que se pueden escribir controlan el comportamiento de un dispositivo. En otras palabras, le permite proporcionar entradas para el dispositivo.  Las propiedades del dispositivo las establece el dispositivo y son de solo lectura en IoT Central. Puede ver y actualizar las propiedades en las vistas de **Detalles del dispositivo**.

1. Elija **Devices** (Dispositivos) en el menú de navegación.

1. Elija la plantilla de dispositivo del dispositivo cuyas propiedades desea modificar y seleccione el dispositivo de destino.

1. Elija la vista que contiene las propiedades del dispositivo. Esta vista le permite especificar valores y seleccionar **Guardar** en la parte superior de la página. Aquí verá las propiedades del dispositivo y sus valores actuales. Tanto las propiedades de la nube como las propiedades que se pueden escribir tienen campos editables, mientras que las propiedades del dispositivo son de solo lectura. En el caso de las propiedades que se pueden escribir, puede ver su estado de sincronización en la parte inferior del campo. 

1. Modifique las propiedades con los valores que necesite. Puede modificar varias propiedades a la vez y actualizarlas todas al mismo tiempo.

1. Elija **Guardar**. Si guardó las propiedades que se pueden escribir, los valores se envían al dispositivo. Cuando el dispositivo confirma el cambio de la propiedad que se puede escribir, el estado vuelve a **synced**(sincronizado). Si guardó una propiedad de la nube, el valor se actualiza.


## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar dispositivos en la aplicación de Azure IoT Central, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [How to use device sets](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Uso de conjuntos de dispositivos)

<!-- Next how-tos in the sequence -->
