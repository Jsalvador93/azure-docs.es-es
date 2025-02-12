---
title: Solución de problemas de Azure Backup Server
description: Solucionar problemas de instalación, registro de Azure Backup Server y copia de seguridad y restauración de las cargas de trabajo de la aplicación.
ms.reviewer: srinathv
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: dacurwin
ms.openlocfilehash: 0f9c2d1d2081ec22898ed3a4fbc73305ff0995e3
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68954682"
---
# <a name="troubleshoot-azure-backup-server"></a>Solución de problemas de Azure Backup Server

Use la información de las tablas siguientes para solucionar los errores detectados al usar Azure Backup Server.

## <a name="basic-troubleshooting"></a>Pasos básicos para solucionar problemas

Antes de empezar a solucionar problemas de Microsoft Azure Backup Server (MABS), se recomienda realizar la validación siguiente:

- [Asegúrese de que el agente de Microsoft Azure Recovery Services (MARS) esté actualizado](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409).
- [Asegúrese de que haya conectividad de red entre el agente de MARS y Azure](https://aka.ms/AB-A4dp50)
- Asegúrese de que se ejecuta Microsoft Azure Recovery Services (en la consola del servicio). Si es necesario, reinicie y vuelva a intentar la operación.
- [Asegúrese de que haya disponible entre un 5 % y un 10 % en la ubicación de la carpeta temporal](https://aka.ms/AB-AA4dwtt)
- Si se produce un error en el registro, asegúrese de que el servidor en el que está intentando instalar Azure Backup Server no esté registrado en otro almacén.
- Si se produce un error de instalación automática, compruebe si el agente de DPM ya está presente. En caso afirmativo, desinstale al agente y vuelva a intentar la instalación.
- [Asegúrese de que ningún otro proceso o software antivirus interfiera con Azure Backup](https://aka.ms/AA4nyr4).<br>
- Asegúrese de que el servicio del agente SQL se esté ejecutando y esté establecido en Automático en el servidor MAB.<br>


## <a name="invalid-vault-credentials-provided"></a>Se han proporcionado credenciales de almacén no válidas

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Registrar en un almacén | Se han proporcionado credenciales de almacén no válidas. El archivo está dañado o no tiene asociadas las credenciales más recientes para el servicio de recuperación. | Acción recomendada: <br> <ul><li> Descargue el archivo de credenciales más reciente del almacén y vuelva a intentarlo. <br>O BIEN</li> <li> Si la acción anterior no funciona, intente descargar las credenciales en un directorio local diferente o cree un nuevo almacén. <br>O BIEN</li> <li> Intente actualizar la configuración de fecha y hora como se describe en [este blog](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/). <br>O BIEN</li> <li> Compruebe si c:\windows\temp tiene más de 65000 archivos. Mueva los archivos obsoletos a otra ubicación o elimine los elementos de la carpeta Temp. <br>O BIEN</li> <li> Compruebe el estado de los certificados. <br> a. Abra **Administrar certificados de equipo** (en el Panel de control). <br> b. Expanda el nodo **Personal** y su nodo secundario **Certificados**.<br> c.  Quite el certificado **Windows Azure Tools**. <br> d. Vuelva a intentar el registro en el cliente de Azure Backup. <br> O BIEN </li> <li> Compruebe si hay alguna directiva de grupo implementada. </li></ul> |

## <a name="replica-is-inconsistent"></a>La réplica es incoherente

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Copia de seguridad | La réplica es incoherente | Compruebe que está activada la opción para habilitar esa coherencia automática en el Asistente para nuevo grupo de protección. Para obtener más información acerca de las causas de la incoherencia de réplica y sugerencias relevantes, vea el artículo [La réplica es incoherente](https://technet.microsoft.com/library/cc161593.aspx).<br> <ol><li> En el caso de una copia de seguridad de BMR o estado del sistema, compruebe si Windows Server Backup está instalado en el servidor protegido.</li><li> Compruebe si hay problemas relacionados con el espacio en el grupo de almacenamiento de DPM en DPM o Microsoft Azure Backup Server, y asigne almacenamiento según sea necesario.</li><li> Compruebe el estado del Servicio de instantáneas de volumen en el servidor protegido. Si se encuentra en un estado deshabilitado, configúrelo para que se inicie manualmente. Inicie el servicio en el servidor. A continuación, vuelva a la consola de DPM o Microsoft Azure Backup Server y comience la sincronización con el trabajo de comprobación de coherencia.</li></ol>|

## <a name="online-recovery-point-creation-failed"></a>No se pudo crear el punto de recuperación en línea

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Copia de seguridad | No se pudo crear el punto de recuperación en línea | **Mensaje de error**: Windows Azure Backup Agent no pudo crear una instantánea del volumen seleccionado. <br> **Solución alternativa**: Intente aumentar el espacio en la réplica y el volumen del punto de recuperación.<br> <br> **Mensaje de error**: Windows Azure Backup Agent no se puede conectar al servicio OBEngine. <br> **Solución alternativa**: Compruebe que el servicio OBEngine figura en la lista de servicios en ejecución en el equipo. Si el servicio OBEngine no se está ejecutando, use el comando "net start OBEngine" para iniciarlo. <br> <br> **Mensaje de error**: No se ha establecido la frase de contraseña de cifrado para este servidor. Configure una frase de contraseña. <br> **Solución alternativa**: Intente configurar una frase de contraseña. Si hay algún problema, realice estos pasos: <br> <ol><li>Compruebe que existe la ubicación temporal. La ubicación mencionada en el Registro **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config** debe existir con el nombre **ScratchLocation**.</li><li> Si no existe la ubicación temporal, intente volver a registrar con la frase de contraseña de cifrado anterior. *Siempre que configure una frase de contraseña de cifrado, guárdela en una ubicación segura.*</li><ol>|

## <a name="the-vault-credentials-provided-are-different-from-the-vault-the-server-is-registered"></a>Las credenciales del almacén proporcionadas son diferentes de las credenciales del almacén en el que está registrado el servidor.

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Restauración | **Código de error**: CBPServerRegisteredVaultDontMatchWithCurrent/Error en las credenciales del almacén: 100110 <br/> <br/>**Mensaje de error**: Las credenciales del almacén proporcionadas son diferentes de las credenciales del almacén en el que está registrado el servidor. | **Causa**: Este problema se produce cuando intenta restaurar archivos en un servidor alternativo desde el servidor original mediante la opción de recuperación del DPM externo y si el servidor que se va a recuperar y el servidor original desde donde se realiza la copia de seguridad de los datos no están asociados al mismo almacén de Recovery Services.<br/> <br/>**Solución alternativa**: para resolver este problema, asegúrese de que tanto el servidor original como el servidor alternativo están registrados en el mismo almacén.|

## <a name="online-recovery-point-creation-jobs-for-vmware-vm-fail"></a>Hubo un error en los trabajos de creación de puntos de recuperación en línea para la máquina virtual de VMware.

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Copia de seguridad | Hubo un error en los trabajos de creación de puntos de recuperación en línea para la máquina virtual de VMware. DPM detectó un error de VMware al intentar obtener información de ChangeTracking. ErrorCode - FileFaultFault (ID 33621) |  <ol><li> Restablezca el CTK en VMware, para las máquinas virtuales afectadas.</li> <li>Compruebe que los discos independientes no están en VMware.</li> <li>Detenga la protección de las máquinas virtuales afectadas y vuelva a protegerlas con el botón **Actualizar**. </li><li>Ejecute CC para las máquinas virtuales afectadas.</li></ol>|


## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>Error en la operación del agente debido a un error de comunicación con el servicio Coordinador de agentes DPM en el servidor

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Insertar agentes en servidores protegidos | Error en la operación del agente debido a un error de comunicación con el servicio Coordinador de agentes DPM en \<nombreDelServidor>. | **Si no funciona la acción recomendada que se muestra en el producto, siga los pasos siguientes**: <ul><li> Si está conectando un equipo desde un dominio que no es de confianza, siga [estos pasos](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx). <br> O BIEN </li><li> Si está conectando un equipo desde un dominio de confianza, solucione el problema mediante los pasos descritos en [este blog](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/). <br>O BIEN</li><li> Pruebe a deshabilitar el antivirus como un paso para la solución de problemas. Si resuelve el problema, modifique la configuración del antivirus como se sugiere en [este artículo](https://technet.microsoft.com/library/hh757911.aspx).</li></ul> |

## <a name="setup-could-not-update-registry-metadata"></a>El programa de instalación no pudo actualizar los metadatos del Registro

| Operación | Detalles del error | Solución alternativa |
|-----------|---------------|------------|
|Instalación | El programa de instalación no pudo actualizar los metadatos del Registro. Este error de actualización podría provocar un uso excesivo de almacenamiento. Para evitar esto, actualice la entrada del Registro del recorte de ReFS. | Ajustar la clave del Registro **SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim**. Establezca el valor Dword en 1. |
|Instalación | El programa de instalación no pudo actualizar los metadatos del Registro. Este error de actualización podría provocar un uso excesivo de almacenamiento. Para evitar esto, actualice la entrada del Registro de SnapOptimization del volumen. | Cree la clave del Registro **SOFTWARE\Microsoft Data Protection Manager\Configuration\VolSnapOptimization\WriteIds** con un valor de cadena vacía. |

## <a name="registration-and-agent-related-issues"></a>Problemas relacionados con el registro y el agente

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Insertar agentes en servidores protegidos | Las credenciales especificadas para el servidor no son válidas. | **Si no funciona la acción recomendada que se muestra en el producto, siga los pasos siguientes**: <br> Pruebe a instalar de forma manual el agente de protección en el servidor de producción, como se especifica en [este artículo](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual).|
| El agente de Azure Backup no pudo conectar con el servicio Azure Backup (Id.: 100050) | El agente de Azure Backup no pudo conectar con el servicio Azure Backup. | **Si no funciona la acción recomendada que se muestra en el producto, siga los pasos siguientes**: <br>1. Desde el símbolo del sistema con privilegios elevados, ejecute el siguiente comando: **psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe**. Se abrirá la ventana de Internet Explorer. <br/> 2. Vaya a **Herramientas** > **Opciones de Internet** > **Conexiones** > **Configuración de LAN**. <br/> 3. Cambie la configuración para usar un servidor proxy. A continuación, proporcione los detalles del servidor proxy.<br/> 4. Si la máquina tiene limitado el acceso a Internet, asegúrese de que su configuración de firewall está establecida para permitir estas direcciones [URL](backup-configure-vault.md#verify-internet-access) e [IP](backup-configure-vault.md#verify-internet-access).|
| Error en la instalación del agente de Azure Backup | Error en la instalación de Microsoft Azure Recovery Services. Se revirtieron todos los cambios que la instalación de Microsoft Azure Recovery Services realizó en el sistema. (Id.: 4024) | Instalación manual del agente de Azure.


## <a name="configuring-protection-group"></a>Configurar grupo de protección

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Configurar grupos de protección | DPM no pudo enumerar el componente de la aplicación en el equipo protegido (nombre del equipo protegido). | Seleccione **Actualizar** en la pantalla de interfaz de usuario para configurar el grupo de protección en el nivel de origen de datos o el componente correspondiente. |
| Configurar grupos de protección | No se puede configurar la protección. | Si el servidor protegido es un servidor SQL Server, compruebe si se han proporcionado permisos de rol sysadmin para la cuenta del sistema (NTAuthority\System) en el equipo protegido, tal como se describe en [este artículo](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx).
| Configurar grupos de protección | No hay suficiente espacio disponible en el grupo de almacenamiento para este grupo de protección. | Los discos que se agregan al grupo de almacenamiento [no deben contener ninguna partición](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Elimine los volúmenes existentes en los discos. A continuación, agréguelos al grupo de almacenamiento.|
| Cambio de directiva |No se ha podido modificar la directiva de copia de seguridad. Error: No se pudo realizar la operación actual debido a un error de servicio interno [0x29834]. Vuelva a intentar la operación más tarde. Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft. | **Causa:**<br/>Este error se produce bajo tres condiciones: cuando está habilitada la configuración de seguridad, cuando intenta reducir la duración de retención por debajo de los valores mínimos especificados anteriormente y cuando se encuentra en una versión no admitida. (Las versiones no admitidas son las anteriores a la versión 2.0.9052 de Microsoft Azure Backup Server y a la actualización 1 de Azure Backup Server.) <br/>**Acción recomendada:**<br/> Para continuar con las actualizaciones relacionadas con la directiva, establezca el período de retención por encima del período mínimo especificado. (El período de retención mínimo es de siete días para diario, cuatro semanas para semanal, tres semanas para mensual o un año para anual.) <br><br>Si lo desea, otro enfoque preferido sería actualizar el agente de copia de seguridad y Azure Backup Server para aprovechar todas las actualizaciones de seguridad. |

## <a name="backup"></a>Copia de seguridad

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Copia de seguridad | Se produjo un error inesperado mientras se ejecutaba el trabajo. El dispositivo no está listo. | **Si no funciona la acción recomendada que se muestra en el producto, siga los pasos siguientes:** <br> <ul><li>Configure el espacio de almacenamiento de instantáneas como ilimitado para los elementos del grupo de protección y, después, ejecute la comprobación de coherencia.<br></li> O BIEN <li>Intente eliminar el grupo de protección existente y la creación de varios grupos nuevos. Cada nuevo grupo de protección debe tener un elemento individual.</li></ul> |
| Copia de seguridad | Si está haciendo una copia de seguridad solo del Estado del sistema, compruebe si hay suficiente espacio disponible en el equipo protegido para almacenarla. | <ol><li>Compruebe que Windows Server Backup está instalado en la máquina protegida.</li><li>Compruebe que hay suficiente espacio en el equipo protegido para el estado del sistema. La manera más fácil de comprobar esto consiste en ir al equipo protegido, abrir Windows Server Backup, hacer clic en las diferentes selecciones y, después, seleccionar BMR. A continuación, la interfaz de usuario indica cuánto espacio es necesario. Abra **WSB** > **Copia de seguridad local** > **Programación de copia de seguridad** > **Seleccionar configuración de copia de seguridad** > **Servidor completo** (se muestra el tamaño). Use este tamaño para la comprobación.</li></ol>
| Copia de seguridad | Error en la copia de seguridad de BMR | Si el tamaño de la reconstrucción completa es grande, mueva algunos archivos de aplicación a la unidad de sistema operativo y vuelva a intentarlo. |
| Copia de seguridad | La opción para volver a proteger una máquina virtual de VMware en una instancia nueva de Microsoft Azure Backup Server no se muestra como disponible para agregarla. | Las propiedades de VMware señalan a una instancia anterior y retirada de Microsoft Azure Backup Server. Para resolver este problema:<br><ol><li>En VCenter (equivalente a SC-VMM), vaya a la pestaña **Resumen** y, después, a **Atributos personalizados**.</li>  <li>Elimine el nombre antiguo de Microsoft Azure Backup Server del valor de **DPMServer**.</li>  <li>Vuelva a la nueva instancia de Microsoft Azure Backup Server y modifique el PG.  Después de usar el botón **Actualizar**, se presentará la máquina virtual con una casilla disponible para agregar a la protección.</li></ol> |
| Copia de seguridad | Error al acceder a carpetas o archivos compartidos | Intente modificar la configuración del antivirus como se sugiere en el artículo [Ejecución de software antivirus en el servidor DPM](https://technet.microsoft.com/library/hh757911.aspx) de TechNet.|


## <a name="change-passphrase"></a>Cambiar la frase de contraseña

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Cambiar la frase de contraseña |El PIN de seguridad escrito no es correcto. Proporcione el PIN de seguridad correcto para completar esta operación. |**Causa:**<br/> Este error se produce cuando se escribe un PIN de seguridad no válido o expirado al realizar una operación crítica (por ejemplo, la modificación de una frase de contraseña). <br/>**Acción recomendada:**<br/> Para completar la operación, debe escribir un PIN de seguridad válido. Para obtener el PIN, inicie sesión en Azure Portal y vaya al almacén de Recovery Services. A continuación, vaya a **Configuración** > **Propiedades** > **Generar PIN de seguridad**. Use este código PIN para cambiar la frase de contraseña. |
| Cambiar la frase de contraseña |Error en la operación Id.: 120002 |**Causa:**<br/>Este error se produce cuando se habilita la configuración de seguridad, o al intentar cambiar la frase de contraseña usando una versión no compatible.<br/>**Acción recomendada:**<br/> Para cambiar la frase de contraseña, primero debe actualizar el agente de copia de seguridad a la versión mínima, que es 2.0.9052. También debe actualizar Azure Backup Server a la actualización 1 como mínimo y, después, escribir un PIN de seguridad válido. Para obtener el PIN, inicie sesión en Azure Portal y vaya al almacén de Recovery Services. A continuación, vaya a **Configuración** > **Propiedades** > **Generar PIN de seguridad**. Use este código PIN para cambiar la frase de contraseña. |


## <a name="configure-email-notifications"></a>Configuración de notificaciones de correo electrónico

| Operación | Detalles del error | Solución alternativa |
| --- | --- | --- |
| Configurar notificaciones por correo electrónico con una cuenta de Office 365 |Id. del error: 2013| **Causa:**<br> Intentar usar la cuenta de Office 365 <br>**Acción recomendada:**<ol><li> En primer lugar, hay que asegurarse de que la opción para permitir la retransmisión anónima en un conector de recepción para el servidor DPM, está configurada en Exchange. Para obtener más información acerca de cómo configurarlo, vea [Permitir la retransmisión anónima en un conector de recepción](https://technet.microsoft.com/library/bb232021.aspx) en TechNet.</li> <li> Si no puede usar una retransmisión SMTP interna y necesita configurarla con un servidor Office 365, puede configurar IIS para que actúe como retransmisión. Configure el servidor DPM para [retransmitir el protocolo SMTP a Office 365 con IIS](https://technet.microsoft.com/library/aa995718(v=exchg.65).aspx).<br><br> **IMPORTANTE:** Asegúrese de usar el formato user\@domain.com y *no* domain\user.<br><br><li>Apunte a DMP para usar el nombre del servidor local como servidor SMTP, puerto 587. A continuación, apunte al correo electrónico del usuario del que deberían provenir los correos electrónicos.<li> El nombre de usuario y la contraseña en la página de configuración SMTP de DPM deben ser para una cuenta de dominio en la que el dominio DPM está habilitado. </li><br> **NOTA**: si modifica la dirección del servidor SMTP, realice el cambio en la nueva configuración, cierre el cuadro de configuración y vuelva a abrirlo para asegurarse de que refleja el nuevo valor.  No basta con cambiar y probar para que la nueva configuración entre en vigor, de modo que se recomienda realizar esta prueba.<br><br>En cualquier momento durante este proceso, puede borrar esta configuración cerrando la consola de DPM y modificando las siguientes claves del Registro: **HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> Elimine las claves SMTPPassword y SMTPUserName**. Puede agregarlas de nuevo en la interfaz de usuario cuando la inicie otra vez.


## <a name="common-issues"></a>Problemas comunes

En esta sección se tratan los errores que se pueden producir al usar Azure Backup Server.


### <a name="cbpsourcesnapshotfailedreplicamissingorinvalid"></a>CBPSourceSnapshotFailedReplicaMissingOrInvalid

Mensaje de error | Acción recomendada |
-- | --
No se pudo realizar la copia de seguridad porque la réplica de copia de seguridad de disco no se encuentra o no es válida. | Para resolver este problema, compruebe lo siguiente y vuelva a intentar la operación: <br/> 1. Cree un punto de recuperación de disco.<br/> 2. Ejecute una comprobación de coherencia en el origen de datos. <br/> 3. Detenga la protección del origen de datos y vuelva a configurar la protección de este origen de datos.

### <a name="cbpsourcesnapshotfailedreplicametadatainvalid"></a>CBPSourceSnapshotFailedReplicaMetadataInvalid

Mensaje de error | Acción recomendada |
-- | --
Se produjo un error en la instantánea de volumen del origen debido a que los metadatos de la réplica no son válidos. | Cree un punto de recuperación de disco de este origen de datos y vuelva a intentar la copia de seguridad en línea.

### <a name="cbpsourcesnapshotfailedreplicainconsistent"></a>CBPSourceSnapshotFailedReplicaInconsistent

Mensaje de error | Acción recomendada |
-- | --
Se produjo un error en la instantánea de volumen del origen debido a una réplica de origen de datos incoherente. | Ejecute una comprobación de coherencia en este origen de datos y vuelva a intentarlo.

### <a name="cbpsourcesnapshotfailedreplicacloningissue"></a>CBPSourceSnapshotFailedReplicaCloningIssue

Mensaje de error | Acción recomendada |
-- | --
No se pudo realizar la copia de seguridad porque no se pudo clonar la réplica de copia de seguridad de disco.| Asegúrese de que todos los archivos de réplica de copia de seguridad de disco (.vhdx) anteriores estén desmontados y no haya ninguna copia de seguridad de disco a disco en curso durante las copias de seguridad en línea.
