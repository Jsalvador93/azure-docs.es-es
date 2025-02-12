---
title: Introducción a Microsoft Azure FXT Edge Filer
description: Se describe la caché de almacenamiento híbrido de Azure FXT Edge Filer, una solución de aceleración de archivado y acceso a archivos destinada a la informática de alto rendimiento
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: overview
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: 58d4a2a52757b6ace1059fcccf379df3de5fd75c
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542902"
---
# <a name="what-is-azure-fxt-edge-filer-hybrid-storage-cache"></a>¿Qué es la caché de almacenamiento híbrido de Azure FXT Edge Filer?

Azure FXT Edge Filer es un dispositivo de almacenamiento en caché híbrido que proporciona acceso rápido a los archivos y archivado activo para las tareas de informática de alto rendimiento (HPC).

Funciona con varios orígenes de datos, tanto si se almacenan en un centro de datos local, de forma remota o en la nube. Azure FXT Edge Filer puede proporcionar un espacio de nombres unificado para los datos situados en diversos sistemas de almacenamiento.

Tres o más dispositivos de hardware de FXT Edge Filer trabajan conjuntamente como un sistema de archivos en clúster para proporcionar la caché. Para más información sobre cómo comprar el hardware necesario, póngase en contacto con su representante de Microsoft. 

Para más información, lea la hoja de información del producto en [Azure FXT Edge Filer](https://azure.microsoft.com/services/fxt-edge-filer/).

## <a name="use-cases"></a>Casos de uso

Azure FXT Edge Filer mejora la productividad en flujos de trabajo como los siguientes:

* Flujo de trabajo de acceso a archivos de lectura intensiva 
* Protocolos NFSv3 o SMB2
* Granjas de servidores de 1000 a 100 000 núcleos de CPU

### <a name="nas-optimization-and-scaling"></a>Optimización y escalado de NAS

Puede usar la caché de Azure FXT Edge Filer para acceder sin problemas a los sistemas existentes de NetApp y Dell EMC Isilon. También puede agregar blobs de Azure u otro almacenamiento en la nube para proporcionar escalabilidad sin tener que rehacer los procesos de acceso a los datos en el lado cliente. 

### <a name="wan-caching"></a>Almacenamiento en caché WAN

Azure FXT Edge Filer puede usarse para permitir el acceso rápido a los archivos para usuarios avanzados cuando los datos que necesitan están almacenados en otra parte. Proporcione acceso mientras mantiene la copia de seguridad y otros sistemas de administración de datos en un centro de datos centralizado. 

### <a name="active-archive-in-azure-blob"></a>Archivado activo en blobs de Azure

Expanda su centro de datos al almacenamiento en la nube con Azure FXT Edge Filer como punto de acceso. 

## <a name="features"></a>Características 

Existen dos modelos de hardware. 

| Modelo | DRAM | SSD NVMe | Puertos de red | 
|-------|------|----------|---------------|
| FXT 6600 | 1536 GB | 25,6 TB | 6 x 25 Gb/10 Gb + 2 x 1 Gb |
| FXT 6400 | 768 GB | 12,8 TB | 6 x 25 Gb/10 Gb + 2 x 1 Gb |


## <a name="next-steps"></a>Pasos siguientes

* Para seguir aprendiendo sobre Azure FXT Edge Filer, lea las [especificaciones](fxt-specs.md) o el [tutorial de instalación](fxt-install.md).
* Conozca cómo comprar Azure FXT Edge Filer en la [página del producto de Azure FXT Edge Filer](https://azure.microsoft.com/services/fxt-edge-filer/).