---
title: 'PowerShell: Habilitar TDE con BYOK: Instancia administrada de Azure SQL Database| Microsoft Docs'
description: Aprenda a configurar una Instancia administrada de Azure SQL Database para comenzar a usar Cifrado de datos transparente (TDE) con BYOK para cifrado en reposo mediante PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 04/19/2019
ms.openlocfilehash: d8f0f4a8e603a9040d166b00682077cff23abd8d
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569703"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault-preview"></a>Administración del cifrado de datos transparente en una instancia administrada con la clave propia desde Azure Key Vault (versión preliminar)

Este ejemplo de script de PowerShell configura el cifrado de datos transparente (TDE) en el escenario Bring Your Own Key (versión preliminar) para Instancia administrada de Azure SQL Database con una clave de Azure Key Vault. Para más información sobre TDE con compatibilidad con Bring Your Own Key (BYOK), vea [BYOK de TDE con Azure SQL](../transparent-data-encryption-byok-azure-sql.md).

## <a name="prerequisites"></a>Requisitos previos

- Una instancia administrada existente. Consulte [Use PowerShell para crear una instancia administrada de Azure SQL Database](sql-database-create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Al usar tanto PowerShell localmente como Azure Cloud Shell se requiere AZ PowerShell 1.1.1-preview o una versión preliminar posterior. Si tiene que actualizar, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps) o ejecute el siguiente script de ejemplo para instalar el módulo.

`Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease -Force`

Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

## <a name="sample-scripts"></a>Scripts de ejemplo

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Encontrará más ejemplos de scripts de PowerShell de SQL Database en los [scripts de PowerShell de Azure SQL Database](../sql-database-powershell-samples.md).
