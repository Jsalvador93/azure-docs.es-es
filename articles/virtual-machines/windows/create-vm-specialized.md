---
title: Creación de una máquina virtual Windows desde un VHD especializado en Azure | Microsoft Docs
description: Cree una nueva máquina virtual Windows asociando un disco administrado especializado como el disco del SO mediante el modelo de implementación de Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 10/10/2018
ms.author: cynthn
ms.openlocfilehash: 5dde098277b16c7ec5339aa6b963b04dd608c8ac
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70079666"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-by-using-powershell"></a>Creación de una VM de Windows desde un disco especializado mediante PowerShell

Cree una nueva VM asociando un disco administrado especializado como disco del SO. Un disco especializado es una copia del disco duro virtual (VHD) de una máquina virtual existente que contiene las cuentas de usuario, las aplicaciones y otros datos de estado de la máquina virtual original. 

Cuando se usa un VHD especializado para crear una máquina virtual, la nueva máquina virtual conserva el nombre de equipo de la máquina virtual original. También se conserva otra información específica del equipo y, en algunos casos, esta información duplicada podría ocasionar problemas. Al copiar una máquina virtual, tenga en cuenta en qué tipo de información específica del equipo se basan las aplicaciones.

Tiene varias opciones:
* [Uso de un disco administrado existente](#option-1-use-an-existing-disk). Esta opción es útil si tiene una VM que no funciona correctamente. Puede eliminar la máquina virtual y, después, reutilizar el disco administrado para crear una nueva. 
* [Carga de un disco duro virtual](#option-2-upload-a-specialized-vhd) 
* [Copia de una VM de Azure existente mediante instantáneas](#option-3-copy-an-existing-azure-vm)

También puede usar Azure Portal para [crear una nueva VM a partir de un VHD especializado](create-vm-specialized-portal.md).

En este artículo se muestra cómo usar los discos administrados. Si tiene una implementación heredada que requiere el uso de una cuenta de almacenamiento, vea [Create a VM from a specialized VHD in a storage account](sa-create-vm-specialized.md) (Creación de una máquina virtual a partir de un VHD especializado en una cuenta de almacenamiento).

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="option-1-use-an-existing-disk"></a>Opción 1: Uso de un disco existente

Si tenía una VM que ha eliminado y quiere volver a usar el disco del sistema operativo para crear una nueva VM, use [Get-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/get-azdisk).

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Ahora puede adjuntar este disco como disco del sistema operativo a una [nueva VM](#create-the-new-vm).

## <a name="option-2-upload-a-specialized-vhd"></a>Opción 2: Carga de un disco duro virtual especializado

Puede cargar el VHD de una máquina virtual especializada creada con una herramienta de virtualización local, como Hyper-V, o de una máquina virtual exportada desde otra nube.

### <a name="prepare-the-vm"></a>Preparación de la VM
Use el disco duro virtual tal cual para crear una nueva máquina virtual. 
  
  * [Preparar de un VHD de Windows para cargar en Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **No** generalice la VM mediante Sysprep.
  * Quite todas las herramientas de virtualización de invitado y los agentes instalados en la VM (como las herramientas de VMware).
  * Asegúrese de que la VM se configura para obtener la dirección IP y la configuración de DNS desde DHCP. Esto garantiza que el servidor obtiene una dirección IP dentro de la red virtual cuando se inicia. 


### <a name="get-the-storage-account"></a>Obtención de la cuenta de almacenamiento
Necesitará una cuenta de almacenamiento de Azure para almacenar el VHD cargado. Puede usar una cuenta de almacenamiento existente o crear una nueva. 

Muestre las cuentas de almacenamiento disponibles.

```powershell
Get-AzStorageAccount
```

Para usar una cuenta de almacenamiento existente, continúe con la sección [Carga del VHD](#upload-the-vhd-to-your-storage-account).

Cree una cuenta de almacenamiento.

1. Necesitará el nombre del grupo de recursos donde se creará la cuenta de almacenamiento. Use Get-AzResourceGroup para ver todos los grupos de recursos de la suscripción.
   
    ```powershell
    Get-AzResourceGroup
    ```

    Cree un grupo de recursos denominado *myResourceGroup* en la región *Oeste de EE. UU*.

    ```powershell
    New-AzResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. Cree una cuenta de almacenamiento denominada *mystorageaccount* en este nuevo grupo de recursos con el cmdlet [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount).
   
    ```powershell
    New-AzStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>Carga del VHD en la cuenta de almacenamiento 
Use el cmdlet [Add-AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) para cargar el VHD en un contenedor de su cuenta de almacenamiento. En este ejemplo se carga el archivo *myVHD.vhd* de "C:\Users\Public\Documents\Virtual hard disks\" a una cuenta de almacenamiento denominada *mystorageaccount* en el grupo de recursos *myResourceGroup*. El archivo se almacena en el contenedor llamado *mycontainer* y el nuevo nombre de archivo será *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzVhd -ResourceGroupName $resourceGroupName `
   -Destination $urlOfUploadedVhd `
   -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Si los comandos son correctos, obtendrá una respuesta similar a la siguiente:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Este comando tardará algún tiempo en completarse, dependiendo de la conexión de red y del tamaño del archivo VHD.

### <a name="create-a-managed-disk-from-the-vhd"></a>Creación de un disco administrado a partir del VHD

Cree un disco administrado a partir del VHD especializado en la cuenta de almacenamiento mediante [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk). En este ejemplo se utiliza *myOSDisk1* para el nombre del disco, coloca el disco en el almacenamiento *Standard_LRS* y usa *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* como el URI para el disco duro virtual de origen.

Cree un grupo de recursos para la máquina virtual.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Cree el disco del SO a partir del VHD cargado. 

```powershell
$sourceUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
$osDiskName = 'myOsDisk'
$osDisk = New-AzDisk -DiskName $osDiskName -Disk `
    (New-AzDiskConfig -AccountType Standard_LRS  `
    -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-3-copy-an-existing-azure-vm"></a>Opción 3: Copia de una máquina virtual de Azure existente

Puede crear una copia de una máquina virtual que use discos administrados mediante la realización de una instantánea de la máquina virtual, para usarla después para crear un disco administrado y una máquina virtual.


### <a name="take-a-snapshot-of-the-os-disk"></a>Realización de la instantánea del disco del SO

Puede realizar una instantánea de una máquina virtual completa (incluidos todos los discos) o solo de un disco. En los pasos siguientes se describe cómo hacer una instantánea solo del disco del SO de la VM con el cmdlet [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot). 

Primero, configure algunos parámetros. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Obtenga el objeto de VM.

```powershell
$vm = Get-AzVM -Name $vmName `
   -ResourceGroupName $resourceGroupName
```
Obtenga el nombre del disco del SO.

 ```powershell
$disk = Get-AzDisk -ResourceGroupName $resourceGroupName `
   -DiskName $vm.StorageProfile.OsDisk.Name
```

Cree la configuración de la instantánea. 

 ```powershell
$snapshotConfig =  New-AzSnapshotConfig `
   -SourceUri $disk.Id `
   -OsType Windows `
   -CreateOption Copy `
   -Location $location 
```

Tome la instantánea.

```powershell
$snapShot = New-AzSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Para usar esta instantánea para crear una VM que precisa de un alto rendimiento, agregue el parámetro `-AccountType Premium_LRS` al comando New-AzSnapshotConfig. Este parámetro crea la instantánea para que se almacene como disco administrado Prémium. Los discos administrados Prémium son más caros que los Estándar, así que asegúrese de que necesita Prémium antes de usar este parámetro.

### <a name="create-a-new-disk-from-the-snapshot"></a>Creación de un disco a partir de la instantánea

Cree un disco administrado a partir de la instantánea mediante [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk). Este ejemplo usa *myOSDisk* para el nombre del disco.

Cree un grupo de recursos para la máquina virtual.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Defina el nombre del disco del SO. 

```powershell
$osDiskName = 'myOsDisk'
```

Cree el disco administrado.

```powershell
$osDisk = New-AzDisk -DiskName $osDiskName -Disk `
    (New-AzDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Creación de la máquina virtual 

Cree recursos de red y otros recursos de máquina virtual para que los use la nueva máquina virtual.

### <a name="create-the-subnet-and-virtual-network"></a>Creación de la subred y de la red virtual

Cree la [red virtual](../../virtual-network/virtual-networks-overview.md) y la subred para la máquina virtual.

1. Cree la subred. En este ejemplo se crea una subred llamada *mySubNet*, en el grupo de recursos *myDestinationResourceGroup* y se establece el prefijo de la dirección de subred en *10.0.0.0/24*.
   
    ```powershell
    $subnetName = 'mySubNet'
    $singleSubnet = New-AzVirtualNetworkSubnetConfig `
       -Name $subnetName `
       -AddressPrefix 10.0.0.0/24
    ```
    
2. Creación de la red virtual. En este ejemplo se establece el nombre de la red virtual en *myVnetName*, la ubicación en *Oeste de EE. UU.* y el prefijo de dirección de la red virtual en *10.0.0.0/16*. 
   
    ```powershell
    $vnetName = "myVnetName"
    $vnet = New-AzVirtualNetwork `
       -Name $vnetName -ResourceGroupName $destinationResourceGroup `
       -Location $location `
       -AddressPrefix 10.0.0.0/16 `
       -Subnet $singleSubnet
    ```    
    

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Creación del grupo de seguridad de red y las reglas de RDP
Para poder iniciar sesión en la máquina virtual con el protocolo de escritorio remoto (RDP), debe tener una regla de seguridad que permita el acceso RDP en el puerto 3389. En nuestro ejemplo, el disco duro virtual de la nueva máquina virtual se creó a partir de una máquina virtual especializada existente, por lo que puede usar una cuenta de la máquina virtual de origen para RDP.

En este ejemplo se establece el nombre del grupo de seguridad de red (NSG) en *myNsg* y el nombre de la regla de RDP en *myRdpRule*.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzNetworkSecurityGroup `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -Name $nsgName -SecurityRules $rdpRule
    
```

Para obtener más información sobre puntos de conexión y las reglas NSG, consulte [Apertura de puertos para una máquina virtual en Azure mediante PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Creación de una dirección IP pública y una NIC
Para permitir la comunicación con la máquina virtual en la red virtual, necesitará una [dirección IP pública](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) y una interfaz de red.

1. Cree la dirección IP pública. En este ejemplo, el nombre de la dirección IP pública se establece en *myIP*.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzPublicIpAddress `
       -Name $ipName -ResourceGroupName $destinationResourceGroup `
       -Location $location `
       -AllocationMethod Dynamic
    ```       
    
2. Cree la NIC. En este ejemplo, el nombre de la NIC se establece en *myNicName*.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzNetworkInterface -Name $nicName `
       -ResourceGroupName $destinationResourceGroup `
       -Location $location -SubnetId $vnet.Subnets[0].Id `
       -PublicIpAddressId $pip.Id `
       -NetworkSecurityGroupId $nsg.Id
    ```
    


### <a name="set-the-vm-name-and-size"></a>Establecimiento del nombre de VM y el tamaño

En este ejemplo se establece el nombre de la máquina virtual como *myVM* y el tamaño de la máquina virtual en *Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>Incorporación de la NIC
    
```powershell
$vm = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>Adición del disco del SO 

Agregue el disco del SO a la configuración mediante [Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk). Este ejemplo establece el tamaño del disco en *128 GB* y adjunta el disco administrado como un disco del SO *Windows*.
 
```powershell
$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>Finalización de la máquina virtual 

Cree la VM mediante [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) con las configuraciones que se acaban de crear.

```powershell
New-AzVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Si este comando es correcto, verá un resultado similar al siguiente:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Comprobación de que se creó la máquina virtual
Debería ver la máquina virtual recién creada en el [Azure Portal](https://portal.azure.com), en **Examinar** > **Máquinas virtuales** o mediante los comandos de PowerShell siguientes.

```powershell
$vmList = Get-AzVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Pasos siguientes
Inicie sesión en la nueva máquina virtual. Para más información, consulte [Conexión a una máquina virtual de Azure donde se ejecuta Windows Server e inicio de sesión en ella](connect-logon.md).

