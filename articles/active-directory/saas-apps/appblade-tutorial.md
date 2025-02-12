---
title: 'Tutorial: Integración de Azure Active Directory con AppBlade | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y AppBlade.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c1a0f15d67d76ca0e1b2e9b348b36be7dee3dd6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106893"
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a>Tutorial: Integración de Azure Active Directory con AppBlade

En este tutorial, obtendrá información sobre cómo integrar AppBlade con Azure Active Directory (Azure AD).
La integración de AppBlade con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a AppBlade.
* Puede permitir que los usuarios inicien sesión automáticamente en AppBlade (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con AppBlade, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en AppBlade

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* AppBlade admite el inicio de sesión único iniciado por **SP**.
* AppBlade admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-appblade-from-the-gallery"></a>Adición de AppBlade desde la galería

Para configurar la integración de AppBlade en Azure AD, deberá agregar AppBlade desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar AppBlade desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **AppBlade**, seleccione **AppBlade** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![AppBlade en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con AppBlade con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de AppBlade.

Para configurar y probar el inicio de sesión único de Azure AD con AppBlade, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de AppBlade](#configure-appblade-single-sign-on)** : para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de AppBlade](#create-appblade-test-user)** : para tener un homólogo de Britta Simon en AppBlade que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con AppBlade, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **AppBlade**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de AppBlade](common/sp-signonurl.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.appblade.com/saml/<tenantid>`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de AppBlade](mailto:support@appblade.com) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up AppBlade** (Configurar AppBlade), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-appblade-single-sign-on"></a>Configuración del inicio de sesión único de AppBlade

Para configurar el inicio de sesión único en **AppBlade**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de AppBlade](mailto:support@appblade.com). Además, pídales que configuren la **URL de usuario de SSO** como `https://appblade.com/saml`. Esta configuración es necesaria para que el inicio de sesión único funcione.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a AppBlade.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **AppBlade**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **AppBlade**.

    ![El vínculo a AppBlade en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-appblade-test-user"></a>Creación de un usuario de prueba de AppBlade

El objetivo de esta sección es crear un usuario llamado Britta Simon en AppBlade. AppBlade admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. **Asegúrese de que el nombre de dominio está configurado con AppBlade para el aprovisionamiento de usuarios. Después de eso solo funciona el aprovisionamiento de usuarios Just-In-Time.**

Si el usuario tiene una dirección de correo electrónico que termina con el dominio configurado por AppBlade para la cuenta, dicho usuario se unirá automáticamente a la cuenta como un miembro con el nivel de permiso que especifique, que puede ser "Básico" (un usuario básico que solamente puede instalar aplicaciones), "Miembro del equipo" (un usuario que puede cargar nuevas versiones de aplicaciones y administrar proyectos) o "Administrador" (privilegios completos de administrador para la cuenta). Normalmente se elegiría Básico y después se promocionaría a los usuarios manualmente a través de un inicio de sesión del administrador (AppBlade debe configurar un inicio de sesión del administrador basado en correo electrónico por adelantado o promocionar a un usuario en nombre del cliente después de iniciar sesión).

No hay ningún elemento de acción para usted en esta sección. Durante un intento de acceder a AppBlade se crea un nuevo usuario, en caso de que no exista.

> [!NOTE]
> Si necesita crear manualmente un usuario, es preciso que se ponga contacto con el [equipo de soporte técnico de AppBlade](mailto:support@appblade.com).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de AppBlade en el panel de acceso, debería iniciar sesión automáticamente en la versión de AppBlade para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
