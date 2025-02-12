---
title: Habilitación de cuadernos en la cuenta de Azure Cosmos DB
description: La integración de cuadernos en Azure Cosmos DB permite analizar y visualizar los datos desde el portal. En este artículo se describe cómo habilitar esta característica para las cuentas de Cosmos.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/22/2019
ms.author: dech
ms.openlocfilehash: 34e1d9fe937118a2eafcdd85c153653eb28752d4
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71220118"
---
# <a name="enable-notebooks-for-azure-cosmos-db-accounts"></a>Habilitación de cuadernos para las cuentas de Azure Cosmos DB

Los cuadernos de Jupyter integrados en Azure Cosmos DB permiten analizar y visualizar los datos desde Azure Portal. En este artículo se describe cómo habilitar esta característica para la cuenta de Azure Cosmos DB.

## <a name="enable-notebooks-in-a-new-cosmos-account"></a>Habilitación de cuadernos en una cuenta nueva de Cosmos
1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
1. Seleccione **Crear un recurso** > **Bases de datos** > **Azure Cosmos DB**.
1. En la página Creación de una cuenta de Azure Cosmos DB, seleccione Notebooks (Cuadernos). 
 
    ![Selección de la opción Notebooks (Cuadernos) en la hoja Creación de una cuenta de Azure Cosmos DB](media/enable-notebooks/create-new-account-with-notebooks.png)
1. Seleccione **Revisar + crear**. Puede omitir las opciones **Red** y **Etiquetas**. 
1. Revise la configuración de la cuenta y seleccione **Crear**. La operación de creación de la cuenta tarda unos minutos. Espere hasta que la página del portal muestre **Se completó la implementación** . 

    ![El panel de las notificaciones de Azure Portal](media/enable-notebooks/create-new-account-with-notebooks-complete.png)
1. Seleccione **Ir al recurso** para ir a la página de la cuenta de Azure Cosmos DB. 

    ![La página de la cuenta de Azure Cosmos DB](../../includes/media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-3.png)

1. Vaya al panel **Explorador de datos**. Ahora debería ver el área de trabajo de Notebooks.

    ![Nuevo área de trabajo de Notebooks en Azure Cosmos DB](media/enable-notebooks/new-notebooks-workspace.png)

## <a name="enable-notebooks-in-an-existing-cosmos-account"></a>Habilitación de cuadernos en una cuenta de Cosmos existente
También puede habilitar cuadernos en las cuentas existentes. Este paso debe realizarse una sola vez por cuenta.

1. Vaya al panel **Explorador de datos** en su cuenta de Cosmos.
1. Seleccione Enable Notebooks (Habilitar Notebooks).

    ![Creación de un nuevo área de trabajo de Notebooks en el Explorador de datos](media/enable-notebooks/enable-notebooks-workspace.png)
1. Se le pedirá que cree un área de trabajo de Notebooks. Seleccione **Complete setup** (Completar configuración).
1. Ahora su cuenta está habilitada para usar Notebooks.

## <a name="create-and-run-your-first-notebook"></a>Creación y ejecución del primer cuaderno

Para comprobar que puede usar Notebooks, seleccione uno de los cuadernos de Sample Notebooks (Cuadernos de ejemplo). Se guardará una copia del cuaderno en el área de trabajo y se abrirá.

En este ejemplo, usaremos **GettingStarted.ipynb**. 

![Visualización del cuaderno GettingStarted.ipynb](media/enable-notebooks/select-getting-started-notebook.png)

Para ejecutar el cuaderno:
1. Seleccione la primera celda de código que contiene el código de Python. 
1. Seleccione **Ejecutar** para ejecutar la celda. También puede usar **Mayús+Entrar** para ejecutar la celda.
1. Actualice el panel de recursos para ver la base de datos y el contenedor que se han creado.

    ![Ejecución del cuaderno inicial](media/enable-notebooks/run-first-notebook-cell.png)

También puede seleccionar New Notebook (Nuevo cuaderno) para crear un cuaderno o cargar un archivo de Notebook (.ipynb) existente al seleccionar Cargar archivo en el menú My Notebooks (Mis cuadernos). 

![Creación o carga de un cuaderno nuevo](media/enable-notebooks/create-or-upload-new-notebook.png)

## <a name="next-steps"></a>Pasos siguientes

- Conozca las ventajas de los [cuadernos de Jupyter para Azure Cosmos DB](cosmosdb-jupyter-notebooks.md)
