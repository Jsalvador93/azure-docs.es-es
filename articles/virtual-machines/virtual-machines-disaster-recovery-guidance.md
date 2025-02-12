---
title: Escenarios de recuperación ante desastres para las máquinas virtuales de Azure | Microsoft Docs
description: Obtenga información sobre qué hacer si se produce una interrupción del servicio de Azure que afecta a las máquinas virtuales de Azure.
services: virtual-machines
documentationcenter: ''
author: kmouss
manager: gwallace
editor: ''
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: gwallace
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bc9ca5f5a638f0b36a28d58172fe8052b3d1522f
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875451"
---
# <a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Qué hacer si se produce una interrupción del servicio de Azure que afecta a las máquinas virtuales de Azure
En Microsoft, hacemos todo lo posible para garantizar que nuestros servicios estén siempre disponibles cuando los necesite. En ocasiones, debido a factores externos que escapan de nuestro control, se producen interrupciones de servicio no planeadas.

Microsoft proporciona Acuerdos de Nivel de Servicio para sus servicios como un compromiso en cuanto al tiempo de actividad y la conectividad. Puede encontrar el Acuerdo de Nivel de Servicio para los diferentes servicios de Azure en [Contratos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/).

Azure ya integra en su plataforma muchas características que admiten aplicaciones de alta disponibilidad. Para obtener más información sobre estos servicios, lea [Recuperación ante desastres y alta disponibilidad para aplicaciones de Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

En este artículo se expone un escenario real de recuperación ante desastres en el que toda una región experimenta una interrupción debido a un desastre natural importante o a una interrupción del servicio generalizada. Se trata de casos muy infrecuentes, pero debe estar preparado para la posibilidad de que se produzca una interrupción en toda una región. Si una región completa experimenta una interrupción del servicio, las copias con redundancia local de los datos estarían temporalmente no disponibles. Si ha habilitado la replicación geográfica, se almacenan en una región distinta tres copias adicionales de los blobs y las tablas de Azure Storage. En caso de una interrupción completa en una región o de un desastre en el que la región primaria no sea recuperable, Azure reasignará todas las entradas DNS a la región de replicación geográfica.

Para ayudarlo a administrar estos eventos poco frecuentes, le proporcionamos las siguientes orientaciones para máquinas virtuales de Azure destinadas a los casos de interrupción del servicio en toda una región donde se ha implementado la aplicación de máquina virtual de Azure.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>Opción 1: inicio de una conmutación por error mediante Azure Site Recovery
Puede configurar Azure Site Recovery para las VM de modo que pueda recuperar la aplicación con un solo clic en cuestión de minutos. Puede realizar replicaciones en la región de Azure que elija, no solo en regiones emparejadas. Puede empezar por [replicar las máquinas virtuales](https://aka.ms/a2a-getting-started). Puede [crear un plan de recuperación](../site-recovery/site-recovery-create-recovery-plans.md) para poder automatizar el proceso de conmutación por error al completo para la aplicación. Puede [probar las conmutaciones por error](../site-recovery/site-recovery-test-failover-to-azure.md) antes sin necesidad de que ni la aplicación de producción ni la replicación en curso se vean afectadas. Si se interrumpe una región primaria, no tiene más que [iniciar una conmutación por error](../site-recovery/site-recovery-failover.md) y traer la aplicación a la región de destino.


## <a name="option-2-wait-for-recovery"></a>Opción 2: Espera para la recuperación
En este caso, no se requieren acciones por su parte. Sabe que trabajaremos con rapidez para que el servicio de Azure vuelva a estar disponible. El estado actual del servicio se puede ver en el [panel de estado de los servicios de Azure](https://azure.microsoft.com/status/).

Esta es la mejor opción si no configuró Azure Site Recovery, el almacenamiento con redundancia geográfica con acceso de lectura o el almacenamiento con redundancia geográfica antes de la interrupción. Si ha configurado el almacenamiento con redundancia geográfica o el almacenamiento con redundancia geográfica con acceso de lectura para la cuenta de almacenamiento donde se almacenan los discos duros virtuales de la máquina virtual (VHD), puede fijarse en cómo recuperar el VHD de la imagen base y tratar de aprovisionar una nueva máquina virtual desde ahí. Esta no es la mejor opción, ya que no hay ninguna garantía de sincronización de datos. Por lo tanto, no se garantiza que esta opción funcione.


> [!NOTE]
> Tenga en cuenta que no tiene ningún control sobre este proceso y que solo se producirá si se produce una interrupción del servicio en toda la región. Por este motivo, también debe confiar en otras estrategias de copia de seguridad específicas de la aplicación para lograr el máximo nivel de disponibilidad. Para obtener más información, consulte la sección sobre las [estrategias de datos para la recuperación ante desastres](https://docs.microsoft.com/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan).
>
>

## <a name="next-steps"></a>Pasos siguientes

- Empiece a [proteger las aplicaciones que se ejecuten en máquinas virtuales de Azure](https://aka.ms/a2a-getting-started) con Azure Site Recovery.

- Para obtener más información sobre cómo implementar una estrategia de alta disponibilidad y recuperación ante desastres, consulte [Recuperación ante desastres y alta disponibilidad para aplicaciones de Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- Para obtener unos conocimientos técnicos detallados sobre las funcionalidades de una plataforma de nube, consulte [Guía técnica sobre resistencia en Azure](../resiliency/resiliency-technical-guidance.md).


- Si las instrucciones no están claras, o si desea que Microsoft realice las operaciones en su lugar, póngase en contacto con el [servicio de asistencia al cliente](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
