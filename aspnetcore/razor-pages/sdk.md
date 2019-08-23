---
title: SDK Razor ASP.NET Core
author: Rick-Anderson
description: Découvrez comment les pages Razor dans ASP.NET Core permettent de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: 1dc001c7c5fe320629835e06fe6db7fadabff94d
ms.sourcegitcommit: 6189b0ced9c115248c6ede02efcd0b29d31f2115
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69985394"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="b9807-103">SDK Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9807-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="b9807-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b9807-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="b9807-105">Présentation</span><span class="sxs-lookup"><span data-stu-id="b9807-105">Overview</span></span>

<span data-ttu-id="b9807-106">Le [!INCLUDE[](~/includes/2.1-SDK.md)] inclut le `Microsoft.NET.Sdk.Razor` MSBuild SDK (Kit de développement logiciel Razor).</span><span class="sxs-lookup"><span data-stu-id="b9807-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="b9807-107">Le SDK Razor :</span><span class="sxs-lookup"><span data-stu-id="b9807-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
* <span data-ttu-id="b9807-108">Normalise l’expérience liée à la génération, à l’empaquetage et à la publication de projets contenant des fichiers [Razor](xref:mvc/views/razor) pour les projets ASP.NET Core basés sur MVC.</span><span class="sxs-lookup"><span data-stu-id="b9807-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="b9807-109">Comprend un ensemble de cibles, de propriétés et d’éléments prédéfinis qui permettent de personnaliser la compilation des fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
* <span data-ttu-id="b9807-110">est requis pour générer, empaqueter et publier des projets contenant des fichiers [Razor](xref:mvc/views/razor) pour ASP.net Core projets basés sur MVC ou des projets [éblouissants](xref:blazor/index)</span><span class="sxs-lookup"><span data-stu-id="b9807-110">is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects, or [Blazor](xref:blazor/index) projects</span></span>
* <span data-ttu-id="b9807-111">Comprend un ensemble de cibles, de propriétés et d’éléments prédéfinis qui permettent de personnaliser la compilation des fichiers Razor (. cshtml ou. Razor).</span><span class="sxs-lookup"><span data-stu-id="b9807-111">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (.cshtml or .razor) files.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="b9807-112">Le kit de développement logiciel `<Content>` (SDK) `Include` Razor comprend un élément `**\*.cshtml` dont l’attribut a pour valeur le modèle globbing.</span><span class="sxs-lookup"><span data-stu-id="b9807-112">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="b9807-113">Les fichiers correspondants sont publiés.</span><span class="sxs-lookup"><span data-stu-id="b9807-113">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9807-114">Le kit de développement `<Content>` logiciel ( `Include` SDK) Razor contient des `**\*.razor` éléments dont les attributs sont définis sur les `**\*.cshtml` modèles et globbing.</span><span class="sxs-lookup"><span data-stu-id="b9807-114">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="b9807-115">Les fichiers correspondants sont publiés.</span><span class="sxs-lookup"><span data-stu-id="b9807-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="b9807-116">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b9807-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="b9807-117">Utiliser le kit de développement logiciel (SDK) Razor</span><span class="sxs-lookup"><span data-stu-id="b9807-117">Use the Razor SDK</span></span>

<span data-ttu-id="b9807-118">La plupart des applications web ne sont pas requis pour référencer explicitement le SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
<span data-ttu-id="b9807-119">Pour utiliser le SDK Razor pour générer des bibliothèques de classes contenant des vues ou des pages Razor :</span><span class="sxs-lookup"><span data-stu-id="b9807-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="b9807-120">Utilisez `Microsoft.NET.Sdk.Razor` à la place de `Microsoft.NET.Sdk` :</span><span class="sxs-lookup"><span data-stu-id="b9807-120">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="b9807-121">En règle générale, une référence de package à `Microsoft.AspNetCore.Mvc` est nécessaire pour recevoir des dépendances supplémentaires sont nécessaires pour générer et compiler les Pages Razor et les vues Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-121">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="b9807-122">Au minimum, votre projet doit ajouter des références de package pour :</span><span class="sxs-lookup"><span data-stu-id="b9807-122">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="b9807-123">Le `Microsoft.AspNetCore.Razor.Design` package fournit les tâches de compilation Razor et les cibles pour le projet.</span><span class="sxs-lookup"><span data-stu-id="b9807-123">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="b9807-124">Les packages précédents sont inclus dans `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="b9807-124">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="b9807-125">Le balisage suivant montre un fichier projet qui utilise le SDK de Razor pour générer les fichiers pour une application ASP.NET Core Razor Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="b9807-125">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="b9807-126">Pour utiliser le kit de développement logiciel (SDK) Razor afin de générer des bibliothèques de classes contenant des vues Razor ou des Razor Pages nous vous recommandons de commencer par le modèle de projet Bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-126">To use the Razor SDK to build class libraries containing Razor views or Razor Pages we recommend starting with the Razor Class Library project template.</span></span> <span data-ttu-id="b9807-127">Une bibliothèque de classes Razor qui est utilisée pour générer des `Microsoft.AspNetCore.Components` fichiers éblouissants (. Razor) nécessite au minimum une référence au package.</span><span class="sxs-lookup"><span data-stu-id="b9807-127">A Razor class library that is used to build Blazor (.razor) files will at minimum require a reference to the `Microsoft.AspNetCore.Components` package.</span></span> <span data-ttu-id="b9807-128">Une bibliothèque de classes Razor qui est utilisée pour générer des vues ou des pages Razor (fichiers. cshtml) nécessite `netcoreapp3.0` le ciblage ou une version `FrameworkReference` plus `Microsoft.AspNetCore.App`récente et a un à.</span><span class="sxs-lookup"><span data-stu-id="b9807-128">A Razor class library that is used to build Razor views or pages (.cshtml files), will require targeting `netcoreapp3.0` or newer and have a `FrameworkReference` to `Microsoft.AspNetCore.App`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="b9807-129">Le `Microsoft.AspNetCore.Razor.Design` et `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages sont inclus dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b9807-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b9807-130">Toutefois, la version sans `Microsoft.AspNetCore.App` référence de package fournit un métapackage à l’application qui n’inclut pas la dernière version de `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="b9807-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="b9807-131">Projets doivent faire référence à une version cohérente du `Microsoft.AspNetCore.Razor.Design` (ou `Microsoft.AspNetCore.Mvc`) afin que les derniers correctifs du moment de la génération pour Razor sont inclus.</span><span class="sxs-lookup"><span data-stu-id="b9807-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="b9807-132">Pour plus d’informations, consultez [ce problème GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="b9807-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="b9807-133">Properties</span><span class="sxs-lookup"><span data-stu-id="b9807-133">Properties</span></span>

<span data-ttu-id="b9807-134">Les propriétés suivantes contrôlent le comportement du SDK Razor dans le cadre d’une build de projet :</span><span class="sxs-lookup"><span data-stu-id="b9807-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="b9807-135">`RazorCompileOnBuild` &ndash; Lorsque `true`, compile et émet l’assembly Razor dans le cadre de la génération du projet.</span><span class="sxs-lookup"><span data-stu-id="b9807-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="b9807-136">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="b9807-136">Defaults to `true`.</span></span>
* <span data-ttu-id="b9807-137">`RazorCompileOnPublish` &ndash; Lorsque `true`, compile et émet l’assembly Razor dans le cadre de la publication du projet.</span><span class="sxs-lookup"><span data-stu-id="b9807-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="b9807-138">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="b9807-138">Defaults to `true`.</span></span>

<span data-ttu-id="b9807-139">Les propriétés et les éléments dans le tableau suivant sont utilisés pour configurer les entrées et sortie pour le SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"
> [!WARNING]
<span data-ttu-id="b9807-140">À compter de ASP.net Core 3,0, les affichages MVC ou Razor pages ne seront pas pris `RazorCompileOnBuild` en `RazorCompileOnPublish` charge par défaut si ou est désactivé.</span><span class="sxs-lookup"><span data-stu-id="b9807-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages will not be served by default if `RazorCompileOnBuild` or `RazorCompileOnPublish` is disabled.</span></span> <span data-ttu-id="b9807-141">Les applications doivent ajouter une référence explicite `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` au package pour ajouter la prise en charge de la compilation du Runtime si elles s’appuient sur la compilation du runtime pour traiter les fichiers cshtml.</span><span class="sxs-lookup"><span data-stu-id="b9807-141">Applications must add an explicit reference to `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package to add support for runtime compilation if they rely on runtime compilation to process cshtml files.</span></span>
::: moniker-end


| <span data-ttu-id="b9807-142">Éléments</span><span class="sxs-lookup"><span data-stu-id="b9807-142">Items</span></span> | <span data-ttu-id="b9807-143">Description</span><span class="sxs-lookup"><span data-stu-id="b9807-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="b9807-144">Éléments Item (fichiers *. cshtml* ) qui sont des entrées de la génération de code.</span><span class="sxs-lookup"><span data-stu-id="b9807-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="b9807-145">Éléments Item (fichiers *. Razor* ) qui sont des entrées de la génération de code du composant.</span><span class="sxs-lookup"><span data-stu-id="b9807-145">Item elements (*.razor* files) that are inputs to Component code generation.</span></span>
| `RazorCompile` | <span data-ttu-id="b9807-146">Éléments Item (fichiers *. cs* ) qui sont des entrées dans les cibles de compilation Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="b9807-147">Utilisez cette `ItemGroup` valeur pour spécifier des fichiers supplémentaires à compiler dans l’assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="b9807-148">Composants d’élément utilisés pour générer le code d’attributs pour l’assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="b9807-149">Exemple :</span><span class="sxs-lookup"><span data-stu-id="b9807-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="b9807-150">Éléments ajoutés en tant que ressources incorporées à l’assembly généré de Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="b9807-151">Propriété</span><span class="sxs-lookup"><span data-stu-id="b9807-151">Property</span></span> | <span data-ttu-id="b9807-152">Description</span><span class="sxs-lookup"><span data-stu-id="b9807-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="b9807-153">Nom de fichier (sans extension) de l’assembly produit par Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-153">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="b9807-154">Répertoire de sortie Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="b9807-155">Permet de déterminer l’ensemble d’outils utilisé pour générer l’assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="b9807-156">Les valeurs valides sont `Implicit`, `RazorSDK` et `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="b9807-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="b9807-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="b9807-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="b9807-158">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="b9807-158">Default is `true`.</span></span> <span data-ttu-id="b9807-159">Lorsque `true`, comprend des fichiers *Web. config*, *. JSON*et *. cshtml* en tant que contenu dans le projet.</span><span class="sxs-lookup"><span data-stu-id="b9807-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="b9807-160">Lorsque référencé par le biais de `Microsoft.NET.Sdk.Web`, les fichiers sous *wwwroot* et fichiers de configuration sont également incluses.</span><span class="sxs-lookup"><span data-stu-id="b9807-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="b9807-161">Si la valeur est `true`, inclut les fichiers *.cshtml* des éléments `Content` dans les éléments `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="b9807-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="b9807-162">Lorsque `true`, génère un *.cs* fichier contenant les attributs spécifiés par `RazorAssemblyAttribute` et inclut le fichier dans la sortie de la compilation.</span><span class="sxs-lookup"><span data-stu-id="b9807-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="b9807-163">Si la valeur est `true`, ajoute un ensemble par défaut d’attributs d’assembly à `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="b9807-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="b9807-164">Lorsque `true`, copies `RazorGenerate` éléments ( *.cshtml*) fichiers au répertoire de publication.</span><span class="sxs-lookup"><span data-stu-id="b9807-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="b9807-165">En règle générale, les fichiers Razor ne sont pas nécessaires pour une application publiée si elles participent compilation au moment de la génération ou au moment de publier.</span><span class="sxs-lookup"><span data-stu-id="b9807-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="b9807-166">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="b9807-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="b9807-167">Si la valeur est `true`, copie les éléments d’assembly de référence dans le répertoire de publication.</span><span class="sxs-lookup"><span data-stu-id="b9807-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="b9807-168">En règle générale, les assemblys de référence ne sont pas nécessaires pour une application publiée si compilation Razor se produit au moment de la génération ou au moment de publier.</span><span class="sxs-lookup"><span data-stu-id="b9807-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="b9807-169">La valeur `true` si votre application publiée nécessite une compilation de runtime.</span><span class="sxs-lookup"><span data-stu-id="b9807-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="b9807-170">Par exemple, la valeur est la valeur `true` si l’application modifie *.cshtml* lors de l’exécution de fichiers ou utilise des vues incorporés.</span><span class="sxs-lookup"><span data-stu-id="b9807-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="b9807-171">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="b9807-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="b9807-172">Lorsque `true`, tous les éléments de contenu Razor ( *.cshtml* fichiers) sont marqués pour inclusion dans le package NuGet généré.</span><span class="sxs-lookup"><span data-stu-id="b9807-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="b9807-173">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="b9807-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="b9807-174">Si la valeur est `true`, ajoute des éléments RazorGenerate ( *.cshtml*) comme fichiers incorporés à l’assembly Razor généré.</span><span class="sxs-lookup"><span data-stu-id="b9807-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="b9807-175">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="b9807-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="b9807-176">Si la valeur est `true`, utilise un processus de serveur de build persistant pour décharger le travail de génération de code.</span><span class="sxs-lookup"><span data-stu-id="b9807-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="b9807-177">Utilise par défaut la valeur de `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="b9807-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="b9807-178">Lorsque `true`la valeur est, le kit de développement logiciel (SDK) génère des attributs supplémentaires utilisés par MVC au moment de l’exécution pour effectuer la découverte des parties</span><span class="sxs-lookup"><span data-stu-id="b9807-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |

<span data-ttu-id="b9807-179">Pour plus d’informations sur les propriétés, voir [Propriétés MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="b9807-179">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="b9807-180">Cibles</span><span class="sxs-lookup"><span data-stu-id="b9807-180">Targets</span></span>

<span data-ttu-id="b9807-181">Le SDK Razor définit deux cibles principales :</span><span class="sxs-lookup"><span data-stu-id="b9807-181">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="b9807-182">`RazorGenerate` &ndash; Génère du code *.cs* fichiers à partir de `RazorGenerate` des éléments item.</span><span class="sxs-lookup"><span data-stu-id="b9807-182">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="b9807-183">Utilisez la propriété `RazorGenerateDependsOn` pour spécifier des cibles supplémentaires pouvant s’exécuter avant ou après cette cible.</span><span class="sxs-lookup"><span data-stu-id="b9807-183">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="b9807-184">`RazorCompile` &ndash; Compile généré *.cs* dans des fichiers à un assembly de Razor.</span><span class="sxs-lookup"><span data-stu-id="b9807-184">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="b9807-185">Utilisez `RazorCompileDependsOn` pour spécifier des cibles supplémentaires qui peuvent s’exécuter avant ou après cette cible.</span><span class="sxs-lookup"><span data-stu-id="b9807-185">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="b9807-186">`RazorComponentGenerate`Le code génère des fichiers *. cs* pour les `RazorComponent` éléments Item. &ndash;</span><span class="sxs-lookup"><span data-stu-id="b9807-186">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="b9807-187">Utilisez la propriété `RazorComponentGenerateDependsOn` pour spécifier des cibles supplémentaires pouvant s’exécuter avant ou après cette cible.</span><span class="sxs-lookup"><span data-stu-id="b9807-187">Use `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="b9807-188">Compilation au moment du runtime des vues Razor</span><span class="sxs-lookup"><span data-stu-id="b9807-188">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="b9807-189">Par défaut, le SDK Razor ne publie pas les assemblys de référence nécessaires à l’exécution de la compilation au moment du runtime.</span><span class="sxs-lookup"><span data-stu-id="b9807-189">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="b9807-190">Cela entraîne des échecs de compilation quand le modèle d’application s’appuie sur la compilation au moment du runtime&mdash;par exemple, l’application utilise des vues incorporées ou change les vues une fois l’application publiée.</span><span class="sxs-lookup"><span data-stu-id="b9807-190">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="b9807-191">Définissez `CopyRefAssembliesToPublishDirectory` sur `true` pour continuer à publier les assemblys de référence.</span><span class="sxs-lookup"><span data-stu-id="b9807-191">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="b9807-192">Pour une application web, vérifiez que votre application cible le `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="b9807-192">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="b9807-193">Version du langage Razor</span><span class="sxs-lookup"><span data-stu-id="b9807-193">Razor language version</span></span>

<span data-ttu-id="b9807-194">Lorsque vous ciblez le `Microsoft.NET.Sdk.Web` Kit de développement logiciel (SDK), la version du langage Razor est déduite de la version du Framework cible de l’application.</span><span class="sxs-lookup"><span data-stu-id="b9807-194">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="b9807-195">Pour les projets ciblant le `Microsoft.NET.Sdk.Razor` Kit de développement logiciel (SDK) ou dans le cas rare où l’application requiert une version de langage Razor différente de la valeur déduite, une version peut être configurée en définissant la `<RazorLangVersion>` propriété dans le fichier projet de l’application:</span><span class="sxs-lookup"><span data-stu-id="b9807-195">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="b9807-196">La version linguistique de Razor est étroitement intégrée à la version du runtime pour laquelle elle a été générée.</span><span class="sxs-lookup"><span data-stu-id="b9807-196">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="b9807-197">Le ciblage d’une version de langage qui n’est pas conçue pour le runtime n’est pas pris en charge et génère probablement des erreurs de Build.</span><span class="sxs-lookup"><span data-stu-id="b9807-197">Targeting a language version that is not designed for the runtime is unsupported, and will likely produce build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9807-198">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b9807-198">Additional resources</span></span>

* [<span data-ttu-id="b9807-199">Ajouts au format csproj pour .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9807-199">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="b9807-200">Éléments communs des projets MSBuild</span><span class="sxs-lookup"><span data-stu-id="b9807-200">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
