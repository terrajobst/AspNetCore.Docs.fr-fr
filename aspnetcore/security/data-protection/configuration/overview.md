---
title: "Configuration de Protection des données dans ASP.NET Core"
author: rick-anderson
description: "Découvrez comment configurer la Protection des données dans ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: b560f360b4962f2d50e0fdc9a99ab389eb113f43
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a><span data-ttu-id="8bad8-103">Configuration de Protection des données dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8bad8-103">Configuring Data Protection in ASP.NET Core</span></span>

<span data-ttu-id="8bad8-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8bad8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8bad8-105">Lorsque le système de Protection des données est initialisé, il s’applique [paramètres par défaut](xref:security/data-protection/configuration/default-settings) en fonction de l’environnement d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="8bad8-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="8bad8-106">Ces paramètres sont généralement appropriés pour les applications qui s’exécutent sur un seul ordinateur.</span><span class="sxs-lookup"><span data-stu-id="8bad8-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="8bad8-107">Il existe des cas où un développeur peut souhaiter modifier les paramètres par défaut, peut-être parce que son application est répartie sur plusieurs ordinateurs ou pour des raisons de compatibilité.</span><span class="sxs-lookup"><span data-stu-id="8bad8-107">There are cases where a developer may want to change the default settings, perhaps because their app is spread across multiple machines or for compliance reasons.</span></span> <span data-ttu-id="8bad8-108">Pour ces scénarios, le système de Protection des données offre une API de configuration complet.</span><span class="sxs-lookup"><span data-stu-id="8bad8-108">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

<span data-ttu-id="8bad8-109">Il existe une méthode d’extension [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) qui retourne un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="8bad8-109">There's an extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) that returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="8bad8-110">`IDataProtectionBuilder`expose des méthodes d’extension que vous pouvez chaîner des options pour configurer la Protection des données.</span><span class="sxs-lookup"><span data-stu-id="8bad8-110">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

## <a name="persistkeystofilesystem"></a><span data-ttu-id="8bad8-111">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="8bad8-111">PersistKeysToFileSystem</span></span>

<span data-ttu-id="8bad8-112">Pour stocker les clés sur un partage UNC plutôt qu’à la *%LocalAppData%* emplacement par défaut, configurez le système avec [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="8bad8-112">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="8bad8-113">Si vous modifiez l’emplacement de la persistance des clés, le système n’est plus automatiquement chiffre au repos, car il ne sait pas si DPAPI est un mécanisme de chiffrement appropriés.</span><span class="sxs-lookup"><span data-stu-id="8bad8-113">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="8bad8-114">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="8bad8-114">ProtectKeysWith\*</span></span>

<span data-ttu-id="8bad8-115">Vous pouvez configurer le système pour protéger les clés au repos en appelant une de le [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) API de configuration.</span><span class="sxs-lookup"><span data-stu-id="8bad8-115">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="8bad8-116">Prenons l’exemple ci-dessous, qui stocke les clés sur un partage UNC et chiffre ces clés au repos avec un certificat X.509 spécifique :</span><span class="sxs-lookup"><span data-stu-id="8bad8-116">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="8bad8-117">Consultez [clé de chiffrement à l’arrêt](xref:security/data-protection/implementation/key-encryption-at-rest) pour plus d’exemples et une discussion sur les mécanismes de chiffrement à clé intégrés.</span><span class="sxs-lookup"><span data-stu-id="8bad8-117">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="8bad8-118">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="8bad8-118">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="8bad8-119">Permet de configurer le système pour utiliser une durée de vie de clé de 14 jours au lieu de la valeur par défaut de 90 jours, [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="8bad8-119">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="8bad8-120">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="8bad8-120">SetApplicationName</span></span>

<span data-ttu-id="8bad8-121">Par défaut, le système de Protection des données isole les applications à partir d’une autre, même si elles partagent le même référentiel clé physique.</span><span class="sxs-lookup"><span data-stu-id="8bad8-121">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="8bad8-122">Cela empêche les applications à partir de la présentation des charges protégé de l’autre.</span><span class="sxs-lookup"><span data-stu-id="8bad8-122">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="8bad8-123">Pour partager les charges utiles de protégé entre deux applications, utilisez [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) avec la même valeur pour chaque application :</span><span class="sxs-lookup"><span data-stu-id="8bad8-123">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="8bad8-124">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="8bad8-124">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="8bad8-125">Vous pouvez avoir un scénario dans lequel vous souhaitez une application pour restaurer automatiquement les clés (créer des clés) qu’ils approchent d’expiration.</span><span class="sxs-lookup"><span data-stu-id="8bad8-125">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="8bad8-126">Un exemple peut être définies dans une relation principal/secondaire, où le principal de l’application est responsable de problèmes de gestion de clés et des applications secondaires ont simplement une vue en lecture seule de l’anneau de clé des applications.</span><span class="sxs-lookup"><span data-stu-id="8bad8-126">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="8bad8-127">Les applications secondaires peuvent être configurées pour traiter l’anneau de clé comme étant en lecture seule en configurant le système avec [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="8bad8-127">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="8bad8-128">Isolation d’application</span><span class="sxs-lookup"><span data-stu-id="8bad8-128">Per-application isolation</span></span>

<span data-ttu-id="8bad8-129">Lorsque le système de Protection des données est fourni par un hôte ASP.NET Core, elle isole automatiquement les applications à partir d’une autre, même si ces applications sont en cours d’exécution sous le même compte de processus de travail et utilisent le même matériel de gestion de clés maître.</span><span class="sxs-lookup"><span data-stu-id="8bad8-129">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="8bad8-130">Cela est quelque peu similaire au modificateur IsolateApps à partir de System.Web  **\<machineKey >** élément.</span><span class="sxs-lookup"><span data-stu-id="8bad8-130">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="8bad8-131">Le mécanisme d’isolation fonctionne en tenant compte de chaque application sur l’ordinateur local en tant qu’un client unique, donc le [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) racine pour une application donnée inclut automatiquement l’ID d’application en tant que discriminateur.</span><span class="sxs-lookup"><span data-stu-id="8bad8-131">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="8bad8-132">ID unique de l’application provient d’un des deux emplacements :</span><span class="sxs-lookup"><span data-stu-id="8bad8-132">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="8bad8-133">Si l’application est hébergée dans IIS, l’identificateur unique est le chemin d’accès de l’application configuration.</span><span class="sxs-lookup"><span data-stu-id="8bad8-133">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="8bad8-134">Si une application est déployée dans un environnement de batterie de serveurs web, cette valeur doit être stable, en supposant que les environnements d’IIS sont configurés de la même manière sur tous les ordinateurs dans la batterie de serveurs web.</span><span class="sxs-lookup"><span data-stu-id="8bad8-134">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="8bad8-135">Si l’application n’est pas hébergée dans IIS, l’identificateur unique est le chemin d’accès physique de l’application.</span><span class="sxs-lookup"><span data-stu-id="8bad8-135">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="8bad8-136">L’identificateur unique est conçu pour résister aux réinitialisations &mdash; à la fois de l’application individuelle et de l’ordinateur lui-même.</span><span class="sxs-lookup"><span data-stu-id="8bad8-136">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="8bad8-137">Ce mécanisme d’isolation suppose que les applications ne sont pas malveillantes.</span><span class="sxs-lookup"><span data-stu-id="8bad8-137">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="8bad8-138">Une application malveillante peut toujours avoir un impact sur toute autre application en cours d’exécution sous le même compte de processus de travail.</span><span class="sxs-lookup"><span data-stu-id="8bad8-138">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="8bad8-139">Dans un environnement d’hébergement partagé où les applications sont mutuellement non approuvées, le fournisseur d’hébergement doit prennent des mesures pour garantir une isolation au niveau du système d’exploitation entre les applications, y compris la séparation des applications sous-jacent référentiels clés.</span><span class="sxs-lookup"><span data-stu-id="8bad8-139">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="8bad8-140">Si le système de Protection des données n’est pas fourni par un hôte ASP.NET Core (par exemple, si vous instanciez via la `DataProtectionProvider` type concret) le niveau d’isolement d’application est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="8bad8-140">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="8bad8-141">Lorsque le niveau d’isolement d’application est désactivé, soutenues par le même jeu de clés de toutes les applications peuvent partager les charges utiles tant qu’ils fournissent les [à des fins](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="8bad8-141">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="8bad8-142">Pour assurer l’isolation d’application dans cet environnement, appelez le [SetApplicationName](#setapplicationname) méthode sur la configuration de l’objet et fournissez un nom unique pour chaque application.</span><span class="sxs-lookup"><span data-stu-id="8bad8-142">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="8bad8-143">Modification des algorithmes avec UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="8bad8-143">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="8bad8-144">La pile de Protection des données vous permet de modifier l’algorithme par défaut utilisé par les clés qui vient d’être générées.</span><span class="sxs-lookup"><span data-stu-id="8bad8-144">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="8bad8-145">La façon la plus simple pour ce faire consiste à appeler [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) à partir du rappel de configuration :</span><span class="sxs-lookup"><span data-stu-id="8bad8-145">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8bad8-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8bad8-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8bad8-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8bad8-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="8bad8-148">La valeur par défaut EncryptionAlgorithm est AES-256-CBC, et la valeur par défaut ValidationAlgorithm est HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="8bad8-148">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="8bad8-149">La stratégie par défaut peut être définie par un administrateur système via une [stratégie au niveau machine](xref:security/data-protection/configuration/machine-wide-policy), mais un appel explicite à `UseCryptographicAlgorithms` remplace la stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="8bad8-149">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="8bad8-150">Appel de `UseCryptographicAlgorithms` permet de spécifier l’algorithme de votre choix dans une liste prédéfinie d’intégrés.</span><span class="sxs-lookup"><span data-stu-id="8bad8-150">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="8bad8-151">Vous n’avez pas besoin de vous soucier de l’implémentation de l’algorithme.</span><span class="sxs-lookup"><span data-stu-id="8bad8-151">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="8bad8-152">Dans le scénario ci-dessus, le système de Protection de données tente d’utiliser l’implémentation CNG de AES s’exécutant sur Windows.</span><span class="sxs-lookup"><span data-stu-id="8bad8-152">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="8bad8-153">Sinon, il revient à managé [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.</span><span class="sxs-lookup"><span data-stu-id="8bad8-153">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="8bad8-154">Vous pouvez spécifier manuellement une implémentation via un appel à [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="8bad8-154">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="8bad8-155">Algorithmes de modification n’affecte pas les clés existantes dans l’anneau de clé.</span><span class="sxs-lookup"><span data-stu-id="8bad8-155">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="8bad8-156">Il affecte uniquement les clés qui vient d’être générées.</span><span class="sxs-lookup"><span data-stu-id="8bad8-156">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="8bad8-157">Spécification des algorithmes personnalisés gérés</span><span class="sxs-lookup"><span data-stu-id="8bad8-157">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8bad8-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8bad8-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8bad8-159">Pour spécifier les algorithmes managés personnalisés, créez un [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance qui pointe vers les types d’implémentation :</span><span class="sxs-lookup"><span data-stu-id="8bad8-159">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8bad8-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8bad8-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8bad8-161">Pour spécifier les algorithmes managés personnalisés, créez un [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance qui pointe vers les types d’implémentation :</span><span class="sxs-lookup"><span data-stu-id="8bad8-161">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="8bad8-162">En général le \*les propriétés de Type doivent pointer vers concrète, implémentations instanciables (via un constructeur sans paramètre public) de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) et [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), même si le système spéciaux-cas des valeurs telles que `typeof(Aes)` pour des raisons pratiques.</span><span class="sxs-lookup"><span data-stu-id="8bad8-162">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="8bad8-163">Le SymmetricAlgorithm doit avoir une longueur de clé de ≥ 128 bits et une taille de bloc de ≥ 64 bits, et il doit prendre en charge le chiffrement en mode CBC avec le remplissage PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="8bad8-163">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="8bad8-164">L’élément KeyedHashAlgorithm impossible doit avoir une taille de condensat de > = 128 bits, et il doit prendre en charge les clés de longueur égale à la longueur de résumé de l’algorithme de hachage.</span><span class="sxs-lookup"><span data-stu-id="8bad8-164">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="8bad8-165">L’élément KeyedHashAlgorithm impossible n’est pas strictement obligatoire pour être HMAC.</span><span class="sxs-lookup"><span data-stu-id="8bad8-165">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="8bad8-166">En spécifiant les algorithmes CNG de Windows personnalisés</span><span class="sxs-lookup"><span data-stu-id="8bad8-166">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8bad8-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8bad8-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8bad8-168">Pour spécifier un algorithme CNG de Windows personnalisé avec le chiffrement d’en mode CBC et validation de HMAC, créez un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="8bad8-168">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8bad8-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8bad8-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8bad8-170">Pour spécifier un algorithme CNG de Windows personnalisé avec le chiffrement d’en mode CBC et validation de HMAC, créez un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="8bad8-170">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="8bad8-171">L’algorithme de chiffrement symétrique doit avoir une longueur de clé > = 128 bits, une taille de bloc de > = 64 bits, et il doit prendre en charge le chiffrement en mode CBC avec le remplissage PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="8bad8-171">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="8bad8-172">L’algorithme de hachage doit avoir une taille de condensat de > = 128 bits et doivent prendre en charge en cours d’ouverture avec le BCRYPT\_ALG\_gérer\_HMAC\_drapeau.</span><span class="sxs-lookup"><span data-stu-id="8bad8-172">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="8bad8-173">Le \*propriétés du fournisseur peuvent être définies sur la valeur null pour utiliser le fournisseur par défaut pour l’algorithme spécifié.</span><span class="sxs-lookup"><span data-stu-id="8bad8-173">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="8bad8-174">Consultez le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="8bad8-174">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8bad8-175">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8bad8-175">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8bad8-176">Pour spécifier un algorithme CNG de Windows personnalisé à l’aide de chiffrement de compteur/Galois Mode avec validation, créez un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="8bad8-176">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8bad8-177">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8bad8-177">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8bad8-178">Pour spécifier un algorithme CNG de Windows personnalisé à l’aide de chiffrement de compteur/Galois Mode avec validation, créez un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="8bad8-178">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="8bad8-179">L’algorithme de chiffrement symétrique doit avoir une longueur de clé > = 128 bits, une taille de bloc de 128 bits exactement, et il doit prendre en charge le chiffrement GCM.</span><span class="sxs-lookup"><span data-stu-id="8bad8-179">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="8bad8-180">Vous pouvez définir le [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propriété null pour utiliser le fournisseur par défaut pour l’algorithme spécifié.</span><span class="sxs-lookup"><span data-stu-id="8bad8-180">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="8bad8-181">Consultez le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="8bad8-181">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="8bad8-182">Spécification d’autres algorithmes personnalisés</span><span class="sxs-lookup"><span data-stu-id="8bad8-182">Specifying other custom algorithms</span></span>

<span data-ttu-id="8bad8-183">Bien que ne pas exposées en tant qu’une API de première classe, le système de Protection des données est suffisamment extensible pour autoriser la spécification de presque n’importe quel type d’algorithme.</span><span class="sxs-lookup"><span data-stu-id="8bad8-183">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="8bad8-184">Par exemple, il est possible de conserver toutes les clés contenues dans un Module de sécurité matériel (HSM) et pour fournir une implémentation personnalisée de la base de routines de chiffrement et le déchiffrement.</span><span class="sxs-lookup"><span data-stu-id="8bad8-184">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="8bad8-185">Consultez [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) dans [extensibilité de chiffrement de base](xref:security/data-protection/extensibility/core-crypto) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="8bad8-185">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="8bad8-186">Clés de persistance lors de l’hébergement dans un conteneur Docker</span><span class="sxs-lookup"><span data-stu-id="8bad8-186">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="8bad8-187">Lors de l’hébergement dans un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) , conteneur de clés doivent être conservées, que ce soit :</span><span class="sxs-lookup"><span data-stu-id="8bad8-187">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="8bad8-188">Un dossier qui est un volume de Docker qui persiste au-delà de la durée de vie du conteneur, comme un volume partagé ou un volume monté d’hôte.</span><span class="sxs-lookup"><span data-stu-id="8bad8-188">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="8bad8-189">Un fournisseur externe, tel que [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="8bad8-189">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="8bad8-190">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="8bad8-190">See also</span></span>

* [<span data-ttu-id="8bad8-191">Scénarios non compatibles avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="8bad8-191">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="8bad8-192">Stratégie à l’échelle de la machine</span><span class="sxs-lookup"><span data-stu-id="8bad8-192">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
