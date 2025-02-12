---
title: Azure Lighthouse y el programa Proveedor de soluciones en la nube
description: Al usar la administración de recursos delegados de Azure, es importante tener en cuenta la seguridad y el control de acceso.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 08/22/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 399d2cb829c0425e3c9ee70a61cafde8568f903b
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70012109"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Lighthouse y el programa Proveedor de soluciones en la nube

Si es un partner [CSP (Proveedor de soluciones en la nube)](https://docs.microsoft.com/partner-center/csp-overview), ya puede acceder a las suscripciones a Azure creadas para sus clientes desde el programa CSP mediante la funcionalidad [Administrar en nombre de (AOBO)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO). Este acceso le permite admitir, configurar y administrar directamente las suscripciones de sus clientes.

Azure Lighthouse permite usar la administración de recursos delegados de Azure junto con AOBO. Esto ayuda a mejorar la seguridad y reduce el acceso innecesario, gracias a la habilitación de permisos más pormenorizados para los usuarios. También permite conseguir mayor eficiencia y escalabilidad, ya que los usuarios pueden trabajar en varias suscripciones de clientes con un único inicio de sesión en el inquilino.

## <a name="administer-on-behalf-of-aobo"></a>Administrar en nombre de (AOBO)

Con AOBO, cualquier usuario con el rol [Agente de administración](https://docs.microsoft.com/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) en el inquilino tendrá acceso AOBO a las suscripciones a Azure que cree desde el programa CSP. Cualquier usuario que necesite tener acceso a las suscripciones de los clientes debe ser miembro de este grupo. AOBO no permite la flexibilidad de crear grupos distintos que funcionen con distintos clientes ni de habilitar diferentes roles para grupos o usuarios.

![Administración de inquilinos mediante AOBO](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Administración de recursos delegados de Azure

Con la administración de recursos delegados de Azure, puede asignar grupos diferentes a distintos clientes o roles, tal y como se muestra en el diagrama siguiente. Dado que los usuarios tendrán el nivel de acceso adecuado a través de la administración de recursos delegados de Azure, puede reducir el número de usuarios que tienen el rol de agente de administración (y, por tanto, tienen acceso AOBO completo). Esto ayuda a mejorar la seguridad, ya que limita el acceso innecesario a los recursos de los clientes. También le ofrece más flexibilidad para administrar varios clientes a escala.

Para incorporar una suscripción creada desde el programa CSP, siga los pasos descritos en [Onboard a subscription to Azure delegated resource management](../how-to/onboard-customer.md) (Incorporación de una suscripción a la administración de recursos delegados de Azure). Cualquier usuario que tenga el rol de agente de administración en el inquilino puede realizar esta incorporación.

![Administración de inquilinos mediante AOBO y la administración de recursos delegados de Azure](../media/csp-2.jpg)

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [experiencias de administración entre inquilinos](cross-tenant-management-experience.md).
- Aprenda a [incorporar una suscripción a la administración de recursos delegados de Azure](../how-to/onboard-customer.md).
- Más información acerca del [programa Proveedor de soluciones en la nube](https://docs.microsoft.com/partner-center/csp-overview).
