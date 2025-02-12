---
title: Entidades de servicio para Azure Kubernetes Service (AKS)
description: Cree y administre una entidad de servicio de Azure Active Directory para un clúster en Azure Kubernetes Service (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mlearned
ms.openlocfilehash: 304b9dae9f3a1e134809d8959a96dc4e3ec0edd3
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615110"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Entidades de servicio con Azure Kubernetes Service (AKS)

Para interactuar con las API de Azure, un clúster de AKS requiere una [entidad de servicio de Azure Active Directory (AD)][aad-service-principal]. La entidad de servicio se necesita para crear y administrar dinámicamente otros recursos de Azure, como Azure Load Balancer o Container Registry.

En este artículo se muestra cómo crear y usar una entidad de servicio para los clústeres de AKS.

## <a name="before-you-begin"></a>Antes de empezar

Para crear una entidad de servicio de Azure AD, es preciso tener los permisos suficientes para registrar una aplicación en un inquilino de Azure AD y asignarle un rol en una suscripción. Si no tiene los permisos necesarios, es posible que tenga que pedir al administrador de Azure AD o de la suscripción que asigne los permisos necesarios o crear previamente una entidad de servicio para su uso con el clúster de AKS.

Si usa una entidad de servicio de otro inquilino de Azure AD, hay consideraciones adicionales en torno a los permisos disponibles al implementar el clúster. Puede que no tenga los permisos adecuados para leer y escribir datos del directorio. Para obtener más información, consulte [¿Cuáles son los permisos de usuario predeterminados en Azure Active Directory?][azure-ad-permissions]

También es preciso que esté instalada y configurada la versión 2.0.59 de la CLI de Azure u otra versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Creación y uso de una entidad de servicio automáticamente

Cuando crea un clúster de AKS en Azure Portal o mediante el comando [az aks create][az-aks-create], Azure puede generar automáticamente una entidad de servicio.

En el siguiente ejemplo de la CLI de Azure, no se especifica una entidad de servicio. En este escenario, la CLI de Azure crea un entidad de servicio para el clúster de AKS. Para completar esta operación correctamente, la cuenta debe tener los derechos apropiados para crear una entidad de servicio.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup
```

## <a name="manually-create-a-service-principal"></a>Creación manual de una entidad de servicio

Para crear manualmente una entidad de servicio con la CLI de Azure, use el comando [az ad sp create-for-rbac][az-ad-sp-create]. En el ejemplo siguiente, el parámetro `--skip-assignment` impide la asignación de asignaciones predeterminadas adicionales:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

La salida será similar al del ejemplo siguiente: Tome nota de sus valores `appId` y `password`. Se usan al crear un clúster de AKS en la sección siguiente.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2019-03-04-21-35-28",
  "name": "http://azure-cli-2019-03-04-21-35-28",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Especificación de una entidad de servicio para un clúster de AKS

Para usar una entidad de servicio existente al crear un clúster de AKS mediante el comando [az aks create][az-aks-create], use los parámetros `--service-principal` y `--client-secret` para especificar los valores `appId` y `password` a partir del resultado del comando [az ad sp create-for-rbac][az-ad-sp-create]:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

Si implementa un clúster de AKS mediante Azure Portal, en la página *autenticación* del cuadro de diálogo **Creación de un clúster de Kubernetes**, elija **Configurar la entidad de servicio**. Seleccione **Usar existente** y especifique los valores siguientes:

- **Id. de cliente de la entidad de servicio** es su *appId*
- **Secreto del cliente de la entidad de servicio** es el valor de *contraseña*

![Imagen de la exploración hasta Azure Vote](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="delegate-access-to-other-azure-resources"></a>Delegación del acceso a otros recursos de Azure

La entidad de servicio del clúster de AKS se puede utilizar para acceder a otros recursos. Por ejemplo, si quiere implementar el clúster de AKS en una subred de red virtual de Azure o conectarse a Azure Container Registry (ACR), debe delegar el acceso a estos recursos a la entidad de servicio.

Para delegar permisos, cree una asignación de roles mediante el comando [az role assignment create][az-role-assignment-create]. Asigne el elemento `appId` a un ámbito determinado, como un grupo de recursos o un recurso de red virtual. Un rol define qué permisos tiene la entidad de servicio en el recurso, como se muestra en el ejemplo siguiente:

```azurecli
az role assignment create --assignee <appId> --scope <resourceScope> --role Contributor
```

El elemento `--scope` de un recurso debe ser un identificador de recurso completo, como */subscriptions/\<guid\>/resourceGroups/myResourceGroup* o */subscriptions/\<guid \>/resourceGroups/myResourceGroupVnet/providers/Microsoft.Network/virtualNetworks/myVnet*

Las secciones siguientes detallan las delegaciones comunes que es posible que deba realizar.

### <a name="azure-container-registry"></a>Azure Container Registry

Si usa Azure Container Registry (ACR) como el almacén de imágenes de contenedor, deberá conceder permisos para que el clúster de AKS pueda leer y extraer imágenes. La entidad de servicio del clúster de AKS debe tener delegado el rol de *lector* en el registro. Para consultar los pasos detallados, vea [Concesión a AKS del acceso a ACR][aks-to-acr].

### <a name="networking"></a>Redes

Puede usar redes avanzadas en las que la red virtual y la subred o las direcciones IP públicas se encuentran en otro grupo de recursos. Asigne uno de los siguientes conjuntos de permisos de rol:

- Cree un [rol personalizado][rbac-custom-role] y defina los siguientes permisos de rol:
  - *Microsoft.Network/virtualNetworks/subnets/join/action*
  - *Microsoft.Network/virtualNetworks/subnets/read*
  - *Microsoft.Network/virtualNetworks/subnets/write*
  - *Microsoft.Network/publicIPAddresses/join/action*
  - *Microsoft.Network/publicIPAddresses/read*
  - *Microsoft.Network/publicIPAddresses/write*
- O bien, asigne el rol integrado [Colaborador de la red][rbac-network-contributor] en la subred dentro de la red virtual.

### <a name="storage"></a>Storage

Es posible que necesite acceder a los recursos de disco existentes en otro grupo de recursos. Asigne uno de los siguientes conjuntos de permisos de rol:

- Cree un [rol personalizado][rbac-custom-role] y defina los siguientes permisos de rol:
  - *Microsoft.Compute/disks/read*
  - *Microsoft.Compute/disks/write*
- O bien, asigne el rol integrado [Colaborador de la cuenta de almacenamiento][rbac-storage-contributor] en el grupo de recursos.

### <a name="azure-container-instances"></a>Azure Container Instances

Si usa Virtual Kubelet para la integración con AKS y elige ejecutar Azure Container Instances (ACI) en un grupo de recursos separado del clúster de AKS, la entidad de servicio AKS debe tener concedidos los permisos de *Colaborador* en el grupo de recursos de ACI.

## <a name="additional-considerations"></a>Consideraciones adicionales

Cuando use entidades de servicio de AKS y Azure AD, tenga en cuenta lo siguiente.

- La entidad de servicio para Kubernetes forma parte de la configuración del clúster. Sin embargo, no use la identidad para implementar el clúster.
- De forma predeterminada, las credenciales de la entidad de servicio son válidas durante un año. Puede [actualizar o rotar las credenciales de la entidad de servicio][update-credentials] en cualquier momento.
- Cada entidad de servicio está asociada a una aplicación de Azure AD. La entidad de servicio de un clúster de Kubernetes puede asociarse con cualquier nombre de aplicación de Azure AD válido (por ejemplo, *https://www.contoso.org/example* ). La dirección URL de la aplicación no tiene por qué ser un punto de conexión real.
- Al especificar el **identificador de cliente**, utilice el valor de `appId`.
- En las VM del nodo de agente del clúster de Kubernetes, las credenciales de la entidad de servicio se almacenan en el archivo `/etc/kubernetes/azure.json`.
- Cuando use el comando [az aks create][az-aks-create] para generar la entidad de servicio automáticamente, sus credenciales se escriben en el archivo `~/.azure/aksServicePrincipal.json` de la máquina que se usa para ejecutar el comando.
- Al eliminar un clúster de AKS creado mediante [az aks create][az-aks-create], no se elimina la entidad de servicio que se ha creado automáticamente.
    - Para eliminar la entidad de servicio, consulte el clúster *servicePrincipalProfile.clientId* y después elimínelo con [az ad app delete][az-ad-app-delete]. Reemplace los nombres de clúster y del grupo de recursos siguientes con los suyos propios.

        ```azurecli
        az ad sp delete --id $(az aks show -g myResourceGroup -n myAKSCluster --query servicePrincipalProfile.clientId -o tsv)
        ```

## <a name="troubleshoot"></a>Solución de problemas

La CLI de Azure almacena en caché las credenciales de la entidad de servicio para un clúster de AKS. Si estas credenciales han expirado, encontrará errores al implementar clústeres de AKS. El siguiente mensaje de error al ejecutar [az aks create][az-aks-create] puede indicar un problema con las credenciales de la entidad de servicio almacenadas en caché:

```console
Operation failed with status: 'Bad Request'.
Details: The credentials in ServicePrincipalProfile were invalid. Please see https://aka.ms/aks-sp-help for more details.
(Details: adal: Refresh request failed. Status Code = '401'.
```

Compruebe la antigüedad del archivo de credenciales con el comando siguiente:

```console
ls -la $HOME/.azure/aksServicePrincipal.json
```

El tiempo de expiración predeterminado para las credenciales de la entidad de servicio es de un año. Si el archivo *aksServicePrincipal.json* tiene más de un año, elimine el archivo e intente volver a implementar un clúster de AKS.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las entidades de servicio de Azure Active Directory, consulte [Objetos de aplicación y de entidad de servicio][service-principal].

Para obtener información sobre cómo actualizar las credenciales, vea [Actualizar o rotar las credenciales de una entidad de servicio en AKS][update-credentials].

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create
[rbac-network-contributor]: ../role-based-access-control/built-in-roles.md#network-contributor
[rbac-custom-role]: ../role-based-access-control/custom-roles.md
[rbac-storage-contributor]: ../role-based-access-control/built-in-roles.md#storage-account-contributor
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[aks-to-acr]: ../container-registry/container-registry-auth-aks.md?toc=%2fazure%2faks%2ftoc.json#grant-aks-access-to-acr
[update-credentials]: update-credentials.md
[azure-ad-permissions]: ../active-directory/fundamentals/users-default-permissions.md
