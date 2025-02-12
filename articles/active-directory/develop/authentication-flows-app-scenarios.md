---
title: Escenarios de autenticación en la Plataforma de identidad de Microsoft | Azure
description: Obtenga información sobre los flujos de autenticación y los escenarios de aplicaciones para la Plataforma de identidad de Microsoft. Obtenga información sobre los diferentes tipos de aplicaciones que pueden autenticar identidades, adquirir tokens y llamar a API protegidas.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/27/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5330111e5ae56471d26ebc39dca1a036246945e1
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348587"
---
# <a name="authentication-flows-and-application-scenarios"></a>Flujos de autenticación y escenarios de aplicaciones

El punto de conexión de la Plataforma de identidad de Microsoft (versión 2.0) admite la autenticación de una variedad de arquitecturas de aplicaciones modernas, todas ellas basadas en los protocolos estándar del sector [OAuth 2.0 u OpenID Connect](active-directory-v2-protocols.md).  Con las [bibliotecas de autenticación](reference-v2-libraries.md), las aplicaciones autentican las identidades y adquieren tokens para obtener acceso a las API protegidas. En este artículo se describen los diferentes flujos de autenticación y los escenarios de aplicaciones en los que se usan.  En este artículo también se proporcionan listas de [escenarios de aplicaciones y flujos de autenticación compatibles](#scenarios-and-supported-authentication-flows) y de [escenarios de aplicaciones y plataformas y lenguajes compatibles](#scenarios-and-supported-platforms-and-languages).

## <a name="application-categories"></a>Categorías de aplicaciones

Los tokens se pueden adquirir desde varios tipos de aplicaciones: Aplicaciones web, aplicaciones de escritorio o móviles, API web y aplicaciones que se ejecutan en dispositivos que no tienen un explorador (o iOT). Las aplicaciones se pueden clasificar por los siguientes elementos:

- [Recursos protegidos frente a aplicaciones cliente](#protected-resources-vs-client-applications). Algunos escenarios están relacionados con la protección de los recursos (Aplicaciones web o API web) y otros con la adquisición de un token de seguridad para llamar a una API web protegida.
- [Con usuarios o sin usuarios](#with-users-or-without-users). Algunos escenarios implican a un usuario que ha iniciado sesión, mientras que otros no implican a ningún usuario (escenarios de demonio).
- [Aplicaciones de una sola página, aplicaciones cliente públicas y aplicaciones cliente confidenciales](#single-page-applications-public-client-applications-and-confidential-client-applications). Son tres grandes categorías de tipos de aplicaciones. Las bibliotecas y los objetos que se usan para manipularlos serán diferentes.
- [Públicos de inicio de sesión](v2-supported-account-types.md#certain-authentication-flows-dont-support-all-the-account-types). Algunos flujos de autenticación no están disponibles para determinados públicos de inicio de sesión. Algunos flujos solo están disponibles para las cuentas profesionales o educativas y otros están disponibles para las cuentas profesionales o educativas y para las cuentas personales de Microsoft. El público permitido depende de los flujos de autenticación.
- [Flujos de OAuth 2.0 admitidos](#scenarios-and-supported-authentication-flows).  Los flujos de autenticación se usan para implementar los escenarios de aplicaciones que solicitan tokens.  No hay ninguna asignación individual entre los escenarios de aplicaciones y los flujos de autenticación.
- [Plataformas compatibles](#scenarios-and-supported-platforms-and-languages). No todos los escenarios de aplicaciones están disponibles para todas las plataformas.

### <a name="protected-resources-vs-client-applications"></a>Recursos protegidos frente a aplicaciones cliente

Los escenarios de autenticación implican dos actividades:

- **Adquirir tokens de seguridad** para una API web protegida. Microsoft le recomienda que use [bibliotecas de autenticación](reference-v2-libraries.md#microsoft-supported-client-libraries) para adquirir tokens; en concreto, la familia de bibliotecas de autenticación de Microsoft (MSAL).
- **Proteger una API web** (o una aplicación web). Uno de los desafíos de proteger un recurso (aplicación web o API web) es validar el token de seguridad. Microsoft ofrece, en algunas plataformas, [bibliotecas de middleware](reference-v2-libraries.md#microsoft-supported-server-middleware-libraries).

### <a name="with-users-or-without-users"></a>Con usuarios o sin usuarios

La mayoría de los escenarios de autenticación adquieren tokens en nombre de un **usuario** (con sesión iniciada).

![escenarios con usuarios](media/scenarios/scenarios-with-users.svg)

Sin embargo, también hay escenarios (aplicaciones de demonio) en los que las aplicaciones adquieren tokens en nombre de sí mismos (sin usuario).

![aplicaciones de demonio](media/scenarios/daemon-app.svg)

### <a name="single-page-applications-public-client-applications-and-confidential-client-applications"></a>Aplicaciones de una sola página, aplicaciones cliente públicas y aplicaciones cliente confidenciales

Los tokens de seguridad se pueden adquirir desde varios tipos de aplicaciones. Las aplicaciones tienden a dividirse en tres categorías:

- Las **aplicaciones de página única** (SPA) son un tipo de aplicaciones web en las que los tokens se adquieren desde la aplicación que se ejecuta en el explorador (escritas en JavaScript o Typescript). Muchas aplicaciones modernas tienen un front-end de aplicación de una página escrito principalmente en JavaScript. A menudo, la aplicación se escribe con un marco como Angular, React o Vue. MSAL.js es la única biblioteca de autenticación de Microsoft que admite aplicaciones de página única.

- **Las aplicaciones cliente públicas** siempre inician sesión de los usuarios. Estas aplicaciones son:
  - Aplicaciones de escritorio que llaman a API web en nombre del usuario que inició sesión.
  - Aplicaciones móviles.
  - Una tercera categoría de aplicaciones que se ejecuta en dispositivos que no tienen un explorador (aplicaciones sin explorador, que se ejecutan en iOT, por ejemplo).

  Están representados por la clase MSAL llamada [PublicClientApplication](msal-client-applications.md).

- **Aplicaciones cliente confidenciales**
  - Aplicaciones web que llaman a una API web
  - API web que llaman a una API web
  - Aplicaciones de demonio (incluso cuando se implementa como un servicio de consola, como un demonio en Linux o un servicio de Windows)
 
  Estos tipos de aplicaciones usan [ConfidentialClientApplication](msal-client-applications.md).

## <a name="application-scenarios"></a>Escenarios de aplicación

El punto de conexión de la Plataforma de identidad de Microsoft admite la autenticación para una variedad de arquitecturas de aplicaciones: aplicaciones de página única, aplicaciones web, API web, aplicaciones móviles y nativas, y aplicaciones demonio y del lado servidor.  Las aplicaciones usan los distintos flujos de autenticación para iniciar sesión de usuarios y obtener tokens para llamar a las API protegidas.

### <a name="single-page-application"></a>Aplicación de una sola página

Muchas aplicaciones web modernas se crean como aplicaciones cliente de una sola página, escritas con JavaScript o un marco de SPA, como Angular, Vue.js y React.js. Estas aplicaciones se ejecutan en un explorador web y tienen características de autenticación diferentes que las aplicaciones web de servidor tradicionales. La Plataforma de identidad de Microsoft permite que las aplicaciones de página única inicien la sesión de los usuarios y obtengan tokens para acceder a los servicios de back-end o a las API web.

![Aplicación de una sola página](media/scenarios/spa-app.svg)

Para más información, consulte [Aplicaciones de página única](scenario-spa-overview.md).

### <a name="web-application-signing-in-a-user"></a>Aplicación web que inicia sesión de un usuario

![Aplicación web que inicia sesión de usuarios](media/scenarios/scenario-webapp-signs-in-users.svg)

Para **proteger una aplicación web** (que inicia sesión del usuario), usará:

- En el entorno de .NET, ASP.NET o ASP.NET Core con el middleware ASP.NET Open ID Connect. En segundo plano, la protección de un recurso implica la validación del token de seguridad, que se realiza con las bibliotecas de [extensiones IdentityModel para la .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki), no con las bibliotecas MSAL.

- Si desarrolla en Node.js, usará Passport.js.

Para más información, consulte [Aplicación web que permite iniciar sesión a los usuarios](scenario-web-app-sign-user-overview.md).

### <a name="web-application-signing-in-a-user-and-calling-a-web-api-on-behalf-of-the-user"></a>Aplicación web que inicia sesión de un usuario y llama a una API web en nombre del usuario

![Aplicación web que llama a API web](media/scenarios/web-app.svg)

Desde la aplicación web, para **llamar a la API web** en nombre del usuario, usará MSAL `ConfidentialClientApplication`. Usará el flujo del código de autorización y almacenará el token adquirido en la caché de tokens. A continuación, el controlador adquirirá tokens de forma silenciosa de la caché cuando sea necesario. MSAL actualizará el token si es necesario.

Para más información, consulte [Aplicación web que llama a API web](scenario-web-app-call-api-overview.md).

### <a name="desktop-application-calling-a-web-api-on-behalf-of-the-signed-in-user"></a>Aplicación de escritorio que llama a una API web en nombre del usuario que ha iniciado sesión

Para llamar a una API web desde una aplicación de escritorio que inicia sesión de usuarios, va a usar los métodos de adquisición de tokens interactivo de PublicClientApplication de MSAL. Estos métodos interactivos le permiten controlar la experiencia de inicio de sesión en la interfaz de usuario. Para habilitar esta interacción, MSAL aprovecha un explorador web.

![Escritorio](media/scenarios/desktop-app.svg)

En el caso de las aplicaciones hospedadas en Windows que se ejecutan en equipos unidos a un dominio de Windows o a AAD, existe otra posibilidad. Estas aplicaciones pueden adquirir un token de forma silenciosa con la [autenticación integrada de Windows](https://aka.ms/msal-net-iwa).

Las aplicaciones que se ejecutan en un dispositivo sin un explorador podrán seguir llamando a una API en nombre de un usuario. Para realizar la autenticación, el usuario tendrá que iniciar sesión en otro dispositivo que tenga un explorador web. Para habilitar este escenario, deberá usar el [Flujo de código de dispositivo.](https://aka.ms/msal-net-device-code-flow)

![Flujo de código de dispositivo](media/scenarios/device-code-flow-app.svg)

Por último, y aunque no es recomendable, puede usar el [nombre de usuario y la contraseña](https://aka.ms/msal-net-up) en las aplicaciones clientes públicas. Este flujo todavía es necesario en algunos escenarios (como DevOps), pero tenga en cuenta que su uso impondrá restricciones a la aplicación. Por ejemplo, las aplicaciones que usan este flujo no pueden iniciar sesión de un usuario que necesite realizar la autenticación multifactor (acceso condicional). La aplicación no se beneficiará tampoco del inicio de sesión único. La autenticación con nombre de usuario y contraseña va en contra de los principios de la autenticación moderna y solo se proporciona por motivos de herencia.

En aplicaciones de escritorio, si quiere que la caché de tokens sea persistente, debe [personalizar la serialización de la caché de tokens](https://aka.ms/msal-net-token-cache-serialization). Incluso puede habilitar las cachés de tokens compatibles con versiones anteriores y posteriores con las generaciones anteriores de las bibliotecas de autenticación (ADAL.NET 3.x y 4.x) con la implmentación de la [serialización de la caché de tokens dobles](https://aka.ms/msal-net-dual-cache-serialization).

Para más información, consulte [Aplicación de escritorio que llama a las API web](scenario-desktop-overview.md).

### <a name="mobile-application-calling-a-web-api-on-behalf-of-the-user-whos-signed-in-interactively"></a>Aplicación móvil que llama a una API web en nombre del usuario que inicio sesión de manera interactiva

Similar a las aplicaciones de escritorio, la aplicación móvil usará los métodos de adquisición de tokens interactivo de PublicClientApplication de MSAL para adquirir un token para llamar a una API web.

![Móvil](media/scenarios/mobile-app.svg)

MSAL iOS y MSAL Android, de forma predeterminada, usan el explorador web del sistema. Sin embargo, también puede dirigirlo para que use la vista web insertada. Existen algunas características específicas en función de la plataforma móvil: (UWP, iOS, Android).

Algunos escenarios que implican el acceso condicional relacionado con el identificador del dispositivo o un dispositivo que se está inscribiendo requieren la instalación de un [agente](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/leveraging-brokers-on-Android-and-iOS) en un dispositivo. Algunos ejemplos de agentes son Microsoft Company Portal (en Android), Microsoft Authenticator (Android e iOS). MSAL ahora puede interactuar con agentes.

> [!NOTE]
> La aplicación móvil (que usa MSAL.iOS, MSAL.Android o MSAL.NET/Xamarin) puede tener políticas de protección de aplicaciones aplicadas (por ejemplo, que impidan que el usuario copie texto protegido). Está [administrado por Intune](https://docs.microsoft.com/intune/app-sdk) y reconocido por Intune como aplicación administrada. El [SDK de Intune](https://docs.microsoft.com/intune/app-sdk-get-started) es independiente de las bibliotecas de MSAL y se comunica con AAD por sí mismo.

Para más información, consulte [Aplicación móvil que llama a las API web](scenario-mobile-overview.md).

### <a name="protected-web-api"></a>API web protegida

Puede usar el punto de conexión de la Plataforma de identidad de Microsoft para proteger los servicios web, como la API web RESTful de la aplicación. Se llama a una API web protegida con un token de acceso para proteger sus datos y autenticar las solicitudes entrantes. El llamador de una API web anexa un token de acceso en el encabezado de autorización de una solicitud HTTP. Si quiere proteger su API web de ASP.NET o ASP.NET Core, tendrá que validar el token de acceso. Para ello, usará el middleware JWT de ASP.NET. En segundo plano, la validación se realiza con la biblioteca de [extensiones IdentityModel para .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki), no MSAL.NET.

Para más información, consulte [API web protegida](scenario-protected-web-api-overview.md).

### <a name="web-api-calling-another-downstream-web-api-on-behalf-of-the-user-for-whom-it-was-called"></a>API web que llama a otra API web de nivel inferior en nombre del usuario para la que se llamó

Si, además, quiere que su API web protegida de ASP.NET o ASP.NET Core llame a otra API web en nombre del usuario, la aplicación tendrá que adquirir un token para la API web de nivel inferior. Para ello, se usará el método de adquisición de tokens de ConfidentialClientApplication en [nombre de un usuario](https://aka.ms/msal-net-on-behalf-of). También se denomina llamadas de servicio a servicios.
Las API web que llaman a otra API web también tendrán que proporcionar una serialización de caché personalizada.

  ![API Web](media/scenarios/web-api.svg)

Para más información, consulte [API web que llama a las API web](scenario-web-api-call-api-overview.md).

### <a name="desktopservice-or-web-daemon-application-calling-web-api-without-a-user-in-its-own-name"></a>Aplicación de escritorio, servicio o de demonio web que llama a la API web sin un usuario (en su propio nombre)

Las aplicaciones que contienen procesos de larga duración o que funcionan sin la interacción de usuario también necesitan un modo de acceder a API web protegidas. Estas aplicaciones pueden autenticarse y obtener tokens mediante la identidad de la aplicación, en lugar de una identidad delegada del usuario. Pueden demostrar su identidad mediante un certificado o secreto de cliente.
Puede escribir aplicaciones de este tipo (aplicación de demonio) que adquieran un token para la aplicación en la parte superior con los métodos de adquisición de [credenciales de cliente](https://aka.ms/msal-net-client-credentials) de ConfidentialClientApplication de MSAL. Se supone que la aplicación registró previamente un secreto (contraseña de aplicación, certificado o aserción de cliente) con Azure AD, que luego se comparte con esta llamada.

![Aplicación de demonio](media/scenarios/daemon-app.svg)

Para más información, consulte [Aplicación de demonio que llama a las API web](scenario-daemon-overview.md).

## <a name="scenarios-and-supported-authentication-flows"></a>Escenarios y flujos de autenticación compatibles

Los escenarios que implican la adquisición de tokens también se asignan a los flujos de autenticación de OAuth 2.0 descritos en los detalles de los [Protocolos de la Plataforma de identidad de Microsoft](active-directory-v2-protocols.md).

<table>
 <thead>
  <tr><th>Escenario</th> <th>Tutorial detallado de escenarios</th> <th>Flujo o concesión de OAuth 2.0</th> <th>Público</th></tr>
 </thead>
 <tbody>
  <tr>
   <td><a href="scenario-spa-overview.md"><img alt="Single Page App" src="media/scenarios/spa-app.svg"></a></td>
   <td><a href="scenario-spa-overview.md">Aplicación de una sola página</a></td>
   <td><a href="v2-oauth2-implicit-grant-flow.md">Implícito</a></td>
   <td>Cuentas profesionales o educativas y cuentas personales, B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-web-app-sign-user-overview.md"><img alt="Web App that signs in users" src="media/scenarios/scenario-webapp-signs-in-users.svg"></a></td>
   <td><a href="scenario-web-app-sign-user-overview.md">Aplicación web que inicia sesión de usuarios</a></td>
   <td><a href="v2-oauth2-auth-code-flow.md">Código de autorización</a></td>
   <td>Cuentas profesionales o educativas y cuentas personales, B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-web-app-call-api-overview.md"><img alt="Web App that signs in users" src="media/scenarios/web-app.svg"></a></td>
   <td><a href="scenario-web-app-call-api-overview.md">Aplicación web que llama a las API web</a></td>
   <td><a href="v2-oauth2-auth-code-flow.md">Código de autorización</a></td>
   <td>Cuentas profesionales o educativas y cuentas personales, B2C</td>
 </tr>

  <tr>
   <td rowspan="3"><a href="scenario-desktop-overview.md"><img alt=Desktop app that calls web APIs" src="media/scenarios/desktop-app.svg"></a></td>
   <td rowspan="4"><a href="scenario-desktop-overview.md">Aplicación de escritorio que llama a las API web</a></td>
   <td>Interactivo (<a href="v2-oauth2-auth-code-flow.md">código de autorización</a> con PKCE)</td>
   <td>Cuentas profesionales o educativas y cuentas personales, B2C</td>
 </tr>

  <tr>
   <td>Autenticación de Windows integrada</td>
   <td>Cuentas profesionales o educativas</td>
 </tr>

  <tr>
   <td><a href="v2-oauth-ropc.md">Contraseña del propietario del recurso</a></td>
   <td>Cuentas profesionales o educativas, B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-desktop-acquire-token.md#command-line-tool-without-web-browser"><img alt="Browserless application" src="media/scenarios/device-code-flow-app.svg"></a></td>
   <td><a href="v2-oauth2-device-code.md">Código del dispositivo</a></td>
   <td>Cuentas profesionales o educativas*</td>
 </tr>

 <tr>
   <td rowspan="2"><a href="scenario-mobile-overview.md"><img alt="Mobile app that calls web APIs" src="media/scenarios/mobile-app.svg"></a></td>
   <td rowspan="2"><a href="scenario-mobile-overview.md">Aplicación móvil que llama a las API web</a></td>
   <td>Interactivo (<a href="v2-oauth2-auth-code-flow.md">código de autorización</a> con PKCE)</td>
   <td>Cuentas profesionales o educativas y cuentas personales, B2C</td>
 </tr>

  <tr>
   <td><a href="v2-oauth-ropc.md">Contraseña del propietario del recurso</a></td>
   <td>Cuentas profesionales o educativas, B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-daemon-overview.md"><img alt="Daemon app that calls Web APIs" src="media/scenarios/daemon-app.svg"></a></td>
   <td><a href=scenario-daemon-overview.md">Aplicación demonio que llama a las API web</a></td>
   <td><a href="v2-oauth2-client-creds-grant-flow.md">Credenciales de cliente</a></td>
   <td>Solo permisos de aplicación (sin usuario) en organizaciones de AAD</td>
 </tr>

  <tr>
   <td><a href=scenario-web-api-call-api-overview.md"><img alt="Web API that calls web APIs" src="media/scenarios/web-api.svg"></a></td>
   <td><a href=scenario-web-api-call-api-overview.md">API web que llama a API web</a></td>
   <td><a href="v2-oauth2-on-behalf-of-flow.md">En nombre de</a></td>
   <td>Cuentas profesionales o educativas y cuentas personales</td>
 </tr>

 </tbody>
</table>

## <a name="scenarios-and-supported-platforms-and-languages"></a>Escenarios, plataformas y lenguajes compatibles

No todos los tipos de aplicaciones están disponibles en todas las plataformas. También puede usar varios lenguajes para compilar las aplicaciones. Las bibliotecas de autenticación de Microsoft admiten varias **plataformas** (JavaScript, .NET Framework, .NET Core, Windows 10/UWP, Xamarin.iOS, Xamarin.Android, iOS nativo, Mac OS, Android nativo, Java y Python). En la tabla siguiente, en Windows, cada vez que se menciona .NET Core, también es posible .NET Framework (se omite para no sobrecargar la tabla)

|Escenario  | Windows | Linux | Mac | iOS | Android
|--|--|--|--|--|--|--|
| [Aplicación de una sola página](scenario-spa-overview.md) <br/>[![Aplicación de una sola página](media/scenarios/spa-app.svg)](scenario-spa-overview.md) | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js
| [Aplicación web que inicia sesión de usuarios](scenario-web-app-sign-user-overview.md) <br/>[![Aplicación web que inicia sesión de usuarios](media/scenarios/scenario-webapp-signs-in-users.svg)](scenario-web-app-sign-user-overview.md) | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core
| [Aplicación web que llama a las API web](scenario-web-app-call-api-overview.md) <br/> <br/>[![Aplicación web que llama a las API web](media/scenarios/web-app.svg)](scenario-web-app-call-api-overview.md) | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core y MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png) <br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Flask y Python de MSAL| ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core y MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Flask y Python de MSAL| ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core y MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/> ![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Flask y Python de MSAL
| [Aplicación de escritorio que llama a las API web](scenario-desktop-overview.md) <br/> <br/>[![Aplicación de escritorio que llama a las API web](media/scenarios/desktop-app.svg)](scenario-desktop-overview.md) ![Flujo de código de dispositivo](media/scenarios/device-code-flow-app.svg) | ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/> ![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL <br/> MSAL.objc |
| [Aplicación móvil que llama a las API web](scenario-mobile-overview.md) <br/> [![Aplicación móvil que llama a las API web](media/scenarios/mobile-app.svg)](scenario-mobile-overview.md) | ![UWP](media/sample-v2-code/small_logo_windows.png) MSAL.NET ![Xamarin](media/sample-v2-code/small_logo_xamarin.png) MSAL.NET | | | ![iOS / Objective C o Swift](media/sample-v2-code/small_logo_iOS.png) MSAL.objc | ![Android](media/sample-v2-code/small_logo_Android.png) MSAL.Android
| [Aplicación de demonio](scenario-daemon-overview.md) <br/> [![Aplicación de demonio](media/scenarios/daemon-app.svg)](scenario-daemon-overview.md) | ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png) MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL
| [API web que llama a API web](scenario-web-api-call-api-overview.md) <br/><br/> [![API web que llama a API web](media/scenarios/web-api.svg)](scenario-web-api-call-api-overview.md) | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core y MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core y MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core y MSAL.NET ![Java de MSAL](media/sample-v2-code/small_logo_java.png)<br/>msal4j<br/>![Python de MSAL](media/sample-v2-code/small_logo_python.png)<br/>Python de MSAL

Consulte también [Bibliotecas compatibles con Microsoft por sistema operativo y lenguaje](reference-v2-libraries.md#microsoft-supported-libraries-by-os--language).

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre [los aspectos básicos de la autenticación](authentication-scenarios.md) y los [tokens de acceso](access-tokens.md).
