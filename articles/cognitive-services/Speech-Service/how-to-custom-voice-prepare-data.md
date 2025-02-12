---
title: 'Preparación de los datos para la voz personalizada: servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Cree una voz personalizada para su marca con el servicio de Voz. Proporcione grabaciones en estudio y los scripts asociados y el servicio generará un modelo de voz único adaptado a la voz grabada. Use esta voz para realizar la síntesis de voz en sus productos, herramientas y aplicaciones.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 8b4b5553605042499a9a8f3343ac4e6678e7006f
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640428"
---
# <a name="prepare-data-to-create-a-custom-voice"></a>Preparación de los datos para crear una voz personalizada

Cuando esté listo para crear un modelo personalizado de texto a voz para su aplicación, el primer paso es reunir las grabaciones de audio y los scripts asociados para empezar a entrenar el modelo de voz. El servicio usa estos datos para crear una voz única optimizada para que coincida con la voz de las grabaciones. Cuando haya entrenado la voz, puede comenzar a sintetizarla en sus aplicaciones.

Puede comenzar con una pequeña cantidad de datos para crear una prueba de concepto. Sin embargo, cuantos más datos proporcione, más natural sonará su voz personalizada. Antes de entrenar su propio modelo de texto a voz, necesitará las grabaciones de audio y las transcripciones de texto asociadas. En esta página, revisaremos los tipos de datos, cómo se usan y cómo administrar cada uno.

## <a name="data-types"></a>Tipos de datos

Un conjunto de datos de entrenamiento de voz incluye grabaciones de audio y un archivo de texto con las transcripciones asociadas. Cada archivo de audio debe contener una sola expresión (una frase única o un solo turno en un sistema de diálogo) y tener una duración de menos de 15 segundos.

En algunos casos, puede que no tenga listo el conjunto de datos adecuado y quiera probar el entrenamiento de voz personalizada con los archivos de audio disponibles, cortos o largos, con o sin transcripciones. Nosotros proporcionamos herramientas (beta) para ayudarle a segmentar el audio en expresiones y preparar las transcripciones mediante la [API Batch Transcription](batch-transcription.md).

En esta tabla se enumeran los tipos de datos y cómo se usa cada uno para crear un modelo personalizado de texto a voz.

| Tipo de datos | DESCRIPCIÓN | Cuándo se deben usar | Servicio adicional necesario | Cantidad para entrenar un modelo | Configuraciones regionales |
| --------- | ----------- | ----------- | --------------------------- | ----------------------------- | --------- |
| **Expresiones individuales + transcripción relacionada** | Una colección (.zip) de archivos de audio (.wav) como expresiones individuales. Cada archivo de audio debe tener una longitud de 15 segundos o menos y estar emparejado con una transcripción con formato (.txt). | Grabaciones profesionales con transcripciones relacionadas | Listo para el entrenamiento. | Sin requisitos fijos para en-US y zh-CN. Más de 2000 expresiones diferentes para otras configuraciones regionales. | Todas las configuraciones regionales de voz personalizada |
| **Audio largo + transcripciones (beta)** | Una colección (.zip) de archivos de audio largos sin segmentar (más de 20 segundos), emparejados con una transcripción (.txt) que contiene todas las palabras habladas. | Tiene archivos de audio y transcripciones relacionadas, pero no están segmentados en expresiones. | Segmentación (mediante transcripción por lotes).<br>Transformación del formato de audio cuando sea necesario. | Sin requisitos fijos para en-US y zh-CN. | `en-US` y `zh-CN` |
| **Solo audio (beta)** | Una colección (.zip) de archivos de audio sin una transcripción. | Solo dispone de archivos de audio, sin transcripciones. | Segmentación + generación de transcripciones (mediante la transcripción por lotes).<br>Transformación del formato de audio cuando sea necesario.| Sin requisitos fijos para `en-US` y `zh-CN`. | `en-US` y `zh-CN` |

Los archivos deben agruparse por tipo en un conjunto de datos y cargarse como un archivo ZIP. Cada conjunto de datos solo puede contener un tipo de datos.

> [!NOTE]
> El número máximo de conjuntos de datos que se pueden importar por suscripción es de 10 archivos ZIP para usuarios de la suscripción gratuita (F0) y 500 para usuarios para la suscripción estándar (S0).

## <a name="individual-utterances--matching-transcript"></a>Expresiones individuales + transcripción relacionada

Puede preparar las grabaciones de expresiones individuales y la transcripción relacionada de dos maneras. Escriba un guion y haga que lo lea un locutor, o bien use el audio disponible públicamente y transcríbalo a texto. En este último caso, deberá editar las disfluencias de los archivos de audio, como las muletillas ("em") y otros sonidos de relleno, tartamudeos, palabras entre dientes o pronunciaciones erróneas.

Para crear una buena fuente de voz, realice las grabaciones en una sala silenciosa con un micrófono de alta calidad. El volumen constante, la velocidad de la conversación, el tono al hablar y las particularidades expresivas del habla son esenciales.

> [!TIP]
> Para crear una voz que se vaya a usar en una producción, le recomendamos que use un estudio de grabación profesional y un locutor. Para obtener más información, consulte [Cómo grabar ejemplos de voz para una voz personalizada](record-custom-voice-samples.md).

### <a name="audio-files"></a>Archivos de audio

Cada archivo de audio debe contener una sola expresión (una sola frase o un solo turno de un sistema de diálogo) y tener una duración inferior a 15 segundos. Todos los archivos deben estar en el mismo idioma hablado. La transformación de texto a voz personalizada en varios idiomas no se admite, excepto en el caso del chino al inglés bilingüe. Los archivos de audio deben tener un nombre de archivo numérico exclusivo con la extensión de nombre de archivo .wav.

Al preparar el audio, siga estas directrices.

| Propiedad | Valor |
| -------- | ----- |
| Formato de archivo | RIFF (.wav), agrupado en un archivo ZIP |
| Frecuencia de muestreo | Al menos 16 000 Hz |
| Formato de ejemplo | PCM, 16 bits |
| Nombre de archivo | Numérico, con la extensión. wav. No se permiten nombres de archivo duplicados. |
| Longitud de audio | Menor de 15 segundos |
| Formato de archivo | .zip |
| Tamaño de archivo máximo | 200 MB |

> [!NOTE]
> Se rechazarán los archivos .wav con una frecuencia de muestreo inferior a 16 000 Hz. Si un archivo ZIP contiene archivos .wav con distintas frecuencias de muestreo, solo se importarán las que sean iguales o superiores a 16 000 Hz. Actualmente el portal importa archivos .zip de hasta 200 MB. Sin embargo, pueden cargarse varios archivos.

### <a name="transcripts"></a>Transcripciones

El archivo de transcripción es un archivo de texto sin formato. Use estas directrices para preparar sus transcripciones.

| Propiedad | Valor |
| -------- | ----- |
| Formato de archivo | Texto sin formato (.txt) |
| Formato de codificación | ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE o UTF-16-BE. Con zh-CN, no se admiten las codificaciones ANSI/ASCII y UTF-8. |
| Número de expresiones por línea | **Una**: cada línea del archivo de transcripción debe contener el nombre de uno de los archivos de audio, seguido de la transcripción correspondiente. El nombre de archivo y la transcripción deben estar separados por un carácter de tabulación (\t). |
| Tamaño de archivo máximo | 50 MB |

Este es un ejemplo de cómo las transcripciones se organizan en expresiones (de una en una) en un archivo txt:

```
0000000001[tab] This is the waistline, and it's falling.
0000000002[tab] We have trouble scoring.
0000000003[tab] It was Janet Maslin.
```
Es importante que las transcripciones tengan una precisión del 100 % respecto al audio correspondiente. Los errores en las transcripciones darán lugar a la pérdida de calidad durante el entrenamiento.

> [!TIP]
> Al compilar las transformaciones de texto a voz en producción, seleccione aquellas expresiones (o scripts de escritura) que tengan en cuenta tanto la cobertura fonética como la eficiencia. ¿Tiene problemas para obtener los resultados que desea? [Póngase en contacto con el equipo de voz personalizada](mailto:speechsupport@microsoft.com) para averiguar más sobre nuestro asesoramiento.

## <a name="long-audio--transcript-beta"></a>Audio largo + transcripciones (beta)

En algunos casos, puede que no disponga de audio segmentado. Nosotros proporcionamos un servicio (beta) a través del portal de voz personalizada para ayudarle a segmentar los archivos de audio largos y crear transcripciones. Tenga en cuenta que este servicio se cobrará en función de su uso de la suscripción de voz a texto.

> [!NOTE]
> El servicio de segmentación de audio largo aprovechará la característica de transcripción de voz a texto por lotes, que solo admite usuarios de la suscripción estándar (S0). Durante el procesamiento de la segmentación, los archivos de audio y las transcripciones también se enviarán al servicio Custom Speech para refinar el modelo de reconocimiento y así pueda mejorar la precisión de los datos. Durante este proceso no se conserva ningún dato. Después de realizar la segmentación, solo las expresiones segmentadas y sus transcripciones de asignación se almacenarán para su descarga y entrenamiento.

### <a name="audio-files"></a>Archivos de audio

Al preparar el audio para la segmentación, siga estas directrices.

| Propiedad | Valor |
| -------- | ----- |
| Formato de archivo | RIFF (.wav) con una frecuencia de muestreo de al menos 16 khz y 16 bits en PCM o .mp3 con una velocidad de bits de al menos 256 KBps, agrupado en un archivo ZIP |
| Nombre de archivo | Solo caracteres ASCII. El uso de caracteres Unicode en el nombre producirá un error (por ejemplo, los caracteres chinos o símbolos como "—"). No se permiten nombres duplicados. |
| Longitud de audio | Más de 20 segundos |
| Formato de archivo | .zip |
| Tamaño de archivo máximo | 200 MB |

Todos los archivos de audio se deben agrupar en un archivo ZIP. Es correcto colocar archivos .wav y archivos .mp3 en un archivo ZIP de audio, pero no subcarpetas. Por ejemplo, puede cargar un archivo ZIP que contenga un archivo de audio llamado "kingstory.wav", que dure 45 segundos, y otro llamado "queenstory.mp3", que dure 200 segundos, sin subcarpetas. Todos los archivos. mp3 se transformarán al formato .wav después del procesamiento.

### <a name="transcripts"></a>Transcripciones

Las transcripciones deben estar preparadas de acuerdo con las especificaciones enumeradas en esta tabla. Cada archivo de audio debe coincidir con una transcripción.

| Propiedad | Valor |
| -------- | ----- |
| Formato de archivo | Texto sin formato (.txt), agrupado en un archivo ZIP |
| Nombre de archivo | Use el mismo nombre que el archivo de audio relacionado. |
| Formato de codificación | Solo UTF-8-BOM |
| Número de expresiones por línea | Sin límite |
| Tamaño de archivo máximo | 50 MB |

Todos los archivos de transcripciones de este tipo de datos deben estar agrupados en un archivo ZIP. No se permite ninguna subcarpeta en el archivo ZIP. Por ejemplo, ha cargado un archivo ZIP que contiene un archivo de audio llamado "kingstory.wav", que dura 45 segundos, y otro llamado "queenstory.mp3", que dura 200 segundos. Deberá cargar otro archivo ZIP que contenga dos transcripciones, una llamada "kingstory.txt" y la otra "queenstory.txt". Dentro de cada archivo de texto sin formato, proporcionará la transcripción completa correcta para el audio relacionado.

Después de que el conjunto de datos se ha cargado correctamente, le ayudaremos a segmentar el archivo de audio en expresiones según la transcripción proporcionada. Para comprobar las expresiones segmentadas y las transcripciones relacionadas, descargue el conjunto de datos. Se asignarán identificadores únicos a las expresiones segmentadas automáticamente. Es importante asegurarse de que las transcripciones que proporciona tengan una precisión del 100 %. Los errores en las transcripciones pueden reducir la precisión durante la segmentación del audio e introducir además pérdida de calidad en la fase de entrenamiento que viene más adelante.

## <a name="audio-only-beta"></a>Solo audio (beta)

Si no tiene transcripciones para las grabaciones de audio, use la opción **Solo audio** para cargar los datos. Nuestro sistema puede ayudarlo a segmentar y transcribir los archivos de audio. Tenga en cuenta que este servicio se tendrá en cuenta en su uso de la suscripción de voz a texto.

Al preparar el audio, siga estas directrices.

> [!NOTE]
> El servicio de segmentación de audio largo aprovechará la característica de transcripción de voz a texto por lotes, que solo admite usuarios de la suscripción estándar (S0).

| Propiedad | Valor |
| -------- | ----- |
| Formato de archivo | RIFF (.wav) con una frecuencia de muestreo de al menos 16 khz y 16 bits en PCM o .mp3 con una velocidad de bits de al menos 256 KBps, agrupado en un archivo ZIP |
| Nombre de archivo | Solo caracteres ASCII. El uso de caracteres Unicode en el nombre producirá un error (por ejemplo, los caracteres chinos o símbolos como "—"). No se permiten nombres duplicados. |
| Longitud de audio | Más de 20 segundos |
| Formato de archivo | .zip |
| Tamaño de archivo máximo | 200 MB |

Todos los archivos de audio se deben agrupar en un archivo ZIP. No se permite ninguna subcarpeta en el archivo ZIP. Una vez que el conjunto de datos se ha cargado correctamente, le ayudaremos a segmentar el archivo de audio en expresiones en función de nuestro servicio de transcripción de voz por lotes. Se asignarán identificadores únicos a las expresiones segmentadas automáticamente. Las transcripciones relacionadas se generarán mediante el reconocimiento de voz. Todos los archivos. mp3 se transformarán al formato .wav después del procesamiento. Para comprobar las expresiones segmentadas y las transcripciones relacionadas, descargue el conjunto de datos.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una voz personalizada](how-to-custom-voice-create-voice.md)
- [Guía: Grabación de muestras de voz](record-custom-voice-samples.md)
