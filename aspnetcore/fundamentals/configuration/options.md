---
title: "Modèle d’options dans ASP.NET Core"
author: guardrex
description: "Découvrez comment utiliser le modèle d’options pour représenter les groupes de paramètres associés dans les applications ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="3021e-103">Modèle d’options dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3021e-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="3021e-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3021e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3021e-105">Le modèle d’options utilise les classes d’options pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="3021e-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="3021e-106">Lorsque les paramètres de configuration sont isolées par fonctionnalité dans les classes d’options distinctes, l’application est conforme aux deux principes important de l’ingénierie logicielle :</span><span class="sxs-lookup"><span data-stu-id="3021e-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="3021e-107">Le [principe de répartition d’Interface (ISP)](http://deviq.com/interface-segregation-principle/): fonctionnalités (classes) qui dépendent des paramètres de configuration varient uniquement sur les paramètres de configuration qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="3021e-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="3021e-108">[Séparation des préoccupations](http://deviq.com/separation-of-concerns/): paramètres pour les différentes parties de l’application ne sont pas dépendants ou couplée à un autre.</span><span class="sxs-lookup"><span data-stu-id="3021e-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="3021e-109">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) cet article est plus facile à suivre avec l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="3021e-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="3021e-110">Configuration des options de base</span><span class="sxs-lookup"><span data-stu-id="3021e-110">Basic options configuration</span></span>

<span data-ttu-id="3021e-111">Configuration des options de base est présentée comme exemple &num;1 dans le [exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="3021e-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="3021e-112">Classe d’options doit être non abstrait avec un constructeur sans paramètre public.</span><span class="sxs-lookup"><span data-stu-id="3021e-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="3021e-113">La classe suivante, `MyOptions`, a deux propriétés, `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="3021e-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="3021e-114">Définition des valeurs par défaut est facultative, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="3021e-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="3021e-115">`Option2`a la valeur par défaut définie par l’initialisation de la propriété directement (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3021e-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="3021e-116">Le `MyOptions` classe est ajoutée au conteneur de service avec [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) et lié à la configuration :</span><span class="sxs-lookup"><span data-stu-id="3021e-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="3021e-117">La page suivante utilise des modèles [injection de dépendances constructeur](xref:fundamentals/dependency-injection#what-is-dependency-injection) avec [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) pour accéder aux paramètres (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="3021e-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="3021e-118">L’exemple *appsettings.json* fichier spécifie des valeurs pour `option1` et `option2`:</span><span class="sxs-lookup"><span data-stu-id="3021e-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="3021e-119">Lorsque l’application est exécutée, le modèle de page `OnGet` méthode retourne une chaîne indiquant les valeurs de la classe option :</span><span class="sxs-lookup"><span data-stu-id="3021e-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="3021e-120">Configurer les options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="3021e-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="3021e-121">Configuration des options simples avec un délégué est illustré comme exemple &num;2 dans le [exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="3021e-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="3021e-122">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="3021e-122">Use a delegate to set options values.</span></span> <span data-ttu-id="3021e-123">L’application d’exemple utilise le `MyOptionsWithDelegateConfig` classe (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3021e-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="3021e-124">Dans le code suivant, une seconde `IConfigureOptions<TOptions>` service est ajouté au conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="3021e-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="3021e-125">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="3021e-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="3021e-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3021e-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="3021e-127">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="3021e-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="3021e-128">Fournisseurs de configuration sont disponibles dans les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="3021e-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="3021e-129">Elles sont appliquées afin que ces dernières sont enregistrées.</span><span class="sxs-lookup"><span data-stu-id="3021e-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="3021e-130">Chaque appel à [configurer&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) ajoute un `IConfigureOptions<TOptions>` service au conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="3021e-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="3021e-131">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont tous deux spécifiés dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="3021e-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="3021e-132">Lorsque plusieurs services de configuration est activée, la dernière source de configuration spécifié *wins* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="3021e-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="3021e-133">Lorsque l’application est exécutée, le modèle de page `OnGet` méthode retourne une chaîne indiquant les valeurs de la classe option :</span><span class="sxs-lookup"><span data-stu-id="3021e-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="3021e-134">Configuration des sous-Options</span><span class="sxs-lookup"><span data-stu-id="3021e-134">Suboptions configuration</span></span>

<span data-ttu-id="3021e-135">Configuration des sous-Options est illustrée comme exemple &num;3 dans le [exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="3021e-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="3021e-136">Applications doivent créer des classes d’options qui se rapportent à des groupes de fonctionnalité spécifique (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="3021e-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="3021e-137">Les parties de l’application qui requièrent des valeurs de configuration doivent ont uniquement accès aux valeurs de configuration qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="3021e-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="3021e-138">Lors de la liaison des options de configuration, chaque propriété du type d’options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="3021e-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="3021e-139">Par exemple, le `MyOptions.Option1` propriété est liée à la clé `Option1`, qui est lu à partir du `option1` propriété dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3021e-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="3021e-140">Dans le code suivant, une troisième `IConfigureOptions<TOptions>` service est ajouté au conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="3021e-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="3021e-141">Il lie `MySubOptions` à la section `subsection` de la *appsettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="3021e-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="3021e-142">Le `GetSection` requiert de la méthode d’extension du [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="3021e-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="3021e-143">Si l’application utilise le [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, le package est automatiquement inclus.</span><span class="sxs-lookup"><span data-stu-id="3021e-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="3021e-144">L’exemple *appsettings.json* fichier définit un `subsection` membres avec des clés pour `suboption1` et `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="3021e-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="3021e-145">Le `MySubOptions` classe définit les propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs d’option sous-chemin (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3021e-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="3021e-146">Le modèle de page `OnGet` méthode retourne une chaîne avec les valeurs d’option sous-chemin (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3021e-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="3021e-147">Lorsque l’application est exécutée, la `OnGet` méthode retourne une chaîne indiquant la sous-option les valeurs de classe :</span><span class="sxs-lookup"><span data-stu-id="3021e-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="3021e-148">Options fournies par un modèle d’affichage ou avec l’injection de vue directe</span><span class="sxs-lookup"><span data-stu-id="3021e-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="3021e-149">Les options fournies par un modèle d’affichage ou avec l’injection de vue directe est illustré comme exemple &num;4 dans le [exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="3021e-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="3021e-150">Options peuvent être fournies dans un modèle d’affichage ou en injectant `IOptions<TOptions>` directement dans une vue (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3021e-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="3021e-151">Pour l’injection directe, injecter `IOptions<MyOptions>` avec un `@inject` la directive :</span><span class="sxs-lookup"><span data-stu-id="3021e-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="3021e-152">Lorsque l’application est exécutée, les valeurs d’option sont affichés dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="3021e-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Options de valeurs Option1 : value1_from_json et Option2 : -1 sont chargés à partir du modèle et par injection dans la vue.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="3021e-154">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="3021e-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="3021e-155">Recharger les données de configuration avec `IOptionsSnapshot` est illustré dans l’exemple &num;5 dans le [exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="3021e-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="3021e-156">*Nécessite ASP.NET Core 1.1 ou version ultérieure.*</span><span class="sxs-lookup"><span data-stu-id="3021e-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="3021e-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) prend en charge de rechargement des options avec une charge de traitement minimal.</span><span class="sxs-lookup"><span data-stu-id="3021e-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="3021e-158">Dans ASP.NET Core 1.1, `IOptionsSnapshot` est un instantané de [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) et mises à jour automatiquement chaque fois que le moniteur déclenche les modifications en fonction de la source de données modifiées.</span><span class="sxs-lookup"><span data-stu-id="3021e-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="3021e-159">Dans ASP.NET Core 2.0 et versions ultérieures, les options sont calculées une fois par demande lorsque accessibles et mis en cache pour la durée de vie de la demande.</span><span class="sxs-lookup"><span data-stu-id="3021e-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="3021e-160">L’exemple suivant montre comment un nouveau `IOptionsSnapshot` est créée après *appsettings.json* modifications (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="3021e-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="3021e-161">Plusieurs demandes au serveur de retournent des valeurs constantes fournies par le *appsettings.json* jusqu'à ce que le fichier est modifié et les rechargements de la configuration du fichier.</span><span class="sxs-lookup"><span data-stu-id="3021e-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="3021e-162">L’illustration suivante montre la première `option1` et `option2` valeurs chargées à partir de la *appsettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="3021e-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="3021e-163">Modifiez les valeurs dans le *appsettings.json* le fichier `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="3021e-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="3021e-164">Enregistrer le *appsettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="3021e-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="3021e-165">Actualisez le navigateur pour voir que les valeurs des options sont mis à jour :</span><span class="sxs-lookup"><span data-stu-id="3021e-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="3021e-166">Nommé prise en charge des options avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="3021e-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="3021e-167">Nommé prise en charge des options avec [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) est montré dans l’exemple &num;6 dans le [exemple d’application](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="3021e-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="3021e-168">*Nécessite un cœur d’ASP.NET 2.0 ou version ultérieure.*</span><span class="sxs-lookup"><span data-stu-id="3021e-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="3021e-169">*Nommé options* prise en charge permet à l’application de faire la distinction entre les configurations d’options nommée.</span><span class="sxs-lookup"><span data-stu-id="3021e-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="3021e-170">Dans l’exemple d’application, nommées options sont déclarées avec le [ConfigureNamedOptions&lt;TOptions&gt;. Configurer](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) méthode :</span><span class="sxs-lookup"><span data-stu-id="3021e-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="3021e-171">L’exemple d’application accède aux options nommées avec [IOptionsSnapshot&lt;TOptions&gt;. Obtenir](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3021e-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="3021e-172">L’exemple d’application en cours d’exécution, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="3021e-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="3021e-173">`named_options_1`les valeurs sont fournies à partir de la configuration, qui sont chargé à partir de la *appsettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="3021e-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="3021e-174">`named_options_2`les valeurs sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="3021e-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="3021e-175">Le `named_options_2` déléguer dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="3021e-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="3021e-176">La valeur par défaut `Option2` fournie par la `MyOptions` classe.</span><span class="sxs-lookup"><span data-stu-id="3021e-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="3021e-177">Configurez toutes les instances nommées d’options avec les [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) (méthode).</span><span class="sxs-lookup"><span data-stu-id="3021e-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="3021e-178">Le code suivant configure `Option1` pour toutes les instances de configuration avec une valeur commune nommées.</span><span class="sxs-lookup"><span data-stu-id="3021e-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="3021e-179">Ajoutez le code suivant manuellement en la `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="3021e-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="3021e-180">L’exemple d’application en cours d’exécution après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="3021e-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="3021e-181">Dans ASP.NET Core 2.0 et versions ultérieures, toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="3021e-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="3021e-182">Existant `IConfigureOption` instances sont traitées comme ciblant le `Options.DefaultName` instance, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="3021e-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="3021e-183">`IConfigureNamedOptions`implémente également `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="3021e-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="3021e-184">L’implémentation par défaut de la [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([source de référence](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) possède une logique à utiliser chacune de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="3021e-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="3021e-185">Le `null` option nommée est utilisée pour toutes les instances nommées spécifiques à une instance nommée au lieu de cibler ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) et [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="3021e-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="3021e-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="3021e-186">IPostConfigureOptions</span></span>

<span data-ttu-id="3021e-187">*Nécessite un cœur d’ASP.NET 2.0 ou version ultérieure.*</span><span class="sxs-lookup"><span data-stu-id="3021e-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="3021e-188">Définir postconfiguration avec [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="3021e-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="3021e-189">Postconfiguration s’exécute une fois toutes les [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration se produit :</span><span class="sxs-lookup"><span data-stu-id="3021e-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="3021e-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) est disponible pour configurer les options nommées après :</span><span class="sxs-lookup"><span data-stu-id="3021e-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="3021e-191">Utilisez [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) post-configurer tous les nommé instances de configuration :</span><span class="sxs-lookup"><span data-stu-id="3021e-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="3021e-192">Fabrique d’options, de surveillance et de cache</span><span class="sxs-lookup"><span data-stu-id="3021e-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="3021e-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) est utilisé pour les notifications lorsque `TOptions` modification des instances.</span><span class="sxs-lookup"><span data-stu-id="3021e-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="3021e-194">`IOptionsMonitor`prend en charge des options reloadable, notifications de modifications, et `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="3021e-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="3021e-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (noyaux d’ASP.NET 2.0 ou version ultérieure) est chargé pour la création de nouvelles options d’instances.</span><span class="sxs-lookup"><span data-stu-id="3021e-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="3021e-196">Il dispose d’un seul [créer](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) (méthode).</span><span class="sxs-lookup"><span data-stu-id="3021e-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="3021e-197">L’implémentation par défaut est enregistrée toutes les `IConfigureOptions` et `IPostConfigureOptions` et exécute tous les le configure en premier, suivi par le post-traitement configure.</span><span class="sxs-lookup"><span data-stu-id="3021e-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="3021e-198">Il fait la distinction entre `IConfigureNamedOptions` et `IConfigureOptions` et n’appelle l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="3021e-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="3021e-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (noyaux d’ASP.NET 2.0 ou version ultérieure) est utilisé par `IOptionsMonitor` au cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="3021e-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="3021e-200">Le `IOptionsMonitorCache` invalide les instances des options dans l’analyse afin que la valeur est recalculée ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="3021e-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="3021e-201">Les valeurs peuvent être manuellement introduites ainsi par [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="3021e-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="3021e-202">Le [clair](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) méthode est utilisée lorsque toutes les instances nommées doivent être recréés à la demande.</span><span class="sxs-lookup"><span data-stu-id="3021e-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="3021e-203">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="3021e-203">See also</span></span>

* [<span data-ttu-id="3021e-204">Configuration</span><span class="sxs-lookup"><span data-stu-id="3021e-204">Configuration</span></span>](xref:fundamentals/configuration/index)
