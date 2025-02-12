---
title: Creación de un recurso de Cognitive Services en Azure Portal
titleSuffix: Azure Cognitive Services
description: Comience a usar Azure Cognitive Services mediante la creación y suscripción a un recurso en Azure Portal.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: aahi
ms.openlocfilehash: d0b2c9c7450b8a44ca93a9d9fa8770b7ab3d9282
ms.sourcegitcommit: 86d49daccdab383331fc4072b2b761876b73510e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70743846"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Creación de un recurso de Cognitive Services con Azure Portal

Use esta guía de inicio rápido para empezar a usar Azure Cognitive Services. Después de crear un recurso de Cognitive Services en Azure Portal, obtendrá un punto de conexión y una clave para autenticar las aplicaciones.


[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure válida: [cree una de manera gratuita](https://azure.microsoft.com/free/).

## <a name="create-a-new-azure-cognitive-services-resource"></a>Creación de un nuevo recurso de Azure Cognitive Services

1. Crea un recurso.

    #### <a name="multi-service-resourcetabmultiservice"></a>[Recurso de varios servicios](#tab/multiservice)
    
    El recurso multiservicio se denomina **Cognitive Services** en el portal. [Cree un recurso de Cognitive Services](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne).
    
    En este momento, el recurso multiservicio permite el acceso a los siguientes servicios de Cognitive Services:
    
    |                  |                                                      |                    |                               |                  |
    |------------------|------------------------------------------------------|--------------------|-------------------------------|------------------|
    | Computer Vision  | Content Moderator                                    | Caras               | Language Understanding (LUIS) | Text Analytics   |
    | Translator Text  | Bing Search versión 7 <br>(Web, imágenes, noticias, vídeos, contenido visual) | Bing Custom Search | Bing Entity Search            | Bing Autosuggest |
    | Bing Spell Check |                                                      |                    |                               |                  |
    
    #### <a name="single-service-resourcetabsingleservice"></a>[Recurso de servicio único](#tab/singleservice)

    Use los vínculos siguientes si desea crear un recurso para los servicios de Cognitive Services disponibles:

    | Visión                      | Voz                  | Idioma                          | Decisión             | Search                 |
    |-----------------------------|-------------------------|-----------------------------------|----------------------|------------------------|
    | [Computer Vision](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)         | [Servicios de Voz](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices)     | [Lector inmersivo](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesImmersiveReader)              | [Anomaly Detector](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) | [Bing Search API V7](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7) |
    | [Servicio Custom Vision](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesCustomVision) | [Speaker Recognition](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeakerRecognition) | [Language Understanding (LUIS)](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUIS) | [Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) | [Bing Custom Search](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingCustomSearch) |
    | [Face](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)                    |                         | [QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)                     | [Personalizer](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)     | [Bing Entity Search](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingEntitySearch) |
    | [Ink Recognizer](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesInkRecognizer)        |                         | [Text Analytics](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)                |                      | [Bing Spell Check](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingSpellCheck-v7)   |
    |           |                         | [Translator Text](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation)               |                      | [Bing Autosuggest](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingAutosuggest-v7)                       |
    ***

3. En la página **Crear**, proporcione la siguiente información:

    #### <a name="multi-service-resourcetabmultiservice"></a>[Recurso de varios servicios](#tab/multiservice)

    |    |    |
    |--|--|
    | **Nombre** | Nombre descriptivo para el recurso de Cognitive Services. Por ejemplo, *MyCognitiveServicesResource*. |
    | **Suscripción** | Seleccione una de las suscripciones de Azure disponibles. |
    | **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. No olvide su ubicación de Azure, ya que puede necesitarla cuando llame a Azure Cognitive Services. |
    | **Plan de tarifa** | Costo de la cuenta de Cognitive Services, que depende del uso y de las opciones que elija. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Grupos de recursos** | El grupo de recursos de Azure que contendrá su recurso de Cognitive Services. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

    ![Pantalla de creación de recursos](media/cognitive-services-apis-create-account/resource_create_screen-multi.png)

    Haga clic en **Create**(Crear).

    #### <a name="single-service-resourcetabsingleservice"></a>[Recurso de servicio único](#tab/singleservice)

    |    |    |
    |--|--|
    | **Nombre** | Nombre descriptivo para el recurso de Cognitive Services. Por ejemplo, *TextAnalyticsResource*. |
    | **Suscripción** | Seleccione una de las suscripciones de Azure disponibles. |
    | **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. No olvide su ubicación de Azure, ya que puede necesitarla cuando llame a Azure Cognitive Services. |
    | **Plan de tarifa** | Costo de la cuenta de Cognitive Services, que depende del uso y de las opciones que elija. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Grupos de recursos** | El grupo de recursos de Azure que contendrá su recurso de Cognitive Services. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

    ![Pantalla de creación de recursos](media/cognitive-services-apis-create-account/resource_create_screen.png)

    Haga clic en **Create**(Crear).

    ***

## <a name="get-the-keys-for-your-resource"></a>Obtención de las claves del recurso

1. Una vez que el recurso se haya implementado correctamente, en **Pasos siguientes**, haga clic en **Ir al recurso**.

    ![Búsqueda de Cognitive Services](media/cognitive-services-apis-create-account/resource-next-steps.png)

2. En el panel de inicio rápido que se abre, puede acceder a la clave y al punto de conexión.

    ![Obtención de una clave y un punto de conexión](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos también se eliminan los demás recursos incluidos en el grupo.

1. En Azure Portal, expanda el menú de la izquierda para abrir el menú de servicios y elija **Grupos de recursos** para ver una lista con sus grupos de recursos.
2. Busque el grupo de recursos que contiene el recurso que quiere eliminar.
3. Haga clic con el botón derecho en la lista de grupos de recursos. Seleccione **Eliminar grupo de recursos** y confirme.

## <a name="see-also"></a>Otras referencias

* [Autenticación de solicitudes en Azure Cognitive Services](authentication.md)
* [¿Qué es Azure Cognitive Services?](Welcome.md)
* [Compatibilidad con idiomas naturales](language-support.md)
* [Compatibilidad con contenedores de Docker](cognitive-services-container-support.md)
