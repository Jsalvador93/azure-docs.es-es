---
title: 'Sistemas operativos y motores de contenedor compatibles: Azure IoT Edge | Microsoft Docs'
description: Obtenga información sobre qué sistemas operativos pueden ejecutar el demonio y el entorno de ejecución de Azure IoT Edge y los motores de contenedor admitidos para los dispositivos de producción
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/13/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 81d19552b56de540f235960c498c64e7b276320c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030965"
---
# <a name="azure-iot-edge-supported-systems"></a>Sistemas compatibles con Azure IoT Edge

En este artículo se proporcionan detalles sobre qué sistemas y componentes son compatibles con IoT Edge, ya sea oficialmente o en versión preliminar. 

Si tiene problemas al usar el servicio Azure IoT Edge, hay varias maneras de buscar soporte técnico. Pruebe uno de los siguientes canales para obtener soporte técnico:

**Notificación de errores**: la mayor parte del desarrollo que lleva el producto de Azure IoT Edge se produce en el proyecto de código abierto de IoT Edge. Los errores se pueden notificar en la [página de problemas](https://github.com/azure/iotedge/issues) del proyecto. Las correcciones pasan pronto del proyecto a las actualizaciones de producto.

**Equipo de soporte técnico al cliente de Microsoft**: los usuarios que dispongan de un [plan de soporte técnico](https://azure.microsoft.com/support/plans/) pueden ponerse en contacto con este equipo creando una incidencia de soporte técnico directamente en [Azure Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Solicitudes de características**: el producto Azure IoT Edge realiza un seguimiento de las solicitudes de características a través de la [página de voz del usuario](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="container-engines"></a>Motores de contenedor

Los módulos de Azure IoT Edge se implementan como contenedores, por lo que IoT Edge necesita un motor de contenedores para iniciarlos. Microsoft ofrece un motor de contenedor, moby-engine, para satisfacer este requisito. Este motor de contenedores se basa en el proyecto de código abierto de Moby. Docker CE y Docker EE son otros motores de contenedores conocidos. También se basan en el proyecto de código abierto de Moby y son compatibles con Azure IoT Edge. Microsoft proporciona el mejor soporte técnico posible para los sistemas que usan esos motores de contenedores; sin embargo, Microsoft no puede proporcionar soluciones para los problemas de ellos. Por esta razón, Microsoft recomienda el uso de moby-engine en sistemas de producción.

<br>
<center>

![Moby como entorno de ejecución de contenedores](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>Sistemas operativos
Azure IoT Edge se ejecuta en la mayoría de los sistemas operativos que pueden ejecutar contenedores. No obstante, la compatibilidad no es igual en todos. Los sistemas operativos se agrupan en niveles que representan el nivel de compatibilidad que los usuarios pueden esperar.
* Se admiten los sistemas de nivel 1. Para los sistemas de nivel 1, Microsoft:
    * tiene este sistema operativo en pruebas automatizadas
    * proporciona paquetes de instalación para ellos
* Los sistemas de nivel 2 son compatibles con Azure IoT Edge y pueden usarse con relativa facilidad. Para los sistemas de nivel 2:
    * Microsoft ha realizado pruebas ad hoc en las plataformas o conoce a un asociado que ha ejecutado correctamente Azure IoT Edge en la plataforma
    * Los paquetes de información de otras plataformas pueden funcionar en estas plataformas
    
La familia del sistema operativo host siempre debe coincidir con la familia del sistema operativo invitado que se utilice en el contenedor de un módulo. En otras palabras, solo se pueden usar contenedores de Linux en Linux y contenedores de Windows en Windows. En el caso de Windows, solo se pueden usar contenedores aislados de los procesos; no se admiten los contenedores aislados de Hyper-V.  

<br>
<center>

![El sistema operativo host coincide con el sistema operativo invitado](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Nivel 1

Los sistemas que se enumeran en la tabla siguiente son compatibles con Microsoft, tanto si están disponibles con carácter general como en versión preliminar pública, y se prueban con cada versión nueva. 

| Sistema operativo | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| Raspbian-stretch |  | ![Raspbian Stretch + ARM32v7](./media/tutorial-c-module/green-check.png) |  |
| Ubuntu Server 16.04 | ![Ubuntu Server 16.04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Versión preliminar pública  |
| Ubuntu Server 18.04 | ![Ubuntu Server 18.04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Versión preliminar pública |
| Windows 10 IoT Enterprise, compilación 17763 | ![Windows 10 IoT Enterprise + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server 2019, compilación 17763 | ![Windows Server 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server IoT 2019, compilación 17763 | ![Windows Server IoT 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows 10 IoT Core, compilación 17763 | ![Windows IoT Core + AMD64](./media/tutorial-c-module/green-check.png) |  |  |


Los sistemas operativos de Windows anteriores son obligatorios en los dispositivos que ejecutan contenedores de Windows en Windows, que es la única configuración compatible para producción. Los paquetes de instalación de Azure IoT Edge para Windows permiten usar contenedores de Linux en Windows, aunque solo en los entornos de desarrollo y pruebas. Para obtener más información, consulte [Uso de IoT Edge en Windows para ejecutar contenedores de Linux](how-to-install-iot-edge-windows-with-linux.md).

### <a name="tier-2"></a>Nivel 2

Los sistemas que se enumeran en la tabla siguiente se consideran compatibles con Azure IoT Edge, pero no se prueban ni mantienen activamente. 

| Sistema operativo | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| CentOS 7.5 | ![CentOS + AMD64](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM32v7](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 8 | ![Debian 8 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 8 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 8 + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 9 | ![Debian 9 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 9 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 9 + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 10<sup>1</sup> | ![Debian 10 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 10 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 10 + ARM64](./media/tutorial-c-module/green-check.png) |
| RHEL 7.5 | ![RHEL 7.5 + AMD64](./media/tutorial-c-module/green-check.png) | ![RHEL 7.5 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![RHEL 7.5 + ARM64](./media/tutorial-c-module/green-check.png) |
| Ubuntu 16.04 | ![Ubuntu 16.04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16.04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16.04 + ARM64](./media/tutorial-c-module/green-check.png) |
| Ubuntu 18.04 | ![Ubuntu 18.04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18.04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18.04 + ARM64](./media/tutorial-c-module/green-check.png) |
| Wind River 8 | ![Wind River 8 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Yocto | ![Yocto + AMD64](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM64](./media/tutorial-c-module/green-check.png) |
| Raspbian Buster<sup>1</sup> |  | ![Raspbian Buster + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Raspbian Buster + ARM64](./media/tutorial-c-module/green-check.png) |

<sup>1</sup> Los sistemas Debian 10, incluido Raspian Buster, usan una versión de OpenSSL que IoT Edge no admite. Use el siguiente comando para instalar una versión anterior antes de instalar IoT Edge: 

```bash
sudo apt-get install libssl1.0.2
```

## <a name="virtual-machines"></a>Virtual Machines
Azure IoT Edge puede ejecutarse en máquinas virtuales. El uso de una máquina virtual como dispositivo de IoT Edge es una práctica habitual cuando los clientes desean mejorar la infraestructura existente con inteligencia perimetral. La familia del sistema operativo de la máquina virtual host debe coincidir con la familia del sistema operativo invitado que se utiliza en el contenedor de un módulo. Este requisito también se aplica cuando Azure IoT Edge se ejecuta directamente en un dispositivo. Azure IoT Edge es independiente de la tecnología de virtualización subyacente y funciona en máquinas virtuales que incorporan plataformas como Hyper-V y vSphere.

<br>
<center>

![Azure IoT Edge en una máquina virtual](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>Requisitos mínimos del sistema
Azure IoT Edge funciona perfectamente en dispositivos tan pequeños como Raspberry Pi3 y en hardware de nivel de servidor. La elección del hardware adecuado para el escenario dependerá de las cargas de trabajo que se deseen ejecutar. Tomar la decisión final sobre el dispositivo que se va a utilizar puede resultar complicado; sin embargo, puede empezar creando un prototipo de una solución en portátiles o equipos de escritorio tradicionales.

La experiencia que adquiera mientras crea ese prototipo le ayudará a realizar la selección final del dispositivo. Debe considerar las siguientes cuestiones: 

* ¿Cuántos módulos hay en la carga de trabajo?
* ¿Cuántas capas comparten los contenedores de los módulos?
* ¿En qué idioma se escriben los módulos? 
* ¿Cuántos datos procesarán los módulos?
* ¿Necesitan los módulos algún hardware especializado para acelerar sus cargas de trabajo?
* ¿Cuáles son las características del rendimiento deseado de la solución?
* ¿Cuál es su presupuesto para hardware?
