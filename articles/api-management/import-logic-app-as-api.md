---
title: Importación de una aplicación lógica como una API desde Azure Portal | Microsoft Docs
description: En este tutorial se muestra cómo utilizar API Management (APIM) para importar una aplicación lógica como API.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: apimpm
ms.openlocfilehash: 57965b0326171f37d33aeae02080c8887c3acf23
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70072072"
---
# <a name="import-a-logic-app-as-an-api"></a>Importación de una aplicación lógica como API

Este artículo muestra cómo importar una aplicación lógica como una API y probar la API importada.

En este artículo, aprenderá a:

> [!div class="checklist"]
>
> -   Importación de una aplicación lógica como API
> -   Prueba de la API en Azure Portal
> -   Pruebe la API en el Portal para desarrolladores

## <a name="prerequisites"></a>Requisitos previos

-   Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)
-   Asegúrese de que hay alguna aplicación lógica en su suscripción que expone un punto de conexión HTTP. Para más información, consulte el artículo acerca del [desencadenamiento de flujos de trabajo con puntos de conexión HTTP](../logic-apps/logic-apps-http-endpoint.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importación y publicación de una API de back-end

1. Seleccione **API** en **API MANAGEMENT**.
2. Seleccione **Aplicación lógica** en la lista **Add a new API** (Agregar una nueva API).

    ![Aplicación lógica](./media/import-logic-app-as-api/logic-app-api.png)

3. Presione **Examinar** para ver la lista de aplicaciones Logic Apps con desencadenador HTTP en su suscripción. (Tenga en cuenta que las aplicaciones Logic Apps sin desencadenador HTTP no aparecerán en la lista).
4. Seleccione la aplicación. API Management busca el swagger asociado a la aplicación seleccionada, lo captura y lo importa.
5. Agregue un sufijo URL de API. El sufijo es un nombre que identifica esta API específica en esta instancia de API Management. Tiene que ser único en esta instancia de API Management.
6. Publique la API asociándola a un producto. En este caso, se usa el producto "_Unlimited_". Si desea que la API se publique y esté disponible para los desarrolladores, agréguela a un producto. Puede hacerlo durante la creación de la API o configurarla más adelante.

    Los productos son asociaciones de una o varias API. Puede incluir varias API y ofrecerlas a los desarrolladores mediante el portal para desarrolladores. En primer lugar, los desarrolladores deben suscribirse a un producto para acceder a la API. Al suscribirse, obtienen una clave de suscripción que funciona con cualquier API de ese producto. Si creó la instancia de API Management, ya es un administrador, así que de forma predeterminada está suscrito a todos los productos.

    De forma predeterminada, cada instancia de API Management incluye dos productos de ejemplo:

    - **Starter**
    - **Sin límite**

7. Seleccione **Crear**.

## <a name="test-the-imported-api-in-the-azure-portal"></a>Prueba de la API importada en Azure Portal

Se puede llamar a las operaciones directamente desde Azure Portal, lo que proporciona una forma cómoda de ver y probar las operaciones de una API.

1. Seleccione la API que creó en los pasos anteriores.
2. Presione la pestaña **Prueba**.
3. Seleccione alguna operación.

    La página muestra los campos de parámetros de consulta y los campos para los encabezados. Uno de los encabezados es "Ocp-Apim-Suscripción-Key", para la clave de suscripción del producto que está asociado a esta API. Si ha creado la instancia de API Management, significa que ya es administrador, por lo que la clave se rellena automáticamente.

4. Presione **Enviar**.

    Back-end responde con **200 Aceptar** y algunos datos.

## <a name="call-operation"></a>Llamada a una operación desde el portal para desarrolladores

También se pueden llamar a las operaciones desde **portal para desarrolladores** para probar las API.

1. Seleccione la API que creó en el paso "Importación y publicación de una API de back-end".
2. Presione **Portal para desarrolladores**.

    Se abre el sitio "Portal para desarrolladores".

3. Seleccione la **API** que ha creado.
4. Haga clic en la operación que desea probar.
5. Presione **Try it** (Probarlo).
6. Presione **Enviar**.

    Después de invocar una operación, el portal para desarrolladores mostrará el **estado de respuesta**, los **encabezados de respuesta** y el **contenido de respuesta**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
>Cada aplicación lógica tiene una operación **manual-invoke**. Si desea que la API contenga varias aplicaciones lógicas, para que no tenga colisión, debe cambiar el nombre de la función.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>
> [Transformación y protección de una API publicada](transform-api.md)
