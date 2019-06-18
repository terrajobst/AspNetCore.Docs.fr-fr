---
title: SDK Razor ASP.NET Core
author: Rick-Anderson
description: Découvrez comment les pages Razor dans ASP.NET Core permettent de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: fa69e4840377e0c1c8291c7ba9305a27bd3e6b82
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196359"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="7de43-103">SDK Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7de43-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="7de43-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7de43-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="7de43-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="7de43-105">Overview</span></span>

<span data-ttu-id="7de43-106">Le [!INCLUDE[](~/includes/2.1-SDK.md)] inclut le `Microsoft.NET.Sdk.Razor` MSBuild SDK (Kit de développement logiciel Razor).</span><span class="sxs-lookup"><span data-stu-id="7de43-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="7de43-107">Le SDK Razor :</span><span class="sxs-lookup"><span data-stu-id="7de43-107">The Razor SDK:</span></span>

* <span data-ttu-id="7de43-108">Normalise l’expérience liée à la génération, à l’empaquetage et à la publication de projets contenant des fichiers [Razor](xref:mvc/views/razor) pour les projets ASP.NET Core basés sur MVC.</span><span class="sxs-lookup"><span data-stu-id="7de43-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="7de43-109">Comprend un ensemble de cibles, de propriétés et d’éléments prédéfinis qui permettent de personnaliser la compilation des fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="7de43-110">Le SDK Razor inclut un `<Content>` élément avec un `Include` attribut défini sur le `**\*.cshtml` modèle de globbing.</span><span class="sxs-lookup"><span data-stu-id="7de43-110">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="7de43-111">Fichiers correspondants sont publiés.</span><span class="sxs-lookup"><span data-stu-id="7de43-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7de43-112">Inclut le SDK Razor `<Content>` éléments avec `Include` les attributs définis pour le `**\*.cshtml` et `**\*.razor` modèles de caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="7de43-112">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="7de43-113">Fichiers correspondants sont publiés.</span><span class="sxs-lookup"><span data-stu-id="7de43-113">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="7de43-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="7de43-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="7de43-115">Utiliser le Kit de développement logiciel de Razor</span><span class="sxs-lookup"><span data-stu-id="7de43-115">Use the Razor SDK</span></span>

<span data-ttu-id="7de43-116">La plupart des applications web ne sont pas requis pour référencer explicitement le SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-116">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="7de43-117">Pour utiliser le SDK Razor pour générer des bibliothèques de classes contenant des vues ou des pages Razor :</span><span class="sxs-lookup"><span data-stu-id="7de43-117">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="7de43-118">Utilisez `Microsoft.NET.Sdk.Razor` à la place de `Microsoft.NET.Sdk` :</span><span class="sxs-lookup"><span data-stu-id="7de43-118">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="7de43-119">En règle générale, une référence de package à `Microsoft.AspNetCore.Mvc` est nécessaire pour recevoir des dépendances supplémentaires sont nécessaires pour générer et compiler les Pages Razor et les vues Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-119">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="7de43-120">Au minimum, votre projet doit ajouter des références de package pour :</span><span class="sxs-lookup"><span data-stu-id="7de43-120">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="7de43-121">Le `Microsoft.AspNetCore.Razor.Design` package fournit les tâches de compilation Razor et les cibles pour le projet.</span><span class="sxs-lookup"><span data-stu-id="7de43-121">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="7de43-122">Les packages précédents sont inclus dans `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="7de43-122">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="7de43-123">Le balisage suivant montre un fichier projet qui utilise le SDK de Razor pour générer les fichiers pour une application ASP.NET Core Razor Pages Razor :</span><span class="sxs-lookup"><span data-stu-id="7de43-123">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="7de43-124">Le `Microsoft.AspNetCore.Razor.Design` et `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages sont inclus dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="7de43-124">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="7de43-125">Toutefois, la version sans `Microsoft.AspNetCore.App` référence de package fournit un métapackage à l’application qui n’inclut pas la dernière version de `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="7de43-125">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="7de43-126">Projets doivent faire référence à une version cohérente du `Microsoft.AspNetCore.Razor.Design` (ou `Microsoft.AspNetCore.Mvc`) afin que les derniers correctifs du moment de la génération pour Razor sont inclus.</span><span class="sxs-lookup"><span data-stu-id="7de43-126">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="7de43-127">Pour plus d’informations, consultez [ce problème GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="7de43-127">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="7de43-128">Properties</span><span class="sxs-lookup"><span data-stu-id="7de43-128">Properties</span></span>

<span data-ttu-id="7de43-129">Les propriétés suivantes contrôlent le comportement du SDK Razor dans le cadre d’une build de projet :</span><span class="sxs-lookup"><span data-stu-id="7de43-129">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="7de43-130">`RazorCompileOnBuild` &ndash; Lorsque `true`, compile et émet l’assembly Razor dans le cadre de la génération du projet.</span><span class="sxs-lookup"><span data-stu-id="7de43-130">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="7de43-131">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="7de43-131">Defaults to `true`.</span></span>
* <span data-ttu-id="7de43-132">`RazorCompileOnPublish` &ndash; Lorsque `true`, compile et émet l’assembly Razor dans le cadre de la publication du projet.</span><span class="sxs-lookup"><span data-stu-id="7de43-132">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="7de43-133">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="7de43-133">Defaults to `true`.</span></span>

<span data-ttu-id="7de43-134">Les propriétés et les éléments dans le tableau suivant sont utilisés pour configurer les entrées et sortie pour le SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-134">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="7de43-135">Éléments</span><span class="sxs-lookup"><span data-stu-id="7de43-135">Items</span></span> | <span data-ttu-id="7de43-136">Description</span><span class="sxs-lookup"><span data-stu-id="7de43-136">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="7de43-137">Composants d’élément (fichiers *.cshtml*) fournis en entrée aux cibles de génération de code.</span><span class="sxs-lookup"><span data-stu-id="7de43-137">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="7de43-138">Des éléments Item ( *.cs* fichiers) qui sont des entrées aux cibles de compilation Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-138">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="7de43-139">Utilisez cette `ItemGroup` pour spécifier des fichiers supplémentaires à être compilé dans l’assembly de Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-139">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="7de43-140">Composants d’élément utilisés pour générer le code d’attributs pour l’assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-140">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="7de43-141">Exemple :</span><span class="sxs-lookup"><span data-stu-id="7de43-141">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="7de43-142">Éléments ajoutés en tant que ressources incorporées à l’assembly généré de Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-142">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="7de43-143">Propriété</span><span class="sxs-lookup"><span data-stu-id="7de43-143">Property</span></span> | <span data-ttu-id="7de43-144">Description</span><span class="sxs-lookup"><span data-stu-id="7de43-144">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="7de43-145">Nom de fichier (sans extension) de l’assembly produit par Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-145">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="7de43-146">Répertoire de sortie Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-146">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="7de43-147">Permet de déterminer l’ensemble d’outils utilisé pour générer l’assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-147">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="7de43-148">Les valeurs valides sont `Implicit`, `RazorSDK` et `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="7de43-148">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="7de43-149">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="7de43-149">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="7de43-150">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="7de43-150">Default is `true`.</span></span> <span data-ttu-id="7de43-151">Lorsque `true`, inclut *web.config*, *.json*, et *.cshtml* fichiers en tant que contenu dans le projet.</span><span class="sxs-lookup"><span data-stu-id="7de43-151">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="7de43-152">Lorsque référencé par le biais de `Microsoft.NET.Sdk.Web`, les fichiers sous *wwwroot* et fichiers de configuration sont également incluses.</span><span class="sxs-lookup"><span data-stu-id="7de43-152">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="7de43-153">Si la valeur est `true`, inclut les fichiers *.cshtml* des éléments `Content` dans les éléments `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="7de43-153">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="7de43-154">Lorsque `true`, génère un *.cs* fichier contenant les attributs spécifiés par `RazorAssemblyAttribute` et inclut le fichier dans la sortie de la compilation.</span><span class="sxs-lookup"><span data-stu-id="7de43-154">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="7de43-155">Si la valeur est `true`, ajoute un ensemble par défaut d’attributs d’assembly à `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7de43-155">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="7de43-156">Lorsque `true`, copies `RazorGenerate` éléments ( *.cshtml*) fichiers au répertoire de publication.</span><span class="sxs-lookup"><span data-stu-id="7de43-156">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="7de43-157">En règle générale, les fichiers Razor ne sont pas nécessaires pour une application publiée si elles participent compilation au moment de la génération ou au moment de publier.</span><span class="sxs-lookup"><span data-stu-id="7de43-157">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="7de43-158">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="7de43-158">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="7de43-159">Si la valeur est `true`, copie les éléments d’assembly de référence dans le répertoire de publication.</span><span class="sxs-lookup"><span data-stu-id="7de43-159">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="7de43-160">En règle générale, les assemblys de référence ne sont pas nécessaires pour une application publiée si compilation Razor se produit au moment de la génération ou au moment de publier.</span><span class="sxs-lookup"><span data-stu-id="7de43-160">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="7de43-161">La valeur `true` si votre application publiée nécessite une compilation de runtime.</span><span class="sxs-lookup"><span data-stu-id="7de43-161">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="7de43-162">Par exemple, la valeur est la valeur `true` si l’application modifie *.cshtml* lors de l’exécution de fichiers ou utilise des vues incorporés.</span><span class="sxs-lookup"><span data-stu-id="7de43-162">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="7de43-163">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="7de43-163">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="7de43-164">Lorsque `true`, tous les éléments de contenu Razor ( *.cshtml* fichiers) sont marqués pour inclusion dans le package NuGet généré.</span><span class="sxs-lookup"><span data-stu-id="7de43-164">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="7de43-165">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="7de43-165">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="7de43-166">Si la valeur est `true`, ajoute des éléments RazorGenerate ( *.cshtml*) comme fichiers incorporés à l’assembly Razor généré.</span><span class="sxs-lookup"><span data-stu-id="7de43-166">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="7de43-167">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="7de43-167">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="7de43-168">Si la valeur est `true`, utilise un processus de serveur de build persistant pour décharger le travail de génération de code.</span><span class="sxs-lookup"><span data-stu-id="7de43-168">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="7de43-169">Utilise par défaut la valeur de `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="7de43-169">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="7de43-170">Pour plus d’informations sur les propriétés, voir [Propriétés MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="7de43-170">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="7de43-171">Cibles</span><span class="sxs-lookup"><span data-stu-id="7de43-171">Targets</span></span>

<span data-ttu-id="7de43-172">Le SDK Razor définit deux cibles principales :</span><span class="sxs-lookup"><span data-stu-id="7de43-172">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="7de43-173">`RazorGenerate` &ndash; Génère du code *.cs* fichiers à partir de `RazorGenerate` des éléments item.</span><span class="sxs-lookup"><span data-stu-id="7de43-173">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="7de43-174">Utilisez la propriété `RazorGenerateDependsOn` pour spécifier des cibles supplémentaires pouvant s’exécuter avant ou après cette cible.</span><span class="sxs-lookup"><span data-stu-id="7de43-174">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="7de43-175">`RazorCompile` &ndash; Compile généré *.cs* dans des fichiers à un assembly de Razor.</span><span class="sxs-lookup"><span data-stu-id="7de43-175">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="7de43-176">Utilisez `RazorCompileDependsOn` pour spécifier des cibles supplémentaires qui peuvent s’exécuter avant ou après cette cible.</span><span class="sxs-lookup"><span data-stu-id="7de43-176">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="7de43-177">Compilation au moment du runtime des vues Razor</span><span class="sxs-lookup"><span data-stu-id="7de43-177">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="7de43-178">Par défaut, le SDK Razor ne publie pas les assemblys de référence nécessaires à l’exécution de la compilation au moment du runtime.</span><span class="sxs-lookup"><span data-stu-id="7de43-178">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="7de43-179">Cela entraîne des échecs de compilation quand le modèle d’application s’appuie sur la compilation au moment du runtime&mdash;par exemple, l’application utilise des vues incorporées ou change les vues une fois l’application publiée.</span><span class="sxs-lookup"><span data-stu-id="7de43-179">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="7de43-180">Définissez `CopyRefAssembliesToPublishDirectory` sur `true` pour continuer à publier les assemblys de référence.</span><span class="sxs-lookup"><span data-stu-id="7de43-180">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="7de43-181">Pour une application web, vérifiez que votre application cible le `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="7de43-181">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="7de43-182">Version de langage Razor</span><span class="sxs-lookup"><span data-stu-id="7de43-182">Razor language version</span></span>

<span data-ttu-id="7de43-183">Lorsque vous ciblez le `Microsoft.NET.Sdk.Web` SDK, la version de langage Razor est déduite à partir de la version du framework cible de l’application.</span><span class="sxs-lookup"><span data-stu-id="7de43-183">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="7de43-184">Pour les projets ciblant le `Microsoft.NET.Sdk.Razor` SDK ou dans les rares cas que l’application requiert une autre version de langage Razor à la valeur déduite, une version peut être configurée en définissant le `<RazorLangVersion>` propriété dans le fichier de projet application :</span><span class="sxs-lookup"><span data-stu-id="7de43-184">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

## <a name="additional-resources"></a><span data-ttu-id="7de43-185">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7de43-185">Additional resources</span></span>

* [<span data-ttu-id="7de43-186">Ajouts au format csproj pour .NET Core</span><span class="sxs-lookup"><span data-stu-id="7de43-186">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="7de43-187">Éléments communs des projets MSBuild</span><span class="sxs-lookup"><span data-stu-id="7de43-187">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
