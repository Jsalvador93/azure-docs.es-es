---
title: 'Inicio rápido: Uso de Azure Portal para crear temas de Service Bus y suscripciones a dichos temas | Microsoft Docs'
description: En este inicio rápido, aprenderá a crear un tema de Service Bus y suscripciones a dicho tema con Azure Portal.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: a392f8b11a7ab1ad72f4da289c54e34b022f1ea6
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990311"
---
# <a name="quickstart-use-the-azure-portal-to-create-a-service-bus-topic-and-subscriptions-to-the-topic"></a>Inicio rápido: Use Azure Portal para crear un tema de Service Bus y suscripciones a dicho tema
En este inicio rápido, usará Azure Portal para crear un tema de Service Bus y, después, crear suscripciones a dicho tema. 

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Qué son los temas y las suscripciones de Service Bus
Las suscripciones y los temas de Service Bus son compatibles con el modelo de comunicación de mensajería de *publicación/suscripción* . Cuando se usan temas y suscripciones, los componentes de una aplicación distribuida no se comunican directamente entre sí, sino que intercambian mensajes a través de un tema, que actúa como un intermediario.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

A diferencia de las colas de Service Bus, en las que un solo destinatario procesa cada mensaje, los temas y las suscripciones proporcionan una forma de comunicación de uno a varios mediante un patrón de publicación/suscripción. Es posible registrar varias suscripciones en un tema. Cuando un mensaje se envía a un tema, pasa a estar disponible para cada suscripción para la administración o el procesamiento de manera independiente. Una suscripción a un tema se asemeja a una cola virtual que recibe copias de los mensajes que se enviaron al tema. Opcionalmente, puede registrar reglas de filtros para un tema por suscripción, lo que le permite filtrar o restringir qué mensajes para un tema reciben las suscripciones a un tema.

Las suscripciones y temas de Service Bus le permiten escalar para realizar el procesamiento de un número elevado de mensajes en una serie amplia de usuarios y aplicaciones.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-topics-three-subscriptions-portal](../../includes/service-bus-create-topics-three-subscriptions-portal.md)]

> [!NOTE]
> Puede administrar los recursos de Service Bus con el [Explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer/). El Explorador de Service Bus permite a los usuarios conectarse a un espacio de nombres de Service Bus y administrar las entidades de mensajería de una forma sencilla. La herramienta dispone de características avanzadas, como la funcionalidad de importación y exportación o la capacidad de probar el tema, las colas, las suscripciones, los servicios de retransmisión, los centros de notificaciones y los centros de eventos. 

## <a name="next-steps"></a>Pasos siguientes
Para obtener información acerca de cómo enviar mensajes a un tema y recibirlos a través de una suscripción, consulte el artículo siguiente: seleccione el lenguaje de programación en la tabla de contenido. 

> [!div class="nextstepaction"]
> [Publicación y suscripción de mensajes](service-bus-dotnet-how-to-use-topics-subscriptions.md)
