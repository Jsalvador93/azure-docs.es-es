---
title: Conexión de datos de Symantec AWS a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Symantec AWS con Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 46ad8f6478e632cd57c009700de182993ce01be1
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240859"
---
# <a name="connect-azure-sentinel-to-aws-cloudtrail"></a>Conectar Azure Sentinel con AWS CloudTrail

Utilice el conector de AWS para transmitir todos los eventos de AWS CloudTrail a Azure Sentinel. Este proceso de conexión delega el acceso de Azure Sentinel en los registros de recursos de AWS, lo que crea una relación de confianza entre AWS CloudTrail y Azure Sentinel. Para ello, en AWS se crea un rol que concede a Azure Sentinel el permiso necesario para acceder a los registros de AWS.

## <a name="prerequisites"></a>Requisitos previos

Debe tener permiso de escritura en el área de trabajo de Azure Sentinel.

> [!NOTE]
> Azure Sentinel recopila eventos de CloudTrail de todas las regiones. Se recomienda no transmitir eventos de una región a otra.

## <a name="connect-aws"></a>Conexión de AWS 


1. En Azure Sentinel, haga clic en **Data connectors** (Conectores de datos), y seleccione la línea **Amazon Web Services** en la tabla y, en el panel de AWS situado a la derecha, haga clic en **Open connector page** (Abrir la página del conector).

1. Siga las instrucciones que aparecen en **Configuration** (Configuración) mediante los pasos siguientes.
 
1.  En la consola de Amazon Web Services, en **Security, Identity & Compliance** (Seguridad, identidad y cumplimiento), haga clic en **IAM**.

    ![AWS1](./media/connect-aws/aws-1.png)

1.  Haga clic en **Roles** y en **Create role** (Crear rol).

    ![AWS2](./media/connect-aws/aws-2.png)

1.  Seleccione **Another AWS account** (Otra cuenta de AWS). En el campo **Account ID** (Id. de cuenta), especifique el **identificador de la cuenta Microsoft** (**123412341234**) que encontrará en la página del conector de AWS del portal de Azure Sentinel.

    ![AWS3](./media/connect-aws/aws-3.png)

1.  Asegúrese de que la opción **Require External ID** (Requerir id. externo) esté seleccionada y escriba el identificador externo (identificador del área de trabajo), que encontrará en la página del conector de AWS del portal de Azure Sentinel.

    ![AWS4](./media/connect-aws/aws-4.png)

1.  En **Attach permissions policy** (Asociar directiva de permisos), seleccione**AWSCloudTrailReadOnlyAccess**.

    ![AWS5](./media/connect-aws/aws-5.png)

1.  Escriba una etiqueta (opcional).

    ![AWS6](./media/connect-aws/aws-6.png)

1.  A continuación, especifique el **nombre del rol** y haga clic en el botón **Create role** (Crear rol).

    ![AWS7](./media/connect-aws/aws-7.png)

1.  En la lista Roles, seleccione el rol que ha creado.

    ![AWS8](./media/connect-aws/aws-8.png)

1.  Copie el valor de **ARN de rol**. En el portal de Azure Sentinel, en la pantalla del conector de Amazon Web Services, pegue el valor en el campo **Role to add** (Rol que se va a agregar) y haga clic en **Add** (Agregar).

    ![AWS9](./media/connect-aws/aws-9.png)

1. Para usar el esquema correspondiente de Log Analytics con los eventos de AWS, busque **AWSCloudTrail**.



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar AWS CloudTrail con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

