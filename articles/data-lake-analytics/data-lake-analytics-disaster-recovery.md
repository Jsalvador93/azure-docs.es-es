---
title: Guía de recuperación ante desastres para Azure Data Lake Analytics
description: Obtenga información sobre cómo planear la recuperación ante desastres para las cuentas de Azure Data Lake Analytics.
services: data-lake-analytics
author: MikeRys
ms.author: mrys
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ea1d4020aa9be23b4839690ae0b386d35bce8a23
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66498895"
---
# <a name="disaster-recovery-guidance-for-azure-data-lake-analytics"></a>Guía de recuperación ante desastres para Azure Data Lake Analytics

Azure Data Lake Analytics es un servicio de trabajos de análisis a petición que simplifican los macrodatos. En lugar de implementar, configurar y ajustar el hardware, escribirá consultas para transformar los datos y extraer ideas valiosas. El servicio de análisis puede administrar trabajos de cualquier escala al instante, simplemente estableciendo el ajuste adecuado. Solo tiene que pagar por su trabajo cuando se está ejecutando, lo que hace que sea una solución económica. En este artículo, se proporciona una guía sobre cómo proteger sus trabajos en caso de producirse una improbable interrupción de toda la región o una eliminación accidental.

## <a name="disaster-recovery-guidance"></a>Guía de recuperación ante desastres

Al usar Azure Data Lake Analytics, es fundamental preparar su propio plan de recuperación ante desastres. Este artículo es una guía para crear un plan de recuperación ante desastres. Aquí tiene algunos recursos adicionales que pueden ayudarle a crear su propio plan:
- [Recuperación ante desastres y errores para aplicaciones de Azure](/azure/architecture/reliability/disaster-recovery)
- [Guía técnica sobre resistencia en Azure](/azure/architecture/reliability)

## <a name="best-practices-and-scenario-guidance"></a>Procedimientos recomendados y guía de escenarios

Puede ejecutar un trabajo de U-SQL recurrente en una cuenta de ADLA en una región que lea y escriba tablas de U-SQL, así como datos no estructurados.  Prepárese para un desastre con estos pasos:

1. Cree cuentas de ADLA y ADLS en la región secundaria que se usará durante una interrupción.

   > [!NOTE]
   > Dado que los nombres de cuenta son únicos a nivel global, utilice un esquema de nomenclatura coherente que indique qué cuenta es secundaria.

2. Para datos no estructurados, consulte [Guía de recuperación ante desastres para datos de Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-disaster-recovery-guidance.md).

3. Para datos estructurados almacenados en bases de datos y tablas de ADLA, cree copias de los artefactos de metadatos, como las bases de datos, las tablas, las funciones con valores de tabla y los ensamblados. Deberá volver a sincronizar estos artefactos periódicamente cuando se produzcan cambios en producción. Por ejemplo, los nuevos datos insertados deben replicarse en la región secundaria copiando los datos e insertándolos en la tabla secundaria.

   > [!NOTE]
   > Estos nombres de objeto se limitan a la cuenta secundaria y no son únicos a nivel global, por lo que pueden tener los mismos nombres que en la cuenta de producción principal.

Durante una interrupción, deberá actualizar los scripts para que las rutas de acceso de entrada señalen al punto de conexión secundario. A continuación, los usuarios enviarán sus trabajos a la cuenta de ADLA en la región secundaria. La salida del trabajo se escribirá en la cuenta de ADLA y de ADLS en la región secundaria.

## <a name="next-steps"></a>Pasos siguientes

[Guía de recuperación ante desastres para datos de Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)
