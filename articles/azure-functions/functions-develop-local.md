---
title: Desarrollo y ejecución de funciones de Azure de forma local | Microsoft Docs
description: Aprenda a codificar y probar funciones de Azure en la máquina local antes de ejecutarlas en Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: c50615b9cbe5c3ca926d893858bb39eeb5720067
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129586"
---
# <a name="code-and-test-azure-functions-locally"></a>Codificación y comprobación de las funciones de Azure Functions en un entorno local

Aunque puede programar y probar las funciones de Azure en [Azure Portal], muchos desarrolladores prefieren una experiencia de desarrollo local. Azure Functions le permite usar el editor de código y las herramientas de desarrollo que prefiera para crear y probar sus funciones en un equipo local. Las funciones locales pueden conectarse a servicios de Azure en directo, y puede depurar sus funciones en el equipo local con el entorno de tiempo de ejecución de Functions completo.

## <a name="local-development-environments"></a>Entornos de desarrollo locales

La manera en la que desarrolla las funciones en el equipo local depende del [lenguaje](supported-languages.md) y las herramientas que prefiera. Los entornos de la siguiente tabla admiten el desarrollo local:

|Entorno                              |Lenguajes         |DESCRIPCIÓN|
|-----------------------------------------|------------|---|
|[Visual Studio Code](functions-develop-vs-code.md)| [C# (biblioteca de clases)](functions-dotnet-class-library.md), [script de C# (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md), [PowerShell](functions-create-first-function-powershell.md), [Python](functions-reference-python.md) | La [extensión de Azure Functions para VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) agrega a Functions compatibilidad con VS Code. Requiere Core Tools. Admite el desarrollo en Linux, MacOS y Windows, cuando se usa la versión 2.x de Core Tools. Para más información, vea [Creación de la primera función mediante Visual Studio Code](functions-create-first-function-vs-code.md). |
| [Símbolo del sistema o terminal](functions-run-local.md) | [C# (biblioteca de clases)](functions-dotnet-class-library.md), [script de C# (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md), [PowerShell](functions-reference-powershell.md), [Python](functions-reference-python.md) | [Azure Functions Core Tools] proporciona el entorno de tiempo de ejecución central y las plantillas para crear las funciones, lo que permite desarrollar localmente. La versión 2.x es compatible con el desarrollo en Linux, MacOS y Windows. Todos los entornos usan Core Tools para el entorno de tiempo de ejecución local de Functions. |
| [Visual Studio 2019](functions-develop-vs.md) | [C# (biblioteca de clases)](functions-dotnet-class-library.md) | Las herramientas de Azure Functions forman parte de la carga de trabajo de **desarrollo de Azure** de la versión [Visual Studio 2019](https://www.visualstudio.com/vs/) y versiones posteriores. Le permite compilar funciones en una biblioteca de clases y publicar el archivo .dll en Azure. Incluye Core Tools para realizar pruebas locales. Para más información, consulte [Desarrollo de Azure Functions con Visual Studio](functions-develop-vs.md) |
| [Maven](functions-create-first-java-maven.md) (varios) | [Java](functions-reference-java.md) | Se integra con Core Tools para poder desarrollar funciones con Java. La versión 2.x es compatible con el desarrollo en Linux, MacOS y Windows. Para más información, consulte [Creación de la primera función con Java y Maven](functions-create-first-java-maven.md). También es compatible con el desarrollo con [Eclipse](functions-create-maven-eclipse.md) y [IntelliJ IDEA](functions-create-maven-intellij.md) |

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Cada uno de estos entornos de desarrollo local le permite crear proyectos de aplicación de función y usar plantillas de Functions predefinidas para crear nuevas funciones. Cada una usa Core Tools para probar y depurar sus funciones en el entorno de tiempo de ejecución real de Functions en su propia máquina, igual que haría con cualquier otra aplicación. También puede publicar su proyecto de aplicación de función desde cualquiera de estos entornos en Azure.  

## <a name="next-steps"></a>Pasos siguientes

+ Para obtener más información sobre el desarrollo local de funciones compiladas para C# con Visual Studio 2019, consulte [Desarrollo de Azure Functions con Visual Studio](functions-develop-vs.md).
+ Para más información sobre el desarrollo local de funciones con VS Code en un equipo Mac, Linux o Windows, consulte la [documentación de VS Code para Azure Functions](https://code.visualstudio.com/tutorials/functions-extension/getting-started).
+ Para más información sobre cómo desarrollar funciones desde un terminal o el símbolo del sistema, consulte [Uso de Azure Functions Core Tools](functions-run-local.md).

<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure Portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
