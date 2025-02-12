---
title: Administración de estados de alertas y de grupos inteligentes
description: Administración de los estados de las instancias de alertas y de grupos inteligentes
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: f72abd1168d14cae703f7fcfc7b58eaa56bdc592
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71702937"
---
# <a name="manage-alert-and-smart-group-states"></a>Administración de estados de alertas y de grupos inteligentes
Las alertas en Azure Monitor ahora tienen un [estado de alerta y una condición de supervisión](https://aka.ms/azure-alerts-overview) y, de forma similar, los grupos inteligentes tienen un [estado de grupo inteligente](https://aka.ms/smart-groups). Los cambios en el estado ahora se registran en el historial que está asociado a la alerta o al grupo inteligente. Este artículo le guiará a través del proceso de cambiar el estado de las alertas y los grupos inteligentes.

## <a name="change-the-state-of-an-alert"></a>Cambio en el estado de una alerta
1. Puede cambiar el estado de una alerta de las siguientes formas: 
    * En la página Todas las alertas, haga clic en la casilla de verificación situada junto a las alertas cuyo estado desea cambiar y, después, en Cambiar estado.   
    ![Supervisión](./media/alerts-managing-alert-states/state-all-alerts.jpg)
    * Puede hacer clic en Cambiar estado desde la página Detalles de la alerta de una instancia de alerta específica   
    ![Supervisión](./media/alerts-managing-alert-states/state-alert-details.jpg)
    * En la página Detalles de la alerta de una instancia de alerta específica, dentro del panel Grupo inteligente, puede hacer clic en la casilla de verificación junto a las alertas que quiera    
    ![Supervisión](./media/alerts-managing-alert-states/state-alert-details-sg.jpg)

    * En la página Detalles del grupo inteligente, puede marcar la casilla al lado de las alertas que aparecen en la lista de miembros si desea cambiar su estado y, a continuación, puede hacer clic en Cambiar estado.   
    ![Supervisión](./media/alerts-managing-alert-states/state-sg-details-alerts.jpg)
1. Al hacer clic en Cambiar estado, se abre un menú emergente que permite seleccionar el estado (Nuevo, Confirmado o Cerrado) y escribir un comentario si es necesario.   
![Supervisión](./media/alerts-managing-alert-states/state-alert-change.jpg)
1. Una vez hecho esto, el cambio de estado se registra en el historial de la alerta correspondiente. Puede comprobar que se haya registrado si abre la página de detalles correspondiente y observa la sección de historial.    
![Supervisión](./media/alerts-managing-alert-states/state-alert-history.jpg)

## <a name="change-the-state-of-a-smart-group"></a>Cambios en el estado del grupo inteligente
1. Puede cambiar el estado de un grupo inteligente de las siguientes formas:
    1. En la página con la lista de grupos inteligentes, puede hacer clic en la casilla de verificación situada junto a los grupos inteligentes cuyo estado desea cambiar y, después, en Cambiar estado  
    ![Supervisión](./media/alerts-managing-alert-states/state-sg-list.jpg)
    1. En la página de detalles del grupo inteligente, puede hacer clic en Cambiar estado        
    ![Supervisión](./media/alerts-managing-alert-states/state-sg-details.jpg)
1. Al hacer clic en Cambiar estado, se abre un menú emergente que permite seleccionar el estado (Nuevo, Confirmado o Cerrado) y escribir un comentario si es necesario. 
![Supervisión](./media/alerts-managing-alert-states/state-sg-change.jpg)
   > [!NOTE]
   >  El cambio de estado de un grupo inteligente no modifica el estado de las alertas individuales que lo componen.

1. Una vez hecho esto, el cambio de estado se registra en el historial del grupo inteligente correspondiente. Puede comprobar que se haya registrado si abre la página de detalles correspondiente y observa la sección de historial.     
![Supervisión](./media/alerts-managing-alert-states/state-sg-history.jpg)

