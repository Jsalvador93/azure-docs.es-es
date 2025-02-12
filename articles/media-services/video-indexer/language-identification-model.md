---
title: Identificación automática del idioma hablado con Video Indexer - Azure
titlesuffix: Azure Media Services
description: En este artículo se describe cómo se usa el modelo de identificación de idiomas de Video Indexer para identificar automáticamente el idioma que se habla en un vídeo.
services: media-services
author: juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/12/2019
ms.author: ellbe
ms.openlocfilehash: 729ac9fc7c8ec6a85b3497c51888d3b5a6fc8558
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71147188"
---
# <a name="automatically-identify-the-spoken-language-with-language-identification-model"></a>Identificación automática del idioma hablado con el modelo de identificación de idiomas

Video Indexer admite la identificación automática de idiomas (LID), que es el proceso de identificar automáticamente el idioma hablado en un audio y enviar el archivo multimedia para que se transcriba al idioma dominante identificado. Actualmente, LID admite los siguientes idiomas: alemán, chino (simplificado), español, francés, inglés, italiano, japonés, portugués (brasileño) y ruso. 

## <a name="choosing-auto-language-identification-on-indexing"></a>Elección de la identificación automática de idioma en la indexación

Al indexar o [volver a indexar](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) un vídeo mediante la API, elija la opción `auto detect` en el parámetro `sourceLanguage`.

Cuando use el portal, vaya a **Account videos** (Vídeos de la cuenta) en la página de inicio de [Video Indexer](https://www.videoindexer.ai/) y mantenga el puntero sobre el nombre del vídeo que quiera volver a indexar. En la esquina inferior derecha, haga clic en el botón para volver a indexar. En el cuadro de diálogo **Volver a indexar el vídeo**, elija *Detección automática* en el cuadro desplegable **Idioma de origen del vídeo**.

![Detección automática](./media/language-identification-model/auto-detect.png)

## <a name="model-output"></a>Salida del modelo

Video Indexer transcribe el vídeo conforme al idioma más probable si la confianza para ese idioma es `> 0.6`. Si el idioma hablado no se puede identificar con confianza, se supone que es inglés. 

El idioma dominante del modelo está disponible en el JSON de Insights como atributo `sourceLanguage` (en root/videos/Insights). En el atributo `sourceLanguageConfidence` también está disponible la puntuación de confianza correspondiente.

```json
"insights": {
        "version": "1.0.0.0",
        "duration": "0:05:30.902",
        "sourceLanguage": "fr-FR",
        "language": "fr-FR",
        "transcript": [...],
        . . .
        "sourceLanguageConfidence": 0.8563
      },
```

## <a name="guidelines-and-limitations"></a>Pautas y limitaciones

* Los idiomas admitidos son alemán, chino (simplificado), español, francés, inglés, italiano, japonés, portugués (brasileño) y ruso.
* Si el audio contiene idiomas distintos de los que se incluyen en la lista anterior, el resultado es inesperado.
* Si Video Indexer no puede identificar el idioma con una confianza lo suficientemente alta (`>0.6`), el idioma de reserva es el inglés.
* Actualmente, no hay compatibilidad con archivos que contienen varios idiomas de audio. Si el audio incluye idiomas distintos, el resultado es inesperado. 
* El audio de baja calidad puede afectar a los resultados del modelo.
* El modelo requiere al menos un minuto de voz en el audio.
* El modelo está diseñado para reconocer una voz conversacional espontánea (no comandos de voz, canciones, etc.).

## <a name="next-steps"></a>Pasos siguientes

* [Información general](video-indexer-overview.md)
* [Identificación y transcripción automática del contenido de varios idiomas](multi-language-identification-transcription.md)
