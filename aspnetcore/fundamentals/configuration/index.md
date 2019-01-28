---
title: Configuration dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/25/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 2465570e469020ae2855508bd1bfc8528e188ebb
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073164"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="b22a2-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b22a2-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="b22a2-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b22a2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b22a2-105">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="b22a2-106">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="b22a2-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b22a2-107">Azure Key Vault</span></span>
* <span data-ttu-id="b22a2-108">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-108">Command-line arguments</span></span>
* <span data-ttu-id="b22a2-109">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="b22a2-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="b22a2-110">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-110">Directory files</span></span>
* <span data-ttu-id="b22a2-111">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-111">Environment variables</span></span>
* <span data-ttu-id="b22a2-112">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-112">In-memory .NET objects</span></span>
* <span data-ttu-id="b22a2-113">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="b22a2-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="b22a2-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b22a2-114">Azure Key Vault</span></span>
* <span data-ttu-id="b22a2-115">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-115">Command-line arguments</span></span>
* <span data-ttu-id="b22a2-116">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="b22a2-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="b22a2-117">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-117">Environment variables</span></span>
* <span data-ttu-id="b22a2-118">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-118">In-memory .NET objects</span></span>
* <span data-ttu-id="b22a2-119">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="b22a2-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="b22a2-120">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-120">Command-line arguments</span></span>
* <span data-ttu-id="b22a2-121">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="b22a2-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="b22a2-122">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-122">Environment variables</span></span>
* <span data-ttu-id="b22a2-123">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-123">In-memory .NET objects</span></span>
* <span data-ttu-id="b22a2-124">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="b22a2-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="b22a2-125">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="b22a2-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="b22a2-126">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="b22a2-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b22a2-127">Pour plus d’informations sur l’utilisation du modèle d’options, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b22a2-128">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b22a2-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b22a2-129">Ces trois packages sont inclus dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-129">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b22a2-130">Ces trois packages sont inclus dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b22a2-130">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="b22a2-131">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="b22a2-131">Host vs. app configuration</span></span>

<span data-ttu-id="b22a2-132">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="b22a2-132">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="b22a2-133">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="b22a2-133">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b22a2-134">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="b22a2-134">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="b22a2-135">Les paires clé-valeur de la configuration de l’hôte font partie de la configuration générale de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-135">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="b22a2-136">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-136">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="b22a2-137">Configuration par défaut</span><span class="sxs-lookup"><span data-stu-id="b22a2-137">Default configuration</span></span>

<span data-ttu-id="b22a2-138">Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pendant la création d’un hôte.</span><span class="sxs-lookup"><span data-stu-id="b22a2-138">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="b22a2-139">`CreateDefaultBuilder` fournit la configuration par défaut de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-139">`CreateDefaultBuilder` provides default configuration for the app.</span></span>

* <span data-ttu-id="b22a2-140">La configuration de l’hôte est fournie à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b22a2-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="b22a2-141">Des variables d’environnement préfixées avec `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b22a2-141">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="b22a2-142">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b22a2-142">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="b22a2-143">La configuration de l’application est fournie à partir des éléments suivants (dans l’ordre) :</span><span class="sxs-lookup"><span data-stu-id="b22a2-143">App configuration is provided from (in the following order):</span></span>
  * <span data-ttu-id="b22a2-144">*appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b22a2-144">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b22a2-145">*appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b22a2-145">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b22a2-146">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="b22a2-146">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="b22a2-147">Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b22a2-147">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="b22a2-148">Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b22a2-148">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="b22a2-149">Les fournisseurs de configuration sont décrits plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="b22a2-149">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="b22a2-150">Pour plus d’informations sur l’hôte et `CreateDefaultBuilder`, consultez <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-150">For more information on the host and `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="b22a2-151">Sécurité</span><span class="sxs-lookup"><span data-stu-id="b22a2-151">Security</span></span>

<span data-ttu-id="b22a2-152">Adoptez les meilleures pratiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="b22a2-152">Adopt the following best practices:</span></span>

* <span data-ttu-id="b22a2-153">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="b22a2-153">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="b22a2-154">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="b22a2-154">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="b22a2-155">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="b22a2-155">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="b22a2-156">En savoir plus sur [l’utilisation de plusieurs environnements](xref:fundamentals/environments) et la gestion du [stockage sécurisé des secrets d’application dans le développement avec Secret Manager](xref:security/app-secrets) (contient des conseils sur l’utilisation des variables d’environnement pour stocker les données sensibles).</span><span class="sxs-lookup"><span data-stu-id="b22a2-156">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="b22a2-157">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="b22a2-157">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="b22a2-158">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="b22a2-158">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="b22a2-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) constitue une option pour le stockage sécurisé des secrets d’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="b22a2-160">Pour plus d'informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-160">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="b22a2-161">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="b22a2-161">Hierarchical configuration data</span></span>

<span data-ttu-id="b22a2-162">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-162">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="b22a2-163">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="b22a2-163">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="b22a2-164">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-164">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="b22a2-165">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="b22a2-165">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="b22a2-166">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-166">section0:key0</span></span>
* <span data-ttu-id="b22a2-167">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-167">section0:key1</span></span>
* <span data-ttu-id="b22a2-168">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-168">section1:key0</span></span>
* <span data-ttu-id="b22a2-169">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-169">section1:key1</span></span>

<span data-ttu-id="b22a2-170">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="b22a2-171">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="b22a2-171">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="b22a2-172">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-172">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="b22a2-173">Conventions</span><span class="sxs-lookup"><span data-stu-id="b22a2-173">Conventions</span></span>

<span data-ttu-id="b22a2-174">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="b22a2-174">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="b22a2-175">Les fournisseurs de configuration de fichier peuvent recharger la configuration lorsqu’un fichier de paramètres sous-jacent est modifié après le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-175">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="b22a2-176">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="b22a2-176">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="b22a2-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur [Dependency Injection (DI)](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b22a2-178">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="b22a2-178">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="b22a2-179">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="b22a2-179">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="b22a2-180">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="b22a2-180">Keys are case-insensitive.</span></span> <span data-ttu-id="b22a2-181">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="b22a2-181">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="b22a2-182">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="b22a2-182">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="b22a2-183">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="b22a2-183">Hierarchical keys</span></span>
  * <span data-ttu-id="b22a2-184">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="b22a2-184">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="b22a2-185">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="b22a2-185">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="b22a2-186">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et converti en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="b22a2-186">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="b22a2-187">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="b22a2-187">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="b22a2-188">Vous devez fournir du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-188">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="b22a2-189"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-189">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b22a2-190">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="b22a2-190">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="b22a2-191">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="b22a2-191">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="b22a2-192">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="b22a2-192">Values are strings.</span></span>
* <span data-ttu-id="b22a2-193">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="b22a2-193">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="b22a2-194">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="b22a2-194">Providers</span></span>

<span data-ttu-id="b22a2-195">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b22a2-195">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="b22a2-196">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="b22a2-196">Provider</span></span> | <span data-ttu-id="b22a2-197">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="b22a2-197">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="b22a2-198">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="b22a2-198">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="b22a2-199">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b22a2-199">Azure Key Vault</span></span> |
| [<span data-ttu-id="b22a2-200">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-200">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="b22a2-201">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-201">Command-line parameters</span></span> |
| [<span data-ttu-id="b22a2-202">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="b22a2-202">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="b22a2-203">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="b22a2-203">Custom source</span></span> |
| [<span data-ttu-id="b22a2-204">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-204">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="b22a2-205">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-205">Environment variables</span></span> |
| [<span data-ttu-id="b22a2-206">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="b22a2-206">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="b22a2-207">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="b22a2-207">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="b22a2-208">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="b22a2-208">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="b22a2-209">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-209">Directory files</span></span> |
| [<span data-ttu-id="b22a2-210">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-210">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="b22a2-211">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-211">In-memory collections</span></span> |
| <span data-ttu-id="b22a2-212">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="b22a2-212">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="b22a2-213">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="b22a2-213">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="b22a2-214">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="b22a2-214">Provider</span></span> | <span data-ttu-id="b22a2-215">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="b22a2-215">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="b22a2-216">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="b22a2-216">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="b22a2-217">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b22a2-217">Azure Key Vault</span></span> |
| [<span data-ttu-id="b22a2-218">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="b22a2-219">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-219">Command-line parameters</span></span> |
| [<span data-ttu-id="b22a2-220">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="b22a2-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="b22a2-221">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="b22a2-221">Custom source</span></span> |
| [<span data-ttu-id="b22a2-222">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="b22a2-223">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-223">Environment variables</span></span> |
| [<span data-ttu-id="b22a2-224">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="b22a2-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="b22a2-225">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="b22a2-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="b22a2-226">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-226">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="b22a2-227">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-227">In-memory collections</span></span> |
| <span data-ttu-id="b22a2-228">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="b22a2-228">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="b22a2-229">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="b22a2-229">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="b22a2-230">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="b22a2-230">Provider</span></span> | <span data-ttu-id="b22a2-231">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="b22a2-231">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="b22a2-232">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-232">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="b22a2-233">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-233">Command-line parameters</span></span> |
| [<span data-ttu-id="b22a2-234">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="b22a2-234">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="b22a2-235">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="b22a2-235">Custom source</span></span> |
| [<span data-ttu-id="b22a2-236">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-236">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="b22a2-237">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-237">Environment variables</span></span> |
| [<span data-ttu-id="b22a2-238">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="b22a2-238">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="b22a2-239">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="b22a2-239">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="b22a2-240">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-240">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="b22a2-241">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-241">In-memory collections</span></span> |
| <span data-ttu-id="b22a2-242">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="b22a2-242">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="b22a2-243">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="b22a2-243">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="b22a2-244">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="b22a2-244">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="b22a2-245">Les fournisseurs de configuration décrits dans cette rubrique sont décrits dans l’ordre alphabétique, et non dans l’ordre où votre code peut les organiser.</span><span class="sxs-lookup"><span data-stu-id="b22a2-245">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="b22a2-246">Organisez les fournisseurs de configuration dans votre code en fonction de vos priorités pour les sources de configuration sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="b22a2-246">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="b22a2-247">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="b22a2-247">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="b22a2-248">Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)</span><span class="sxs-lookup"><span data-stu-id="b22a2-248">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="b22a2-249">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b22a2-249">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="b22a2-250">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="b22a2-250">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="b22a2-251">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-251">Environment variables</span></span>
1. <span data-ttu-id="b22a2-252">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-252">Command-line arguments</span></span>

<span data-ttu-id="b22a2-253">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="b22a2-253">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-254">Cette séquence de fournisseurs est mise en place lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-254">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b22a2-255">Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b22a2-255">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-256">Cette séquence de fournisseurs peut être créée pour l’application (pas l’hôte) avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> et un appel à sa méthode <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> dans `Startup` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-256">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="b22a2-257">Dans l’exemple précédent, le nom de l’environnement (`env.EnvironmentName`) et le nom de l’assembly d’application (`env.ApplicationName`) sont fournis par <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-257">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="b22a2-258">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-258">For more information, see <xref:fundamentals/environments>.</span></span> <span data-ttu-id="b22a2-259">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-259">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-260">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-260">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
<span data-ttu-id="b22a2-261">.</span><span class="sxs-lookup"><span data-stu-id="b22a2-261">.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="b22a2-262">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="b22a2-262">ConfigureAppConfiguration</span></span>

<span data-ttu-id="b22a2-263">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> :</span><span class="sxs-lookup"><span data-stu-id="b22a2-263">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="b22a2-264">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-264">Command-line Configuration Provider</span></span>

<span data-ttu-id="b22a2-265"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b22a2-265">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="b22a2-266">Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-266">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-267">`AddCommandLine` est appelé automatiquement lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-267">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b22a2-268">Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b22a2-268">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b22a2-269">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="b22a2-269">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b22a2-270">Configuration facultative à partir d’*appsettings.json* et d’*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-270">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="b22a2-271">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="b22a2-271">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b22a2-272">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="b22a2-272">Environment variables.</span></span>

<span data-ttu-id="b22a2-273">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b22a2-273">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="b22a2-274">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="b22a2-274">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="b22a2-275">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="b22a2-275">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="b22a2-276">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="b22a2-276">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b22a2-277">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-277">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="b22a2-278">`AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-278">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b22a2-279">Si vous devez fournir la configuration de l'application tout en étant capable de remplacer cette configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l'application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-279">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="b22a2-280">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-280">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b22a2-281">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-281">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="b22a2-282">`AddCommandLine` a déjà été appelé par `CreateDefaultBuilder` lorsque `UseConfiguration` est appelé.</span><span class="sxs-lookup"><span data-stu-id="b22a2-282">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="b22a2-283">Si vous devez fournir la configuration de l'application tout en étant capable de remplacer cette configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l'application sur un `ConfigurationBuilder` et appelez `AddCommandLine` en dernier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-283">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="b22a2-284">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-284">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-285">Pour activer la configuration en ligne de commande, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-285">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b22a2-286">Appelez le fournisseur en dernier pour permettre aux arguments de ligne de commande passés au moment de l’exécution de remplacer la configuration définie par les autres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-286">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="b22a2-287">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="b22a2-287">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

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

<span data-ttu-id="b22a2-288">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="b22a2-288">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-289">L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-289">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-290">L’exemple d’application 1.x appelle <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> sur un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-290">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="b22a2-291">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="b22a2-291">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="b22a2-292">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-292">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="b22a2-293">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-293">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b22a2-294">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-294">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="b22a2-295">Arguments</span><span class="sxs-lookup"><span data-stu-id="b22a2-295">Arguments</span></span>

<span data-ttu-id="b22a2-296">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="b22a2-296">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="b22a2-297">La valeur peut être null si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="b22a2-297">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="b22a2-298">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="b22a2-298">Key prefix</span></span>               | <span data-ttu-id="b22a2-299">Exemple</span><span class="sxs-lookup"><span data-stu-id="b22a2-299">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="b22a2-300">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="b22a2-300">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="b22a2-301">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="b22a2-301">Two dashes (`--`)</span></span>        | <span data-ttu-id="b22a2-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="b22a2-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="b22a2-303">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="b22a2-303">Forward slash (`/`)</span></span>      | <span data-ttu-id="b22a2-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="b22a2-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="b22a2-305">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="b22a2-305">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="b22a2-306">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="b22a2-306">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="b22a2-307">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="b22a2-307">Switch mappings</span></span>

<span data-ttu-id="b22a2-308">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="b22a2-308">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="b22a2-309">Lorsque vous générez manuellement la configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, vous pouvez fournir un dictionnaire des remplacements de commutateur à la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-309">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="b22a2-310">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b22a2-310">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="b22a2-311">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-311">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="b22a2-312">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="b22a2-312">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="b22a2-313">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="b22a2-313">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="b22a2-314">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="b22a2-314">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="b22a2-315">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="b22a2-315">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b22a2-316">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="b22a2-316">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b22a2-317">Comme indiqué dans l’exemple précédent, l’appel à `CreateDefaultBuilder` ne doit pas passer des arguments lorsque des correspondances de commutateur sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="b22a2-317">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="b22a2-318">L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-318">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b22a2-319">Si les arguments incluent un commutateur mappé et sont passés à `CreateDefaultBuilder`, son fournisseur `AddCommandLine` ne parvient pas à s’initialiser avec un <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-319">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="b22a2-320">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="b22a2-320">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="b22a2-321">Comme indiqué dans l’exemple précédent, l’appel à `CreateDefaultBuilder` ne doit pas passer des arguments lorsque des correspondances de commutateur sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="b22a2-321">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="b22a2-322">L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-322">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b22a2-323">Si les arguments incluent un commutateur mappé et sont passés à `CreateDefaultBuilder`, son fournisseur `AddCommandLine` ne parvient pas à s’initialiser avec un <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-323">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="b22a2-324">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de mappage de commutateur.</span><span class="sxs-lookup"><span data-stu-id="b22a2-324">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="b22a2-325">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="b22a2-325">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="b22a2-326">Touche</span><span class="sxs-lookup"><span data-stu-id="b22a2-326">Key</span></span>       | <span data-ttu-id="b22a2-327">Value</span><span class="sxs-lookup"><span data-stu-id="b22a2-327">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="b22a2-328">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="b22a2-328">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="b22a2-329">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="b22a2-329">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="b22a2-330">Touche</span><span class="sxs-lookup"><span data-stu-id="b22a2-330">Key</span></span>               | <span data-ttu-id="b22a2-331">Value</span><span class="sxs-lookup"><span data-stu-id="b22a2-331">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="b22a2-332">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-332">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="b22a2-333"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b22a2-333">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="b22a2-334">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-334">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b22a2-335">Lors de l’utilisation de clés hiérarchiques dans les variables d’environnement, un séparateur sous forme de signe deux-points (`:`) peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="b22a2-335">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="b22a2-336">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et remplacé par un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="b22a2-336">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="b22a2-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) vous permet de définir des variables d’environnement dans le portail Azure capables de remplacer la configuration d’application à l’aide du Fournisseur de configuration de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="b22a2-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="b22a2-338">Pour plus d’informations, consultez les [applications Azure : Remplacer la configuration de l’application à l’aide du Portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="b22a2-338">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-339">`AddEnvironmentVariables` est appelé automatiquement pour les variables d’environnement précédées de `ASPNETCORE_` à l’initialisation d’un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-339">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="b22a2-340">Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b22a2-340">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b22a2-341">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="b22a2-341">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b22a2-342">Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="b22a2-342">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="b22a2-343">Configuration facultative à partir d’*appsettings.json* et d’*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-343">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="b22a2-344">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="b22a2-344">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b22a2-345">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-345">Command-line arguments.</span></span>

<span data-ttu-id="b22a2-346">Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-346">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="b22a2-347">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-347">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b22a2-348">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-348">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="b22a2-349">Si vous devez fournir la configuration de l'application à partir de variables d'environnement supplémentaires, appelez les fournisseurs supplémentaires de l'application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> et appelez `AddEnvironmentVariables` avec le préfixe.</span><span class="sxs-lookup"><span data-stu-id="b22a2-349">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="b22a2-350">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-350">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b22a2-351">Appelez la méthode d’extension `AddEnvironmentVariables` sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-351">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="b22a2-352">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-352">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="b22a2-353">Si vous devez fournir la configuration de l'application à partir de variables d'environnement supplémentaires, appelez les fournisseurs supplémentaires de l'application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> et appelez `AddEnvironmentVariables` avec le préfixe.</span><span class="sxs-lookup"><span data-stu-id="b22a2-353">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="b22a2-354">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-354">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-355">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="b22a2-355">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="b22a2-356">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="b22a2-356">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-357">L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-357">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-358">L’exemple d’application 1.x appelle `AddEnvironmentVariables` sur un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-358">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="b22a2-359">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-359">Run the sample app.</span></span> <span data-ttu-id="b22a2-360">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-360">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b22a2-361">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-361">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="b22a2-362">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="b22a2-362">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="b22a2-363">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement en fonction de celles qui commencent par :</span><span class="sxs-lookup"><span data-stu-id="b22a2-363">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="b22a2-364">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="b22a2-364">ASPNETCORE_</span></span>
* <span data-ttu-id="b22a2-365">urls</span><span class="sxs-lookup"><span data-stu-id="b22a2-365">urls</span></span>
* <span data-ttu-id="b22a2-366">Journalisation</span><span class="sxs-lookup"><span data-stu-id="b22a2-366">Logging</span></span>
* <span data-ttu-id="b22a2-367">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="b22a2-367">ENVIRONMENT</span></span>
* <span data-ttu-id="b22a2-368">contentRoot</span><span class="sxs-lookup"><span data-stu-id="b22a2-368">contentRoot</span></span>
* <span data-ttu-id="b22a2-369">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="b22a2-369">AllowedHosts</span></span>
* <span data-ttu-id="b22a2-370">applicationName</span><span class="sxs-lookup"><span data-stu-id="b22a2-370">applicationName</span></span>
* <span data-ttu-id="b22a2-371">CommandLine</span><span class="sxs-lookup"><span data-stu-id="b22a2-371">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-372">Si vous souhaitez exposer toutes les variables d’environnement disponibles pour l’application, remplacez `FilteredConfiguration` dans *Pages/Index.cshtml.cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="b22a2-372">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-373">Si vous souhaitez exposer toutes les variables d’environnement disponibles pour l’application, remplacez `FilteredConfiguration` dans *Controllers/HomeController.cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="b22a2-373">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="b22a2-374">Préfixes</span><span class="sxs-lookup"><span data-stu-id="b22a2-374">Prefixes</span></span>

<span data-ttu-id="b22a2-375">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe pour la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-375">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="b22a2-376">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-376">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="b22a2-377">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="b22a2-377">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-378">La méthode pratique statique `CreateDefaultBuilder` crée un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> pour établir l’hôte de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-378">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="b22a2-379">Lorsque `WebHostBuilder` est créé, il trouve la configuration de son hôte dans les variables d’environnement avec le préfixe `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-379">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="b22a2-380">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="b22a2-380">**Connection string prefixes**</span></span>

<span data-ttu-id="b22a2-381">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-381">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="b22a2-382">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-382">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="b22a2-383">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="b22a2-383">Connection string prefix</span></span> | <span data-ttu-id="b22a2-384">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="b22a2-384">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="b22a2-385">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="b22a2-385">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="b22a2-386">MySQL</span><span class="sxs-lookup"><span data-stu-id="b22a2-386">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="b22a2-387">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b22a2-387">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="b22a2-388">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b22a2-388">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="b22a2-389">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="b22a2-389">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="b22a2-390">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="b22a2-390">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="b22a2-391">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="b22a2-391">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="b22a2-392">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="b22a2-392">Environment variable key</span></span> | <span data-ttu-id="b22a2-393">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="b22a2-393">Converted configuration key</span></span> | <span data-ttu-id="b22a2-394">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="b22a2-394">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b22a2-395">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="b22a2-395">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b22a2-396">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-396">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b22a2-397">Valeur : `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="b22a2-397">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b22a2-398">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-398">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b22a2-399">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b22a2-399">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b22a2-400">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-400">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b22a2-401">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b22a2-401">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="b22a2-402">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="b22a2-402">File Configuration Provider</span></span>

<span data-ttu-id="b22a2-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="b22a2-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="b22a2-404">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="b22a2-404">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="b22a2-405">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="b22a2-405">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="b22a2-406">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="b22a2-406">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="b22a2-407">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="b22a2-407">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="b22a2-408">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="b22a2-408">INI Configuration Provider</span></span>

<span data-ttu-id="b22a2-409"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b22a2-409">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="b22a2-410">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-410">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b22a2-411">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="b22a2-411">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="b22a2-412">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="b22a2-412">Overloads permit specifying:</span></span>

* <span data-ttu-id="b22a2-413">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="b22a2-413">Whether the file is optional.</span></span>
* <span data-ttu-id="b22a2-414">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="b22a2-414">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b22a2-415">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-415">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b22a2-416">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="b22a2-416">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b22a2-417">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-417">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-418">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-418">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-419">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-419">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b22a2-420">Lors de l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-420">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="b22a2-421">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-421">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-422">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-422">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-423">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-423">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-424">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="b22a2-424">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

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

<span data-ttu-id="b22a2-425">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-425">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-426">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-426">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-427">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="b22a2-427">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="b22a2-428">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-428">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b22a2-429">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-429">section0:key0</span></span>
* <span data-ttu-id="b22a2-430">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-430">section0:key1</span></span>
* <span data-ttu-id="b22a2-431">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="b22a2-431">section1:subsection:key</span></span>
* <span data-ttu-id="b22a2-432">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="b22a2-432">section2:subsection0:key</span></span>
* <span data-ttu-id="b22a2-433">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="b22a2-433">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="b22a2-434">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="b22a2-434">JSON Configuration Provider</span></span>

<span data-ttu-id="b22a2-435"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b22a2-435">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="b22a2-436">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-436">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b22a2-437">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="b22a2-437">Overloads permit specifying:</span></span>

* <span data-ttu-id="b22a2-438">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="b22a2-438">Whether the file is optional.</span></span>
* <span data-ttu-id="b22a2-439">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="b22a2-439">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b22a2-440">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-440">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-441">`AddJsonFile` est appelé automatiquement deux fois lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-441">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b22a2-442">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="b22a2-442">The method is called to load configuration from:</span></span>

* <span data-ttu-id="b22a2-443">*appSettings.JSON* &ndash; Ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-443">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="b22a2-444">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-444">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="b22a2-445">*appsettings.{Environment}.json* &ndash; La version de l’environnement du fichier est chargée à partir du fichier [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="b22a2-445">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="b22a2-446">Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b22a2-446">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b22a2-447">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="b22a2-447">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b22a2-448">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="b22a2-448">Environment variables.</span></span>
* <span data-ttu-id="b22a2-449">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="b22a2-449">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b22a2-450">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b22a2-450">Command-line arguments.</span></span>

<span data-ttu-id="b22a2-451">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-451">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="b22a2-452">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-452">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b22a2-453">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :</span><span class="sxs-lookup"><span data-stu-id="b22a2-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="b22a2-454">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-455">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-456">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b22a2-457">Vous pouvez aussi appeler directement la méthode d’extension `AddJsonFile` sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-457">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b22a2-458">Lors de l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-458">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="b22a2-459">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-459">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-460">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-460">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-461">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-461">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-462">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="b22a2-462">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

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

<span data-ttu-id="b22a2-463">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-463">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-464">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-464">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-465">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="b22a2-465">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-466">L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut deux appels à `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-466">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="b22a2-467">La configuration est chargée à partir d’*appsettings.json* et d’*appsettings{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-467">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-468">L’exemple d’application 1.x appelle `AddJsonFile` deux fois sur un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-468">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="b22a2-469">La configuration est chargée à partir d’*appsettings.json* et d’*appsettings{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-469">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="b22a2-470">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-470">Run the sample app.</span></span> <span data-ttu-id="b22a2-471">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-471">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b22a2-472">Notez que la sortie contient des paires clé-valeur pour la configuration représentée dans le tableau en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="b22a2-472">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="b22a2-473">Les clés de configuration de la journalisation utilisent le signe deux-points (`:`) comme séparateur hiérarchique.</span><span class="sxs-lookup"><span data-stu-id="b22a2-473">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="b22a2-474">Touche</span><span class="sxs-lookup"><span data-stu-id="b22a2-474">Key</span></span>                        | <span data-ttu-id="b22a2-475">Valeur de développement</span><span class="sxs-lookup"><span data-stu-id="b22a2-475">Development Value</span></span> | <span data-ttu-id="b22a2-476">Valeur de production</span><span class="sxs-lookup"><span data-stu-id="b22a2-476">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="b22a2-477">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="b22a2-477">Logging:LogLevel:System</span></span>    | <span data-ttu-id="b22a2-478">Information</span><span class="sxs-lookup"><span data-stu-id="b22a2-478">Information</span></span>       | <span data-ttu-id="b22a2-479">Information</span><span class="sxs-lookup"><span data-stu-id="b22a2-479">Information</span></span>      |
| <span data-ttu-id="b22a2-480">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="b22a2-480">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="b22a2-481">Information</span><span class="sxs-lookup"><span data-stu-id="b22a2-481">Information</span></span>       | <span data-ttu-id="b22a2-482">Information</span><span class="sxs-lookup"><span data-stu-id="b22a2-482">Information</span></span>      |
| <span data-ttu-id="b22a2-483">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="b22a2-483">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="b22a2-484">Débogage</span><span class="sxs-lookup"><span data-stu-id="b22a2-484">Debug</span></span>             | <span data-ttu-id="b22a2-485">Error</span><span class="sxs-lookup"><span data-stu-id="b22a2-485">Error</span></span>            |
| <span data-ttu-id="b22a2-486">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="b22a2-486">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="b22a2-487">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="b22a2-487">XML Configuration Provider</span></span>

<span data-ttu-id="b22a2-488"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b22a2-488">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="b22a2-489">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-489">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b22a2-490">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="b22a2-490">Overloads permit specifying:</span></span>

* <span data-ttu-id="b22a2-491">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="b22a2-491">Whether the file is optional.</span></span>
* <span data-ttu-id="b22a2-492">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="b22a2-492">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b22a2-493">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-493">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b22a2-494">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="b22a2-494">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="b22a2-495">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-495">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b22a2-496">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="b22a2-496">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b22a2-497">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-497">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-498">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-498">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-499">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-499">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b22a2-500">Lors de l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-500">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="b22a2-501">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-501">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-502">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-502">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-503">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-503">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-504">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="b22a2-504">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

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

<span data-ttu-id="b22a2-505">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-505">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-506">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-506">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-507">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="b22a2-507">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="b22a2-508">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-508">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b22a2-509">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-509">section0:key0</span></span>
* <span data-ttu-id="b22a2-510">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-510">section0:key1</span></span>
* <span data-ttu-id="b22a2-511">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-511">section1:key0</span></span>
* <span data-ttu-id="b22a2-512">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-512">section1:key1</span></span>

<span data-ttu-id="b22a2-513">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="b22a2-513">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="b22a2-514">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-514">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b22a2-515">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-515">section:section0:key:key0</span></span>
* <span data-ttu-id="b22a2-516">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-516">section:section0:key:key1</span></span>
* <span data-ttu-id="b22a2-517">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-517">section:section1:key:key0</span></span>
* <span data-ttu-id="b22a2-518">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-518">section:section1:key:key1</span></span>

<span data-ttu-id="b22a2-519">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="b22a2-519">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="b22a2-520">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-520">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b22a2-521">key:attribute</span><span class="sxs-lookup"><span data-stu-id="b22a2-521">key:attribute</span></span>
* <span data-ttu-id="b22a2-522">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="b22a2-522">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="b22a2-523">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="b22a2-523">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="b22a2-524">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-524">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="b22a2-525">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-525">The key is the file name.</span></span> <span data-ttu-id="b22a2-526">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="b22a2-526">The value contains the file's contents.</span></span> <span data-ttu-id="b22a2-527">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="b22a2-527">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="b22a2-528">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-528">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="b22a2-529">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="b22a2-529">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="b22a2-530">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="b22a2-530">Overloads permit specifying:</span></span>

* <span data-ttu-id="b22a2-531">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="b22a2-531">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="b22a2-532">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="b22a2-532">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="b22a2-533">Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="b22a2-533">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="b22a2-534">Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-534">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="b22a2-535">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="b22a2-535">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b22a2-536">Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-536">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b22a2-537">`SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-537">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-538">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-538">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="b22a2-539">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="b22a2-539">Memory Configuration Provider</span></span>

<span data-ttu-id="b22a2-540">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-540">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="b22a2-541">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-541">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b22a2-542">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-542">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b22a2-543">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="b22a2-543">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b22a2-544">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-544">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b22a2-545">Lors de l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-545">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

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
}
```

<span data-ttu-id="b22a2-546">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-546">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-547">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="b22a2-547">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

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

## <a name="getvalue"></a><span data-ttu-id="b22a2-548">GetValue</span><span class="sxs-lookup"><span data-stu-id="b22a2-548">GetValue</span></span>

<span data-ttu-id="b22a2-549">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur à partir de la configuration avec une clé spécifiée et la convertit selon le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="b22a2-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="b22a2-550">Une surcharge vous permet de fournir une valeur par défaut si la clé est introuvable.</span><span class="sxs-lookup"><span data-stu-id="b22a2-550">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="b22a2-551">L’exemple suivant extrait la valeur de la chaîne à partir de la configuration avec la clé `NumberKey`, tape la valeur en tant que `int` et stocke la valeur dans la variable `intValue`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-551">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="b22a2-552">Si `NumberKey` est introuvable dans les clés de configuration, `intValue` reçoit la valeur par défaut `99` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-552">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="b22a2-553">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="b22a2-553">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="b22a2-554">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="b22a2-554">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="b22a2-555">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="b22a2-555">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="b22a2-556">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-556">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="b22a2-557">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-557">section0:key0</span></span>
* <span data-ttu-id="b22a2-558">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-558">section0:key1</span></span>
* <span data-ttu-id="b22a2-559">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-559">section1:key0</span></span>
* <span data-ttu-id="b22a2-560">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-560">section1:key1</span></span>
* <span data-ttu-id="b22a2-561">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-561">section2:subsection0:key0</span></span>
* <span data-ttu-id="b22a2-562">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-562">section2:subsection0:key1</span></span>
* <span data-ttu-id="b22a2-563">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="b22a2-563">section2:subsection1:key0</span></span>
* <span data-ttu-id="b22a2-564">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="b22a2-564">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="b22a2-565">GetSection</span><span class="sxs-lookup"><span data-stu-id="b22a2-565">GetSection</span></span>

<span data-ttu-id="b22a2-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="b22a2-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="b22a2-567">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-567">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-568">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="b22a2-568">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="b22a2-569">`configSection` n’a pas de valeur, seulement une clé et un chemin.</span><span class="sxs-lookup"><span data-stu-id="b22a2-569">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="b22a2-570">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="b22a2-570">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="b22a2-571">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-571">`GetSection` never returns `null`.</span></span> <span data-ttu-id="b22a2-572">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="b22a2-572">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="b22a2-573">Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli.</span><span class="sxs-lookup"><span data-stu-id="b22a2-573">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="b22a2-574"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.</span><span class="sxs-lookup"><span data-stu-id="b22a2-574">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="b22a2-575">GetChildren</span><span class="sxs-lookup"><span data-stu-id="b22a2-575">GetChildren</span></span>

<span data-ttu-id="b22a2-576">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="b22a2-576">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="b22a2-577">Existe</span><span class="sxs-lookup"><span data-stu-id="b22a2-577">Exists</span></span>

<span data-ttu-id="b22a2-578">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="b22a2-578">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="b22a2-579">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-579">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="b22a2-580">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="b22a2-580">Bind to a class</span></span>

<span data-ttu-id="b22a2-581">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="b22a2-581">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="b22a2-582">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-582">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b22a2-583">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="b22a2-583">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="b22a2-584">`Bind` est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-584">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-585">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b22a2-585">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-586">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-586">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="b22a2-587">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="b22a2-587">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="b22a2-588">Touche</span><span class="sxs-lookup"><span data-stu-id="b22a2-588">Key</span></span>                   | <span data-ttu-id="b22a2-589">Value</span><span class="sxs-lookup"><span data-stu-id="b22a2-589">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="b22a2-590">starship:name</span><span class="sxs-lookup"><span data-stu-id="b22a2-590">starship:name</span></span>         | <span data-ttu-id="b22a2-591">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="b22a2-591">USS Enterprise</span></span>                                    |
| <span data-ttu-id="b22a2-592">starship:registry</span><span class="sxs-lookup"><span data-stu-id="b22a2-592">starship:registry</span></span>     | <span data-ttu-id="b22a2-593">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="b22a2-593">NCC-1701</span></span>                                          |
| <span data-ttu-id="b22a2-594">starship:class</span><span class="sxs-lookup"><span data-stu-id="b22a2-594">starship:class</span></span>        | <span data-ttu-id="b22a2-595">Constitution</span><span class="sxs-lookup"><span data-stu-id="b22a2-595">Constitution</span></span>                                      |
| <span data-ttu-id="b22a2-596">starship:length</span><span class="sxs-lookup"><span data-stu-id="b22a2-596">starship:length</span></span>       | <span data-ttu-id="b22a2-597">304.8</span><span class="sxs-lookup"><span data-stu-id="b22a2-597">304.8</span></span>                                             |
| <span data-ttu-id="b22a2-598">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="b22a2-598">starship:commissioned</span></span> | <span data-ttu-id="b22a2-599">False</span><span class="sxs-lookup"><span data-stu-id="b22a2-599">False</span></span>                                             |
| <span data-ttu-id="b22a2-600">trademark</span><span class="sxs-lookup"><span data-stu-id="b22a2-600">trademark</span></span>             | <span data-ttu-id="b22a2-601">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="b22a2-601">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="b22a2-602">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-602">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="b22a2-603">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="b22a2-603">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="b22a2-604">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-604">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="b22a2-605">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="b22a2-605">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

<span data-ttu-id="b22a2-606">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-606">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="b22a2-607">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="b22a2-607">Bind to an object graph</span></span>

<span data-ttu-id="b22a2-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="b22a2-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="b22a2-609">`Bind` est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-609">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b22a2-610">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b22a2-610">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-611">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-611">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="b22a2-612">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-612">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="b22a2-613">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="b22a2-613">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="b22a2-614">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="b22a2-614">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="b22a2-615">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-615">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="b22a2-616">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="b22a2-616">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

<span data-ttu-id="b22a2-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b22a2-618">`Get<T>` est disponible dans ASP.NET Core 1.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="b22a2-618">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="b22a2-619">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-619">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="b22a2-620">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="b22a2-620">Bind an array to a class</span></span>

<span data-ttu-id="b22a2-621">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="b22a2-621">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="b22a2-622"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-622">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b22a2-623">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer la liaison de tableau avec un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="b22a2-623">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="b22a2-624">« Bind » est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-624">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="b22a2-625">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="b22a2-625">Binding is provided by convention.</span></span> <span data-ttu-id="b22a2-626">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="b22a2-626">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="b22a2-627">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="b22a2-627">**In-memory array processing**</span></span>

<span data-ttu-id="b22a2-628">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="b22a2-628">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="b22a2-629">Touche</span><span class="sxs-lookup"><span data-stu-id="b22a2-629">Key</span></span>             | <span data-ttu-id="b22a2-630">Value</span><span class="sxs-lookup"><span data-stu-id="b22a2-630">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b22a2-631">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="b22a2-631">array:entries:0</span></span> | <span data-ttu-id="b22a2-632">value0</span><span class="sxs-lookup"><span data-stu-id="b22a2-632">value0</span></span> |
| <span data-ttu-id="b22a2-633">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="b22a2-633">array:entries:1</span></span> | <span data-ttu-id="b22a2-634">value1</span><span class="sxs-lookup"><span data-stu-id="b22a2-634">value1</span></span> |
| <span data-ttu-id="b22a2-635">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="b22a2-635">array:entries:2</span></span> | <span data-ttu-id="b22a2-636">value2</span><span class="sxs-lookup"><span data-stu-id="b22a2-636">value2</span></span> |
| <span data-ttu-id="b22a2-637">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="b22a2-637">array:entries:4</span></span> | <span data-ttu-id="b22a2-638">value4</span><span class="sxs-lookup"><span data-stu-id="b22a2-638">value4</span></span> |
| <span data-ttu-id="b22a2-639">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="b22a2-639">array:entries:5</span></span> | <span data-ttu-id="b22a2-640">value5</span><span class="sxs-lookup"><span data-stu-id="b22a2-640">value5</span></span> |

<span data-ttu-id="b22a2-641">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="b22a2-641">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="b22a2-642">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="b22a2-642">The array skips a value for index &num;3.</span></span> <span data-ttu-id="b22a2-643">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="b22a2-643">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="b22a2-644">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="b22a2-644">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-645">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="b22a2-645">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="b22a2-646">`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b22a2-646">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="b22a2-647">La syntaxe [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="b22a2-647">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="b22a2-648">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-648">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="b22a2-649">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b22a2-649">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b22a2-650">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b22a2-650">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b22a2-651">0</span><span class="sxs-lookup"><span data-stu-id="b22a2-651">0</span></span>                            | <span data-ttu-id="b22a2-652">value0</span><span class="sxs-lookup"><span data-stu-id="b22a2-652">value0</span></span>                       |
| <span data-ttu-id="b22a2-653">1</span><span class="sxs-lookup"><span data-stu-id="b22a2-653">1</span></span>                            | <span data-ttu-id="b22a2-654">value1</span><span class="sxs-lookup"><span data-stu-id="b22a2-654">value1</span></span>                       |
| <span data-ttu-id="b22a2-655">2</span><span class="sxs-lookup"><span data-stu-id="b22a2-655">2</span></span>                            | <span data-ttu-id="b22a2-656">value2</span><span class="sxs-lookup"><span data-stu-id="b22a2-656">value2</span></span>                       |
| <span data-ttu-id="b22a2-657">3</span><span class="sxs-lookup"><span data-stu-id="b22a2-657">3</span></span>                            | <span data-ttu-id="b22a2-658">value4</span><span class="sxs-lookup"><span data-stu-id="b22a2-658">value4</span></span>                       |
| <span data-ttu-id="b22a2-659">4</span><span class="sxs-lookup"><span data-stu-id="b22a2-659">4</span></span>                            | <span data-ttu-id="b22a2-660">value5</span><span class="sxs-lookup"><span data-stu-id="b22a2-660">value5</span></span>                       |

<span data-ttu-id="b22a2-661">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-661">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="b22a2-662">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="b22a2-662">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="b22a2-663">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="b22a2-663">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="b22a2-664">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-664">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="b22a2-665">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-665">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="b22a2-666">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="b22a2-666">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b22a2-667">Dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="b22a2-667">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b22a2-668">Dans le constructeur `Startup` :</span><span class="sxs-lookup"><span data-stu-id="b22a2-668">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="b22a2-669">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-669">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="b22a2-670">Touche</span><span class="sxs-lookup"><span data-stu-id="b22a2-670">Key</span></span>             | <span data-ttu-id="b22a2-671">Value</span><span class="sxs-lookup"><span data-stu-id="b22a2-671">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b22a2-672">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="b22a2-672">array:entries:3</span></span> | <span data-ttu-id="b22a2-673">valeur3</span><span class="sxs-lookup"><span data-stu-id="b22a2-673">value3</span></span> |

<span data-ttu-id="b22a2-674">Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="b22a2-674">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="b22a2-675">Index `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b22a2-675">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b22a2-676">Valeur `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b22a2-676">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b22a2-677">0</span><span class="sxs-lookup"><span data-stu-id="b22a2-677">0</span></span>                            | <span data-ttu-id="b22a2-678">value0</span><span class="sxs-lookup"><span data-stu-id="b22a2-678">value0</span></span>                       |
| <span data-ttu-id="b22a2-679">1</span><span class="sxs-lookup"><span data-stu-id="b22a2-679">1</span></span>                            | <span data-ttu-id="b22a2-680">value1</span><span class="sxs-lookup"><span data-stu-id="b22a2-680">value1</span></span>                       |
| <span data-ttu-id="b22a2-681">2</span><span class="sxs-lookup"><span data-stu-id="b22a2-681">2</span></span>                            | <span data-ttu-id="b22a2-682">value2</span><span class="sxs-lookup"><span data-stu-id="b22a2-682">value2</span></span>                       |
| <span data-ttu-id="b22a2-683">3</span><span class="sxs-lookup"><span data-stu-id="b22a2-683">3</span></span>                            | <span data-ttu-id="b22a2-684">valeur3</span><span class="sxs-lookup"><span data-stu-id="b22a2-684">value3</span></span>                       |
| <span data-ttu-id="b22a2-685">4</span><span class="sxs-lookup"><span data-stu-id="b22a2-685">4</span></span>                            | <span data-ttu-id="b22a2-686">value4</span><span class="sxs-lookup"><span data-stu-id="b22a2-686">value4</span></span>                       |
| <span data-ttu-id="b22a2-687">5</span><span class="sxs-lookup"><span data-stu-id="b22a2-687">5</span></span>                            | <span data-ttu-id="b22a2-688">value5</span><span class="sxs-lookup"><span data-stu-id="b22a2-688">value5</span></span>                       |

<span data-ttu-id="b22a2-689">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="b22a2-689">**JSON array processing**</span></span>

<span data-ttu-id="b22a2-690">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="b22a2-690">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="b22a2-691">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="b22a2-691">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="b22a2-692">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="b22a2-692">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="b22a2-693">Touche</span><span class="sxs-lookup"><span data-stu-id="b22a2-693">Key</span></span>                     | <span data-ttu-id="b22a2-694">Value</span><span class="sxs-lookup"><span data-stu-id="b22a2-694">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="b22a2-695">json_array:key</span><span class="sxs-lookup"><span data-stu-id="b22a2-695">json_array:key</span></span>          | <span data-ttu-id="b22a2-696">valueA</span><span class="sxs-lookup"><span data-stu-id="b22a2-696">valueA</span></span> |
| <span data-ttu-id="b22a2-697">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="b22a2-697">json_array:subsection:0</span></span> | <span data-ttu-id="b22a2-698">valueB</span><span class="sxs-lookup"><span data-stu-id="b22a2-698">valueB</span></span> |
| <span data-ttu-id="b22a2-699">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="b22a2-699">json_array:subsection:1</span></span> | <span data-ttu-id="b22a2-700">valueC</span><span class="sxs-lookup"><span data-stu-id="b22a2-700">valueC</span></span> |
| <span data-ttu-id="b22a2-701">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="b22a2-701">json_array:subsection:2</span></span> | <span data-ttu-id="b22a2-702">valueD</span><span class="sxs-lookup"><span data-stu-id="b22a2-702">valueD</span></span> |

<span data-ttu-id="b22a2-703">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="b22a2-703">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-704">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-704">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="b22a2-705">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-705">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="b22a2-706">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="b22a2-706">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="b22a2-707">Valeur `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="b22a2-707">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="b22a2-708">0</span><span class="sxs-lookup"><span data-stu-id="b22a2-708">0</span></span>                                   | <span data-ttu-id="b22a2-709">valueB</span><span class="sxs-lookup"><span data-stu-id="b22a2-709">valueB</span></span>                              |
| <span data-ttu-id="b22a2-710">1</span><span class="sxs-lookup"><span data-stu-id="b22a2-710">1</span></span>                                   | <span data-ttu-id="b22a2-711">valueC</span><span class="sxs-lookup"><span data-stu-id="b22a2-711">valueC</span></span>                              |
| <span data-ttu-id="b22a2-712">2</span><span class="sxs-lookup"><span data-stu-id="b22a2-712">2</span></span>                                   | <span data-ttu-id="b22a2-713">valueD</span><span class="sxs-lookup"><span data-stu-id="b22a2-713">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="b22a2-714">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="b22a2-714">Custom configuration provider</span></span>

<span data-ttu-id="b22a2-715">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="b22a2-715">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="b22a2-716">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="b22a2-716">The provider has the following characteristics:</span></span>

* <span data-ttu-id="b22a2-717">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-717">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="b22a2-718">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="b22a2-718">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="b22a2-719">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b22a2-719">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="b22a2-720">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="b22a2-720">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="b22a2-721">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-721">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="b22a2-722">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b22a2-722">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="b22a2-723">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="b22a2-723">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-724">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="b22a2-724">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="b22a2-725">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="b22a2-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-726">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-726">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="b22a2-727">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="b22a2-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-728">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-728">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="b22a2-729">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="b22a2-729">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="b22a2-730">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="b22a2-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-731">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-731">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="b22a2-732">*Extensions/EntityFrameworkExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="b22a2-732">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b22a2-733">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="b22a2-733">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="b22a2-734">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="b22a2-734">Access configuration during startup</span></span>

<span data-ttu-id="b22a2-735">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b22a2-735">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b22a2-736">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="b22a2-736">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="b22a2-737">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="b22a2-737">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="b22a2-738">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="b22a2-738">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="b22a2-739">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="b22a2-739">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="b22a2-740">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="b22a2-740">In a Razor Pages page:</span></span>

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

<span data-ttu-id="b22a2-741">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="b22a2-741">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="b22a2-742">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="b22a2-742">Add configuration from an external assembly</span></span>

<span data-ttu-id="b22a2-743">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22a2-743">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="b22a2-744">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b22a2-744">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b22a2-745">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b22a2-745">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="b22a2-746">Présentation approfondie de la Configuration Microsoft</span><span class="sxs-lookup"><span data-stu-id="b22a2-746">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
