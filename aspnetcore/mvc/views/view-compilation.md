---
title: Compilation de fichiers Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment se produit la compilation de fichiers Razor dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: cd096bba5eb580c0a606699a2bf7c36442fb56f7
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809066"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="0f9fb-103">Compilation de fichiers Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f9fb-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="0f9fb-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0f9fb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="0f9fb-105">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="0f9fb-106">La publication de fichiers Razor au moment de la génération n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="0f9fb-107">Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0f9fb-108">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="0f9fb-109">La publication de fichiers Razor au moment de la génération n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="0f9fb-110">Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="0f9fb-111">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="0f9fb-112">Les fichiers Razor sont compilés au moment de la génération et de la publication à l’aide du kit [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="0f9fb-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0f9fb-113">Les fichiers Razor avec une extension *. cshtml* sont compilés au moment de la génération et de la publication à l’aide du [Kit de développement logiciel (SDK) Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="0f9fb-113">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="0f9fb-114">La compilation au moment de l’exécution peut éventuellement être activée en configurant votre application.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="0f9fb-115">Compilation Razor</span><span class="sxs-lookup"><span data-stu-id="0f9fb-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0f9fb-116">La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le kit SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="0f9fb-117">Quand elle est activée, la compilation au moment de l’exécution complète la compilation au moment de la génération, ce qui permet aux fichiers Razor d’être mis à jour s’ils sont modifiés.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-117">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="0f9fb-118">La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le kit SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="0f9fb-119">La modification des fichiers Razor une fois que ceux-ci sont mis à jour est prise en charge au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="0f9fb-120">Par défaut, seuls les fichiers *Views.dll* et les fichiers autres que *.cshtml* compilés ou les assemblys de référence nécessaires à la compilation des fichiers Razor sont déployés avec votre application.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0f9fb-121">L’outil de précompilation est désormais déprécié et sera supprimé dans ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="0f9fb-122">Nous vous recommandons de migrer vers le [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="0f9fb-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="0f9fb-123">Le kit SDK Razor est actif seulement si aucune propriété spécifique à la précompilation n’est définie dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="0f9fb-124">Par exemple, la définition de la propriété `MvcRazorCompileOnPublish` du fichier *.csproj* sur la valeur `true` désactive le kit SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0f9fb-125">Si votre projet cible le .NET Framework, installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="0f9fb-126">Si votre projet cible .NET Core, aucune modification n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="0f9fb-127">Les modèles de projet ASP.NET Core 2.x définissent implicitement la propriété `MvcRazorCompileOnPublish` sur la valeur `true` par défaut.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="0f9fb-128">Par conséquent, cet élément peut être supprimé sans problème du fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0f9fb-129">L’outil de précompilation est désormais déprécié et sera supprimé dans ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="0f9fb-130">Nous vous recommandons de migrer vers le [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="0f9fb-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="0f9fb-131">La précompilation de fichiers Razor n’est pas disponible quand vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="0f9fb-132">Définissez la propriété `MvcRazorCompileOnPublish` sur la valeur `true`, puis installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="0f9fb-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="0f9fb-133">Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="0f9fb-134">Préparer l’application pour un [déploiement dépendant de l’infrastructure](/dotnet/core/deploying/#framework-dependent-deployments-fdd) avec la [commande de publication .NET Core CLI](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="0f9fb-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="0f9fb-135">Par exemple, exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-135">For example, execute the following command at the project root:</span></span>

```dotnetcli
dotnet publish -c Release
```

<span data-ttu-id="0f9fb-136">Un fichier *\<nom_projet>.PrecompiledViews.dll*, contenant les fichiers Razor compilés, est généré lorsque la précompilation réussit.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="0f9fb-137">Par exemple, la capture d’écran ci-dessous illustre le contenu du fichier *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="0f9fb-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="0f9fb-139">Compilation du runtime</span><span class="sxs-lookup"><span data-stu-id="0f9fb-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="0f9fb-140">La compilation au moment de la génération est complétée par la compilation au moment de l’exécution des fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="0f9fb-141">ASP.NET Core MVC recompile les fichiers Razor quand le contenu d’un fichier *.cshtml* change.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="0f9fb-142">La compilation au moment de la génération est complétée par la compilation au moment de l’exécution des fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="0f9fb-143">La <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> obtient ou définit une valeur qui détermine si les fichiers Razor (vues Razor et Razor Pages) sont recompilés et mis à jour si les fichiers sont modifiés sur le disque.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="0f9fb-144">La valeur par défaut de `true` pour :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-144">The default value is `true` for:</span></span>

* <span data-ttu-id="0f9fb-145">Si la version de compatibilité de l’application est définie sur <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> ou une version antérieure</span><span class="sxs-lookup"><span data-stu-id="0f9fb-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="0f9fb-146">Si la version de compatibilité de l’application est définie sur <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> ou ultérieur et que l’application se trouve dans l’environnement de développement <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-146">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="0f9fb-147">En d’autres termes, les fichiers Razor ne sont pas recompilés dans un environnement de non-développement, sauf si <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> est défini explicitement.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-147">In other words, Razor files wouldn't recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="0f9fb-148">Pour des conseils et des exemples concernant la définition de la version de compatibilité de l’application, consultez <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0f9fb-149">Pour activer la compilation au moment de l’exécution pour tous les environnements et modes de configuration :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-149">To enable runtime compilation for all environments and configuration modes:</span></span>

1. <span data-ttu-id="0f9fb-150">Installer le package NuGet [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/).</span><span class="sxs-lookup"><span data-stu-id="0f9fb-150">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>

1. <span data-ttu-id="0f9fb-151">Mettez à jour la méthode `Startup.ConfigureServices` du projet pour inclure un appel à `AddRazorRuntimeCompilation`.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-151">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="0f9fb-152">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-152">For example:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a><span data-ttu-id="0f9fb-153">Activation conditionnelle de la compilation du Runtime</span><span class="sxs-lookup"><span data-stu-id="0f9fb-153">Conditionally enable runtime compilation</span></span>

<span data-ttu-id="0f9fb-154">La compilation au moment de l’exécution peut être activée de sorte qu’elle ne soit disponible que pour le développement local.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-154">Runtime compilation can be enabled such that it's only available for local development.</span></span> <span data-ttu-id="0f9fb-155">L’activation conditionnelle de cette manière garantit que la sortie publiée :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-155">Conditionally enabling in this manner ensures that the published output:</span></span>

* <span data-ttu-id="0f9fb-156">Utilise des vues compilées.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-156">Uses compiled views.</span></span>
* <span data-ttu-id="0f9fb-157">Est de taille inférieure.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-157">Is smaller in size.</span></span>
* <span data-ttu-id="0f9fb-158">N’active pas les observateurs de fichiers en production.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-158">Doesn't enable file watchers in production.</span></span>

<span data-ttu-id="0f9fb-159">Pour activer la compilation au moment de l’exécution en fonction de l’environnement et du mode de configuration :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-159">To enable runtime compilation based on the environment and configuration mode:</span></span>

1. <span data-ttu-id="0f9fb-160">Référencez de manière conditionnelle le package [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) en fonction de la valeur active `Configuration` :</span><span class="sxs-lookup"><span data-stu-id="0f9fb-160">Conditionally reference the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) package based on the active `Configuration` value:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. <span data-ttu-id="0f9fb-161">Mettez à jour la méthode `Startup.ConfigureServices` du projet pour inclure un appel à `AddRazorRuntimeCompilation`.</span><span class="sxs-lookup"><span data-stu-id="0f9fb-161">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="0f9fb-162">Exécutez une `AddRazorRuntimeCompilation` de manière conditionnelle, de sorte qu’elle s’exécute uniquement en mode débogage lorsque la variable `ASPNETCORE_ENVIRONMENT` est définie sur `Development`:</span><span class="sxs-lookup"><span data-stu-id="0f9fb-162">Conditionally execute `AddRazorRuntimeCompilation` such that it only runs in Debug mode when the `ASPNETCORE_ENVIRONMENT` variable is set to `Development`:</span></span>

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0f9fb-163">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0f9fb-163">Additional resources</span></span>

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
