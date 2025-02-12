---
title: Plantillas del Administrador de recursos de Azure
description: Describe cómo usar las plantillas de Azure Resource Manager para la implementación de recursos.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: tomfitz
ms.openlocfilehash: 96f140cfa5e6151ad53ca242d1fc87ba3397316e
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300150"
---
# <a name="azure-resource-manager-templates"></a>Plantillas del Administrador de recursos de Azure

Con el traslado a la nube, muchos equipos han adoptado métodos de desarrollo más ágiles. Estos equipos se iteran rápidamente. Necesitan implementar repetidamente sus soluciones en la nube y saber que su infraestructura se encuentra en un estado confiable. Dado que la infraestructura se ha convertido en parte del proceso iterativo, ha desaparecido la división entre las operaciones y el desarrollo. Por ello, los equipos necesitan administrar la infraestructura y el código de aplicación a través de un proceso unificado.

Para cumplir estos desafíos, puede automatizar las implementaciones y usar la práctica de infraestructura como código. En el código, debe definir la infraestructura que va a implementar. Así pues, el código de infraestructura se convierte en parte del proyecto. Al igual que el código de la aplicación, puede almacenar el código de infraestructura en un repositorio de origen y agregarle un número de versión. Cualquier miembro del equipo podrá ejecutar el código e implementar entornos similares.

Para implementar la infraestructura como código para las soluciones de Azure, use las plantillas de Azure Resource Manager. La plantilla es un archivo de notación de objetos JavaScript (JSON) que contiene la infraestructura y la configuración del proyecto. La plantilla usa sintaxis declarativa, lo que permite establecer lo que pretende implementar sin tener que escribir la secuencia de comandos de programación para crearla. En la plantilla se especifican los recursos que se van a implementar y las propiedades de esos recursos.

## <a name="why-choose-resource-manager-templates"></a>¿Por qué elegir plantillas de Resource Manager?

Si intenta decidir entre usar plantillas de Resource Manager o una de las demás infraestructuras como servicios de código, tenga en cuenta las siguientes ventajas del uso de plantillas:

* **Sintaxis declarativa**: Las plantillas de Resource Manager le permiten crear e implementar una infraestructura de Azure completa de forma declarativa. Por ejemplo, puede implementar no solo máquinas virtuales, sino también la infraestructura de red, los sistemas de almacenamiento y cualquier otro recurso que pueda necesitar.

* **Resultados repetibles**: Implemente repetidamente la infraestructura a lo largo del ciclo de vida del desarrollo y tenga la seguridad de que los recursos se implementan de forma coherente. Las plantillas son idempotentes, lo que significa que puede implementar la misma plantilla varias veces y obtener los mismos tipos de recursos en el mismo estado. Puede desarrollar una plantilla que represente el estado deseado, en lugar de desarrollar muchas plantillas independientes para representar las actualizaciones.

* **Orquestación**: No tiene que preocuparse por la complejidad de las operaciones de ordenación. Resource Manager se encarga de gestionar la implementación de recursos interdependientes para que se creen en el orden correcto. Cuando es posible, Resource Manager implementa los recursos en paralelo para que las implementaciones finalicen más rápido que las implementaciones en serie. La plantilla se implementa mediante un comando, en lugar de hacerlo con varios comandos imperativos.

   ![Comparación de la implementación de plantillas](./media/template-deployment-overview/template-processing.png)

* **Validación integrada**: La plantilla solo se implementa después de pasar la validación. Resource Manager se encarga de comprobar la plantilla antes de iniciar la implementación para asegurarse de que esta se realizará correctamente. Es menos probable que la implementación se detenga a medio acabar.

* **Archivos modulares**: Puede dividir las plantillas en componentes más pequeños y reutilizables y vincularlos en el momento de la implementación. También puede anidar una plantilla dentro de otras plantillas.

* **Cree cualquier recurso de Azure**: Puede usar inmediatamente los nuevos servicios y características de Azure en las plantillas. En cuanto un proveedor de recursos introduce nuevos recursos, puede implementarlos a través de plantillas. No tiene que esperar a que se actualicen las herramientas o los módulos antes de usar los nuevos servicios.

* **Implementaciones con seguimiento**: En Azure Portal puede revisar el historial de implementación y obtener información sobre la implementación de la plantilla. También puede ver la plantilla que se implementó, los valores de parámetro agregados y los valores de salida. Recuerde que no se realiza el seguimiento de otras infraestructuras como servicios de código a través del portal.

   ![Historial de implementación](./media/template-deployment-overview/deployment-history.png)

* **Directiva como código**: [Azure Policy](../governance/policy/overview.md) es un marco de directiva como código para automatizar la gobernanza. Si usa directivas de Azure, la corrección de la directiva se realiza en recursos no compatibles cuando se implementa mediante plantillas.

* **Planos técnicos de implementación**: Puede aprovechar las ventajas de los [Planos técnicos](../governance/blueprints/overview.md) proporcionados por Microsoft para cumplir los estándares de cumplimiento normativo. Estos planos técnicos incluyen plantillas precompiladas para distintas arquitecturas.

* **Integración de CI/CD**: Puede integrar plantillas en sus herramientas de integración e implementación continuas (CI/CD), que pueden automatizar las canalizaciones de versión para llevar a cabo actualizaciones de infraestructura y aplicaciones rápidas y confiables. Mediante la tarea de plantilla de Resource Manager y Azure DevOps puede usar Azure Pipelines para compilar e implementar proyectos de plantillas de Azure Resource Manager de manera continua. Para obtener más información, consulte [Proyecto de VS con canalizaciones](./vs-resource-groups-project-devops-pipelines.md) e [Integración continua con Azure Pipelines](./resource-manager-tutorial-use-azure-pipelines.md).

* **Código exportable**: Puede recuperar una plantilla de un grupo de recursos existente mediante la exportación del estado actual del grupo de recursos o la visualización de la plantilla de una implementación determinada. Una buena estrategia para aprender sobre la sintaxis de una plantilla es consultar la [plantilla exportada](export-template-portal.md).

* **Herramientas de creación**: Puede crear plantillas con [Visual Studio Code](resource-manager-tools-vs-code.md) y la extensión de la herramienta de plantillas. Podrá utilizar IntelliSense, el resaltado de sintaxis, la ayuda en línea y muchas otras funciones de lenguaje. Además de Visual Studio Code, también puede usar [Visual Studio](./vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="template-file"></a>Archivo de plantilla

Dentro de la plantilla, puede escribir [expresiones de plantilla](template-expressions.md) que aumentan las capacidades de JSON. Estas expresiones hacen uso de las [funciones](resource-group-template-functions.md) que proporciona Resource Manager.

La plantilla contiene las secciones siguientes:

* [Parámetros](template-parameters.md): proporcione valores durante la implementación que permitan usar la misma plantilla con entornos diferentes.

* [Variables](template-variables.md): defina los valores que se reutilizan en las plantillas. Se pueden crear a partir de valores de parámetro.

* [Funciones definidas por el usuario](template-user-defined-functions.md): cree funciones personalizadas que simplifiquen la plantilla.

* [Recursos](resource-group-authoring-templates.md#resources): especifique los recursos que se van a implementar.

* [Salidas](template-outputs.md): devuelva valores de los recursos implementados.

## <a name="template-deployment-process"></a>Proceso de implementación de plantillas

Cuando se implementa una plantilla, Resource Manager la convierte en operaciones de la API de REST. Por ejemplo, cuando Resource Manager recibe una plantilla con la siguiente definición de recursos:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

Convierte la definición en la siguiente operación de API de REST, que se envía al proveedor de recursos Microsoft.Storage:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "Storage"
}
```

## <a name="template-design"></a>Diseño de plantilla

La definición de plantillas y grupos de recursos depende únicamente de usted, al igual que la administración de la solución. Por ejemplo, puede implementar su aplicación de tres niveles a través de una única plantilla en un único grupo de recursos.

![plantilla de tres niveles](./media/template-deployment-overview/3-tier-template.png)

No obstante, no es necesario que defina toda la infraestructura en una sola plantilla. A menudo, tiene sentido dividir los requisitos de implementación en un conjunto de plantillas seleccionadas, específicas para un propósito. Estas plantillas se pueden reutilizar fácilmente para distintas soluciones. Para implementar una solución concreta, cree una plantilla maestra que vincule todas las plantillas necesarias. La imagen siguiente muestra cómo implementar una solución de tres niveles mediante una plantilla principal que incluye tres plantillas anidadas.

![plantilla de niveles anidada](./media/template-deployment-overview/nested-tiers-template.png)

Si desea que sus niveles tengan ciclos de vida independientes, puede implementar los tres niveles en grupos de recursos independientes. Observe que todavía se pueden vincular los recursos a los recursos de otros grupos.

![plantilla de niveles](./media/template-deployment-overview/tier-templates.png)

Para más información acerca de las plantillas anidadas, consulte [Uso de plantillas vinculadas con Azure Resource Manager](resource-group-linked-templates.md).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre las propiedades en los archivos de plantilla, consulte [Información sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](resource-group-authoring-templates.md).
* Para obtener información sobre la exportación de plantillas, consulte [Guía de inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md).
