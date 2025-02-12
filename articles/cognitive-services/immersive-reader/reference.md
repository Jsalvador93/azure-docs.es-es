---
title: Referencia del SDK del Lector inmersivo
titleSuffix: Azure Cognitive Services
description: Referencia del SDK del Lector inmersivo
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 1d9fc20055fe3adb571b5a77330cc6537998cb5f
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534472"
---
# <a name="immersive-reader-sdk-reference"></a>Referencia del SDK del Lector inmersivo

El SDK del Lector inmersivo es una biblioteca de JavaScript que permite integrar dicho lector en la aplicación web.

## <a name="functions"></a>Functions

El SDK expone una sola función: `ImmersiveReader.launchAsync(token, subdomain, content, options)`.

### <a name="launchasync"></a>launchAsync

Inicia el Lector inmersivo dentro de un `iframe` en la aplicación web.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<HTMLDivElement>;
```

#### <a name="parameters"></a>Parámetros

| NOMBRE | type | DESCRIPCIÓN |
| ---- | ---- |------------ |
| `token` | string | Token de autenticación de Azure AD Consulte el [procedimiento de autenticación de Azure AD](./azure-active-directory-authentication.md). |
| `subdomain` | string | El subdominio personalizado del recurso Lector inmersivo en Azure. Consulte el [procedimiento de autenticación de Azure AD](./azure-active-directory-authentication.md). |
| `content` | [Contenido](#content) | Un objeto que contiene el contenido que se mostrará en el Lector inmersivo. |
| `options` | [Opciones](#options) | Opciones para configurar ciertos comportamientos del Lector inmersivo. Opcional. |

#### <a name="returns"></a>Devuelve

Devuelve un `Promise<HTMLDivElement>` que se resuelve cuando el Lector inmersivo se carga. `Promise` se resuelve en un elemento `div` cuyo único elemento secundario es un elemento `iframe` que contiene la página del Lector inmersivo.

#### <a name="exceptions"></a>Excepciones

El valor devuelto de `Promise` se rechazará con un objeto [`Error`](#error) si el Lector inmersivo no se puede cargar. Para más información, consulte los [códigos de error](#error-codes).

## <a name="types"></a>Tipos

### <a name="content"></a>Contenido

Contiene el contenido que se mostrará en el Lector inmersivo.

```typescript
{
    title?: string;      // Title text shown at the top of the Immersive Reader (optional)
    chunks: [ {          // Array of chunks
        content: string; // Plain text string
        lang?: string;   // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
        mimeType?: string; // MIME type of the content (optional). Defaults to 'text/plain' if not specified.
    } ];
}
```

#### <a name="supported-mime-types"></a>Tipos MIME compatibles

| Tipo MIME | DESCRIPCIÓN |
| --------- | ----------- |
| text/plain | Texto sin formato. |
| application/mathml+xml | Lenguaje de marcado matemático (MathML). [Más información](https://developer.mozilla.org/en-US/docs/Web/MathML).
| application/vnd.openxmlformats-officedocument.wordprocessingml.document | Documento de Microsoft Word con formato .docx.

### <a name="options"></a>Opciones

Contiene propiedades que configuran ciertos comportamientos del Lector inmersivo.

```typescript
{
    uiLang?: string;   // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;  // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number; // Z-index of the iframe that will be created (default is 1000)
    useWebview?: boolean; // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
}
```

### <a name="error"></a>Error

Contiene información sobre el error.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Códigos de error

| Código | DESCRIPCIÓN |
| ---- | ----------- |
| BadArgument | El argumento proporcionado no es válido; consulte `message` para detalles. |
| Tiempo de espera | El Lector inmersivo no se pudo cargar en el tiempo de espera especificado. |
| TokenExpired | El token suministrado en caché ha expirado. |
| Limitado | Se ha superado el límite de frecuencia de llamadas. |

## <a name="launching-the-immersive-reader"></a>Inicio del Lector inmersivo

El SDK proporciona el estilo predeterminado para el botón para iniciar el Lector inmersivo. Use el atributo de clase `immersive-reader-button` para habilitar este estilo.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>Atributos opcionales

Use los siguientes atributos para configurar la apariencia y sensibilidad del botón.

| Atributo | DESCRIPCIÓN |
| --------- | ----------- |
| `data-button-style` | Establece el estilo del botón. Puede ser `icon`, `text` o `iconAndText`. De manera predeterminada, su valor es `icon`. |
| `data-locale` | Establece la configuración regional, por ejemplo, `en-US` o `fr-FR`. El valor predeterminado es el idioma inglés. |
| `data-icon-px-size` | Establece el tamaño del icono en píxeles. El valor predeterminado es 20 píxeles. |

## <a name="browser-support"></a>Compatibilidad con exploradores

Use las versiones más recientes de los siguientes exploradores para disfrutar de la mejor experiencia con el Lector inmersivo.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Pasos siguientes

* Explorar el [SDK del Lector inmersivo en GitHub](https://github.com/microsoft/immersive-reader-sdk)
* [Inicio rápido: Creación de una aplicación web que inicia el Lector inmersivo (C#)](./quickstart.md)