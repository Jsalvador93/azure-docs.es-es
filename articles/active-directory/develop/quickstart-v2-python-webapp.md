---
title: Inicio rápido de la aplicación web de Python para la plataforma de identidad de Microsoft | Azure
description: Aprenda a implementar el inicio de sesión de Microsoft en una aplicación web de Python mediante OAuth2.
services: active-directory
documentationcenter: dev-center-name
author: abhidnya13
editor: ''
ms.assetid: 9551f0b5-04f2-44d7-87b5-756409180fe9
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2019
ms.author: abpati
ms.custom: aaddev
ms.openlocfilehash: 587e7a82e2a9cde8ff6d08274928ab22aa969061
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71309621"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-python-web-app"></a>Inicio rápido: Adición del inicio de sesión con Microsoft a una aplicación web de Python

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

En este inicio rápido, aprenderá a integrar una aplicación web de Python con la plataforma de identidad de Microsoft. Su aplicación iniciará la sesión de un usuario, obtendrá un token de acceso para llamar a Microsoft Graph API y realizará una solicitud a Microsoft Graph API.

Cuando haya completado la guía, la aplicación podrá aceptar inicios de sesión de cuentas Microsoft personales (como outlook.com, live.com y otras), y cuentas profesionales y educativas de cualquier empresa u organización que utilice Azure Active Directory.

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-python-webapp/python-quickstart.svg)

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar este ejemplo, necesitará lo siguiente:

- [Python 2.7+](https://www.python.org/downloads/release/python-2713) o [Python 3+](https://www.python.org/downloads/release/python-364/)
- [Flask](http://flask.pocoo.org/), [Flask-Session](https://pythonhosted.org/Flask-Session/), [solicitudes](https://2.python-requests.org/en/master/)
- [Python de MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-python) 
- Un inquilino de Azure Active Directory (Azure AD). Para más información sobre cómo obtener una suscripción de Azure AD, consulte [Obtención de un inquilino de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).

> [!div renderon="docs"]
>
> ## <a name="register-and-download-your-quickstart-app"></a>Registro y descarga de la aplicación de inicio rápido
>
> Tiene dos opciones para iniciar la aplicación de inicio rápido: rápido (opción 1) y manual (opción 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo
>
> 1. Vaya a [Azure Portal: registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps).
> 1. Seleccione **Nuevo registro**.
> 1. Escriba un nombre para la aplicación y seleccione **Registrar**.
> 1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo
>
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
>
> Para registrar la aplicación y agregar la información de registro de la aplicación a la solución de forma manual, siga estos pasos:
>
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
> 1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
> 1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
> 1. Seleccione **Nuevo registro**.
> 1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
>      - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `python-webapp`.
>      - En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
>      - En la sección **URI de redirección**, en la lista desplegable seleccione la plataforma **Web** y establezca el valor en `http://localhost:5000/getAToken`.
>      - Seleccione **Registrar**. En la página de **información general** de la aplicación, anote el valor del **Identificador de aplicación (cliente)** para su uso posterior.
> 1. En el menú de la izquierda, elija **Certificates & secrets** (Certificados y secretos) y haga clic en **Nuevo secreto de cliente** en la sección **Client Secrets** (Secretos de cliente):
>
>      - Escriba una descripción de la clave (del secreto de la aplicación de instancia).
>      - Seleccione una duración de la clave de **En 1 año**.
>      - Al hacer clic en **Agregar**, se mostrará el valor de la clave.
>      - Copie el valor de la clave. Lo necesitará más adelante.
>
> [!div class="sxs-lookup" renderon="portal"]
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
>
> Para que el código de ejemplo de este inicio rápido funcione, es preciso:
>
> 1. Agregue una dirección URL de respuesta como `http://localhost:5000/getAToken`.
> 1. Cree un secreto de cliente.
>
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-aspnet-webapp/green-check.png) La aplicación está configurada con este atributo

#### <a name="step-2-download-your-project"></a>Paso 2: Descarga del proyecto

[Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

#### <a name="step-3-configure-the-application"></a>Paso 3: Configuración de la aplicación

1. Extraiga el archivo ZIP en la carpeta local más próxima a la carpeta raíz (por ejemplo, **C:\Azure-Samples**).
1. Si usa un entorno de desarrollo integrado, abra el ejemplo en su IDE favorito (opcional).
1. Abra el archivo **app_config.py**, que se puede encontrar en la carpeta raíz, y reemplace el siguiente fragmento de código:

```python
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
CLIENT_ID = "Enter_the_Application_Id_here"
```

> [!div renderon="docs"]
> Donde:
>
> - `Enter_the_Application_Id_here`: es el identificador de aplicación de la aplicación que registró.
> - `Enter_the_Client_Secret_Here`: es el valor de **Secreto de cliente** que creó en **Certificates & Secrets** (Certificados y secretos) para la aplicación registrada.

#### <a name="step-4-run-the-code-sample"></a>Paso 4: Ejecución del ejemplo de código

1. Deberá instalar la biblioteca Python de MSAL, el marco de Flask, las sesiones de Flask para la administración de sesiones del lado servidor y solicitudes mediante pip de la manera siguiente:

   ```Shell
   pip install -r requirements.txt
   ```

2. Ejecute app.py desde el shell o la línea de comandos:

   ```Shell
   python app.py
   ```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las aplicaciones web que inician la sesión de los usuarios y que luego llaman a las API web:

> [!div class="nextstepaction"]
> [Escenario: Aplicaciones web que inician la sesión de los usuarios](scenario-web-app-sign-user-overview.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
