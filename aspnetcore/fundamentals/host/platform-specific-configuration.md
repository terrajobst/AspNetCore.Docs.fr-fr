---
title: Utiliser des assemblys de démarrage d’hébergement dans ASP.NET Core
author: guardrex
description: Découvrez comment améliorer une application ASP.NET Core à partir d’un assembly externe à l’aide d’une implémentation IHostingStartup.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/23/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: c174d658c84ada88eef17528c663735a91347ba7
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419444"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="ed817-103">Utiliser des assemblys de démarrage d’hébergement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed817-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="ed817-104">Par [Luke Latham](https://github.com/guardrex) et [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="ed817-104">By [Luke Latham](https://github.com/guardrex) and [Pavel Krymets](https://github.com/pakrym)</span></span>

<span data-ttu-id="ed817-105">Une implémentation [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hébergement au démarrage) ajoute des améliorations à une application au démarrage à partir d’un assembly externe.</span><span class="sxs-lookup"><span data-stu-id="ed817-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="ed817-106">Par exemple, une bibliothèque externe peut utiliser une implémentation d’hébergement au démarrage pour fournir des fournisseurs ou services de configuration supplémentaires à une application.</span><span class="sxs-lookup"><span data-stu-id="ed817-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="ed817-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ed817-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="ed817-108">Attribut HostingStartup</span><span class="sxs-lookup"><span data-stu-id="ed817-108">HostingStartup attribute</span></span>

<span data-ttu-id="ed817-109">Un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indique la présence d’un assembly d’hébergement au démarrage à activer au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="ed817-109">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="ed817-110">L’assembly d’entrée ou l’assembly contenant la classe `Startup` est automatiquement analysé pour détecter l’attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="ed817-111">La liste des assemblys où les attributs `HostingStartup` doivent être recherchés est chargée au moment de l’exécution à partir de la configuration spécifiée dans [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="ed817-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="ed817-112">La liste des assemblys à exclure de cette détection est chargée de [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="ed817-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="ed817-113">Pour plus d’informations, consultez l’[hôte web : Assemblys d’hébergement au démarrage](xref:fundamentals/host/web-host#hosting-startup-assemblies) et l’[hôte web : Assemblys d’hébergement à exclure au démarrage](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="ed817-113">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="ed817-114">Dans l’exemple suivant, l’espace de noms de l’assembly d’hébergement au démarrage est `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="ed817-114">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="ed817-115">La classe contenant le code d’hébergement au démarrage est `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="ed817-115">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="ed817-116">L’attribut `HostingStartup` se trouve généralement dans le fichier de classe d’implémentation `IHostingStartup` de l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-116">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="ed817-117">Découvrir les assemblys d’hébergement au démarrage chargés</span><span class="sxs-lookup"><span data-stu-id="ed817-117">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="ed817-118">Pour détecter les assemblys d’hébergement au démarrage chargés, activez la journalisation et analysez les journaux de l’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-118">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="ed817-119">Les erreurs qui se produisent durant le chargement des assemblys sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="ed817-119">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="ed817-120">Les assemblys d’hébergement au démarrage chargés sont journalisés au niveau Débogage, et toutes les erreurs sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="ed817-120">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="ed817-121">Désactiver le chargement automatique des assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="ed817-121">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="ed817-122">Pour désactiver le chargement automatique des assemblys d’hébergement au démarrage, choisissez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="ed817-122">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="ed817-123">Pour bloquer le chargement de tous les assemblys d’hébergement au démarrage, définissez l’une des valeurs suivantes sur `true` ou `1` :</span><span class="sxs-lookup"><span data-stu-id="ed817-123">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="ed817-124">Le paramètre de configuration d’hôte [PreventHostingStartup](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="ed817-124">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="ed817-125">La variable d’environnement `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="ed817-125">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="ed817-126">Pour bloquer le chargement de certains assemblys d’hébergement au démarrage, définissez l’une des valeurs suivantes sous la forme d’une liste délimitée par des points-virgules contenant les assemblys d’hébergement au démarrage à exclure au moment du démarrage :</span><span class="sxs-lookup"><span data-stu-id="ed817-126">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="ed817-127">Le paramètre de configuration d’hôte [HostingStartupExcludeAssemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="ed817-127">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="ed817-128">La variable d’environnement `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ed817-128">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="ed817-129">Si le paramètre de configuration d’hôte et la variable d’environnement sont définis tous les deux, c’est le paramètre d’hôte qui détermine le comportement.</span><span class="sxs-lookup"><span data-stu-id="ed817-129">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="ed817-130">La désactivation des assemblys d’hébergement au démarrage à l’aide du paramètre d’hôte ou de la variable d’environnement désactive l’assembly globalement et peut donc désactiver plusieurs caractéristiques d’une application.</span><span class="sxs-lookup"><span data-stu-id="ed817-130">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="ed817-131">Projet</span><span class="sxs-lookup"><span data-stu-id="ed817-131">Project</span></span>

<span data-ttu-id="ed817-132">Créez un hébergement au démarrage avec un des types de projet suivants :</span><span class="sxs-lookup"><span data-stu-id="ed817-132">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="ed817-133">Bibliothèque de classes</span><span class="sxs-lookup"><span data-stu-id="ed817-133">Class library</span></span>](#class-library)
* [<span data-ttu-id="ed817-134">Application console sans point d’entrée</span><span class="sxs-lookup"><span data-stu-id="ed817-134">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="ed817-135">Bibliothèque de classes</span><span class="sxs-lookup"><span data-stu-id="ed817-135">Class library</span></span>

<span data-ttu-id="ed817-136">Une amélioration de l’hébergement au démarrage peut être fournie dans une bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="ed817-136">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="ed817-137">La bibliothèque contient un attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-137">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="ed817-138">[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) inclut une application Razor Pages (*HostingStartupApp*) et une bibliothèque de classes (*HostingStartupLibrary*).</span><span class="sxs-lookup"><span data-stu-id="ed817-138">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="ed817-139">La bibliothèque de classes :</span><span class="sxs-lookup"><span data-stu-id="ed817-139">The class library:</span></span>

* <span data-ttu-id="ed817-140">Contient une classe d’hébergement au démarrage, `ServiceKeyInjection`, qui implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-140">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="ed817-141">`ServiceKeyInjection` ajoute une paire de chaînes de service à la configuration de l’application par le biais du fournisseur de configuration en mémoire ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="ed817-141">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="ed817-142">Inclut un attribut `HostingStartup` qui identifie l’espace de noms et la classe d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-142">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="ed817-143">La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la classe `ServiceKeyInjection` utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des améliorations à une application.</span><span class="sxs-lookup"><span data-stu-id="ed817-143">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span>

<span data-ttu-id="ed817-144">*HostingStartupLibrary/ServiceKeyInjection.cs* :</span><span class="sxs-lookup"><span data-stu-id="ed817-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="ed817-145">La page d’index de l’application lit et affiche les valeurs de configuration des deux clés définies par l’assembly d’hébergement au démarrage de la bibliothèque de classes :</span><span class="sxs-lookup"><span data-stu-id="ed817-145">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="ed817-146">*HostingStartupApp/Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="ed817-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="ed817-147">[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) inclut également un projet de package NuGet qui fournit un hébergement au démarrage distinct, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="ed817-147">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="ed817-148">Le package a les mêmes caractéristiques que la bibliothèque de classes décrite précédemment.</span><span class="sxs-lookup"><span data-stu-id="ed817-148">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="ed817-149">Le package :</span><span class="sxs-lookup"><span data-stu-id="ed817-149">The package:</span></span>

* <span data-ttu-id="ed817-150">Contient une classe d’hébergement au démarrage, `ServiceKeyInjection`, qui implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-150">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="ed817-151">`ServiceKeyInjection` ajoute une paire de chaînes de service à la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-151">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="ed817-152">Inclut un attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-152">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="ed817-153">*HostingStartupPackage/ServiceKeyInjection.cs* :</span><span class="sxs-lookup"><span data-stu-id="ed817-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="ed817-154">La page d’index de l’application lit et affiche les valeurs de configuration des deux clés définies par l’assembly d’hébergement au démarrage du package :</span><span class="sxs-lookup"><span data-stu-id="ed817-154">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="ed817-155">*HostingStartupApp/Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="ed817-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="ed817-156">Application console sans point d’entrée</span><span class="sxs-lookup"><span data-stu-id="ed817-156">Console app without an entry point</span></span>

<span data-ttu-id="ed817-157">*Cette approche s’applique aux applications .NET Core, mais pas aux applications .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="ed817-157">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="ed817-158">Une amélioration d’hébergement au démarrage dynamique qui ne nécessite pas de référence au moment de la compilation pour l’activation peut être fournie dans une application console sans point d’entrée qui contient un attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-158">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="ed817-159">La publication de l’application console génère un assembly d’hébergement au démarrage qui peut être utilisé à partir du magasin de runtime.</span><span class="sxs-lookup"><span data-stu-id="ed817-159">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="ed817-160">Une application console sans point d’entrée est utilisée dans ce processus, car :</span><span class="sxs-lookup"><span data-stu-id="ed817-160">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="ed817-161">Un fichier de dépendances est nécessaire pour utiliser l’hébergement au démarrage dans l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-161">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="ed817-162">Un fichier de dépendances est une ressource d’application exécutable qui est générée par la publication d’une application, et non d’une bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="ed817-162">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="ed817-163">Une bibliothèque ne peut pas être ajoutée directement au [magasin de packages de runtime](/dotnet/core/deploying/runtime-store), qui nécessite un projet exécutable ciblant le runtime partagé.</span><span class="sxs-lookup"><span data-stu-id="ed817-163">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="ed817-164">Lors de la création d’un hébergement au démarrage dynamique :</span><span class="sxs-lookup"><span data-stu-id="ed817-164">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="ed817-165">Un assembly d’hébergement au démarrage est créé à partir de l’application console sans point d’entrée qui :</span><span class="sxs-lookup"><span data-stu-id="ed817-165">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="ed817-166">Inclut une classe qui contient l’implémentation `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-166">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="ed817-167">Inclut un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) pour identifier la classe d’implémentation `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-167">Includes a [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="ed817-168">L’application console est publiée pour obtenir les dépendances de l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-168">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="ed817-169">La publication de l’application console entraîne la suppression des dépendances inutilisées dans le fichier de dépendances.</span><span class="sxs-lookup"><span data-stu-id="ed817-169">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="ed817-170">Le fichier de dépendances est modifié pour définir l’emplacement d’exécution de l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-170">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="ed817-171">L’assembly d’hébergement au démarrage et le fichier de dépendances associé sont placés dans le magasin de packages de runtime.</span><span class="sxs-lookup"><span data-stu-id="ed817-171">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="ed817-172">Pour permettre leur détection, l’assembly d’hébergement au démarrage et le fichier de dépendances associé sont référencés dans une paire de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="ed817-172">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="ed817-173">L’application console référence le package [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :</span><span class="sxs-lookup"><span data-stu-id="ed817-173">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="ed817-174">Un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifie une classe en tant qu’implémentation de `IHostingStartup` pour le chargement et l’exécution durant la génération de [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="ed817-174">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="ed817-175">Dans l’exemple suivant, l’espace de noms est `StartupEnhancement`, et la classe est `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="ed817-175">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="ed817-176">Une classe implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-176">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="ed817-177">La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la classe utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des améliorations à une application.</span><span class="sxs-lookup"><span data-stu-id="ed817-177">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="ed817-178">`IHostingStartup.Configure` dans l’assembly d’hébergement au démarrage est appelé par le runtime avant `Startup.Configure` dans le code utilisateur, ce qui permet au code utilisateur de remplacer la configuration fournie par l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-178">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="ed817-179">Durant la génération d’un projet `IHostingStartup`, le fichier de dépendances (*\*. deps.json*) définit le dossier *bin* comme emplacement du `runtime` :</span><span class="sxs-lookup"><span data-stu-id="ed817-179">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ed817-180">Seule une partie du fichier est affichée.</span><span class="sxs-lookup"><span data-stu-id="ed817-180">Only part of the file is shown.</span></span> <span data-ttu-id="ed817-181">Le nom de l’assembly dans l’exemple est `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="ed817-181">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="ed817-182">Configuration fournie par le démarrage d’hébergement</span><span class="sxs-lookup"><span data-stu-id="ed817-182">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="ed817-183">Il existe deux approches de gestion de la configuration selon si vous souhaitez que la configuration du démarrage d’hébergement ait la priorité ou que la configuration d’application ait la priorité :</span><span class="sxs-lookup"><span data-stu-id="ed817-183">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="ed817-184">Fournissez la configuration à l’application à l’aide de <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> pour charger la configuration après l’exécution des délégués <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> de l’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-184">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="ed817-185">Avec cette approche, la configuration de démarrage d’hébergement est prioritaire sur la configuration d’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-185">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="ed817-186">Fournissez la configuration à l’application à l’aide de <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> pour charger la configuration avant l’exécution des délégués <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> de l’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-186">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="ed817-187">Avec cette approche, les valeurs de la configuration d’application sont prioritaires sur celles fournies par le démarrage d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="ed817-187">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="ed817-188">Spécifier l’assembly d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="ed817-188">Specify the hosting startup assembly</span></span>

<span data-ttu-id="ed817-189">Pour l’hébergement au démarrage fourni par une bibliothèque de classes ou une application console, spécifiez le nom de l’assembly d’hébergement au démarrage dans la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ed817-189">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="ed817-190">Cette variable est spécifiée sous la forme d’une liste d’assemblys délimitée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="ed817-190">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="ed817-191">L’analyse de détection de l’attribut `HostingStartup` porte uniquement sur les assemblys d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-191">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="ed817-192">Dans l’exemple d’application (*HostingStartupApp*), pour permettre la détection des hébergements au démarrage décrits précédemment, la variable d’environnement est définie à la valeur suivante :</span><span class="sxs-lookup"><span data-stu-id="ed817-192">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="ed817-193">Un assembly d’hébergement au démarrage peut également être défini à l’aide du paramètre de configuration d’hôte [HostingStartupAssemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="ed817-193">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="ed817-194">Quand plusieurs assemblys de démarrage d’hébergement sont présents, leurs méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) sont exécutées dans l’ordre dans lequel ils sont répertoriés.</span><span class="sxs-lookup"><span data-stu-id="ed817-194">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="ed817-195">Activation</span><span class="sxs-lookup"><span data-stu-id="ed817-195">Activation</span></span>

<span data-ttu-id="ed817-196">Les options d’activation de l’hébergement au démarrage sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="ed817-196">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="ed817-197">[Magasin de runtime](#runtime-store) &ndash; L’activation ne nécessite pas de référence au moment de la compilation pour l’activation.</span><span class="sxs-lookup"><span data-stu-id="ed817-197">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="ed817-198">L’exemple d’application place les fichiers de l’assembly d’hébergement au démarrage et de ses dépendances dans le dossier *deployment* pour faciliter le déploiement de l’hébergement au démarrage dans un environnement multimachine.</span><span class="sxs-lookup"><span data-stu-id="ed817-198">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="ed817-199">Le dossier *deployment* inclut également un script PowerShell qui crée ou modifie des variables d’environnement sur le système de déploiement pour activer l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-199">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="ed817-200">Référence au moment de la compilation requise pour l’activation</span><span class="sxs-lookup"><span data-stu-id="ed817-200">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="ed817-201">Package NuGet</span><span class="sxs-lookup"><span data-stu-id="ed817-201">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="ed817-202">Dossier bin du projet</span><span class="sxs-lookup"><span data-stu-id="ed817-202">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="ed817-203">Magasin de runtime</span><span class="sxs-lookup"><span data-stu-id="ed817-203">Runtime store</span></span>

<span data-ttu-id="ed817-204">L’implémentation d’hébergement au démarrage est placée dans le [magasin de runtime](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="ed817-204">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="ed817-205">L’application améliorée ne nécessite pas de référence à l’assembly au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="ed817-205">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="ed817-206">Une fois l’hébergement au démarrage créé, un magasin de runtime est généré à l’aide du fichier manifeste de projet et de la commande [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="ed817-206">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="ed817-207">Dans l’exemple d’application (projet *RuntimeStore*), la commande suivante est utilisée :</span><span class="sxs-lookup"><span data-stu-id="ed817-207">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="ed817-208">Pour que le runtime découvre le magasin de runtime, l’emplacement du magasin de runtime est ajouté à la variable d’environnement `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="ed817-208">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="ed817-209">**Modifier et placer le fichier de dépendances de l’hébergement au démarrage**</span><span class="sxs-lookup"><span data-stu-id="ed817-209">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="ed817-210">Pour activer l’amélioration sans référence de package à l’amélioration, spécifiez les dépendances supplémentaires pour le runtime avec `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="ed817-210">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="ed817-211">`additionalDeps` vous permet de :</span><span class="sxs-lookup"><span data-stu-id="ed817-211">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="ed817-212">Étendre le graphe de la bibliothèque de l’application en fournissant un ensemble de fichiers *\*.deps.json* supplémentaires à fusionner avec le fichier *\*.deps.json* de l’application au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-212">Extend the app's library graph by providing a set of additional *\*.deps.json* files to merge with the app's own *\*.deps.json* file on startup.</span></span>
* <span data-ttu-id="ed817-213">Rendre l’assembly d’hébergement au démarrage détectable et chargeable.</span><span class="sxs-lookup"><span data-stu-id="ed817-213">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="ed817-214">L’approche recommandée pour la génération du fichier de dépendances supplémentaire est la suivante :</span><span class="sxs-lookup"><span data-stu-id="ed817-214">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="ed817-215">Exécutez `dotnet publish` sur le fichier manifeste du magasin de runtime référencé dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="ed817-215">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="ed817-216">Supprimez la référence au manifeste des bibliothèques et la section `runtime` du fichier *\*deps.json* obtenu.</span><span class="sxs-lookup"><span data-stu-id="ed817-216">Remove the manifest reference from libraries and the `runtime` section of the resulting *\*deps.json* file.</span></span>

<span data-ttu-id="ed817-217">Dans l’exemple de projet, la propriété `store.manifest/1.0.0` est supprimée de la section `targets` et de la section `libraries` :</span><span class="sxs-lookup"><span data-stu-id="ed817-217">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="ed817-218">Placez le fichier *\*.deps.json* à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="ed817-218">Place the *\*.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="ed817-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Emplacement ajouté à la variable d’environnement `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="ed817-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="ed817-220">`{SHARED FRAMEWORK NAME}` &ndash; Framework partagé requis pour ce fichier de dépendances supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="ed817-220">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="ed817-221">`{SHARED FRAMEWORK VERSION}` &ndash; Version minimale du framework partagé.</span><span class="sxs-lookup"><span data-stu-id="ed817-221">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="ed817-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; Nom de l’assembly de l’amélioration.</span><span class="sxs-lookup"><span data-stu-id="ed817-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="ed817-223">Dans l’exemple d’application (projet *RuntimeStore*), le fichier de dépendances supplémentaire est placé à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="ed817-223">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="ed817-224">Pour que le runtime découvre l’emplacement du magasin de runtime, l’emplacement du fichier de dépendances supplémentaire est ajouté à la variable d’environnement `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="ed817-224">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="ed817-225">Dans l’exemple d’application (projet *RuntimeStore*), la génération du magasin de runtime et du fichier de dépendances supplémentaire s’effectue à l’aide d’un script [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="ed817-225">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="ed817-226">Pour obtenir des exemples montrant comment définir des variables d’environnement pour différents systèmes d’exploitation, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="ed817-226">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="ed817-227">**Déploiement**</span><span class="sxs-lookup"><span data-stu-id="ed817-227">**Deployment**</span></span>

<span data-ttu-id="ed817-228">Pour faciliter le déploiement d’un hébergement au démarrage dans un environnement multimachine, l’exemple d’application crée un dossier *deployment* dans la sortie publiée qui contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ed817-228">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="ed817-229">Le magasin de runtime d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-229">The hosting startup runtime store.</span></span>
* <span data-ttu-id="ed817-230">Le fichier des dépendances de l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-230">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="ed817-231">Un script PowerShell qui crée ou modifie les variables `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE` et `DOTNET_ADDITIONAL_DEPS` pour prendre en charge l’activation de l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-231">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="ed817-232">Exécutez ce script à partir d’une invite de commandes PowerShell d’administration sur le système de déploiement.</span><span class="sxs-lookup"><span data-stu-id="ed817-232">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="ed817-233">Package NuGet</span><span class="sxs-lookup"><span data-stu-id="ed817-233">NuGet package</span></span>

<span data-ttu-id="ed817-234">Une amélioration de l’hébergement au démarrage peut être fournie dans un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="ed817-234">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="ed817-235">Le package a un attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ed817-235">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="ed817-236">Les types d’hébergement au démarrage fournis par le package sont mis à la disposition de l’application au moyen de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="ed817-236">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="ed817-237">Le fichier projet de l’application améliorée crée une référence de package à l’hébergement au démarrage dans le fichier projet de l’application (référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="ed817-237">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="ed817-238">Une fois la référence au moment de la compilation créée, l’assembly d’hébergement au démarrage et toutes ses dépendances sont ajoutés au fichier de dépendances de l’application (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="ed817-238">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="ed817-239">Cette approche s’applique à un package d’assembly d’hébergement au démarrage qui a été publié sur [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="ed817-239">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="ed817-240">Le fichier de dépendances de l’hébergement au démarrage est mis à la disposition de l’application améliorée de la façon décrite dans la section [Magasin de runtime](#runtime-store) (sans référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="ed817-240">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="ed817-241">Pour plus d’informations sur les packages NuGet et le magasin de runtime, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="ed817-241">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="ed817-242">Création d’un package NuGet avec les outils multiplateformes</span><span class="sxs-lookup"><span data-stu-id="ed817-242">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="ed817-243">Publication de packages</span><span class="sxs-lookup"><span data-stu-id="ed817-243">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="ed817-244">Magasin de packages de runtime</span><span class="sxs-lookup"><span data-stu-id="ed817-244">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="ed817-245">Dossier bin du projet</span><span class="sxs-lookup"><span data-stu-id="ed817-245">Project bin folder</span></span>

<span data-ttu-id="ed817-246">Une amélioration de l’hébergement au démarrage peut être fournie par un assembly déployé à partir du dossier *bin* dans l’application améliorée.</span><span class="sxs-lookup"><span data-stu-id="ed817-246">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="ed817-247">Les types d’hébergement au démarrage fournis par l’assembly sont mis à la disposition de l’application au moyen de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="ed817-247">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="ed817-248">Le fichier projet de l’application améliorée crée une référence d’assembly à l’hébergement au démarrage (référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="ed817-248">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="ed817-249">Une fois la référence au moment de la compilation créée, l’assembly d’hébergement au démarrage et toutes ses dépendances sont ajoutés au fichier de dépendances de l’application (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="ed817-249">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="ed817-250">Cette approche s’applique quand le scénario de déploiement appelle le déplacement de l’assembly de la bibliothèque d’hébergement au démarrage compilée (fichier DLL) vers le projet de consommation, ou vers un emplacement auquel ce projet a accès, et quand une référence au moment de la compilation est créée vers l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ed817-250">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="ed817-251">Le fichier de dépendances de l’hébergement au démarrage est mis à la disposition de l’application améliorée de la façon décrite dans la section [Magasin de runtime](#runtime-store) (sans référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="ed817-251">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="ed817-252">Exemple de code</span><span class="sxs-lookup"><span data-stu-id="ed817-252">Sample code</span></span>

<span data-ttu-id="ed817-253">[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([Comment télécharger un exemple](xref:index#how-to-download-a-sample)) montre des scénarios d’implémentation de l’hébergement au démarrage :</span><span class="sxs-lookup"><span data-stu-id="ed817-253">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="ed817-254">Deux assemblys d’hébergement au démarrage (bibliothèques de classes) définissent chacun une paire clé-valeur de configuration en mémoire :</span><span class="sxs-lookup"><span data-stu-id="ed817-254">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="ed817-255">Package NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="ed817-255">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="ed817-256">Bibliothèque de classes (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="ed817-256">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="ed817-257">Un hébergement au démarrage est activé à partir d’un assembly déployé depuis le magasin de runtime (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="ed817-257">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="ed817-258">L’assembly ajoute deux middleware (intergiciels) à l’application au démarrage, qui fournissent des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="ed817-258">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="ed817-259">Services inscrits</span><span class="sxs-lookup"><span data-stu-id="ed817-259">Registered services</span></span>
  * <span data-ttu-id="ed817-260">Adresse (schéma, hôte, chemin de base, chemin, chaîne de requête)</span><span class="sxs-lookup"><span data-stu-id="ed817-260">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="ed817-261">Connexion (adresse IP distante, port distant, adresse IP locale, port local, certificat client)</span><span class="sxs-lookup"><span data-stu-id="ed817-261">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="ed817-262">En-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="ed817-262">Request headers</span></span>
  * <span data-ttu-id="ed817-263">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="ed817-263">Environment variables</span></span>

<span data-ttu-id="ed817-264">Pour exécuter l’exemple :</span><span class="sxs-lookup"><span data-stu-id="ed817-264">To run the sample:</span></span>

<span data-ttu-id="ed817-265">**Activation à partir d’un package NuGet**</span><span class="sxs-lookup"><span data-stu-id="ed817-265">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="ed817-266">Compilez le package *HostingStartupPackage* à l’aide de la commande [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="ed817-266">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="ed817-267">Ajoutez le nom de l’assembly du package *HostingStartupPackage* à la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ed817-267">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="ed817-268">Compilez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-268">Compile and run the app.</span></span> <span data-ttu-id="ed817-269">Une référence de package est présente dans l’application améliorée (référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="ed817-269">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="ed817-270">Un `<PropertyGroup>` dans le fichier projet de l’application spécifie la sortie du projet de package (*../HostingStartupPackage/bin/Debug*) comme source de package.</span><span class="sxs-lookup"><span data-stu-id="ed817-270">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="ed817-271">L’application peut ainsi utiliser le package sans avoir à le charger sur [nuget.org](https://www.nuget.org/). Pour plus d’informations, consultez les remarques dans le fichier projet de l’application HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="ed817-271">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="ed817-272">Notez que les valeurs des clés de configuration de service affichées dans la page d’index correspondent aux valeurs définies par la méthode `ServiceKeyInjection.Configure` du package.</span><span class="sxs-lookup"><span data-stu-id="ed817-272">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="ed817-273">Si vous modifiez le projet *HostingStartupPackage* et le recompilez, effacez les caches locaux du package NuGet pour vous assurer que *HostingStartupApp* reçoit le package mis à jour et pas un package périmé du cache local.</span><span class="sxs-lookup"><span data-stu-id="ed817-273">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="ed817-274">Pour effacer les caches NuGet locaux, exécutez la commande [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) suivante :</span><span class="sxs-lookup"><span data-stu-id="ed817-274">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="ed817-275">**Activation à partir d’une bibliothèque de classes**</span><span class="sxs-lookup"><span data-stu-id="ed817-275">**Activation from a class library**</span></span>

1. <span data-ttu-id="ed817-276">Compilez la bibliothèque de classes *HostingStartupLibrary* à l’aide de la commande [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="ed817-276">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="ed817-277">Ajoutez le nom de l’assembly de la bibliothèque de classes *HostingStartupLibrary* à la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ed817-277">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="ed817-278">À partir du dossier *bin*, déployez l’assembly de la bibliothèque de classes dans l’application en copiant le fichier *HostingStartupLibrary.dll* du résultat de la compilation de la bibliothèque de classes dans le dossier *bin/Debug* de l’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-278">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="ed817-279">Compilez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-279">Compile and run the app.</span></span> <span data-ttu-id="ed817-280">Dans le fichier projet de l’application, un `<ItemGroup>` référence l’assembly de la bibliothèque de classes (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="ed817-280">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="ed817-281">Pour plus d’informations, consultez les remarques dans le fichier projet de l’application HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="ed817-281">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="ed817-282">Notez que les valeurs des clés de configuration de service affichées dans la page d’index correspondent aux valeurs définies par la méthode `ServiceKeyInjection.Configure` de la bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="ed817-282">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="ed817-283">**Activation à partir d’un assembly déployé depuis le magasin de runtime**</span><span class="sxs-lookup"><span data-stu-id="ed817-283">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="ed817-284">Le projet *StartupDiagnostics* utilise [PowerShell](/powershell/scripting/powershell-scripting) pour modifier le fichier *StartupDiagnostics.deps.json* associé.</span><span class="sxs-lookup"><span data-stu-id="ed817-284">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="ed817-285">PowerShell est installé par défaut sur Windows à compter de Windows 7 SP1 et de Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="ed817-285">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="ed817-286">Pour obtenir PowerShell sur d’autres plateformes, consultez [Installation de Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="ed817-286">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="ed817-287">Exécutez le script *build.ps1* du dossier *RuntimeStore*.</span><span class="sxs-lookup"><span data-stu-id="ed817-287">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="ed817-288">La commande `dotnet store` du script utilise l’[identificateur de runtime (RID)](/dotnet/core/rid-catalog) `win7-x64` pour un hébergement au démarrage déployé sur Windows.</span><span class="sxs-lookup"><span data-stu-id="ed817-288">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="ed817-289">Si vous fournissez l’hébergement au démarrage pour un autre runtime, spécifiez l’identificateur de runtime approprié.</span><span class="sxs-lookup"><span data-stu-id="ed817-289">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="ed817-290">Exécutez le script *deploy.ps1* du dossier *Deployment*.</span><span class="sxs-lookup"><span data-stu-id="ed817-290">Run the *deploy.ps1* script in the *deployment* folder.</span></span>
1. <span data-ttu-id="ed817-291">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-291">Run the sample app.</span></span>
1. <span data-ttu-id="ed817-292">Demandez le point de terminaison `/services` pour voir les services inscrits de l’application.</span><span class="sxs-lookup"><span data-stu-id="ed817-292">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="ed817-293">Demandez le point de terminaison `/diag` pour voir les informations de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="ed817-293">Request the `/diag` endpoint to see the diagnostic information.</span></span>
