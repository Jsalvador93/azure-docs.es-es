---
title: Personalización de la interfaz de usuario de Azure IoT Central | Microsoft Docs
description: Cómo personalizar los vínculos de ayuda y el tema de la aplicación de Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 6d120ecc322318e6daf72c506131bcd85c3be4e5
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879324"
---
# <a name="customize-the-azure-iot-central-ui-preview-features"></a>Personalización de la interfaz de usuario de Azure IoT Central (características de versión preliminar)

En este artículo se describe cómo, siendo administrador, puede personalizar la interfaz de usuario de la aplicación mediante la aplicación de temas personalizados y la modificación de los vínculos de ayuda para que apunten a sus propios recursos de ayuda personalizados. 

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

La captura de pantalla siguiente muestra una página con el tema estándar:

![Tema estándar de IoT Central](./media/howto-customize-ui-pnp/standard-ui.png)

La captura de pantalla siguiente muestra una página con una captura de pantalla personalizada con los elementos de la interfaz de usuario personalizados resaltados:

![Tema personalizado de IoT Central](./media/howto-customize-ui-pnp/themed-ui.png)

## <a name="create-theme"></a>Creación de un tema

Para crear un tema personalizado, vaya hasta la página **Personalizar la aplicación** en la sección **Administración**:

![Temas de IoT Central](./media/howto-customize-ui-pnp/themes.png)

En esta página, puede personalizar los siguientes aspectos de la aplicación:

### <a name="application-logo"></a>Logotipo de la aplicación

Una imagen PNG, no mayor de 1 MB, con un fondo transparente. Este logotipo se muestra a la izquierda en la barra de título de la aplicación de IoT Central.

Si la imagen del logotipo incluye el nombre de la aplicación, puede ocultar el texto. Para más información, consulte [Administración de la aplicación](./howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json#change-application-name-and-url).

### <a name="browser-icon-favicon"></a>Icono del explorador (favicon)

Una imagen PNG, no mayor de 32 x 32 píxeles, con un fondo transparente. Un explorador web puede usar esta imagen en la barra de direcciones, el historial, los marcadores y la pestaña del explorador.

### <a name="browser-colors"></a>Colores del explorador

Puede cambiar el color del encabezado de página y el color usado para resaltar los botones y otros elementos destacados. Utilice un valor de color hexadecimal de seis caracteres con el formato `##ff6347`. Para más información acerca de la notación de colores **Valor HEXADECIMAL**, consulte [Colores HTML](https://www.w3schools.com/html/html_colors.asp).

> [!NOTE]
> Siempre puede revertir a las opciones predeterminadas en la página **Personalización de la aplicación**.

### <a name="changes-for-operators"></a>Cambios para operadores

Si un administrador crea un tema personalizado, los operadores y otros usuarios de la aplicación ya no pueden elegir un tema en **Configuración**.

## <a name="replace-help-links"></a>Reemplazo de los vínculos de ayuda

Para proporcionar información de ayuda personalizada a los operadores y a otros usuarios, puede modificar los vínculos en el menú **Ayuda** de la aplicación.

Para modificar los vínculos de ayuda, vaya a la página **Personalizar la ayuda** en la sección **Administración**:

![Personalización de los vínculos de ayuda de IoT Central](./media/howto-customize-ui-pnp/help-links.png)

También puede agregar nuevas entradas al menú de ayuda y quitar las entradas predeterminadas:

![Ayuda de IoT Central personalizada](./media/howto-customize-ui-pnp/custom-help.png)

> [!NOTE]
> Siempre puede revertir a los vínculos de ayuda predeterminados en la página **Personalizar la ayuda**.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a personalizar la interfaz de usuario en la aplicación de Azure IoT Central, le sugerimos los pasos siguientes:

- [Administrar su aplicación](./howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
- [Configurar el panel de la aplicación](./howto-configure-homepage.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)