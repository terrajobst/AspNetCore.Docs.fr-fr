---
title: Configuration dans ASP.NET Core
author: rick-anderson
description: Utilisez l’API de configuration pour configurer une application ASP.NET Core selon plusieurs méthodes.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 4637ff6312f32f5887ff0f7a6e74d10f5beb0ca5
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="acc7f-103">Configuration dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="acc7f-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="acc7f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="acc7f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="acc7f-105">L’API de configuration fournit un moyen de configurer une application web ASP.NET Core basé sur une liste de paires nom/valeur.</span><span class="sxs-lookup"><span data-stu-id="acc7f-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="acc7f-106">La configuration est lue au moment de l’exécution à partir de plusieurs sources.</span><span class="sxs-lookup"><span data-stu-id="acc7f-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="acc7f-107">Les paires nom/valeur peuvent être regroupées dans une hiérarchie à plusieurs niveaux.</span><span class="sxs-lookup"><span data-stu-id="acc7f-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="acc7f-108">Il existe des fournisseurs de configuration pour les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="acc7f-108">There are configuration providers for:</span></span>

* <span data-ttu-id="acc7f-109">Formats de fichiers (INI, JSON et XML).</span><span class="sxs-lookup"><span data-stu-id="acc7f-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="acc7f-110">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="acc7f-110">Command-line arguments.</span></span>
* <span data-ttu-id="acc7f-111">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="acc7f-111">Environment variables.</span></span>
* <span data-ttu-id="acc7f-112">Objets .NET en mémoire.</span><span class="sxs-lookup"><span data-stu-id="acc7f-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="acc7f-113">Stockage [Secret Manager](xref:security/app-secrets) non chiffré.</span><span class="sxs-lookup"><span data-stu-id="acc7f-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="acc7f-114">Magasin utilisateur chiffré comme [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="acc7f-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="acc7f-115">Fournisseurs personnalisés (installés ou créés).</span><span class="sxs-lookup"><span data-stu-id="acc7f-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="acc7f-116">Chaque valeur de configuration correspond à une clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="acc7f-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="acc7f-117">Une liaison intégrée est prise en charge pour désérialiser les paramètres dans un objet [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personnalisé (une classe .NET simple avec des propriétés).</span><span class="sxs-lookup"><span data-stu-id="acc7f-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="acc7f-118">Le modèle d’options utilise des classes d’options pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="acc7f-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="acc7f-119">Pour plus d’informations sur l’utilisation du modèle d’options, consultez la rubrique [Options](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="acc7f-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="acc7f-120">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="acc7f-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="acc7f-121">Configuration JSON</span><span class="sxs-lookup"><span data-stu-id="acc7f-121">JSON configuration</span></span>

<span data-ttu-id="acc7f-122">L’application console suivante utilise le fournisseur de configuration JSON :</span><span class="sxs-lookup"><span data-stu-id="acc7f-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="acc7f-123">L’application lit et affiche les paramètres de configuration suivants :</span><span class="sxs-lookup"><span data-stu-id="acc7f-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="acc7f-124">La configuration est constituée d’une liste hiérarchique de paires nom/valeur, dans laquelle les nœuds sont séparés par un signe deux-points (`:`).</span><span class="sxs-lookup"><span data-stu-id="acc7f-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="acc7f-125">Pour récupérer une valeur, accédez à l’indexeur `Configuration` avec la clé de l’élément correspondant :</span><span class="sxs-lookup"><span data-stu-id="acc7f-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="acc7f-126">Pour utiliser des tableaux dans des sources de configuration au format JSON, utilisez un index de tableau comme partie d’une chaîne séparée par des signes deux-points.</span><span class="sxs-lookup"><span data-stu-id="acc7f-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="acc7f-127">L’exemple suivant obtient le nom du premier élément dans le tableau `wizards` précédent :</span><span class="sxs-lookup"><span data-stu-id="acc7f-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="acc7f-128">Les paires nom/valeur écrites dans les fournisseurs [Configuration](/dotnet/api/microsoft.extensions.configuration) intégrés ne sont **pas** conservées.</span><span class="sxs-lookup"><span data-stu-id="acc7f-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="acc7f-129">Toutefois, un fournisseur personnalisé qui enregistre les valeurs peut être créé.</span><span class="sxs-lookup"><span data-stu-id="acc7f-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="acc7f-130">Consultez la section relative à la création d’un [fournisseur de configuration personnalisé](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="acc7f-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="acc7f-131">L’exemple précédent utilise l’indexeur de configuration pour lire des valeurs.</span><span class="sxs-lookup"><span data-stu-id="acc7f-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="acc7f-132">Pour accéder à la configuration en dehors de `Startup`, utilisez le *modèle d’options*.</span><span class="sxs-lookup"><span data-stu-id="acc7f-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="acc7f-133">Pour plus d’informations, consultez la rubrique [Options](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="acc7f-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="acc7f-134">Configuration XML</span><span class="sxs-lookup"><span data-stu-id="acc7f-134">XML configuration</span></span>

<span data-ttu-id="acc7f-135">Pour utiliser des tableaux dans des sources de configuration au format XML, fournissez un index `name` à chaque élément.</span><span class="sxs-lookup"><span data-stu-id="acc7f-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="acc7f-136">Utilisez l’index pour accéder aux valeurs :</span><span class="sxs-lookup"><span data-stu-id="acc7f-136">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="acc7f-137">Configuration par environnement</span><span class="sxs-lookup"><span data-stu-id="acc7f-137">Configuration by environment</span></span>

<span data-ttu-id="acc7f-138">Il est courant d’avoir des paramètres de configuration différents pour différents environnements, par exemple pour l’environnement de développement, de test et de production.</span><span class="sxs-lookup"><span data-stu-id="acc7f-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="acc7f-139">La méthode d’extension `CreateDefaultBuilder` dans une application ASP.NET Core 2.x (ou l’utilisation de `AddJsonFile` et de `AddEnvironmentVariables` directement dans une application ASP.NET Core 1.x) ajoute des fournisseurs de configuration pour la lecture des fichiers JSON et des sources de configuration système :</span><span class="sxs-lookup"><span data-stu-id="acc7f-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="acc7f-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="acc7f-140">*appsettings.json*</span></span>
* <span data-ttu-id="acc7f-141">*appsettings.\<nom_environnement>.json*</span><span class="sxs-lookup"><span data-stu-id="acc7f-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="acc7f-142">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="acc7f-142">Environment variables</span></span>

<span data-ttu-id="acc7f-143">Les applications ASP.NET Core 1.x doivent appeler `AddJsonFile` et [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="acc7f-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="acc7f-144">Consultez [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) pour obtenir une explication des paramètres.</span><span class="sxs-lookup"><span data-stu-id="acc7f-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="acc7f-145">`reloadOnChange` est pris en charge uniquement dans ASP.NET Core 1.1 et ultérieur.</span><span class="sxs-lookup"><span data-stu-id="acc7f-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="acc7f-146">Les sources de configuration sont lues dans l’ordre où elles sont spécifiées.</span><span class="sxs-lookup"><span data-stu-id="acc7f-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="acc7f-147">Dans le code précédent, les variables d’environnement sont lues en dernier.</span><span class="sxs-lookup"><span data-stu-id="acc7f-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="acc7f-148">Toutes les valeurs de configuration définies dans l’environnement remplacent celles définies dans les deux fournisseurs précédents.</span><span class="sxs-lookup"><span data-stu-id="acc7f-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="acc7f-149">Considérez le fichier *appsettings.Staging.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="acc7f-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="acc7f-150">Lorsque l’environnement a la valeur `Staging`, la méthode `Configure` suivante lit la valeur de `MyConfig` :</span><span class="sxs-lookup"><span data-stu-id="acc7f-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="acc7f-151">L’environnement est généralement défini sur `Development`, `Staging` ou `Production`.</span><span class="sxs-lookup"><span data-stu-id="acc7f-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="acc7f-152">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="acc7f-152">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="acc7f-153">Points à prendre en considération pour la configuration :</span><span class="sxs-lookup"><span data-stu-id="acc7f-153">Configuration considerations:</span></span>

* <span data-ttu-id="acc7f-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) peut recharger les données de configuration quand elles changent.</span><span class="sxs-lookup"><span data-stu-id="acc7f-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="acc7f-155">Les clés de configuration ne respectent **pas** la casse.</span><span class="sxs-lookup"><span data-stu-id="acc7f-155">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="acc7f-156">Ne stockez **jamais** des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.</span><span class="sxs-lookup"><span data-stu-id="acc7f-156">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="acc7f-157">N’utilisez aucun secret de production dans les environnements de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="acc7f-157">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="acc7f-158">Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.</span><span class="sxs-lookup"><span data-stu-id="acc7f-158">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="acc7f-159">Découvrez plus en détail comment [utiliser plusieurs environnements](xref:fundamentals/environments) et gérer le [stockage sécurisé des secrets des applications lors du développement](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="acc7f-159">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="acc7f-160">Pour les valeurs de configuration hiérarchiques spécifiées dans des variables d’environnement, un signe deux-points (`:`) peut ne pas fonctionner sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="acc7f-160">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="acc7f-161">Le trait de soulignement double (`__`) est pris en charge par toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="acc7f-161">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="acc7f-162">Pour l’interaction avec l’API de configuration, un signe deux-points (`:`) fonctionne sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="acc7f-162">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="acc7f-163">Fournisseur en mémoire et liaison à une classe POCO</span><span class="sxs-lookup"><span data-stu-id="acc7f-163">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="acc7f-164">L’exemple suivant montre comment utiliser le fournisseur en mémoire et le lier à une classe :</span><span class="sxs-lookup"><span data-stu-id="acc7f-164">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="acc7f-165">Les valeurs de configuration sont retournées sous forme de chaînes, mais la liaison permet la construction d’objets.</span><span class="sxs-lookup"><span data-stu-id="acc7f-165">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="acc7f-166">En effet, la liaison permet la récupération des objets POCO ou même des graphes d’objets entiers.</span><span class="sxs-lookup"><span data-stu-id="acc7f-166">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="acc7f-167">GetValue</span><span class="sxs-lookup"><span data-stu-id="acc7f-167">GetValue</span></span>

<span data-ttu-id="acc7f-168">L’exemple suivant illustre la méthode d’extension [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) :</span><span class="sxs-lookup"><span data-stu-id="acc7f-168">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="acc7f-169">La méthode `GetValue<T>` de ConfigurationBinder permet de spécifier une valeur par défaut (80 dans l’exemple).</span><span class="sxs-lookup"><span data-stu-id="acc7f-169">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="acc7f-170">`GetValue<T>` est destiné aux scénarios simples et n’établit pas de liaison à des sections entières.</span><span class="sxs-lookup"><span data-stu-id="acc7f-170">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="acc7f-171">`GetValue<T>` obtient les valeurs scalaires de `GetSection(key).Value` converties en un type spécifique.</span><span class="sxs-lookup"><span data-stu-id="acc7f-171">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="acc7f-172">Établir une liaison à un graphe d’objets</span><span class="sxs-lookup"><span data-stu-id="acc7f-172">Bind to an object graph</span></span>

<span data-ttu-id="acc7f-173">Chaque objet d’une classe peut se voir établir une liaison récursive.</span><span class="sxs-lookup"><span data-stu-id="acc7f-173">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="acc7f-174">Considérez la classe `AppSettings` suivante :</span><span class="sxs-lookup"><span data-stu-id="acc7f-174">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="acc7f-175">L’exemple suivant crée une liaison à la classe `AppSettings` :</span><span class="sxs-lookup"><span data-stu-id="acc7f-175">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="acc7f-176">**ASP.NET Core 1.1** et les versions ultérieures peuvent utiliser `Get<T>`, qui fonctionne avec des sections entières.</span><span class="sxs-lookup"><span data-stu-id="acc7f-176">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="acc7f-177">Il peut être plus pratique d’utiliser `Get<T>` que `Bind`.</span><span class="sxs-lookup"><span data-stu-id="acc7f-177">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="acc7f-178">Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="acc7f-178">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="acc7f-179">En utilisant le fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="acc7f-179">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="acc7f-180">Le programme affiche `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="acc7f-180">The program displays `Height 11`.</span></span>

<span data-ttu-id="acc7f-181">Le code suivant peut être utilisé pour effectuer un test unitaire sur la configuration :</span><span class="sxs-lookup"><span data-stu-id="acc7f-181">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="acc7f-182">Créer un fournisseur personnalisé Entity Framework</span><span class="sxs-lookup"><span data-stu-id="acc7f-182">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="acc7f-183">Dans cette section, un fournisseur de configuration de base qui lit des paires nom/valeur à partir d’une base de données utilisant Entity Framework est créé.</span><span class="sxs-lookup"><span data-stu-id="acc7f-183">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="acc7f-184">Définissez une entité `ConfigurationValue` pour le stockage des valeurs de configuration dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="acc7f-184">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="acc7f-185">Ajoutez un `ConfigurationContext` pour stocker les valeurs configurées et y accéder :</span><span class="sxs-lookup"><span data-stu-id="acc7f-185">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="acc7f-186">Créez une classe qui implémente [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) :</span><span class="sxs-lookup"><span data-stu-id="acc7f-186">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="acc7f-187">Créez le fournisseur de configuration personnalisé en héritant de [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="acc7f-187">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="acc7f-188">Le fournisseur de configuration initialise la base de données quand elle est vide :</span><span class="sxs-lookup"><span data-stu-id="acc7f-188">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="acc7f-189">Les valeurs en surbrillance provenant de la base de données ("value_from_ef_1" et "value_from_ef_2") sont affichées quand l’exemple est exécuté.</span><span class="sxs-lookup"><span data-stu-id="acc7f-189">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="acc7f-190">Vous pouvez utiliser une méthode d’extension `EFConfigSource` pour l’ajout de la source de configuration :</span><span class="sxs-lookup"><span data-stu-id="acc7f-190">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="acc7f-191">Le code suivant montre comment utiliser l’`EFConfigProvider` personnalisé :</span><span class="sxs-lookup"><span data-stu-id="acc7f-191">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="acc7f-192">Notez que l’exemple ajoute l’`EFConfigProvider` personnalisé après le fournisseur JSON. Par conséquent, tous les paramètres de la base de données substituent les paramètres du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="acc7f-192">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="acc7f-193">En utilisant le fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="acc7f-193">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="acc7f-194">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="acc7f-194">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="acc7f-195">Fournisseur de configuration CommandLine</span><span class="sxs-lookup"><span data-stu-id="acc7f-195">CommandLine configuration provider</span></span>

<span data-ttu-id="acc7f-196">Le [fournisseur de configuration CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) reçoit des paires clé/valeur d’arguments de ligne de commande pour la configuration au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="acc7f-196">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="acc7f-197">Afficher ou télécharger l’exemple de configuration CommandLine</span><span class="sxs-lookup"><span data-stu-id="acc7f-197">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="acc7f-198">Configurer et utiliser le fournisseur de configuration CommandLine</span><span class="sxs-lookup"><span data-stu-id="acc7f-198">Setup and use the CommandLine configuration provider</span></span>

#### <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="acc7f-199">Configuration de base</span><span class="sxs-lookup"><span data-stu-id="acc7f-199">Basic Configuration</span></span>](#tab/basicconfiguration/)
<span data-ttu-id="acc7f-200">Pour activer la configuration en ligne de commande, appelez la méthode d’extension `AddCommandLine` sur une instance de [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) :</span><span class="sxs-lookup"><span data-stu-id="acc7f-200">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="acc7f-201">Quand le code est exécuté, la sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="acc7f-201">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="acc7f-202">Le passage de paires clé/valeur d’arguments sur la ligne de commande modifie les valeurs de `Profile:MachineName` et de `App:MainWindow:Left` :</span><span class="sxs-lookup"><span data-stu-id="acc7f-202">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="acc7f-203">La fenêtre de console s’affiche :</span><span class="sxs-lookup"><span data-stu-id="acc7f-203">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="acc7f-204">Pour substituer la configuration fournie par d’autres fournisseurs de configuration avec la configuration en ligne de commande, appelez `AddCommandLine` en dernier sur `ConfigurationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="acc7f-204">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc7f-205">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc7f-205">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="acc7f-206">Les applications ASP.NET Core 2.x classiques utilisent la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte :</span><span class="sxs-lookup"><span data-stu-id="acc7f-206">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="acc7f-207">`CreateDefaultBuilder` charge une configuration facultative à partir d’*appsettings.json*, d’*appsettings.{Environment}.json*, de [secrets d’utilisateur](xref:security/app-secrets) (dans l’environnement `Development`), de variables d’environnement et d’arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="acc7f-207">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="acc7f-208">Le fournisseur de configuration CommandLine est appelé en dernier.</span><span class="sxs-lookup"><span data-stu-id="acc7f-208">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="acc7f-209">Le fait d’appeler le fournisseur en dernier permet aux arguments de ligne de commande passés au moment de l’exécution de substituer la configuration définie par les autres fournisseurs de configuration appelés précédemment.</span><span class="sxs-lookup"><span data-stu-id="acc7f-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="acc7f-210">Pour les fichiers *appsettings* où :</span><span class="sxs-lookup"><span data-stu-id="acc7f-210">For *appsettings* files where:</span></span>

* <span data-ttu-id="acc7f-211">`reloadOnChange` est activé.</span><span class="sxs-lookup"><span data-stu-id="acc7f-211">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="acc7f-212">Contiennent le même paramètre dans les arguments de ligne de commande et un fichier *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="acc7f-212">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="acc7f-213">Le fichier *appsettings* contenant l’argument de ligne de commande correspondant est modifié après le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="acc7f-213">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="acc7f-214">Si toutes les conditions précédentes sont remplies, les arguments de ligne de commande sont remplacés.</span><span class="sxs-lookup"><span data-stu-id="acc7f-214">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="acc7f-215">L’application ASP.NET Core 2.x peut utiliser [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) au lieu de `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="acc7f-215">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="acc7f-216">Lorsque vous utilisez `WebHostBuilder`, définissez manuellement la configuration avec [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="acc7f-216">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="acc7f-217">Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="acc7f-217">See the ASP.NET Core 1.x tab for more information.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc7f-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc7f-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="acc7f-219">Créez un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) et appelez la méthode `AddCommandLine` pour utiliser le fournisseur de configuration CommandLine.</span><span class="sxs-lookup"><span data-stu-id="acc7f-219">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="acc7f-220">Le fait d’appeler le fournisseur en dernier permet aux arguments de ligne de commande passés au moment de l’exécution de substituer la configuration définie par les autres fournisseurs de configuration appelés précédemment.</span><span class="sxs-lookup"><span data-stu-id="acc7f-220">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="acc7f-221">Appliquez la configuration à [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) avec la méthode `UseConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="acc7f-221">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

* * *
### <a name="arguments"></a><span data-ttu-id="acc7f-222">Arguments</span><span class="sxs-lookup"><span data-stu-id="acc7f-222">Arguments</span></span>

<span data-ttu-id="acc7f-223">Les arguments passés sur la ligne de commande doivent être conformes à l’un des deux formats indiqués dans le tableau suivant :</span><span class="sxs-lookup"><span data-stu-id="acc7f-223">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="acc7f-224">Format d’argument</span><span class="sxs-lookup"><span data-stu-id="acc7f-224">Argument format</span></span>                                                     | <span data-ttu-id="acc7f-225">Exemple</span><span class="sxs-lookup"><span data-stu-id="acc7f-225">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="acc7f-226">Argument unique : une paire clé/valeur séparée par un signe égal (`=`)</span><span class="sxs-lookup"><span data-stu-id="acc7f-226">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="acc7f-227">Séquence de deux arguments : une paire clé/valeur séparée par un espace</span><span class="sxs-lookup"><span data-stu-id="acc7f-227">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="acc7f-228">**Argument unique**</span><span class="sxs-lookup"><span data-stu-id="acc7f-228">**Single argument**</span></span>

<span data-ttu-id="acc7f-229">La valeur doit suivre un signe égal (`=`).</span><span class="sxs-lookup"><span data-stu-id="acc7f-229">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="acc7f-230">La valeur peut être null (par exemple, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="acc7f-230">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="acc7f-231">La clé peut avoir un préfixe.</span><span class="sxs-lookup"><span data-stu-id="acc7f-231">The key may have a prefix.</span></span>

| <span data-ttu-id="acc7f-232">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="acc7f-232">Key prefix</span></span>               | <span data-ttu-id="acc7f-233">Exemple</span><span class="sxs-lookup"><span data-stu-id="acc7f-233">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="acc7f-234">Aucun préfixe</span><span class="sxs-lookup"><span data-stu-id="acc7f-234">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="acc7f-235">Un seul tiret (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="acc7f-235">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="acc7f-236">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="acc7f-236">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="acc7f-237">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="acc7f-237">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="acc7f-238">&#8224;Une clé avec un préfixe composé d’un seul préfixe (`-`) doit être fournie dans les [correspondances de commutateur](#switch-mappings), décrits ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="acc7f-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="acc7f-239">Exemple de commande :</span><span class="sxs-lookup"><span data-stu-id="acc7f-239">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="acc7f-240">Remarque : Si `-key2` n’est pas présent dans les [correspondances de commutateur](#switch-mappings) donnés au fournisseur de configuration, un `FormatException` est levé.</span><span class="sxs-lookup"><span data-stu-id="acc7f-240">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="acc7f-241">**Séquence de deux arguments**</span><span class="sxs-lookup"><span data-stu-id="acc7f-241">**Sequence of two arguments**</span></span>

<span data-ttu-id="acc7f-242">La valeur ne peut pas être null et doit suivre la clé, séparée par un espace.</span><span class="sxs-lookup"><span data-stu-id="acc7f-242">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="acc7f-243">La clé doit avoir un préfixe.</span><span class="sxs-lookup"><span data-stu-id="acc7f-243">The key must have a prefix.</span></span>

| <span data-ttu-id="acc7f-244">Préfixe de clé</span><span class="sxs-lookup"><span data-stu-id="acc7f-244">Key prefix</span></span>               | <span data-ttu-id="acc7f-245">Exemple</span><span class="sxs-lookup"><span data-stu-id="acc7f-245">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="acc7f-246">Un seul tiret (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="acc7f-246">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="acc7f-247">Deux tirets (`--`)</span><span class="sxs-lookup"><span data-stu-id="acc7f-247">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="acc7f-248">Barre oblique (`/`)</span><span class="sxs-lookup"><span data-stu-id="acc7f-248">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="acc7f-249">&#8224;Une clé avec un préfixe composé d’un seul préfixe (`-`) doit être fournie dans les [correspondances de commutateur](#switch-mappings), décrits ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="acc7f-249">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="acc7f-250">Exemple de commande :</span><span class="sxs-lookup"><span data-stu-id="acc7f-250">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="acc7f-251">Remarque : Si `-key1` n’est pas présent dans les [correspondances de commutateur](#switch-mappings) donnés au fournisseur de configuration, un `FormatException` est levé.</span><span class="sxs-lookup"><span data-stu-id="acc7f-251">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="acc7f-252">Clés en double</span><span class="sxs-lookup"><span data-stu-id="acc7f-252">Duplicate keys</span></span>

<span data-ttu-id="acc7f-253">Si des clés en double sont fournies, la dernière paire clé/valeur est utilisée.</span><span class="sxs-lookup"><span data-stu-id="acc7f-253">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="acc7f-254">Correspondances de commutateur</span><span class="sxs-lookup"><span data-stu-id="acc7f-254">Switch mappings</span></span>

<span data-ttu-id="acc7f-255">Lors de la génération manuelle d’une configuration avec `ConfigurationBuilder`, un dictionnaire de correspondances de commutateur peut être ajouté à la méthode `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="acc7f-255">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="acc7f-256">Les correspondances de commutateur permettent une logique de remplacement des noms de clés.</span><span class="sxs-lookup"><span data-stu-id="acc7f-256">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="acc7f-257">Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="acc7f-257">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="acc7f-258">Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la configuration.</span><span class="sxs-lookup"><span data-stu-id="acc7f-258">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="acc7f-259">Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).</span><span class="sxs-lookup"><span data-stu-id="acc7f-259">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="acc7f-260">Règles des clés du dictionnaire de correspondances de commutateur :</span><span class="sxs-lookup"><span data-stu-id="acc7f-260">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="acc7f-261">Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).</span><span class="sxs-lookup"><span data-stu-id="acc7f-261">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="acc7f-262">Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.</span><span class="sxs-lookup"><span data-stu-id="acc7f-262">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="acc7f-263">Dans l’exemple suivant, la méthode `GetSwitchMappings` permet aux arguments de ligne de commande d’utiliser un préfixe de clé composé d’un tiret unique (`-`) et d’éviter les préfixes de sous-clés.</span><span class="sxs-lookup"><span data-stu-id="acc7f-263">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="acc7f-264">Si aucun argument de ligne de commande n’est spécifié, le dictionnaire fourni à `AddInMemoryCollection` définit les valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="acc7f-264">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="acc7f-265">Exécutez l’application avec la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="acc7f-265">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="acc7f-266">La fenêtre de console s’affiche :</span><span class="sxs-lookup"><span data-stu-id="acc7f-266">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="acc7f-267">Utilisez le code suivant pour passer des paramètres de configuration :</span><span class="sxs-lookup"><span data-stu-id="acc7f-267">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="acc7f-268">La fenêtre de console s’affiche :</span><span class="sxs-lookup"><span data-stu-id="acc7f-268">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="acc7f-269">Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant :</span><span class="sxs-lookup"><span data-stu-id="acc7f-269">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="acc7f-270">Touche</span><span class="sxs-lookup"><span data-stu-id="acc7f-270">Key</span></span>            | <span data-ttu-id="acc7f-271">Value</span><span class="sxs-lookup"><span data-stu-id="acc7f-271">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="acc7f-272">Pour illustrer le remplacement des clés à l’aide du dictionnaire, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="acc7f-272">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="acc7f-273">Les clés de ligne de commande sont remplacées.</span><span class="sxs-lookup"><span data-stu-id="acc7f-273">The command-line keys are swapped.</span></span> <span data-ttu-id="acc7f-274">La fenêtre de console affiche les valeurs de configuration pour `Profile:MachineName` et `App:MainWindow:Left` :</span><span class="sxs-lookup"><span data-stu-id="acc7f-274">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="acc7f-275">fichier web.config</span><span class="sxs-lookup"><span data-stu-id="acc7f-275">web.config file</span></span>

<span data-ttu-id="acc7f-276">Un fichier *web.config* est nécessaire pour héberger l’application dans IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="acc7f-276">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="acc7f-277">Les paramètres de *web.config* permettent au [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) de lancer l’application et de configurer d’autres modules et paramètres IIS.</span><span class="sxs-lookup"><span data-stu-id="acc7f-277">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="acc7f-278">Si le fichier *web.config* n’est pas présent et que le fichier projet contient `<Project Sdk="Microsoft.NET.Sdk.Web">`, la publication du projet crée un fichier *web.config* dans la sortie publiée (le dossier *publish*).</span><span class="sxs-lookup"><span data-stu-id="acc7f-278">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="acc7f-279">Pour plus d’informations, consultez [Héberger ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="acc7f-279">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="acc7f-280">Accéder à la configuration lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="acc7f-280">Access configuration during startup</span></span>

<span data-ttu-id="acc7f-281">Pour accéder à la configuration dans `ConfigureServices` ou `Configure` au démarrage, consultez les exemples dans la rubrique [Démarrage de l’application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="acc7f-281">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="acc7f-282">Ajout de la configuration à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="acc7f-282">Adding configuration from an external assembly</span></span>

<span data-ttu-id="acc7f-283">L’implémentation de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="acc7f-283">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="acc7f-284">Pour plus d’informations, consultez [Améliorer une application à partir d’un assembly externe](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="acc7f-284">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="acc7f-285">Accéder à la configuration dans une page Razor ou une vue MVC</span><span class="sxs-lookup"><span data-stu-id="acc7f-285">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="acc7f-286">Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](/dotnet/api/microsoft.extensions.configuration) et injectez [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dans la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="acc7f-286">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="acc7f-287">Dans une page Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="acc7f-287">In a Razor Pages page:</span></span>

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
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="acc7f-288">Dans une vue MVC :</span><span class="sxs-lookup"><span data-stu-id="acc7f-288">In an MVC view:</span></span>

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
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="acc7f-289">Remarques supplémentaires</span><span class="sxs-lookup"><span data-stu-id="acc7f-289">Additional notes</span></span>

* <span data-ttu-id="acc7f-290">L’injection de dépendances n’est pas définie tant que `ConfigureServices` n’est pas appelé.</span><span class="sxs-lookup"><span data-stu-id="acc7f-290">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="acc7f-291">Le système de configuration ne prend pas en charge l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="acc7f-291">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="acc7f-292">`IConfiguration` a deux spécialisations :</span><span class="sxs-lookup"><span data-stu-id="acc7f-292">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="acc7f-293">`IConfigurationRoot` Utilisé pour le nœud racine.</span><span class="sxs-lookup"><span data-stu-id="acc7f-293">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="acc7f-294">Peut déclencher un rechargement.</span><span class="sxs-lookup"><span data-stu-id="acc7f-294">Can trigger a reload.</span></span>
  * <span data-ttu-id="acc7f-295">`IConfigurationSection` Représente une section de valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="acc7f-295">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="acc7f-296">Les méthodes `GetSection` et `GetChildren` retournent un `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="acc7f-296">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="acc7f-297">Utilisez [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) quand vous rechargez la configuration ou accéder à chaque fournisseur.</span><span class="sxs-lookup"><span data-stu-id="acc7f-297">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="acc7f-298">Aucune de ces situations n’est courante.</span><span class="sxs-lookup"><span data-stu-id="acc7f-298">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="acc7f-299">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="acc7f-299">Additional resources</span></span>

* [<span data-ttu-id="acc7f-300">Options</span><span class="sxs-lookup"><span data-stu-id="acc7f-300">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="acc7f-301">Utiliser plusieurs environnements</span><span class="sxs-lookup"><span data-stu-id="acc7f-301">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="acc7f-302">Stockage sécurisé des secrets des applications lors du développement</span><span class="sxs-lookup"><span data-stu-id="acc7f-302">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="acc7f-303">Hébergement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="acc7f-303">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="acc7f-304">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="acc7f-304">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="acc7f-305">Fournisseur de configuration Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="acc7f-305">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
