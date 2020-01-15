---
title: Configuration dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 09ef06f179e34cd7f4f04ac30c3b5dd95d058244
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75951884"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="e1a53-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1a53-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="e1a53-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e1a53-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e1a53-105">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="e1a53-106">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="e1a53-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e1a53-107">Azure Key Vault</span></span>
* <span data-ttu-id="e1a53-108">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="e1a53-108">Azure App Configuration</span></span>
* <span data-ttu-id="e1a53-109">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e1a53-109">Command-line arguments</span></span>
* <span data-ttu-id="e1a53-110">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="e1a53-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e1a53-111">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="e1a53-111">Directory files</span></span>
* <span data-ttu-id="e1a53-112">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="e1a53-112">Environment variables</span></span>
* <span data-ttu-id="e1a53-113">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="e1a53-113">In-memory .NET objects</span></span>
* <span data-ttu-id="e1a53-114">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="e1a53-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e1a53-115">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le framework.</span><span class="sxs-lookup"><span data-stu-id="e1a53-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e1a53-116">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e1a53-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="e1a53-117">Les exemples de code qui suivent et dans l’échantillon d’application utilisent l’espace de noms <xref:Microsoft.Extensions.Configuration> :</span><span class="sxs-lookup"><span data-stu-id="e1a53-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="e1a53-118">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e1a53-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="e1a53-119">Les options utilisent des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="e1a53-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="e1a53-120">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e1a53-121">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e1a53-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="e1a53-122">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="e1a53-122">Host versus app configuration</span></span>

<span data-ttu-id="e1a53-123">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="e1a53-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="e1a53-124">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="e1a53-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e1a53-125">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e1a53-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="e1a53-126">Les paires clé-valeur de la configuration de l’hôte sont également incluses dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="e1a53-127">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="e1a53-128">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="e1a53-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e1a53-129">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="e1a53-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="e1a53-130">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="e1a53-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="e1a53-131">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="e1a53-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="e1a53-132">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte Web](xref:fundamentals/host/web-host), consultez la [version ASP.NET Core 2.2. de cette rubrique](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="e1a53-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="e1a53-133">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e1a53-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="e1a53-134">Des variables d’environnement préfixées avec `DOTNET_` (par exemple, `DOTNET_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="e1a53-135">Le préfixe (`DOTNET_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="e1a53-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="e1a53-136">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="e1a53-137">La configuration par défaut de l’hôte Web est établie (`ConfigureWebHostDefaults`) :</span><span class="sxs-lookup"><span data-stu-id="e1a53-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="e1a53-138">Kestrel est utilisé comme serveur web et configuré à l’aide des fournisseurs de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="e1a53-139">Ajoutez l’intergiciel de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="e1a53-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="e1a53-140">Ajoutez l’intergiciel d’en-têtes transférés si la variable d'environnement `ASPNETCORE_FORWARDEDHEADERS_ENABLED` est définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="e1a53-141">Activez l’intégration d’IIS.</span><span class="sxs-lookup"><span data-stu-id="e1a53-141">Enable IIS integration.</span></span>
* <span data-ttu-id="e1a53-142">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e1a53-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="e1a53-143">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e1a53-144">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e1a53-145">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="e1a53-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="e1a53-146">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="e1a53-147">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e1a53-148">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="e1a53-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="e1a53-149">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="e1a53-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="e1a53-150">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="e1a53-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="e1a53-151">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte générique](xref:fundamentals/host/generic-host), consultez la [dernière version de cette rubrique](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e1a53-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="e1a53-152">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e1a53-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="e1a53-153">Des variables d’environnement préfixées avec `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="e1a53-154">Le préfixe (`ASPNETCORE_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="e1a53-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="e1a53-155">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="e1a53-156">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e1a53-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="e1a53-157">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e1a53-158">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e1a53-159">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="e1a53-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="e1a53-160">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="e1a53-161">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e1a53-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="e1a53-162">Sécurité</span><span class="sxs-lookup"><span data-stu-id="e1a53-162">Security</span></span>

<span data-ttu-id="e1a53-163">Adoptez les pratiques suivantes pour sécuriser les données de configuration sensibles :</span><span class="sxs-lookup"><span data-stu-id="e1a53-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="e1a53-164">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="e1a53-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="e1a53-165">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="e1a53-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="e1a53-166">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="e1a53-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="e1a53-167">Pour plus d’informations, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="e1a53-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="e1a53-168"><xref:security/app-secrets> &ndash; fournit des conseils sur l’utilisation de variables d’environnement pour stocker des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="e1a53-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="e1a53-169">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="e1a53-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="e1a53-170">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e1a53-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="e1a53-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stocke en toute sécurité des secrets d’application pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1a53-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="e1a53-172">Pour plus d'informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="e1a53-173">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="e1a53-173">Hierarchical configuration data</span></span>

<span data-ttu-id="e1a53-174">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="e1a53-175">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="e1a53-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="e1a53-176">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="e1a53-177">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="e1a53-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="e1a53-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-178">section0:key0</span></span>
* <span data-ttu-id="e1a53-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-179">section0:key1</span></span>
* <span data-ttu-id="e1a53-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-180">section1:key0</span></span>
* <span data-ttu-id="e1a53-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-181">section1:key1</span></span>

<span data-ttu-id="e1a53-182">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="e1a53-183">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="e1a53-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="e1a53-184">Conventions</span><span class="sxs-lookup"><span data-stu-id="e1a53-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="e1a53-185">Sources et fournisseurs</span><span class="sxs-lookup"><span data-stu-id="e1a53-185">Sources and providers</span></span>

<span data-ttu-id="e1a53-186">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="e1a53-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="e1a53-187">Les fournisseurs de configuration qui implémentent la détection des modifications peuvent recharger la configuration lorsqu’un paramètre sous-jacent est modifié.</span><span class="sxs-lookup"><span data-stu-id="e1a53-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="e1a53-188">Par exemple, le fournisseur de configuration de fichier (décrit plus loin dans cette rubrique) et le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) implémentent la détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="e1a53-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="e1a53-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e1a53-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> peuvent être injectées dans un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> ou MVC <xref:Microsoft.AspNetCore.Mvc.Controller> pour obtenir la configuration de la classe.</span><span class="sxs-lookup"><span data-stu-id="e1a53-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="e1a53-191">Dans les exemples suivants, le champ `_config` est utilisé pour accéder aux valeurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-191">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="e1a53-192">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="e1a53-192">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="e1a53-193">Touches</span><span class="sxs-lookup"><span data-stu-id="e1a53-193">Keys</span></span>

<span data-ttu-id="e1a53-194">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e1a53-194">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="e1a53-195">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e1a53-195">Keys are case-insensitive.</span></span> <span data-ttu-id="e1a53-196">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="e1a53-196">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="e1a53-197">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="e1a53-197">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="e1a53-198">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="e1a53-198">Hierarchical keys</span></span>
  * <span data-ttu-id="e1a53-199">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="e1a53-199">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="e1a53-200">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="e1a53-200">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="e1a53-201">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et automatiquement transformé en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="e1a53-201">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="e1a53-202">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="e1a53-202">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="e1a53-203">Écrivez du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-203">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="e1a53-204"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-204">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e1a53-205">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="e1a53-205">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="e1a53-206">Valeurs</span><span class="sxs-lookup"><span data-stu-id="e1a53-206">Values</span></span>

<span data-ttu-id="e1a53-207">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e1a53-207">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="e1a53-208">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="e1a53-208">Values are strings.</span></span>
* <span data-ttu-id="e1a53-209">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="e1a53-209">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="e1a53-210">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="e1a53-210">Providers</span></span>

<span data-ttu-id="e1a53-211">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1a53-211">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="e1a53-212">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="e1a53-212">Provider</span></span> | <span data-ttu-id="e1a53-213">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="e1a53-213">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="e1a53-214">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e1a53-214">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="e1a53-215">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e1a53-215">Azure Key Vault</span></span> |
| <span data-ttu-id="e1a53-216">[Fournisseur Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentation Azure)</span><span class="sxs-lookup"><span data-stu-id="e1a53-216">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="e1a53-217">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="e1a53-217">Azure App Configuration</span></span> |
| [<span data-ttu-id="e1a53-218">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e1a53-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e1a53-219">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e1a53-219">Command-line parameters</span></span> |
| [<span data-ttu-id="e1a53-220">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="e1a53-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e1a53-221">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="e1a53-221">Custom source</span></span> |
| [<span data-ttu-id="e1a53-222">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e1a53-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e1a53-223">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="e1a53-223">Environment variables</span></span> |
| [<span data-ttu-id="e1a53-224">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="e1a53-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e1a53-225">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="e1a53-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e1a53-226">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="e1a53-226">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="e1a53-227">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="e1a53-227">Directory files</span></span> |
| [<span data-ttu-id="e1a53-228">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="e1a53-228">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e1a53-229">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="e1a53-229">In-memory collections</span></span> |
| <span data-ttu-id="e1a53-230">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e1a53-230">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e1a53-231">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="e1a53-231">File in the user profile directory</span></span> |

<span data-ttu-id="e1a53-232">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="e1a53-232">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="e1a53-233">Les fournisseurs de configuration décrits dans cette rubrique sont décrits par ordre alphabétique, et non pas dans l’ordre dans lequel le code les réorganise.</span><span class="sxs-lookup"><span data-stu-id="e1a53-233">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="e1a53-234">Commandez des fournisseurs de configuration dans le code pour répondre aux priorités des sources de configuration sous-jacentes requises par l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-234">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="e1a53-235">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="e1a53-235">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="e1a53-236">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="e1a53-236">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="e1a53-237">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e1a53-237">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="e1a53-238">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="e1a53-238">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="e1a53-239">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="e1a53-239">Environment variables</span></span>
1. <span data-ttu-id="e1a53-240">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e1a53-240">Command-line arguments</span></span>

<span data-ttu-id="e1a53-241">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="e1a53-241">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="e1a53-242">La séquence de fournisseurs précédente est utilisée lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-242">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e1a53-243">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e1a53-243">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="e1a53-244">Configurer le générateur d’ordinateur hôte avec ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="e1a53-244">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="e1a53-245">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> et fournissez la configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-245">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="e1a53-246">`ConfigureHostConfiguration` est utilisé pour initialiser <xref:Microsoft.Extensions.Hosting.IHostEnvironment> pour une utilisation ultérieure dans le processus de génération.</span><span class="sxs-lookup"><span data-stu-id="e1a53-246">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="e1a53-247">`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="e1a53-247">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="e1a53-248">Configurer le générateur d’ordinateur hôte avec UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="e1a53-248">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="e1a53-249">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> sur le générateur d’hôte avec la configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-249">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

::: moniker-end

## <a name="configureappconfiguration"></a><span data-ttu-id="e1a53-250">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="e1a53-250">ConfigureAppConfiguration</span></span>

<span data-ttu-id="e1a53-251">Appelez `ConfigureAppConfiguration` quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="e1a53-251">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="e1a53-252">Remplacez la configuration précédente par des arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e1a53-252">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="e1a53-253">Pour fournir une configuration d’application pouvant être remplacée par des arguments de ligne de commande, appelez `AddCommandLine` en dernier :</span><span class="sxs-lookup"><span data-stu-id="e1a53-253">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="e1a53-254">Supprimer les fournisseurs ajoutés par CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="e1a53-254">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="e1a53-255">Pour supprimer les fournisseurs ajoutés par `CreateDefaultBuilder`, appelez d’abord [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) sur [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="e1a53-255">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="e1a53-256">Utiliser la configuration lors du démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="e1a53-256">Consume configuration during app startup</span></span>

<span data-ttu-id="e1a53-257">La configuration fournie à l’application dans `ConfigureAppConfiguration` est disponible lors du démarrage de l’application, notamment `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-257">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e1a53-258">Pour plus d’informations, consultez la section [Accéder à la configuration lors du démarrage](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="e1a53-258">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="e1a53-259">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e1a53-259">Command-line Configuration Provider</span></span>

<span data-ttu-id="e1a53-260"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e1a53-260">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="e1a53-261">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-261">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e1a53-262">`AddCommandLine` est appelé automatiquement quand `CreateDefaultBuilder(string [])` est appelé.</span><span class="sxs-lookup"><span data-stu-id="e1a53-262">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="e1a53-263">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e1a53-263">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e1a53-264">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e1a53-264">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e1a53-265">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-265">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="e1a53-266">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-266">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e1a53-267">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-267">Environment variables.</span></span>

<span data-ttu-id="e1a53-268">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e1a53-268">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="e1a53-269">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="e1a53-269">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="e1a53-270">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="e1a53-270">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="e1a53-271">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="e1a53-271">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="e1a53-272">Pour les applications basées sur les modèles ASP.NET Core, `AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-272">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e1a53-273">Pour ajouter des fournisseurs de configuration supplémentaires et conserver la capacité de remplacer leur configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-273">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="e1a53-274">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e1a53-274">**Example**</span></span>

<span data-ttu-id="e1a53-275">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-275">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="e1a53-276">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="e1a53-276">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="e1a53-277">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-277">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="e1a53-278">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-278">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e1a53-279">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-279">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="e1a53-280">Arguments</span><span class="sxs-lookup"><span data-stu-id="e1a53-280">Arguments</span></span>

<span data-ttu-id="e1a53-281">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="e1a53-281">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="e1a53-282">La valeur n’est pas requise si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="e1a53-282">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="e1a53-283">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="e1a53-283">Key prefix</span></span>               | <span data-ttu-id="e1a53-284">Exemple</span><span class="sxs-lookup"><span data-stu-id="e1a53-284">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="e1a53-285">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="e1a53-285">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="e1a53-286">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="e1a53-286">Two dashes (`--`)</span></span>        | <span data-ttu-id="e1a53-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="e1a53-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="e1a53-288">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="e1a53-288">Forward slash (`/`)</span></span>      | <span data-ttu-id="e1a53-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="e1a53-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="e1a53-290">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="e1a53-290">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="e1a53-291">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="e1a53-291">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="e1a53-292">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="e1a53-292">Switch mappings</span></span>

<span data-ttu-id="e1a53-293">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="e1a53-293">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="e1a53-294">Lors de la génération manuelle d’une configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fournissez un dictionnaire de remplacements de commutateur dans la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-294">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="e1a53-295">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e1a53-295">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e1a53-296">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-296">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="e1a53-297">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="e1a53-297">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e1a53-298">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="e1a53-298">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e1a53-299">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="e1a53-299">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e1a53-300">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="e1a53-300">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="e1a53-301">Créez un dictionnaire des mappages de commutateurs.</span><span class="sxs-lookup"><span data-stu-id="e1a53-301">Create a switch mappings dictionary.</span></span> <span data-ttu-id="e1a53-302">Dans l’exemple suivant, deux mappages de commutateurs sont créés :</span><span class="sxs-lookup"><span data-stu-id="e1a53-302">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="e1a53-303">Lorsque l’hôte est généré, appelez `AddCommandLine` avec le dictionnaire de mappages de commutateurs :</span><span class="sxs-lookup"><span data-stu-id="e1a53-303">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="e1a53-304">Pour les applications qui utilisent des mappages de commutateurs, l’appel à `CreateDefaultBuilder` ne doit pas passer d’arguments.</span><span class="sxs-lookup"><span data-stu-id="e1a53-304">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="e1a53-305">L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-305">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e1a53-306">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="e1a53-306">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="e1a53-307">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e1a53-307">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="e1a53-308">Clé</span><span class="sxs-lookup"><span data-stu-id="e1a53-308">Key</span></span>       | <span data-ttu-id="e1a53-309">Value</span><span class="sxs-lookup"><span data-stu-id="e1a53-309">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="e1a53-310">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="e1a53-310">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="e1a53-311">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e1a53-311">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="e1a53-312">Clé</span><span class="sxs-lookup"><span data-stu-id="e1a53-312">Key</span></span>               | <span data-ttu-id="e1a53-313">Value</span><span class="sxs-lookup"><span data-stu-id="e1a53-313">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="e1a53-314">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e1a53-314">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="e1a53-315"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e1a53-315">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="e1a53-316">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-316">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="e1a53-317">[Azure App service](https://azure.microsoft.com/services/app-service/) permet de définir des variables d’environnement dans le portail Azure qui peuvent remplacer la configuration de l’application à l’aide du fournisseur de configuration des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-317">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="e1a53-318">Pour plus d’informations, consultez [Azure Apps : remplacer la configuration de l’application à l’aide du portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="e1a53-318">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e1a53-319">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `DOTNET_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte générique](xref:fundamentals/host/generic-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="e1a53-319">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="e1a53-320">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e1a53-320">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e1a53-321">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `ASPNETCORE_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte web](xref:fundamentals/host/web-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="e1a53-321">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="e1a53-322">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e1a53-322">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="e1a53-323">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e1a53-323">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e1a53-324">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="e1a53-324">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="e1a53-325">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-325">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="e1a53-326">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-326">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e1a53-327">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e1a53-327">Command-line arguments.</span></span>

<span data-ttu-id="e1a53-328">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-328">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="e1a53-329">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-329">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="e1a53-330">Pour fournir la configuration d’application à partir de variables d’environnement supplémentaires, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddEnvironmentVariables` avec le préfixe :</span><span class="sxs-lookup"><span data-stu-id="e1a53-330">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="e1a53-331">Appelez `AddEnvironmentVariables` dernier pour autoriser les variables d’environnement avec le préfixe spécifié à substituer des valeurs d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="e1a53-331">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="e1a53-332">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e1a53-332">**Example**</span></span>

<span data-ttu-id="e1a53-333">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-333">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="e1a53-334">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-334">Run the sample app.</span></span> <span data-ttu-id="e1a53-335">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-335">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e1a53-336">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-336">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="e1a53-337">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="e1a53-337">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="e1a53-338">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-338">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="e1a53-339">Consultez le fichier *Pages/Index.cshtml.cs* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-339">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="e1a53-340">Pour exposer toutes les variables d’environnement disponibles pour l’application, remplacez la `FilteredConfiguration` dans *pages/index. cshtml. cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="e1a53-340">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="e1a53-341">Préfixes</span><span class="sxs-lookup"><span data-stu-id="e1a53-341">Prefixes</span></span>

<span data-ttu-id="e1a53-342">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe à la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-342">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="e1a53-343">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-343">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="e1a53-344">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="e1a53-344">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="e1a53-345">Lorsque le générateur d’hôte est créé, la configuration de l’hôte est fournie par des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-345">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="e1a53-346">Pour plus d’informations sur le préfixe utilisé pour ces variables d’environnement, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e1a53-346">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e1a53-347">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="e1a53-347">**Connection string prefixes**</span></span>

<span data-ttu-id="e1a53-348">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-348">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="e1a53-349">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-349">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="e1a53-350">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="e1a53-350">Connection string prefix</span></span> | <span data-ttu-id="e1a53-351">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="e1a53-351">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="e1a53-352">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="e1a53-352">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="e1a53-353">MySQL</span><span class="sxs-lookup"><span data-stu-id="e1a53-353">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="e1a53-354">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e1a53-354">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="e1a53-355">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e1a53-355">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="e1a53-356">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="e1a53-356">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="e1a53-357">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="e1a53-357">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="e1a53-358">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="e1a53-358">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="e1a53-359">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="e1a53-359">Environment variable key</span></span> | <span data-ttu-id="e1a53-360">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="e1a53-360">Converted configuration key</span></span> | <span data-ttu-id="e1a53-361">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="e1a53-361">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e1a53-362">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="e1a53-362">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e1a53-363">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e1a53-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e1a53-364">Valeur : `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="e1a53-364">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e1a53-365">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e1a53-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e1a53-366">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e1a53-366">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e1a53-367">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e1a53-367">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e1a53-368">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e1a53-368">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="e1a53-369">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="e1a53-369">File Configuration Provider</span></span>

<span data-ttu-id="e1a53-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="e1a53-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="e1a53-371">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="e1a53-371">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="e1a53-372">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="e1a53-372">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="e1a53-373">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="e1a53-373">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="e1a53-374">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="e1a53-374">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="e1a53-375">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="e1a53-375">INI Configuration Provider</span></span>

<span data-ttu-id="e1a53-376"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e1a53-376">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="e1a53-377">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-377">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e1a53-378">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="e1a53-378">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="e1a53-379">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e1a53-379">Overloads permit specifying:</span></span>

* <span data-ttu-id="e1a53-380">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e1a53-380">Whether the file is optional.</span></span>
* <span data-ttu-id="e1a53-381">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e1a53-381">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e1a53-382">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-382">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e1a53-383">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="e1a53-383">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e1a53-384">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="e1a53-384">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="e1a53-385">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e1a53-385">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e1a53-386">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-386">section0:key0</span></span>
* <span data-ttu-id="e1a53-387">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-387">section0:key1</span></span>
* <span data-ttu-id="e1a53-388">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="e1a53-388">section1:subsection:key</span></span>
* <span data-ttu-id="e1a53-389">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="e1a53-389">section2:subsection0:key</span></span>
* <span data-ttu-id="e1a53-390">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="e1a53-390">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="e1a53-391">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="e1a53-391">JSON Configuration Provider</span></span>

<span data-ttu-id="e1a53-392"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e1a53-392">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="e1a53-393">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-393">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e1a53-394">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e1a53-394">Overloads permit specifying:</span></span>

* <span data-ttu-id="e1a53-395">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e1a53-395">Whether the file is optional.</span></span>
* <span data-ttu-id="e1a53-396">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e1a53-396">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e1a53-397">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-397">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e1a53-398">`AddJsonFile` est appelé automatiquement deux fois lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-398">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e1a53-399">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="e1a53-399">The method is called to load configuration from:</span></span>

* <span data-ttu-id="e1a53-400">*appSettings. json* &ndash; ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-400">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="e1a53-401">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-401">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="e1a53-402">*appSettings. {Environment}. JSON* &ndash; la version de l’environnement du fichier est chargée à partir de [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="e1a53-402">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="e1a53-403">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e1a53-403">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e1a53-404">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e1a53-404">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e1a53-405">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-405">Environment variables.</span></span>
* <span data-ttu-id="e1a53-406">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-406">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e1a53-407">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e1a53-407">Command-line arguments.</span></span>

<span data-ttu-id="e1a53-408">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-408">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="e1a53-409">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-409">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="e1a53-410">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-410">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e1a53-411">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e1a53-411">**Example**</span></span>

<span data-ttu-id="e1a53-412">L’exemple d’application tire parti de la méthode de commodité statique `CreateDefaultBuilder` pour créer l’hôte, ce qui comprend deux appels à `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="e1a53-412">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="e1a53-413">Le premier appel à `AddJsonFile` charge la configuration à partir de *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e1a53-413">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="e1a53-414">Le deuxième appel à `AddJsonFile` charge la configuration à partir de *appSettings. { Environnement}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-414">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="e1a53-415">Pour *appSettings. Development. JSON* dans l’exemple d’application, le fichier suivant est chargé :</span><span class="sxs-lookup"><span data-stu-id="e1a53-415">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="e1a53-416">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-416">Run the sample app.</span></span> <span data-ttu-id="e1a53-417">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-417">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e1a53-418">La sortie contient des paires clé-valeur pour la configuration en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-418">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="e1a53-419">Le niveau de journalisation de la `Logging:LogLevel:Default` de clé est `Debug` lors de l’exécution de l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-419">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="e1a53-420">Exécutez à nouveau l’exemple d’application dans l’environnement de production :</span><span class="sxs-lookup"><span data-stu-id="e1a53-420">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="e1a53-421">Ouvrez le fichier *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e1a53-421">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="e1a53-422">Dans le profil `ConfigurationSample`, remplacez la valeur de la variable d’environnement `ASPNETCORE_ENVIRONMENT` par `Production`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-422">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="e1a53-423">Enregistrez le fichier et exécutez l’application avec `dotnet run` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="e1a53-423">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="e1a53-424">Paramètres dans *appSettings. Development. JSON* ne remplace plus les paramètres dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-424">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="e1a53-425">Le niveau de journalisation de la clé `Logging:LogLevel:Default` est `Information`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-425">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="e1a53-426">Le premier appel à `AddJsonFile` charge la configuration à partir de *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e1a53-426">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="e1a53-427">Le deuxième appel à `AddJsonFile` charge la configuration à partir de *appSettings. { Environnement}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-427">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="e1a53-428">Pour *appSettings. Development. JSON* dans l’exemple d’application, le fichier suivant est chargé :</span><span class="sxs-lookup"><span data-stu-id="e1a53-428">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="e1a53-429">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-429">Run the sample app.</span></span> <span data-ttu-id="e1a53-430">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-430">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e1a53-431">La sortie contient des paires clé-valeur pour la configuration en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-431">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="e1a53-432">Le niveau de journalisation de la `Logging:LogLevel:Default` de clé est `Debug` lors de l’exécution de l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e1a53-432">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="e1a53-433">Exécutez à nouveau l’exemple d’application dans l’environnement de production :</span><span class="sxs-lookup"><span data-stu-id="e1a53-433">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="e1a53-434">Ouvrez le fichier *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e1a53-434">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="e1a53-435">Dans le profil `ConfigurationSample`, remplacez la valeur de la variable d’environnement `ASPNETCORE_ENVIRONMENT` par `Production`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-435">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="e1a53-436">Enregistrez le fichier et exécutez l’application avec `dotnet run` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="e1a53-436">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="e1a53-437">Paramètres dans *appSettings. Development. JSON* ne remplace plus les paramètres dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-437">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="e1a53-438">Le niveau de journalisation de la clé `Logging:LogLevel:Default` est `Warning`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-438">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

::: moniker-end

### <a name="xml-configuration-provider"></a><span data-ttu-id="e1a53-439">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="e1a53-439">XML Configuration Provider</span></span>

<span data-ttu-id="e1a53-440"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e1a53-440">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="e1a53-441">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-441">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e1a53-442">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e1a53-442">Overloads permit specifying:</span></span>

* <span data-ttu-id="e1a53-443">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e1a53-443">Whether the file is optional.</span></span>
* <span data-ttu-id="e1a53-444">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e1a53-444">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e1a53-445">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-445">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e1a53-446">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="e1a53-446">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="e1a53-447">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-447">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="e1a53-448">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="e1a53-448">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e1a53-449">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="e1a53-449">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="e1a53-450">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e1a53-450">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e1a53-451">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-451">section0:key0</span></span>
* <span data-ttu-id="e1a53-452">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-452">section0:key1</span></span>
* <span data-ttu-id="e1a53-453">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-453">section1:key0</span></span>
* <span data-ttu-id="e1a53-454">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-454">section1:key1</span></span>

<span data-ttu-id="e1a53-455">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="e1a53-455">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="e1a53-456">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e1a53-456">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e1a53-457">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-457">section:section0:key:key0</span></span>
* <span data-ttu-id="e1a53-458">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-458">section:section0:key:key1</span></span>
* <span data-ttu-id="e1a53-459">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-459">section:section1:key:key0</span></span>
* <span data-ttu-id="e1a53-460">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-460">section:section1:key:key1</span></span>

<span data-ttu-id="e1a53-461">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="e1a53-461">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="e1a53-462">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e1a53-462">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e1a53-463">key:attribute</span><span class="sxs-lookup"><span data-stu-id="e1a53-463">key:attribute</span></span>
* <span data-ttu-id="e1a53-464">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="e1a53-464">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="e1a53-465">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="e1a53-465">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="e1a53-466">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-466">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="e1a53-467">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-467">The key is the file name.</span></span> <span data-ttu-id="e1a53-468">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="e1a53-468">The value contains the file's contents.</span></span> <span data-ttu-id="e1a53-469">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="e1a53-469">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="e1a53-470">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-470">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="e1a53-471">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="e1a53-471">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="e1a53-472">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e1a53-472">Overloads permit specifying:</span></span>

* <span data-ttu-id="e1a53-473">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="e1a53-473">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="e1a53-474">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="e1a53-474">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="e1a53-475">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="e1a53-475">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="e1a53-476">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-476">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="e1a53-477">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="e1a53-477">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="e1a53-478">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="e1a53-478">Memory Configuration Provider</span></span>

<span data-ttu-id="e1a53-479">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-479">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="e1a53-480">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-480">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e1a53-481">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-481">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="e1a53-482">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-482">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="e1a53-483">Dans l’exemple suivant, un dictionnaire de configuration est créé :</span><span class="sxs-lookup"><span data-stu-id="e1a53-483">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="e1a53-484">Le dictionnaire est utilisé avec un appel à `AddInMemoryCollection` pour fournir la configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-484">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="e1a53-485">GetValue</span><span class="sxs-lookup"><span data-stu-id="e1a53-485">GetValue</span></span>

<span data-ttu-id="e1a53-486">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur unique de la configuration avec une clé spécifiée et la convertit en type non collection spécifié.</span><span class="sxs-lookup"><span data-stu-id="e1a53-486">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="e1a53-487">Une surcharge accepte une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="e1a53-487">An overload accepts a default value.</span></span>

<span data-ttu-id="e1a53-488">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="e1a53-488">The following example:</span></span>

* <span data-ttu-id="e1a53-489">Extrait la valeur de chaîne de la configuration avec la clé `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-489">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="e1a53-490">Si `NumberKey` est introuvable dans les clés de configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="e1a53-490">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="e1a53-491">Tape la valeur comme `int`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-491">Types the value as an `int`.</span></span>
* <span data-ttu-id="e1a53-492">Stocke la valeur dans la propriété `NumberConfig` pour une utilisation par la page.</span><span class="sxs-lookup"><span data-stu-id="e1a53-492">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="e1a53-493">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="e1a53-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="e1a53-494">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="e1a53-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="e1a53-495">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="e1a53-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="e1a53-496">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="e1a53-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-497">section0:key0</span></span>
* <span data-ttu-id="e1a53-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-498">section0:key1</span></span>
* <span data-ttu-id="e1a53-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-499">section1:key0</span></span>
* <span data-ttu-id="e1a53-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-500">section1:key1</span></span>
* <span data-ttu-id="e1a53-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="e1a53-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="e1a53-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="e1a53-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="e1a53-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="e1a53-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="e1a53-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="e1a53-505">GetSection</span></span>

<span data-ttu-id="e1a53-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="e1a53-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="e1a53-507">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="e1a53-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="e1a53-508">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="e1a53-508">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="e1a53-509">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="e1a53-509">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="e1a53-510">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-510">`GetSection` never returns `null`.</span></span> <span data-ttu-id="e1a53-511">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="e1a53-511">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="e1a53-512">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="e1a53-512">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="e1a53-513"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="e1a53-513">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="e1a53-514">GetChildren</span><span class="sxs-lookup"><span data-stu-id="e1a53-514">GetChildren</span></span>

<span data-ttu-id="e1a53-515">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="e1a53-515">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="e1a53-516">Existe</span><span class="sxs-lookup"><span data-stu-id="e1a53-516">Exists</span></span>

<span data-ttu-id="e1a53-517">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="e1a53-517">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="e1a53-518">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-518">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="e1a53-519">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="e1a53-519">Bind to a class</span></span>

<span data-ttu-id="e1a53-520">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="e1a53-520">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="e1a53-521">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-521">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e1a53-522">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="e1a53-522">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="e1a53-523">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="e1a53-523">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e1a53-524">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-524">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="e1a53-525">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="e1a53-525">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="e1a53-526">Clé</span><span class="sxs-lookup"><span data-stu-id="e1a53-526">Key</span></span>                   | <span data-ttu-id="e1a53-527">Value</span><span class="sxs-lookup"><span data-stu-id="e1a53-527">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="e1a53-528">starship:name</span><span class="sxs-lookup"><span data-stu-id="e1a53-528">starship:name</span></span>         | <span data-ttu-id="e1a53-529">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="e1a53-529">USS Enterprise</span></span>                                    |
| <span data-ttu-id="e1a53-530">starship:registry</span><span class="sxs-lookup"><span data-stu-id="e1a53-530">starship:registry</span></span>     | <span data-ttu-id="e1a53-531">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="e1a53-531">NCC-1701</span></span>                                          |
| <span data-ttu-id="e1a53-532">starship:class</span><span class="sxs-lookup"><span data-stu-id="e1a53-532">starship:class</span></span>        | <span data-ttu-id="e1a53-533">Constitution</span><span class="sxs-lookup"><span data-stu-id="e1a53-533">Constitution</span></span>                                      |
| <span data-ttu-id="e1a53-534">starship:length</span><span class="sxs-lookup"><span data-stu-id="e1a53-534">starship:length</span></span>       | <span data-ttu-id="e1a53-535">304.8</span><span class="sxs-lookup"><span data-stu-id="e1a53-535">304.8</span></span>                                             |
| <span data-ttu-id="e1a53-536">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="e1a53-536">starship:commissioned</span></span> | <span data-ttu-id="e1a53-537">False</span><span class="sxs-lookup"><span data-stu-id="e1a53-537">False</span></span>                                             |
| <span data-ttu-id="e1a53-538">trademark</span><span class="sxs-lookup"><span data-stu-id="e1a53-538">trademark</span></span>             | <span data-ttu-id="e1a53-539">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="e1a53-539">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="e1a53-540">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-540">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="e1a53-541">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="e1a53-541">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="e1a53-542">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-542">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="e1a53-543">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e1a53-543">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="e1a53-544">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="e1a53-544">Bind to an object graph</span></span>

<span data-ttu-id="e1a53-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="e1a53-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="e1a53-546">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="e1a53-546">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e1a53-547">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-547">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="e1a53-548">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-548">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="e1a53-549">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e1a53-549">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="e1a53-550">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="e1a53-550">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="e1a53-551">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-551">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="e1a53-552">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e1a53-552">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="e1a53-553">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="e1a53-553">Bind an array to a class</span></span>

<span data-ttu-id="e1a53-554">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="e1a53-554">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="e1a53-555"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-555">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e1a53-556">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer une liaison de tableau à un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="e1a53-556">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="e1a53-557">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="e1a53-557">Binding is provided by convention.</span></span> <span data-ttu-id="e1a53-558">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="e1a53-558">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="e1a53-559">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="e1a53-559">**In-memory array processing**</span></span>

<span data-ttu-id="e1a53-560">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e1a53-560">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="e1a53-561">Clé</span><span class="sxs-lookup"><span data-stu-id="e1a53-561">Key</span></span>             | <span data-ttu-id="e1a53-562">Value</span><span class="sxs-lookup"><span data-stu-id="e1a53-562">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e1a53-563">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="e1a53-563">array:entries:0</span></span> | <span data-ttu-id="e1a53-564">value0</span><span class="sxs-lookup"><span data-stu-id="e1a53-564">value0</span></span> |
| <span data-ttu-id="e1a53-565">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="e1a53-565">array:entries:1</span></span> | <span data-ttu-id="e1a53-566">valeur1</span><span class="sxs-lookup"><span data-stu-id="e1a53-566">value1</span></span> |
| <span data-ttu-id="e1a53-567">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="e1a53-567">array:entries:2</span></span> | <span data-ttu-id="e1a53-568">valeur2</span><span class="sxs-lookup"><span data-stu-id="e1a53-568">value2</span></span> |
| <span data-ttu-id="e1a53-569">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="e1a53-569">array:entries:4</span></span> | <span data-ttu-id="e1a53-570">value4</span><span class="sxs-lookup"><span data-stu-id="e1a53-570">value4</span></span> |
| <span data-ttu-id="e1a53-571">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="e1a53-571">array:entries:5</span></span> | <span data-ttu-id="e1a53-572">value5</span><span class="sxs-lookup"><span data-stu-id="e1a53-572">value5</span></span> |

<span data-ttu-id="e1a53-573">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="e1a53-573">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="e1a53-574">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="e1a53-574">The array skips a value for index &num;3.</span></span> <span data-ttu-id="e1a53-575">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="e1a53-575">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="e1a53-576">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="e1a53-576">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e1a53-577">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="e1a53-577">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="e1a53-578">La syntaxe [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="e1a53-578">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="e1a53-579">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-579">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="e1a53-580">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e1a53-580">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="e1a53-581">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e1a53-581">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="e1a53-582">0</span><span class="sxs-lookup"><span data-stu-id="e1a53-582">0</span></span>                            | <span data-ttu-id="e1a53-583">value0</span><span class="sxs-lookup"><span data-stu-id="e1a53-583">value0</span></span>                       |
| <span data-ttu-id="e1a53-584">1</span><span class="sxs-lookup"><span data-stu-id="e1a53-584">1</span></span>                            | <span data-ttu-id="e1a53-585">valeur1</span><span class="sxs-lookup"><span data-stu-id="e1a53-585">value1</span></span>                       |
| <span data-ttu-id="e1a53-586">2</span><span class="sxs-lookup"><span data-stu-id="e1a53-586">2</span></span>                            | <span data-ttu-id="e1a53-587">valeur2</span><span class="sxs-lookup"><span data-stu-id="e1a53-587">value2</span></span>                       |
| <span data-ttu-id="e1a53-588">3</span><span class="sxs-lookup"><span data-stu-id="e1a53-588">3</span></span>                            | <span data-ttu-id="e1a53-589">value4</span><span class="sxs-lookup"><span data-stu-id="e1a53-589">value4</span></span>                       |
| <span data-ttu-id="e1a53-590">4</span><span class="sxs-lookup"><span data-stu-id="e1a53-590">4</span></span>                            | <span data-ttu-id="e1a53-591">value5</span><span class="sxs-lookup"><span data-stu-id="e1a53-591">value5</span></span>                       |

<span data-ttu-id="e1a53-592">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-592">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="e1a53-593">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="e1a53-593">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="e1a53-594">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="e1a53-594">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="e1a53-595">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-595">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="e1a53-596">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-596">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="e1a53-597">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-597">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="e1a53-598">Dans `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="e1a53-598">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="e1a53-599">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-599">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="e1a53-600">Clé</span><span class="sxs-lookup"><span data-stu-id="e1a53-600">Key</span></span>             | <span data-ttu-id="e1a53-601">Value</span><span class="sxs-lookup"><span data-stu-id="e1a53-601">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e1a53-602">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="e1a53-602">array:entries:3</span></span> | <span data-ttu-id="e1a53-603">valeur3</span><span class="sxs-lookup"><span data-stu-id="e1a53-603">value3</span></span> |

<span data-ttu-id="e1a53-604">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="e1a53-604">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="e1a53-605">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e1a53-605">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="e1a53-606">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e1a53-606">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="e1a53-607">0</span><span class="sxs-lookup"><span data-stu-id="e1a53-607">0</span></span>                            | <span data-ttu-id="e1a53-608">value0</span><span class="sxs-lookup"><span data-stu-id="e1a53-608">value0</span></span>                       |
| <span data-ttu-id="e1a53-609">1</span><span class="sxs-lookup"><span data-stu-id="e1a53-609">1</span></span>                            | <span data-ttu-id="e1a53-610">valeur1</span><span class="sxs-lookup"><span data-stu-id="e1a53-610">value1</span></span>                       |
| <span data-ttu-id="e1a53-611">2</span><span class="sxs-lookup"><span data-stu-id="e1a53-611">2</span></span>                            | <span data-ttu-id="e1a53-612">valeur2</span><span class="sxs-lookup"><span data-stu-id="e1a53-612">value2</span></span>                       |
| <span data-ttu-id="e1a53-613">3</span><span class="sxs-lookup"><span data-stu-id="e1a53-613">3</span></span>                            | <span data-ttu-id="e1a53-614">valeur3</span><span class="sxs-lookup"><span data-stu-id="e1a53-614">value3</span></span>                       |
| <span data-ttu-id="e1a53-615">4</span><span class="sxs-lookup"><span data-stu-id="e1a53-615">4</span></span>                            | <span data-ttu-id="e1a53-616">value4</span><span class="sxs-lookup"><span data-stu-id="e1a53-616">value4</span></span>                       |
| <span data-ttu-id="e1a53-617">5</span><span class="sxs-lookup"><span data-stu-id="e1a53-617">5</span></span>                            | <span data-ttu-id="e1a53-618">value5</span><span class="sxs-lookup"><span data-stu-id="e1a53-618">value5</span></span>                       |

<span data-ttu-id="e1a53-619">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="e1a53-619">**JSON array processing**</span></span>

<span data-ttu-id="e1a53-620">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="e1a53-620">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="e1a53-621">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="e1a53-621">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="e1a53-622">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e1a53-622">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="e1a53-623">Clé</span><span class="sxs-lookup"><span data-stu-id="e1a53-623">Key</span></span>                     | <span data-ttu-id="e1a53-624">Value</span><span class="sxs-lookup"><span data-stu-id="e1a53-624">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="e1a53-625">json_array:key</span><span class="sxs-lookup"><span data-stu-id="e1a53-625">json_array:key</span></span>          | <span data-ttu-id="e1a53-626">valueA</span><span class="sxs-lookup"><span data-stu-id="e1a53-626">valueA</span></span> |
| <span data-ttu-id="e1a53-627">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="e1a53-627">json_array:subsection:0</span></span> | <span data-ttu-id="e1a53-628">valueB</span><span class="sxs-lookup"><span data-stu-id="e1a53-628">valueB</span></span> |
| <span data-ttu-id="e1a53-629">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="e1a53-629">json_array:subsection:1</span></span> | <span data-ttu-id="e1a53-630">valueC</span><span class="sxs-lookup"><span data-stu-id="e1a53-630">valueC</span></span> |
| <span data-ttu-id="e1a53-631">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="e1a53-631">json_array:subsection:2</span></span> | <span data-ttu-id="e1a53-632">valueD</span><span class="sxs-lookup"><span data-stu-id="e1a53-632">valueD</span></span> |

<span data-ttu-id="e1a53-633">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="e1a53-633">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e1a53-634">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-634">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="e1a53-635">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-635">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="e1a53-636">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e1a53-636">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="e1a53-637">Valeur `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e1a53-637">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="e1a53-638">0</span><span class="sxs-lookup"><span data-stu-id="e1a53-638">0</span></span>                                   | <span data-ttu-id="e1a53-639">valueB</span><span class="sxs-lookup"><span data-stu-id="e1a53-639">valueB</span></span>                              |
| <span data-ttu-id="e1a53-640">1</span><span class="sxs-lookup"><span data-stu-id="e1a53-640">1</span></span>                                   | <span data-ttu-id="e1a53-641">valueC</span><span class="sxs-lookup"><span data-stu-id="e1a53-641">valueC</span></span>                              |
| <span data-ttu-id="e1a53-642">2</span><span class="sxs-lookup"><span data-stu-id="e1a53-642">2</span></span>                                   | <span data-ttu-id="e1a53-643">valueD</span><span class="sxs-lookup"><span data-stu-id="e1a53-643">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="e1a53-644">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="e1a53-644">Custom configuration provider</span></span>

<span data-ttu-id="e1a53-645">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="e1a53-645">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="e1a53-646">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="e1a53-646">The provider has the following characteristics:</span></span>

* <span data-ttu-id="e1a53-647">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-647">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="e1a53-648">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e1a53-648">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="e1a53-649">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="e1a53-649">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="e1a53-650">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="e1a53-650">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="e1a53-651">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-651">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="e1a53-652">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e1a53-652">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e1a53-653">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-653">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="e1a53-654">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="e1a53-654">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="e1a53-655">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-655">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e1a53-656">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-656">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="e1a53-657">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-657">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="e1a53-658">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-658">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="e1a53-659">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="e1a53-659">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="e1a53-660">Étant donné que les [clés de configuration ne](#keys)respectent pas la casse, le dictionnaire utilisé pour initialiser la base de données est créé avec le comparateur ne respectant pas la casse ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="e1a53-660">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="e1a53-661">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-661">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="e1a53-662">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-662">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="e1a53-663">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-663">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="e1a53-664">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-664">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e1a53-665">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-665">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="e1a53-666">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="e1a53-666">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="e1a53-667">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-667">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e1a53-668">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-668">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="e1a53-669">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-669">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="e1a53-670">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-670">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="e1a53-671">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="e1a53-671">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="e1a53-672">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-672">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="e1a53-673">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-673">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="e1a53-674">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-674">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="e1a53-675">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="e1a53-675">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="e1a53-676">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="e1a53-676">Access configuration during startup</span></span>

<span data-ttu-id="e1a53-677">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e1a53-677">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e1a53-678">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="e1a53-678">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="e1a53-679">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="e1a53-679">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="e1a53-680">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="e1a53-680">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="e1a53-681">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="e1a53-681">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="e1a53-682">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="e1a53-682">In a Razor Pages page:</span></span>

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

<span data-ttu-id="e1a53-683">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="e1a53-683">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="e1a53-684">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="e1a53-684">Add configuration from an external assembly</span></span>

<span data-ttu-id="e1a53-685">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="e1a53-685">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="e1a53-686">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e1a53-686">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1a53-687">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e1a53-687">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
