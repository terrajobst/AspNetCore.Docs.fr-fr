---
title: Modèle d’options dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser le modèle d’options pour représenter des groupes de paramètres associés dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/18/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 4192bab8acef7c4f7bdf1ac481c468cd0a835420
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239794"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="30ac1-103">Modèle d’options dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30ac1-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="30ac1-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="30ac1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="30ac1-105">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="30ac1-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="30ac1-106">Quand les [paramètres de configuration](xref:fundamentals/configuration/index) sont isolés par scénario dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :</span><span class="sxs-lookup"><span data-stu-id="30ac1-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="30ac1-107">[Principe de séparation des interfaces ou encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash;: les scénarios (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="30ac1-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="30ac1-108">[Séparation des préoccupations](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)&ndash; : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="30ac1-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="30ac1-109">Ces options fournissent également un mécanisme de validation des données de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="30ac1-110">Pour plus d'informations, reportez-vous à la section [Validation des options](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="30ac1-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="30ac1-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="30ac1-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="30ac1-112">Package</span><span class="sxs-lookup"><span data-stu-id="30ac1-112">Package</span></span>

<span data-ttu-id="30ac1-113">Le package [Microsoft. extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) est implicitement référencé dans les applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="30ac1-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="30ac1-114">Interfaces d’options</span><span class="sxs-lookup"><span data-stu-id="30ac1-114">Options interfaces</span></span>

<span data-ttu-id="30ac1-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> permet de récupérer des options et de gérer les notifications d’options pour les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="30ac1-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="30ac1-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="30ac1-117">Notifications de modifications</span><span class="sxs-lookup"><span data-stu-id="30ac1-117">Change notifications</span></span>
* [<span data-ttu-id="30ac1-118">Options nommées</span><span class="sxs-lookup"><span data-stu-id="30ac1-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="30ac1-119">Configuration rechargeable</span><span class="sxs-lookup"><span data-stu-id="30ac1-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="30ac1-120">Invalidation sélective des options (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="30ac1-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="30ac1-121">Des scénarios [post-configuration](#options-post-configuration) vous permettent de définir ou de modifier les options après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="30ac1-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> est chargée de créer les instances d’options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="30ac1-123">Elle dispose d’une seule méthode <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="30ac1-124">L’implémentation par défaut prend toutes les <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> inscrites et exécute toutes les configurations, puis les post-configurations.</span><span class="sxs-lookup"><span data-stu-id="30ac1-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="30ac1-125">Elle fait la distinction entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et n’appelle que l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="30ac1-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> est utilisée par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour mettre en cache les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="30ac1-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalide les instances des options dans le moniteur afin que la valeur soit recalculée (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="30ac1-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="30ac1-128">Les valeurs peuvent aussi être introduites manuellement avec <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="30ac1-129">La méthode <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> est utilisée quand toutes les instances nommées doivent être recréées à la demande.</span><span class="sxs-lookup"><span data-stu-id="30ac1-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="30ac1-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est utile dans les scénarios où des options doivent être recalculées à chaque requête.</span><span class="sxs-lookup"><span data-stu-id="30ac1-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="30ac1-131">Pour plus d’informations, consultez la section [Recharger les données de configuration avec IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="30ac1-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="30ac1-132"><xref:Microsoft.Extensions.Options.IOptions%601> peut être utilisée pour prendre en charge des options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="30ac1-133">Toutefois, <xref:Microsoft.Extensions.Options.IOptions%601> ne prend pas en charge les scénarios <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> précédents.</span><span class="sxs-lookup"><span data-stu-id="30ac1-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="30ac1-134">Vous pouvez continuer à utiliser <xref:Microsoft.Extensions.Options.IOptions%601> dans des infrastructures et bibliothèques existantes qui utilisent déjà l’interface <xref:Microsoft.Extensions.Options.IOptions%601> mais ne nécessitent pas les scénarios fournis par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="30ac1-135">Configuration des options générales</span><span class="sxs-lookup"><span data-stu-id="30ac1-135">General options configuration</span></span>

<span data-ttu-id="30ac1-136">La configuration des options générales est illustrée dans l’exemple &num;1 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="30ac1-137">Une classe d’options doit être non abstraite avec un constructeur public sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="30ac1-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="30ac1-138">La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="30ac1-139">Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="30ac1-140">`Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="30ac1-141">La classe `MyOptions` est ajoutée au conteneur de service avec <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> et liée à la configuration :</span><span class="sxs-lookup"><span data-stu-id="30ac1-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="30ac1-142">Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:mvc/controllers/dependency-injection) avec <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="30ac1-143">Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="30ac1-144">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="30ac1-145">Quand vous utilisez un <xref:System.Configuration.ConfigurationBuilder> personnalisé pour charger une configuration d’options à partir d’un fichier de paramètres, vérifiez que le chemin de base est correctement défini :</span><span class="sxs-lookup"><span data-stu-id="30ac1-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="30ac1-146">Vous n’avez pas besoin de définir explicitement le chemin de base quand vous chargez une configuration d’options à partir du fichier de paramètres via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="30ac1-147">Configurer des options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="30ac1-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="30ac1-148">La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="30ac1-149">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-149">Use a delegate to set options values.</span></span> <span data-ttu-id="30ac1-150">L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="30ac1-151">Dans le code suivant, un second service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="30ac1-152">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="30ac1-153">*Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="30ac1-154">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="30ac1-155">Des fournisseurs de configuration sont disponibles à partir de packages NuGet et sont appliqués dans l’ordre de leur inscription.</span><span class="sxs-lookup"><span data-stu-id="30ac1-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="30ac1-156">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="30ac1-157">Chaque appel à <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> ajoute un service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="30ac1-158">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="30ac1-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="30ac1-159">Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="30ac1-160">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="30ac1-161">Configuration des sous-options</span><span class="sxs-lookup"><span data-stu-id="30ac1-161">Suboptions configuration</span></span>

<span data-ttu-id="30ac1-162">La configuration des sous-options est illustrée dans l’exemple &num;3 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="30ac1-163">Les applications doivent créer des classes d’options qui appartiennent à des groupes de scénarios spécifiques (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="30ac1-164">Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="30ac1-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="30ac1-165">Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="30ac1-166">Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="30ac1-167">Dans le code suivant, un troisième service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="30ac1-168">Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="30ac1-169">La méthode `GetSection` requiert l’espace de noms <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="30ac1-170">Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="30ac1-171">La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs des options (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="30ac1-172">La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs des options (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="30ac1-173">Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="30ac1-174">Injection d’options</span><span class="sxs-lookup"><span data-stu-id="30ac1-174">Options injection</span></span>

<span data-ttu-id="30ac1-175">L’injection d’options est illustrée comme exemple &num;4 dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-175">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="30ac1-176">Injectez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans :</span><span class="sxs-lookup"><span data-stu-id="30ac1-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="30ac1-177">Une page Razor ou une vue MVC avec la directive Razor [@inject](xref:mvc/views/razor#inject) .</span><span class="sxs-lookup"><span data-stu-id="30ac1-177">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="30ac1-178">Modèle de page ou de vue.</span><span class="sxs-lookup"><span data-stu-id="30ac1-178">A page or view model.</span></span>

<span data-ttu-id="30ac1-179">L’exemple suivant tiré de l’exemple d’application injecte <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans un modèle de page (*pages/index. cshtml. cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="30ac1-180">L’exemple d’application montre comment injecter `IOptionsMonitor<MyOptions>` avec une directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="30ac1-181">Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="30ac1-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="30ac1-183">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="30ac1-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="30ac1-184">Le rechargement des données de configuration avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est illustré dans l’exemple &num;5 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="30ac1-185">À l’aide de <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, les options sont calculées une fois par demande, en cas d’accès et de mise en cache pour la durée de vie de la demande.</span><span class="sxs-lookup"><span data-stu-id="30ac1-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="30ac1-186">La différence entre `IOptionsMonitor` et `IOptionsSnapshot` est que :</span><span class="sxs-lookup"><span data-stu-id="30ac1-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="30ac1-187">`IOptionsMonitor` est un [service Singleton](xref:fundamentals/dependency-injection#singleton) qui récupère les valeurs d’option actuelles à tout moment, ce qui est particulièrement utile dans les dépendances Singleton.</span><span class="sxs-lookup"><span data-stu-id="30ac1-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="30ac1-188">`IOptionsSnapshot` est un [service étendu](xref:fundamentals/dependency-injection#scoped) et fournit un instantané des options au moment de la construction de l’objet `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="30ac1-189">Les instantanés d’options sont conçus pour être utilisés avec des dépendances transitoires et délimitées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="30ac1-190">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="30ac1-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="30ac1-191">Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="30ac1-192">L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="30ac1-193">Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="30ac1-194">Enregistrez le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="30ac1-195">Actualisez le navigateur pour constater la mise à jour des valeurs des options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="30ac1-196">Prise en charge des options nommées avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="30ac1-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="30ac1-197">La prise en charge des options nommées avec <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> est illustrée dans l’exemple &num;6 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="30ac1-198">La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-198">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="30ac1-199">Dans l’exemple d’application, les options nommées sont déclarées avec [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), qui appelle la méthode d’extension [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="30ac1-200">L’exemple d’application accède aux options nommées avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="30ac1-201">Quand l’exemple d’application est exécuté, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="30ac1-201">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="30ac1-202">Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="30ac1-203">Les valeurs `named_options_2` sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="30ac1-203">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="30ac1-204">Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="30ac1-205">La valeur par défaut `Option2` fournie par la classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-205">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="30ac1-206">Configurer toutes les options avec la méthode ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="30ac1-206">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="30ac1-207">Configurez toutes les instances d’options avec la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="30ac1-208">Le code suivant configure `Option1` pour toutes les instances de configuration ayant une valeur commune.</span><span class="sxs-lookup"><span data-stu-id="30ac1-208">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="30ac1-209">Ajoutez le code suivant manuellement à la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-209">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="30ac1-210">Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="30ac1-210">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="30ac1-211">Toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-211">All options are named instances.</span></span> <span data-ttu-id="30ac1-212">Les instances <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="30ac1-213">En outre, <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="30ac1-214">L’implémentation par défaut de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> possède une logique qui utilise chaque élément de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="30ac1-215">L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> et <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="30ac1-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="30ac1-216">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="30ac1-216">OptionsBuilder API</span></span>

<span data-ttu-id="30ac1-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> permet de configurer des instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="30ac1-218">`OptionsBuilder` simplifie la création d’options nommées. En effet, il est le seul paramètre de l’appel `AddOptions<TOptions>(string optionsName)` initial et n’apparaît pas dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="30ac1-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="30ac1-219">La validation des options et les surcharges `ConfigureOptions` qui acceptent des dépendances de service sont uniquement disponibles avec `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="30ac1-220">Utiliser les services d’injection de dépendances (DI) pour configurer des options</span><span class="sxs-lookup"><span data-stu-id="30ac1-220">Use DI services to configure options</span></span>

<span data-ttu-id="30ac1-221">Vous pouvez accéder à d’autres services à partir de l’injection de dépendances pendant que vous configurez des options de deux manières différentes :</span><span class="sxs-lookup"><span data-stu-id="30ac1-221">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="30ac1-222">Transmettez un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) sur [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="30ac1-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="30ac1-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) fournit des surcharges de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) qui vous permettent d’utiliser jusqu’à cinq services pour configurer des options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="30ac1-224">Créez votre propre type qui implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ou <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et inscrit le type en tant que service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="30ac1-225">Nous vous recommandons de transmettre un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), car il est plus complexe de créer un service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="30ac1-226">Créer votre propre type équivaut à ce que fait automatiquement le framework quand vous utilisez [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="30ac1-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="30ac1-227">L’appel de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) a pour effet d’inscrire une instance générique temporaire de <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, dont l’un des constructeurs accepte les types de service génériques spécifiés.</span><span class="sxs-lookup"><span data-stu-id="30ac1-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="30ac1-228">Validation des options</span><span class="sxs-lookup"><span data-stu-id="30ac1-228">Options validation</span></span>

<span data-ttu-id="30ac1-229">La validation des options vous permet de valider les options lorsque celles-ci sont configurées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-229">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="30ac1-230">Appelez `Validate` avec une méthode de validation qui retourne `true` si les options sont valides et `false` si elles ne sont pas valides :</span><span class="sxs-lookup"><span data-stu-id="30ac1-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="30ac1-231">L’exemple précédent définit l’instance d’options nommée sur `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-231">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="30ac1-232">L'instance d’options par défaut est `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-232">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="30ac1-233">La validation s’exécute lorsque l’instance d’options est créée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-233">Validation runs when the options instance is created.</span></span> <span data-ttu-id="30ac1-234">Votre instance d’options est garantie de réussir la validation lors du premier accès.</span><span class="sxs-lookup"><span data-stu-id="30ac1-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="30ac1-235">La validation des options ne protège pas contre les modifications des options une fois que les options sont initialement configurées et validées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="30ac1-236">La méthode `Validate` accepte un `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="30ac1-237">Pour personnaliser entièrement la validation, implémentez `IValidateOptions<TOptions>`, ce qui permet :</span><span class="sxs-lookup"><span data-stu-id="30ac1-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="30ac1-238">Validation de plusieurs types d’options : `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="30ac1-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="30ac1-239">Validation qui dépend d’un autre type d’option : `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="30ac1-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="30ac1-240">`IValidateOptions` valide :</span><span class="sxs-lookup"><span data-stu-id="30ac1-240">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="30ac1-241">Une instance d’options nommée spécifique.</span><span class="sxs-lookup"><span data-stu-id="30ac1-241">A specific named options instance.</span></span>
* <span data-ttu-id="30ac1-242">Toutes les options quand `name` est `null`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-242">All options when `name` is `null`.</span></span>

<span data-ttu-id="30ac1-243">Retourne `ValidateOptionsResult` à partir de votre implémentation de l’interface :</span><span class="sxs-lookup"><span data-stu-id="30ac1-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="30ac1-244">La validation basée sur l’annotation de données est disponible dans le package [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations). Pour cela, appelez la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> sur `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="30ac1-245">`Microsoft.Extensions.Options.DataAnnotations` est implicitement référencé dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="30ac1-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="30ac1-246">La validation hâtive (échec rapide au démarrage) est à l’étude pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="30ac1-246">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="30ac1-247">Options de post-configuration</span><span class="sxs-lookup"><span data-stu-id="30ac1-247">Options post-configuration</span></span>

<span data-ttu-id="30ac1-248">Définissez la post-configuration avec <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="30ac1-249">La post-configuration s’exécute après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> :</span><span class="sxs-lookup"><span data-stu-id="30ac1-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="30ac1-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> permet de post-configurer les options nommées :</span><span class="sxs-lookup"><span data-stu-id="30ac1-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="30ac1-251">Utilisez <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> pour post-configurer toutes les instances de configuration :</span><span class="sxs-lookup"><span data-stu-id="30ac1-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="30ac1-252">Accès aux options au démarrage</span><span class="sxs-lookup"><span data-stu-id="30ac1-252">Accessing options during startup</span></span>

<span data-ttu-id="30ac1-253"><xref:Microsoft.Extensions.Options.IOptions%601> et <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> peuvent être utilisées dans `Startup.Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute.</span><span class="sxs-lookup"><span data-stu-id="30ac1-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="30ac1-254">N’utilisez pas <xref:Microsoft.Extensions.Options.IOptions%601> ou <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="30ac1-255">Un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-255">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="30ac1-256">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="30ac1-256">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="30ac1-257">Quand les [paramètres de configuration](xref:fundamentals/configuration/index) sont isolés par scénario dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :</span><span class="sxs-lookup"><span data-stu-id="30ac1-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="30ac1-258">[Principe de séparation des interfaces ou encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash;: les scénarios (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="30ac1-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="30ac1-259">[Séparation des préoccupations](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)&ndash; : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="30ac1-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="30ac1-260">Ces options fournissent également un mécanisme de validation des données de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-260">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="30ac1-261">Pour plus d'informations, reportez-vous à la section [Validation des options](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="30ac1-261">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="30ac1-262">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="30ac1-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30ac1-263">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="30ac1-263">Prerequisites</span></span>

<span data-ttu-id="30ac1-264">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="30ac1-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="30ac1-265">Interfaces d’options</span><span class="sxs-lookup"><span data-stu-id="30ac1-265">Options interfaces</span></span>

<span data-ttu-id="30ac1-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> permet de récupérer des options et de gérer les notifications d’options pour les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="30ac1-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="30ac1-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="30ac1-268">Notifications de modifications</span><span class="sxs-lookup"><span data-stu-id="30ac1-268">Change notifications</span></span>
* [<span data-ttu-id="30ac1-269">Options nommées</span><span class="sxs-lookup"><span data-stu-id="30ac1-269">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="30ac1-270">Configuration rechargeable</span><span class="sxs-lookup"><span data-stu-id="30ac1-270">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="30ac1-271">Invalidation sélective des options (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="30ac1-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="30ac1-272">Des scénarios [post-configuration](#options-post-configuration) vous permettent de définir ou de modifier les options après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="30ac1-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> est chargée de créer les instances d’options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="30ac1-274">Elle dispose d’une seule méthode <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="30ac1-275">L’implémentation par défaut prend toutes les <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> inscrites et exécute toutes les configurations, puis les post-configurations.</span><span class="sxs-lookup"><span data-stu-id="30ac1-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="30ac1-276">Elle fait la distinction entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et n’appelle que l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="30ac1-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> est utilisée par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour mettre en cache les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="30ac1-278"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalide les instances des options dans le moniteur afin que la valeur soit recalculée (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="30ac1-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="30ac1-279">Les valeurs peuvent aussi être introduites manuellement avec <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="30ac1-280">La méthode <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> est utilisée quand toutes les instances nommées doivent être recréées à la demande.</span><span class="sxs-lookup"><span data-stu-id="30ac1-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="30ac1-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est utile dans les scénarios où des options doivent être recalculées à chaque requête.</span><span class="sxs-lookup"><span data-stu-id="30ac1-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="30ac1-282">Pour plus d’informations, consultez la section [Recharger les données de configuration avec IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="30ac1-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="30ac1-283"><xref:Microsoft.Extensions.Options.IOptions%601> peut être utilisée pour prendre en charge des options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="30ac1-284">Toutefois, <xref:Microsoft.Extensions.Options.IOptions%601> ne prend pas en charge les scénarios <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> précédents.</span><span class="sxs-lookup"><span data-stu-id="30ac1-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="30ac1-285">Vous pouvez continuer à utiliser <xref:Microsoft.Extensions.Options.IOptions%601> dans des infrastructures et bibliothèques existantes qui utilisent déjà l’interface <xref:Microsoft.Extensions.Options.IOptions%601> mais ne nécessitent pas les scénarios fournis par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="30ac1-286">Configuration des options générales</span><span class="sxs-lookup"><span data-stu-id="30ac1-286">General options configuration</span></span>

<span data-ttu-id="30ac1-287">La configuration des options générales est illustrée dans l’exemple &num;1 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="30ac1-288">Une classe d’options doit être non abstraite avec un constructeur public sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="30ac1-288">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="30ac1-289">La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="30ac1-290">Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="30ac1-291">`Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="30ac1-292">La classe `MyOptions` est ajoutée au conteneur de service avec <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> et liée à la configuration :</span><span class="sxs-lookup"><span data-stu-id="30ac1-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="30ac1-293">Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:mvc/controllers/dependency-injection) avec <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="30ac1-294">Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="30ac1-295">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="30ac1-296">Quand vous utilisez un <xref:System.Configuration.ConfigurationBuilder> personnalisé pour charger une configuration d’options à partir d’un fichier de paramètres, vérifiez que le chemin de base est correctement défini :</span><span class="sxs-lookup"><span data-stu-id="30ac1-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="30ac1-297">Vous n’avez pas besoin de définir explicitement le chemin de base quand vous chargez une configuration d’options à partir du fichier de paramètres via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="30ac1-298">Configurer des options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="30ac1-298">Configure simple options with a delegate</span></span>

<span data-ttu-id="30ac1-299">La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="30ac1-300">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-300">Use a delegate to set options values.</span></span> <span data-ttu-id="30ac1-301">L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="30ac1-302">Dans le code suivant, un second service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="30ac1-303">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="30ac1-304">*Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-304">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="30ac1-305">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-305">You can add multiple configuration providers.</span></span> <span data-ttu-id="30ac1-306">Des fournisseurs de configuration sont disponibles à partir de packages NuGet et sont appliqués dans l’ordre de leur inscription.</span><span class="sxs-lookup"><span data-stu-id="30ac1-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="30ac1-307">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-307">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="30ac1-308">Chaque appel à <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> ajoute un service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="30ac1-309">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="30ac1-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="30ac1-310">Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="30ac1-311">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="30ac1-312">Configuration des sous-options</span><span class="sxs-lookup"><span data-stu-id="30ac1-312">Suboptions configuration</span></span>

<span data-ttu-id="30ac1-313">La configuration des sous-options est illustrée dans l’exemple &num;3 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="30ac1-314">Les applications doivent créer des classes d’options qui appartiennent à des groupes de scénarios spécifiques (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="30ac1-315">Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="30ac1-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="30ac1-316">Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="30ac1-317">Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="30ac1-318">Dans le code suivant, un troisième service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="30ac1-319">Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="30ac1-320">La méthode `GetSection` requiert l’espace de noms <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="30ac1-321">Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="30ac1-322">La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs des options (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="30ac1-323">La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs des options (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="30ac1-324">Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="30ac1-325">Injection d’options</span><span class="sxs-lookup"><span data-stu-id="30ac1-325">Options injection</span></span>

<span data-ttu-id="30ac1-326">L’injection d’options est illustrée comme exemple &num;4 dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-326">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="30ac1-327">Injectez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans :</span><span class="sxs-lookup"><span data-stu-id="30ac1-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="30ac1-328">Une page Razor ou une vue MVC avec la directive Razor [@inject](xref:mvc/views/razor#inject) .</span><span class="sxs-lookup"><span data-stu-id="30ac1-328">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="30ac1-329">Modèle de page ou de vue.</span><span class="sxs-lookup"><span data-stu-id="30ac1-329">A page or view model.</span></span>

<span data-ttu-id="30ac1-330">L’exemple suivant tiré de l’exemple d’application injecte <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans un modèle de page (*pages/index. cshtml. cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="30ac1-331">L’exemple d’application montre comment injecter `IOptionsMonitor<MyOptions>` avec une directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="30ac1-332">Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="30ac1-332">When the app is run, the options values are shown in the rendered page:</span></span>

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="30ac1-334">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="30ac1-334">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="30ac1-335">Le rechargement des données de configuration avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est illustré dans l’exemple &num;5 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="30ac1-336">À l’aide de <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, les options sont calculées une fois par demande, en cas d’accès et de mise en cache pour la durée de vie de la demande.</span><span class="sxs-lookup"><span data-stu-id="30ac1-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="30ac1-337">La différence entre `IOptionsMonitor` et `IOptionsSnapshot` est que :</span><span class="sxs-lookup"><span data-stu-id="30ac1-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="30ac1-338">`IOptionsMonitor` est un [service Singleton](xref:fundamentals/dependency-injection#singleton) qui récupère les valeurs d’option actuelles à tout moment, ce qui est particulièrement utile dans les dépendances Singleton.</span><span class="sxs-lookup"><span data-stu-id="30ac1-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="30ac1-339">`IOptionsSnapshot` est un [service étendu](xref:fundamentals/dependency-injection#scoped) et fournit un instantané des options au moment de la construction de l’objet `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="30ac1-340">Les instantanés d’options sont conçus pour être utilisés avec des dépendances transitoires et délimitées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-340">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="30ac1-341">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="30ac1-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="30ac1-342">Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="30ac1-343">L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="30ac1-344">Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="30ac1-345">Enregistrez le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-345">Save the *appsettings.json* file.</span></span> <span data-ttu-id="30ac1-346">Actualisez le navigateur pour constater la mise à jour des valeurs des options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-346">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="30ac1-347">Prise en charge des options nommées avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="30ac1-347">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="30ac1-348">La prise en charge des options nommées avec <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> est illustrée dans l’exemple &num;6 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="30ac1-349">La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-349">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="30ac1-350">Dans l’exemple d’application, les options nommées sont déclarées avec [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), qui appelle la méthode d’extension [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="30ac1-351">L’exemple d’application accède aux options nommées avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="30ac1-352">Quand l’exemple d’application est exécuté, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="30ac1-352">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="30ac1-353">Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="30ac1-354">Les valeurs `named_options_2` sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="30ac1-354">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="30ac1-355">Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="30ac1-356">La valeur par défaut `Option2` fournie par la classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-356">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="30ac1-357">Configurer toutes les options avec la méthode ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="30ac1-357">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="30ac1-358">Configurez toutes les instances d’options avec la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="30ac1-359">Le code suivant configure `Option1` pour toutes les instances de configuration ayant une valeur commune.</span><span class="sxs-lookup"><span data-stu-id="30ac1-359">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="30ac1-360">Ajoutez le code suivant manuellement à la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-360">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="30ac1-361">Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="30ac1-361">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="30ac1-362">Toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-362">All options are named instances.</span></span> <span data-ttu-id="30ac1-363">Les instances <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="30ac1-364">En outre, <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="30ac1-365">L’implémentation par défaut de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> possède une logique qui utilise chaque élément de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="30ac1-366">L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> et <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="30ac1-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="30ac1-367">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="30ac1-367">OptionsBuilder API</span></span>

<span data-ttu-id="30ac1-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> permet de configurer des instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="30ac1-369">`OptionsBuilder` simplifie la création d’options nommées. En effet, il est le seul paramètre de l’appel `AddOptions<TOptions>(string optionsName)` initial et n’apparaît pas dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="30ac1-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="30ac1-370">La validation des options et les surcharges `ConfigureOptions` qui acceptent des dépendances de service sont uniquement disponibles avec `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="30ac1-371">Utiliser les services d’injection de dépendances (DI) pour configurer des options</span><span class="sxs-lookup"><span data-stu-id="30ac1-371">Use DI services to configure options</span></span>

<span data-ttu-id="30ac1-372">Vous pouvez accéder à d’autres services à partir de l’injection de dépendances pendant que vous configurez des options de deux manières différentes :</span><span class="sxs-lookup"><span data-stu-id="30ac1-372">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="30ac1-373">Transmettez un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) sur [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="30ac1-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="30ac1-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) fournit des surcharges de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) qui vous permettent d’utiliser jusqu’à cinq services pour configurer des options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="30ac1-375">Créez votre propre type qui implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ou <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et inscrit le type en tant que service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="30ac1-376">Nous vous recommandons de transmettre un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), car il est plus complexe de créer un service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="30ac1-377">Créer votre propre type équivaut à ce que fait automatiquement le framework quand vous utilisez [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="30ac1-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="30ac1-378">L’appel de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) a pour effet d’inscrire une instance générique temporaire de <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, dont l’un des constructeurs accepte les types de service génériques spécifiés.</span><span class="sxs-lookup"><span data-stu-id="30ac1-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="30ac1-379">Validation des options</span><span class="sxs-lookup"><span data-stu-id="30ac1-379">Options validation</span></span>

<span data-ttu-id="30ac1-380">La validation des options vous permet de valider les options lorsque celles-ci sont configurées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-380">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="30ac1-381">Appelez `Validate` avec une méthode de validation qui retourne `true` si les options sont valides et `false` si elles ne sont pas valides :</span><span class="sxs-lookup"><span data-stu-id="30ac1-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="30ac1-382">L’exemple précédent définit l’instance d’options nommée sur `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-382">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="30ac1-383">L'instance d’options par défaut est `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-383">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="30ac1-384">La validation s’exécute lorsque l’instance d’options est créée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-384">Validation runs when the options instance is created.</span></span> <span data-ttu-id="30ac1-385">Votre instance d’options est garantie de réussir la validation lors du premier accès.</span><span class="sxs-lookup"><span data-stu-id="30ac1-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="30ac1-386">La validation des options ne protège pas contre les modifications des options une fois que les options sont initialement configurées et validées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="30ac1-387">La méthode `Validate` accepte un `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="30ac1-388">Pour personnaliser entièrement la validation, implémentez `IValidateOptions<TOptions>`, ce qui permet :</span><span class="sxs-lookup"><span data-stu-id="30ac1-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="30ac1-389">Validation de plusieurs types d’options : `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="30ac1-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="30ac1-390">Validation qui dépend d’un autre type d’option : `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="30ac1-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="30ac1-391">`IValidateOptions` valide :</span><span class="sxs-lookup"><span data-stu-id="30ac1-391">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="30ac1-392">Une instance d’options nommée spécifique.</span><span class="sxs-lookup"><span data-stu-id="30ac1-392">A specific named options instance.</span></span>
* <span data-ttu-id="30ac1-393">Toutes les options quand `name` est `null`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-393">All options when `name` is `null`.</span></span>

<span data-ttu-id="30ac1-394">Retourne `ValidateOptionsResult` à partir de votre implémentation de l’interface :</span><span class="sxs-lookup"><span data-stu-id="30ac1-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="30ac1-395">La validation basée sur l’annotation de données est disponible dans le package [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations). Pour cela, appelez la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> sur `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="30ac1-396">`Microsoft.Extensions.Options.DataAnnotations` est inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="30ac1-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="30ac1-397">La validation hâtive (échec rapide au démarrage) est à l’étude pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="30ac1-397">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="30ac1-398">Options de post-configuration</span><span class="sxs-lookup"><span data-stu-id="30ac1-398">Options post-configuration</span></span>

<span data-ttu-id="30ac1-399">Définissez la post-configuration avec <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="30ac1-400">La post-configuration s’exécute après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> :</span><span class="sxs-lookup"><span data-stu-id="30ac1-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="30ac1-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> permet de post-configurer les options nommées :</span><span class="sxs-lookup"><span data-stu-id="30ac1-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="30ac1-402">Utilisez <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> pour post-configurer toutes les instances de configuration :</span><span class="sxs-lookup"><span data-stu-id="30ac1-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="30ac1-403">Accès aux options au démarrage</span><span class="sxs-lookup"><span data-stu-id="30ac1-403">Accessing options during startup</span></span>

<span data-ttu-id="30ac1-404"><xref:Microsoft.Extensions.Options.IOptions%601> et <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> peuvent être utilisées dans `Startup.Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute.</span><span class="sxs-lookup"><span data-stu-id="30ac1-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="30ac1-405">N’utilisez pas <xref:Microsoft.Extensions.Options.IOptions%601> ou <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="30ac1-406">Un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-406">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="30ac1-407">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="30ac1-407">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="30ac1-408">Quand les [paramètres de configuration](xref:fundamentals/configuration/index) sont isolés par scénario dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :</span><span class="sxs-lookup"><span data-stu-id="30ac1-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="30ac1-409">[Principe de séparation des interfaces ou encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash;: les scénarios (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="30ac1-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="30ac1-410">[Séparation des préoccupations](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)&ndash; : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="30ac1-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="30ac1-411">Ces options fournissent également un mécanisme de validation des données de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-411">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="30ac1-412">Pour plus d'informations, reportez-vous à la section [Validation des options](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="30ac1-412">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="30ac1-413">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="30ac1-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30ac1-414">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="30ac1-414">Prerequisites</span></span>

<span data-ttu-id="30ac1-415">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="30ac1-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="30ac1-416">Interfaces d’options</span><span class="sxs-lookup"><span data-stu-id="30ac1-416">Options interfaces</span></span>

<span data-ttu-id="30ac1-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> permet de récupérer des options et de gérer les notifications d’options pour les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="30ac1-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="30ac1-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="30ac1-419">Notifications de modifications</span><span class="sxs-lookup"><span data-stu-id="30ac1-419">Change notifications</span></span>
* [<span data-ttu-id="30ac1-420">Options nommées</span><span class="sxs-lookup"><span data-stu-id="30ac1-420">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="30ac1-421">Configuration rechargeable</span><span class="sxs-lookup"><span data-stu-id="30ac1-421">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="30ac1-422">Invalidation sélective des options (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="30ac1-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="30ac1-423">Des scénarios [post-configuration](#options-post-configuration) vous permettent de définir ou de modifier les options après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="30ac1-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> est chargée de créer les instances d’options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="30ac1-425">Elle dispose d’une seule méthode <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="30ac1-426">L’implémentation par défaut prend toutes les <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> inscrites et exécute toutes les configurations, puis les post-configurations.</span><span class="sxs-lookup"><span data-stu-id="30ac1-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="30ac1-427">Elle fait la distinction entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et <xref:Microsoft.Extensions.Options.IConfigureOptions%601> et n’appelle que l’interface appropriée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="30ac1-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> est utilisée par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour mettre en cache les instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="30ac1-429"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalide les instances des options dans le moniteur afin que la valeur soit recalculée (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="30ac1-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="30ac1-430">Les valeurs peuvent aussi être introduites manuellement avec <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="30ac1-431">La méthode <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> est utilisée quand toutes les instances nommées doivent être recréées à la demande.</span><span class="sxs-lookup"><span data-stu-id="30ac1-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="30ac1-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est utile dans les scénarios où des options doivent être recalculées à chaque requête.</span><span class="sxs-lookup"><span data-stu-id="30ac1-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="30ac1-433">Pour plus d’informations, consultez la section [Recharger les données de configuration avec IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="30ac1-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="30ac1-434"><xref:Microsoft.Extensions.Options.IOptions%601> peut être utilisée pour prendre en charge des options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="30ac1-435">Toutefois, <xref:Microsoft.Extensions.Options.IOptions%601> ne prend pas en charge les scénarios <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> précédents.</span><span class="sxs-lookup"><span data-stu-id="30ac1-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="30ac1-436">Vous pouvez continuer à utiliser <xref:Microsoft.Extensions.Options.IOptions%601> dans des infrastructures et bibliothèques existantes qui utilisent déjà l’interface <xref:Microsoft.Extensions.Options.IOptions%601> mais ne nécessitent pas les scénarios fournis par <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="30ac1-437">Configuration des options générales</span><span class="sxs-lookup"><span data-stu-id="30ac1-437">General options configuration</span></span>

<span data-ttu-id="30ac1-438">La configuration des options générales est illustrée dans l’exemple &num;1 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="30ac1-439">Une classe d’options doit être non abstraite avec un constructeur public sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="30ac1-439">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="30ac1-440">La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="30ac1-441">Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="30ac1-442">`Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="30ac1-443">La classe `MyOptions` est ajoutée au conteneur de service avec <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> et liée à la configuration :</span><span class="sxs-lookup"><span data-stu-id="30ac1-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="30ac1-444">Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:mvc/controllers/dependency-injection) avec <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="30ac1-445">Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="30ac1-446">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="30ac1-447">Quand vous utilisez un <xref:System.Configuration.ConfigurationBuilder> personnalisé pour charger une configuration d’options à partir d’un fichier de paramètres, vérifiez que le chemin de base est correctement défini :</span><span class="sxs-lookup"><span data-stu-id="30ac1-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="30ac1-448">Vous n’avez pas besoin de définir explicitement le chemin de base quand vous chargez une configuration d’options à partir du fichier de paramètres via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="30ac1-449">Configurer des options simples avec un délégué</span><span class="sxs-lookup"><span data-stu-id="30ac1-449">Configure simple options with a delegate</span></span>

<span data-ttu-id="30ac1-450">La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="30ac1-451">Utilisez un délégué pour définir les valeurs des options.</span><span class="sxs-lookup"><span data-stu-id="30ac1-451">Use a delegate to set options values.</span></span> <span data-ttu-id="30ac1-452">L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="30ac1-453">Dans le code suivant, un second service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="30ac1-454">Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="30ac1-455">*Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-455">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="30ac1-456">Vous pouvez ajouter plusieurs fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-456">You can add multiple configuration providers.</span></span> <span data-ttu-id="30ac1-457">Des fournisseurs de configuration sont disponibles à partir de packages NuGet et sont appliqués dans l’ordre de leur inscription.</span><span class="sxs-lookup"><span data-stu-id="30ac1-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="30ac1-458">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-458">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="30ac1-459">Chaque appel à <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> ajoute un service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="30ac1-460">Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.</span><span class="sxs-lookup"><span data-stu-id="30ac1-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="30ac1-461">Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration.</span><span class="sxs-lookup"><span data-stu-id="30ac1-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="30ac1-462">Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="30ac1-463">Configuration des sous-options</span><span class="sxs-lookup"><span data-stu-id="30ac1-463">Suboptions configuration</span></span>

<span data-ttu-id="30ac1-464">La configuration des sous-options est illustrée dans l’exemple &num;3 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="30ac1-465">Les applications doivent créer des classes d’options qui appartiennent à des groupes de scénarios spécifiques (classes) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="30ac1-466">Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="30ac1-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="30ac1-467">Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="30ac1-468">Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="30ac1-469">Dans le code suivant, un troisième service <xref:Microsoft.Extensions.Options.IConfigureOptions%601> est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="30ac1-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="30ac1-470">Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="30ac1-471">La méthode `GetSection` requiert l’espace de noms <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="30ac1-472">Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="30ac1-473">La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs des options (*Models/MySubOptions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="30ac1-474">La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs des options (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="30ac1-475">Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="30ac1-476">Options fournies par un modèle d’affichage ou avec une injection de vue directe</span><span class="sxs-lookup"><span data-stu-id="30ac1-476">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="30ac1-477">Les options fournies par un modèle d’affichage ou avec une injection de vue directe sont illustrées dans l’exemple &num;4 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="30ac1-478">Vous pouvez fournir les options dans un modèle d’affichage ou en injectant <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directement dans une vue (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="30ac1-479">L’exemple d’application montre comment injecter `IOptionsMonitor<MyOptions>` avec une directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="30ac1-480">Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :</span><span class="sxs-lookup"><span data-stu-id="30ac1-480">When the app is run, the options values are shown in the rendered page:</span></span>

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="30ac1-482">Recharger les données de configuration avec IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="30ac1-482">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="30ac1-483">Le rechargement des données de configuration avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est illustré dans l’exemple &num;5 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="30ac1-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> prend en charge le rechargement des options avec une charge de traitement minimale.</span><span class="sxs-lookup"><span data-stu-id="30ac1-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="30ac1-485">Les options sont calculées une fois par requête quand le système y accède et sont mises en cache pour toute la durée de vie de la requête.</span><span class="sxs-lookup"><span data-stu-id="30ac1-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="30ac1-486">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="30ac1-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="30ac1-487">Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="30ac1-488">L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="30ac1-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="30ac1-489">Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="30ac1-490">Enregistrez le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-490">Save the *appsettings.json* file.</span></span> <span data-ttu-id="30ac1-491">Actualisez le navigateur pour constater la mise à jour des valeurs des options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-491">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="30ac1-492">Prise en charge des options nommées avec IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="30ac1-492">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="30ac1-493">La prise en charge des options nommées avec <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> est illustrée dans l’exemple &num;6 de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="30ac1-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="30ac1-494">La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-494">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="30ac1-495">Dans l’exemple d’application, les options nommées sont déclarées avec [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), qui appelle la méthode d’extension [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="30ac1-496">L’exemple d’application accède aux options nommées avec <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="30ac1-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="30ac1-497">Quand l’exemple d’application est exécuté, les options nommées sont retournées :</span><span class="sxs-lookup"><span data-stu-id="30ac1-497">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="30ac1-498">Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="30ac1-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="30ac1-499">Les valeurs `named_options_2` sont fournies par :</span><span class="sxs-lookup"><span data-stu-id="30ac1-499">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="30ac1-500">Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="30ac1-501">La valeur par défaut `Option2` fournie par la classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-501">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="30ac1-502">Configurer toutes les options avec la méthode ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="30ac1-502">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="30ac1-503">Configurez toutes les instances d’options avec la méthode <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="30ac1-504">Le code suivant configure `Option1` pour toutes les instances de configuration ayant une valeur commune.</span><span class="sxs-lookup"><span data-stu-id="30ac1-504">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="30ac1-505">Ajoutez le code suivant manuellement à la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="30ac1-505">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="30ac1-506">Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :</span><span class="sxs-lookup"><span data-stu-id="30ac1-506">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="30ac1-507">Toutes les options sont des instances nommées.</span><span class="sxs-lookup"><span data-stu-id="30ac1-507">All options are named instances.</span></span> <span data-ttu-id="30ac1-508">Les instances <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="30ac1-509">En outre, <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="30ac1-510">L’implémentation par défaut de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> possède une logique qui utilise chaque élément de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="30ac1-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="30ac1-511">L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> et <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> utilisent cette convention).</span><span class="sxs-lookup"><span data-stu-id="30ac1-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="30ac1-512">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="30ac1-512">OptionsBuilder API</span></span>

<span data-ttu-id="30ac1-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> permet de configurer des instances `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="30ac1-514">`OptionsBuilder` simplifie la création d’options nommées. En effet, il est le seul paramètre de l’appel `AddOptions<TOptions>(string optionsName)` initial et n’apparaît pas dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="30ac1-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="30ac1-515">La validation des options et les surcharges `ConfigureOptions` qui acceptent des dépendances de service sont uniquement disponibles avec `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="30ac1-516">Utiliser les services d’injection de dépendances (DI) pour configurer des options</span><span class="sxs-lookup"><span data-stu-id="30ac1-516">Use DI services to configure options</span></span>

<span data-ttu-id="30ac1-517">Vous pouvez accéder à d’autres services à partir de l’injection de dépendances pendant que vous configurez des options de deux manières différentes :</span><span class="sxs-lookup"><span data-stu-id="30ac1-517">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="30ac1-518">Transmettez un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) sur [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="30ac1-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="30ac1-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) fournit des surcharges de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) qui vous permettent d’utiliser jusqu’à cinq services pour configurer des options :</span><span class="sxs-lookup"><span data-stu-id="30ac1-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="30ac1-520">Créez votre propre type qui implémente <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ou <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> et inscrit le type en tant que service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="30ac1-521">Nous vous recommandons de transmettre un délégué de configuration à [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), car il est plus complexe de créer un service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="30ac1-522">Créer votre propre type équivaut à ce que fait automatiquement le framework quand vous utilisez [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="30ac1-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="30ac1-523">L’appel de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) a pour effet d’inscrire une instance générique temporaire de <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, dont l’un des constructeurs accepte les types de service génériques spécifiés.</span><span class="sxs-lookup"><span data-stu-id="30ac1-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="30ac1-524">Options de post-configuration</span><span class="sxs-lookup"><span data-stu-id="30ac1-524">Options post-configuration</span></span>

<span data-ttu-id="30ac1-525">Définissez la post-configuration avec <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="30ac1-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="30ac1-526">La post-configuration s’exécute après chaque configuration de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> :</span><span class="sxs-lookup"><span data-stu-id="30ac1-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="30ac1-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> permet de post-configurer les options nommées :</span><span class="sxs-lookup"><span data-stu-id="30ac1-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="30ac1-528">Utilisez <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> pour post-configurer toutes les instances de configuration :</span><span class="sxs-lookup"><span data-stu-id="30ac1-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="30ac1-529">Accès aux options au démarrage</span><span class="sxs-lookup"><span data-stu-id="30ac1-529">Accessing options during startup</span></span>

<span data-ttu-id="30ac1-530"><xref:Microsoft.Extensions.Options.IOptions%601> et <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> peuvent être utilisées dans `Startup.Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute.</span><span class="sxs-lookup"><span data-stu-id="30ac1-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="30ac1-531">N’utilisez pas <xref:Microsoft.Extensions.Options.IOptions%601> ou <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="30ac1-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="30ac1-532">Un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="30ac1-532">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="30ac1-533">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="30ac1-533">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
