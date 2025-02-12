---
title: 'Instalación de una puerta de enlace de datos local: Azure Logic Apps'
description: Antes de poder acceder a datos locales desde Azure Logic Apps, descargue e instale la puerta de enlace de datos local
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 09/01/2019
ms.openlocfilehash: a070b224b0cb1378f0ba6729af33840715dc0a29
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087720"
---
# <a name="install-on-premises-data-gateway-for-azure-logic-apps"></a>Instalación de una puerta de enlace de datos local para Azure Logic Apps

Para poder [conectarse a orígenes de datos locales desde Azure Logic Apps](../logic-apps/logic-apps-gateway-connection.md), descargue e instale la [puerta de enlace de datos local](https://aka.ms/on-premises-data-gateway-installer) en un equipo local. La puerta de enlace funciona como un puente que permite la transferencia de datos rápida y el cifrado entre orígenes de datos locales y las aplicaciones lógicas. Puede usar la misma instalación de puerta de enlace con otros servicios en la nube, tales como Power BI, Microsoft Flow, PowerApps y Azure Analysis Services. Para obtener información sobre cómo usar la puerta de enlace con estos servicios, consulte estos artículos:

* [Puerta de enlace de datos local de Microsoft Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Administración de una puerta de enlace de datos local en Microsoft PowerApps](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Puerta de enlace de datos local de Microsoft Flow](https://flow.microsoft.com/documentation/gateway-manage/)
* [Puerta de enlace de datos local de Azure Analysis Services](../analysis-services/analysis-services-gateway.md)

En este artículo se muestra cómo descargar, instalar y configurar la puerta de enlace de datos local para que pueda acceder a los orígenes de datos locales desde Azure Logic Apps. También puede obtener más información sobre [cómo funciona la puerta de enlace de datos](#gateway-cloud-service) más adelante en este tema. Para más información sobre la puerta de enlace, consulte [¿Qué es una puerta de enlace de datos local?](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem).

<a name="requirements"></a>

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

  * Debe usar la misma cuenta de Azure para instalar y administrar la puerta de enlace. Durante la instalación, usará esta cuenta de Azure para asociar la puerta de enlace del equipo con una suscripción de Azure. Más adelante, usará la misma cuenta al crear un recurso de Azure en Azure Portal para la instalación de la puerta de enlace. 

  * Debe iniciar sesión con una cuenta profesional o educativa, también conocida como *cuenta organizativa*, que se parece a `username@contoso.com`. No puede usar cuentas de Azure B2B (invitado) o cuentas Microsoft personales, como @hotmail.com o @outlook.com.

    > [!TIP]
    > Si se suscribió a una oferta de Office 365 y no proporcionó su correo electrónico del trabajo, la dirección de inicio de sesión podría tener un aspecto similar a `username@domain.onmicrosoft.com`. La cuenta se almacena en un inquilino de Azure Active Directory (Azure AD). En la mayoría de los casos, el nombre principal de usuario de la cuenta de Azure AD es el mismo que su dirección de correo electrónico.
    >
    > Para usar una [suscripción estándar de Visual Studio](https://visualstudio.microsoft.com/vs/pricing/) que esté asociada a una cuenta Microsoft, primero [cree un inquilino en Azure AD](../active-directory/develop/quickstart-create-new-tenant.md) o use el directorio predeterminado. Agregue un usuario con una contraseña al directorio y, a continuación, conceda acceso a la suscripción a ese usuario. 
    > A continuación, puede iniciar sesión durante la instalación de la puerta de enlace con este nombre de usuario y contraseña.

* Estos son los requisitos para el equipo local:

  **Requisitos mínimos**

  * .NET Framework 4.7.2
  * versión de 64 bits de Windows 7 o Windows Server 2008 R2 (o posterior)

  **Requisitos recomendados**

  * CPU de 8 núcleos
  * 8 GB de memoria
  * Versión de 64 bits de Windows Server 2012 R2 o posterior
  * Almacenamiento de unidad de estado sólido (SSD) para la puesta en cola

  > [!NOTE]
  > La puerta de enlace no es compatible con Windows Server 2016 Core.

* **Consideraciones relacionadas**

  * Solo puede instalar la puerta de enlace de datos local en un equipo local, no en un controlador de dominio. Aunque no tiene que instalar la puerta de enlace en el mismo equipo que el origen de datos. Solo necesita una puerta de enlace para todos los orígenes de datos, por lo que no es necesario instalar la puerta de enlace para cada uno de ellos.

    > [!TIP]
    > Para minimizar la latencia, puede instalar la puerta de enlace lo más cerca posible del origen de datos o en el mismo equipo, suponiendo que tiene los permisos necesarios.

  * Instale la puerta de enlace en un equipo que esté conectado a Internet por cable, esté siempre activado y no entre al modo de suspensión. De lo contrario, la puerta de enlace no se puede ejecutar, y el rendimiento podría verse afectado en una red inalámbrica.

  * Si tiene previsto usar la autenticación de Windows, asegúrese de instalar la puerta de enlace en un equipo que sea miembro del mismo entorno de Active Directory que los orígenes de datos.

  * La región que seleccione para la instalación de la puerta de enlace es la misma ubicación que debe seleccionar cuando cree posteriormente el recurso de puerta de enlace de Azure para la aplicación lógica. De forma predeterminada, esta región es la misma ubicación de su inquilino de Azure AD que administra su cuenta de Azure. Sin embargo, puede cambiar la ubicación durante la instalación de la puerta de enlace.

  * La puerta de enlace tiene dos modos: modo estándar y modo personal, que solo se aplica a Power BI. No se puede ejecutar más de una puerta de enlace en el mismo modo en el mismo equipo.

<a name="install-gateway"></a>

## <a name="install-data-gateway"></a>Instalación de la puerta de enlace de datos

1. [Descargue y ejecute el programa de instalación de la puerta de enlace en un equipo local](https://aka.ms/on-premises-data-gateway-installer).

1. Una vez que se abra el instalador, seleccione **Siguiente**.

   ![Introducción al instalador](./media/logic-apps-gateway-install/gateway-intro-screen.png)

1. Seleccione **Puerta de enlace de datos local (se recomienda)** , que es el modo estándar, y luego seleccione **Siguiente**.

   ![Selección del modo de puerta de enlace](./media/logic-apps-gateway-install/select-gateway-mode.png)

1. Revise los requisitos mínimos, mantenga la ruta de instalación predeterminada, acepte los términos de uso y, después, seleccione **Instalar**.

   ![Revisión de los requisitos y aceptación de los términos de uso](./media/logic-apps-gateway-install/accept-terms.png)

1. Cuando la puerta de enlace esté correctamente instalada, indique la dirección de correo electrónico de su cuenta organizativa y elija **Iniciar sesión**, por ejemplo:

   ![Inicio de sesión con una cuenta profesional o educativa](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

   Ya ha iniciado sesión en su cuenta.

1. Seleccione **Registrar una nueva puerta de enlace en este equipo** > **Siguiente**. Con este paso se registra la instalación de la puerta de enlace con el [servicio en la nube de la puerta de enlace](#gateway-cloud-service).

   ![Registro de la puerta de enlace](./media/logic-apps-gateway-install/register-gateway.png)

1. Facilite esta información para la instalación de puerta de enlace:

   * Un nombre de puerta de enlace que sea único en el inquilino de Azure AD.
   * La clave de recuperación que quiera usar, que debe tener al menos ocho caracteres.
   * Confirmación de la clave de recuperación

   ![Configuración de la puerta de enlace](./media/logic-apps-gateway-install/set-up-gateway.png)

   > [!IMPORTANT]
   > Guarde y conserve la clave de recuperación en un lugar seguro. Necesita esta clave por si alguna vez desea cambiar la ubicación o mover, recuperar o asumir el control de la instalación de una puerta de enlace.

   Observe la opción para **agregar a un clúster de puerta de enlace existente**, que se selecciona al instalar puertas de enlace adicionales para [escenarios de alta disponibilidad](#high-availability).

1. Compruebe la región para el servicio en la nube de la puerta de enlace y la instancia de [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) que se usó en la instalación de la puerta de enlace. De forma predeterminada, esta región es la misma ubicación de su inquilino de Azure AD para su cuenta de Azure.

   ![Comprobación de región](./media/logic-apps-gateway-install/check-region.png)

1. Para aceptar la región predeterminada, elija **Configurar**. Sin embargo, si la región predeterminada no es la que tiene más cerca, puede cambiar la región.

   *¿Por qué cambiar la región de la instalación de puerta de enlace?*

   Por ejemplo, para reducir la latencia, puede cambiar la región de la puerta de enlace a la misma región que la aplicación lógica. O bien, puede seleccionar la región más cercana al origen de datos local. El *recurso de puerta de enlace de Azure* y la aplicación lógica pueden tener ubicaciones distintas.

   1. Junto a la región actual, seleccione **Cambiar la región**.

      ![Cambio de región](./media/logic-apps-gateway-install/change-region.png)

   1. En la siguiente página, abra la lista **Seleccionar región**, seleccione la región que quiera y luego elija **Listo**.

      ![Seleccione otra región.](./media/logic-apps-gateway-install/select-region-gateway-install.png)

1. Revise la información en la ventana de confirmación final. En este ejemplo se utiliza la misma cuenta para Logic Apps, Power BI, PowerApps y Microsoft Flow, por lo que la puerta de enlace está disponible para todos estos servicios. Cuando esté listo, elija **Cerrar**.

   ![Puerta de enlace finalizada](./media/logic-apps-gateway-install/finished-gateway-default-location.png)

1. Ahora [cree el recurso de Azure para la instalación de la puerta de enlace](../logic-apps/logic-apps-gateway-connection.md).

## <a name="check-or-adjust-communication-settings"></a>Comprobación o ajuste de la configuración de comunicación

La puerta de enlace de datos local depende de Azure Service Bus para la conectividad en la nube y establece las conexiones salientes correspondientes a la región de Azure asociada de la puerta de enlace. Si su entorno de trabajo requiere que el tráfico pase a través de un proxy o firewall para acceder a Internet, esta restricción podría impedir la conexión de la puerta de enlace de datos local al servicio en la nube de la puerta de enlace y Azure Service Bus. La puerta de enlace tiene varias configuraciones de comunicación que se pueden ajustar. Para más información, consulte los temas siguientes:

* [Ajuste de la configuración de comunicación para la puerta de enlace de datos local](https://docs.microsoft.com/data-integration/gateway/service-gateway-communication)
* [Configuración de los valores del proxy para la puerta de enlace de datos local](https://docs.microsoft.com/data-integration/gateway/service-gateway-proxy)

<a name="high-availability"></a>

## <a name="high-availability-support"></a>Alta disponibilidad

Para evitar puntos de error únicos en el acceso a datos locales, puede tener varias instalaciones de puerta de enlace (solo en modo estándar) de modo que cada una esté en un equipo diferente, y configurarlas como clúster o grupo. De este modo, si la puerta de enlace principal no está disponible, las solicitudes de datos se enrutan a la segunda puerta de enlace, y así sucesivamente. Dado que solo puede instalar una puerta de enlace estándar en un equipo, debe instalar cada puerta de enlace adicional que se encuentre en el clúster en un equipo diferente. Todos los conectores que funcionan con la puerta de enlace de datos local admiten alta disponibilidad.

* Debe tener al menos una instalación de puerta de enlace en la misma suscripción de Azure que la puerta de enlace principal y la clave de recuperación para esa instalación.

* La puerta de enlace principal debe ejecutar la actualización de puerta de enlace de noviembre de 2017 o posterior.

Después de configurar la puerta de enlace principal, cuando vaya a instalar otra puerta de enlace, seleccione **Agregar a un clúster de puerta de enlace existente**, seleccione la puerta de enlace principal, que es la primera puerta de enlace que instaló, y proporcione la clave de recuperación para esa puerta. Para obtener más información, consulte [Clústeres de alta disponibilidad para puerta de enlace a datos local](https://docs.microsoft.com/data-integration/gateway/service-gateway-install#add-another-gateway-to-create-a-cluster).

<a name="update-gateway-installation"></a>

## <a name="change-location-migrate-restore-or-take-over-existing-gateway"></a>Cambio de ubicación, migración, restauración o control de una puerta de enlace existente

Si tiene que cambiar la ubicación de la puerta de enlace, trasladar la instalación de puerta de enlace a otro equipo, recuperar una puerta de enlace dañada o asumir la propiedad de una puerta de enlace existente, necesitará la clave de recuperación que se le facilitó durante la instalación de puerta de enlace.

1. Ejecute el instalador de puerta de enlace en el equipo que tiene la puerta de enlace existente. Si no tiene el instalador de la puerta de enlace más reciente, [descargue la versión más reciente de la puerta de enlace](https://aka.ms/on-premises-data-gateway-installer).

   > [!NOTE]
   > Antes de restaurar la puerta de enlace en el equipo que tiene la instalación de la puerta de enlace original, primero debe desinstalar la puerta de enlace en dicho equipo. Esta acción desconecta la puerta de enlace original.
   > Si quita o elimina un clúster de puerta de enlace para cualquier servicio en la nube, no podrá restaurar ese clúster.

1. Cuando se abra el instalador, inicie sesión con la misma cuenta de Azure que se usó para instalar la puerta de enlace.

1. Seleccione **Migrate, restore, or takeover an existing gateway** > (Migrar, restaurar o asumir el control de una puerta de enlace existente)**Siguiente**:

   ![Seleccione "Migrate, restore, or takeover an existing gateway" (Migrar, restaurar o asumir el control de una puerta de enlace existente).](./media/logic-apps-gateway-install/migrate-recover-take-over-gateway.png)

1. Seleccione entre los clústeres y las puertas de enlace disponibles y especifique la clave de recuperación para la puerta de enlace seleccionada; por ejemplo:

   ![Selección de puerta de enlace](./media/logic-apps-gateway-install/select-existing-gateway.png)

1. Para cambiar la región, seleccione **Cambiar la región** y luego elija la nueva región.

1. Cuando esté listo, seleccione **Configurar** para finalizar la tarea.

## <a name="tenant-level-administration"></a>Administración de nivel de inquilino

Para obtener visibilidad de todas las puertas de enlace de datos locales en un inquilino de Azure AD, los administradores globales de ese inquilino pueden iniciar sesión en el [centro de administración de Power Platform](https://powerplatform.microsoft.com) como administrador de inquilinos y seleccionar la opción **Puertas de enlace de datos**. Para obtener más información, consulte [Administración de nivel de inquilino para la puerta de enlace de datos local](https://docs.microsoft.com/data-integration/gateway/service-gateway-tenant-level-admin).

<a name="restart-gateway"></a>

## <a name="restart-gateway"></a>Reinicio de la puerta de enlace

De forma predeterminada, la instalación de la puerta de enlace del equipo local se ejecuta como una cuenta de servicio de Windows denominada "Servicio de puerta de enlace de datos local". Sin embargo, la instalación de la puerta de enlace usa el nombre `NT SERVICE\PBIEgwService` para sus credenciales de cuenta "Iniciar sesión como" y tiene los permisos "Iniciar sesión como servicio".

> [!NOTE]
> Su cuenta de servicio de Windows no es la misma que se usa para conectarse a orígenes de datos locales, ni la cuenta de Azure que usa para iniciar sesión en servicios en la nube.

Al igual que con cualquier otro servicio de Windows, puede iniciar y detener la puerta de enlace de varias maneras. Para obtener más información, consulte [Reinicio de una puerta de enlace de datos local](https://docs.microsoft.com/data-integration/gateway/service-gateway-restart).

<a name="gateway-cloud-service"></a>

## <a name="how-the-gateway-works"></a>Funcionamiento de la puerta de enlace

Los usuarios de su organización pueden acceder a los datos locales para los que ya tienen acceso autorizado. Sin embargo, antes de que estos usuarios puedan conectarse al origen de datos local, debe instalar y configurar una puerta de enlace de datos local. Normalmente, un administrador es la persona que instala y configura una puerta de enlace. Estas acciones pueden requerir permisos de administrador de servidor o un conocimiento especial de los servidores locales.

La puerta de enlace facilita la comunicación rápida y segura en segundo plano. Esta comunicación fluye entre un usuario en la nube, el servicio en la nube de la puerta de enlace y el origen de datos local. El servicio en la nube de la puerta de enlace cifra y almacena los detalles de la puerta de enlace y las credenciales del origen de datos. El servicio también enruta las consultas y sus resultados entre el usuario, la puerta de enlace y el origen de datos local.

La puerta de enlace funciona con firewalls y solo usa conexiones de salida. Todo el tráfico se origina como tráfico de salida seguro desde el agente de puerta de enlace. La puerta de enlace retransmite datos desde orígenes locales en canales cifrados a través de Azure Service Bus. Service Bus crea un canal entre la puerta de enlace y el servicio que realiza la llamada, pero no almacena ningún dato. Todos los datos que pasan por la puerta de enlace se cifran.

![Arquitectura para la puerta de enlace de datos local](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

> [!NOTE]
> Dependiendo del servicio en la nube, puede que tenga que configurar un origen de datos para la puerta de enlace.

Estos pasos describen lo que sucede cuando interactúa con un elemento conectado a un origen de datos local:

1. El servicio en la nube crea una consulta, junto con las credenciales cifradas para el origen de datos. Después, el servicio envía la consulta y las credenciales a la cola de la puerta de enlace para su procesamiento.

1. El servicio en la nube de puerta de enlace analiza la consulta e inserta la solicitud en Azure Service Bus.

1. Azure Service Bus envía las solicitudes pendientes a la puerta de enlace.

1. La puerta de enlace obtiene la consulta, descifra las credenciales y se conecta a uno o varios orígenes de datos con ellas.

1. La puerta de enlace envía la consulta al origen de datos para su ejecución.

1. Los resultados se envían del origen de datos a la puerta de enlace y, luego, al servicio en la nube de la puerta de enlace. Seguidamente, el servicio en la nube de la puerta de enlace usa los resultados.

### <a name="authentication-to-on-premises-data-sources"></a>Autenticación a orígenes de datos locales

Una credencial almacenada se usa para conectarse desde la puerta de enlace a los orígenes de datos locales. Independientemente del usuario, la puerta de enlace usa la credencial almacenada para conectarse. Puede haber excepciones de autenticación para servicios específicos, como DirectQuery y LiveConnect para Analysis Services en Power BI.

### <a name="azure-active-directory"></a>Azure Active Directory

Los servicios en la nube de Microsoft utilizan [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) para autenticar a los usuarios. Un inquilino de Azure AD contiene nombres de usuario y grupos de seguridad. Normalmente, la dirección de correo electrónico que se usa para el inicio de sesión es la misma que la del nombre principal de usuario (UPN) de la cuenta.

### <a name="what-is-my-upn"></a>¿Qué es mi UPN?

Si no es administrador de dominio, es posible que no conozca su UPN. Para buscar el UPN de la cuenta, ejecute el comando `whoami /upn` desde la estación de trabajo. Aunque el resultado parece una dirección de correo electrónico, en realidad se trata del UPN para su cuenta de dominio local.

### <a name="synchronize-an-on-premises-active-directory-with-azure-active-directory"></a>Sincronización de una instancia de Active Directory local con Azure Active Directory

El UPN para las cuentas de Active Directory locales y las cuentas de Azure AD debe ser el mismo. Por lo tanto, asegúrese de que cada cuenta de Active Directory local coincide con la cuenta de Azure AD. Los servicios en la nube solo conocen las cuentas dentro de Azure AD. Por lo tanto, no es necesario agregar una cuenta a su Active Directory local. Si la cuenta no existe en Azure AD, no puede usar esa cuenta. 

Estas son algunas formas de hacer coincidir las cuentas de Active Directory locales con Azure AD. 

* Agregue las cuentas manualmente a Azure AD.

  Cree una cuenta en Azure Portal o en el centro de administración de Microsoft 365. Asegúrese de que el nombre de cuenta coincide con el UPN de la cuenta de Active Directory local.

* Sincronice las cuentas locales con el inquilino de Azure AD mediante la herramienta de Azure Active Directory Connect.

  La herramienta Azure AD Connect proporciona opciones para la sincronización de directorios y la configuración de autenticación. Estas opciones incluyen la sincronización de hash de contraseñas, la autenticación de paso a través y la federación. Si no es un administrador de inquilinos o un administrador de dominio local, póngase en contacto con el administrador de TI para configurar Azure AD Connect. Azure AD Connect garantiza que su UPN de Azure AD coincida con el UPN de Active Directory local. Esta coincidencia ayuda si usa las conexiones en vivo de Analysis Services con funcionalidades de inicio de sesión único (SSO) o Power BI.

  > [!NOTE]
  > La sincronización de cuentas con la herramienta Azure AD Connect crea nuevas cuentas en su inquilino de Azure AD.

<a name="faq"></a>

## <a name="faq-and-troubleshooting"></a>Preguntas más frecuentes y solución de problemas

Para más información, consulte los temas siguientes:

* [Preguntas más frecuentes sobre la puerta de enlace de datos local](/data-integration/gateway/service-gateway-onprem-faq)
* [Solución de problemas de la puerta de enlace de datos local](/data-integration/gateway/service-gateway-tshoot)
* [Supervisión y optimización del rendimiento de la puerta de enlace](/data-integration/gateway/service-gateway-performance)

## <a name="next-steps"></a>Pasos siguientes

* [Conexión a datos locales desde aplicaciones lógicas](../logic-apps/logic-apps-gateway-connection.md)
* [Características de Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Conectores para Azure Logic Apps](../connectors/apis-list.md)
