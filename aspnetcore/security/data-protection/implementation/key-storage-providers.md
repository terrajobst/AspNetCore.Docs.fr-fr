---
title: Fournisseurs de stockage de clés dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur les fournisseurs de stockage de clés dans ASP.NET Core et comment configurer les emplacements de stockage de clés.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 35e2cea4b6404af94de95352dc6ebf3071925cb1
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477148"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="0a39a-103">Fournisseurs de stockage de clés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a39a-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="0a39a-104">Le système de protection des données [utilise un mécanisme de découverte par défaut](xref:security/data-protection/configuration/default-settings) pour déterminer où les clés de chiffrement doivent être rendue persistante.</span><span class="sxs-lookup"><span data-stu-id="0a39a-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="0a39a-105">Le développeur peut remplacer le mécanisme de découverte par défaut et spécifier manuellement l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="0a39a-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="0a39a-106">Si vous spécifiez un emplacement de persistance des clés explicites, le système de protection des données annule l’inscription du chiffrement à clé par défaut au mécanisme de rest, donc les clés ne sont plus chiffrés au repos.</span><span class="sxs-lookup"><span data-stu-id="0a39a-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="0a39a-107">Il est recommandé que vous en outre [spécifier un mécanisme de chiffrement à clé explicite](xref:security/data-protection/implementation/key-encryption-at-rest) pour les déploiements de production.</span><span class="sxs-lookup"><span data-stu-id="0a39a-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="0a39a-108">Système de fichiers</span><span class="sxs-lookup"><span data-stu-id="0a39a-108">File system</span></span>

<span data-ttu-id="0a39a-109">Pour configurer un référentiel de clé basée sur le système de fichiers, appelez le [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) routine de configuration comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0a39a-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="0a39a-110">Fournir un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointant vers le référentiel où les clés doivent être stockées :</span><span class="sxs-lookup"><span data-stu-id="0a39a-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="0a39a-111">Le `DirectoryInfo` peut pointer vers un répertoire sur l’ordinateur local, ou il peut pointer vers un dossier sur un partage réseau.</span><span class="sxs-lookup"><span data-stu-id="0a39a-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="0a39a-112">Si vous pointez vers un répertoire sur l’ordinateur local (et le scénario est que seules les applications sur l’ordinateur local requièrent l’accès à utiliser ce référentiel), envisagez d’utiliser [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="0a39a-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="0a39a-113">Sinon, envisagez d’utiliser un [certificat X.509](xref:security/data-protection/implementation/key-encryption-at-rest) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="0a39a-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="0a39a-114">Azure et Redis</span><span class="sxs-lookup"><span data-stu-id="0a39a-114">Azure and Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0a39a-115">Le [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) et [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) packages permettent de stocker des clés de protection des données dans le stockage Azure ou un Redis cache.</span><span class="sxs-lookup"><span data-stu-id="0a39a-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="0a39a-116">Les clés peuvent être partagées entre plusieurs instances d’une application web.</span><span class="sxs-lookup"><span data-stu-id="0a39a-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="0a39a-117">Applications peuvent partager des cookies d’authentification ou de protection de CSRF sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="0a39a-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0a39a-118">Le [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) et [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages permettent de stocker des clés de protection des données dans le stockage Azure ou un cache Redis.</span><span class="sxs-lookup"><span data-stu-id="0a39a-118">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="0a39a-119">Les clés peuvent être partagées entre plusieurs instances d’une application web.</span><span class="sxs-lookup"><span data-stu-id="0a39a-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="0a39a-120">Applications peuvent partager des cookies d’authentification ou de protection de CSRF sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="0a39a-120">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

<span data-ttu-id="0a39a-121">Pour configurer le fournisseur de stockage Blob Azure, appelez une de la [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) surcharges :</span><span class="sxs-lookup"><span data-stu-id="0a39a-121">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0a39a-122">Pour configurer sur Redis, appelez une de la [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) surcharges :</span><span class="sxs-lookup"><span data-stu-id="0a39a-122">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToStackExchangeRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0a39a-123">Pour configurer sur Redis, appelez une de la [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) surcharges :</span><span class="sxs-lookup"><span data-stu-id="0a39a-123">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="0a39a-124">Pour plus d’informations, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="0a39a-124">For more information, see the following topics:</span></span>

* [<span data-ttu-id="0a39a-125">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="0a39a-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="0a39a-126">Cache Redis Azure</span><span class="sxs-lookup"><span data-stu-id="0a39a-126">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="0a39a-127">exemples d’ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="0a39a-127">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a><span data-ttu-id="0a39a-128">Registre</span><span class="sxs-lookup"><span data-stu-id="0a39a-128">Registry</span></span>

<span data-ttu-id="0a39a-129">**S’applique uniquement aux déploiements de Windows.**</span><span class="sxs-lookup"><span data-stu-id="0a39a-129">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="0a39a-130">Parfois l’application ne peut pas avoir accès en écriture au système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="0a39a-130">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="0a39a-131">Imaginez un scénario dans lequel une application s’exécute comme compte de service virtuel (tel que *w3wp.exe*d’identité du pool d’application).</span><span class="sxs-lookup"><span data-stu-id="0a39a-131">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="0a39a-132">Dans ce cas, l’administrateur peut configurer une clé de Registre qui est accessible par l’identité de compte de service.</span><span class="sxs-lookup"><span data-stu-id="0a39a-132">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="0a39a-133">Appelez le [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) méthode d’extension comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0a39a-133">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="0a39a-134">Fournir un [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointant vers l’emplacement de stockage des clés de chiffrement :</span><span class="sxs-lookup"><span data-stu-id="0a39a-134">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="0a39a-135">Nous vous recommandons d’utiliser [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="0a39a-135">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="0a39a-136">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0a39a-136">Entity Framework Core</span></span>

<span data-ttu-id="0a39a-137">Le [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package fournit un mécanisme pour stocker les clés de protection des données à une base de données à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0a39a-137">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="0a39a-138">Le `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` package NuGet doit être ajouté au fichier projet, il n’est pas dans le cadre de la [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0a39a-138">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="0a39a-139">Avec ce package, les clés peuvent être partagées entre plusieurs instances d’une application web.</span><span class="sxs-lookup"><span data-stu-id="0a39a-139">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="0a39a-140">Pour configurer le fournisseur EF Core, appelez le [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) méthode :</span><span class="sxs-lookup"><span data-stu-id="0a39a-140">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

<span data-ttu-id="0a39a-141">Le paramètre générique, `TContext`, doit hériter de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) et [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="0a39a-141">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="0a39a-142">Dépôt de clé personnalisé</span><span class="sxs-lookup"><span data-stu-id="0a39a-142">Custom key repository</span></span>

<span data-ttu-id="0a39a-143">Si les mécanismes d’origine ne sont pas appropriées, le développeur peut spécifier leur propre mécanisme de persistance des clés en fournissant un personnalisé [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="0a39a-143">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
