---
title: 'Cómo instalar y ejecutar contenedores: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Cómo descargar, instalar y ejecutar contenedores para Computer Vision en este tutorial paso a paso.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: 98330e88b0b94c488fd968d8fc18806ec6908b26
ms.sourcegitcommit: 9fba13cdfce9d03d202ada4a764e574a51691dcd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316205"
---
# <a name="install-and-run-computer-vision-containers"></a>Instalación y uso de contenedores de Computer Vision

Los contenedores le permiten ejecutar las API de Computer Vision en su propio entorno. Los contenedores son excelentes para requisitos específicos de control de datos y seguridad. En este artículo, aprenderá a descargar, instalar y ejecutar un contenedor de Computer Vision.

Hay dos contenedores de Docker disponibles para Computer Vision: *Reconocer texto* y *Leer*. El contenedor *Reconocer texto* le permite detectar y extraer *texto impreso* de imágenes que muestren diversos objetos con diferentes superficies y fondos, como recibos, pósteres y tarjetas de visita. Por su parte, el contenedor *Leer* también detecta *texto manuscrito* en imágenes y es compatible con los formatos PDF/TIFF/multipágina. Para obtener más información, consulte la documentación de la [API Read](concept-recognizing-text.md#read-api).

> [!IMPORTANT]
> El contenedor Reconocer texto se dejará de usar y, en su lugar, se usará el contenedor Leer. El contenedor Leer es un superconjunto de su predecesor, el contenedor Reconocer texto, y los consumidores deben migrar al uso del contenedor Leer. Ambos contenedores funcionan solo en inglés.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos para poder usar los contenedores:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.| 
|Recurso de Computer Vision |Para poder usar el contenedor, debe tener:<br><br>Un recurso de **Computer Vision** de Azure y la clave de API asociada con el URI del punto de conexión. Ambos valores están disponibles en las páginas de introducción y claves del recurso y son necesarios para iniciar el contenedor.<br><br>**{API_KEY}** : una de las dos claves de recurso disponibles en la página **Claves**<br><br>**{ENDPOINT_URI}** : el punto de conexión tal como se proporciona en la página de **Introducción**.|

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-private-container-registry"></a>Solicitud de acceso al registro de contenedor privado

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>El equipo host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

# <a name="readtabread"></a>[Lectura](#tab/read)

Hay imágenes de contenedor para Leer disponibles.

| Contenedor | Container Registry/Repositorio/Nombre de imagen |
|-----------|------------|
| Lectura | `containerpreview.azurecr.io/microsoft/cognitive-services-read:latest` |

# <a name="recognize-texttabrecognize-text"></a>[Reconocer texto](#tab/recognize-text)

Hay imágenes de contenedor disponibles para Reconocer texto.

| Contenedor | Container Registry/Repositorio/Nombre de imagen |
|-----------|------------|
| Reconocer texto | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

***

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar una imagen de contenedor.

# <a name="readtabread"></a>[Lectura](#tab/read)

### <a name="docker-pull-for-the-read-container"></a>Docker pull para el contenedor Leer

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-read:latest
```

# <a name="recognize-texttabrecognize-text"></a>[Reconocer texto](#tab/recognize-text)

### <a name="docker-pull-for-the-recognize-text-container"></a>Comando Docker pull para el contenedor Reconocer texto

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación requerida. Hay más [ejemplos](computer-vision-resource-container-config.md) del comando `docker run` disponibles. 
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar el contenedor. Consulte [Recopilación de los parámetros obligatorios](#gathering-required-parameters) para más información sobre cómo obtener los valores de `{ENDPOINT_URI}` y `{API_KEY}`.

Hay disponibles [ejemplos](computer-vision-resource-container-config.md#example-docker-run-commands) del comando `docker run`.

# <a name="readtabread"></a>[Lectura](#tab/read)

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
containerpreview.azurecr.io/microsoft/cognitive-services-read \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Este comando:

* Ejecuta el contenedor Leer desde la imagen de contenedor.
* Asigna un núcleo de 8 CPU y 16 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host.

# <a name="recognize-texttabrecognize-text"></a>[Reconocer texto](#tab/recognize-text)

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Este comando:

* Ejecuta el contenedor Reconocer texto a partir de la imagen de contenedor.
* Asigna un núcleo de 8 CPU y 16 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host.

***

Hay más [ejemplos](./computer-vision-resource-container-config.md#example-docker-run-commands) del comando `docker run` disponibles. 

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

El contenedor proporciona varias API de puntos de conexión de predicción de consultas basadas en REST. 

Utilice el host, `http://localhost:5000`, con las API de contenedor.

# <a name="readtabread"></a>[Lectura](#tab/read)

### <a name="asynchronous-read"></a>Lectura asincrónica

Puede usar las operaciones `POST /vision/v2.0/read/core/asyncBatchAnalyze` y `GET /vision/v2.0/read/operations/{operationId}` conjuntamente para leer una imagen asincrónicamente, de manera similar a cómo el servicio Computer Vision usa las operaciones de REST correspondientes. El método POST asincrónico devolverá un valor `operationId` que se usa como identificador de la solicitud HTTP GET.

En la interfaz de usuario de Swagger, seleccione `asyncBatchAnalyze` para expandirlo en el explorador. A continuación, seleccione **Probarlo** > **Elegir archivo**. En este ejemplo, usaremos la imagen siguiente:

![pestañas o espacios](media/tabs-vs-spaces.png)

Una vez ejecutado correctamente el método POST, devuelve un código de estado **HTTP 202**. Como parte de la respuesta, hay un encabezado `operation-location` que contiene el punto de conexión del resultado de la solicitud.

```http
 content-length: 0
 date: Fri, 13 Sep 2019 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v2.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

El elemento `operation-location` es la dirección URL completa y se obtiene acceso a ella a través de HTTP GET. Esta es la respuesta JSON a la ejecución de la dirección URL `operation-location` desde la imagen anterior:

```json
{
  "status": "Succeeded",
  "recognitionResults": [
    {
      "page": 1,
      "clockwiseOrientation": 2.42,
      "width": 502,
      "height": 252,
      "unit": "pixel",
      "lines": [
        {
          "boundingBox": [
            56,
            39,
            317,
            50,
            313,
            134,
            53,
            123
          ],
          "text": "Tabs VS",
          "words": [
            {
              "boundingBox": [
                90,
                43,
                243,
                53,
                243,
                123,
                94,
                125
              ],
              "text": "Tabs",
              "confidence": "Low"
            },
            {
              "boundingBox": [
                259,
                55,
                313,
                62,
                313,
                122,
                259,
                123
              ],
              "text": "VS"
            }
          ]
        },
        {
          "boundingBox": [
            221,
            148,
            417,
            146,
            417,
            206,
            227,
            218
          ],
          "text": "Spaces",
          "words": [
            {
              "boundingBox": [
                230,
                148,
                416,
                141,
                419,
                211,
                232,
                218
              ],
              "text": "Spaces"
            }
          ]
        }
      ]
    }
  ]
}
```

### <a name="synchronous-read"></a>Lectura sincrónica

Puede usar la operación `POST /vision/v2.0/read/core/Analyze` para leer sincrónicamente una imagen. Una vez leída la imagen en su totalidad, entonces, y solo entonces, devuelve la API una respuesta JSON. La única excepción a esto es un escenario de error. Cuando se produce un error, se devuelve el siguiente código JSON:

```json
{
    status: "Failed"
}
```

El objeto de respuesta JSON tiene el mismo gráfico de objetos que la versión asincrónica. Si es un usuario de JavaScript y quiere seguridad de tipos, puede usar los tipos siguientes para convertir la respuesta JSON como un objeto `AnalyzeResult`.

```typescript
export interface AnalyzeResult {
    status: Status;
    recognitionResults?: RecognitionResult[] | null;
}

export enum Status {
    NotStarted = 0,
    Running = 1,
    Failed = 2,
    Succeeded = 3
}

export enum Unit {
    Pixel = 0,
    Inch = 1
}

export interface RecognitionResult {
    page?: number | null;
    clockwiseOrientation?: number | null;
    width?: number | null;
    height?: number | null;
    unit?: Unit | null;
    lines?: Line[] | null;
}

export interface Line {
    boundingBox?: number[] | null;
    text: string;
    words?: Word[] | null;
}

export interface Word {
  boundingBox?: number[] | null;
  text: string;
  confidence?: string | null;
}
```

Para ver un caso de uso de ejemplo, consulte [este espacio aislado de TypeScript](https://aka.ms/ts-read-api-types) y seleccione "Ejecutar" para visualizar su facilidad de uso.

# <a name="recognize-texttabrecognize-text"></a>[Reconocer texto](#tab/recognize-text)

### <a name="asynchronous-text-recognition"></a>Reconocimiento de texto asincrónico

Puede usar las operaciones `POST /vision/v2.0/recognizeText` y `GET /vision/v2.0/textOperations/*{id}*` conjuntamente para reconocer de forma asincrónica texto impreso en una imagen, de manera similar a cómo el servicio de Computer Vision usa esas operaciones de REST correspondientes. Actualmente, el contenedor de Reconocer texto solo reconoce texto impreso, no texto escrito a mano, por lo que dicho contenedor ignora el parámetro `mode` especificado normalmente para la operación del servicio de Computer Vision.

### <a name="synchronous-text-recognition"></a>Reconocimiento de texto sincrónico

Puede usar la operación `POST /vision/v2.0/recognizeTextDirect` para reconocer sincrónicamente texto impreso en una imagen. Dado que esta operación es sincrónica, el cuerpo de solicitud para esta operación es el mismo que para la operación `POST /vision/v2.0/recognizeText`, pero el cuerpo de la respuesta para esta operación es el mismo que el devuelto por la operación `GET /vision/v2.0/textOperations/*{id}*`.

***

## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Si ejecuta el contenedor con un [montaje](./computer-vision-resource-container-config.md#mount-settings) de salida y el registro habilitados, el contenedor genera archivos de registro que resultan útiles para solucionar problemas que se producen al iniciar o ejecutar el contenedor.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Facturación

Los contenedores de Cognitive Services envían información de facturación a Azure mediante el recurso correspondiente de la cuenta de Azure.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obtener más información acerca de estas opciones, consulte [Configure containers](./computer-vision-resource-container-config.md) (Configuración de contenedores).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo para la descarga, instalación y ejecución de contenedores de Computer Vision. En resumen:

* Computer Vision proporciona un contenedor de Linux para Docker que incluye tanto Reconocer texto como Leer.
* Las imágenes de contenedor se descargan desde el registro de contenedores "Contenedor (versión preliminar)" en Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en los contenedores Reconocer texto o Leer mediante la especificación del URI del host del contenedor.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](computer-vision-resource-container-config.md) (Configuración de contenedores) para ver las opciones de configuración.
* Revise [Introducción a Computer Vision](Home.md) para obtener más información sobre el reconocimiento de texto escrito a mano e impreso.
* Consulte [Computer Vision API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) para obtener más información acerca de los métodos que admite el contenedor.
* Consulte [Preguntas más frecuentes (P+F)](FAQ.md) para resolver problemas relacionados con la funcionalidad de Computer Vision.
* Use más [contenedores de Cognitive Services](../cognitive-services-container-support.md)
