---
title: Acceso a un laboratorio de clase en Azure Lab Services | Microsoft Docs
description: En este tutorial, va a acceder a máquinas virtuales en un laboratorio educativo configurado por un profesor.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/19/2019
ms.author: spelluru
ms.openlocfilehash: 10813600621293e9f559129230193f81ea0c505a
ms.sourcegitcommit: a7a9d7f366adab2cfca13c8d9cbcf5b40d57e63a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71161534"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Acceso a un laboratorio de clase en Azure Lab Services
En este artículo se describe cómo registrarse en un laboratorio educativo, ver todos los laboratorios a los que puede acceder, iniciar o detener una máquina virtual en el laboratorio y conectarse a la máquina virtual. 

## <a name="register-to-the-lab"></a>Registro en el laboratorio

1. Vaya a la **dirección URL de registro** que recibió del profesor o educador. No es necesario usar la dirección URL de registro después de completar el registro. En su lugar, use la dirección URL: [https://labs.azure.com](https://labs.azure.com). Todavía no se admite Internet Explorer 11. 
1. Inicie sesión en el servicio con su cuenta organizativa para completar el registro. 

    > [!NOTE]
    > Para usar Azure Lab Services se requiere una cuenta Microsoft. Si intenta usar una cuenta que no sea de Microsoft, como las cuentas de Yahoo o de Google, para iniciar sesión en el portal, siga las instrucciones para crear una cuenta Microsoft que se vincule a una cuenta que no sea de Microsoft. Luego, siga los pasos para completar el proceso de registro. 
1. Una vez registrado, confirme que ve las máquinas virtuales de los laboratorios a los que tiene acceso. 
1. Espere hasta que la máquina virtual esté lista. En el icono de VM, observe los campos siguientes:
    1. En la parte superior del icono, se puede ver el **nombre del laboratorio**.
    1. A su derecha, aparece el icono que representa el **sistema operativo (SO)** de la máquina virtual. En este ejemplo, es el sistema operativo Windows. 
    1. En la parte inferior del icono hay iconos o botones para iniciar o detener la máquina virtual y para conectarse a ella. 
    1. A la derecha de los botones, se puede ver el estado de la máquina virtual. Confirme que el estado de la máquina virtual aparece como **Detenido**.

        ![Máquina virtual con estado Detenido](../media/tutorial-connect-vm-in-classroom-lab/vm-in-stopped-state.png)

## <a name="start-or-stop-the-vm"></a>Inicio o detención de la máquina virtual
1. **Inicie** la máquina virtual seleccionando el primer botón, tal como se muestra en la siguiente imagen. Este proceso tarda algún tiempo.  

    ![Inicio de la máquina virtual](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)
4. Confirme que el estado de la máquina virtual aparece como **En ejecución**. 

    ![Máquina virtual con estado En ejecución](../media/tutorial-connect-vm-in-classroom-lab/vm-running.png)

    Observe que el icono del primer botón ha cambiado para representar una operación de **detención**. Puede seleccionar este botón para detener la máquina virtual. 

## <a name="connect-to-the-vm"></a>Conexión a la máquina virtual

1. Seleccione el segundo botón, tal como se muestra en la siguiente imagen, para **conectarse** a la máquina virtual del laboratorio. 

    ![Conexión con una máquina virtual](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Realice uno de los siguientes pasos: 
    1. Para máquinas virtuales **Windows**, guarde el archivo **RDP** en el disco duro. Abra el archivo RDP para conectarse a la máquina virtual. Utilice el **nombre de usuario** y la **contraseña** que obtenga de su profesor o educador para iniciar sesión en la máquina. 
    3. Puede usar **SSH** o **RDP** (si está habilitado) para conectarse a máquinas virtuales **Linux**. Para más información, consulte el artículo sobre la [Habilitación de la conexión a Escritorio remoto para máquinas Linux](how-to-enable-remote-desktop-linux.md). 
    1. Si usa un **equipo Mac** para conectarse a la máquina virtual del laboratorio, siga las instrucciones de la sección siguiente. 

## <a name="connect-to-a-vm-using-rdp-on-a-mac"></a>Conexión a una máquina virtual mediante RDP en un equipo Mac
En esta sección se muestra cómo un alumno puede conectarse a una máquina virtual desde un equipo Mac mediante RDP.

### <a name="step-1-install-microsoft-remote-desktop-on-a-mac"></a>Paso 1: Instalar Escritorio remoto de Microsoft en un equipo Mac
1. Abra App Store en el equipo Mac y busque **Escritorio remoto de Microsoft**.

    ![Escritorio remoto de Microsoft](../media/how-to-use-classroom-lab/install-ms-remote-desktop.png)
1. Instale la versión más reciente de Escritorio remoto de Microsoft. 

### <a name="step-2-access-the-vm-from-your-mac-using-rdp"></a>Paso 2: Acceder a la máquina virtual desde el equipo Mac con RDP
1. Abra el archivo **RDP** que se descarga en el equipo con **Escritorio remoto de Microsoft** instalado. Debe empezar a conectarse a la máquina virtual. 

    ![Conexión con una máquina virtual](../media/how-to-use-classroom-lab/connect-linux-vm.png)
1. Seleccione **Continuar** si recibe la advertencia siguiente. 

    ![Advertencia de certificado](../media/how-to-use-classroom-lab/certificate-error.png)
1. Debería ver la máquina virtual. 

    > [!NOTE]
    > El ejemplo siguiente es para una máquina virtual Linux de CentOS. 

    ![máquina virtual](../media/how-to-use-classroom-lab/vm-ui.png)

## <a name="progress-bar"></a>Barra de progreso 
La barra de progreso del icono muestra el número de horas usadas en relación con el número de [horas de cuota](how-to-configure-student-usage.md#set-quotas-for-users) que tiene asignadas. Este tiempo es el tiempo adicional que se le asigna además del tiempo programado para el laboratorio. El color de la barra de progreso y el texto en la barra de progreso varía según los siguientes escenarios:

- Si una clase está en curso (dentro del horario de la clase), la barra de progreso aparece atenuada para representar que no se están usando horas de cuota. 

    ![Barra de progreso en gris](../media/tutorial-connect-vm-in-classroom-lab/progress-bar-class-in-progress.png)
- Si no se asigna ninguna cuota (cero horas), se muestra el texto **Available during classes only** (Solo disponible durante las clases) en lugar de la barra de progreso. 
    
    ![Estado cuando no se establece ninguna cuota](../media/tutorial-connect-vm-in-classroom-lab/available-during-class.png)
- Si se ha **agotado su cuota**, la barra de progreso se muestra en **rojo**. 

    ![Barra de progreso en rojo](../media/tutorial-connect-vm-in-classroom-lab/progress-bar-red-color.png)
- El color de la barra de progreso se muestra en **azul** cuando está fuera del tiempo programado para el laboratorio y se ha usado parte del tiempo de cuota. 

    ![Barra de progreso en azul](../media/tutorial-connect-vm-in-classroom-lab/progress-bar-blue-color.png)


## <a name="view-all-the-classroom-labs"></a>Visualización de todos los laboratorios de clase
Después de registrarse en los laboratorios, puede ver todos los laboratorios educativos siguiendo estos pasos: 

1. Vaya a [https://labs.azure.com](https://labs.azure.com). Todavía no se admite Internet Explorer 11. 
2. Inicie sesión en el servicio mediante la cuenta de usuario que usó para registrarse en el laboratorio. 
3. Confirme que puede ver todos los laboratorios a los que tiene acceso. 

    ![Visualización de todos los laboratorios](../media/how-to-use-classroom-lab/all-labs.png)


## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
 