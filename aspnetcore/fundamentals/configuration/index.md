---
title: Configuration dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: b4fa082c5a53bc9ecb3c7b8ddcbf243ef0d94ba7
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989694"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="01c5a-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01c5a-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="01c5a-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="01c5a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="01c5a-105">La configuration dans ASP.NET Core est effectuée à l’aide d’un ou de plusieurs [fournisseurs de configuration](#cp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-105">Configuration in ASP.NET Core is performed using one or more [configuration providers](#cp).</span></span> <span data-ttu-id="01c5a-106">Les fournisseurs de configuration lisent les données de configuration des paires clé-valeur à l’aide d’une variété de sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-106">Configuration providers read configuration data from key-value pairs using a variety of configuration sources:</span></span>

* <span data-ttu-id="01c5a-107">Fichiers de paramètres, tels que *appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="01c5a-107">Settings files, such as *appsettings.json*</span></span>
* <span data-ttu-id="01c5a-108">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-108">Environment variables</span></span>
* <span data-ttu-id="01c5a-109">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="01c5a-109">Azure Key Vault</span></span>
* <span data-ttu-id="01c5a-110">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-110">Azure App Configuration</span></span>
* <span data-ttu-id="01c5a-111">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-111">Command-line arguments</span></span>
* <span data-ttu-id="01c5a-112">Fournisseurs personnalisés, installés ou créés</span><span class="sxs-lookup"><span data-stu-id="01c5a-112">Custom providers, installed or created</span></span>
* <span data-ttu-id="01c5a-113">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-113">Directory files</span></span>
* <span data-ttu-id="01c5a-114">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-114">In-memory .NET objects</span></span>

<span data-ttu-id="01c5a-115">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="01c5a-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="default"></a>

## <a name="default-configuration"></a><span data-ttu-id="01c5a-116">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="01c5a-116">Default configuration</span></span>

<span data-ttu-id="01c5a-117">ASP.NET Core les applications Web créées avec [dotnet New](/dotnet/core/tools/dotnet-new) ou Visual Studio génèrent le code suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-117">ASP.NET Core web apps created with [dotnet new](/dotnet/core/tools/dotnet-new) or Visual Studio generate the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <span data-ttu-id="01c5a-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

1. <span data-ttu-id="01c5a-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Ajoute un `IConfiguration` existant en tant que source.</span><span class="sxs-lookup"><span data-stu-id="01c5a-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Adds an existing `IConfiguration` as a source.</span></span> <span data-ttu-id="01c5a-120">Dans le cas de configuration par défaut, ajoute la configuration d' [hôte](#hvac) et la définit en tant que première source de la configuration de l' _application_ .</span><span class="sxs-lookup"><span data-stu-id="01c5a-120">In the default configuration case, adds the [host](#hvac) configuration and setting it as the first source for the _app_ configuration.</span></span>
1. <span data-ttu-id="01c5a-121">[appSettings. JSON](#appsettingsjson) à l’aide du [fournisseur de configuration JSON](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-121">[appsettings.json](#appsettingsjson) using the [JSON configuration provider](#file-configuration-provider).</span></span>
1. <span data-ttu-id="01c5a-122">*appSettings.* `Environment` *. JSON* à l’aide du [fournisseur de configuration JSON](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-122">*appsettings.*`Environment`*.json* using the [JSON configuration provider](#file-configuration-provider).</span></span> <span data-ttu-id="01c5a-123">Par exemple, *appSettings*. ***Production***. *JSON* et *appSettings*. ***Développement***. *JSON*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-123">For example, *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json*.</span></span>
1. <span data-ttu-id="01c5a-124">[Secrets d’application](xref:security/app-secrets) lorsque l’application s’exécute dans l’environnement `Development`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-124">[App secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
1. <span data-ttu-id="01c5a-125">Variables d’environnement à l’aide du [fournisseur de configuration des variables d’environnement](#evcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-125">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="01c5a-126">Arguments de ligne de commande à l’aide du [fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-126">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="01c5a-127">Les fournisseurs de configuration ajoutés ultérieurement remplacent les paramètres de clé précédents.</span><span class="sxs-lookup"><span data-stu-id="01c5a-127">Configuration providers that are added later override previous key settings.</span></span> <span data-ttu-id="01c5a-128">Par exemple, si `MyKey` est défini à la fois dans *appSettings. JSON* et dans l’environnement, la valeur d’environnement est utilisée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-128">For example, if `MyKey` is set in both *appsettings.json* and the environment, the environment value is used.</span></span> <span data-ttu-id="01c5a-129">À l’aide des fournisseurs de configuration par défaut, le [fournisseur de configuration de ligne de commande](#command-line-configuration-provider) remplace tous les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="01c5a-129">Using the default configuration providers, the  [Command-line configuration provider](#command-line-configuration-provider) overrides all other providers.</span></span>

<span data-ttu-id="01c5a-130">Pour plus d’informations sur les `CreateDefaultBuilder`, consultez [paramètres du générateur par défaut](xref:fundamentals/host/generic-host#default-builder-settings).</span><span class="sxs-lookup"><span data-stu-id="01c5a-130">For more information on `CreateDefaultBuilder`, see [Default builder settings](xref:fundamentals/host/generic-host#default-builder-settings).</span></span>

<span data-ttu-id="01c5a-131">Le code suivant affiche les fournisseurs de configuration activés dans l’ordre dans lequel ils ont été ajoutés :</span><span class="sxs-lookup"><span data-stu-id="01c5a-131">The following code displays the enabled configuration providers in the order they were added:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a><span data-ttu-id="01c5a-132">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="01c5a-132">appsettings.json</span></span>

<span data-ttu-id="01c5a-133">Prenons le fichier *appSettings. JSON* suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-133">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="01c5a-134">Le code suivant de l' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) affiche plusieurs des paramètres de configuration précédents :</span><span class="sxs-lookup"><span data-stu-id="01c5a-134">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-135">La <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> par défaut charge la configuration dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-135">The default <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration in the following order:</span></span>

1. <span data-ttu-id="01c5a-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="01c5a-136">*appsettings.json*</span></span>
1. <span data-ttu-id="01c5a-137">*appSettings.* `Environment` *. JSON* : Par exemple, *appSettings*. ***Production***. *JSON* et *appSettings*. ***Développement***. fichiers *JSON* .</span><span class="sxs-lookup"><span data-stu-id="01c5a-137">*appsettings.*`Environment`*.json* : For example, the *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json* files.</span></span> <span data-ttu-id="01c5a-138">La version de l’environnement du fichier est chargée à partir de [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="01c5a-138">The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="01c5a-139">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-139">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="01c5a-140">*appSettings*.`Environment`. les valeurs *JSON* remplacent les clés dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-140">*appsettings*.`Environment`.*json* values override keys in *appsettings.json*.</span></span> <span data-ttu-id="01c5a-141">Par exemple, par défaut :</span><span class="sxs-lookup"><span data-stu-id="01c5a-141">For example, by default:</span></span>

* <span data-ttu-id="01c5a-142">Dans le développement, *appSettings*. ***Développement***. la configuration *JSON* remplace les valeurs trouvées dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-142">In development, *appsettings*.***Development***.*json* configuration overwrites values found in *appsettings.json*.</span></span>
* <span data-ttu-id="01c5a-143">En production, *appSettings*. ***Production***. la configuration *JSON* remplace les valeurs trouvées dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-143">In production, *appsettings*.***Production***.*json* configuration overwrites values found in *appsettings.json*.</span></span> <span data-ttu-id="01c5a-144">Par exemple, lors du déploiement de l’application sur Azure.</span><span class="sxs-lookup"><span data-stu-id="01c5a-144">For example, when deploying the app to Azure.</span></span>

<a name="optpat"></a>

#### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a><span data-ttu-id="01c5a-145">Lier des données de configuration hiérarchiques à l’aide du modèle options</span><span class="sxs-lookup"><span data-stu-id="01c5a-145">Bind hierarchical configuration data using the options pattern</span></span>

<span data-ttu-id="01c5a-146">La méthode recommandée pour lire les valeurs de configuration associées utilise le [modèle d’options](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="01c5a-146">The preferred way to read related configuration values is using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="01c5a-147">Par exemple, pour lire les valeurs de configuration suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-147">For example, to read the following configuration values:</span></span>

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

<span data-ttu-id="01c5a-148">Créez la classe de `PositionOptions` suivante :</span><span class="sxs-lookup"><span data-stu-id="01c5a-148">Create the following `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

<span data-ttu-id="01c5a-149">Toutes les propriétés publiques en lecture-écriture du type sont liées.</span><span class="sxs-lookup"><span data-stu-id="01c5a-149">All the public read-write properties of the type are bound.</span></span> <span data-ttu-id="01c5a-150">Les champs ne sont ***pas*** liés.</span><span class="sxs-lookup"><span data-stu-id="01c5a-150">Fields are ***not*** bound.</span></span>

<span data-ttu-id="01c5a-151">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-151">The following code:</span></span>

* <span data-ttu-id="01c5a-152">Appelle [ConfigurationBinder. bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) pour lier la classe `PositionOptions` à la section `Position`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-152">Calls [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) to bind the `PositionOptions` class to the `Position` section.</span></span>
* <span data-ttu-id="01c5a-153">Affiche les données de configuration de `Position`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-153">Displays the `Position` configuration data.</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="01c5a-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="01c5a-155">`ConfigurationBinder.Get<T>` peut être plus commode que l’utilisation de `ConfigurationBinder.Bind`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-155">`ConfigurationBinder.Get<T>` may be more convenient than using `ConfigurationBinder.Bind`.</span></span> <span data-ttu-id="01c5a-156">Le code suivant montre comment utiliser `ConfigurationBinder.Get<T>` avec la classe `PositionOptions` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-156">The following code shows how to use `ConfigurationBinder.Get<T>` with the `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-157">Une autre approche de l’utilisation du ***modèle options*** consiste à lier la section `Position` et à l’ajouter au [conteneur du service d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="01c5a-157">An alternative approach when using the ***options pattern*** is to bind the `Position` section and add it to the [dependency injection service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="01c5a-158">Dans le code suivant, `PositionOptions` est ajouté au conteneur de services avec <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> et lié à la configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-158">In the following code, `PositionOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

<span data-ttu-id="01c5a-159">À l’aide du code précédent, le code suivant lit les options de position :</span><span class="sxs-lookup"><span data-stu-id="01c5a-159">Using the preceding code, the following code reads the position options:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-160">À l’aide de la configuration [par défaut](#default) , les fichiers *appSettings. JSON* et *appSettings.* `Environment` *. JSON* sont activés avec [reloadOnChange : true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span><span class="sxs-lookup"><span data-stu-id="01c5a-160">Using the [default](#default) configuration, the *appsettings.json* and *appsettings.*`Environment`*.json* files are enabled with [reloadOnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span></span> <span data-ttu-id="01c5a-161">Les modifications apportées au fichier *appSettings. JSON* et *appSettings.* `Environment` *. JSON* ***après*** le démarrage de l’application sont lues par le [fournisseur de configuration JSON](#jcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-161">Changes made to the *appsettings.json* and *appsettings.*`Environment`*.json* file ***after*** the app starts are read by the [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="01c5a-162">Pour plus d’informations sur l’ajout de fichiers de configuration JSON supplémentaires, consultez [fournisseur de configuration JSON](#jcp) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="01c5a-162">See [JSON configuration provider](#jcp) in this document for information on adding additional JSON configuration files.</span></span>

<a name="security"></a>

## <a name="security-and-secret-manager"></a><span data-ttu-id="01c5a-163">Responsable de la sécurité et du secret</span><span class="sxs-lookup"><span data-stu-id="01c5a-163">Security and secret manager</span></span>

<span data-ttu-id="01c5a-164">Instructions relatives aux données de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-164">Configuration data guidelines:</span></span>

* <span data-ttu-id="01c5a-165">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="01c5a-165">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="01c5a-166">Le [Gestionnaire de secret](xref:security/app-secrets) peut être utilisé pour stocker les secrets en développement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-166">The [Secret manager](xref:security/app-secrets) can be used to store secrets in development.</span></span>
* <span data-ttu-id="01c5a-167">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="01c5a-167">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="01c5a-168">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="01c5a-168">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="01c5a-169">Par [défaut](#default), le [Gestionnaire de secret](xref:security/app-secrets) lit les paramètres de configuration après *appSettings. JSON* et *appSettings.* `Environment` *. JSON*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-169">By [default](#default), [Secret manager](xref:security/app-secrets) reads configuration settings after *appsettings.json* and *appsettings.*`Environment`*.json*.</span></span>

<span data-ttu-id="01c5a-170">Pour plus d’informations sur le stockage des mots de passe ou d’autres données sensibles :</span><span class="sxs-lookup"><span data-stu-id="01c5a-170">For more information on storing passwords or other sensitive data:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="01c5a-171"><xref:security/app-secrets>:  Fournit des conseils sur l’utilisation de variables d’environnement pour stocker des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="01c5a-171"><xref:security/app-secrets>:  Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="01c5a-172">Le gestionnaire de secret utilise le [fournisseur de configuration de fichiers](#fcp) pour stocker les secrets de l’utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="01c5a-172">The Secret Manager uses the [File configuration provider](#fcp) to store user secrets in a JSON file on the local system.</span></span>

<span data-ttu-id="01c5a-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stocke en toute sécurité des secrets d’application pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01c5a-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="01c5a-174">Pour plus d'informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-174">For more information, see <xref:security/key-vault-configuration>.</span></span>

<a name="evcp"></a>

## <a name="environment-variables"></a><span data-ttu-id="01c5a-175">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-175">Environment variables</span></span>

<span data-ttu-id="01c5a-176">À l’aide de la configuration [par défaut](#default) , le <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir de paires clé-valeur de variable d’environnement après avoir lu *appSettings. JSON*, *appSettings.* `Environment` *. JSON*et le [Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="01c5a-176">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs after reading *appsettings.json*, *appsettings.*`Environment`*.json*, and [Secret manager](xref:security/app-secrets).</span></span> <span data-ttu-id="01c5a-177">Par conséquent, les valeurs de clés lues à partir de l’environnement remplacent les valeurs lues dans *appSettings. JSON*, *appSettings.* `Environment` *. JSON*et le gestionnaire de secret.</span><span class="sxs-lookup"><span data-stu-id="01c5a-177">Therefore, key values read from the environment override values read from *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="01c5a-178">Les commandes de `set` suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-178">The following `set` commands:</span></span>

* <span data-ttu-id="01c5a-179">Définissez les clés et les valeurs d’environnement de l' [exemple précédent](#appsettingsjson) sur Windows.</span><span class="sxs-lookup"><span data-stu-id="01c5a-179">Set the environment keys and values of the [preceding example](#appsettingsjson) on Windows.</span></span>
* <span data-ttu-id="01c5a-180">Testez les paramètres lors de l’utilisation de l' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span><span class="sxs-lookup"><span data-stu-id="01c5a-180">Test the settings when using the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span></span> <span data-ttu-id="01c5a-181">La commande `dotnet run` doit être exécutée dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="01c5a-181">The `dotnet run` command must be run in the project directory.</span></span>

```dotnetcli
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

<span data-ttu-id="01c5a-182">Paramètres d’environnement précédents :</span><span class="sxs-lookup"><span data-stu-id="01c5a-182">The preceding environment settings:</span></span>

* <span data-ttu-id="01c5a-183">Sont uniquement définies dans les processus lancés à partir de la fenêtre de commande dans laquelle ils ont été définis.</span><span class="sxs-lookup"><span data-stu-id="01c5a-183">Are only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="01c5a-184">Ne seront pas lues par les navigateurs lancés avec Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c5a-184">Won't be read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="01c5a-185">Les commandes [setx](/windows-server/administration/windows-commands/setx) suivantes peuvent être utilisées pour définir les clés et les valeurs d’environnement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="01c5a-185">The following [setx](/windows-server/administration/windows-commands/setx) commands can be used to set the environment keys and values on Windows.</span></span> <span data-ttu-id="01c5a-186">Contrairement à `set`, `setx` paramètres sont conservés.</span><span class="sxs-lookup"><span data-stu-id="01c5a-186">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="01c5a-187">`/M` définit la variable dans l’environnement système.</span><span class="sxs-lookup"><span data-stu-id="01c5a-187">`/M` sets the variable in the system environment.</span></span> <span data-ttu-id="01c5a-188">Si le commutateur `/M` n’est pas utilisé, une variable d’environnement utilisateur est définie.</span><span class="sxs-lookup"><span data-stu-id="01c5a-188">If the `/M` switch isn't used, a user environment variable is set.</span></span>

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

<span data-ttu-id="01c5a-189">Pour vérifier que les commandes précédentes remplacent *appSettings. JSON* et *appSettings.* `Environment` *. JSON*:</span><span class="sxs-lookup"><span data-stu-id="01c5a-189">To test that the preceding commands override *appsettings.json* and *appsettings.*`Environment`*.json*:</span></span>

* <span data-ttu-id="01c5a-190">Avec Visual Studio : Quittez et redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c5a-190">With Visual Studio: Exit and restart Visual Studio.</span></span>
* <span data-ttu-id="01c5a-191">Avec l’interface CLI : Démarrez une nouvelle fenêtre de commande et entrez `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-191">With the CLI: Start a new command window and enter `dotnet run`.</span></span>

<span data-ttu-id="01c5a-192">Appelez <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> avec une chaîne pour spécifier un préfixe pour les variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="01c5a-192">Call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> with a string to specify a prefix for environment variables:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

<span data-ttu-id="01c5a-193">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="01c5a-193">In the preceding code:</span></span>

* <span data-ttu-id="01c5a-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` est ajouté après les [fournisseurs de configuration par défaut](#default).</span><span class="sxs-lookup"><span data-stu-id="01c5a-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="01c5a-195">Pour obtenir un exemple de classement des fournisseurs de configuration, consultez [fournisseur de configuration JSON](#jcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-195">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>
* <span data-ttu-id="01c5a-196">Les variables d’environnement définies avec le préfixe `MyCustomPrefix_` remplacent les [fournisseurs de configuration par défaut](#default).</span><span class="sxs-lookup"><span data-stu-id="01c5a-196">Environment variables set with the `MyCustomPrefix_` prefix override the [default configuration providers](#default).</span></span> <span data-ttu-id="01c5a-197">Cela comprend les variables d’environnement sans le préfixe.</span><span class="sxs-lookup"><span data-stu-id="01c5a-197">This includes environment variables without the prefix.</span></span>

<span data-ttu-id="01c5a-198">Le préfixe est supprimé lorsque les paires clé-valeur de configuration sont lues.</span><span class="sxs-lookup"><span data-stu-id="01c5a-198">The prefix is stripped off when the configuration key-value pairs are read.</span></span>

<span data-ttu-id="01c5a-199">Les commandes suivantes testent le préfixe personnalisé :</span><span class="sxs-lookup"><span data-stu-id="01c5a-199">The following commands test the custom prefix:</span></span>

```dotnetcli
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

<span data-ttu-id="01c5a-200">La [configuration par défaut](#default) charge les variables d’environnement et les arguments de ligne de commande précédés du préfixe `DOTNET_` et `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-200">The [default configuration](#default) loads environment variables and command line arguments prefixed with `DOTNET_` and `ASPNETCORE_`.</span></span> <span data-ttu-id="01c5a-201">Les préfixes `DOTNET_` et `ASPNETCORE_` sont utilisés par les ASP.NET Core pour la configuration de l' [hôte et](xref:fundamentals/host/generic-host#host-configuration)de l’application, mais pas pour la configuration de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="01c5a-201">The `DOTNET_` and `ASPNETCORE_` prefixes are used by ASP.NET Core for [host and app configuration](xref:fundamentals/host/generic-host#host-configuration), but not for user configuration.</span></span> <span data-ttu-id="01c5a-202">Pour plus d’informations sur la configuration de l’hôte et de l’application, consultez [hôte générique .net](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="01c5a-202">For more information on host and app configuration, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

<span data-ttu-id="01c5a-203">Dans [Azure App service](https://azure.microsoft.com/services/app-service/), sélectionnez **nouveau paramètre d’application** dans la page **Paramètres > Configuration** .</span><span class="sxs-lookup"><span data-stu-id="01c5a-203">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="01c5a-204">Azure App Service paramètres de l’application sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="01c5a-204">Azure App Service application settings are:</span></span>

* <span data-ttu-id="01c5a-205">Chiffré au repos et transmis sur un canal chiffré.</span><span class="sxs-lookup"><span data-stu-id="01c5a-205">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="01c5a-206">Exposés en tant que variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-206">Exposed as environment variables.</span></span>

<span data-ttu-id="01c5a-207">Pour plus d’informations, consultez les [applications Azure : Remplacer la configuration de l’application à l’aide du Portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="01c5a-207">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="01c5a-208">Consultez [préfixes de chaîne de connexion](#constr) pour plus d’informations sur les chaînes de connexion de base de données Azure.</span><span class="sxs-lookup"><span data-stu-id="01c5a-208">See [Connection string prefixes](#constr) for information on Azure database connection strings.</span></span>

<a name="clcp"></a>

## <a name="command-line"></a><span data-ttu-id="01c5a-209">Ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-209">Command-line</span></span>

<span data-ttu-id="01c5a-210">À l’aide de la configuration [par défaut](#default) , le <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir de paires clé-valeur d’argument de ligne de commande après les sources de configuration suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-210">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs after the following configuration sources:</span></span>

* <span data-ttu-id="01c5a-211">*appSettings. JSON* et *appSettings*.`Environment`. fichiers *JSON* .</span><span class="sxs-lookup"><span data-stu-id="01c5a-211">*appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="01c5a-212">[Secrets d’application (gestionnaire de secret)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-212">[App secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="01c5a-213">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-213">Environment variables.</span></span>

<span data-ttu-id="01c5a-214">Par [défaut](#default), les valeurs de configuration définies sur la ligne de commande remplacent les valeurs de configuration définies avec tous les autres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-214">By [default](#default), configuration values set on the command-line override configuration values set with all the other configuration providers.</span></span>

### <a name="command-line-arguments"></a><span data-ttu-id="01c5a-215">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-215">Command-line arguments</span></span>

<span data-ttu-id="01c5a-216">La commande suivante définit des clés et des valeurs à l’aide de `=`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-216">The following command sets keys and values using `=`:</span></span>

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

<span data-ttu-id="01c5a-217">La commande suivante définit des clés et des valeurs à l’aide de `/`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-217">The following command sets keys and values using `/`:</span></span>

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

<span data-ttu-id="01c5a-218">La commande suivante définit des clés et des valeurs à l’aide de `--`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-218">The following command sets keys and values using `--`:</span></span>

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

<span data-ttu-id="01c5a-219">Valeur de la clé :</span><span class="sxs-lookup"><span data-stu-id="01c5a-219">The key value:</span></span>

* <span data-ttu-id="01c5a-220">Doit suivre `=`, ou la clé doit avoir un préfixe de `--` ou `/` lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="01c5a-220">Must follow `=`, or the key must have a prefix of `--` or `/` when the value follows a space.</span></span>
* <span data-ttu-id="01c5a-221">N’est pas requis si `=` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="01c5a-221">Isn't required if `=` is used.</span></span> <span data-ttu-id="01c5a-222">Par exemple, `MySetting=`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-222">For example, `MySetting=`.</span></span>

<span data-ttu-id="01c5a-223">Dans la même commande, ne mélangez pas les paires clé-valeur d’argument de ligne de commande qui utilisent `=` avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="01c5a-223">Within the same command, don't mix command-line argument key-value pairs that use `=` with key-value pairs that use a space.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="01c5a-224">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="01c5a-224">Switch mappings</span></span>

<span data-ttu-id="01c5a-225">Les mappages de commutateur autorisent la logique de remplacement de nom de **clé** .</span><span class="sxs-lookup"><span data-stu-id="01c5a-225">Switch mappings allow **key** name replacement logic.</span></span> <span data-ttu-id="01c5a-226">Fournissez un dictionnaire de remplacements de commutateur dans la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-226">Provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="01c5a-227">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="01c5a-227">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="01c5a-228">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire est retournée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-228">If the command-line key is found in the dictionary, the dictionary value is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="01c5a-229">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="01c5a-229">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="01c5a-230">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="01c5a-230">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="01c5a-231">Les commutateurs doivent commencer par `-` ou `--`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-231">Switches must start with `-` or `--`.</span></span>
* <span data-ttu-id="01c5a-232">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="01c5a-232">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="01c5a-233">Pour utiliser un dictionnaire de mappages de commutateur, transmettez-le dans l’appel à `AddCommandLine`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-233">To use a switch mappings dictionary, pass it into the call to `AddCommandLine`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

<span data-ttu-id="01c5a-234">Le code suivant montre les valeurs de clé pour les clés remplacées :</span><span class="sxs-lookup"><span data-stu-id="01c5a-234">The following code shows the key values for the replaced keys:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-235">Exécutez la commande suivante pour tester le remplacement de la clé :</span><span class="sxs-lookup"><span data-stu-id="01c5a-235">Run the following command to test the key replacement:</span></span>

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="01c5a-236">Remarque : Actuellement, `=` ne peut pas être utilisé pour définir des valeurs de remplacement de clé avec un seul tiret `-`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-236">Note: Currently, `=` cannot be used to set key-replacement values with a single dash `-`.</span></span> <span data-ttu-id="01c5a-237">Consultez [ce problème GitHub](https://github.com/dotnet/extensions/issues/3059).</span><span class="sxs-lookup"><span data-stu-id="01c5a-237">See [this GitHub issue](https://github.com/dotnet/extensions/issues/3059).</span></span>

<span data-ttu-id="01c5a-238">La commande suivante fonctionne pour tester le remplacement de la clé :</span><span class="sxs-lookup"><span data-stu-id="01c5a-238">The following command works to test key replacement:</span></span>

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="01c5a-239">Pour les applications qui utilisent des mappages de commutateurs, l’appel à `CreateDefaultBuilder` ne doit pas passer d’arguments.</span><span class="sxs-lookup"><span data-stu-id="01c5a-239">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="01c5a-240">L’appel de `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés, et il n’existe aucun moyen de passer le dictionnaire de mappage de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-240">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch-mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="01c5a-241">La solution ne consiste pas à passer les arguments à `CreateDefaultBuilder` mais à permettre à la méthode `AddCommandLine` de `ConfigurationBuilder` de traiter à la fois les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="01c5a-241">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch-mapping dictionary.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="01c5a-242">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="01c5a-242">Hierarchical configuration data</span></span>

<span data-ttu-id="01c5a-243">L’API de configuration lit les données de configuration hiérarchiques en aplatit les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-243">The Configuration API is reads hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="01c5a-244">L' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contient le fichier *appSettings. JSON* suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-244">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="01c5a-245">Le code suivant de l' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) affiche plusieurs des paramètres de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-245">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-246">La meilleure façon de lire des données de configuration hiérarchiques consiste à utiliser le modèle d’options.</span><span class="sxs-lookup"><span data-stu-id="01c5a-246">The preferred way to read hierarchical configuration data is using the options pattern.</span></span> <span data-ttu-id="01c5a-247">Pour plus d’informations, consultez [lier des données de configuration hiérarchiques](#optpat) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="01c5a-247">For more information, see [Bind hierarchical configuration data](#optpat) in this document.</span></span>

<span data-ttu-id="01c5a-248">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-248"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="01c5a-249">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection).</span><span class="sxs-lookup"><span data-stu-id="01c5a-249">These methods are described later in [GetSection, GetChildren, and Exists](#getsection).</span></span>

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a><span data-ttu-id="01c5a-250">Clés et valeurs de configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-250">Configuration keys and values</span></span>

<span data-ttu-id="01c5a-251">Clés de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-251">Configuration keys:</span></span>

* <span data-ttu-id="01c5a-252">Ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="01c5a-252">Are case-insensitive.</span></span> <span data-ttu-id="01c5a-253">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-253">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="01c5a-254">Si une clé et une valeur sont définies dans plusieurs fournisseurs de configuration, la valeur du dernier fournisseur ajouté est utilisée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-254">If a key and value is set in more than one configuration providers, the value from the last provider added is used.</span></span> <span data-ttu-id="01c5a-255">Pour plus d’informations, consultez [configuration par défaut](#default).</span><span class="sxs-lookup"><span data-stu-id="01c5a-255">For more information, see [Default configuration](#default).</span></span>
* <span data-ttu-id="01c5a-256">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="01c5a-256">Hierarchical keys</span></span>
  * <span data-ttu-id="01c5a-257">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-257">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="01c5a-258">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-258">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="01c5a-259">Un trait de soulignement double, `__`, est pris en charge par toutes les plateformes et est automatiquement converti en deux-points `:`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-259">A double underscore, `__`, is supported by all platforms and is automatically converted into a colon `:`.</span></span>
  * <span data-ttu-id="01c5a-260">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="01c5a-260">In Azure Key Vault, hierarchical keys use `--` as a separator.</span></span> <span data-ttu-id="01c5a-261">Écrivez du code pour remplacer le `--` par un `:` lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-261">Write code to replace the `--` with a `:` when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="01c5a-262"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-262">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="01c5a-263">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#boa).</span><span class="sxs-lookup"><span data-stu-id="01c5a-263">Array binding is described in the [Bind an array to a class](#boa) section.</span></span>

<span data-ttu-id="01c5a-264">Valeurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-264">Configuration values:</span></span>

* <span data-ttu-id="01c5a-265">Sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-265">Are strings.</span></span>
* <span data-ttu-id="01c5a-266">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="01c5a-266">Null values can't be stored in configuration or bound to objects.</span></span>

<a name="cp"></a>

## <a name="configuration-providers"></a><span data-ttu-id="01c5a-267">Fournisseurs de configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-267">Configuration providers</span></span>

<span data-ttu-id="01c5a-268">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01c5a-268">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="01c5a-269">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="01c5a-269">Provider</span></span> | <span data-ttu-id="01c5a-270">Fournit la configuration à partir de</span><span class="sxs-lookup"><span data-stu-id="01c5a-270">Provides configuration from</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="01c5a-271">Fournisseur de configuration Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="01c5a-271">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration) | <span data-ttu-id="01c5a-272">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="01c5a-272">Azure Key Vault</span></span> |
| [<span data-ttu-id="01c5a-273">Fournisseur de configuration Azure App</span><span class="sxs-lookup"><span data-stu-id="01c5a-273">Azure App configuration provider</span></span>](/azure/azure-app-configuration/quickstart-aspnet-core-app) | <span data-ttu-id="01c5a-274">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-274">Azure App Configuration</span></span> |
| [<span data-ttu-id="01c5a-275">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-275">Command-line configuration provider</span></span>](#clcp) | <span data-ttu-id="01c5a-276">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-276">Command-line parameters</span></span> |
| [<span data-ttu-id="01c5a-277">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="01c5a-277">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="01c5a-278">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="01c5a-278">Custom source</span></span> |
| [<span data-ttu-id="01c5a-279">Fournisseur de configuration des variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-279">Environment Variables configuration provider</span></span>](#evcp) | <span data-ttu-id="01c5a-280">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-280">Environment variables</span></span> |
| [<span data-ttu-id="01c5a-281">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="01c5a-281">File configuration provider</span></span>](#file-configuration-provider) | <span data-ttu-id="01c5a-282">Fichiers INI, JSON et XML</span><span class="sxs-lookup"><span data-stu-id="01c5a-282">INI, JSON, and XML files</span></span> |
| [<span data-ttu-id="01c5a-283">Fournisseur de configuration de clé par fichier</span><span class="sxs-lookup"><span data-stu-id="01c5a-283">Key-per-file configuration provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="01c5a-284">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-284">Directory files</span></span> |
| [<span data-ttu-id="01c5a-285">Fournisseur de configuration de la mémoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-285">Memory configuration provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="01c5a-286">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-286">In-memory collections</span></span> |
| [<span data-ttu-id="01c5a-287">Gestionnaire de secret</span><span class="sxs-lookup"><span data-stu-id="01c5a-287">Secret Manager</span></span>](xref:security/app-secrets)  | <span data-ttu-id="01c5a-288">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="01c5a-288">File in the user profile directory</span></span> |

<span data-ttu-id="01c5a-289">Les sources de configuration sont lues dans l’ordre dans lequel leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="01c5a-289">Configuration sources are read in the order that their configuration providers are specified.</span></span> <span data-ttu-id="01c5a-290">Commandez des fournisseurs de configuration dans le code pour répondre aux priorités des sources de configuration sous-jacentes requises par l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-290">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="01c5a-291">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="01c5a-291">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="01c5a-292">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="01c5a-292">*appsettings.json*</span></span>
1. <span data-ttu-id="01c5a-293">*appSettings*.`Environment`. *JSON*</span><span class="sxs-lookup"><span data-stu-id="01c5a-293">*appsettings*.`Environment`.*json*</span></span>
1. [<span data-ttu-id="01c5a-294">Gestionnaire de secret</span><span class="sxs-lookup"><span data-stu-id="01c5a-294">Secret Manager</span></span>](xref:security/app-secrets)
1. <span data-ttu-id="01c5a-295">Variables d’environnement à l’aide du [fournisseur de configuration des variables d’environnement](#evcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-295">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="01c5a-296">Arguments de ligne de commande à l’aide du [fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-296">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="01c5a-297">Une pratique courante consiste à ajouter le dernier fournisseur de configuration de ligne de commande dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="01c5a-297">A common practice is to add the Command-line configuration provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="01c5a-298">La séquence de fournisseurs précédente est utilisée dans la [configuration par défaut](#default).</span><span class="sxs-lookup"><span data-stu-id="01c5a-298">The preceding sequence of providers is used in the [default configuration](#default).</span></span>

<a name="constr"></a>

### <a name="connection-string-prefixes"></a><span data-ttu-id="01c5a-299">Préfixes de chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="01c5a-299">Connection string prefixes</span></span>

<span data-ttu-id="01c5a-300">L’API de configuration a des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="01c5a-300">The Configuration API has special processing rules for four connection string environment variables.</span></span> <span data-ttu-id="01c5a-301">Ces chaînes de connexion sont impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-301">These connection strings are involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="01c5a-302">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application avec la [configuration par défaut](#default) ou lorsqu’aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-302">Environment variables with the prefixes shown in the table are loaded into the app with the [default configuration](#default) or when no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="01c5a-303">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="01c5a-303">Connection string prefix</span></span> | <span data-ttu-id="01c5a-304">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="01c5a-304">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="01c5a-305">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="01c5a-305">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="01c5a-306">MySQL</span><span class="sxs-lookup"><span data-stu-id="01c5a-306">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="01c5a-307">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="01c5a-307">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="01c5a-308">SQL Server</span><span class="sxs-lookup"><span data-stu-id="01c5a-308">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="01c5a-309">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="01c5a-309">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="01c5a-310">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="01c5a-310">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="01c5a-311">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="01c5a-311">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="01c5a-312">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-312">Environment variable key</span></span> | <span data-ttu-id="01c5a-313">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="01c5a-313">Converted configuration key</span></span> | <span data-ttu-id="01c5a-314">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="01c5a-314">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="01c5a-315">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-315">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="01c5a-316">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-316">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="01c5a-317">Valeur : `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="01c5a-317">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="01c5a-318">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-318">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="01c5a-319">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="01c5a-319">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="01c5a-320">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-320">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="01c5a-321">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="01c5a-321">Value: `System.Data.SqlClient`</span></span>  |

<a name="jcp"></a>

### <a name="json-configuration-provider"></a><span data-ttu-id="01c5a-322">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="01c5a-322">JSON configuration provider</span></span>

<span data-ttu-id="01c5a-323">Le <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir de paires clé-valeur de fichier JSON.</span><span class="sxs-lookup"><span data-stu-id="01c5a-323">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs.</span></span>

<span data-ttu-id="01c5a-324">Les surcharges peuvent spécifier :</span><span class="sxs-lookup"><span data-stu-id="01c5a-324">Overloads can specify:</span></span>

* <span data-ttu-id="01c5a-325">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="01c5a-325">Whether the file is optional.</span></span>
* <span data-ttu-id="01c5a-326">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="01c5a-326">Whether the configuration is reloaded if the file changes.</span></span>

<span data-ttu-id="01c5a-327">Examinons le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="01c5a-327">Consider the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

<span data-ttu-id="01c5a-328">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="01c5a-328">The preceding code:</span></span>

* <span data-ttu-id="01c5a-329">Configure le fournisseur de configuration JSON pour charger le fichier *MyConfig. JSON* avec les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-329">Configures the JSON configuration provider to load the *MyConfig.json* file with the following options:</span></span>
  * <span data-ttu-id="01c5a-330">`optional: true`: Le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="01c5a-330">`optional: true`: The file is optional.</span></span>
  * <span data-ttu-id="01c5a-331">`reloadOnChange: true` : Le fichier est rechargé lorsque des modifications sont enregistrées.</span><span class="sxs-lookup"><span data-stu-id="01c5a-331">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>
* <span data-ttu-id="01c5a-332">Lit les [fournisseurs de configuration par défaut](#default) avant le fichier *MyConfig. JSON* .</span><span class="sxs-lookup"><span data-stu-id="01c5a-332">Reads the [default configuration providers](#default) before the *MyConfig.json* file.</span></span> <span data-ttu-id="01c5a-333">Paramètres dans le paramètre de remplacement de fichier *MyConfig. JSON* des fournisseurs de configuration par défaut, y compris le [fournisseur de configuration des variables d’environnement](#evcp) et le fournisseur de configuration de ligne de [commande](#clcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-333">Settings in the *MyConfig.json* file override setting in the default configuration providers, including the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="01c5a-334">En général, vous ***ne souhaitez pas*** qu’une valeur de substitution de fichier JSON personnalisée soit définie dans le fournisseur de configuration des [variables d’environnement](#evcp) et dans le fournisseur de configuration de [ligne de commande](#clcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-334">You typically ***don't*** want a custom JSON file overriding values set in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="01c5a-335">Le code suivant efface tous les fournisseurs de configuration et ajoute plusieurs fournisseurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-335">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

<span data-ttu-id="01c5a-336">Dans le code précédent, les paramètres dans *MyConfig. JSON* et *MyConfig*.`Environment`. fichiers *JSON* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-336">In the preceding code, settings in the *MyConfig.json* and  *MyConfig*.`Environment`.*json* files:</span></span>

* <span data-ttu-id="01c5a-337">Substituez les paramètres dans *appSettings. JSON* et *appSettings*.`Environment`. fichiers *JSON* .</span><span class="sxs-lookup"><span data-stu-id="01c5a-337">Override settings in the *appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="01c5a-338">Sont remplacées par les paramètres dans le [fournisseur de configuration des variables d’environnement](#evcp) et le fournisseur de configuration de ligne de [commande](#clcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-338">Are overridden by settings in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="01c5a-339">L' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contient le fichier *MyConfig. JSON* suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-339">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *MyConfig.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

<span data-ttu-id="01c5a-340">Le code suivant de l' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) affiche plusieurs des paramètres de configuration précédents :</span><span class="sxs-lookup"><span data-stu-id="01c5a-340">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a><span data-ttu-id="01c5a-341">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="01c5a-341">File configuration provider</span></span>

<span data-ttu-id="01c5a-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="01c5a-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="01c5a-343">Les fournisseurs de configuration suivants dérivent de `FileConfigurationProvider`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-343">The following configuration providers derive from `FileConfigurationProvider`:</span></span>

* [<span data-ttu-id="01c5a-344">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="01c5a-344">INI configuration provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="01c5a-345">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="01c5a-345">JSON configuration provider</span></span>](#jcp)
* [<span data-ttu-id="01c5a-346">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="01c5a-346">XML configuration provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="01c5a-347">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="01c5a-347">INI configuration provider</span></span>

<span data-ttu-id="01c5a-348"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="01c5a-348">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="01c5a-349">Le code suivant efface tous les fournisseurs de configuration et ajoute plusieurs fournisseurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-349">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

<span data-ttu-id="01c5a-350">Dans le code précédent, les paramètres dans *MyIniConfig. ini* et *MyIniConfig*.`Environment`. les fichiers *ini* sont remplacés par les paramètres dans le :</span><span class="sxs-lookup"><span data-stu-id="01c5a-350">In the preceding code, settings in the *MyIniConfig.ini* and  *MyIniConfig*.`Environment`.*ini* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="01c5a-351">Fournisseur de configuration des variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-351">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="01c5a-352">[Fournisseur de configuration de ligne de commande](#clcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-352">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="01c5a-353">L' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contient le fichier *MyIniConfig. ini* suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-353">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyIniConfig.ini* file:</span></span>

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

<span data-ttu-id="01c5a-354">Le code suivant de l' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) affiche plusieurs des paramètres de configuration précédents :</span><span class="sxs-lookup"><span data-stu-id="01c5a-354">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a><span data-ttu-id="01c5a-355">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="01c5a-355">XML configuration provider</span></span>

<span data-ttu-id="01c5a-356"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="01c5a-356">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="01c5a-357">Le code suivant efface tous les fournisseurs de configuration et ajoute plusieurs fournisseurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-357">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

<span data-ttu-id="01c5a-358">Dans le code précédent, les paramètres dans *MyXMLFile. xml* et *MyXMLFile*.`Environment`. les fichiers *XML* sont remplacés par les paramètres dans le :</span><span class="sxs-lookup"><span data-stu-id="01c5a-358">In the preceding code, settings in the *MyXMLFile.xml* and  *MyXMLFile*.`Environment`.*xml* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="01c5a-359">Fournisseur de configuration des variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-359">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="01c5a-360">[Fournisseur de configuration de ligne de commande](#clcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-360">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="01c5a-361">L' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contient le fichier *MyXMLFile. xml* suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-361">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyXMLFile.xml* file:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

<span data-ttu-id="01c5a-362">Le code suivant de l' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) affiche plusieurs des paramètres de configuration précédents :</span><span class="sxs-lookup"><span data-stu-id="01c5a-362">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-363">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="01c5a-363">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

<span data-ttu-id="01c5a-364">Le code suivant lit le fichier de configuration précédent et affiche les clés et les valeurs :</span><span class="sxs-lookup"><span data-stu-id="01c5a-364">The following code reads the previous configuration file and displays the keys and values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-365">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="01c5a-365">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="01c5a-366">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-366">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="01c5a-367">key:attribute</span><span class="sxs-lookup"><span data-stu-id="01c5a-367">key:attribute</span></span>
* <span data-ttu-id="01c5a-368">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="01c5a-368">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="01c5a-369">Fournisseur de configuration de clé par fichier</span><span class="sxs-lookup"><span data-stu-id="01c5a-369">Key-per-file configuration provider</span></span>

<span data-ttu-id="01c5a-370">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-370">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="01c5a-371">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-371">The key is the file name.</span></span> <span data-ttu-id="01c5a-372">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-372">The value contains the file's contents.</span></span> <span data-ttu-id="01c5a-373">Le fournisseur de configuration par fichier clé est utilisé dans les scénarios d’hébergement de l’ancrage.</span><span class="sxs-lookup"><span data-stu-id="01c5a-373">The Key-per-file configuration provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="01c5a-374">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-374">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="01c5a-375">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="01c5a-375">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="01c5a-376">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="01c5a-376">Overloads permit specifying:</span></span>

* <span data-ttu-id="01c5a-377">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="01c5a-377">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="01c5a-378">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="01c5a-378">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="01c5a-379">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="01c5a-379">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="01c5a-380">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-380">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="01c5a-381">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="01c5a-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a><span data-ttu-id="01c5a-382">Fournisseur de configuration de la mémoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-382">Memory configuration provider</span></span>

<span data-ttu-id="01c5a-383">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-383">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="01c5a-384">Le code suivant ajoute une collection de mémoire au système de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-384">The following code adds a memory collection to the configuration system:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

<span data-ttu-id="01c5a-385">Le code suivant de l' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) affiche les paramètres de configuration précédents :</span><span class="sxs-lookup"><span data-stu-id="01c5a-385">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-386">Dans le code précédent, `config.AddInMemoryCollection(Dict)` est ajouté après les [fournisseurs de configuration par défaut](#default).</span><span class="sxs-lookup"><span data-stu-id="01c5a-386">In the preceding code, `config.AddInMemoryCollection(Dict)` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="01c5a-387">Pour obtenir un exemple de classement des fournisseurs de configuration, consultez [fournisseur de configuration JSON](#jcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-387">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="01c5a-388">Pour obtenir un exemple de classement des fournisseurs de configuration, consultez [fournisseur de configuration JSON](#jcp).</span><span class="sxs-lookup"><span data-stu-id="01c5a-388">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="01c5a-389">Pour un autre exemple, consultez [lier un tableau](#boa) à l’aide de `MemoryConfigurationProvider`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-389">See [Bind an array](#boa) for another example using `MemoryConfigurationProvider`.</span></span>

## <a name="getvalue"></a><span data-ttu-id="01c5a-390">GetValue</span><span class="sxs-lookup"><span data-stu-id="01c5a-390">GetValue</span></span>

<span data-ttu-id="01c5a-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur unique de la configuration avec une clé spécifiée et la convertit en type spécifié :</span><span class="sxs-lookup"><span data-stu-id="01c5a-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified type:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-392">Dans le code précédent, si `NumberKey` est introuvable dans la configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-392">In the preceding code,  if `NumberKey` isn't found in the configuration, the default value of `99` is used.</span></span>

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="01c5a-393">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="01c5a-393">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="01c5a-394">Pour les exemples qui suivent, examinez le fichier *MySubsection. JSON* suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-394">For the examples that follow, consider the following *MySubsection.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

<span data-ttu-id="01c5a-395">Le code suivant ajoute *MySubsection. JSON* aux fournisseurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-395">The following code adds *MySubsection.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a><span data-ttu-id="01c5a-396">GetSection</span><span class="sxs-lookup"><span data-stu-id="01c5a-396">GetSection</span></span>

<span data-ttu-id="01c5a-397">[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) retourne une sous-section de configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-397">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) returns a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="01c5a-398">Le code suivant retourne des valeurs pour `section1`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-398">The following code returns values for `section1`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-399">Le code suivant retourne des valeurs pour `section2:subsection0`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-399">The following code returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-400">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-400">`GetSection` never returns `null`.</span></span> <span data-ttu-id="01c5a-401">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-401">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="01c5a-402">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="01c5a-402">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="01c5a-403"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="01c5a-403">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren-and-exists"></a><span data-ttu-id="01c5a-404">GetChildren et EXISTS</span><span class="sxs-lookup"><span data-stu-id="01c5a-404">GetChildren and Exists</span></span>

<span data-ttu-id="01c5a-405">Le code suivant appelle [IConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) et retourne des valeurs pour `section2:subsection0`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-405">The following code calls [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) and returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-406">Le code précédent appelle [ConfigurationExtensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour vérifier l’existence de la section :</span><span class="sxs-lookup"><span data-stu-id="01c5a-406">The preceding code calls [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to verify the  section exists:</span></span>

 <a name="boa"></a>

## <a name="bind-an-array"></a><span data-ttu-id="01c5a-407">Lier un tableau</span><span class="sxs-lookup"><span data-stu-id="01c5a-407">Bind an array</span></span>

<span data-ttu-id="01c5a-408">[ConfigurationBinder. bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) prend en charge les tableaux de liaison aux objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-408">The [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="01c5a-409">Tout format de tableau qui expose un segment de clé numérique est capable d’effectuer une liaison de tableau à un tableau de classes [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) .</span><span class="sxs-lookup"><span data-stu-id="01c5a-409">Any array format that exposes a numeric key segment is capable of array binding to a [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) class array.</span></span>

<span data-ttu-id="01c5a-410">Examinez *myArray. JSON* de l' [exemple de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span><span class="sxs-lookup"><span data-stu-id="01c5a-410">Consider *MyArray.json* from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

<span data-ttu-id="01c5a-411">Le code suivant ajoute *myArray. JSON* aux fournisseurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-411">The following code adds *MyArray.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

<span data-ttu-id="01c5a-412">Le code suivant lit la configuration et affiche les valeurs :</span><span class="sxs-lookup"><span data-stu-id="01c5a-412">The following code reads the configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-413">Le code précédent retourne la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="01c5a-413">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

<span data-ttu-id="01c5a-414">Dans la sortie précédente, l’index 3 a la valeur `value40`, correspondant à `"4": "value40",` dans *myArray. JSON*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-414">In the preceding output, Index 3 has value `value40`, corresponding to `"4": "value40",` in *MyArray.json*.</span></span> <span data-ttu-id="01c5a-415">Les index de tableau liés sont continus et non liés à l’index de clé de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-415">The bound array indices are continuous and not bound to the configuration key index.</span></span> <span data-ttu-id="01c5a-416">Le Binder de configuration n’est pas en capacité à lier des valeurs null ou à créer des entrées NULL dans des objets liés</span><span class="sxs-lookup"><span data-stu-id="01c5a-416">The configuration binder isn't capable of binding null values or creating null entries in bound objects</span></span>

<span data-ttu-id="01c5a-417">Le code suivant charge la configuration `array:entries` avec la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> :</span><span class="sxs-lookup"><span data-stu-id="01c5a-417">The  following code loads the `array:entries` configuration with the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

<span data-ttu-id="01c5a-418">Le code suivant lit la configuration dans le `Dictionary` `arrayDict` et affiche les valeurs :</span><span class="sxs-lookup"><span data-stu-id="01c5a-418">The following code reads the configuration in the `arrayDict` `Dictionary` and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-419">Le code précédent retourne la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="01c5a-419">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

<span data-ttu-id="01c5a-420">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-420">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="01c5a-421">Lorsque les données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="01c5a-421">When configuration data containing an array is bound, the array indices in the configuration keys are used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="01c5a-422">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="01c5a-422">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="01c5a-423">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui lit la paire clé/valeur de l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="01c5a-423">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that reads the index &num;3 key/value pair.</span></span> <span data-ttu-id="01c5a-424">Considérez le fichier *valeur3. JSON* suivant dans l’exemple de téléchargement :</span><span class="sxs-lookup"><span data-stu-id="01c5a-424">Consider the following *Value3.json* file from the sample download:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

<span data-ttu-id="01c5a-425">Le code suivant comprend la configuration de *valeur3. JSON* et le `arrayDict` `Dictionary`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-425">The following code includes configuration for *Value3.json* and the `arrayDict` `Dictionary`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

<span data-ttu-id="01c5a-426">Le code suivant lit la configuration précédente et affiche les valeurs :</span><span class="sxs-lookup"><span data-stu-id="01c5a-426">The following code reads the preceding configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="01c5a-427">Le code précédent retourne la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="01c5a-427">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

<span data-ttu-id="01c5a-428">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="01c5a-428">Custom configuration providers aren't required to implement array binding.</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="01c5a-429">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="01c5a-429">Custom configuration provider</span></span>

<span data-ttu-id="01c5a-430">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="01c5a-430">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="01c5a-431">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-431">The provider has the following characteristics:</span></span>

* <span data-ttu-id="01c5a-432">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-432">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="01c5a-433">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-433">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="01c5a-434">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="01c5a-434">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="01c5a-435">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="01c5a-435">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="01c5a-436">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-436">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="01c5a-437">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="01c5a-437">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="01c5a-438">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-438">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="01c5a-439">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="01c5a-439">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="01c5a-440">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-440">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="01c5a-441">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-441">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="01c5a-442">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-442">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="01c5a-443">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-443">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="01c5a-444">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="01c5a-444">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="01c5a-445">Étant donné que les [clés de configuration ne](#keys)respectent pas la casse, le dictionnaire utilisé pour initialiser la base de données est créé avec le comparateur ne respectant pas la casse ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="01c5a-445">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="01c5a-446">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-446">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="01c5a-447">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-447">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="01c5a-448">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-448">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="01c5a-449">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-449">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a><span data-ttu-id="01c5a-450">Configuration de l’accès au démarrage</span><span class="sxs-lookup"><span data-stu-id="01c5a-450">Access configuration in Startup</span></span>

<span data-ttu-id="01c5a-451">Le code suivant affiche les données de configuration dans `Startup` méthodes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-451">The following code displays configuration data in `Startup` methods:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

<span data-ttu-id="01c5a-452">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="01c5a-452">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-razor-pages"></a><span data-ttu-id="01c5a-453">Configuration de l’accès dans Razor Pages</span><span class="sxs-lookup"><span data-stu-id="01c5a-453">Access configuration in Razor Pages</span></span>

<span data-ttu-id="01c5a-454">Le code suivant affiche les données de configuration dans une page Razor :</span><span class="sxs-lookup"><span data-stu-id="01c5a-454">The following code displays configuration data in a Razor Page:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a><span data-ttu-id="01c5a-455">Configuration de l’accès dans un fichier de vue MVC</span><span class="sxs-lookup"><span data-stu-id="01c5a-455">Access configuration in a MVC view file</span></span>

<span data-ttu-id="01c5a-456">Le code suivant affiche les données de configuration dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="01c5a-456">The following code displays configuration data in a MVC view:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="01c5a-457">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="01c5a-457">Host versus app configuration</span></span>

<span data-ttu-id="01c5a-458">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="01c5a-458">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="01c5a-459">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="01c5a-459">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="01c5a-460">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="01c5a-460">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="01c5a-461">Les paires clé-valeur de la configuration de l’hôte sont également incluses dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-461">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="01c5a-462">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-462">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

<a name="dhc"></a>

## <a name="default-host-configuration"></a><span data-ttu-id="01c5a-463">Configuration de l’hôte par défaut</span><span class="sxs-lookup"><span data-stu-id="01c5a-463">Default host configuration</span></span>

<span data-ttu-id="01c5a-464">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte Web](xref:fundamentals/host/web-host), consultez la [version ASP.NET Core 2.2. de cette rubrique](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="01c5a-464">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="01c5a-465">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="01c5a-465">Host configuration is provided from:</span></span>
  * <span data-ttu-id="01c5a-466">Les variables d’environnement précédées de `DOTNET_` (par exemple, `DOTNET_ENVIRONMENT`) à l’aide du [fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-466">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables configuration provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="01c5a-467">Le préfixe (`DOTNET_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="01c5a-467">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="01c5a-468">Arguments de ligne de commande à l’aide du [fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-468">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="01c5a-469">La configuration par défaut de l’hôte Web est établie (`ConfigureWebHostDefaults`) :</span><span class="sxs-lookup"><span data-stu-id="01c5a-469">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="01c5a-470">Kestrel est utilisé comme serveur web et configuré à l’aide des fournisseurs de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-470">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="01c5a-471">Ajoutez l’intergiciel de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-471">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="01c5a-472">Ajoutez l’intergiciel d’en-têtes transférés si la variable d'environnement `ASPNETCORE_FORWARDEDHEADERS_ENABLED` est définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-472">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="01c5a-473">Activez l’intégration d’IIS.</span><span class="sxs-lookup"><span data-stu-id="01c5a-473">Enable IIS integration.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="01c5a-474">Autre configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-474">Other configuration</span></span>

<span data-ttu-id="01c5a-475">Cette rubrique se rapporte uniquement à la configuration de l' *application*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-475">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="01c5a-476">D’autres aspects de l’exécution et de l’hébergement des applications ASP.NET Core sont configurés à l’aide des fichiers de configuration non traités dans cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="01c5a-476">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="01c5a-477">*Launch. json*/*launchSettings. JSON* sont des fichiers de configuration d’outils pour l’environnement de développement, décrits ci-après :</span><span class="sxs-lookup"><span data-stu-id="01c5a-477">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="01c5a-478">Dans <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-478">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="01c5a-479">Dans l’ensemble de la documentation dans lequel les fichiers sont utilisés pour configurer des applications ASP.NET Core pour les scénarios de développement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-479">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="01c5a-480">*Web. config* est un fichier de configuration de serveur, décrit dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-480">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="01c5a-481">Pour plus d’informations sur la migration de la configuration d’application à partir de versions antérieures de ASP.NET, consultez <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-481">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="01c5a-482">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="01c5a-482">Add configuration from an external assembly</span></span>

<span data-ttu-id="01c5a-483">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-483">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="01c5a-484">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-484">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01c5a-485">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="01c5a-485">Additional resources</span></span>

* [<span data-ttu-id="01c5a-486">Code source de configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-486">Configuration source code</span></span>](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="01c5a-487">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-487">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="01c5a-488">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-488">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="01c5a-489">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="01c5a-489">Azure Key Vault</span></span>
* <span data-ttu-id="01c5a-490">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-490">Azure App Configuration</span></span>
* <span data-ttu-id="01c5a-491">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-491">Command-line arguments</span></span>
* <span data-ttu-id="01c5a-492">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="01c5a-492">Custom providers (installed or created)</span></span>
* <span data-ttu-id="01c5a-493">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-493">Directory files</span></span>
* <span data-ttu-id="01c5a-494">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-494">Environment variables</span></span>
* <span data-ttu-id="01c5a-495">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-495">In-memory .NET objects</span></span>
* <span data-ttu-id="01c5a-496">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="01c5a-496">Settings files</span></span>

<span data-ttu-id="01c5a-497">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="01c5a-497">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="01c5a-498">Les exemples de code qui suivent et dans l’échantillon d’application utilisent l’espace de noms <xref:Microsoft.Extensions.Configuration> :</span><span class="sxs-lookup"><span data-stu-id="01c5a-498">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="01c5a-499">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="01c5a-499">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="01c5a-500">Les options utilisent des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="01c5a-500">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="01c5a-501">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-501">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="01c5a-502">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="01c5a-502">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="01c5a-503">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="01c5a-503">Host versus app configuration</span></span>

<span data-ttu-id="01c5a-504">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="01c5a-504">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="01c5a-505">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="01c5a-505">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="01c5a-506">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="01c5a-506">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="01c5a-507">Les paires clé-valeur de la configuration de l’hôte sont également incluses dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-507">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="01c5a-508">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-508">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="01c5a-509">Autre configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-509">Other configuration</span></span>

<span data-ttu-id="01c5a-510">Cette rubrique se rapporte uniquement à la configuration de l' *application*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-510">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="01c5a-511">D’autres aspects de l’exécution et de l’hébergement des applications ASP.NET Core sont configurés à l’aide des fichiers de configuration non traités dans cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="01c5a-511">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="01c5a-512">*Launch. json*/*launchSettings. JSON* sont des fichiers de configuration d’outils pour l’environnement de développement, décrits ci-après :</span><span class="sxs-lookup"><span data-stu-id="01c5a-512">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="01c5a-513">Dans <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-513">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="01c5a-514">Dans l’ensemble de la documentation dans lequel les fichiers sont utilisés pour configurer des applications ASP.NET Core pour les scénarios de développement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-514">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="01c5a-515">*Web. config* est un fichier de configuration de serveur, décrit dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-515">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="01c5a-516">Pour plus d’informations sur la migration de la configuration d’application à partir de versions antérieures de ASP.NET, consultez <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-516">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="01c5a-517">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="01c5a-517">Default configuration</span></span>

<span data-ttu-id="01c5a-518">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="01c5a-518">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="01c5a-519">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-519">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="01c5a-520">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="01c5a-520">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="01c5a-521">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte générique](xref:fundamentals/host/generic-host), consultez la [dernière version de cette rubrique](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="01c5a-521">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="01c5a-522">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="01c5a-522">Host configuration is provided from:</span></span>
  * <span data-ttu-id="01c5a-523">Des variables d’environnement préfixées avec `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-523">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="01c5a-524">Le préfixe (`ASPNETCORE_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="01c5a-524">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="01c5a-525">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-525">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="01c5a-526">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="01c5a-526">App configuration is provided from:</span></span>
  * <span data-ttu-id="01c5a-527">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-527">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="01c5a-528">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-528">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="01c5a-529">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-529">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="01c5a-530">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-530">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="01c5a-531">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="01c5a-531">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="01c5a-532">Sécurité</span><span class="sxs-lookup"><span data-stu-id="01c5a-532">Security</span></span>

<span data-ttu-id="01c5a-533">Adoptez les pratiques suivantes pour sécuriser les données de configuration sensibles :</span><span class="sxs-lookup"><span data-stu-id="01c5a-533">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="01c5a-534">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="01c5a-534">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="01c5a-535">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="01c5a-535">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="01c5a-536">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="01c5a-536">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="01c5a-537">Pour plus d’informations, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-537">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="01c5a-538"><xref:security/app-secrets> &ndash; fournit des conseils sur l’utilisation de variables d’environnement pour stocker des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="01c5a-538"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="01c5a-539">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="01c5a-539">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="01c5a-540">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="01c5a-540">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="01c5a-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stocke en toute sécurité des secrets d’application pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01c5a-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="01c5a-542">Pour plus d'informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-542">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="01c5a-543">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="01c5a-543">Hierarchical configuration data</span></span>

<span data-ttu-id="01c5a-544">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-544">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="01c5a-545">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="01c5a-545">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="01c5a-546">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-546">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="01c5a-547">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="01c5a-547">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="01c5a-548">section0:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-548">section0:key0</span></span>
* <span data-ttu-id="01c5a-549">section0:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-549">section0:key1</span></span>
* <span data-ttu-id="01c5a-550">section1:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-550">section1:key0</span></span>
* <span data-ttu-id="01c5a-551">section1:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-551">section1:key1</span></span>

<span data-ttu-id="01c5a-552">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-552"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="01c5a-553">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="01c5a-553">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="01c5a-554">Conventions</span><span class="sxs-lookup"><span data-stu-id="01c5a-554">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="01c5a-555">Sources et fournisseurs</span><span class="sxs-lookup"><span data-stu-id="01c5a-555">Sources and providers</span></span>

<span data-ttu-id="01c5a-556">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="01c5a-556">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="01c5a-557">Les fournisseurs de configuration qui implémentent la détection des modifications peuvent recharger la configuration lorsqu’un paramètre sous-jacent est modifié.</span><span class="sxs-lookup"><span data-stu-id="01c5a-557">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="01c5a-558">Par exemple, le fournisseur de configuration de fichier (décrit plus loin dans cette rubrique) et le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) implémentent la détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="01c5a-558">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="01c5a-559"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-559"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="01c5a-560"><xref:Microsoft.Extensions.Configuration.IConfiguration> peuvent être injectées dans un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> ou MVC <xref:Microsoft.AspNetCore.Mvc.Controller> pour obtenir la configuration de la classe.</span><span class="sxs-lookup"><span data-stu-id="01c5a-560"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="01c5a-561">Dans les exemples suivants, le champ `_config` est utilisé pour accéder aux valeurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-561">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="01c5a-562">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="01c5a-562">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="01c5a-563">Keys</span><span class="sxs-lookup"><span data-stu-id="01c5a-563">Keys</span></span>

<span data-ttu-id="01c5a-564">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-564">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="01c5a-565">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="01c5a-565">Keys are case-insensitive.</span></span> <span data-ttu-id="01c5a-566">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-566">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="01c5a-567">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="01c5a-567">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="01c5a-568">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="01c5a-568">Hierarchical keys</span></span>
  * <span data-ttu-id="01c5a-569">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-569">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="01c5a-570">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-570">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="01c5a-571">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et automatiquement transformé en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="01c5a-571">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="01c5a-572">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="01c5a-572">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="01c5a-573">Écrivez du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-573">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="01c5a-574"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-574">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="01c5a-575">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="01c5a-575">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="01c5a-576">Valeurs</span><span class="sxs-lookup"><span data-stu-id="01c5a-576">Values</span></span>

<span data-ttu-id="01c5a-577">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-577">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="01c5a-578">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-578">Values are strings.</span></span>
* <span data-ttu-id="01c5a-579">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="01c5a-579">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="01c5a-580">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="01c5a-580">Providers</span></span>

<span data-ttu-id="01c5a-581">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01c5a-581">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="01c5a-582">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="01c5a-582">Provider</span></span> | <span data-ttu-id="01c5a-583">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="01c5a-583">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="01c5a-584">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="01c5a-584">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="01c5a-585">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="01c5a-585">Azure Key Vault</span></span> |
| <span data-ttu-id="01c5a-586">[Fournisseur Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentation Azure)</span><span class="sxs-lookup"><span data-stu-id="01c5a-586">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="01c5a-587">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="01c5a-587">Azure App Configuration</span></span> |
| [<span data-ttu-id="01c5a-588">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-588">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="01c5a-589">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-589">Command-line parameters</span></span> |
| [<span data-ttu-id="01c5a-590">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="01c5a-590">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="01c5a-591">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="01c5a-591">Custom source</span></span> |
| [<span data-ttu-id="01c5a-592">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-592">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="01c5a-593">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-593">Environment variables</span></span> |
| [<span data-ttu-id="01c5a-594">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="01c5a-594">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="01c5a-595">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="01c5a-595">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="01c5a-596">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="01c5a-596">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="01c5a-597">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-597">Directory files</span></span> |
| [<span data-ttu-id="01c5a-598">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-598">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="01c5a-599">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-599">In-memory collections</span></span> |
| <span data-ttu-id="01c5a-600">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="01c5a-600">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="01c5a-601">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="01c5a-601">File in the user profile directory</span></span> |

<span data-ttu-id="01c5a-602">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="01c5a-602">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="01c5a-603">Les fournisseurs de configuration décrits dans cette rubrique sont décrits par ordre alphabétique, et non pas dans l’ordre dans lequel le code les réorganise.</span><span class="sxs-lookup"><span data-stu-id="01c5a-603">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="01c5a-604">Commandez des fournisseurs de configuration dans le code pour répondre aux priorités des sources de configuration sous-jacentes requises par l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-604">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="01c5a-605">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="01c5a-605">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="01c5a-606">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="01c5a-606">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="01c5a-607">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="01c5a-607">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="01c5a-608">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="01c5a-608">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="01c5a-609">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-609">Environment variables</span></span>
1. <span data-ttu-id="01c5a-610">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-610">Command-line arguments</span></span>

<span data-ttu-id="01c5a-611">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="01c5a-611">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="01c5a-612">La séquence de fournisseurs précédente est utilisée lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-612">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="01c5a-613">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="01c5a-613">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="01c5a-614">Configurer le générateur d’ordinateur hôte avec UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="01c5a-614">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="01c5a-615">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> sur le générateur d’hôte avec la configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-615">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

## <a name="configureappconfiguration"></a><span data-ttu-id="01c5a-616">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="01c5a-616">ConfigureAppConfiguration</span></span>

<span data-ttu-id="01c5a-617">Appelez `ConfigureAppConfiguration` quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-617">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="01c5a-618">Remplacez la configuration précédente par des arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-618">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="01c5a-619">Pour fournir une configuration d’application pouvant être remplacée par des arguments de ligne de commande, appelez `AddCommandLine` en dernier :</span><span class="sxs-lookup"><span data-stu-id="01c5a-619">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="01c5a-620">Supprimer les fournisseurs ajoutés par CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="01c5a-620">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="01c5a-621">Pour supprimer les fournisseurs ajoutés par `CreateDefaultBuilder`, appelez d’abord [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) sur [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="01c5a-621">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="01c5a-622">Utiliser la configuration lors du démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="01c5a-622">Consume configuration during app startup</span></span>

<span data-ttu-id="01c5a-623">La configuration fournie à l’application dans `ConfigureAppConfiguration` est disponible lors du démarrage de l’application, notamment `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-623">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="01c5a-624">Pour plus d’informations, consultez la section [Accéder à la configuration lors du démarrage](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="01c5a-624">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="01c5a-625">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-625">Command-line Configuration Provider</span></span>

<span data-ttu-id="01c5a-626"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="01c5a-626">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="01c5a-627">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-627">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="01c5a-628">`AddCommandLine` est appelé automatiquement quand `CreateDefaultBuilder(string [])` est appelé.</span><span class="sxs-lookup"><span data-stu-id="01c5a-628">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="01c5a-629">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="01c5a-629">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="01c5a-630">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="01c5a-630">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="01c5a-631">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-631">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="01c5a-632">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-632">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="01c5a-633">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-633">Environment variables.</span></span>

<span data-ttu-id="01c5a-634">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="01c5a-634">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="01c5a-635">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="01c5a-635">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="01c5a-636">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="01c5a-636">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="01c5a-637">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="01c5a-637">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="01c5a-638">Pour les applications basées sur les modèles ASP.NET Core, `AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-638">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="01c5a-639">Pour ajouter des fournisseurs de configuration supplémentaires et conserver la capacité de remplacer leur configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-639">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="01c5a-640">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="01c5a-640">**Example**</span></span>

<span data-ttu-id="01c5a-641">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-641">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="01c5a-642">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="01c5a-642">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="01c5a-643">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-643">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="01c5a-644">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-644">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="01c5a-645">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-645">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="01c5a-646">Arguments</span><span class="sxs-lookup"><span data-stu-id="01c5a-646">Arguments</span></span>

<span data-ttu-id="01c5a-647">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="01c5a-647">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="01c5a-648">La valeur n’est pas requise si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="01c5a-648">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="01c5a-649">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="01c5a-649">Key prefix</span></span>               | <span data-ttu-id="01c5a-650">Exemple</span><span class="sxs-lookup"><span data-stu-id="01c5a-650">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="01c5a-651">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="01c5a-651">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="01c5a-652">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="01c5a-652">Two dashes (`--`)</span></span>        | <span data-ttu-id="01c5a-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="01c5a-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="01c5a-654">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="01c5a-654">Forward slash (`/`)</span></span>      | <span data-ttu-id="01c5a-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="01c5a-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="01c5a-656">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="01c5a-656">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="01c5a-657">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-657">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="01c5a-658">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="01c5a-658">Switch mappings</span></span>

<span data-ttu-id="01c5a-659">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="01c5a-659">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="01c5a-660">Lors de la génération manuelle d’une configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fournissez un dictionnaire de remplacements de commutateur dans la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-660">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="01c5a-661">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="01c5a-661">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="01c5a-662">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-662">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="01c5a-663">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="01c5a-663">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="01c5a-664">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="01c5a-664">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="01c5a-665">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="01c5a-665">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="01c5a-666">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="01c5a-666">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="01c5a-667">Créez un dictionnaire des mappages de commutateurs.</span><span class="sxs-lookup"><span data-stu-id="01c5a-667">Create a switch mappings dictionary.</span></span> <span data-ttu-id="01c5a-668">Dans l’exemple suivant, deux mappages de commutateurs sont créés :</span><span class="sxs-lookup"><span data-stu-id="01c5a-668">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="01c5a-669">Lorsque l’hôte est généré, appelez `AddCommandLine` avec le dictionnaire de mappages de commutateurs :</span><span class="sxs-lookup"><span data-stu-id="01c5a-669">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="01c5a-670">Pour les applications qui utilisent des mappages de commutateurs, l’appel à `CreateDefaultBuilder` ne doit pas passer d’arguments.</span><span class="sxs-lookup"><span data-stu-id="01c5a-670">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="01c5a-671">L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-671">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="01c5a-672">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="01c5a-672">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="01c5a-673">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="01c5a-673">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="01c5a-674">Touche</span><span class="sxs-lookup"><span data-stu-id="01c5a-674">Key</span></span>       | <span data-ttu-id="01c5a-675">Value</span><span class="sxs-lookup"><span data-stu-id="01c5a-675">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="01c5a-676">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="01c5a-676">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="01c5a-677">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="01c5a-677">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="01c5a-678">Touche</span><span class="sxs-lookup"><span data-stu-id="01c5a-678">Key</span></span>               | <span data-ttu-id="01c5a-679">Value</span><span class="sxs-lookup"><span data-stu-id="01c5a-679">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="01c5a-680">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-680">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="01c5a-681"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="01c5a-681">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="01c5a-682">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-682">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="01c5a-683">[Azure App service](https://azure.microsoft.com/services/app-service/) permet de définir des variables d’environnement dans le portail Azure qui peuvent remplacer la configuration de l’application à l’aide du fournisseur de configuration des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-683">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="01c5a-684">Pour plus d’informations, consultez les [applications Azure : Remplacer la configuration de l’application à l’aide du Portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="01c5a-684">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="01c5a-685">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `ASPNETCORE_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte web](xref:fundamentals/host/web-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="01c5a-685">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="01c5a-686">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="01c5a-686">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="01c5a-687">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="01c5a-687">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="01c5a-688">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="01c5a-688">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="01c5a-689">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-689">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="01c5a-690">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-690">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="01c5a-691">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-691">Command-line arguments.</span></span>

<span data-ttu-id="01c5a-692">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-692">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="01c5a-693">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-693">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="01c5a-694">Pour fournir la configuration d’application à partir de variables d’environnement supplémentaires, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddEnvironmentVariables` avec le préfixe :</span><span class="sxs-lookup"><span data-stu-id="01c5a-694">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="01c5a-695">Appelez `AddEnvironmentVariables` dernier pour autoriser les variables d’environnement avec le préfixe spécifié à substituer des valeurs d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="01c5a-695">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="01c5a-696">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="01c5a-696">**Example**</span></span>

<span data-ttu-id="01c5a-697">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-697">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="01c5a-698">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-698">Run the sample app.</span></span> <span data-ttu-id="01c5a-699">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-699">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="01c5a-700">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-700">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="01c5a-701">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="01c5a-701">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="01c5a-702">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-702">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="01c5a-703">Consultez le fichier *Pages/Index.cshtml.cs* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-703">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="01c5a-704">Pour exposer toutes les variables d’environnement disponibles pour l’application, remplacez la `FilteredConfiguration` dans *pages/index. cshtml. cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="01c5a-704">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="01c5a-705">Préfixes</span><span class="sxs-lookup"><span data-stu-id="01c5a-705">Prefixes</span></span>

<span data-ttu-id="01c5a-706">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe à la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-706">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="01c5a-707">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-707">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="01c5a-708">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="01c5a-708">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="01c5a-709">Lorsque le générateur d’hôte est créé, la configuration de l’hôte est fournie par des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-709">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="01c5a-710">Pour plus d’informations sur le préfixe utilisé pour ces variables d’environnement, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="01c5a-710">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="01c5a-711">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="01c5a-711">**Connection string prefixes**</span></span>

<span data-ttu-id="01c5a-712">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-712">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="01c5a-713">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-713">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="01c5a-714">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="01c5a-714">Connection string prefix</span></span> | <span data-ttu-id="01c5a-715">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="01c5a-715">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="01c5a-716">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="01c5a-716">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="01c5a-717">MySQL</span><span class="sxs-lookup"><span data-stu-id="01c5a-717">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="01c5a-718">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="01c5a-718">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="01c5a-719">SQL Server</span><span class="sxs-lookup"><span data-stu-id="01c5a-719">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="01c5a-720">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="01c5a-720">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="01c5a-721">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="01c5a-721">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="01c5a-722">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="01c5a-722">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="01c5a-723">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="01c5a-723">Environment variable key</span></span> | <span data-ttu-id="01c5a-724">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="01c5a-724">Converted configuration key</span></span> | <span data-ttu-id="01c5a-725">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="01c5a-725">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="01c5a-726">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-726">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="01c5a-727">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-727">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="01c5a-728">Valeur : `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="01c5a-728">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="01c5a-729">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-729">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="01c5a-730">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="01c5a-730">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="01c5a-731">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-731">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="01c5a-732">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="01c5a-732">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="01c5a-733">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="01c5a-733">**Example**</span></span>

<span data-ttu-id="01c5a-734">Une variable d’environnement de chaîne de connexion personnalisée est créée sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="01c5a-734">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="01c5a-735">Nom &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="01c5a-735">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="01c5a-736">Valeur &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="01c5a-736">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="01c5a-737">Si `IConfiguration` est injecté et affecté à un champ nommé `_config`, lisez la valeur :</span><span class="sxs-lookup"><span data-stu-id="01c5a-737">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="01c5a-738">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="01c5a-738">File Configuration Provider</span></span>

<span data-ttu-id="01c5a-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="01c5a-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="01c5a-740">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="01c5a-740">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="01c5a-741">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="01c5a-741">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="01c5a-742">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="01c5a-742">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="01c5a-743">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="01c5a-743">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="01c5a-744">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="01c5a-744">INI Configuration Provider</span></span>

<span data-ttu-id="01c5a-745"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="01c5a-745">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="01c5a-746">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-746">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="01c5a-747">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="01c5a-747">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="01c5a-748">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="01c5a-748">Overloads permit specifying:</span></span>

* <span data-ttu-id="01c5a-749">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="01c5a-749">Whether the file is optional.</span></span>
* <span data-ttu-id="01c5a-750">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="01c5a-750">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="01c5a-751">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-751">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="01c5a-752">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="01c5a-752">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="01c5a-753">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="01c5a-753">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="01c5a-754">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-754">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="01c5a-755">section0:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-755">section0:key0</span></span>
* <span data-ttu-id="01c5a-756">section0:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-756">section0:key1</span></span>
* <span data-ttu-id="01c5a-757">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="01c5a-757">section1:subsection:key</span></span>
* <span data-ttu-id="01c5a-758">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="01c5a-758">section2:subsection0:key</span></span>
* <span data-ttu-id="01c5a-759">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="01c5a-759">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="01c5a-760">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="01c5a-760">JSON Configuration Provider</span></span>

<span data-ttu-id="01c5a-761"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="01c5a-761">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="01c5a-762">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-762">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="01c5a-763">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="01c5a-763">Overloads permit specifying:</span></span>

* <span data-ttu-id="01c5a-764">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="01c5a-764">Whether the file is optional.</span></span>
* <span data-ttu-id="01c5a-765">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="01c5a-765">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="01c5a-766">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-766">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="01c5a-767">`AddJsonFile` est appelé automatiquement deux fois lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-767">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="01c5a-768">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="01c5a-768">The method is called to load configuration from:</span></span>

* <span data-ttu-id="01c5a-769">*appSettings. json* &ndash; ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-769">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="01c5a-770">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-770">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="01c5a-771">*appSettings. {Environment}. JSON* &ndash; la version de l’environnement du fichier est chargée à partir de [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="01c5a-771">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="01c5a-772">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="01c5a-772">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="01c5a-773">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="01c5a-773">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="01c5a-774">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-774">Environment variables.</span></span>
* <span data-ttu-id="01c5a-775">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-775">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="01c5a-776">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="01c5a-776">Command-line arguments.</span></span>

<span data-ttu-id="01c5a-777">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-777">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="01c5a-778">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-778">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="01c5a-779">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-779">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="01c5a-780">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="01c5a-780">**Example**</span></span>

<span data-ttu-id="01c5a-781">L’exemple d’application tire parti de la méthode de commodité statique `CreateDefaultBuilder` pour créer l’hôte, ce qui comprend deux appels à `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-781">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="01c5a-782">Le premier appel à `AddJsonFile` charge la configuration à partir de *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="01c5a-782">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="01c5a-783">Le deuxième appel à `AddJsonFile` charge la configuration à partir de *appSettings. { Environnement}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-783">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="01c5a-784">Pour *appSettings. Development. JSON* dans l’exemple d’application, le fichier suivant est chargé :</span><span class="sxs-lookup"><span data-stu-id="01c5a-784">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="01c5a-785">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-785">Run the sample app.</span></span> <span data-ttu-id="01c5a-786">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-786">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="01c5a-787">La sortie contient des paires clé-valeur pour la configuration en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-787">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="01c5a-788">Le niveau de journalisation de la `Logging:LogLevel:Default` de clé est `Debug` lors de l’exécution de l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="01c5a-788">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="01c5a-789">Exécutez à nouveau l’exemple d’application dans l’environnement de production :</span><span class="sxs-lookup"><span data-stu-id="01c5a-789">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="01c5a-790">Ouvrez le fichier *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="01c5a-790">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="01c5a-791">Dans le profil `ConfigurationSample`, remplacez la valeur de la variable d’environnement `ASPNETCORE_ENVIRONMENT` par `Production`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-791">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="01c5a-792">Enregistrez le fichier et exécutez l’application avec `dotnet run` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="01c5a-792">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="01c5a-793">Paramètres dans *appSettings. Development. JSON* ne remplace plus les paramètres dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="01c5a-793">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="01c5a-794">Le niveau de journalisation de la clé `Logging:LogLevel:Default` est `Warning`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-794">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="01c5a-795">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="01c5a-795">XML Configuration Provider</span></span>

<span data-ttu-id="01c5a-796"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="01c5a-796">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="01c5a-797">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-797">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="01c5a-798">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="01c5a-798">Overloads permit specifying:</span></span>

* <span data-ttu-id="01c5a-799">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="01c5a-799">Whether the file is optional.</span></span>
* <span data-ttu-id="01c5a-800">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="01c5a-800">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="01c5a-801">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-801">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="01c5a-802">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="01c5a-802">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="01c5a-803">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-803">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="01c5a-804">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="01c5a-804">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="01c5a-805">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="01c5a-805">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="01c5a-806">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-806">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="01c5a-807">section0:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-807">section0:key0</span></span>
* <span data-ttu-id="01c5a-808">section0:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-808">section0:key1</span></span>
* <span data-ttu-id="01c5a-809">section1:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-809">section1:key0</span></span>
* <span data-ttu-id="01c5a-810">section1:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-810">section1:key1</span></span>

<span data-ttu-id="01c5a-811">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="01c5a-811">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="01c5a-812">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-812">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="01c5a-813">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-813">section:section0:key:key0</span></span>
* <span data-ttu-id="01c5a-814">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-814">section:section0:key:key1</span></span>
* <span data-ttu-id="01c5a-815">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-815">section:section1:key:key0</span></span>
* <span data-ttu-id="01c5a-816">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-816">section:section1:key:key1</span></span>

<span data-ttu-id="01c5a-817">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="01c5a-817">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="01c5a-818">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="01c5a-818">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="01c5a-819">key:attribute</span><span class="sxs-lookup"><span data-stu-id="01c5a-819">key:attribute</span></span>
* <span data-ttu-id="01c5a-820">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="01c5a-820">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="01c5a-821">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="01c5a-821">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="01c5a-822">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-822">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="01c5a-823">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-823">The key is the file name.</span></span> <span data-ttu-id="01c5a-824">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="01c5a-824">The value contains the file's contents.</span></span> <span data-ttu-id="01c5a-825">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="01c5a-825">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="01c5a-826">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-826">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="01c5a-827">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="01c5a-827">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="01c5a-828">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="01c5a-828">Overloads permit specifying:</span></span>

* <span data-ttu-id="01c5a-829">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="01c5a-829">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="01c5a-830">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="01c5a-830">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="01c5a-831">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="01c5a-831">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="01c5a-832">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-832">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="01c5a-833">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="01c5a-833">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="01c5a-834">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="01c5a-834">Memory Configuration Provider</span></span>

<span data-ttu-id="01c5a-835">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-835">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="01c5a-836">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-836">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="01c5a-837">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-837">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="01c5a-838">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-838">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="01c5a-839">Dans l’exemple suivant, un dictionnaire de configuration est créé :</span><span class="sxs-lookup"><span data-stu-id="01c5a-839">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="01c5a-840">Le dictionnaire est utilisé avec un appel à `AddInMemoryCollection` pour fournir la configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-840">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="01c5a-841">GetValue</span><span class="sxs-lookup"><span data-stu-id="01c5a-841">GetValue</span></span>

<span data-ttu-id="01c5a-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur unique de la configuration avec une clé spécifiée et la convertit en type de non-collection spécifié.</span><span class="sxs-lookup"><span data-stu-id="01c5a-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="01c5a-843">Une surcharge accepte une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="01c5a-843">An overload accepts a default value.</span></span>

<span data-ttu-id="01c5a-844">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="01c5a-844">The following example:</span></span>

* <span data-ttu-id="01c5a-845">Extrait la valeur de chaîne de la configuration avec la clé `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-845">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="01c5a-846">Si `NumberKey` est introuvable dans les clés de configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-846">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="01c5a-847">Tape la valeur comme `int`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-847">Types the value as an `int`.</span></span>
* <span data-ttu-id="01c5a-848">Stocke la valeur dans la propriété `NumberConfig` pour une utilisation par la page.</span><span class="sxs-lookup"><span data-stu-id="01c5a-848">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="01c5a-849">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="01c5a-849">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="01c5a-850">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="01c5a-850">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="01c5a-851">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="01c5a-851">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="01c5a-852">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-852">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="01c5a-853">section0:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-853">section0:key0</span></span>
* <span data-ttu-id="01c5a-854">section0:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-854">section0:key1</span></span>
* <span data-ttu-id="01c5a-855">section1:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-855">section1:key0</span></span>
* <span data-ttu-id="01c5a-856">section1:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-856">section1:key1</span></span>
* <span data-ttu-id="01c5a-857">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-857">section2:subsection0:key0</span></span>
* <span data-ttu-id="01c5a-858">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-858">section2:subsection0:key1</span></span>
* <span data-ttu-id="01c5a-859">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="01c5a-859">section2:subsection1:key0</span></span>
* <span data-ttu-id="01c5a-860">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="01c5a-860">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="01c5a-861">GetSection</span><span class="sxs-lookup"><span data-stu-id="01c5a-861">GetSection</span></span>

<span data-ttu-id="01c5a-862">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-862">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="01c5a-863">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="01c5a-863">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="01c5a-864">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="01c5a-864">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="01c5a-865">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="01c5a-865">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="01c5a-866">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-866">`GetSection` never returns `null`.</span></span> <span data-ttu-id="01c5a-867">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="01c5a-867">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="01c5a-868">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="01c5a-868">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="01c5a-869"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="01c5a-869">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="01c5a-870">GetChildren</span><span class="sxs-lookup"><span data-stu-id="01c5a-870">GetChildren</span></span>

<span data-ttu-id="01c5a-871">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="01c5a-871">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="01c5a-872">Existe</span><span class="sxs-lookup"><span data-stu-id="01c5a-872">Exists</span></span>

<span data-ttu-id="01c5a-873">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="01c5a-873">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="01c5a-874">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-874">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="01c5a-875">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="01c5a-875">Bind to an object graph</span></span>

<span data-ttu-id="01c5a-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="01c5a-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="01c5a-877">Comme pour la liaison d’un objet simple, seules les propriétés accessibles en lecture/écriture publiques sont liées.</span><span class="sxs-lookup"><span data-stu-id="01c5a-877">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="01c5a-878">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="01c5a-878">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="01c5a-879">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-879">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="01c5a-880">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-880">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="01c5a-881">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="01c5a-881">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="01c5a-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="01c5a-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="01c5a-883">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-883">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="01c5a-884">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="01c5a-884">The following code shows how to use `Get<T>` with the preceding example:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="01c5a-885">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="01c5a-885">Bind an array to a class</span></span>

<span data-ttu-id="01c5a-886">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="01c5a-886">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="01c5a-887"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-887">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="01c5a-888">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer une liaison de tableau à un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="01c5a-888">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="01c5a-889">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="01c5a-889">Binding is provided by convention.</span></span> <span data-ttu-id="01c5a-890">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="01c5a-890">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="01c5a-891">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="01c5a-891">**In-memory array processing**</span></span>

<span data-ttu-id="01c5a-892">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="01c5a-892">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="01c5a-893">Touche</span><span class="sxs-lookup"><span data-stu-id="01c5a-893">Key</span></span>             | <span data-ttu-id="01c5a-894">Value</span><span class="sxs-lookup"><span data-stu-id="01c5a-894">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="01c5a-895">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="01c5a-895">array:entries:0</span></span> | <span data-ttu-id="01c5a-896">value0</span><span class="sxs-lookup"><span data-stu-id="01c5a-896">value0</span></span> |
| <span data-ttu-id="01c5a-897">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="01c5a-897">array:entries:1</span></span> | <span data-ttu-id="01c5a-898">valeur1</span><span class="sxs-lookup"><span data-stu-id="01c5a-898">value1</span></span> |
| <span data-ttu-id="01c5a-899">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="01c5a-899">array:entries:2</span></span> | <span data-ttu-id="01c5a-900">valeur2</span><span class="sxs-lookup"><span data-stu-id="01c5a-900">value2</span></span> |
| <span data-ttu-id="01c5a-901">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="01c5a-901">array:entries:4</span></span> | <span data-ttu-id="01c5a-902">value4</span><span class="sxs-lookup"><span data-stu-id="01c5a-902">value4</span></span> |
| <span data-ttu-id="01c5a-903">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="01c5a-903">array:entries:5</span></span> | <span data-ttu-id="01c5a-904">value5</span><span class="sxs-lookup"><span data-stu-id="01c5a-904">value5</span></span> |

<span data-ttu-id="01c5a-905">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="01c5a-905">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="01c5a-906">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="01c5a-906">The array skips a value for index &num;3.</span></span> <span data-ttu-id="01c5a-907">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="01c5a-907">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="01c5a-908">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="01c5a-908">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="01c5a-909">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="01c5a-909">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="01c5a-910">vous pouvez également utiliser la syntaxe de [`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) , ce qui se traduit par un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="01c5a-910">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="01c5a-911">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-911">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="01c5a-912">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="01c5a-912">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="01c5a-913">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="01c5a-913">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="01c5a-914">0</span><span class="sxs-lookup"><span data-stu-id="01c5a-914">0</span></span>                            | <span data-ttu-id="01c5a-915">value0</span><span class="sxs-lookup"><span data-stu-id="01c5a-915">value0</span></span>                       |
| <span data-ttu-id="01c5a-916">1</span><span class="sxs-lookup"><span data-stu-id="01c5a-916">1</span></span>                            | <span data-ttu-id="01c5a-917">valeur1</span><span class="sxs-lookup"><span data-stu-id="01c5a-917">value1</span></span>                       |
| <span data-ttu-id="01c5a-918">2</span><span class="sxs-lookup"><span data-stu-id="01c5a-918">2</span></span>                            | <span data-ttu-id="01c5a-919">valeur2</span><span class="sxs-lookup"><span data-stu-id="01c5a-919">value2</span></span>                       |
| <span data-ttu-id="01c5a-920">3</span><span class="sxs-lookup"><span data-stu-id="01c5a-920">3</span></span>                            | <span data-ttu-id="01c5a-921">value4</span><span class="sxs-lookup"><span data-stu-id="01c5a-921">value4</span></span>                       |
| <span data-ttu-id="01c5a-922">4</span><span class="sxs-lookup"><span data-stu-id="01c5a-922">4</span></span>                            | <span data-ttu-id="01c5a-923">value5</span><span class="sxs-lookup"><span data-stu-id="01c5a-923">value5</span></span>                       |

<span data-ttu-id="01c5a-924">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-924">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="01c5a-925">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="01c5a-925">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="01c5a-926">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="01c5a-926">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="01c5a-927">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-927">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="01c5a-928">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-928">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="01c5a-929">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-929">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="01c5a-930">Dans `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="01c5a-930">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="01c5a-931">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-931">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="01c5a-932">Touche</span><span class="sxs-lookup"><span data-stu-id="01c5a-932">Key</span></span>             | <span data-ttu-id="01c5a-933">Value</span><span class="sxs-lookup"><span data-stu-id="01c5a-933">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="01c5a-934">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="01c5a-934">array:entries:3</span></span> | <span data-ttu-id="01c5a-935">valeur3</span><span class="sxs-lookup"><span data-stu-id="01c5a-935">value3</span></span> |

<span data-ttu-id="01c5a-936">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="01c5a-936">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="01c5a-937">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="01c5a-937">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="01c5a-938">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="01c5a-938">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="01c5a-939">0</span><span class="sxs-lookup"><span data-stu-id="01c5a-939">0</span></span>                            | <span data-ttu-id="01c5a-940">value0</span><span class="sxs-lookup"><span data-stu-id="01c5a-940">value0</span></span>                       |
| <span data-ttu-id="01c5a-941">1</span><span class="sxs-lookup"><span data-stu-id="01c5a-941">1</span></span>                            | <span data-ttu-id="01c5a-942">valeur1</span><span class="sxs-lookup"><span data-stu-id="01c5a-942">value1</span></span>                       |
| <span data-ttu-id="01c5a-943">2</span><span class="sxs-lookup"><span data-stu-id="01c5a-943">2</span></span>                            | <span data-ttu-id="01c5a-944">valeur2</span><span class="sxs-lookup"><span data-stu-id="01c5a-944">value2</span></span>                       |
| <span data-ttu-id="01c5a-945">3</span><span class="sxs-lookup"><span data-stu-id="01c5a-945">3</span></span>                            | <span data-ttu-id="01c5a-946">valeur3</span><span class="sxs-lookup"><span data-stu-id="01c5a-946">value3</span></span>                       |
| <span data-ttu-id="01c5a-947">4</span><span class="sxs-lookup"><span data-stu-id="01c5a-947">4</span></span>                            | <span data-ttu-id="01c5a-948">value4</span><span class="sxs-lookup"><span data-stu-id="01c5a-948">value4</span></span>                       |
| <span data-ttu-id="01c5a-949">5\.</span><span class="sxs-lookup"><span data-stu-id="01c5a-949">5</span></span>                            | <span data-ttu-id="01c5a-950">value5</span><span class="sxs-lookup"><span data-stu-id="01c5a-950">value5</span></span>                       |

<span data-ttu-id="01c5a-951">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="01c5a-951">**JSON array processing**</span></span>

<span data-ttu-id="01c5a-952">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="01c5a-952">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="01c5a-953">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="01c5a-953">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="01c5a-954">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-954">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="01c5a-955">Touche</span><span class="sxs-lookup"><span data-stu-id="01c5a-955">Key</span></span>                     | <span data-ttu-id="01c5a-956">Value</span><span class="sxs-lookup"><span data-stu-id="01c5a-956">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="01c5a-957">json_array:key</span><span class="sxs-lookup"><span data-stu-id="01c5a-957">json_array:key</span></span>          | <span data-ttu-id="01c5a-958">valueA</span><span class="sxs-lookup"><span data-stu-id="01c5a-958">valueA</span></span> |
| <span data-ttu-id="01c5a-959">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="01c5a-959">json_array:subsection:0</span></span> | <span data-ttu-id="01c5a-960">valueB</span><span class="sxs-lookup"><span data-stu-id="01c5a-960">valueB</span></span> |
| <span data-ttu-id="01c5a-961">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="01c5a-961">json_array:subsection:1</span></span> | <span data-ttu-id="01c5a-962">valueC</span><span class="sxs-lookup"><span data-stu-id="01c5a-962">valueC</span></span> |
| <span data-ttu-id="01c5a-963">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="01c5a-963">json_array:subsection:2</span></span> | <span data-ttu-id="01c5a-964">valueD</span><span class="sxs-lookup"><span data-stu-id="01c5a-964">valueD</span></span> |

<span data-ttu-id="01c5a-965">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="01c5a-965">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="01c5a-966">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-966">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="01c5a-967">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-967">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="01c5a-968">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="01c5a-968">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="01c5a-969">Valeur `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="01c5a-969">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="01c5a-970">0</span><span class="sxs-lookup"><span data-stu-id="01c5a-970">0</span></span>                                   | <span data-ttu-id="01c5a-971">valueB</span><span class="sxs-lookup"><span data-stu-id="01c5a-971">valueB</span></span>                              |
| <span data-ttu-id="01c5a-972">1</span><span class="sxs-lookup"><span data-stu-id="01c5a-972">1</span></span>                                   | <span data-ttu-id="01c5a-973">valueC</span><span class="sxs-lookup"><span data-stu-id="01c5a-973">valueC</span></span>                              |
| <span data-ttu-id="01c5a-974">2</span><span class="sxs-lookup"><span data-stu-id="01c5a-974">2</span></span>                                   | <span data-ttu-id="01c5a-975">valueD</span><span class="sxs-lookup"><span data-stu-id="01c5a-975">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="01c5a-976">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="01c5a-976">Custom configuration provider</span></span>

<span data-ttu-id="01c5a-977">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="01c5a-977">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="01c5a-978">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="01c5a-978">The provider has the following characteristics:</span></span>

* <span data-ttu-id="01c5a-979">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-979">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="01c5a-980">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="01c5a-980">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="01c5a-981">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="01c5a-981">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="01c5a-982">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="01c5a-982">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="01c5a-983">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-983">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="01c5a-984">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="01c5a-984">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="01c5a-985">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-985">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="01c5a-986">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="01c5a-986">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="01c5a-987">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-987">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="01c5a-988">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-988">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="01c5a-989">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-989">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="01c5a-990">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-990">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="01c5a-991">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="01c5a-991">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="01c5a-992">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-992">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="01c5a-993">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-993">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="01c5a-994">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-994">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="01c5a-995">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="01c5a-995">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="01c5a-996">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="01c5a-996">Access configuration during startup</span></span>

<span data-ttu-id="01c5a-997">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="01c5a-997">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="01c5a-998">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="01c5a-998">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="01c5a-999">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="01c5a-999">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="01c5a-1000">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="01c5a-1000">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="01c5a-1001">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="01c5a-1001">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="01c5a-1002">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="01c5a-1002">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="01c5a-1003">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="01c5a-1003">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="01c5a-1004">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="01c5a-1004">Add configuration from an external assembly</span></span>

<span data-ttu-id="01c5a-1005">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="01c5a-1005">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="01c5a-1006">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="01c5a-1006">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01c5a-1007">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="01c5a-1007">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
