---
title: Preguntas más frecuentes (P+F) sobre Azure Files | Microsoft Docs
description: Obtenga respuestas a algunas preguntas frecuentes sobre Azure Files.
author: roygara
ms.service: storage
ms.date: 07/30/2019
ms.author: rogarana
ms.subservice: files
ms.topic: conceptual
ms.openlocfilehash: cc827f52d227ee36620bd215dfcba96b433804d3
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71103054"
---
# <a name="frequently-asked-questions-faq-about-azure-files"></a>Preguntas más frecuentes (P+F) sobre Azure Files
[Azure Files](storage-files-introduction.md) ofrece recursos compartidos de archivos en la nube totalmente administrados, a los que se puede acceder mediante el [protocolo de bloque de mensajes del servidor (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) estándar. Los recursos compartidos de archivos de Azure se pueden montar simultáneamente en implementaciones de Windows, Linux y macOS en la nube o locales. También puede almacenar en caché recursos compartidos de archivos de Azure en máquinas con Windows Server mediante Azure File Sync para tener un acceso rápido cerca de donde se usan los datos.

En este artículo se responden las preguntas más frecuentes sobre las características y las funcionalidades de Azure Files, incluido el uso de Azure File Sync con Azure Files. Si no encuentra una respuesta a su pregunta, póngase en contacto con nosotros mediante los siguientes canales (en orden incremental):

1. La sección Comentarios de este artículo.
2. [Foro de Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [UserVoice de Azure Files](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Soporte técnico de Microsoft. Para crear una solicitud de soporte técnico, en Azure Portal, vaya a la pestaña **Ayuda**, seleccione el botón **Ayuda y soporte técnico** y elija **Nueva solicitud de soporte técnico**.

## <a name="general"></a>General
* <a id="why-files-useful"></a>
   **¿Por qué es útil Azure Files?**  
   Puede usar Azure Files para crear recursos compartidos de archivos en la nube, sin tener que administrar la sobrecarga de un servidor físico o dispositivo. El trabajo más monótono se realiza automáticamente, como aplicar las actualizaciones del sistema operativo y reemplazar los discos incorrectos. Para obtener más información sobre los escenarios en los que Azure Files puede ayudarle, vea [¿Por qué es útil Azure Files?](storage-files-introduction.md#why-azure-files-is-useful)

* <a id="file-access-options"></a>
   **¿Cuáles son las diferentes formas de obtener acceso a los archivos de Azure Files?**  
    Puede montar el recurso compartido de archivos en la máquina local mediante el protocolo SMB 3.0, o bien puede usar herramientas como el [Explorador de Storage](https://storageexplorer.com/) para obtener acceso a los archivos en el recurso compartido de archivos. Desde la aplicación, puede usar las bibliotecas de cliente de almacenamiento, las API de REST, PowerShell o la CLI de Azure para obtener acceso a los archivos en el recurso compartido de archivos de Azure.

* <a id="what-is-afs"></a>
   **¿Qué es Azure File Sync?**  
    Puede usar Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma las máquinas de Windows Server en una caché rápida de los recursos compartidos de archivos de Azure. Puede usar cualquier protocolo disponible en Windows Server para tener acceso a sus datos localmente, como SMB, Network File System (NFS) y el servicio de protocolo de transferencia de archivos (FTPS). Puede tener todas las cachés que necesite en todo el mundo.

* <a id="files-versus-blobs"></a>
   **¿Por qué debo usar para mis datos un recurso compartido de archivos de Azure en vez de Azure Blob Storage?**  
    Tanto Azure Files como Azure Blob Storage ofrecen una manera de almacenar grandes cantidades de datos en la nube, pero son útiles para fines ligeramente diferentes. 
    
    Azure Blob Storage es útil para aplicaciones de gran escala y nativas de la nube que deben almacenar datos no estructurados. Para maximizar el rendimiento y la escala, Azure Blob Storage es una abstracción de almacenamiento más sencilla que un sistema de archivos verdadero. Solo puede tener acceso a Azure Blob Storage a través de bibliotecas de cliente basadas en REST (o directamente mediante el protocolo basado en REST).

    Azure Files es específicamente un sistema de archivos. Azure Files tiene todos los resúmenes de archivo con los que está familiarizado después de trabajar durante años con sistemas operativos locales. Al igual que Azure Blob Storage, Azure Files ofrece una interfaz de REST y bibliotecas de cliente basadas en REST. A diferencia de Azure Blob Storage, Azure Files ofrece a SMB acceso a recursos compartidos de archivos de Azure. Mediante el uso de SMB, puede montar directamente un recurso compartido de archivos de Azure en Windows, Linux o macOS, ya sea en máquinas virtuales locales como en la nube, sin tener que escribir ningún código ni adjuntar controladores especiales al sistema de archivos. También puede almacenar en caché recursos compartidos de archivos de Azure en servidores de archivos locales mediante el uso de Azure File Sync para tener un acceso rápido cerca de donde se usan los datos. 
   
    Para obtener una explicación más detallada sobre las diferencias entre Azure Files y Azure Blob Storage, vea [Decisión sobre cuándo usar Azure Blobs, Azure Files o Azure Disks](../common/storage-decide-blobs-files-disks.md). Para saber más de Azure Blob Storage, consulte [Introducción a Blob Storage](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a> **¿Por qué debo usar un recurso compartido de archivos de Azure en vez de Azure Disks?**  
    Un disco en Azure Disks no es más que un disco. Para sacar provecho de Azure Disks, tendrá que conectarlo a una máquina virtual que se ejecute en Azure. Azure Disks puede usarse para todo aquello para lo que quiera usar un disco en un servidor local. Puede usarlo como disco del sistema operativo, como espacio de intercambio para un sistema operativo, o como almacenamiento dedicado para una aplicación. Un uso interesante de Azure Disks consiste en crear un servidor de archivos en la nube para usarlo en los mismos lugares en los que usaría un recurso compartido de archivos de Azure. Implementar un servidor de archivos en las máquinas virtuales de Azure es una manera de alto rendimiento de obtener almacenamiento de archivos en Azure cuando necesite opciones de implementación no compatibles actualmente con Azure Files (por ejemplo, la compatibilidad con el protocolo NFS o almacenamiento premium). 

    Pero la ejecución de un servidor de archivos con Azure Disks como almacenamiento back-end suele ser mucho más costosa que el uso de un recurso compartido de archivos de Azure por diversos motivos. En primer lugar, además de pagar por el almacenamiento en disco, también debe pagar por el gasto de ejecutar una o más máquinas virtuales de Azure. En segundo lugar, también debe administrar las máquinas virtuales que se usan para ejecutar el servidor de archivos. Por ejemplo, usted es el responsable de las actualizaciones del sistema operativo. Por último, si finalmente se necesita que los datos se almacenen en la caché local, el usuario tiene la última palabra para configurar y administrar las tecnologías de replicación (como la Replicación del sistema de archivos distribuido, o DFSR) para que esto ocurra.

    Un enfoque para obtener lo mejor tanto de Azure Files como de un servidor de archivos hospedado en máquinas virtuales de Azure (además de usar Azure Disks como almacenamiento de back-end) consiste en instalar Azure File Sync en un servidor de archivos hospedado en máquina virtual en la nube. Si el recurso compartido de archivos de Azure se encuentra en la misma región que el servidor de archivos, puede habilitar los niveles en la nube y establecer un porcentaje de espacio libre en el volumen al máximo (99 %). Esto garantiza la duplicación mínima de datos. También puede usar todas las aplicaciones que quiera con los servidores de archivos, como aplicaciones que requieren compatibilidad con el protocolo NFS.

    Para obtener más información sobre una opción para configurar un elevado rendimiento y un servidor de archivos que tenga una alta disponibilidad en Azure, vea [Deploying IaaS VM guest clusters in Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/) (Implementación de clústeres invitados de máquinas virtuales de IaaS en Microsoft Azure). Para obtener una descripción más detallada sobre las diferencias entre Azure Files y Azure Disks, vea [Decisión sobre cuándo usar Azure Blobs, Azure Files o Azure Disks](../common/storage-decide-blobs-files-disks.md). Para obtener más información sobre Azure Disks, vea [Introducción a Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>
   **¿Cómo puedo empezar a usar Azure Files?**  
   Empezar a trabajar con Azure Files es fácil. En primer lugar, [cree un recurso compartido de archivos](storage-how-to-create-file-share.md) y, después, móntelo en su sistema operativo preferido: 

  * [Montaje en Windows](storage-how-to-use-files-windows.md)
  * [Montaje en Linux](storage-how-to-use-files-linux.md)
  * [Montaje en macOS](storage-how-to-use-files-mac.md)

    Para obtener información más detallada sobre cómo implementar un recurso compartido de archivos de Azure para reemplazar los recursos compartidos de archivos de producción en su organización, vea [Planeamiento de una implementación de Azure Files](storage-files-planning.md).

* <a id="redundancy-options"></a>
   **¿Qué opciones de redundancia de almacenamiento son compatibles con Azure Files?**  
    Actualmente, Azure Files admite el almacenamiento con redundancia local (LRS), el almacenamiento con redundancia de zona (ZRS), el almacenamiento con redundancia geográfica (GRS) y el almacenamiento con redundancia de zona geográfica (GZRS). En un futuro, se admitirá el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS), pero aún no se sabe cuándo estará disponible.

* <a id="tier-options"></a>
   **¿Qué capas de almacenamiento son compatibles con Azure Files?**  
    Azure Files admite dos niveles de almacenamiento: Premium y estándar. Los recursos compartidos de archivos estándar se crean en cuentas de almacenamiento de uso general (GPv1 o de GPv2) y los recursos compartidos de archivos Premium se crean en cuentas de almacenamiento FileStorage. Obtenga más información sobre cómo crear [recursos compartidos de archivos estándar](storage-how-to-create-file-share.md) y [recursos compartidos de archivos Premium](storage-how-to-create-premium-fileshare.md). 
    
    > [!NOTE]
    > No se pueden crear recursos compartidos de archivos de Azure desde cuentas de almacenamiento de blobs ni desde cuentas de almacenamiento *Premium* de uso general (GPv1 o GPv2). Los recursos compartidos de archivos de Azure estándar deben crearse solo en cuentas de uso general *estándar* y los recursos compartido de archivos de Azure Premium solo deben crearse en cuentas de almacenamiento FileStorage. Las cuentas de almacenamiento *Premium* de uso general (GPv1 y GPv2) son para blobs en páginas Premium solamente. 

* <a id="give-us-feedback"></a>
  **Me gustaría que se agregara una característica específica a Azure Files. ¿Pueden agregarla?**  
    El equipo de Azure Files está interesado en conocer todos los comentarios que tenga sobre nuestro servicio. Vote en las solicitudes de características en [UserVoice de Azure Files](https://feedback.azure.com/forums/217298-storage/category/180670-files). Estamos deseando entusiasmarle con muchas características nuevas.

## <a name="azure-file-sync"></a>Azure File Sync

* <a id="afs-region-availability"></a>
   **¿Qué regiones se admiten en Azure File Sync?**  
    Encontrará la lista de regiones disponibles en la sección [Disponibilidad en regiones](storage-sync-files-planning.md#region-availability) de la Guía de planeamiento de Azure File Sync. Continuamente se agregará compatibilidad con otras regiones, como regiones no públicas.

* <a id="cross-domain-sync"></a>
   **¿Puedo tener servidores unidos a un dominio y no unidos a un dominio en el mismo grupo de sincronización?**  
    Sí. Un grupo de sincronización puede contener puntos de conexión de servidor que tienen pertenencias diferentes de Active Directory, incluso aunque no estén unidos a dominio. Aunque técnicamente la configuración funciona, no se recomienda como configuración normal, ya que las listas de control de acceso (ACL) que se definen para los archivos y carpetas de un servidor no podrán aplicarse a otros servidores del grupo de sincronización. Para obtener mejores resultados, se recomienda realizar la sincronización entre servidores en el mismo bosque de Active Directory, entre servidores en bosques distintos de Active Directory con relaciones de confianza establecidas o entre servidores que no están en un dominio. Se recomienda no usar una combinación de estas configuraciones.

* <a id="afs-change-detection"></a>
  **He creado un archivo directamente en el recurso compartido de archivos de Azure mediante SMB o en el portal. ¿Cuánto tiempo tarda el archivo en sincronizarse con los servidores del grupo de sincronización?**  
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Si se cambia el mismo archivo en dos servidores aproximadamente al mismo tiempo, ¿qué sucede?**  
    Azure File Sync usa una estrategia simple de resolución de conflictos: conservamos los cambios realizados en los archivos que se modifican en dos servidores al mismo tiempo. El cambio de escritura más reciente mantiene el nombre de archivo original. El archivo anterior tiene la máquina de "origen" y el número de conflicto anexados al nombre. Sigue esta taxonomía: 
   
    \<NombreDeArchivoSinExtensión\>-\<NombreDeMáquina\>\[-#\].\<ext\>  

    Por ejemplo, el primer conflicto de CompanyReport.docx se convertiría en CompanyReport-CentralServer.docx si CentralServer es donde se ha producido la operación de escritura anterior. El segundo conflicto se denominará CompanyReport-CentralServer-1.docx. Azure File Sync admite 100 archivos de conflicto por archivo. Una vez alcanzado el número máximo de archivos de conflicto, el archivo no se sincronizará hasta que el número de archivos de conflicto sea inferior a 100.

* <a id="afs-storage-redundancy"></a>
   **¿Se admite el almacenamiento con redundancia geográfica en Azure File Sync?**  
    Sí, Azure Files admite almacenamiento con redundancia local (LRS) y almacenamiento con redundancia geográfica (GRS). Si inicia la conmutación por error de una cuenta de almacenamiento entre regiones emparejadas desde una cuenta configurada para GRS, Microsoft recomienda que trate la nueva región como una copia de seguridad de datos únicamente. Azure File Sync no inicia automáticamente la sincronización con la nueva región principal. 

* <a id="sizeondisk-versus-size"></a>
   **¿Por qué la propiedad *Tamaño en disco* de un archivo no coincide con la propiedad *Tamaño* después de usar Azure File Sync?**  
  Consulte [Información general de nube por niveles](storage-sync-cloud-tiering.md#sizeondisk-versus-size).

* <a id="is-my-file-tiered"></a>
   **¿Cómo se puede saber si un archivo se ha organizado en niveles?**  
  Consulte [Información general de nube por niveles](storage-sync-cloud-tiering.md#is-my-file-tiered).

* <a id="afs-recall-file"></a>**Uno de los archivos que quiero usar se ha organizado en niveles. ¿Cómo puedo recuperarlo en el disco para usarlo de forma local?**  
  Consulte [Información general de nube por niveles](storage-sync-cloud-tiering.md#afs-recall-file).

* <a id="afs-force-tiering"></a>
   **¿Cómo puedo forzar la organización en niveles de un archivo o directorio?**  
  Consulte [Información general de nube por niveles](storage-sync-cloud-tiering.md#afs-force-tiering).

* <a id="afs-effective-vfs"></a>
   **¿Cómo se interpreta el *espacio disponible del volumen* cuando tengo varios puntos de conexión de servidor en un volumen?**  
  Consulte [Información general de nube por niveles](storage-sync-cloud-tiering.md#afs-effective-vfs).

* <a id="afs-files-excluded"></a>
   **¿Qué archivos o carpetas excluye automáticamente Azure File Sync?**  
    De forma predeterminada, Azure File Sync excluye los archivos siguientes:
  * desktop.ini
  * thumbs.db
  * ehthumbs.db
  * ~$\*.\*
  * \*.laccdb
  * \*.tmp
  * 635D02A9D91C401B97884B82B3BCDAEA.\*

    Las siguientes carpetas también se excluyen de forma predeterminada:

  * \System Volume Information
  * \$RECYCLE.BIN
  * \SyncShareState

* <a id="afs-os-support"></a>
   **¿Puedo usar Azure File Sync con Windows Server 2008 R2, Linux o un dispositivo de almacenamiento conectado a la red (NAS)?**  
    En la actualidad, Azure File Sync solo admite Windows Server 2019 y Windows Server 2016 y Windows Server 2012 R2. En este momento, no tenemos otros planes para compartir, pero estamos dispuestos a admitir plataformas adicionales según la demanda de los clientes. Indíquenos a través de [UserVoice de Azure Files](https://feedback.azure.com/forums/217298-storage/category/180670-files) qué plataformas le gustaría que fueran compatibles.

* <a id="afs-tiered-files-out-of-endpoint"></a>
   **¿Por qué los archivos en capas se encuentran fuera del espacio de nombres del punto de conexión de servidor?**  
    Antes de la versión 3 del agente de Azure File Sync, Azure File Sync bloqueaba el movimiento de archivos en capas fuera del punto de conexión del servidor, pero en el mismo volumen que el punto de conexión del servidor. Las operaciones de copia, los movimientos de archivos sin capas y los movimientos de capas a otros volúmenes no resultaban afectados. La razón de este comportamiento era la presunción implícita que tienen el Explorador de archivos y otras API de Windows de que las operaciones de movimiento en el mismo volumen son operaciones de cambio de nombre (casi) instantáneas. Esto significa que los movimientos que haga el Explorador de archivos u otros métodos de movimiento (como la línea de comandos o PowerShell) parecen no responder mientras Azure File Sync recupera los datos de la nube. A partir de la [versión 3.0.12.0 del agente de Azure File Sync](storage-files-release-notes.md#supported-versions), Azure File Sync le permitirá mover un archivo con capas fuera del punto de conexión del servidor. Evitamos los efectos negativos que se mencionaron anteriormente permitiendo que el archivo con capas exista como un archivo con capas fuera del punto de conexión del servidor y, a continuación, recuperando el archivo en segundo plano. Esto significa que los movimientos en el mismo volumen son instantáneos y nosotros nos ocupamos por completo de recuperar el archivo en el disco una vez que el movimiento se ha completado. 

* <a id="afs-do-not-delete-server-endpoint"></a>
  **Tengo un problema con Azure File Sync en mi servidor (sincronización, nube por niveles, etc.). ¿Debería quitar y volver a crear el punto de conexión del servidor?**  
    [!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]
    
* <a id="afs-resource-move"></a>
   **¿Puedo mover el servicio de sincronización del almacenamiento o la cuenta de almacenamiento a un grupo de recursos o suscripción diferentes?**  
   El servicio de sincronización del almacenamiento o la cuenta de almacenamiento se pueden mover a otro grupo de recursos o suscripción del inquilino de Azure AD existente. Si se mueve la cuenta de almacenamiento, debe dar acceso al servicio File Sync híbrido a la cuenta de almacenamiento (consulte el apartado [Asegúrese de que Azure File Sync tiene acceso a la cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync no permite mover la suscripción a un inquilino de Azure AD distinto.
    
* <a id="afs-ntfs-acls"></a>
   **¿Mantiene Azure File Sync las listas ACL de NTFS a nivel de directorio/archivo junto con los datos almacenados en Azure Files?**

    Las listas ACL de NTFS que provienen de los servidores de archivos locales se conservan en Azure File Sync como metadatos. Azure Files no admite la autenticación con credenciales de Azure AD para acceder a recursos compartidos de archivos que administra el servicio Azure File Sync.
    
## <a name="security-authentication-and-access-control"></a>Seguridad, autenticación y control de acceso
* <a id="ad-support"></a>
 **¿Admite Azure Files la autenticación y el control de acceso basados en identidades?**  
    
    Sí, Azure Files admite la autenticación basada en identidades y el control de acceso mediante el uso de Azure AD Domain Services (Azure AD DS). La autenticación de Azure AD DS a través de SMB para Azure Files permite que las máquinas virtuales Windows unidas a un dominio de Azure AD DS tengan acceso a recursos compartidos, directorios y archivos mediante las credenciales de Azure AD. Para obtener más información, consulte la [Introducción a la compatibilidad de la autenticación de Azure Active Directory Domain Services (Azure AD DS) de Azure Files con el acceso SMB](storage-files-active-directory-overview.md). 

    Azure Files ofrece dos formas adicionales de administrar el control de acceso:

    - Puede usar firmas de acceso compartido (SAS) para generar tokens que tengan permisos específicos y que sean válidos para un intervalo de tiempo específico. Por ejemplo, se puede generar un token con acceso de solo lectura a un archivo específico que expire al cabo de 10 minutos. Todos los usuarios que posean dicho token, mientras tenga validez, tendrán acceso de solo lectura al archivo durante esos 10 minutos. Actualmente, solo se admiten claves de firma de acceso compartido a través de la API de REST o en bibliotecas de cliente. Debe montar el recurso compartido de archivos de Azure a través de SMB mediante el uso de las claves de cuenta de almacenamiento.

    - Azure File Sync conserva y replica todas las ACL discrecionales, o DACL, locales o basadas en Active Directory en todos los puntos de conexión de servidor con los que se sincroniza. Dado que Windows Server ya se autentica con Active Directory, Azure File Sync es una opción provisional eficaz hasta que llegue la compatibilidad total con autenticación basada en Active Directory y ACL.
    
    Puede hacer referencia al artículo [Autorización de acceso a Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para obtener una representación completa de todos los protocolos admitidos en los servicios de Azure Storage. 

* <a id="ad-support-devices"></a>
 **¿La autenticación de Azure AD DS para Azure Files admite el acceso SMB mediante las credenciales de Azure AD desde dispositivos unidos o registrados a Azure AD?**

    No, este escenario no se admite.

* <a id="ad-support-rest-apis"></a>
 **¿Hay API REST que admitan las operaciones para obtener, establecer o copiar las listas ACL de NTFS en directorios o archivos?**

    Por ahora, no se admiten las API REST para obtener, establecer o copiar ACL de NTFS para directorios o archivos.

* <a id="ad-vm-subscription"></a>
 **¿Puedo obtener acceso a Azure Files con las credenciales de Azure AD desde una máquina virtual que esté en una suscripción diferente?**

    Si la suscripción con la que se implementa el recurso compartido de archivos está asociada al mismo inquilino de Azure AD como, por ejemplo, la función de implementación de Azure AD Domain Services a la que está unida la máquina virtual mediante el dominio, puede obtener acceso a Azure Files con las mismas credenciales de Azure AD. La limitación no se impone en la suscripción, sino en el inquilino de Azure AD asociado.    
    
* <a id="ad-support-subscription"></a>
 **¿Puedo habilitar la autenticación de Azure AD DS para Azure Files con un inquilino de Azure AD distinto del inquilino principal con el que está asociado el recurso compartido de archivos?**

    No, Azure Files solo es compatible con la integración de Azure AD DS con un inquilino de Azure AD que reside en la misma suscripción que el recurso compartido de archivos. Cada suscripción está asociada a un inquilino de Azure AD.

* <a id="ad-linux-vms"></a>
 **¿La autenticación de Azure AD DS para Azure Files es compatible con máquinas virtuales Linux?**

    No, no se admite la autenticación desde máquinas virtuales Linux.

* <a id="ad-aad-smb-afs"></a>
 **¿Puedo aprovechar la autenticación de Azure AD DS para Azure Files en los recursos compartidos de archivos que administra Azure File Sync?**

    No, Azure Files no admite la preservación de las listas ACL de NTFS en recursos compartidos de archivos que administre Azure File Sync. Las listas ACL de archivos que provienen de los servidores de archivos locales se conservan en Azure File Sync. El servicio Azure File Sync se encargará de sobrescribir cualquier lista ACL de NTFS configurada de forma nativa en Azure Files. Además, Azure Files no admite la autenticación con credenciales de Azure AD para obtener acceso a los recursos compartidos de archivos que administra el servicio Azure File Sync.

* <a id="encryption-at-rest"></a>
 **¿Cómo se puede garantizar que el recurso compartido de archivos de Azure está cifrado en reposo?**  

    Sí. Para más información, vea [Azure Storage Service Encryption](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>
 **¿Cómo se puede proporcionar acceso a un archivo específico a través de un explorador web?**  

    Puede usar firmas de acceso compartido para generar tokens que tengan permisos específicos y que sean válidos para un intervalo de tiempo específico. Por ejemplo, puede generar un token que proporcione acceso de solo lectura a un archivo específico durante un período de tiempo determinado. Cualquier persona que posea la dirección URL puede obtener acceso al archivo directamente desde cualquier explorador web mientras el token sea válido. Puede generar fácilmente una clave de firma de acceso compartido desde una interfaz de usuario, como el Explorador de Storage.

* <a id="file-level-permissions"></a>
 **¿Es posible especificar permisos de solo lectura o solo escritura en las carpetas del recurso compartido?**  

    Si monta el recurso compartido de archivos a través de SMB, no tendrá control de nivel de carpeta sobre los permisos. Pero si crea una firma de acceso compartido mediante la API de REST o bibliotecas de cliente, puede especificar permisos de solo lectura o de solo escritura en las carpetas del recurso compartido.

* <a id="ip-restrictions"></a>
 **¿Puedo implementar restricciones de IP en un recurso compartido de archivos de Azure?**  

    Sí. Es posible limitar el acceso al recurso compartido de archivos de Azure en el nivel de cuenta de almacenamiento. Para más información, vea [Configuración de Firewalls y redes virtuales de Azure Storage](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>
 **¿Qué directivas de cumplimiento de datos admite Azure Files?**  

   Azure Files se ejecuta sobre la misma arquitectura de almacenamiento que se usa en otros servicios de almacenamiento de Azure Storage. Azure Files aplica las mismas directivas de cumplimiento de datos que se usan en otros servicios de Azure Storage. Para obtener más información sobre el cumplimiento de datos de Azure Storage, puede consultar [Ofertas de cumplimiento de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-compliance-offerings) e ir al [Centro de confianza de Microsoft](https://microsoft.com/trustcenter/default.aspx).

## <a name="on-premises-access"></a>Acceso local

* <a id="port-445-blocked"></a>
**Mi ISP o TI bloquean el puerto 445 e impiden que se monte Azure Files, ¿qué debo hacer?**

    Puede obtener información sobre [diversos métodos para solucionar el bloqueo del puerto 445 aquí](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems#cause-1-port-445-is-blocked). Azure Files solo permite conexiones con SMB 3.0 (con compatibilidad con el cifrado) desde fuera de la región o el centro de datos. El protocolo SMB 3.0 presenta muchas características de seguridad, incluido el cifrado de canal, que es muy seguro para usar a través de Internet. No obstante, es posible que el puerto 445 se haya bloqueado por motivos históricos de vulnerabilidades que se encuentran en versiones inferiores de SMB. En el caso ideal, el puerto debería bloquearse solo para el tráfico de SMB 1.0, y debería desactivarse SMB 1.0 en todos los clientes.

* <a id="expressroute-not-required"></a>
 **¿Tengo que usar Azure ExpressRoute para conectarme a Azure Files o debo usar Azure File Sync en un entorno local?**  

    No. No es necesario ExpressRoute para obtener acceso a un recurso compartido de archivos de Azure. Si está montando un recurso compartido de archivos de Azure directamente en un entorno local, lo único que se necesita es tener abierto el puerto 445 (salida TCP) para tener acceso a Internet (este es el puerto que SMB usa para comunicarse). Si usa Azure File Sync, lo único que se necesita es el puerto 443 (salida TCP) para el acceso HTTPS (no se necesita SMB). Pero *puede usar* ExpressRoute con cualquiera de estas opciones de acceso.

* <a id="mount-locally"></a>
 **¿Cómo puedo montar un recurso compartido de archivos de Azure en mi máquina local?**  

    Puede montar el recurso compartido de archivos a través del protocolo SMB, siempre y cuando el puerto 445 (salida TCP) esté abierto y el cliente admita el protocolo SMB 3.0 (por ejemplo, si usa Windows 10 o Windows Server 2016). Si el puerto 445 está bloqueado por una directiva de su organización o por su ISP, puede usar Azure File Sync para obtener acceso al recurso compartido de archivos de Azure.

## <a name="backup"></a>Copia de seguridad
* <a id="backup-share"></a>
 **¿Cómo puedo realizar una copia de seguridad de mi recurso compartido de archivos de Azure?**  
    Puede usar [instantáneas periódicas de recursos compartidos](storage-snapshots-files.md) para evitar cualquier eliminación accidental. También puede usar AzCopy, Robocopy o una herramienta de copia de seguridad de terceros que pueda hacer una copia de seguridad de un recurso compartido de archivos montado. Azure Backup ofrece la copia de seguridad de Azure Files. Obtenga más información sobre la [copia de seguridad de recursos compartidos de archivos de Azure mediante Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-files).

## <a name="share-snapshots"></a>Instantáneas de recursos compartido

### <a name="share-snapshots-general"></a>Instantáneas de recursos compartido: General
* <a id="what-are-snaphots"></a>
 **¿Qué son las instantáneas de recursos compartidos de archivos?**  
    Puede usar las instantáneas de recurso compartido de archivos de Azure para crear versiones de solo lectura de los recursos compartidos de archivos. También puede usar Azure Files para copiar una versión anterior del contenido en el mismo recurso compartido, en una ubicación alternativa en Azure o de forma local para modificaciones posteriores. Para obtener más información sobre las instantáneas de recurso compartido, vea [Información general de las instantáneas de recurso compartido](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>
 **¿Dónde se almacenan mis instantáneas de recurso compartido?**  
    Las instantáneas de recurso compartido se almacenan en la misma cuenta de almacenamiento que el recurso compartido de archivos.

* <a id="snapshot-consistency"></a>
 **¿Son coherentes con la aplicación las instantáneas de recurso compartido?**  
    No, las instantáneas de recurso compartido no son coherentes con la aplicación. El usuario debe vaciar las escrituras de la aplicación en el recurso compartido antes de realizar la instantánea de recurso compartido.

* <a id="snapshot-limits"></a>
 **¿Hay límites en el número de instantáneas de recurso compartido que se pueden usar?**  
    Sí. Azure Files puede retener un máximo de 200 instantáneas de recurso compartido. Las instantáneas de recurso compartido no cuentan en la cuota del recurso compartido, así que no hay ningún límite de recurso compartido en el espacio total usado por todas las instantáneas de recurso compartido. Los límites de cuenta de almacenamiento se siguen aplicando. Una vez que llegue a las 200 instantáneas de recurso compartido, debe eliminar las instantáneas más antiguas para poder crear otras.

* <a id="snapshot-cost"></a>
 **¿Cuánto cuestan las instantáneas de recurso compartido?**  
    El costo de transacciones estándar y de almacenamiento estándar se aplicará a la instantánea. Las instantáneas tienen una naturaleza incremental. La instantánea de base es el recurso compartido mismo. Todas las instantáneas siguientes son incrementales y solo almacenarán la diferencia de la instantánea anterior. Esto significa que los cambios diferenciales que se verán en la factura será mínimos si la renovación de la carga de trabajo es mínima. Vea la [página de precios](https://azure.microsoft.com/pricing/details/storage/files/) para obtener información sobre precios estándar de Azure Files. En la actualidad, la manera de ver el tamaño consumido por instantánea de recurso compartido es comparando la capacidad facturada con la capacidad usada. Estamos trabajando en herramientas para mejorar los informes.

* <a id="ntfs-acls-snaphsots"></a>
 **¿Las listas ACL de NTFS que están en los directorios y archivos se conservan en instantáneas del recurso compartido?**  
    Las listas ACL de NTFS que están en los directorios y archivos se conservan en instantáneas del recurso compartido.

### <a name="create-share-snapshots"></a>Creación de instantáneas de recurso compartido
* <a id="file-snaphsots"></a>
 **¿Puedo crear instantáneas de recurso compartido de archivos individuales?**  
    Las instantáneas de recurso compartido se crean en el nivel de recurso compartido de archivos. Puede restaurar archivos individuales desde la instantánea de recurso compartido de archivos, pero no puede crear instantáneas de recurso compartido de nivel de archivo. Aun así, si ha realizado una instantánea de recurso compartido de nivel de recurso compartido y quiere enumerar las instantáneas de recurso compartido en las que ha cambiado un archivo determinado, puede hacerlo en **Versiones anteriores** en un recurso compartido montado en Windows. 
    
    Si necesita una característica de instantánea de archivos, indíquenoslo a través de [UserVoice de Azure Files](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encrypted-snapshots"></a>
 **¿Puedo crear instantáneas de recurso compartido a partir de un recurso compartido de archivos cifrados?**  
    Puede realizar una instantánea de recurso compartido de los recursos compartidos de archivos de Azure que tengan el cifrado en reposo habilitado. Puede restaurar archivos de una instantánea de recurso compartido a un recurso compartido de archivos cifrados. Si el recurso compartido está cifrado, la instantánea de recurso compartido también estará cifrada.

* <a id="geo-redundant-snaphsots"></a>
 **¿Tienen mis instantáneas de recurso compartido redundancia geográfica?**  
    Las instantáneas de recurso compartido tienen la misma redundancia que el recurso compartido de archivos de Azure para el que se han tomado. Si ha seleccionado el almacenamiento con redundancia geográfica para la cuenta, la instantánea de recurso compartido también se almacena de manera redundante en la región emparejada.

### <a name="manage-share-snapshots"></a>Administración de instantáneas de recurso compartido
* <a id="browse-snapshots-linux"></a>
 **¿Puedo examinar mis instantáneas de recurso compartido con Linux?**  
    Puede usar la CLI de Azure para crear, enumerar, examinar y restaurar instantáneas de recurso compartido en Linux.

* <a id="copy-snapshots-to-other-storage-account"></a>
 **¿Puedo copiar las instantáneas de recurso compartido a una cuenta de almacenamiento diferente?**  
    Puede copiar archivos desde instantáneas de recurso compartido en otra ubicación, pero no puede copiar las instantáneas en sí mismas.

### <a name="restore-data-from-share-snapshots"></a>Restauración de datos desde instantáneas de recurso compartido
* <a id="promote-share-snapshot"></a>
 **¿Puedo promover una instantánea de recurso compartido al recurso compartido de base?**  
    Puede copiar datos desde una instantánea de recurso compartido en otro destino, pero no puede promover una instantánea de recurso compartido al recurso compartido de base.

* <a id="restore-snapshotted-file-to-other-share"></a>
 **¿Puedo restaurar los datos desde mi instantánea de recurso compartido a otra cuenta de almacenamiento?**  
    Sí. Los archivos de una instantánea de recurso compartido se pueden copiar en la ubicación original o en una ubicación alternativa que incluya la misma cuenta de almacenamiento o una cuenta de almacenamiento diferente, ya sea en la misma región o en regiones diferentes. También puede copiar archivos en una ubicación local o en cualquier otra nube.    
  
### <a name="clean-up-share-snapshots"></a>Limpiar instantáneas de recurso compartido
* <a id="delete-share-keep-snapshots"></a>
 **¿Puedo eliminar mi recurso compartido pero no las instantáneas de recurso compartido?**  
    Si tiene instantáneas de recurso compartido activas en el recurso, no puede eliminar el recurso compartido. Puede usar una API para eliminar instantáneas de recurso compartido junto con el recurso compartido. También puede eliminar las instantáneas de recurso compartido y el recurso compartido en Azure Portal.

* <a id="delete-share-with-snapshots"></a>
 **¿Qué les ocurrirá a mis instantáneas de recurso compartido si elimino la cuenta de almacenamiento?**  
    Si elimina la cuenta de almacenamiento, también se eliminarán las instantáneas de recurso compartido.

## <a name="billing-and-pricing"></a>Precios y facturación
* <a id="vm-file-share-network-traffic"></a>
 **¿El tráfico de red entre una máquina virtual de Azure y un recurso compartido de archivos de Azure cuenta como ancho de banda externo que se carga a la suscripción?**  
    Si el recurso compartido de archivos y la máquina virtual están en la misma región de Azure, no hay ningún cargo adicional por el tráfico entre el recurso compartido de archivos y la máquina virtual. Si el recurso compartido de archivos y la máquina virtual se encuentran en distintas regiones, el tráfico entre ellos se cargará como ancho de banda externo.

* <a id="share-snapshot-price"></a>
 **¿Cuánto cuestan las instantáneas de recurso compartido?**  
     Durante la versión preliminar, no hay ningún cargo por la característica de instantánea de recurso compartido. Se seguirá aplicando la salida de almacenamiento estándar y los costos de transacción. Después de la disponibilidad general, se cobrará a las suscripciones la capacidad y las transacciones en las instantáneas de recurso compartido.
     
     Las instantáneas de recurso compartido son de naturaleza incremental. La instantánea de recurso compartido de base es el mismo recurso compartido. Todas las instantáneas de recurso compartido siguientes son incrementales y solo almacenan la diferencia de la instantánea de recurso compartido anterior. Se le facturará únicamente por el contenido cambiado. Si tiene un recurso compartido con 100 GB de datos, pero solo se han cambiado 5 GB desde la última instantánea de recurso compartido, esa instantánea de recurso compartido consumirá solo 5 GB adicionales y se le facturarán por tanto 105 GB. Para obtener más información sobre los cargos de salida estándar y de transacciones, vea la [página de precios](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="scale-and-performance"></a>Escala y rendimiento
* <a id="files-scale-limits"></a>
 **¿Cuáles son los límites de escala de Azure Files?**  
    Para obtener información sobre los objetivos de escalabilidad y rendimiento de Azure Files, vea [Objetivos de escalabilidad y rendimiento de Azure Files](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>
 **¿Qué tamaños están disponibles para los recursos compartidos de archivos de Azure?**  
    Los tamaños de recursos compartidos de archivos de Azure (Premium y estándar) pueden escalar hasta 100 TiB. Existen tamaños de recursos compartidos de archivos Premium de hasta 100 TiB disponibles como oferta de GA. Los tamaños de recursos compartidos de archivos estándar de hasta 5 TiB están disponibles como un oferta de GA, aunque los tamaños de hasta 100 TiB están en versión preliminar. Consulte la sección [Incorporación de recursos compartidos de archivos de mayor tamaño (nivel estándar)](storage-files-planning.md#onboard-to-larger-file-shares-standard-tier) de la Guía de planeación para obtener instrucciones de incorporación para la versión preliminar de recursos compartidos de archivos más grandes para el nivel estándar.

* <a id="open-handles-quota"></a>
 **¿Cuántos clientes pueden obtener acceso al mismo archivo simultáneamente?**    
    Hay una cuota de 2000 identificadores abiertos en un único archivo. Si tiene 2000 identificadores abiertos, se muestra un mensaje de error que indica que se ha alcanzado la cuota.

* <a id="zip-slow-performance"></a>
**El rendimiento es lento cuando descomprimo archivos en Azure Files. ¿qué debo hacer?**  
    Para transferir una gran cantidad de archivos a Azure Files, es aconsejable usar AzCopy (para Windows; en versión preliminar para Linux y UNIX) o Azure PowerShell. Estas herramientas se han optimizado para la transferencia a través de red.

* <a id="slow-perf-windows-81-2012r2"></a>
**He montado el recurso compartido de archivos de Azure en Windows Server 2012 R2 o Windows 8.1 y el rendimiento es lento. ¿Por qué?**  
    Hay un problema conocido al montar un recurso compartido de archivos de Azure en Windows Server 2012 R2 y Windows 8.1. El problema se ha revisado en la actualización acumulativa de abril de 2014 para Windows 8.1 y Windows Server 2012 R2. Para obtener un rendimiento óptimo, asegúrese de que todas las instancias de Windows Server 2012 R2 y Windows 8.1 tienen aplicada esta revisión. (Siempre debería recibir revisiones de Windows a través de Windows Update). Para obtener más información, vea el artículo asociado de Microsoft Knowledge Base, [Rendimiento lento en el acceso a Azure Files desde Windows 8.1 o Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Características e interoperabilidad con otros servicios
* <a id="cluster-witness"></a>
 **¿Puedo usar mi recurso compartido de archivos de Azure como un *testigo del recurso compartido de archivos* para el clúster de conmutación por error de Windows Server?**  
    Actualmente, esta configuración no se admite para un recurso compartido de archivos de Azure. Para obtener más información sobre cómo configurar esta opción para Azure Blob Storage, consulte [Implementar un testigo en la nube para un clúster de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>
 **¿Puedo montar un recurso compartido de archivos de Azure en una instancia de Azure Container Instances?**  
    Sí, los recursos compartidos de archivos de Azure son una buena opción si quiere conservar información sin tener que depender de la duración de una instancia del contenedor. Para obtener más información, vea [Montaje de un recurso compartido de archivos de Azure en Azure Container Instances](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>
 **¿Hay alguna operación de cambio de nombre en la API de REST?**  
    De momento, no.

* <a id="nested-shares"></a>
 **¿Puedo configurar recursos compartidos anidados, es decir, un recurso compartido en otro recurso compartido?**  
    No. El recurso compartido de archivos *es* el controlador virtual que se puede montar, por lo que no se admiten recursos compartidos anidados.

* <a id="ibm-mq"></a>
 **¿Cómo se usa Azure Files con IBM MQ?**  
    IBM ha publicado un documento que ayuda a los clientes de IBM MQ a la hora de configurar Azure Files con el servicio IBM. Para obtener más información, vea [How to set up an IBM MQ multi-instance queue manager with Microsoft Azure Files service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service)(Configuración del administrador de colas de varias instancias de IBM MQ con el servicio Microsoft Azure Files).

## <a name="see-also"></a>Otras referencias
* [Solución de problemas de Azure Files en Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Solución de problemas de Azure Files en Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Solución de problemas de Azure File Sync](storage-sync-files-troubleshoot.md)
