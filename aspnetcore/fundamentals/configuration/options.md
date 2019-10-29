---
title: Modèle d’options dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser le modèle d’options pour représenter des groupes de paramètres associés dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/28/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: f9e94e8d1736b7ffaa2640aba03da6b239a34f0a
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034012"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="ab521-103">Modèle d’options dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab521-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="ab521-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ab521-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ab521-105">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="ab521-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="ab521-106">Quand les [paramètres de configuration](xref:fundamentals/configuration/index) sont isolés par scénario dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :</span><span class="sxs-lookup"><span data-stu-id="ab521-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="ab521-107">[Principe de séparation des interfaces ou encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash;: les scénarios (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="ab521-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="ab521-108">[Séparation des préoccupations](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)&ndash; : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="ab521-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="ab521-109">Ces options fournissent également un mécanisme de validation des données de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="ab521-110">Pour plus d'informations, reportez-vous à la section [Validation des options](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="ab521-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="ab521-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab521-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="ab521-112">Package</span><span class="sxs-lookup"><span data-stu-id="ab521-112">Package</span></span>

<span data-ttu-id="ab521-113">Le package [Microsoft. extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) est implicitement référencé dans les applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="ab521-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="ab521-114">Interfaces d’options</span><span class="sxs-lookup"><span data-stu-id="ab521-114">Options interfaces</span></span>

<span data-ttu-id="ab521-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> permet de récupérer des options et de gérer les notifications d’options pour les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="ab521-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="ab521-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="ab521-117">Notifications de modifications</span><span class="sxs-lookup"><span data-stu-id="ab521-117">Change notifications</span></span>
* [<span data-ttu-id="ab521-118">Options nommées</span><span class="sxs-lookup"><span data-stu-id="ab521-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="ab521-119">Configuration rechargeable</span><span class="sxs-lookup"><span data-stu-id="ab521-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="ab521-120">Invalidation sélective des options (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="ab521-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="ab521-121">Des scénarios [post-configuration](#options-post-configuration) vous permettent de définir ou de modifier les options après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="ab521-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> est chargée de créer les instances d’options.</span><span class="sxs-lookup"><span data-stu-id="ab521-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="ab521-123">Elle dispose d’une seule méthode <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="ab521-124">L’implémentation par défaut prend toutes les <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> inscrites et exécute toutes les configurations, puis les post-configurations.</span><span class="sxs-lookup"><span data-stu-id="ab521-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="ab521-125">Elle fait la distinction entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et n’appelle que l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="ab521-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="ab521-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> est utilisée par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour mettre en cache les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="ab521-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalide les instances des options dans le moniteur afin que la valeur soit recalculée (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="ab521-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="ab521-128">Les valeurs peuvent aussi être introduites manuellement avec <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="ab521-129">La méthode <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> est utilisée quand toutes les instances nommées doivent être recréées à la demande.</span><span class="sxs-lookup"><span data-stu-id="ab521-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="ab521-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est utile dans les scénarios où des options doivent être recalculées à chaque requête.</span><span class="sxs-lookup"><span data-stu-id="ab521-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="ab521-131">Pour plus d’informations, consultez la section [Recharger les données de configuration avec IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="ab521-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="ab521-132"><xref:Microsoft.Extensions.Options.IOptions%601> peut être utilisée pour prendre en charge des options.</span><span class="sxs-lookup"><span data-stu-id="ab521-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="ab521-133">Toutefois, <xref:Microsoft.Extensions.Options.IOptions%601> ne prend pas en charge les scénarios <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> précédents.</span><span class="sxs-lookup"><span data-stu-id="ab521-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="ab521-134">Vous pouvez continuer à utiliser <xref:Microsoft.Extensions.Options.IOptions%601> dans des infrastructures et bibliothèques existantes qui utilisent déjà l’interface <xref:Microsoft.Extensions.Options.IOptions%601> mais ne nécessitent pas les scénarios fournis par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="ab521-135">Configuration des options générales</span><span class="sxs-lookup"><span data-stu-id="ab521-135">General options configuration</span></span>

<span data-ttu-id="ab521-136">La configuration des options générales est illustrée dans l’exemple &num;1 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="ab521-137">Une classe d’options doit être non abstraite avec un constructeur public sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="ab521-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ab521-138">La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="ab521-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="ab521-139">Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="ab521-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="ab521-140">`Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="ab521-141">La classe `MyOptions` est ajoutée au conteneur de service avec <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> et liée à la configuration :</span><span class="sxs-lookup"><span data-stu-id="ab521-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="ab521-142">Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:mvc/controllers/dependency-injection) avec <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="ab521-143">Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :</span><span class="sxs-lookup"><span data-stu-id="ab521-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="ab521-144">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="ab521-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="ab521-145">Quand vous utilisez un <xref:System.Configuration.ConfigurationBuilder> personnalisé pour charger une configuration d’options à partir d’un fichier de paramètres, vérifiez que le chemin de base est correctement défini :</span><span class="sxs-lookup"><span data-stu-id="ab521-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="ab521-146">Vous n’avez pas besoin de définir explicitement le chemin de base quand vous chargez une configuration d’options à partir du fichier de paramètres via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="ab521-147">Configurer des options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="ab521-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="ab521-148">La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="ab521-149">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="ab521-149">Use a delegate to set options values.</span></span> <span data-ttu-id="ab521-150">L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="ab521-151">Dans le code suivant, un second service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ab521-152">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :</span><span class="sxs-lookup"><span data-stu-id="ab521-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="ab521-153">*Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="ab521-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="ab521-154">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="ab521-155">Des fournisseurs de configuration sont disponibles à partir de packages NuGet et sont appliqués dans l’ordre de leur inscription.</span><span class="sxs-lookup"><span data-stu-id="ab521-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="ab521-156">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ab521-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="ab521-157">Chaque appel à <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> ajoute un service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="ab521-158">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="ab521-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="ab521-159">Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="ab521-160">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="ab521-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="ab521-161">Configuration des sous-options</span><span class="sxs-lookup"><span data-stu-id="ab521-161">Suboptions configuration</span></span>

<span data-ttu-id="ab521-162">La configuration des sous-options est illustrée dans l’exemple &num;3 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="ab521-163">Les applications doivent créer des classes d’options qui appartiennent à des groupes de scénarios spécifiques (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="ab521-164">Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="ab521-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="ab521-165">Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="ab521-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ab521-166">Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="ab521-167">Dans le code suivant, un troisième service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ab521-168">Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="ab521-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="ab521-169">La méthode `GetSection` requiert l’espace de noms <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="ab521-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="ab521-170">Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :</span><span class="sxs-lookup"><span data-stu-id="ab521-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="ab521-171">La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs des options (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ab521-172">La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs des options (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="ab521-173">Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :</span><span class="sxs-lookup"><span data-stu-id="ab521-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="ab521-174">Options fournies par un modèle d’affichage ou avec une injection de vue directe</span><span class="sxs-lookup"><span data-stu-id="ab521-174">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="ab521-175">Les options fournies par un modèle d’affichage ou avec une injection de vue directe sont illustrées dans l’exemple &num;4 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-175">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="ab521-176">Vous pouvez fournir les options dans un modèle d’affichage ou en injectant <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directement dans une vue (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-176">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="ab521-177">L’exemple d’application montre comment injecter `IOptionsMonitor<MyOptions>` avec une directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="ab521-177">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="ab521-178">Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="ab521-178">When the app is run, the options values are shown in the rendered page:</span></span>

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="ab521-180">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="ab521-180">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="ab521-181">Le rechargement des données de configuration avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est illustré dans l’exemple &num;5 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-181">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="ab521-182"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> prend en charge le rechargement des options avec une charge de traitement minimale.</span><span class="sxs-lookup"><span data-stu-id="ab521-182"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="ab521-183">Les options sont calculées une fois par requête quand le système y accède et sont mises en cache pour toute la durée de vie de la requête.</span><span class="sxs-lookup"><span data-stu-id="ab521-183">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="ab521-184">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="ab521-184">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="ab521-185">Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.</span><span class="sxs-lookup"><span data-stu-id="ab521-185">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="ab521-186">L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="ab521-186">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="ab521-187">Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="ab521-187">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="ab521-188">Enregistrez le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-188">Save the *appsettings.json* file.</span></span> <span data-ttu-id="ab521-189">Actualisez le navigateur pour constater la mise à jour des valeurs des options :</span><span class="sxs-lookup"><span data-stu-id="ab521-189">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="ab521-190">Prise en charge des options nommées avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="ab521-190">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="ab521-191">La prise en charge des options nommées avec <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> est illustrée dans l’exemple &num;6 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-191">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="ab521-192">La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées.</span><span class="sxs-lookup"><span data-stu-id="ab521-192">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="ab521-193">Dans l’exemple d’application, les options nommées sont déclarées avec [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), qui appelle la méthode d’extension [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-193">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="ab521-194">L’exemple d’application accède aux options nommées avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-194">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="ab521-195">Quand l’exemple d’application est exécuté, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="ab521-195">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="ab521-196">Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-196">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="ab521-197">Les valeurs `named_options_2` sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="ab521-197">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="ab521-198">Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="ab521-198">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="ab521-199">La valeur par défaut `Option2` fournie par la classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-199">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="ab521-200">Configurer toutes les options avec la méthode ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="ab521-200">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="ab521-201">Configurez toutes les instances d’options avec la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-201">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="ab521-202">Le code suivant configure `Option1` pour toutes les instances de configuration ayant une valeur commune.</span><span class="sxs-lookup"><span data-stu-id="ab521-202">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="ab521-203">Ajoutez le code suivant manuellement à la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="ab521-203">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="ab521-204">Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="ab521-204">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="ab521-205">Toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="ab521-205">All options are named instances.</span></span> <span data-ttu-id="ab521-206">Les instances <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="ab521-206">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="ab521-207">En outre, <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-207"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="ab521-208">L’implémentation par défaut de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> possède une logique qui utilise chaque élément de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="ab521-208">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="ab521-209">L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> et <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="ab521-209">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="ab521-210">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="ab521-210">OptionsBuilder API</span></span>

<span data-ttu-id="ab521-211"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> permet de configurer des instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-211"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="ab521-212">`OptionsBuilder` simplifie la création d’options nommées. En effet, il est le seul paramètre de l’appel `AddOptions<TOptions>(string optionsName)` initial et n’apparaît pas dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="ab521-212">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="ab521-213">La validation des options et les surcharges `ConfigureOptions` qui acceptent des dépendances de service sont uniquement disponibles avec `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ab521-213">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="ab521-214">Utiliser les services d’injection de dépendances (DI) pour configurer des options</span><span class="sxs-lookup"><span data-stu-id="ab521-214">Use DI services to configure options</span></span>

<span data-ttu-id="ab521-215">Vous pouvez accéder à d’autres services à partir de l’injection de dépendances pendant que vous configurez des options de deux manières différentes :</span><span class="sxs-lookup"><span data-stu-id="ab521-215">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="ab521-216">Transmettez un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) sur [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="ab521-216">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="ab521-217">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) fournit des surcharges de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) qui vous permettent d’utiliser jusqu’à cinq services pour configurer des options :</span><span class="sxs-lookup"><span data-stu-id="ab521-217">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="ab521-218">Créez votre propre type qui implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ou <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et inscrit le type en tant que service.</span><span class="sxs-lookup"><span data-stu-id="ab521-218">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="ab521-219">Nous vous recommandons de transmettre un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), car il est plus complexe de créer un service.</span><span class="sxs-lookup"><span data-stu-id="ab521-219">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="ab521-220">Créer votre propre type équivaut à ce que fait automatiquement le framework quand vous utilisez [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="ab521-220">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="ab521-221">L’appel de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) a pour effet d’inscrire une instance générique temporaire de <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, dont l’un des constructeurs accepte les types de service génériques spécifiés.</span><span class="sxs-lookup"><span data-stu-id="ab521-221">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="ab521-222">Validation des options</span><span class="sxs-lookup"><span data-stu-id="ab521-222">Options validation</span></span>

<span data-ttu-id="ab521-223">La validation des options vous permet de valider les options lorsque celles-ci sont configurées.</span><span class="sxs-lookup"><span data-stu-id="ab521-223">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="ab521-224">Appelez `Validate` avec une méthode de validation qui retourne `true` si les options sont valides et `false` si elles ne sont pas valides :</span><span class="sxs-lookup"><span data-stu-id="ab521-224">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="ab521-225">L’exemple précédent définit l’instance d’options nommée sur `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="ab521-225">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="ab521-226">L'instance d’options par défaut est `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="ab521-226">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="ab521-227">La validation s’exécute lorsque l’instance d’options est créée.</span><span class="sxs-lookup"><span data-stu-id="ab521-227">Validation runs when the options instance is created.</span></span> <span data-ttu-id="ab521-228">Votre instance d’options est garantie de réussir la validation lors du premier accès.</span><span class="sxs-lookup"><span data-stu-id="ab521-228">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab521-229">La validation des options ne protège pas contre les modifications des options une fois que les options sont initialement configurées et validées.</span><span class="sxs-lookup"><span data-stu-id="ab521-229">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="ab521-230">La méthode `Validate` accepte un `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="ab521-230">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="ab521-231">Pour personnaliser entièrement la validation, implémentez `IValidateOptions<TOptions>`, ce qui permet :</span><span class="sxs-lookup"><span data-stu-id="ab521-231">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="ab521-232">Validation de plusieurs types d’options : `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="ab521-232">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="ab521-233">Validation qui dépend d’un autre type d’option : `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="ab521-233">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="ab521-234">`IValidateOptions` valide :</span><span class="sxs-lookup"><span data-stu-id="ab521-234">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="ab521-235">Une instance d’options nommée spécifique.</span><span class="sxs-lookup"><span data-stu-id="ab521-235">A specific named options instance.</span></span>
* <span data-ttu-id="ab521-236">Toutes les options quand `name` est `null`.</span><span class="sxs-lookup"><span data-stu-id="ab521-236">All options when `name` is `null`.</span></span>

<span data-ttu-id="ab521-237">Retourne `ValidateOptionsResult` à partir de votre implémentation de l’interface :</span><span class="sxs-lookup"><span data-stu-id="ab521-237">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="ab521-238">La validation basée sur l’annotation de données est disponible dans le package [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations). Pour cela, appelez la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> sur `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="ab521-238">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="ab521-239">`Microsoft.Extensions.Options.DataAnnotations` est implicitement référencé dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ab521-239">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.Extensions.DependencyInjection;

private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="ab521-240">La validation hâtive (échec rapide au démarrage) est à l’étude pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="ab521-240">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="ab521-241">Options de post-configuration</span><span class="sxs-lookup"><span data-stu-id="ab521-241">Options post-configuration</span></span>

<span data-ttu-id="ab521-242">Définissez la post-configuration avec <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-242">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="ab521-243">La post-configuration s’exécute après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> :</span><span class="sxs-lookup"><span data-stu-id="ab521-243">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ab521-244"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> permet de post-configurer les options nommées :</span><span class="sxs-lookup"><span data-stu-id="ab521-244"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ab521-245">Utilisez <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> pour post-configurer toutes les instances de configuration :</span><span class="sxs-lookup"><span data-stu-id="ab521-245">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="ab521-246">Accès aux options au démarrage</span><span class="sxs-lookup"><span data-stu-id="ab521-246">Accessing options during startup</span></span>

<span data-ttu-id="ab521-247"><xref:Microsoft.Extensions.Options.IOptions%601> et <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> peuvent être utilisées dans `Startup.Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute.</span><span class="sxs-lookup"><span data-stu-id="ab521-247"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="ab521-248">N’utilisez pas <xref:Microsoft.Extensions.Options.IOptions%601> ou <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ab521-248">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ab521-249">Un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="ab521-249">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ab521-250">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="ab521-250">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="ab521-251">Quand les [paramètres de configuration](xref:fundamentals/configuration/index) sont isolés par scénario dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :</span><span class="sxs-lookup"><span data-stu-id="ab521-251">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="ab521-252">[Principe de séparation des interfaces ou encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash;: les scénarios (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="ab521-252">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="ab521-253">[Séparation des préoccupations](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)&ndash; : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="ab521-253">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="ab521-254">Ces options fournissent également un mécanisme de validation des données de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-254">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="ab521-255">Pour plus d'informations, reportez-vous à la section [Validation des options](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="ab521-255">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="ab521-256">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab521-256">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab521-257">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="ab521-257">Prerequisites</span></span>

<span data-ttu-id="ab521-258">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="ab521-258">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="ab521-259">Interfaces d’options</span><span class="sxs-lookup"><span data-stu-id="ab521-259">Options interfaces</span></span>

<span data-ttu-id="ab521-260"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> permet de récupérer des options et de gérer les notifications d’options pour les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-260"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="ab521-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="ab521-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="ab521-262">Notifications de modifications</span><span class="sxs-lookup"><span data-stu-id="ab521-262">Change notifications</span></span>
* [<span data-ttu-id="ab521-263">Options nommées</span><span class="sxs-lookup"><span data-stu-id="ab521-263">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="ab521-264">Configuration rechargeable</span><span class="sxs-lookup"><span data-stu-id="ab521-264">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="ab521-265">Invalidation sélective des options (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="ab521-265">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="ab521-266">Des scénarios [post-configuration](#options-post-configuration) vous permettent de définir ou de modifier les options après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-266">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="ab521-267"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> est chargée de créer les instances d’options.</span><span class="sxs-lookup"><span data-stu-id="ab521-267"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="ab521-268">Elle dispose d’une seule méthode <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-268">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="ab521-269">L’implémentation par défaut prend toutes les <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> inscrites et exécute toutes les configurations, puis les post-configurations.</span><span class="sxs-lookup"><span data-stu-id="ab521-269">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="ab521-270">Elle fait la distinction entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et n’appelle que l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="ab521-270">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="ab521-271"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> est utilisée par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour mettre en cache les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-271"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="ab521-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalide les instances des options dans le moniteur afin que la valeur soit recalculée (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="ab521-272">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="ab521-273">Les valeurs peuvent aussi être introduites manuellement avec <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-273">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="ab521-274">La méthode <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> est utilisée quand toutes les instances nommées doivent être recréées à la demande.</span><span class="sxs-lookup"><span data-stu-id="ab521-274">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="ab521-275"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est utile dans les scénarios où des options doivent être recalculées à chaque requête.</span><span class="sxs-lookup"><span data-stu-id="ab521-275"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="ab521-276">Pour plus d’informations, consultez la section [Recharger les données de configuration avec IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="ab521-276">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="ab521-277"><xref:Microsoft.Extensions.Options.IOptions%601> peut être utilisée pour prendre en charge des options.</span><span class="sxs-lookup"><span data-stu-id="ab521-277"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="ab521-278">Toutefois, <xref:Microsoft.Extensions.Options.IOptions%601> ne prend pas en charge les scénarios <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> précédents.</span><span class="sxs-lookup"><span data-stu-id="ab521-278">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="ab521-279">Vous pouvez continuer à utiliser <xref:Microsoft.Extensions.Options.IOptions%601> dans des infrastructures et bibliothèques existantes qui utilisent déjà l’interface <xref:Microsoft.Extensions.Options.IOptions%601> mais ne nécessitent pas les scénarios fournis par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-279">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="ab521-280">Configuration des options générales</span><span class="sxs-lookup"><span data-stu-id="ab521-280">General options configuration</span></span>

<span data-ttu-id="ab521-281">La configuration des options générales est illustrée dans l’exemple &num;1 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-281">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="ab521-282">Une classe d’options doit être non abstraite avec un constructeur public sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="ab521-282">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ab521-283">La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="ab521-283">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="ab521-284">Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="ab521-284">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="ab521-285">`Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-285">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="ab521-286">La classe `MyOptions` est ajoutée au conteneur de service avec <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> et liée à la configuration :</span><span class="sxs-lookup"><span data-stu-id="ab521-286">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="ab521-287">Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:mvc/controllers/dependency-injection) avec <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-287">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="ab521-288">Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :</span><span class="sxs-lookup"><span data-stu-id="ab521-288">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="ab521-289">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="ab521-289">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="ab521-290">Quand vous utilisez un <xref:System.Configuration.ConfigurationBuilder> personnalisé pour charger une configuration d’options à partir d’un fichier de paramètres, vérifiez que le chemin de base est correctement défini :</span><span class="sxs-lookup"><span data-stu-id="ab521-290">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="ab521-291">Vous n’avez pas besoin de définir explicitement le chemin de base quand vous chargez une configuration d’options à partir du fichier de paramètres via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-291">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="ab521-292">Configurer des options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="ab521-292">Configure simple options with a delegate</span></span>

<span data-ttu-id="ab521-293">La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-293">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="ab521-294">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="ab521-294">Use a delegate to set options values.</span></span> <span data-ttu-id="ab521-295">L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-295">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="ab521-296">Dans le code suivant, un second service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-296">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ab521-297">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :</span><span class="sxs-lookup"><span data-stu-id="ab521-297">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="ab521-298">*Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="ab521-298">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="ab521-299">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-299">You can add multiple configuration providers.</span></span> <span data-ttu-id="ab521-300">Des fournisseurs de configuration sont disponibles à partir de packages NuGet et sont appliqués dans l’ordre de leur inscription.</span><span class="sxs-lookup"><span data-stu-id="ab521-300">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="ab521-301">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ab521-301">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="ab521-302">Chaque appel à <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> ajoute un service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-302">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="ab521-303">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="ab521-303">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="ab521-304">Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-304">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="ab521-305">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="ab521-305">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="ab521-306">Configuration des sous-options</span><span class="sxs-lookup"><span data-stu-id="ab521-306">Suboptions configuration</span></span>

<span data-ttu-id="ab521-307">La configuration des sous-options est illustrée dans l’exemple &num;3 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-307">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="ab521-308">Les applications doivent créer des classes d’options qui appartiennent à des groupes de scénarios spécifiques (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-308">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="ab521-309">Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="ab521-309">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="ab521-310">Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="ab521-310">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ab521-311">Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-311">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="ab521-312">Dans le code suivant, un troisième service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-312">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ab521-313">Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="ab521-313">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="ab521-314">La méthode `GetSection` requiert l’espace de noms <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="ab521-314">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="ab521-315">Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :</span><span class="sxs-lookup"><span data-stu-id="ab521-315">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="ab521-316">La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs des options (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-316">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ab521-317">La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs des options (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-317">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="ab521-318">Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :</span><span class="sxs-lookup"><span data-stu-id="ab521-318">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="ab521-319">Options fournies par un modèle d’affichage ou avec une injection de vue directe</span><span class="sxs-lookup"><span data-stu-id="ab521-319">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="ab521-320">Les options fournies par un modèle d’affichage ou avec une injection de vue directe sont illustrées dans l’exemple &num;4 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-320">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="ab521-321">Vous pouvez fournir les options dans un modèle d’affichage ou en injectant <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directement dans une vue (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-321">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="ab521-322">L’exemple d’application montre comment injecter `IOptionsMonitor<MyOptions>` avec une directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="ab521-322">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="ab521-323">Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="ab521-323">When the app is run, the options values are shown in the rendered page:</span></span>

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="ab521-325">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="ab521-325">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="ab521-326">Le rechargement des données de configuration avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est illustré dans l’exemple &num;5 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-326">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="ab521-327"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> prend en charge le rechargement des options avec une charge de traitement minimale.</span><span class="sxs-lookup"><span data-stu-id="ab521-327"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="ab521-328">Les options sont calculées une fois par requête quand le système y accède et sont mises en cache pour toute la durée de vie de la requête.</span><span class="sxs-lookup"><span data-stu-id="ab521-328">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="ab521-329">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="ab521-329">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="ab521-330">Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.</span><span class="sxs-lookup"><span data-stu-id="ab521-330">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="ab521-331">L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="ab521-331">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="ab521-332">Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="ab521-332">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="ab521-333">Enregistrez le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-333">Save the *appsettings.json* file.</span></span> <span data-ttu-id="ab521-334">Actualisez le navigateur pour constater la mise à jour des valeurs des options :</span><span class="sxs-lookup"><span data-stu-id="ab521-334">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="ab521-335">Prise en charge des options nommées avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="ab521-335">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="ab521-336">La prise en charge des options nommées avec <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> est illustrée dans l’exemple &num;6 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-336">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="ab521-337">La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées.</span><span class="sxs-lookup"><span data-stu-id="ab521-337">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="ab521-338">Dans l’exemple d’application, les options nommées sont déclarées avec [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), qui appelle la méthode d’extension [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-338">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="ab521-339">L’exemple d’application accède aux options nommées avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-339">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="ab521-340">Quand l’exemple d’application est exécuté, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="ab521-340">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="ab521-341">Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-341">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="ab521-342">Les valeurs `named_options_2` sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="ab521-342">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="ab521-343">Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="ab521-343">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="ab521-344">La valeur par défaut `Option2` fournie par la classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-344">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="ab521-345">Configurer toutes les options avec la méthode ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="ab521-345">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="ab521-346">Configurez toutes les instances d’options avec la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-346">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="ab521-347">Le code suivant configure `Option1` pour toutes les instances de configuration ayant une valeur commune.</span><span class="sxs-lookup"><span data-stu-id="ab521-347">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="ab521-348">Ajoutez le code suivant manuellement à la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="ab521-348">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="ab521-349">Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="ab521-349">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="ab521-350">Toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="ab521-350">All options are named instances.</span></span> <span data-ttu-id="ab521-351">Les instances <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="ab521-351">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="ab521-352">En outre, <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-352"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="ab521-353">L’implémentation par défaut de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> possède une logique qui utilise chaque élément de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="ab521-353">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="ab521-354">L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> et <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="ab521-354">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="ab521-355">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="ab521-355">OptionsBuilder API</span></span>

<span data-ttu-id="ab521-356"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> permet de configurer des instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-356"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="ab521-357">`OptionsBuilder` simplifie la création d’options nommées. En effet, il est le seul paramètre de l’appel `AddOptions<TOptions>(string optionsName)` initial et n’apparaît pas dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="ab521-357">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="ab521-358">La validation des options et les surcharges `ConfigureOptions` qui acceptent des dépendances de service sont uniquement disponibles avec `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ab521-358">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="ab521-359">Utiliser les services d’injection de dépendances (DI) pour configurer des options</span><span class="sxs-lookup"><span data-stu-id="ab521-359">Use DI services to configure options</span></span>

<span data-ttu-id="ab521-360">Vous pouvez accéder à d’autres services à partir de l’injection de dépendances pendant que vous configurez des options de deux manières différentes :</span><span class="sxs-lookup"><span data-stu-id="ab521-360">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="ab521-361">Transmettez un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) sur [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="ab521-361">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="ab521-362">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) fournit des surcharges de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) qui vous permettent d’utiliser jusqu’à cinq services pour configurer des options :</span><span class="sxs-lookup"><span data-stu-id="ab521-362">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="ab521-363">Créez votre propre type qui implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ou <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et inscrit le type en tant que service.</span><span class="sxs-lookup"><span data-stu-id="ab521-363">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="ab521-364">Nous vous recommandons de transmettre un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), car il est plus complexe de créer un service.</span><span class="sxs-lookup"><span data-stu-id="ab521-364">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="ab521-365">Créer votre propre type équivaut à ce que fait automatiquement le framework quand vous utilisez [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="ab521-365">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="ab521-366">L’appel de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) a pour effet d’inscrire une instance générique temporaire de <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, dont l’un des constructeurs accepte les types de service génériques spécifiés.</span><span class="sxs-lookup"><span data-stu-id="ab521-366">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="ab521-367">Validation des options</span><span class="sxs-lookup"><span data-stu-id="ab521-367">Options validation</span></span>

<span data-ttu-id="ab521-368">La validation des options vous permet de valider les options lorsque celles-ci sont configurées.</span><span class="sxs-lookup"><span data-stu-id="ab521-368">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="ab521-369">Appelez `Validate` avec une méthode de validation qui retourne `true` si les options sont valides et `false` si elles ne sont pas valides :</span><span class="sxs-lookup"><span data-stu-id="ab521-369">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="ab521-370">L’exemple précédent définit l’instance d’options nommée sur `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="ab521-370">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="ab521-371">L'instance d’options par défaut est `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="ab521-371">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="ab521-372">La validation s’exécute lorsque l’instance d’options est créée.</span><span class="sxs-lookup"><span data-stu-id="ab521-372">Validation runs when the options instance is created.</span></span> <span data-ttu-id="ab521-373">Votre instance d’options est garantie de réussir la validation lors du premier accès.</span><span class="sxs-lookup"><span data-stu-id="ab521-373">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab521-374">La validation des options ne protège pas contre les modifications des options une fois que les options sont initialement configurées et validées.</span><span class="sxs-lookup"><span data-stu-id="ab521-374">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="ab521-375">La méthode `Validate` accepte un `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="ab521-375">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="ab521-376">Pour personnaliser entièrement la validation, implémentez `IValidateOptions<TOptions>`, ce qui permet :</span><span class="sxs-lookup"><span data-stu-id="ab521-376">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="ab521-377">Validation de plusieurs types d’options : `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="ab521-377">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="ab521-378">Validation qui dépend d’un autre type d’option : `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="ab521-378">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="ab521-379">`IValidateOptions` valide :</span><span class="sxs-lookup"><span data-stu-id="ab521-379">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="ab521-380">Une instance d’options nommée spécifique.</span><span class="sxs-lookup"><span data-stu-id="ab521-380">A specific named options instance.</span></span>
* <span data-ttu-id="ab521-381">Toutes les options quand `name` est `null`.</span><span class="sxs-lookup"><span data-stu-id="ab521-381">All options when `name` is `null`.</span></span>

<span data-ttu-id="ab521-382">Retourne `ValidateOptionsResult` à partir de votre implémentation de l’interface :</span><span class="sxs-lookup"><span data-stu-id="ab521-382">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="ab521-383">La validation basée sur l’annotation de données est disponible dans le package [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations). Pour cela, appelez la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> sur `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="ab521-383">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="ab521-384">`Microsoft.Extensions.Options.DataAnnotations` est inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ab521-384">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="ab521-385">La validation hâtive (échec rapide au démarrage) est à l’étude pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="ab521-385">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="ab521-386">Options de post-configuration</span><span class="sxs-lookup"><span data-stu-id="ab521-386">Options post-configuration</span></span>

<span data-ttu-id="ab521-387">Définissez la post-configuration avec <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-387">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="ab521-388">La post-configuration s’exécute après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> :</span><span class="sxs-lookup"><span data-stu-id="ab521-388">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ab521-389"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> permet de post-configurer les options nommées :</span><span class="sxs-lookup"><span data-stu-id="ab521-389"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ab521-390">Utilisez <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> pour post-configurer toutes les instances de configuration :</span><span class="sxs-lookup"><span data-stu-id="ab521-390">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="ab521-391">Accès aux options au démarrage</span><span class="sxs-lookup"><span data-stu-id="ab521-391">Accessing options during startup</span></span>

<span data-ttu-id="ab521-392"><xref:Microsoft.Extensions.Options.IOptions%601> et <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> peuvent être utilisées dans `Startup.Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute.</span><span class="sxs-lookup"><span data-stu-id="ab521-392"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="ab521-393">N’utilisez pas <xref:Microsoft.Extensions.Options.IOptions%601> ou <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ab521-393">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ab521-394">Un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="ab521-394">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="ab521-395">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="ab521-395">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="ab521-396">Quand les [paramètres de configuration](xref:fundamentals/configuration/index) sont isolés par scénario dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :</span><span class="sxs-lookup"><span data-stu-id="ab521-396">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="ab521-397">[Principe de séparation des interfaces ou encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash;: les scénarios (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="ab521-397">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="ab521-398">[Séparation des préoccupations](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)&ndash; : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="ab521-398">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="ab521-399">Ces options fournissent également un mécanisme de validation des données de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-399">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="ab521-400">Pour plus d'informations, reportez-vous à la section [Validation des options](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="ab521-400">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="ab521-401">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab521-401">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab521-402">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="ab521-402">Prerequisites</span></span>

<span data-ttu-id="ab521-403">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="ab521-403">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="ab521-404">Interfaces d’options</span><span class="sxs-lookup"><span data-stu-id="ab521-404">Options interfaces</span></span>

<span data-ttu-id="ab521-405"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> permet de récupérer des options et de gérer les notifications d’options pour les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-405"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="ab521-406"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="ab521-406"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="ab521-407">Notifications de modifications</span><span class="sxs-lookup"><span data-stu-id="ab521-407">Change notifications</span></span>
* [<span data-ttu-id="ab521-408">Options nommées</span><span class="sxs-lookup"><span data-stu-id="ab521-408">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="ab521-409">Configuration rechargeable</span><span class="sxs-lookup"><span data-stu-id="ab521-409">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="ab521-410">Invalidation sélective des options (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="ab521-410">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="ab521-411">Des scénarios [post-configuration](#options-post-configuration) vous permettent de définir ou de modifier les options après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-411">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="ab521-412"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> est chargée de créer les instances d’options.</span><span class="sxs-lookup"><span data-stu-id="ab521-412"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="ab521-413">Elle dispose d’une seule méthode <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-413">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="ab521-414">L’implémentation par défaut prend toutes les <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> inscrites et exécute toutes les configurations, puis les post-configurations.</span><span class="sxs-lookup"><span data-stu-id="ab521-414">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="ab521-415">Elle fait la distinction entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et n’appelle que l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="ab521-415">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="ab521-416"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> est utilisée par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour mettre en cache les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-416"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="ab521-417"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalide les instances des options dans le moniteur afin que la valeur soit recalculée (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="ab521-417">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="ab521-418">Les valeurs peuvent aussi être introduites manuellement avec <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-418">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="ab521-419">La méthode <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> est utilisée quand toutes les instances nommées doivent être recréées à la demande.</span><span class="sxs-lookup"><span data-stu-id="ab521-419">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="ab521-420"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est utile dans les scénarios où des options doivent être recalculées à chaque requête.</span><span class="sxs-lookup"><span data-stu-id="ab521-420"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="ab521-421">Pour plus d’informations, consultez la section [Recharger les données de configuration avec IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="ab521-421">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="ab521-422"><xref:Microsoft.Extensions.Options.IOptions%601> peut être utilisée pour prendre en charge des options.</span><span class="sxs-lookup"><span data-stu-id="ab521-422"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="ab521-423">Toutefois, <xref:Microsoft.Extensions.Options.IOptions%601> ne prend pas en charge les scénarios <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> précédents.</span><span class="sxs-lookup"><span data-stu-id="ab521-423">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="ab521-424">Vous pouvez continuer à utiliser <xref:Microsoft.Extensions.Options.IOptions%601> dans des infrastructures et bibliothèques existantes qui utilisent déjà l’interface <xref:Microsoft.Extensions.Options.IOptions%601> mais ne nécessitent pas les scénarios fournis par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-424">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="ab521-425">Configuration des options générales</span><span class="sxs-lookup"><span data-stu-id="ab521-425">General options configuration</span></span>

<span data-ttu-id="ab521-426">La configuration des options générales est illustrée dans l’exemple &num;1 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-426">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="ab521-427">Une classe d’options doit être non abstraite avec un constructeur public sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="ab521-427">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ab521-428">La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="ab521-428">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="ab521-429">Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="ab521-429">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="ab521-430">`Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-430">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="ab521-431">La classe `MyOptions` est ajoutée au conteneur de service avec <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> et liée à la configuration :</span><span class="sxs-lookup"><span data-stu-id="ab521-431">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="ab521-432">Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:mvc/controllers/dependency-injection) avec <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-432">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="ab521-433">Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :</span><span class="sxs-lookup"><span data-stu-id="ab521-433">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="ab521-434">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="ab521-434">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="ab521-435">Quand vous utilisez un <xref:System.Configuration.ConfigurationBuilder> personnalisé pour charger une configuration d’options à partir d’un fichier de paramètres, vérifiez que le chemin de base est correctement défini :</span><span class="sxs-lookup"><span data-stu-id="ab521-435">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="ab521-436">Vous n’avez pas besoin de définir explicitement le chemin de base quand vous chargez une configuration d’options à partir du fichier de paramètres via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-436">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="ab521-437">Configurer des options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="ab521-437">Configure simple options with a delegate</span></span>

<span data-ttu-id="ab521-438">La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-438">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="ab521-439">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="ab521-439">Use a delegate to set options values.</span></span> <span data-ttu-id="ab521-440">L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-440">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="ab521-441">Dans le code suivant, un second service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-441">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ab521-442">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :</span><span class="sxs-lookup"><span data-stu-id="ab521-442">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="ab521-443">*Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="ab521-443">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="ab521-444">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-444">You can add multiple configuration providers.</span></span> <span data-ttu-id="ab521-445">Des fournisseurs de configuration sont disponibles à partir de packages NuGet et sont appliqués dans l’ordre de leur inscription.</span><span class="sxs-lookup"><span data-stu-id="ab521-445">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="ab521-446">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ab521-446">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="ab521-447">Chaque appel à <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> ajoute un service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-447">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="ab521-448">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="ab521-448">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="ab521-449">Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="ab521-449">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="ab521-450">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="ab521-450">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="ab521-451">Configuration des sous-options</span><span class="sxs-lookup"><span data-stu-id="ab521-451">Suboptions configuration</span></span>

<span data-ttu-id="ab521-452">La configuration des sous-options est illustrée dans l’exemple &num;3 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-452">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="ab521-453">Les applications doivent créer des classes d’options qui appartiennent à des groupes de scénarios spécifiques (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-453">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="ab521-454">Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="ab521-454">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="ab521-455">Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="ab521-455">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ab521-456">Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-456">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="ab521-457">Dans le code suivant, un troisième service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="ab521-457">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ab521-458">Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="ab521-458">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="ab521-459">La méthode `GetSection` requiert l’espace de noms <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="ab521-459">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="ab521-460">Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :</span><span class="sxs-lookup"><span data-stu-id="ab521-460">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="ab521-461">La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs des options (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-461">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ab521-462">La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs des options (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-462">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="ab521-463">Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :</span><span class="sxs-lookup"><span data-stu-id="ab521-463">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="ab521-464">Options fournies par un modèle d’affichage ou avec une injection de vue directe</span><span class="sxs-lookup"><span data-stu-id="ab521-464">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="ab521-465">Les options fournies par un modèle d’affichage ou avec une injection de vue directe sont illustrées dans l’exemple &num;4 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-465">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="ab521-466">Vous pouvez fournir les options dans un modèle d’affichage ou en injectant <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directement dans une vue (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-466">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="ab521-467">L’exemple d’application montre comment injecter `IOptionsMonitor<MyOptions>` avec une directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="ab521-467">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="ab521-468">Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="ab521-468">When the app is run, the options values are shown in the rendered page:</span></span>

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="ab521-470">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="ab521-470">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="ab521-471">Le rechargement des données de configuration avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est illustré dans l’exemple &num;5 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-471">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="ab521-472"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> prend en charge le rechargement des options avec une charge de traitement minimale.</span><span class="sxs-lookup"><span data-stu-id="ab521-472"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="ab521-473">Les options sont calculées une fois par requête quand le système y accède et sont mises en cache pour toute la durée de vie de la requête.</span><span class="sxs-lookup"><span data-stu-id="ab521-473">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="ab521-474">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="ab521-474">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="ab521-475">Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.</span><span class="sxs-lookup"><span data-stu-id="ab521-475">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="ab521-476">L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="ab521-476">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="ab521-477">Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="ab521-477">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="ab521-478">Enregistrez le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-478">Save the *appsettings.json* file.</span></span> <span data-ttu-id="ab521-479">Actualisez le navigateur pour constater la mise à jour des valeurs des options :</span><span class="sxs-lookup"><span data-stu-id="ab521-479">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="ab521-480">Prise en charge des options nommées avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="ab521-480">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="ab521-481">La prise en charge des options nommées avec <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> est illustrée dans l’exemple &num;6 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ab521-481">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="ab521-482">La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées.</span><span class="sxs-lookup"><span data-stu-id="ab521-482">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="ab521-483">Dans l’exemple d’application, les options nommées sont déclarées avec [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), qui appelle la méthode d’extension [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-483">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="ab521-484">L’exemple d’application accède aux options nommées avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="ab521-484">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="ab521-485">Quand l’exemple d’application est exécuté, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="ab521-485">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="ab521-486">Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ab521-486">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="ab521-487">Les valeurs `named_options_2` sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="ab521-487">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="ab521-488">Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="ab521-488">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="ab521-489">La valeur par défaut `Option2` fournie par la classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-489">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="ab521-490">Configurer toutes les options avec la méthode ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="ab521-490">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="ab521-491">Configurez toutes les instances d’options avec la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="ab521-491">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="ab521-492">Le code suivant configure `Option1` pour toutes les instances de configuration ayant une valeur commune.</span><span class="sxs-lookup"><span data-stu-id="ab521-492">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="ab521-493">Ajoutez le code suivant manuellement à la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="ab521-493">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="ab521-494">Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="ab521-494">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="ab521-495">Toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="ab521-495">All options are named instances.</span></span> <span data-ttu-id="ab521-496">Les instances <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="ab521-496">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="ab521-497">En outre, <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-497"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="ab521-498">L’implémentation par défaut de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> possède une logique qui utilise chaque élément de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="ab521-498">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="ab521-499">L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> et <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="ab521-499">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="ab521-500">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="ab521-500">OptionsBuilder API</span></span>

<span data-ttu-id="ab521-501"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> permet de configurer des instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="ab521-501"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="ab521-502">`OptionsBuilder` simplifie la création d’options nommées. En effet, il est le seul paramètre de l’appel `AddOptions<TOptions>(string optionsName)` initial et n’apparaît pas dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="ab521-502">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="ab521-503">La validation des options et les surcharges `ConfigureOptions` qui acceptent des dépendances de service sont uniquement disponibles avec `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ab521-503">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="ab521-504">Utiliser les services d’injection de dépendances (DI) pour configurer des options</span><span class="sxs-lookup"><span data-stu-id="ab521-504">Use DI services to configure options</span></span>

<span data-ttu-id="ab521-505">Vous pouvez accéder à d’autres services à partir de l’injection de dépendances pendant que vous configurez des options de deux manières différentes :</span><span class="sxs-lookup"><span data-stu-id="ab521-505">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="ab521-506">Transmettez un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) sur [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="ab521-506">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="ab521-507">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) fournit des surcharges de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) qui vous permettent d’utiliser jusqu’à cinq services pour configurer des options :</span><span class="sxs-lookup"><span data-stu-id="ab521-507">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="ab521-508">Créez votre propre type qui implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ou <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et inscrit le type en tant que service.</span><span class="sxs-lookup"><span data-stu-id="ab521-508">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="ab521-509">Nous vous recommandons de transmettre un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), car il est plus complexe de créer un service.</span><span class="sxs-lookup"><span data-stu-id="ab521-509">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="ab521-510">Créer votre propre type équivaut à ce que fait automatiquement le framework quand vous utilisez [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="ab521-510">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="ab521-511">L’appel de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) a pour effet d’inscrire une instance générique temporaire de <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, dont l’un des constructeurs accepte les types de service génériques spécifiés.</span><span class="sxs-lookup"><span data-stu-id="ab521-511">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="ab521-512">Options de post-configuration</span><span class="sxs-lookup"><span data-stu-id="ab521-512">Options post-configuration</span></span>

<span data-ttu-id="ab521-513">Définissez la post-configuration avec <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="ab521-513">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="ab521-514">La post-configuration s’exécute après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> :</span><span class="sxs-lookup"><span data-stu-id="ab521-514">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ab521-515"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> permet de post-configurer les options nommées :</span><span class="sxs-lookup"><span data-stu-id="ab521-515"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ab521-516">Utilisez <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> pour post-configurer toutes les instances de configuration :</span><span class="sxs-lookup"><span data-stu-id="ab521-516">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="ab521-517">Accès aux options au démarrage</span><span class="sxs-lookup"><span data-stu-id="ab521-517">Accessing options during startup</span></span>

<span data-ttu-id="ab521-518"><xref:Microsoft.Extensions.Options.IOptions%601> et <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> peuvent être utilisées dans `Startup.Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute.</span><span class="sxs-lookup"><span data-stu-id="ab521-518"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="ab521-519">N’utilisez pas <xref:Microsoft.Extensions.Options.IOptions%601> ou <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ab521-519">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ab521-520">Un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="ab521-520">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ab521-521">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ab521-521">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
