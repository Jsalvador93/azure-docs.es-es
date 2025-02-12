---
title: Herramientas de visualización y exploración de datos
titleSuffix: Azure Data Science Virtual Machine
description: Herramientas de visualización y exploración de datos de Data Science Virtual Machine.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 03/16/2018
ms.openlocfilehash: 14c34a13440d50fcf42c2207f9933dc1de014a14
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70191965"
---
# <a name="data-exploration-and-visualization-tools-on-the-azure-data-science-virtual-machine"></a>Herramientas de visualización y exploración de datos en Azure Data Science Virtual Machine

Un paso clave en la ciencia de datos es comprender los datos. Las herramientas de visualización y de exploración de datos permiten acelerar la comprensión de los datos. Las siguientes herramientas, que se proporcionan en Data Science Virtual Machine (DSVM), facilitan este paso clave.

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| ¿Qué es?   | Motor de consultas SQL de código abierto en macrodatos    |
| Versiones de DSVM compatibles      | Windows, Linux  |
| ¿Cómo se configura e instala en DSVM?      |  Solo se instala en `/dsvm/tools/drill*` en modo incrustado   |
| Usos típicos      |  Para la exploración de datos en contexto sin necesidad de extraerlos, transformarlos y cargarlos (ETL). Consulta de distintos orígenes y formatos de datos, como CSV, JSON, tablas relacionales y Hadoop.     |
| Cómo usarla y ejecutarla      | Acceso directo de escritorio  <br/> [Comience a usar Drill en 10 minutos](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| Herramientas relacionadas en DSVM      |   Rattle, Weka y SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |  Una colección de algoritmos de aprendizaje automático para las tareas de minería de datos. Los algoritmos se pueden aplicar directamente a un conjunto de datos o se pueden llamar desde su propio código de Java. Weka contiene herramientas para el preprocesamiento, la clasificación, la regresión, la agrupación en clústeres, las reglas de asociación y la visualización de datos. |
| Ediciones compatibles de DSVM     | Windows, Linux     |
| Usos típicos      | Herramienta de aprendizaje automático general     |
| Cómo usarla y ejecutarla      | En Windows, busque Weka en el menú Inicio. En Linux, inicie sesión con X2Go y, a continuación, vaya a Applications (Aplicaciones) > Development (Desarrollo) > Weka. |
| Vínculos a ejemplos      | [Ejemplos de Weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Herramientas relacionadas en DSVM      |LightGBM, Rattle y Xgboost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |   Una interfaz gráfica de usuario (GUI) para la minería de datos con R   |
| Ediciones compatibles de DSVM     | Windows, Linux     |
| Usos típicos      | Herramienta general de minería de datos de la IU para R    |
| Cómo usarla y ejecutarla      | Herramienta de interfaz de usuario. En Windows, abra un símbolo del sistema, ejecute R y, dentro de R, ejecute `rattle()`. En Linux, conéctese a X2Go, inicie un terminal, ejecute R y, dentro de R, ejecute `rattle()`. |
| Vínculos a ejemplos      | [Rattle](https://togaware.com/onepager/) |
| Herramientas relacionadas en DSVM      |LightGBM, Weka y Xgboost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| ¿Qué es?   | Visualización de datos interactivos y herramienta de BI    |
| Versiones de DSVM compatibles      | Windows  |
| Usos típicos      |  Visualización de datos y creación de paneles   |
| Cómo usarla y ejecutarla      | Acceso directo de escritorio (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2019, Visual Studio Code, Juno      |

