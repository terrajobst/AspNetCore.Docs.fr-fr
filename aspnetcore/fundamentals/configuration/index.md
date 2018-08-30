---
title: Configuration dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: a0c57e75b28bc7c5590d20a8fa59b00b6bb9af4e
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927876"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="e8a23-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8a23-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="e8a23-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e8a23-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e8a23-105">La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="e8a23-106">Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="e8a23-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e8a23-107">Azure Key Vault</span></span>
* <span data-ttu-id="e8a23-108">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-108">Command-line arguments</span></span>
* <span data-ttu-id="e8a23-109">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="e8a23-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e8a23-110">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-110">Directory files</span></span>
* <span data-ttu-id="e8a23-111">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-111">Environment variables</span></span>
* <span data-ttu-id="e8a23-112">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-112">In-memory .NET objects</span></span>
* <span data-ttu-id="e8a23-113">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="e8a23-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="e8a23-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e8a23-114">Azure Key Vault</span></span>
* <span data-ttu-id="e8a23-115">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-115">Command-line arguments</span></span>
* <span data-ttu-id="e8a23-116">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="e8a23-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e8a23-117">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-117">Environment variables</span></span>
* <span data-ttu-id="e8a23-118">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-118">In-memory .NET objects</span></span>
* <span data-ttu-id="e8a23-119">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="e8a23-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="e8a23-120">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-120">Command-line arguments</span></span>
* <span data-ttu-id="e8a23-121">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="e8a23-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e8a23-122">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-122">Environment variables</span></span>
* <span data-ttu-id="e8a23-123">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-123">In-memory .NET objects</span></span>
* <span data-ttu-id="e8a23-124">Fichiers de paramètres</span><span class="sxs-lookup"><span data-stu-id="e8a23-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="e8a23-125">Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e8a23-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="e8a23-126">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="e8a23-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="e8a23-127">Pour plus d’informations sur l’utilisation du modèle d’options, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e8a23-128">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e8a23-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e8a23-129">Les exemples fournis dans cette rubrique s’appuient sur :</span><span class="sxs-lookup"><span data-stu-id="e8a23-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="e8a23-130">Définition du chemin de base de l’application avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="e8a23-131">`SetBasePath` est mis à la disposition d’une application en référençant le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="e8a23-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="e8a23-132">Résolution des sections des fichiers de configuration avec <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="e8a23-133">`GetSection` est mis à la disposition d’une application en référençant le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="e8a23-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="e8a23-134">Liaison de la configuration aux classes .NET avec <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> et [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="e8a23-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="e8a23-135">`Bind` et `Get<T>` sont mis à la disposition d’une application en référençant le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="e8a23-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="e8a23-136">`Get<T>` est disponible dans ASP.NET Core 1.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="e8a23-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e8a23-137">Ces trois packages sont inclus dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e8a23-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e8a23-138">Ces trois packages sont inclus dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="e8a23-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="e8a23-139">Configuration de l’hôte ou configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="e8a23-139">Host vs. app configuration</span></span>

<span data-ttu-id="e8a23-140">Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé.</span><span class="sxs-lookup"><span data-stu-id="e8a23-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="e8a23-141">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="e8a23-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e8a23-142">L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e8a23-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="e8a23-143">Les paires clé-valeur de la configuration de l’hôte font partie de la configuration générale de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="e8a23-144">Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="e8a23-145">Sécurité</span><span class="sxs-lookup"><span data-stu-id="e8a23-145">Security</span></span>

<span data-ttu-id="e8a23-146">Adoptez les meilleures pratiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8a23-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="e8a23-147">Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="e8a23-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="e8a23-148">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="e8a23-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="e8a23-149">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="e8a23-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="e8a23-150">En savoir plus sur [l’utilisation de plusieurs environnements](xref:fundamentals/environments) et la gestion du [stockage sécurisé des secrets d’application dans le développement avec Secret Manager](xref:security/app-secrets) (contient des conseils sur l’utilisation des variables d’environnement pour stocker les données sensibles).</span><span class="sxs-lookup"><span data-stu-id="e8a23-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="e8a23-151">Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local.</span><span class="sxs-lookup"><span data-stu-id="e8a23-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="e8a23-152">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e8a23-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="e8a23-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) constitue une option pour le stockage sécurisé des secrets d’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="e8a23-154">Pour plus d'informations, consultez <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="e8a23-155">Données de configuration hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="e8a23-155">Hierarchical configuration data</span></span>

<span data-ttu-id="e8a23-156">L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="e8a23-157">Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :</span><span class="sxs-lookup"><span data-stu-id="e8a23-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="e8a23-158">Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="e8a23-159">Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :</span><span class="sxs-lookup"><span data-stu-id="e8a23-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="e8a23-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-160">section0:key0</span></span>
* <span data-ttu-id="e8a23-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-161">section0:key1</span></span>
* <span data-ttu-id="e8a23-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-162">section1:key0</span></span>
* <span data-ttu-id="e8a23-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-163">section1:key1</span></span>

<span data-ttu-id="e8a23-164">Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="e8a23-165">Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="e8a23-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="e8a23-166">Conventions</span><span class="sxs-lookup"><span data-stu-id="e8a23-166">Conventions</span></span>

<span data-ttu-id="e8a23-167">Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="e8a23-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="e8a23-168">Les fournisseurs de configuration de fichier peuvent recharger la configuration lorsqu’un fichier de paramètres sous-jacent est modifié après le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="e8a23-169">Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e8a23-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="e8a23-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur [Dependency Injection (DI)](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e8a23-171">Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="e8a23-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="e8a23-172">Les clés de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8a23-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="e8a23-173">Les clés ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e8a23-173">Keys are case-insensitive.</span></span> <span data-ttu-id="e8a23-174">Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.</span><span class="sxs-lookup"><span data-stu-id="e8a23-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="e8a23-175">Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.</span><span class="sxs-lookup"><span data-stu-id="e8a23-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="e8a23-176">Clés hiérarchiques</span><span class="sxs-lookup"><span data-stu-id="e8a23-176">Hierarchical keys</span></span>
  * <span data-ttu-id="e8a23-177">Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="e8a23-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="e8a23-178">Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="e8a23-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="e8a23-179">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et converti en signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="e8a23-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="e8a23-180">Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="e8a23-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="e8a23-181">Vous devez fournir du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="e8a23-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e8a23-183">La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="e8a23-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="e8a23-184">Les valeurs de configuration adoptent les conventions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8a23-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="e8a23-185">Les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="e8a23-185">Values are strings.</span></span>
* <span data-ttu-id="e8a23-186">Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.</span><span class="sxs-lookup"><span data-stu-id="e8a23-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="e8a23-187">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="e8a23-187">Providers</span></span>

<span data-ttu-id="e8a23-188">Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8a23-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="e8a23-189">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="e8a23-189">Provider</span></span> | <span data-ttu-id="e8a23-190">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="e8a23-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="e8a23-191">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e8a23-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="e8a23-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e8a23-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="e8a23-193">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e8a23-194">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-194">Command-line parameters</span></span> |
| [<span data-ttu-id="e8a23-195">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="e8a23-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e8a23-196">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="e8a23-196">Custom source</span></span> |
| [<span data-ttu-id="e8a23-197">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e8a23-198">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-198">Environment variables</span></span> |
| [<span data-ttu-id="e8a23-199">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="e8a23-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e8a23-200">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="e8a23-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e8a23-201">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="e8a23-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="e8a23-202">Fichiers de répertoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-202">Directory files</span></span> |
| [<span data-ttu-id="e8a23-203">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e8a23-204">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-204">In-memory collections</span></span> |
| <span data-ttu-id="e8a23-205">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e8a23-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e8a23-206">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="e8a23-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="e8a23-207">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="e8a23-207">Provider</span></span> | <span data-ttu-id="e8a23-208">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="e8a23-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="e8a23-209">[Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e8a23-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="e8a23-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e8a23-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="e8a23-211">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e8a23-212">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-212">Command-line parameters</span></span> |
| [<span data-ttu-id="e8a23-213">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="e8a23-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e8a23-214">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="e8a23-214">Custom source</span></span> |
| [<span data-ttu-id="e8a23-215">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e8a23-216">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-216">Environment variables</span></span> |
| [<span data-ttu-id="e8a23-217">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="e8a23-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e8a23-218">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="e8a23-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e8a23-219">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e8a23-220">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-220">In-memory collections</span></span> |
| <span data-ttu-id="e8a23-221">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e8a23-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e8a23-222">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="e8a23-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="e8a23-223">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="e8a23-223">Provider</span></span> | <span data-ttu-id="e8a23-224">Fournit la configuration à partir de&hellip;</span><span class="sxs-lookup"><span data-stu-id="e8a23-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="e8a23-225">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e8a23-226">Paramètres de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-226">Command-line parameters</span></span> |
| [<span data-ttu-id="e8a23-227">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="e8a23-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e8a23-228">Source personnalisée</span><span class="sxs-lookup"><span data-stu-id="e8a23-228">Custom source</span></span> |
| [<span data-ttu-id="e8a23-229">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e8a23-230">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-230">Environment variables</span></span> |
| [<span data-ttu-id="e8a23-231">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="e8a23-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e8a23-232">Fichiers (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="e8a23-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e8a23-233">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e8a23-234">Collections en mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-234">In-memory collections</span></span> |
| <span data-ttu-id="e8a23-235">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*)</span><span class="sxs-lookup"><span data-stu-id="e8a23-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e8a23-236">Fichier dans le répertoire de profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="e8a23-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="e8a23-237">Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="e8a23-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="e8a23-238">Les fournisseurs de configuration décrits dans cette rubrique sont décrits dans l’ordre alphabétique, et non dans l’ordre où votre code peut les organiser.</span><span class="sxs-lookup"><span data-stu-id="e8a23-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="e8a23-239">Organisez les fournisseurs de configuration dans votre code en fonction de vos priorités pour les sources de configuration sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="e8a23-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="e8a23-240">Une séquence type des fournisseurs de configuration est la suivante :</span><span class="sxs-lookup"><span data-stu-id="e8a23-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="e8a23-241">Fichiers (*appsettings.json*, *appsettings.&lt; Environnement&gt;.json*, où `<Environment>` est l’environnement d’hébergement actuel de l’application)</span><span class="sxs-lookup"><span data-stu-id="e8a23-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="e8a23-242">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)</span><span class="sxs-lookup"><span data-stu-id="e8a23-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="e8a23-243">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-243">Environment variables</span></span>
1. <span data-ttu-id="e8a23-244">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-244">Command-line arguments</span></span>

<span data-ttu-id="e8a23-245">Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="e8a23-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-246">Cette séquence de fournisseurs est mise en place lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="e8a23-247">Pour plus d’informations, consultez [Hôte web : configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="e8a23-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="e8a23-248">Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte web pour spécifier les fournisseurs de configuration de l’application :</span><span class="sxs-lookup"><span data-stu-id="e8a23-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="e8a23-249">`ConfigureAppConfiguration` *est disponible dans ASP.NET Core 2.1 et versions ultérieures.*</span><span class="sxs-lookup"><span data-stu-id="e8a23-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-250">Cette séquence de fournisseurs peut être créée pour l’application (pas l’hôte) avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> et un appel à sa méthode <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> dans `Startup` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="e8a23-251">Dans l’exemple précédent, le nom de l’environnement (`env.EnvironmentName`) et le nom de l’assembly d’application (`env.ApplicationName`) sont fournis par <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="e8a23-252">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="e8a23-253">Fournisseur de configuration de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="e8a23-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e8a23-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-255">Pour activer la configuration en ligne de commande, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e8a23-256">`AddCommandLine` est appelé automatiquement lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="e8a23-257">Pour plus d’informations, consultez [Hôte web : configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="e8a23-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="e8a23-258">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e8a23-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e8a23-259">Configuration facultative à partir *appsettings.json* et *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="e8a23-260">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="e8a23-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="e8a23-261">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="e8a23-261">Environment variables.</span></span>

<span data-ttu-id="e8a23-262">`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e8a23-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="e8a23-263">Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="e8a23-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="e8a23-264">`CreateDefaultBuilder` agit lors de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="e8a23-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="e8a23-265">Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.</span><span class="sxs-lookup"><span data-stu-id="e8a23-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="e8a23-266">Lors de la génération de l’hôte manuellement sans appeler `CreateDefaultBuilder`, appelez la méthode d’extension `AddCommandLine` sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> :</span><span class="sxs-lookup"><span data-stu-id="e8a23-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-267">Pour activer la configuration en ligne de commande, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e8a23-268">Appelez le fournisseur en dernier pour permettre aux arguments de ligne de commande passés au moment de l’exécution de remplacer la configuration définie par les autres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="e8a23-269">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="e8a23-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="e8a23-270">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e8a23-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-271">L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-272">L’exemple d’application 1.x appelle <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> sur un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="e8a23-273">Ouvrez une invite de commandes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="e8a23-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="e8a23-274">Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="e8a23-275">Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e8a23-276">Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="e8a23-277">Arguments</span><span class="sxs-lookup"><span data-stu-id="e8a23-277">Arguments</span></span>

<span data-ttu-id="e8a23-278">La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace.</span><span class="sxs-lookup"><span data-stu-id="e8a23-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="e8a23-279">La valeur peut être null si un signe égal est utilisé (par exemple, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="e8a23-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="e8a23-280">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="e8a23-280">Key prefix</span></span>               | <span data-ttu-id="e8a23-281">Exemple</span><span class="sxs-lookup"><span data-stu-id="e8a23-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="e8a23-282">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="e8a23-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="e8a23-283">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="e8a23-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="e8a23-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="e8a23-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="e8a23-285">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="e8a23-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="e8a23-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="e8a23-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="e8a23-287">Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.</span><span class="sxs-lookup"><span data-stu-id="e8a23-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="e8a23-288">Exemples de commandes :</span><span class="sxs-lookup"><span data-stu-id="e8a23-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="e8a23-289">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="e8a23-289">Switch mappings</span></span>

<span data-ttu-id="e8a23-290">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="e8a23-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="e8a23-291">Lorsque vous générez manuellement la configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, vous pouvez fournir un dictionnaire des remplacements de commutateur à la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="e8a23-292">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e8a23-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e8a23-293">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="e8a23-294">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="e8a23-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e8a23-295">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="e8a23-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e8a23-296">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="e8a23-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e8a23-297">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="e8a23-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="e8a23-298">Comme indiqué dans l’exemple précédent, l’appel à `CreateDefaultBuilder` ne doit pas passer des arguments lorsque des correspondances de commutateur sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="e8a23-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="e8a23-299">L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e8a23-300">Si les arguments incluent un commutateur mappé et sont passés à `CreateDefaultBuilder`, son fournisseur `AddCommandLine` ne parvient pas à s’initialiser avec un <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="e8a23-301">La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder` mais de permettre plutôt à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de correspondance de commutateur.</span><span class="sxs-lookup"><span data-stu-id="e8a23-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="e8a23-302">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e8a23-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="e8a23-303">Touche</span><span class="sxs-lookup"><span data-stu-id="e8a23-303">Key</span></span>       | <span data-ttu-id="e8a23-304">Value</span><span class="sxs-lookup"><span data-stu-id="e8a23-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="e8a23-305">Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :</span><span class="sxs-lookup"><span data-stu-id="e8a23-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="e8a23-306">Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e8a23-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="e8a23-307">Touche</span><span class="sxs-lookup"><span data-stu-id="e8a23-307">Key</span></span>               | <span data-ttu-id="e8a23-308">Value</span><span class="sxs-lookup"><span data-stu-id="e8a23-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="e8a23-309">Fournisseur de configuration de variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="e8a23-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e8a23-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="e8a23-311">Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e8a23-312">Lors de l’utilisation de clés hiérarchiques dans les variables d’environnement, un séparateur sous forme de signe deux-points (`:`) peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="e8a23-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="e8a23-313">Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et remplacé par un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="e8a23-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="e8a23-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) vous permet de définir des variables d’environnement dans le portail Azure capables de remplacer la configuration d’application à l’aide du Fournisseur de configuration de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e8a23-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="e8a23-315">Pour plus d’informations, consultez [Azure Apps : remplacer la configuration de l’application à l’aide du portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="e8a23-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-316">`AddEnvironmentVariables` est appelé automatiquement lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="e8a23-317">Pour plus d’informations, consultez [Hôte web : configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="e8a23-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="e8a23-318">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e8a23-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e8a23-319">Configuration facultative à partir *appsettings.json* et *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="e8a23-320">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="e8a23-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="e8a23-321">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-321">Command-line arguments.</span></span>

<span data-ttu-id="e8a23-322">Le Fournisseur de configuration de variable d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="e8a23-323">Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="e8a23-324">Vous pouvez aussi appeler directement la méthode d’extension `AddEnvironmentVariables` sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> :</span><span class="sxs-lookup"><span data-stu-id="e8a23-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-325">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode `UseConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="e8a23-326">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e8a23-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-327">L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-328">L’exemple d’application 1.x appelle `AddEnvironmentVariables` sur un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="e8a23-329">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-329">Run the sample app.</span></span> <span data-ttu-id="e8a23-330">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e8a23-331">Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="e8a23-332">La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.</span><span class="sxs-lookup"><span data-stu-id="e8a23-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="e8a23-333">Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement en fonction de celles qui commencent par :</span><span class="sxs-lookup"><span data-stu-id="e8a23-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="e8a23-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="e8a23-334">ASPNETCORE_</span></span>
* <span data-ttu-id="e8a23-335">urls</span><span class="sxs-lookup"><span data-stu-id="e8a23-335">urls</span></span>
* <span data-ttu-id="e8a23-336">Journalisation</span><span class="sxs-lookup"><span data-stu-id="e8a23-336">Logging</span></span>
* <span data-ttu-id="e8a23-337">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="e8a23-337">ENVIRONMENT</span></span>
* <span data-ttu-id="e8a23-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="e8a23-338">contentRoot</span></span>
* <span data-ttu-id="e8a23-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="e8a23-339">AllowedHosts</span></span>
* <span data-ttu-id="e8a23-340">applicationName</span><span class="sxs-lookup"><span data-stu-id="e8a23-340">applicationName</span></span>
* <span data-ttu-id="e8a23-341">CommandLine</span><span class="sxs-lookup"><span data-stu-id="e8a23-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-342">Si vous souhaitez exposer toutes les variables d’environnement disponibles pour l’application, remplacez `FilteredConfiguration` dans *Pages/Index.cshtml.cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="e8a23-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-343">Si vous souhaitez exposer toutes les variables d’environnement disponibles pour l’application, remplacez `FilteredConfiguration` dans *Controllers/HomeController.cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="e8a23-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="e8a23-344">Préfixes</span><span class="sxs-lookup"><span data-stu-id="e8a23-344">Prefixes</span></span>

<span data-ttu-id="e8a23-345">Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe pour la méthode `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="e8a23-346">Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="e8a23-347">Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="e8a23-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-348">La méthode pratique statique `CreateDefaultBuilder` crée un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> pour établir l’hôte de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="e8a23-349">Lorsque `WebHostBuilder` est créé, il trouve la configuration de son hôte dans les variables d’environnement avec le préfixe `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="e8a23-350">**Préfixes des chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="e8a23-350">**Connection string prefixes**</span></span>

<span data-ttu-id="e8a23-351">L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="e8a23-352">Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="e8a23-353">Préfixe de la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="e8a23-353">Connection string prefix</span></span> | <span data-ttu-id="e8a23-354">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="e8a23-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="e8a23-355">Fournisseur personnalisé</span><span class="sxs-lookup"><span data-stu-id="e8a23-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="e8a23-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="e8a23-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="e8a23-357">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e8a23-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="e8a23-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e8a23-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="e8a23-359">Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :</span><span class="sxs-lookup"><span data-stu-id="e8a23-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="e8a23-360">La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="e8a23-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="e8a23-361">Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).</span><span class="sxs-lookup"><span data-stu-id="e8a23-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="e8a23-362">Clé de variable d’environnement</span><span class="sxs-lookup"><span data-stu-id="e8a23-362">Environment variable key</span></span> | <span data-ttu-id="e8a23-363">Clé de configuration convertie</span><span class="sxs-lookup"><span data-stu-id="e8a23-363">Converted configuration key</span></span> | <span data-ttu-id="e8a23-364">Entrée de configuration de fournisseur</span><span class="sxs-lookup"><span data-stu-id="e8a23-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e8a23-365">Entrée de configuration non créée.</span><span class="sxs-lookup"><span data-stu-id="e8a23-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e8a23-366">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e8a23-367">Valeur : `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="e8a23-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e8a23-368">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e8a23-369">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e8a23-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e8a23-370">Clé : `ConnectionStrings:<KEY>_ProviderName` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e8a23-371">Valeur : `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e8a23-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="e8a23-372">Fournisseur de configuration de fichier</span><span class="sxs-lookup"><span data-stu-id="e8a23-372">File Configuration Provider</span></span>

<span data-ttu-id="e8a23-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="e8a23-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="e8a23-374">Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :</span><span class="sxs-lookup"><span data-stu-id="e8a23-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="e8a23-375">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="e8a23-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="e8a23-376">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="e8a23-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="e8a23-377">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="e8a23-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="e8a23-378">Fournisseur de configuration INI</span><span class="sxs-lookup"><span data-stu-id="e8a23-378">INI Configuration Provider</span></span>

<span data-ttu-id="e8a23-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e8a23-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="e8a23-380">Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e8a23-381">Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.</span><span class="sxs-lookup"><span data-stu-id="e8a23-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="e8a23-382">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e8a23-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="e8a23-383">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e8a23-383">Whether the file is optional.</span></span>
* <span data-ttu-id="e8a23-384">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e8a23-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e8a23-385">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e8a23-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-386">Lors de l’appel de `CreateDefaultBuilder`, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="e8a23-387">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-388">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode `UseConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="e8a23-389">Exemple générique d’un fichier de configuration INI :</span><span class="sxs-lookup"><span data-stu-id="e8a23-389">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="e8a23-390">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e8a23-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-391">section0:key0</span></span>
* <span data-ttu-id="e8a23-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-392">section0:key1</span></span>
* <span data-ttu-id="e8a23-393">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="e8a23-393">section1:subsection:key</span></span>
* <span data-ttu-id="e8a23-394">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="e8a23-394">section2:subsection0:key</span></span>
* <span data-ttu-id="e8a23-395">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="e8a23-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="e8a23-396">Fournisseur de configuration JSON</span><span class="sxs-lookup"><span data-stu-id="e8a23-396">JSON Configuration Provider</span></span>

<span data-ttu-id="e8a23-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e8a23-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="e8a23-398">Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e8a23-399">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e8a23-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="e8a23-400">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e8a23-400">Whether the file is optional.</span></span>
* <span data-ttu-id="e8a23-401">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e8a23-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e8a23-402">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e8a23-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-403">`AddJsonFile` est appelé automatiquement deux fois lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="e8a23-404">La méthode est appelée pour charger la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="e8a23-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="e8a23-405">*appSettings.JSON* &ndash; Ce fichier est lu en premier.</span><span class="sxs-lookup"><span data-stu-id="e8a23-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="e8a23-406">La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="e8a23-407">*appsettings.&lt;Environment&gt;.json* &ndash; La version de l’environnement du fichier est chargée en fonction de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="e8a23-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="e8a23-408">Pour plus d’informations, consultez [Hôte web : configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="e8a23-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="e8a23-409">`CreateDefaultBuilder` charge également :</span><span class="sxs-lookup"><span data-stu-id="e8a23-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e8a23-410">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="e8a23-410">Environment variables.</span></span>
* <span data-ttu-id="e8a23-411">[Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="e8a23-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="e8a23-412">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8a23-412">Command-line arguments.</span></span>

<span data-ttu-id="e8a23-413">Le Fournisseur de configuration JSON est établi en premier.</span><span class="sxs-lookup"><span data-stu-id="e8a23-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="e8a23-414">Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="e8a23-415">Vous pouvez aussi appeler directement la méthode d’extension `AddJsonFile` sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e8a23-416">Lors de l’appel de `CreateDefaultBuilder`, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="e8a23-417">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-418">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode `UseConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="e8a23-419">**Exemple**</span><span class="sxs-lookup"><span data-stu-id="e8a23-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-420">L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut deux appels à `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="e8a23-421">La configuration est chargée à partir *appsettings.json* et *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-422">L’exemple d’application 1.x appelle `AddJsonFile` deux fois sur un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="e8a23-423">La configuration est chargée à partir *appsettings.json* et *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="e8a23-424">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-424">Run the sample app.</span></span> <span data-ttu-id="e8a23-425">Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e8a23-426">Notez que la sortie contient des paires clé-valeur pour la configuration représentée dans le tableau en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="e8a23-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="e8a23-427">Les clés de configuration de la journalisation utilisent le signe deux-points (`:`) comme séparateur hiérarchique.</span><span class="sxs-lookup"><span data-stu-id="e8a23-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="e8a23-428">Touche</span><span class="sxs-lookup"><span data-stu-id="e8a23-428">Key</span></span>                        | <span data-ttu-id="e8a23-429">Valeur de développement</span><span class="sxs-lookup"><span data-stu-id="e8a23-429">Development Value</span></span> | <span data-ttu-id="e8a23-430">Valeur de production</span><span class="sxs-lookup"><span data-stu-id="e8a23-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="e8a23-431">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="e8a23-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="e8a23-432">Information</span><span class="sxs-lookup"><span data-stu-id="e8a23-432">Information</span></span>       | <span data-ttu-id="e8a23-433">Information</span><span class="sxs-lookup"><span data-stu-id="e8a23-433">Information</span></span>      |
| <span data-ttu-id="e8a23-434">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="e8a23-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="e8a23-435">Information</span><span class="sxs-lookup"><span data-stu-id="e8a23-435">Information</span></span>       | <span data-ttu-id="e8a23-436">Information</span><span class="sxs-lookup"><span data-stu-id="e8a23-436">Information</span></span>      |
| <span data-ttu-id="e8a23-437">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="e8a23-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="e8a23-438">Débogage</span><span class="sxs-lookup"><span data-stu-id="e8a23-438">Debug</span></span>             | <span data-ttu-id="e8a23-439">Error</span><span class="sxs-lookup"><span data-stu-id="e8a23-439">Error</span></span>            |
| <span data-ttu-id="e8a23-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="e8a23-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="e8a23-441">Fournisseur de configuration XML</span><span class="sxs-lookup"><span data-stu-id="e8a23-441">XML Configuration Provider</span></span>

<span data-ttu-id="e8a23-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e8a23-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="e8a23-443">Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e8a23-444">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e8a23-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="e8a23-445">Si le fichier est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e8a23-445">Whether the file is optional.</span></span>
* <span data-ttu-id="e8a23-446">Si la configuration est rechargée quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="e8a23-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e8a23-447">Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.</span><span class="sxs-lookup"><span data-stu-id="e8a23-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e8a23-448">Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées.</span><span class="sxs-lookup"><span data-stu-id="e8a23-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="e8a23-449">Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="e8a23-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-450">Lors de l’appel de `CreateDefaultBuilder`, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="e8a23-451">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-452">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode `UseConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="e8a23-453">Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :</span><span class="sxs-lookup"><span data-stu-id="e8a23-453">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="e8a23-454">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e8a23-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-455">section0:key0</span></span>
* <span data-ttu-id="e8a23-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-456">section0:key1</span></span>
* <span data-ttu-id="e8a23-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-457">section1:key0</span></span>
* <span data-ttu-id="e8a23-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-458">section1:key1</span></span>

<span data-ttu-id="e8a23-459">Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :</span><span class="sxs-lookup"><span data-stu-id="e8a23-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="e8a23-460">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e8a23-461">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-461">section:section0:key:key0</span></span>
* <span data-ttu-id="e8a23-462">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-462">section:section0:key:key1</span></span>
* <span data-ttu-id="e8a23-463">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-463">section:section1:key:key0</span></span>
* <span data-ttu-id="e8a23-464">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-464">section:section1:key:key1</span></span>

<span data-ttu-id="e8a23-465">Les attributs peuvent être utilisés pour fournir des valeurs :</span><span class="sxs-lookup"><span data-stu-id="e8a23-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="e8a23-466">Le fichier de configuration précédent charge les clés suivantes avec `value` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e8a23-467">key:attribute</span><span class="sxs-lookup"><span data-stu-id="e8a23-467">key:attribute</span></span>
* <span data-ttu-id="e8a23-468">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="e8a23-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="e8a23-469">Fournisseur de configuration clé par fichier</span><span class="sxs-lookup"><span data-stu-id="e8a23-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="e8a23-470">Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="e8a23-471">La clé est le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="e8a23-471">The key is the file name.</span></span> <span data-ttu-id="e8a23-472">La valeur contient le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="e8a23-472">The value contains the file's contents.</span></span> <span data-ttu-id="e8a23-473">Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.</span><span class="sxs-lookup"><span data-stu-id="e8a23-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="e8a23-474">Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="e8a23-475">Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="e8a23-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="e8a23-476">Les surcharges permettent de spécifier :</span><span class="sxs-lookup"><span data-stu-id="e8a23-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="e8a23-477">Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.</span><span class="sxs-lookup"><span data-stu-id="e8a23-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="e8a23-478">Si le répertoire est facultatif et le chemin d’accès au répertoire.</span><span class="sxs-lookup"><span data-stu-id="e8a23-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="e8a23-479">Lors de l’appel de `CreateDefaultBuilder`, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="e8a23-480">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="e8a23-481">Fournisseur de configuration de mémoire</span><span class="sxs-lookup"><span data-stu-id="e8a23-481">Memory Configuration Provider</span></span>

<span data-ttu-id="e8a23-482">Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="e8a23-483">Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e8a23-484">Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-485">Lors de l’appel de `CreateDefaultBuilder`, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="e8a23-486">Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez `UseConfiguration` avec la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-487">Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode `UseConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="e8a23-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="e8a23-488">GetValue</span></span>

<span data-ttu-id="e8a23-489">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur à partir de la configuration avec une clé spécifiée et la convertit selon le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="e8a23-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="e8a23-490">Une surcharge vous permet de fournir une valeur par défaut si la clé est introuvable.</span><span class="sxs-lookup"><span data-stu-id="e8a23-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="e8a23-491">L’exemple suivant extrait la valeur de la chaîne à partir de la configuration avec la clé `NumberKey`, tape la valeur en tant que `int` et stocke la valeur dans la variable `intValue`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="e8a23-492">Si `NumberKey` est introuvable dans les clés de configuration, `intValue` reçoit la valeur par défaut `99` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="e8a23-493">GetSection, GetChildren et Exists</span><span class="sxs-lookup"><span data-stu-id="e8a23-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="e8a23-494">Pour les exemples qui suivent, utilisez le fichier JSON suivant.</span><span class="sxs-lookup"><span data-stu-id="e8a23-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="e8a23-495">Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :</span><span class="sxs-lookup"><span data-stu-id="e8a23-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="e8a23-496">Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="e8a23-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-497">section0:key0</span></span>
* <span data-ttu-id="e8a23-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-498">section0:key1</span></span>
* <span data-ttu-id="e8a23-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-499">section1:key0</span></span>
* <span data-ttu-id="e8a23-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-500">section1:key1</span></span>
* <span data-ttu-id="e8a23-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="e8a23-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="e8a23-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="e8a23-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="e8a23-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="e8a23-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="e8a23-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="e8a23-505">GetSection</span></span>

<span data-ttu-id="e8a23-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée.</span><span class="sxs-lookup"><span data-stu-id="e8a23-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="e8a23-507">Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :</span><span class="sxs-lookup"><span data-stu-id="e8a23-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="e8a23-508">De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :</span><span class="sxs-lookup"><span data-stu-id="e8a23-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="e8a23-509">`GetSection` ne retourne jamais `null`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="e8a23-510">Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="e8a23-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="e8a23-511">GetChildren</span><span class="sxs-lookup"><span data-stu-id="e8a23-511">GetChildren</span></span>

<span data-ttu-id="e8a23-512">Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :</span><span class="sxs-lookup"><span data-stu-id="e8a23-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="e8a23-513">Existe</span><span class="sxs-lookup"><span data-stu-id="e8a23-513">Exists</span></span>

<span data-ttu-id="e8a23-514">Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :</span><span class="sxs-lookup"><span data-stu-id="e8a23-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="e8a23-515">Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="e8a23-516">Lier à une classe</span><span class="sxs-lookup"><span data-stu-id="e8a23-516">Bind to a class</span></span>

<span data-ttu-id="e8a23-517">La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="e8a23-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="e8a23-518">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e8a23-519">Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="e8a23-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="e8a23-520">L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :</span><span class="sxs-lookup"><span data-stu-id="e8a23-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-521">La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="e8a23-522">Les paires clé-valeur de configuration suivantes sont créées :</span><span class="sxs-lookup"><span data-stu-id="e8a23-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="e8a23-523">Touche</span><span class="sxs-lookup"><span data-stu-id="e8a23-523">Key</span></span>                   | <span data-ttu-id="e8a23-524">Value</span><span class="sxs-lookup"><span data-stu-id="e8a23-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="e8a23-525">starship:name</span><span class="sxs-lookup"><span data-stu-id="e8a23-525">starship:name</span></span>         | <span data-ttu-id="e8a23-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="e8a23-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="e8a23-527">starship:registry</span><span class="sxs-lookup"><span data-stu-id="e8a23-527">starship:registry</span></span>     | <span data-ttu-id="e8a23-528">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="e8a23-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="e8a23-529">starship:class</span><span class="sxs-lookup"><span data-stu-id="e8a23-529">starship:class</span></span>        | <span data-ttu-id="e8a23-530">Constitution</span><span class="sxs-lookup"><span data-stu-id="e8a23-530">Constitution</span></span>                                      |
| <span data-ttu-id="e8a23-531">starship:length</span><span class="sxs-lookup"><span data-stu-id="e8a23-531">starship:length</span></span>       | <span data-ttu-id="e8a23-532">304.8</span><span class="sxs-lookup"><span data-stu-id="e8a23-532">304.8</span></span>                                             |
| <span data-ttu-id="e8a23-533">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="e8a23-533">starship:commissioned</span></span> | <span data-ttu-id="e8a23-534">False</span><span class="sxs-lookup"><span data-stu-id="e8a23-534">False</span></span>                                             |
| <span data-ttu-id="e8a23-535">trademark</span><span class="sxs-lookup"><span data-stu-id="e8a23-535">trademark</span></span>             | <span data-ttu-id="e8a23-536">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="e8a23-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="e8a23-537">L’exemple d’application appelle `GetSection` avec la clé `starship`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="e8a23-538">Les paires clé-valeur `starship` sont isolées.</span><span class="sxs-lookup"><span data-stu-id="e8a23-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="e8a23-539">La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="e8a23-540">Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e8a23-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="e8a23-541">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="e8a23-541">Bind to an object graph</span></span>

<span data-ttu-id="e8a23-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO.</span><span class="sxs-lookup"><span data-stu-id="e8a23-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="e8a23-543">L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :</span><span class="sxs-lookup"><span data-stu-id="e8a23-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-544">L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="e8a23-545">La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="e8a23-546">L’instance liée est affectée à une propriété pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e8a23-546">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="e8a23-547">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="e8a23-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="e8a23-548">Il est plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="e8a23-549">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :</span><span class="sxs-lookup"><span data-stu-id="e8a23-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="e8a23-550">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="e8a23-550">Bind an array to a class</span></span>

<span data-ttu-id="e8a23-551">*L’exemple d’application illustre les concepts abordés dans cette section.*</span><span class="sxs-lookup"><span data-stu-id="e8a23-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="e8a23-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e8a23-553">Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer la liaison de tableau avec un tableau de classes POCO.</span><span class="sxs-lookup"><span data-stu-id="e8a23-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="e8a23-554">La liaison est fournie par convention.</span><span class="sxs-lookup"><span data-stu-id="e8a23-554">Binding is provided by convention.</span></span> <span data-ttu-id="e8a23-555">Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.</span><span class="sxs-lookup"><span data-stu-id="e8a23-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="e8a23-556">**Traitement de tableau en mémoire**</span><span class="sxs-lookup"><span data-stu-id="e8a23-556">**In-memory array processing**</span></span>

<span data-ttu-id="e8a23-557">Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="e8a23-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="e8a23-558">Touche</span><span class="sxs-lookup"><span data-stu-id="e8a23-558">Key</span></span>     | <span data-ttu-id="e8a23-559">Value</span><span class="sxs-lookup"><span data-stu-id="e8a23-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="e8a23-560">array:0</span><span class="sxs-lookup"><span data-stu-id="e8a23-560">array:0</span></span> | <span data-ttu-id="e8a23-561">value0</span><span class="sxs-lookup"><span data-stu-id="e8a23-561">value0</span></span> |
| <span data-ttu-id="e8a23-562">array:1</span><span class="sxs-lookup"><span data-stu-id="e8a23-562">array:1</span></span> | <span data-ttu-id="e8a23-563">value1</span><span class="sxs-lookup"><span data-stu-id="e8a23-563">value1</span></span> |
| <span data-ttu-id="e8a23-564">array:2</span><span class="sxs-lookup"><span data-stu-id="e8a23-564">array:2</span></span> | <span data-ttu-id="e8a23-565">value2</span><span class="sxs-lookup"><span data-stu-id="e8a23-565">value2</span></span> |
| <span data-ttu-id="e8a23-566">array:4</span><span class="sxs-lookup"><span data-stu-id="e8a23-566">array:4</span></span> | <span data-ttu-id="e8a23-567">value4</span><span class="sxs-lookup"><span data-stu-id="e8a23-567">value4</span></span> |
| <span data-ttu-id="e8a23-568">array:5</span><span class="sxs-lookup"><span data-stu-id="e8a23-568">array:5</span></span> | <span data-ttu-id="e8a23-569">value5</span><span class="sxs-lookup"><span data-stu-id="e8a23-569">value5</span></span> |

<span data-ttu-id="e8a23-570">Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :</span><span class="sxs-lookup"><span data-stu-id="e8a23-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="e8a23-571">Le tableau ignore une valeur pour l’index &num;3.</span><span class="sxs-lookup"><span data-stu-id="e8a23-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="e8a23-572">Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.</span><span class="sxs-lookup"><span data-stu-id="e8a23-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="e8a23-573">Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :</span><span class="sxs-lookup"><span data-stu-id="e8a23-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-574">Les données de configuration sont liées à l’objet :</span><span class="sxs-lookup"><span data-stu-id="e8a23-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="e8a23-575">La syntaxe [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :</span><span class="sxs-lookup"><span data-stu-id="e8a23-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="e8a23-576">L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="e8a23-577">Index `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="e8a23-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="e8a23-578">Valeur `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="e8a23-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="e8a23-579">0</span><span class="sxs-lookup"><span data-stu-id="e8a23-579">0</span></span>                             | <span data-ttu-id="e8a23-580">value0</span><span class="sxs-lookup"><span data-stu-id="e8a23-580">value0</span></span>                        |
| <span data-ttu-id="e8a23-581">1</span><span class="sxs-lookup"><span data-stu-id="e8a23-581">1</span></span>                             | <span data-ttu-id="e8a23-582">value1</span><span class="sxs-lookup"><span data-stu-id="e8a23-582">value1</span></span>                        |
| <span data-ttu-id="e8a23-583">2</span><span class="sxs-lookup"><span data-stu-id="e8a23-583">2</span></span>                             | <span data-ttu-id="e8a23-584">value2</span><span class="sxs-lookup"><span data-stu-id="e8a23-584">value2</span></span>                        |
| <span data-ttu-id="e8a23-585">3</span><span class="sxs-lookup"><span data-stu-id="e8a23-585">3</span></span>                             | <span data-ttu-id="e8a23-586">value4</span><span class="sxs-lookup"><span data-stu-id="e8a23-586">value4</span></span>                        |
| <span data-ttu-id="e8a23-587">4</span><span class="sxs-lookup"><span data-stu-id="e8a23-587">4</span></span>                             | <span data-ttu-id="e8a23-588">value5</span><span class="sxs-lookup"><span data-stu-id="e8a23-588">value5</span></span>                        |

<span data-ttu-id="e8a23-589">L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="e8a23-590">Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet.</span><span class="sxs-lookup"><span data-stu-id="e8a23-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="e8a23-591">Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.</span><span class="sxs-lookup"><span data-stu-id="e8a23-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="e8a23-592">L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExamples` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="e8a23-593">Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExamples.Entries` correspond à l’intégralité du tableau de configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="e8a23-594">*missing_value.json* :</span><span class="sxs-lookup"><span data-stu-id="e8a23-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8a23-595">Dans `ConfigureAppConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8a23-596">Dans le constructeur `Startup` :</span><span class="sxs-lookup"><span data-stu-id="e8a23-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="e8a23-597">La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="e8a23-598">Touche</span><span class="sxs-lookup"><span data-stu-id="e8a23-598">Key</span></span>             | <span data-ttu-id="e8a23-599">Value</span><span class="sxs-lookup"><span data-stu-id="e8a23-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e8a23-600">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="e8a23-600">array:entries:3</span></span> | <span data-ttu-id="e8a23-601">valeur3</span><span class="sxs-lookup"><span data-stu-id="e8a23-601">value3</span></span> |

<span data-ttu-id="e8a23-602">Si l’instance de classe `ArrayExamples` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExamples.Entries` inclut la valeur.</span><span class="sxs-lookup"><span data-stu-id="e8a23-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="e8a23-603">Index `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="e8a23-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="e8a23-604">Valeur `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="e8a23-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="e8a23-605">0</span><span class="sxs-lookup"><span data-stu-id="e8a23-605">0</span></span>                             | <span data-ttu-id="e8a23-606">value0</span><span class="sxs-lookup"><span data-stu-id="e8a23-606">value0</span></span>                        |
| <span data-ttu-id="e8a23-607">1</span><span class="sxs-lookup"><span data-stu-id="e8a23-607">1</span></span>                             | <span data-ttu-id="e8a23-608">value1</span><span class="sxs-lookup"><span data-stu-id="e8a23-608">value1</span></span>                        |
| <span data-ttu-id="e8a23-609">2</span><span class="sxs-lookup"><span data-stu-id="e8a23-609">2</span></span>                             | <span data-ttu-id="e8a23-610">value2</span><span class="sxs-lookup"><span data-stu-id="e8a23-610">value2</span></span>                        |
| <span data-ttu-id="e8a23-611">3</span><span class="sxs-lookup"><span data-stu-id="e8a23-611">3</span></span>                             | <span data-ttu-id="e8a23-612">valeur3</span><span class="sxs-lookup"><span data-stu-id="e8a23-612">value3</span></span>                        |
| <span data-ttu-id="e8a23-613">4</span><span class="sxs-lookup"><span data-stu-id="e8a23-613">4</span></span>                             | <span data-ttu-id="e8a23-614">value4</span><span class="sxs-lookup"><span data-stu-id="e8a23-614">value4</span></span>                        |
| <span data-ttu-id="e8a23-615">5</span><span class="sxs-lookup"><span data-stu-id="e8a23-615">5</span></span>                             | <span data-ttu-id="e8a23-616">value5</span><span class="sxs-lookup"><span data-stu-id="e8a23-616">value5</span></span>                        |

<span data-ttu-id="e8a23-617">**Traitement de tableau JSON**</span><span class="sxs-lookup"><span data-stu-id="e8a23-617">**JSON array processing**</span></span>

<span data-ttu-id="e8a23-618">Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro.</span><span class="sxs-lookup"><span data-stu-id="e8a23-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="e8a23-619">Dans le fichier de configuration suivant, `subsection` est un tableau :</span><span class="sxs-lookup"><span data-stu-id="e8a23-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="e8a23-620">Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8a23-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="e8a23-621">Touche</span><span class="sxs-lookup"><span data-stu-id="e8a23-621">Key</span></span>                     | <span data-ttu-id="e8a23-622">Value</span><span class="sxs-lookup"><span data-stu-id="e8a23-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="e8a23-623">json_array:key</span><span class="sxs-lookup"><span data-stu-id="e8a23-623">json_array:key</span></span>          | <span data-ttu-id="e8a23-624">valueA</span><span class="sxs-lookup"><span data-stu-id="e8a23-624">valueA</span></span> |
| <span data-ttu-id="e8a23-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="e8a23-625">json_array:subsection:0</span></span> | <span data-ttu-id="e8a23-626">valueB</span><span class="sxs-lookup"><span data-stu-id="e8a23-626">valueB</span></span> |
| <span data-ttu-id="e8a23-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="e8a23-627">json_array:subsection:1</span></span> | <span data-ttu-id="e8a23-628">valueC</span><span class="sxs-lookup"><span data-stu-id="e8a23-628">valueC</span></span> |
| <span data-ttu-id="e8a23-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="e8a23-629">json_array:subsection:2</span></span> | <span data-ttu-id="e8a23-630">valueD</span><span class="sxs-lookup"><span data-stu-id="e8a23-630">valueD</span></span> |

<span data-ttu-id="e8a23-631">Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="e8a23-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-632">Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="e8a23-633">Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="e8a23-634">Index `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e8a23-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="e8a23-635">Valeur `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="e8a23-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="e8a23-636">0</span><span class="sxs-lookup"><span data-stu-id="e8a23-636">0</span></span>                                   | <span data-ttu-id="e8a23-637">valueB</span><span class="sxs-lookup"><span data-stu-id="e8a23-637">valueB</span></span>                              |
| <span data-ttu-id="e8a23-638">1</span><span class="sxs-lookup"><span data-stu-id="e8a23-638">1</span></span>                                   | <span data-ttu-id="e8a23-639">valueC</span><span class="sxs-lookup"><span data-stu-id="e8a23-639">valueC</span></span>                              |
| <span data-ttu-id="e8a23-640">2</span><span class="sxs-lookup"><span data-stu-id="e8a23-640">2</span></span>                                   | <span data-ttu-id="e8a23-641">valueD</span><span class="sxs-lookup"><span data-stu-id="e8a23-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="e8a23-642">Fournisseur de configuration personnalisé</span><span class="sxs-lookup"><span data-stu-id="e8a23-642">Custom configuration provider</span></span>

<span data-ttu-id="e8a23-643">L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="e8a23-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="e8a23-644">Le fournisseur présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8a23-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="e8a23-645">La base de données en mémoire EF est utilisée à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="e8a23-646">Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="e8a23-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="e8a23-647">Le fournisseur lit une table de base de données dans la configuration au démarrage.</span><span class="sxs-lookup"><span data-stu-id="e8a23-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="e8a23-648">Le fournisseur n’interroge pas la base de données par clé.</span><span class="sxs-lookup"><span data-stu-id="e8a23-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="e8a23-649">Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="e8a23-650">Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e8a23-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="e8a23-651">*Models/EFConfigurationValue.cs* :</span><span class="sxs-lookup"><span data-stu-id="e8a23-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-652">Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.</span><span class="sxs-lookup"><span data-stu-id="e8a23-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="e8a23-653">*EFConfigurationProvider/EFConfigurationContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="e8a23-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-654">Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="e8a23-655">*EFConfigurationProvider/EFConfigurationSource.cs* :</span><span class="sxs-lookup"><span data-stu-id="e8a23-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-656">Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="e8a23-657">Le fournisseur de configuration initialise la base de données quand elle est vide.</span><span class="sxs-lookup"><span data-stu-id="e8a23-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="e8a23-658">*EFConfigurationProvider/EFConfigurationProvider.cs* :</span><span class="sxs-lookup"><span data-stu-id="e8a23-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-659">Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="e8a23-660">*EFConfigurationProvider/EFConfigurationExtensions.cs* :</span><span class="sxs-lookup"><span data-stu-id="e8a23-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e8a23-661">Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="e8a23-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="e8a23-662">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="e8a23-662">Access configuration during startup</span></span>

<span data-ttu-id="e8a23-663">Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e8a23-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e8a23-664">Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :</span><span class="sxs-lookup"><span data-stu-id="e8a23-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="e8a23-665">Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="e8a23-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="e8a23-666">Accéder à la configuration dans une page Razor Pages ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="e8a23-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="e8a23-667">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="e8a23-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="e8a23-668">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="e8a23-668">In a Razor Pages page:</span></span>

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

<span data-ttu-id="e8a23-669">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="e8a23-669">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="e8a23-670">Ajouter la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="e8a23-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="e8a23-671">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8a23-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="e8a23-672">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e8a23-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8a23-673">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e8a23-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="e8a23-674">Présentation approfondie de la Configuration Microsoft</span><span class="sxs-lookup"><span data-stu-id="e8a23-674">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
