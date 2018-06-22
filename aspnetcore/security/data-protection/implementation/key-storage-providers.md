---
title: Fournisseurs de stockage de clés dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur les fournisseurs de stockage de clés dans ASP.NET Core et comment configurer les emplacements de stockage de clés.
ms.author: riande
ms.date: 01/14/2017
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 432c2690f216325470bbea9b974ea772bcdc39ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273767"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="79a5b-103">Fournisseurs de stockage de clés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79a5b-103">Key storage providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="79a5b-104">Par défaut, le système de protection des données [utilise une heuristique](xref:security/data-protection/configuration/default-settings) pour déterminer où le matériel de clé de chiffrement doit être persistante.</span><span class="sxs-lookup"><span data-stu-id="79a5b-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="79a5b-105">Le développeur peut substituer l’heuristique et spécifier manuellement l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="79a5b-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="79a5b-106">Si vous spécifiez un emplacement de persistance clé explicite, le système de protection des données sera annuler l’inscription au chiffrement à clé par défaut au mécanisme de rest l’heuristique fournie, afin de clés n’est plus seront chiffrées au repos.</span><span class="sxs-lookup"><span data-stu-id="79a5b-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="79a5b-107">Il est recommandé que vous en outre [spécifient un mécanisme de chiffrement à clé explicite](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) pour les applications de production.</span><span class="sxs-lookup"><span data-stu-id="79a5b-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="79a5b-108">Le système de protection des données est fourni avec plusieurs fournisseurs de stockage de clés de boîte aux lettres.</span><span class="sxs-lookup"><span data-stu-id="79a5b-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="79a5b-109">Système de fichiers</span><span class="sxs-lookup"><span data-stu-id="79a5b-109">File system</span></span>

<span data-ttu-id="79a5b-110">Nous estimons que de nombreuses applications utilisent un dépôt de clé basée sur le système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="79a5b-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="79a5b-111">Pour configurer cela, appelez le [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) routine de configuration comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="79a5b-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="79a5b-112">Fournir un `DirectoryInfo` pointant vers le référentiel dans lequel les clés doivent être stockées.</span><span class="sxs-lookup"><span data-stu-id="79a5b-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="79a5b-113">Le `DirectoryInfo` peut pointer vers un répertoire sur l’ordinateur local, ou il peut pointer vers un dossier sur un partage réseau.</span><span class="sxs-lookup"><span data-stu-id="79a5b-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="79a5b-114">Si vous pointez vers un répertoire sur l’ordinateur local (et le scénario est que seules les applications sur l’ordinateur local devrez utiliser ce référentiel), envisagez d’utiliser [DPAPI Windows](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="79a5b-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="79a5b-115">Sinon, envisagez d’utiliser un [certificat X.509](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="79a5b-115">Otherwise consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="79a5b-116">Azure et Redis</span><span class="sxs-lookup"><span data-stu-id="79a5b-116">Azure and Redis</span></span>

<span data-ttu-id="79a5b-117">Le `Microsoft.AspNetCore.DataProtection.AzureStorage` et `Microsoft.AspNetCore.DataProtection.Redis` packages permettent le stockage de vos clés de protection des données dans le stockage Azure ou un cache Redis.</span><span class="sxs-lookup"><span data-stu-id="79a5b-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="79a5b-118">Les clés peuvent être partagées entre plusieurs instances d’une application web.</span><span class="sxs-lookup"><span data-stu-id="79a5b-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="79a5b-119">Votre application ASP.NET Core peut partager des cookies d’authentification ou de la protection de CSRF sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="79a5b-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="79a5b-120">Pour configurer sur Azure, appelez l’une de le [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) les surcharges comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="79a5b-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="79a5b-121">Voir aussi la [code de test Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="79a5b-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="79a5b-122">Pour configurer sur Redis, appelez l’une de le [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) les surcharges comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="79a5b-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="79a5b-123">Pour plus d’informations, consultez :</span><span class="sxs-lookup"><span data-stu-id="79a5b-123">See the following for more information:</span></span>

- [<span data-ttu-id="79a5b-124">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="79a5b-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="79a5b-125">Cache Redis Azure</span><span class="sxs-lookup"><span data-stu-id="79a5b-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="79a5b-126">[Code de test de redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="79a5b-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="79a5b-127">Registre</span><span class="sxs-lookup"><span data-stu-id="79a5b-127">Registry</span></span>

<span data-ttu-id="79a5b-128">Parfois, l’application peut-être pas accès en écriture au système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="79a5b-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="79a5b-129">Considérez un scénario dans lequel une application s’exécute sous un compte de service virtuel (telles que l’identité du pool d’application de w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="79a5b-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="79a5b-130">Dans ce cas, l’administrateur peut vous avoir fourni une clé de Registre est tiennent approprié pour l’identité de compte de service.</span><span class="sxs-lookup"><span data-stu-id="79a5b-130">In these cases, the administrator may have provisioned a registry key that's appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="79a5b-131">Appelez le [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) routine de configuration comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="79a5b-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="79a5b-132">Fournir un `RegistryKey` pointant vers l’emplacement de stockage des services de chiffrement de clés/valeurs.</span><span class="sxs-lookup"><span data-stu-id="79a5b-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="79a5b-133">Si vous utilisez le Registre du système comme un mécanisme de persistance, envisagez d’utiliser [DPAPI Windows](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) pour chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="79a5b-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="79a5b-134">Référentiel de clés personnalisé</span><span class="sxs-lookup"><span data-stu-id="79a5b-134">Custom key repository</span></span>

<span data-ttu-id="79a5b-135">Si les mécanismes de boîte aux lettres ne sont pas appropriées, le développeur peut spécifier leur propre mécanisme de persistance de clé en fournissant une personnalisée `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="79a5b-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
