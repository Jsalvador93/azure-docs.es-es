---
title: 'Introducción a Azure Key Vault: Azure Key Vault | Microsoft Docs'
description: Azure Key Vault es un servicio en la nube que funciona como un almacén de secretos seguro.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: overview
ms.custom: mvc
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: e814c9f7859fb18280c3a9ffd72bd4053a4895ad
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70883223"
---
# <a name="what-is-azure-key-vault"></a>¿Qué es Azure Key Vault?

Azure Key Vault ayuda a solucionar los problemas siguientes:

- **Administración de secretos**: Azure Key Vault se puede utilizar para almacenar de forma segura y controlar de manera estricta el acceso a los tokens, contraseñas, certificados, claves de API y otros secretos.
- **Administración de claves**: también se puede usar Azure Key Vault como una solución de administración de claves. Azure Key Vault facilita la creación y control de las claves de cifrado utilizadas para cifrar los datos. 
- **Administración de certificados**: Azure Key Vault también es un servicio que le permite aprovisionar, administrar e implementar fácilmente certificados públicos y privados de la Capa de sockets seguros y de Seguridad de la capa de transporte (SSL/TLS) para su uso con Azure y sus recursos internos conectados. 
- **Almacenamiento de secretos basado en módulos de seguridad de hardware**: las claves y secretos se pueden proteger mediante software o mediante dispositivos HSM validados por FIPS 140-2 de nivel 2

## <a name="why-use-azure-key-vault"></a>Motivos para usar Azure Key Vault

### <a name="centralize-application-secrets"></a>Centralización de secretos de aplicación

La centralización del almacenamiento de los secretos de aplicación le permite controlar su distribución. Key Vault reduce en gran medida las posibilidades de que se puedan filtrar por accidente los secretos. Cuando usan Key Vault, los desarrolladores de aplicaciones ya no tienen que almacenar la información de seguridad en su aplicación. No tener que almacenar información de seguridad en las aplicaciones elimina la necesidad de que esta información sea parte del código. Por ejemplo, puede que una aplicación necesite conectarse a una base de datos. En lugar de almacenar la cadena de conexión en el código de la aplicación, puede almacenarla de forma segura en Key Vault.

Las aplicaciones pueden acceder de manera protegida a la información que necesitan a través de URI. Estos URI permiten que las aplicaciones recuperen versiones específicas de un secreto. No hay ninguna necesidad de escribir código personalizado para proteger información secreta almacenada en Key Vault.

### <a name="securely-store-secrets-and-keys"></a>Almacenamiento seguro de secretos y claves

Los secretos y las claves se protegen mediante Azure, para lo que se usan algoritmos estándar del sector, longitudes de clave y módulos de seguridad de hardware (HSM). Los módulos de seguridad de hardware se validan mediante los Estándares federales de procesamiento de información (FIPS) 140-2 de nivel 2.

El acceso a un almacén de claves requiere una autorización y autenticación correctas antes de que un autor de llamada (usuario o aplicación) pueda obtener acceso. La autenticación establece la identidad del autor de la llamada, mientras que la autorización determina las operaciones que puede realizar.

La autenticación se realiza a través de Azure Active Directory. La autorización puede realizarse mediante el control de acceso basado en rol (RBAC) o la directiva de acceso de Key Vault. Se usa el control de acceso basado en rol cuando se trata de la administración de los almacenes y la directiva de acceso de Key Vault cuando se intenta acceder a los datos almacenados en un almacén.

Las instancias de Azure Key Vault se pueden proteger mediante software o con módulos de seguridad de hardware (HSM). En aquellos casos en los que necesita obtener seguridad adicional, puede importar o generar claves en módulos de seguridad de hardware (HSM) que no se salen nunca del límite de los HSM. Microsoft utiliza los módulos de seguridad de hardware de nCipher. Puede usar las herramientas de nCipher para mover una clave desde el módulo de seguridad de hardware a Azure Key Vault.

Además, Azure Key Vault está diseñado de modo que Microsoft no pueda ver ni extraer sus datos.

### <a name="monitor-access-and-use"></a>Supervisión del acceso y uso

Una vez que ha creado un par de almacenes de claves, querrá supervisar cómo y cuándo se accede a sus claves y secretos. Para supervisar la actividad, puede habilitar el registro de los almacenes. Puede configurar Azure Key Vault para:

- Archivar en una cuenta de almacenamiento.
- Transmitir a un centro de eventos.
- Envíe los registros a los registros de Azure Monitor.

Tiene el control sobre los registros y puede protegerlos de forma segura restringiendo el acceso y, además, puede eliminar los registros que ya no necesita.

### <a name="simplified-administration-of-application-secrets"></a>Administración simplificada de secretos de aplicación

Al almacenar datos importantes, debe realizar varios pasos. Se debe proteger la información de seguridad, esta debe seguir un ciclo de vida y debe tener alta disponibilidad. Azure Key Vault simplifica el proceso de cumplimiento de estos requisitos mediante:

- La eliminación de la necesidad de poseer conocimientos internos sobre los módulos de seguridad de hardware (HSM).
- El escalado vertical en un breve plazo de tiempo para adaptarse a los picos de uso de la organización.
- La replicación del contenido de una instancia de Key Vault de una región en una región secundaria. La replicación de datos garantiza la alta disponibilidad y elimina la necesidad de intervención del administrador para desencadenar la conmutación por error.
- La disposición de opciones estándar de administración de Azure a través del portal, la CLI de Azure y PowerShell.
- La automatización de determinadas tareas de los certificados que adquiere de entidades de certificación públicas, como la inscripción y la renovación.

Además, Azure Key Vault le permite segregar los secretos de aplicación. Las aplicaciones solo pueden acceder al almacén para el que tengan permiso, y se puede limitar el acceso para realizar únicamente operaciones específicas. Puede crear una instancia de Azure Key Vault por aplicación y restringir los secretos almacenados en un almacén de claves a una aplicación y a un equipo de desarrolladores concretos.

### <a name="integrate-with-other-azure-services"></a>Integración con otros servicios de Azure

Como almacén seguro de Azure, Key Vault se ha usado para simplificar escenarios como:
-  [Azure Disk Encryption](../security/azure-security-disk-encryption.md)
-  La funcionalidad de [cifrado siempre]( https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) de SQL server y Azure SQL Database
- [Azure App Service]( https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site). 

Key Vault se puede integrar con cuentas de almacenamiento e instancias de Event Hubs y Log Analytics.

## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido: Creación de una instancia de Azure Key Vault mediante la CLI](quick-create-cli.md)
- [Configuración de una aplicación web de Azure para que lea un secreto desde el almacén de claves](tutorial-web-application-keyvault.md)
