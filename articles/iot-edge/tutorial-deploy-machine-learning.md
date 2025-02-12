---
title: 'Implementación de Azure Machine Learning en un dispositivo: Azure IoT Edge | Microsoft Docs'
description: En este tutorial se crea un modelo de Azure Machine Learning y se implementa como módulo en un dispositivo perimetral
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 6c48aaf404803c45122ed3fad0d6af1345406f7b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239661"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Tutorial: Implementación de Azure Machine Learning como un módulo de IoT Edge (versión preliminar)

Use Azure Notebooks para desarrollar un módulo de aprendizaje automático e implementarlo en un dispositivo Linux que ejecuta Azure IoT Edge. 

Los módulos de IoT Edge se pueden usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos de IoT Edge. Este tutorial le guía por la implementación de un módulo de Azure Machine Learning que predice cuándo un dispositivo produce un error basándose en datos de temperatura de la máquina simulada. Para más información sobre Azure Machine Learning Service en IoT Edge, consulte la [documentación de Azure Machine Learning](../machine-learning/service/how-to-deploy-to-iot.md).

El módulo de Azure Machine Learning que crea en este tutorial lee los datos de entorno que el dispositivo genera y etiqueta los mensajes como anómalos o no.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de un módulo de Azure Machine Learning
> * Insertar un contenedor de módulo en Azure Container Registry
> * Implementar un modelo de Azure Machine Learning en un dispositivo de IoT Edge
> * Visualización de datos generados

>[!NOTE]
>Los módulos Azure Machine Learning en Azure IoT Edge están en versión preliminar pública.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar una máquina de Azure como dispositivo de IoT Edge; para ello, siga los pasos que se indican en el artículo de inicio rápido para [Linux](quickstart-linux.md).
* El módulo de Azure Machine Learning no admite contenedores Windows.
* El módulo de Azure Machine Learning no admite procesadores ARM.

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure.
* Un área de trabajo de Azure Machine Learning. Siga las instrucciones del artículo sobre el [Uso de Azure Portal para empezar con Azure Machine Learning](../machine-learning/service/quickstart-get-started.md) para crear una y aprender a usarla.
   * Anote el nombre de área de trabajo, el grupo de recursos y el identificador de suscripción. Estos valores están disponibles en la información general del área de trabajo en Azure Portal. Estos valores le servirán más adelante en el tutorial para conectar un cuaderno de Azure con los recursos del área de trabajo. 


## <a name="create-and-deploy-azure-machine-learning-module"></a>Creación e implementación de un módulo de Azure Machine Learning

En esta sección, se entrenan los archivos del modelo de Machine Learning y se convierten en un contenedor del servicio Azure Machine Learning. Todos los componentes necesarios para la imagen de Docker están en el [repositorio de Git del kit de herramientas de inteligencia artificial para Azure IoT Edge](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial). Siga estos pasos para cargar el repositorio en Microsoft Azure Notebooks para crear el contenedor e insertarlo en Azure Container Registry.


1. Vaya a los proyectos de Azure Notebooks. Puede obtenerlos desde el área de trabajo de Azure Machine Learning Service de [Azure Portal](https://portal.azure.com) o al iniciar sesión en [Microsoft Azure Notebooks](https://notebooks.azure.com/home/projects) con la cuenta de Azure.

2. Seleccione **Upload GitHub Repo** (Cargar repositorio de GitHub).

3. Proporcione el siguiente nombre de repositorio de GitHub: `Azure/ai-toolkit-iot-edge`. Desactive el cuadro **Public** (Público) si desea mantener la privacidad del proyecto. Seleccione **Import** (Importar). 

4. Una vez finalizada la importación, vaya al nuevo proyecto **ai-toolkit-iot-edge** y abra la carpeta **IoT Edge anomaly detection tutorial** (Tutorial de detección de anomalías de IoT Edge). 

5. Compruebe que el proyecto se esté ejecutando. En caso contrario, seleccione **Run on Free Compute** (Ejecutar en un proceso gratuito).

   ![Run on Free Compute (Ejecutar en un proceso gratuito)](./media/tutorial-deploy-machine-learning/run-on-free-compute.png)

6. Abra el archivo **aml_config/config.json**.

7. Edite el archivo de configuración para incluir los valores para el identificador de suscripción de Azure, un grupo de recursos en la suscripción y el nombre del área de trabajo del servicio Azure Machine Learning. Puede obtener todos estos valores de a sección **Información general** del área de trabajo de Azure. 

8. Guarde el archivo de configuración.

9. Abra el archivo **00-anomaly-detection-tutorial.ipynb**.

10. Cuando se le solicite, seleccione el kernel de **Python 3.6** y **Set Kernel** (Establecer kernel).

11. Edite la primera celda del cuaderno según las instrucciones de los comentarios. Use el mismo grupo de recursos, el identificador de suscripción y el nombre del área de trabajo que ha agregado al archivo de configuración.

12. Ejecute las celdas del cuaderno; puede seleccionarlas directamente, seleccionar **Run** (Ejecutar) o presionar `Shift + Enter`.

    >[!TIP]
    >Algunas de las celdas del cuaderno del tutorial de detección de anomalías son opcionales, ya que crean recursos que algunos usuarios podrían tener o no aún, como IoT Hub. Si coloca la información del recurso existente en la primera celda, recibirá errores si ejecuta las celdas que crean recursos, ya que Azure no crea recursos duplicados. Esto es correcto, puede pasar por alto los errores u omitir completamente esas secciones opcionales. 

Al completar todos los pasos del cuaderno, ha entrenado el modelo de detección de anomalías, lo ha compilado como imagen de contenedor de Docker y ha insertado dicha imagen en Azure Container Registry. A continuación, habrá probado el modelo y, finalmente, lo habrá implementado en el dispositivo IoT Edge. 

## <a name="view-container-repository"></a>Visualización del repositorio del contenedor

Compruebe que la imagen de contenedor se creó y almacenó correctamente en la instancia de Azure Container Registry asociada al entorno de aprendizaje automático. El cuaderno que usó en la sección anterior proporciona automáticamente la imagen de contenedor y las credenciales del registro para el dispositivo IoT Edge, pero debe saber dónde se almacenan para poder encontrar la información más adelante. 

1. En [Azure Portal](https://portal.azure.com), vaya al área de trabajo del servicio Azure Machine Learning. 

2. En la sección **Información general** se enumeran los detalles del área de trabajo y los recursos asociados. Seleccione el valor de **Registro**, que debe ser el nombre del área de trabajo seguido de números aleatorios. 

3. En el registro de contenedor, seleccione **Repositorios**. Debería ver un repositorio denominado **tempanomalydetection** que se creó mediante el cuaderno que se ejecutó en la sección anterior. 

4. Seleccione **tempanomalydetection**. Debería ver que el repositorio tiene una etiqueta: **1**. 

   Ahora que sabe el nombre del registro, el nombre del repositorio y la etiqueta, conoce la ruta de acceso completa de la imagen del contenedor. Las rutas de acceso de imagen tienen este aspecto: **\<nombre_del_registro\>.azurecr.io/tempanomalydetection:1**. Puede usar la ruta de acceso de la imagen para implementar este contenedor en dispositivos IoT Edge. 

5. En el registro del contenedor, seleccione **Claves de acceso**. Debería ver un número de credenciales de acceso, incluidos el **Servidor de inicio de sesión**, y el **Nombre de usuario** y la **Contraseña** de un usuario administrador.

   Estas credenciales se pueden incluir en el manifiesto de implementación para proporcionar acceso a los dispositivos IoT Edge para extraer imágenes de contenedor del registro. 

Ahora ya sabe dónde se almacena la imagen de contenedor de Machine Learning. La siguiente sección le guía en los pasos para ver cómo se ejecuta el contenedor como un módulo en su dispositivo IoT Edge. 

## <a name="view-generated-data"></a>Visualización de datos generados

Se pueden ver los mensajes que está generando cada módulo de IoT Edge, y se pueden ver los mensajes que se envían al centro de IoT.

### <a name="view-data-on-your-iot-edge-device"></a>Visualización de datos en un dispositivo IoT Edge

En el dispositivo IoT Edge puede ver los mensajes que se envían desde cada módulo individual.

Es posible que deba usar `sudo` para tener más permisos y ejecutar comandos `iotedge`. Cierre la sesión en el dispositivo y vuelva a iniciarla para actualizar los permisos automáticamente.

1. Vea todos los módulos en el dispositivo IoT Edge.

   ```cmd/sh
   iotedge list
   ```

2. Vea los mensajes que se envían desde un dispositivo específico. Utilice el nombre del módulo de la salida del comando anterior.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>Visualización de los datos que llegan al centro de IoT

Puede ver los mensajes del dispositivo a la nube que el centro de IoT recibe mediante el uso de la [extensión de Azure IoT Hub Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (anteriormente, la extensión de Azure IoT Toolkit).

Los siguientes pasos le muestran cómo configurar Visual Studio Code para supervisar los mensajes del dispositivo a la nube que llegan al centro de IoT.

1. En Visual Studio Code, seleccione **IoT Hub Devices** (Dispositivos de IoT Hub).

2. Seleccione **...**  y, después, seleccione **Set IoT Hub Connection String** (Establecer cadena de conexión de IoT Hub) en el menú.

   ![Establecimiento de la cadena de conexión de IoT Hub](./media/tutorial-deploy-machine-learning/set-connection.png)

3. En el cuadro de texto que aparece en la parte superior de la página, escriba la cadena de conexión iothubowner para IoT Hub. El dispositivo IoT Edge debe aparecer en la lista de dispositivos de IoT Hub.

4. Seleccione **...** de nuevo y luego **Start Monitoring Built-in Event Endpoint** (Iniciar supervisión del punto de conexión del evento integrado).

5. Observe los mensajes procedentes de tempSensor cada cinco segundos. El cuerpo del mensaje contiene una propiedad denominada **anomaly** que el módulo de aprendizaje automático proporciona con un valor true o false. La propiedad **AzureMLResponse** contiene el valor "OK" si el modelo se ejecutó correctamente.

   ![Respuesta de Azure Machine Learning Service en el cuerpo del mensaje](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba.

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha implementado un módulo de IoT Edge con tecnología de Azure Machine Learning. Puede continuar con cualquiera de los otros tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en perspectivas empresariales en el perímetro.

> [!div class="nextstepaction"]
> [Clasificación de imágenes con Custom Vision Service](tutorial-deploy-custom-vision.md)

