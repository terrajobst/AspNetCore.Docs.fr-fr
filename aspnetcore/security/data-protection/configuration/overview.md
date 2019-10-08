---
title: Configurer la protection des données ASP.NET Core
author: rick-anderson
description: Découvrez comment configurer la protection des données dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 380293f650c9548c286f98c0447c7ed08b918f2a
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007377"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="0a0bc-103">Configurer la protection des données ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a0bc-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="0a0bc-104">Lorsque le système de protection des données est initialisé, il applique les [paramètres par défaut](xref:security/data-protection/configuration/default-settings) en fonction de l’environnement opérationnel.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="0a0bc-105">Ces paramètres sont généralement appropriés pour les applications qui s’exécutent sur un seul ordinateur.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="0a0bc-106">Dans certains cas, un développeur peut souhaiter modifier les paramètres par défaut :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="0a0bc-107">L’application est répartie sur plusieurs ordinateurs.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="0a0bc-108">Pour des raisons de conformité.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-108">For compliance reasons.</span></span>

<span data-ttu-id="0a0bc-109">Pour ces scénarios, le système de protection des données offre une API de configuration riche.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="0a0bc-110">Comme pour les fichiers de configuration, l’anneau de clé de protection des données doit être protégé à l’aide des autorisations appropriées.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="0a0bc-111">Vous pouvez choisir de chiffrer les clés au repos, mais cela n’empêche pas les attaquants de créer de nouvelles clés.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="0a0bc-112">Par conséquent, la sécurité de votre application est affectée.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="0a0bc-113">L’accès à l’emplacement de stockage configuré avec la protection des données doit être limité à l’application elle-même, comme dans le cas où vous protégeriez les fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="0a0bc-114">Par exemple, si vous choisissez de stocker votre clé Ring sur disque, utilisez les autorisations du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="0a0bc-115">Assurez-vous que l’identité sous laquelle votre application Web s’exécute dispose de l’accès en lecture, en écriture et en création à ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="0a0bc-116">Si vous utilisez le stockage d’objets BLOB Azure, seule l’application Web doit avoir la possibilité de lire, d’écrire ou de créer de nouvelles entrées dans le magasin d’objets BLOB, etc.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-116">If you use Azure Blob Storage, only the web app should have the ability to read, write, or create new entries in the blob store, etc.</span></span>
>
> <span data-ttu-id="0a0bc-117">La méthode d’extension [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) retourne un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="0a0bc-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="0a0bc-118">`IDataProtectionBuilder` expose les méthodes d’extension que vous pouvez lier pour configurer des options de protection des données.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="0a0bc-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="0a0bc-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="0a0bc-120">Pour stocker des clés dans [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurez le système avec [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) dans la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="0a0bc-121">Définissez l’emplacement de stockage de la sonnerie de clé (par exemple, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="0a0bc-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="0a0bc-122">L’emplacement doit être défini, car l’appel de `ProtectKeysWithAzureKeyVault` implémente un [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) qui désactive les paramètres de protection automatique des données, y compris l’emplacement de stockage de la sonnerie de clé.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="0a0bc-123">L’exemple précédent utilise le stockage d’objets BLOB Azure pour conserver l’anneau de clé.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="0a0bc-124">Pour plus d’informations, consultez [Key Storage Providers : Stockage Azure](xref:security/data-protection/implementation/key-storage-providers#azure-storage).</span><span class="sxs-lookup"><span data-stu-id="0a0bc-124">For more information, see [Key storage providers: Azure Storage](xref:security/data-protection/implementation/key-storage-providers#azure-storage).</span></span> <span data-ttu-id="0a0bc-125">Vous pouvez également conserver l’anneau de clé localement avec [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="0a0bc-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="0a0bc-126">Le `keyIdentifier` est l’identificateur de clé du coffre de clés utilisé pour le chiffrement à clé.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-126">The `keyIdentifier` is the key vault key identifier used for key encryption.</span></span> <span data-ttu-id="0a0bc-127">Par exemple, une clé créée dans le coffre de clés nommé `dataprotection` dans la `contosokeyvault` a l’identificateur de clé `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-127">For example, a key created in key vault named `dataprotection` in the `contosokeyvault` has the key identifier `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span></span> <span data-ttu-id="0a0bc-128">Fournissez l’application avec les autorisations de clé de **désencapsulage** et de retour à la **ligne** pour le coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-128">Provide the app with **Unwrap Key** and **Wrap Key** permissions to the key vault.</span></span>

<span data-ttu-id="0a0bc-129">surcharges `ProtectKeysWithAzureKeyVault` :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-129">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="0a0bc-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) autorise l’utilisation d’un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) pour permettre au système de protection des données d’utiliser le coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="0a0bc-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) autorise l’utilisation d’un `ClientId` et [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) pour permettre au système de protection des données d’utiliser le coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="0a0bc-132">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) autorise l’utilisation d’un `ClientId` et `ClientSecret` pour permettre au système de protection des données d’utiliser le coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-132">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="0a0bc-133">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="0a0bc-133">PersistKeysToFileSystem</span></span>

<span data-ttu-id="0a0bc-134">Pour stocker des clés sur un partage UNC au lieu de l’emplacement *% LocalAppData%* par défaut, configurez le système avec [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="0a0bc-134">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="0a0bc-135">Si vous modifiez l’emplacement de persistance des clés, le système ne chiffre plus automatiquement les clés au repos, car il ne sait pas si DPAPI est un mécanisme de chiffrement approprié.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-135">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="0a0bc-136">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="0a0bc-136">ProtectKeysWith\*</span></span>

<span data-ttu-id="0a0bc-137">Vous pouvez configurer le système pour protéger les clés au repos en appelant l’une des API de configuration [ProtectKeysWith @ no__t-1](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) .</span><span class="sxs-lookup"><span data-stu-id="0a0bc-137">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="0a0bc-138">Prenons l’exemple ci-dessous, qui stocke les clés sur un partage UNC et chiffre ces clés au repos avec un certificat X. 509 spécifique :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-138">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0a0bc-139">Dans ASP.NET Core 2,1 ou version ultérieure, vous pouvez fournir un [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) à [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), tel qu’un certificat chargé à partir d’un fichier :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-139">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

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

<span data-ttu-id="0a0bc-140">Pour obtenir plus d’exemples et de discussion sur les mécanismes de chiffrement à clé intégrés, consultez [chiffrement de clé au repos](xref:security/data-protection/implementation/key-encryption-at-rest) .</span><span class="sxs-lookup"><span data-stu-id="0a0bc-140">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="0a0bc-141">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="0a0bc-141">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="0a0bc-142">Dans ASP.NET Core 2,1 ou version ultérieure, vous pouvez faire pivoter des certificats et déchiffrer des clés au repos à l’aide d’un tableau de certificats [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) avec [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="0a0bc-142">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

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

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="0a0bc-143">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="0a0bc-143">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="0a0bc-144">Pour configurer le système pour qu’il utilise une durée de vie de clé de 14 jours au lieu des jours par défaut de 90, utilisez [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="0a0bc-144">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="0a0bc-145">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="0a0bc-145">SetApplicationName</span></span>

<span data-ttu-id="0a0bc-146">Par défaut, le système de protection des données isole les applications les unes des autres en fonction de leurs chemins [racine de contenu](xref:fundamentals/index#content-root) , même s’ils partagent le même référentiel de clé physique.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-146">By default, the Data Protection system isolates apps from one another based on their [content root](xref:fundamentals/index#content-root) paths, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="0a0bc-147">Cela empêche les applications de comprendre les charges utiles protégées par les autres.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-147">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="0a0bc-148">Pour partager des charges utiles protégées entre les applications :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-148">To share protected payloads among apps:</span></span>

* <span data-ttu-id="0a0bc-149">Configurez <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> dans chaque application avec la même valeur.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-149">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="0a0bc-150">Utilisez la même version de la pile de l’API de protection des données dans les applications.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-150">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="0a0bc-151">Effectuez l' **une** des opérations suivantes dans les fichiers projet des applications :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-151">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="0a0bc-152">Référencez la même version du Framework partagé par le biais du sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0a0bc-152">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="0a0bc-153">Référencez la même version du [package de protection des données](xref:security/data-protection/introduction#package-layout) .</span><span class="sxs-lookup"><span data-stu-id="0a0bc-153">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="0a0bc-154">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="0a0bc-154">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="0a0bc-155">Vous pouvez avoir un scénario dans lequel vous ne voulez pas qu’une application passe automatiquement des clés (créer de nouvelles clés), car elles approchent de l’expiration.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-155">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="0a0bc-156">Par exemple, les applications peuvent être configurées dans une relation principale/secondaire, où seule l’application principale est responsable des problèmes de gestion des clés et les applications secondaires disposent simplement d’une vue en lecture seule de l’anneau de clé.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-156">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="0a0bc-157">Les applications secondaires peuvent être configurées pour traiter l’anneau clé en lecture seule en configurant le système avec <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*> :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-157">The secondary apps can be configured to treat the key ring as read-only by configuring the system with <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="0a0bc-158">Isolation par application</span><span class="sxs-lookup"><span data-stu-id="0a0bc-158">Per-application isolation</span></span>

<span data-ttu-id="0a0bc-159">Lorsque le système de protection des données est fourni par un hôte ASP.NET Core, il isole automatiquement les applications les unes des autres, même si celles-ci s’exécutent sous le même compte de processus de travail et utilisent le même support de gestion de la clé principale.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-159">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="0a0bc-160">Cela est quelque peu similaire au modificateur IsolateApps de l’élément `<machineKey>` de System. Web.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-160">This is somewhat similar to the IsolateApps modifier from System.Web's `<machineKey>` element.</span></span>

<span data-ttu-id="0a0bc-161">Le mécanisme d’isolation fonctionne en considérant chaque application sur l’ordinateur local comme un locataire unique, de sorte que la racine <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> pour une application donnée comprend automatiquement l’ID d’application en tant que discriminateur.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-161">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="0a0bc-162">L’ID unique de l’application est le chemin d’accès physique de l’application :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-162">The app's unique ID is the app's physical path:</span></span>

* <span data-ttu-id="0a0bc-163">Pour les applications hébergées dans IIS, l’ID unique est le chemin d’accès physique IIS de l’application.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-163">For apps hosted in IIS, the unique ID is the IIS physical path of the app.</span></span> <span data-ttu-id="0a0bc-164">Si une application est déployée dans un environnement de batterie de serveurs Web, cette valeur est stable en supposant que les environnements IIS sont configurés de façon similaire sur tous les ordinateurs de la batterie de serveurs Web.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-164">If an app is deployed in a web farm environment, this value is stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>
* <span data-ttu-id="0a0bc-165">Pour les applications auto-hébergées exécutées sur le [serveur Kestrel](xref:fundamentals/servers/index#kestrel), l’ID unique est le chemin d’accès physique à l’application sur le disque.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-165">For self-hosted apps running on the [Kestrel server](xref:fundamentals/servers/index#kestrel), the unique ID is the physical path to the app on disk.</span></span>

<span data-ttu-id="0a0bc-166">L’identificateur unique est conçu pour survivre aux réinitialisations @ no__t-0both de l’application individuelle et de l’ordinateur lui-même.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-166">The unique identifier is designed to survive resets&mdash;both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="0a0bc-167">Ce mécanisme d’isolation suppose que les applications ne sont pas malveillantes.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-167">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="0a0bc-168">Une application malveillante peut toujours avoir un impact sur les autres applications qui s’exécutent sous le même compte de processus de travail.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-168">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="0a0bc-169">Dans un environnement d’hébergement partagé où les applications sont mutuellement non approuvées, le fournisseur d’hébergement doit prendre des mesures pour garantir l’isolement au niveau du système d’exploitation entre les applications, y compris la séparation des dépôts de clé sous-jacents des applications.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-169">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="0a0bc-170">Si le système de protection des données n’est pas fourni par un hôte ASP.NET Core (par exemple, si vous l’instanciez via le type concret `DataProtectionProvider`), l’isolation des applications est désactivée par défaut.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-170">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="0a0bc-171">Lorsque l’isolation des applications est désactivée, toutes les applications sauvegardées par le même support de génération de clé peuvent partager des charges utiles tant qu’elles fournissent les [objectifs](xref:security/data-protection/consumer-apis/purpose-strings)appropriés.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-171">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="0a0bc-172">Pour fournir l’isolation des applications dans cet environnement, appelez la méthode [SetApplicationName](#setapplicationname) sur l’objet de configuration et fournissez un nom unique pour chaque application.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-172">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="0a0bc-173">Modification des algorithmes avec UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="0a0bc-173">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="0a0bc-174">La pile de protection des données vous permet de modifier l’algorithme par défaut utilisé par les clés nouvellement générées.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-174">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="0a0bc-175">La façon la plus simple de procéder consiste à appeler [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) à partir du rappel de configuration :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-175">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

<span data-ttu-id="0a0bc-176">Le EncryptionAlgorithm par défaut est AES-256-CBC et le ValidationAlgorithm par défaut est HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-176">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="0a0bc-177">La stratégie par défaut peut être définie par un administrateur système via une stratégie à l’ensemble de l' [ordinateur](xref:security/data-protection/configuration/machine-wide-policy), mais un appel explicite à `UseCryptographicAlgorithms` remplace la stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-177">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="0a0bc-178">L’appel de `UseCryptographicAlgorithms` vous permet de spécifier l’algorithme souhaité à partir d’une liste prédéfinie prédéfinie.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-178">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="0a0bc-179">Vous n’avez pas besoin de vous soucier de l’implémentation de l’algorithme.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-179">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="0a0bc-180">Dans le scénario ci-dessus, le système de protection des données tente d’utiliser l’implémentation CNG d’AES s’il s’exécute sur Windows.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-180">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="0a0bc-181">Dans le cas contraire, elle revient à la classe gérée [System. Security. Cryptography. AES](/dotnet/api/system.security.cryptography.aes) .</span><span class="sxs-lookup"><span data-stu-id="0a0bc-181">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="0a0bc-182">Vous pouvez spécifier manuellement une implémentation à l’aide d’un appel à [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="0a0bc-182">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="0a0bc-183">La modification des algorithmes n’affecte pas les clés existantes dans l’anneau de clé.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-183">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="0a0bc-184">Il affecte uniquement les clés nouvellement générées.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-184">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="0a0bc-185">Spécification d’algorithmes managés personnalisés</span><span class="sxs-lookup"><span data-stu-id="0a0bc-185">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0a0bc-186">Pour spécifier des algorithmes managés personnalisés, créez une instance [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) qui pointe vers les types d’implémentation :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-186">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0a0bc-187">Pour spécifier des algorithmes managés personnalisés, créez une instance [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) qui pointe vers les types d’implémentation :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-187">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

::: moniker-end

<span data-ttu-id="0a0bc-188">En général, les propriétés \*Type doivent pointer vers concrète, en instanciant (via une implémentation de constructeur sans paramètre public) de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) et [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), bien que les cas spéciaux du système soient des valeurs comme `typeof(Aes)` pour pure.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-188">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="0a0bc-189">L’SymmetricAlgorithm doit avoir une longueur de clé de ≥ 128 bits et une taille de bloc de ≥ 64 bits, et doit prendre en charge le chiffrement en mode CBC avec un remplissage de #7 PKCS.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-189">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="0a0bc-190">L’opération KeyedHashAlgorithm doit avoir une taille Digest de > = 128 bits, et elle doit prendre en charge des clés de longueur égale à la longueur Digest de l’algorithme de hachage.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-190">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="0a0bc-191">KeyedHashAlgorithm n’est pas strictement requis pour être HMAC.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-191">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="0a0bc-192">Spécification d’algorithmes CNG Windows personnalisés</span><span class="sxs-lookup"><span data-stu-id="0a0bc-192">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0a0bc-193">Pour spécifier un algorithme CNG Windows personnalisé à l’aide du chiffrement en mode CBC avec validation HMAC, créez une instance [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-193">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0a0bc-194">Pour spécifier un algorithme CNG Windows personnalisé à l’aide du chiffrement en mode CBC avec validation HMAC, créez une instance [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-194">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0a0bc-195">L’algorithme de chiffrement par bloc symétrique doit avoir une longueur de clé de > = 128 bits, une taille de bloc de > = 64 bits et doit prendre en charge le chiffrement en mode CBC avec un remplissage de #7 PKCS.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-195">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="0a0bc-196">L’algorithme de hachage doit avoir une taille Digest de > = 128 bits et doit prendre en charge l’ouverture avec l’indicateur BCRYPT @ no__t-0ALG @ no__t-1HANDLE @ no__t-2HMAC @ no__t-3FLAG.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-196">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="0a0bc-197">Les propriétés \*Provider peuvent avoir la valeur null pour utiliser le fournisseur par défaut pour l’algorithme spécifié.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-197">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="0a0bc-198">Pour plus d’informations, consultez la documentation de [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) .</span><span class="sxs-lookup"><span data-stu-id="0a0bc-198">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0a0bc-199">Pour spécifier un algorithme CNG Windows personnalisé à l’aide du chiffrement du mode Galois/Counter avec validation, créez une instance [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0a0bc-200">Pour spécifier un algorithme CNG Windows personnalisé à l’aide du chiffrement du mode Galois/Counter avec validation, créez une instance [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-200">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0a0bc-201">L’algorithme de chiffrement par bloc symétrique doit avoir une longueur de clé de > = 128 bits, une taille de bloc de 128 bits exactement et le chiffrement GCM doit être pris en charge.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-201">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="0a0bc-202">Vous pouvez définir la propriété [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) sur la valeur null pour utiliser le fournisseur par défaut pour l’algorithme spécifié.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-202">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="0a0bc-203">Pour plus d’informations, consultez la documentation de [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) .</span><span class="sxs-lookup"><span data-stu-id="0a0bc-203">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="0a0bc-204">Spécification d’autres algorithmes personnalisés</span><span class="sxs-lookup"><span data-stu-id="0a0bc-204">Specifying other custom algorithms</span></span>

<span data-ttu-id="0a0bc-205">Bien qu’elles ne soient pas exposées comme API de première classe, le système de protection des données est suffisamment extensible pour permettre la spécification de presque n’importe quel type d’algorithme.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-205">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="0a0bc-206">Par exemple, il est possible de conserver toutes les clés contenues dans un module de sécurité matériel (HSM) et de fournir une implémentation personnalisée des routines de chiffrement et de déchiffrement de base.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-206">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="0a0bc-207">Pour plus d’informations, consultez [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) dans [Core Cryptography Extensibility](xref:security/data-protection/extensibility/core-crypto) .</span><span class="sxs-lookup"><span data-stu-id="0a0bc-207">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="0a0bc-208">Persistance des clés lors de l’hébergement dans un conteneur d’ancrage</span><span class="sxs-lookup"><span data-stu-id="0a0bc-208">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="0a0bc-209">Lors de l’hébergement dans un conteneur d' [ancrage](/dotnet/standard/microservices-architecture/container-docker-introduction/) , les clés doivent être gérées dans :</span><span class="sxs-lookup"><span data-stu-id="0a0bc-209">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="0a0bc-210">Dossier qui est un volume d’ancrage qui persiste au-delà de la durée de vie du conteneur, par exemple un volume partagé ou un volume monté sur un ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-210">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="0a0bc-211">Fournisseur externe, tel que [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [redims](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="0a0bc-211">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="persisting-keys-with-redis"></a><span data-ttu-id="0a0bc-212">Persistance des clés avec des ReDim</span><span class="sxs-lookup"><span data-stu-id="0a0bc-212">Persisting keys with Redis</span></span>

<span data-ttu-id="0a0bc-213">Seules les versions de ReDim qui prennent en charge la [persistance des données redims](/azure/azure-cache-for-redis/cache-how-to-premium-persistence) doivent être utilisées pour stocker des clés.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-213">Only Redis versions supporting [Redis Data Persistence](/azure/azure-cache-for-redis/cache-how-to-premium-persistence) should be used to store keys.</span></span> <span data-ttu-id="0a0bc-214">Le [stockage d’objets BLOB Azure](/azure/storage/blobs/storage-blobs-introduction) est persistant et peut être utilisé pour stocker des clés.</span><span class="sxs-lookup"><span data-stu-id="0a0bc-214">[Azure Blob storage](/azure/storage/blobs/storage-blobs-introduction) is persistent and can be used to store keys.</span></span> <span data-ttu-id="0a0bc-215">Pour plus d’informations, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/13476).</span><span class="sxs-lookup"><span data-stu-id="0a0bc-215">For more information, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/13476).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a0bc-216">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0a0bc-216">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
