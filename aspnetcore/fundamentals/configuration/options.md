---
title: Modèle d’options dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser le modèle d’options pour représenter des groupes de paramètres associés dans les applications ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: 660ee2365e2e186dd93d57ec79628e0bd7d24d52
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2018
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="9914b-103">Modèle d’options dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9914b-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="9914b-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9914b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9914b-105">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="9914b-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="9914b-106">Quand les paramètres de configuration sont isolés par fonctionnalité dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :</span><span class="sxs-lookup"><span data-stu-id="9914b-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="9914b-107">[Principe de séparation des interfaces](http://deviq.com/interface-segregation-principle/) : les fonctionnalités (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="9914b-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="9914b-108">[Séparation des préoccupations](http://deviq.com/separation-of-concerns/) : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="9914b-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="9914b-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)) Cet article est plus facile à suivre avec l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="9914b-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="9914b-110">Configuration des options de base</span><span class="sxs-lookup"><span data-stu-id="9914b-110">Basic options configuration</span></span>

<span data-ttu-id="9914b-111">La configuration des options de base est illustrée dans l’exemple &num;1 de [l’exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="9914b-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="9914b-112">Une classe d’options doit être non abstraite avec un constructeur public sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="9914b-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="9914b-113">La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="9914b-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="9914b-114">Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="9914b-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="9914b-115">`Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9914b-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="9914b-116">La classe `MyOptions` est ajoutée au conteneur de service avec [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) et liée à la configuration :</span><span class="sxs-lookup"><span data-stu-id="9914b-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="9914b-117">Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:fundamentals/dependency-injection#what-is-dependency-injection) avec [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9914b-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="9914b-118">Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :</span><span class="sxs-lookup"><span data-stu-id="9914b-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json)]

<span data-ttu-id="9914b-119">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="9914b-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="9914b-120">Configurer des options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="9914b-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="9914b-121">La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de [l’exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="9914b-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="9914b-122">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="9914b-122">Use a delegate to set options values.</span></span> <span data-ttu-id="9914b-123">L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9914b-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="9914b-124">Dans le code suivant, un second service `IConfigureOptions<TOptions>` est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="9914b-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="9914b-125">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :</span><span class="sxs-lookup"><span data-stu-id="9914b-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="9914b-126">*Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="9914b-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="9914b-127">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="9914b-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="9914b-128">Des fournisseurs de configuration sont disponibles dans les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="9914b-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="9914b-129">Ils sont appliqués dans l’ordre dans lequel ils sont inscrits.</span><span class="sxs-lookup"><span data-stu-id="9914b-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="9914b-130">Chaque appel à [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) ajoute un service `IConfigureOptions<TOptions>` au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="9914b-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="9914b-131">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="9914b-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="9914b-132">Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="9914b-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="9914b-133">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="9914b-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="9914b-134">Configuration des sous-options</span><span class="sxs-lookup"><span data-stu-id="9914b-134">Suboptions configuration</span></span>

<span data-ttu-id="9914b-135">La configuration des sous-options est illustrée dans l’exemple &num;3 de [l’exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="9914b-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="9914b-136">Les applications doivent créer des classes d’options qui appartiennent à des groupes de fonctionnalités spécifiques (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="9914b-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="9914b-137">Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="9914b-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="9914b-138">Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="9914b-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="9914b-139">Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9914b-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="9914b-140">Dans le code suivant, un troisième service `IConfigureOptions<TOptions>` est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="9914b-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="9914b-141">Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="9914b-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="9914b-142">La méthode d’extension `GetSection` requiert le package NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="9914b-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="9914b-143">Si l’application utilise le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), le package est automatiquement inclus.</span><span class="sxs-lookup"><span data-stu-id="9914b-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="9914b-144">Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :</span><span class="sxs-lookup"><span data-stu-id="9914b-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="9914b-145">La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs de sous-options (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9914b-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="9914b-146">La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs de sous-option (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9914b-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="9914b-147">Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :</span><span class="sxs-lookup"><span data-stu-id="9914b-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="9914b-148">Options fournies par un modèle d’affichage ou avec une injection de vue directe</span><span class="sxs-lookup"><span data-stu-id="9914b-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="9914b-149">Les options fournies par un modèle d’affichage ou avec une injection de vue directe sont illustrées dans l’exemple &num;4 de [l’exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="9914b-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="9914b-150">Vous pouvez fournir les options dans un modèle d’affichage ou en injectant `IOptions<TOptions>` directement dans une vue (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9914b-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="9914b-151">Pour l’injection directe, injectez `IOptions<MyOptions>` avec une directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="9914b-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="9914b-152">Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="9914b-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="9914b-154">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="9914b-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="9914b-155">Le rechargement des données de configuration avec `IOptionsSnapshot` est illustré dans l’exemple &num;5 de [l’exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="9914b-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="9914b-156">*Nécessite ASP.NET Core 1.1 ou version ultérieure.*</span><span class="sxs-lookup"><span data-stu-id="9914b-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="9914b-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) prend en charge le rechargement des options avec une charge de traitement minimale.</span><span class="sxs-lookup"><span data-stu-id="9914b-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="9914b-158">Dans ASP.NET Core 1.1, `IOptionsSnapshot` est un instantané [d’IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) et se met à jour automatiquement chaque fois que le moniteur déclenche des changements liés à la modification de la source de données.</span><span class="sxs-lookup"><span data-stu-id="9914b-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="9914b-159">Dans ASP.NET Core 2.0 et versions ultérieures, les options sont calculées une fois par demande quand le système y accède et sont mises en cache pour toute la durée de vie de la demande.</span><span class="sxs-lookup"><span data-stu-id="9914b-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="9914b-160">Dans l’exemple suivant, un `IOptionsSnapshot` est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="9914b-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="9914b-161">Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.</span><span class="sxs-lookup"><span data-stu-id="9914b-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="9914b-162">L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="9914b-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="9914b-163">Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="9914b-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="9914b-164">Enregistrez le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9914b-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="9914b-165">Actualisez le navigateur pour constater la mise à jour des valeurs des options :</span><span class="sxs-lookup"><span data-stu-id="9914b-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="9914b-166">Prise en charge des options nommées avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="9914b-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="9914b-167">La prise en charge des options nommées avec [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) est illustrée dans l’exemple &num;6 de [l’exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="9914b-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="9914b-168">*Nécessite ASP.NET Core 2.0 ou version ultérieure.*</span><span class="sxs-lookup"><span data-stu-id="9914b-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="9914b-169">La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées.</span><span class="sxs-lookup"><span data-stu-id="9914b-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="9914b-170">Dans l’exemple d’application, les options nommées sont déclarées avec la méthode [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) :</span><span class="sxs-lookup"><span data-stu-id="9914b-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="9914b-171">L’exemple d’application accède aux options nommées avec [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9914b-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="9914b-172">Quand l’exemple d’application est exécuté, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="9914b-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="9914b-173">Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9914b-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="9914b-174">Les valeurs `named_options_2` sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="9914b-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="9914b-175">Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="9914b-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="9914b-176">La valeur par défaut `Option2` fournie par la classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="9914b-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="9914b-177">Configurez toutes les instances d’options nommées avec la méthode [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="9914b-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="9914b-178">Le code suivant configure `Option1` pour toutes les instances de configuration nommées ayant une valeur commune.</span><span class="sxs-lookup"><span data-stu-id="9914b-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="9914b-179">Ajoutez le code suivant manuellement à la méthode `Configure` :</span><span class="sxs-lookup"><span data-stu-id="9914b-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="9914b-180">Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="9914b-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="9914b-181">Dans ASP.NET Core 2.0 et versions ultérieures, toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="9914b-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="9914b-182">Les instances `IConfigureOption` existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="9914b-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="9914b-183">En outre, `IConfigureNamedOptions` implémente `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="9914b-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="9914b-184">L’implémentation par défaut de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([source de référence](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) a une logique qui permet de les utiliser chacune de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="9914b-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="9914b-185">L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) et [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="9914b-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="9914b-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="9914b-186">IPostConfigureOptions</span></span>

<span data-ttu-id="9914b-187">*Nécessite ASP.NET Core 2.0 ou version ultérieure.*</span><span class="sxs-lookup"><span data-stu-id="9914b-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="9914b-188">Définir la post-configuration avec [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="9914b-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="9914b-189">La post-configuration s’exécute après la totalité de la configuration [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) :</span><span class="sxs-lookup"><span data-stu-id="9914b-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9914b-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) permet de post-configurer les options nommées :</span><span class="sxs-lookup"><span data-stu-id="9914b-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9914b-191">Utilisez [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) pour post-configurer toutes les instances de configuration nommées :</span><span class="sxs-lookup"><span data-stu-id="9914b-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="9914b-192">Fabrique d’options, surveillance et cache</span><span class="sxs-lookup"><span data-stu-id="9914b-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="9914b-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) est utilisé pour les notifications quand des instances `TOptions` changent.</span><span class="sxs-lookup"><span data-stu-id="9914b-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="9914b-194">`IOptionsMonitor` prend en charge les options rechargeables, les notifications de modifications et `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="9914b-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="9914b-195">La classe [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 ou version ultérieure) est chargée de créer les instances d’options.</span><span class="sxs-lookup"><span data-stu-id="9914b-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="9914b-196">Elle dispose d’une seule méthode ([Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)).</span><span class="sxs-lookup"><span data-stu-id="9914b-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="9914b-197">L’implémentation par défaut prend toutes les `IConfigureOptions` et `IPostConfigureOptions` inscrites et exécute toutes les configurations, puis les post-configurations.</span><span class="sxs-lookup"><span data-stu-id="9914b-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="9914b-198">Elle fait la distinction entre `IConfigureNamedOptions` et `IConfigureOptions` et n’appelle que l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="9914b-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="9914b-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 ou version ultérieure) est utilisé par `IOptionsMonitor` pour la mise en cache des instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="9914b-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="9914b-200">`IOptionsMonitorCache` invalide les instances des options dans le moniteur afin que la valeur soit recalculée ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="9914b-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="9914b-201">Les valeurs peuvent aussi être introduites manuellement avec [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="9914b-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="9914b-202">La méthode [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) est utilisée quand toutes les instances nommées doivent être recréées à la demande.</span><span class="sxs-lookup"><span data-stu-id="9914b-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="9914b-203">Accès aux options au démarrage</span><span class="sxs-lookup"><span data-stu-id="9914b-203">Accessing options during startup</span></span>

<span data-ttu-id="9914b-204">`IOptions` peut être utilisé dans `Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute.</span><span class="sxs-lookup"><span data-stu-id="9914b-204">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="9914b-205">Si un fournisseur de services est créé dans `ConfigureServices` pour accéder aux options, il ne contient aucune configuration d’options fournie après sa création.</span><span class="sxs-lookup"><span data-stu-id="9914b-205">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="9914b-206">Ainsi, un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="9914b-206">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="9914b-207">Les options étant généralement chargées à partir de la configuration, celle-ci peut être utilisée au démarrage à la fois dans `Configure` et `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9914b-207">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="9914b-208">Pour obtenir des exemples d’utilisation de la configuration au démarrage, consultez la rubrique [Démarrage de l’application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="9914b-208">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="9914b-209">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="9914b-209">See also</span></span>

* [<span data-ttu-id="9914b-210">Configuration</span><span class="sxs-lookup"><span data-stu-id="9914b-210">Configuration</span></span>](xref:fundamentals/configuration/index)
