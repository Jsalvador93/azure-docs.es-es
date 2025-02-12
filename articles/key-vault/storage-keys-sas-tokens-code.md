---
title: 'Cuenta de almacenamiento administrado por Azure Key Vault: versión de PowerShell'
description: La característica de cuenta de almacenamiento administrado ofrece una integración sin problemas entre Azure Key Vault y una cuenta de almacenamiento de Azure.
ms.topic: conceptual
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/10/2019
ms.openlocfilehash: a20f5b16b90b9bac7e6bc3ff808cf443b252fb44
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204491"
---
# <a name="fetch-shared-access-signature-tokens-in-code"></a>Captura de tokens de firma de acceso compartido en el código

Puede administrar su cuenta de almacenamiento con los [tokens de firma de acceso compartido](../storage/common/storage-dotnet-shared-access-signature-part-1.md) del almacén de claves. En este artículo se proporcionan ejemplos de código C# que captura un token de SAS y realiza operaciones con él.  Para más información sobre cómo crear y almacenar tokens de SAS, consulte [Administración de claves de cuenta de almacenamiento con Key Vault y la CLI de Azure](key-vault-ovw-storage-keys.md) o [Administración de claves de cuenta de almacenamiento con Key Vault y Azure PowerShell](key-vault-overview-storage-keys-powershell.md).

# <a name="code-samples"></a>Ejemplos de código

En este ejemplo, el código captura un token de SAS del almacén de claves, lo usa para crear una nueva cuenta de almacenamiento y crea un nuevo cliente de Blob service.  

```cs
// After you get a security token, create KeyVaultClient with vault credentials.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a shared access signature token for your storage from Key Vault.
// The format for SecretUri is https://<YourKeyVaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials by using the shared access signature token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Si el token de firma de acceso compartido está a punto de expirar, puede capturar el token de firma de acceso compartido desde el almacén de claves y actualizar el código.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```


## <a name="next-steps"></a>Pasos siguientes
- Consulte [Administración de claves de cuenta de almacenamiento con Key Vault y la CLI de Azure](key-vault-ovw-storage-keys.md) o [Azure PowerShell](key-vault-overview-storage-keys-powershell.md).
- Consulte [Ejemplos de clave de cuenta de almacenamiento administrada](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=).
- [Información acerca de claves, secretos y certificados](about-keys-secrets-and-certificates.md)
- [Referencia de PowerShell para Key Vault](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)
