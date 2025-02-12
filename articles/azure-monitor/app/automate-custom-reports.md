---
title: Automatizar informes personalizados con datos de Azure Application Insights
description: Automatizar informes personalizados diarios, semanales o mensuales con datos de Azure Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/20/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 3becf5ef579acdc52a51f9ad618e37460491c2ec
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71146742"
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Automatizar informes personalizados con datos de Azure Application Insights

Los informes periódicos ayudan a mantener informado al equipo sobre cómo van sus servicios críticos para la empresa. Los desarrolladores, los equipos de DevOps o SRE y sus administradores pueden ser productivos con informes automatizados que proporcionan información de forma fiable sin necesidad de que todos los usuarios inicien sesión en el portal. Estos informes también pueden ayudar a identificar aumentos graduales de las latencias y las velocidades de carga o error que es posible que no desencadenen ninguna regla de alertas.

Cada empresa tiene sus necesidades de informes únicas, como: 

* Agregaciones concretas de percentil de métricas o métricas personalizadas en un informe.
* Tener diferentes informes para los resúmenes diarios, semanales y mensuales de datos para los distintos destinatarios.
* Segmentación por atributos personalizados como la región o el entorno. 
* Agrupar algunos recursos de IA en un único informe, incluso si pueden estar en distintas suscripciones o grupos de recursos, etc.
* Informes independientes que contienen métricas confidenciales que se envían a un público selectivo.
* Informes para las partes interesadas que es posible que no tengan acceso a los recursos del portal.

> [!NOTE] 
> El correo electrónico de resumen semanal de Application Insights no permitía ninguna personalización y se suspenderá en favor de las opciones personalizadas que se muestran a continuación. El último correo electrónico de resumen semanal se enviará el 11 de junio de 2018. Configure una de las opciones siguientes para obtener informes personalizados similares (use la consulta que se sugiere a continuación).

## <a name="to-automate-custom-report-emails"></a>Para automatizar los correos electrónicos de informe personalizados

Puede [consultar mediante programación datos de Application Insights](https://dev.applicationinsights.io/) para generar informes personalizados en una programación. Las opciones siguientes pueden ayudarle a empezar rápidamente:

* [Automatizar informes con Microsoft Flow](automate-with-flow.md)
* [Automatizar informes con Logic Apps](automate-with-logic-apps.md)
* Use la plantilla "Resumen programado de Application Insights" de [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) en el escenario Supervisión. Esta función usa SendGrid para enviar el correo electrónico. 

    ![Plantilla de Azure Functions](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Consulta de ejemplo para un correo electrónico de resumen semanal
La consulta siguiente muestra la unión de varios conjuntos de datos para un correo electrónico de resumen semanal, por ejemplo, un informe. Personalícela según sea necesario y úsela con cualquiera de las opciones enumeradas anteriormente para automatizar un informe semanal.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

## <a name="application-insights-scheduled-digest-report"></a>Informe de resumen programado de Application Insights

1. En Azure Portal, seleccione **Crear un recurso** > **Compute** > **Function App**.

   ![Captura de pantalla de la creación de una aplicación Function App de recursos de Azure](./media/automate-custom-reports/function-app-01.png)

2. Escriba la información correspondiente para la aplicación y seleccione _Crear_. (La _activación_ de Application Insights solo es necesaria si quiere supervisar la nueva aplicación Function App con Application Insights).

   ![Captura de pantalla de la configuración para crear una aplicación Function App de recursos de Azure](./media/automate-custom-reports/function-app-02.png)

3. Una vez que la nueva aplicación Function App haya completado la implementación, seleccione **Ir al recurso**.

4. Seleccione **Nueva función**.

   ![Captura de pantalla para crear una nueva función](./media/automate-custom-reports/function-app-03.png)

5. Seleccione la **_plantilla de resumen programada de Application Insights_** .

     > [!NOTE]
     > De forma predeterminada, las aplicaciones de función se crean con la versión 2.x del entorno en tiempo de ejecución. Debe [dirigirse a la versión del entorno de ejecución de Azure Functions](https://docs.microsoft.com/azure/azure-functions/set-runtime-version) **1.x** para usar la plantilla de resumen programado de Application Insights.  ![captura de pantalla del entorno de ejecución](./../../../includes/media/functions-view-update-version-portal/function-app-view-version.png)



   ![Captura de pantalla de la plantilla de Application Insights de la nueva función](./media/automate-custom-reports/function-app-04.png)

6. Escriba una dirección de correo electrónico del destinatario correcta para el informe y seleccione **Crear**.

   ![Captura de pantalla de configuración de la función](./media/automate-custom-reports/function-app-05.png)

7. Seleccione **Function App** > **Características de la plataforma** > **Configuración de la aplicación**.

    ![Captura de pantalla de configuración de la aplicación de función de Azure](./media/automate-custom-reports/function-app-07.png)

8. Cree tres nuevas configuraciones de la aplicación con los valores correspondientes adecuados ``AI_APP_ID``, ``AI_APP_KEY`` y ``SendGridAPI``. Seleccione **Guardar**.

     ![Captura de pantalla de interfaz de integración de la función](./media/automate-custom-reports/function-app-08.png)
    
    (Los valores AI_ pueden encontrarse en el acceso de API para el recurso de Application Insights sobre el cual quiere informar. Si no tiene ninguna clave de API de Application Insights, existe la opción **Crear clave de API**).
    
   * AI_APP_ID = Id. de aplicación
   * AI_APP_KEY = Clave de API
   * SendGridAPI = Clave de API de SendGrid

     > [!NOTE]
     > Si no tiene ninguna cuenta en SendGrid, puede crear una. Puede encontrar la documentación de SendGrid para Azure Functions [aquí](https://docs.microsoft.com/azure/azure-functions/functions-bindings-sendgrid). Si solo quiere una explicación básica sobre cómo configurar SendGrid y generar una clave de API, se proporciona una al final de este artículo. 

9. Seleccione **Integrar** y, en Salidas, haga clic en **SendGrid ($return)** .

     ![Captura de pantalla de salida](./media/automate-custom-reports/function-app-09.png)

10. En **SendGridAPI Key App Setting** (Configuración de la aplicación de clave de SendGridAPI), seleccione la configuración de la aplicación recién creada para **SendGridAPI**.

     ![Captura de pantalla de la ejecución de Function App](./media/automate-custom-reports/function-app-010.png)

11. Ejecute y pruebe la aplicación de Function App.

     ![Captura de pantalla de la prueba](./media/automate-custom-reports/function-app-11.png)

12. Compruebe su correo electrónico para confirmar que el mensaje se ha enviado o recibido correctamente.

     ![Captura de pantalla de la línea del asunto del correo electrónico](./media/automate-custom-reports/function-app-12.png)

## <a name="sendgrid-with-azure"></a>SendGrid con Azure

Estos pasos se aplican solo si aún no ha configurado ninguna cuenta de SendGrid.

1. En Azure Portal, seleccione **Crear un recurso**, busque **SendGrid Email Delivery** (Entrega de correo electrónico de SendGrid) > haga clic en **Crear** > y rellene las instrucciones de creación específicas de SendGrid. 

     ![Captura de pantalla para crear recursos de SendGrid](./media/automate-custom-reports/function-app-13.png)

2. Una vez creada, en Cuentas de SendGrid, seleccione **Administrar**.

     ![Captura de pantalla de configuración de la clave de API](./media/automate-custom-reports/function-app-14.png)

3. Esto iniciará el sitio de SendGrid. Seleccione **Configuración** > **Claves de API**.

     ![Captura de pantalla de creación y visualización de la aplicación de claves de API](./media/automate-custom-reports/function-app-15.png)

4. Cree una clave de API > elija **Create & View** (Crear y ver) (compruebe la documentación de SendGrid sobre el acceso restringido para determinar qué nivel de permisos es adecuado para su clave de API. A continuación, se selecciona el acceso total solo como ejemplo).

   ![Captura de pantalla de acceso total](./media/automate-custom-reports/function-app-16.png)

5. Copie la clave completa. Este es el valor que necesita como valor de SendGridAPI en la configuración de Function App.

   ![Captura de pantalla de la copia de la clave de API](./media/automate-custom-reports/function-app-17.png)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo crear [consultas de análisis](../../azure-monitor/log-query/get-started-queries.md).
* Obtenga más información sobre [consultar mediante programación los datos de Application Insights](https://dev.applicationinsights.io/).
* Más información acerca de [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).
* Más información sobre [Microsoft Flow](https://ms.flow.microsoft.com).
