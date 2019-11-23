---
title: Détecter les modifications avec des jetons de modification dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser des jetons de modification pour effectuer le suivi des modifications.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: bb30d7a4c7dc82200821c60a49c314b246562111
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007210"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="9948b-103">Détecter les modifications avec des jetons de modification dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9948b-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="9948b-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9948b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9948b-105">Un *jeton de modification* est un module à usage général de bas niveau, utilisé pour effectuer le suivi des modifications de l’état.</span><span class="sxs-lookup"><span data-stu-id="9948b-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="9948b-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9948b-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="9948b-107">Interface d’IChangeToken</span><span class="sxs-lookup"><span data-stu-id="9948b-107">IChangeToken interface</span></span>

<span data-ttu-id="9948b-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propage des notifications indiquant qu’une modification s’est produite.</span><span class="sxs-lookup"><span data-stu-id="9948b-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="9948b-109">`IChangeToken` réside dans l’espace de noms <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="9948b-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="9948b-110">Le package NuGet [Microsoft. extensions. Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) est fourni implicitement aux applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="9948b-110">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="9948b-111">`IChangeToken` a deux propriétés :</span><span class="sxs-lookup"><span data-stu-id="9948b-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="9948b-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indique si le jeton déclenche des rappels de façon proactive.</span><span class="sxs-lookup"><span data-stu-id="9948b-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="9948b-113">Si `ActiveChangedCallbacks` est défini sur `false`, un rappel n’est jamais appelé, et l’application doit interroger `HasChanged` pour connaître les modifications.</span><span class="sxs-lookup"><span data-stu-id="9948b-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="9948b-114">Il est également possible qu’un jeton ne soit jamais annulé si aucune modification ne se produit, ou si l’écouteur de modifications sous-jacent est supprimé ou désactivé.</span><span class="sxs-lookup"><span data-stu-id="9948b-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="9948b-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> obtient une valeur qui indique si une modification a eu lieu.</span><span class="sxs-lookup"><span data-stu-id="9948b-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="9948b-116">L’interface `IChangeToken` inclut la méthode [RegisterChangeCallback(Action\<Objet>, Objet)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*), qui inscrit un rappel qui est appelé quand le jeton a été modifié.</span><span class="sxs-lookup"><span data-stu-id="9948b-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="9948b-117">`HasChanged` doit être défini avant que le rappel soit appelé.</span><span class="sxs-lookup"><span data-stu-id="9948b-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="9948b-118">Classe ChangeToken</span><span class="sxs-lookup"><span data-stu-id="9948b-118">ChangeToken class</span></span>

<span data-ttu-id="9948b-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> est une classe statique utilisée pour propager des notifications indiquant qu’une modification s’est produite.</span><span class="sxs-lookup"><span data-stu-id="9948b-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="9948b-120">`ChangeToken` réside dans l’espace de noms <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="9948b-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="9948b-121">Le package NuGet [Microsoft. extensions. Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) est fourni implicitement aux applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="9948b-121">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="9948b-122">La méthode [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) inscrit une `Action` à appeler chaque fois que le jeton change :</span><span class="sxs-lookup"><span data-stu-id="9948b-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="9948b-123">`Func<IChangeToken>` produit le jeton.</span><span class="sxs-lookup"><span data-stu-id="9948b-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="9948b-124">`Action` est appelée quand le jeton change.</span><span class="sxs-lookup"><span data-stu-id="9948b-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="9948b-125">La surcharge [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) prend un paramètre `TState` supplémentaire qui est passé dans le consommateur du jeton `Action`.</span><span class="sxs-lookup"><span data-stu-id="9948b-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="9948b-126">`OnChange`Retourne un <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="9948b-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="9948b-127">L’appel de <xref:System.IDisposable.Dispose*> fait que le jeton cesse d’être à l’écoute des modifications suivantes et libère les ressources du jeton.</span><span class="sxs-lookup"><span data-stu-id="9948b-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="9948b-128">Exemples d’utilisation de jetons de modification dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9948b-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="9948b-129">Les jetons de modification sont utilisés dans des zones importantes d’ASP.NET Core pour surveiller les modifications apportées aux objets :</span><span class="sxs-lookup"><span data-stu-id="9948b-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="9948b-130">Pour surveiller les modifications apportées aux fichiers, la méthode <xref:Microsoft.Extensions.FileProviders.IFileProvider> de <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> crée un `IChangeToken` pour les fichiers ou le dossier à surveiller.</span><span class="sxs-lookup"><span data-stu-id="9948b-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="9948b-131">Les jetons `IChangeToken` peuvent être ajoutés aux entrées du cache pour déclencher des suppressions dans le cache en cas de modification.</span><span class="sxs-lookup"><span data-stu-id="9948b-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="9948b-132">Pour les modifications `TOptions`, l’implémentation <xref:Microsoft.Extensions.Options.OptionsMonitor`1> par défaut de <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> a une surcharge qui accepte une ou plusieurs instances <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>.</span><span class="sxs-lookup"><span data-stu-id="9948b-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="9948b-133">Chaque instance retourne un `IChangeToken` pour inscrire un rappel de notification de modification pour les modifications des options de suivi.</span><span class="sxs-lookup"><span data-stu-id="9948b-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="9948b-134">Surveiller les modifications de configuration</span><span class="sxs-lookup"><span data-stu-id="9948b-134">Monitor for configuration changes</span></span>

<span data-ttu-id="9948b-135">Par défaut, les modèles ASP.NET Core utilisent des [fichiers de configuration JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json* et *appsettings.Production.json*) pour charger les paramètres de configuration des applications.</span><span class="sxs-lookup"><span data-stu-id="9948b-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="9948b-136">Ces fichiers sont configurés avec la méthode d’extension [AddJsonFile(IConfigurationBuilder, chaîne, booléen, booléen)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) sur <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> qui accepte un paramètre `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="9948b-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="9948b-137">`reloadOnChange` indique si la configuration doit être rechargée en cas de modification d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="9948b-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="9948b-138">Ce paramètre s’affiche dans la méthode pratique <xref:Microsoft.Extensions.Hosting.Host> <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> :</span><span class="sxs-lookup"><span data-stu-id="9948b-138">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="9948b-139">La configuration basée sur les fichiers est représentée par <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="9948b-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="9948b-140">`FileConfigurationSource` utilise <xref:Microsoft.Extensions.FileProviders.IFileProvider> pour surveiller les fichiers.</span><span class="sxs-lookup"><span data-stu-id="9948b-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="9948b-141">Par défaut, `IFileMonitor` est fourni par un <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, qui utilise <xref:System.IO.FileSystemWatcher> pour surveiller les modifications des fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="9948b-142">L’exemple d’application montre les deux implémentations de la surveillance des modifications de configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="9948b-143">Si l’un des fichiers *appsettings* change, les deux implémentations d’analyse du fichier exécutent du code personnalisé&mdash;l’exemple d’application écrit un message dans la console.</span><span class="sxs-lookup"><span data-stu-id="9948b-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="9948b-144">Le `FileSystemWatcher` d’un fichier de configuration peut déclencher plusieurs rappels de jeton pour une même modification du fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="9948b-145">Pour vous assurer que le code personnalisé n’est exécuté qu’une seule fois lors du déclenchement de plusieurs rappels de jeton, l’implémentation de l’exemple vérifie les hachages des fichiers.</span><span class="sxs-lookup"><span data-stu-id="9948b-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="9948b-146">L’exemple utilise le hachage de fichier SHA1.</span><span class="sxs-lookup"><span data-stu-id="9948b-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="9948b-147">Une nouvelle tentative est implémentée avec une interruption d’une durée exponentielle.</span><span class="sxs-lookup"><span data-stu-id="9948b-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="9948b-148">Une nouvelle tentative est effectuée, car il peut se produire un verrouillage du fichier qui empêche temporairement le calcul d’un nouveau hachage sur un fichier.</span><span class="sxs-lookup"><span data-stu-id="9948b-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="9948b-149">*Utilities/Utilities.cs* :</span><span class="sxs-lookup"><span data-stu-id="9948b-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="9948b-150">Jeton de modification de démarrage simple</span><span class="sxs-lookup"><span data-stu-id="9948b-150">Simple startup change token</span></span>

<span data-ttu-id="9948b-151">Inscrivez un rappel `Action` de consommateur de jeton pour les notifications de modification au jeton de rechargement de configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="9948b-152">Dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9948b-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="9948b-153">`config.GetReloadToken()` fournit le jeton.</span><span class="sxs-lookup"><span data-stu-id="9948b-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="9948b-154">Le rappel est la méthode `InvokeChanged` :</span><span class="sxs-lookup"><span data-stu-id="9948b-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="9948b-155">Le `state` du rappel est utilisé pour transmettre le `IWebHostEnvironment`, ce qui est utile pour spécifier le bon fichier de configuration *appsettings* à surveiller (par exemple, *appsettings.Development.json* dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="9948b-155">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="9948b-156">Les hachages de fichier sont utilisés pour empêcher l’instruction `WriteConsole` d’être exécutée plusieurs fois en raison de plusieurs rappels de jeton alors que le fichier de configuration n’a été modifié qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="9948b-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="9948b-157">Ce système s’exécute tant que l’application est en cours d’exécution et ne peut pas être désactivé par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9948b-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="9948b-158">Surveiller les modifications de configuration en tant que service</span><span class="sxs-lookup"><span data-stu-id="9948b-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="9948b-159">L’exemple implémente :</span><span class="sxs-lookup"><span data-stu-id="9948b-159">The sample implements:</span></span>

* <span data-ttu-id="9948b-160">Surveillance de jeton du démarrage de base.</span><span class="sxs-lookup"><span data-stu-id="9948b-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="9948b-161">Surveillance en tant que service.</span><span class="sxs-lookup"><span data-stu-id="9948b-161">Monitoring as a service.</span></span>
* <span data-ttu-id="9948b-162">Un mécanisme pour activer et désactiver la surveillance.</span><span class="sxs-lookup"><span data-stu-id="9948b-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="9948b-163">L’exemple établit une interface `IConfigurationMonitor`.</span><span class="sxs-lookup"><span data-stu-id="9948b-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="9948b-164">*Extensions/Configurationmonitor.cs* :</span><span class="sxs-lookup"><span data-stu-id="9948b-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="9948b-165">Le constructeur de la classe implémentée, `ConfigurationMonitor`, inscrit un rappel pour les notifications de modification :</span><span class="sxs-lookup"><span data-stu-id="9948b-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="9948b-166">`config.GetReloadToken()` fournit le jeton.</span><span class="sxs-lookup"><span data-stu-id="9948b-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="9948b-167">`InvokeChanged` est la méthode de rappel.</span><span class="sxs-lookup"><span data-stu-id="9948b-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="9948b-168">Le `state` dans cette instance est une référence à l’instance `IConfigurationMonitor` qui est utilisée pour accéder à l’état du monitoring.</span><span class="sxs-lookup"><span data-stu-id="9948b-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="9948b-169">Deux propriétés sont utilisées :</span><span class="sxs-lookup"><span data-stu-id="9948b-169">Two properties are used:</span></span>

* <span data-ttu-id="9948b-170">`MonitoringEnabled` &ndash; Indique si le rappel doit exécuter son code personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9948b-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="9948b-171">`CurrentState` &ndash; Décrit l’état actuel de la surveillance pour une utilisation dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9948b-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="9948b-172">La méthode `InvokeChanged` est similaire à l’approche précédente, excepté que :</span><span class="sxs-lookup"><span data-stu-id="9948b-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="9948b-173">Elle n’exécute pas son code, sauf si `MonitoringEnabled` est `true`.</span><span class="sxs-lookup"><span data-stu-id="9948b-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="9948b-174">Elle indique le `state` actuel dans sa sortie `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="9948b-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="9948b-175">Une instance `ConfigurationMonitor` est inscrite en tant que service dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="9948b-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="9948b-176">La page Index permet à l’utilisateur de contrôler la surveillance de la configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="9948b-177">L’instance de `IConfigurationMonitor` est injectée dans le `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="9948b-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="9948b-178">*Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="9948b-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="9948b-179">Le moniteur de configuration (`_monitor`) est utilisé pour activer ou désactiver l’analyse et définir l’état actuel pour les commentaires sur l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="9948b-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="9948b-180">Quand `OnPostStartMonitoring` est déclenché, la surveillance est activée et l’état actuel est effacé.</span><span class="sxs-lookup"><span data-stu-id="9948b-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="9948b-181">Quand `OnPostStopMonitoring` est déclenché, la surveillance est désactivée et l’état est défini de façon à indiquer que la surveillance n’est pas effectuée.</span><span class="sxs-lookup"><span data-stu-id="9948b-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="9948b-182">Des boutons dans l’IU activent et désactivent la surveillance.</span><span class="sxs-lookup"><span data-stu-id="9948b-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="9948b-183">*Pages/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9948b-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="9948b-184">Surveiller les modifications de fichiers mis en cache</span><span class="sxs-lookup"><span data-stu-id="9948b-184">Monitor cached file changes</span></span>

<span data-ttu-id="9948b-185">Le contenu des fichiers peut être mis en cache en mémoire avec <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="9948b-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="9948b-186">La mise en cache en mémoire est décrite dans la rubrique [Mise en cache en mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="9948b-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="9948b-187">Sans actions supplémentaires, comme l’implémentation décrite ci-dessous, les données *périmées* (obsolètes) sont retournées depuis un cache si la source de données change.</span><span class="sxs-lookup"><span data-stu-id="9948b-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="9948b-188">Par exemple, le fait de ne pas prendre en compte l’état d’un fichier source en cache lors du renouvellement d’une période [d’expiration décalée](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) rend obsolètes les données du fichier mis cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="9948b-189">Chaque demande de données renouvelle la période d’expiration décalée, mais le fichier n’est jamais rechargé dans le cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="9948b-190">Toutes les fonctionnalités d’une application qui utilisent le contenu mis en cache d’un fichier sont exposées au risque de recevoir du contenu obsolète.</span><span class="sxs-lookup"><span data-stu-id="9948b-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="9948b-191">L’utilisation de jetons de modification dans un scénario de mise en cache de fichier empêche la présence de contenu obsolète dans le cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="9948b-192">L’exemple d’application montre une implémentation de l’approche.</span><span class="sxs-lookup"><span data-stu-id="9948b-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="9948b-193">L’exemple utilise `GetFileContent` pour :</span><span class="sxs-lookup"><span data-stu-id="9948b-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="9948b-194">Retourner le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="9948b-194">Return file content.</span></span>
* <span data-ttu-id="9948b-195">Implémenter un algorithme de nouvelle tentative avec interruption exponentielle pour couvrir les cas où un verrou de fichier empêche temporairement la lecture d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="9948b-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="9948b-196">*Utilities/Utilities.cs* :</span><span class="sxs-lookup"><span data-stu-id="9948b-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="9948b-197">Un `FileService` est créé pour gérer les recherches des fichiers mis en cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="9948b-198">L’appel de la méthode `GetFileContent` du service tente d’obtenir le contenu du fichier à partir du cache en mémoire et le retourne à l’appelant (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="9948b-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="9948b-199">Si le contenu en cache n’est pas trouvé avec la clé du cache, les actions suivantes sont effectuées :</span><span class="sxs-lookup"><span data-stu-id="9948b-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="9948b-200">Le contenu du fichier est obtenu à l’aide de `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="9948b-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="9948b-201">Un jeton de modification est obtenu auprès du fournisseur du fichier avec [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="9948b-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="9948b-202">Le rappel du jeton est déclenché quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="9948b-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="9948b-203">Le contenu du fichier est mis en cache avec une période [d’expiration décalée](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration).</span><span class="sxs-lookup"><span data-stu-id="9948b-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="9948b-204">Le jeton de modification est attaché avec [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) pour supprimer l’entrée du cache si le fichier change alors qu’il est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="9948b-205">Dans l’exemple suivant, les fichiers sont stockés dans la [racine de contenu](xref:fundamentals/index#content-root)de l’application.</span><span class="sxs-lookup"><span data-stu-id="9948b-205">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="9948b-206">`IWebHostEnvironment.ContentRootFileProvider` est utilisé pour obtenir un <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointant sur le `IWebHostEnvironment.ContentRootPath`de l’application.</span><span class="sxs-lookup"><span data-stu-id="9948b-206">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="9948b-207">`filePath` est obtenue avec [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="9948b-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="9948b-208">`FileService` est inscrit dans le conteneur de service, ainsi que le service de mise en cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="9948b-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="9948b-209">Dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9948b-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="9948b-210">Le modèle de page charge le contenu du fichier en utilisant le service.</span><span class="sxs-lookup"><span data-stu-id="9948b-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="9948b-211">Dans la méthode `OnGet` de la page Index (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9948b-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="9948b-212">Classe CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="9948b-212">CompositeChangeToken class</span></span>

<span data-ttu-id="9948b-213">Pour représenter une ou plusieurs instances de `IChangeToken` dans un même objet, utilisez la classe <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="9948b-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="9948b-214">`HasChanged` sur le jeton composite indique `true` si l’élément `HasChanged` d’un jeton représenté est `true`.</span><span class="sxs-lookup"><span data-stu-id="9948b-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="9948b-215">`ActiveChangeCallbacks` sur le jeton composite indique `true` si l’élément `ActiveChangeCallbacks` d’un jeton représenté est `true`.</span><span class="sxs-lookup"><span data-stu-id="9948b-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="9948b-216">Si plusieurs modifications simultanées se produisent, le rappel de modification composite n’est appelé qu’une fois.</span><span class="sxs-lookup"><span data-stu-id="9948b-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9948b-217">Un *jeton de modification* est un module à usage général de bas niveau, utilisé pour effectuer le suivi des modifications de l’état.</span><span class="sxs-lookup"><span data-stu-id="9948b-217">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="9948b-218">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9948b-218">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="9948b-219">Interface d’IChangeToken</span><span class="sxs-lookup"><span data-stu-id="9948b-219">IChangeToken interface</span></span>

<span data-ttu-id="9948b-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propage des notifications indiquant qu’une modification s’est produite.</span><span class="sxs-lookup"><span data-stu-id="9948b-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="9948b-221">`IChangeToken` réside dans l’espace de noms <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="9948b-221">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="9948b-222">Pour les applications qui n’utilisent pas le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), créez une référence au package NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/).</span><span class="sxs-lookup"><span data-stu-id="9948b-222">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="9948b-223">`IChangeToken` a deux propriétés :</span><span class="sxs-lookup"><span data-stu-id="9948b-223">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="9948b-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indique si le jeton déclenche des rappels de façon proactive.</span><span class="sxs-lookup"><span data-stu-id="9948b-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="9948b-225">Si `ActiveChangedCallbacks` est défini sur `false`, un rappel n’est jamais appelé, et l’application doit interroger `HasChanged` pour connaître les modifications.</span><span class="sxs-lookup"><span data-stu-id="9948b-225">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="9948b-226">Il est également possible qu’un jeton ne soit jamais annulé si aucune modification ne se produit, ou si l’écouteur de modifications sous-jacent est supprimé ou désactivé.</span><span class="sxs-lookup"><span data-stu-id="9948b-226">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="9948b-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> obtient une valeur qui indique si une modification a eu lieu.</span><span class="sxs-lookup"><span data-stu-id="9948b-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="9948b-228">L’interface `IChangeToken` inclut la méthode [RegisterChangeCallback(Action\<Objet>, Objet)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*), qui inscrit un rappel qui est appelé quand le jeton a été modifié.</span><span class="sxs-lookup"><span data-stu-id="9948b-228">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="9948b-229">`HasChanged` doit être défini avant que le rappel soit appelé.</span><span class="sxs-lookup"><span data-stu-id="9948b-229">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="9948b-230">Classe ChangeToken</span><span class="sxs-lookup"><span data-stu-id="9948b-230">ChangeToken class</span></span>

<span data-ttu-id="9948b-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> est une classe statique utilisée pour propager des notifications indiquant qu’une modification s’est produite.</span><span class="sxs-lookup"><span data-stu-id="9948b-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="9948b-232">`ChangeToken` réside dans l’espace de noms <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="9948b-232">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="9948b-233">Pour les applications qui n’utilisent pas le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), créez une référence au package NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/).</span><span class="sxs-lookup"><span data-stu-id="9948b-233">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="9948b-234">La méthode [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) inscrit une `Action` à appeler chaque fois que le jeton change :</span><span class="sxs-lookup"><span data-stu-id="9948b-234">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="9948b-235">`Func<IChangeToken>` produit le jeton.</span><span class="sxs-lookup"><span data-stu-id="9948b-235">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="9948b-236">`Action` est appelée quand le jeton change.</span><span class="sxs-lookup"><span data-stu-id="9948b-236">`Action` is called when the token changes.</span></span>

<span data-ttu-id="9948b-237">La surcharge [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) prend un paramètre `TState` supplémentaire qui est passé dans le consommateur du jeton `Action`.</span><span class="sxs-lookup"><span data-stu-id="9948b-237">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="9948b-238">`OnChange`Retourne un <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="9948b-238">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="9948b-239">L’appel de <xref:System.IDisposable.Dispose*> fait que le jeton cesse d’être à l’écoute des modifications suivantes et libère les ressources du jeton.</span><span class="sxs-lookup"><span data-stu-id="9948b-239">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="9948b-240">Exemples d’utilisation de jetons de modification dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9948b-240">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="9948b-241">Les jetons de modification sont utilisés dans des zones importantes d’ASP.NET Core pour surveiller les modifications apportées aux objets :</span><span class="sxs-lookup"><span data-stu-id="9948b-241">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="9948b-242">Pour surveiller les modifications apportées aux fichiers, la méthode <xref:Microsoft.Extensions.FileProviders.IFileProvider> de <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> crée un `IChangeToken` pour les fichiers ou le dossier à surveiller.</span><span class="sxs-lookup"><span data-stu-id="9948b-242">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="9948b-243">Les jetons `IChangeToken` peuvent être ajoutés aux entrées du cache pour déclencher des suppressions dans le cache en cas de modification.</span><span class="sxs-lookup"><span data-stu-id="9948b-243">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="9948b-244">Pour les modifications `TOptions`, l’implémentation <xref:Microsoft.Extensions.Options.OptionsMonitor`1> par défaut de <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> a une surcharge qui accepte une ou plusieurs instances <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>.</span><span class="sxs-lookup"><span data-stu-id="9948b-244">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="9948b-245">Chaque instance retourne un `IChangeToken` pour inscrire un rappel de notification de modification pour les modifications des options de suivi.</span><span class="sxs-lookup"><span data-stu-id="9948b-245">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="9948b-246">Surveiller les modifications de configuration</span><span class="sxs-lookup"><span data-stu-id="9948b-246">Monitor for configuration changes</span></span>

<span data-ttu-id="9948b-247">Par défaut, les modèles ASP.NET Core utilisent des [fichiers de configuration JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json* et *appsettings.Production.json*) pour charger les paramètres de configuration des applications.</span><span class="sxs-lookup"><span data-stu-id="9948b-247">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="9948b-248">Ces fichiers sont configurés avec la méthode d’extension [AddJsonFile(IConfigurationBuilder, chaîne, booléen, booléen)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) sur <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> qui accepte un paramètre `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="9948b-248">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="9948b-249">`reloadOnChange` indique si la configuration doit être rechargée en cas de modification d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="9948b-249">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="9948b-250">Ce paramètre s’affiche dans la méthode pratique <xref:Microsoft.AspNetCore.WebHost> <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> :</span><span class="sxs-lookup"><span data-stu-id="9948b-250">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="9948b-251">La configuration basée sur les fichiers est représentée par <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="9948b-251">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="9948b-252">`FileConfigurationSource` utilise <xref:Microsoft.Extensions.FileProviders.IFileProvider> pour surveiller les fichiers.</span><span class="sxs-lookup"><span data-stu-id="9948b-252">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="9948b-253">Par défaut, `IFileMonitor` est fourni par un <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, qui utilise <xref:System.IO.FileSystemWatcher> pour surveiller les modifications des fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-253">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="9948b-254">L’exemple d’application montre les deux implémentations de la surveillance des modifications de configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-254">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="9948b-255">Si l’un des fichiers *appsettings* change, les deux implémentations d’analyse du fichier exécutent du code personnalisé&mdash;l’exemple d’application écrit un message dans la console.</span><span class="sxs-lookup"><span data-stu-id="9948b-255">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="9948b-256">Le `FileSystemWatcher` d’un fichier de configuration peut déclencher plusieurs rappels de jeton pour une même modification du fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-256">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="9948b-257">Pour vous assurer que le code personnalisé n’est exécuté qu’une seule fois lors du déclenchement de plusieurs rappels de jeton, l’implémentation de l’exemple vérifie les hachages des fichiers.</span><span class="sxs-lookup"><span data-stu-id="9948b-257">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="9948b-258">L’exemple utilise le hachage de fichier SHA1.</span><span class="sxs-lookup"><span data-stu-id="9948b-258">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="9948b-259">Une nouvelle tentative est implémentée avec une interruption d’une durée exponentielle.</span><span class="sxs-lookup"><span data-stu-id="9948b-259">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="9948b-260">Une nouvelle tentative est effectuée, car il peut se produire un verrouillage du fichier qui empêche temporairement le calcul d’un nouveau hachage sur un fichier.</span><span class="sxs-lookup"><span data-stu-id="9948b-260">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="9948b-261">*Utilities/Utilities.cs* :</span><span class="sxs-lookup"><span data-stu-id="9948b-261">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="9948b-262">Jeton de modification de démarrage simple</span><span class="sxs-lookup"><span data-stu-id="9948b-262">Simple startup change token</span></span>

<span data-ttu-id="9948b-263">Inscrivez un rappel `Action` de consommateur de jeton pour les notifications de modification au jeton de rechargement de configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-263">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="9948b-264">Dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9948b-264">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="9948b-265">`config.GetReloadToken()` fournit le jeton.</span><span class="sxs-lookup"><span data-stu-id="9948b-265">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="9948b-266">Le rappel est la méthode `InvokeChanged` :</span><span class="sxs-lookup"><span data-stu-id="9948b-266">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="9948b-267">Le `state` du rappel est utilisé pour transmettre le `IHostingEnvironment`, ce qui est utile pour spécifier le bon fichier de configuration *appsettings* à surveiller (par exemple, *appsettings.Development.json* dans l’environnement de développement).</span><span class="sxs-lookup"><span data-stu-id="9948b-267">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="9948b-268">Les hachages de fichier sont utilisés pour empêcher l’instruction `WriteConsole` d’être exécutée plusieurs fois en raison de plusieurs rappels de jeton alors que le fichier de configuration n’a été modifié qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="9948b-268">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="9948b-269">Ce système s’exécute tant que l’application est en cours d’exécution et ne peut pas être désactivé par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9948b-269">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="9948b-270">Surveiller les modifications de configuration en tant que service</span><span class="sxs-lookup"><span data-stu-id="9948b-270">Monitor configuration changes as a service</span></span>

<span data-ttu-id="9948b-271">L’exemple implémente :</span><span class="sxs-lookup"><span data-stu-id="9948b-271">The sample implements:</span></span>

* <span data-ttu-id="9948b-272">Surveillance de jeton du démarrage de base.</span><span class="sxs-lookup"><span data-stu-id="9948b-272">Basic startup token monitoring.</span></span>
* <span data-ttu-id="9948b-273">Surveillance en tant que service.</span><span class="sxs-lookup"><span data-stu-id="9948b-273">Monitoring as a service.</span></span>
* <span data-ttu-id="9948b-274">Un mécanisme pour activer et désactiver la surveillance.</span><span class="sxs-lookup"><span data-stu-id="9948b-274">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="9948b-275">L’exemple établit une interface `IConfigurationMonitor`.</span><span class="sxs-lookup"><span data-stu-id="9948b-275">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="9948b-276">*Extensions/Configurationmonitor.cs* :</span><span class="sxs-lookup"><span data-stu-id="9948b-276">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="9948b-277">Le constructeur de la classe implémentée, `ConfigurationMonitor`, inscrit un rappel pour les notifications de modification :</span><span class="sxs-lookup"><span data-stu-id="9948b-277">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="9948b-278">`config.GetReloadToken()` fournit le jeton.</span><span class="sxs-lookup"><span data-stu-id="9948b-278">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="9948b-279">`InvokeChanged` est la méthode de rappel.</span><span class="sxs-lookup"><span data-stu-id="9948b-279">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="9948b-280">Le `state` dans cette instance est une référence à l’instance `IConfigurationMonitor` qui est utilisée pour accéder à l’état du monitoring.</span><span class="sxs-lookup"><span data-stu-id="9948b-280">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="9948b-281">Deux propriétés sont utilisées :</span><span class="sxs-lookup"><span data-stu-id="9948b-281">Two properties are used:</span></span>

* <span data-ttu-id="9948b-282">`MonitoringEnabled` &ndash; Indique si le rappel doit exécuter son code personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9948b-282">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="9948b-283">`CurrentState` &ndash; Décrit l’état actuel de la surveillance pour une utilisation dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9948b-283">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="9948b-284">La méthode `InvokeChanged` est similaire à l’approche précédente, excepté que :</span><span class="sxs-lookup"><span data-stu-id="9948b-284">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="9948b-285">Elle n’exécute pas son code, sauf si `MonitoringEnabled` est `true`.</span><span class="sxs-lookup"><span data-stu-id="9948b-285">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="9948b-286">Elle indique le `state` actuel dans sa sortie `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="9948b-286">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="9948b-287">Une instance `ConfigurationMonitor` est inscrite en tant que service dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="9948b-287">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="9948b-288">La page Index permet à l’utilisateur de contrôler la surveillance de la configuration.</span><span class="sxs-lookup"><span data-stu-id="9948b-288">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="9948b-289">L’instance de `IConfigurationMonitor` est injectée dans le `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="9948b-289">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="9948b-290">*Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="9948b-290">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="9948b-291">Le moniteur de configuration (`_monitor`) est utilisé pour activer ou désactiver l’analyse et définir l’état actuel pour les commentaires sur l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="9948b-291">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="9948b-292">Quand `OnPostStartMonitoring` est déclenché, la surveillance est activée et l’état actuel est effacé.</span><span class="sxs-lookup"><span data-stu-id="9948b-292">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="9948b-293">Quand `OnPostStopMonitoring` est déclenché, la surveillance est désactivée et l’état est défini de façon à indiquer que la surveillance n’est pas effectuée.</span><span class="sxs-lookup"><span data-stu-id="9948b-293">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="9948b-294">Des boutons dans l’IU activent et désactivent la surveillance.</span><span class="sxs-lookup"><span data-stu-id="9948b-294">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="9948b-295">*Pages/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9948b-295">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="9948b-296">Surveiller les modifications de fichiers mis en cache</span><span class="sxs-lookup"><span data-stu-id="9948b-296">Monitor cached file changes</span></span>

<span data-ttu-id="9948b-297">Le contenu des fichiers peut être mis en cache en mémoire avec <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="9948b-297">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="9948b-298">La mise en cache en mémoire est décrite dans la rubrique [Mise en cache en mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="9948b-298">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="9948b-299">Sans actions supplémentaires, comme l’implémentation décrite ci-dessous, les données *périmées* (obsolètes) sont retournées depuis un cache si la source de données change.</span><span class="sxs-lookup"><span data-stu-id="9948b-299">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="9948b-300">Par exemple, le fait de ne pas prendre en compte l’état d’un fichier source en cache lors du renouvellement d’une période [d’expiration décalée](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) rend obsolètes les données du fichier mis cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-300">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="9948b-301">Chaque demande de données renouvelle la période d’expiration décalée, mais le fichier n’est jamais rechargé dans le cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-301">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="9948b-302">Toutes les fonctionnalités d’une application qui utilisent le contenu mis en cache d’un fichier sont exposées au risque de recevoir du contenu obsolète.</span><span class="sxs-lookup"><span data-stu-id="9948b-302">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="9948b-303">L’utilisation de jetons de modification dans un scénario de mise en cache de fichier empêche la présence de contenu obsolète dans le cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-303">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="9948b-304">L’exemple d’application montre une implémentation de l’approche.</span><span class="sxs-lookup"><span data-stu-id="9948b-304">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="9948b-305">L’exemple utilise `GetFileContent` pour :</span><span class="sxs-lookup"><span data-stu-id="9948b-305">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="9948b-306">Retourner le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="9948b-306">Return file content.</span></span>
* <span data-ttu-id="9948b-307">Implémenter un algorithme de nouvelle tentative avec interruption exponentielle pour couvrir les cas où un verrou de fichier empêche temporairement la lecture d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="9948b-307">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="9948b-308">*Utilities/Utilities.cs* :</span><span class="sxs-lookup"><span data-stu-id="9948b-308">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="9948b-309">Un `FileService` est créé pour gérer les recherches des fichiers mis en cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-309">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="9948b-310">L’appel de la méthode `GetFileContent` du service tente d’obtenir le contenu du fichier à partir du cache en mémoire et le retourne à l’appelant (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="9948b-310">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="9948b-311">Si le contenu en cache n’est pas trouvé avec la clé du cache, les actions suivantes sont effectuées :</span><span class="sxs-lookup"><span data-stu-id="9948b-311">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="9948b-312">Le contenu du fichier est obtenu à l’aide de `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="9948b-312">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="9948b-313">Un jeton de modification est obtenu auprès du fournisseur du fichier avec [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="9948b-313">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="9948b-314">Le rappel du jeton est déclenché quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="9948b-314">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="9948b-315">Le contenu du fichier est mis en cache avec une période [d’expiration décalée](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration).</span><span class="sxs-lookup"><span data-stu-id="9948b-315">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="9948b-316">Le jeton de modification est attaché avec [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) pour supprimer l’entrée du cache si le fichier change alors qu’il est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="9948b-316">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="9948b-317">Dans l’exemple suivant, les fichiers sont stockés dans la [racine de contenu](xref:fundamentals/index#content-root)de l’application.</span><span class="sxs-lookup"><span data-stu-id="9948b-317">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="9948b-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) est utilisé pour obtenir un <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointant vers <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> de l’application.</span><span class="sxs-lookup"><span data-stu-id="9948b-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="9948b-319">`filePath` est obtenue avec [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="9948b-319">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="9948b-320">`FileService` est inscrit dans le conteneur de service, ainsi que le service de mise en cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="9948b-320">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="9948b-321">Dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9948b-321">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="9948b-322">Le modèle de page charge le contenu du fichier en utilisant le service.</span><span class="sxs-lookup"><span data-stu-id="9948b-322">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="9948b-323">Dans la méthode `OnGet` de la page Index (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="9948b-323">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="9948b-324">Classe CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="9948b-324">CompositeChangeToken class</span></span>

<span data-ttu-id="9948b-325">Pour représenter une ou plusieurs instances de `IChangeToken` dans un même objet, utilisez la classe <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="9948b-325">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="9948b-326">`HasChanged` sur le jeton composite indique `true` si l’élément `HasChanged` d’un jeton représenté est `true`.</span><span class="sxs-lookup"><span data-stu-id="9948b-326">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="9948b-327">`ActiveChangeCallbacks` sur le jeton composite indique `true` si l’élément `ActiveChangeCallbacks` d’un jeton représenté est `true`.</span><span class="sxs-lookup"><span data-stu-id="9948b-327">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="9948b-328">Si plusieurs modifications simultanées se produisent, le rappel de modification composite n’est appelé qu’une fois.</span><span class="sxs-lookup"><span data-stu-id="9948b-328">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9948b-329">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9948b-329">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
