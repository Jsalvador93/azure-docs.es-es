---
title: Reglas y grupos de reglas de CRS de firewall de aplicaciones web de Azure Application Gateway
description: Esta página proporciona información sobre las reglas y grupos de reglas de CRS de firewall de aplicaciones web.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 4/11/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 9f90f373bd6f1cfd34de1605783bf3a7f0185f4c
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240337"
---
# <a name="web-application-firewall-crs-rule-groups-and-rules"></a>Reglas y grupos de reglas de CRS del firewall de aplicaciones web

El firewall de aplicaciones web de Application Gateway protege las aplicaciones web de las vulnerabilidades más habituales. Esta operación se realiza mediante reglas que se definen en función de las versiones 3.0 o 2.2.9 de OWASP Core Rule Set. Estas reglas se pueden deshabilitar individualmente. En este artículo se incluyen las reglas y los conjuntos de reglas actuales que se ofrecen.

Los grupos de reglas y reglas siguientes están disponibles al usar Application Gateway con el firewall de aplicaciones web.

# <a name="owasp-30tabowasp3"></a>[OWASP 3.0](#tab/owasp3)

## <a name="owasp30"></a> Conjuntos de reglas

### <a name="General"></a> <p x-ms-format-detection="none">General</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|200004|Posible límite sin coincidencia con varias partes.|

### <a name="crs911"></a> <p x-ms-format-detection="none">REQUEST-911-METHOD-ENFORCEMENT</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|911100|Método no permitido por la directiva|


### <a name="crs913"></a> <p x-ms-format-detection="none">REQUEST-913-SCANNER-DETECTION</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|913100|Se ha encontrado un agente de usuario asociado a un examen de seguridad.|
|913110|Se ha encontrado un encabezado de solicitud asociado a un examen de seguridad.|
|913120|Se ha encontrado un argumento o nombre de archivo de solicitud asociado a un examen de seguridad.|
|913101|Se ha encontrado un agente de usuario asociado al cliente HTTP genérico o de scripts.|
|913102|Se ha encontrado un agente de usuario asociado a un robot o agente de búsqueda.|

### <a name="crs920"></a> <p x-ms-format-detection="none">REQUEST-920-PROTOCOL-ENFORCEMENT</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|920100|Línea de solicitud HTTP no válida|
|920130|Error al analizar el cuerpo de la solicitud|
|920140|Error de validación estricta del cuerpo de la solicitud de varias partes|
|920160|El encabezado Content-Length HTTP no es numérico.|
|920170|Solicitud GET o HEAD con contenido del cuerpo|
|920180|Falta el encabezado Content-Length en la solicitud POST.|
|920190|Intervalo = Último valor de bytes no válido|
|920210|Se han encontrado múltiples datos de encabezado de conexión en conflicto.|
|920220|Intento de ataque de abuso de codificación de direcciones URL|
|920240|Intento de ataque de abuso de codificación de direcciones URL|
|920250|Intento de ataque de abuso de codificación UTF8|
|920260|Intento de ataque de abuso de caracteres de ancho medio y ancho completo Unicode|
|920270|Carácter no válido en la solicitud (carácter nulo)|
|920280|Falta un encabezado host en la solicitud.|
|920290|Encabezado host vacío|
|920310|La solicitud tiene un encabezado de aceptación (Accept) vacío.|
|920311|La solicitud tiene un encabezado de aceptación (Accept) vacío.|
|920330|Encabezado de agente de usuario vacío|
|920340|La solicitud tiene contenido, pero falta el encabezado Content-Type.|
|920350|El encabezado host es una dirección IP numérica.|
|920380|Hay demasiados argumentos en la solicitud.|
|920360|Nombre de argumento demasiado largo|
|920370|Valor de argumento demasiado largo|
|920390|Se ha superado el tamaño total de argumentos.|
|920400|El tamaño del archivo cargado es demasiado grande.|
|920410|El tamaño total de los archivos cargados es demasiado grande.|
|920420|Tipo de contenido de solicitud no permitido por una directiva|
|920430|Versión del protocolo HTTP no permitida por una directiva|
|920440|Extensión de archivo URL restringida por una directiva|
|920450|Encabezado HTTP restringido por una directiva (%@{MATCHED_VAR})|
|920200|Intervalo = Demasiados campos (6 o más)|
|920201|Intervalo = Demasiados campos para solicitud PDF (35 o más)|
|920230|Varias codificaciones de direcciones URL detectadas|
|920300|Falta un encabezado de aceptación (Accept) en la solicitud.|
|920271|Carácter no válido en la solicitud (caracteres no imprimibles)|
|920320|Falta el encabezado de agente de usuario.|
|920272|Carácter no válido en la solicitud (fuera de los caracteres imprimibles, por debajo de ASCII 127)|
|920202|Intervalo = Demasiados campos para solicitud PDF (6 o más)|
|920273|Carácter no válido en la solicitud (fuera de conjunto muy estricto)|
|920274|Carácter no válido en encabezados de solicitud (fuera de conjunto muy estricto)|
|920460|Caracteres de escape anómalos|

### <a name="crs921"></a> <p x-ms-format-detection="none">REQUEST-921-PROTOCOL-ATTACK</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|921100|Ataque de contrabando de solicitudes HTTP|
|921110|Ataque de contrabando de solicitudes HTTP|
|921120|Ataque de división de respuestas HTTP|
|921130|Ataque de división de respuestas HTTP|
|921140|Ataque por inyección de encabezado HTTP a través de encabezados|
|921150|Ataque por inyección de encabezado HTTP a través de la carga (Retorno de carro / Avance de línea detectado)|
|921160|Ataque por inyección de encabezado HTTP a través de la carga (Retorno de carro/Avance de línea y header-name detectados)|
|921151|Ataque por inyección de encabezado HTTP a través de la carga (Retorno de carro / Avance de línea detectado)|
|921170|Polución de parámetros HTTP|
|921180|Polución de parámetros HTTP (%@{TX.1})|

### <a name="crs930"></a> <p x-ms-format-detection="none">REQUEST-930-APPLICATION-ATTACK-LFI</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|930100|Ataque punto punto barra (/.. /)|
|930110|Ataque punto punto barra (/.. /)|
|930120|Intento de acceso a archivo del sistema operativo|
|930130|Intento de acceso a archivo restringido|

### <a name="crs931"></a> <p x-ms-format-detection="none">REQUEST-931-APPLICATION-ATTACK-RFI</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|931100|Posible ataque remoto de inclusión de archivos (RFI) = El parámetro de dirección URL utiliza una dirección IP|
|931110|Posible ataque remoto de inclusión de archivos (RFI) = Nombre de parámetro vulnerable de RFI común utilizado con carga de dirección URL|
|931120|Posible ataque remoto de inclusión de archivos (RFI) = Carga de dirección URL utilizada con carácter de interrogación de cierre (?)|
|931130|Posible ataque remoto de inclusión de archivos (RFI) = Referencia o vínculo fuera del dominio|

### <a name="crs932"></a> <p x-ms-format-detection="none">REQUEST-932-APPLICATION-ATTACK-RCE</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|932120|Ejecución de comando remoto = Comando de Windows PowerShell encontrado|
|932130|Ejecución de comando remoto = Expresión de shell de Unix encontrada|
|932140|Ejecución de comando remoto = Comando FOR/IF de Windows encontrado|
|932160|Ejecución de comando remoto = Código de shell de Unix encontrado|
|932170|Ejecución de comando remoto = Shellshock (CVE-2014-6271)|
|932171|Ejecución de comando remoto = Shellshock (CVE-2014-6271)|

### <a name="crs933"></a> <p x-ms-format-detection="none">REQUEST-933-APPLICATION-ATTACK-PHP</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|933100|Ataque por inyección en PHP = Se ha encontrado etiqueta de apertura o cierre|
|933110|Ataque por inyección en PHP = Se ha encontrado carga de archivo de script PHP|
|933120|Ataque por inyección en PHP = Se ha encontrado directiva de configuración|
|933130|Ataque por inyección en PHP = Se han encontrado variables|
|933150|Ataque por inyección en PHP = Se ha encontrado nombre de función de PHP de alto riesgo|
|933160|Ataque por inyección en PHP = Se ha encontrado llamada de función de PHP de alto riesgo|
|933180|Ataque por inyección en PHP = Se ha encontrado llamada de función de variable|
|933151|Ataque por inyección en PHP = Se ha encontrado nombre de función de PHP de riesgo medio|
|933131|Ataque por inyección en PHP = Se han encontrado variables|
|933161|Ataque por inyección en PHP = Se ha encontrado llamada de función de PHP de valor bajo|
|933111|Ataque por inyección en PHP = Se ha encontrado carga de archivo de script PHP|

### <a name="crs941"></a> <p x-ms-format-detection="none">REQUEST-941-APPLICATION-ATTACK-XSS</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|941100|Ataque XSS detectado mediante libinjection|
|941110|Filtro XSS - Categoría 1 = Vector de etiqueta de script|
|941130|Filtro XSS - Categoría 3 = Vector de atributo|
|941140|Filtro XSS - Categoría 4 = Vector URI de JavaScript|
|941150|Filtro XSS - Categoría 5 = Atributos HTML no permitidos|
|941180|Palabras clave de lista negra de node-validator|
|941190|XSS mediante hojas de estilos|
|941200|XSS mediante fotogramas VML|
|941210|XSS mediante Javascript ofuscado|
|941220|XSS mediante VBScript ofuscado|
|941230|XSS mediante etiqueta "embed"|
|941240|XSS mediante el atributo "import" o "implementación"|
|941260|XSS mediante la etiqueta "meta"|
|941270|XSS mediante href "link"|
|941280|XSS mediante la etiqueta "base"|
|941290|XSS mediante la etiqueta "applet"|
|941300|XSS mediante la etiqueta "object"|
|941310|Filtro XSS de codificación mal formulada US-ASCII: ataque detectado|
|941330|Filtros XSS de IE: ataque detectado|
|941340|Filtros XSS de IE: ataque detectado|
|941350|XSS de IE con codificación UTF-7: ataque detectado|
|941320|Posible ataque XSS detectado: controlador de etiquetas HTML|

### <a name="crs942"></a> <p x-ms-format-detection="none">REQUEST-942-APPLICATION-ATTACK-SQLI</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|942100|Ataque por inyección de código SQL detectado mediante libinjection|
|942110|Ataque por inyección de código SQL: Pruebas de inyección de código detectadas|
|942130|Ataque por inyección de código SQL: Tautología de SQL detectada.|
|942140|Ataque por inyección de código SQL = nombres de base de datos comunes detectados|
|942160|Detección de pruebas de inyección de código SQL a ciegas mediante sleep() o benchmark()|
|942170|Detección de intentos de inyección con las funciones benchmark y sleep que incluyen consultas condicionales|
|942190|Detecta la ejecución de código MSSQL y los intentos de recopilación de información|
|942200|Detecta inyecciones de código MySQL cuyo espacio o comentarios resultan confusos y terminaciones con el carácter de acento grave|
|942230|Detección de intentos de inyección de código SQL condicionales|
|942260|Detecta intentos básicos de omisión de la autenticación SQL (2/3)|
|942270|Búsqueda de inyección de código SQL básico Cadena de ataque común para Oracle MySQL y otras bases de datos|
|942290|Búsqueda de intentos de inyección de código SQL MongoDB básico|
|942300|Detecta comentarios, condiciones e inyecciones de caracteres de MySQL|
|942310|Detecta los intentos de inyección de SQL encadenado 2/2|
|942320|Detección de inyecciones de funciones o procedimientos almacenados de MySQL y PostgreSQL|
|942330|Detecta sondeos clásicos de inyección de código SQL (1/2)|
|942340|Detecta intentos básicos de omisión de la autenticación SQL (3/3)|
|942350|Detección de intentos de inyección de UDF MySQL y otras manipulaciones de datos o estructuras|
|942360|Detecta intentos concatenados de SQLLFI e inyecciones de código SQL básicas|
|942370|Detecta sondeos clásicos de inyección de código SQL (2/2)|
|942150|Ataque por inyección de código SQL|
|942410|Ataque por inyección de código SQL|
|942430|Restringe la detección de anomalías de caracteres de SQL (args): número de caracteres especiales que se han excedido (12)|
|942440|Secuencia de comentario SQL detectada|
|942450|Codificación hexadecimal de SQL identificada|
|942251|Detección de inyecciones HAVING|
|942460|Alerta de detección de anomalías de metacaracteres: repetición de caracteres que no se usan en las palabras|

### <a name="crs943"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|943100|Posible ataque de fijación de sesión = Definición de valores de cookies en HTML|
|943110|Posible ataque de fijación de sesión = Nombre del parámetro SessionID con origen de referencia fuera del dominio|
|943120|Posible ataque de fijación de sesión = Nombre del parámetro SessionID con origen de referencia fuera del dominio|

# <a name="owasp-229tabowasp2"></a>[OWASP 2.2.9](#tab/owasp2)

## <a name="owasp229"></a> Conjuntos de reglas

### <a name="crs20"></a> crs_20_protocol_violations

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|960911|Línea de solicitud HTTP no válida|
|981227|Error de Apache = URI no válido en la solicitud|
|960912|Error al analizar el cuerpo de la solicitud|
|960914|Error de validación estricta del cuerpo de la solicitud de varias partes|
|960915|El analizador de varias partes ha detectado un posible límite no coincidente.|
|960016|El encabezado Content-Length HTTP no es numérico.|
|960011|Solicitud GET o HEAD con contenido del cuerpo|
|960012|Falta el encabezado Content-Length en la solicitud POST.|
|960902|Uso no válido de codificación de identidades|
|960022|Encabezado esperado no permitido para HTTP 1.0|
|960020|El encabezado Pragma requiere el encabezado Cache-Control para las solicitudes HTTP/1.1.|
|958291|Intervalo = El campo existe y comienza por 0|
|958230|Intervalo = Último valor de bytes no válido|
|958295|Se han encontrado múltiples datos de encabezado de conexión en conflicto.|
|950107|Intento de ataque de abuso de codificación de direcciones URL|
|950109|Varias codificaciones de direcciones URL detectadas|
|950108|Intento de ataque de abuso de codificación de direcciones URL|
|950801|Intento de ataque de abuso de codificación UTF8|
|950116|Intento de ataque de abuso de caracteres de ancho medio y ancho completo Unicode|
|960901|Carácter no válido en la solicitud|
|960018|Carácter no válido en la solicitud|

### <a name="crs21"></a> crs_21_protocol_anomalies

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|960008|Falta un encabezado host en la solicitud.|
|960007|Encabezado host vacío|
|960015|Falta un encabezado de aceptación (Accept) en la solicitud.|
|960021|La solicitud tiene un encabezado de aceptación (Accept) vacío.|
|960009|Falta un encabezado de agente de usuario en la solicitud|
|960006|Encabezado de agente de usuario vacío|
|960904|La solicitud tiene contenido, pero falta el encabezado Content-Type.|
|960017|El encabezado host es una dirección IP numérica.|

### <a name="crs23"></a> crs_23_request_limits

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|960209|Nombre de argumento demasiado largo|
|960208|Valor de argumento demasiado largo|
|960335|Hay demasiados argumentos en la solicitud.|
|960341|Se ha superado el tamaño total de argumentos.|
|960342|El tamaño del archivo cargado es demasiado grande.|
|960343|El tamaño total de los archivos cargados es demasiado grande.|

### <a name="crs30"></a> crs_30_http_policy

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|960032|Método no permitido por la directiva|
|960010|Tipo de contenido de solicitud no permitido por una directiva|
|960034|Versión del protocolo HTTP no permitida por una directiva|
|960035|Extensión de archivo URL restringida por una directiva|
|960038|Encabezado HTTP restringido por una directiva|

### <a name="crs35"></a> crs_35_bad_robots

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|990002|La solicitud indica que un examen de seguridad ha analizado el sitio|
|990901|La solicitud indica que un examen de seguridad ha analizado el sitio|
|990902|La solicitud indica que un examen de seguridad ha analizado el sitio|
|990012|Agente de búsqueda de sitios web malintencionados|

### <a name="crs40"></a>crs_40_generic_attacks

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|960024|Alerta de detección de anomalías de metacaracteres: repetición de caracteres que no se usan en las palabras|
|950008|Inyección de etiquetas de ColdFusion no documentadas|
|950010|Ataque por inyección de LDAP|
|950011|Ataque por inyección de SSI|
|950018|Dirección URL de XSS de PDF universal detectada|
|950019|Ataque por inyección de correo electrónico|
|950012|Ataque de contrabando de solicitudes HTTP|
|950910|Ataque de división de respuestas HTTP|
|950911|Ataque de división de respuestas HTTP|
|950117|Ataque remoto de inclusión de archivos|
|950118|Ataque remoto de inclusión de archivos|
|950119|Ataque remoto de inclusión de archivos|
|950120|Posible ataque remoto de inclusión de archivos (RFI) = Referencia o vínculo fuera del dominio|
|981133|Regla 981133|
|981134|Regla 981134|
|950009|Ataque de fijación de sesión|
|950003|Fijación de sesión|
|950000|Fijación de sesión|
|950005|Intento de acceso a archivo remoto|
|950002|Acceso a comando del sistema|
|950006|Inyección de comando del sistema|
|959151|Ataque por inyección en PHP|
|958976|Ataque por inyección en PHP|
|958977|Ataque por inyección en PHP|

### <a name="crs41sql"></a> crs_41_sql_injection_attacks

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|981231|Secuencia de comentario SQL detectada|
|981260|Codificación hexadecimal de SQL identificada|
|981320|Ataque por inyección de código SQL = nombres de base de datos comunes detectados|
|981300|Regla 981300|
|981301|Regla 981301|
|981302|Regla 981302|
|981303|Regla 981303|
|981304|Regla 981304|
|981305|Regla 981305|
|981306|Regla 981306|
|981307|Regla 981307|
|981308|Regla 981308|
|981309|Regla 981309|
|981310|Regla 981310|
|981311|Regla 981311|
|981312|Regla 981312|
|981313|Regla 981313|
|981314|Regla 981314|
|981315|Regla 981315|
|981316|Regla 981316|
|981317|Alerta de detección de anomalías de instrucción SELECT SQL|
|950007|Ataque por inyección de código SQL a ciegas|
|950001|Ataque por inyección de código SQL|
|950908|Ataque por inyección de código SQL|
|959073|Ataque por inyección de código SQL|
|981272|Detección de pruebas de inyección de código SQL a ciegas mediante sleep() o benchmark()|
|981250|Detección de intentos de inyección con las funciones benchmark y sleep que incluyen consultas condicionales|
|981241|Detección de intentos de inyección de código SQL condicionales|
|981276|Búsqueda de inyección de código SQL básico Cadena de ataque común para Oracle MySQL y otras bases de datos|
|981270|Búsqueda de intentos de inyección de código SQL MongoDB básico|
|981253|Detección de inyecciones de funciones o procedimientos almacenados de MySQL y PostgreSQL|
|981251|Detección de intentos de inyección de UDF MySQL y otras manipulaciones de datos o estructuras|

### <a name="crs41xss"></a> crs_41_xss_attacks

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|973336|Filtro XSS - Categoría 1 = Vector de etiqueta de script|
|973338|Filtro XSS - Categoría 3 = Vector URI de JavaScript|
|981136|Regla 981136|
|981018|Regla 981018|
|958016|Ataque de scripts de sitios (XSS)|
|958414|Ataque de scripts de sitios (XSS)|
|958032|Ataque de scripts de sitios (XSS)|
|958026|Ataque de scripts de sitios (XSS)|
|958027|Ataque de scripts de sitios (XSS)|
|958054|Ataque de scripts de sitios (XSS)|
|958418|Ataque de scripts de sitios (XSS)|
|958034|Ataque de scripts de sitios (XSS)|
|958019|Ataque de scripts de sitios (XSS)|
|958013|Ataque de scripts de sitios (XSS)|
|958408|Ataque de scripts de sitios (XSS)|
|958012|Ataque de scripts de sitios (XSS)|
|958423|Ataque de scripts de sitios (XSS)|
|958002|Ataque de scripts de sitios (XSS)|
|958017|Ataque de scripts de sitios (XSS)|
|958007|Ataque de scripts de sitios (XSS)|
|958047|Ataque de scripts de sitios (XSS)|
|958410|Ataque de scripts de sitios (XSS)|
|958415|Ataque de scripts de sitios (XSS)|
|958022|Ataque de scripts de sitios (XSS)|
|958405|Ataque de scripts de sitios (XSS)|
|958419|Ataque de scripts de sitios (XSS)|
|958028|Ataque de scripts de sitios (XSS)|
|958057|Ataque de scripts de sitios (XSS)|
|958031|Ataque de scripts de sitios (XSS)|
|958006|Ataque de scripts de sitios (XSS)|
|958033|Ataque de scripts de sitios (XSS)|
|958038|Ataque de scripts de sitios (XSS)|
|958409|Ataque de scripts de sitios (XSS)|
|958001|Ataque de scripts de sitios (XSS)|
|958005|Ataque de scripts de sitios (XSS)|
|958404|Ataque de scripts de sitios (XSS)|
|958023|Ataque de scripts de sitios (XSS)|
|958010|Ataque de scripts de sitios (XSS)|
|958411|Ataque de scripts de sitios (XSS)|
|958422|Ataque de scripts de sitios (XSS)|
|958036|Ataque de scripts de sitios (XSS)|
|958000|Ataque de scripts de sitios (XSS)|
|958018|Ataque de scripts de sitios (XSS)|
|958406|Ataque de scripts de sitios (XSS)|
|958040|Ataque de scripts de sitios (XSS)|
|958052|Ataque de scripts de sitios (XSS)|
|958037|Ataque de scripts de sitios (XSS)|
|958049|Ataque de scripts de sitios (XSS)|
|958030|Ataque de scripts de sitios (XSS)|
|958041|Ataque de scripts de sitios (XSS)|
|958416|Ataque de scripts de sitios (XSS)|
|958024|Ataque de scripts de sitios (XSS)|
|958059|Ataque de scripts de sitios (XSS)|
|958417|Ataque de scripts de sitios (XSS)|
|958020|Ataque de scripts de sitios (XSS)|
|958045|Ataque de scripts de sitios (XSS)|
|958004|Ataque de scripts de sitios (XSS)|
|958421|Ataque de scripts de sitios (XSS)|
|958009|Ataque de scripts de sitios (XSS)|
|958025|Ataque de scripts de sitios (XSS)|
|958413|Ataque de scripts de sitios (XSS)|
|958051|Ataque de scripts de sitios (XSS)|
|958420|Ataque de scripts de sitios (XSS)|
|958407|Ataque de scripts de sitios (XSS)|
|958056|Ataque de scripts de sitios (XSS)|
|958011|Ataque de scripts de sitios (XSS)|
|958412|Ataque de scripts de sitios (XSS)|
|958008|Ataque de scripts de sitios (XSS)|
|958046|Ataque de scripts de sitios (XSS)|
|958039|Ataque de scripts de sitios (XSS)|
|958003|Ataque de scripts de sitios (XSS)|
|973300|Posible ataque XSS detectado: controlador de etiquetas HTML|
|973301|Ataque XSS detectado|
|973302|Ataque XSS detectado|
|973303|Ataque XSS detectado|
|973304|Ataque XSS detectado|
|973305|Ataque XSS detectado|
|973306|Ataque XSS detectado|
|973307|Ataque XSS detectado|
|973308|Ataque XSS detectado|
|973309|Ataque XSS detectado|
|973311|Ataque XSS detectado|
|973313|Ataque XSS detectado|
|973314|Ataque XSS detectado|
|973331|Filtros XSS de IE: ataque detectado|
|973315|Filtros XSS de IE: ataque detectado|
|973330|Filtros XSS de IE: ataque detectado|
|973327|Filtros XSS de IE: ataque detectado|
|973326|Filtros XSS de IE: ataque detectado|
|973346|Filtros XSS de IE: ataque detectado|
|973345|Filtros XSS de IE: ataque detectado|
|973324|Filtros XSS de IE: ataque detectado|
|973323|Filtros XSS de IE: ataque detectado|
|973348|Filtros XSS de IE: ataque detectado|
|973321|Filtros XSS de IE: ataque detectado|
|973320|Filtros XSS de IE: ataque detectado|
|973318|Filtros XSS de IE: ataque detectado|
|973317|Filtros XSS de IE: ataque detectado|
|973329|Filtros XSS de IE: ataque detectado|
|973328|Filtros XSS de IE: ataque detectado|

### <a name="crs42"></a> crs_42_tight_security

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|950103|Ataque punto punto barra|

### <a name="crs45"></a> crs_45_trojans

|Identificador de regla|DESCRIPCIÓN|
|---|---|
|950110|Acceso a puerta trasera|
|950921|Acceso a puerta trasera|
|950922|Acceso a puerta trasera|

---

## <a name="next-steps"></a>Pasos siguientes

Aprenda a deshabilitar reglas de WAF: [Personalización de las reglas de WAF](application-gateway-customize-waf-rules-portal.md)
