---
title: archivo de inclusión
description: archivo de inclusión
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/10/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 3b4992a16061bef782f012aa7887b248e3423234
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67568373"
---
**Requisitos del servidor de proceso/configuración**

**Componente** | **Requisito** 
--- | ---
**CONFIGURACIÓN DE HARDWARE** | 
Núcleos de CPU | 8 
RAM | 16 GB
Número de discos | 3, incluidos el disco del sistema operativo, el disco de memoria caché del servidor de procesos, la unidad de retención para la conmutación por recuperación 
Espacio libre en el disco (caché del servidor de procesos) | 600 GB
Espacio libre en el disco (disco de retención) | 600 GB
 | 
**CONFIGURACIÓN DE SOFTWARE** | 
Sistema operativo | Windows Server 2012 R2 <br> Windows Server 2016
Configuración regional del sistema operativo | Español (es-es)
Roles de Windows Server | No habilite estos roles: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
Directivas de grupo | No habilite estas directivas de grupo: <br> - Impedir el acceso al símbolo del sistema. <br> - Impedir el acceso a herramientas de edición del Registro. <br> - Confiar en la lógica de datos adjuntos de archivos. <br> - Activar la ejecución de scripts. <br> [Más información](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Ningún sitio web predeterminado debe existir previamente <br> - Ningún sitio web o aplicación que escuche en el puerto 443 deben existir previamente <br>- Habilitar la [autenticación anónima](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - Habilitar la configuración de [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) 
| 
**CONFIGURACIÓN DE RED** | 
Tipo de dirección IP | estática 
Puertos | 443 (orquestación del canal de control)<br>9443 (Transporte de datos) 
Tipo de NIC | VMXNET3 (si el servidor de configuración es una máquina virtual de VMware)
 |
**Acceso a Internet** (El servidor necesita acceder a estas direcciones URL, directamente o mediante proxy):|
\*.backup.windowsazure.com | Se usa para la transferencia y coordinación de datos replicados
\*.store.core.windows.net | Se usa para la transferencia y coordinación de datos replicados
\*blob.core.windows.net | Se usa para tener acceso a la cuenta de almacenamiento que almacena los datos replicados
\*.hypervrecoverymanager.windowsazure.com | Se usa para las operaciones de administración de replicación y coordinación
https:\//management.azure.com | Se usa para las operaciones de administración de replicación y coordinación 
*.services.visualstudio.com | Se usa con fines de telemetría (es opcional)
time.nist.gov | Se usan para comprobar la sincronización de la hora entre el sistema y la hora global.
time.windows.com | Se usan para comprobar la sincronización de la hora entre el sistema y la hora global.
| <ul> <li> https:\//login.microsoftonline.com </li><li> https:\//secure.aadcdn.microsoftonline-p.com </li><li> https:\//login.live.com </li><li> https:\//graph.windows.net </li><li> https:\//login.windows.net </li><li> https:\//www.live.com </li><li> https:\//www.microsoft.com </li></ul> | La configuración de OVF necesita acceder a estas direcciones URL. Azure Active Directory las usa para la administración de identidades y control de acceso
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi  | Para completar la descarga de MySQL. </br> En algunas regiones, la descarga podría redirigirse a la dirección URL de la red CDN. Asegúrese de que la dirección URL de la red CDN también esté en la lista blanca, si es necesario.
|
**SOFTWARE QUE DEBE INSTALAR** | 
VMware vSphere PowerCLI | Se debe instalar [PowerCLI versión 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) si el servidor de configuración se ejecuta en una máquina virtual de VMware.
MYSQL | Se debe instalar MySQL. Lo puede instalar manualmente o Site Recovery puede instalarlo. (Para más información, vaya a la [definición de configuración](../articles/site-recovery/vmware-azure-deploy-configuration-server.md#configure-settings))

**Requisitos de tamaño del servidor de proceso/configuración**

**CPU** | **Memoria** | **Disco de caché** | **Frecuencia de cambio de datos** | **Máquinas replicadas**
--- | --- | --- | --- | ---
8 vCPU<br/><br/> 2 sockets * 4 núcleos \@ 2,5 GHz | 16 GB | < 300 GB | 500 GB o menos | < 100 máquinas
12 vCPU<br/><br/> 2 socks * 6 núcleos a 2,5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | De 100 a 150 máquinas
16 vCPU<br/><br/> 2 socks * 8 núcleos \@ 2,5 GHz | 32 GB | 1 TB | 1 - 2 TB | De 150 a 200 máquinas

