---
title: Configuration dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 63a876c09f952537d790f2a5df4b8672df49d015
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517018"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="d688c-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d688c-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="d688c-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d688c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d688c-105">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="d688c-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="d688c-106">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="d688c-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d688c-107">Azure Key Vault</span></span>
* <span data-ttu-id="d688c-108">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="d688c-108">Command-line arguments</span></span>
* <span data-ttu-id="d688c-109">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="d688c-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="d688c-110">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="d688c-110">Directory files</span></span>
* <span data-ttu-id="d688c-111">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="d688c-111">Environment variables</span></span>
* <span data-ttu-id="d688c-112">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="d688c-112">In-memory .NET objects</span></span>
* <span data-ttu-id="d688c-113">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="d688c-113">Settings files</span></span>

<span data-ttu-id="d688c-114">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="d688c-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="d688c-115">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="d688c-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="d688c-116">Pour plus d’informations sur l’utilisation du modèle d’options, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d688c-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d688c-117">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d688c-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d688c-118">Ces trois packages sont inclus dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="d688c-119">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="d688c-119">Host vs. app configuration</span></span>

<span data-ttu-id="d688c-120">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="d688c-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="d688c-121">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="d688c-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d688c-122">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="d688c-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="d688c-123">Les paires clé-valeur de la configuration de l’hôte font partie de la configuration générale de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="d688c-124">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez [L’hôte](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="d688c-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="d688c-125">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="d688c-125">Default configuration</span></span>

<span data-ttu-id="d688c-126">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="d688c-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="d688c-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> fournit la configuration par défaut de l’application dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="d688c-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="d688c-128">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="d688c-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="d688c-129">Des variables d’environnement préfixées avec `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d688c-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="d688c-130">Le préfixe (`ASPNETCORE_`) est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="d688c-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="d688c-131">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d688c-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="d688c-132">La configuration de l’application est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="d688c-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="d688c-133">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d688c-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="d688c-134">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d688c-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="d688c-135">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="d688c-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d688c-136">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d688c-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="d688c-137">Si un préfixe personnalisé est utilisé (par exemple, `PREFIX_` avec `.AddEnvironmentVariables(prefix: "PREFIX_")`), le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont chargées.</span><span class="sxs-lookup"><span data-stu-id="d688c-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="d688c-138">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d688c-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="d688c-139">Les fournisseurs de configuration sont décrits plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="d688c-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="d688c-140">Pour plus d’informations sur l’hôte et <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, consultez <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="d688c-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="d688c-141">Sécurité</span><span class="sxs-lookup"><span data-stu-id="d688c-141">Security</span></span>

<span data-ttu-id="d688c-142">Adoptez les meilleures pratiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="d688c-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="d688c-143">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="d688c-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="d688c-144">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="d688c-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="d688c-145">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="d688c-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="d688c-146">En savoir plus sur [l’utilisation de plusieurs environnements](xref:fundamentals/environments) et la gestion du [stockage sécurisé des secrets d’application dans le développement avec Secret Manager](xref:security/app-secrets) (contient des conseils sur l’utilisation des variables d’environnement pour stocker les données sensibles).</span><span class="sxs-lookup"><span data-stu-id="d688c-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="d688c-147">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="d688c-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="d688c-148">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="d688c-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="d688c-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) constitue une option pour le stockage sécurisé des secrets d’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="d688c-150">Pour plus d'informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d688c-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="d688c-151">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="d688c-151">Hierarchical configuration data</span></span>

<span data-ttu-id="d688c-152">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="d688c-153">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="d688c-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="d688c-154">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="d688c-155">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="d688c-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="d688c-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-156">section0:key0</span></span>
* <span data-ttu-id="d688c-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-157">section0:key1</span></span>
* <span data-ttu-id="d688c-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-158">section1:key0</span></span>
* <span data-ttu-id="d688c-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-159">section1:key1</span></span>

<span data-ttu-id="d688c-160">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="d688c-161">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="d688c-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="d688c-162">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="d688c-163">Conventions</span><span class="sxs-lookup"><span data-stu-id="d688c-163">Conventions</span></span>

<span data-ttu-id="d688c-164">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="d688c-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="d688c-165">Les fournisseurs de configuration qui implémentent la détection des modifications peuvent recharger la configuration lorsqu’un paramètre sous-jacent est modifié.</span><span class="sxs-lookup"><span data-stu-id="d688c-165">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="d688c-166">Par exemple, le fournisseur de configuration de fichier (décrit plus loin dans cette rubrique) et le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) implémentent la détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="d688c-166">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="d688c-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d688c-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> peut être injecté dans une Razor Page <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> pour obtenir la configuration de la classe :</span><span class="sxs-lookup"><span data-stu-id="d688c-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

<span data-ttu-id="d688c-169">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="d688c-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="d688c-170">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="d688c-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="d688c-171">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="d688c-171">Keys are case-insensitive.</span></span> <span data-ttu-id="d688c-172">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="d688c-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="d688c-173">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="d688c-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="d688c-174">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="d688c-174">Hierarchical keys</span></span>
  * <span data-ttu-id="d688c-175">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="d688c-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="d688c-176">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="d688c-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="d688c-177">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et converti en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="d688c-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="d688c-178">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="d688c-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="d688c-179">Vous devez fournir du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="d688c-180"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d688c-181">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="d688c-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="d688c-182">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="d688c-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="d688c-183">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="d688c-183">Values are strings.</span></span>
* <span data-ttu-id="d688c-184">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="d688c-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="d688c-185">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="d688c-185">Providers</span></span>

<span data-ttu-id="d688c-186">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d688c-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="d688c-187">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="d688c-187">Provider</span></span> | <span data-ttu-id="d688c-188">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="d688c-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="d688c-189">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="d688c-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="d688c-190">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d688c-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="d688c-191">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="d688c-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="d688c-192">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="d688c-192">Command-line parameters</span></span> |
| [<span data-ttu-id="d688c-193">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="d688c-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="d688c-194">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="d688c-194">Custom source</span></span> |
| [<span data-ttu-id="d688c-195">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="d688c-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="d688c-196">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="d688c-196">Environment variables</span></span> |
| [<span data-ttu-id="d688c-197">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="d688c-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="d688c-198">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="d688c-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="d688c-199">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="d688c-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="d688c-200">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="d688c-200">Directory files</span></span> |
| [<span data-ttu-id="d688c-201">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="d688c-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="d688c-202">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="d688c-202">In-memory collections</span></span> |
| <span data-ttu-id="d688c-203">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="d688c-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="d688c-204">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="d688c-204">File in the user profile directory</span></span> |

<span data-ttu-id="d688c-205">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="d688c-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="d688c-206">Les fournisseurs de configuration décrits dans cette rubrique sont décrits dans l’ordre alphabétique, et non dans l’ordre où votre code peut les organiser.</span><span class="sxs-lookup"><span data-stu-id="d688c-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="d688c-207">Organisez les fournisseurs de configuration dans votre code en fonction de vos priorités pour les sources de configuration sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="d688c-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="d688c-208">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="d688c-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="d688c-209">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="d688c-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="d688c-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d688c-210">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="d688c-211">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="d688c-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="d688c-212">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="d688c-212">Environment variables</span></span>
1. <span data-ttu-id="d688c-213">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="d688c-213">Command-line arguments</span></span>

<span data-ttu-id="d688c-214">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="d688c-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="d688c-215">Cette séquence de fournisseurs est mise en place lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d688c-216">Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="d688c-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="d688c-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="d688c-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="d688c-218">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> :</span><span class="sxs-lookup"><span data-stu-id="d688c-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="d688c-219">La configuration fournie à l’application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> est disponible lors du démarrage de l’application, notamment `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d688c-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d688c-220">Pour plus d’informations, consultez la section [Accéder à la configuration lors du démarrage](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="d688c-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="d688c-221">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="d688c-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="d688c-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="d688c-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="d688c-223">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d688c-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d688c-224">`AddCommandLine` est appelé automatiquement lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d688c-225">Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="d688c-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d688c-226">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="d688c-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d688c-227">Configuration facultative à partir d’*appsettings.json* et d’*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d688c-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="d688c-228">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="d688c-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d688c-229">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="d688c-229">Environment variables.</span></span>

<span data-ttu-id="d688c-230">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="d688c-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="d688c-231">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="d688c-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="d688c-232">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="d688c-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="d688c-233">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="d688c-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="d688c-234">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="d688c-235">`AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d688c-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d688c-236">Si vous devez fournir la configuration de l'application tout en étant capable de remplacer cette configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l'application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="d688c-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d688c-237">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="d688c-238">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="d688c-238">**Example**</span></span>

<span data-ttu-id="d688c-239">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="d688c-240">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="d688c-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="d688c-241">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="d688c-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="d688c-242">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d688c-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d688c-243">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d688c-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="d688c-244">Arguments</span><span class="sxs-lookup"><span data-stu-id="d688c-244">Arguments</span></span>

<span data-ttu-id="d688c-245">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="d688c-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="d688c-246">La valeur peut être null si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="d688c-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="d688c-247">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="d688c-247">Key prefix</span></span>               | <span data-ttu-id="d688c-248">Exemple</span><span class="sxs-lookup"><span data-stu-id="d688c-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="d688c-249">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="d688c-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="d688c-250">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="d688c-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="d688c-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="d688c-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="d688c-252">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="d688c-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="d688c-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="d688c-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="d688c-254">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="d688c-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="d688c-255">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="d688c-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="d688c-256">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="d688c-256">Switch mappings</span></span>

<span data-ttu-id="d688c-257">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="d688c-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="d688c-258">Lorsque vous générez manuellement la configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, vous pouvez fournir un dictionnaire des remplacements de commutateur à la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="d688c-259">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="d688c-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="d688c-260">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="d688c-261">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="d688c-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="d688c-262">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="d688c-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="d688c-263">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="d688c-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="d688c-264">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="d688c-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="d688c-265">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="d688c-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d688c-266">Comme indiqué dans l’exemple précédent, l’appel à `CreateDefaultBuilder` ne doit pas passer des arguments lorsque des correspondances de commutateur sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="d688c-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="d688c-267">L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d688c-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d688c-268">Si les arguments incluent un commutateur mappé et sont passés à `CreateDefaultBuilder`, son fournisseur `AddCommandLine` ne parvient pas à s’initialiser avec un <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="d688c-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="d688c-269">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="d688c-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="d688c-270">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="d688c-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="d688c-271">Touche</span><span class="sxs-lookup"><span data-stu-id="d688c-271">Key</span></span>       | <span data-ttu-id="d688c-272">Value</span><span class="sxs-lookup"><span data-stu-id="d688c-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="d688c-273">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="d688c-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="d688c-274">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="d688c-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="d688c-275">Touche</span><span class="sxs-lookup"><span data-stu-id="d688c-275">Key</span></span>               | <span data-ttu-id="d688c-276">Value</span><span class="sxs-lookup"><span data-stu-id="d688c-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="d688c-277">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="d688c-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="d688c-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="d688c-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="d688c-279">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d688c-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="d688c-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) vous permet de définir des variables d’environnement dans le portail Azure capables de remplacer la configuration d’application à l’aide du Fournisseur de configuration de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="d688c-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="d688c-281">Pour plus d’informations, consultez les [applications Azure : Remplacer la configuration de l’application à l’aide du Portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="d688c-281">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="d688c-282">`AddEnvironmentVariables` est appelé automatiquement pour les variables d’environnement précédées de `ASPNETCORE_` à l’initialisation d’un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d688c-282">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="d688c-283">Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="d688c-283">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d688c-284">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="d688c-284">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d688c-285">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="d688c-285">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="d688c-286">Configuration facultative à partir d’*appsettings.json* et d’*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d688c-286">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="d688c-287">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="d688c-287">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d688c-288">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="d688c-288">Command-line arguments.</span></span>

<span data-ttu-id="d688c-289">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d688c-289">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="d688c-290">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d688c-290">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="d688c-291">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-291">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="d688c-292">Si vous devez fournir la configuration de l'application à partir de variables d'environnement supplémentaires, appelez les fournisseurs supplémentaires de l'application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> et appelez `AddEnvironmentVariables` avec le préfixe.</span><span class="sxs-lookup"><span data-stu-id="d688c-292">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d688c-293">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-293">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="d688c-294">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="d688c-294">**Example**</span></span>

<span data-ttu-id="d688c-295">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d688c-295">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="d688c-296">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-296">Run the sample app.</span></span> <span data-ttu-id="d688c-297">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d688c-297">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d688c-298">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d688c-298">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="d688c-299">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="d688c-299">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="d688c-300">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement en fonction de celles qui commencent par :</span><span class="sxs-lookup"><span data-stu-id="d688c-300">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="d688c-301">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="d688c-301">ASPNETCORE_</span></span>
* <span data-ttu-id="d688c-302">urls</span><span class="sxs-lookup"><span data-stu-id="d688c-302">urls</span></span>
* <span data-ttu-id="d688c-303">Journalisation</span><span class="sxs-lookup"><span data-stu-id="d688c-303">Logging</span></span>
* <span data-ttu-id="d688c-304">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="d688c-304">ENVIRONMENT</span></span>
* <span data-ttu-id="d688c-305">contentRoot</span><span class="sxs-lookup"><span data-stu-id="d688c-305">contentRoot</span></span>
* <span data-ttu-id="d688c-306">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="d688c-306">AllowedHosts</span></span>
* <span data-ttu-id="d688c-307">applicationName</span><span class="sxs-lookup"><span data-stu-id="d688c-307">applicationName</span></span>
* <span data-ttu-id="d688c-308">CommandLine</span><span class="sxs-lookup"><span data-stu-id="d688c-308">CommandLine</span></span>

<span data-ttu-id="d688c-309">Si vous souhaitez exposer toutes les variables d’environnement disponibles pour l’application, remplacez `FilteredConfiguration` dans *Pages/Index.cshtml.cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="d688c-309">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="d688c-310">Préfixes</span><span class="sxs-lookup"><span data-stu-id="d688c-310">Prefixes</span></span>

<span data-ttu-id="d688c-311">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe pour la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d688c-311">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="d688c-312">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-312">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="d688c-313">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="d688c-313">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="d688c-314">La méthode pratique statique `CreateDefaultBuilder` crée un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> pour établir l’hôte de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-314">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="d688c-315">Lorsque `WebHostBuilder` est créé, il trouve la configuration de son hôte dans les variables d’environnement avec le préfixe `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="d688c-315">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="d688c-316">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="d688c-316">**Connection string prefixes**</span></span>

<span data-ttu-id="d688c-317">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-317">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="d688c-318">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d688c-318">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="d688c-319">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="d688c-319">Connection string prefix</span></span> | <span data-ttu-id="d688c-320">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="d688c-320">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="d688c-321">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="d688c-321">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="d688c-322">MySQL</span><span class="sxs-lookup"><span data-stu-id="d688c-322">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="d688c-323">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="d688c-323">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="d688c-324">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d688c-324">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="d688c-325">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="d688c-325">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="d688c-326">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="d688c-326">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="d688c-327">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="d688c-327">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="d688c-328">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="d688c-328">Environment variable key</span></span> | <span data-ttu-id="d688c-329">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="d688c-329">Converted configuration key</span></span> | <span data-ttu-id="d688c-330">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="d688c-330">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d688c-331">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="d688c-331">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d688c-332">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="d688c-332">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d688c-333">Valeur : `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="d688c-333">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d688c-334">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="d688c-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d688c-335">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d688c-335">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d688c-336">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="d688c-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d688c-337">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d688c-337">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="d688c-338">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="d688c-338">File Configuration Provider</span></span>

<span data-ttu-id="d688c-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="d688c-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="d688c-340">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="d688c-340">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="d688c-341">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="d688c-341">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="d688c-342">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="d688c-342">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="d688c-343">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="d688c-343">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="d688c-344">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="d688c-344">INI Configuration Provider</span></span>

<span data-ttu-id="d688c-345"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="d688c-345">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="d688c-346">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d688c-346">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d688c-347">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="d688c-347">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="d688c-348">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="d688c-348">Overloads permit specifying:</span></span>

* <span data-ttu-id="d688c-349">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="d688c-349">Whether the file is optional.</span></span>
* <span data-ttu-id="d688c-350">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="d688c-350">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d688c-351">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="d688c-351">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d688c-352">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="d688c-352">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d688c-353">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-353">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d688c-354">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-354">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-355">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-355">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="d688c-356">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-356">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d688c-357">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-357">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-358">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="d688c-358">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="d688c-359">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="d688c-359">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d688c-360">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-360">section0:key0</span></span>
* <span data-ttu-id="d688c-361">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-361">section0:key1</span></span>
* <span data-ttu-id="d688c-362">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="d688c-362">section1:subsection:key</span></span>
* <span data-ttu-id="d688c-363">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="d688c-363">section2:subsection0:key</span></span>
* <span data-ttu-id="d688c-364">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="d688c-364">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="d688c-365">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="d688c-365">JSON Configuration Provider</span></span>

<span data-ttu-id="d688c-366"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="d688c-366">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="d688c-367">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d688c-367">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d688c-368">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="d688c-368">Overloads permit specifying:</span></span>

* <span data-ttu-id="d688c-369">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="d688c-369">Whether the file is optional.</span></span>
* <span data-ttu-id="d688c-370">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="d688c-370">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d688c-371">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="d688c-371">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d688c-372">`AddJsonFile` est appelé automatiquement deux fois lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-372">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d688c-373">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="d688c-373">The method is called to load configuration from:</span></span>

* <span data-ttu-id="d688c-374">*appSettings.JSON* &ndash; Ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="d688c-374">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="d688c-375">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d688c-375">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="d688c-376">*appsettings.{Environment}.json* &ndash; La version de l’environnement du fichier est chargée à partir du fichier [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="d688c-376">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="d688c-377">Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="d688c-377">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d688c-378">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="d688c-378">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d688c-379">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="d688c-379">Environment variables.</span></span>
* <span data-ttu-id="d688c-380">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="d688c-380">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d688c-381">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="d688c-381">Command-line arguments.</span></span>

<span data-ttu-id="d688c-382">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="d688c-382">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="d688c-383">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d688c-383">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="d688c-384">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="d688c-384">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d688c-385">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-385">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d688c-386">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-386">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-387">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="d688c-388">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-388">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d688c-389">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-389">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-390">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="d688c-390">**Example**</span></span>

<span data-ttu-id="d688c-391">L’exemple d’application tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut deux appels à `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="d688c-391">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="d688c-392">La configuration est chargée à partir d’*appsettings.json* et d’*appsettings{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d688c-392">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="d688c-393">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-393">Run the sample app.</span></span> <span data-ttu-id="d688c-394">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d688c-394">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d688c-395">Notez que la sortie contient des paires clé-valeur pour la configuration représentée dans le tableau en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="d688c-395">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="d688c-396">Les clés de configuration de la journalisation utilisent le signe deux-points (`:`) comme séparateur hiérarchique.</span><span class="sxs-lookup"><span data-stu-id="d688c-396">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="d688c-397">Touche</span><span class="sxs-lookup"><span data-stu-id="d688c-397">Key</span></span>                        | <span data-ttu-id="d688c-398">Valeur de développement</span><span class="sxs-lookup"><span data-stu-id="d688c-398">Development Value</span></span> | <span data-ttu-id="d688c-399">Valeur de production</span><span class="sxs-lookup"><span data-stu-id="d688c-399">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="d688c-400">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="d688c-400">Logging:LogLevel:System</span></span>    | <span data-ttu-id="d688c-401">Information</span><span class="sxs-lookup"><span data-stu-id="d688c-401">Information</span></span>       | <span data-ttu-id="d688c-402">Information</span><span class="sxs-lookup"><span data-stu-id="d688c-402">Information</span></span>      |
| <span data-ttu-id="d688c-403">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="d688c-403">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="d688c-404">Information</span><span class="sxs-lookup"><span data-stu-id="d688c-404">Information</span></span>       | <span data-ttu-id="d688c-405">Information</span><span class="sxs-lookup"><span data-stu-id="d688c-405">Information</span></span>      |
| <span data-ttu-id="d688c-406">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="d688c-406">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="d688c-407">Débogage</span><span class="sxs-lookup"><span data-stu-id="d688c-407">Debug</span></span>             | <span data-ttu-id="d688c-408">Error</span><span class="sxs-lookup"><span data-stu-id="d688c-408">Error</span></span>            |
| <span data-ttu-id="d688c-409">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="d688c-409">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="d688c-410">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="d688c-410">XML Configuration Provider</span></span>

<span data-ttu-id="d688c-411"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="d688c-411">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="d688c-412">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d688c-412">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d688c-413">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="d688c-413">Overloads permit specifying:</span></span>

* <span data-ttu-id="d688c-414">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="d688c-414">Whether the file is optional.</span></span>
* <span data-ttu-id="d688c-415">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="d688c-415">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d688c-416">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="d688c-416">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d688c-417">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="d688c-417">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="d688c-418">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="d688c-418">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="d688c-419">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="d688c-419">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d688c-420">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-420">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d688c-421">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-421">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-422">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-422">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="d688c-423">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-423">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d688c-424">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-424">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-425">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="d688c-425">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="d688c-426">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="d688c-426">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d688c-427">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-427">section0:key0</span></span>
* <span data-ttu-id="d688c-428">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-428">section0:key1</span></span>
* <span data-ttu-id="d688c-429">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-429">section1:key0</span></span>
* <span data-ttu-id="d688c-430">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-430">section1:key1</span></span>

<span data-ttu-id="d688c-431">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="d688c-431">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="d688c-432">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="d688c-432">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d688c-433">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-433">section:section0:key:key0</span></span>
* <span data-ttu-id="d688c-434">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-434">section:section0:key:key1</span></span>
* <span data-ttu-id="d688c-435">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-435">section:section1:key:key0</span></span>
* <span data-ttu-id="d688c-436">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-436">section:section1:key:key1</span></span>

<span data-ttu-id="d688c-437">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="d688c-437">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="d688c-438">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="d688c-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d688c-439">key:attribute</span><span class="sxs-lookup"><span data-stu-id="d688c-439">key:attribute</span></span>
* <span data-ttu-id="d688c-440">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="d688c-440">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="d688c-441">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="d688c-441">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="d688c-442">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-442">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="d688c-443">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="d688c-443">The key is the file name.</span></span> <span data-ttu-id="d688c-444">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="d688c-444">The value contains the file's contents.</span></span> <span data-ttu-id="d688c-445">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="d688c-445">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="d688c-446">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d688c-446">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="d688c-447">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="d688c-447">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="d688c-448">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="d688c-448">Overloads permit specifying:</span></span>

* <span data-ttu-id="d688c-449">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="d688c-449">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="d688c-450">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="d688c-450">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="d688c-451">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="d688c-451">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="d688c-452">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="d688c-452">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="d688c-453">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="d688c-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d688c-454">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d688c-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d688c-455">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-456">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="d688c-457">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="d688c-457">Memory Configuration Provider</span></span>

<span data-ttu-id="d688c-458">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-458">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="d688c-459">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d688c-459">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d688c-460">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="d688c-460">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="d688c-461">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="d688c-461">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d688c-462">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-462">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="d688c-463">GetValue</span><span class="sxs-lookup"><span data-stu-id="d688c-463">GetValue</span></span>

<span data-ttu-id="d688c-464">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur à partir de la configuration avec une clé spécifiée et la convertit selon le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="d688c-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="d688c-465">Une surcharge vous permet de fournir une valeur par défaut si la clé est introuvable.</span><span class="sxs-lookup"><span data-stu-id="d688c-465">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="d688c-466">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="d688c-466">The following example:</span></span>

* <span data-ttu-id="d688c-467">Extrait la valeur de chaîne de la configuration avec la clé `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="d688c-467">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="d688c-468">Si `NumberKey` est introuvable dans les clés de configuration, la valeur par défaut de `99` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="d688c-468">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="d688c-469">Tape la valeur comme `int`.</span><span class="sxs-lookup"><span data-stu-id="d688c-469">Types the value as an `int`.</span></span>
* <span data-ttu-id="d688c-470">Stocke la valeur dans la propriété `NumberConfig` pour une utilisation par la page.</span><span class="sxs-lookup"><span data-stu-id="d688c-470">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="d688c-471">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="d688c-471">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="d688c-472">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="d688c-472">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="d688c-473">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="d688c-473">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="d688c-474">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-474">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="d688c-475">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-475">section0:key0</span></span>
* <span data-ttu-id="d688c-476">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-476">section0:key1</span></span>
* <span data-ttu-id="d688c-477">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-477">section1:key0</span></span>
* <span data-ttu-id="d688c-478">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-478">section1:key1</span></span>
* <span data-ttu-id="d688c-479">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-479">section2:subsection0:key0</span></span>
* <span data-ttu-id="d688c-480">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-480">section2:subsection0:key1</span></span>
* <span data-ttu-id="d688c-481">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="d688c-481">section2:subsection1:key0</span></span>
* <span data-ttu-id="d688c-482">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="d688c-482">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="d688c-483">GetSection</span><span class="sxs-lookup"><span data-stu-id="d688c-483">GetSection</span></span>

<span data-ttu-id="d688c-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="d688c-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="d688c-485">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-485">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-486">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="d688c-486">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="d688c-487">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="d688c-487">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="d688c-488">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="d688c-488">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="d688c-489">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="d688c-489">`GetSection` never returns `null`.</span></span> <span data-ttu-id="d688c-490">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="d688c-490">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="d688c-491">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="d688c-491">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="d688c-492"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="d688c-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="d688c-493">GetChildren</span><span class="sxs-lookup"><span data-stu-id="d688c-493">GetChildren</span></span>

<span data-ttu-id="d688c-494">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="d688c-494">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="d688c-495">Existe</span><span class="sxs-lookup"><span data-stu-id="d688c-495">Exists</span></span>

<span data-ttu-id="d688c-496">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="d688c-496">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="d688c-497">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-497">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="d688c-498">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="d688c-498">Bind to a class</span></span>

<span data-ttu-id="d688c-499">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="d688c-499">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="d688c-500">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d688c-500">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d688c-501">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="d688c-501">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="d688c-502">`Bind` est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-502">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-503">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="d688c-503">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="d688c-504">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-504">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="d688c-505">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="d688c-505">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="d688c-506">Touche</span><span class="sxs-lookup"><span data-stu-id="d688c-506">Key</span></span>                   | <span data-ttu-id="d688c-507">Value</span><span class="sxs-lookup"><span data-stu-id="d688c-507">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="d688c-508">starship:name</span><span class="sxs-lookup"><span data-stu-id="d688c-508">starship:name</span></span>         | <span data-ttu-id="d688c-509">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="d688c-509">USS Enterprise</span></span>                                    |
| <span data-ttu-id="d688c-510">starship:registry</span><span class="sxs-lookup"><span data-stu-id="d688c-510">starship:registry</span></span>     | <span data-ttu-id="d688c-511">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="d688c-511">NCC-1701</span></span>                                          |
| <span data-ttu-id="d688c-512">starship:class</span><span class="sxs-lookup"><span data-stu-id="d688c-512">starship:class</span></span>        | <span data-ttu-id="d688c-513">Constitution</span><span class="sxs-lookup"><span data-stu-id="d688c-513">Constitution</span></span>                                      |
| <span data-ttu-id="d688c-514">starship:length</span><span class="sxs-lookup"><span data-stu-id="d688c-514">starship:length</span></span>       | <span data-ttu-id="d688c-515">304.8</span><span class="sxs-lookup"><span data-stu-id="d688c-515">304.8</span></span>                                             |
| <span data-ttu-id="d688c-516">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="d688c-516">starship:commissioned</span></span> | <span data-ttu-id="d688c-517">False</span><span class="sxs-lookup"><span data-stu-id="d688c-517">False</span></span>                                             |
| <span data-ttu-id="d688c-518">trademark</span><span class="sxs-lookup"><span data-stu-id="d688c-518">trademark</span></span>             | <span data-ttu-id="d688c-519">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="d688c-519">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="d688c-520">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="d688c-520">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="d688c-521">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="d688c-521">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="d688c-522">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="d688c-522">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="d688c-523">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="d688c-523">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="d688c-524">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-524">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="d688c-525">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="d688c-525">Bind to an object graph</span></span>

<span data-ttu-id="d688c-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="d688c-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="d688c-527">`Bind` est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-527">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-528">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="d688c-528">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="d688c-529">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-529">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="d688c-530">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="d688c-530">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="d688c-531">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="d688c-531">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="d688c-532">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="d688c-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="d688c-533">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="d688c-533">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="d688c-534">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="d688c-534">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="d688c-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="d688c-536">`Get<T>` est disponible dans ASP.NET Core 1.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="d688c-536">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="d688c-537">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-537">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="d688c-538">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="d688c-538">Bind an array to a class</span></span>

<span data-ttu-id="d688c-539">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="d688c-539">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="d688c-540"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-540">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d688c-541">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer la liaison de tableau avec un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="d688c-541">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="d688c-542">« Bind » est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-542">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="d688c-543">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="d688c-543">Binding is provided by convention.</span></span> <span data-ttu-id="d688c-544">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="d688c-544">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="d688c-545">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="d688c-545">**In-memory array processing**</span></span>

<span data-ttu-id="d688c-546">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="d688c-546">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="d688c-547">Touche</span><span class="sxs-lookup"><span data-stu-id="d688c-547">Key</span></span>             | <span data-ttu-id="d688c-548">Value</span><span class="sxs-lookup"><span data-stu-id="d688c-548">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d688c-549">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="d688c-549">array:entries:0</span></span> | <span data-ttu-id="d688c-550">value0</span><span class="sxs-lookup"><span data-stu-id="d688c-550">value0</span></span> |
| <span data-ttu-id="d688c-551">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="d688c-551">array:entries:1</span></span> | <span data-ttu-id="d688c-552">valeur1</span><span class="sxs-lookup"><span data-stu-id="d688c-552">value1</span></span> |
| <span data-ttu-id="d688c-553">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="d688c-553">array:entries:2</span></span> | <span data-ttu-id="d688c-554">valeur2</span><span class="sxs-lookup"><span data-stu-id="d688c-554">value2</span></span> |
| <span data-ttu-id="d688c-555">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="d688c-555">array:entries:4</span></span> | <span data-ttu-id="d688c-556">value4</span><span class="sxs-lookup"><span data-stu-id="d688c-556">value4</span></span> |
| <span data-ttu-id="d688c-557">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="d688c-557">array:entries:5</span></span> | <span data-ttu-id="d688c-558">value5</span><span class="sxs-lookup"><span data-stu-id="d688c-558">value5</span></span> |

<span data-ttu-id="d688c-559">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="d688c-559">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="d688c-560">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="d688c-560">The array skips a value for index &num;3.</span></span> <span data-ttu-id="d688c-561">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="d688c-561">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="d688c-562">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="d688c-562">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="d688c-563">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="d688c-563">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="d688c-564">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d688c-564">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d688c-565">La syntaxe [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="d688c-565">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="d688c-566">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-566">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="d688c-567">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="d688c-567">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="d688c-568">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="d688c-568">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="d688c-569">0</span><span class="sxs-lookup"><span data-stu-id="d688c-569">0</span></span>                            | <span data-ttu-id="d688c-570">value0</span><span class="sxs-lookup"><span data-stu-id="d688c-570">value0</span></span>                       |
| <span data-ttu-id="d688c-571">1</span><span class="sxs-lookup"><span data-stu-id="d688c-571">1</span></span>                            | <span data-ttu-id="d688c-572">valeur1</span><span class="sxs-lookup"><span data-stu-id="d688c-572">value1</span></span>                       |
| <span data-ttu-id="d688c-573">2</span><span class="sxs-lookup"><span data-stu-id="d688c-573">2</span></span>                            | <span data-ttu-id="d688c-574">valeur2</span><span class="sxs-lookup"><span data-stu-id="d688c-574">value2</span></span>                       |
| <span data-ttu-id="d688c-575">3</span><span class="sxs-lookup"><span data-stu-id="d688c-575">3</span></span>                            | <span data-ttu-id="d688c-576">value4</span><span class="sxs-lookup"><span data-stu-id="d688c-576">value4</span></span>                       |
| <span data-ttu-id="d688c-577">4</span><span class="sxs-lookup"><span data-stu-id="d688c-577">4</span></span>                            | <span data-ttu-id="d688c-578">value5</span><span class="sxs-lookup"><span data-stu-id="d688c-578">value5</span></span>                       |

<span data-ttu-id="d688c-579">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="d688c-579">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="d688c-580">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="d688c-580">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="d688c-581">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="d688c-581">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="d688c-582">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-582">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="d688c-583">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-583">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="d688c-584">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="d688c-584">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="d688c-585">Dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="d688c-585">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="d688c-586">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-586">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="d688c-587">Touche</span><span class="sxs-lookup"><span data-stu-id="d688c-587">Key</span></span>             | <span data-ttu-id="d688c-588">Value</span><span class="sxs-lookup"><span data-stu-id="d688c-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d688c-589">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="d688c-589">array:entries:3</span></span> | <span data-ttu-id="d688c-590">valeur3</span><span class="sxs-lookup"><span data-stu-id="d688c-590">value3</span></span> |

<span data-ttu-id="d688c-591">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="d688c-591">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="d688c-592">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="d688c-592">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="d688c-593">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="d688c-593">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="d688c-594">0</span><span class="sxs-lookup"><span data-stu-id="d688c-594">0</span></span>                            | <span data-ttu-id="d688c-595">value0</span><span class="sxs-lookup"><span data-stu-id="d688c-595">value0</span></span>                       |
| <span data-ttu-id="d688c-596">1</span><span class="sxs-lookup"><span data-stu-id="d688c-596">1</span></span>                            | <span data-ttu-id="d688c-597">valeur1</span><span class="sxs-lookup"><span data-stu-id="d688c-597">value1</span></span>                       |
| <span data-ttu-id="d688c-598">2</span><span class="sxs-lookup"><span data-stu-id="d688c-598">2</span></span>                            | <span data-ttu-id="d688c-599">valeur2</span><span class="sxs-lookup"><span data-stu-id="d688c-599">value2</span></span>                       |
| <span data-ttu-id="d688c-600">3</span><span class="sxs-lookup"><span data-stu-id="d688c-600">3</span></span>                            | <span data-ttu-id="d688c-601">valeur3</span><span class="sxs-lookup"><span data-stu-id="d688c-601">value3</span></span>                       |
| <span data-ttu-id="d688c-602">4</span><span class="sxs-lookup"><span data-stu-id="d688c-602">4</span></span>                            | <span data-ttu-id="d688c-603">value4</span><span class="sxs-lookup"><span data-stu-id="d688c-603">value4</span></span>                       |
| <span data-ttu-id="d688c-604">5</span><span class="sxs-lookup"><span data-stu-id="d688c-604">5</span></span>                            | <span data-ttu-id="d688c-605">value5</span><span class="sxs-lookup"><span data-stu-id="d688c-605">value5</span></span>                       |

<span data-ttu-id="d688c-606">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="d688c-606">**JSON array processing**</span></span>

<span data-ttu-id="d688c-607">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="d688c-607">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="d688c-608">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="d688c-608">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="d688c-609">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="d688c-609">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="d688c-610">Touche</span><span class="sxs-lookup"><span data-stu-id="d688c-610">Key</span></span>                     | <span data-ttu-id="d688c-611">Value</span><span class="sxs-lookup"><span data-stu-id="d688c-611">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="d688c-612">json_array:key</span><span class="sxs-lookup"><span data-stu-id="d688c-612">json_array:key</span></span>          | <span data-ttu-id="d688c-613">valueA</span><span class="sxs-lookup"><span data-stu-id="d688c-613">valueA</span></span> |
| <span data-ttu-id="d688c-614">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="d688c-614">json_array:subsection:0</span></span> | <span data-ttu-id="d688c-615">valueB</span><span class="sxs-lookup"><span data-stu-id="d688c-615">valueB</span></span> |
| <span data-ttu-id="d688c-616">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="d688c-616">json_array:subsection:1</span></span> | <span data-ttu-id="d688c-617">valueC</span><span class="sxs-lookup"><span data-stu-id="d688c-617">valueC</span></span> |
| <span data-ttu-id="d688c-618">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="d688c-618">json_array:subsection:2</span></span> | <span data-ttu-id="d688c-619">valueD</span><span class="sxs-lookup"><span data-stu-id="d688c-619">valueD</span></span> |

<span data-ttu-id="d688c-620">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="d688c-620">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="d688c-621">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="d688c-621">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="d688c-622">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="d688c-622">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="d688c-623">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="d688c-623">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="d688c-624">Valeur `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="d688c-624">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="d688c-625">0</span><span class="sxs-lookup"><span data-stu-id="d688c-625">0</span></span>                                   | <span data-ttu-id="d688c-626">valueB</span><span class="sxs-lookup"><span data-stu-id="d688c-626">valueB</span></span>                              |
| <span data-ttu-id="d688c-627">1</span><span class="sxs-lookup"><span data-stu-id="d688c-627">1</span></span>                                   | <span data-ttu-id="d688c-628">valueC</span><span class="sxs-lookup"><span data-stu-id="d688c-628">valueC</span></span>                              |
| <span data-ttu-id="d688c-629">2</span><span class="sxs-lookup"><span data-stu-id="d688c-629">2</span></span>                                   | <span data-ttu-id="d688c-630">valueD</span><span class="sxs-lookup"><span data-stu-id="d688c-630">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="d688c-631">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="d688c-631">Custom configuration provider</span></span>

<span data-ttu-id="d688c-632">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="d688c-632">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="d688c-633">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="d688c-633">The provider has the following characteristics:</span></span>

* <span data-ttu-id="d688c-634">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="d688c-634">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="d688c-635">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="d688c-635">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="d688c-636">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="d688c-636">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="d688c-637">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="d688c-637">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="d688c-638">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-638">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="d688c-639">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="d688c-639">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="d688c-640">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="d688c-640">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="d688c-641">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="d688c-641">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="d688c-642">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="d688c-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="d688c-643">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="d688c-643">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="d688c-644">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="d688c-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="d688c-645">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="d688c-645">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="d688c-646">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="d688c-646">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="d688c-647">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="d688c-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="d688c-648">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d688c-648">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="d688c-649">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="d688c-649">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="d688c-650">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="d688c-650">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="d688c-651">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="d688c-651">Access configuration during startup</span></span>

<span data-ttu-id="d688c-652">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d688c-652">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d688c-653">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="d688c-653">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="d688c-654">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="d688c-654">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="d688c-655">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="d688c-655">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="d688c-656">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="d688c-656">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="d688c-657">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="d688c-657">In a Razor Pages page:</span></span>

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

<span data-ttu-id="d688c-658">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="d688c-658">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="d688c-659">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="d688c-659">Add configuration from an external assembly</span></span>

<span data-ttu-id="d688c-660">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="d688c-660">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="d688c-661">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d688c-661">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d688c-662">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d688c-662">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="d688c-663">Présentation approfondie de la Configuration Microsoft</span><span class="sxs-lookup"><span data-stu-id="d688c-663">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
