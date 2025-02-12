---
title: Creación y configuración de clústeres de Enterprise Security Package en Azure HDInsight
description: Aprenda a crear clústeres de Enterprise Security Package en Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 05/09/2019
ms.openlocfilehash: 9b9071eae4ec18cb1d5fd277f6f5403ce3997f48
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71261738"
---
# <a name="create-and-configure-enterprise-security-package-clusters-in-azure-hdinsight"></a>Creación y configuración de clústeres de Enterprise Security Package en Azure HDInsight

Enterprise Security Package (ESP) para Azure HDInsight le proporciona acceso a la autenticación basada en Active Directory, compatibilidad con varios usuarios y control de acceso basado en rol para los clústeres de Apache Hadoop en Azure. Los clústeres de ESP para HDInsight permiten a las organizaciones normalmente adheridas a estrictas directivas de seguridad corporativa procesar información confidencial de manera segura.

En esta guía se muestra cómo crear un clúster de Azure HDInsight habilitado para ESP. También muestra cómo crear una máquina virtual de IaaS Windows en la que se habilitan tanto Active Directory y Sistema de nombres de dominio (DNS). Use esta guía para configurar los recursos necesarios para permitir a los usuarios locales iniciar sesión en un clúster de HDInsight habilitado para ESP.

El servidor que cree actuará como sustitución de su entorno local *real*. Lo usará para los pasos de instalación y configuración. Más tarde repetirá los pasos en su propio entorno. 

Esta guía también le ayudará a crear un entorno de identidad híbrida mediante la sincronización de hash de contraseñas con Azure Active Directory (Azure AD). La guía complementa [Uso de Enterprise Security Package en HDInsight](apache-domain-joined-architecture.md).

Antes de que use este proceso en su propio entorno:
* Configure Active Directory y DNS.
* Habilite Azure AD.
* Sincronice cuentas de usuario locales en Azure AD.

![Diagrama de arquitectura de Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0002.png)

## <a name="create-an-on-premises-environment"></a>Creación de un entorno local

En esta sección, usará una plantilla de implementación rápida de Azure para crear nuevas máquinas virtuales, configurar instancias DNS y agregar un nuevo bosque de Active Directory.

1. Vaya a la plantilla de implementación rápida para [crear una máquina virtual de Azure con un nuevo bosque de Active Directory](https://azure.microsoft.com/resources/templates/active-directory-new-domain/).

1. Seleccione **Implementar en Azure**.
1. Inicie sesión en la suscripción de Azure.
1. En la página **Creación de una máquina virtual de Azure con un nuevo bosque de AD**:
    1. En la lista desplegable **Suscripción**, seleccione la suscripción donde desea implementar los recursos.
    1. Junto a **Grupo de recursos**, seleccione **Crear nuevo** y escriba el nombre *OnPremADVRG*.
    1. En el resto de los campos de la plantilla, escriba la siguiente información:

        * **Ubicación**: Centro de EE. UU.
        * **Nombre de usuario administrador**: HDIFabrikamAdmin
        * **Contraseña de administrador**: \<YOUR_PASSWORD>
        * **Domain Name** (Nombre de dominio): HDIFabrikam.com
        * **Prefijo de DNS**: hdifabrikam

        ![Plantilla correspondiente a Creación de una máquina virtual de Azure con un nuevo bosque de Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-azure-vm-ad-forest.png)

    1. Seleccione **Comprar**.
    1. Supervise la implementación y espere a que finalice.
    1. Asegúrese de que los recursos se crean en el grupo de recursos correcto, **OnPremADVRG**.

## <a name="configure-users-and-groups-for-cluster-access"></a>Configuración de usuarios y grupos para el acceso al clúster

En esta sección, va a crear usuarios que tendrán acceso al clúster de HDInsight al final de esta guía.

1. Conéctese al controlador de dominio mediante Escritorio remoto.
    1. Si ha usado la plantilla que se ha mencionado al principio, el controlador de dominio será una máquina virtual denominada **adVM** del grupo de recursos **OnPremADVRG**.
    1. En Azure Portal, seleccione **Grupos de recursos** > **OnPremADVRG** > **adVM** > **Conectar**.
    1. Seleccione la pestaña **RDP** > **Descargar archivo RDP**.
    1. Guarde el archivo en el equipo y ábralo.
    1. Si se le piden credenciales, use *HDIFabrikam\HDIFabrikamAdmin* como nombre de usuario. A continuación, escriba la contraseña que eligió de la cuenta de administrador.

1. Al abrirse la sesión de Escritorio remoto en la máquina virtual del controlador de dominio, en el panel **Administrador del servidor**, abra **Usuarios y equipos de Active Directory**. En la esquina superior derecha, seleccione **Herramientas** > **Usuarios y equipos de Active Directory**.

    ![En el panel Administrador del servidor, abra Administración de Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/server-manager-active-directory-screen.png)

1. Cree dos nuevos usuarios: **HDIAdmin** y **HDIUser**. Estos dos usuarios iniciarán sesión en los clústeres de HDInsight.

    1. En la página **Usuarios y equipos de Active Directory**, seleccione **Acción** > **Nuevo** > **Usuario**.

        ![Creación de un usuario de Active Directory nuevo](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-active-directory-user.png)

    1. En la página **Nuevo objeto: Usuario**, en **Nombre de inicio de sesión de usuario**, escriba *HDIUser*. Luego, seleccione **Siguiente**.

        ![Creación del primer objeto de usuario administrador](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0020.png)

    1. En la ventana emergente que aparece, escriba una contraseña para la nueva cuenta. Seleccione **La contraseña nunca expira** > **Aceptar**.
    1. Seleccione **Finalizar** para crear la nueva cuenta.
    1. Cree otro usuario llamado *HDIAdmin*.

        ![Creación de un segundo objeto de usuario administrador](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0024.png)

1. En la página **Usuarios y equipos de Active Directory**, seleccione **Acción** > **Nuevo** > **Grupo**. Agregue un grupo llamado *HDIUserGroup*.

    ![Creación de un grupo de Active Directory nuevo](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-active-directory-group.png)

    ![Creación de un nuevo objeto](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0028.png)

1. Agregue **HDIUser** a **HDIUserGroup** como miembro del grupo.

    1. Haga clic con el botón derecho en **HDIUserGroup** y seleccione **Propiedades**.
    1. En la pestaña **Miembros**, seleccione **Agregar**.
    1. En el cuadro **Escriba los nombres de objeto que desea seleccionar**, escriba *HDIUser*. Después seleccione **Aceptar**.
    1. Repita los pasos anteriores para la cuenta **HDIAdmin**.

        ![Incorporación del miembro HDIUser al grupo HDIUserGroup](./media/apache-domain-joined-create-configure-enterprise-security-cluster/active-directory-add-users-to-group.png)

Ahora ha creado su entorno de Active Directory. Ha agregado dos usuarios y un grupo de usuarios que pueden obtener acceso al clúster de HDInsight.

Los usuarios se sincronizarán con Azure AD.

### <a name="create-an-azure-ad-directory"></a>Crear un directorio de Azure AD

1. Inicie sesión en el Portal de Azure.
1. Seleccione **Crear un recurso** y escriba *directorio*. Seleccione **Azure Active Directory** > **Crear**.
1. En **Nombre de la organización**, escriba *HDIFabrikam*.
1. En **Nombre de dominio inicial**, escriba *HDIFabrikamoutlook*.
1. Seleccione **Crear**.

    ![Crear un directorio de Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-directory.png)

1. En el lado izquierdo de Azure Portal, seleccione **Azure Active Directory**.
1. Si es necesario, seleccione **Cambiar directorio** para cambiar al directorio nuevo: **HDIFabrikamoutlook**.
1. En **Administrar**, seleccione **Nombres de dominio personalizados** > **Agregar dominio personalizado**.
1. En **Nombre de dominio personalizado**, escriba *HDIFabrikam.com* y seleccione **Agregar dominio**.

![Creación de un dominio personalizado](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-custom-domain.png)

## <a name="configure-your-azure-ad-tenant"></a>Configuración del inquilino de Azure AD

Ahora va a configurar el inquilino de Azure AD de forma que pueda sincronizar usuarios y grupos desde la instancia de Active Directory en el entorno local a la nube.

1. Cree un administrador de inquilinos de Active Directory.
    1. Inicie sesión en Azure Portal y seleccione el inquilino de Azure AD, **HDIFabrikam**.
    1. En **Administrar**, seleccione **Usuarios** > **Nuevo usuario**.
    1. Escriba la siguiente información del nuevo usuario:
        * **Nombre**: fabrikamazureadmin
        * **Nombre de usuario**: fabrikamazureadmin@hdifabrikam.com
        * **Contraseña**: una contraseña segura de su elección

    1. En la sección **Grupos**, busque **Administradores de controladores de dominio de AAD** y haga clic en **Seleccionar**.

        ![Cuadro de diálogo Grupos de Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0038.png)

    1. Abra la sección **Rol del directorio** y, a la derecha, seleccione **Administrador global** > **Aceptar**.

        ![Cuadro de diálogo Rol de Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0040.png)

    1. Escriba una contraseña para el usuario. Seleccione **Crear**.

1. Si desea cambiar la contraseña del usuario recién creado \<fabrikamazureadmin@hdifabrikam.com>, use la identidad para iniciar sesión en Azure Portal. A continuación, se le pedirá que cambie la contraseña.

## <a name="sync-on-premises-users-to-azure-ad"></a>Sincronización de usuarios locales en Azure AD

### <a name="download-and-install-azure-ad-connect"></a>Descarga e instalación de Azure AD Connect

1. [Descarga de Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594).

1. Instale Azure AD Connect en el controlador de dominio.

    1. Abra el archivo ejecutable que descargó y acepte los términos de licencia. Seleccione **Continuar**.

        ![Página "Bienvenida a Azure AD Connect"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0052.png)

    1. Seleccione **Usar la configuración rápida** y complete la instalación.

        ![Configuración rápida de Azure AD Connect](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0054.png)

### <a name="configure-a-sync-with-the-on-premises-domain-controller"></a>Configuración de una sincronización con el controlador de dominio local

1. En la página **Conectar a Azure AD**, escriba el nombre de usuario y la contraseña del administrador global de Azure AD. Use el nombre de usuario `fabrikamazureadmin@hdifabrikam.com` que creó al configurar su inquilino de Active Directory. A continuación, seleccione  **Siguiente**. 

    ![Página "Conectar a Azure AD"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0058.png)

1. En la página **Conectarse a Active Directory Domain Services**, escriba el nombre de usuario y la contraseña de una cuenta de administrador de empresa. Use el nombre de usuario `HDIFabrikam\HDIFabrikamAdmin` y la contraseña que creó anteriormente. A continuación, seleccione  **Siguiente**. 

   ![Página "Conectar a Azure AD"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0060.png)
1. En la página **Configuración de inicio de sesión de Azure AD**, seleccione **Siguiente**.
   ![Página "Configuración de inicio de sesión de Azure AD"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0062.png)

1. En la página **Listo para configurar**, seleccione  **Instalar**.

   ![Página "Listo para configurar"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0064.png)

1. En la página **Configuración completa**, seleccione **Salir**.
   ![Página "Configuración completada"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0078.png)

1. Tras finalizar la sincronización, confirme que los usuarios que creó en el directorio IaaS se sincronizan con Azure AD.
   1. Inicie sesión en el Portal de Azure.
   1. Seleccione **Azure Active Directory** > **HDIFabrikam** > **Usuarios**.

### <a name="create-a-user-assigned-managed-identity"></a>Crear una identidad administrada asignada por el usuario

Cree una identidad administrada asignada por el usuario que puede usar para configurar Azure AD Domain Services (Azure AD DS). Para obtener más información, consulte [Creación, enumeración, eliminación o asignación de un rol a una identidad administrada que haya asignado el usuario mediante Azure Portal](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

1. Inicie sesión en el Portal de Azure.
1. Seleccione **Crear un recurso** y escriba *identidad administrada*. Seleccione **Identidad administrada asignada por el usuario** > **Crear**.
1. En **Nombre de recurso**, escriba *HDIFabrikamManagedIdentity*.
1. Seleccione su suscripción.
1. En **Grupo de recursos**, seleccione **Crear nuevo** y escriba *HDIFabrikam-CentralUS*.
1. En **Ubicación**, seleccione **Centro de EE. UU.**
1. Seleccione **Crear**.

![Creación de una identidad administrada asignada por el usuario](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0082.png)

### <a name="enable-azure-ad-ds"></a>Habilitación de Azure AD DS

Siga estos pasos para habilitar Azure AD DS. Para más información, consulte [Habilitación de Azure AD mediante Azure Portal](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started).

1. Cree una red virtual para hospedar a Azure AD DS. Ejecute el siguiente código de PowerShell.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Set-AzContext -Subscription 'SUBSCRIPTION_ID'
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS' -Location 'Central US' -Name 'HDIFabrikam-AADDSVNET' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'AADDS-subnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Inicie sesión en el Portal de Azure.
1. Seleccione **Crear recurso**, escriba *Servicios de dominio* y seleccione **Azure AD Domain Services**.
1. En la página **Básico**:
    1. En **Nombre de directorio**, seleccione el directorio de Azure AD que creó: **HDIFabrikam**.
    1. En **Nombre de dominio DNS**, escriba *HDIFabrikam.com*.
    1. Seleccione su suscripción.
    1. Especifique el grupo de recursos **HDIFabrikam-CentralUS**. En **Ubicación**, seleccione **Centro de EE. UU.**

        ![Datos básicos de Azure AD DS](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0084.png)

1. En la página **Red**, seleccione la red (**HDIFabrikam-VNET**) y la subred (**AADDS-subnet**) que creó con el script de PowerShell. O bien, elija **Crear nueva** para crear una red virtual ahora.

    ![Paso "Crear red virtual"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0086.png)

1. En la página **Grupo de administradores**, aparecerá una notificación que le indica que un grupo denominado **Administradores de DC de AAD** ya se ha creado para administrar este grupo. Puede modificar la pertenencia de este grupo si lo desea, pero en este caso no es necesario cambiarla. Seleccione **Aceptar**.

    ![Visualización del grupo de administradores de Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0088.png)

1. En la página **Sincronización**, habilite la sincronización completa seleccionando **Todos** > **Aceptar**.

    ![Habilitación de la sincronización de Azure AD DS](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0090.png)

1. En la página **Resumen**, compruebe los detalles de Azure AD DS y seleccione **Aceptar**.

    ![Resumen de la "habilitación de Azure AD Domain Services"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0092.png)

Después de habilitar Azure AD DS, un servidor DNS local se ejecuta en las máquinas virtuales de Azure AD.

### <a name="configure-your-azure-ad-ds-virtual-network"></a>Configuración de la red virtual de Azure AD DS

Siga estos pasos para configurar la red virtual de Azure AD DS (**HDIFabrikam-AADDSVNET**) para que use los servidores DNS personalizados.

1. Busque las direcciones IP de los servidores DNS personalizados. 
    1. Seleccione el recurso de Azure AD DS **HDIFabrikam.com**. 
    1. En **Administrar**, seleccione  **Propiedades**. 
    1. Busque las direcciones IP en  **Dirección IP en la red virtual**.

    ![Búsqueda de direcciones IP de DNS personalizadas para Azure AD DS](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0096.png)

1. Configure **HDIFabrikam-AADDSVNET** para usar las direcciones IP personalizadas 10.0.0.4 y 10.0.0.5.

    1. En **Configuración**, seleccione **Servidores DNS**. 
    1. seleccione **Personalizada**.
    1. En el cuadro de texto, escriba la primera dirección IP (*10.0.0.4*).
    1. Seleccione  **Guardar**.
    1. Repita los pasos para agregar la otra dirección IP (*10.0.0.5*).

En nuestro escenario, configuramos Azure AD DS para usar las direcciones IP 10.0.0.4 y 10.0.0.5, configurando la misma dirección IP en la red virtual de Azure AD DS:

![Página de los servidores DNS personalizados](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0098.png)

## <a name="securing-ldap-traffic"></a>Protección del tráfico de LDAP

El protocolo ligero de acceso a directorios (LDAP) se usa para leer de Azure Active Directory y escribir en él. Puede hacer que el tráfico de LDAP sea confidencial y seguro mediante la tecnología de Capa de sockets seguros (SSL) o Seguridad de la capa de transporte (TLS). Puede habilitar LDAP a través de SSL (LDAPS) mediante la instalación de un certificado con el formato correcto.

Para obtener más información sobre LDAP seguro, consulte [Configuración de LDAPS para un dominio administrado con Azure AD DS](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap).

En esta sección, cree un certificado autofirmado, descargue el certificado y configure LDAPS para el dominio administrado de Azure AD DS **HDIFabrikam**.

El script siguiente crea un certificado para **HDIFabrikam**. El certificado se guarda en la ruta de acceso *LocalMachine*.

```powershell
$lifetime = Get-Date
New-SelfSignedCertificate -Subject hdifabrikam.com `
-NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
-Type SSLServerAuthentication -DnsName *.hdifabrikam.com, hdifabrikam.com
```

> [!NOTE] 
> Cualquier utilidad o aplicación que crea una solicitud Public Key Cryptography Standards (PKCS) \#10 válida puede usarse para formar la solicitud de certificado SSL.

Compruebe que el certificado está instalado en el almacén **Personal** del equipo:

1. Inicie Microsoft Management Console (MMC).
1. Agregue el complemento **Certificados** que administra certificados en el equipo local.
1. Expanda  **Certificados (equipo local)**  > **Personal** > **Certificados**. Debe existir un nuevo certificado en el almacén **Personal**. Este certificado se emite para el nombre de host completo.

    ![Comprobación de la creación de certificados locales](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0102.png)

1. En el panel de la derecha, haga clic con el botón derecho en el certificado que creó. Apunte a  **Todas las tareas** y, a continuación, seleccione  **Exportar**.

1. En la página  **Exportar la clave privada** , seleccione  **Sí, exportar la clave privada**. El equipo donde se importará la clave necesita la clave privada para leer los mensajes cifrados.

    ![Página Exportar la clave privada del Asistente para exportar certificados](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0103.png)

1. En la página  **Exportar formato de archivo** , deje la configuración predeterminada y, a continuación, seleccione  **Siguiente**. 
1. En la página  **Contraseña** , escriba una contraseña para la clave privada. En **Cifrado**, seleccione **TripleDES-SHA1**. Luego, seleccione **Siguiente**.
1. En la página  **Archivo que se va a exportar** , escriba la ruta y el nombre del archivo de certificado exportado y seleccione  **Siguiente**. El nombre de archivo debe tener la extensión .pfx. Este archivo se configura en Azure Portal para establecer una conexión segura.
1. Habilite LDAPS para un dominio administrado de Azure AD DS.
    1. En Azure Portal, seleccione el dominio **HDIFabrikam.com**.
    1. En **Administrar**, seleccione **LDAP seguro**.
    1. En la página **LDAP seguro**, en **LDAP seguro**, seleccione **Habilitar**.
    1. Busque el archivo de certificado .pfx que exportó en el equipo.
    1. Escriba la contraseña del certificado.

    ![Habilitación de LDAP seguro](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0113.png)

1. Ahora que ha habilitado el protocolo LDAPS, asegúrese de que sea accesible habilitando el puerto 636.
    1. En el grupo de recursos **HDIFabrikam-CentralUS**, seleccione el grupo de seguridad de red **AADDS-HDIFabrikam.com-NSG**.
    1. En **Configuración**, seleccione **Reglas de seguridad de entrada** > **Agregar**.
    1. En la página **Agregar regla de seguridad de entrada**, especifique las siguientes propiedades y seleccione **Agregar**:

        | Propiedad | Valor |
        |---|---|
        | Source | Any |
        | Source port ranges | * |
        | Destination | Any |
        | Destination port range | 636 |
        | Protocolo | Any |
        | Action | Allow |
        | Priority | \<Número deseado> |
        | NOMBRE | Port_LDAP_636 |

    ![Cuadro de diálogo "Agregar regla de seguridad de entrada"](./media/apache-domain-joined-create-configure-enterprise-security-cluster/add-inbound-security-rule.png)

**HDIFabrikamManagedIdentity** es la identidad administrada asignada por el usuario. El rol de colaborador de Domain Services para HDInsight se habilita para la identidad administrada que permitirá esta identidad para leer, crear, modificar y eliminar las operaciones de Domain Services.

![Crear una identidad administrada asignada por el usuario](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0117.png)

## <a name="create-an-esp-enabled-hdinsight-cluster"></a>Creación de un clúster de HDInsight habilitado para ESP

Este paso requiere los siguientes requisitos previos:

1. Cree un nuevo grupo de recursos *HDIFabrikam-WestUS* en la ubicación **Oeste de EE. UU.**
1. Cree una red virtual que hospedará el clúster de HDInsight habilitado para ESP.

    ```powershell
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS' -Location 'West US' -Name 'HDIFabrikam-HDIVNet' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'SparkSubnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Cree una relación del mismo nivel entre la red virtual que hospeda Azure AD DS (`HDIFabrikam-AADDSVNET`) y la red virtual que hospedará el clúster de HDInsight habilitado para ESP (`HDIFabrikam-HDIVNet`). Use el siguiente código de PowerShell para emparejar las dos redes virtuales.

    ```powershell
    Add-AzVirtualNetworkPeering -Name 'HDIVNet-AADDSVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS')

    Add-AzVirtualNetworkPeering -Name 'AADDSVNet-HDIVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS')
    ```

1. Cree una nueva cuenta de Azure Data Lake Storage Gen2 llamada **Hdigen2store**. Configure la cuenta con la identidad administrada por el usuario **HDIFabrikamManagedIdentity**. Para obtener más información, consulte [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](../hdinsight-hadoop-use-data-lake-storage-gen2.md).

1. Configure DNS personalizado en la red virtual **HDIFabrikam-AADDSVNET**.
    1. Vaya a Azure Portal > **Grupos de recursos** > **OnPremADVRG** > **HDIFabrikam-AADDSVNET** > **Servidores DNS**.
    1. Seleccione **Personalizado** y escriba *10.0.0.4* y *10.0.0.5*.
    1. Seleccione **Guardar**.

        ![Almacenamiento de la configuración DNS personalizada para una red virtual](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0123.png)

1. Cree un nuevo clúster de Spark en HDInsight habilitado para ESP.
    1. Seleccione **Personalizado (tamaño, configuración, aplicaciones)** .
    2. Escriba detalles para **Aspectos básicos** (sección 1). Asegúrese de que el **Tipo de clúster** es **Spark 2.3 (HDI 3.6)** . Asegúrese de que el **Grupo de recursos** es **HDIFabrikam-CentralUS**.

    1. En **Seguridad y redes** (sección 2), rellene los siguientes detalles:
        * En **Enterprise Security Package**, seleccione **Habilitado**.
        * Seleccione **Usuario administrador de clúster** y seleccione la cuenta **HDIAdmin** que creó como usuario administrador local. Haga clic en **Seleccionar**.
        * Seleccione **Grupo de acceso de clúster** > **HDIUserGroup**. Cualquier usuario que agregue a este grupo en el futuro podrá acceder a los clústeres de HDInsight.

            ![Selección del grupo de acceso de clúster HDIUserGroup](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0129.jpg)

    1. Complete los demás pasos de la configuración del clúster y compruebe los detalles en **Resumen del clúster**. Seleccione **Crear**.

1. Inicie sesión en la interfaz de usuario de Ambari para el clúster recién creado en `https://CLUSTERNAME.azurehdinsight.net`. Use el nombre de usuario de administrador `hdiadmin@hdifabrikam.com` y su contraseña.

    ![Ventana de inicio de sesión de la interfaz de usuario de Apache Ambari](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0135.jpg)

1. En el panel del clúster, seleccione **Roles**.
1. En la página **Roles**, en **Asignar roles a estos**, junto al rol **Administrador de clústeres**, escriba el grupo *hdiusergroup*. 

    ![Asignación del rol Administrador de clústeres a hdiusergroup](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0137.jpg)

1. Abra su cliente de Secure Shell (SSH) e inicie sesión en el clúster. Use el **hdiuser** que creó en la instancia de Active Directory local.

    ![Inicio de sesión en el clúster con el cliente SSH](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0139.jpg)

Si puede iniciar sesión con esta cuenta, ha configurado el clúster de ESP correctamente para la sincronización con la instancia de Active Directory local.

## <a name="next-steps"></a>Pasos siguientes

Lea [Introducción a la seguridad de Apache Hadoop con ESP](hdinsight-security-overview.md).
