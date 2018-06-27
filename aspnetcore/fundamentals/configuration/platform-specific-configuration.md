---
title: Améliorer une application à partir d’un assembly externe dans ASP.NET Core avec IHostingStartup
author: guardrex
description: Découvrez comment améliorer une application ASP.NET Core à partir d’un assembly externe à l’aide d’une implémentation IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: bd9605dd8efee2c3ba8bc82a81554cace40630be
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278081"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="33f22-103">Améliorer une application à partir d’un assembly externe dans ASP.NET Core avec IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="33f22-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="33f22-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="33f22-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="33f22-105">L’implémentation de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="33f22-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="33f22-106">Par exemple, une bibliothèque d’outils externe peut utiliser une implémentation `IHostingStartup` pour fournir des fournisseurs ou des services de configuration supplémentaires à une application.</span><span class="sxs-lookup"><span data-stu-id="33f22-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="33f22-107">`IHostingStartup` *est disponible dans ASP.NET Core 2.0 et ultérieur.*</span><span class="sxs-lookup"><span data-stu-id="33f22-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="33f22-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="33f22-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="33f22-109">Découvrir les assemblys d’hébergement au démarrage chargés</span><span class="sxs-lookup"><span data-stu-id="33f22-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="33f22-110">Pour découvrir les assemblys d’hébergement au démarrage chargés par l’application ou des bibliothèques, activez la journalisation, puis vérifiez les journaux des applications.</span><span class="sxs-lookup"><span data-stu-id="33f22-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="33f22-111">Les erreurs qui se produisent durant le chargement des assemblys sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="33f22-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="33f22-112">Les assemblys d’hébergement au démarrage chargés sont journalisés au niveau Débogage, et toutes les erreurs sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="33f22-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="33f22-113">L’exemple d’application lit [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) dans un tableau `string` et affiche le résultat dans la page Index de l’application :</span><span class="sxs-lookup"><span data-stu-id="33f22-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="33f22-114">Désactiver le chargement automatique des assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="33f22-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="33f22-115">Pour désactiver le chargement automatique des assemblys d’hébergement au démarrage, procédez de l’une des manières suivantes :</span><span class="sxs-lookup"><span data-stu-id="33f22-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="33f22-116">Définissez le paramètre de configuration d’hôte [Empêcher l’hébergement au démarrage](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="33f22-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="33f22-117">Définissez la variable d’environnement `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="33f22-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="33f22-118">Si le paramètre d’hôte ou la variable d’environnement a la valeur `true` ou `1`, les assemblys d’hébergement au démarrage ne sont pas chargés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="33f22-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="33f22-119">Si les deux sont définis, le paramètre d’hôte contrôle le comportement.</span><span class="sxs-lookup"><span data-stu-id="33f22-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="33f22-120">La désactivation des assemblys d’hébergement au démarrage à l’aide du paramètre d’hôte ou de la variable d’environnement est globale et peut donc désactiver plusieurs caractéristiques d’une application.</span><span class="sxs-lookup"><span data-stu-id="33f22-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="33f22-121">Il n’est pas possible pour le moment de désactiver de manière sélective un assembly d’hébergement au démarrage ajouté par une bibliothèque, à moins que la bibliothèque n’offre sa propre option de configuration.</span><span class="sxs-lookup"><span data-stu-id="33f22-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="33f22-122">Dans une version ultérieure, il sera possible de désactiver de manière sélective les assemblys d’hébergement au démarrage (consultez le [problème aspnet/Hosting 1243 sur GitHub](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="33f22-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="33f22-123">Implémenter IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="33f22-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="33f22-124">Créer l’assembly</span><span class="sxs-lookup"><span data-stu-id="33f22-124">Create the assembly</span></span>

<span data-ttu-id="33f22-125">Une amélioration apportée à `IHostingStartup` est déployée sous la forme d’un assembly basé sur une application console sans point d’entrée.</span><span class="sxs-lookup"><span data-stu-id="33f22-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="33f22-126">L’assembly référence le package [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :</span><span class="sxs-lookup"><span data-stu-id="33f22-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="33f22-127">Un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifie une classe en tant qu’implémentation de `IHostingStartup` pour le chargement et l’exécution durant la génération de [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="33f22-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="33f22-128">Dans l’exemple suivant, l’espace de noms est `StartupEnhancement`, et la classe est `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="33f22-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="33f22-129">Une classe implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="33f22-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="33f22-130">La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la classe utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des améliorations à une application.</span><span class="sxs-lookup"><span data-stu-id="33f22-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="33f22-131">`IHostingStartup.Configure` dans l’assembly de démarrage de l’hébergement est appelé par le runtime avant `Startup.Configure` dans le code utilisateur, ce qui permet au code utilisateur de remplacer toute configuration fournie par l’assembly de démarrage de l’hébergement.</span><span class="sxs-lookup"><span data-stu-id="33f22-131">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configruation provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="33f22-132">Durant la génération d’un projet `IHostingStartup`, le fichier de dépendances (*\*. deps.json*) définit le dossier *bin* comme emplacement du `runtime` :</span><span class="sxs-lookup"><span data-stu-id="33f22-132">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="33f22-133">Seule une partie du fichier est affichée.</span><span class="sxs-lookup"><span data-stu-id="33f22-133">Only part of the file is shown.</span></span> <span data-ttu-id="33f22-134">Le nom de l’assembly dans l’exemple est `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="33f22-134">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="33f22-135">Mettre à jour le fichier de dépendances</span><span class="sxs-lookup"><span data-stu-id="33f22-135">Update the dependencies file</span></span>

<span data-ttu-id="33f22-136">L’emplacement du runtime est spécifié dans le fichier *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="33f22-136">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="33f22-137">Pour activer l’amélioration, l’élément `runtime` doit spécifier l’emplacement de l’assembly du runtime de l’amélioration.</span><span class="sxs-lookup"><span data-stu-id="33f22-137">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="33f22-138">Attribuez à l’emplacement du `runtime` le préfixe `lib/<TARGET_FRAMEWORK_MONIKER>/` :</span><span class="sxs-lookup"><span data-stu-id="33f22-138">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="33f22-139">Dans l’exemple d’application, la modification du fichier *\*.deps.json* est effectuée par un script [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="33f22-139">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="33f22-140">Le script PowerShell est automatiquement déclenché par une cible de build dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="33f22-140">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="33f22-141">Activation de l’amélioration</span><span class="sxs-lookup"><span data-stu-id="33f22-141">Enhancement activation</span></span>

<span data-ttu-id="33f22-142">**Placer le fichier d’assembly**</span><span class="sxs-lookup"><span data-stu-id="33f22-142">**Place the assembly file**</span></span>

<span data-ttu-id="33f22-143">Le fichier d’assembly de l’implémentation `IHostingStartup` doit être déployé dans le dossier *bin* de l’application ou placé dans le [magasin de runtime](/dotnet/core/deploying/runtime-store) :</span><span class="sxs-lookup"><span data-stu-id="33f22-143">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="33f22-144">Pour une utilisation par utilisateur, placez l’assembly dans le magasin de runtime du profil utilisateur à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="33f22-144">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

<span data-ttu-id="33f22-145">Pour une utilisation globale, placez l’assembly dans le magasin de runtime de l’installation .NET Core :</span><span class="sxs-lookup"><span data-stu-id="33f22-145">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

<span data-ttu-id="33f22-146">En cas de déploiement de l’assembly dans le magasin de runtime, le fichier de symboles peut également être déployé, mais il n’est pas nécessaire au bon fonctionnement de l’amélioration.</span><span class="sxs-lookup"><span data-stu-id="33f22-146">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="33f22-147">**Placer le fichier de dépendances**</span><span class="sxs-lookup"><span data-stu-id="33f22-147">**Place the dependencies file**</span></span>

<span data-ttu-id="33f22-148">Le fichier *\*.deps.json* de l’implémentation doit se trouver dans un emplacement accessible.</span><span class="sxs-lookup"><span data-stu-id="33f22-148">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="33f22-149">Pour une utilisation par utilisateur, placez le fichier dans le dossier `additonalDeps` des paramètres `.dotnet` du profil utilisateur :</span><span class="sxs-lookup"><span data-stu-id="33f22-149">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

<span data-ttu-id="33f22-150">Pour une utilisation globale, placez le fichier dans le dossier `additonalDeps` de l’installation .NET Core :</span><span class="sxs-lookup"><span data-stu-id="33f22-150">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

<span data-ttu-id="33f22-151">La version de framework partagé reflète la version du runtime partagé qu’utilise l’application cible.</span><span class="sxs-lookup"><span data-stu-id="33f22-151">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="33f22-152">Le runtime partagé est indiqué dans le fichier *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="33f22-152">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="33f22-153">Dans l’exemple d’application, le runtime partagé est spécifié dans le fichier *HostingStartupSample.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="33f22-153">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="33f22-154">**Définir des variables d’environnement**</span><span class="sxs-lookup"><span data-stu-id="33f22-154">**Set environment variables**</span></span>

<span data-ttu-id="33f22-155">Définissez les variables d’environnement suivantes dans le contexte de l’application qui utilise l’amélioration.</span><span class="sxs-lookup"><span data-stu-id="33f22-155">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="33f22-156">ASPNETCORE_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="33f22-156">ASPNETCORE_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="33f22-157">Seuls les assemblys d’hébergement au démarrage sont analysés à la recherche de `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="33f22-157">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="33f22-158">Le nom d’assembly de l’implémentation est fourni dans cette variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="33f22-158">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="33f22-159">L’exemple d’application définit la valeur `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="33f22-159">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="33f22-160">Vous pouvez également définir la valeur à l’aide du paramètre de configuration d’hôte [Assemblys d’hébergement au démarrage](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="33f22-160">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="33f22-161">Quand plusieurs assemblys de démarrage d’hébergement sont présents, leurs méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) sont exécutées dans l’ordre dans lequel ils sont répertoriés.</span><span class="sxs-lookup"><span data-stu-id="33f22-161">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

<span data-ttu-id="33f22-162">DOTNET_ADDITIONAL_DEPS</span><span class="sxs-lookup"><span data-stu-id="33f22-162">DOTNET_ADDITIONAL_DEPS</span></span>

<span data-ttu-id="33f22-163">L’emplacement du fichier *\*.deps.json* de l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="33f22-163">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="33f22-164">Si le fichier est placé dans le dossier *.dotnet* du profil utilisateur pour une utilisation par utilisateur :</span><span class="sxs-lookup"><span data-stu-id="33f22-164">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="33f22-165">Si le fichier est placé dans l’installation .NET Core pour une utilisation globale, fournissez le chemin complet au fichier :</span><span class="sxs-lookup"><span data-stu-id="33f22-165">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="33f22-166">L’exemple d’application définit la valeur suivante :</span><span class="sxs-lookup"><span data-stu-id="33f22-166">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="33f22-167">Pour obtenir des exemples montrant comment définir des variables d’environnement pour différents systèmes d’exploitation, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="33f22-167">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="33f22-168">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="33f22-168">Sample app</span></span>

<span data-ttu-id="33f22-169">[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) utilise `IHostingStartup` pour créer un outil de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="33f22-169">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="33f22-170">L’outil ajoute deux middlewares (intergiciels) à l’application au démarrage, qui fournissent des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="33f22-170">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="33f22-171">Services inscrits</span><span class="sxs-lookup"><span data-stu-id="33f22-171">Registered services</span></span>
* <span data-ttu-id="33f22-172">Adresse : schéma, hôte, base du chemin, chemin, chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="33f22-172">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="33f22-173">Connexion : adresse IP distante, port distant, adresse IP locale, port local, certificat client</span><span class="sxs-lookup"><span data-stu-id="33f22-173">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="33f22-174">En-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="33f22-174">Request headers</span></span>
* <span data-ttu-id="33f22-175">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="33f22-175">Environment variables</span></span>

<span data-ttu-id="33f22-176">Pour exécuter l’exemple :</span><span class="sxs-lookup"><span data-stu-id="33f22-176">To run the sample:</span></span>

1. <span data-ttu-id="33f22-177">Le projet StartupDiagnostics utilise [PowerShell](/powershell/scripting/powershell-scripting) pour modifier son fichier *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="33f22-177">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="33f22-178">PowerShell est installé par défaut sur les systèmes d’exploitation Windows à compter de Windows 7 SP1 et de Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="33f22-178">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="33f22-179">Pour obtenir PowerShell sur d’autres plateformes, consultez [Installation de Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="33f22-179">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="33f22-180">Générez le projet StartupDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="33f22-180">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="33f22-181">Une cible de build dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="33f22-181">A build target in the project file:</span></span>
   * <span data-ttu-id="33f22-182">Déplace l’assembly et les fichiers de symboles dans le magasin de runtime du profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="33f22-182">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="33f22-183">Déclenche le script PowerShell pour modifier le fichier *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="33f22-183">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="33f22-184">Déplace le fichier *StartupDiagnostics.deps.json* dans le dossier `additionalDeps` du profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="33f22-184">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="33f22-185">Définissez les variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="33f22-185">Set the environment variables:</span></span>
    * <span data-ttu-id="33f22-186">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="33f22-186">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="33f22-187">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="33f22-187">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="33f22-188">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="33f22-188">Run the sample app.</span></span>
5. <span data-ttu-id="33f22-189">Demandez le point de terminaison `/services` pour voir les services inscrits de l’application.</span><span class="sxs-lookup"><span data-stu-id="33f22-189">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="33f22-190">Demandez le point de terminaison `/diag` pour voir les informations de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="33f22-190">Request the `/diag` endpoint to see the diagnostic information.</span></span>
