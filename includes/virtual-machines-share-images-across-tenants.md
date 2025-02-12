---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cf05468af17a4fafa7c81c7ad8bc89b3306a54af
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68286169"
---
Las galerías de imágenes compartidas le permiten compartir las imágenes mediante RBAC. Puede utilizar RBAC para compartir imágenes dentro de su inquilino e incluso con personas de fuera de él. Pero si quiere compartir las imágenes fuera de su inquilino de Azure, a escala, debe crear un registro de aplicación para facilitar el uso compartido.  Usar un registro de aplicación puede habilitar escenarios de uso compartidos más complejos, como por ejemplo: 

* Administración de imágenes compartidas cuando una compañía adquiere otra y la infraestructura de Azure se distribuye entre inquilinos independientes. 
* Los asociados de Azure administran la infraestructura de Azure en nombre de sus clientes. La personalización de imágenes se realiza en el inquilino de los asociados, pero las implementaciones de infraestructura se realizarán en el inquilino del cliente. 


## <a name="create-the-app-registration"></a>Creación del registro de aplicaciones

Cree un registro de aplicación que usarán ambos inquilinos para compartir los recursos de la galería de imágenes.
1. Abra [Registros de aplicaciones (versión preliminar) en Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType//sourceType/).    
1. En el menú de la parte superior de la página, seleccione **Nuevo registro**.
1. En **Nombre**, escriba *myGalleryApp*.
1. En **Tipos de cuenta admitidos**, seleccione **Cuentas en cualquier directorio organizativo y cuentas Microsoft personales**.
1. En **URI de redirección**, escriba *https://www.microsoft.com* y luego haga clic en **Registrar**. Una vez creado el registro de aplicación, se abrirá la página de información general.
1. En la página de información general, copie el **identificador de aplicación (cliente)** y guárdelo para usarlo más adelante.   
1. Seleccione **Certificados y secretos** y luego seleccione **Nuevo secreto de cliente**.
1. En **Descripción**, escriba *Shared image gallery cross-tenant app secret* (Secreto de aplicación entre inquilinos de la Galería de imágenes compartidas).
1. En **Expires**, deje el valor predeterminado de **En 1 año** y, después, seleccione **Agregar**.
1. Copie el valor del secreto y guárdelo en un lugar seguro. No podrá recuperarlo después de salir de la página.


Conceda permiso al registro de aplicación para usar la Galería de imágenes compartidas.
1. En Azure Portal, seleccione la Galería de imágenes compartidas que quiere compartir con otro inquilino.
1. Seleccione **Control de acceso (IAM)** y en **Agregar asignación de roles**, seleccione *Agregar*. 
1. En **Rol**, seleccione **Lector**.
1. En **Asignar acceso a**, déjelo como **Usuario, grupo o entidad de servicio de Azure AD**.
1. En **Seleccionar**, escriba *myGalleryApp* y haga clic sobre él cuando aparezca en la lista. Cuando finalice, seleccione **Guardar**.


## <a name="give-tenant-2-access"></a>Conceder acceso a Inquilino 2

Conceda acceso a Inquilino 2 a la aplicación solicitando un inicio de sesión mediante un explorador. Reemplace *\<Tenant2 ID>* con el id. del inquilino con el que le gustaría compartir la galería de imágenes. Reemplace *\<Application (client) ID>* con el identificador de la aplicación del registro de aplicación que creó. Cuando haya terminado de realizar los reemplazos, pegue la dirección URL en un explorador y siga las instrucciones de inicio de sesión para iniciar sesión en Inquilino 2.

```
https://login.microsoftonline.com/<Tenant 2 ID>/oauth2/authorize?client_id=<Application (client) ID>&response_type=code&redirect_uri=https%3A%2F%2Fwww.microsoft.com%2F 
```

En [Azure Portal](https://portal.azure.com), inicie sesión como Inquilino 2 y dé acceso de registro de aplicación al grupo de recursos donde quiere crear la máquina virtual.

1. Seleccione el grupo de recursos, y luego haga clic en **Control de acceso (IAM)** . En **Agregar asignación de roles**, seleccione **Agregar**. 
1. En **Rol**, escriba **Colaborador**.
1. En **Asignar acceso a**, déjelo como **Usuario, grupo o entidad de servicio de Azure AD**.
1. En **Seleccionar**, escriba *myGalleryApp* y luego haga clic sobre él cuando aparezca en la lista. Cuando finalice, seleccione **Guardar**.

> [!NOTE]
> Deberá esperar a que la versión de la imagen termine de compilarse y replicarse por completo antes de poder usar la misma imagen administrada para crear otra versión de la imagen.

