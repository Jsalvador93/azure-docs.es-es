---
title: Habilitar aplicaciones multiinquilino con Azure Digital Twins | Microsoft Docs
description: Cómo configurar aplicaciones multiinquilino de Azure Active Directory para Azure Digital Twins.
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: mavoge
ms.openlocfilehash: 2ee3681640f68839c32e2963b34d5547abb6943b
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976889"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Habilitar aplicaciones multiinquilino con Azure Digital Twins

Los desarrolladores de soluciones que se basan en Azure Digital Twins pueden tener que admitir varios clientes con un único servicio o solución. De hecho, las aplicaciones *multiinquilino* se encuentran entre las configuraciones más comunes de Azure Digital Twins.

En este documento se describe cómo configurar una aplicación de Azure Digital Twins para admitir varios inquilinos y clientes de Azure Active Directory.

## <a name="multitenancy"></a>Multiinquilino

Un recurso *multiinquilino* es una instancia aprovisionada única que admite varios clientes. Cada cliente tiene sus propios privilegios y datos independientes. La experiencia de cada cliente queda aislada de la de los demás por lo que su "vista" de la aplicación es distinta.

Para más información sobre la arquitectura multiinquilino, lea [Aplicaciones multiempresa en Azure](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications).

## <a name="problem-scenario"></a>Escenario del problema

En este escenario, imagine un desarrollador que crea una solución de Azure Digital Twins (**DESARROLLADOR**) y un cliente que usa esa solución (**CLIENTE**):

- El **DESARROLLADOR** tiene una suscripción a Azure con un inquilino de Azure Active Directory.
- El **DESARROLLADOR** implementa una instancia de Azure Digital Twins en su suscripción de Azure. Azure Active Directory creó automáticamente una entidad de servicio en el inquilino de Azure Active Directory del **DESAROLLADOR**.
- Los usuarios del inquilino de Azure Active Directory del **DESARROLLADOR** pueden entonces [adquirir tokens de OAuth 2.0](./security-authenticating-apis.md) del servicio Azure Digital Twins.
- El **DESARROLLADOR** crea ahora una aplicación móvil que se integra directamente con la API de administración de Azure Digital Twins.
- El **DESARROLLADOR** permite al **CLIENTE** usar la aplicación móvil.
- El **CLIENTE** debe estar autorizado para usar la API de administración de Azure Digital Twins dentro de la aplicación del **DESARROLLADOR**.

El problema:

- Cuando el **CLIENTE** inicia sesión en la aplicación del **DESARROLLADOR**, la aplicación no puede adquirir tokens para que los usuarios del **CLIENTE** se autentiquen con las API de administración de Azure Digital Twins.
- Se emite una excepción en Azure Active Directory que indica que no se reconoce Azure Digital Twins en el directorio del **CLIENTE**.

## <a name="problem-solution"></a>Solución del problema

Para resolver el problema del escenario anterior, debe realizar las siguientes acciones a fin de crear una entidad de servicio de Azure Digital Twins dentro del inquilino de Azure Active Directory del **CLIENTE**:

- Si el **CLIENTE** no tiene aún una suscripción a Azure con un inquilino de Azure Active Directory:

  - El administrador del inquilino de Azure Active Directory del **CLIENTE** debe adquirir una [suscripción a Azure de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - El administrador del inquilino de Azure Active Directory del **CLIENTE** debe [vincular su inquilino con la nueva suscripción](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity).

- En [Azure Portal](https://portal.azure.com), el inquilino de Azure Active Directory del **CLIENTE** realiza los siguientes pasos:

  1. Abra la página de **Suscripciones**.
  1. Seleccione la suscripción correspondiente al inquilino de Azure Active Directory que se usará en la aplicación del **DESARROLLADOR**.

     ![Suscripciones de Azure Active Directory][1]

  1. Seleccione **Proveedores de recursos**.
  1. Busque **Microsoft.IoTSpaces**.
  1. Seleccione **Registrar**.

     ![Proveedores de recursos de Azure Active Directory][2]
  
## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo usar funciones definidas por el usuario con Azure Digital Twins, lea [Creación de funciones definidas por el usuario en Azure Digital Twins](./how-to-user-defined-functions.md).

- Para información sobre cómo usar el control de acceso basado en rol para proteger aún más la aplicación con las asignaciones de roles, lea [Crear y administrar el control de acceso basado en rol de Azure Digital Twins](./security-create-manage-role-assignments.md).

<!-- Images -->
[1]: media/multitenant/ad-subscriptions.png
[2]: media/multitenant/ad-resource-providers.png
