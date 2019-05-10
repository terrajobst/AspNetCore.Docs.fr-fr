---
title: Clé de chiffrement au repos dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation du chiffrement de clé de Protection des données ASP.NET Core au repos.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892306"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Clé de chiffrement au repos dans ASP.NET Core

Le système de protection des données [utilise un mécanisme de découverte par défaut](xref:security/data-protection/configuration/default-settings) pour déterminer les clés de chiffrement comment doivent être chiffrées au repos. Le développeur peut remplacer le mécanisme de découverte et spécifier manuellement la façon dont les clés doivent être chiffrées au repos.

> [!WARNING]
> Si vous spécifiez un texte explicite [emplacement de persistance de la clé](xref:security/data-protection/implementation/key-storage-providers), le système de protection des données annule l’inscription du chiffrement à clé par défaut au mécanisme de rest. Par conséquent, les clés ne sont plus chiffrés au repos. Il est recommandé que vous [spécifier un mécanisme de chiffrement à clé explicite](xref:security/data-protection/implementation/key-encryption-at-rest) pour les déploiements de production. Les options de mécanisme de chiffrement au repos sont décrits dans cette rubrique.

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a>Azure Key Vault

Pour stocker les clés dans [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurer le système avec [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) dans la `Startup` classe :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Pour plus d’informations, consultez [Protection des données configurer ASP.NET Core : ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).

::: moniker-end

## <a name="windows-dpapi"></a>Windows DPAPI

**S’applique uniquement aux déploiements de Windows.**

Lorsque Windows DPAPI est utilisé, le matériel de clé est chiffré avec [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) avant d’être rendues persistantes dans le stockage. DPAPI est un mécanisme de chiffrement approprié pour les données qui ne sont jamais lue en dehors de l’ordinateur actuel (Cependant, il est possible de sauvegarder ces clés à Active Directory ; voir [DPAPI et les profils itinérants](https://support.microsoft.com/kb/309408/#6)). Pour configurer le chiffrement de clé au repos DPAPI, appelez une de la [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) méthodes d’extension :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

Si `ProtectKeysWithDpapi` est appelée sans paramètres, seul le compte d’utilisateur Windows actuel peut déchiffrer le key ring persistant. Vous pouvez éventuellement spécifier que n’importe quel compte d’utilisateur sur l’ordinateur (pas seulement le compte d’utilisateur actuel) être en mesure de déchiffrer le key ring :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a>Certificat X.509

Si l’application est répartie sur plusieurs ordinateurs, il peut être pratique de distribuer un certificat X.509 partagé entre les machines et de configurer les applications hébergées pour utiliser le certificat pour le chiffrement des clés au repos :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

En raison des limitations de .NET Framework, seuls les certificats avec clés privées CAPI sont pris en charge. Consultez le contenu ci-dessous pour les solutions possibles à ces limitations.

::: moniker-end

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

**Ce mécanisme est disponible uniquement sur Windows 8/Windows Server 2012 ou version ultérieure.**

À compter de Windows 8, le système d’exploitation Windows prend en charge DPAPI-NG (également appelé CNG DPAPI). Pour plus d’informations, consultez [sur CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).

Le principal est encodé comme une règle de descripteur de protection. Dans l’exemple suivant appelle [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), seul l’utilisateur de domaine avec le SID spécifié peut déchiffrer le key ring :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Il existe également une surcharge sans paramètre de `ProtectKeysWithDpapiNG`. Utilisez cette méthode pratique pour spécifier la règle « SID = {CURRENT_ACCOUNT_SID} », où *CURRENT_ACCOUNT_SID* est le SID du compte d’utilisateur Windows actuel :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

Dans ce scénario, le contrôleur de domaine Active Directory est responsable de la distribution de clés de chiffrement utilisées par les opérations de DPAPI-NG. L’utilisateur cible peut déchiffrer la charge utile chiffrée à partir de n’importe quel ordinateur joint au domaine (à condition que le processus s’exécute sous leur identité).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Chiffrement par certificat avec Windows DPAPI-NG

Si l’application s’exécute sur Windows 8.1 / Windows Server 2012 R2 ou version ultérieure, vous pouvez utiliser Windows DPAPI-NG pour effectuer un chiffrement par certificat. Utiliser la chaîne de descripteur de règle « certificat = HashId:THUMBPRINT », où *empreinte* est l’empreinte numérique SHA1 codé en hexadécimal du certificat :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

N’importe quelle application pointée vers ce dépôt doit être en cours d’exécution sur Windows 8.1 / Windows Server 2012 R2 ou version ultérieure pour déchiffrer les clés.

## <a name="custom-key-encryption"></a>Clé de chiffrement personnalisé

Si les mécanismes d’origine ne sont pas appropriées, le développeur peut spécifier leur propre mécanisme de chiffrement à clé en fournissant un personnalisé [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).
