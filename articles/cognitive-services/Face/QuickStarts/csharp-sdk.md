---
title: 'Inicio rápido: Biblioteca cliente de Face para .NET | Microsoft Docs'
description: Introducción a la biblioteca cliente de Face para .NET.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 08/20/2019
ms.author: pafarley
ms.openlocfilehash: 09f46b3d938e57f996a18f1558b587dcf9e410a8
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70165376"
---
# <a name="quickstart-face-client-library-for-net"></a>Inicio rápido: Biblioteca cliente de Face para .NET

Introducción a la biblioteca cliente de Face para .NET. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas. El servicio Face API le proporciona acceso a algoritmos avanzados para detectar y reconocer caras humanas en imágenes.

Use la biblioteca cliente de Face para .NET para:

* [Autenticar el cliente](#authenticate-the-client)
* [Detección de caras en una imagen](#detect-faces-in-an-image)
* [Búsqueda de caras similares](#find-similar-faces)
* [Creación y entrenamiento de un grupo de personas](#create-and-train-a-person-group)
* [Identificación de una cara](#identify-a-face)
* [Realización de una instantánea para la migración de datos](#take-a-snapshot-for-data-migration)

[Documentación de referencia](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.Face) | [Paquete (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.5.0-preview.1) | [Ejemplos](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=Face&sort=0)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* La versión actual de [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="setting-up"></a>Instalación

### <a name="create-a-face-azure-resource"></a>Creación de un recurso de Azure para Face

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para Face mediante [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local. También puede:

* Obtener una [clave de prueba](https://azure.microsoft.com/try/cognitive-services/#decision) válida durante siete días de forma gratuita Después de registrarse, estará disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Ver el recurso en [Azure Portal](https://portal.azure.com/)

Después de obtener una clave de la suscripción de evaluación o el recurso, [cree una variable de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para la clave y la dirección URL del punto de conexión, denominadas `FACE_SUBSCRIPTION_KEY` y `FACE_ENDPOINT`, respectivamente.

### <a name="create-a-new-c-application"></a>Creación de una aplicación de C#

Cree una nueva aplicación de consola de .NET Core en el IDE o editor que prefiera. 

En una ventana de consola (por ejemplo, cmd, PowerShell o Bash), use el comando `dotnet new` para crear una nueva aplicación de consola con el nombre `face-quickstart`. Este comando crea un sencillo proyecto "Hola mundo" de C# con un solo archivo de origen: *Program.cs*. 

```console
dotnet new console -n face-quickstart
```

Cambie el directorio a la carpeta de aplicaciones recién creada. Para compilar la aplicación:

```console
dotnet build
```

La salida de la compilación no debe contener advertencias ni errores. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

En el directorio del proyecto, abra el archivo *Program.cs* en el editor o IDE que prefiera. Agregue las siguientes directivas `using`:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_using)]

En el método `Main` de la aplicación, cree variables para el punto de conexión y la clave de Azure del recurso.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_mainvars)]

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Dentro del directorio de aplicaciones, instale la biblioteca cliente de Face para .NET con el siguiente comando:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.5.0-preview.1
```

Si usa el IDE de Visual Studio, la biblioteca cliente estará disponible como un paquete de NuGet descargable.

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales del SDK de .NET para Face:

|NOMBRE|DESCRIPCIÓN|
|---|---|
|[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) | Esta clase representa la autorización para usar el servicio Face, se necesita para que Face funcione correctamente. Cree una instancia de ella con la información de suscripción y úsela para generar instancias de otras clases. |
|[FaceOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperations?view=azure-dotnet)|Esta clase controla las tareas básicas de detección y reconocimiento que puede realizar con las caras humanas. |
|[DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)|Esta clase representa todos los datos que se detectaron de una única cara en una imagen. Puede usarla para recuperar información detallada sobre la cara.|
|[FaceListOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet)|Esta clase administra las construcciones **FaceList** almacenadas en la nube, que almacenan a su vez un conjunto ordenado de caras. |
|[PersonGroupPersonExtensions](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongrouppersonextensions?view=azure-dotnet)| Esta clase administra las construcciones **Person** almacenadas en la nube, que almacenan a su vez un conjunto de caras que pertenecen a una sola persona.|
|[PersonGroupOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet)| Esta clase administra las construcciones **PersonGroup** almacenadas en la nube, que almacenan a su vez un conjunto de objetos **Person** ordenados. |
|[ShapshotOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)|Esta clase administra la funcionalidad de instantáneas. Puede usarla para guardar temporalmente todos los datos de caras en la nube y migrar los datos a una nueva suscripción de Azure. |

## <a name="code-examples"></a>Ejemplos de código

En estos fragmentos de código se muestra cómo realizar las siguientes tareas con la biblioteca cliente de Face para .NET:

* [Autenticar el cliente](#authenticate-the-client)
* [Detección de caras en una imagen](#detect-faces-in-an-image)
* [Búsqueda de caras similares](#find-similar-faces)
* [Creación y entrenamiento de un grupo de personas](#create-and-train-a-person-group)
* [Identificación de una cara](#identify-a-face)
* [Realización de una instantánea para la migración de datos](#take-a-snapshot-for-data-migration)


## <a name="authenticate-the-client"></a>Autenticar el cliente

> [!NOTE]
> En este inicio rápido se da por supuesto que ha [creado variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para la clave de Face y el punto de conexión, denominadas `FACE_SUBSCRIPTION_KEY` y `FACE_ENDPOINT`.

En un nuevo método, cree una instancia de un cliente con la clave y el punto de conexión. Cree un objeto [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) con la clave y úselo con el punto de conexión para crear un objeto [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet).

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_auth)]

Es probable que desee llamar a este método en el método `Main`.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_client)]

## <a name="detect-faces-in-an-image"></a>Detectar caras en una imagen

En la raíz de la clase, defina la siguiente cadena de dirección URL. Esta dirección URL señala a un conjunto de imágenes de ejemplo.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_image_url)]

Opcionalmente, puede elegir el modelo de AI que se va a usar para extraer datos de las caras detectadas. Consulte [Especificación de un modelo de reconocimiento](../Face-API-How-to-Topics/specify-recognition-model.md) para información sobre estas opciones.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_models)]

La operación de detección final tomará un objeto [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet), una dirección URL de imagen y un modelo de reconocimiento.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_call)]

En este caso, el método `DetectFaceExtract` detecta caras en tres de las imágenes en la dirección URL dada y crea una lista de objetos [DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet) en la memoria del programa. La lista de valores [FaceAttributeType](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet) especifica qué rasgos se van a extraer. Consulte el código de ejemplo en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/Face/Program.cs) para ver el código auxiliar que puede imprimir estos datos de una manera intuitiva.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect)]

## <a name="find-similar-faces"></a>Búsqueda de caras similares

El siguiente código toma una sola cara detectada (origen) y busca coincidencias en un conjunto de otras caras (destino). Cuando la encuentra, imprime el identificador de la cara coincidente en la consola.

### <a name="detect-faces-for-comparison"></a>Detección de caras para la comparación

En primer lugar, defina un segundo método de detección de caras. Debe detectar las caras en las imágenes para poder compararlas y este método de detección está optimizado para las operaciones de comparación. No extrae los atributos de cara detallados como en la sección anterior y usa un modelo de reconocimiento diferente.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_face_detect_recognize)]

### <a name="find-matches"></a>Búsqueda de coincidencias

El método siguiente detecta caras en un conjunto de imágenes de destino y en una sola imagen de origen. Después, los compara y busca todas las imágenes de destino que son similares a la imagen de origen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_find_similar)]

### <a name="print-matches"></a>Impresión de las coincidencias

El siguiente código imprime los detalles coincidentes en la consola.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_find_similar_print)]

## <a name="create-and-train-a-person-group"></a>Crear y entrenar un grupo de personas

En el siguiente código se crea un objeto **PersonGroup** con seis objetos **Person** distintos. Asocia cada objeto **Person** con un conjunto de imágenes de ejemplo y lo entrena para reconocer a cada persona por sus rasgos faciales. Los objetos  **Person** y **PersonGroup** se utilizan en las operaciones de comprobación, identificación y agrupación.

Si todavía no lo ha hecho, defina la siguiente cadena de dirección URL en la raíz de la clase. Señala a un conjunto de imágenes de ejemplo.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_image_url)]

En el código que aparece más adelante en esta sección, se especifica un modelo de reconocimiento para extraer datos de caras y el siguiente fragmento de código crea referencias a los modelos disponibles. Consulte [Especificación de un modelo de reconocimiento](../Face-API-How-to-Topics/specify-recognition-model.md) para información sobre los modelos de reconocimiento.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_models)]

### <a name="create-persongroup"></a>Creación de un objeto PersonGroup

Declare una variable de cadena en la raíz de la clase para representar el identificador del objeto **PersonGroup** que va a crear.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_declare)]

En el método nuevo, agregue el siguiente código. Este código asocia los nombres de las personas con sus imágenes de ejemplo.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_files)]

A continuación, agregue el código siguiente para crear un objeto **Person** para cada persona del diccionario y agregar los datos de la imagen a partir de las imágenes adecuadas. Cada objeto **Person** se asocia con el mismo objeto  **PersonGroup** mediante su cadena de identificador único. Recuerde pasar las variables `client`, `url` y `RECOGNITION_MODEL1` en este método.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_create)]

### <a name="train-persongroup"></a>Entrenamiento del objeto PersonGroup

Una vez que haya extraído los datos faciales de las imágenes y los haya ordenado en objetos **Person** diferentes, debe entrenar el objeto **PersonGroup** para identificar las características visuales asociadas a cada uno de sus objetos **Person**. El siguiente código llama al método **train** asincrónico, sondea los resultados e imprime el estado en la consola.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_train)]

Este grupo de objetos **Person** y sus objetos **Person** asociados ya están listos para usarse en las operaciones de comprobación, identificación o agrupación.

## <a name="identify-a-face"></a>Identificar una cara

La operación de identificación toma una imagen de una persona (o de varias) y busca la identidad de cada una de las caras de la imagen. Compara cada cara detectada con un objeto **PersonGroup**, una base de datos con distintos objetos **Person** cuyos rasgos faciales se conocen.

> [!IMPORTANT]
> Para ejecutar este ejemplo, primero debe ejecutar el código de [Creación y entrenamiento de un grupo de personas](#create-and-train-a-person-group). Las variables que se usan en esa sección &mdash;`client`, `url` y `RECOGNITION_MODEL1`&mdash; también deben estar disponibles aquí.

### <a name="get-a-test-image"></a>Obtención de una imagen de prueba

Tenga en cuenta que el código para [crear y entrenar a un grupo de personas](#create-and-train-a-person-group) define una variable `sourceImageFileName`. Esta variable corresponde a la imagen de origen: la imagen que contiene los usuarios que se deben identificar.

### <a name="identify-faces"></a>Identificación de caras

El siguiente código toma la imagen de origen y crea una lista de todas las caras detectadas en ella. Estas son las caras que se identificarán en el objeto **PersonGroup**.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_identify_sources)]

El siguiente fragmento de código llama a la operación de identificación e imprime los resultados en la consola. En este caso, el servicio intenta hacer coincidir cada una de las caras de la imagen de origen con un objeto **Person** del objeto **PersonGroup** dado.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_identify)]

## <a name="take-a-snapshot-for-data-migration"></a>Tomar una instantánea para la migración de datos

La característica de instantáneas permite trasladar los datos de Face guardados, como un objeto **PersonGroup** entrenado, a otra suscripción de Face de Azure Cognitive Services. Puede ser conveniente usar esta característica si, por ejemplo, ha creado un objeto **PersonGroup** mediante una suscripción de evaluación gratuita y desea migrarlo a una de pago. Consulte el artículo de [Migración de los datos de las caras](../Face-API-How-to-Topics/how-to-migrate-face-data.md) para información general de la característica de instantáneas.

En este ejemplo migrará el objeto **PersonGroup** que ha creado en [Creación y entrenamiento de un grupo de personas](#create-and-train-a-person-group). Puede completar esa sección primero o crear sus propias construcciones de Face que migrar.

### <a name="set-up-target-subscription"></a>Configuración de la suscripción de destino

En primer lugar, debe tener una segunda suscripción de Azure con un recurso de Face; puede crearlo si sigue los pasos descritos en la sección [Instalación](#setting-up). 

A continuación, defina las siguientes variables en método `Main` del programa. Necesitará crear nuevas variables de entorno para el identificador de suscripción de la cuenta de Azure, así como la clave, punto de conexión y el identificador de suscripción de la cuenta nueva (de destino). 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_vars)]

En este ejemplo, declare una variable para el identificador del objeto **PersonGroup**&mdash;, el objeto que pertenece a la nueva suscripción, a la que se copiarán los datos.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_vars)]

### <a name="authenticate-target-client"></a>Autenticación del cliente de destino

A continuación, agregue el código para autenticar la suscripción de Face secundaria.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_client)]

### <a name="use-a-snapshot"></a>Uso de una instantánea

El resto de las operaciones de instantánea tienen lugar dentro de un método asincrónico. 

1. El primer paso es **tomar** la instantánea, que guarda los datos de la cara de la suscripción original en una ubicación temporal en la nube. Este método devuelve un identificador que se utiliza para consultar el estado de la operación.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_take)]

1. Después, consulte el identificador hasta que se haya completado la operación.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_take_wait)]

1. Después, use la operación **apply** para escribir los datos de la cara en la suscripción de destino. Este método también devuelve un valor de identificador.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_apply)]

1. Después, consulte el nuevo identificador hasta que se haya completado la operación.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_apply)]

1. Por último, complete el bloque try/catch y finalice el método.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_trycatch)]

En este momento, el nuevo objeto **PersonGroup** debe tener los mismos datos que el original y debe ser accesible desde su nueva suscripción de Azure Face (destino).

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación desde el directorio de la aplicación con el comando `dotnet run`.

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Si ha creado un objeto **PersonGroup** en este inicio rápido y desea eliminarlo, ejecute el siguiente código en el programa:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_delete)]

Reemplace el método de eliminación por el código siguiente:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_deletepersongroup)]

Además, si migró los datos mediante la característica de instantánea en este inicio rápido, también deberá eliminar el objeto **PersonGroup** guardado en la suscripción de destino.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_target_persongroup_delete)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a usar la biblioteca de Face para .NET para realizar tareas básicas. A continuación, consulte la documentación de referencia para más información sobre la biblioteca.

> [!div class="nextstepaction"]
> [Referencia de Face API (.NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)

* [¿Qué es Face API?](../overview.md)
* El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/Face/Program.cs).