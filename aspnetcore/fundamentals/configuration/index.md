---
title: Configuration dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 3dcabae3f76d81e641057c346dbb9097c2da42c7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656330"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="dbdfd-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dbdfd-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="dbdfd-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="dbdfd-105">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="dbdfd-106">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="dbdfd-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dbdfd-107">Azure Key Vault</span></span>
* <span data-ttu-id="dbdfd-108">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-108">Azure App Configuration</span></span>
* <span data-ttu-id="dbdfd-109">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-109">Command-line arguments</span></span>
* <span data-ttu-id="dbdfd-110">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="dbdfd-111">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-111">Directory files</span></span>
* <span data-ttu-id="dbdfd-112">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-112">Environment variables</span></span>
* <span data-ttu-id="dbdfd-113">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-113">In-memory .NET objects</span></span>
* <span data-ttu-id="dbdfd-114">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="dbdfd-114">Settings files</span></span>

<span data-ttu-id="dbdfd-115">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le framework.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

<span data-ttu-id="dbdfd-116">Les exemples de code qui suivent et dans l’échantillon d’application utilisent l’espace de noms <xref:Microsoft.Extensions.Configuration> :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="dbdfd-117">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="dbdfd-118">Les options utilisent des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-118">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="dbdfd-119">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-119">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="dbdfd-120">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dbdfd-120">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="dbdfd-121">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="dbdfd-121">Host versus app configuration</span></span>

<span data-ttu-id="dbdfd-122">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="dbdfd-123">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="dbdfd-124">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="dbdfd-125">Les paires clé-valeur de la configuration de l’hôte sont également incluses dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-125">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="dbdfd-126">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="dbdfd-127">Autre configuration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-127">Other configuration</span></span>

<span data-ttu-id="dbdfd-128">Cette rubrique se rapporte uniquement à la configuration de l' *application*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-128">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="dbdfd-129">D’autres aspects de l’exécution et de l’hébergement des applications ASP.NET Core sont configurés à l’aide des fichiers de configuration non traités dans cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-129">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="dbdfd-130">*Launch. json*/*launchSettings. JSON* sont des fichiers de configuration d’outils pour l’environnement de développement, décrits ci-après :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-130">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="dbdfd-131">Dans <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-131">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="dbdfd-132">Dans l’ensemble de la documentation dans lequel les fichiers sont utilisés pour configurer des applications ASP.NET Core pour les scénarios de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-132">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="dbdfd-133">*Web. config* est un fichier de configuration de serveur, décrit dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-133">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="dbdfd-134">Pour plus d’informations sur la migration de la configuration d’application à partir de versions antérieures de ASP.NET, consultez <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-134">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="dbdfd-135">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="dbdfd-135">Default configuration</span></span>

<span data-ttu-id="dbdfd-136">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-136">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="dbdfd-137">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-137">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="dbdfd-138">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-138">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="dbdfd-139">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte Web](xref:fundamentals/host/web-host), consultez la [version ASP.NET Core 2.2. de cette rubrique](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-139">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="dbdfd-140">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="dbdfd-141">Des variables d’environnement préfixées avec `DOTNET_` (par exemple, `DOTNET_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-141">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="dbdfd-142">Le préfixe (`DOTNET_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-142">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="dbdfd-143">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-143">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="dbdfd-144">La configuration par défaut de l’hôte Web est établie (`ConfigureWebHostDefaults`) :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-144">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="dbdfd-145">Kestrel est utilisé comme serveur web et configuré à l’aide des fournisseurs de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-145">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="dbdfd-146">Ajoutez l’intergiciel de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-146">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="dbdfd-147">Ajoutez l’intergiciel d’en-têtes transférés si la variable d'environnement `ASPNETCORE_FORWARDEDHEADERS_ENABLED` est définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-147">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="dbdfd-148">Activez l’intégration d’IIS.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-148">Enable IIS integration.</span></span>
* <span data-ttu-id="dbdfd-149">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-149">App configuration is provided from:</span></span>
  * <span data-ttu-id="dbdfd-150">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-150">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="dbdfd-151">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-151">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="dbdfd-152">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-152">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="dbdfd-153">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-153">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="dbdfd-154">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-154">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="dbdfd-155">Sécurité</span><span class="sxs-lookup"><span data-stu-id="dbdfd-155">Security</span></span>

<span data-ttu-id="dbdfd-156">Adoptez les pratiques suivantes pour sécuriser les données de configuration sensibles :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-156">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="dbdfd-157">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-157">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="dbdfd-158">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-158">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="dbdfd-159">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="dbdfd-160">Pour plus d'informations, voir les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-160">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="dbdfd-161"><xref:security/app-secrets> &ndash; fournit des conseils sur l’utilisation de variables d’environnement pour stocker des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-161"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="dbdfd-162">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-162">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="dbdfd-163">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-163">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="dbdfd-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stocke en toute sécurité des secrets d’application pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="dbdfd-165">Pour plus d’informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-165">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="dbdfd-166">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="dbdfd-166">Hierarchical configuration data</span></span>

<span data-ttu-id="dbdfd-167">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-167">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="dbdfd-168">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-168">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="dbdfd-169">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-169">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="dbdfd-170">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-170">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="dbdfd-171">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-171">section0:key0</span></span>
* <span data-ttu-id="dbdfd-172">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-172">section0:key1</span></span>
* <span data-ttu-id="dbdfd-173">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-173">section1:key0</span></span>
* <span data-ttu-id="dbdfd-174">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-174">section1:key1</span></span>

<span data-ttu-id="dbdfd-175">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="dbdfd-176">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-176">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="dbdfd-177">Conventions</span><span class="sxs-lookup"><span data-stu-id="dbdfd-177">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="dbdfd-178">Sources et fournisseurs</span><span class="sxs-lookup"><span data-stu-id="dbdfd-178">Sources and providers</span></span>

<span data-ttu-id="dbdfd-179">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-179">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="dbdfd-180">Les fournisseurs de configuration qui implémentent la détection des modifications peuvent recharger la configuration lorsqu’un paramètre sous-jacent est modifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-180">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="dbdfd-181">Par exemple, le fournisseur de configuration de fichier (décrit plus loin dans cette rubrique) et le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) implémentent la détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-181">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="dbdfd-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="dbdfd-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> peuvent être injectées dans un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> ou MVC <xref:Microsoft.AspNetCore.Mvc.Controller> pour obtenir la configuration de la classe.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="dbdfd-184">Dans les exemples suivants, le champ `_config` est utilisé pour accéder aux valeurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-184">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="dbdfd-185">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-185">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="dbdfd-186">Keys</span><span class="sxs-lookup"><span data-stu-id="dbdfd-186">Keys</span></span>

<span data-ttu-id="dbdfd-187">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-187">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="dbdfd-188">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-188">Keys are case-insensitive.</span></span> <span data-ttu-id="dbdfd-189">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-189">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="dbdfd-190">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-190">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="dbdfd-191">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="dbdfd-191">Hierarchical keys</span></span>
  * <span data-ttu-id="dbdfd-192">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-192">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="dbdfd-193">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-193">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="dbdfd-194">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et automatiquement transformé en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-194">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="dbdfd-195">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-195">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="dbdfd-196">Écrivez du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-196">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="dbdfd-197"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-197">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="dbdfd-198">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-198">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="dbdfd-199">Valeurs</span><span class="sxs-lookup"><span data-stu-id="dbdfd-199">Values</span></span>

<span data-ttu-id="dbdfd-200">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-200">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="dbdfd-201">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-201">Values are strings.</span></span>
* <span data-ttu-id="dbdfd-202">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-202">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="dbdfd-203">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="dbdfd-203">Providers</span></span>

<span data-ttu-id="dbdfd-204">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-204">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="dbdfd-205">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-205">Provider</span></span> | <span data-ttu-id="dbdfd-206">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="dbdfd-206">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="dbdfd-207">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-207">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="dbdfd-208">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dbdfd-208">Azure Key Vault</span></span> |
| <span data-ttu-id="dbdfd-209">[Fournisseur Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentation Azure)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-209">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="dbdfd-210">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-210">Azure App Configuration</span></span> |
| [<span data-ttu-id="dbdfd-211">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="dbdfd-212">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-212">Command-line parameters</span></span> |
| [<span data-ttu-id="dbdfd-213">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="dbdfd-214">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="dbdfd-214">Custom source</span></span> |
| [<span data-ttu-id="dbdfd-215">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="dbdfd-216">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-216">Environment variables</span></span> |
| [<span data-ttu-id="dbdfd-217">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="dbdfd-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="dbdfd-218">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="dbdfd-219">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="dbdfd-219">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="dbdfd-220">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-220">Directory files</span></span> |
| [<span data-ttu-id="dbdfd-221">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-221">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="dbdfd-222">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-222">In-memory collections</span></span> |
| <span data-ttu-id="dbdfd-223">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-223">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="dbdfd-224">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-224">File in the user profile directory</span></span> |

<span data-ttu-id="dbdfd-225">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-225">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="dbdfd-226">Les fournisseurs de configuration décrits dans cette rubrique sont décrits par ordre alphabétique, et non pas dans l’ordre dans lequel le code les réorganise.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-226">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="dbdfd-227">Commandez des fournisseurs de configuration dans le code pour répondre aux priorités des sources de configuration sous-jacentes requises par l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-227">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="dbdfd-228">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-228">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="dbdfd-229">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-229">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="dbdfd-230">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dbdfd-230">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="dbdfd-231">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-231">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="dbdfd-232">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-232">Environment variables</span></span>
1. <span data-ttu-id="dbdfd-233">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-233">Command-line arguments</span></span>

<span data-ttu-id="dbdfd-234">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-234">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="dbdfd-235">La séquence de fournisseurs précédente est utilisée lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-235">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dbdfd-236">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-236">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="dbdfd-237">Configurer le générateur d’ordinateur hôte avec ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-237">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="dbdfd-238">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> et fournissez la configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-238">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="dbdfd-239">`ConfigureHostConfiguration` est utilisé pour initialiser <xref:Microsoft.Extensions.Hosting.IHostEnvironment> pour une utilisation ultérieure dans le processus de génération.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-239">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="dbdfd-240">`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-240">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="dbdfd-241">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-241">ConfigureAppConfiguration</span></span>

<span data-ttu-id="dbdfd-242">Appelez `ConfigureAppConfiguration` quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-242">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="dbdfd-243">Remplacez la configuration précédente par des arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-243">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="dbdfd-244">Pour fournir une configuration d’application pouvant être remplacée par des arguments de ligne de commande, appelez `AddCommandLine` en dernier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-244">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="dbdfd-245">Supprimer les fournisseurs ajoutés par CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="dbdfd-245">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="dbdfd-246">Pour supprimer les fournisseurs ajoutés par `CreateDefaultBuilder`, appelez d’abord [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) sur [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-246">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="dbdfd-247">Utiliser la configuration lors du démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="dbdfd-247">Consume configuration during app startup</span></span>

<span data-ttu-id="dbdfd-248">La configuration fournie à l’application dans `ConfigureAppConfiguration` est disponible lors du démarrage de l’application, notamment `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-248">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="dbdfd-249">Pour plus d’informations, consultez la section [Accéder à la configuration lors du démarrage](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-249">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="dbdfd-250">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-250">Command-line Configuration Provider</span></span>

<span data-ttu-id="dbdfd-251"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-251">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="dbdfd-252">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-252">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-253">`AddCommandLine` est appelé automatiquement quand `CreateDefaultBuilder(string [])` est appelé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-253">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="dbdfd-254">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-254">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="dbdfd-255">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-255">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dbdfd-256">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-256">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="dbdfd-257">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-257">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="dbdfd-258">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-258">Environment variables.</span></span>

<span data-ttu-id="dbdfd-259">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-259">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="dbdfd-260">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-260">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="dbdfd-261">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-261">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="dbdfd-262">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-262">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="dbdfd-263">Pour les applications basées sur les modèles ASP.NET Core, `AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-263">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dbdfd-264">Pour ajouter des fournisseurs de configuration supplémentaires et conserver la capacité de remplacer leur configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-264">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="dbdfd-265">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-265">**Example**</span></span>

<span data-ttu-id="dbdfd-266">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-266">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="dbdfd-267">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-267">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="dbdfd-268">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-268">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="dbdfd-269">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-269">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dbdfd-270">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-270">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="dbdfd-271">Arguments</span><span class="sxs-lookup"><span data-stu-id="dbdfd-271">Arguments</span></span>

<span data-ttu-id="dbdfd-272">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-272">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="dbdfd-273">La valeur n’est pas requise si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-273">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="dbdfd-274">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-274">Key prefix</span></span>               | <span data-ttu-id="dbdfd-275">Exemple</span><span class="sxs-lookup"><span data-stu-id="dbdfd-275">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="dbdfd-276">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="dbdfd-276">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="dbdfd-277">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-277">Two dashes (`--`)</span></span>        | <span data-ttu-id="dbdfd-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="dbdfd-279">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-279">Forward slash (`/`)</span></span>      | <span data-ttu-id="dbdfd-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="dbdfd-281">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-281">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="dbdfd-282">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-282">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="dbdfd-283">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-283">Switch mappings</span></span>

<span data-ttu-id="dbdfd-284">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-284">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="dbdfd-285">Lors de la génération manuelle d’une configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fournissez un dictionnaire de remplacements de commutateur dans la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-285">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="dbdfd-286">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-286">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="dbdfd-287">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-287">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="dbdfd-288">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-288">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="dbdfd-289">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-289">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="dbdfd-290">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-290">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="dbdfd-291">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-291">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="dbdfd-292">Créez un dictionnaire des mappages de commutateurs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-292">Create a switch mappings dictionary.</span></span> <span data-ttu-id="dbdfd-293">Dans l’exemple suivant, deux mappages de commutateurs sont créés :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-293">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="dbdfd-294">Lorsque l’hôte est généré, appelez `AddCommandLine` avec le dictionnaire de mappages de commutateurs :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-294">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="dbdfd-295">Pour les applications qui utilisent des mappages de commutateurs, l’appel à `CreateDefaultBuilder` ne doit pas passer d’arguments.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-295">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="dbdfd-296">L’appel `CreateDefaultBuilder` de la méthode `AddCommandLine` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-296">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dbdfd-297">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `ConfigurationBuilder` de la méthode `AddCommandLine` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-297">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="dbdfd-298">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-298">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="dbdfd-299">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-299">Key</span></span>       | <span data-ttu-id="dbdfd-300">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-300">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="dbdfd-301">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-301">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="dbdfd-302">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-302">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="dbdfd-303">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-303">Key</span></span>               | <span data-ttu-id="dbdfd-304">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-304">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="dbdfd-305">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-305">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="dbdfd-306"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-306">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="dbdfd-307">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-307">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="dbdfd-308">[Azure App service](https://azure.microsoft.com/services/app-service/) permet de définir des variables d’environnement dans le portail Azure qui peuvent remplacer la configuration de l’application à l’aide du fournisseur de configuration des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="dbdfd-309">Pour plus d’informations, consultez [Azure Apps : remplacer la configuration de l’application à l’aide du portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-309">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="dbdfd-310">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `DOTNET_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte générique](xref:fundamentals/host/generic-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-310">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="dbdfd-311">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-311">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="dbdfd-312">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-312">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dbdfd-313">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-313">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="dbdfd-314">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-314">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="dbdfd-315">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-315">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="dbdfd-316">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-316">Command-line arguments.</span></span>

<span data-ttu-id="dbdfd-317">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-317">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="dbdfd-318">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-318">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="dbdfd-319">Pour fournir la configuration d’application à partir de variables d’environnement supplémentaires, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddEnvironmentVariables` avec le préfixe :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-319">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="dbdfd-320">Appelez `AddEnvironmentVariables` dernier pour autoriser les variables d’environnement avec le préfixe spécifié à substituer des valeurs d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-320">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="dbdfd-321">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-321">**Example**</span></span>

<span data-ttu-id="dbdfd-322">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-322">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="dbdfd-323">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-323">Run the sample app.</span></span> <span data-ttu-id="dbdfd-324">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-324">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dbdfd-325">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-325">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="dbdfd-326">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-326">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="dbdfd-327">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-327">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="dbdfd-328">Consultez le fichier *Pages/Index.cshtml.cs* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-328">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="dbdfd-329">Pour exposer toutes les variables d’environnement disponibles pour l’application, remplacez la `FilteredConfiguration` dans *pages/index. cshtml. cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-329">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="dbdfd-330">Préfixes</span><span class="sxs-lookup"><span data-stu-id="dbdfd-330">Prefixes</span></span>

<span data-ttu-id="dbdfd-331">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe à la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-331">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="dbdfd-332">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-332">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="dbdfd-333">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-333">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="dbdfd-334">Lorsque le générateur d’hôte est créé, la configuration de l’hôte est fournie par des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-334">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="dbdfd-335">Pour plus d’informations sur le préfixe utilisé pour ces variables d’environnement, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-335">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="dbdfd-336">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-336">**Connection string prefixes**</span></span>

<span data-ttu-id="dbdfd-337">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-337">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="dbdfd-338">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-338">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="dbdfd-339">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="dbdfd-339">Connection string prefix</span></span> | <span data-ttu-id="dbdfd-340">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-340">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="dbdfd-341">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-341">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="dbdfd-342">MySQL</span><span class="sxs-lookup"><span data-stu-id="dbdfd-342">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="dbdfd-343">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="dbdfd-343">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="dbdfd-344">SQL Server</span><span class="sxs-lookup"><span data-stu-id="dbdfd-344">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="dbdfd-345">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-345">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="dbdfd-346">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-346">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="dbdfd-347">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-347">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="dbdfd-348">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-348">Environment variable key</span></span> | <span data-ttu-id="dbdfd-349">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="dbdfd-349">Converted configuration key</span></span> | <span data-ttu-id="dbdfd-350">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-350">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="dbdfd-351">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-351">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="dbdfd-352">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-352">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="dbdfd-353">Valeur: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-353">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="dbdfd-354">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-354">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="dbdfd-355">Valeur: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-355">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="dbdfd-356">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-356">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="dbdfd-357">Valeur: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-357">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="dbdfd-358">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-358">**Example**</span></span>

<span data-ttu-id="dbdfd-359">Une variable d’environnement de chaîne de connexion personnalisée est créée sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-359">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="dbdfd-360">Nom &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-360">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="dbdfd-361">Valeur &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-361">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="dbdfd-362">Si `IConfiguration` est injecté et affecté à un champ nommé `_config`, lisez la valeur :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-362">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="dbdfd-363">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="dbdfd-363">File Configuration Provider</span></span>

<span data-ttu-id="dbdfd-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="dbdfd-365">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-365">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="dbdfd-366">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="dbdfd-366">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="dbdfd-367">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="dbdfd-367">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="dbdfd-368">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="dbdfd-368">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="dbdfd-369">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="dbdfd-369">INI Configuration Provider</span></span>

<span data-ttu-id="dbdfd-370"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-370">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="dbdfd-371">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-371">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-372">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-372">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="dbdfd-373">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-373">Overloads permit specifying:</span></span>

* <span data-ttu-id="dbdfd-374">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-374">Whether the file is optional.</span></span>
* <span data-ttu-id="dbdfd-375">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-375">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dbdfd-376">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-376">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dbdfd-377">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-377">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="dbdfd-378">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-378">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="dbdfd-379">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-379">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dbdfd-380">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-380">section0:key0</span></span>
* <span data-ttu-id="dbdfd-381">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-381">section0:key1</span></span>
* <span data-ttu-id="dbdfd-382">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="dbdfd-382">section1:subsection:key</span></span>
* <span data-ttu-id="dbdfd-383">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="dbdfd-383">section2:subsection0:key</span></span>
* <span data-ttu-id="dbdfd-384">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="dbdfd-384">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="dbdfd-385">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="dbdfd-385">JSON Configuration Provider</span></span>

<span data-ttu-id="dbdfd-386"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-386">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="dbdfd-387">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-387">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-388">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-388">Overloads permit specifying:</span></span>

* <span data-ttu-id="dbdfd-389">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-389">Whether the file is optional.</span></span>
* <span data-ttu-id="dbdfd-390">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-390">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dbdfd-391">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-391">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dbdfd-392">`AddJsonFile` est appelé automatiquement deux fois lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-392">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dbdfd-393">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-393">The method is called to load configuration from:</span></span>

* <span data-ttu-id="dbdfd-394">*appSettings. json* &ndash; ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-394">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="dbdfd-395">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-395">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="dbdfd-396">*appSettings. {Environment}. JSON* &ndash; la version de l’environnement du fichier est chargée à partir de [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-396">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="dbdfd-397">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-397">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="dbdfd-398">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-398">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dbdfd-399">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-399">Environment variables.</span></span>
* <span data-ttu-id="dbdfd-400">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-400">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="dbdfd-401">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-401">Command-line arguments.</span></span>

<span data-ttu-id="dbdfd-402">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-402">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="dbdfd-403">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-403">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="dbdfd-404">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-404">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="dbdfd-405">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-405">**Example**</span></span>

<span data-ttu-id="dbdfd-406">L’exemple d’application tire parti de la méthode de commodité statique `CreateDefaultBuilder` pour créer l’hôte, ce qui comprend deux appels à `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="dbdfd-406">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="dbdfd-407">Le premier appel à `AddJsonFile` charge la configuration à partir de *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="dbdfd-407">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="dbdfd-408">Le deuxième appel à `AddJsonFile` charge la configuration à partir de *appSettings. { Environnement}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-408">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="dbdfd-409">Pour *appSettings. Development. JSON* dans l’exemple d’application, le fichier suivant est chargé :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-409">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="dbdfd-410">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-410">Run the sample app.</span></span> <span data-ttu-id="dbdfd-411">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dbdfd-412">La sortie contient des paires clé-valeur pour la configuration en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-412">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="dbdfd-413">Le niveau de journalisation de la `Logging:LogLevel:Default` de clé est `Debug` lors de l’exécution de l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-413">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="dbdfd-414">Exécutez à nouveau l’exemple d’application dans l’environnement de production :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-414">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="dbdfd-415">Ouvrez le fichier *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="dbdfd-415">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="dbdfd-416">Dans le profil `ConfigurationSample`, remplacez la valeur de la variable d’environnement `ASPNETCORE_ENVIRONMENT` par `Production`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-416">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="dbdfd-417">Enregistrez le fichier et exécutez l’application avec `dotnet run` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-417">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="dbdfd-418">Paramètres dans *appSettings. Development. JSON* ne remplace plus les paramètres dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-418">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="dbdfd-419">Le niveau de journalisation de la clé `Logging:LogLevel:Default` est `Information`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-419">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="dbdfd-420">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="dbdfd-420">XML Configuration Provider</span></span>

<span data-ttu-id="dbdfd-421"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-421">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="dbdfd-422">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-422">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-423">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-423">Overloads permit specifying:</span></span>

* <span data-ttu-id="dbdfd-424">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-424">Whether the file is optional.</span></span>
* <span data-ttu-id="dbdfd-425">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-425">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dbdfd-426">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-426">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dbdfd-427">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-427">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="dbdfd-428">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-428">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="dbdfd-429">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-429">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="dbdfd-430">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-430">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="dbdfd-431">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dbdfd-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-432">section0:key0</span></span>
* <span data-ttu-id="dbdfd-433">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-433">section0:key1</span></span>
* <span data-ttu-id="dbdfd-434">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-434">section1:key0</span></span>
* <span data-ttu-id="dbdfd-435">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-435">section1:key1</span></span>

<span data-ttu-id="dbdfd-436">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-436">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="dbdfd-437">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-437">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dbdfd-438">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-438">section:section0:key:key0</span></span>
* <span data-ttu-id="dbdfd-439">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-439">section:section0:key:key1</span></span>
* <span data-ttu-id="dbdfd-440">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-440">section:section1:key:key0</span></span>
* <span data-ttu-id="dbdfd-441">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-441">section:section1:key:key1</span></span>

<span data-ttu-id="dbdfd-442">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-442">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="dbdfd-443">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-443">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dbdfd-444">key:attribute</span><span class="sxs-lookup"><span data-stu-id="dbdfd-444">key:attribute</span></span>
* <span data-ttu-id="dbdfd-445">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="dbdfd-445">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="dbdfd-446">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="dbdfd-446">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="dbdfd-447">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-447">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="dbdfd-448">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-448">The key is the file name.</span></span> <span data-ttu-id="dbdfd-449">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-449">The value contains the file's contents.</span></span> <span data-ttu-id="dbdfd-450">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-450">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="dbdfd-451">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-451">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="dbdfd-452">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-452">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="dbdfd-453">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-453">Overloads permit specifying:</span></span>

* <span data-ttu-id="dbdfd-454">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-454">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="dbdfd-455">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-455">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="dbdfd-456">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-456">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="dbdfd-457">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-457">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="dbdfd-458">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-458">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="dbdfd-459">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-459">Memory Configuration Provider</span></span>

<span data-ttu-id="dbdfd-460">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="dbdfd-461">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-462">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="dbdfd-463">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-463">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="dbdfd-464">Dans l’exemple suivant, un dictionnaire de configuration est créé :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-464">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="dbdfd-465">Le dictionnaire est utilisé avec un appel à `AddInMemoryCollection` pour fournir la configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-465">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="dbdfd-466">GetValue</span><span class="sxs-lookup"><span data-stu-id="dbdfd-466">GetValue</span></span>

<span data-ttu-id="dbdfd-467">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur unique de la configuration avec une clé spécifiée et la convertit en type non collection spécifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="dbdfd-468">Une surcharge accepte une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-468">An overload accepts a default value.</span></span>

<span data-ttu-id="dbdfd-469">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-469">The following example:</span></span>

* <span data-ttu-id="dbdfd-470">Extrait la valeur de chaîne de la configuration avec la clé `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-470">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="dbdfd-471">Si `NumberKey` est introuvable dans les clés de configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-471">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="dbdfd-472">Tape la valeur comme `int`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-472">Types the value as an `int`.</span></span>
* <span data-ttu-id="dbdfd-473">Stocke la valeur dans la propriété `NumberConfig` pour une utilisation par la page.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-473">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="dbdfd-474">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="dbdfd-474">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="dbdfd-475">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-475">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="dbdfd-476">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-476">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="dbdfd-477">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-477">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="dbdfd-478">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-478">section0:key0</span></span>
* <span data-ttu-id="dbdfd-479">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-479">section0:key1</span></span>
* <span data-ttu-id="dbdfd-480">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-480">section1:key0</span></span>
* <span data-ttu-id="dbdfd-481">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-481">section1:key1</span></span>
* <span data-ttu-id="dbdfd-482">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-482">section2:subsection0:key0</span></span>
* <span data-ttu-id="dbdfd-483">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-483">section2:subsection0:key1</span></span>
* <span data-ttu-id="dbdfd-484">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-484">section2:subsection1:key0</span></span>
* <span data-ttu-id="dbdfd-485">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-485">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="dbdfd-486">GetSection</span><span class="sxs-lookup"><span data-stu-id="dbdfd-486">GetSection</span></span>

<span data-ttu-id="dbdfd-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="dbdfd-488">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="dbdfd-489">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="dbdfd-490">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="dbdfd-491">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="dbdfd-492">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="dbdfd-493">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="dbdfd-494"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="dbdfd-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="dbdfd-495">GetChildren</span></span>

<span data-ttu-id="dbdfd-496">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="dbdfd-497">Exists</span><span class="sxs-lookup"><span data-stu-id="dbdfd-497">Exists</span></span>

<span data-ttu-id="dbdfd-498">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="dbdfd-499">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="dbdfd-500">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="dbdfd-500">Bind to a class</span></span>

<span data-ttu-id="dbdfd-501">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="dbdfd-502">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="dbdfd-503">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="dbdfd-504">Le Binder lie les valeurs à toutes les propriétés publiques en lecture/écriture du type fourni.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-504">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="dbdfd-505">Les champs ne sont **pas** liés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-505">Fields are **not** bound.</span></span>

<span data-ttu-id="dbdfd-506">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-506">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="dbdfd-507">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-507">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

<span data-ttu-id="dbdfd-508">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-508">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="dbdfd-509">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-509">Key</span></span>                   | <span data-ttu-id="dbdfd-510">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-510">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="dbdfd-511">starship:name</span><span class="sxs-lookup"><span data-stu-id="dbdfd-511">starship:name</span></span>         | <span data-ttu-id="dbdfd-512">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="dbdfd-512">USS Enterprise</span></span>                                    |
| <span data-ttu-id="dbdfd-513">starship:registry</span><span class="sxs-lookup"><span data-stu-id="dbdfd-513">starship:registry</span></span>     | <span data-ttu-id="dbdfd-514">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="dbdfd-514">NCC-1701</span></span>                                          |
| <span data-ttu-id="dbdfd-515">starship:class</span><span class="sxs-lookup"><span data-stu-id="dbdfd-515">starship:class</span></span>        | <span data-ttu-id="dbdfd-516">Constitution</span><span class="sxs-lookup"><span data-stu-id="dbdfd-516">Constitution</span></span>                                      |
| <span data-ttu-id="dbdfd-517">starship:length</span><span class="sxs-lookup"><span data-stu-id="dbdfd-517">starship:length</span></span>       | <span data-ttu-id="dbdfd-518">304.8</span><span class="sxs-lookup"><span data-stu-id="dbdfd-518">304.8</span></span>                                             |
| <span data-ttu-id="dbdfd-519">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="dbdfd-519">starship:commissioned</span></span> | <span data-ttu-id="dbdfd-520">False</span><span class="sxs-lookup"><span data-stu-id="dbdfd-520">False</span></span>                                             |
| <span data-ttu-id="dbdfd-521">trademark</span><span class="sxs-lookup"><span data-stu-id="dbdfd-521">trademark</span></span>             | <span data-ttu-id="dbdfd-522">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="dbdfd-522">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="dbdfd-523">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-523">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="dbdfd-524">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-524">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="dbdfd-525">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-525">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="dbdfd-526">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-526">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="dbdfd-527">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="dbdfd-527">Bind to an object graph</span></span>

<span data-ttu-id="dbdfd-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="dbdfd-529">Comme pour la liaison d’un objet simple, seules les propriétés accessibles en lecture/écriture publiques sont liées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-529">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="dbdfd-530">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="dbdfd-531">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="dbdfd-532">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="dbdfd-533">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="dbdfd-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="dbdfd-535">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="dbdfd-536">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="dbdfd-537">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="dbdfd-537">Bind an array to a class</span></span>

<span data-ttu-id="dbdfd-538">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="dbdfd-538">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="dbdfd-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-539">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="dbdfd-540">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer une liaison de tableau à un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-540">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="dbdfd-541">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-541">Binding is provided by convention.</span></span> <span data-ttu-id="dbdfd-542">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-542">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="dbdfd-543">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-543">**In-memory array processing**</span></span>

<span data-ttu-id="dbdfd-544">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-544">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="dbdfd-545">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-545">Key</span></span>             | <span data-ttu-id="dbdfd-546">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-546">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="dbdfd-547">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-547">array:entries:0</span></span> | <span data-ttu-id="dbdfd-548">value0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-548">value0</span></span> |
| <span data-ttu-id="dbdfd-549">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-549">array:entries:1</span></span> | <span data-ttu-id="dbdfd-550">valeur1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-550">value1</span></span> |
| <span data-ttu-id="dbdfd-551">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-551">array:entries:2</span></span> | <span data-ttu-id="dbdfd-552">valeur2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-552">value2</span></span> |
| <span data-ttu-id="dbdfd-553">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-553">array:entries:4</span></span> | <span data-ttu-id="dbdfd-554">value4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-554">value4</span></span> |
| <span data-ttu-id="dbdfd-555">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-555">array:entries:5</span></span> | <span data-ttu-id="dbdfd-556">value5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-556">value5</span></span> |

<span data-ttu-id="dbdfd-557">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-557">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="dbdfd-558">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-558">The array skips a value for index &num;3.</span></span> <span data-ttu-id="dbdfd-559">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-559">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="dbdfd-560">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-560">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="dbdfd-561">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-561">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="dbdfd-562">La syntaxe [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-562">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="dbdfd-563">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-563">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="dbdfd-564">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-564">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="dbdfd-565">`ArrayExample.Entries` Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-565">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="dbdfd-566">0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-566">0</span></span>                            | <span data-ttu-id="dbdfd-567">value0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-567">value0</span></span>                       |
| <span data-ttu-id="dbdfd-568">1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-568">1</span></span>                            | <span data-ttu-id="dbdfd-569">valeur1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-569">value1</span></span>                       |
| <span data-ttu-id="dbdfd-570">2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-570">2</span></span>                            | <span data-ttu-id="dbdfd-571">valeur2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-571">value2</span></span>                       |
| <span data-ttu-id="dbdfd-572">3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-572">3</span></span>                            | <span data-ttu-id="dbdfd-573">value4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-573">value4</span></span>                       |
| <span data-ttu-id="dbdfd-574">4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-574">4</span></span>                            | <span data-ttu-id="dbdfd-575">value5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-575">value5</span></span>                       |

<span data-ttu-id="dbdfd-576">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-576">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="dbdfd-577">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-577">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="dbdfd-578">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-578">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="dbdfd-579">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-579">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="dbdfd-580">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-580">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="dbdfd-581">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-581">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="dbdfd-582">Dans `ConfigureAppConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-582">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="dbdfd-583">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-583">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="dbdfd-584">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-584">Key</span></span>             | <span data-ttu-id="dbdfd-585">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-585">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="dbdfd-586">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-586">array:entries:3</span></span> | <span data-ttu-id="dbdfd-587">valeur3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-587">value3</span></span> |

<span data-ttu-id="dbdfd-588">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-588">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="dbdfd-589">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-589">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="dbdfd-590">`ArrayExample.Entries` Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-590">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="dbdfd-591">0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-591">0</span></span>                            | <span data-ttu-id="dbdfd-592">value0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-592">value0</span></span>                       |
| <span data-ttu-id="dbdfd-593">1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-593">1</span></span>                            | <span data-ttu-id="dbdfd-594">valeur1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-594">value1</span></span>                       |
| <span data-ttu-id="dbdfd-595">2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-595">2</span></span>                            | <span data-ttu-id="dbdfd-596">valeur2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-596">value2</span></span>                       |
| <span data-ttu-id="dbdfd-597">3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-597">3</span></span>                            | <span data-ttu-id="dbdfd-598">valeur3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-598">value3</span></span>                       |
| <span data-ttu-id="dbdfd-599">4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-599">4</span></span>                            | <span data-ttu-id="dbdfd-600">value4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-600">value4</span></span>                       |
| <span data-ttu-id="dbdfd-601">5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-601">5</span></span>                            | <span data-ttu-id="dbdfd-602">value5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-602">value5</span></span>                       |

<span data-ttu-id="dbdfd-603">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-603">**JSON array processing**</span></span>

<span data-ttu-id="dbdfd-604">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-604">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="dbdfd-605">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-605">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="dbdfd-606">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-606">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="dbdfd-607">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-607">Key</span></span>                     | <span data-ttu-id="dbdfd-608">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-608">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="dbdfd-609">json_array:key</span><span class="sxs-lookup"><span data-stu-id="dbdfd-609">json_array:key</span></span>          | <span data-ttu-id="dbdfd-610">valueA</span><span class="sxs-lookup"><span data-stu-id="dbdfd-610">valueA</span></span> |
| <span data-ttu-id="dbdfd-611">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-611">json_array:subsection:0</span></span> | <span data-ttu-id="dbdfd-612">valueB</span><span class="sxs-lookup"><span data-stu-id="dbdfd-612">valueB</span></span> |
| <span data-ttu-id="dbdfd-613">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-613">json_array:subsection:1</span></span> | <span data-ttu-id="dbdfd-614">valueC</span><span class="sxs-lookup"><span data-stu-id="dbdfd-614">valueC</span></span> |
| <span data-ttu-id="dbdfd-615">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-615">json_array:subsection:2</span></span> | <span data-ttu-id="dbdfd-616">valueD</span><span class="sxs-lookup"><span data-stu-id="dbdfd-616">valueD</span></span> |

<span data-ttu-id="dbdfd-617">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-617">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="dbdfd-618">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-618">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="dbdfd-619">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-619">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="dbdfd-620">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-620">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="dbdfd-621">`JsonArrayExample.Subsection` Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-621">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="dbdfd-622">0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-622">0</span></span>                                   | <span data-ttu-id="dbdfd-623">valueB</span><span class="sxs-lookup"><span data-stu-id="dbdfd-623">valueB</span></span>                              |
| <span data-ttu-id="dbdfd-624">1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-624">1</span></span>                                   | <span data-ttu-id="dbdfd-625">valueC</span><span class="sxs-lookup"><span data-stu-id="dbdfd-625">valueC</span></span>                              |
| <span data-ttu-id="dbdfd-626">2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-626">2</span></span>                                   | <span data-ttu-id="dbdfd-627">valueD</span><span class="sxs-lookup"><span data-stu-id="dbdfd-627">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="dbdfd-628">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-628">Custom configuration provider</span></span>

<span data-ttu-id="dbdfd-629">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-629">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="dbdfd-630">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-630">The provider has the following characteristics:</span></span>

* <span data-ttu-id="dbdfd-631">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-631">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="dbdfd-632">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-632">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="dbdfd-633">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-633">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="dbdfd-634">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-634">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="dbdfd-635">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-635">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="dbdfd-636">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-636">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="dbdfd-637">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-637">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="dbdfd-638">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-638">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="dbdfd-639">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="dbdfd-640">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-640">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="dbdfd-641">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="dbdfd-642">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-642">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="dbdfd-643">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-643">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="dbdfd-644">Étant donné que les [clés de configuration ne](#keys)respectent pas la casse, le dictionnaire utilisé pour initialiser la base de données est créé avec le comparateur ne respectant pas la casse ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-644">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="dbdfd-645">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="dbdfd-646">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-646">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="dbdfd-647">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-647">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="dbdfd-648">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-648">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="dbdfd-649">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="dbdfd-649">Access configuration during startup</span></span>

<span data-ttu-id="dbdfd-650">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-650">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="dbdfd-651">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-651">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="dbdfd-652">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-652">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="dbdfd-653">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="dbdfd-653">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="dbdfd-654">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-654">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="dbdfd-655">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-655">In a Razor Pages page:</span></span>

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

<span data-ttu-id="dbdfd-656">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-656">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="dbdfd-657">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="dbdfd-657">Add configuration from an external assembly</span></span>

<span data-ttu-id="dbdfd-658">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-658">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="dbdfd-659">Pour plus d’informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-659">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dbdfd-660">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dbdfd-660">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="dbdfd-661">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-661">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="dbdfd-662">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-662">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="dbdfd-663">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dbdfd-663">Azure Key Vault</span></span>
* <span data-ttu-id="dbdfd-664">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-664">Azure App Configuration</span></span>
* <span data-ttu-id="dbdfd-665">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-665">Command-line arguments</span></span>
* <span data-ttu-id="dbdfd-666">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-666">Custom providers (installed or created)</span></span>
* <span data-ttu-id="dbdfd-667">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-667">Directory files</span></span>
* <span data-ttu-id="dbdfd-668">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-668">Environment variables</span></span>
* <span data-ttu-id="dbdfd-669">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-669">In-memory .NET objects</span></span>
* <span data-ttu-id="dbdfd-670">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="dbdfd-670">Settings files</span></span>

<span data-ttu-id="dbdfd-671">Les packages de configuration pour les scénarios de fournisseur de configuration courants ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sont inclus implicitement dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-671">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dbdfd-672">Les exemples de code qui suivent et dans l’échantillon d’application utilisent l’espace de noms <xref:Microsoft.Extensions.Configuration> :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-672">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="dbdfd-673">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-673">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="dbdfd-674">Les options utilisent des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-674">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="dbdfd-675">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-675">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="dbdfd-676">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dbdfd-676">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="dbdfd-677">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="dbdfd-677">Host versus app configuration</span></span>

<span data-ttu-id="dbdfd-678">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-678">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="dbdfd-679">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-679">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="dbdfd-680">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-680">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="dbdfd-681">Les paires clé-valeur de la configuration de l’hôte sont également incluses dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-681">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="dbdfd-682">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-682">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="dbdfd-683">Autre configuration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-683">Other configuration</span></span>

<span data-ttu-id="dbdfd-684">Cette rubrique se rapporte uniquement à la configuration de l' *application*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-684">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="dbdfd-685">D’autres aspects de l’exécution et de l’hébergement des applications ASP.NET Core sont configurés à l’aide des fichiers de configuration non traités dans cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-685">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="dbdfd-686">*Launch. json*/*launchSettings. JSON* sont des fichiers de configuration d’outils pour l’environnement de développement, décrits ci-après :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-686">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="dbdfd-687">Dans <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-687">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="dbdfd-688">Dans l’ensemble de la documentation dans lequel les fichiers sont utilisés pour configurer des applications ASP.NET Core pour les scénarios de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-688">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="dbdfd-689">*Web. config* est un fichier de configuration de serveur, décrit dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-689">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="dbdfd-690">Pour plus d’informations sur la migration de la configuration d’application à partir de versions antérieures de ASP.NET, consultez <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-690">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="dbdfd-691">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="dbdfd-691">Default configuration</span></span>

<span data-ttu-id="dbdfd-692">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-692">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="dbdfd-693">`CreateDefaultBuilder` fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-693">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="dbdfd-694">Les éléments suivants s’appliquent aux applications qui utilisent l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-694">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="dbdfd-695">Pour plus de détails sur la configuration par défaut lors de l’utilisation de l’[hôte générique](xref:fundamentals/host/generic-host), consultez la [dernière version de cette rubrique](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-695">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="dbdfd-696">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-696">Host configuration is provided from:</span></span>
  * <span data-ttu-id="dbdfd-697">Des variables d’environnement préfixées avec `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-697">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="dbdfd-698">Le préfixe (`ASPNETCORE_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-698">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="dbdfd-699">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-699">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="dbdfd-700">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-700">App configuration is provided from:</span></span>
  * <span data-ttu-id="dbdfd-701">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-701">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="dbdfd-702">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-702">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="dbdfd-703">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-703">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="dbdfd-704">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-704">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="dbdfd-705">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-705">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="dbdfd-706">Sécurité</span><span class="sxs-lookup"><span data-stu-id="dbdfd-706">Security</span></span>

<span data-ttu-id="dbdfd-707">Adoptez les pratiques suivantes pour sécuriser les données de configuration sensibles :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-707">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="dbdfd-708">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-708">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="dbdfd-709">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-709">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="dbdfd-710">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-710">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="dbdfd-711">Pour plus d'informations, voir les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-711">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="dbdfd-712"><xref:security/app-secrets> &ndash; fournit des conseils sur l’utilisation de variables d’environnement pour stocker des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-712"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="dbdfd-713">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-713">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="dbdfd-714">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-714">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="dbdfd-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stocke en toute sécurité des secrets d’application pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="dbdfd-716">Pour plus d’informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-716">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="dbdfd-717">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="dbdfd-717">Hierarchical configuration data</span></span>

<span data-ttu-id="dbdfd-718">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-718">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="dbdfd-719">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-719">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="dbdfd-720">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-720">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="dbdfd-721">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-721">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="dbdfd-722">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-722">section0:key0</span></span>
* <span data-ttu-id="dbdfd-723">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-723">section0:key1</span></span>
* <span data-ttu-id="dbdfd-724">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-724">section1:key0</span></span>
* <span data-ttu-id="dbdfd-725">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-725">section1:key1</span></span>

<span data-ttu-id="dbdfd-726">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="dbdfd-727">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-727">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="dbdfd-728">Conventions</span><span class="sxs-lookup"><span data-stu-id="dbdfd-728">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="dbdfd-729">Sources et fournisseurs</span><span class="sxs-lookup"><span data-stu-id="dbdfd-729">Sources and providers</span></span>

<span data-ttu-id="dbdfd-730">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-730">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="dbdfd-731">Les fournisseurs de configuration qui implémentent la détection des modifications peuvent recharger la configuration lorsqu’un paramètre sous-jacent est modifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-731">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="dbdfd-732">Par exemple, le fournisseur de configuration de fichier (décrit plus loin dans cette rubrique) et le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) implémentent la détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-732">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="dbdfd-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="dbdfd-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> peuvent être injectées dans un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> ou MVC <xref:Microsoft.AspNetCore.Mvc.Controller> pour obtenir la configuration de la classe.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="dbdfd-735">Dans les exemples suivants, le champ `_config` est utilisé pour accéder aux valeurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-735">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="dbdfd-736">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-736">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="dbdfd-737">Keys</span><span class="sxs-lookup"><span data-stu-id="dbdfd-737">Keys</span></span>

<span data-ttu-id="dbdfd-738">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-738">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="dbdfd-739">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-739">Keys are case-insensitive.</span></span> <span data-ttu-id="dbdfd-740">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-740">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="dbdfd-741">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-741">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="dbdfd-742">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="dbdfd-742">Hierarchical keys</span></span>
  * <span data-ttu-id="dbdfd-743">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-743">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="dbdfd-744">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-744">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="dbdfd-745">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et automatiquement transformé en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-745">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="dbdfd-746">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-746">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="dbdfd-747">Écrivez du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-747">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="dbdfd-748"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-748">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="dbdfd-749">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-749">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="dbdfd-750">Valeurs</span><span class="sxs-lookup"><span data-stu-id="dbdfd-750">Values</span></span>

<span data-ttu-id="dbdfd-751">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-751">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="dbdfd-752">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-752">Values are strings.</span></span>
* <span data-ttu-id="dbdfd-753">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-753">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="dbdfd-754">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="dbdfd-754">Providers</span></span>

<span data-ttu-id="dbdfd-755">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-755">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="dbdfd-756">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-756">Provider</span></span> | <span data-ttu-id="dbdfd-757">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="dbdfd-757">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="dbdfd-758">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-758">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="dbdfd-759">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dbdfd-759">Azure Key Vault</span></span> |
| <span data-ttu-id="dbdfd-760">[Fournisseur Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentation Azure)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-760">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="dbdfd-761">Azure App Configuration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-761">Azure App Configuration</span></span> |
| [<span data-ttu-id="dbdfd-762">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-762">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="dbdfd-763">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-763">Command-line parameters</span></span> |
| [<span data-ttu-id="dbdfd-764">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-764">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="dbdfd-765">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="dbdfd-765">Custom source</span></span> |
| [<span data-ttu-id="dbdfd-766">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-766">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="dbdfd-767">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-767">Environment variables</span></span> |
| [<span data-ttu-id="dbdfd-768">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="dbdfd-768">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="dbdfd-769">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-769">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="dbdfd-770">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="dbdfd-770">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="dbdfd-771">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-771">Directory files</span></span> |
| [<span data-ttu-id="dbdfd-772">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-772">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="dbdfd-773">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-773">In-memory collections</span></span> |
| <span data-ttu-id="dbdfd-774">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-774">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="dbdfd-775">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-775">File in the user profile directory</span></span> |

<span data-ttu-id="dbdfd-776">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-776">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="dbdfd-777">Les fournisseurs de configuration décrits dans cette rubrique sont décrits par ordre alphabétique, et non pas dans l’ordre dans lequel le code les réorganise.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-777">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="dbdfd-778">Commandez des fournisseurs de configuration dans le code pour répondre aux priorités des sources de configuration sous-jacentes requises par l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-778">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="dbdfd-779">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-779">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="dbdfd-780">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-780">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="dbdfd-781">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dbdfd-781">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="dbdfd-782">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-782">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="dbdfd-783">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-783">Environment variables</span></span>
1. <span data-ttu-id="dbdfd-784">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-784">Command-line arguments</span></span>

<span data-ttu-id="dbdfd-785">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-785">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="dbdfd-786">La séquence de fournisseurs précédente est utilisée lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-786">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dbdfd-787">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-787">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="dbdfd-788">Configurer le générateur d’ordinateur hôte avec UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-788">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="dbdfd-789">Pour configurer le générateur d’ordinateur hôte, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> sur le générateur d’hôte avec la configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-789">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="dbdfd-790">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="dbdfd-790">ConfigureAppConfiguration</span></span>

<span data-ttu-id="dbdfd-791">Appelez `ConfigureAppConfiguration` quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-791">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="dbdfd-792">Remplacez la configuration précédente par des arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-792">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="dbdfd-793">Pour fournir une configuration d’application pouvant être remplacée par des arguments de ligne de commande, appelez `AddCommandLine` en dernier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-793">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="dbdfd-794">Supprimer les fournisseurs ajoutés par CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="dbdfd-794">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="dbdfd-795">Pour supprimer les fournisseurs ajoutés par `CreateDefaultBuilder`, appelez d’abord [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) sur [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-795">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="dbdfd-796">Utiliser la configuration lors du démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="dbdfd-796">Consume configuration during app startup</span></span>

<span data-ttu-id="dbdfd-797">La configuration fournie à l’application dans `ConfigureAppConfiguration` est disponible lors du démarrage de l’application, notamment `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-797">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="dbdfd-798">Pour plus d’informations, consultez la section [Accéder à la configuration lors du démarrage](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-798">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="dbdfd-799">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-799">Command-line Configuration Provider</span></span>

<span data-ttu-id="dbdfd-800"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-800">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="dbdfd-801">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-801">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-802">`AddCommandLine` est appelé automatiquement quand `CreateDefaultBuilder(string [])` est appelé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-802">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="dbdfd-803">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-803">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="dbdfd-804">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-804">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dbdfd-805">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-805">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="dbdfd-806">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-806">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="dbdfd-807">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-807">Environment variables.</span></span>

<span data-ttu-id="dbdfd-808">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-808">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="dbdfd-809">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-809">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="dbdfd-810">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-810">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="dbdfd-811">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-811">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="dbdfd-812">Pour les applications basées sur les modèles ASP.NET Core, `AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-812">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dbdfd-813">Pour ajouter des fournisseurs de configuration supplémentaires et conserver la capacité de remplacer leur configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-813">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="dbdfd-814">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-814">**Example**</span></span>

<span data-ttu-id="dbdfd-815">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-815">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="dbdfd-816">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-816">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="dbdfd-817">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-817">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="dbdfd-818">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-818">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dbdfd-819">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-819">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="dbdfd-820">Arguments</span><span class="sxs-lookup"><span data-stu-id="dbdfd-820">Arguments</span></span>

<span data-ttu-id="dbdfd-821">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-821">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="dbdfd-822">La valeur n’est pas requise si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-822">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="dbdfd-823">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-823">Key prefix</span></span>               | <span data-ttu-id="dbdfd-824">Exemple</span><span class="sxs-lookup"><span data-stu-id="dbdfd-824">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="dbdfd-825">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="dbdfd-825">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="dbdfd-826">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-826">Two dashes (`--`)</span></span>        | <span data-ttu-id="dbdfd-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="dbdfd-828">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="dbdfd-828">Forward slash (`/`)</span></span>      | <span data-ttu-id="dbdfd-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="dbdfd-830">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-830">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="dbdfd-831">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-831">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="dbdfd-832">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-832">Switch mappings</span></span>

<span data-ttu-id="dbdfd-833">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-833">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="dbdfd-834">Lors de la génération manuelle d’une configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fournissez un dictionnaire de remplacements de commutateur dans la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-834">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="dbdfd-835">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-835">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="dbdfd-836">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-836">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="dbdfd-837">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-837">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="dbdfd-838">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-838">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="dbdfd-839">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-839">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="dbdfd-840">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-840">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="dbdfd-841">Créez un dictionnaire des mappages de commutateurs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-841">Create a switch mappings dictionary.</span></span> <span data-ttu-id="dbdfd-842">Dans l’exemple suivant, deux mappages de commutateurs sont créés :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-842">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="dbdfd-843">Lorsque l’hôte est généré, appelez `AddCommandLine` avec le dictionnaire de mappages de commutateurs :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-843">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="dbdfd-844">Pour les applications qui utilisent des mappages de commutateurs, l’appel à `CreateDefaultBuilder` ne doit pas passer d’arguments.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-844">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="dbdfd-845">L’appel `CreateDefaultBuilder` de la méthode `AddCommandLine` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-845">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dbdfd-846">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `ConfigurationBuilder` de la méthode `AddCommandLine` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-846">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="dbdfd-847">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-847">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="dbdfd-848">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-848">Key</span></span>       | <span data-ttu-id="dbdfd-849">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-849">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="dbdfd-850">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-850">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="dbdfd-851">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-851">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="dbdfd-852">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-852">Key</span></span>               | <span data-ttu-id="dbdfd-853">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-853">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="dbdfd-854">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-854">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="dbdfd-855"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-855">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="dbdfd-856">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-856">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="dbdfd-857">[Azure App service](https://azure.microsoft.com/services/app-service/) permet de définir des variables d’environnement dans le portail Azure qui peuvent remplacer la configuration de l’application à l’aide du fournisseur de configuration des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="dbdfd-858">Pour plus d’informations, consultez [Azure Apps : remplacer la configuration de l’application à l’aide du portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-858">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="dbdfd-859">`AddEnvironmentVariables` sert à charger les variables d’environnement préfixées avec `ASPNETCORE_` pour la [configuration hôte](#host-versus-app-configuration) lorsqu’un nouveau générateur d’hôte est initialisé avec l’[hôte web](xref:fundamentals/host/web-host) et que `CreateDefaultBuilder` est appelé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-859">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="dbdfd-860">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-860">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="dbdfd-861">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-861">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dbdfd-862">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-862">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="dbdfd-863">Configuration facultative à partir des fichiers *appsettings.json* et *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-863">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="dbdfd-864">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-864">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="dbdfd-865">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-865">Command-line arguments.</span></span>

<span data-ttu-id="dbdfd-866">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-866">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="dbdfd-867">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-867">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="dbdfd-868">Pour fournir la configuration d’application à partir de variables d’environnement supplémentaires, appelez les fournisseurs supplémentaires de l’application dans `ConfigureAppConfiguration` et appelez `AddEnvironmentVariables` avec le préfixe :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-868">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="dbdfd-869">Appelez `AddEnvironmentVariables` dernier pour autoriser les variables d’environnement avec le préfixe spécifié à substituer des valeurs d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-869">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="dbdfd-870">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-870">**Example**</span></span>

<span data-ttu-id="dbdfd-871">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-871">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="dbdfd-872">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-872">Run the sample app.</span></span> <span data-ttu-id="dbdfd-873">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-873">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dbdfd-874">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-874">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="dbdfd-875">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-875">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="dbdfd-876">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-876">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="dbdfd-877">Consultez le fichier *Pages/Index.cshtml.cs* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-877">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="dbdfd-878">Pour exposer toutes les variables d’environnement disponibles pour l’application, remplacez la `FilteredConfiguration` dans *pages/index. cshtml. cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-878">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="dbdfd-879">Préfixes</span><span class="sxs-lookup"><span data-stu-id="dbdfd-879">Prefixes</span></span>

<span data-ttu-id="dbdfd-880">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe à la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-880">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="dbdfd-881">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-881">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="dbdfd-882">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-882">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="dbdfd-883">Lorsque le générateur d’hôte est créé, la configuration de l’hôte est fournie par des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-883">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="dbdfd-884">Pour plus d’informations sur le préfixe utilisé pour ces variables d’environnement, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-884">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="dbdfd-885">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-885">**Connection string prefixes**</span></span>

<span data-ttu-id="dbdfd-886">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-886">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="dbdfd-887">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-887">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="dbdfd-888">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="dbdfd-888">Connection string prefix</span></span> | <span data-ttu-id="dbdfd-889">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-889">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="dbdfd-890">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-890">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="dbdfd-891">MySQL</span><span class="sxs-lookup"><span data-stu-id="dbdfd-891">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="dbdfd-892">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="dbdfd-892">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="dbdfd-893">SQL Server</span><span class="sxs-lookup"><span data-stu-id="dbdfd-893">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="dbdfd-894">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-894">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="dbdfd-895">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-895">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="dbdfd-896">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-896">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="dbdfd-897">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="dbdfd-897">Environment variable key</span></span> | <span data-ttu-id="dbdfd-898">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="dbdfd-898">Converted configuration key</span></span> | <span data-ttu-id="dbdfd-899">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-899">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="dbdfd-900">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-900">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="dbdfd-901">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-901">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="dbdfd-902">Valeur: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-902">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="dbdfd-903">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-903">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="dbdfd-904">Valeur: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-904">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="dbdfd-905">Clé : `ConnectionStrings:{KEY}_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-905">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="dbdfd-906">Valeur: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-906">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="dbdfd-907">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-907">**Example**</span></span>

<span data-ttu-id="dbdfd-908">Une variable d’environnement de chaîne de connexion personnalisée est créée sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-908">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="dbdfd-909">Nom &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-909">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="dbdfd-910">Valeur &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-910">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="dbdfd-911">Si `IConfiguration` est injecté et affecté à un champ nommé `_config`, lisez la valeur :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-911">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="dbdfd-912">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="dbdfd-912">File Configuration Provider</span></span>

<span data-ttu-id="dbdfd-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="dbdfd-914">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-914">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="dbdfd-915">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="dbdfd-915">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="dbdfd-916">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="dbdfd-916">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="dbdfd-917">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="dbdfd-917">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="dbdfd-918">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="dbdfd-918">INI Configuration Provider</span></span>

<span data-ttu-id="dbdfd-919"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-919">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="dbdfd-920">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-920">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-921">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-921">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="dbdfd-922">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-922">Overloads permit specifying:</span></span>

* <span data-ttu-id="dbdfd-923">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-923">Whether the file is optional.</span></span>
* <span data-ttu-id="dbdfd-924">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-924">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dbdfd-925">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-925">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dbdfd-926">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-926">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="dbdfd-927">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-927">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="dbdfd-928">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-928">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dbdfd-929">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-929">section0:key0</span></span>
* <span data-ttu-id="dbdfd-930">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-930">section0:key1</span></span>
* <span data-ttu-id="dbdfd-931">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="dbdfd-931">section1:subsection:key</span></span>
* <span data-ttu-id="dbdfd-932">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="dbdfd-932">section2:subsection0:key</span></span>
* <span data-ttu-id="dbdfd-933">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="dbdfd-933">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="dbdfd-934">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="dbdfd-934">JSON Configuration Provider</span></span>

<span data-ttu-id="dbdfd-935"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-935">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="dbdfd-936">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-936">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-937">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-937">Overloads permit specifying:</span></span>

* <span data-ttu-id="dbdfd-938">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-938">Whether the file is optional.</span></span>
* <span data-ttu-id="dbdfd-939">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-939">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dbdfd-940">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-940">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dbdfd-941">`AddJsonFile` est appelé automatiquement deux fois lorsqu’un nouveau générateur d’hôte est initialisé avec `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-941">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dbdfd-942">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-942">The method is called to load configuration from:</span></span>

* <span data-ttu-id="dbdfd-943">*appSettings. json* &ndash; ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-943">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="dbdfd-944">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-944">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="dbdfd-945">*appSettings. {Environment}. JSON* &ndash; la version de l’environnement du fichier est chargée à partir de [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-945">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="dbdfd-946">Pour plus d’informations, consultez la section [Configuration par défaut](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-946">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="dbdfd-947">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-947">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dbdfd-948">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-948">Environment variables.</span></span>
* <span data-ttu-id="dbdfd-949">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-949">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="dbdfd-950">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="dbdfd-950">Command-line arguments.</span></span>

<span data-ttu-id="dbdfd-951">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-951">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="dbdfd-952">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-952">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="dbdfd-953">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-953">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="dbdfd-954">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-954">**Example**</span></span>

<span data-ttu-id="dbdfd-955">L’exemple d’application tire parti de la méthode de commodité statique `CreateDefaultBuilder` pour créer l’hôte, ce qui comprend deux appels à `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="dbdfd-955">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="dbdfd-956">Le premier appel à `AddJsonFile` charge la configuration à partir de *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="dbdfd-956">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="dbdfd-957">Le deuxième appel à `AddJsonFile` charge la configuration à partir de *appSettings. { Environnement}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-957">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="dbdfd-958">Pour *appSettings. Development. JSON* dans l’exemple d’application, le fichier suivant est chargé :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-958">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="dbdfd-959">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-959">Run the sample app.</span></span> <span data-ttu-id="dbdfd-960">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-960">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dbdfd-961">La sortie contient des paires clé-valeur pour la configuration en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-961">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="dbdfd-962">Le niveau de journalisation de la `Logging:LogLevel:Default` de clé est `Debug` lors de l’exécution de l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-962">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="dbdfd-963">Exécutez à nouveau l’exemple d’application dans l’environnement de production :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-963">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="dbdfd-964">Ouvrez le fichier *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="dbdfd-964">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="dbdfd-965">Dans le profil `ConfigurationSample`, remplacez la valeur de la variable d’environnement `ASPNETCORE_ENVIRONMENT` par `Production`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-965">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="dbdfd-966">Enregistrez le fichier et exécutez l’application avec `dotnet run` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-966">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="dbdfd-967">Paramètres dans *appSettings. Development. JSON* ne remplace plus les paramètres dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-967">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="dbdfd-968">Le niveau de journalisation de la clé `Logging:LogLevel:Default` est `Warning`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-968">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="dbdfd-969">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="dbdfd-969">XML Configuration Provider</span></span>

<span data-ttu-id="dbdfd-970"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-970">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="dbdfd-971">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-971">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-972">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-972">Overloads permit specifying:</span></span>

* <span data-ttu-id="dbdfd-973">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-973">Whether the file is optional.</span></span>
* <span data-ttu-id="dbdfd-974">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-974">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dbdfd-975">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-975">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dbdfd-976">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-976">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="dbdfd-977">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-977">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="dbdfd-978">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-978">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="dbdfd-979">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-979">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="dbdfd-980">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-980">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dbdfd-981">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-981">section0:key0</span></span>
* <span data-ttu-id="dbdfd-982">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-982">section0:key1</span></span>
* <span data-ttu-id="dbdfd-983">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-983">section1:key0</span></span>
* <span data-ttu-id="dbdfd-984">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-984">section1:key1</span></span>

<span data-ttu-id="dbdfd-985">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-985">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="dbdfd-986">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-986">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dbdfd-987">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-987">section:section0:key:key0</span></span>
* <span data-ttu-id="dbdfd-988">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-988">section:section0:key:key1</span></span>
* <span data-ttu-id="dbdfd-989">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-989">section:section1:key:key0</span></span>
* <span data-ttu-id="dbdfd-990">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-990">section:section1:key:key1</span></span>

<span data-ttu-id="dbdfd-991">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-991">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="dbdfd-992">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-992">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dbdfd-993">key:attribute</span><span class="sxs-lookup"><span data-stu-id="dbdfd-993">key:attribute</span></span>
* <span data-ttu-id="dbdfd-994">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="dbdfd-994">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="dbdfd-995">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="dbdfd-995">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="dbdfd-996">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-996">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="dbdfd-997">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-997">The key is the file name.</span></span> <span data-ttu-id="dbdfd-998">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-998">The value contains the file's contents.</span></span> <span data-ttu-id="dbdfd-999">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-999">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="dbdfd-1000">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1000">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="dbdfd-1001">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1001">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="dbdfd-1002">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1002">Overloads permit specifying:</span></span>

* <span data-ttu-id="dbdfd-1003">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1003">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="dbdfd-1004">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1004">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="dbdfd-1005">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1005">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="dbdfd-1006">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1006">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="dbdfd-1007">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1007">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="dbdfd-1008">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1008">Memory Configuration Provider</span></span>

<span data-ttu-id="dbdfd-1009">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1009">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="dbdfd-1010">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1010">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dbdfd-1011">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1011">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="dbdfd-1012">Appelez `ConfigureAppConfiguration` lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1012">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="dbdfd-1013">Dans l’exemple suivant, un dictionnaire de configuration est créé :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1013">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="dbdfd-1014">Le dictionnaire est utilisé avec un appel à `AddInMemoryCollection` pour fournir la configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1014">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="dbdfd-1015">GetValue</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1015">GetValue</span></span>

<span data-ttu-id="dbdfd-1016">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur unique de la configuration avec une clé spécifiée et la convertit en type non collection spécifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="dbdfd-1017">Une surcharge accepte une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1017">An overload accepts a default value.</span></span>

<span data-ttu-id="dbdfd-1018">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1018">The following example:</span></span>

* <span data-ttu-id="dbdfd-1019">Extrait la valeur de chaîne de la configuration avec la clé `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1019">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="dbdfd-1020">Si `NumberKey` est introuvable dans les clés de configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1020">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="dbdfd-1021">Tape la valeur comme `int`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1021">Types the value as an `int`.</span></span>
* <span data-ttu-id="dbdfd-1022">Stocke la valeur dans la propriété `NumberConfig` pour une utilisation par la page.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1022">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="dbdfd-1023">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1023">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="dbdfd-1024">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1024">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="dbdfd-1025">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1025">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="dbdfd-1026">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1026">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="dbdfd-1027">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1027">section0:key0</span></span>
* <span data-ttu-id="dbdfd-1028">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1028">section0:key1</span></span>
* <span data-ttu-id="dbdfd-1029">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1029">section1:key0</span></span>
* <span data-ttu-id="dbdfd-1030">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1030">section1:key1</span></span>
* <span data-ttu-id="dbdfd-1031">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1031">section2:subsection0:key0</span></span>
* <span data-ttu-id="dbdfd-1032">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1032">section2:subsection0:key1</span></span>
* <span data-ttu-id="dbdfd-1033">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1033">section2:subsection1:key0</span></span>
* <span data-ttu-id="dbdfd-1034">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1034">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="dbdfd-1035">GetSection</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1035">GetSection</span></span>

<span data-ttu-id="dbdfd-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="dbdfd-1037">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1037">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="dbdfd-1038">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1038">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="dbdfd-1039">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1039">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="dbdfd-1040">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1040">`GetSection` never returns `null`.</span></span> <span data-ttu-id="dbdfd-1041">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1041">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="dbdfd-1042">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1042">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="dbdfd-1043"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1043">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="dbdfd-1044">GetChildren</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1044">GetChildren</span></span>

<span data-ttu-id="dbdfd-1045">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1045">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="dbdfd-1046">Exists</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1046">Exists</span></span>

<span data-ttu-id="dbdfd-1047">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="dbdfd-1048">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1048">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="dbdfd-1049">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1049">Bind to a class</span></span>

<span data-ttu-id="dbdfd-1050">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1050">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="dbdfd-1051">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1051">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="dbdfd-1052">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1052">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="dbdfd-1053">Le Binder lie les valeurs à toutes les propriétés publiques en lecture/écriture du type fourni.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1053">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="dbdfd-1054">Les champs ne sont **pas** liés.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1054">Fields are **not** bound.</span></span>

<span data-ttu-id="dbdfd-1055">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1055">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1056">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1056">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="dbdfd-1057">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1057">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="dbdfd-1058">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1058">Key</span></span>                   | <span data-ttu-id="dbdfd-1059">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1059">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="dbdfd-1060">starship:name</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1060">starship:name</span></span>         | <span data-ttu-id="dbdfd-1061">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1061">USS Enterprise</span></span>                                    |
| <span data-ttu-id="dbdfd-1062">starship:registry</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1062">starship:registry</span></span>     | <span data-ttu-id="dbdfd-1063">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1063">NCC-1701</span></span>                                          |
| <span data-ttu-id="dbdfd-1064">starship:class</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1064">starship:class</span></span>        | <span data-ttu-id="dbdfd-1065">Constitution</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1065">Constitution</span></span>                                      |
| <span data-ttu-id="dbdfd-1066">starship:length</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1066">starship:length</span></span>       | <span data-ttu-id="dbdfd-1067">304.8</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1067">304.8</span></span>                                             |
| <span data-ttu-id="dbdfd-1068">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1068">starship:commissioned</span></span> | <span data-ttu-id="dbdfd-1069">False</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1069">False</span></span>                                             |
| <span data-ttu-id="dbdfd-1070">trademark</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1070">trademark</span></span>             | <span data-ttu-id="dbdfd-1071">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1071">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="dbdfd-1072">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1072">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="dbdfd-1073">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1073">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="dbdfd-1074">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1074">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="dbdfd-1075">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1075">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="dbdfd-1076">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1076">Bind to an object graph</span></span>

<span data-ttu-id="dbdfd-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="dbdfd-1078">Comme pour la liaison d’un objet simple, seules les propriétés accessibles en lecture/écriture publiques sont liées.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1078">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="dbdfd-1079">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1079">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1080">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1080">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="dbdfd-1081">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1081">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="dbdfd-1082">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1082">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="dbdfd-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="dbdfd-1084">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1084">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="dbdfd-1085">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1085">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="dbdfd-1086">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1086">Bind an array to a class</span></span>

<span data-ttu-id="dbdfd-1087">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1087">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="dbdfd-1088"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1088">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="dbdfd-1089">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer une liaison de tableau à un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1089">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="dbdfd-1090">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1090">Binding is provided by convention.</span></span> <span data-ttu-id="dbdfd-1091">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1091">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="dbdfd-1092">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1092">**In-memory array processing**</span></span>

<span data-ttu-id="dbdfd-1093">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1093">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="dbdfd-1094">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1094">Key</span></span>             | <span data-ttu-id="dbdfd-1095">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1095">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="dbdfd-1096">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1096">array:entries:0</span></span> | <span data-ttu-id="dbdfd-1097">value0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1097">value0</span></span> |
| <span data-ttu-id="dbdfd-1098">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1098">array:entries:1</span></span> | <span data-ttu-id="dbdfd-1099">valeur1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1099">value1</span></span> |
| <span data-ttu-id="dbdfd-1100">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1100">array:entries:2</span></span> | <span data-ttu-id="dbdfd-1101">valeur2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1101">value2</span></span> |
| <span data-ttu-id="dbdfd-1102">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1102">array:entries:4</span></span> | <span data-ttu-id="dbdfd-1103">value4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1103">value4</span></span> |
| <span data-ttu-id="dbdfd-1104">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1104">array:entries:5</span></span> | <span data-ttu-id="dbdfd-1105">value5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1105">value5</span></span> |

<span data-ttu-id="dbdfd-1106">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1106">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="dbdfd-1107">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1107">The array skips a value for index &num;3.</span></span> <span data-ttu-id="dbdfd-1108">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1108">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="dbdfd-1109">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1109">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1110">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1110">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="dbdfd-1111">La syntaxe [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1111">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="dbdfd-1112">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1112">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="dbdfd-1113">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1113">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="dbdfd-1114">`ArrayExample.Entries` Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1114">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="dbdfd-1115">0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1115">0</span></span>                            | <span data-ttu-id="dbdfd-1116">value0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1116">value0</span></span>                       |
| <span data-ttu-id="dbdfd-1117">1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1117">1</span></span>                            | <span data-ttu-id="dbdfd-1118">valeur1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1118">value1</span></span>                       |
| <span data-ttu-id="dbdfd-1119">2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1119">2</span></span>                            | <span data-ttu-id="dbdfd-1120">valeur2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1120">value2</span></span>                       |
| <span data-ttu-id="dbdfd-1121">3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1121">3</span></span>                            | <span data-ttu-id="dbdfd-1122">value4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1122">value4</span></span>                       |
| <span data-ttu-id="dbdfd-1123">4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1123">4</span></span>                            | <span data-ttu-id="dbdfd-1124">value5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1124">value5</span></span>                       |

<span data-ttu-id="dbdfd-1125">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1125">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="dbdfd-1126">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1126">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="dbdfd-1127">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1127">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="dbdfd-1128">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1128">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="dbdfd-1129">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1129">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="dbdfd-1130">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1130">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="dbdfd-1131">Dans `ConfigureAppConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1131">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="dbdfd-1132">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1132">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="dbdfd-1133">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1133">Key</span></span>             | <span data-ttu-id="dbdfd-1134">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1134">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="dbdfd-1135">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1135">array:entries:3</span></span> | <span data-ttu-id="dbdfd-1136">valeur3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1136">value3</span></span> |

<span data-ttu-id="dbdfd-1137">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1137">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="dbdfd-1138">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1138">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="dbdfd-1139">`ArrayExample.Entries` Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1139">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="dbdfd-1140">0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1140">0</span></span>                            | <span data-ttu-id="dbdfd-1141">value0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1141">value0</span></span>                       |
| <span data-ttu-id="dbdfd-1142">1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1142">1</span></span>                            | <span data-ttu-id="dbdfd-1143">valeur1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1143">value1</span></span>                       |
| <span data-ttu-id="dbdfd-1144">2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1144">2</span></span>                            | <span data-ttu-id="dbdfd-1145">valeur2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1145">value2</span></span>                       |
| <span data-ttu-id="dbdfd-1146">3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1146">3</span></span>                            | <span data-ttu-id="dbdfd-1147">valeur3</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1147">value3</span></span>                       |
| <span data-ttu-id="dbdfd-1148">4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1148">4</span></span>                            | <span data-ttu-id="dbdfd-1149">value4</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1149">value4</span></span>                       |
| <span data-ttu-id="dbdfd-1150">5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1150">5</span></span>                            | <span data-ttu-id="dbdfd-1151">value5</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1151">value5</span></span>                       |

<span data-ttu-id="dbdfd-1152">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1152">**JSON array processing**</span></span>

<span data-ttu-id="dbdfd-1153">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1153">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="dbdfd-1154">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1154">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="dbdfd-1155">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1155">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="dbdfd-1156">Clé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1156">Key</span></span>                     | <span data-ttu-id="dbdfd-1157">Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1157">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="dbdfd-1158">json_array:key</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1158">json_array:key</span></span>          | <span data-ttu-id="dbdfd-1159">valueA</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1159">valueA</span></span> |
| <span data-ttu-id="dbdfd-1160">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1160">json_array:subsection:0</span></span> | <span data-ttu-id="dbdfd-1161">valueB</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1161">valueB</span></span> |
| <span data-ttu-id="dbdfd-1162">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1162">json_array:subsection:1</span></span> | <span data-ttu-id="dbdfd-1163">valueC</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1163">valueC</span></span> |
| <span data-ttu-id="dbdfd-1164">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1164">json_array:subsection:2</span></span> | <span data-ttu-id="dbdfd-1165">valueD</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1165">valueD</span></span> |

<span data-ttu-id="dbdfd-1166">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1166">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1167">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1167">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="dbdfd-1168">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1168">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="dbdfd-1169">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1169">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="dbdfd-1170">`JsonArrayExample.Subsection` Valeur</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1170">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="dbdfd-1171">0</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1171">0</span></span>                                   | <span data-ttu-id="dbdfd-1172">valueB</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1172">valueB</span></span>                              |
| <span data-ttu-id="dbdfd-1173">1</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1173">1</span></span>                                   | <span data-ttu-id="dbdfd-1174">valueC</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1174">valueC</span></span>                              |
| <span data-ttu-id="dbdfd-1175">2</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1175">2</span></span>                                   | <span data-ttu-id="dbdfd-1176">valueD</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1176">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="dbdfd-1177">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1177">Custom configuration provider</span></span>

<span data-ttu-id="dbdfd-1178">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1178">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="dbdfd-1179">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1179">The provider has the following characteristics:</span></span>

* <span data-ttu-id="dbdfd-1180">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1180">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="dbdfd-1181">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1181">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="dbdfd-1182">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1182">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="dbdfd-1183">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1183">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="dbdfd-1184">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1184">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="dbdfd-1185">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1185">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="dbdfd-1186">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1186">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1187">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1187">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="dbdfd-1188">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1189">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1189">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="dbdfd-1190">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1191">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1191">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="dbdfd-1192">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1192">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="dbdfd-1193">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1194">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1194">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="dbdfd-1195">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1195">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="dbdfd-1196">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1196">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="dbdfd-1197">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1197">Access configuration during startup</span></span>

<span data-ttu-id="dbdfd-1198">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1198">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="dbdfd-1199">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1199">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="dbdfd-1200">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1200">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="dbdfd-1201">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1201">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="dbdfd-1202">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1202">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="dbdfd-1203">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1203">In a Razor Pages page:</span></span>

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

<span data-ttu-id="dbdfd-1204">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1204">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="dbdfd-1205">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1205">Add configuration from an external assembly</span></span>

<span data-ttu-id="dbdfd-1206">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1206">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="dbdfd-1207">Pour plus d’informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1207">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dbdfd-1208">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dbdfd-1208">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
