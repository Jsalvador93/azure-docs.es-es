---
title: Modificación de datos (LUIS)
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo se pueden modificar los datos antes de las predicciones en Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 198ce98808c8a62a839d154c365518c9e8263056
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619898"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>Modificación de datos de expresiones antes o durante la predicción
LUIS proporciona distintos métodos para manipular la expresión antes o durante la predicción. Entre ellos se incluyen la [corrección de la ortografía](luis-tutorial-bing-spellcheck.md) y la solución de los problemas de la zona horaria para elementos [datetimeV2](luis-reference-prebuilt-datetimev2.md) creados previamente. 

## <a name="correct-spelling-errors-in-utterance"></a>Corregir errores de ortografía de una expresión
LUIS usa [Bing Spell Check API V7](../Bing-Spell-Check/overview.md) para corregir los errores de ortografía de la expresión. LUIS necesita la clave asociada a ese servicio. Cree la clave y agréguela como parámetro de cadena de consulta en el [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356). 

También puede corregir los errores de ortografía en el panel **Prueba** [introduciendo la clave](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). La clave se conserva como variable de sesión en el explorador del panel Prueba. Agregue la clave al panel Prueba en todas las sesiones del explorador en las que quiera que se corrija la ortografía. 

El uso de la clave en el panel Prueba y en el punto de conexión cuentan para la cuota de [uso de claves](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/). LUIS implementa límites de Bing Spell Check para la longitud del texto. 

El punto de conexión requiere dos parámetros para que las correcciones ortográficas funcionen:

|Parámetro|Valor|
|--|--|
|`spellCheck`|boolean|
|`bing-spell-check-subscription-key`|Clave de punto de conexión de [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/)|

Cuando [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) detecta un error, se devuelven la expresión original y la expresión corregida junto con las predicciones del punto de conexión.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
### <a name="list-of-allowed-words"></a>Lista de palabras permitidas
La Bing Spell Check API que se utiliza en LUIS no admite una lista (también denominada lista blanca) de palabras permitidas para omitir durante las modificaciones de la corrección ortográfica. Si necesita admitir una lista de palabras o acrónimos, procese la expresión en la aplicación cliente antes de enviar la expresión a LUIS para la predicción de intenciones.

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Cambiar la zona horaria de la entidad datetimeV2 creada previamente
Cuando una aplicación de LUIS usa la entidad [datetimeV2](luis-reference-prebuilt-datetimev2.md) creada previamente, se puede devolver un valor de fecha y hora en la respuesta de la predicción. La zona horaria de la solicitud se usa para determinar la fecha y hora correctas que se van a devolver. Si la solicitud procede de un bot o de otra aplicación centralizada antes de llegar a LUIS, corrija la zona horaria que usa LUIS. 

### <a name="endpoint-querystring-parameter"></a>Parámetro de cadena de consulta de punto de conexión
La zona horaria se puede corregir agregando la zona horaria del usuario al [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356) mediante el parámetro `timezoneOffset`. El valor de `timezoneOffset` debe ser un número positivo o negativo (en minutos) para modificar la hora.  

|Parámetro|Valor|
|--|--|
|`timezoneOffset`|número positivo o negativo (en minutos)|

### <a name="daylight-savings-example"></a>Ejemplo de horario de verano
Si necesita que la entidad datetimeV2 creada previamente que se ha devuelto se ajuste al horario de verano, debe usar el parámetro de cadena de consulta `timezoneOffset` con un valor +/- en minutos para la consulta del [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356).

Agregar 60 minutos: 

https://{región}.api.cognitive.microsoft.com/luis/v2.0/apps/{IdAplicación}?q=Turn the lights on?**timezoneOffset=60**&verbose={booleano}&spellCheck={booleano}&staging={booleano}&bing-spell-check-subscription-key={cadena}&log={booleano}

Quitar 60 minutos: 

https://{región}.api.cognitive.microsoft.com/luis/v2.0/apps/{IdAplicación}?q=Turn the lights on?**timezoneOffset=-60**&verbose={booleano}&spellCheck={booleano}&staging={booleano}&bing-spell-check-subscription-key={cadena}&log={booleano}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>El código de C# determina el valor correcto de timezoneOffset
El siguiente código de C# usa el método [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples) de la clase [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo) para determinar el `timezoneOffset` correcto según la hora del sistema:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Corregir errores de ortografía con este tutorial](luis-tutorial-bing-spellcheck.md)
