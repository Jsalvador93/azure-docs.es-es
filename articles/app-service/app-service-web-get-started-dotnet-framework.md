---
title: 'Creación de una aplicación web ASP.NET Framework de C#: Azure App Service | Microsoft Docs'
description: Aprenda a ejecutar aplicaciones web en Azure App Service mediante la implementación de la aplicación web ASP.NET de C# predeterminada.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 04a1becf-7756-4d4e-92d8-d9471c263d23
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 08/30/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 4f479ad60d74f1c7381b5fb776c5508aaa0785f1
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70242297"
---
# <a name="create-an-aspnet-framework-web-app-in-azure"></a>Creación de una aplicación web de ASP.NET Framework en Azure

[Azure App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático.

En este inicio rápido se muestra cómo implementar su primera aplicación web ASP.NET en Azure App Service. Cuando haya terminado, tendrá un plan de App Service. También tendrá una aplicación App Service con una aplicación web implementada.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, instale <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> con la carga de trabajo de **ASP.NET y desarrollo web**.

Si ya ha instalado Visual Studio 2019:

- Para instalar las actualizaciones más recientes de Visual Studio, seleccione **Ayuda** > **Buscar actualizaciones**.
- Para agregar la carga de trabajo, seleccione **Herramientas** > **Obtener herramientas y características**.

## Creación de una aplicación web ASP.NET<a name="create-and-publish-the-web-app"></a>

Cree una aplicación web ASP.NET siguiendo estos pasos:

1. Abra Visual Studio y seleccione **Crear un proyecto**.

1. En **Crear un proyecto**, busque y elija **Aplicación web ASP.NET Core (.NET Framework)** para C# y, a continuación, seleccione **Siguiente**.

1. En **Configurar el nuevo proyecto**, asigne al proyecto el nombre _myFirstAzureWebApp_ y luego seleccione **Crear**.

   ![Configuración del proyecto de aplicación web](./media/app-service-web-get-started-dotnet-framework/configure-web-app-project-framework.png)

1. Puede implementar cualquier tipo de aplicación web de ASP.NET en Azure. Para este inicio rápido, elija la plantilla **MVC**. 

1. Asegúrese de establecer la autenticación en **Sin autenticación**. Seleccione **Crear**.

   ![Creación de una aplicación web ASP.NET](./media/app-service-web-get-started-dotnet-framework/select-mvc-template-vs2019.png)

1. En el menú de Visual Studio, seleccione **Depurar** > **Iniciar sin depurar** para ejecutar la aplicación web localmente.

   ![Ejecución de la aplicación de forma local](./media/app-service-web-get-started-dotnet-framework/local-web-app.png)

## Publicación de la aplicación web <a name="launch-the-publish-wizard"></a>

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **myFirstAzureWebApp** y seleccione **Publicar**.

1. Elija **App Service** y seleccione **Publicar**.

   ![Publicar desde la página de información general del proyecto](./media/app-service-web-get-started-dotnet-framework/publish-app-framework-vs2019.png)

1. En **App Service Create new** (App Service - Crear nuevo), las opciones dependen de si ya ha iniciado sesión en Azure y de si tiene una cuenta de Visual Studio vinculada a una cuenta de Azure. Seleccione **Agregar una cuenta** o **Iniciar sesión** para iniciar sesión en la suscripción de Azure. Si ya ha iniciado sesión, seleccione la cuenta que desee.

   > [!NOTE]
   > Si ya ha iniciado sesión, no seleccione **Crear** todavía.
   >
   >

   ![Inicio de sesión en Azure](./media/app-service-web-get-started-dotnet-framework/sign-in-azure-framework-vs2019.png)

   [!INCLUDE [resource group intro text](../../includes/resource-group.md)]

1. En **Grupo de recursos**, seleccione **Nuevo**.

1. En **Nuevo nombre de grupo de recursos**, escriba *myResourceGroup* y seleccione **Aceptar**.

   [!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

1. En **Plan de hospedaje**, seleccione **Nuevo**.

1. En el cuadro de diálogo **Configurar un plan de hospedaje**, escriba los valores en la tabla siguiente y, después, seleccione **Aceptar**.

   | Configuración | Valor sugerido | DESCRIPCIÓN |
   |-|-|-|
   |Plan de App Service| myAppServicePlan | Nombre del plan de App Service. |
   | Location | Europa occidental | El centro de datos donde se hospeda la aplicación web. |
   | Size | Gratuito | [Plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) determina las características de hospedaje. |

   ![Creación de un plan de App Service](./media/app-service-web-get-started-dotnet-framework/app-service-plan-framework-vs2019.png)

1. En **Nombre**, escriba un nombre de aplicación único que incluya solo los caracteres válidos, que son `a-z`, `A-Z`, `0-9` y `-`. Puede aceptar el nombre único generado automáticamente. La dirección URL de la aplicación web es `http://<app_name>.azurewebsites.net`, donde `<app_name>` es el nombre de la aplicación.

   ![Configuración del nombre de la aplicación](./media/app-service-web-get-started-dotnet-framework/web-app-name-framework-vs2019.png)

1. Seleccione **Crear** para comenzar a crear los recursos de Azure.

Una vez completado el asistente, publica la aplicación web ASP.NET en Azure y, a continuación, inicia la aplicación en el explorador predeterminado.

![Aplicación web de ASP.NET publicada en Azure](./media/app-service-web-get-started-dotnet-framework/published-azure-web-app.png)

El nombre de aplicación especificado en la página **App Service Create new** (App Service - Crear nuevo) se usa como el prefijo de dirección URL en el formato `http://<app_name>.azurewebsites.net`.

**¡Enhorabuena!** La aplicación web ASP.NET se está ejecutando en Azure App Service.

## <a name="update-the-app-and-redeploy"></a>Actualización de la aplicación y nueva implementación

1. En **Explorador de soluciones**, en el proyecto, abra **Views** > **Home** > **Index.cshtml**.

1. Busque la etiqueta HTML `<div class="jumbotron">` en la parte superior y reemplace el elemento entero por el código siguiente:

   ```HTML
   <div class="jumbotron">
       <h1>ASP.NET in Azure!</h1>
       <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
   </div>
   ```

1. Para volver a implementar en Azure, haga clic con el botón derecho en el proyecto **myFirstAzureWebApp**, en el **Explorador de soluciones** y seleccione **Publicar**.

1. En la página de resumen **Publicar**, seleccione **Publicar**.

   ![Página de resumen de Visual Studio para su publicación](./media/app-service-web-get-started-dotnet-framework/publish-summary-page-framework-vs2019.png)

Cuando se completa la publicación, Visual Studio inicia un explorador en la dirección URL de la aplicación web.

![Aplicación web actualizada de ASP.NET en Azure](./media/app-service-web-get-started-dotnet-framework/updated-azure-web-app.png)

## <a name="manage-the-azure-app"></a>Administración de la aplicación de Azure

1. Vaya a <a href="https://portal.azure.com" target="_blank">Azure Portal</a> para administrar la aplicación web.

1. En el menú izquierdo, seleccione **App Services** y, después, el nombre de la aplicación de Azure.

   ![Navegación en el portal a la aplicación de Azure](./media/app-service-web-get-started-dotnet-framework/access-portal-framework-vs2019.png)

   Podrá ver la página de información general de la aplicación web. En ella, puede realizar una administración básica como examinar, detener, iniciar, reiniciar y eliminar.

   ![Introducción a App Service en Azure Portal](./media/app-service-web-get-started-dotnet-framework/web-app-general-framework-vs2019.png)

   El menú izquierdo proporciona distintas páginas para configurar la aplicación.

## <a name="video"></a>Vídeo

Vea el vídeo para ver este inicio rápido en acción y, a continuación, siga los pasos para publicar su primera aplicación de .NET en Azure.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [ASP.NET con SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)
