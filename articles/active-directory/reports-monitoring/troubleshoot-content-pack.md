---
title: Solución de errores de los paquetes de contenido de los registros de actividad de Azure Active Directory | Microsoft Docs
description: Proporciona una lista de mensajes de error del paquete de contenido de la actividad de Azure Active Directory y los pasos para corregirlos.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 06/07/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e50f2b92318ada729ad8e3405af8403f31d7b6e
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129286"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Solución de errores de los paquetes de contenido de los registros de actividad de Azure Active Directory 

|  |
|--|
|Actualmente, el paquete de contenido de Power BI de Azure AD usa las Graph API de Azure AD para recuperar datos del inquilino de Azure AD. Como resultado, es posible que vea alguna discrepancia entre los datos disponibles en el paquete de contenido y los datos recuperados con la [API de Microsoft Graph para crear informes](concept-reporting-api.md). |
|  |

Cuando se trabaja con el paquete de contenido de Power BI para Azure Active Directory, es posible que se encuentre con los siguientes errores: 

- [Error al actualizar](troubleshoot-content-pack.md#refresh-failed) 
- [Error al actualizar las credenciales del origen de datos ](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [La importación de datos tarda demasiado](#data-import-is-too-slow) 

Este artículo proporciona información sobre las posibles causas y cómo corregir estos errores.
 
## <a name="refresh-failed"></a>Error al actualizar 
 
**¿Cómo aparece este error?** : correo electrónico desde Power BI o estado de error en el historial de actualización. 


| Causa | Solución |
| ---   | ---        |
| Los errores de actualización pueden producirse cuando se han restablecido las credenciales de los usuarios que se conectan al paquete de contenido, pero no se actualizan en la configuración de conexión del paquete de contenido. | En Power BI, busque el conjunto de datos correspondiente al panel de registros de actividad de Azure AD (**registros de actividad de Azure Active Directory**), elija programar la actualización y, luego, escriba sus credenciales de Azure AD. |
| Una actualización puede no realizarse debido a grandes conjuntos de datos. | Actualmente, el paquete de contenido de Azure AD con Power BI solo puede admitir conjuntos de datos pequeños (con menos de 500 000 filas), ya que hay ciertas limitaciones con respecto al tiempo de expiración en el servicio Power BI. Si se producen errores de limitación o si la actualización no se realiza debido a problemas de tiempo de expiración, puede deberse a que está intentando capturar un conjunto de datos grande. Reduzca el período en la consulta y vuelva a intentarlo.|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Error al actualizar las credenciales del origen de datos 
 
**¿Cómo aparece este error?** : en Power BI, cuando se conecta al paquete de contenido de registros de actividad de Azure AD. 

| Causa | Solución |
| ---   | ---        |
| El usuario que se conecta no es un administrador global, un lector de seguridad o un administrador de seguridad. | Use una cuenta que sea administrador global, lector de seguridad o administrador de seguridad para acceder a los paquetes de contenido. |
| El inquilino no es Premium o no tiene al menos un usuario con un archivo de licencia Premium. | [Abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 


## <a name="data-import-is-too-slow"></a>La importación de datos es demasiado lenta 
 
**¿Cómo aparece este error?** : en Power BI, después de conectar el paquete de contenido, el proceso de importación de datos comienza a preparar su panel para los registros de actividad de Azure AD. Verá el mensaje: **Importando datos...**  sin ningún progreso adicional.  

| Causa | Solución |
| ---   | ---        |
| Según el tamaño de su inquilino, este paso puede tardar desde algunos minutos hasta media hora. | Si el mensaje no cambia para mostrar el panel dentro de una hora, [abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Pasos siguientes

* [Instalación del paquete de contenido de Power BI para informes de Azure AD](quickstart-install-power-bi-content-pack.md).
* [Uso del paquete de contenido de Power BI para informes de Azure AD para visualizar los datos](howto-power-bi-content-pack.md)
* [Obtención de soporte técnico para Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md)
