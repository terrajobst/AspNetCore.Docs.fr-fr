---
title: Configuration dans ASP.NET Core
author: rick-anderson
description: "Utilisez l’API de configuration pour configurer une application ASP.NET Core selon plusieurs méthodes."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 1/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 0f8618898089418f709506aee5eb013f983dc294
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="0a7ec-103">Configurer une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a7ec-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="0a7ec-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0a7ec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0a7ec-105">L’API de configuration fournit un moyen de configurer une application web ASP.NET Core basé sur une liste de paires nom/valeur.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="0a7ec-106">La configuration est lue au moment de l’exécution à partir de plusieurs sources.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="0a7ec-107">Vous pouvez regrouper ces paires nom/valeur dans une hiérarchie à plusieurs niveaux.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-107">You can group these name-value pairs into a multi-level hierarchy.</span></span>

<span data-ttu-id="0a7ec-108">Il existe des fournisseurs de configuration pour les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-108">There are configuration providers for:</span></span>

* <span data-ttu-id="0a7ec-109">Formats de fichiers (INI, JSON et XML)</span><span class="sxs-lookup"><span data-stu-id="0a7ec-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="0a7ec-110">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0a7ec-110">Command-line arguments</span></span>
* <span data-ttu-id="0a7ec-111">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="0a7ec-111">Environment variables</span></span>
* <span data-ttu-id="0a7ec-112">Objets .NET en mémoire</span><span class="sxs-lookup"><span data-stu-id="0a7ec-112">In-memory .NET objects</span></span>
* <span data-ttu-id="0a7ec-113">Magasin d’utilisateurs chiffrés</span><span class="sxs-lookup"><span data-stu-id="0a7ec-113">An encrypted user store</span></span>
* [<span data-ttu-id="0a7ec-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0a7ec-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="0a7ec-115">Fournisseurs personnalisés (installés ou créés)</span><span class="sxs-lookup"><span data-stu-id="0a7ec-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="0a7ec-116">Chaque valeur de configuration correspond à une clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="0a7ec-117">Une liaison intégrée est prise en charge pour désérialiser les paramètres dans un objet [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personnalisé (une classe .NET simple avec des propriétés).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="0a7ec-118">Le modèle d’options utilise des classes d’options pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="0a7ec-119">Pour plus d’informations sur l’utilisation du modèle d’options, consultez la rubrique [Options](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="0a7ec-120">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a7ec-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="0a7ec-121">Configuration JSON</span><span class="sxs-lookup"><span data-stu-id="0a7ec-121">JSON configuration</span></span>

<span data-ttu-id="0a7ec-122">L’application console suivante utilise le fournisseur de configuration JSON :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="0a7ec-123">L’application lit et affiche les paramètres de configuration suivants :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="0a7ec-124">La configuration se compose d’une liste hiérarchique des paires nom/valeur dans laquelle les nœuds sont séparés par un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="0a7ec-125">Pour récupérer une valeur, accédez à l’indexeur `Configuration` avec la clé de l’élément correspondant :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=24-24)]

<span data-ttu-id="0a7ec-126">Pour utiliser des tableaux dans des sources de configuration au format JSON, utilisez un index de tableau comme partie d’une chaîne séparée par des signes deux-points.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="0a7ec-127">L’exemple suivant obtient le nom du premier élément dans le tableau `wizards` précédent :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="0a7ec-128">Les paires nom/valeur écrites dans les fournisseurs [Configuration](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration) intégrés ne sont **pas** conservées.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-128">Name-value pairs written to the built-in [Configuration](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="0a7ec-129">Toutefois, vous pouvez créer un fournisseur personnalisé qui enregistre les valeurs.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="0a7ec-130">Consultez la section relative à la création d’un [fournisseur de configuration personnalisé](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="0a7ec-131">L’exemple précédent utilise l’indexeur de configuration pour lire des valeurs.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="0a7ec-132">Pour accéder à la configuration en dehors de `Startup`, utilisez le *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="0a7ec-133">Pour plus d’informations, consultez la rubrique [Options](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="0a7ec-134">Configuration par environnement</span><span class="sxs-lookup"><span data-stu-id="0a7ec-134">Configuration by environment</span></span>

<span data-ttu-id="0a7ec-135">Il est courant d’avoir des paramètres de configuration différents pour différents environnements, par exemple pour l’environnement de développement, de test et de production.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="0a7ec-136">La méthode d’extension `CreateDefaultBuilder` dans une application ASP.NET Core 2.x (ou l’utilisation de `AddJsonFile` et de `AddEnvironmentVariables` directement dans une application ASP.NET Core 1.x) ajoute des fournisseurs de configuration pour la lecture des fichiers JSON et des sources de configuration système :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="0a7ec-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="0a7ec-137">*appsettings.json*</span></span>
* <span data-ttu-id="0a7ec-138">*appsettings.\<nom_environnement>.json*</span><span class="sxs-lookup"><span data-stu-id="0a7ec-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="0a7ec-139">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="0a7ec-139">Environment variables</span></span>

<span data-ttu-id="0a7ec-140">Les applications ASP.NET Core 1.x doivent appeler `AddJsonFile` et [AddEnvironmentVariables](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables #Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables #Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="0a7ec-141">Consultez [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) pour obtenir une explication des paramètres.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="0a7ec-142">`reloadOnChange` est pris en charge uniquement dans ASP.NET Core 1.1 et ultérieur.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="0a7ec-143">Les sources de configuration sont lues dans l’ordre où elles sont spécifiées.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="0a7ec-144">Dans le code précédent, les variables d’environnement sont lues en dernier.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="0a7ec-145">Toutes les valeurs de configuration définies dans l’environnement remplacent celles définies dans les deux fournisseurs précédents.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="0a7ec-146">Considérez le fichier *appsettings.Staging.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="0a7ec-147">Lorsque l’environnement a la valeur `Staging`, la méthode `Configure` suivante lit la valeur de `MyConfig` :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="0a7ec-148">L’environnement est généralement défini sur `Development`, `Staging` ou `Production`.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="0a7ec-149">Pour plus d’informations, consultez [Utilisation de plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="0a7ec-150">Points à prendre en considération pour la configuration :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-150">Configuration considerations:</span></span>

* <span data-ttu-id="0a7ec-151">`IOptionsSnapshot` peut recharger les données de configuration quand elles changent.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="0a7ec-152">Pour plus d’informations, consultez [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="0a7ec-153">Les clés de configuration ne respectent **pas** la casse.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="0a7ec-154">Ne stockez **jamais** des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="0a7ec-155">N’utilisez aucun secret de production dans vos environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-155">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="0a7ec-156">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans votre référentiel.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-156">Specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="0a7ec-157">Découvrez des informations supplémentaires sur l’[Utilisation de plusieurs environnements](xref:fundamentals/environments) et la gestion du [Stockage sécurisé des secrets d’application lors du développement](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="0a7ec-158">S’il n’est pas possible d’utiliser un signe deux-points (`:`) dans les variables d’environnement de votre système, remplacez le signe deux-points (`:`) par un double trait de soulignement (`__`).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-158">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="0a7ec-159">Fournisseur en mémoire et liaison à une classe POCO</span><span class="sxs-lookup"><span data-stu-id="0a7ec-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="0a7ec-160">L’exemple suivant montre comment utiliser le fournisseur en mémoire et le lier à une classe :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="0a7ec-161">Les valeurs de configuration sont retournées sous forme de chaînes, mais la liaison permet la construction d’objets.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="0a7ec-162">En effet, la liaison vous permet de récupérer des objets POCO ou même des graphes d’objets entiers.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-162">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="0a7ec-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="0a7ec-163">GetValue</span></span>

<span data-ttu-id="0a7ec-164">L’exemple suivant illustre la méthode d’extension [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-164">The following sample demonstrates the [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="0a7ec-165">La méthode `GetValue<T>` de ConfigurationBinder vous permet de spécifier une valeur par défaut (80 dans l’exemple).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-165">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="0a7ec-166">`GetValue<T>` est destiné aux scénarios simples et n’établit pas de liaison à des sections entières.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-166">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="0a7ec-167">`GetValue<T>` obtient les valeurs scalaires de `GetSection(key).Value` converties en un type spécifique.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-167">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="0a7ec-168">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="0a7ec-168">Bind to an object graph</span></span>

<span data-ttu-id="0a7ec-169">Vous pouvez établir une liaison récursive à chaque objet d’une classe.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-169">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="0a7ec-170">Considérez la classe `AppSettings` suivante :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="0a7ec-171">L’exemple suivant crée une liaison à la classe `AppSettings` :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="0a7ec-172">**ASP.NET Core 1.1** et les versions ultérieures peuvent utiliser `Get<T>`, qui fonctionne avec des sections entières.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="0a7ec-173">Il peut être plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="0a7ec-174">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="0a7ec-175">En utilisant le fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="0a7ec-176">Le programme affiche `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="0a7ec-177">Le code suivant peut être utilisé pour effectuer un test unitaire sur la configuration :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-177">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="0a7ec-178">Créer un fournisseur personnalisé Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0a7ec-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="0a7ec-179">Dans cette section, un fournisseur de configuration de base qui lit des paires nom/valeur à partir d’une base de données utilisant Entity Framework est créé.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="0a7ec-180">Définissez une entité `ConfigurationValue` pour le stockage des valeurs de configuration dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="0a7ec-181">Ajoutez un `ConfigurationContext` pour stocker les valeurs configurées et y accéder :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="0a7ec-182">Créez une classe qui implémente [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource) :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-182">Create a class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="0a7ec-183">Créez le fournisseur de configuration personnalisé en héritant de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span> <span data-ttu-id="0a7ec-184">Le fournisseur de configuration initialise la base de données quand elle est vide :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="0a7ec-185">Les valeurs en surbrillance provenant de la base de données ("value_from_ef_1" et "value_from_ef_2") sont affichées quand l’exemple est exécuté.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="0a7ec-186">Vous pouvez ajouter une méthode d’extension `EFConfigSource` pour l’ajout de la source de configuration :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-186">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="0a7ec-187">Le code suivant montre comment utiliser l’`EFConfigProvider` personnalisé :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="0a7ec-188">Notez que l’exemple ajoute l’`EFConfigProvider` personnalisé après le fournisseur JSON. Par conséquent, tous les paramètres de la base de données substituent les paramètres du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="0a7ec-189">En utilisant le fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="0a7ec-190">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="0a7ec-191">Fournisseur de configuration CommandLine</span><span class="sxs-lookup"><span data-stu-id="0a7ec-191">CommandLine configuration provider</span></span>

<span data-ttu-id="0a7ec-192">Le [fournisseur de configuration CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) reçoit des paires clé/valeur d’arguments de ligne de commande pour la configuration au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="0a7ec-193">Afficher ou télécharger l’exemple de configuration CommandLine</span><span class="sxs-lookup"><span data-stu-id="0a7ec-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="0a7ec-194">Configurer et utiliser le fournisseur de configuration CommandLine</span><span class="sxs-lookup"><span data-stu-id="0a7ec-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="0a7ec-195">Configuration de base</span><span class="sxs-lookup"><span data-stu-id="0a7ec-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="0a7ec-196">Pour activer la configuration en ligne de commande, appelez la méthode d’extension `AddCommandLine` sur une instance de [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="0a7ec-197">Quand le code est exécuté, la sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="0a7ec-198">Le passage de paires clé/valeur d’arguments sur la ligne de commande modifie les valeurs de `Profile:MachineName` et de `App:MainWindow:Left` :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="0a7ec-199">La fenêtre de console s’affiche :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="0a7ec-200">Pour substituer la configuration fournie par d’autres fournisseurs de configuration avec la configuration en ligne de commande, appelez `AddCommandLine` en dernier sur `ConfigurationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0a7ec-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0a7ec-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0a7ec-202">Les applications ASP.NET Core 2.x classiques utilisent la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="0a7ec-203">`CreateDefaultBuilder` charge une configuration facultative à partir d’*appsettings.json*, d’*appsettings.{Environment}.json*, de [secrets d’utilisateur](xref:security/app-secrets) (dans l’environnement `Development`), de variables d’environnement et d’arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="0a7ec-204">Le fournisseur de configuration CommandLine est appelé en dernier.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="0a7ec-205">Le fait d’appeler le fournisseur en dernier permet aux arguments de ligne de commande passés au moment de l’exécution de substituer la configuration définie par les autres fournisseurs de configuration appelés précédemment.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="0a7ec-206">Pour les fichiers *appsettings* où :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="0a7ec-207">`reloadOnChange` est activé.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="0a7ec-208">Contiennent le même paramètre dans les arguments de ligne de commande et un fichier *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="0a7ec-209">Le fichier *appsettings* contenant l’argument de ligne de commande correspondant est modifié après le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="0a7ec-210">Si toutes les conditions précédentes sont remplies, les arguments de ligne de commande sont remplacés.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="0a7ec-211">L’application ASP.NET Core 2.x peut utiliser WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) à la place de \`\`CreateDefaultBuilder`. When using `WebHostBuilder\`, configuration définie manuellement avec [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-211">ASP.NET Core 2.x app can use WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of \`\`CreateDefaultBuilder`. When using `WebHostBuilder\`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="0a7ec-212">Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-212">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0a7ec-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0a7ec-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0a7ec-214">Créez un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) et appelez la méthode `AddCommandLine` pour utiliser le fournisseur de configuration CommandLine.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-214">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="0a7ec-215">Le fait d’appeler le fournisseur en dernier permet aux arguments de ligne de commande passés au moment de l’exécution de substituer la configuration définie par les autres fournisseurs de configuration appelés précédemment.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-215">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="0a7ec-216">Appliquez la configuration à [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) avec la méthode `UseConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-216">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="0a7ec-217">Arguments</span><span class="sxs-lookup"><span data-stu-id="0a7ec-217">Arguments</span></span>

<span data-ttu-id="0a7ec-218">Les arguments passés sur la ligne de commande doivent être conformes à l’un des deux formats indiqués dans le tableau suivant :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-218">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="0a7ec-219">Format d’argument</span><span class="sxs-lookup"><span data-stu-id="0a7ec-219">Argument format</span></span>                                                     | <span data-ttu-id="0a7ec-220">Exemple</span><span class="sxs-lookup"><span data-stu-id="0a7ec-220">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="0a7ec-221">Argument unique : une paire clé/valeur séparée par un signe égal (`=`)</span><span class="sxs-lookup"><span data-stu-id="0a7ec-221">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="0a7ec-222">Séquence de deux arguments : une paire clé/valeur séparée par un espace</span><span class="sxs-lookup"><span data-stu-id="0a7ec-222">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="0a7ec-223">**Argument unique**</span><span class="sxs-lookup"><span data-stu-id="0a7ec-223">**Single argument**</span></span>

<span data-ttu-id="0a7ec-224">La valeur doit suivre un signe égal (`=`).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-224">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="0a7ec-225">La valeur peut être null (par exemple, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-225">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="0a7ec-226">La clé peut avoir un préfixe.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-226">The key may have a prefix.</span></span>

| <span data-ttu-id="0a7ec-227">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="0a7ec-227">Key prefix</span></span>               | <span data-ttu-id="0a7ec-228">Exemple</span><span class="sxs-lookup"><span data-stu-id="0a7ec-228">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="0a7ec-229">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="0a7ec-229">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="0a7ec-230">Un seul tiret (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="0a7ec-230">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="0a7ec-231">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="0a7ec-231">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="0a7ec-232">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="0a7ec-232">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="0a7ec-233">&#8224;Une clé avec un préfixe composé d’un seul préfixe (`-`) doit être fournie dans les [correspondances de commutateur](#switch-mappings), décrits ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-233">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="0a7ec-234">Exemple de commande :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-234">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="0a7ec-235">Remarque : Si `-key1` n’est pas présent dans les [correspondances de commutateur](#switch-mappings) donnés au fournisseur de configuration, un `FormatException` est levé.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-235">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="0a7ec-236">**Séquence de deux arguments**</span><span class="sxs-lookup"><span data-stu-id="0a7ec-236">**Sequence of two arguments**</span></span>

<span data-ttu-id="0a7ec-237">La valeur ne peut pas être null et doit suivre la clé, séparée par un espace.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-237">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="0a7ec-238">La clé doit avoir un préfixe.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-238">The key must have a prefix.</span></span>

| <span data-ttu-id="0a7ec-239">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="0a7ec-239">Key prefix</span></span>               | <span data-ttu-id="0a7ec-240">Exemple</span><span class="sxs-lookup"><span data-stu-id="0a7ec-240">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="0a7ec-241">Un seul tiret (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="0a7ec-241">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="0a7ec-242">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="0a7ec-242">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="0a7ec-243">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="0a7ec-243">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="0a7ec-244">&#8224;Une clé avec un préfixe composé d’un seul préfixe (`-`) doit être fournie dans les [correspondances de commutateur](#switch-mappings), décrits ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-244">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="0a7ec-245">Exemple de commande :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-245">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="0a7ec-246">Remarque : Si `-key1` n’est pas présent dans les [correspondances de commutateur](#switch-mappings) donnés au fournisseur de configuration, un `FormatException` est levé.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-246">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="0a7ec-247">Clés en double</span><span class="sxs-lookup"><span data-stu-id="0a7ec-247">Duplicate keys</span></span>

<span data-ttu-id="0a7ec-248">Si des clés en double sont fournies, la dernière paire clé/valeur est utilisée.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-248">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="0a7ec-249">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="0a7ec-249">Switch mappings</span></span>

<span data-ttu-id="0a7ec-250">Lors de la génération manuelle d’une configuration avec `ConfigurationBuilder`, vous pouvez éventuellement fournir un dictionnaire de correspondances de commutateur pour la méthode `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-250">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="0a7ec-251">Les correspondances de commutateur vous permettent de fournir une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-251">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="0a7ec-252">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-252">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="0a7ec-253">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la configuration.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-253">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="0a7ec-254">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-254">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="0a7ec-255">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-255">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="0a7ec-256">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-256">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="0a7ec-257">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-257">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="0a7ec-258">Dans l’exemple suivant, la méthode `GetSwitchMappings` permet à vos arguments de ligne de commande d’utiliser un préfixe de clé composé d’un tiret unique (`-`) et d’éviter les préfixes de sous-clés.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-258">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="0a7ec-259">Si aucun argument de ligne de commande n’est spécifié, le dictionnaire fourni à `AddInMemoryCollection` définit les valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-259">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="0a7ec-260">Exécutez l’application avec la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-260">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="0a7ec-261">La fenêtre de console s’affiche :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-261">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="0a7ec-262">Utilisez le code suivant pour passer des paramètres de configuration :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-262">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="0a7ec-263">La fenêtre de console s’affiche :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-263">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="0a7ec-264">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-264">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="0a7ec-265">Touche</span><span class="sxs-lookup"><span data-stu-id="0a7ec-265">Key</span></span>            | <span data-ttu-id="0a7ec-266">Value</span><span class="sxs-lookup"><span data-stu-id="0a7ec-266">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="0a7ec-267">Pour illustrer le remplacement des clés à l’aide du dictionnaire, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-267">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="0a7ec-268">Les clés de ligne de commande sont remplacées.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-268">The command-line keys are swapped.</span></span> <span data-ttu-id="0a7ec-269">La fenêtre de console affiche les valeurs de configuration pour `Profile:MachineName` et `App:MainWindow:Left` :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-269">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="0a7ec-270">Le fichier web.config</span><span class="sxs-lookup"><span data-stu-id="0a7ec-270">The web.config file</span></span>

<span data-ttu-id="0a7ec-271">Un fichier *web.config* est nécessaire pour héberger l’application dans IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-271">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="0a7ec-272">Les paramètres de *web.config* permettent au [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) de lancer l’application et de configurer d’autres modules et paramètres IIS.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-272">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="0a7ec-273">Si le fichier *web.config* n’est pas présent et que le fichier projet contient `<Project Sdk="Microsoft.NET.Sdk.Web">`, la publication du projet crée un fichier *web.config* dans la sortie publiée (le dossier *publish*).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-273">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="0a7ec-274">Pour plus d’informations, consultez [Héberger ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index#webconfig).</span><span class="sxs-lookup"><span data-stu-id="0a7ec-274">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="additional-notes"></a><span data-ttu-id="0a7ec-275">Remarques supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0a7ec-275">Additional notes</span></span>

* <span data-ttu-id="0a7ec-276">L’injection de dépendances n’est pas définie tant que `ConfigureServices` n’est pas appelé.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-276">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="0a7ec-277">Le système de configuration ne prend pas en charge l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-277">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="0a7ec-278">`IConfiguration` a deux spécialisations :</span><span class="sxs-lookup"><span data-stu-id="0a7ec-278">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="0a7ec-279">`IConfigurationRoot` Utilisé pour le nœud racine.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-279">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="0a7ec-280">Peut déclencher un rechargement.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-280">Can trigger a reload.</span></span>
  * <span data-ttu-id="0a7ec-281">`IConfigurationSection` Représente une section de valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-281">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="0a7ec-282">Les méthodes `GetSection` et `GetChildren` retournent un `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-282">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="0a7ec-283">Utilisez [IConfigurationRoot](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration.iconfigurationroot) quand vous rechargez la configuration ou devez accéder à chaque fournisseur.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-283">Use [IConfigurationRoot](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or need access to each provider.</span></span> <span data-ttu-id="0a7ec-284">Aucune de ces situations n’est courante.</span><span class="sxs-lookup"><span data-stu-id="0a7ec-284">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a7ec-285">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0a7ec-285">Additional resources</span></span>

* [<span data-ttu-id="0a7ec-286">Options</span><span class="sxs-lookup"><span data-stu-id="0a7ec-286">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="0a7ec-287">Utilisation de plusieurs environnements</span><span class="sxs-lookup"><span data-stu-id="0a7ec-287">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="0a7ec-288">Stockage sécurisé des secrets de l’application durant le développement</span><span class="sxs-lookup"><span data-stu-id="0a7ec-288">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="0a7ec-289">Hébergement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a7ec-289">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="0a7ec-290">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="0a7ec-290">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="0a7ec-291">Fournisseur de configuration Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0a7ec-291">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
