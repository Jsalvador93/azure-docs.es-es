---
title: 'Conexión a sistemas de archivos locales: Azure Logic Apps'
description: Automatice tareas y flujos de trabajo que conectan a sistemas de archivos locales con el conector del sistema de archivos a través de la puerta de enlace de datos local y Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan, LADocs
ms.topic: article
ms.date: 01/13/2019
ms.openlocfilehash: 5a6a57fb05d59e70df13f6800c8fa7bf87df91c6
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295883"
---
# <a name="connect-to-on-premises-file-systems-with-azure-logic-apps"></a>Conexión a sistemas de archivos locales con Azure Logic Apps

Con el conector de sistema de archivos y Azure Logic Apps, puede crear tareas automatizadas y flujos de trabajo que crean y administran los archivos en un recurso compartido de archivos local, por ejemplo:  

- Crear, obtener, anexar, actualizar y eliminar archivos.
- Enumerar archivos en carpetas o carpetas raíz.
- Obtener contenido de los archivos y metadatos.

En este artículo se muestra cómo conectar a un sistema de archivos local como se describe en este escenario de ejemplo básico: copiar un archivo que se carga en Dropbox a un recurso compartido de archivos y, después, enviar un correo electrónico. Para conectarse de forma segura y tener acceso a sistemas locales, las aplicaciones lógicas utilizan la [puerta de enlace de datos local](../logic-apps/logic-apps-gateway-connection.md). Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) Para obtener información técnica específica del conector, consulte la [referencia sobre el conector de sistema de archivos](/connectors/filesystem/).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Antes de que pueda conectar las aplicaciones lógicas con sistemas locales como el servidor del sistema de archivos, tiene que [instalar y configurar una puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md). De este modo, cuando cree la conexión del sistema de archivos desde la aplicación lógica, puede especificar que se use esta instalación de puerta de enlace.

* Una [cuenta de Dropbox](https://www.dropbox.com/), que se puede registrar de forma gratuita. Las credenciales de la cuenta son necesarias para crear una conexión entre la aplicación lógica y la cuenta de Dropbox.

* Acceda al equipo que tiene el sistema de archivos que quiere usar. Por ejemplo, si instala la puerta de enlace de datos en el mismo equipo que el sistema de archivos, necesita las credenciales de la cuenta para ese equipo.

* Una cuenta de correo de un proveedor de correo compatible con Logic Apps, como Office 365 Outlook, Outlook.com o Gmail. En el caso de otros proveedores, [consulte la lista de conectores que se muestra aquí](https://docs.microsoft.com/connectors/). Esta aplicación lógica usa una cuenta de Office 365 Outlook. Si usa otra cuenta de correo electrónico, los pasos generales serán los mismos, pero su interfaz de usuario puede ser ligeramente distinta.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md). En este ejemplo, necesita una aplicación lógica en blanco.

## <a name="add-trigger"></a>Incorporación de un desencadenador

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el Diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. En el cuadro de búsqueda, escriba "dropbox" para el filtro. En la lista de desencadenadores, seleccione este desencadenador: **Cuando se crea un archivo**

   ![Selección del desencadenador de Dropbox](media/logic-apps-using-file-connector/select-dropbox-trigger.png)

1. Inicie sesión con sus credenciales de cuenta de Dropbox y autorice el acceso a los datos de Dropbox para Azure Logic Apps.

1. Proporcione la información necesaria para el desencadenador.

   ![Desencadenador de Dropbox](media/logic-apps-using-file-connector/dropbox-trigger.png)

## <a name="add-actions"></a>Incorporación de una acción

1. En el desencadenador, elija **Next step** (Paso siguiente). En el cuadro de búsqueda, escriba "sistema de archivos" como filtro. En la lista de acciones, seleccione esta acción: **Crear archivo**

   ![Selección del conector de sistema de archivos](media/logic-apps-using-file-connector/find-file-system-action.png)

1. Si todavía no tiene ninguna conexión al sistema de archivos, se le pedirá que la cree ahora.

   ![Crear conexión](media/logic-apps-using-file-connector/file-system-connection.png)

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   | -------- | -------- | ----- | ----------- |
   | **Nombre de la conexión** | Sí | <*connection-name*> | El nombre que quiere para la conexión |
   | **Carpeta raíz** | Sí | <*root-folder-name*> | La carpeta raíz para el sistema de archivos, por ejemplo, si ha instalado la puerta de enlace de datos local, como una carpeta local en el equipo donde está instalada la puerta de enlace de datos local, o la carpeta para un recurso compartido de red al que el equipo puede acceder. <p>Por ejemplo: `\\PublicShare\\DropboxFiles` <p>La carpeta raíz es la carpeta primaria principal, que se usa para las rutas de acceso relativas para todas las acciones relacionadas con archivos. |
   | **Tipo de autenticación** | Sin | <*auth-type*> | El tipo de autenticación que usa el sistema de archivos, por ejemplo, **Windows** |
   | **Nombre de usuario** | Sí | <*domain*>\\<*username*> | El nombre de usuario para el equipo donde tiene el sistema de archivos |
   | **Contraseña** | Sí | <*your-password*> | La contraseña para el equipo donde tiene el sistema de archivos |
   | **puerta de enlace** | Sí | <*installed-gateway-name*> | El nombre de la puerta de enlace instalada previamente |
   |||||

1. Cuando termine, seleccione **Crear**.

   Logic Apps configura y comprueba la conexión, asegurándose de que la conexión funciona correctamente. Si la conexión está configurada correctamente, aparecen las opciones de la acción que ha seleccionado anteriormente.

1. En la acción **Create file** (Crear archivo) proporcione los detalles para copiar archivos desde Dropbox a la carpeta raíz del recurso compartido de archivos local. Para agregar las salidas de los pasos anteriores, haga clic dentro de los cuadros y seleccione desde los campos disponibles cuando aparezca la lista de contenido dinámico.

   ![Acción Crear archivo](media/logic-apps-using-file-connector/create-file-filled.png)

1. Ahora, agregue una acción de Outlook que envíe un correo electrónico para que los usuarios adecuados tengan conocimiento del nuevo archivo. Especifique los destinatarios, el título y el cuerpo del correo electrónico. Para realizar pruebas, puede usar su propia dirección de correo electrónico.

   ![Acción Enviar correo electrónico](media/logic-apps-using-file-connector/send-email.png)

1. Guarde la aplicación lógica. Pruebe la aplicación cargando archivos en Dropbox.

   La aplicación lógica debe copiar el archivo en el recurso compartido local y enviar un correo electrónico a los destinatarios acerca del archivo copiado.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/fileconnector/) del conector.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda cómo [conectarse a datos locales](../logic-apps/logic-apps-gateway-connection.md) 
* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
