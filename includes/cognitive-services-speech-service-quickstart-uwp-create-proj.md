---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/19/2019
ms.author: erhopf
ms.openlocfilehash: 0140981a694a7a7cd8556f7139a90d0656679d7d
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382167"
---
Para crear un proyecto de Visual Studio C++ para el desarrollo de la Plataforma universal de Windows (UPW), debe configurar las opciones de desarrollo de Visual Studio, crear el proyecto, seleccionar la arquitectura de destino, configurar la captura de audio e instalar el SDK de voz.

### <a name="set-up-visual-studio-development-options"></a>Configuración de las opciones de desarrollo de Visual Studio

Para empezar, asegúrese de que Visual Studio está configurado correctamente para el desarrollo de UWP:

1. Abra Visual Studio 2019 para mostrar la ventana **Inicio**.

   ![Ventana Inicio: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-start-window.png)

1. Seleccione **Continuar sin código** para ir al IDE de Visual Studio.

1. En la barra de menús de Visual Studio, seleccione **Herramientas** > **Get Tools and Features** (Obtener herramientas y características) para abrir el Instalador de Visual Studio y ver el cuadro de diálogo **Modificar**.

   ![Pestaña Cargas de trabajo, cuadro de diálogo Modificar, Instalador de Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-workload.png)

1. En la pestaña **Cargas de trabajo**, en **Windows**, busque la carga de trabajo **Desarrollo de la Plataforma universal de Windows**. Si la casilla situada junto a esa carga de trabajo ya está seleccionada, cierre el cuadro de diálogo **Modificar** y vaya al paso 6.

1. Active la casilla **Desarrollo de la Plataforma universal de Windows**, seleccione **Modificar** y, a continuación, en el cuadro de diálogo **Antes de comenzar**, seleccione **Continuar** para instalar la carga de trabajo de desarrollo de UWP. La instalación de la nueva característica puede tardar un rato.

1. Cierre el Instalador de Visual Studio.

### <a name="create-the-project-and-select-the-target-architecture"></a>Cree el proyecto y seleccione la arquitectura de destino.

Después, cree el proyecto.

1. En la barra de menús de Visual Studio, elija **Archivo** > **Nuevo** > **Proyecto** para mostrar la ventana **Crear un nuevo proyecto**.

   ![Creación de un proyecto: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-create-new-project.png)

1. Busque y seleccione **Aplicación vacía (Windows universal)** . Asegúrese de seleccionar la versión de C# de este tipo de proyecto (en lugar de Visual Basic).

1. Seleccione **Siguiente** para abrir la pantalla **Configurar el nuevo proyecto**. 

   ![Configuración de un nuevo proyecto: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-configure-your-new-project.png)

1. En **Nombre del proyecto**, escriba `helloworld`.

1. En **Ubicación**, vaya la carpeta en la que desea guardar el proyecto y selecciónela o créela.

1. Seleccione **Crear** para ir a la ventana **Nuevo proyecto de la Plataforma universal de Windows**.

   ![Cuadro de diálogo Nuevo proyecto de la Plataforma universal de Windows: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. En **Versión mínima** (el segundo cuadro desplegable), elija **Windows 10 Fall Creators Update (10.0; Compilación 16299)** , que es el requisito mínimo para el SDK de voz.

1. En **Versión de destino** (el primer cuadro desplegable), elija un valor idéntico o posterior al valor de **Versión mínima**.

1. Seleccione **Aceptar**. Se le devolverá al IDE de Visual Studio, con el nuevo proyecto creado y visible en el panel **Explorador de soluciones**.

   ![Proyecto HelloWorld: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-helloworld.png)

Ahora seleccione la arquitectura de la plataforma de destino. En la barra de herramientas de Visual Studio, busque el cuadro desplegable **Plataformas de solución**. (Si no lo ve, elija **Ver** > **Barra de herramientas** > **Estándar** para mostrar la barra de herramientas que contiene **Plataformas de solución**). Si está ejecutando Windows de 64 bits, elija **x64** en el cuadro desplegable. Windows de 64 bits puede ejecutar también aplicaciones de 32 bits, por lo que puede elegir **x86**, si lo prefiere.

> [!NOTE]
> El SDK de Voz solo admite los procesadores compatibles con Intel. Actualmente no se admiten los procesadores ARM.

### <a name="set-up-audio-capture"></a>Configuración de la captura de audio

A continuación, permita que el proyecto capture la entrada de audio:

1. En el **Explorador de soluciones**, haga doble clic en **Package.appxmanifest** para abrir el manifiesto de aplicación del paquete.

1. Seleccione la pestaña **Funcionalidades**.

   ![Pestaña Funcionalidades, manifiesto de aplicación del paquete: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-07-capabilities.png)

1. Active la casilla de la funcionalidad **Micrófono**.

1. En la barra de menús, elija **Archivo** > **Guardar Package.appxmanifest** para guardar los cambios.

### <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

Por último, instale el [paquete NuGet del SDK de voz](https://aka.ms/csspeech/nuget) y haga referencia al SDK de voz en el proyecto:

1. En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Manage NuGet Packages for Solution** (Administrar paquetes NuGet para la solución) para ir a la ventana **NuGet: solución**.

1. Seleccione **Examinar**.

   ![Captura de pantalla del cuadro de diálogo Administrar paquetes para la solución](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. En **Origen del paquete**, elija **nuget.org**.

1. En el cuadro de búsqueda **Buscar**, escriba `Microsoft.CognitiveServices.Speech` y, a continuación, elija el paquete cuando aparezca en los resultados de la búsqueda.

   ![Captura de pantalla del cuadro de diálogo Administrar paquetes para la solución](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png)

1. En el panel de estado del paquete situado junto a los resultados de la búsqueda, seleccione el proyecto **HelloWorld**.

1. Seleccione **Instalar**.

1. En el cuadro de diálogo **Vista previa de los cambios**, seleccione **Aceptar**.

1. En el cuadro de diálogo **Aceptación de licencia**, vea la licencia y, a continuación, seleccione **Acepto**. La instalación del paquete comienza y, cuando se completa, el panel **Salida** muestra un mensaje similar al siguiente: `Successfully installed 'Microsoft.CognitiveServices.Speech 1.6.0' to helloworld`.
