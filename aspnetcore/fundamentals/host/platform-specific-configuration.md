---
title: Améliorer une application à partir d’un assembly externe dans ASP.NET Core avec IHostingStartup
author: guardrex
description: Découvrez comment améliorer une application ASP.NET Core à partir d’un assembly externe à l’aide d’une implémentation IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 2eddfa03b28564fcca7cc098e353b05e23b7c6f6
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336272"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="b6ef5-103">Améliorer une application à partir d’un assembly externe dans ASP.NET Core avec IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="b6ef5-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="b6ef5-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b6ef5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b6ef5-105">Une implémentation [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hébergement au démarrage) ajoute des améliorations à une application au démarrage à partir d’un assembly externe.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="b6ef5-106">Par exemple, une bibliothèque externe peut utiliser une implémentation d’hébergement au démarrage pour fournir des fournisseurs ou services de configuration supplémentaires à une application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="b6ef5-107">`IHostingStartup` *est disponible dans ASP.NET Core 2.0 ou versions ultérieures.*</span><span class="sxs-lookup"><span data-stu-id="b6ef5-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="b6ef5-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6ef5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="b6ef5-109">Attribut HostingStartup</span><span class="sxs-lookup"><span data-stu-id="b6ef5-109">HostingStartup attribute</span></span>

<span data-ttu-id="b6ef5-110">Un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indique la présence d’un assembly d’hébergement au démarrage à activer au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="b6ef5-111">L’assembly d’entrée ou l’assembly contenant la classe `Startup` est automatiquement analysé pour détecter l’attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="b6ef5-112">La liste des assemblys où les attributs `HostingStartup` doivent être recherchés est chargée au moment de l’exécution à partir de la configuration spécifiée dans [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="b6ef5-113">La liste des assemblys à exclure de cette détection est chargée de [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="b6ef5-114">Pour plus d’informations, consultez [Hôte web : Assemblys d’hébergement au démarrage](xref:fundamentals/host/web-host#hosting-startup-assemblies) et [Hôte web : Assemblys exclus de l’hébergement au démarrage](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="b6ef5-115">Dans l’exemple suivant, l’espace de noms de l’assembly d’hébergement au démarrage est `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="b6ef5-116">La classe contenant le code d’hébergement au démarrage est `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="b6ef5-117">L’attribut `HostingStartup` se trouve généralement dans le fichier de classe d’implémentation `IHostingStartup` de l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="b6ef5-118">Découvrir les assemblys d’hébergement au démarrage chargés</span><span class="sxs-lookup"><span data-stu-id="b6ef5-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="b6ef5-119">Pour détecter les assemblys d’hébergement au démarrage chargés, activez la journalisation et analysez les journaux de l’application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="b6ef5-120">Les erreurs qui se produisent durant le chargement des assemblys sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="b6ef5-121">Les assemblys d’hébergement au démarrage chargés sont journalisés au niveau Débogage, et toutes les erreurs sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="b6ef5-122">Désactiver le chargement automatique des assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="b6ef5-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b6ef5-123">Pour désactiver le chargement automatique des assemblys d’hébergement au démarrage, choisissez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="b6ef5-124">Pour bloquer le chargement de tous les assemblys d’hébergement au démarrage, définissez l’une des valeurs suivantes sur `true` ou `1` :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="b6ef5-125">Le paramètre de configuration d’hôte [PreventHostingStartup](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="b6ef5-126">La variable d’environnement `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="b6ef5-127">Pour bloquer le chargement de certains assemblys d’hébergement au démarrage, définissez l’une des valeurs suivantes sous la forme d’une liste délimitée par des points-virgules contenant les assemblys d’hébergement au démarrage à exclure au moment du démarrage :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="b6ef5-128">Le paramètre de configuration d’hôte [HostingStartupExcludeAssemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="b6ef5-129">La variable d’environnement `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b6ef5-130">Pour désactiver le chargement automatique des assemblys d’hébergement au démarrage, définissez l’une des valeurs suivantes sur `true` ou `1` :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="b6ef5-131">Le paramètre de configuration d’hôte [PreventHostingStartup](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="b6ef5-132">La variable d’environnement `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="b6ef5-133">Si le paramètre de configuration d’hôte et la variable d’environnement sont définis tous les deux, c’est le paramètre d’hôte qui détermine le comportement.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="b6ef5-134">La désactivation des assemblys d’hébergement au démarrage à l’aide du paramètre d’hôte ou de la variable d’environnement désactive l’assembly globalement et peut donc désactiver plusieurs caractéristiques d’une application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="b6ef5-135">Projet</span><span class="sxs-lookup"><span data-stu-id="b6ef5-135">Project</span></span>

<span data-ttu-id="b6ef5-136">Créez un hébergement au démarrage avec un des types de projet suivants :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="b6ef5-137">Bibliothèque de classes</span><span class="sxs-lookup"><span data-stu-id="b6ef5-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="b6ef5-138">Application console sans point d’entrée</span><span class="sxs-lookup"><span data-stu-id="b6ef5-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="b6ef5-139">Bibliothèque de classes</span><span class="sxs-lookup"><span data-stu-id="b6ef5-139">Class library</span></span>

<span data-ttu-id="b6ef5-140">Une amélioration de l’hébergement au démarrage peut être fournie dans une bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="b6ef5-141">La bibliothèque contient un attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="b6ef5-142">[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) inclut une application Razor Pages (*HostingStartupApp*) et une bibliothèque de classes (*HostingStartupLibrary*).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="b6ef5-143">La bibliothèque de classes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-143">The class library:</span></span>

* <span data-ttu-id="b6ef5-144">Contient une classe d’hébergement au démarrage, `ServiceKeyInjection`, qui implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="b6ef5-145">`ServiceKeyInjection` ajoute une paire de chaînes de service à la configuration de l’application par le biais du fournisseur de configuration en mémoire ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="b6ef5-146">Inclut un attribut `HostingStartup` qui identifie l’espace de noms et la classe d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="b6ef5-147">La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la classe `ServiceKeyInjection` utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des améliorations à une application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="b6ef5-148">`IHostingStartup.Configure` dans l’assembly d’hébergement au démarrage est appelé par le runtime avant `Startup.Configure` dans le code utilisateur, ce qui permet au code utilisateur de remplacer la configuration fournie par l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="b6ef5-149">*HostingStartupLibrary/ServiceKeyInjection.cs* :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="b6ef5-150">La page d’index de l’application lit et affiche les valeurs de configuration des deux clés définies par l’assembly d’hébergement au démarrage de la bibliothèque de classes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="b6ef5-151">*HostingStartupApp/Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="b6ef5-152">[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) inclut également un projet de package NuGet qui fournit un hébergement au démarrage distinct, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="b6ef5-153">Le package a les mêmes caractéristiques que la bibliothèque de classes décrite précédemment.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="b6ef5-154">Le package :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-154">The package:</span></span>

* <span data-ttu-id="b6ef5-155">Contient une classe d’hébergement au démarrage, `ServiceKeyInjection`, qui implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="b6ef5-156">`ServiceKeyInjection` ajoute une paire de chaînes de service à la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="b6ef5-157">Inclut un attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="b6ef5-158">*HostingStartupPackage/ServiceKeyInjection.cs* :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="b6ef5-159">La page d’index de l’application lit et affiche les valeurs de configuration des deux clés définies par l’assembly d’hébergement au démarrage du package :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="b6ef5-160">*HostingStartupApp/Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="b6ef5-161">Application console sans point d’entrée</span><span class="sxs-lookup"><span data-stu-id="b6ef5-161">Console app without an entry point</span></span>

<span data-ttu-id="b6ef5-162">*Cette approche s’applique aux applications .NET Core, mais pas aux applications .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="b6ef5-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="b6ef5-163">Une amélioration d’hébergement au démarrage dynamique qui ne nécessite pas de référence au moment de la compilation pour l’activation peut être fournie dans une application console sans point d’entrée.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="b6ef5-164">L’application contient un attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="b6ef5-165">Pour créer un hébergement au démarrage dynamique :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="b6ef5-166">Une bibliothèque d’implémentation est créée à partir de la classe contenant l’implémentation `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="b6ef5-167">Cette bibliothèque est considérée comme un package standard.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="b6ef5-168">Une application console sans point d’entrée référence le package de la bibliothèque d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="b6ef5-169">Une application console est utilisée pour les raisons suivantes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-169">A console app is used because:</span></span>
   * <span data-ttu-id="b6ef5-170">Un fichier de dépendances est une ressource d’application exécutable, et donc une bibliothèque ne peut pas fournir un fichier de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="b6ef5-171">Une bibliothèque ne peut pas être ajoutée directement au [magasin de packages de runtime](/dotnet/core/deploying/runtime-store), qui nécessite un projet exécutable ciblant le runtime partagé.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="b6ef5-172">L’application console est publiée pour obtenir les dépendances de l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="b6ef5-173">La publication de l’application console entraîne la suppression des dépendances inutilisées dans le fichier de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="b6ef5-174">L’application et le fichier de dépendances associé sont placés dans le magasin de packages de runtime.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="b6ef5-175">Pour permettre leur détection, l’assembly d’hébergement au démarrage et le fichier de dépendances associé sont référencés dans une paire de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="b6ef5-176">L’application console référence le package [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="b6ef5-177">Un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifie une classe en tant qu’implémentation de `IHostingStartup` pour le chargement et l’exécution durant la génération de [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="b6ef5-178">Dans l’exemple suivant, l’espace de noms est `StartupEnhancement`, et la classe est `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="b6ef5-179">Une classe implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="b6ef5-180">La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la classe utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des améliorations à une application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="b6ef5-181">`IHostingStartup.Configure` dans l’assembly d’hébergement au démarrage est appelé par le runtime avant `Startup.Configure` dans le code utilisateur, ce qui permet au code utilisateur de remplacer la configuration fournie par l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="b6ef5-182">Durant la génération d’un projet `IHostingStartup`, le fichier de dépendances (*\*. deps.json*) définit le dossier *bin* comme emplacement du `runtime` :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="b6ef5-183">Seule une partie du fichier est affichée.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-183">Only part of the file is shown.</span></span> <span data-ttu-id="b6ef5-184">Le nom de l’assembly dans l’exemple est `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="b6ef5-185">Spécifier l’assembly d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="b6ef5-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="b6ef5-186">Pour l’hébergement au démarrage fourni par une bibliothèque de classes ou une application console, spécifiez le nom de l’assembly d’hébergement au démarrage dans la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="b6ef5-187">Cette variable est spécifiée sous la forme d’une liste d’assemblys délimitée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="b6ef5-188">L’analyse de détection de l’attribut `HostingStartup` porte uniquement sur les assemblys d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="b6ef5-189">Dans l’exemple d’application (*HostingStartupApp*), pour permettre la détection des hébergements au démarrage décrits précédemment, la variable d’environnement est définie à la valeur suivante :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="b6ef5-190">Un assembly d’hébergement au démarrage peut également être défini à l’aide du paramètre de configuration d’hôte [HostingStartupAssemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="b6ef5-191">Quand plusieurs assemblys de démarrage d’hébergement sont présents, leurs méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) sont exécutées dans l’ordre dans lequel ils sont répertoriés.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="b6ef5-192">Activation</span><span class="sxs-lookup"><span data-stu-id="b6ef5-192">Activation</span></span>

<span data-ttu-id="b6ef5-193">Les options d’activation de l’hébergement au démarrage sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="b6ef5-194">[Magasin de runtime](#runtime-store) &ndash; L’activation ne nécessite pas de référence au moment de la compilation pour l’activation.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="b6ef5-195">L’exemple d’application place les fichiers de l’assembly d’hébergement au démarrage et de ses dépendances dans le dossier *deployment* pour faciliter le déploiement de l’hébergement au démarrage dans un environnement multimachine.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="b6ef5-196">Le dossier *deployment* inclut également un script PowerShell qui crée ou modifie des variables d’environnement sur le système de déploiement pour activer l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="b6ef5-197">Référence au moment de la compilation requise pour l’activation</span><span class="sxs-lookup"><span data-stu-id="b6ef5-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="b6ef5-198">Package NuGet</span><span class="sxs-lookup"><span data-stu-id="b6ef5-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="b6ef5-199">Dossier bin du projet</span><span class="sxs-lookup"><span data-stu-id="b6ef5-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="b6ef5-200">Magasin de runtime</span><span class="sxs-lookup"><span data-stu-id="b6ef5-200">Runtime store</span></span>

<span data-ttu-id="b6ef5-201">L’implémentation d’hébergement au démarrage est placée dans le [magasin de runtime](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="b6ef5-202">L’application améliorée ne nécessite pas de référence à l’assembly au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="b6ef5-203">Une fois l’hébergement au démarrage généré, le fichier projet correspondant est utilisé comme fichier manifeste dans la commande [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="b6ef5-204">Cette commande place l’assembly d’hébergement au démarrage et d’autres dépendances qui ne font pas partie du framework partagé dans le magasin de runtime du profil utilisateur, à l’emplacement ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6ef5-205">Windows</span><span class="sxs-lookup"><span data-stu-id="b6ef5-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="b6ef5-206">macOS</span><span class="sxs-lookup"><span data-stu-id="b6ef5-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6ef5-207">Linux</span><span class="sxs-lookup"><span data-stu-id="b6ef5-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="b6ef5-208">Si vous souhaitez placer l’assembly et les dépendances à un emplacement permettant une utilisation globale, ajoutez l’option `-o|--output` à la commande `dotnet store` dans le chemin suivant :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6ef5-209">Windows</span><span class="sxs-lookup"><span data-stu-id="b6ef5-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="b6ef5-210">macOS</span><span class="sxs-lookup"><span data-stu-id="b6ef5-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6ef5-211">Linux</span><span class="sxs-lookup"><span data-stu-id="b6ef5-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="b6ef5-212">**Modifier et placer le fichier de dépendances de l’hébergement au démarrage**</span><span class="sxs-lookup"><span data-stu-id="b6ef5-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="b6ef5-213">L’emplacement du runtime est spécifié dans le fichier *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="b6ef5-214">Pour activer l’amélioration, l’élément `runtime` doit spécifier l’emplacement de l’assembly du runtime de l’amélioration.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="b6ef5-215">Attribuez à l’emplacement du `runtime` le préfixe `lib/<TARGET_FRAMEWORK_MONIKER>/` :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="b6ef5-216">Dans l’exemple de code (projet *StartupDiagnostics*), la modification du fichier *\*.deps.json* est effectuée par un script [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="b6ef5-217">Le script PowerShell est automatiquement déclenché par une cible de build dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="b6ef5-218">Le fichier *\*.deps.json* de l’implémentation doit se trouver dans un emplacement accessible.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="b6ef5-219">Pour une utilisation individuelle, placez le fichier dans le dossier *additonalDeps* des paramètres `.dotnet` du profil utilisateur :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6ef5-220">Windows</span><span class="sxs-lookup"><span data-stu-id="b6ef5-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="b6ef5-221">macOS</span><span class="sxs-lookup"><span data-stu-id="b6ef5-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6ef5-222">Linux</span><span class="sxs-lookup"><span data-stu-id="b6ef5-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="b6ef5-223">Pour une utilisation globale, placez le fichier dans le dossier *additonalDeps* de l’installation .NET Core :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6ef5-224">Windows</span><span class="sxs-lookup"><span data-stu-id="b6ef5-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="b6ef5-225">macOS</span><span class="sxs-lookup"><span data-stu-id="b6ef5-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6ef5-226">Linux</span><span class="sxs-lookup"><span data-stu-id="b6ef5-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="b6ef5-227">La version de framework partagé reflète la version du runtime partagé qu’utilise l’application cible.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="b6ef5-228">Le runtime partagé est indiqué dans le fichier *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="b6ef5-229">Dans l’exemple d’application (*HostingStartupApp*), le runtime partagé est spécifié dans le fichier *HostingStartupApp.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="b6ef5-230">**Détecter le fichier de dépendances de l’hébergement au démarrage**</span><span class="sxs-lookup"><span data-stu-id="b6ef5-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="b6ef5-231">L’emplacement du fichier *\*.deps.json* de l’implémentation est indiqué dans la variable d’environnement `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="b6ef5-232">Si le fichier est placé dans le dossier *.dotnet* du profil utilisateur, définissez la variable d’environnement à la valeur ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6ef5-233">Windows</span><span class="sxs-lookup"><span data-stu-id="b6ef5-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="b6ef5-234">macOS</span><span class="sxs-lookup"><span data-stu-id="b6ef5-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6ef5-235">Linux</span><span class="sxs-lookup"><span data-stu-id="b6ef5-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="b6ef5-236">Si le fichier est placé dans l’installation .NET Core pour une utilisation globale, fournissez le chemin complet au fichier :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6ef5-237">Windows</span><span class="sxs-lookup"><span data-stu-id="b6ef5-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="b6ef5-238">macOS</span><span class="sxs-lookup"><span data-stu-id="b6ef5-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6ef5-239">Linux</span><span class="sxs-lookup"><span data-stu-id="b6ef5-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="b6ef5-240">Dans l’exemple d’application (*HostingStartupApp*), pour permettre la détection du fichier de dépendances (*HostingStartupApp.runtimeconfig.json*), ce fichier est placé dans le profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6ef5-241">Windows</span><span class="sxs-lookup"><span data-stu-id="b6ef5-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="b6ef5-242">Définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à la valeur suivante :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="b6ef5-243">macOS</span><span class="sxs-lookup"><span data-stu-id="b6ef5-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="b6ef5-244">Définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à la valeur suivante :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6ef5-245">Linux</span><span class="sxs-lookup"><span data-stu-id="b6ef5-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="b6ef5-246">Définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à la valeur suivante :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="b6ef5-247">Pour obtenir des exemples montrant comment définir des variables d’environnement pour différents systèmes d’exploitation, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="b6ef5-248">**Déploiement**</span><span class="sxs-lookup"><span data-stu-id="b6ef5-248">**Deployment**</span></span>

<span data-ttu-id="b6ef5-249">Pour faciliter le déploiement d’un hébergement au démarrage dans un environnement multimachine, l’exemple d’application crée un dossier *deployment* dans la sortie publiée qui contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="b6ef5-250">L’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="b6ef5-251">Le fichier des dépendances de l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="b6ef5-252">Un script PowerShell qui crée ou modifie les variables `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` et `DOTNET_ADDITIONAL_DEPS` pour prendre en charge l’activation de l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="b6ef5-253">Exécutez ce script à partir d’une invite de commandes PowerShell d’administration sur le système de déploiement.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="b6ef5-254">Package NuGet</span><span class="sxs-lookup"><span data-stu-id="b6ef5-254">NuGet package</span></span>

<span data-ttu-id="b6ef5-255">Une amélioration de l’hébergement au démarrage peut être fournie dans un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="b6ef5-256">Le package a un attribut `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="b6ef5-257">Les types d’hébergement au démarrage fournis par le package sont mis à la disposition de l’application au moyen de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="b6ef5-258">Le fichier projet de l’application améliorée crée une référence de package à l’hébergement au démarrage dans le fichier projet de l’application (référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="b6ef5-259">Une fois la référence au moment de la compilation créée, l’assembly d’hébergement au démarrage et toutes ses dépendances sont ajoutés au fichier de dépendances de l’application (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="b6ef5-260">Cette approche s’applique à un package d’assembly d’hébergement au démarrage qui a été publié sur [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="b6ef5-261">Le fichier de dépendances de l’hébergement au démarrage est mis à la disposition de l’application améliorée de la façon décrite dans la section [Magasin de runtime](#runtime-store) (sans référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="b6ef5-262">Pour plus d’informations sur les packages NuGet et le magasin de runtime, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="b6ef5-263">Création d’un package NuGet avec les outils multiplateformes</span><span class="sxs-lookup"><span data-stu-id="b6ef5-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="b6ef5-264">Publication de packages</span><span class="sxs-lookup"><span data-stu-id="b6ef5-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="b6ef5-265">Magasin de packages de runtime</span><span class="sxs-lookup"><span data-stu-id="b6ef5-265">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="b6ef5-266">Dossier bin du projet</span><span class="sxs-lookup"><span data-stu-id="b6ef5-266">Project bin folder</span></span>

<span data-ttu-id="b6ef5-267">Une amélioration de l’hébergement au démarrage peut être fournie par un assembly déployé à partir du dossier *bin* dans l’application améliorée.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="b6ef5-268">Les types d’hébergement au démarrage fournis par l’assembly sont mis à la disposition de l’application au moyen de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="b6ef5-269">Le fichier projet de l’application améliorée crée une référence d’assembly à l’hébergement au démarrage (référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="b6ef5-270">Une fois la référence au moment de la compilation créée, l’assembly d’hébergement au démarrage et toutes ses dépendances sont ajoutés au fichier de dépendances de l’application (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="b6ef5-271">Cette approche s’applique quand le scénario de déploiement appelle le déplacement de l’assembly de la bibliothèque d’hébergement au démarrage compilée (fichier DLL) vers le projet de consommation, ou vers un emplacement auquel ce projet a accès, et quand une référence au moment de la compilation est créée vers l’assembly d’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="b6ef5-272">Le fichier de dépendances de l’hébergement au démarrage est mis à la disposition de l’application améliorée de la façon décrite dans la section [Magasin de runtime](#runtime-store) (sans référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="b6ef5-273">Exemple de code</span><span class="sxs-lookup"><span data-stu-id="b6ef5-273">Sample code</span></span>

<span data-ttu-id="b6ef5-274">[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([Comment télécharger un exemple](xref:tutorials/index#how-to-download-a-sample)) montre des scénarios d’implémentation de l’hébergement au démarrage :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="b6ef5-275">Deux assemblys d’hébergement au démarrage (bibliothèques de classes) définissent chacun une paire clé-valeur de configuration en mémoire :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="b6ef5-276">Package NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="b6ef5-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="b6ef5-277">Bibliothèque de classes (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="b6ef5-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="b6ef5-278">Un hébergement au démarrage est activé à partir d’un assembly déployé depuis le magasin de runtime (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="b6ef5-279">L’assembly ajoute deux middleware (intergiciels) à l’application au démarrage, qui fournissent des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="b6ef5-280">Services inscrits</span><span class="sxs-lookup"><span data-stu-id="b6ef5-280">Registered services</span></span>
  * <span data-ttu-id="b6ef5-281">Adresse (schéma, hôte, chemin de base, chemin, chaîne de requête)</span><span class="sxs-lookup"><span data-stu-id="b6ef5-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="b6ef5-282">Connexion (adresse IP distante, port distant, adresse IP locale, port local, certificat client)</span><span class="sxs-lookup"><span data-stu-id="b6ef5-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="b6ef5-283">En-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="b6ef5-283">Request headers</span></span>
  * <span data-ttu-id="b6ef5-284">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="b6ef5-284">Environment variables</span></span>

<span data-ttu-id="b6ef5-285">Pour exécuter l’exemple :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-285">To run the sample:</span></span>

<span data-ttu-id="b6ef5-286">**Activation à partir d’un package NuGet**</span><span class="sxs-lookup"><span data-stu-id="b6ef5-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="b6ef5-287">Compilez le package *HostingStartupPackage* à l’aide de la commande [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="b6ef5-288">Ajoutez le nom de l’assembly du package *HostingStartupPackage* à la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="b6ef5-289">Compilez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-289">Compile and run the app.</span></span> <span data-ttu-id="b6ef5-290">Une référence de package est présente dans l’application améliorée (référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="b6ef5-291">Un `<PropertyGroup>` dans le fichier projet de l’application spécifie la sortie du projet de package (*../HostingStartupPackage/bin/Debug*) comme source de package.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="b6ef5-292">L’application peut ainsi utiliser le package sans avoir à le charger sur [nuget.org](https://www.nuget.org/). Pour plus d’informations, consultez les remarques dans le fichier projet de l’application HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="b6ef5-293">Notez que les valeurs des clés de configuration de service affichées dans la page d’index correspondent aux valeurs définies par la méthode `ServiceKeyInjection.Configure` du package.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="b6ef5-294">Si vous modifiez le projet *HostingStartupPackage* et le recompilez, effacez les caches locaux du package NuGet pour vous assurer que *HostingStartupApp* reçoit le package mis à jour et pas un package périmé du cache local.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="b6ef5-295">Pour effacer les caches NuGet locaux, exécutez la commande [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) suivante :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="b6ef5-296">**Activation à partir d’une bibliothèque de classes**</span><span class="sxs-lookup"><span data-stu-id="b6ef5-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="b6ef5-297">Compilez la bibliothèque de classes *HostingStartupLibrary* à l’aide de la commande [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="b6ef5-298">Ajoutez le nom de l’assembly de la bibliothèque de classes *HostingStartupLibrary* à la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="b6ef5-299">À partir du dossier *bin*, déployez l’assembly de la bibliothèque de classes dans l’application en copiant le fichier *HostingStartupLibrary.dll* du résultat de la compilation de la bibliothèque de classes dans le dossier *bin/Debug* de l’application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="b6ef5-300">Compilez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-300">Compile and run the app.</span></span> <span data-ttu-id="b6ef5-301">Dans le fichier projet de l’application, un `<ItemGroup>` référence l’assembly de la bibliothèque de classes (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (référence au moment de la compilation).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="b6ef5-302">Pour plus d’informations, consultez les remarques dans le fichier projet de l’application HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="b6ef5-303">Notez que les valeurs des clés de configuration de service affichées dans la page d’index correspondent aux valeurs définies par la méthode `ServiceKeyInjection.Configure` de la bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="b6ef5-304">**Activation à partir d’un assembly déployé depuis le magasin de runtime**</span><span class="sxs-lookup"><span data-stu-id="b6ef5-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="b6ef5-305">Le projet *StartupDiagnostics* utilise [PowerShell](/powershell/scripting/powershell-scripting) pour modifier le fichier *StartupDiagnostics.deps.json* associé.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="b6ef5-306">PowerShell est installé par défaut sur Windows à compter de Windows 7 SP1 et de Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="b6ef5-307">Pour obtenir PowerShell sur d’autres plateformes, consultez [Installation de Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="b6ef5-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="b6ef5-308">Compilez le projet *StartupDiagnostics*.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="b6ef5-309">Une fois le projet compilé, une build cible dans le fichier projet lance automatiquement les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="b6ef5-310">Déclenche le script PowerShell pour modifier le fichier *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="b6ef5-311">Déplace le fichier *StartupDiagnostics.deps.json* dans le dossier *additionalDeps* du profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="b6ef5-312">Exécutez la commande `dotnet store` à partir d’une invite de commandes dans le répertoire de l’hébergement au démarrage pour stocker l’assembly et ses dépendances dans le magasin de runtime du profil utilisateur :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="b6ef5-313">Sur Windows, la commande utilise [l’identificateur de runtime](/dotnet/core/rid-catalog) `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="b6ef5-314">Si vous fournissez l’hébergement au démarrage pour un autre runtime, spécifiez l’identificateur de runtime approprié.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="b6ef5-315">Définissez les variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="b6ef5-315">Set the environment variables:</span></span>
   * <span data-ttu-id="b6ef5-316">Ajoutez le nom d’assembly de *StartupDiagnostics* à la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="b6ef5-317">Sur Windows, définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="b6ef5-318">Sur macOS/Linux, définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, où `<USER>` est le profil utilisateur contenant l’hébergement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="b6ef5-319">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-319">Run the sample app.</span></span>
1. <span data-ttu-id="b6ef5-320">Demandez le point de terminaison `/services` pour voir les services inscrits de l’application.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="b6ef5-321">Demandez le point de terminaison `/diag` pour voir les informations de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="b6ef5-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
