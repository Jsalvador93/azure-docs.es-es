---
title: 'Guía de inicio rápido: Búsqueda de imágenes: API de REST Bing Image Search y Python'
titleSuffix: Azure Cognitive Services
description: Use esta guía de inicio rápido para enviar solicitudes de búsqueda de imágenes a la API de REST Bing Image Search mediante Python y reciba respuestas JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 08/26/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 9f43b056275ba83630e711ff1a512cb73e84216a
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034638"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-python"></a>Guía de inicio rápido: Búsqueda de imágenes con la API de REST Bing Image Search y Python

Utilice este inicio rápido para empezar a enviar solicitudes de búsqueda a Bing Image Search API. Esta aplicación de Python envía una consulta de búsqueda a la API y muestra la dirección URL de la primera imagen de los resultados. Si bien esta aplicación está escrita en Python, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

Para ejecutar este ejemplo como Jupyter Notebook en [MyBinder](https://mybinder.org), puede hacer clic en el distintivo launch Binder:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py) con anotaciones y control de errores adicionales.


## <a name="prerequisites"></a>Requisitos previos

* [Python 2.x o 3.x](https://www.python.org/)
* [Python Imaging Library (PIL)](https://pillow.readthedocs.io/en/stable/index.html)
* [matplotlib](https://matplotlib.org/) 

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Python en su IDE o editor preferidos, e importe los siguientes módulos. Cree una variable para su clave de suscripción, punto de conexión de búsqueda y término de búsqueda.

    ```python
    import requests
    import matplotlib.pyplot as plt
    from PIL import Image
    from io import BytesIO
    
    subscription_key = "your-subscription-key"
    search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
    search_term = "puppies"
    ```

2. Agregue su clave de suscripción al encabezado `Ocp-Apim-Subscription-Key` mediante la creación de un diccionario y la incorporación de la clave como un valor. 

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    ```

## <a name="create-and-send-a-search-request"></a>Creación y envío de una solicitud de búsqueda

1. Cree un diccionario para los parámetros de la solicitud de búsqueda. Agregue el término de búsqueda al parámetro `q`. Use "public" en el parámetro `license` para buscar imágenes en el dominio público. Use "photo" en `imageType` para buscar solo fotografías.

    ```python
    params  = {"q": search_term, "license": "public", "imageType": "photo"}
    ```

2. Use la biblioteca `requests` para llamar a Bing Image Search API. Agregue el encabezado y los parámetros a la solicitud, y devuelva la respuesta en forma de objeto JSON. 

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    ```

## <a name="view-the-response"></a>Visualización de la respuesta

1. Cree una nueva ilustración con cuatro columnas y cuatro filas mediante la biblioteca matplotlib. 

2. Recorrer en iteración las filas y columnas de la ilustración y use el método `Image.open()` de biblioteca de PIL para agregar una imagen en miniatura a cada espacio. 

    ```python
    f, axes = plt.subplots(4, 4)
    for i in range(4):
        for j in range(4):
            image_data = requests.get(thumbnail_urls[i+4*j])
            image_data.raise_for_status()
            image = Image.open(BytesIO(image_data.content))        
            axes[i][j].imshow(image)
            axes[i][j].axis("off")
    ```

3. Use `plt.show()` para dibujar en la ilustración y mostrar las imágenes.

## <a name="example-json-response"></a>Ejemplo de respuesta JSON

Las respuestas de Bing Image Search API se devuelven como JSON. Esta respuesta de ejemplo se ha truncado para mostrar un único resultado.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }]
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Image Search](../tutorial-bing-image-search-single-page-app.md)

* [¿Qué es Bing Image Search API?](../overview.md)  
* [Detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) de Bing Search API. 
* [Obtención de una clave de acceso de Cognitive Services gratis](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentación de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Referencia de Bing Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
