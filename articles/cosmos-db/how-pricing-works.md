---
title: Modelo de precios de Azure Cosmos DB
description: En este artículo se explica el modelo de precios de Azure Cosmos DB y cómo simplifica la administración y el planeamiento de los costos.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: rimman
ms.openlocfilehash: 43a4b46199277da11d2820a11ec0801c0345e62b
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68717616"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Modelo de precios de Azure Cosmos DB 

El modelo de precios de Azure Cosmos DB simplifica la administración y el planeamiento de los costos. Con Azure Cosmos DB se paga por el rendimiento aprovisionado y el almacenamiento consumido.

* **Rendimiento aprovisionado**: el rendimiento aprovisionado (también denominado "reservado") garantiza el alto rendimiento a cualquier escala. Se especifica el rendimiento (RU/s) necesario y Azure Cosmos DB dedica los recursos necesarios para garantizar el rendimiento configurado. La facturación se realiza por hora al rendimiento máximo aprovisionado para una concreta.

   > [!NOTE]
   > Dado que el modelo de rendimiento aprovisionado dedica recursos a su contenedor o base de datos, se le cobrará por el rendimiento aprovisionado aunque no ejecute ninguna carga de trabajo.

* **Almacenamiento consumido**: se le cobrará una tarifa plana por la cantidad total de almacenamiento (GB) usada para los datos y los índices en una hora concreta.

El rendimiento aprovisionado, que se conoce como [unidades de solicitud](request-units.md) por segundo (RU/s), permite leer los datos de los contenedores o bases de datos, o escribirlos en ellos. También puede [aprovisionar el rendimiento en una base de datos o un contenedor](set-throughput.md). Según sus necesidades de carga de trabajo, puede escalar o reducir verticalmente el rendimiento en cualquier momento. Los precios de Azure Cosmos DB son elásticos y proporcionales al rendimiento que configure en una base de datos o un contenedor. Los valores mínimos de rendimiento y almacenamiento y los incrementos de escalado proporcionan una gama completa de precios respecto al espectro de elasticidad para todos los segmentos de clientes, desde contenedores a pequeña escala hasta los de gran escala. Las bases de datos o contenedores se facturan por hora por el rendimiento aprovisionado en unidades de 100 RU/s, con un mínimo de 400 RU/s y el almacenamiento consumido en GB. A diferencia del rendimiento aprovisionado, el almacenamiento se factura según el consumo. Es decir, no tiene que reservar almacenamiento de antemano. Se le cobrará solo por el almacenamiento que consuma.

Para más información, consulte la página [Precios de Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) y [Understanding your Azure Cosmos DB bill](understand-your-bill.md) (Explicación de la factura de Azure Cosmos DB).

El modelo de precios de Azure Cosmos DB es coherente en todas las API. Para más información, consulte el artículo sobre la [Rentabilidad del modelo de precios de Azure Cosmos DB para los clientes](total-cost-ownership.md). El contenedor o la base de datos tienen un rendimiento mínimo para garantizar los Acuerdos de Nivel de Servicio; el rendimiento aprovisionado se puede aumentar o disminuir 6 USD por cada 100 RU/s.

Actualmente el precio mínimo del rendimiento para las bases de datos y los contenedores es de 24 USD/mes (consulte la página [Precios de Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) para conocer la información más reciente). Si la carga de trabajo usa varios contenedores, puede optimizarse el costo mediante el rendimiento de nivel de base de datos, ya que este permite tener cualquier número de contenedores en una base de datos con el rendimiento compartido entre ellos. En la siguiente tabla se resumen el rendimiento aprovisionado y los costos para diferentes entidades:

|**Entidad**  | **Capacidad de proceso y costo mínimos** |**Incrementos de escala y costo** |**Ámbito del aprovisionamiento** |
|---------|---------|---------|-------|
|Base de datos    | 400 RU/s (24 USD/mes)    | 100 RU/s (6 USD/mes)   |La capacidad de proceso está reservada para la base de datos y se comparte entre los contenedores dentro de esta |
|Contenedor     | 400 RU/s (24 USD/mes)    | 100 RU/s (6 USD/mes)  |La capacidad de proceso está reservada para un contenedor específico |

Como se muestra en la tabla anterior, la capacidad de proceso mínima en Azure Cosmos DB empieza por un precio de 24 USD/mes. Si se empieza por la capacidad de proceso mínima y se escala verticalmente con el tiempo para admitir las cargas de trabajo de producción, los costos aumentarán gradualmente en incrementos de 6 USD/mes. El modelo de precios de Azure Cosmos DB es elástico y la escalada y la reducción vertical suponen un aumento o una reducción del precio gradual.

## <a name="try-azure-cosmos-db-for-free"></a>Pruebe gratis Azure Cosmos DB 

Azure Cosmos DB ofrece varias opciones para que los desarrolladores lo prueben de forma gratuita. Entre estas opciones se incluyen:

* **Cuenta gratuita de Azure**: Azure ofrece una [cuenta gratuita](https://azure.microsoft.com/free/) con 200 USD de crédito de Azure para los 30 primeros días y una cantidad limitada de servicios gratuitos durante 12 meses. Para más información consulte el sitio de [cuentas gratuitas de Azure](../billing/billing-avoid-charges-free-account.md). Azure Cosmos DB forma parte de esta cuenta gratuita de Azure. Específicamente para Azure Cosmos DB, esta cuenta gratuita ofrece 5 GB de almacenamiento y 400 RU de aprovisionamiento de capacidad de proceso para todo el año. 

* **Probar gratis Azure Cosmos DB**: Azure Cosmos DB se ofrece con las cuentas gratuitas por tiempo limitado. Puede crear una cuenta de Azure Cosmos DB, bases de datos y colecciones, y ejecutar una aplicación de ejemplo con la ayuda de artículos de inicio rápido y tutoriales. Puede ejecutar la aplicación de ejemplo sin tener que suscribirse a una cuenta de Azure o con la tarjeta de crédito. [Pruebe gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) para disfrutar de ello durante un mes, con la posibilidad de renovar la cuenta las veces que desee.

* **Emulador de Azure Cosmos DB**: El emulador de Azure Cosmos DB proporciona un entorno local que emula el servicio Azure Cosmos DB con fines de desarrollo. El emulador se ofrece sin costo alguno y con alta fidelidad para el servicio en la nube. Mediante el emulador de Azure Cosmos DB, puede desarrollar y probar su aplicación localmente, sin crear una suscripción de Azure ni incurrir en costos. Puede desarrollar sus aplicaciones mediante el emulador localmente antes de pasar a producción. Cuando esté satisfecho con la funcionalidad de la aplicación en el emulador, puede cambiar a la cuenta de Azure Cosmos DB en la nube y ahorrará costos considerables. Para más información sobre el emulador, consulte el artículo sobre el [uso de Azure Cosmos DB para desarrollo y pruebas](local-emulator.md) para más detalles.

## <a name="pricing-with-reserved-capacity"></a>Precios con capacidad reservada

La [capacidad reservada](cosmos-db-reserved-capacity.md) de Azure Cosmos DB le ayuda a ahorrar dinero gracias al pago anticipado de los recursos de Azure Cosmos DB durante uno o tres años. Con los compromisos de un año o tres por adelantado puede reducir los costos considerablemente y ahorrar de un 20 a un 65 % de descuento en comparación con los precios normales. La capacidad reservada de Azure Cosmos DB ayuda a reducir los costos al pagar previamente por la capacidad de proceso aprovisionada (RU/s) durante un período de uno o tres años y obtener un descuento en la capacidad de proceso aprovisionada. 

La capacidad reservada ofrece un descuento en la facturación y no afecta el estado del entorno de ejecución de sus recursos de Azure Cosmos DB. La capacidad reservada está disponible de manera coherente para todas las API (incluidos MongoDB, Cassandra, SQL, Gremlin y Azure Tables) y en todas las regiones del mundo. Puede encontrar más información sobre la capacidad reservada en el artículo sobre el [pago por adelantado de los recursos de Azure Cosmos DB con capacidad reservada](cosmos-db-reserved-capacity.md) y comprar capacidad reservada en [Azure Portal](https://portal.azure.com/).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la optimización de los costos de los recursos de Azure Cosmos DB en los siguientes artículos:

* Sobre la [optimización para el desarrollo y pruebas](optimize-dev-test.md)
* Obtenga más información sobre [la factura de Azure Cosmos DB](understand-your-bill.md).
* Obtenga más información sobre la [optimización del costo de la capacidad del rendimiento](optimize-cost-throughput.md).
* Obtenga más información sobre la [optimización del costo del almacenamiento](optimize-cost-storage.md).
* Obtenga más información sobre la [optimización del costo de la lectura y la escritura](optimize-cost-reads-writes.md).
* Sobre la [Optimización del costo de las consultas](optimize-cost-queries.md)
* Más información sobre la [optimización del costo de las cuentas de Cosmos de varias regiones](optimize-cost-regions.md)
* Información sobre la [capacidad reservada de Azure Cosmos DB](cosmos-db-reserved-capacity.md)
* Información sobre el [emulador de Azure Cosmos DB](local-emulator.md)
