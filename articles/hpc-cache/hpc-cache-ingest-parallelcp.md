---
title: 'Ingesta de datos en la versión preliminar de Azure HPC Cache: script de copia en paralelo'
description: Uso de un script de copia en paralelo para mover datos a un destino de Blob Storage en Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: v-erkell
ms.openlocfilehash: 852b4e692a4316c7701c8c179039104bee561949
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71180958"
---
# <a name="azure-hpc-cache-preview-data-ingest---parallel-copy-script-method"></a>Ingesta de datos en Azure HPC Cache (versión preliminar): método de script de copia en paralelo

En este artículo se proporcionan instrucciones para crear el script ``parallelcp`` y usarlo para mover datos a un contenedor de Blob Storage para su uso con Azure HPC Cache.

Para más información sobre cómo mover datos a Azure Blob Storage para Azure HPC Cache, consulte [Traslado de datos a Azure Blob Storage para Azure HPC Cache](hpc-cache-ingest.md).

## <a name="create-the-parallelcp-script"></a>Creación del script parallelcp

El script siguiente agregará el archivo ejecutable `parallelcp`. (Este script está diseñado para Ubuntu; si usa otra distribución, debe instalar ``parallel`` por separado).

```bash
sudo touch /usr/bin/parallelcp && sudo chmod 755 /usr/bin/parallelcp && sudo sh -c "/bin/cat >/usr/bin/parallelcp" <<EOM 
#!/bin/bash

display_usage() { 
    echo -e "\nUsage: \$0 SOURCE_DIR DEST_DIR\n" 
} 

if [  \$# -le 1 ] ; then 
    display_usage
    exit 1
fi 
 
if [[ ( \$# == "--help") ||  \$# == "-h" ]] ; then 
    display_usage
    exit 0
fi 

SOURCE_DIR="\$1"
DEST_DIR="\$2"

if [ ! -d "\$SOURCE_DIR" ] ; then
    echo "Source directory \$SOURCE_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -d "\$DEST_DIR" ] && ! mkdir -p \$DEST_DIR ; then
    echo "Destination directory \$DEST_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -w "\$DEST_DIR" ] ; then
    echo "Destination directory \$DEST_DIR is not writeable, or is not a directory"
    display_usage
    exit 3
fi

if ! which parallel > /dev/null ; then
    sudo apt-get update && sudo apt install -y parallel
fi

DIRJOBS=225
JOBS=225
find \$SOURCE_DIR -mindepth 1 -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$DIRJOBS -0 "mkdir -p \$DEST_DIR/{}"
find \$SOURCE_DIR -mindepth 1 ! -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$JOBS -0 "cp -P \$SOURCE_DIR/{} \$DEST_DIR/{}"
EOM
```

## <a name="parallel-copy-example"></a>Ejemplo de copia paralela

En este ejemplo se usa el script de copia en paralelo para compilar ``glibc`` mediante archivos de origen en Azure HPC Cache.

Los archivos de origen se almacenan en caché en el punto de montaje de Azure HPC Cache y los archivos objeto se almacenan en el disco duro local.

En este ejemplo se usa el script de copia en paralelo con la opción ``-j`` y ``make`` para conseguir paralelización.

```bash
sudo apt-get update
sudo apt install -y gcc bison gcc binutils make parallel
cd
wget https://mirrors.kernel.org/gnu/libc/glibc-2.27.tar.bz2
tar jxf glibc-2.27.tar.bz2
ln -s /nfs/cache1 hpccache
time parallelcp glibc-2.27 avere/glibc-2.27
cd
mkdir obj
mkdir usr
cd obj
/home/azureuser/avere/glibc-2.27/configure --prefix=/home/azureuser/usr
time make -j
```
