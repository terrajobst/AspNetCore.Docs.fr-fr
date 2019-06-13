---
title: Fournisseurs de stockage de clés dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur les fournisseurs de stockage de clés dans ASP.NET Core et comment configurer les emplacements de stockage de clés.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 64c7e6b25d5b4acc72e96747a77826efaeb693fd
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034771"
---
# <a name="key-storage-providers-in-aspnet-core"></a>Fournisseurs de stockage de clés dans ASP.NET Core

Le système de protection des données [utilise un mécanisme de découverte par défaut](xref:security/data-protection/configuration/default-settings) pour déterminer où les clés de chiffrement doivent être rendue persistante. Le développeur peut remplacer le mécanisme de découverte par défaut et spécifier manuellement l’emplacement.

> [!WARNING]
> Si vous spécifiez un emplacement de persistance des clés explicites, le système de protection des données annule l’inscription du chiffrement à clé par défaut au mécanisme de rest, donc les clés ne sont plus chiffrés au repos. Il est recommandé que vous en outre [spécifier un mécanisme de chiffrement à clé explicite](xref:security/data-protection/implementation/key-encryption-at-rest) pour les déploiements de production.

## <a name="file-system"></a>Système de fichiers

Pour configurer un référentiel de clé basée sur le système de fichiers, appelez le [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) routine de configuration comme indiqué ci-dessous. Fournir un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointant vers le référentiel où les clés doivent être stockées :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

Le `DirectoryInfo` peut pointer vers un répertoire sur l’ordinateur local, ou il peut pointer vers un dossier sur un partage réseau. Si vous pointez vers un répertoire sur l’ordinateur local (et le scénario est que seules les applications sur l’ordinateur local requièrent l’accès à utiliser ce référentiel), envisagez d’utiliser [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) pour chiffrer les clés au repos. Sinon, envisagez d’utiliser un [certificat X.509](xref:security/data-protection/implementation/key-encryption-at-rest) pour chiffrer les clés au repos.

## <a name="azure-storage"></a>Azure Storage

Le [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) package permet de stocker des clés de protection des données dans le stockage Blob Azure. Les clés peuvent être partagées entre plusieurs instances d’une application web. Applications peuvent partager des cookies d’authentification ou de protection de CSRF sur plusieurs serveurs.

Pour configurer le fournisseur de stockage Blob Azure, appelez une de la [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) surcharges. 

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Si l’application web s’exécute comme un service Azure, les jetons d’authentification peuvent être créés automatiquement à l’aide de [ Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/). 

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

Consultez [plus d’informations sur la configuration de l’authentification de service à service.](/azure/key-vault/service-to-service-authentication)

## <a name="redis"></a>Redis

::: moniker range=">= aspnetcore-2.2"

Le [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) package permet de stocker des clés de protection des données dans un cache Redis. Les clés peuvent être partagées entre plusieurs instances d’une application web. Applications peuvent partager des cookies d’authentification ou de protection de CSRF sur plusieurs serveurs.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Le [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) package permet de stocker des clés de protection des données dans un cache Redis. Les clés peuvent être partagées entre plusieurs instances d’une application web. Applications peuvent partager des cookies d’authentification ou de protection de CSRF sur plusieurs serveurs.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Pour configurer sur Redis, appelez une de la [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) surcharges :

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

Pour configurer sur Redis, appelez une de la [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) surcharges :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

Pour plus d’informations, consultez les rubriques suivantes :

* [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Cache Redis Azure](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [exemples d’ASPNET/DataProtection](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a>Registre

**S’applique uniquement aux déploiements de Windows.**

Parfois l’application ne peut pas avoir accès en écriture au système de fichiers. Imaginez un scénario dans lequel une application s’exécute comme compte de service virtuel (tel que *w3wp.exe*d’identité du pool d’application). Dans ce cas, l’administrateur peut configurer une clé de Registre qui est accessible par l’identité de compte de service. Appelez le [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) méthode d’extension comme indiqué ci-dessous. Fournir un [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointant vers l’emplacement de stockage des clés de chiffrement :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> Nous vous recommandons d’utiliser [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) pour chiffrer les clés au repos.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

Le [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package fournit un mécanisme pour stocker les clés de protection des données à une base de données à l’aide d’Entity Framework Core. Le `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` package NuGet doit être ajouté au fichier projet, il n’est pas dans le cadre de la [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).

Avec ce package, les clés peuvent être partagées entre plusieurs instances d’une application web.

Pour configurer le fournisseur EF Core, appelez le [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) méthode :

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

Le paramètre générique, `TContext`, doit hériter de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) et [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

Créer le `DataProtectionKeys` table. 

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Exécutez les commandes suivantes dans le **Console du Gestionnaire de Package** fenêtre de (PMC) :

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Dans une invite de commandes, exécutez les commandes suivantes :

```console
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

`MyKeysContext` est le `DbContext` défini dans l’exemple de code précédent. Si vous utilisez un `DbContext` avec un nom différent, remplacez votre `DbContext` nom `MyKeysContext`.

Le `DataProtectionKeys` classe/entité adopte la structure illustrée dans le tableau suivant.

| Propriété ou un champ | Type CLR | Type SQL              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | `int`, PK, non null   |
| `FriendlyName` | `string` | `nvarchar(MAX)`, null |
| `Xml`          | `string` | `nvarchar(MAX)`, null |

::: moniker-end

## <a name="custom-key-repository"></a>Dépôt de clé personnalisé

Si les mécanismes d’origine ne sont pas appropriées, le développeur peut spécifier leur propre mécanisme de persistance des clés en fournissant un personnalisé [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
