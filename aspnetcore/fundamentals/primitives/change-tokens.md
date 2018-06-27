---
title: Détecter les modifications avec des jetons de modification dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser des jetons de modification pour effectuer le suivi des modifications.
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: d132747cb7c92ef5afac4664c91634a4ad290e5f
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734521"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="3de84-103">Détecter les modifications avec des jetons de modification dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3de84-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="3de84-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3de84-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3de84-105">Un *jeton de modification* est un module à usage général de bas niveau, utilisé pour effectuer le suivi des modifications.</span><span class="sxs-lookup"><span data-stu-id="3de84-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="3de84-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3de84-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="3de84-107">Interface d’IChangeToken</span><span class="sxs-lookup"><span data-stu-id="3de84-107">IChangeToken interface</span></span>

<span data-ttu-id="3de84-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propage des notifications indiquant qu’une modification s’est produite.</span><span class="sxs-lookup"><span data-stu-id="3de84-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="3de84-109">`IChangeToken` se trouve dans l’espace de noms [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives).</span><span class="sxs-lookup"><span data-stu-id="3de84-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="3de84-110">Pour les applications qui n’utilisent pas le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou version ultérieure), référencez le package NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="3de84-110">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="3de84-111">`IChangeToken` a deux propriétés :</span><span class="sxs-lookup"><span data-stu-id="3de84-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="3de84-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indique si le jeton déclenche des rappels de façon proactive.</span><span class="sxs-lookup"><span data-stu-id="3de84-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="3de84-113">Si `ActiveChangedCallbacks` est défini sur `false`, un rappel n’est jamais appelé, et l’application doit interroger `HasChanged` pour connaître les modifications.</span><span class="sxs-lookup"><span data-stu-id="3de84-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="3de84-114">Il est également possible qu’un jeton ne soit jamais annulé si aucune modification ne se produit, ou si l’écouteur de modifications sous-jacent est supprimé ou désactivé.</span><span class="sxs-lookup"><span data-stu-id="3de84-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="3de84-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) obtient une valeur qui indique si une modification a eu lieu.</span><span class="sxs-lookup"><span data-stu-id="3de84-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="3de84-116">L’interface a une méthode, [RegisterChangeCallback(Action&lt;objet&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), qui inscrit un rappel qui est appelé quand le jeton a été modifié.</span><span class="sxs-lookup"><span data-stu-id="3de84-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="3de84-117">`HasChanged` doit être défini avant que le rappel soit appelé.</span><span class="sxs-lookup"><span data-stu-id="3de84-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="3de84-118">Classe ChangeToken</span><span class="sxs-lookup"><span data-stu-id="3de84-118">ChangeToken class</span></span>

<span data-ttu-id="3de84-119">`ChangeToken` est une classe statique utilisée pour propager des notifications indiquant qu’une modification s’est produite.</span><span class="sxs-lookup"><span data-stu-id="3de84-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="3de84-120">`ChangeToken` se trouve dans l’espace de noms [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives).</span><span class="sxs-lookup"><span data-stu-id="3de84-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="3de84-121">Pour les applications qui n’utilisent pas le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), référencez le package NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="3de84-121">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="3de84-122">La méthode `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) inscrit une `Action` à appeler chaque fois que le jeton change :</span><span class="sxs-lookup"><span data-stu-id="3de84-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="3de84-123">`Func<IChangeToken>` produit le jeton.</span><span class="sxs-lookup"><span data-stu-id="3de84-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="3de84-124">`Action` est appelée quand le jeton change.</span><span class="sxs-lookup"><span data-stu-id="3de84-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="3de84-125">`ChangeToken` a une surcharge [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) qui prend un paramètre `TState` supplémentaire passé dans l’élément `Action` du consommateur du jeton.</span><span class="sxs-lookup"><span data-stu-id="3de84-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="3de84-126">`OnChange` retourne un [IDisposable](/dotnet/api/system.idisposable).</span><span class="sxs-lookup"><span data-stu-id="3de84-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="3de84-127">L’appel de [Dispose](/dotnet/api/system.idisposable.dispose) fait que le jeton cesse d’être à l’écoute des modifications suivantes et libère les ressources du jeton.</span><span class="sxs-lookup"><span data-stu-id="3de84-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="3de84-128">Exemples d’utilisation de jetons de modification dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3de84-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="3de84-129">Les jetons de modification sont utilisés dans des zones importantes d’ASP.NET Core pour la surveillance des modifications apportées aux objets :</span><span class="sxs-lookup"><span data-stu-id="3de84-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="3de84-130">Pour surveiller les modifications apportées aux fichiers, la méthode [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) de [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) crée un `IChangeToken` pour les fichiers ou le dossier à surveiller.</span><span class="sxs-lookup"><span data-stu-id="3de84-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="3de84-131">Les jetons `IChangeToken` peuvent être ajoutés aux entrées du cache pour déclencher des suppressions dans le cache en cas de modification.</span><span class="sxs-lookup"><span data-stu-id="3de84-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="3de84-132">Pour les modifications `TOptions`, l’implémentation par défaut de [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) de [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) a une surcharge qui accepte une ou plusieurs instances de [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1).</span><span class="sxs-lookup"><span data-stu-id="3de84-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="3de84-133">Chaque instance retourne un `IChangeToken` pour inscrire un rappel de notification de modification pour les modifications des options de suivi.</span><span class="sxs-lookup"><span data-stu-id="3de84-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="3de84-134">Surveillance des modifications de configuration</span><span class="sxs-lookup"><span data-stu-id="3de84-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="3de84-135">Par défaut, les modèles ASP.NET Core utilisent des [fichiers de configuration JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json* et *appsettings.Production.json*) pour charger les paramètres de configuration des applications.</span><span class="sxs-lookup"><span data-stu-id="3de84-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="3de84-136">Ces fichiers sont configurés avec la méthode d’extension [AddJsonFile(IConfigurationBuilder, chaîne, booléen, booléen)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) sur [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) qui accepte un paramètre `reloadOnChange` (ASP.NET Core 1.1 et ultérieur).</span><span class="sxs-lookup"><span data-stu-id="3de84-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="3de84-137">`reloadOnChange` indique si la configuration doit être rechargée en cas de modification d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="3de84-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="3de84-138">Vous pouvez voir ce paramètre dans la méthode pratique [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) de [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) :</span><span class="sxs-lookup"><span data-stu-id="3de84-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="3de84-139">La configuration basée sur les fichiers est représentée par [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span><span class="sxs-lookup"><span data-stu-id="3de84-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="3de84-140">`FileConfigurationSource` utilise [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) pour surveiller des fichiers.</span><span class="sxs-lookup"><span data-stu-id="3de84-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) to monitor files.</span></span>

<span data-ttu-id="3de84-141">Par défaut, `IFileMonitor` est fourni par un [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), qui utilise [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) pour surveiller les modifications des fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="3de84-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="3de84-142">L’exemple d’application montre les deux implémentations de la surveillance des modifications de configuration.</span><span class="sxs-lookup"><span data-stu-id="3de84-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="3de84-143">Si le fichier *appsettings.json* change ou que la version de l’environnement du fichier change, chaque implémentation exécute du code personnalisé.</span><span class="sxs-lookup"><span data-stu-id="3de84-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="3de84-144">L’exemple d’application écrit un message sur la console.</span><span class="sxs-lookup"><span data-stu-id="3de84-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="3de84-145">Le `FileSystemWatcher` d’un fichier de configuration peut déclencher plusieurs rappels de jeton pour une même modification du fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="3de84-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="3de84-146">L’implémentation de l’exemple protège de ce problème en vérifiant les hachages de fichier sur les fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="3de84-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="3de84-147">La vérification des hachages de fichier garantit qu’au moins un des fichiers de configuration a changé avant d’exécuter le code personnalisé.</span><span class="sxs-lookup"><span data-stu-id="3de84-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="3de84-148">L’exemple utilise le hachage de fichier SHA1 (*Utilities/Utilities.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3de84-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="3de84-149">Une nouvelle tentative est implémentée avec une interruption d’une durée exponentielle.</span><span class="sxs-lookup"><span data-stu-id="3de84-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="3de84-150">Une nouvelle tentative est effectuée, car il peut se produire un verrouillage du fichier qui empêche temporairement le calcul d’un nouveau hachage sur un des fichiers.</span><span class="sxs-lookup"><span data-stu-id="3de84-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="3de84-151">Jeton de modification de démarrage simple</span><span class="sxs-lookup"><span data-stu-id="3de84-151">Simple startup change token</span></span>

<span data-ttu-id="3de84-152">Inscrivez un rappel `Action` de consommateur de jeton pour les notifications de modification au jeton de rechargement de configuration (*Startup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3de84-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="3de84-153">`config.GetReloadToken()` fournit le jeton.</span><span class="sxs-lookup"><span data-stu-id="3de84-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="3de84-154">Le rappel est la méthode `InvokeChanged` :</span><span class="sxs-lookup"><span data-stu-id="3de84-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="3de84-155">Le `state` du rappel est utilisé pour passer le `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="3de84-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="3de84-156">C’est utile pour déterminer le fichier JSON de configuration *appsettings* correct à surveiller, *appsettings.&lt;Environnement&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="3de84-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="3de84-157">Les hachages de fichier sont utilisés pour empêcher l’instruction `WriteConsole` d’être exécutée plusieurs fois en raison de plusieurs rappels de jeton alors que le fichier de configuration n’a été modifié qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="3de84-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="3de84-158">Ce système s’exécute tant que l’application est en cours d’exécution et ne peut pas être désactivé par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3de84-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="3de84-159">Surveillance des modifications de configuration en tant que service</span><span class="sxs-lookup"><span data-stu-id="3de84-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="3de84-160">L’exemple implémente :</span><span class="sxs-lookup"><span data-stu-id="3de84-160">The sample implements:</span></span>

* <span data-ttu-id="3de84-161">Surveillance de jeton du démarrage de base.</span><span class="sxs-lookup"><span data-stu-id="3de84-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="3de84-162">Surveillance en tant que service.</span><span class="sxs-lookup"><span data-stu-id="3de84-162">Monitoring as a service.</span></span>
* <span data-ttu-id="3de84-163">Un mécanisme pour activer et désactiver la surveillance.</span><span class="sxs-lookup"><span data-stu-id="3de84-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="3de84-164">L’exemple établit une interface `IConfigurationMonitor` (*Extensions/ConfigurationMonitor.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3de84-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="3de84-165">Le constructeur de la classe implémentée, `ConfigurationMonitor`, inscrit un rappel pour les notifications de modification :</span><span class="sxs-lookup"><span data-stu-id="3de84-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="3de84-166">`config.GetReloadToken()` fournit le jeton.</span><span class="sxs-lookup"><span data-stu-id="3de84-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="3de84-167">`InvokeChanged` est la méthode de rappel.</span><span class="sxs-lookup"><span data-stu-id="3de84-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="3de84-168">Le `state` dans cette instance est une référence à l’instance `IConfigurationMonitor` qui est utilisée pour accéder à l’état du monitoring.</span><span class="sxs-lookup"><span data-stu-id="3de84-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that is used to access the monitoring state.</span></span> <span data-ttu-id="3de84-169">Deux propriétés sont utilisées :</span><span class="sxs-lookup"><span data-stu-id="3de84-169">Two properties are used:</span></span>

* <span data-ttu-id="3de84-170">`MonitoringEnabled` indique si le rappel doit exécuter son code personnalisé.</span><span class="sxs-lookup"><span data-stu-id="3de84-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="3de84-171">`CurrentState` décrit l’état actuel de la surveillance pour une utilisation dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3de84-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="3de84-172">La méthode `InvokeChanged` est similaire à l’approche précédente, excepté que :</span><span class="sxs-lookup"><span data-stu-id="3de84-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="3de84-173">Elle n’exécute pas son code, sauf si `MonitoringEnabled` est `true`.</span><span class="sxs-lookup"><span data-stu-id="3de84-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="3de84-174">Elle indique le `state` actuel dans sa sortie `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="3de84-174">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="3de84-175">Une instance de `ConfigurationMonitor` est inscrite en tant que service dans `ConfigureServices` de *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="3de84-175">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="3de84-176">La page Index permet à l’utilisateur de contrôler la surveillance de la configuration.</span><span class="sxs-lookup"><span data-stu-id="3de84-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="3de84-177">L’instance de `IConfigurationMonitor` est injectée dans le `IndexModel` :</span><span class="sxs-lookup"><span data-stu-id="3de84-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="3de84-178">Un bouton active et désactive la surveillance :</span><span class="sxs-lookup"><span data-stu-id="3de84-178">A button enables and disables monitoring:</span></span>

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="3de84-179">Quand `OnPostStartMonitoring` est déclenché, la surveillance est activée et l’état actuel est effacé.</span><span class="sxs-lookup"><span data-stu-id="3de84-179">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="3de84-180">Quand `OnPostStopMonitoring` est déclenché, la surveillance est désactivée et l’état est défini de façon à indiquer que la surveillance n’est pas effectuée.</span><span class="sxs-lookup"><span data-stu-id="3de84-180">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="3de84-181">Surveillance des modifications de fichiers mis en cache</span><span class="sxs-lookup"><span data-stu-id="3de84-181">Monitoring cached file changes</span></span>

<span data-ttu-id="3de84-182">Le contenu des fichiers peut être mis en cache en mémoire avec [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span><span class="sxs-lookup"><span data-stu-id="3de84-182">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="3de84-183">La mise en cache en mémoire est décrite dans la rubrique [Mise en cache en mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="3de84-183">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="3de84-184">Sans actions supplémentaires, comme l’implémentation décrite ci-dessous, les données *périmées* (obsolètes) sont retournées depuis un cache si la source de données change.</span><span class="sxs-lookup"><span data-stu-id="3de84-184">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="3de84-185">Le fait de ne pas prendre en compte l’état d’un fichier source en cache lors du renouvellement d’une période [d’expiration décalée](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) rend obsolètes les données du cache.</span><span class="sxs-lookup"><span data-stu-id="3de84-185">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="3de84-186">Chaque demande de données renouvelle la période d’expiration décalée, mais le fichier n’est jamais rechargé dans le cache.</span><span class="sxs-lookup"><span data-stu-id="3de84-186">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="3de84-187">Toutes les fonctionnalités d’une application qui utilisent le contenu mis en cache d’un fichier sont exposées au risque de recevoir du contenu obsolète.</span><span class="sxs-lookup"><span data-stu-id="3de84-187">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="3de84-188">L’utilisation de jetons de modification dans un scénario de mise en cache de fichier empêche la présence de contenu obsolète dans le cache.</span><span class="sxs-lookup"><span data-stu-id="3de84-188">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="3de84-189">L’exemple d’application montre une implémentation de l’approche.</span><span class="sxs-lookup"><span data-stu-id="3de84-189">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="3de84-190">L’exemple utilise `GetFileContent` pour :</span><span class="sxs-lookup"><span data-stu-id="3de84-190">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="3de84-191">Retourner le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="3de84-191">Return file content.</span></span>
* <span data-ttu-id="3de84-192">Implémenter un algorithme de nouvelle tentative avec interruption exponentielle pour couvrir les cas où un verrou de fichier empêche temporairement la lecture d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="3de84-192">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="3de84-193">*Utilities/Utilities.cs* :</span><span class="sxs-lookup"><span data-stu-id="3de84-193">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="3de84-194">Un `FileService` est créé pour gérer les recherches des fichiers mis en cache.</span><span class="sxs-lookup"><span data-stu-id="3de84-194">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="3de84-195">L’appel de la méthode `GetFileContent` du service tente d’obtenir le contenu du fichier à partir du cache en mémoire et le retourne à l’appelant (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="3de84-195">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="3de84-196">Si le contenu en cache n’est pas trouvé avec la clé du cache, les actions suivantes sont effectuées :</span><span class="sxs-lookup"><span data-stu-id="3de84-196">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="3de84-197">Le contenu du fichier est obtenu à l’aide de `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="3de84-197">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="3de84-198">Un jeton de modification est obtenu auprès du fournisseur du fichier avec [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span><span class="sxs-lookup"><span data-stu-id="3de84-198">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="3de84-199">Le rappel du jeton est déclenché quand le fichier est modifié.</span><span class="sxs-lookup"><span data-stu-id="3de84-199">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="3de84-200">Le contenu du fichier est mis en cache avec une période [d’expiration décalée](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration).</span><span class="sxs-lookup"><span data-stu-id="3de84-200">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="3de84-201">Le jeton de modification est attaché avec [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) pour supprimer l’entrée du cache si le fichier change alors qu’il est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="3de84-201">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="3de84-202">Le `FileService` est inscrit dans le conteneur de service, ainsi que le service de mise en cache en mémoire (*Startup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3de84-202">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="3de84-203">Le modèle de page charge le contenu du fichier en utilisant le service (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3de84-203">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="3de84-204">Classe CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="3de84-204">CompositeChangeToken class</span></span>

<span data-ttu-id="3de84-205">Pour représenter une ou plusieurs instances de `IChangeToken` dans un même objet, utilisez la classe [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken).</span><span class="sxs-lookup"><span data-stu-id="3de84-205">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class.</span></span>

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

<span data-ttu-id="3de84-206">`HasChanged` sur le jeton composite indique `true` si l’élément `HasChanged` d’un jeton représenté est `true`.</span><span class="sxs-lookup"><span data-stu-id="3de84-206">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="3de84-207">`ActiveChangeCallbacks` sur le jeton composite indique `true` si l’élément `ActiveChangeCallbacks` d’un jeton représenté est `true`.</span><span class="sxs-lookup"><span data-stu-id="3de84-207">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="3de84-208">Si plusieurs modifications simultanées se produisent, le rappel de modification composite n’est appelé qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="3de84-208">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3de84-209">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3de84-209">Additional resources</span></span>

* [<span data-ttu-id="3de84-210">Mettre en cache en mémoire</span><span class="sxs-lookup"><span data-stu-id="3de84-210">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="3de84-211">Utiliser un cache distribué</span><span class="sxs-lookup"><span data-stu-id="3de84-211">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="3de84-212">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="3de84-212">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="3de84-213">Intergiciel de mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="3de84-213">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="3de84-214">Tag Helper de cache</span><span class="sxs-lookup"><span data-stu-id="3de84-214">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="3de84-215">Tag Helper de cache distribué</span><span class="sxs-lookup"><span data-stu-id="3de84-215">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
