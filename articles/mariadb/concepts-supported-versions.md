---
title: Versiones admitidas de Azure Database for MariaDB
description: Se describen las versiones admitidas de Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: e5d1dbc8c212d4cdefb12fb740a454324d3adfa1
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70962916"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Versiones de servidores de Azure Database for MariaDB admitidas

Azure Database for MariaDB se ha desarrollado a partir del [servidor MariaDB](https://downloads.mariadb.org/) de origen abierto, mediante el motor InnoDB. 

MariaDB usa el esquema de nomenclatura X.Y.Z. X es la versión principal, Y es la versión secundaria y Z es la versión de revisión.

> [!NOTE]
> En el servicio, se usa una puerta de enlace para redirigir las conexiones a las instancias de servidor. Una vez establecida la conexión, el cliente de MySQL muestra la versión de MariaDB establecida en la puerta de enlace, no la versión real que se ejecuta en la instancia del servidor MariaDB. Para determinar la versión de la instancia del servidor MariaDB, use el comando `SELECT VERSION();`.

Azure Database for MariaDB actualmente admite las siguientes versiones:

## <a name="mariadb-version-102"></a>MariaDB versión 10.2

Versión de revisión: 10.2.25

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/library/mariadb-10225-release-notes/) para más información sobre las mejoras y correcciones de esta versión.

## <a name="mariadb-version-103"></a>MariaDB versión 10.3

Versión de revisión: 10.3.16

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/library/mariadb-10316-release-notes/) para más información sobre las mejoras y correcciones de esta versión.

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones
El servicio administra automáticamente las actualizaciones de las actualizaciones de revisión. Por ejemplo, 10.2.21 a 10.2.23.  

Actualmente, no se admiten las actualizaciones de versiones principales y secundarias. Por ejemplo, no se admite la actualización de MariaDB 10.2 a MariaDB 10.3. Si quiere actualizar de la versión 10.2 a la 10.3, realice un [volcado y restáurelo](./howto-migrate-dump-restore.md) en un servidor que se haya creado con la nueva versión del motor.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre las cuotas y las limitaciones aplicables a recursos específicos en función de su **nivel de servicio**, consulte [Niveles de servicio](./concepts-pricing-tiers.md).
