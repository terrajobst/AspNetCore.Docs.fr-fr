---
title: Précompilation et compilation de fichiers Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez les avantages de la précompilation des fichiers Razor et comment effectuer la précompilation des fichiers Razor dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 6ef450a24f57c721021f77f6df5088574caa2645
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274038"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="f3975-103">Compilation de fichiers Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3975-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="f3975-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f3975-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="f3975-105">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="f3975-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="f3975-106">La publication de fichiers Razor au moment de la génération n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="f3975-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="f3975-107">Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.</span><span class="sxs-lookup"><span data-stu-id="f3975-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="f3975-108">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="f3975-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="f3975-109">La publication de fichiers Razor au moment de la génération n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="f3975-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="f3975-110">Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.</span><span class="sxs-lookup"><span data-stu-id="f3975-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f3975-111">Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée.</span><span class="sxs-lookup"><span data-stu-id="f3975-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="f3975-112">Les fichiers Razor sont compilés au moment de la génération et de la publication à l’aide du kit [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f3975-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="f3975-113">Considérations relatives à la précompilation</span><span class="sxs-lookup"><span data-stu-id="f3975-113">Precompilation considerations</span></span>

<span data-ttu-id="f3975-114">Les effets secondaires de la précompilation des fichiers Razor sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="f3975-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="f3975-115">Un plus petit bundle publié</span><span class="sxs-lookup"><span data-stu-id="f3975-115">A smaller published bundle</span></span>
* <span data-ttu-id="f3975-116">Un temps de démarrage plus court</span><span class="sxs-lookup"><span data-stu-id="f3975-116">A faster startup time</span></span>
* <span data-ttu-id="f3975-117">Vous ne pouvez pas modifier les fichiers Razor&mdash;le contenu associé est absent du bundle publié.</span><span class="sxs-lookup"><span data-stu-id="f3975-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="f3975-118">Déployer les fichiers précompilés</span><span class="sxs-lookup"><span data-stu-id="f3975-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f3975-119">La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le kit SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="f3975-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="f3975-120">La modification des fichiers Razor une fois que ceux-ci sont mis à jour est prise en charge au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="f3975-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="f3975-121">Par défaut, seul le fichier *Views.dll* compilé est déployé avec votre application. Aucun fichier *.cshtml* n’est déployé.</span><span class="sxs-lookup"><span data-stu-id="f3975-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3975-122">Le kit SDK Razor est actif seulement si aucune propriété spécifique à la précompilation n’est définie dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="f3975-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="f3975-123">Par exemple, la définition de la propriété `MvcRazorCompileOnPublish` du fichier *.csproj* sur la valeur `true` désactive le kit SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="f3975-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="f3975-124">Si votre projet cible le .NET Framework, installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :</span><span class="sxs-lookup"><span data-stu-id="f3975-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

<span data-ttu-id="f3975-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span><span class="sxs-lookup"><span data-stu-id="f3975-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span></span>

<span data-ttu-id="f3975-126">Si votre projet cible .NET Core, aucune modification n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="f3975-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="f3975-127">Les modèles de projet ASP.NET Core 2.x définissent implicitement la propriété `MvcRazorCompileOnPublish` sur la valeur `true` par défaut.</span><span class="sxs-lookup"><span data-stu-id="f3975-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="f3975-128">Par conséquent, cet élément peut être supprimé sans problème du fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f3975-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3975-129">La précompilation de fichiers Razor n’est pas disponible quand vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="f3975-129">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="f3975-130">Définissez la propriété `MvcRazorCompileOnPublish` sur la valeur `true`, puis installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="f3975-130">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="f3975-131">Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :</span><span class="sxs-lookup"><span data-stu-id="f3975-131">The following *.csproj* sample highlights these settings:</span></span>

<span data-ttu-id="f3975-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span><span class="sxs-lookup"><span data-stu-id="f3975-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f3975-133">Préparer l’application pour un [déploiement dépendant de l’infrastructure](/dotnet/core/deploying/#framework-dependent-deployments-fdd) avec la [commande de publication .NET Core CLI](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="f3975-133">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="f3975-134">Par exemple, exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="f3975-134">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="f3975-135">Un fichier *<nom_projet>.PrecompiledViews.dll*, contenant les fichiers Razor compilés, est généré lorsque la précompilation réussit.</span><span class="sxs-lookup"><span data-stu-id="f3975-135">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="f3975-136">Par exemple, la capture d’écran ci-dessous illustre le contenu du fichier *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="f3975-136">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f3975-138">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f3975-138">Additional resources</span></span>

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