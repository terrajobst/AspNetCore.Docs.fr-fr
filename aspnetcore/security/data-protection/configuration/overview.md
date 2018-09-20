---
title: Configurer la Protection des données ASP.NET Core
author: rick-anderson
description: Découvrez comment configurer la Protection des données dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 0377fe9fbe5a1eeddb384443370751baa3c0ee43
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482994"
---
# <a name="configure-aspnet-core-data-protection"></a>Configurer la Protection des données ASP.NET Core

Lorsque le système de Protection des données est initialisé, il s’applique [paramètres par défaut](xref:security/data-protection/configuration/default-settings) selon l’environnement d’exploitation. Ces paramètres sont généralement appropriés pour les applications exécutées sur un seul ordinateur. Il existe des cas où un développeur peut souhaiter modifier les paramètres par défaut :

* L’application est répartie sur plusieurs ordinateurs.
* Pour des raisons de conformité.

Pour ces scénarios, le système de Protection des données offre une API de configuration riche.

> [!WARNING]
> Comme pour les fichiers de configuration, le key ring de protection de données doivent être protégé à l’aide des autorisations appropriées. Vous pouvez choisir de chiffrer les clés au repos, mais cela n’empêche pas les pirates de créer de nouvelles clés. Par conséquent, la sécurité de votre application est affectée. L’emplacement de stockage configuré avec la Protection des données doit avoir son accès limité à l’application elle-même, similaire à la façon dont vous protégera les fichiers de configuration. Par exemple, si vous choisissez de stocker votre porte-clés sur le disque, utilisez des autorisations de système de fichiers. Vérifiez seulement l’identité sous laquelle s’exécute votre application web a lire, écrire et créer l’accès à ce répertoire. Si vous utilisez le stockage Table Azure, seule l’application web doit avoir la possibilité de lire, écrire ou créer de nouvelles entrées dans le magasin de tables, etc.
>
> La méthode d’extension [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) retourne un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder` expose des méthodes d’extension que vous pouvez chaîner des options pour configurer la Protection des données.

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

Pour stocker les clés dans [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurer le système avec [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) dans la `Startup` classe :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Définir l’emplacement de stockage de porte-clés (par exemple, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)). L’emplacement doit être défini, car l’appel `ProtectKeysWithAzureKeyVault` implémente un [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) qui désactive les paramètres de protection automatique des données, y compris l’emplacement de stockage de porte-clés. L’exemple précédent utilise le stockage Blob Azure pour conserver le key ring. Pour plus d’informations, consultez [les fournisseurs de stockage de clés : Azure et Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis). Vous pouvez également conserver le key ring localement avec [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).

Le `keyIdentifier` est l’identificateur de clé de coffre de clés utilisée pour le chiffrement de clé (par exemple, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).

`ProtectKeysWithAzureKeyVault` surcharges :

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permet d’utiliser un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) pour permettre au système de protection des données à utiliser le coffre de clés.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permet d’utiliser un `ClientId` et [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) pour permettre au système de protection des données à utiliser le coffre de clés.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permet d’utiliser un `ClientId` et `ClientSecret` pour permettre au système de protection des données à utiliser le coffre de clés.

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Pour stocker les clés sur un partage UNC plutôt qu’à la *% LocalAppData%* emplacement par défaut, configurez le système avec [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> Si vous modifiez l’emplacement de la persistance des clés, le système chiffre n’est plus automatiquement les clés au repos, dans la mesure où il ne sait pas si DPAPI est un mécanisme de chiffrement approprié.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Vous pouvez configurer le système pour protéger les clés au repos en appelant une de le [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) API de configuration. Prenons l’exemple ci-dessous, qui stocke les clés sur un partage UNC et chiffre ces clés au repos avec un certificat X.509 spécifique :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

Dans ASP.NET Core 2.1 ou version ultérieure, vous pouvez fournir un [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) à [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), comme un certificat chargé à partir d’un fichier :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

Consultez [clé de chiffrement au repos](xref:security/data-protection/implementation/key-encryption-at-rest) pour obtenir des exemples et des discussions sur les mécanismes de chiffrement de clé intégrée.

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a>UnprotectKeysWithAnyCertificate

Dans ASP.NET Core 2.1 ou version ultérieure, vous pouvez faire pivoter des certificats et déchiffrer les clés au repos à l’aide d’un tableau de [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificats avec [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Pour configurer le système pour utiliser une durée de vie de clé de 14 jours au lieu de la valeur par défaut de 90 jours, utilisez [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

Par défaut, le système de Protection des données isole les applications à partir d’une autre, même si elles le partagent le même référentiel clé physique. Cela empêche les applications de la compréhension des charges protégé entre eux. Pour partager les charges utiles protégées entre les deux applications, utilisez [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) avec la même valeur pour chaque application :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Vous pouvez avoir un scénario dans lequel vous voulez une application à déployer automatiquement des clés (créer des clés) qu’ils approchent d’expiration. Un exemple de ce peut être définies dans une relation principal/secondaire, où le principal de l’application est responsable de problèmes de gestion des clés et des applications secondaires ont simplement une vue en lecture seule de l’anneau de clé des applications. Les applications secondaire peuvent être configurées pour traiter le key ring en lecture seule en configurant le système avec [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Isolation par application

Lorsque le système de Protection des données est fourni par un hôte ASP.NET Core, elle isole automatiquement les applications à partir d’une autre, même si ces applications sont exécutent sous le même compte de processus de travail et que vous utilisez le même matériel de gestion de clés principal. Cela s’apparente au modificateur IsolateApps à partir de System.Web  **\<machineKey >** élément.

Le mécanisme d’isolation fonctionne en tenant compte de chaque application sur l’ordinateur local en tant qu’un locataire unique, donc le [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooté pour une application donnée inclut automatiquement l’ID d’application en tant que discriminateur. ID unique de l’application provient d’un des deux emplacements :

1. Si l’application est hébergée dans IIS, l’identificateur unique est le chemin d’accès de l’application configuration. Si une application est déployée dans un environnement de batterie de serveurs web, cette valeur doit être stable, en supposant que les environnements d’IIS sont configurés de façon similaire sur tous les ordinateurs dans la batterie de serveurs web.

2. Si l’application n’est pas hébergée dans IIS, l’identificateur unique est le chemin d’accès physique de l’application.

L’identificateur unique est conçu pour résister aux réinitialisations &mdash; de l’application individuelle et de la machine elle-même.

Ce mécanisme d’isolation part du principe que les applications ne sont pas malveillantes. Une application malveillante peut toujours avoir un impact sur n’importe quel autre application en cours d’exécution sous le même compte de processus de travail. Dans un environnement d’hébergement partagé où les applications sont mutuellement non approuvées, le fournisseur d’hébergement doit prendre étapes pour garantir l’isolation au niveau du système d’exploitation entre les applications, notamment pour séparer les applications sous-jacentes référentiels des clés.

Si le système de Protection des données n’est pas fourni par un hôte ASP.NET Core (par exemple, si vous instanciez via la `DataProtectionProvider` type concret) isolation de l’application est désactivée par défaut. Lors de l’isolation de l’application est désactivée, soutenues par le même jeu de clés de toutes les applications peuvent partager les charges utiles tant qu’ils fournissent approprié [à des fins](xref:security/data-protection/consumer-apis/purpose-strings). Pour assurer l’isolation d’application dans cet environnement, appelez le [SetApplicationName](#setapplicationname) méthode sur la configuration de l’objet et fournissez un nom unique pour chaque application.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Modification des algorithmes avec UseCryptographicAlgorithms

La pile de Protection des données vous permet de modifier l’algorithme par défaut utilisé par les clés qui vient d’être généré. La façon la plus simple pour ce faire consiste à appeler [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) à partir du rappel de configuration :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

La valeur par défaut EncryptionAlgorithm est AES-256-CBC, et la valeur par défaut ValidationAlgorithm est HMACSHA256. La stratégie par défaut peut être définie par un administrateur système via une [-échelle de l’ordinateur stratégie](xref:security/data-protection/configuration/machine-wide-policy), mais un appel explicite à `UseCryptographicAlgorithms` substitue la stratégie par défaut.

Appel `UseCryptographicAlgorithms` vous permet de spécifier l’algorithme souhaité à partir d’une liste prédéfinie intégrée. Vous n’avez pas besoin de vous soucier de l’implémentation de l’algorithme. Dans le scénario ci-dessus, le système de Protection de données tente d’utiliser l’implémentation CNG de AES si s’exécutant sur Windows. Sinon, il repasse automatiquement managé [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.

Vous pouvez spécifier manuellement une implémentation via un appel à [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Algorithmes de modification n’affecte pas les clés existantes dans le key ring. Il affecte uniquement les clés qui vient d’être générées.

### <a name="specifying-custom-managed-algorithms"></a>En spécifiant des algorithmes personnalisés gérés

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Pour spécifier les algorithmes managés personnalisés, créez un [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance qui pointe vers les types d’implémentation :

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pour spécifier les algorithmes managés personnalisés, créez un [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance qui pointe vers les types d’implémentation :

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

En règle générale le \*propriétés de Type doivent pointer vers concret, implémentations instanciables (via un constructeur sans paramètre public) de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) et [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), bien que le système spécial cas certaines valeurs telles que `typeof(Aes)` pour des raisons pratiques.

> [!NOTE]
> Le SymmetricAlgorithm doit avoir une longueur de clé de ≥ 128 bits et une taille de bloc de ≥ 64 bits, et il doit prendre en charge le chiffrement en mode CBC avec un remplissage de PKCS #7. L’élément KeyedHashAlgorithm impossible doit avoir une taille de condensat de > = 128 bits, et il doit prendre en charge les clés de longueur égale à la longueur du résumé de l’algorithme de hachage. L’élément KeyedHashAlgorithm impossible n’est pas strictement obligatoire d’être HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>En spécifiant les algorithmes CNG de Windows personnalisés

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Pour spécifier un algorithme CNG de Windows personnalisé à l’aide du chiffrement en mode CBC avec validation de HMAC, créez un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance qui contient les informations algorithmiques :

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pour spécifier un algorithme CNG de Windows personnalisé à l’aide du chiffrement en mode CBC avec validation de HMAC, créez un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance qui contient les informations algorithmiques :

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> L’algorithme de chiffrement symétrique doit avoir une longueur de clé > = 128 bits, une taille de bloc de > = 64 bits, et il doit prendre en charge le chiffrement en mode CBC avec un remplissage de PKCS #7. L’algorithme de hachage doit avoir une taille de condensat de > = 128 bits et doit prendre en charge en cours d’ouverture avec le BCRYPT\_ALG\_gérer\_HMAC\_indicateur d’indicateur. Le \*propriétés de fournisseur peuvent être définies sur la valeur null pour utiliser le fournisseur par défaut pour l’algorithme spécifié. Consultez le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation pour plus d’informations.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Pour spécifier un algorithme CNG de Windows personnalisé à l’aide de compteur Galois/Mode de chiffrement avec la validation, créez un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance qui contient les informations algorithmiques :

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pour spécifier un algorithme CNG de Windows personnalisé à l’aide de compteur Galois/Mode de chiffrement avec la validation, créez un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance qui contient les informations algorithmiques :

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> L’algorithme de chiffrement symétrique doit avoir une longueur de clé > = 128 bits, une taille de bloc de 128 bits exactement, et il doit prendre en charge le chiffrement GCM. Vous pouvez définir le [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propriété null pour utiliser le fournisseur par défaut pour l’algorithme spécifié. Consultez le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation pour plus d’informations.

### <a name="specifying-other-custom-algorithms"></a>Spécification d’autres algorithmes personnalisés

Bien que ne pas exposée comme une API de première classe, le système de Protection des données est suffisamment extensible pour autoriser la spécification de presque n’importe quel type d’algorithme. Par exemple, il est possible de conserver toutes les clés contenues dans un Module de sécurité matériel (HSM) et de fournir une implémentation personnalisée de la base les routines de chiffrement et le déchiffrement. Consultez [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) dans [extensibilité de chiffrement de base](xref:security/data-protection/extensibility/core-crypto) pour plus d’informations.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Clés de persistance lors de l’hébergement dans un conteneur Docker

Lorsque vous hébergez dans un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) , conteneur de clés doivent être conservées dans le :

* Un dossier qui est un volume Docker qui persiste au-delà de durée de vie du conteneur, comme un volume partagé ou un volume monté par l’hôte.
* Un fournisseur externe, tel que [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/).

## <a name="see-also"></a>Voir aussi

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
