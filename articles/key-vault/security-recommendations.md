---
title: Recomendaciones de seguridad para Azure Key Vault
description: Recomendaciones de seguridad para Azure Key Vault. La implementación de esta guía le ayudará a cumplir sus obligaciones de seguridad, tal y como se describe en nuestro modelo de responsabilidad compartida.
services: key-vault
author: barclayn
manager: rkarlin
ms.service: key-vault
ms.topic: article
ms.date: 09/23/2019
ms.author: barclayn
ms.custom: security-recommendations
ms.openlocfilehash: 90965618b1d2cb126d56453d46a5bebc85319c0f
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219650"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Recomendaciones de seguridad para Azure Key Vault

Este artículo contiene recomendaciones de seguridad para Azure Key Vault. La implementación de estas recomendaciones le ayudará a cumplir sus obligaciones de seguridad, tal y como se describe en nuestro modelo de responsabilidad compartida. Para más información sobre lo que Microsoft hace para cumplir las responsabilidades del proveedor de servicios, consulte [Responsabilidades compartidas de la informática en la nube](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf).

Algunas de las recomendaciones incluidas en este artículo se pueden supervisar automáticamente mediante Azure Security Center. Azure Security Center es la primera línea de defensa en la protección de los recursos de Azure. Se encarga de analizar el estado de seguridad de los recursos de Azure para identificar posibles vulnerabilidades de seguridad. Después, proporciona recomendaciones sobre cómo abordarlas.

- Para más información sobre las recomendaciones de seguridad de Azure Security Center, consulte [Recomendaciones de seguridad en Azure Security Center](../security-center/security-center-recommendations.md).
- Para más información sobre Azure Security Center, consulte [¿Qué es Azure Security Center?](../security-center/security-center-intro.md)

## <a name="recommendations"></a>Recomendaciones

| Category | Recomendación | Comentarios | Security Center |
|-|-|----|--|
| Protección de datos |Habilitación de la eliminación temporal | La [eliminación temporal](key-vault-ovw-soft-delete.md) permite recuperar almacenes y objetos de almacén eliminados. |  - |
| Protección de datos | Limitación del acceso a datos de almacén  | Siga el principio de privilegios mínimos y limite qué miembros de su organización tienen acceso a los datos del almacén. |  - |
| Administración de identidades y acceso | Limitación del número de usuarios con acceso de colaborador | Si un usuario tiene permisos de colaborador en un plano de administración de Key Vault, el usuario se puede conceder a sí mismo acceso al plano de datos estableciendo una directiva de acceso de Key Vault. Debe controlar estrechamente quién tiene acceso de rol de colaborador a los almacenes de claves. Asegúrese de que solo las personas autorizadas que necesiten acceso puedan acceder a sus almacenes y administrarlos. Puede consultar [Protección del acceso a un almacén de claves](key-vault-secure-your-key-vault.md). | - |
| Supervisión | Se deben habilitar los registros de diagnóstico en Key Vault | Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. | [Sí](../security-center/security-center-identity-access.md) |
| Supervisión | Restricción del acceso a los registros de Azure Key Vault | Los [registros de Key Vault](key-vault-logging.md) guardan información sobre las actividades realizadas en el almacén, como la creación o eliminación de almacenes, claves y secretos, y se pueden usar durante una investigación. |  - |
| Redes |Limitación de la exposición de la red | El acceso a la red debe limitarse a redes virtuales usadas por soluciones que requieren acceso al almacén. Consulte la información de [Puntos de conexión de servicio de red virtual para Azure Key Vault](key-vault-overview-vnet-service-endpoints.md). | - |

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con el proveedor de la aplicación para ver si hay más requisitos de seguridad. Para obtener más información sobre el desarrollo de aplicaciones seguras, vea [Documentación sobre desarrollo seguro](../security/fundamentals/abstract-develop-secure-apps.md).
