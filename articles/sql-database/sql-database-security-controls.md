---
title: Controles de seguridad para Azure SQL Database
description: Lista de comprobación de los controles de seguridad para evaluar Azure SQL Database
services: sql-database
author: msmbaldwin
manager: rkalrin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 77ff55389bac53d8719d86b4ac77f281415af49f
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886177"
---
# <a name="security-controls-for-azure-sql-database"></a>Controles de seguridad para Azure SQL Database

En este artículo se explican los controles de seguridad integrados en Azure SQL Database.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

SQL Database incluye una [base de datos única](sql-database-single-index.yml) y una [instancia administrada](sql-database-managed-instance.md). Las entradas siguientes se aplican a ambas ofertas, excepto si se indica lo contrario.

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | Se aplica solo a las [bases de datos únicas](sql-database-single-index.yml). |
| Compatibilidad con la inserción de Azure Virtual Network| Sí | Se aplica solo a las [instancias administradas](sql-database-managed-instance.md). |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Firewall en el nivel de base de datos y de servidor. El aislamiento de red es solo para [instancias administradas](sql-database-managed-instance.md). |
| Compatibilidad con la tunelización forzada| Sí | [Instancia administrada](sql-database-managed-instance.md) mediante VPN de [ExpressRoute](../expressroute/index.yml). |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure, como Log Analytics o Application Insights| Sí | SecureSphere, la solución SIEM de Imperva, también se admite gracias a la integración de [Azure Event Hubs](../event-hubs/index.yml) mediante la [auditoría de SQL](sql-database-auditing.md). |
| Registro y auditoría del plano de administración y de control| Sí | Sí, pero solo para algunos eventos |
| Registro y auditoría del plano de datos | Sí | Mediante la [auditoría de SQL](sql-database-auditing.md) |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory (Azure AD) |
| Authorization| Sí | None |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí | Se denomina "cifrado en uso", tal y como se describe en el artículo [Always Encrypted](sql-database-always-encrypted.md). El cifrado del servidor usa el [cifrado de datos transparente](transparent-data-encryption-azure-sql.md).|
| Cifrado en tránsito:<ul><li>Cifrado de Azure ExpressRoute</li><li>Cifrado en una red virtual</li><li>Cifrado entre redes virtuales</ul>| Sí | Se usa HTTPS. |
| Control de claves de cifrado, como CMK o BYOK| Sí | Se ofrece control de claves tanto administradas por el servicio como por el cliente. Este último se ofrece mediante [Azure Key Vault](../key-vault/index.yml). |
| Cifrado de nivel de columna proporcionado por Azure Data Services| Sí | Mediante [Always Encrypted](sql-database-always-encrypted.md). |
| Llamadas a API cifradas| Sí | Con HTTPS/SSL. |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración, como el control de versiones de configuración| Sin  | None |

## <a name="additional-security-controls-for-sql-database"></a>Controles de seguridad adicionales para SQL Database

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Preventivo: evaluación de vulnerabilidad | Sí | Consulte [El servicio de evaluación de vulnerabilidad de SQL le ayuda a identificar los puntos vulnerables de la base de datos](sql-vulnerability-assessment.md). |
| Preventivo: detección y clasificación de datos  | Sí | Consulte [Clasificación y detección de datos de Azure SQL Database y Azure SQL Data Warehouse](sql-database-data-discovery-and-classification.md). |
| Detección: detección de amenazas | Sí | Consulte [Advanced Threat Protection en Azure SQL Database](sql-database-threat-detection-overview.md). |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).
