---
title: 'Acceso condicional: Bloqueo del acceso por ubicación (Azure Active Directory)'
description: Cree una directiva de acceso condicional personalizada para bloquear el acceso a los recursos por ubicación de IP.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: b31fc3aed0d412646d9924e87170dffcd5145409
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575991"
---
# <a name="conditional-access-block-access-by-location"></a>Acceso condicional: Bloqueo del acceso por ubicación

Gracias a la condición de ubicación del acceso condicional, podrá controlar el acceso a las aplicaciones en la nube en función de la ubicación de red de un usuario. La condición de ubicación se usa normalmente para bloquear el acceso desde países de donde la organización sabe que no debe provenir el tráfico.

## <a name="define-locations"></a>Definición de ubicaciones

1. Inicie sesión en  **Azure Portal**  como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Acceso condicional**.
1. Elija **Nueva ubicación**.
1. Asigne un nombre a la ubicación.
1. Elija **Intervalos IP** si conoce los intervalos de direcciones IPv4 específicos accesibles externamente que constituyen esa ubicación, o seleccione **Países o regiones**.
   1. Proporcione los valores de **Intervalos IP** o seleccione el valor de **Países o regiones** de la ubicación especificada.
      * Si elige Países o regiones, puede optar por incluir áreas desconocidas.
1. Elija **Guardar**.

Para más información sobre la condición de ubicación en el acceso condicional, consulte el artículo [¿Qué es la condición de ubicación del acceso condicional de Azure Active Directory?](location-condition.md)

## <a name="create-a-conditional-access-policy"></a>Creación de una directiva de acceso condicional

1. Inicie sesión en  **Azure Portal**  como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Todos los usuarios**.
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Todas las aplicaciones en la nube** y, luego, **Listo**.
1. En **Condiciones** > **Ubicación**:
   1. Establezca **Configurar** en **Sí**.
   1. En **Incluir**, seleccione **Ubicaciones seleccionadas**
   1. Seleccione la ubicación bloqueada que ha creado para su organización.
   1. Haga clic en **Seleccionar** > **Listo** > **Listo**.
1. En **Controles de acceso** > **Bloquear**, elija **Seleccionar**.
1. Confirme la configuración y establezca **Habilitar directiva** en **Activado**.
1. Seleccione **Crear** para crear la directiva.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

[Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional](troubleshoot-conditional-access-what-if.md)
