---
title: Solución de problemas de copia de seguridad del estado del sistema con Azure Backup
description: Solucione problemas en la copia de seguridad del estado del sistema.
ms.reviewer: srinathv
author: dcurwin
manager: carmonm
keywords: procedimientos de copia de seguridad; copia de seguridad del estado del sistema
ms.service: backup
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: dacurwin
ms.openlocfilehash: 26ba811eba1a25dacddd04814f8e0d2805360920
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69018779"
---
# <a name="troubleshoot-system-state-backup"></a>Solución de problemas en la copia de seguridad del estado del sistema

En este artículo se describen soluciones para problemas que puede encontrar al usar Copia de seguridad de estado del sistema.

## <a name="basic-troubleshooting"></a>Pasos básicos para solucionar problemas
Antes de empezar a solucionar problemas de copia de seguridad del estado del sistema, se recomienda realizar la validación siguiente:

- [Asegúrese de que el agente de Microsoft Azure Recovery Services (MARS) esté actualizado](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [Asegúrese de que haya conectividad de red entre el agente de MARS y Azure](https://aka.ms/AB-A4dp50)
- Asegúrese de que se ejecuta Microsoft Azure Recovery Services (en la consola del servicio). Si es necesario, reinicie y vuelva a intentar la operación.
- [Asegúrese de que haya disponible entre un 5 % y un 10 % en la ubicación de la carpeta temporal](https://aka.ms/AB-AA4dwtt)
- [Compruebe si otro proceso o software antivirus interfiere con Azure Backup](https://aka.ms/AB-AA4dwtk)
- [Se produce un error en las copias de seguridad programadas pero no en las copias de seguridad manuales](https://aka.ms/ScheduledBackupFailManualWorks)
- Asegúrese de que su sistema operativo tiene las actualizaciones más recientes.
- [Asegúrese de excluir de la copia de seguridad las unidades no compatibles y los archivos con atributos no compatibles](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Asegúrese de que el **reloj del sistema** del sistema protegido está configurado con la zona horaria correcta <br>
- [Asegúrese de que el servidor tiene como mínimo .NET Framework 4.5.2 o versiones posteriores](https://www.microsoft.com/download/details.aspx?id=30653).<br>
- Si está intentando **volver a registrar el servidor** en un almacén: <br>
  - Asegúrese de que el agente no está instalado en el servidor y de que se ha eliminado del portal. <br>
  - Utilice la misma frase de contraseña que se usó inicialmente para registrar el servidor. <br>
- En caso de una copia de seguridad sin conexión, asegúrese de que Azure PowerShell versión 3.7.0 esté instalado en el equipo de origen y de copia antes de comenzar la operación de copia de seguridad sin conexión.
- [Consideración cuando el agente de copia de seguridad se ejecuta en una máquina virtual de Azure](https://aka.ms/AB-AA4dwtr)

### <a name="limitation"></a>Limitación
- Microsoft no recomienda la recuperación en un hardware diferente mediante la recuperación de estado del sistema.
- La copia de seguridad del estado del sistema actualmente admite servidores Windows "locales", funcionalidad que no está disponible para máquinas virtuales de Azure.

## <a name="pre-requisite"></a>Requisito previo

Antes de solucionar problemas de copia de seguridad del estado del sistema con Azure Backup, realice la comprobación de los requisitos previos siguientes.  

### <a name="verify-windows-server-backup-is-installed"></a>Comprobación de que Copias de seguridad de Windows Server está instalado

Asegúrese de que Copias de seguridad de Windows Server está instalado y habilitado en el servidor. Para comprobar el estado de la instalación, ejecute este comando de PowerShell:

 ```powershell
Get-WindowsFeature Windows-Server-Backup
 ```
Si en la salida se muestra **Instalar estado** como **disponible**, significa que la característica de copia de seguridad de Windows Server está disponible para la instalación pero no está instalada en el servidor. Pero si Copias de seguridad de Windows Server no está instalado, use uno de los métodos siguientes para instalarlo.

**Método 1: Instalación de Copias de seguridad de Windows Server mediante PowerShell**

Para instalar Copias de seguridad de Windows Server con PowerShell, ejecute el comando siguiente:

  ```powershell
  Install-WindowsFeature -Name Windows-Server-Backup
  ```

**Método 2: Instalación de Copias de seguridad de Windows Server mediante Administrador del servidor**

Para instalar Copias de seguridad de Windows Server mediante Administrador del servidor, ejecute los pasos siguientes:

1. En **Administrador del servidor**, haga clic en **Agregar roles y características**. Aparecerá el **Asistente para agregar roles y características**.

    ![panel](./media/backup-azure-system-state-troubleshoot/server_management.jpg)

2. Seleccione **Tipo de instalación** y haga clic en **Siguiente**.

    ![Tipo de instalación](./media/backup-azure-system-state-troubleshoot/install_type.jpg)

3. Seleccione un servidor del grupo de servidores y haga clic en **Siguiente**. En Rol de servidor, deje la selección predeterminada y haga clic en **Siguiente**.
4. Seleccione **Copias de seguridad de Windows Server** en la pestaña **Características** y haga clic en **Siguiente**.

    ![features](./media/backup-azure-system-state-troubleshoot/features.png)

5. En la pestaña **Confirmación**, haga clic en **Instalar** para iniciar el proceso de instalación.
6. En la pestaña **Resultados** se mostrará la característica Copias de seguridad de Windows Server instalada correctamente en el servidor de Windows.

    ![result](./media/backup-azure-system-state-troubleshoot/results.jpg)


### <a name="system-volume-information-permission"></a>Permiso Información de volumen del sistema

Asegúrese de que el sistema local tiene control total sobre la carpeta **Información de volumen del sistema** ubicada en el volumen donde está instalado Windows. Normalmente es **C:\Información de volumen del sistema**. Se puede producir un error en Copias de seguridad de Windows Server si los permisos anteriores no están establecidos correctamente

### <a name="dependent-services"></a>Servicios dependientes

Asegúrese de que todos los servicios siguientes están en estado de ejecución:

**Nombre del servicio** | **Tipo de inicio**
--- | ---
Llamada a procedimiento remoto (RPC) | Automático
Sistema de eventos COM+ (EventSystem) | Automático
Servicio de notificación de eventos de sistema (SENS) | Automático
Instantáneas de volumen (VSS) | Manual
Proveedor de instantáneas de software de Microsoft (SWPRV) | Manual

### <a name="validate-windows-server-backup-status"></a>Validación del estado de Copias de seguridad de Windows Server

Para validar el estado de Copias de seguridad de Windows Server, siga estos pasos:

  * Asegúrese de que WSB PowerShell está en ejecución.

    -   Ejecute `Get-WBJob` desde PowerShell con privilegios elevados y asegúrese de que no devuelve el error siguiente:

    > [!WARNING]
    > Get-WBJob: El término "Get-WBJob" no se reconoce como nombre de un cmdlet, una función, un archivo de script o un programa ejecutable. Compruebe si escribió correctamente el nombre o, si incluyó una ruta de acceso, compruebe que dicha ruta es correcta e inténtelo de nuevo.

    -   Si se produce este error, vuelva a instalar la característica Copias de seguridad de Windows Server en el equipo del servidor como se ha mencionado en los requisitos previos del paso 1.

  * Asegúrese de que la copia de seguridad de WSB funciona correctamente, mediante la ejecución del comando siguiente desde el símbolo del sistema con privilegios elevados:

      `wbadmin start systemstatebackup -backuptarget:X: -quiet`

      > [!NOTE]
      >Reemplace X por la letra de unidad del volumen donde quiera almacenar la imagen de copia de seguridad del estado del sistema.

    - Compruebe periódicamente el estado del trabajo mediante la ejecución del comando `Get-WBJob` desde PowerShell con privilegios elevados.        
    - Cuando finalice el trabajo de copia de seguridad, compruebe el estado final del trabajo mediante la ejecución del comando `Get-WBJob -Previous 1`.

Si se produce un error en el trabajo, indica un problema de WSB, lo que daría lugar a un error de copias de seguridad de estado del sistema del agente de MARS.

## <a name="common-errors"></a>Errores comunes

### <a name="vss-writer-timeout-error"></a>Error de tiempo de expiración de VSS Writer

| Síntoma | Causa | Resolución
| -- | -- | --
| - Se produce un error en el agente de MARS con el mensaje de error: "No se pudo realizar el trabajo de WSB porque se produjeron errores de VSS. Compruebe los registros de eventos de VSS para resolver estos errores"<br/><br/> - El registro de errores siguiente está presente en los registros de eventos de aplicación de VSS: "Un VSS Writer rechazó un evento con el error 0x800423f2, se agotó el tiempo de espera del escritor entre los eventos Freeze y Thaw".| VSS Writer no se puede completar a tiempo debido a la falta de recursos de CPU y memoria en el equipo <br/><br/> Otro software de copia de seguridad ya usa VSS Writer, y como resultado no se pudo completar la operación de instantánea para esta copia de seguridad | Espere a que se libere la CPU o la memoria en el sistema, o bien anule los procesos que consuman demasiada memoria y CPU, y vuelva a intentarlo <br/><br/>  Espere a que se complete la copia de seguridad en curso e intente la operación en un momento posterior, cuando no se ejecuten copias de seguridad en el equipo


### <a name="insufficient-disk-space-to-grow-shadow-copies"></a>Espacio en disco insuficiente para crear instantáneas

| Síntoma | Resolución
| -- | --
| - Se produce un error en el agente de MARS con el mensaje de error: Error al realizar la copia de seguridad debido a que el volumen de instantáneas no pudo crecer por la falta de espacio en disco en los volúmenes que contienen los archivos del sistema <br/><br/> - El registro de advertencia o error siguiente aparece en los registros de eventos del sistema de volsnap: "No hay espacio en disco suficiente en el volumen C: para aumentar el almacenamiento de instantáneas para las instantáneas de C: debido a este error, todas las instantáneas del volumen C: corren el riesgo de ser eliminadas" | - Libere espacio en el volumen resaltado en el registro de eventos con el fin de que haya suficiente espacio para que las instantáneas crezcan mientras la copia de seguridad está en curso <br/><br/> - Al configurar el espacio de instantáneas se puede restringir la cantidad de espacio que se usa para las instantáneas. Para más información, consulte este [artículo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788050(v=ws.11)#syntax).


### <a name="efi-partition-locked"></a>Partición EFI bloqueada

| Síntoma | Resolución
| -- | --
| Se produce un error en el agente de MARS con el mensaje de error: "Error de copia de seguridad del estado del sistema porque la partición de sistema EFI está bloqueada. Esto se puede deber al acceso a la partición del sistema por parte de software de seguridad o copia de seguridad de terceros" | - Si el problema se debe a software de seguridad de terceros, tendrá que ponerse en contacto con el proveedor de antivirus para que permita el agente de MARS <br/><br/> - Si se está ejecutando un software de copia de seguridad de terceros, espere que termine y vuelva a intentar la copia de seguridad


## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre el estado del sistema de Windows en la implementación de Resource Manager, vea [Copia de seguridad del estado del sistema de Windows Server](backup-azure-system-state.md).
