---
title: archivo de inclusión
description: archivo de inclusión
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/01/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: feec6a695ad867d26d32904d020648b029f9da35
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186405"
---
## <a name="create-a-media-services-account"></a>Creación de una cuenta de Media Services

Primero debe crear una cuenta de Media Services. En esta sección se muestra lo que necesita para la creación de la cuenta mediante la CLI de Azure.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el siguiente comando. Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran recursos como las cuentas de Azure Media Services y las cuentas de almacenamiento asociadas.

Puede sustituir `amsResourceGroup` por su valor.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Al crear una cuenta de Media Services, debe proporcionar el nombre de un recurso de cuenta de Azure Storage. La cuenta de almacenamiento especificada está asociada a su cuenta de Media Services. Para más información sobre cómo se utilizan las cuentas de almacenamiento en Media Services, consulte [Cuentas de almacenamiento](../articles/media-services/latest/storage-account-concept.md).

Debe tener una cuenta de almacenamiento **Principal** y puede tener cualquier número de cuentas de almacenamiento **Secundarias** asociadas a su cuenta de Media Services. Media Services admite cuentas de **Uso general v2** (GPv2) o **Uso general v1** (GPv1). No se permiten cuentas de solo BLOB como **Principal**. Si quiere obtener más información sobre las cuentas de almacenamiento, consulte [Opciones de la cuenta de Azure Storage](../articles/storage/common/storage-account-options.md). 

En este ejemplo, se va a crear una cuenta LRS estándar de uso general v2. Si quiere experimentar con las cuentas de almacenamiento, use `--sku Standard_LRS`. Sin embargo, al seleccionar una SKU de producción debe considerar `--sku Standard_RAGRS`, que proporciona replicación geográfica para la continuidad empresarial. Para más información, consulte los comandos [storage accounts](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest).
 
El siguiente comando crea una cuenta de almacenamiento que se asociará a la cuenta de Media Services. En el siguiente script, puede sustituir `storageaccountforams` por su valor. `amsResourceGroup` debe coincidir con el valor asignado al grupo de recursos del paso anterior. El nombre de la cuenta de almacenamiento debe tener una longitud inferior a 24.

```azurecli
az storage account create --name storageaccountforams \  
  --kind StorageV2 \
  --sku Standard_LRS \
  -l westus2 \
  -g amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Creación de una cuenta de Media Services

El siguiente comando de la CLI de Azure crea una nueva cuenta de Media Services. Puede reemplazar los valores siguientes: `amsaccount` `storageaccountforams` (deben coincidir con el valor que especificó para la cuenta de almacenamiento) y `amsResourceGroup` (debe coincidir con el valor que especificó para el grupo de recursos).

```azurecli
az ams account create --name amsaccount \
  -l westus2 \
  -g amsResourceGroup --storage-account storageaccountforams
```
