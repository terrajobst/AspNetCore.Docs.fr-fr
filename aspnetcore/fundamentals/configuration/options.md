---
title: Modèle d’options dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser le modèle d’options pour représenter des groupes de paramètres associés dans les applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: ef6b0117b88c4c79771f0280267bd99993028ac8
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655418"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="00ab3-103">Modèle d’options dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00ab3-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="00ab3-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="00ab3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="00ab3-105">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="00ab3-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="00ab3-106">Quand les paramètres de configuration sont isolés par fonctionnalité dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :</span><span class="sxs-lookup"><span data-stu-id="00ab3-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="00ab3-107">[Principe de séparation des interfaces](http://deviq.com/interface-segregation-principle/) : les fonctionnalités (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="00ab3-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="00ab3-108">[Séparation des préoccupations](http://deviq.com/separation-of-concerns/) : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="00ab3-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="00ab3-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)) Cet article est plus facile à suivre avec l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="00ab3-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="00ab3-110">Configuration des options de base</span><span class="sxs-lookup"><span data-stu-id="00ab3-110">Basic options configuration</span></span>

<span data-ttu-id="00ab3-111">La configuration des options de base est illustrée dans l’exemple &num;1 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="00ab3-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="00ab3-112">Une classe d’options doit être non abstraite avec un constructeur public sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="00ab3-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="00ab3-113">La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="00ab3-114">Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="00ab3-115">`Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="00ab3-116">La classe `MyOptions` est ajoutée au conteneur de service avec [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) et liée à la configuration :</span><span class="sxs-lookup"><span data-stu-id="00ab3-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="00ab3-117">Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:mvc/controllers/dependency-injection) avec [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-117">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="00ab3-118">Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :</span><span class="sxs-lookup"><span data-stu-id="00ab3-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="00ab3-119">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="00ab3-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="00ab3-120">Quand vous utilisez un [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personnalisé pour charger une configuration d’options à partir d’un fichier de paramètres, vérifiez que le chemin de base est correctement défini :</span><span class="sxs-lookup"><span data-stu-id="00ab3-120">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="00ab3-121">Vous n’avez pas besoin de définir explicitement le chemin de base quand vous chargez une configuration d’options à partir du fichier de paramètres via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="00ab3-121">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="00ab3-122">Configurer des options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="00ab3-122">Configure simple options with a delegate</span></span>

<span data-ttu-id="00ab3-123">La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="00ab3-123">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="00ab3-124">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="00ab3-124">Use a delegate to set options values.</span></span> <span data-ttu-id="00ab3-125">L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-125">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="00ab3-126">Dans le code suivant, un second service `IConfigureOptions<TOptions>` est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="00ab3-126">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="00ab3-127">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :</span><span class="sxs-lookup"><span data-stu-id="00ab3-127">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="00ab3-128">*Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="00ab3-128">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="00ab3-129">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="00ab3-129">You can add multiple configuration providers.</span></span> <span data-ttu-id="00ab3-130">Des fournisseurs de configuration sont disponibles dans les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="00ab3-130">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="00ab3-131">Ils sont appliqués dans l’ordre dans lequel ils sont inscrits.</span><span class="sxs-lookup"><span data-stu-id="00ab3-131">They're applied in order that they're registered.</span></span>

<span data-ttu-id="00ab3-132">Chaque appel à [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) ajoute un service `IConfigureOptions<TOptions>` au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="00ab3-132">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="00ab3-133">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="00ab3-133">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="00ab3-134">Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="00ab3-134">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="00ab3-135">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="00ab3-135">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="00ab3-136">Configuration des sous-options</span><span class="sxs-lookup"><span data-stu-id="00ab3-136">Suboptions configuration</span></span>

<span data-ttu-id="00ab3-137">La configuration des sous-options est illustrée dans l’exemple &num;3 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="00ab3-137">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="00ab3-138">Les applications doivent créer des classes d’options qui appartiennent à des groupes de fonctionnalités spécifiques (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="00ab3-138">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="00ab3-139">Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="00ab3-139">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="00ab3-140">Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-140">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="00ab3-141">Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="00ab3-141">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="00ab3-142">Dans le code suivant, un troisième service `IConfigureOptions<TOptions>` est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="00ab3-142">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="00ab3-143">Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="00ab3-143">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="00ab3-144">La méthode d’extension `GetSection` requiert le package NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="00ab3-144">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="00ab3-145">Si l’application utilise le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou version ultérieure), le package est automatiquement inclus.</span><span class="sxs-lookup"><span data-stu-id="00ab3-145">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="00ab3-146">Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :</span><span class="sxs-lookup"><span data-stu-id="00ab3-146">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="00ab3-147">La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs des options (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-147">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="00ab3-148">La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs des options (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-148">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="00ab3-149">Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :</span><span class="sxs-lookup"><span data-stu-id="00ab3-149">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="00ab3-150">Options fournies par un modèle d’affichage ou avec une injection de vue directe</span><span class="sxs-lookup"><span data-stu-id="00ab3-150">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="00ab3-151">Les options fournies par un modèle d’affichage ou avec une injection de vue directe sont illustrées dans l’exemple &num;4 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="00ab3-151">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="00ab3-152">Vous pouvez fournir les options dans un modèle d’affichage ou en injectant `IOptions<TOptions>` directement dans une vue (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-152">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="00ab3-153">Pour l’injection directe, injectez `IOptions<MyOptions>` avec une directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="00ab3-153">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="00ab3-154">Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="00ab3-154">When the app is run, the options values are shown in the rendered page:</span></span>

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="00ab3-156">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="00ab3-156">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="00ab3-157">Le rechargement des données de configuration avec `IOptionsSnapshot` est illustré dans l’exemple &num;5 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="00ab3-157">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="00ab3-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) prend en charge le rechargement des options avec une charge de traitement minimale.</span><span class="sxs-lookup"><span data-stu-id="00ab3-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="00ab3-159">Les options sont calculées une fois par requête quand le système y accède et sont mises en cache pour toute la durée de vie de la requête.</span><span class="sxs-lookup"><span data-stu-id="00ab3-159">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="00ab3-160">`IOptionsSnapshot` est un instantané d’[IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) et se met à jour automatiquement chaque fois que le moniteur déclenche des changements liés à la modification de la source de données.</span><span class="sxs-lookup"><span data-stu-id="00ab3-160">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="00ab3-161">Dans l’exemple suivant, un `IOptionsSnapshot` est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="00ab3-161">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="00ab3-162">Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.</span><span class="sxs-lookup"><span data-stu-id="00ab3-162">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="00ab3-163">L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="00ab3-163">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="00ab3-164">Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-164">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="00ab3-165">Enregistrez le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="00ab3-165">Save the *appsettings.json* file.</span></span> <span data-ttu-id="00ab3-166">Actualisez le navigateur pour constater la mise à jour des valeurs des options :</span><span class="sxs-lookup"><span data-stu-id="00ab3-166">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="00ab3-167">Prise en charge des options nommées avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="00ab3-167">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="00ab3-168">La prise en charge des options nommées avec [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) est illustrée dans l’exemple &num;6 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="00ab3-168">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="00ab3-169">La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées.</span><span class="sxs-lookup"><span data-stu-id="00ab3-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="00ab3-170">Dans l’exemple d’application, les options nommées sont déclarées avec [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure), qui appelle ensuite la méthode d’extension [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-170">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="00ab3-171">L’exemple d’application accède aux options nommées avec [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="00ab3-172">Quand l’exemple d’application est exécuté, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="00ab3-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="00ab3-173">Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="00ab3-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="00ab3-174">Les valeurs `named_options_2` sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="00ab3-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="00ab3-175">Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="00ab3-176">La valeur par défaut `Option2` fournie par la classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="00ab3-177">Configurez toutes les instances d’options nommées avec la méthode [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="00ab3-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="00ab3-178">Le code suivant configure `Option1` pour toutes les instances de configuration nommées ayant une valeur commune.</span><span class="sxs-lookup"><span data-stu-id="00ab3-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="00ab3-179">Ajoutez le code suivant manuellement à la méthode `Configure` :</span><span class="sxs-lookup"><span data-stu-id="00ab3-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="00ab3-180">Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="00ab3-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="00ab3-181">Toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="00ab3-181">All options are named instances.</span></span> <span data-ttu-id="00ab3-182">Les instances `IConfigureOption` existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="00ab3-183">En outre, `IConfigureNamedOptions` implémente `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="00ab3-184">L’implémentation par défaut de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([source de référence](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) a une logique qui permet de les utiliser chacune de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="00ab3-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="00ab3-185">L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) et [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="00ab3-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="00ab3-186">Validation des options</span><span class="sxs-lookup"><span data-stu-id="00ab3-186">Options validation</span></span>

<span data-ttu-id="00ab3-187">La validation des options vous permet de valider les options lorsque celles-ci sont configurées.</span><span class="sxs-lookup"><span data-stu-id="00ab3-187">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="00ab3-188">Appelez `Validate` avec une méthode de validation qui retourne `true` si les options sont valides et `false` si elles ne sont pas valides :</span><span class="sxs-lookup"><span data-stu-id="00ab3-188">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="00ab3-189">L’exemple précédent définit l’instance d’options nommée sur `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-189">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="00ab3-190">L'instance d’options par défaut est `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-190">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="00ab3-191">La validation s’exécute lorsque l’instance d’options est créée.</span><span class="sxs-lookup"><span data-stu-id="00ab3-191">Validation runs when the options instance is created.</span></span> <span data-ttu-id="00ab3-192">Votre instance d’options est garantie de réussir la validation lors du premier accès.</span><span class="sxs-lookup"><span data-stu-id="00ab3-192">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00ab3-193">La validation des options ne protège pas contre les modifications des options une fois que les options sont initialement configurées et validées.</span><span class="sxs-lookup"><span data-stu-id="00ab3-193">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="00ab3-194">La méthode `Validate` accepte un `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-194">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="00ab3-195">Pour personnaliser entièrement la validation, implémentez `IValidateOptions<TOptions>`, ce qui permet :</span><span class="sxs-lookup"><span data-stu-id="00ab3-195">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="00ab3-196">Validation de plusieurs types d’options : `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="00ab3-196">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="00ab3-197">Validation qui dépend d’un autre type d’option : `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="00ab3-197">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span></span>

<span data-ttu-id="00ab3-198">`IValidateOptions` valide :</span><span class="sxs-lookup"><span data-stu-id="00ab3-198">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="00ab3-199">Une instance d’options nommée spécifique.</span><span class="sxs-lookup"><span data-stu-id="00ab3-199">A specific named options instance.</span></span>
* <span data-ttu-id="00ab3-200">Toutes les options quand `name` est `null`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-200">All options when `name` is `null`.</span></span>

<span data-ttu-id="00ab3-201">Retourne `ValidateOptionsResult` à partir de votre implémentation de l’interface :</span><span class="sxs-lookup"><span data-stu-id="00ab3-201">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="00ab3-202">La validation stricte (échec rapide au démarrage) et la validation basée sur l’annotation des données sont planifiées pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="00ab3-202">Eager validation (fail fast at startup) and data annotation-based validation are scheduled for a future release.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a><span data-ttu-id="00ab3-203">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="00ab3-203">IPostConfigureOptions</span></span>

<span data-ttu-id="00ab3-204">Définir la post-configuration avec [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="00ab3-204">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="00ab3-205">La post-configuration s’exécute après la totalité de la configuration [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) :</span><span class="sxs-lookup"><span data-stu-id="00ab3-205">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="00ab3-206">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) permet de post-configurer les options nommées :</span><span class="sxs-lookup"><span data-stu-id="00ab3-206">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="00ab3-207">Utilisez [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) pour post-configurer toutes les instances de configuration nommées :</span><span class="sxs-lookup"><span data-stu-id="00ab3-207">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="00ab3-208">Fabrique d’options, surveillance et cache</span><span class="sxs-lookup"><span data-stu-id="00ab3-208">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="00ab3-209">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) est utilisé pour les notifications quand des instances `TOptions` changent.</span><span class="sxs-lookup"><span data-stu-id="00ab3-209">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="00ab3-210">`IOptionsMonitor` prend en charge les options rechargeables, les notifications de modifications et `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-210">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="00ab3-211">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) est chargé de créer les instances d’options.</span><span class="sxs-lookup"><span data-stu-id="00ab3-211">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="00ab3-212">Elle dispose d’une seule méthode ([Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)).</span><span class="sxs-lookup"><span data-stu-id="00ab3-212">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="00ab3-213">L’implémentation par défaut prend toutes les `IConfigureOptions` et `IPostConfigureOptions` inscrites et exécute toutes les configurations, puis les post-configurations.</span><span class="sxs-lookup"><span data-stu-id="00ab3-213">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="00ab3-214">Elle fait la distinction entre `IConfigureNamedOptions` et `IConfigureOptions` et n’appelle que l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="00ab3-214">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="00ab3-215">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) est utilisé par `IOptionsMonitor` pour la mise en cache des instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-215">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="00ab3-216">`IOptionsMonitorCache` invalide les instances des options dans le moniteur afin que la valeur soit recalculée ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="00ab3-216">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="00ab3-217">Les valeurs peuvent aussi être introduites manuellement avec [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="00ab3-217">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="00ab3-218">La méthode [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) est utilisée quand toutes les instances nommées doivent être recréées à la demande.</span><span class="sxs-lookup"><span data-stu-id="00ab3-218">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="00ab3-219">Accès aux options au démarrage</span><span class="sxs-lookup"><span data-stu-id="00ab3-219">Accessing options during startup</span></span>

<span data-ttu-id="00ab3-220">`IOptions` peut être utilisé dans `Startup.Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute.</span><span class="sxs-lookup"><span data-stu-id="00ab3-220">`IOptions` can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

<span data-ttu-id="00ab3-221">`IOptions` ne doit pas être utilisé dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="00ab3-221">`IOptions` shouldn't be used in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="00ab3-222">Un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="00ab3-222">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00ab3-223">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="00ab3-223">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
