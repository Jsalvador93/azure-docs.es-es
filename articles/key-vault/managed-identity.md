---
title: Uso de identidades administradas asignadas por el sistema de aplicaciones de App Service para acceder a Azure Key Vault
description: Descubra cómo crear una identidad administrada para aplicaciones de App Service y cómo usarla para acceder a Azure Key Vault.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 8ac6f9be80d31804089ae2589998079dc7df66b3
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71004181"
---
# <a name="use-an-app-service-managed-identity-to-access-azure-key-vault"></a>Uso de identidades administradas de App Service para acceder a Azure Key Vault 

En este artículo se describe cómo crear una identidad administrada para aplicaciones de App Service y cómo usarla para acceder a Azure Key Vault. Para aplicaciones hospedadas en máquinas virtuales de Azure, consulte [Uso de las identidades administradas asignadas por el sistema de una máquina virtual Windows para acceder a Azure Key Vault](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-nonaad.md). 

Una identidad administrada de Azure Active Directory permite a la aplicación acceder fácilmente a otros recursos protegidos por Azure AD. La identidad está administrada por la plataforma Azure y no requiere que aprovisione o rote los secretos. Para más información sobre las identidades administradas en Azure AD, consulte [Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Requisitos previos 

Para completar esta guía, necesitará los recursos siguientes: 

- Un almacén de claves. Puede usar un almacén de claves existente o crear uno nuevo siguiendo los pasos de uno de estos artículos de inicio rápido:
   - [Creación de un almacén de claves con la CLI de Azure](quick-create-cli.md)
   - [Creación de un almacén de claves con Azure PowerShell](quick-create-powershell.md)
   - [Creación de un almacén de claves con Azure Portal](quick-create-portal.md)
- Una aplicación de App Service existente a la que conceder acceso al almacén de claves. Puede crear una rápidamente siguiendo los pasos descritos en la [documentación de App Service](../app-service/overview.md)/.


## <a name="adding-a-system-assigned-identity"></a>Adición de una identidad asignada por el sistema 

En primer lugar, debe agregar una identidad asignada por el sistema a una aplicación. 
 
### <a name="azure-portal"></a>Portal de Azure 

Para configurar una identidad administrada en el portal, primero creará una aplicación como lo hace normalmente y, a continuación, habilitará la característica. 

1. Si utiliza una aplicación de función, vaya a **Características de la plataforma**. Para otros tipos de aplicación, desplácese hacia abajo hasta el grupo **Configuración** en el panel de navegación izquierdo. 

1. Seleccione **Identidad administrada**. 

1. En la pestaña **Asignado por el sistema**, cambie **Estado** a **Activado**. Haga clic en **Save**(Guardar). 

    ![](./media/managed-identity-system-assigned.png)

### <a name="azure-cli"></a>CLI de Azure

Para realizar este inicio rápido es necesaria la versión 2.0.4 o superior de la CLI de Azure. Ejecute `az --version` para buscar la versión actual. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest). 

Para iniciar sesión con la CLI de Azure, use el comando [az login](/cli/azure/reference-index?view=azure-cli-latest#az-login):

```azurecli-interactive
az login
```

Para más información sobre las opciones de inicio de sesión con la CLI de Azure, consulte [Inicio de sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest). 

Para crear la identidad de esta aplicación, use el comando [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) o el comando [az functionapp identity assign](/cli/azure/functionapp/identity?view=azure-cli-latest#az-functionapp-identity-assign) de la CLI de Azure:


```azurecli-interactive
az webapp identity assign --name myApp --resource-group myResourceGroup
```

```azurecli-interactive
az functionapp identity assign --name myApp --resource-group myResourceGroup
```

Anote `PrincipalId`, que se necesitará en la siguiente sección.

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```
## <a name="grant-your-app-access-to-key-vault"></a>Concesión a la aplicación de acceso a Key Vault 

### <a name="azure-portal"></a>Portal de Azure

1.  Navegue al recurso de Key Vault. 

1.  Seleccione **Directivas de acceso** y haga clic en **Agregar directiva de acceso**. 

1.  En **Permisos de secretos**, seleccione **Obtener, Enumerar**. 

1.  Elija **Seleccionar la entidad de seguridad** y, en el campo de búsqueda, escriba el nombre de la aplicación.  Seleccione la aplicación en la lista de resultados y haga clic en **Seleccionar**. 

1.  Haga clic en **Agregar** para terminar de agregar la nueva directiva de acceso.

    ![](./media/managed-identity-access-policy.png)

### <a name="azure-cli"></a>CLI de Azure

Para conceder a la aplicación acceso al almacén de claves, use el comando de la CLI de Azure [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy), proporcionando el parámetro **ObjectId** con el valor de **principalId* que anotó anteriormente.

```azurecli-interactive
az keyvault set-policy --name myKeyVault --object-id <PrincipalId> --secret-permissions get list 
```

## <a name="next-steps"></a>Pasos siguientes

- Lea una [introducción a Azure Key Vault](key-vault-overview.md).
- Consulte la [guía del desarrollador de Azure Key Vault](key-vault-developers-guide.md).
- Consulte sobre las [claves, secretos y certificados](about-keys-secrets-and-certificates.md).
- Consulte los [procedimientos recomendados de Azure Key Vault](key-vault-best-practices.md).
