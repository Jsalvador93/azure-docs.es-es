---
title: Cómo auditar el contenido de una máquina virtual
description: Obtenga información sobre cómo Azure Policy usa Rego y el agente de directivas público para administrar clústeres en Azure Kubernetes Service.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: e16c40c6a4f3539aa286c4c2d0859459ca18a91c
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338362"
---
# <a name="understand-azure-policy-for-azure-kubernetes-service"></a>Cómo usar Azure Policy para Azure Kubernetes Service

La integración de Azure Policy con [Azure Kubernetes Service](../../../aks/intro-kubernetes.md) (AKS) permite a los usuarios aplicar medidas de protección a escala para clústeres de un modo centralizado y coherente.
Al extender el uso del [equipo selector](https://github.com/open-policy-agent/gatekeeper), un _webhook de controlador de admisión_ para el [Agente de directivas público](https://www.openpolicyagent.org/) (OPA), Azure Policy le permite administrar e informar sobre el estado de cumplimiento de sus recursos de Azure y clústeres de AKS desde un solo lugar.

> [!NOTE]
> Azure Policy para AKS está en su versión preliminar limitada y solo admite definiciones de directivas integradas.

## <a name="overview"></a>Información general

Para habilitar y usar la Azure Policy para AKS con su clúster de AKS, realice las siguientes acciones:

- [Participar en las características de la versión preliminar](#opt-in-for-preview)
- [Instale el complemento de Azure Policy](#installation-steps)
- [Asigne una definición de directiva para AKS](#built-in-policies)
- [Espere la validación](#validation-and-reporting-frequency)

## <a name="opt-in-for-preview"></a>Participar en la versión preliminar

Antes de instalar el complemento de Azure Policy o habilitar cualquiera de las características del servicio, su suscripción debe tener habilitados el proveedor de recursos **Microsoft.ContainerService** y el proveedor de recursos **Microsoft.PolicyInsights**; a continuación, debe ser aprobado para unirse a la versión preliminar. Para unirse a la versión preliminar, siga estos pasos en Azure Portal o con la CLI de Azure:

- Azure Portal:

  1. Registre los proveedores de recursos **Microsoft.ContainerService** y **Microsoft.PolicyInsights**. Para conocer los pasos, consulte [Proveedores de recursos y sus tipos](../../../azure-resource-manager/resource-manager-supported-services.md#azure-portal).

  1. Inicie el servicio Azure Policy en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva**.

     ![Búsqueda de la directiva en todos los servicios](../media/rego-for-aks/search-policy.png)

  1. Seleccione **Join Preview** (Unirse a la versión preliminar) en el lado izquierdo de la página Azure Policy.

     ![Unirse a la directiva de la versión preliminar de AKS](../media/rego-for-aks/join-aks-preview.png)

  1. Seleccione la fila de la suscripción que quiera agregar a la versión preliminar.

  1. Seleccione el botón **Opt-in** (Participar) en la parte superior de la lista de suscripciones.

- CLI de Azure:

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  # Provider register: Register the Azure Kubernetes Services provider
  az provider register --namespace Microsoft.ContainerService

  # Provider register: Register the Azure Policy provider
  az provider register --namespace Microsoft.PolicyInsights

  # Feature register: enables installing the add-on
  az feature register --namespace Microsoft.ContainerService --name AKS-AzurePolicyAutoApprove

  # Feature register: enables the add-on to call the Azure Policy resource provider
  az feature register --namespace Microsoft.PolicyInsights --name AKS-DataplaneAutoApprove
  ```

## <a name="azure-policy-add-on"></a>Complemento de Azure Policy

El _complemento de Azure Policy_ para Kubernetes conecta el servicio de Azure Policy al controlador de admisión del equipo selector. El complemento, que se instala en el espacio de nombres _azure-policy_, ejecuta las siguientes funciones:

- Comprueba con Azure Policy las asignaciones al clúster de AKS.
- Descarga y almacena en la caché los detalles de la directiva, incluyendo la definición de la directiva _rego_, como **configmaps**.
- Ejecuta una comprobación de cumplimiento de examen completo en el clúster de AKS.
- Realiza informes de auditoría y cumplimiento de los detalles de Azure Policy.

### <a name="installing-the-add-on"></a>Instalación del complemento

#### <a name="prerequisites"></a>Requisitos previos

Antes de instalar el complemento en su clúster de AKS, debe instalar la extensión de la versión preliminar. Este paso se realiza con la CLI de Azure:

1. Es preciso que esté instalada y configurada la versión 2.0.62 de la CLI de Azure, o cualquier otra versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

1. Se debe usar la versión del clúster de AKS _1.10_ o superior. Use el siguiente script para validar la versión del clúster de AKS:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Look for the value in kubernetesVersion
   az aks list
   ```

1. Instale la versión _0.4.0_ de la extensión de la versión preliminar de la CLI de Azure para AKS, `aks-preview`:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Install/update the preview extension
   az extension add --name aks-preview

   # Validate the version of the preview extension
   az extension show --name aks-preview --query [version]
   ```

   > [!NOTE]
   > Si ha instalado anteriormente la extensión _aks-preview_, instale todas las actualizaciones con el comando `az extension update --name aks-preview`.

#### <a name="installation-steps"></a>Pasos de instalación

Una vez completados los requisitos previos, instale el complemento de Azure Policy en el clúster de AKS que quiera administrar.

- Portal de Azure

  1. Inicie el servicio AKS en Azure Portal haciendo clic en **Todos los servicios**; a continuación, busque y seleccione **Servicios de Kubernetes**.

  1. Seleccione uno de sus clústeres de AKS.

  1. Seleccione **Policies (preview)** (Directivas [versión preliminar]) en el lado izquierdo de la página de servicio de Kubernetes.

     ![Directivas del clúster de AKS](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. En la página principal, seleccione el botón **Enable add-on** (Habilitar complemento).

     ![Habilitar la directiva de Azure para el complemento de AKS](../media/rego-for-aks/enable-policy-add-on.png)

     > [!NOTE]
     > Si el botón **Enable add-on** (Habilitar complemento) está en gris, es que la suscripción aún no se ha agregado a la versión preliminar. Consulte [Opt-in for preview](#opt-in-for-preview) (Participar en la versión preliminar) para ver los pasos necesarios.

- CLI de Azure

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks enable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

### <a name="validation-and-reporting-frequency"></a>Validación y frecuencia de los informes

El complemento se registra con Azure Policy para detectar cambios en las asignaciones de directivas cada 5 minutos. Durante este ciclo de actualización, el complemento quita todos los elementos _configmaps_ en el espacio de nombres _azure-policy_ y, a continuación, vuelve a crear el elemento _configmaps_ para su uso en el equipo selector.

> [!NOTE]
> Si bien un _administrador de clúster_ puede tener permiso para el espacio de nombres _azure-policy_, no se recomienda realizar cambios en el espacio de nombres, puesto que no se admitirán. Cualquier cambio manual realizado se pierde durante el ciclo de actualización.

Cada 5 minutos, el complemento solicita un examen completo del clúster. Después de recopilar los detalles del examen completo y las evaluaciones en tiempo real que realiza el equipo selector sobre los intentos de cambios en el clúster, el complemento proporciona los resultados a Azure Policy para los agregue en los [detalles de cumplimiento](../how-to/get-compliance-data.md) como cualquier otra asignación de Azure Policy. Solo los resultados de las asignaciones de directivas activas se devuelven durante el ciclo de auditoría.

## <a name="policy-language"></a>Idioma de directiva

La estructura del lenguaje de Azure Policy para administrar AKS sigue el camino de las directivas existentes. El efecto _EnforceRegoPolicy_ se usa para administrar los clústeres de AKS y recopila propiedades de _detalles_ específicas para trabajar con OPA y el equipo selector. Para obtener detalles y ejemplos, consulte el efecto [EnforceRegoPolicy](effects.md#enforceregopolicy).

Como parte de la propiedad _details.policy_ en la definición de la directiva, Azure Policy pasa el URI de una directiva rego al complemento. Rego es el lenguaje que OPA y el equipo selector admiten para validar o mutar una solicitud al clúster de Kubernetes. Al admitir un estándar existente para la administración de Kubernetes, Azure Policy le permite reutilizar las reglas existentes y vincularlas con Azure Policy para obtener una experiencia de informes de cumplimiento unificada en la nube. Para obtener más información, consulte [¿Qué es Rego?](https://www.openpolicyagent.org/docs/how-do-i-write-policies.html#what-is-rego)

## <a name="built-in-policies"></a>Directivas integradas

Para encontrar las directivas integradas para administrar AKS mediante Azure Portal, siga estos pasos:

1. Inicie el servicio Azure Policy en Azure Portal. Seleccione **All services** (Todos los servicios) en el panel izquierdo y busque y seleccione la opción **Policy** (Directiva).

1. En el panel izquierdo de la página de Azure Policy, seleccione **Definitions** (Definiciones).

1. En el cuadro de lista desplegable Category (Categoría), use **Select all** (Seleccionar todo) para borrar el filtro y seleccione **Kubernetes service** (Servicio Kubernetes).

1. Seleccione la definición de directiva y pulse el botón **Assign** (Asignar).

> [!NOTE]
> Al asignar Azure Policy para la definición de AKS, el **Ámbito** debe incluir el recurso de clúster de AKS.

Alternativamente, use la opción de inicio rápido [Assign a policy - Portal](../assign-policy-portal.md) (Asignar una directiva - Portal) para encontrar y asignar una directiva de AKS. Busque una definición de directiva de Kubernetes en lugar del ejemplo "audit vms".

## <a name="logging"></a>Registro

### <a name="azure-policy-add-on-logs"></a>Registros del complemento de Azure Policy

Como controlador o contenedor de Kubernetes, el complemento de Azure Policy mantiene registros en el clúster de AKS. Los registros se exponen en la página **Insights** (Detalles) del clúster AKS. Para obtener más información, consulte [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](../../../azure-monitor/insights/container-insights-analyze.md).

### <a name="gatekeeper-logs"></a>Registros del equipo selector

Para habilitar los registros del equipo selector para nuevas solicitudes de recursos, siga los pasos en [Enable and review Kubernetes master node logs in AKS](../../../aks/view-master-logs.md) (Habilitar y revisar los registros del nodo maestro de Kubernetes en AKS).
Esta es una consulta de ejemplo para ver los eventos denegados en nuevas solicitudes de recursos:

```kusto
| where Category == "kube-audit"
| where log_s contains "admission webhook"
| limit 100
```

Para ver los registros de los contenedores del equipo selector, siga los pasos en [Habilitar y revisar los registros del nodo maestro de Kubernetes en AKS](../../../aks/view-master-logs.md) y marque la opción _kube-apiserver_ en el panel **Diagnostic settings** (Configuración de diagnóstico).

## <a name="remove-the-add-on"></a>Eliminar el complemento

Para eliminar el complemento de Azure Policy de su clúster de AKS, use Azure Portal o la CLI de Azure:

- Portal de Azure

  1. Inicie el servicio AKS en Azure Portal haciendo clic en **Todos los servicios**; a continuación, busque y seleccione **Servicios de Kubernetes**.

  1. Seleccione el clúster de AKS en el que quiera deshabilitar el complemento de Azure Policy.

  1. Seleccione **Policies (preview)** (Directivas [versión preliminar]) en el lado izquierdo de la página de servicio de Kubernetes.

     ![Directivas del clúster de AKS](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. En la página principal, pulse **Disable add-on** (Deshabilitar complemento).

     ![Deshabilitar Azure Policy para el complemento de AKS](../media/rego-for-aks/disable-policy-add-on.png)

- CLI de Azure

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md).
- Vea la [Estructura de definición de Azure Policy](definition-structure.md).
- Vea la [Descripción de los efectos de directivas](effects.md).
- Obtenga información acerca de cómo se pueden [crear directivas mediante programación](../how-to/programmatically-create.md).
- Obtenga información sobre cómo [obtener datos de cumplimiento](../how-to/getting-compliance-data.md).
- Obtenga información sobre cómo [corregir recursos no compatibles](../how-to/remediate-resources.md).
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.