---
title: Configuración de reglas personalizadas del firewall de aplicaciones web v2 con Azure PowerShell
description: Información sobre cómo configurar reglas personalizadas del firewall de aplicaciones web v2 con Azure PowerShell
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: f4d2fd7342e0efe95a1bc69e0dba77692053cf14
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164742"
---
# <a name="configure-web-application-firewall-v2--with-a-custom-rule-using-azure-powershell"></a>Configuración del firewall de aplicaciones web v2 con una regla personalizada mediante Azure PowerShell

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

Las reglas personalizadas permiten crear reglas propias que se evalúan en cada solicitud que pasa por el firewall de aplicaciones web (WAF) v2. Estas reglas tienen una prioridad mayor que el resto de las reglas de los conjuntos de reglas administrados. Las reglas personalizadas incluyen una acción (para permitir o bloquear), una condición de coincidencia y un operador para permitir la personalización completa.

En este artículo se crea un firewall de aplicaciones web v2 de Application Gateway en el que se usa una regla personalizada. La regla personalizada bloqueará el tráfico si el encabezado de la solicitud contiene User-Agent *evilbot*.

Para ver más ejemplos de reglas personalizadas, consulte [Creación y uso de reglas personalizadas del firewall de aplicaciones web](create-custom-waf-rules.md).

Si quiere ejecutar la instancia de Azure PowerShell de este artículo en un script continuo que puede copiar, pegar y ejecutar, consulte [Ejemplos de Azure PowerShell con Azure Application Gateway](powershell-samples.md).

## <a name="prerequisites"></a>Requisitos previos

### <a name="azure-powershell-module"></a>Módulo de Azure PowerShell

Si decide instalar y usar Azure PowerShell localmente, este script requiere la versión 2.1.0 del módulo de Azure PowerShell o cualquier versión posterior.

1. Para encontrar la versión, ejecute `Get-Module -ListAvailable Az`. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps).
2. Ejecute `Connect-AzAccount` para crear una conexión con Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="example-script"></a>Script de ejemplo

### <a name="set-up-variables"></a>Configuración de variables

```azurepowershell
$rgname = "CustomRulesTest"

$location = "East US"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-vnet"></a>Creación de una red virtual

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>Creación de una IP pública estática

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-pool-and-frontend-port"></a>Creación de un grupo y un puerto de front-end

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>Creación de un agente de escucha, una configuración de http, una regla y escalado automático

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled

$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3

$sku = New-AzApplicationGatewaySku -Name WAF_v2 -Tier WAF_v2
```

### <a name="create-the-custom-rule-and-apply-it-to-waf-policy"></a>Creación de la regla personalizada y aplicación a la directiva del WAF

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-the-application-gateway"></a>Creación de Application Gateway

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $rgname `
   -Location $location -BackendAddressPools $pool `
   -BackendHttpSettingsCollection  $poolSetting01 `
   -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
   -FrontendPorts $fp01 -HttpListeners $listener01 `
   -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig `
   -WebApplicationFirewallConfig $wafConfig `
   -FirewallPolicy $wafPolicy
```

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre el firewall de aplicaciones web](waf-overview.md)