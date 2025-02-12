---
title: 'Enlaces para Durable Functions: Azure'
description: Aprenda a utilizar desencadenadores y enlaces en la extensión Durable Functions para Azure Functions.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: f297c89d2c3ba5692a44fab631c0d46c75f48692
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71033584"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Enlaces para Durable Functions (Azure Functions)

La extensión [Durable Functions](durable-functions-overview.md) presenta dos nuevos enlaces de desencadenador que controlan la ejecución de las funciones del orquestador y de actividad. También presenta a un enlace de salida que actúa como un cliente para el tiempo de ejecución de Durable Functions.

## <a name="orchestration-trigger"></a>Desencadenador de orquestación

El desencadenador de orquestación permite crear [funciones de orquestador durables](durable-functions-types-features-overview.md#orchestrator-functions). Este desencadenador admite iniciar nuevas instancias de función del orquestador y reanudar las instancias existentes de función del orquestador que están en "espera" de una tarea.

Al utilizar las herramientas de Visual Studio para Azure Functions, el desencadenador de orquestación se configura usando el atributo .NET [OrchestrationTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html).

Al escribir funciones de orquestador en lenguajes de scripting (por ejemplo, JavaScript o scripting en C#), el desencadenador de orquestación se define mediante el objeto JSON siguiente en la matriz `bindings` del archivo *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration`es el nombre de la orquestación. Este es el valor que los clientes tienen que usar cuando deseen iniciar nuevas instancias de esta función del orquestador. Esta propiedad es opcional. Si no se especifica, se utiliza el nombre de la función.

Internamente este enlace de desencadenador sondea una serie de colas de la cuenta de almacenamiento predeterminado para la aplicación de la función. Estas colas son detalles de implementación internos de la extensión, esta es la razón de que no se configuren explícitamente en las propiedades de enlace.

### <a name="trigger-behavior"></a>Comportamiento de un desencadenador

Estas son algunas notas acerca del desencadenador de orquestación:

* **Subprocesamiento único**: un único subproceso de distribución se usa para toda la ejecución de función del orquestador en una instancia de host. Por esta razón, es importante asegurarse de que el código de función del orquestador es eficaz y no realiza ninguna E/S. También es importante asegurarse de que este subproceso no realizar ningún trabajo asincrónico excepto cuando se espera en tipos de tareas específicos de Durable Functions.
* **Control de mensajes dudosos**: no hay compatibilidad con mensajes dudosos en los desencadenadores de orquestación.
* **Visibilidad de los mensajes**: los mensajes de desencadenador de orquestación se quitan de la cola y se mantienen invisibles durante un tiempo configurable. La visibilidad de estos mensajes se renueva automáticamente siempre que la aplicación de la función se esté ejecutando y se mantenga correcta.
* **Valores devueltos**: los valores devueltos se serializan en JSON y se conservan en la tabla de historial de orquestación en Azure Table Storage. Estos valores devueltos pueden ser consultados por el enlace de cliente de orquestación que se describe más adelante.

> [!WARNING]
> Las funciones del orquestador nunca deben usar ningún enlace de entrada o salida que no sea el enlace de desencadenador de orquestación. Si lo hacen, existe la posibilidad de que se produzcan problemas con la extensión Durable Task porque esos enlaces pueden no seguir las reglas de E/S y subprocesamiento único. Si desea usar otros enlaces, agréguelos a una función de actividad llamada desde la función de orquestador.

> [!WARNING]
> Las funciones del orquestador de JavaScript nunca se deberían declarar `async`.

### <a name="trigger-usage-net"></a>Uso del desencadenador (.NET)

El enlace de desencadenador de orquestación admite entradas y salidas. Estas son algunas cosas que hay que saber acerca del control de entradas y salidas:

* **entradas**: las funciones de orquestación de .NET admiten solamente [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) como tipo de parámetro. No se admite deserialización de entradas directamente en la firma de función. El código debe usar el método [GetInput\<T>](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1)(.NET) o `getInput` (JavaScript) para capturar entradas de la función del orquestador. Estas entradas tienen que ser tipos serializables con JSON.
* **salidas**: los desencadenadores de orquestación admiten valores de salida, así como de entrada. El valor devuelto de la función se utiliza para asignar el valor de salida y tiene que ser serializable con JSON. Si una función .NET devuelve `Task` o `void`, un valor `null` se guardará como salida.

### <a name="trigger-sample"></a>Ejemplo de desencadenador

El código de ejemplo siguiente muestra el aspecto que podría tener la función del orquestador más simple "Hola mundo":

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    return `Hello ${name}!`;
});
```

> [!NOTE]
> El objeto `context` de JavaScript no representa a DurableOrchestrationContext sino al [contexto de la función en su conjunto](../functions-reference-node.md#context-object). Puede acceder a los métodos de la orquestación a través de la propiedad `context` del objeto `df`.

> [!NOTE]
> Los orquestadores de JavaScript deben usar `return`. La biblioteca `durable-functions` se encarga de llamar al método `context.done`.

La mayoría de las funciones del orquestador llaman a funciones de actividad, por lo que aquí tenemos un ejemplo de "Hola mundo" que muestra cómo llamar a una función de actividad:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    const result = yield context.df.callActivity("SayHello", name);
    return result;
});
```

## <a name="activity-trigger"></a>Desencadenador de actividad

El desencadenador de actividad le permite crear funciones que las funciones del orquestador llaman, conocidas como [funciones de actividad](durable-functions-types-features-overview.md#activity-functions).

Si usa Visual Studio, el desencadenador de actividad se configura mediante el atributo [ActvityTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) de .NET.

Si utiliza VS Code o Azure Portal para el desarrollo, el desencadenador de actividad está definido por el objeto JSON siguiente en la matriz `bindings` de *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity`es el nombre de la actividad. Este valor es el nombre que utilizan las funciones del orquestador para invocar esta función de actividad. Esta propiedad es opcional. Si no se especifica, se utiliza el nombre de la función.

Internamente este enlace de desencadenador sondea una cola de la cuenta de almacenamiento predeterminado para la aplicación de la función. Esta cola es un detalle de implementación interno de la extensión, esta es la razón de que no se configure explícitamente en las propiedades de enlace.

### <a name="trigger-behavior"></a>Comportamiento de un desencadenador

Estas son algunas notas acerca del desencadenador de actividad:

* **Subprocesamiento**: a diferencia del desencadenador de orquestación, los desencadenadores de actividad no tienen ninguna restricción en relación a los subprocesos o E/S. Se puede tratar como funciones normales.
* **Control de mensajes dudosos**: no hay compatibilidad con mensajes dudosos en los desencadenadores de actividad.
* **Visibilidad de los mensajes**: los mensajes de desencadenador de actividad se quitan de la cola y se mantienen invisibles durante un tiempo configurable. La visibilidad de estos mensajes se renueva automáticamente siempre que la aplicación de la función se esté ejecutando y se mantenga correcta.
* **Valores devueltos**: los valores devueltos se serializan en JSON y se conservan en la tabla de historial de orquestación en Azure Table Storage.

> [!WARNING]
> El back-end de almacenamiento para las funciones de actividad es un detalle de implementación y el código de usuario no debe interactuar directamente con estas entidades de almacenamiento.

### <a name="trigger-usage-net"></a>Uso del desencadenador (.NET)

El enlace de desencadenador de actividad admite entradas y salidas como el desencadenador de orquestación. Estas son algunas cosas que hay que saber acerca del control de entradas y salidas:

* **entradas**: las funciones de actividad de .NET usan de forma nativa [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) como tipo de parámetro. De forma alternativa, una función de actividad puede declararse con cualquier tipo de parámetro que se puede serializar con JSON. Cuando usa `DurableActivityContext`, puede llamar a [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) para capturar y deserializar la entrada de la función de actividad.
* **salidas**: las funciones de actividad admiten valores de salida, así como de entrada. El valor devuelto de la función se utiliza para asignar el valor de salida y tiene que ser serializable con JSON. Si una función .NET devuelve `Task` o `void`, un valor `null` se guardará como salida.
* **metadatos**: las funciones de actividad de .NET pueden enlazar a un parámetro `string instanceId` para obtener el identificador de instancia de la orquestación primaria.

### <a name="trigger-sample"></a>Ejemplo de desencadenador

El ejemplo de código siguiente muestra el aspecto que podría tener una función de actividad simple "Hola mundo":

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

El tipo de parámetro predeterminado para el enlace de .NET `ActivityTriggerAttribute` es `DurableActivityContext`. Sin embargo, los desencadenadores de actividad de .NET también admiten enlazar directamente con tipos serializables con JSON (incluidos los tipos primitivos), por lo que la misma función podría simplificarse como sigue:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
module.exports = async function(context) {
    return `Hello ${context.bindings.name}!`;
};
```

Los enlaces de JavaScript también se pueden pasar como parámetros adicionales, por lo que la misma función podría simplificarse como sigue:

```javascript
module.exports = async function(context, name) {
    return `Hello ${name}!`;
};
```


### <a name="using-input-and-output-bindings"></a>Uso de enlaces de entrada y de salida

Puede usar los enlaces de entrada y salida normales además del enlace de desencadenador de actividad. Por ejemplo, puede tomar la entrada del enlace de actividad y enviar un mensaje a un objeto EventHub mediante el enlace de salida de EventHub:

```json
{
  "bindings": [
    {
      "name": "message",
      "type": "activityTrigger",
      "direction": "in"
    },
    {
      "type": "eventHub",
      "name": "outputEventHubMessage",
      "connection": "EventhubConnectionSetting",
      "eventHubName": "eh_messages",
      "direction": "out"
  }
  ]
}
```

```javascript
module.exports = async function (context) {
    context.bindings.outputEventHubMessage = context.bindings.message;
};
```

## <a name="orchestration-client"></a>Cliente de orquestación

El enlace del cliente de orquestación le permite escribir funciones que interactúan con las funciones del orquestador. Estas funciones se conocen a veces como [funciones de cliente](durable-functions-types-features-overview.md#client-functions). Por ejemplo, puede actuar en las instancias de orquestación de las siguientes formas:

* Iniciarlas.
* Consultar su estado.
* Finalizarlas.
* Enviarles eventos mientras se están ejecutando.
* Purgar del historial de instancias.

Si está utilizando Visual Studio, puede enlazar al cliente de orquestación mediante el atributo [OrchestrationClientAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) de .NET para Durable Functions 1.0. A partir de la versión preliminar de Durable Functions 2.0, puede enlazar con el cliente de orquestación mediante el atributo `DurableClientAttribute` de .NET.

Si utiliza lenguajes de scripting (por ejemplo, archivos *.csx* o *.js*) para desarrollar, el desencadenador de orquestación está definido por el objeto JSON siguiente en la matriz `bindings` de *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "in"
}
```

* `taskHub`: se usa en escenarios donde varias aplicaciones de función comparten la misma cuenta de almacenamiento, pero tienen que estar aisladas entre sí. Si no se especifica, se usa el valor predeterminado `host.json`. Este valor tiene que coincidir con el valor usado por las funciones de orquestador de destino.
* `connectionName`: nombre de una configuración de aplicación que contiene una cadena de conexión de cuenta de almacenamiento. La cuenta de almacenamiento representada por esta cadena de conexión tiene que ser la misma que utilizan las funciones del orquestador de destino. Si no se especifica, se utiliza la cadena de conexión de cuenta de almacenamiento predeterminada de la aplicación de función.

> [!NOTE]
> En la mayoría de los casos, se recomienda omitir estas propiedades y confiar en el comportamiento predeterminado.

### <a name="client-usage"></a>Uso del cliente

En funciones de .NET, habitualmente se enlaza a `DurableOrchestrationClient`, lo que proporciona acceso completo a todas las API de cliente compatibles con Durable Functions. En su lugar, a partir de Durable Functions 2.0, se enlaza a la interfaz `IDurableOrchestrationClient`. En JavaScript, el objeto que se devuelve desde `getClient` expone las mismas API. Las API en el objeto de cliente incluyen:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)
* [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_)
* [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_)
* [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_)

Como alternativa, las funciones de .NET pueden enlazar a `IAsyncCollector<T>` donde `T` es [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) o `JObject`.

Para más información sobre estas operaciones, consulte la documentación de la API [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html).

### <a name="client-sample-visual-studio-development"></a>Ejemplo de cliente (Desarrollo de Visual Studio)

Aquí tenemos un ejemplo de una función desencadenada por la cola que inicia una orquestación "HelloWorld".

```csharp
[FunctionName("QueueStart")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [OrchestrationClient] DurableOrchestrationClient starter)
{
    // Orchestration input comes from the queue message content.
    return starter.StartNewAsync("HelloWorld", input);
}
```

### <a name="client-sample-not-visual-studio"></a>Ejemplo de cliente (no de Visual Studio)

Si no está usando Visual Studio para el desarrollo, puede crear el siguiente archivo *function.json*. En este ejemplo se muestra cómo configurar una función desencadenada por la cola que utiliza el enlace del cliente de orquestación durable:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "queueTrigger",
      "queueName": "durable-function-trigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    }
  ]
}
```

A continuación se encuentran ejemplos específicos del idioma que inician nuevas instancias de función de orquestador.

#### <a name="c-sample"></a>Ejemplo de C#

El ejemplo siguiente muestra cómo utilizar el enlace de cliente de orquestación para iniciar una nueva instancia de función desde una función de script de C#:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static Task<string> Run(string input, DurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

#### <a name="javascript-sample"></a>Ejemplo de JavaScript

El ejemplo siguiente muestra cómo utilizar el enlace de cliente de orquestación durable para iniciar una nueva instancia de función desde una función de JavaScript:

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    return instanceId = await client.startNew("HelloWorld", undefined, context.bindings.input);
};
```

Pueden encontrar más información acerca de cómo iniciar instancias en [Administración de instancias](durable-functions-instance-management.md).

## <a name="entity-trigger"></a>Desencadenador de entidad

Los desencadenadores de entidad permiten crear [funciones de entidad](durable-functions-entities.md). Este desencadenador admite el procesamiento de eventos para una instancia de entidad específica.

Cuando se usa Visual Studio Tools para Azure Functions, el desencadenador de entidad se configura con el atributo `EntityTriggerAttribute` de .NET.

> [!NOTE]
> Los desencadenadores de entidad están disponibles en Durable Functions 2.0 y versiones posteriores. Los desencadenadores de entidad todavía no están disponibles para JavaScript.

Internamente este enlace de desencadenador sondea una serie de colas de la cuenta de almacenamiento predeterminado para la aplicación de la función. Estas colas son detalles de implementación internos de la extensión, esta es la razón de que no se configuren explícitamente en las propiedades de enlace.

### <a name="trigger-behavior"></a>Comportamiento de un desencadenador

Estas son algunas notas sobre el desencadenador de entidad:

* **De un único subproceso**: un único subproceso de distribuidor se utiliza para procesar las operaciones de una entidad determinada. Si se envían varios mensajes a una sola entidad de manera simultánea, las operaciones se procesarán una a una.
* **Control de mensajes dudosos**: no hay compatibilidad con mensajes dudosos en los desencadenadores de entidad.
* **Visibilidad de los mensajes**: los mensajes de desencadenador de entidad se quitan de la cola y se mantienen invisibles durante un tiempo configurable. La visibilidad de estos mensajes se renueva automáticamente siempre que la aplicación de la función se esté ejecutando y se mantenga correcta.
* **Valores devueltos**: las funciones de entidad no admiten valores devueltos. Hay API específicas que se pueden usar para guardar el estado o pasar valores devuelta a las orquestaciones.

Los cambios de estado realizados en una entidad durante su ejecución se conservarán automáticamente una vez completada la ejecución.

### <a name="trigger-usage-net"></a>Uso del desencadenador (.NET)

Cada función de entidad tiene un tipo de parámetro de `IDurableEntityContext`, que tiene los miembros siguientes:

* **EntityName**: obtiene el nombre de la entidad que se está ejecutando actualmente.
* **EntityKey**: obtiene la clave de la entidad que se está ejecutando actualmente.
* **EntityId**: obtiene el identificador de la entidad que se está ejecutando actualmente.
* **OperationName**: obtiene el nombre de la operación actual.
* **IsNewlyConstructed**: devuelve `true` si la entidad no existía antes de la operación.
* **GetState\<TState>()** : obtiene el estado actual de la entidad. El parámetro `TState` debe ser un tipo primitivo o serializable de JSON.
* **SetState(objetct)** : actualiza el estado de la entidad. El parámetro `object` debe ser un objeto primitivo o serializable de JSON.
* **GetInput\<TInput>()** : obtiene la entrada para la operación actual. El parámetro de tipo `TInput` debe representar un tipo primitivo o serializable de JSON.
* **Return(object)** : devuelve un valor a la orquestación que ha llamado a la operación. El parámetro `object` debe ser un objeto primitivo o serializable de JSON.
* **DestructOnExit()** : elimina la entidad después de finalizar la operación actual.
* **SignalEntity(EntityId, string, object)** : envía un mensaje unidireccional a una entidad. El parámetro `object` debe ser un objeto primitivo o serializable de JSON.

Al usar el modo de programación de entidades basadas en clases, se puede hacer referencia al objeto`IDurableEntityContext` con la propiedad de subproceso estático `Entity.Current`.

### <a name="trigger-sample---entity-function"></a>Ejemplo de desencadenador: función de entidad

El código siguiente es un ejemplo de una entidad *Counter* simple implementada como una función estándar. Esta función define tres *operaciones* (`add`, `reset` y `get`), cada una de las cuales opera en un valor de estado entero, `currentValue`.

```csharp
[FunctionName(nameof(Counter))]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();

    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            currentValue += operand;
            break;
        case "reset":
            currentValue = 0;
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }

    ctx.SetState(currentValue);
}
```

### <a name="trigger-sample---entity-class"></a>Ejemplo de desencadenador: clase de entidad

El ejemplo siguiente es una implementación equivalente de la entidad `Counter` anterior que usa clases y métodos .NET.

```csharp
public class Counter
{
    [JsonProperty("value")]
    public int CurrentValue { get; set; }

    public void Add(int amount) => this.CurrentValue += amount;
    
    public void Reset() => this.CurrentValue = 0;
    
    public int Get() => this.CurrentValue;

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

> [!NOTE]
> El método de punto de entrada de la función con el atributo `[FunctionName]` se *debe* declarar `static` cuando se usan clases de entidad. Los métodos de punto de entrada no estáticos pueden dar lugar a la inicialización de varios objetos y, potencialmente, a otros comportamientos no definidos.

Las clases de entidad tienen mecanismos especiales para interactuar con los enlaces y la inserción de dependencias de .NET. Para más información, consulte el artículo sobre las [entidades duraderas](durable-functions-entities.md).

## <a name="entity-client"></a>Cliente de entidad

El enlace de cliente de entidad permite desencadenar de manera asincrónica las [funciones de entidad](#entity-trigger). Estas funciones se conocen a veces como [funciones de cliente](durable-functions-types-features-overview.md#client-functions).

Si usa Visual Studio, puede enlazar con el cliente de entidad mediante el atributo `DurableClientAttribute` de .NET.

> [!NOTE]
> `[DurableClientAttribute]` también se puede usar para enlazar con el [cliente de orquestación](#orchestration-client).

Si utiliza lenguajes de scripting (por ejemplo, archivos *.csx* o *.js*) para desarrollar, el desencadenador de entidad está definido por el objeto JSON siguiente en la matriz `bindings` de *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "durableClient",
    "direction": "out"
}
```

* `taskHub`: se usa en escenarios donde varias aplicaciones de función comparten la misma cuenta de almacenamiento, pero tienen que estar aisladas entre sí. Si no se especifica, se usa el valor predeterminado `host.json`. Este valor tiene que coincidir con el valor usado por las funciones de entidad de destino.
* `connectionName`: nombre de una configuración de aplicación que contiene una cadena de conexión de cuenta de almacenamiento. La cuenta de almacenamiento representada por esta cadena de conexión tiene que ser la misma que utilizan las funciones de entidad de destino. Si no se especifica, se utiliza la cadena de conexión de cuenta de almacenamiento predeterminada de la aplicación de función.

> [!NOTE]
> En la mayoría de los casos, se recomienda omitir las propiedades opcionales y confiar en el comportamiento predeterminado.

### <a name="entity-client-usage"></a>Uso del cliente de entidad

En funciones de .NET, habitualmente se enlaza a `IDurableEntityClient`, lo que proporciona acceso completo a todas las API de cliente compatibles con las entidades duraderas. También puede enlazar a la interfaz `IDurableClient`, que proporciona acceso a las API de cliente, tanto para las entidades como para las orquestaciones. Las API en el objeto de cliente incluyen:

* **ReadEntityStateAsync\<T >** : lee el estado de una entidad.
* **SignalEntityAsync**: envía un mensaje unidireccional a una entidad y espera a que se ponga en cola.
* **SignalEntityAsync\<TEntityInterface>** : igual que `SignalEntityAsync`, pero usa un objeto proxy generado de tipo `TEntityInterface`.
* **CreateEntityProxy\<TEntityInterface>** : genera dinámicamente un proxy dinámico de tipo `TEntityInterface` para realizar llamadas con seguridad de tipos a las entidades.

> [!NOTE]
> Es importante comprender que las operaciones de "señal" anteriores son asincrónicas. No es posible invocar una función de entidad y obtener un valor devuelto de un cliente. Del mismo modo, es posible que `SignalEntityAsync` se devuelva antes de que la entidad empiece a ejecutar la operación. Solo las funciones del orquestador pueden invocar funciones de entidad de manera sincrónica y procesar los valores devueltos.

Las API `SignalEntityAsync` requieren que se especifique el identificador único de la entidad como `EntityId`. Estas API también toman de manera opcional el nombre de la operación de entidad como `string` y la carga útil de la operación como `object` serializable de JSON. Si la entidad de destino no existe, se creará automáticamente con el identificador de entidad especificado.

### <a name="client-sample-untyped"></a>Ejemplo de cliente (sin tipo)

Este es un ejemplo de una función desencadenada por la cola que invoca una entidad "Counter".

```csharp
[FunctionName("AddFromQueue")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [DurableClient] IDurableEntityClient client)
{
    // Entity operation input comes from the queue message content.
    var entityId = new EntityId(nameof(Counter), "myCounter");
    int amount = int.Parse(input);
    return client.SignalEntityAsync(entityId, "Add", amount);
}
```

### <a name="client-sample-typed"></a>Ejemplo de cliente (con tipo)

Es posible generar un objeto proxy para el acceso con seguridad de tipos a las operaciones de entidad. Para generar un proxy con seguridad de tipos, el tipo de entidad debe implementar una interfaz. Por ejemplo, supongamos que la entidad `Counter` mencionada anteriormente implementó una interfaz `ICounter`, definida de la siguiente manera:

```csharp
public interface ICounter
{
    void Add(int amount);
    void Reset();
    int Get();
}

public class Counter : ICounter
{
    // ...
}
```

El código de cliente podría usar `SignalEntityAsync<TEntityInterface>` y especificar la interfaz `ICounter` como parámetro de tipo para generar un proxy con seguridad de tipos. Este uso de proxies con seguridad de tipos se muestra en el ejemplo de código siguiente:

```csharp
[FunctionName("UserDeleteAvailable")]
public static async Task AddValueClient(
    [QueueTrigger("my-queue")] string message,
    [DurableClient] IDurableEntityClient client)
{
    var target = new EntityId(nameof(Counter), "myCounter");
    int amount = int.Parse(message);
    await client.SignalEntityAsync<ICounter>(target, proxy => proxy.Add(amount));
}
```

En el ejemplo anterior, el parámetro `proxy` es una instancia generada dinámicamente de `ICounter`, que traslada internamente la llamada a `Add` a la llamada equivalente (sin tipo) a `SignalEntityAsync`.

Hay algunas reglas para definir las interfaces de entidad:

* El parámetro de tipo `TEntityInterface` en `SignalEntityAsync<TEntityInterface>` debe ser una interfaz.
* Las interfaces de entidad solo deben definir métodos.
* Los métodos de interfaz de entidad no deben definir más de un parámetro.
* Los métodos de interfaz de entidad deben devolver `void`, `Task` o `Task<T>`, donde `T` es algún valor devuelto.
* Las interfaces de entidad deben tener exactamente una clase de implementación concreta dentro del mismo ensamblado (es decir, la clase de entidad).

Si se infringe alguna de estas reglas, se iniciará una excepción `InvalidOperationException` en tiempo de ejecución. En el mensaje de excepción se explicará qué regla se infringió.

> [!NOTE]
> Las API `SignalEntityAsync` representan operaciones unidireccionales. Si una interfaz de identidad devuelve `Task<T>`, el valor del parámetro `T` siempre será NULL o `default`.

<a name="host-json"></a>

## <a name="hostjson-settings"></a>configuración de host.json

[!INCLUDE [durabletask](../../../includes/functions-host-json-durabletask.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de la API HTTP integrada para la administración de instancias](durable-functions-http-api.md)