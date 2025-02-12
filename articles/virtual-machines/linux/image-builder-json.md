---
title: Creación de una plantilla de Azure Image Builder, versión preliminar
description: Obtenga información sobre cómo crear una plantilla para usarla con Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 07/31/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: gwallace
ms.openlocfilehash: 240e0dadaebde6725974604b578328ede0b20652
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129058"
---
# <a name="preview-create-an-azure-image-builder-template"></a>Vista previa: Creación de una plantilla de Azure Image Builder 

Azure Image Builder utiliza un archivo .json para trasladar la información al servicio Image Builder. En este artículo analizaremos las secciones del archivo json, para que pueda compilar su propio archivo. Para ver ejemplos de archivos .json completos, consulte el [GitHub de Azure Image Builder](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).

Este es el formato de plantilla básico:

```json
 { 
    "type": "Microsoft.VirtualMachineImages/imageTemplates", 
    "apiVersion": "2019-05-01-preview", 
    "location": "<region>", 
    "tags": {
        "<name": "<value>",
        "<name>": "<value>"
             }
    "identity":{},           
    "dependsOn": [], 
    "properties": { 
        "buildTimeoutInMinutes": <minutes>, 
        "build": {}, 
        "customize": {}, 
        "distribute": {} 
      } 
 } 
```



## <a name="type-and-api-version"></a>Tipo y versión de API

`type` es el tipo de recurso, que debe ser `"Microsoft.VirtualMachineImages/imageTemplates"`. `apiVersion` cambiará con el tiempo a medida que cambie la API, pero debería ser `"2019-05-01-preview"` para la versión preliminar.

```json
    "type": "Microsoft.VirtualMachineImages/imageTemplates",
    "apiVersion": "2019-05-01-preview",
```

## <a name="location"></a>Location

La ubicación es la región donde se creará la imagen personalizada. Para la versión preliminar de Image Builder, se admiten las siguientes regiones:

- East US
- Este de EE. UU. 2
- Centro occidental de EE.UU.
- Oeste de EE. UU.
- Oeste de EE. UU. 2


```json
    "location": "<region>",
```

## <a name="tags"></a>Etiquetas

Estos son los pares clave-valor que puede especificar para la imagen que se genera.

## <a name="depends-on-optional"></a>Dependencia (opcional)

Esta sección opcional se puede usar para asegurarse de que las dependencias se completan antes de continuar. 

```json
    "dependsOn": [],
```

Para obtener más información, consulte [Definir dependencias de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies#dependson).

## <a name="identity"></a>Identidad
De forma predeterminada, Image Builder admite el uso de scripts o la copia de archivos desde varias ubicaciones, como, por ejemplo, GitHub y Azure Storage. Para utilizar estos servicios, deben estar accesibles públicamente.

También puede usar una identidad administrada de Azure asignada por el usuario, que usted habrá definido, para permitir que Image Builder acceda a Azure Storage, siempre que la identidad tenga como mínimo el permiso de "Lector de datos de Storage Blob" en la cuenta de Azure Storage. Esto significa que no es necesario que los blobs de almacenamiento sean accesibles externamente ni configurar tokens SAS de instalación.


```json
    "identity": {
    "type": "UserAssigned",
          "userAssignedIdentities": {
            "<imgBuilderId>": {}
        }
        },
```

Para ver un ejemplo completo, consulte [Usar una identidad administrada de Azure asignada por el usuario para acceder a los archivos de Azure Storage](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

Compatibilidad de Image Builder para una identidad asignada por el usuario: • Solo admite una identidad • No admite nombres de dominio personalizados

Para obtener más información, consulte [¿Qué son las identidades administradas para los recursos de Azure?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
Para obtener más información sobre la implementación de esta característica, consulte [Configurar identidades administradas para los recursos de Azure en una máquina virtual de Azure mediante la CLI de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity).

## <a name="properties-source"></a>Propiedades: origen

La sección `source` contiene información acerca de la imagen de origen que usará Image Builder.

La API requiere un "SourceType" que define el origen de la compilación de imagen. Actualmente hay tres tipos:
- ISO: utilice esta opción cuando el origen sea un ISO de RHEL.
- PlatformImage: indicado para los casos en que la imagen de origen es una imagen de Marketplace.
- ManagedImage: use esta opción cuando empiece desde una imagen administrada normal.
- SharedImageVersion: se utiliza cuando se usa como origen una versión de la imagen de una galería de imágenes compartidas.

### <a name="iso-source"></a>Origen de ISO

Para la versión preliminar, Azure Image Builder solo admite el uso de ISO de DVD binarios de Red Hat Enterprise Linux 7.x publicados. Image Builder admite:
- RHEL 7.3 
- RHEL 7.4 
- RHEL 7.5 
 
```json
"source": {
       "type": "ISO",
       "sourceURI": "<sourceURI from the download center>",
       "sha256Checksum": "<checksum associated with ISO>"
}
```

Para obtener los valores `sourceURI` y `sha256Checksum`, vaya a `https://access.redhat.com/downloads` y, a continuación, seleccione el producto **Red Hat Enterprise Linux** y una versión compatible. 

En la lista de **Instaladores e imágenes para Red Hat Enterprise Linux Server**, deberá copiar el vínculo de DVD binario de Red Hat Enterprise Linux 7.x y la suma de comprobación.

> [!NOTE]
> Los tokens de acceso de los vínculos se actualizan a intervalos frecuentes, por lo que, cada vez que desee enviar una plantilla, deberá comprobar si la dirección del vínculo de RH ha cambiado.
 
### <a name="platformimage-source"></a>Origen de PlatformImage 
Azure Image Builder admite las siguientes imágenes de Azure Marketplace:
* Ubuntu 18.04
* Ubuntu 16.04
* RHEL 7.6
* CentOS 7.6
* Windows 2016
* Windows 2019

```json
        "source": {
            "type": "PlatformImage",
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "18.04-LTS",
                "version": "18.04.201903060"
        },
```


Aquí, las propiedades son las mismas que se utilizan para crear máquinas virtuales. Mediante la CLI de Azure, ejecute lo siguiente para obtener las propiedades: 
 
```azurecli-interactive
az vm image list -l westus -f UbuntuServer -p Canonical --output table –-all 
```

> [!NOTE]
> La versión no puede ser la "más reciente", deberá usar el comando anterior para obtener un número de versión. 

### <a name="managedimage-source"></a>Origen de ManagedImage

Establece la imagen de origen como una imagen administrada existente de una máquina virtual o un disco duro virtual generalizados. La imagen administrada de origen debe ser de un sistema operativo compatible y encontrarse en la misma región que la plantilla de Azure Image Builder. 

```json
        "source": { 
            "type": "ManagedImage", 
                "imageId": "/subscriptions/<subscriptionId>/resourceGroups/{destinationResourceGroupName}/providers/Microsoft.Compute/images/<imageName>"
        }
```

`imageId` debe ser el valor de ResourceId de la imagen administrada. Use `az image list` para enumerar las imágenes disponibles.


### <a name="sharedimageversion-source"></a>Origen de SharedImageVersion
Establece la imagen de origen en una versión de la imagen existente de una galería de imágenes compartidas. La versión de la imagen debe ser de un sistema operativo compatible y la imagen debe replicarse a la misma región que la plantilla de Azure Image Builder. 

```json
        "source": { 
            "type": "SharedImageVersion", 
            "imageVersionID": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/p  roviders/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageDefinitionName/versions/<imageVersion>" 
   } 
```

El `imageVersionId` debe ser el valor de ResourceId de la versión de la imagen. Use [lista de versiones de imagen con firma de Azure](/cli/azure/sig/image-version#az-sig-image-version-list) para obtener una lista de las versiones de imagen.

## <a name="properties-buildtimeoutinminutes"></a>Propiedades: buildTimeoutInMinutes

De manera predeterminada, Image Builder se ejecutará durante 240 minutos. Después de esto, se agotará el tiempo de espera y se detendrá, así se haya completado la compilación de la imagen como si no. Si se alcanza el tiempo de espera, verá un error similar al siguiente:

```text
[ERROR] Failed while waiting for packerizer: Timeout waiting for microservice to
[ERROR] complete: 'context deadline exceeded'
```

Si no se especifica un valor de buildTimeoutInMinutes o se establece en 0, se usará el valor predeterminado. Puede aumentar o disminuir el valor, hasta el máximo de 960 min (16 horas). En Windows, no se recomienda establecer este valor por debajo de 60 minutos. Si alcanza el tiempo de espera, revise los [registros](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-image-build-logs) para ver si el paso de personalización está esperando algo, como la entrada del usuario. 

Si necesita más tiempo para que se completen las personalizaciones, establezca el valor en lo que crea que necesita, con una pequeña sobrecarga. Pero no lo establezca demasiado alto, ya que es posible que tenga que esperar a que se agote el tiempo de espera antes de ver un error. 


## <a name="properties-customize"></a>Propiedades: personalización

Image Builder admite varios "personalizadores". Los personalizadores son funciones que se utilizan para personalizar la imagen, como, por ejemplo, ejecutar scripts o reiniciar los servidores. 

Al usar `customize`: 
- Puede usar varios personalizadores, pero deben tener un único `name`.
- Los personalizadores se ejecutan en el orden especificado en la plantilla.
- Si se produce un error en un personalizador, todo el componente de personalización producirá un error e informará de un error.
- Es muy recomendable que pruebe exhaustivamente el script antes de usarlo en una plantilla. Será más fácil depurar el script en su propia máquina virtual.
- No incluya información confidencial en los scripts. 
- Las ubicaciones de los scripts deben estar accesibles públicamente, a menos que esté usando [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

```json
        "customize": [
            {
                "type": "Shell",
                "name": "<name>",
                "scriptUri": "<path to script>"
            },
            {
                "type": "Shell",
                "name": "<name>",
                "inline": [
                    "<command to run inline>",
                ]
            }

        ],
```     

 
La sección de personalización es una matriz. Azure Image Builder recorrerá los personalizadores en orden secuencial. Cualquier error en un personalizador producirá un error en el proceso de compilación. 
 
 
### <a name="shell-customizer"></a>Personalizador de shell

El personalizador de shell admite la ejecución de scripts de shell, que deben estar accesibles públicamente para que IB pueda acceder a ellos.

```json
    "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "scriptUri": "<link to script>"        
        }, 
    ], 
        "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "inline": "<commands to run>"
        }, 
    ], 
```

SO compatible: Linux 
 
Propiedades de personalización:

- **tipo**: Shell 
- **nombre**: nombre para realizar el seguimiento de la personalización 
- **scriptUri**: URI a la ubicación del archivo 
- **inline**: matriz de comandos de shell, separados por comas.
 
> [!NOTE]
> Al ejecutar el personalizador de shell con el origen ISO de RHEL, deberá asegurarse de que el primer shell de personalización admite el registro con un servidor de derechos de Red Hat antes de que se produzca cualquier personalización. Una vez completada la personalización, deberá anular el registro del script con el servidor de derechos.

### <a name="windows-restart-customizer"></a>Personalizador de reinicio de Windows 
El personalizador de reinicio le permite reiniciar una máquina virtual de Windows y esperar a que vuelva a conectarse, lo que le permite instalar software que requiere un reinicio.  

```json 
     "customize": [ 
            "type{ ": "WindowsRestart", 
            "restartCommand": "shutdown /r /f /t 0 /c", 
            "restartCheckCommand": "echo Azure-Image-Builder-Restarted-the-VM  > buildArtifacts/azureImageBuilderRestart.txt",
            "restartTimeout": "5m"
         }],
```

SO compatible:  Windows
 
Propiedades de personalización:
- **Tipo**: WindowsRestart
- **restartCommand**: comando para ejecutar el reinicio (opcional). El valor predeterminado es `'shutdown /r /f /t 0 /c \"packer restart\"'`.
- **restartCheckCommand**: comando para comprobar si el reinicio se realizó correctamente (opcional). 
- **restartTimeout**: tiempo de expiración del reinicio especificado como una cadena de magnitud y unidad. Por ejemplo, `5m` (5 minutos) o `2h` (2 horas). El valor predeterminado es: "5 m"


### <a name="powershell-customizer"></a>Personalizador de PowerShell 
El personalizador de shell admite la ejecución de scripts de PowerShell y comandos alineados; los scripts deben estar accesibles públicamente para que IB pueda acceder a ellos.

```json 
     "customize": [
        { 
             "type": "PowerShell",
             "name":   "<name>",  
             "scriptUri": "<path to script>" 
        },  
        { 
             "type": "PowerShell", 
             "name": "<name>", 
             "inline": "<PowerShell syntax to run>", 
             "valid_exit_codes": "<exit code>" 
         } 
    ], 
```

SO compatible: Windows y Linux

Propiedades de personalización:

- **type**: PowerShell.
- **scriptUri**: URI a la ubicación del archivo de script de PowerShell. 
- **inline**: comandos alineados que se ejecutarán, separados por comas.
- **valid_exit_codes**: opcional, códigos válidos que pueden devolverse desde el comando de script o alineado; esto evitará que se informe de un error del comando de script o alineado.

### <a name="file-customizer"></a>Personalizador de archivos

El personalizador de archivos permite que el generador de imágenes descargue un archivo desde un almacenamiento de GitHub o Azure Storage. Si tiene una canalización de compilación de imagen basada en artefactos de compilación, puede configurar el personalizador de archivos para que se descargue desde el recurso compartido de la compilación y mover los artefactos a la imagen.  

```json
     "customize": [ 
         { 
            "type": "File", 
             "name": "<name>", 
             "sourceUri": "<source location>",
             "destination": "<destination>" 
         }
     ]
```

SO compatible: Linux y Windows 

Propiedades del personalizador de archivos:

- **sourceUri**: un punto de conexión de almacenamiento accesible, puede tratarse de GitHub o Azure Storage. Solo puede descargar un archivo, no un directorio completo. Si necesita descargar un directorio, utilice un archivo comprimido y, a continuación, descomprímalo mediante los personalizadores de Shell o PowerShell. 
- **destination**: es la ruta de acceso completa y el nombre de archivo de destino. Todas las rutas de acceso y los subdirectorios a los que se hace referencia deben existir; use los personalizadores de Shell o PowerShell para establecerlos con antelación. Puede usar los personalizadores de script para crear la ruta de acceso. 

Los directorios de Windows y las rutas de acceso de Linux lo admiten, aunque hay algunas diferencias: 
- Sistema operativo Linux: la única ruta de acceso en la que el generador de imágenes puede escribir es /tmp.
- Windows: no hay ninguna restricción en la ruta de acceso, pero esta debe existir.
 
 
Si se produce un error al intentar descargar el archivo o colocarlo en un directorio concreto, se producirá un error en el paso de personalización, que se registrará en customization.log.

> [!NOTE]
> el personalizador de archivos solo es adecuado para descargas de archivos pequeños, inferiores a 20 MB. Para descargas de archivos más grandes, use un script o un comando insertado, el código de uso para descargar archivos, como `wget` o `curl` de Linux, o `Invoke-WebRequest` de Windows.

Los archivos del personalizador de archivos se pueden descargar desde Azure Storage mediante [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

### <a name="generalize"></a>Generalize 
De forma predeterminada, Azure Image Builder también ejecutará código de "desaprovisionamiento" al final de cada fase de personalización de la imagen con el fin de "generalizar" la imagen. La generalización es un proceso en el que la imagen se configura para que pueda volver a usarse para crear varias máquinas virtuales. Para las máquinas virtuales de Windows, Azure Image Builder utiliza Sysprep. Para Linux, Azure Image Builder ejecuta "waagent-deprovision". 

Es posible que los comandos que utiliza Image Builder para la generalización no sean adecuados para todas las situaciones, por lo que Azure Image Builder le permitirá personalizar este comando si es necesario. 

Si está migrando una personalización existente y usa comandos Sysprep/waagent diferentes, puede usar los comandos genéricos de Image Builder y, si la creación de la máquina virtual produce un error, usar sus propios comandos Sysprep o waagent.

Si Azure Image Builder crea una imagen personalizada de Windows correctamente y usted crea una máquina virtual a partir de ella pero la creación produce un error o no se realiza correctamente, deberá revisar la documentación de Windows Server Sysprep o presentar una solicitud de soporte técnico al equipo de soporte técnico del Servicio al cliente de Windows Server Sysprep, quien podrá solucionar problemas y orientarlo en la utilización correcta de Sysprep.


#### <a name="default-sysprep-command"></a>Comando de Sysprep predeterminado
```powershell
echo '>>> Waiting for GA to start ...'
while ((Get-Service RdAgent).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureTelemetryService).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureGuestAgent).Status -ne 'Running') { Start-Sleep -s 5 }
echo '>>> Sysprepping VM ...'
if( Test-Path $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml ){ rm $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml -Force} & $Env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit
while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 5  } else { break } }
```
#### <a name="default-linux-deprovision-command"></a>Comando de desaprovisionamiento de Linux predeterminado

```bash
/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync
```

#### <a name="overriding-the-commands"></a>Reemplazo de los comandos
Para reemplazar los comandos, use los aprovisionadores de scripts de PowerShell o Shell para crear los archivos de comandos con el nombre de archivo exacto y colóquelos en los directorios correctos:

* Windows: c:\DeprovisioningScript.ps1
* Linux: /tmp/DeprovisioningScript.sh

Image Builder leerá estos comandos, que se escriben en los registros de AIB, "customization.log". Consulte la [solución de problemas](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-logs) sobre cómo recopilar registros.
 
## <a name="properties-distribute"></a>Propiedades: distribución

Azure Image Builder admite tres destinos de distribución: 

- **managedImage**: imagen administrada.
- **sharedImage**: galería de imágenes compartidas.
- **VHD**: disco duro virtual en una cuenta de almacenamiento.

Puede distribuir una imagen a ambos tipos de destino en la misma configuración; para ello, consulte estos [ejemplos](https://github.com/danielsollondon/azvmimagebuilder/blob/7f3d8c01eb3bf960d8b6df20ecd5c244988d13b6/armTemplates/azplatform_image_deploy_sigmdi.json#L80).

Dado que puede tener más de un destino al que distribuir, Image Builder mantiene un estado para cada destino de distribución al que puede accederse consultando el `runOutputName`.  El `runOutputName` es un objeto que puede consultar después de la distribución para obtener información acerca de esa distribución. Por ejemplo, puede consultar la ubicación del disco duro virtual o las regiones a las que se ha replicado la versión de la imagen. Se trata de una propiedad de cada destino de distribución. El `runOutputName` debe ser único para cada destino de distribución.
 
### <a name="distribute-managedimage"></a>Distribución: managedImage

El resultado de la imagen será un recurso de imagen administrada.

```json
"distribute": [
        {
"type":"managedImage",
       "imageId": "<resource ID>",
       "location": "<region>",
       "runOutputName": "<name>",
       "artifactTags": {
            "<name": "<value>",
             "<name>": "<value>"
               }
         }]
```
 
Propiedades de la distribución:
- **type**: managedImage 
- **imageId**: identificador de recurso de la imagen de destino, formato esperado: /subscriptions/\<subscriptionId>/resourceGroups/\<destinationResourceGroupName>/providers/Microsoft.Compute/images/\<imageName>
- **location**: ubicación de la imagen administrada.  
- **runOutputName**: nombre único para identificar la distribución.  
- **artifactTags**: etiquetas de par clave-valor opcionales especificadas por el usuario.
 
 
> [!NOTE]
> El grupo de recursos de destino debe existir.
> Si quiere que la imagen se distribuya a otra región, el tiempo de implementación será mayor. 

### <a name="distribute-sharedimage"></a>Distribución: sharedImage 
La Galería de imágenes compartidas de Azure es un nuevo servicio de administración de imágenes que permite administrar la replicación de las regiones de la imagen, el control de versiones y el uso compartido de imágenes personalizadas. Azure Image Builder admite la distribución con este servicio, por lo que puede distribuir imágenes a las regiones admitidas por las galerías de imágenes compartidas. 
 
Una galería de imágenes compartidas tiene los siguientes componentes: 
 
- Galería: contenedor para varias imágenes compartidas. Una galería se implementa en una región.
- Definiciones de imagen: una agrupación conceptual para las imágenes. 
- Versiones de las imágenes: se trata de un tipo de imagen usado para implementar una máquina virtual o un conjunto de escalado. Las versiones de las imágenes se pueden replicar a otras regiones en las que deban implementarse máquinas virtuales.
 
Antes de poder distribuir a la Galería de imágenes, debe crear una galería y una definición de imagen; para ello, consulte [Imágenes compartidas](shared-images.md). 

```json
{
     "type": "sharedImage",
     "galleryImageId": “<resource ID>”,
     "runOutputName": "<name>",
     "artifactTags": {
          "<name": "<value>",
           "<name>": "<value>"
             }
     "replicationRegions": [
        "<region where the gallery is deployed>",
        "<region>"
    ]}
``` 

Propiedades de la distribución para las galerías de imágenes compartidas:

- **tipo**: sharedImage  
- **galleryImageId**: id. de la galería de imágenes compartidas. El formato es: /subscriptions/\<subscriptionId>/resourceGroups/\<resourceGroupName>/providers/Microsoft.Compute/galleries/\<sharedImageGalleryName>/images/\<imageGalleryName>.
- **runOutputName**: nombre único para identificar la distribución.  
- **artifactTags**: etiquetas de par clave-valor opcionales especificadas por el usuario.
- **replicationRegions**: matriz de regiones para la replicación. Una de las regiones debe ser la región en la que está implementada la galería.
 
> [!NOTE]
> Puede usar Azure Image Builder en una región distinta a la de la galería, pero el servicio Azure Image Builder tendrá que transferir la imagen entre los centros de datos, lo que requerirá más tiempo. Image Builder creará automáticamente una versión de la imagen, basándose en un entero monotónico. Actualmente no lo puede especificar. 

### <a name="distribute-vhd"></a>Distribución: VHD  
Puede generar un disco duro virtual. A continuación, puede copiar el disco duro virtual y usarlo para publicar en Azure MarketPlace o usarlo con Azure Stack.  

```json
 { 
     "type": "VHD",
     "runOutputName": "<VHD name>",
     "tags": {
          "<name": "<value>",
           "<name>": "<value>"
             }
 }
```
 
SO compatible: Windows y Linux

Parámetros de distribución de VHD:

- **tipo**: VHD.
- **runOutputName**: nombre único para identificar la distribución.  
- **etiquetas**: etiquetas de par clave-valor opcionales especificadas por el usuario.
 
Azure Image Builder no permite al usuario especificar una ubicación de la cuenta de almacenamiento, pero puede consultar el estado de `runOutputs` para obtener la ubicación.  

```azurecli-interactive
az resource show \
   --ids "/subscriptions/$subscriptionId/resourcegroups/<imageResourceGroup>/providers/Microsoft.VirtualMachineImages/imageTemplates/<imageTemplateName>/runOutputs/<runOutputName>"  | grep artifactUri 
```

> [!NOTE]
> Una vez creado el disco duro virtual, cópielo en una ubicación distinta tan pronto como sea posible. El disco duro virtual se almacena en una cuenta de almacenamiento del grupo de recursos temporal creado al enviar la plantilla de imagen al servicio Azure Image Builder. Si elimina la plantilla de imagen, perderá el disco duro virtual. 
 
## <a name="next-steps"></a>Pasos siguientes

Encontrará archivos .json de ejemplo para diferentes escenarios en el [GitHub de Azure Image Builder](https://github.com/danielsollondon/azvmimagebuilder).
 
 
 
 
 
 
 
 
 
 
