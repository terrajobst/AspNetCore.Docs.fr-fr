---
title: Précompilation et compilation de fichiers Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez les avantages de la précompilation des fichiers Razor et comment effectuer la précompilation des fichiers Razor dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: c4e8f722fdf3d3f64807cc35ff9f349af7f32abd
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248184"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="21c8d-103">Compilation de fichiers Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21c8d-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="21c8d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21c8d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="21c8d-105">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="21c8d-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="21c8d-106">La publication de fichiers Razor au moment de la génération n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="21c8d-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="21c8d-107">Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.</span><span class="sxs-lookup"><span data-stu-id="21c8d-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="21c8d-108">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="21c8d-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="21c8d-109">La publication de fichiers Razor au moment de la génération n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="21c8d-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="21c8d-110">Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.</span><span class="sxs-lookup"><span data-stu-id="21c8d-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="21c8d-111">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="21c8d-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="21c8d-112">Les fichiers Razor sont compilés au moment de la génération et de la publication à l’aide du kit [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="21c8d-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="21c8d-113">Considérations relatives à la précompilation</span><span class="sxs-lookup"><span data-stu-id="21c8d-113">Precompilation considerations</span></span>

<span data-ttu-id="21c8d-114">Les effets secondaires de la précompilation des fichiers Razor sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="21c8d-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="21c8d-115">Un plus petit bundle publié</span><span class="sxs-lookup"><span data-stu-id="21c8d-115">A smaller published bundle</span></span>
* <span data-ttu-id="21c8d-116">Un temps de démarrage plus court</span><span class="sxs-lookup"><span data-stu-id="21c8d-116">A faster startup time</span></span>
* <span data-ttu-id="21c8d-117">Vous ne pouvez pas modifier les fichiers Razor&mdash;le contenu associé est absent du bundle publié.</span><span class="sxs-lookup"><span data-stu-id="21c8d-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="21c8d-118">Déployer les fichiers précompilés</span><span class="sxs-lookup"><span data-stu-id="21c8d-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="21c8d-119">La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le kit SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="21c8d-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="21c8d-120">La modification des fichiers Razor une fois que ceux-ci sont mis à jour est prise en charge au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="21c8d-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="21c8d-121">Par défaut, seul le fichier *Views.dll* compilé est déployé avec votre application. Aucun fichier *.cshtml* n’est déployé.</span><span class="sxs-lookup"><span data-stu-id="21c8d-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21c8d-122">L’outil de précompilation sera supprimé dans ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="21c8d-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="21c8d-123">Nous vous recommandons de migrer vers le [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="21c8d-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="21c8d-124">Le kit SDK Razor est actif seulement si aucune propriété spécifique à la précompilation n’est définie dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="21c8d-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="21c8d-125">Par exemple, la définition de la propriété `MvcRazorCompileOnPublish` du fichier *.csproj* sur la valeur `true` désactive le kit SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="21c8d-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="21c8d-126">Si votre projet cible le .NET Framework, installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :</span><span class="sxs-lookup"><span data-stu-id="21c8d-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="21c8d-127">Si votre projet cible .NET Core, aucune modification n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="21c8d-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="21c8d-128">Les modèles de projet ASP.NET Core 2.x définissent implicitement la propriété `MvcRazorCompileOnPublish` sur la valeur `true` par défaut.</span><span class="sxs-lookup"><span data-stu-id="21c8d-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="21c8d-129">Par conséquent, cet élément peut être supprimé sans problème du fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="21c8d-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21c8d-130">L’outil de précompilation sera supprimé dans ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="21c8d-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="21c8d-131">Nous vous recommandons de migrer vers le [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="21c8d-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="21c8d-132">La précompilation de fichiers Razor n’est pas disponible quand vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="21c8d-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="21c8d-133">Définissez la propriété `MvcRazorCompileOnPublish` sur la valeur `true`, puis installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="21c8d-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="21c8d-134">Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :</span><span class="sxs-lookup"><span data-stu-id="21c8d-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="21c8d-135">Préparer l’application pour un [déploiement dépendant de l’infrastructure](/dotnet/core/deploying/#framework-dependent-deployments-fdd) avec la [commande de publication .NET Core CLI](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="21c8d-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="21c8d-136">Par exemple, exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="21c8d-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="21c8d-137">Un fichier *<nom_projet>.PrecompiledViews.dll*, contenant les fichiers Razor compilés, est généré lorsque la précompilation réussit.</span><span class="sxs-lookup"><span data-stu-id="21c8d-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="21c8d-138">Par exemple, la capture d’écran ci-dessous illustre le contenu du fichier *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="21c8d-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a><span data-ttu-id="21c8d-140">Recompiler les fichiers Razor en cas de modification</span><span class="sxs-lookup"><span data-stu-id="21c8d-140">Recompile Razor files on change</span></span>

<span data-ttu-id="21c8d-141"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> obtient ou définit une valeur qui détermine si les fichiers Razor (vues Razor et Razor Pages) sont recompilés et mis à jour si les fichiers changent sur le disque.</span><span class="sxs-lookup"><span data-stu-id="21c8d-141">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="21c8d-142">Lorsqu’il est défini sur `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) observe si des modifications sont apportées aux fichiers Razor dans les instances <xref:Microsoft.Extensions.FileProviders.IFileProvider> configurées.</span><span class="sxs-lookup"><span data-stu-id="21c8d-142">When set to `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) watches for changes to Razor files in configured <xref:Microsoft.Extensions.FileProviders.IFileProvider> instances.</span></span>

<span data-ttu-id="21c8d-143">La valeur par défaut de `true` pour :</span><span class="sxs-lookup"><span data-stu-id="21c8d-143">The default value is `true` for:</span></span>

* <span data-ttu-id="21c8d-144">Applications ASP.NET Core 2.1 ou versions antérieures.</span><span class="sxs-lookup"><span data-stu-id="21c8d-144">ASP.NET Core 2.1 or earlier apps.</span></span>
* <span data-ttu-id="21c8d-145">Applications ASP.NET Core 2.2 ou versions ultérieures dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="21c8d-145">ASP.NET Core 2.2 or later apps in the Development environment.</span></span>

<span data-ttu-id="21c8d-146"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> est associé à un commutateur de compatibilité et peut fournir un comportement différent selon la version de compatibilité configurée pour l’application.</span><span class="sxs-lookup"><span data-stu-id="21c8d-146"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is associated with a compatibility switch and can provide different behavior depending on the configured compatibility version for the app.</span></span> <span data-ttu-id="21c8d-147">La configuration de l’application en définissant <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> est prioritaire sur la valeur déduite à partir de la version de compatibilité de l’application.</span><span class="sxs-lookup"><span data-stu-id="21c8d-147">Configuring the app by setting <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> takes precedence over the value implied by the app's compatibility version.</span></span>

<span data-ttu-id="21c8d-148">Si la version de compatibilité de l’application est définie sur <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> ou une version antérieure, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> a la valeur `true` sauf configuration explicite.</span><span class="sxs-lookup"><span data-stu-id="21c8d-148">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is set to `true` unless explicitly configured.</span></span>

<span data-ttu-id="21c8d-149">Si la version de compatibilité de l’application est définie sur <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> ou une version ultérieure, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> a la valeur `false`, sauf si l’environnement est de type Développement ou si la valeur est explicitement configurée.</span><span class="sxs-lookup"><span data-stu-id="21c8d-149">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is set to `false` unless the environment is Development or the value is explicitly configured.</span></span>

<span data-ttu-id="21c8d-150">Pour des conseils et des exemples concernant la définition de la version de compatibilité de l’application, consultez <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="21c8d-150">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21c8d-151">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="21c8d-151">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
