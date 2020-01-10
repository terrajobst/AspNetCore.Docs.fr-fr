---
title: Fournisseurs de stockage de clés dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur les fournisseurs de stockage de clés dans ASP.NET Core et comment configurer les emplacements de stockage de clés.
ms.author: riande
ms.date: 12/05/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: c1d2ac1304230af88e63e1aca441f044b32038fd
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829086"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="4a1c2-103">Fournisseurs de stockage de clés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a1c2-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="4a1c2-104">Le système de protection des données [utilise un mécanisme de découverte par défaut](xref:security/data-protection/configuration/default-settings) pour déterminer où les clés de chiffrement doivent être rendue persistante.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="4a1c2-105">Le développeur peut remplacer le mécanisme de découverte par défaut et spécifier manuellement l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="4a1c2-106">Si vous spécifiez un emplacement de persistance des clés explicites, le système de protection des données annule l’inscription du chiffrement à clé par défaut au mécanisme de rest, donc les clés ne sont plus chiffrés au repos.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="4a1c2-107">Il est recommandé que vous en outre [spécifier un mécanisme de chiffrement à clé explicite](xref:security/data-protection/implementation/key-encryption-at-rest) pour les déploiements de production.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="4a1c2-108">Système de fichiers</span><span class="sxs-lookup"><span data-stu-id="4a1c2-108">File system</span></span>

<span data-ttu-id="4a1c2-109">Pour configurer un référentiel de clé basée sur le système de fichiers, appelez le [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) routine de configuration comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="4a1c2-110">Fournir un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointant vers le référentiel où les clés doivent être stockées :</span><span class="sxs-lookup"><span data-stu-id="4a1c2-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="4a1c2-111">Le `DirectoryInfo` peut pointer vers un répertoire sur l’ordinateur local, ou il peut pointer vers un dossier sur un partage réseau.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="4a1c2-112">Si vous pointez vers un répertoire sur l’ordinateur local (et le scénario est que seules les applications sur l’ordinateur local requièrent l’accès à utiliser ce référentiel), envisagez d’utiliser [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="4a1c2-113">Sinon, envisagez d’utiliser un [certificat X.509](xref:security/data-protection/implementation/key-encryption-at-rest) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="4a1c2-114">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="4a1c2-114">Azure Storage</span></span>

<span data-ttu-id="4a1c2-115">Le package [Microsoft. AspNetCore. dataprotection. AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) permet de stocker les clés de protection des données dans le stockage d’objets BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) package allows storing data protection keys in Azure Blob Storage.</span></span> <span data-ttu-id="4a1c2-116">Les clés peuvent être partagées entre plusieurs instances d’une application web.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="4a1c2-117">Applications peuvent partager des cookies d’authentification ou de protection de CSRF sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

<span data-ttu-id="4a1c2-118">Pour configurer le fournisseur de stockage d’objets BLOB Azure, appelez l’une des surcharges [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) .</span><span class="sxs-lookup"><span data-stu-id="4a1c2-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="4a1c2-119">Si l’application Web s’exécute en tant que service Azure, les jetons d’authentification peuvent être créés automatiquement à l’aide de [Microsoft. Azure. services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="4a1c2-119">If the web app is running as an Azure service, authentication tokens can be automatically created using [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span></span>

```csharp
var tokenProvider = new AzureServiceTokenProvider();
var token = await tokenProvider.GetAccessTokenAsync("https://storage.azure.com/");
var credentials = new StorageCredentials(new TokenCredential(token));
var storageAccount = new CloudStorageAccount(credentials, "mystorageaccount", "core.windows.net", useHttps: true);
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("my-key-container");

// optional - provision the container automatically
await container.CreateIfNotExistsAsync();

services.AddDataProtection()
    .PersistKeysToAzureBlobStorage(container, "keys.xml");
```

<span data-ttu-id="4a1c2-120">[Pour plus d’informations sur la configuration de l’authentification de service à service](/azure/key-vault/service-to-service-authentication) , consultez.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-120">See [more details about configuring service-to-service authentication.](/azure/key-vault/service-to-service-authentication)</span></span>

## <a name="redis"></a><span data-ttu-id="4a1c2-121">Redis</span><span class="sxs-lookup"><span data-stu-id="4a1c2-121">Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4a1c2-122">Le package [Microsoft. AspNetCore. dataprotection. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) permet de stocker les clés de protection des données dans un cache ReDim.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-122">The [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="4a1c2-123">Les clés peuvent être partagées entre plusieurs instances d’une application web.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-123">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="4a1c2-124">Applications peuvent partager des cookies d’authentification ou de protection de CSRF sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-124">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4a1c2-125">Le package [Microsoft. AspNetCore. dataprotection. redims](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) permet de stocker les clés de protection des données dans un cache ReDim.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-125">The [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="4a1c2-126">Les clés peuvent être partagées entre plusieurs instances d’une application web.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-126">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="4a1c2-127">Applications peuvent partager des cookies d’authentification ou de protection de CSRF sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-127">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4a1c2-128">Pour configurer sur Redis, appelez une de la [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) surcharges :</span><span class="sxs-lookup"><span data-stu-id="4a1c2-128">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

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

<span data-ttu-id="4a1c2-129">Pour configurer sur Redis, appelez une de la [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) surcharges :</span><span class="sxs-lookup"><span data-stu-id="4a1c2-129">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="4a1c2-130">Pour plus d’informations, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="4a1c2-130">For more information, see the following topics:</span></span>

* [<span data-ttu-id="4a1c2-131">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="4a1c2-131">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="4a1c2-132">Cache Redis Azure</span><span class="sxs-lookup"><span data-stu-id="4a1c2-132">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="4a1c2-133">Exemples de DataProtection ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a1c2-133">ASP.NET Core DataProtection samples</span></span>](https://github.com/dotnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="4a1c2-134">Registre</span><span class="sxs-lookup"><span data-stu-id="4a1c2-134">Registry</span></span>

<span data-ttu-id="4a1c2-135">**S’applique uniquement aux déploiements de Windows.**</span><span class="sxs-lookup"><span data-stu-id="4a1c2-135">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="4a1c2-136">Parfois l’application ne peut pas avoir accès en écriture au système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-136">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="4a1c2-137">Imaginez un scénario dans lequel une application s’exécute comme compte de service virtuel (tel que *w3wp.exe*d’identité du pool d’application).</span><span class="sxs-lookup"><span data-stu-id="4a1c2-137">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="4a1c2-138">Dans ce cas, l’administrateur peut configurer une clé de Registre qui est accessible par l’identité de compte de service.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-138">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="4a1c2-139">Appelez le [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) méthode d’extension comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-139">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="4a1c2-140">Fournir un [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointant vers l’emplacement de stockage des clés de chiffrement :</span><span class="sxs-lookup"><span data-stu-id="4a1c2-140">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="4a1c2-141">Nous vous recommandons d’utiliser [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-141">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="4a1c2-142">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4a1c2-142">Entity Framework Core</span></span>

<span data-ttu-id="4a1c2-143">Le [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package fournit un mécanisme pour stocker les clés de protection des données à une base de données à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-143">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="4a1c2-144">Le `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` package NuGet doit être ajouté au fichier projet, il n’est pas dans le cadre de la [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4a1c2-144">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4a1c2-145">Avec ce package, les clés peuvent être partagées entre plusieurs instances d’une application web.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-145">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="4a1c2-146">Pour configurer le fournisseur de EF Core, appelez la méthode [PersistKeysToDbContext\<TContext >](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) :</span><span class="sxs-lookup"><span data-stu-id="4a1c2-146">To configure the EF Core provider, call the [PersistKeysToDbContext\<TContext>](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-20)]

<span data-ttu-id="4a1c2-147">Le paramètre générique, `TContext`, doit hériter de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) et implémenter [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="4a1c2-147">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and implement [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

<span data-ttu-id="4a1c2-148">Créez la table `DataProtectionKeys`.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-148">Create the `DataProtectionKeys` table.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a1c2-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a1c2-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4a1c2-150">Exécutez les commandes suivantes dans la fenêtre **console du gestionnaire de package** (PMC) :</span><span class="sxs-lookup"><span data-stu-id="4a1c2-150">Execute the following commands in the **Package Manager Console** (PMC) window:</span></span>

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4a1c2-151">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4a1c2-151">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4a1c2-152">Exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="4a1c2-152">Execute the following commands in a command shell:</span></span>

```dotnetcli
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

<span data-ttu-id="4a1c2-153">`MyKeysContext` est le `DbContext` défini dans l’exemple de code précédent.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-153">`MyKeysContext` is the `DbContext` defined in the preceding code sample.</span></span> <span data-ttu-id="4a1c2-154">Si vous utilisez un `DbContext` avec un nom différent, remplacez le nom de votre `DbContext` par `MyKeysContext`.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-154">If you're using a `DbContext` with a different name, substitute your `DbContext` name for `MyKeysContext`.</span></span>

<span data-ttu-id="4a1c2-155">La classe/entité `DataProtectionKeys` adopte la structure indiquée dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="4a1c2-155">The `DataProtectionKeys` class/entity adopts the structure shown in the following table.</span></span>

| <span data-ttu-id="4a1c2-156">Propriété/champ</span><span class="sxs-lookup"><span data-stu-id="4a1c2-156">Property/Field</span></span> | <span data-ttu-id="4a1c2-157">Type CLR</span><span class="sxs-lookup"><span data-stu-id="4a1c2-157">CLR Type</span></span> | <span data-ttu-id="4a1c2-158">Type SQL</span><span class="sxs-lookup"><span data-stu-id="4a1c2-158">SQL Type</span></span>              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | <span data-ttu-id="4a1c2-159">`int`, PK, non null</span><span class="sxs-lookup"><span data-stu-id="4a1c2-159">`int`, PK, not null</span></span>   |
| `FriendlyName` | `string` | <span data-ttu-id="4a1c2-160">`nvarchar(MAX)`, null</span><span class="sxs-lookup"><span data-stu-id="4a1c2-160">`nvarchar(MAX)`, null</span></span> |
| `Xml`          | `string` | <span data-ttu-id="4a1c2-161">`nvarchar(MAX)`, null</span><span class="sxs-lookup"><span data-stu-id="4a1c2-161">`nvarchar(MAX)`, null</span></span> |

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="4a1c2-162">Dépôt de clé personnalisé</span><span class="sxs-lookup"><span data-stu-id="4a1c2-162">Custom key repository</span></span>

<span data-ttu-id="4a1c2-163">Si les mécanismes d’origine ne sont pas appropriées, le développeur peut spécifier leur propre mécanisme de persistance des clés en fournissant un personnalisé [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="4a1c2-163">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
