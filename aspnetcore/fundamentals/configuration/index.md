---
title: Configuration dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: d0ef670aa0ac4960318f86ea7888b9eab71f17fd
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171899"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="0faaa-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0faaa-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="0faaa-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0faaa-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0faaa-105">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="0faaa-106">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="0faaa-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0faaa-107">Azure Key Vault</span></span>
* <span data-ttu-id="0faaa-108">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="0faaa-108">Azure App Configuration</span></span>
* <span data-ttu-id="0faaa-109">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-109">Command-line arguments</span></span>
* <span data-ttu-id="0faaa-110">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="0faaa-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="0faaa-111">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-111">Directory files</span></span>
* <span data-ttu-id="0faaa-112">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-112">Environment variables</span></span>
* <span data-ttu-id="0faaa-113">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-113">In-memory .NET objects</span></span>
* <span data-ttu-id="0faaa-114">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="0faaa-114">Settings files</span></span>

<span data-ttu-id="0faaa-115">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le framework.</span><span class="sxs-lookup"><span data-stu-id="0faaa-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

<span data-ttu-id="0faaa-116">Les exemples de code qui suivent et dans l’échantillon d’application utilisent l’espace de noms <xref:Microsoft.Extensions.Configuration> :</span><span class="sxs-lookup"><span data-stu-id="0faaa-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="0faaa-117">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="0faaa-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="0faaa-118">Les options utilisent des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-118">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="0faaa-119">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-119">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="0faaa-120">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0faaa-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="0faaa-121">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="0faaa-121">Host versus app configuration</span></span>

<span data-ttu-id="0faaa-122">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="0faaa-123">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="0faaa-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0faaa-124">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="0faaa-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="0faaa-125">Les paires clé-valeur de la configuration de l’hôte sont également incluses dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-125">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="0faaa-126">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="0faaa-127">Autre configuration</span><span class="sxs-lookup"><span data-stu-id="0faaa-127">Other configuration</span></span>

<span data-ttu-id="0faaa-128">Cette rubrique se rapporte uniquement à la configuration de l' *application*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-128">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="0faaa-129">D’autres aspects de l’exécution et de l’hébergement des applications ASP.NET Core sont configurés à l’aide des fichiers de configuration non traités dans cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="0faaa-129">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="0faaa-130">*Launch. json*/*launchSettings. JSON* sont des fichiers de configuration d’outils pour l’environnement de développement, décrits ci-après :</span><span class="sxs-lookup"><span data-stu-id="0faaa-130">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="0faaa-131">Dans <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-131">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="0faaa-132">Dans l’ensemble de la documentation dans lequel les fichiers sont utilisés pour configurer des applications ASP.NET Core pour les scénarios de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-132">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="0faaa-133">*Web. config* est un fichier de configuration de serveur, décrit dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-133">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="0faaa-134">Pour plus d’informations sur la migration de la configuration d’application à partir de versions antérieures de ASP.NET, consultez <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-134">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="0faaa-135">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="0faaa-135">Default configuration</span></span>

<span data-ttu-id="0faaa-136">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="0faaa-136">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="0faaa-137">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="0faaa-137">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="0faaa-138">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="0faaa-138">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="0faaa-139">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte Web](xref:fundamentals/host/web-host), consultez la [version ASP.NET Core 2.2. de cette rubrique](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="0faaa-139">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="0faaa-140">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0faaa-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="0faaa-141">Des variables d’environnement préfixées avec `DOTNET_` (par exemple, `DOTNET_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-141">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="0faaa-142">Le préfixe (`DOTNET_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-142">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="0faaa-143">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-143">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="0faaa-144">La configuration par défaut de l’hôte Web est établie (`ConfigureWebHostDefaults`) :</span><span class="sxs-lookup"><span data-stu-id="0faaa-144">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="0faaa-145">Kestrel est utilisé comme serveur web et configuré à l’aide des fournisseurs de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-145">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="0faaa-146">Ajoutez l’intergiciel de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-146">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="0faaa-147">Ajoutez l’intergiciel d’en-têtes transférés si la variable d'environnement `ASPNETCORE_FORWARDEDHEADERS_ENABLED` est définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-147">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="0faaa-148">Activez l’intégration d’IIS.</span><span class="sxs-lookup"><span data-stu-id="0faaa-148">Enable IIS integration.</span></span>
* <span data-ttu-id="0faaa-149">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0faaa-149">App configuration is provided from:</span></span>
  * <span data-ttu-id="0faaa-150">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-150">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="0faaa-151">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-151">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="0faaa-152">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-152">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="0faaa-153">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-153">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="0faaa-154">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-154">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="0faaa-155">Sécurité</span><span class="sxs-lookup"><span data-stu-id="0faaa-155">Security</span></span>

<span data-ttu-id="0faaa-156">Adoptez les pratiques suivantes pour sécuriser les données de configuration sensibles :</span><span class="sxs-lookup"><span data-stu-id="0faaa-156">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="0faaa-157">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="0faaa-157">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="0faaa-158">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="0faaa-158">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="0faaa-159">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="0faaa-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="0faaa-160">Pour plus d'informations, voir les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-160">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="0faaa-161"><xref:security/app-secrets> &ndash; fournit des conseils sur l’utilisation de variables d’environnement pour stocker des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="0faaa-161"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="0faaa-162">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="0faaa-162">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="0faaa-163">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="0faaa-163">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="0faaa-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stocke en toute sécurité des secrets d’application pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0faaa-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="0faaa-165">Pour plus d’informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-165">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="0faaa-166">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="0faaa-166">Hierarchical configuration data</span></span>

<span data-ttu-id="0faaa-167">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-167">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="0faaa-168">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="0faaa-168">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="0faaa-169">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-169">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="0faaa-170">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="0faaa-170">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="0faaa-171">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-171">section0:key0</span></span>
* <span data-ttu-id="0faaa-172">section0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-172">section0:key1</span></span>
* <span data-ttu-id="0faaa-173">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-173">section1:key0</span></span>
* <span data-ttu-id="0faaa-174">section1:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-174">section1:key1</span></span>

<span data-ttu-id="0faaa-175">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="0faaa-176">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="0faaa-176">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="0faaa-177">Conventions</span><span class="sxs-lookup"><span data-stu-id="0faaa-177">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="0faaa-178">Sources et fournisseurs</span><span class="sxs-lookup"><span data-stu-id="0faaa-178">Sources and providers</span></span>

<span data-ttu-id="0faaa-179">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-179">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="0faaa-180">Les fournisseurs de configuration qui implémentent la détection des modifications peuvent recharger la configuration lorsqu’un paramètre sous-jacent est modifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-180">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="0faaa-181">Par exemple, le fournisseur de configuration de fichier (décrit plus loin dans cette rubrique) et le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) implémentent la détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="0faaa-181">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="0faaa-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0faaa-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> peuvent être injectées dans un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> ou MVC <xref:Microsoft.AspNetCore.Mvc.Controller> pour obtenir la configuration de la classe.</span><span class="sxs-lookup"><span data-stu-id="0faaa-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="0faaa-184">Dans les exemples suivants, le champ `_config` est utilisé pour accéder aux valeurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-184">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="0faaa-185">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="0faaa-185">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="0faaa-186">Keys</span><span class="sxs-lookup"><span data-stu-id="0faaa-186">Keys</span></span>

<span data-ttu-id="0faaa-187">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-187">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="0faaa-188">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="0faaa-188">Keys are case-insensitive.</span></span> <span data-ttu-id="0faaa-189">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-189">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="0faaa-190">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-190">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="0faaa-191">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="0faaa-191">Hierarchical keys</span></span>
  * <span data-ttu-id="0faaa-192">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-192">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="0faaa-193">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-193">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="0faaa-194">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et automatiquement transformé en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="0faaa-194">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="0faaa-195">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="0faaa-195">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="0faaa-196">Écrivez du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-196">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="0faaa-197"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-197">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="0faaa-198">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="0faaa-198">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="0faaa-199">Valeurs</span><span class="sxs-lookup"><span data-stu-id="0faaa-199">Values</span></span>

<span data-ttu-id="0faaa-200">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-200">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="0faaa-201">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-201">Values are strings.</span></span>
* <span data-ttu-id="0faaa-202">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="0faaa-202">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="0faaa-203">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="0faaa-203">Providers</span></span>

<span data-ttu-id="0faaa-204">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0faaa-204">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="0faaa-205">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="0faaa-205">Provider</span></span> | <span data-ttu-id="0faaa-206">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="0faaa-206">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="0faaa-207">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="0faaa-207">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="0faaa-208">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0faaa-208">Azure Key Vault</span></span> |
| <span data-ttu-id="0faaa-209">[Fournisseur Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentation Azure)</span><span class="sxs-lookup"><span data-stu-id="0faaa-209">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="0faaa-210">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="0faaa-210">Azure App Configuration</span></span> |
| [<span data-ttu-id="0faaa-211">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="0faaa-212">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-212">Command-line parameters</span></span> |
| [<span data-ttu-id="0faaa-213">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="0faaa-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="0faaa-214">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="0faaa-214">Custom source</span></span> |
| [<span data-ttu-id="0faaa-215">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="0faaa-216">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-216">Environment variables</span></span> |
| [<span data-ttu-id="0faaa-217">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="0faaa-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="0faaa-218">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="0faaa-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="0faaa-219">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="0faaa-219">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="0faaa-220">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-220">Directory files</span></span> |
| [<span data-ttu-id="0faaa-221">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-221">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="0faaa-222">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-222">In-memory collections</span></span> |
| <span data-ttu-id="0faaa-223">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="0faaa-223">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="0faaa-224">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="0faaa-224">File in the user profile directory</span></span> |

<span data-ttu-id="0faaa-225">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-225">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="0faaa-226">Les fournisseurs de configuration décrits dans cette rubrique sont décrits par ordre alphabétique, et non pas dans l’ordre dans lequel le code les réorganise.</span><span class="sxs-lookup"><span data-stu-id="0faaa-226">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="0faaa-227">Commandez des fournisseurs de configuration dans le code pour répondre aux priorités des sources de configuration sous-jacentes requises par l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-227">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="0faaa-228">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="0faaa-228">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="0faaa-229">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="0faaa-229">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="0faaa-230">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0faaa-230">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="0faaa-231">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="0faaa-231">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="0faaa-232">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-232">Environment variables</span></span>
1. <span data-ttu-id="0faaa-233">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-233">Command-line arguments</span></span>

<span data-ttu-id="0faaa-234">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-234">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="0faaa-235">La séquence de fournisseurs précédente est utilisée lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-235">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0faaa-236">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-236">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="0faaa-237">Configurer le générateur d’ordinateur hôte avec ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="0faaa-237">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="0faaa-238">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> et fournissez la configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-238">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="0faaa-239">`ConfigureHostConfiguration` est utilisé pour initialiser <xref:Microsoft.Extensions.Hosting.IHostEnvironment> pour une utilisation ultérieure dans le processus de génération.</span><span class="sxs-lookup"><span data-stu-id="0faaa-239">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="0faaa-240">`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-240">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="0faaa-241">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="0faaa-241">ConfigureAppConfiguration</span></span>

<span data-ttu-id="0faaa-242">Appelez `ConfigureAppConfiguration` quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-242">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="0faaa-243">Remplacez la configuration précédente par des arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-243">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="0faaa-244">Pour fournir une configuration d’application pouvant être remplacée par des arguments de ligne de commande, appelez `AddCommandLine` en dernier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-244">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="0faaa-245">Supprimer les fournisseurs ajoutés par CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="0faaa-245">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="0faaa-246">Pour supprimer les fournisseurs ajoutés par `CreateDefaultBuilder`, appelez d’abord [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) sur [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="0faaa-246">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="0faaa-247">Utiliser la configuration lors du démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="0faaa-247">Consume configuration during app startup</span></span>

<span data-ttu-id="0faaa-248">La configuration fournie à l’application dans `ConfigureAppConfiguration` est disponible lors du démarrage de l’application, notamment `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-248">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0faaa-249">Pour plus d’informations, consultez la section [Accéder à la configuration lors du démarrage](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="0faaa-249">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="0faaa-250">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-250">Command-line Configuration Provider</span></span>

<span data-ttu-id="0faaa-251"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-251">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="0faaa-252">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-252">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-253">`AddCommandLine` est appelé automatiquement quand `CreateDefaultBuilder(string [])` est appelé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-253">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="0faaa-254">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-254">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0faaa-255">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="0faaa-255">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0faaa-256">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-256">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="0faaa-257">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-257">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0faaa-258">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-258">Environment variables.</span></span>

<span data-ttu-id="0faaa-259">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0faaa-259">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="0faaa-260">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-260">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="0faaa-261">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="0faaa-261">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="0faaa-262">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="0faaa-262">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="0faaa-263">Pour les applications basées sur les modèles ASP.NET Core, `AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-263">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0faaa-264">Pour ajouter des fournisseurs de configuration supplémentaires et conserver la capacité de remplacer leur configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-264">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="0faaa-265">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="0faaa-265">**Example**</span></span>

<span data-ttu-id="0faaa-266">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-266">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="0faaa-267">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="0faaa-267">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="0faaa-268">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-268">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="0faaa-269">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-269">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0faaa-270">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-270">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="0faaa-271">Arguments</span><span class="sxs-lookup"><span data-stu-id="0faaa-271">Arguments</span></span>

<span data-ttu-id="0faaa-272">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="0faaa-272">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="0faaa-273">La valeur n’est pas requise si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="0faaa-273">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="0faaa-274">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-274">Key prefix</span></span>               | <span data-ttu-id="0faaa-275">Exemple</span><span class="sxs-lookup"><span data-stu-id="0faaa-275">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="0faaa-276">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="0faaa-276">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="0faaa-277">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="0faaa-277">Two dashes (`--`)</span></span>        | <span data-ttu-id="0faaa-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="0faaa-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="0faaa-279">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="0faaa-279">Forward slash (`/`)</span></span>      | <span data-ttu-id="0faaa-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="0faaa-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="0faaa-281">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="0faaa-281">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="0faaa-282">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-282">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="0faaa-283">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="0faaa-283">Switch mappings</span></span>

<span data-ttu-id="0faaa-284">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-284">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="0faaa-285">Lors de la génération manuelle d’une configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fournissez un dictionnaire de remplacements de commutateur dans la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-285">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="0faaa-286">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0faaa-286">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="0faaa-287">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-287">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="0faaa-288">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="0faaa-288">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="0faaa-289">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="0faaa-289">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="0faaa-290">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="0faaa-290">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="0faaa-291">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="0faaa-291">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="0faaa-292">Créez un dictionnaire des mappages de commutateurs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-292">Create a switch mappings dictionary.</span></span> <span data-ttu-id="0faaa-293">Dans l’exemple suivant, deux mappages de commutateurs sont créés :</span><span class="sxs-lookup"><span data-stu-id="0faaa-293">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="0faaa-294">Lorsque l’hôte est généré, appelez `AddCommandLine` avec le dictionnaire de mappages de commutateurs :</span><span class="sxs-lookup"><span data-stu-id="0faaa-294">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="0faaa-295">Pour les applications qui utilisent des mappages de commutateurs, l’appel à `CreateDefaultBuilder` ne doit pas passer d’arguments.</span><span class="sxs-lookup"><span data-stu-id="0faaa-295">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="0faaa-296">L’appel `CreateDefaultBuilder` de la méthode `AddCommandLine` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-296">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0faaa-297">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `ConfigurationBuilder` de la méthode `AddCommandLine` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="0faaa-297">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="0faaa-298">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0faaa-298">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="0faaa-299">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-299">Key</span></span>       | <span data-ttu-id="0faaa-300">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-300">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="0faaa-301">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="0faaa-301">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="0faaa-302">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0faaa-302">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="0faaa-303">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-303">Key</span></span>               | <span data-ttu-id="0faaa-304">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-304">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="0faaa-305">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-305">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="0faaa-306"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-306">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="0faaa-307">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-307">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="0faaa-308">[Azure App service](https://azure.microsoft.com/services/app-service/) permet de définir des variables d’environnement dans le portail Azure qui peuvent remplacer la configuration de l’application à l’aide du fournisseur de configuration des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="0faaa-309">Pour plus d’informations, consultez [Azure Apps : remplacer la configuration de l’application à l’aide du portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="0faaa-309">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="0faaa-310">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `DOTNET_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte générique](xref:fundamentals/host/generic-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-310">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="0faaa-311">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-311">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0faaa-312">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="0faaa-312">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0faaa-313">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="0faaa-313">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="0faaa-314">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-314">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="0faaa-315">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-315">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0faaa-316">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-316">Command-line arguments.</span></span>

<span data-ttu-id="0faaa-317">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-317">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="0faaa-318">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-318">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="0faaa-319">Pour fournir la configuration d’application à partir de variables d’environnement supplémentaires, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddEnvironmentVariables` avec le préfixe :</span><span class="sxs-lookup"><span data-stu-id="0faaa-319">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="0faaa-320">Appelez `AddEnvironmentVariables` dernier pour autoriser les variables d’environnement avec le préfixe spécifié à substituer des valeurs d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-320">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="0faaa-321">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="0faaa-321">**Example**</span></span>

<span data-ttu-id="0faaa-322">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-322">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="0faaa-323">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-323">Run the sample app.</span></span> <span data-ttu-id="0faaa-324">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-324">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0faaa-325">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-325">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="0faaa-326">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="0faaa-326">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="0faaa-327">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-327">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="0faaa-328">Consultez le fichier *Pages/Index.cshtml.cs* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-328">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="0faaa-329">Pour exposer toutes les variables d’environnement disponibles pour l’application, remplacez la `FilteredConfiguration` dans *pages/index. cshtml. cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="0faaa-329">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="0faaa-330">Préfixes</span><span class="sxs-lookup"><span data-stu-id="0faaa-330">Prefixes</span></span>

<span data-ttu-id="0faaa-331">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe à la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-331">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="0faaa-332">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-332">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="0faaa-333">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-333">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="0faaa-334">Lorsque le générateur d’hôte est créé, la configuration de l’hôte est fournie par des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-334">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="0faaa-335">Pour plus d’informations sur le préfixe utilisé pour ces variables d’environnement, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-335">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0faaa-336">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="0faaa-336">**Connection string prefixes**</span></span>

<span data-ttu-id="0faaa-337">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-337">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="0faaa-338">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-338">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="0faaa-339">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="0faaa-339">Connection string prefix</span></span> | <span data-ttu-id="0faaa-340">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="0faaa-340">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="0faaa-341">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="0faaa-341">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="0faaa-342">MySQL</span><span class="sxs-lookup"><span data-stu-id="0faaa-342">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="0faaa-343">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="0faaa-343">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="0faaa-344">SQL Server</span><span class="sxs-lookup"><span data-stu-id="0faaa-344">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="0faaa-345">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="0faaa-345">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="0faaa-346">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="0faaa-346">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="0faaa-347">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="0faaa-347">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="0faaa-348">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-348">Environment variable key</span></span> | <span data-ttu-id="0faaa-349">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="0faaa-349">Converted configuration key</span></span> | <span data-ttu-id="0faaa-350">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="0faaa-350">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="0faaa-351">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-351">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="0faaa-352">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-352">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="0faaa-353">Valeur: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="0faaa-353">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="0faaa-354">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-354">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="0faaa-355">Valeur: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="0faaa-355">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="0faaa-356">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-356">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="0faaa-357">Valeur: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="0faaa-357">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="0faaa-358">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="0faaa-358">**Example**</span></span>

<span data-ttu-id="0faaa-359">Une variable d’environnement de chaîne de connexion personnalisée est créée sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="0faaa-359">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="0faaa-360">Nom &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="0faaa-360">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="0faaa-361">Valeur &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="0faaa-361">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="0faaa-362">Si `IConfiguration` est injecté et affecté à un champ nommé `_config`, lisez la valeur :</span><span class="sxs-lookup"><span data-stu-id="0faaa-362">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="0faaa-363">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="0faaa-363">File Configuration Provider</span></span>

<span data-ttu-id="0faaa-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="0faaa-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="0faaa-365">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="0faaa-365">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="0faaa-366">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="0faaa-366">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="0faaa-367">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="0faaa-367">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="0faaa-368">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="0faaa-368">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="0faaa-369">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="0faaa-369">INI Configuration Provider</span></span>

<span data-ttu-id="0faaa-370"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-370">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="0faaa-371">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-371">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-372">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="0faaa-372">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="0faaa-373">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-373">Overloads permit specifying:</span></span>

* <span data-ttu-id="0faaa-374">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="0faaa-374">Whether the file is optional.</span></span>
* <span data-ttu-id="0faaa-375">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-375">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0faaa-376">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-376">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0faaa-377">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="0faaa-377">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0faaa-378">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="0faaa-378">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="0faaa-379">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-379">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0faaa-380">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-380">section0:key0</span></span>
* <span data-ttu-id="0faaa-381">section0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-381">section0:key1</span></span>
* <span data-ttu-id="0faaa-382">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="0faaa-382">section1:subsection:key</span></span>
* <span data-ttu-id="0faaa-383">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="0faaa-383">section2:subsection0:key</span></span>
* <span data-ttu-id="0faaa-384">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="0faaa-384">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="0faaa-385">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="0faaa-385">JSON Configuration Provider</span></span>

<span data-ttu-id="0faaa-386"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-386">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="0faaa-387">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-387">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-388">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-388">Overloads permit specifying:</span></span>

* <span data-ttu-id="0faaa-389">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="0faaa-389">Whether the file is optional.</span></span>
* <span data-ttu-id="0faaa-390">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-390">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0faaa-391">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-391">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0faaa-392">`AddJsonFile` est appelé automatiquement deux fois lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-392">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0faaa-393">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="0faaa-393">The method is called to load configuration from:</span></span>

* <span data-ttu-id="0faaa-394">*appSettings. json* &ndash; ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-394">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="0faaa-395">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-395">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="0faaa-396">*appSettings. {Environment}. JSON* &ndash; la version de l’environnement du fichier est chargée à partir de [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="0faaa-396">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="0faaa-397">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-397">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0faaa-398">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="0faaa-398">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0faaa-399">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-399">Environment variables.</span></span>
* <span data-ttu-id="0faaa-400">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-400">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0faaa-401">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-401">Command-line arguments.</span></span>

<span data-ttu-id="0faaa-402">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-402">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="0faaa-403">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-403">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="0faaa-404">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-404">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0faaa-405">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="0faaa-405">**Example**</span></span>

<span data-ttu-id="0faaa-406">L’exemple d’application tire parti de la méthode de commodité statique `CreateDefaultBuilder` pour créer l’hôte, ce qui comprend deux appels à `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="0faaa-406">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="0faaa-407">Le premier appel à `AddJsonFile` charge la configuration à partir de *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="0faaa-407">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="0faaa-408">Le deuxième appel à `AddJsonFile` charge la configuration à partir de *appSettings. { Environnement}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-408">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="0faaa-409">Pour *appSettings. Development. JSON* dans l’exemple d’application, le fichier suivant est chargé :</span><span class="sxs-lookup"><span data-stu-id="0faaa-409">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="0faaa-410">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-410">Run the sample app.</span></span> <span data-ttu-id="0faaa-411">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0faaa-412">La sortie contient des paires clé-valeur pour la configuration en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-412">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="0faaa-413">Le niveau de journalisation de la `Logging:LogLevel:Default` de clé est `Debug` lors de l’exécution de l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-413">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="0faaa-414">Exécutez à nouveau l’exemple d’application dans l’environnement de production :</span><span class="sxs-lookup"><span data-stu-id="0faaa-414">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="0faaa-415">Ouvrez le fichier *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="0faaa-415">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="0faaa-416">Dans le profil `ConfigurationSample`, remplacez la valeur de la variable d’environnement `ASPNETCORE_ENVIRONMENT` par `Production`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-416">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="0faaa-417">Enregistrez le fichier et exécutez l’application avec `dotnet run` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-417">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="0faaa-418">Paramètres dans *appSettings. Development. JSON* ne remplace plus les paramètres dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-418">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="0faaa-419">Le niveau de journalisation de la clé `Logging:LogLevel:Default` est `Information`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-419">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="0faaa-420">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="0faaa-420">XML Configuration Provider</span></span>

<span data-ttu-id="0faaa-421"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-421">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="0faaa-422">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-422">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-423">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-423">Overloads permit specifying:</span></span>

* <span data-ttu-id="0faaa-424">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="0faaa-424">Whether the file is optional.</span></span>
* <span data-ttu-id="0faaa-425">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-425">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0faaa-426">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-426">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0faaa-427">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-427">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="0faaa-428">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-428">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="0faaa-429">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="0faaa-429">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0faaa-430">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="0faaa-430">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="0faaa-431">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0faaa-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-432">section0:key0</span></span>
* <span data-ttu-id="0faaa-433">section0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-433">section0:key1</span></span>
* <span data-ttu-id="0faaa-434">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-434">section1:key0</span></span>
* <span data-ttu-id="0faaa-435">section1:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-435">section1:key1</span></span>

<span data-ttu-id="0faaa-436">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="0faaa-436">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="0faaa-437">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-437">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0faaa-438">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-438">section:section0:key:key0</span></span>
* <span data-ttu-id="0faaa-439">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-439">section:section0:key:key1</span></span>
* <span data-ttu-id="0faaa-440">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-440">section:section1:key:key0</span></span>
* <span data-ttu-id="0faaa-441">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-441">section:section1:key:key1</span></span>

<span data-ttu-id="0faaa-442">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="0faaa-442">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="0faaa-443">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-443">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0faaa-444">key:attribute</span><span class="sxs-lookup"><span data-stu-id="0faaa-444">key:attribute</span></span>
* <span data-ttu-id="0faaa-445">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="0faaa-445">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="0faaa-446">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="0faaa-446">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="0faaa-447">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-447">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="0faaa-448">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-448">The key is the file name.</span></span> <span data-ttu-id="0faaa-449">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-449">The value contains the file's contents.</span></span> <span data-ttu-id="0faaa-450">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="0faaa-450">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="0faaa-451">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-451">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="0faaa-452">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="0faaa-452">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="0faaa-453">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-453">Overloads permit specifying:</span></span>

* <span data-ttu-id="0faaa-454">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="0faaa-454">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="0faaa-455">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="0faaa-455">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="0faaa-456">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="0faaa-456">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="0faaa-457">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-457">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="0faaa-458">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="0faaa-458">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="0faaa-459">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-459">Memory Configuration Provider</span></span>

<span data-ttu-id="0faaa-460">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="0faaa-461">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-462">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="0faaa-463">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-463">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="0faaa-464">Dans l’exemple suivant, un dictionnaire de configuration est créé :</span><span class="sxs-lookup"><span data-stu-id="0faaa-464">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="0faaa-465">Le dictionnaire est utilisé avec un appel à `AddInMemoryCollection` pour fournir la configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-465">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="0faaa-466">GetValue</span><span class="sxs-lookup"><span data-stu-id="0faaa-466">GetValue</span></span>

<span data-ttu-id="0faaa-467">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur unique de la configuration avec une clé spécifiée et la convertit en type non collection spécifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="0faaa-468">Une surcharge accepte une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="0faaa-468">An overload accepts a default value.</span></span>

<span data-ttu-id="0faaa-469">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="0faaa-469">The following example:</span></span>

* <span data-ttu-id="0faaa-470">Extrait la valeur de chaîne de la configuration avec la clé `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-470">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="0faaa-471">Si `NumberKey` est introuvable dans les clés de configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-471">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="0faaa-472">Tape la valeur comme `int`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-472">Types the value as an `int`.</span></span>
* <span data-ttu-id="0faaa-473">Stocke la valeur dans la propriété `NumberConfig` pour une utilisation par la page.</span><span class="sxs-lookup"><span data-stu-id="0faaa-473">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="0faaa-474">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="0faaa-474">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="0faaa-475">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="0faaa-475">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="0faaa-476">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="0faaa-476">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="0faaa-477">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-477">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="0faaa-478">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-478">section0:key0</span></span>
* <span data-ttu-id="0faaa-479">section0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-479">section0:key1</span></span>
* <span data-ttu-id="0faaa-480">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-480">section1:key0</span></span>
* <span data-ttu-id="0faaa-481">section1:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-481">section1:key1</span></span>
* <span data-ttu-id="0faaa-482">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-482">section2:subsection0:key0</span></span>
* <span data-ttu-id="0faaa-483">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-483">section2:subsection0:key1</span></span>
* <span data-ttu-id="0faaa-484">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-484">section2:subsection1:key0</span></span>
* <span data-ttu-id="0faaa-485">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-485">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="0faaa-486">GetSection</span><span class="sxs-lookup"><span data-stu-id="0faaa-486">GetSection</span></span>

<span data-ttu-id="0faaa-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="0faaa-488">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="0faaa-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="0faaa-489">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="0faaa-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="0faaa-490">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="0faaa-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="0faaa-491">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="0faaa-492">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="0faaa-493">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="0faaa-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="0faaa-494"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="0faaa-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="0faaa-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="0faaa-495">GetChildren</span></span>

<span data-ttu-id="0faaa-496">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="0faaa-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="0faaa-497">Exists</span><span class="sxs-lookup"><span data-stu-id="0faaa-497">Exists</span></span>

<span data-ttu-id="0faaa-498">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="0faaa-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="0faaa-499">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="0faaa-500">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="0faaa-500">Bind to a class</span></span>

<span data-ttu-id="0faaa-501">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="0faaa-502">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="0faaa-503">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="0faaa-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="0faaa-504">Le Binder lie les valeurs à toutes les propriétés publiques en lecture/écriture du type fourni.</span><span class="sxs-lookup"><span data-stu-id="0faaa-504">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="0faaa-505">Les champs ne sont **pas** liés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-505">Fields are **not** bound.</span></span>

<span data-ttu-id="0faaa-506">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="0faaa-506">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="0faaa-507">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-507">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

<span data-ttu-id="0faaa-508">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="0faaa-508">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="0faaa-509">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-509">Key</span></span>                   | <span data-ttu-id="0faaa-510">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-510">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="0faaa-511">starship:name</span><span class="sxs-lookup"><span data-stu-id="0faaa-511">starship:name</span></span>         | <span data-ttu-id="0faaa-512">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="0faaa-512">USS Enterprise</span></span>                                    |
| <span data-ttu-id="0faaa-513">starship:registry</span><span class="sxs-lookup"><span data-stu-id="0faaa-513">starship:registry</span></span>     | <span data-ttu-id="0faaa-514">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="0faaa-514">NCC-1701</span></span>                                          |
| <span data-ttu-id="0faaa-515">starship:class</span><span class="sxs-lookup"><span data-stu-id="0faaa-515">starship:class</span></span>        | <span data-ttu-id="0faaa-516">Constitution</span><span class="sxs-lookup"><span data-stu-id="0faaa-516">Constitution</span></span>                                      |
| <span data-ttu-id="0faaa-517">starship:length</span><span class="sxs-lookup"><span data-stu-id="0faaa-517">starship:length</span></span>       | <span data-ttu-id="0faaa-518">304.8</span><span class="sxs-lookup"><span data-stu-id="0faaa-518">304.8</span></span>                                             |
| <span data-ttu-id="0faaa-519">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="0faaa-519">starship:commissioned</span></span> | <span data-ttu-id="0faaa-520">False</span><span class="sxs-lookup"><span data-stu-id="0faaa-520">False</span></span>                                             |
| <span data-ttu-id="0faaa-521">trademark</span><span class="sxs-lookup"><span data-stu-id="0faaa-521">trademark</span></span>             | <span data-ttu-id="0faaa-522">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="0faaa-522">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="0faaa-523">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-523">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="0faaa-524">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-524">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="0faaa-525">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-525">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="0faaa-526">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="0faaa-526">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="0faaa-527">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="0faaa-527">Bind to an object graph</span></span>

<span data-ttu-id="0faaa-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="0faaa-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="0faaa-529">Comme pour la liaison d’un objet simple, seules les propriétés accessibles en lecture/écriture publiques sont liées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-529">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="0faaa-530">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="0faaa-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="0faaa-531">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="0faaa-532">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="0faaa-533">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="0faaa-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="0faaa-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="0faaa-535">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="0faaa-536">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="0faaa-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="0faaa-537">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="0faaa-537">Bind an array to a class</span></span>

<span data-ttu-id="0faaa-538">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="0faaa-538">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="0faaa-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-539">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="0faaa-540">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer une liaison de tableau à un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="0faaa-540">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="0faaa-541">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="0faaa-541">Binding is provided by convention.</span></span> <span data-ttu-id="0faaa-542">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="0faaa-542">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="0faaa-543">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="0faaa-543">**In-memory array processing**</span></span>

<span data-ttu-id="0faaa-544">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0faaa-544">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="0faaa-545">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-545">Key</span></span>             | <span data-ttu-id="0faaa-546">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-546">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="0faaa-547">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="0faaa-547">array:entries:0</span></span> | <span data-ttu-id="0faaa-548">value0</span><span class="sxs-lookup"><span data-stu-id="0faaa-548">value0</span></span> |
| <span data-ttu-id="0faaa-549">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="0faaa-549">array:entries:1</span></span> | <span data-ttu-id="0faaa-550">valeur1</span><span class="sxs-lookup"><span data-stu-id="0faaa-550">value1</span></span> |
| <span data-ttu-id="0faaa-551">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="0faaa-551">array:entries:2</span></span> | <span data-ttu-id="0faaa-552">valeur2</span><span class="sxs-lookup"><span data-stu-id="0faaa-552">value2</span></span> |
| <span data-ttu-id="0faaa-553">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="0faaa-553">array:entries:4</span></span> | <span data-ttu-id="0faaa-554">value4</span><span class="sxs-lookup"><span data-stu-id="0faaa-554">value4</span></span> |
| <span data-ttu-id="0faaa-555">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="0faaa-555">array:entries:5</span></span> | <span data-ttu-id="0faaa-556">value5</span><span class="sxs-lookup"><span data-stu-id="0faaa-556">value5</span></span> |

<span data-ttu-id="0faaa-557">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="0faaa-557">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="0faaa-558">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="0faaa-558">The array skips a value for index &num;3.</span></span> <span data-ttu-id="0faaa-559">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="0faaa-559">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="0faaa-560">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="0faaa-560">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="0faaa-561">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="0faaa-561">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="0faaa-562">La syntaxe [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="0faaa-562">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="0faaa-563">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-563">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="0faaa-564">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="0faaa-564">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="0faaa-565">`ArrayExample.Entries` Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-565">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="0faaa-566">0</span><span class="sxs-lookup"><span data-stu-id="0faaa-566">0</span></span>                            | <span data-ttu-id="0faaa-567">value0</span><span class="sxs-lookup"><span data-stu-id="0faaa-567">value0</span></span>                       |
| <span data-ttu-id="0faaa-568">1</span><span class="sxs-lookup"><span data-stu-id="0faaa-568">1</span></span>                            | <span data-ttu-id="0faaa-569">valeur1</span><span class="sxs-lookup"><span data-stu-id="0faaa-569">value1</span></span>                       |
| <span data-ttu-id="0faaa-570">2</span><span class="sxs-lookup"><span data-stu-id="0faaa-570">2</span></span>                            | <span data-ttu-id="0faaa-571">valeur2</span><span class="sxs-lookup"><span data-stu-id="0faaa-571">value2</span></span>                       |
| <span data-ttu-id="0faaa-572">3</span><span class="sxs-lookup"><span data-stu-id="0faaa-572">3</span></span>                            | <span data-ttu-id="0faaa-573">value4</span><span class="sxs-lookup"><span data-stu-id="0faaa-573">value4</span></span>                       |
| <span data-ttu-id="0faaa-574">4</span><span class="sxs-lookup"><span data-stu-id="0faaa-574">4</span></span>                            | <span data-ttu-id="0faaa-575">value5</span><span class="sxs-lookup"><span data-stu-id="0faaa-575">value5</span></span>                       |

<span data-ttu-id="0faaa-576">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-576">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="0faaa-577">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="0faaa-577">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="0faaa-578">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="0faaa-578">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="0faaa-579">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-579">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="0faaa-580">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-580">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="0faaa-581">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-581">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="0faaa-582">Dans `ConfigureAppConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-582">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="0faaa-583">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-583">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="0faaa-584">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-584">Key</span></span>             | <span data-ttu-id="0faaa-585">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-585">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="0faaa-586">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="0faaa-586">array:entries:3</span></span> | <span data-ttu-id="0faaa-587">valeur3</span><span class="sxs-lookup"><span data-stu-id="0faaa-587">value3</span></span> |

<span data-ttu-id="0faaa-588">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="0faaa-588">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="0faaa-589">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="0faaa-589">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="0faaa-590">`ArrayExample.Entries` Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-590">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="0faaa-591">0</span><span class="sxs-lookup"><span data-stu-id="0faaa-591">0</span></span>                            | <span data-ttu-id="0faaa-592">value0</span><span class="sxs-lookup"><span data-stu-id="0faaa-592">value0</span></span>                       |
| <span data-ttu-id="0faaa-593">1</span><span class="sxs-lookup"><span data-stu-id="0faaa-593">1</span></span>                            | <span data-ttu-id="0faaa-594">valeur1</span><span class="sxs-lookup"><span data-stu-id="0faaa-594">value1</span></span>                       |
| <span data-ttu-id="0faaa-595">2</span><span class="sxs-lookup"><span data-stu-id="0faaa-595">2</span></span>                            | <span data-ttu-id="0faaa-596">valeur2</span><span class="sxs-lookup"><span data-stu-id="0faaa-596">value2</span></span>                       |
| <span data-ttu-id="0faaa-597">3</span><span class="sxs-lookup"><span data-stu-id="0faaa-597">3</span></span>                            | <span data-ttu-id="0faaa-598">valeur3</span><span class="sxs-lookup"><span data-stu-id="0faaa-598">value3</span></span>                       |
| <span data-ttu-id="0faaa-599">4</span><span class="sxs-lookup"><span data-stu-id="0faaa-599">4</span></span>                            | <span data-ttu-id="0faaa-600">value4</span><span class="sxs-lookup"><span data-stu-id="0faaa-600">value4</span></span>                       |
| <span data-ttu-id="0faaa-601">5</span><span class="sxs-lookup"><span data-stu-id="0faaa-601">5</span></span>                            | <span data-ttu-id="0faaa-602">value5</span><span class="sxs-lookup"><span data-stu-id="0faaa-602">value5</span></span>                       |

<span data-ttu-id="0faaa-603">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="0faaa-603">**JSON array processing**</span></span>

<span data-ttu-id="0faaa-604">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="0faaa-604">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="0faaa-605">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="0faaa-605">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="0faaa-606">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-606">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="0faaa-607">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-607">Key</span></span>                     | <span data-ttu-id="0faaa-608">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-608">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="0faaa-609">json_array:key</span><span class="sxs-lookup"><span data-stu-id="0faaa-609">json_array:key</span></span>          | <span data-ttu-id="0faaa-610">valueA</span><span class="sxs-lookup"><span data-stu-id="0faaa-610">valueA</span></span> |
| <span data-ttu-id="0faaa-611">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="0faaa-611">json_array:subsection:0</span></span> | <span data-ttu-id="0faaa-612">valueB</span><span class="sxs-lookup"><span data-stu-id="0faaa-612">valueB</span></span> |
| <span data-ttu-id="0faaa-613">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="0faaa-613">json_array:subsection:1</span></span> | <span data-ttu-id="0faaa-614">valueC</span><span class="sxs-lookup"><span data-stu-id="0faaa-614">valueC</span></span> |
| <span data-ttu-id="0faaa-615">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="0faaa-615">json_array:subsection:2</span></span> | <span data-ttu-id="0faaa-616">valueD</span><span class="sxs-lookup"><span data-stu-id="0faaa-616">valueD</span></span> |

<span data-ttu-id="0faaa-617">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-617">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="0faaa-618">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-618">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="0faaa-619">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-619">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="0faaa-620">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="0faaa-620">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="0faaa-621">`JsonArrayExample.Subsection` Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-621">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="0faaa-622">0</span><span class="sxs-lookup"><span data-stu-id="0faaa-622">0</span></span>                                   | <span data-ttu-id="0faaa-623">valueB</span><span class="sxs-lookup"><span data-stu-id="0faaa-623">valueB</span></span>                              |
| <span data-ttu-id="0faaa-624">1</span><span class="sxs-lookup"><span data-stu-id="0faaa-624">1</span></span>                                   | <span data-ttu-id="0faaa-625">valueC</span><span class="sxs-lookup"><span data-stu-id="0faaa-625">valueC</span></span>                              |
| <span data-ttu-id="0faaa-626">2</span><span class="sxs-lookup"><span data-stu-id="0faaa-626">2</span></span>                                   | <span data-ttu-id="0faaa-627">valueD</span><span class="sxs-lookup"><span data-stu-id="0faaa-627">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="0faaa-628">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="0faaa-628">Custom configuration provider</span></span>

<span data-ttu-id="0faaa-629">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="0faaa-629">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="0faaa-630">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-630">The provider has the following characteristics:</span></span>

* <span data-ttu-id="0faaa-631">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-631">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="0faaa-632">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-632">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="0faaa-633">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="0faaa-633">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="0faaa-634">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-634">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="0faaa-635">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-635">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="0faaa-636">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="0faaa-636">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="0faaa-637">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-637">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="0faaa-638">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="0faaa-638">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="0faaa-639">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="0faaa-640">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-640">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="0faaa-641">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="0faaa-642">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-642">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="0faaa-643">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="0faaa-643">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="0faaa-644">Étant donné que les [clés de configuration ne](#keys)respectent pas la casse, le dictionnaire utilisé pour initialiser la base de données est créé avec le comparateur ne respectant pas la casse ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="0faaa-644">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="0faaa-645">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="0faaa-646">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-646">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="0faaa-647">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-647">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="0faaa-648">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-648">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="0faaa-649">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="0faaa-649">Access configuration during startup</span></span>

<span data-ttu-id="0faaa-650">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-650">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0faaa-651">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="0faaa-651">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="0faaa-652">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="0faaa-652">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="0faaa-653">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="0faaa-653">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="0faaa-654">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="0faaa-654">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="0faaa-655">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="0faaa-655">In a Razor Pages page:</span></span>

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

<span data-ttu-id="0faaa-656">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="0faaa-656">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="0faaa-657">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="0faaa-657">Add configuration from an external assembly</span></span>

<span data-ttu-id="0faaa-658">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-658">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="0faaa-659">Pour plus d’informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-659">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0faaa-660">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0faaa-660">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0faaa-661">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-661">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="0faaa-662">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-662">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="0faaa-663">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0faaa-663">Azure Key Vault</span></span>
* <span data-ttu-id="0faaa-664">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="0faaa-664">Azure App Configuration</span></span>
* <span data-ttu-id="0faaa-665">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-665">Command-line arguments</span></span>
* <span data-ttu-id="0faaa-666">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="0faaa-666">Custom providers (installed or created)</span></span>
* <span data-ttu-id="0faaa-667">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-667">Directory files</span></span>
* <span data-ttu-id="0faaa-668">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-668">Environment variables</span></span>
* <span data-ttu-id="0faaa-669">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-669">In-memory .NET objects</span></span>
* <span data-ttu-id="0faaa-670">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="0faaa-670">Settings files</span></span>

<span data-ttu-id="0faaa-671">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0faaa-671">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="0faaa-672">Les exemples de code qui suivent et dans l’échantillon d’application utilisent l’espace de noms <xref:Microsoft.Extensions.Configuration> :</span><span class="sxs-lookup"><span data-stu-id="0faaa-672">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="0faaa-673">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="0faaa-673">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="0faaa-674">Les options utilisent des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-674">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="0faaa-675">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-675">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="0faaa-676">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0faaa-676">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="0faaa-677">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="0faaa-677">Host versus app configuration</span></span>

<span data-ttu-id="0faaa-678">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-678">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="0faaa-679">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="0faaa-679">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0faaa-680">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="0faaa-680">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="0faaa-681">Les paires clé-valeur de la configuration de l’hôte sont également incluses dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-681">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="0faaa-682">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-682">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="0faaa-683">Autre configuration</span><span class="sxs-lookup"><span data-stu-id="0faaa-683">Other configuration</span></span>

<span data-ttu-id="0faaa-684">Cette rubrique se rapporte uniquement à la configuration de l' *application*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-684">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="0faaa-685">D’autres aspects de l’exécution et de l’hébergement des applications ASP.NET Core sont configurés à l’aide des fichiers de configuration non traités dans cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="0faaa-685">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="0faaa-686">*Launch. json*/*launchSettings. JSON* sont des fichiers de configuration d’outils pour l’environnement de développement, décrits ci-après :</span><span class="sxs-lookup"><span data-stu-id="0faaa-686">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="0faaa-687">Dans <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-687">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="0faaa-688">Dans l’ensemble de la documentation dans lequel les fichiers sont utilisés pour configurer des applications ASP.NET Core pour les scénarios de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-688">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="0faaa-689">*Web. config* est un fichier de configuration de serveur, décrit dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-689">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="0faaa-690">Pour plus d’informations sur la migration de la configuration d’application à partir de versions antérieures de ASP.NET, consultez <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-690">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="0faaa-691">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="0faaa-691">Default configuration</span></span>

<span data-ttu-id="0faaa-692">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="0faaa-692">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="0faaa-693">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="0faaa-693">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="0faaa-694">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="0faaa-694">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="0faaa-695">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte générique](xref:fundamentals/host/generic-host), consultez la [dernière version de cette rubrique](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="0faaa-695">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="0faaa-696">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0faaa-696">Host configuration is provided from:</span></span>
  * <span data-ttu-id="0faaa-697">Des variables d’environnement préfixées avec `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-697">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="0faaa-698">Le préfixe (`ASPNETCORE_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-698">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="0faaa-699">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-699">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="0faaa-700">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0faaa-700">App configuration is provided from:</span></span>
  * <span data-ttu-id="0faaa-701">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-701">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="0faaa-702">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-702">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="0faaa-703">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-703">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="0faaa-704">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-704">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="0faaa-705">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0faaa-705">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="0faaa-706">Sécurité</span><span class="sxs-lookup"><span data-stu-id="0faaa-706">Security</span></span>

<span data-ttu-id="0faaa-707">Adoptez les pratiques suivantes pour sécuriser les données de configuration sensibles :</span><span class="sxs-lookup"><span data-stu-id="0faaa-707">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="0faaa-708">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="0faaa-708">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="0faaa-709">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="0faaa-709">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="0faaa-710">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="0faaa-710">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="0faaa-711">Pour plus d'informations, voir les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-711">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="0faaa-712"><xref:security/app-secrets> &ndash; fournit des conseils sur l’utilisation de variables d’environnement pour stocker des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="0faaa-712"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="0faaa-713">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="0faaa-713">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="0faaa-714">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="0faaa-714">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="0faaa-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stocke en toute sécurité des secrets d’application pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0faaa-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="0faaa-716">Pour plus d’informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-716">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="0faaa-717">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="0faaa-717">Hierarchical configuration data</span></span>

<span data-ttu-id="0faaa-718">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-718">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="0faaa-719">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="0faaa-719">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="0faaa-720">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-720">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="0faaa-721">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="0faaa-721">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="0faaa-722">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-722">section0:key0</span></span>
* <span data-ttu-id="0faaa-723">section0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-723">section0:key1</span></span>
* <span data-ttu-id="0faaa-724">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-724">section1:key0</span></span>
* <span data-ttu-id="0faaa-725">section1:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-725">section1:key1</span></span>

<span data-ttu-id="0faaa-726">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="0faaa-727">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="0faaa-727">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="0faaa-728">Conventions</span><span class="sxs-lookup"><span data-stu-id="0faaa-728">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="0faaa-729">Sources et fournisseurs</span><span class="sxs-lookup"><span data-stu-id="0faaa-729">Sources and providers</span></span>

<span data-ttu-id="0faaa-730">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-730">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="0faaa-731">Les fournisseurs de configuration qui implémentent la détection des modifications peuvent recharger la configuration lorsqu’un paramètre sous-jacent est modifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-731">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="0faaa-732">Par exemple, le fournisseur de configuration de fichier (décrit plus loin dans cette rubrique) et le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) implémentent la détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="0faaa-732">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="0faaa-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0faaa-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> peuvent être injectées dans un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> ou MVC <xref:Microsoft.AspNetCore.Mvc.Controller> pour obtenir la configuration de la classe.</span><span class="sxs-lookup"><span data-stu-id="0faaa-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="0faaa-735">Dans les exemples suivants, le champ `_config` est utilisé pour accéder aux valeurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-735">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="0faaa-736">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="0faaa-736">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="0faaa-737">Keys</span><span class="sxs-lookup"><span data-stu-id="0faaa-737">Keys</span></span>

<span data-ttu-id="0faaa-738">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-738">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="0faaa-739">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="0faaa-739">Keys are case-insensitive.</span></span> <span data-ttu-id="0faaa-740">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-740">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="0faaa-741">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-741">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="0faaa-742">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="0faaa-742">Hierarchical keys</span></span>
  * <span data-ttu-id="0faaa-743">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-743">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="0faaa-744">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-744">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="0faaa-745">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et automatiquement transformé en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="0faaa-745">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="0faaa-746">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="0faaa-746">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="0faaa-747">Écrivez du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-747">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="0faaa-748"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-748">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="0faaa-749">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="0faaa-749">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="0faaa-750">Valeurs</span><span class="sxs-lookup"><span data-stu-id="0faaa-750">Values</span></span>

<span data-ttu-id="0faaa-751">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-751">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="0faaa-752">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-752">Values are strings.</span></span>
* <span data-ttu-id="0faaa-753">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="0faaa-753">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="0faaa-754">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="0faaa-754">Providers</span></span>

<span data-ttu-id="0faaa-755">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0faaa-755">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="0faaa-756">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="0faaa-756">Provider</span></span> | <span data-ttu-id="0faaa-757">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="0faaa-757">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="0faaa-758">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="0faaa-758">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="0faaa-759">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0faaa-759">Azure Key Vault</span></span> |
| <span data-ttu-id="0faaa-760">[Fournisseur Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentation Azure)</span><span class="sxs-lookup"><span data-stu-id="0faaa-760">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="0faaa-761">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="0faaa-761">Azure App Configuration</span></span> |
| [<span data-ttu-id="0faaa-762">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-762">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="0faaa-763">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-763">Command-line parameters</span></span> |
| [<span data-ttu-id="0faaa-764">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="0faaa-764">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="0faaa-765">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="0faaa-765">Custom source</span></span> |
| [<span data-ttu-id="0faaa-766">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-766">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="0faaa-767">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-767">Environment variables</span></span> |
| [<span data-ttu-id="0faaa-768">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="0faaa-768">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="0faaa-769">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="0faaa-769">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="0faaa-770">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="0faaa-770">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="0faaa-771">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-771">Directory files</span></span> |
| [<span data-ttu-id="0faaa-772">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-772">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="0faaa-773">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-773">In-memory collections</span></span> |
| <span data-ttu-id="0faaa-774">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="0faaa-774">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="0faaa-775">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="0faaa-775">File in the user profile directory</span></span> |

<span data-ttu-id="0faaa-776">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-776">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="0faaa-777">Les fournisseurs de configuration décrits dans cette rubrique sont décrits par ordre alphabétique, et non pas dans l’ordre dans lequel le code les réorganise.</span><span class="sxs-lookup"><span data-stu-id="0faaa-777">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="0faaa-778">Commandez des fournisseurs de configuration dans le code pour répondre aux priorités des sources de configuration sous-jacentes requises par l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-778">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="0faaa-779">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="0faaa-779">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="0faaa-780">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="0faaa-780">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="0faaa-781">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0faaa-781">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="0faaa-782">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="0faaa-782">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="0faaa-783">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-783">Environment variables</span></span>
1. <span data-ttu-id="0faaa-784">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-784">Command-line arguments</span></span>

<span data-ttu-id="0faaa-785">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-785">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="0faaa-786">La séquence de fournisseurs précédente est utilisée lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-786">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0faaa-787">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-787">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="0faaa-788">Configurer le générateur d’ordinateur hôte avec UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="0faaa-788">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="0faaa-789">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> sur le générateur d’hôte avec la configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-789">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="0faaa-790">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="0faaa-790">ConfigureAppConfiguration</span></span>

<span data-ttu-id="0faaa-791">Appelez `ConfigureAppConfiguration` quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-791">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="0faaa-792">Remplacez la configuration précédente par des arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-792">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="0faaa-793">Pour fournir une configuration d’application pouvant être remplacée par des arguments de ligne de commande, appelez `AddCommandLine` en dernier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-793">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="0faaa-794">Supprimer les fournisseurs ajoutés par CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="0faaa-794">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="0faaa-795">Pour supprimer les fournisseurs ajoutés par `CreateDefaultBuilder`, appelez d’abord [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) sur [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="0faaa-795">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="0faaa-796">Utiliser la configuration lors du démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="0faaa-796">Consume configuration during app startup</span></span>

<span data-ttu-id="0faaa-797">La configuration fournie à l’application dans `ConfigureAppConfiguration` est disponible lors du démarrage de l’application, notamment `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-797">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0faaa-798">Pour plus d’informations, consultez la section [Accéder à la configuration lors du démarrage](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="0faaa-798">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="0faaa-799">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-799">Command-line Configuration Provider</span></span>

<span data-ttu-id="0faaa-800"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-800">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="0faaa-801">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-801">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-802">`AddCommandLine` est appelé automatiquement quand `CreateDefaultBuilder(string [])` est appelé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-802">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="0faaa-803">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-803">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0faaa-804">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="0faaa-804">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0faaa-805">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-805">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="0faaa-806">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-806">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0faaa-807">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-807">Environment variables.</span></span>

<span data-ttu-id="0faaa-808">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0faaa-808">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="0faaa-809">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-809">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="0faaa-810">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="0faaa-810">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="0faaa-811">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="0faaa-811">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="0faaa-812">Pour les applications basées sur les modèles ASP.NET Core, `AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-812">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0faaa-813">Pour ajouter des fournisseurs de configuration supplémentaires et conserver la capacité de remplacer leur configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-813">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="0faaa-814">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="0faaa-814">**Example**</span></span>

<span data-ttu-id="0faaa-815">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-815">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="0faaa-816">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="0faaa-816">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="0faaa-817">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-817">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="0faaa-818">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-818">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0faaa-819">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-819">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="0faaa-820">Arguments</span><span class="sxs-lookup"><span data-stu-id="0faaa-820">Arguments</span></span>

<span data-ttu-id="0faaa-821">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="0faaa-821">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="0faaa-822">La valeur n’est pas requise si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="0faaa-822">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="0faaa-823">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-823">Key prefix</span></span>               | <span data-ttu-id="0faaa-824">Exemple</span><span class="sxs-lookup"><span data-stu-id="0faaa-824">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="0faaa-825">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="0faaa-825">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="0faaa-826">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="0faaa-826">Two dashes (`--`)</span></span>        | <span data-ttu-id="0faaa-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="0faaa-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="0faaa-828">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="0faaa-828">Forward slash (`/`)</span></span>      | <span data-ttu-id="0faaa-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="0faaa-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="0faaa-830">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="0faaa-830">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="0faaa-831">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-831">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="0faaa-832">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="0faaa-832">Switch mappings</span></span>

<span data-ttu-id="0faaa-833">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-833">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="0faaa-834">Lors de la génération manuelle d’une configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fournissez un dictionnaire de remplacements de commutateur dans la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-834">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="0faaa-835">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0faaa-835">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="0faaa-836">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-836">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="0faaa-837">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="0faaa-837">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="0faaa-838">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="0faaa-838">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="0faaa-839">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="0faaa-839">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="0faaa-840">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="0faaa-840">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="0faaa-841">Créez un dictionnaire des mappages de commutateurs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-841">Create a switch mappings dictionary.</span></span> <span data-ttu-id="0faaa-842">Dans l’exemple suivant, deux mappages de commutateurs sont créés :</span><span class="sxs-lookup"><span data-stu-id="0faaa-842">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="0faaa-843">Lorsque l’hôte est généré, appelez `AddCommandLine` avec le dictionnaire de mappages de commutateurs :</span><span class="sxs-lookup"><span data-stu-id="0faaa-843">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="0faaa-844">Pour les applications qui utilisent des mappages de commutateurs, l’appel à `CreateDefaultBuilder` ne doit pas passer d’arguments.</span><span class="sxs-lookup"><span data-stu-id="0faaa-844">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="0faaa-845">L’appel `CreateDefaultBuilder` de la méthode `AddCommandLine` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-845">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0faaa-846">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `ConfigurationBuilder` de la méthode `AddCommandLine` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="0faaa-846">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="0faaa-847">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0faaa-847">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="0faaa-848">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-848">Key</span></span>       | <span data-ttu-id="0faaa-849">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-849">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="0faaa-850">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="0faaa-850">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="0faaa-851">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0faaa-851">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="0faaa-852">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-852">Key</span></span>               | <span data-ttu-id="0faaa-853">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-853">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="0faaa-854">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-854">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="0faaa-855"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-855">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="0faaa-856">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-856">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="0faaa-857">[Azure App service](https://azure.microsoft.com/services/app-service/) permet de définir des variables d’environnement dans le portail Azure qui peuvent remplacer la configuration de l’application à l’aide du fournisseur de configuration des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="0faaa-858">Pour plus d’informations, consultez [Azure Apps : remplacer la configuration de l’application à l’aide du portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="0faaa-858">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="0faaa-859">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `ASPNETCORE_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte web](xref:fundamentals/host/web-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-859">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="0faaa-860">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-860">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0faaa-861">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="0faaa-861">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0faaa-862">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="0faaa-862">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="0faaa-863">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-863">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="0faaa-864">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-864">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0faaa-865">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-865">Command-line arguments.</span></span>

<span data-ttu-id="0faaa-866">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-866">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="0faaa-867">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-867">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="0faaa-868">Pour fournir la configuration d’application à partir de variables d’environnement supplémentaires, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddEnvironmentVariables` avec le préfixe :</span><span class="sxs-lookup"><span data-stu-id="0faaa-868">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="0faaa-869">Appelez `AddEnvironmentVariables` dernier pour autoriser les variables d’environnement avec le préfixe spécifié à substituer des valeurs d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="0faaa-869">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="0faaa-870">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="0faaa-870">**Example**</span></span>

<span data-ttu-id="0faaa-871">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-871">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="0faaa-872">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-872">Run the sample app.</span></span> <span data-ttu-id="0faaa-873">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-873">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0faaa-874">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-874">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="0faaa-875">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="0faaa-875">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="0faaa-876">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-876">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="0faaa-877">Consultez le fichier *Pages/Index.cshtml.cs* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-877">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="0faaa-878">Pour exposer toutes les variables d’environnement disponibles pour l’application, remplacez la `FilteredConfiguration` dans *pages/index. cshtml. cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="0faaa-878">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="0faaa-879">Préfixes</span><span class="sxs-lookup"><span data-stu-id="0faaa-879">Prefixes</span></span>

<span data-ttu-id="0faaa-880">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe à la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-880">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="0faaa-881">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-881">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="0faaa-882">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-882">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="0faaa-883">Lorsque le générateur d’hôte est créé, la configuration de l’hôte est fournie par des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-883">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="0faaa-884">Pour plus d’informations sur le préfixe utilisé pour ces variables d’environnement, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-884">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0faaa-885">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="0faaa-885">**Connection string prefixes**</span></span>

<span data-ttu-id="0faaa-886">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-886">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="0faaa-887">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-887">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="0faaa-888">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="0faaa-888">Connection string prefix</span></span> | <span data-ttu-id="0faaa-889">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="0faaa-889">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="0faaa-890">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="0faaa-890">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="0faaa-891">MySQL</span><span class="sxs-lookup"><span data-stu-id="0faaa-891">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="0faaa-892">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="0faaa-892">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="0faaa-893">SQL Server</span><span class="sxs-lookup"><span data-stu-id="0faaa-893">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="0faaa-894">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="0faaa-894">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="0faaa-895">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="0faaa-895">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="0faaa-896">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="0faaa-896">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="0faaa-897">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="0faaa-897">Environment variable key</span></span> | <span data-ttu-id="0faaa-898">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="0faaa-898">Converted configuration key</span></span> | <span data-ttu-id="0faaa-899">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="0faaa-899">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="0faaa-900">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-900">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="0faaa-901">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-901">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="0faaa-902">Valeur: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="0faaa-902">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="0faaa-903">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-903">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="0faaa-904">Valeur: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="0faaa-904">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="0faaa-905">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-905">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="0faaa-906">Valeur: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="0faaa-906">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="0faaa-907">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="0faaa-907">**Example**</span></span>

<span data-ttu-id="0faaa-908">Une variable d’environnement de chaîne de connexion personnalisée est créée sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="0faaa-908">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="0faaa-909">Nom &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="0faaa-909">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="0faaa-910">Valeur &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="0faaa-910">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="0faaa-911">Si `IConfiguration` est injecté et affecté à un champ nommé `_config`, lisez la valeur :</span><span class="sxs-lookup"><span data-stu-id="0faaa-911">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="0faaa-912">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="0faaa-912">File Configuration Provider</span></span>

<span data-ttu-id="0faaa-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="0faaa-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="0faaa-914">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="0faaa-914">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="0faaa-915">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="0faaa-915">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="0faaa-916">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="0faaa-916">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="0faaa-917">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="0faaa-917">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="0faaa-918">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="0faaa-918">INI Configuration Provider</span></span>

<span data-ttu-id="0faaa-919"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-919">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="0faaa-920">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-920">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-921">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="0faaa-921">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="0faaa-922">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-922">Overloads permit specifying:</span></span>

* <span data-ttu-id="0faaa-923">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="0faaa-923">Whether the file is optional.</span></span>
* <span data-ttu-id="0faaa-924">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-924">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0faaa-925">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-925">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0faaa-926">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="0faaa-926">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0faaa-927">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="0faaa-927">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="0faaa-928">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-928">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0faaa-929">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-929">section0:key0</span></span>
* <span data-ttu-id="0faaa-930">section0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-930">section0:key1</span></span>
* <span data-ttu-id="0faaa-931">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="0faaa-931">section1:subsection:key</span></span>
* <span data-ttu-id="0faaa-932">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="0faaa-932">section2:subsection0:key</span></span>
* <span data-ttu-id="0faaa-933">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="0faaa-933">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="0faaa-934">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="0faaa-934">JSON Configuration Provider</span></span>

<span data-ttu-id="0faaa-935"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-935">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="0faaa-936">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-936">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-937">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-937">Overloads permit specifying:</span></span>

* <span data-ttu-id="0faaa-938">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="0faaa-938">Whether the file is optional.</span></span>
* <span data-ttu-id="0faaa-939">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-939">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0faaa-940">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-940">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0faaa-941">`AddJsonFile` est appelé automatiquement deux fois lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-941">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0faaa-942">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="0faaa-942">The method is called to load configuration from:</span></span>

* <span data-ttu-id="0faaa-943">*appSettings. json* &ndash; ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-943">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="0faaa-944">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-944">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="0faaa-945">*appSettings. {Environment}. JSON* &ndash; la version de l’environnement du fichier est chargée à partir de [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="0faaa-945">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="0faaa-946">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="0faaa-946">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="0faaa-947">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="0faaa-947">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="0faaa-948">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-948">Environment variables.</span></span>
* <span data-ttu-id="0faaa-949">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-949">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="0faaa-950">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0faaa-950">Command-line arguments.</span></span>

<span data-ttu-id="0faaa-951">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-951">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="0faaa-952">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-952">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="0faaa-953">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-953">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0faaa-954">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="0faaa-954">**Example**</span></span>

<span data-ttu-id="0faaa-955">L’exemple d’application tire parti de la méthode de commodité statique `CreateDefaultBuilder` pour créer l’hôte, ce qui comprend deux appels à `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="0faaa-955">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="0faaa-956">Le premier appel à `AddJsonFile` charge la configuration à partir de *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="0faaa-956">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="0faaa-957">Le deuxième appel à `AddJsonFile` charge la configuration à partir de *appSettings. { Environnement}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-957">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="0faaa-958">Pour *appSettings. Development. JSON* dans l’exemple d’application, le fichier suivant est chargé :</span><span class="sxs-lookup"><span data-stu-id="0faaa-958">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="0faaa-959">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-959">Run the sample app.</span></span> <span data-ttu-id="0faaa-960">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-960">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="0faaa-961">La sortie contient des paires clé-valeur pour la configuration en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-961">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="0faaa-962">Le niveau de journalisation de la `Logging:LogLevel:Default` de clé est `Debug` lors de l’exécution de l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0faaa-962">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="0faaa-963">Exécutez à nouveau l’exemple d’application dans l’environnement de production :</span><span class="sxs-lookup"><span data-stu-id="0faaa-963">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="0faaa-964">Ouvrez le fichier *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="0faaa-964">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="0faaa-965">Dans le profil `ConfigurationSample`, remplacez la valeur de la variable d’environnement `ASPNETCORE_ENVIRONMENT` par `Production`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-965">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="0faaa-966">Enregistrez le fichier et exécutez l’application avec `dotnet run` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="0faaa-966">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="0faaa-967">Paramètres dans *appSettings. Development. JSON* ne remplace plus les paramètres dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-967">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="0faaa-968">Le niveau de journalisation de la clé `Logging:LogLevel:Default` est `Warning`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-968">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="0faaa-969">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="0faaa-969">XML Configuration Provider</span></span>

<span data-ttu-id="0faaa-970"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0faaa-970">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="0faaa-971">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-971">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-972">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-972">Overloads permit specifying:</span></span>

* <span data-ttu-id="0faaa-973">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="0faaa-973">Whether the file is optional.</span></span>
* <span data-ttu-id="0faaa-974">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-974">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="0faaa-975">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-975">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="0faaa-976">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-976">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="0faaa-977">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-977">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="0faaa-978">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="0faaa-978">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="0faaa-979">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="0faaa-979">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="0faaa-980">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-980">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0faaa-981">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-981">section0:key0</span></span>
* <span data-ttu-id="0faaa-982">section0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-982">section0:key1</span></span>
* <span data-ttu-id="0faaa-983">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-983">section1:key0</span></span>
* <span data-ttu-id="0faaa-984">section1:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-984">section1:key1</span></span>

<span data-ttu-id="0faaa-985">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="0faaa-985">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="0faaa-986">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-986">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0faaa-987">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-987">section:section0:key:key0</span></span>
* <span data-ttu-id="0faaa-988">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-988">section:section0:key:key1</span></span>
* <span data-ttu-id="0faaa-989">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-989">section:section1:key:key0</span></span>
* <span data-ttu-id="0faaa-990">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-990">section:section1:key:key1</span></span>

<span data-ttu-id="0faaa-991">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="0faaa-991">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="0faaa-992">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-992">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="0faaa-993">key:attribute</span><span class="sxs-lookup"><span data-stu-id="0faaa-993">key:attribute</span></span>
* <span data-ttu-id="0faaa-994">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="0faaa-994">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="0faaa-995">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="0faaa-995">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="0faaa-996">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-996">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="0faaa-997">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-997">The key is the file name.</span></span> <span data-ttu-id="0faaa-998">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="0faaa-998">The value contains the file's contents.</span></span> <span data-ttu-id="0faaa-999">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="0faaa-999">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="0faaa-1000">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1000">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="0faaa-1001">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1001">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="0faaa-1002">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1002">Overloads permit specifying:</span></span>

* <span data-ttu-id="0faaa-1003">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1003">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="0faaa-1004">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1004">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="0faaa-1005">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1005">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="0faaa-1006">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1006">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="0faaa-1007">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1007">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="0faaa-1008">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="0faaa-1008">Memory Configuration Provider</span></span>

<span data-ttu-id="0faaa-1009">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1009">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="0faaa-1010">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1010">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="0faaa-1011">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1011">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="0faaa-1012">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1012">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="0faaa-1013">Dans l’exemple suivant, un dictionnaire de configuration est créé :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1013">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="0faaa-1014">Le dictionnaire est utilisé avec un appel à `AddInMemoryCollection` pour fournir la configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1014">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="0faaa-1015">GetValue</span><span class="sxs-lookup"><span data-stu-id="0faaa-1015">GetValue</span></span>

<span data-ttu-id="0faaa-1016">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur unique de la configuration avec une clé spécifiée et la convertit en type non collection spécifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="0faaa-1017">Une surcharge accepte une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1017">An overload accepts a default value.</span></span>

<span data-ttu-id="0faaa-1018">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1018">The following example:</span></span>

* <span data-ttu-id="0faaa-1019">Extrait la valeur de chaîne de la configuration avec la clé `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1019">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="0faaa-1020">Si `NumberKey` est introuvable dans les clés de configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1020">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="0faaa-1021">Tape la valeur comme `int`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1021">Types the value as an `int`.</span></span>
* <span data-ttu-id="0faaa-1022">Stocke la valeur dans la propriété `NumberConfig` pour une utilisation par la page.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1022">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="0faaa-1023">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="0faaa-1023">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="0faaa-1024">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1024">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="0faaa-1025">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1025">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="0faaa-1026">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1026">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="0faaa-1027">section0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1027">section0:key0</span></span>
* <span data-ttu-id="0faaa-1028">section0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1028">section0:key1</span></span>
* <span data-ttu-id="0faaa-1029">section1:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1029">section1:key0</span></span>
* <span data-ttu-id="0faaa-1030">section1:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1030">section1:key1</span></span>
* <span data-ttu-id="0faaa-1031">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1031">section2:subsection0:key0</span></span>
* <span data-ttu-id="0faaa-1032">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1032">section2:subsection0:key1</span></span>
* <span data-ttu-id="0faaa-1033">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1033">section2:subsection1:key0</span></span>
* <span data-ttu-id="0faaa-1034">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1034">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="0faaa-1035">GetSection</span><span class="sxs-lookup"><span data-stu-id="0faaa-1035">GetSection</span></span>

<span data-ttu-id="0faaa-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="0faaa-1037">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1037">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="0faaa-1038">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1038">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="0faaa-1039">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1039">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="0faaa-1040">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1040">`GetSection` never returns `null`.</span></span> <span data-ttu-id="0faaa-1041">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1041">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="0faaa-1042">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1042">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="0faaa-1043"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1043">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="0faaa-1044">GetChildren</span><span class="sxs-lookup"><span data-stu-id="0faaa-1044">GetChildren</span></span>

<span data-ttu-id="0faaa-1045">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1045">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="0faaa-1046">Exists</span><span class="sxs-lookup"><span data-stu-id="0faaa-1046">Exists</span></span>

<span data-ttu-id="0faaa-1047">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="0faaa-1048">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1048">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="0faaa-1049">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="0faaa-1049">Bind to a class</span></span>

<span data-ttu-id="0faaa-1050">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1050">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="0faaa-1051">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1051">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="0faaa-1052">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="0faaa-1052">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="0faaa-1053">Le Binder lie les valeurs à toutes les propriétés publiques en lecture/écriture du type fourni.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1053">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="0faaa-1054">Les champs ne sont **pas** liés.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1054">Fields are **not** bound.</span></span>

<span data-ttu-id="0faaa-1055">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1055">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="0faaa-1056">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1056">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="0faaa-1057">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1057">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="0faaa-1058">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-1058">Key</span></span>                   | <span data-ttu-id="0faaa-1059">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-1059">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="0faaa-1060">starship:name</span><span class="sxs-lookup"><span data-stu-id="0faaa-1060">starship:name</span></span>         | <span data-ttu-id="0faaa-1061">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="0faaa-1061">USS Enterprise</span></span>                                    |
| <span data-ttu-id="0faaa-1062">starship:registry</span><span class="sxs-lookup"><span data-stu-id="0faaa-1062">starship:registry</span></span>     | <span data-ttu-id="0faaa-1063">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="0faaa-1063">NCC-1701</span></span>                                          |
| <span data-ttu-id="0faaa-1064">starship:class</span><span class="sxs-lookup"><span data-stu-id="0faaa-1064">starship:class</span></span>        | <span data-ttu-id="0faaa-1065">Constitution</span><span class="sxs-lookup"><span data-stu-id="0faaa-1065">Constitution</span></span>                                      |
| <span data-ttu-id="0faaa-1066">starship:length</span><span class="sxs-lookup"><span data-stu-id="0faaa-1066">starship:length</span></span>       | <span data-ttu-id="0faaa-1067">304.8</span><span class="sxs-lookup"><span data-stu-id="0faaa-1067">304.8</span></span>                                             |
| <span data-ttu-id="0faaa-1068">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="0faaa-1068">starship:commissioned</span></span> | <span data-ttu-id="0faaa-1069">False</span><span class="sxs-lookup"><span data-stu-id="0faaa-1069">False</span></span>                                             |
| <span data-ttu-id="0faaa-1070">trademark</span><span class="sxs-lookup"><span data-stu-id="0faaa-1070">trademark</span></span>             | <span data-ttu-id="0faaa-1071">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="0faaa-1071">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="0faaa-1072">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1072">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="0faaa-1073">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1073">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="0faaa-1074">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1074">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="0faaa-1075">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1075">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="0faaa-1076">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="0faaa-1076">Bind to an object graph</span></span>

<span data-ttu-id="0faaa-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="0faaa-1078">Comme pour la liaison d’un objet simple, seules les propriétés accessibles en lecture/écriture publiques sont liées.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1078">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="0faaa-1079">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1079">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="0faaa-1080">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1080">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="0faaa-1081">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1081">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="0faaa-1082">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1082">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="0faaa-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="0faaa-1084">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1084">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="0faaa-1085">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1085">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="0faaa-1086">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="0faaa-1086">Bind an array to a class</span></span>

<span data-ttu-id="0faaa-1087">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="0faaa-1087">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="0faaa-1088"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1088">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="0faaa-1089">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer une liaison de tableau à un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1089">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="0faaa-1090">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1090">Binding is provided by convention.</span></span> <span data-ttu-id="0faaa-1091">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1091">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="0faaa-1092">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="0faaa-1092">**In-memory array processing**</span></span>

<span data-ttu-id="0faaa-1093">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1093">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="0faaa-1094">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-1094">Key</span></span>             | <span data-ttu-id="0faaa-1095">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-1095">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="0faaa-1096">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1096">array:entries:0</span></span> | <span data-ttu-id="0faaa-1097">value0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1097">value0</span></span> |
| <span data-ttu-id="0faaa-1098">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1098">array:entries:1</span></span> | <span data-ttu-id="0faaa-1099">valeur1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1099">value1</span></span> |
| <span data-ttu-id="0faaa-1100">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="0faaa-1100">array:entries:2</span></span> | <span data-ttu-id="0faaa-1101">valeur2</span><span class="sxs-lookup"><span data-stu-id="0faaa-1101">value2</span></span> |
| <span data-ttu-id="0faaa-1102">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="0faaa-1102">array:entries:4</span></span> | <span data-ttu-id="0faaa-1103">value4</span><span class="sxs-lookup"><span data-stu-id="0faaa-1103">value4</span></span> |
| <span data-ttu-id="0faaa-1104">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="0faaa-1104">array:entries:5</span></span> | <span data-ttu-id="0faaa-1105">value5</span><span class="sxs-lookup"><span data-stu-id="0faaa-1105">value5</span></span> |

<span data-ttu-id="0faaa-1106">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1106">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="0faaa-1107">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1107">The array skips a value for index &num;3.</span></span> <span data-ttu-id="0faaa-1108">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1108">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="0faaa-1109">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1109">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="0faaa-1110">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1110">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="0faaa-1111">La syntaxe [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1111">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="0faaa-1112">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1112">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="0faaa-1113">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="0faaa-1113">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="0faaa-1114">`ArrayExample.Entries` Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-1114">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="0faaa-1115">0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1115">0</span></span>                            | <span data-ttu-id="0faaa-1116">value0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1116">value0</span></span>                       |
| <span data-ttu-id="0faaa-1117">1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1117">1</span></span>                            | <span data-ttu-id="0faaa-1118">valeur1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1118">value1</span></span>                       |
| <span data-ttu-id="0faaa-1119">2</span><span class="sxs-lookup"><span data-stu-id="0faaa-1119">2</span></span>                            | <span data-ttu-id="0faaa-1120">valeur2</span><span class="sxs-lookup"><span data-stu-id="0faaa-1120">value2</span></span>                       |
| <span data-ttu-id="0faaa-1121">3</span><span class="sxs-lookup"><span data-stu-id="0faaa-1121">3</span></span>                            | <span data-ttu-id="0faaa-1122">value4</span><span class="sxs-lookup"><span data-stu-id="0faaa-1122">value4</span></span>                       |
| <span data-ttu-id="0faaa-1123">4</span><span class="sxs-lookup"><span data-stu-id="0faaa-1123">4</span></span>                            | <span data-ttu-id="0faaa-1124">value5</span><span class="sxs-lookup"><span data-stu-id="0faaa-1124">value5</span></span>                       |

<span data-ttu-id="0faaa-1125">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1125">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="0faaa-1126">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1126">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="0faaa-1127">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1127">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="0faaa-1128">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1128">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="0faaa-1129">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1129">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="0faaa-1130">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1130">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="0faaa-1131">Dans `ConfigureAppConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1131">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="0faaa-1132">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1132">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="0faaa-1133">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-1133">Key</span></span>             | <span data-ttu-id="0faaa-1134">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-1134">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="0faaa-1135">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="0faaa-1135">array:entries:3</span></span> | <span data-ttu-id="0faaa-1136">valeur3</span><span class="sxs-lookup"><span data-stu-id="0faaa-1136">value3</span></span> |

<span data-ttu-id="0faaa-1137">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1137">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="0faaa-1138">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="0faaa-1138">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="0faaa-1139">`ArrayExample.Entries` Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-1139">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="0faaa-1140">0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1140">0</span></span>                            | <span data-ttu-id="0faaa-1141">value0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1141">value0</span></span>                       |
| <span data-ttu-id="0faaa-1142">1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1142">1</span></span>                            | <span data-ttu-id="0faaa-1143">valeur1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1143">value1</span></span>                       |
| <span data-ttu-id="0faaa-1144">2</span><span class="sxs-lookup"><span data-stu-id="0faaa-1144">2</span></span>                            | <span data-ttu-id="0faaa-1145">valeur2</span><span class="sxs-lookup"><span data-stu-id="0faaa-1145">value2</span></span>                       |
| <span data-ttu-id="0faaa-1146">3</span><span class="sxs-lookup"><span data-stu-id="0faaa-1146">3</span></span>                            | <span data-ttu-id="0faaa-1147">valeur3</span><span class="sxs-lookup"><span data-stu-id="0faaa-1147">value3</span></span>                       |
| <span data-ttu-id="0faaa-1148">4</span><span class="sxs-lookup"><span data-stu-id="0faaa-1148">4</span></span>                            | <span data-ttu-id="0faaa-1149">value4</span><span class="sxs-lookup"><span data-stu-id="0faaa-1149">value4</span></span>                       |
| <span data-ttu-id="0faaa-1150">5</span><span class="sxs-lookup"><span data-stu-id="0faaa-1150">5</span></span>                            | <span data-ttu-id="0faaa-1151">value5</span><span class="sxs-lookup"><span data-stu-id="0faaa-1151">value5</span></span>                       |

<span data-ttu-id="0faaa-1152">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="0faaa-1152">**JSON array processing**</span></span>

<span data-ttu-id="0faaa-1153">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1153">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="0faaa-1154">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1154">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="0faaa-1155">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1155">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="0faaa-1156">Clé</span><span class="sxs-lookup"><span data-stu-id="0faaa-1156">Key</span></span>                     | <span data-ttu-id="0faaa-1157">Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-1157">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="0faaa-1158">json_array:key</span><span class="sxs-lookup"><span data-stu-id="0faaa-1158">json_array:key</span></span>          | <span data-ttu-id="0faaa-1159">valueA</span><span class="sxs-lookup"><span data-stu-id="0faaa-1159">valueA</span></span> |
| <span data-ttu-id="0faaa-1160">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1160">json_array:subsection:0</span></span> | <span data-ttu-id="0faaa-1161">valueB</span><span class="sxs-lookup"><span data-stu-id="0faaa-1161">valueB</span></span> |
| <span data-ttu-id="0faaa-1162">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1162">json_array:subsection:1</span></span> | <span data-ttu-id="0faaa-1163">valueC</span><span class="sxs-lookup"><span data-stu-id="0faaa-1163">valueC</span></span> |
| <span data-ttu-id="0faaa-1164">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="0faaa-1164">json_array:subsection:2</span></span> | <span data-ttu-id="0faaa-1165">valueD</span><span class="sxs-lookup"><span data-stu-id="0faaa-1165">valueD</span></span> |

<span data-ttu-id="0faaa-1166">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1166">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="0faaa-1167">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1167">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="0faaa-1168">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1168">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="0faaa-1169">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="0faaa-1169">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="0faaa-1170">`JsonArrayExample.Subsection` Valeur</span><span class="sxs-lookup"><span data-stu-id="0faaa-1170">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="0faaa-1171">0</span><span class="sxs-lookup"><span data-stu-id="0faaa-1171">0</span></span>                                   | <span data-ttu-id="0faaa-1172">valueB</span><span class="sxs-lookup"><span data-stu-id="0faaa-1172">valueB</span></span>                              |
| <span data-ttu-id="0faaa-1173">1</span><span class="sxs-lookup"><span data-stu-id="0faaa-1173">1</span></span>                                   | <span data-ttu-id="0faaa-1174">valueC</span><span class="sxs-lookup"><span data-stu-id="0faaa-1174">valueC</span></span>                              |
| <span data-ttu-id="0faaa-1175">2</span><span class="sxs-lookup"><span data-stu-id="0faaa-1175">2</span></span>                                   | <span data-ttu-id="0faaa-1176">valueD</span><span class="sxs-lookup"><span data-stu-id="0faaa-1176">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="0faaa-1177">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="0faaa-1177">Custom configuration provider</span></span>

<span data-ttu-id="0faaa-1178">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="0faaa-1178">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="0faaa-1179">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1179">The provider has the following characteristics:</span></span>

* <span data-ttu-id="0faaa-1180">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1180">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="0faaa-1181">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1181">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="0faaa-1182">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1182">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="0faaa-1183">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1183">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="0faaa-1184">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1184">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="0faaa-1185">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1185">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="0faaa-1186">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1186">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="0faaa-1187">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1187">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="0faaa-1188">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="0faaa-1189">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1189">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="0faaa-1190">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="0faaa-1191">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1191">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="0faaa-1192">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1192">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="0faaa-1193">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="0faaa-1194">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1194">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="0faaa-1195">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1195">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="0faaa-1196">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1196">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="0faaa-1197">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="0faaa-1197">Access configuration during startup</span></span>

<span data-ttu-id="0faaa-1198">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1198">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0faaa-1199">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1199">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="0faaa-1200">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="0faaa-1200">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="0faaa-1201">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="0faaa-1201">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="0faaa-1202">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1202">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="0faaa-1203">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1203">In a Razor Pages page:</span></span>

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

<span data-ttu-id="0faaa-1204">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="0faaa-1204">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="0faaa-1205">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="0faaa-1205">Add configuration from an external assembly</span></span>

<span data-ttu-id="0faaa-1206">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1206">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="0faaa-1207">Pour plus d’informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0faaa-1207">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0faaa-1208">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0faaa-1208">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
