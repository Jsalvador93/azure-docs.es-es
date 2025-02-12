---
title: Implementación de recursos con la API de REST y plantilla | Microsoft Docs
description: Use Azure Resource Manager y la API REST de Resource Manager para implementar recursos en Azure. Los recursos se definen en una plantilla de Resource Manager.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: tomfitz
ms.openlocfilehash: 42f6ce96cf339e90ed0a0dcdbdb3f1b6924430e9
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206395"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Implementación de recursos con las plantillas de Resource Manager y la API de REST de Resource Manager

En este artículo se explica cómo usar la API de REST de Resource Manager con plantillas de Resource Manager para implementar sus recursos en Azure.  

Puede incluir la plantilla en el cuerpo de solicitud o vincularla a un archivo. Al usar un archivo, este puede ser un archivo local o un archivo externo disponible a través de un identificador URI. Cuando la plantilla se encuentra en una cuenta de almacenamiento, puede restringir el acceso a ella y proporcionar un token de firma de acceso compartido (SAS) durante la implementación.

## <a name="deployment-scope"></a>Ámbito de la implementación

La implementación puede tener como destino un grupo de administración, una suscripción de Azure o un grupo de recursos. En la mayoría de los casos, la implementación tendrá como destino un grupo de recursos. Use las implementaciones en un grupo de recursos o en una suscripción para aplicar directivas y asignaciones de roles en el ámbito especificado. También puede usar las implementaciones de la suscripción para crear un grupo de recursos e implementar recursos en él. Según el ámbito de la implementación, usará comandos diferentes.

Para implementar en un **grupo de recursos**, use [Deployments - Create](/rest/api/resources/deployments/createorupdate). La solicitud se envía a:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Para implementar en una **suscripción**, use [Deployments - Create At Subscription Scope](/rest/api/resources/deployments/createorupdateatsubscriptionscope). La solicitud se envía a:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Para implementar en un **grupo de administración**, use [Deployments - Create At Management Group Scope](/rest/api/resources/deployments/createorupdateatmanagementgroupscope). La solicitud se envía a:

```HTTP
PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Los ejemplos de este artículo usan las implementaciones del grupo de recursos. Para obtener más información sobre la implementación de suscripciones, vea [Creación de grupos de recursos y otros recursos en el nivel de suscripción](deploy-to-subscription.md).

## <a name="deploy-with-the-rest-api"></a>Implementación con la API de REST

1. Establezca los [encabezados y parámetros comunes](/rest/api/azure/), incluidos los tokens de autenticación.

1. Si no tiene un grupo de recursos existente, puede crear uno. Especifique el identificador de la suscripción, el nombre del nuevo grupo de recursos y la ubicación que necesita para la solución. Para obtener más información, consulte [Crear un grupo de recursos](/rest/api/resources/resourcegroups/createorupdate).

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2019-05-01
   ```

   Con un cuerpo de la solicitud como:

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. Valide la implementación antes de ejecutarla. Para ello, ejecute la operación de [validación de una implementación de plantilla](/rest/api/resources/deployments/validate). Al probar la implementación, proporcione los parámetros exactamente como lo haría al ejecutar la implementación (como se muestra en el paso siguiente).

1. Para implementar una plantilla, especifique el identificador de suscripción, el nombre del grupo de recursos y el nombre de la implementación en el URI de solicitud. 

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2019-05-01
   ```

   En el cuerpo de solicitud, proporcione un vínculo al archivo de plantilla y parámetros. Observe que el **modo** se establece en **Incremental**. Para ejecutar una implementación completa, establezca el **modo** en **Completo**. Tenga cuidado al usar este modo, ya que puede eliminar accidentalmente los recursos que no estén en la plantilla.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    Si desea registrar el contenido de la respuesta y el de la solicitud, o ambos, incluya **debugSetting** en la solicitud.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    Puede configurar la cuenta de almacenamiento para utilizar un token de firma de acceso compartido (SAS). Para obtener más información, consulte [Delegating Access with a Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature)(Delegación del acceso con una firma de acceso compartido).

1. En lugar de crear vínculos a archivos para la plantilla y los parámetros, puede incluirlos en el cuerpo de la solicitud. El ejemplo siguiente muestra el cuerpo de la solicitud con la plantilla y el parámetro en línea:

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2018-02-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. Para obtener el estado de la implementación de la plantilla, use [Deployments - Get](/rest/api/resources/deployments/get).

   ```HTTP
   GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

## <a name="redeploy-when-deployment-fails"></a>Nueva implementación cuando se produce un error en la implementación

Esta característica también es conocida como *reversión en caso de error*. Cuando se produce un error en una implementación, puede ejecutar automáticamente desde el historial de implementación una implementación anterior que sea correcta. Para especificar una nueva implementación, utilice la propiedad `onErrorDeployment` en el cuerpo de la solicitud. Esta funcionalidad es útil si tiene un estado correcto conocido para la implementación de la infraestructura y quiere volver a ese estado. Hay una serie de advertencias y restricciones:

- La reimplementación se ejecuta exactamente como se ejecutó previamente y con los mismos parámetros. Los parámetros no se pueden cambiar.
- La implementación anterior se ejecuta con el [modo completo](./deployment-modes.md#complete-mode). Los recursos no incluidos en la implementación anterior se eliminan, y se establecen las configuraciones del recurso al estado anterior. Asegúrese de que comprende perfectamente los [modos de implementación](./deployment-modes.md).
- La reimplementación solo afecta a los recursos, los cambios de datos no se ven afectados.
- Esta característica solo se admite en implementaciones del grupo de recursos, no en implementaciones en el nivel de suscripción. Para obtener más información sobre las implementaciones en el nivel de suscripción, vea [Creación de grupos de recursos y otros recursos en el nivel de suscripción](./deploy-to-subscription.md).

Para usar esta opción, las implementaciones deben tener nombres únicos para que se puedan identificar en el historial. Si no tienen nombres únicos, la implementación con error en cuestión podría sobrescribir la implementación anteriormente correcta en el historial. Solo se puede usar esta opción con las implementaciones de nivel de raíz. Las implementaciones de una plantilla anidada no están disponibles para volver a implementarse.

Para volver a implementar la última implementación correcta si se produce un error en la actual, use:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "LastSuccessful",
    }
  }
}
```

Para volver a implementar una implementación específica si se produce un error en la implementación actual, use:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "SpecificDeployment",
      "deploymentName": "<deploymentname>"
    }
  }
}
```

La implementación especificada debe haberse realizado correctamente.

## <a name="parameter-file"></a>Archivo de parámetros

Si utiliza un archivo de parámetros para pasar los valores de parámetro durante la implementación, tendrá que crear un archivo JSON con un formato similar al del ejemplo siguiente:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               },
               "secretName": "sqlAdminPassword"
            }
        }
   }
}
```

El tamaño del archivo de parámetros no puede ser superior a 64 KB.

Si necesita proporcionar un valor confidencial para un parámetro (por ejemplo, una contraseña), agregue ese valor a un almacén de claves. Recupere el almacén de claves durante la implementación, como se muestra en el ejemplo anterior. Para más información, consulte [Paso de valores seguros durante la implementación](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Pasos siguientes

- Para especificar cómo controlar los recursos que existen en el grupo de recursos, pero que no están definidos en la plantilla, consulte [Modos de implementación de Azure Resource Manager](deployment-modes.md).
- Para obtener información sobre el control de operaciones asincrónicas de REST, vea [Seguimiento de las operaciones asincrónicas de Azure](resource-manager-async-operations.md).
- Para obtener más información sobre las plantillas, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](resource-group-authoring-templates.md).

