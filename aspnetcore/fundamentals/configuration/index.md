---
title: Configuration dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 9f0ad2791e504a0ff46daad07054b6bf909a546a
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634074"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="e2c4a-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2c4a-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="e2c4a-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e2c4a-105">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="e2c4a-106">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="e2c4a-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e2c4a-107">Azure Key Vault</span></span>
* <span data-ttu-id="e2c4a-108">Configuration de Azure App</span><span class="sxs-lookup"><span data-stu-id="e2c4a-108">Azure App Configuration</span></span>
* <span data-ttu-id="e2c4a-109">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e2c4a-109">Command-line arguments</span></span>
* <span data-ttu-id="e2c4a-110">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e2c4a-111">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="e2c4a-111">Directory files</span></span>
* <span data-ttu-id="e2c4a-112">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e2c4a-112">Environment variables</span></span>
* <span data-ttu-id="e2c4a-113">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="e2c4a-113">In-memory .NET objects</span></span>
* <span data-ttu-id="e2c4a-114">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="e2c4a-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e2c4a-115">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le framework.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e2c4a-116">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="e2c4a-117">Les exemples de code qui suivent et dans l’échantillon d’application utilisent l’espace de noms <xref:Microsoft.Extensions.Configuration> :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="e2c4a-118">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="e2c4a-119">Les options utilisent des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="e2c4a-120">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e2c4a-121">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2c4a-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="e2c4a-122">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="e2c4a-122">Host versus app configuration</span></span>

<span data-ttu-id="e2c4a-123">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="e2c4a-124">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e2c4a-125">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="e2c4a-126">Les paires clé-valeur de la configuration de l’hôte sont également incluses dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="e2c4a-127">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="e2c4a-128">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="e2c4a-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e2c4a-129">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="e2c4a-130">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="e2c4a-131">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="e2c4a-132">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte Web](xref:fundamentals/host/web-host), consultez la [version ASP.NET Core 2.2. de cette rubrique](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="e2c4a-133">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="e2c4a-134">Des variables d’environnement préfixées avec `DOTNET_` (par exemple, `DOTNET_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="e2c4a-135">Le préfixe (`DOTNET_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="e2c4a-136">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="e2c4a-137">La configuration par défaut de l’hôte Web est établie (`ConfigureWebHostDefaults`) :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="e2c4a-138">Kestrel est utilisé comme serveur web et configuré à l’aide des fournisseurs de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="e2c4a-139">Ajoutez l’intergiciel de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="e2c4a-140">Ajoutez l’intergiciel d’en-têtes transférés si la variable d'environnement `ASPNETCORE_FORWARDEDHEADERS_ENABLED` est définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="e2c4a-141">Activez l’intégration d’IIS.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-141">Enable IIS integration.</span></span>
* <span data-ttu-id="e2c4a-142">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="e2c4a-143">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e2c4a-144">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e2c4a-145">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="e2c4a-146">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="e2c4a-147">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e2c4a-148">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="e2c4a-149">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="e2c4a-150">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="e2c4a-151">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte générique](xref:fundamentals/host/generic-host), consultez la [dernière version de cette rubrique](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="e2c4a-152">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="e2c4a-153">Des variables d’environnement préfixées avec `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="e2c4a-154">Le préfixe (`ASPNETCORE_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="e2c4a-155">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="e2c4a-156">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="e2c4a-157">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e2c4a-158">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="e2c4a-159">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="e2c4a-160">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="e2c4a-161">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="e2c4a-162">Sécurité</span><span class="sxs-lookup"><span data-stu-id="e2c4a-162">Security</span></span>

<span data-ttu-id="e2c4a-163">Adoptez les pratiques suivantes pour sécuriser les données de configuration sensibles :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="e2c4a-164">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="e2c4a-165">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="e2c4a-166">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="e2c4a-167">Pour plus d’informations, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="e2c4a-168"><xref:security/app-secrets> &ndash; inclut des conseils sur l’utilisation de variables d’environnement pour stocker des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="e2c4a-169">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="e2c4a-170">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="e2c4a-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stocke en toute sécurité des secrets d’application pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="e2c4a-172">Pour plus d'informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="e2c4a-173">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="e2c4a-173">Hierarchical configuration data</span></span>

<span data-ttu-id="e2c4a-174">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="e2c4a-175">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="e2c4a-176">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="e2c4a-177">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="e2c4a-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-178">section0:key0</span></span>
* <span data-ttu-id="e2c4a-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-179">section0:key1</span></span>
* <span data-ttu-id="e2c4a-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-180">section1:key0</span></span>
* <span data-ttu-id="e2c4a-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-181">section1:key1</span></span>

<span data-ttu-id="e2c4a-182">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="e2c4a-183">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="e2c4a-184">Conventions</span><span class="sxs-lookup"><span data-stu-id="e2c4a-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="e2c4a-185">Sources et fournisseurs</span><span class="sxs-lookup"><span data-stu-id="e2c4a-185">Sources and providers</span></span>

<span data-ttu-id="e2c4a-186">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="e2c4a-187">Les fournisseurs de configuration qui implémentent la détection des modifications peuvent recharger la configuration lorsqu’un paramètre sous-jacent est modifié.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="e2c4a-188">Par exemple, le fournisseur de configuration de fichier (décrit plus loin dans cette rubrique) et le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) implémentent la détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="e2c4a-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e2c4a-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> peut être injecté dans une Razor Page <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> pour obtenir la configuration de la classe :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

<span data-ttu-id="e2c4a-191">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="e2c4a-192">Touches</span><span class="sxs-lookup"><span data-stu-id="e2c4a-192">Keys</span></span>

<span data-ttu-id="e2c4a-193">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="e2c4a-194">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-194">Keys are case-insensitive.</span></span> <span data-ttu-id="e2c4a-195">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="e2c4a-196">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="e2c4a-197">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="e2c4a-197">Hierarchical keys</span></span>
  * <span data-ttu-id="e2c4a-198">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="e2c4a-199">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="e2c4a-200">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et automatiquement transformé en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="e2c4a-201">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="e2c4a-202">Vous devez fournir du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="e2c4a-203"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e2c4a-204">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="e2c4a-205">Valeurs</span><span class="sxs-lookup"><span data-stu-id="e2c4a-205">Values</span></span>

<span data-ttu-id="e2c4a-206">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="e2c4a-207">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-207">Values are strings.</span></span>
* <span data-ttu-id="e2c4a-208">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="e2c4a-209">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="e2c4a-209">Providers</span></span>

<span data-ttu-id="e2c4a-210">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="e2c4a-211">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-211">Provider</span></span> | <span data-ttu-id="e2c4a-212">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="e2c4a-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="e2c4a-213">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="e2c4a-214">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e2c4a-214">Azure Key Vault</span></span> |
| <span data-ttu-id="e2c4a-215">[Fournisseur Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentation Azure)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="e2c4a-216">Configuration de Azure App</span><span class="sxs-lookup"><span data-stu-id="e2c4a-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="e2c4a-217">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e2c4a-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e2c4a-218">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e2c4a-218">Command-line parameters</span></span> |
| [<span data-ttu-id="e2c4a-219">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="e2c4a-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e2c4a-220">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="e2c4a-220">Custom source</span></span> |
| [<span data-ttu-id="e2c4a-221">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e2c4a-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e2c4a-222">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e2c4a-222">Environment variables</span></span> |
| [<span data-ttu-id="e2c4a-223">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="e2c4a-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e2c4a-224">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e2c4a-225">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="e2c4a-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="e2c4a-226">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="e2c4a-226">Directory files</span></span> |
| [<span data-ttu-id="e2c4a-227">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="e2c4a-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e2c4a-228">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="e2c4a-228">In-memory collections</span></span> |
| <span data-ttu-id="e2c4a-229">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e2c4a-230">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-230">File in the user profile directory</span></span> |

<span data-ttu-id="e2c4a-231">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="e2c4a-232">Les fournisseurs de configuration décrits dans cette rubrique sont décrits dans l’ordre alphabétique, et non dans l’ordre où votre code peut les organiser.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="e2c4a-233">Organisez les fournisseurs de configuration dans votre code en fonction de vos priorités pour les sources de configuration sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="e2c4a-234">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="e2c4a-235">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="e2c4a-236">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e2c4a-236">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="e2c4a-237">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="e2c4a-238">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e2c4a-238">Environment variables</span></span>
1. <span data-ttu-id="e2c4a-239">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e2c4a-239">Command-line arguments</span></span>

<span data-ttu-id="e2c4a-240">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="e2c4a-241">La séquence de fournisseurs précédente est utilisée lorsque vous initialisez un nouveau générateur d’hôte avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e2c4a-242">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="e2c4a-243">Configurer le générateur d’ordinateur hôte avec ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="e2c4a-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="e2c4a-244">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> et fournissez la configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="e2c4a-245">`ConfigureHostConfiguration` est utilisé pour initialiser <xref:Microsoft.Extensions.Hosting.IHostEnvironment> pour une utilisation ultérieure dans le processus de génération.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="e2c4a-246">`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="e2c4a-247">Configurer le générateur d’ordinateur hôte avec UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="e2c4a-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="e2c4a-248">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> sur le générateur d’hôte avec la configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="e2c4a-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="e2c4a-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="e2c4a-250">Appelez `ConfigureAppConfiguration` quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="e2c4a-251">Remplacez la configuration précédente par des arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e2c4a-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="e2c4a-252">Pour fournir une configuration d’application pouvant être remplacée par des arguments de ligne de commande, appelez `AddCommandLine` en dernier :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="e2c4a-253">Supprimer les fournisseurs ajoutés par CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="e2c4a-253">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="e2c4a-254">Pour supprimer les fournisseurs ajoutés par `CreateDefaultBuilder`, appelez d’abord [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) sur [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-254">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="e2c4a-255">Utiliser la configuration lors du démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="e2c4a-255">Consume configuration during app startup</span></span>

<span data-ttu-id="e2c4a-256">La configuration fournie à l’application dans `ConfigureAppConfiguration` est disponible lors du démarrage de l’application, notamment `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-256">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e2c4a-257">Pour plus d’informations, consultez la section [Accéder à la configuration lors du démarrage](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-257">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="e2c4a-258">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e2c4a-258">Command-line Configuration Provider</span></span>

<span data-ttu-id="e2c4a-259"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-259">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="e2c4a-260">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-260">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e2c4a-261">`AddCommandLine` est appelé automatiquement quand `CreateDefaultBuilder(string [])` est appelé.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-261">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="e2c4a-262">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-262">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e2c4a-263">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-263">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e2c4a-264">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-264">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="e2c4a-265">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-265">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e2c4a-266">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-266">Environment variables.</span></span>

<span data-ttu-id="e2c4a-267">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-267">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="e2c4a-268">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-268">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="e2c4a-269">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-269">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="e2c4a-270">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-270">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="e2c4a-271">Pour les applications basées sur les modèles ASP.NET Core, `AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-271">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e2c4a-272">Pour ajouter des fournisseurs de configuration supplémentaires et conserver la capacité de remplacer leur configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-272">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="e2c4a-273">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e2c4a-273">**Example**</span></span>

<span data-ttu-id="e2c4a-274">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-274">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="e2c4a-275">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-275">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="e2c4a-276">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-276">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="e2c4a-277">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-277">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e2c4a-278">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-278">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="e2c4a-279">Arguments</span><span class="sxs-lookup"><span data-stu-id="e2c4a-279">Arguments</span></span>

<span data-ttu-id="e2c4a-280">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-280">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="e2c4a-281">La valeur n’est pas requise si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-281">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="e2c4a-282">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="e2c4a-282">Key prefix</span></span>               | <span data-ttu-id="e2c4a-283">Exemple</span><span class="sxs-lookup"><span data-stu-id="e2c4a-283">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="e2c4a-284">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="e2c4a-284">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="e2c4a-285">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-285">Two dashes (`--`)</span></span>        | <span data-ttu-id="e2c4a-286">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-286">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="e2c4a-287">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="e2c4a-287">Forward slash (`/`)</span></span>      | <span data-ttu-id="e2c4a-288">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-288">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="e2c4a-289">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-289">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="e2c4a-290">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-290">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="e2c4a-291">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-291">Switch mappings</span></span>

<span data-ttu-id="e2c4a-292">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-292">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="e2c4a-293">Lorsque vous générez manuellement la configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, vous pouvez fournir un dictionnaire des remplacements de commutateur à la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-293">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="e2c4a-294">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-294">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e2c4a-295">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-295">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="e2c4a-296">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-296">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e2c4a-297">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-297">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e2c4a-298">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-298">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e2c4a-299">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-299">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="e2c4a-300">Créez un dictionnaire des mappages de commutateurs.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-300">Create a switch mappings dictionary.</span></span> <span data-ttu-id="e2c4a-301">Dans l’exemple suivant, deux mappages de commutateurs sont créés :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-301">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="e2c4a-302">Lorsque l’hôte est généré, appelez `AddCommandLine` avec le dictionnaire de mappages de commutateurs :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-302">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="e2c4a-303">Pour les applications qui utilisent des mappages de commutateurs, l’appel à `CreateDefaultBuilder` ne doit pas passer d’arguments.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-303">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="e2c4a-304">L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-304">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e2c4a-305">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-305">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="e2c4a-306">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-306">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="e2c4a-307">Touche</span><span class="sxs-lookup"><span data-stu-id="e2c4a-307">Key</span></span>       | <span data-ttu-id="e2c4a-308">valeur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-308">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="e2c4a-309">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-309">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="e2c4a-310">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-310">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="e2c4a-311">Touche</span><span class="sxs-lookup"><span data-stu-id="e2c4a-311">Key</span></span>               | <span data-ttu-id="e2c4a-312">valeur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-312">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="e2c4a-313">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e2c4a-313">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="e2c4a-314"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-314">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="e2c4a-315">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-315">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="e2c4a-316">[Azure App Service](https://azure.microsoft.com/services/app-service/) vous permet de définir des variables d’environnement dans le portail Azure capables de remplacer la configuration d’application à l’aide du Fournisseur de configuration de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-316">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="e2c4a-317">Pour plus d’informations, consultez [Azure Apps : remplacer la configuration de l’application à l’aide du portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-317">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e2c4a-318">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `DOTNET_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte générique](xref:fundamentals/host/generic-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="e2c4a-319">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e2c4a-320">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `ASPNETCORE_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte web](xref:fundamentals/host/web-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-320">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="e2c4a-321">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-321">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="e2c4a-322">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-322">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e2c4a-323">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-323">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="e2c4a-324">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-324">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="e2c4a-325">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-325">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e2c4a-326">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e2c4a-326">Command-line arguments.</span></span>

<span data-ttu-id="e2c4a-327">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-327">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="e2c4a-328">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-328">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="e2c4a-329">Si vous devez fournir la configuration de l'application à partir de variables d'environnement supplémentaires, appelez les fournisseurs supplémentaires de l'application dans `ConfigureAppConfiguration` et appelez `AddEnvironmentVariables` avec le préfixe.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-329">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call additional providers here as needed.
    // Call AddEnvironmentVariables last if you need to allow
    // environment variables to override values from other 
    // providers.
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
}
```

<span data-ttu-id="e2c4a-330">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e2c4a-330">**Example**</span></span>

<span data-ttu-id="e2c4a-331">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-331">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="e2c4a-332">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-332">Run the sample app.</span></span> <span data-ttu-id="e2c4a-333">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-333">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e2c4a-334">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-334">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="e2c4a-335">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-335">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="e2c4a-336">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-336">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="e2c4a-337">Consultez le fichier *Pages/Index.cshtml.cs* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-337">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="e2c4a-338">Si vous souhaitez exposer toutes les variables d’environnement disponibles pour l’application, remplacez `FilteredConfiguration` dans *Pages/Index.cshtml.cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-338">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="e2c4a-339">Préfixes</span><span class="sxs-lookup"><span data-stu-id="e2c4a-339">Prefixes</span></span>

<span data-ttu-id="e2c4a-340">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe pour la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-340">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="e2c4a-341">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-341">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="e2c4a-342">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-342">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="e2c4a-343">Lorsque le générateur d’hôte est créé, la configuration de l’hôte est fournie par des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-343">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="e2c4a-344">Pour plus d’informations sur le préfixe utilisé pour ces variables d’environnement, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-344">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e2c4a-345">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="e2c4a-345">**Connection string prefixes**</span></span>

<span data-ttu-id="e2c4a-346">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-346">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="e2c4a-347">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-347">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="e2c4a-348">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="e2c4a-348">Connection string prefix</span></span> | <span data-ttu-id="e2c4a-349">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-349">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="e2c4a-350">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="e2c4a-350">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="e2c4a-351">MySQL</span><span class="sxs-lookup"><span data-stu-id="e2c4a-351">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="e2c4a-352">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e2c4a-352">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="e2c4a-353">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e2c4a-353">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="e2c4a-354">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-354">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="e2c4a-355">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-355">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="e2c4a-356">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-356">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="e2c4a-357">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="e2c4a-357">Environment variable key</span></span> | <span data-ttu-id="e2c4a-358">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="e2c4a-358">Converted configuration key</span></span> | <span data-ttu-id="e2c4a-359">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-359">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e2c4a-360">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-360">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e2c4a-361">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e2c4a-362">Valeur : `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-362">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e2c4a-363">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e2c4a-364">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-364">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e2c4a-365">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e2c4a-366">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-366">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="e2c4a-367">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="e2c4a-367">File Configuration Provider</span></span>

<span data-ttu-id="e2c4a-368"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-368"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="e2c4a-369">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-369">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="e2c4a-370">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="e2c4a-370">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="e2c4a-371">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="e2c4a-371">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="e2c4a-372">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="e2c4a-372">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="e2c4a-373">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="e2c4a-373">INI Configuration Provider</span></span>

<span data-ttu-id="e2c4a-374"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-374">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="e2c4a-375">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-375">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e2c4a-376">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-376">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="e2c4a-377">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-377">Overloads permit specifying:</span></span>

* <span data-ttu-id="e2c4a-378">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-378">Whether the file is optional.</span></span>
* <span data-ttu-id="e2c4a-379">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-379">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e2c4a-380">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-380">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e2c4a-381">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e2c4a-382">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-382">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="e2c4a-383">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-383">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e2c4a-384">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-384">section0:key0</span></span>
* <span data-ttu-id="e2c4a-385">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-385">section0:key1</span></span>
* <span data-ttu-id="e2c4a-386">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="e2c4a-386">section1:subsection:key</span></span>
* <span data-ttu-id="e2c4a-387">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="e2c4a-387">section2:subsection0:key</span></span>
* <span data-ttu-id="e2c4a-388">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="e2c4a-388">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="e2c4a-389">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="e2c4a-389">JSON Configuration Provider</span></span>

<span data-ttu-id="e2c4a-390"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-390">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="e2c4a-391">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-391">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e2c4a-392">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-392">Overloads permit specifying:</span></span>

* <span data-ttu-id="e2c4a-393">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-393">Whether the file is optional.</span></span>
* <span data-ttu-id="e2c4a-394">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-394">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e2c4a-395">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-395">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e2c4a-396">`AddJsonFile` est appelé automatiquement deux fois lorsque vous initialisez un nouveau générateur d’hôte avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-396">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e2c4a-397">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-397">The method is called to load configuration from:</span></span>

* <span data-ttu-id="e2c4a-398">*appSettings.JSON* &ndash; Ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-398">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="e2c4a-399">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-399">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="e2c4a-400">*appsettings.{Environment}.json* &ndash; La version de l’environnement du fichier est chargée à partir du fichier [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-400">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="e2c4a-401">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-401">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="e2c4a-402">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-402">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e2c4a-403">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-403">Environment variables.</span></span>
* <span data-ttu-id="e2c4a-404">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-404">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="e2c4a-405">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e2c4a-405">Command-line arguments.</span></span>

<span data-ttu-id="e2c4a-406">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-406">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="e2c4a-407">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-407">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="e2c4a-408">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-408">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e2c4a-409">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e2c4a-409">**Example**</span></span>

<span data-ttu-id="e2c4a-410">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut deux appels à `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-410">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="e2c4a-411">La configuration est chargée à partir d’*appsettings.json* et d’*appsettings{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-411">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="e2c4a-412">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-412">Run the sample app.</span></span> <span data-ttu-id="e2c4a-413">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-413">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e2c4a-414">Notez que la sortie contient des paires clé-valeur pour la configuration représentée dans le tableau en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-414">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="e2c4a-415">Les clés de configuration de la journalisation utilisent le signe deux-points (`:`) comme séparateur hiérarchique.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-415">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="e2c4a-416">Touche</span><span class="sxs-lookup"><span data-stu-id="e2c4a-416">Key</span></span>                        | <span data-ttu-id="e2c4a-417">Valeur de développement</span><span class="sxs-lookup"><span data-stu-id="e2c4a-417">Development Value</span></span> | <span data-ttu-id="e2c4a-418">Valeur de production</span><span class="sxs-lookup"><span data-stu-id="e2c4a-418">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="e2c4a-419">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="e2c4a-419">Logging:LogLevel:System</span></span>    | <span data-ttu-id="e2c4a-420">Informations</span><span class="sxs-lookup"><span data-stu-id="e2c4a-420">Information</span></span>       | <span data-ttu-id="e2c4a-421">Informations</span><span class="sxs-lookup"><span data-stu-id="e2c4a-421">Information</span></span>      |
| <span data-ttu-id="e2c4a-422">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="e2c4a-422">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="e2c4a-423">Informations</span><span class="sxs-lookup"><span data-stu-id="e2c4a-423">Information</span></span>       | <span data-ttu-id="e2c4a-424">Informations</span><span class="sxs-lookup"><span data-stu-id="e2c4a-424">Information</span></span>      |
| <span data-ttu-id="e2c4a-425">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="e2c4a-425">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="e2c4a-426">Débogage</span><span class="sxs-lookup"><span data-stu-id="e2c4a-426">Debug</span></span>             | <span data-ttu-id="e2c4a-427">Erreur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-427">Error</span></span>            |
| <span data-ttu-id="e2c4a-428">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="e2c4a-428">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="e2c4a-429">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="e2c4a-429">XML Configuration Provider</span></span>

<span data-ttu-id="e2c4a-430"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-430">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="e2c4a-431">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-431">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e2c4a-432">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-432">Overloads permit specifying:</span></span>

* <span data-ttu-id="e2c4a-433">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-433">Whether the file is optional.</span></span>
* <span data-ttu-id="e2c4a-434">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-434">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e2c4a-435">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-435">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e2c4a-436">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-436">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="e2c4a-437">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-437">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="e2c4a-438">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-438">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="e2c4a-439">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-439">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="e2c4a-440">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-440">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e2c4a-441">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-441">section0:key0</span></span>
* <span data-ttu-id="e2c4a-442">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-442">section0:key1</span></span>
* <span data-ttu-id="e2c4a-443">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-443">section1:key0</span></span>
* <span data-ttu-id="e2c4a-444">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-444">section1:key1</span></span>

<span data-ttu-id="e2c4a-445">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-445">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="e2c4a-446">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-446">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e2c4a-447">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-447">section:section0:key:key0</span></span>
* <span data-ttu-id="e2c4a-448">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-448">section:section0:key:key1</span></span>
* <span data-ttu-id="e2c4a-449">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-449">section:section1:key:key0</span></span>
* <span data-ttu-id="e2c4a-450">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-450">section:section1:key:key1</span></span>

<span data-ttu-id="e2c4a-451">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-451">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="e2c4a-452">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-452">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e2c4a-453">key:attribute</span><span class="sxs-lookup"><span data-stu-id="e2c4a-453">key:attribute</span></span>
* <span data-ttu-id="e2c4a-454">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="e2c4a-454">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="e2c4a-455">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="e2c4a-455">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="e2c4a-456">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-456">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="e2c4a-457">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-457">The key is the file name.</span></span> <span data-ttu-id="e2c4a-458">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-458">The value contains the file's contents.</span></span> <span data-ttu-id="e2c4a-459">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-459">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="e2c4a-460">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-460">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="e2c4a-461">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-461">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="e2c4a-462">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-462">Overloads permit specifying:</span></span>

* <span data-ttu-id="e2c4a-463">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-463">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="e2c4a-464">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-464">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="e2c4a-465">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-465">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="e2c4a-466">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-466">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="e2c4a-467">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-467">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="e2c4a-468">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="e2c4a-468">Memory Configuration Provider</span></span>

<span data-ttu-id="e2c4a-469">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-469">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="e2c4a-470">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-470">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e2c4a-471">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-471">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="e2c4a-472">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-472">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="e2c4a-473">Dans l’exemple suivant, un dictionnaire de configuration est créé :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-473">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="e2c4a-474">Le dictionnaire est utilisé avec un appel à `AddInMemoryCollection` pour fournir la configuration :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-474">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="e2c4a-475">GetValue</span><span class="sxs-lookup"><span data-stu-id="e2c4a-475">GetValue</span></span>

<span data-ttu-id="e2c4a-476">[ConfigurationBinder. GetValue \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur unique de la configuration avec une clé spécifiée et la convertit en type non collection spécifié.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-476">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="e2c4a-477">Une surcharge accepte une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-477">An overload accepts a default value.</span></span>

<span data-ttu-id="e2c4a-478">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-478">The following example:</span></span>

* <span data-ttu-id="e2c4a-479">Extrait la valeur de chaîne de la configuration avec la clé `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-479">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="e2c4a-480">Si `NumberKey` est introuvable dans les clés de configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-480">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="e2c4a-481">Tape la valeur comme `int`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-481">Types the value as an `int`.</span></span>
* <span data-ttu-id="e2c4a-482">Stocke la valeur dans la propriété `NumberConfig` pour une utilisation par la page.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-482">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="e2c4a-483">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="e2c4a-483">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="e2c4a-484">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-484">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="e2c4a-485">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-485">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="e2c4a-486">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-486">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="e2c4a-487">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-487">section0:key0</span></span>
* <span data-ttu-id="e2c4a-488">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-488">section0:key1</span></span>
* <span data-ttu-id="e2c4a-489">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-489">section1:key0</span></span>
* <span data-ttu-id="e2c4a-490">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-490">section1:key1</span></span>
* <span data-ttu-id="e2c4a-491">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-491">section2:subsection0:key0</span></span>
* <span data-ttu-id="e2c4a-492">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-492">section2:subsection0:key1</span></span>
* <span data-ttu-id="e2c4a-493">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-493">section2:subsection1:key0</span></span>
* <span data-ttu-id="e2c4a-494">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-494">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="e2c4a-495">GetSection</span><span class="sxs-lookup"><span data-stu-id="e2c4a-495">GetSection</span></span>

<span data-ttu-id="e2c4a-496">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-496">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="e2c4a-497">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-497">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="e2c4a-498">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-498">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="e2c4a-499">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-499">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="e2c4a-500">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-500">`GetSection` never returns `null`.</span></span> <span data-ttu-id="e2c4a-501">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-501">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="e2c4a-502">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-502">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="e2c4a-503"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-503">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="e2c4a-504">GetChildren</span><span class="sxs-lookup"><span data-stu-id="e2c4a-504">GetChildren</span></span>

<span data-ttu-id="e2c4a-505">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-505">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="e2c4a-506">Existe</span><span class="sxs-lookup"><span data-stu-id="e2c4a-506">Exists</span></span>

<span data-ttu-id="e2c4a-507">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-507">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="e2c4a-508">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-508">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="e2c4a-509">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="e2c4a-509">Bind to a class</span></span>

<span data-ttu-id="e2c4a-510">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-510">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="e2c4a-511">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-511">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e2c4a-512">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-512">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="e2c4a-513">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-513">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e2c4a-514">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-514">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="e2c4a-515">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-515">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="e2c4a-516">Touche</span><span class="sxs-lookup"><span data-stu-id="e2c4a-516">Key</span></span>                   | <span data-ttu-id="e2c4a-517">valeur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-517">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="e2c4a-518">starship:name</span><span class="sxs-lookup"><span data-stu-id="e2c4a-518">starship:name</span></span>         | <span data-ttu-id="e2c4a-519">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="e2c4a-519">USS Enterprise</span></span>                                    |
| <span data-ttu-id="e2c4a-520">starship:registry</span><span class="sxs-lookup"><span data-stu-id="e2c4a-520">starship:registry</span></span>     | <span data-ttu-id="e2c4a-521">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="e2c4a-521">NCC-1701</span></span>                                          |
| <span data-ttu-id="e2c4a-522">starship:class</span><span class="sxs-lookup"><span data-stu-id="e2c4a-522">starship:class</span></span>        | <span data-ttu-id="e2c4a-523">Constitution</span><span class="sxs-lookup"><span data-stu-id="e2c4a-523">Constitution</span></span>                                      |
| <span data-ttu-id="e2c4a-524">starship:length</span><span class="sxs-lookup"><span data-stu-id="e2c4a-524">starship:length</span></span>       | <span data-ttu-id="e2c4a-525">304.8</span><span class="sxs-lookup"><span data-stu-id="e2c4a-525">304.8</span></span>                                             |
| <span data-ttu-id="e2c4a-526">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="e2c4a-526">starship:commissioned</span></span> | <span data-ttu-id="e2c4a-527">False</span><span class="sxs-lookup"><span data-stu-id="e2c4a-527">False</span></span>                                             |
| <span data-ttu-id="e2c4a-528">trademark</span><span class="sxs-lookup"><span data-stu-id="e2c4a-528">trademark</span></span>             | <span data-ttu-id="e2c4a-529">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="e2c4a-529">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="e2c4a-530">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-530">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="e2c4a-531">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-531">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="e2c4a-532">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-532">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="e2c4a-533">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-533">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="e2c4a-534">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="e2c4a-534">Bind to an object graph</span></span>

<span data-ttu-id="e2c4a-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="e2c4a-536">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-536">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e2c4a-537">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-537">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="e2c4a-538">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-538">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="e2c4a-539">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-539">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="e2c4a-540">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-540">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="e2c4a-541">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-541">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="e2c4a-542">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-542">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="e2c4a-543">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="e2c4a-543">Bind an array to a class</span></span>

<span data-ttu-id="e2c4a-544">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="e2c4a-544">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="e2c4a-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-545">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e2c4a-546">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer la liaison de tableau avec un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-546">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c4a-547">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-547">Binding is provided by convention.</span></span> <span data-ttu-id="e2c4a-548">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-548">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="e2c4a-549">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="e2c4a-549">**In-memory array processing**</span></span>

<span data-ttu-id="e2c4a-550">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-550">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="e2c4a-551">Touche</span><span class="sxs-lookup"><span data-stu-id="e2c4a-551">Key</span></span>             | <span data-ttu-id="e2c4a-552">valeur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-552">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e2c4a-553">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-553">array:entries:0</span></span> | <span data-ttu-id="e2c4a-554">value0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-554">value0</span></span> |
| <span data-ttu-id="e2c4a-555">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-555">array:entries:1</span></span> | <span data-ttu-id="e2c4a-556">valeur1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-556">value1</span></span> |
| <span data-ttu-id="e2c4a-557">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="e2c4a-557">array:entries:2</span></span> | <span data-ttu-id="e2c4a-558">valeur2</span><span class="sxs-lookup"><span data-stu-id="e2c4a-558">value2</span></span> |
| <span data-ttu-id="e2c4a-559">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="e2c4a-559">array:entries:4</span></span> | <span data-ttu-id="e2c4a-560">value4</span><span class="sxs-lookup"><span data-stu-id="e2c4a-560">value4</span></span> |
| <span data-ttu-id="e2c4a-561">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="e2c4a-561">array:entries:5</span></span> | <span data-ttu-id="e2c4a-562">value5</span><span class="sxs-lookup"><span data-stu-id="e2c4a-562">value5</span></span> |

<span data-ttu-id="e2c4a-563">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-563">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="e2c4a-564">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-564">The array skips a value for index &num;3.</span></span> <span data-ttu-id="e2c4a-565">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-565">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="e2c4a-566">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-566">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e2c4a-567">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-567">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="e2c4a-568">La syntaxe [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-568">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="e2c4a-569">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-569">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="e2c4a-570">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-570">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="e2c4a-571">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-571">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="e2c4a-572">0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-572">0</span></span>                            | <span data-ttu-id="e2c4a-573">value0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-573">value0</span></span>                       |
| <span data-ttu-id="e2c4a-574">1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-574">1</span></span>                            | <span data-ttu-id="e2c4a-575">valeur1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-575">value1</span></span>                       |
| <span data-ttu-id="e2c4a-576">2</span><span class="sxs-lookup"><span data-stu-id="e2c4a-576">2</span></span>                            | <span data-ttu-id="e2c4a-577">valeur2</span><span class="sxs-lookup"><span data-stu-id="e2c4a-577">value2</span></span>                       |
| <span data-ttu-id="e2c4a-578">3</span><span class="sxs-lookup"><span data-stu-id="e2c4a-578">3</span></span>                            | <span data-ttu-id="e2c4a-579">value4</span><span class="sxs-lookup"><span data-stu-id="e2c4a-579">value4</span></span>                       |
| <span data-ttu-id="e2c4a-580">4</span><span class="sxs-lookup"><span data-stu-id="e2c4a-580">4</span></span>                            | <span data-ttu-id="e2c4a-581">value5</span><span class="sxs-lookup"><span data-stu-id="e2c4a-581">value5</span></span>                       |

<span data-ttu-id="e2c4a-582">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-582">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="e2c4a-583">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-583">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="e2c4a-584">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-584">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="e2c4a-585">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-585">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="e2c4a-586">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-586">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="e2c4a-587">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-587">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="e2c4a-588">Dans `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="e2c4a-588">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="e2c4a-589">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-589">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="e2c4a-590">Touche</span><span class="sxs-lookup"><span data-stu-id="e2c4a-590">Key</span></span>             | <span data-ttu-id="e2c4a-591">valeur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-591">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e2c4a-592">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="e2c4a-592">array:entries:3</span></span> | <span data-ttu-id="e2c4a-593">valeur3</span><span class="sxs-lookup"><span data-stu-id="e2c4a-593">value3</span></span> |

<span data-ttu-id="e2c4a-594">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-594">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="e2c4a-595">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-595">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="e2c4a-596">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-596">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="e2c4a-597">0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-597">0</span></span>                            | <span data-ttu-id="e2c4a-598">value0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-598">value0</span></span>                       |
| <span data-ttu-id="e2c4a-599">1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-599">1</span></span>                            | <span data-ttu-id="e2c4a-600">valeur1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-600">value1</span></span>                       |
| <span data-ttu-id="e2c4a-601">2</span><span class="sxs-lookup"><span data-stu-id="e2c4a-601">2</span></span>                            | <span data-ttu-id="e2c4a-602">valeur2</span><span class="sxs-lookup"><span data-stu-id="e2c4a-602">value2</span></span>                       |
| <span data-ttu-id="e2c4a-603">3</span><span class="sxs-lookup"><span data-stu-id="e2c4a-603">3</span></span>                            | <span data-ttu-id="e2c4a-604">valeur3</span><span class="sxs-lookup"><span data-stu-id="e2c4a-604">value3</span></span>                       |
| <span data-ttu-id="e2c4a-605">4</span><span class="sxs-lookup"><span data-stu-id="e2c4a-605">4</span></span>                            | <span data-ttu-id="e2c4a-606">value4</span><span class="sxs-lookup"><span data-stu-id="e2c4a-606">value4</span></span>                       |
| <span data-ttu-id="e2c4a-607">5</span><span class="sxs-lookup"><span data-stu-id="e2c4a-607">5</span></span>                            | <span data-ttu-id="e2c4a-608">value5</span><span class="sxs-lookup"><span data-stu-id="e2c4a-608">value5</span></span>                       |

<span data-ttu-id="e2c4a-609">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="e2c4a-609">**JSON array processing**</span></span>

<span data-ttu-id="e2c4a-610">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-610">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="e2c4a-611">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-611">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="e2c4a-612">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-612">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="e2c4a-613">Touche</span><span class="sxs-lookup"><span data-stu-id="e2c4a-613">Key</span></span>                     | <span data-ttu-id="e2c4a-614">valeur</span><span class="sxs-lookup"><span data-stu-id="e2c4a-614">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="e2c4a-615">json_array:key</span><span class="sxs-lookup"><span data-stu-id="e2c4a-615">json_array:key</span></span>          | <span data-ttu-id="e2c4a-616">valueA</span><span class="sxs-lookup"><span data-stu-id="e2c4a-616">valueA</span></span> |
| <span data-ttu-id="e2c4a-617">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-617">json_array:subsection:0</span></span> | <span data-ttu-id="e2c4a-618">valueB</span><span class="sxs-lookup"><span data-stu-id="e2c4a-618">valueB</span></span> |
| <span data-ttu-id="e2c4a-619">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-619">json_array:subsection:1</span></span> | <span data-ttu-id="e2c4a-620">valueC</span><span class="sxs-lookup"><span data-stu-id="e2c4a-620">valueC</span></span> |
| <span data-ttu-id="e2c4a-621">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="e2c4a-621">json_array:subsection:2</span></span> | <span data-ttu-id="e2c4a-622">valueD</span><span class="sxs-lookup"><span data-stu-id="e2c4a-622">valueD</span></span> |

<span data-ttu-id="e2c4a-623">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-623">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e2c4a-624">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-624">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="e2c4a-625">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-625">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="e2c4a-626">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-626">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="e2c4a-627">Valeur `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e2c4a-627">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="e2c4a-628">0</span><span class="sxs-lookup"><span data-stu-id="e2c4a-628">0</span></span>                                   | <span data-ttu-id="e2c4a-629">valueB</span><span class="sxs-lookup"><span data-stu-id="e2c4a-629">valueB</span></span>                              |
| <span data-ttu-id="e2c4a-630">1</span><span class="sxs-lookup"><span data-stu-id="e2c4a-630">1</span></span>                                   | <span data-ttu-id="e2c4a-631">valueC</span><span class="sxs-lookup"><span data-stu-id="e2c4a-631">valueC</span></span>                              |
| <span data-ttu-id="e2c4a-632">2</span><span class="sxs-lookup"><span data-stu-id="e2c4a-632">2</span></span>                                   | <span data-ttu-id="e2c4a-633">valueD</span><span class="sxs-lookup"><span data-stu-id="e2c4a-633">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="e2c4a-634">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="e2c4a-634">Custom configuration provider</span></span>

<span data-ttu-id="e2c4a-635">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-635">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="e2c4a-636">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-636">The provider has the following characteristics:</span></span>

* <span data-ttu-id="e2c4a-637">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-637">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="e2c4a-638">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-638">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="e2c4a-639">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-639">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="e2c4a-640">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-640">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="e2c4a-641">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-641">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="e2c4a-642">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-642">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e2c4a-643">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-643">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="e2c4a-644">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-644">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="e2c4a-645">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-645">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e2c4a-646">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-646">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="e2c4a-647">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-647">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="e2c4a-648">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-648">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="e2c4a-649">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-649">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="e2c4a-650">Étant donné que les [clés de configuration ne](#keys)respectent pas la casse, le dictionnaire utilisé pour initialiser la base de données est créé avec le comparateur ne respectant pas la casse ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-650">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="e2c4a-651">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-651">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="e2c4a-652">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-652">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="e2c4a-653">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-653">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="e2c4a-654">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-654">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e2c4a-655">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-655">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="e2c4a-656">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-656">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="e2c4a-657">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-657">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e2c4a-658">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-658">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="e2c4a-659">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-659">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="e2c4a-660">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-660">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="e2c4a-661">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-661">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="e2c4a-662">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-662">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="e2c4a-663">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-663">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="e2c4a-664">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-664">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="e2c4a-665">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-665">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="e2c4a-666">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="e2c4a-666">Access configuration during startup</span></span>

<span data-ttu-id="e2c4a-667">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-667">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e2c4a-668">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-668">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="e2c4a-669">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="e2c4a-669">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="e2c4a-670">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="e2c4a-670">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="e2c4a-671">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-671">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="e2c4a-672">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-672">In a Razor Pages page:</span></span>

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

<span data-ttu-id="e2c4a-673">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="e2c4a-673">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="e2c4a-674">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="e2c4a-674">Add configuration from an external assembly</span></span>

<span data-ttu-id="e2c4a-675">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-675">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="e2c4a-676">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e2c4a-676">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2c4a-677">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e2c4a-677">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
