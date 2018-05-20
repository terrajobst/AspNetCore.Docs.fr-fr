---
title: Précompilation et compilation de vues Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment activer la précompilation et la compilation de vues Razor MVC dans les applications ASP.Net Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a><span data-ttu-id="c55c3-103">Compilation du fichier Razor (.cshtml) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c55c3-103">Razor file (.cshtml) compilation in ASP.NET Core</span></span>

<span data-ttu-id="c55c3-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c55c3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c55c3-105">Les vues Razor sont compilées à l'exécution lors de leur premier appel.</span><span class="sxs-lookup"><span data-stu-id="c55c3-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="c55c3-106">ASP.NET Core 2.1.0 ou ultérieur compile les vues au moment de la génération et de la publication à l’aide du [SDK Razor](/aspnetcore/mvc/razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="c55c3-106">ASP.NET Core 2.1.0 or later compile views at build and publish time using [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk).</span></span> <span data-ttu-id="c55c3-107">Dans ASP.NET Core 1.1 et ASP.NET Core 2.0, vous pouvez compiler les vues au moment de la publication et les déployer avec l’application à l’aide de l’outil de précompilation.</span><span class="sxs-lookup"><span data-stu-id="c55c3-107">In ASP.NET Core 1.1, and ASP.NET Core 2.0, views can optionally be compiled at publish and deployed with the app &mdash; using the precompilation tool.</span></span> 



<span data-ttu-id="c55c3-108">Considérations relatives à la précompilation :</span><span class="sxs-lookup"><span data-stu-id="c55c3-108">Precompilation considerations:</span></span>

* <span data-ttu-id="c55c3-109">La précompilation des vues réduit la taille du bundle publié et le temps de démarrage.</span><span class="sxs-lookup"><span data-stu-id="c55c3-109">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="c55c3-110">Vous ne pouvez pas modifier les vues précompilées.</span><span class="sxs-lookup"><span data-stu-id="c55c3-110">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="c55c3-111">Elles ne font pas partie de l’application publiée.</span><span class="sxs-lookup"><span data-stu-id="c55c3-111">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="c55c3-112">Pour déployer des vues précompilées :</span><span class="sxs-lookup"><span data-stu-id="c55c3-112">To deploy precompiled views:</span></span>

# <a name="aspnet-core-21tabaspnetcore21"></a>[<span data-ttu-id="c55c3-113">ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="c55c3-113">ASP.NET Core 2.1</span></span>](#tab/aspnetcore21/)
<span data-ttu-id="c55c3-114">La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="c55c3-114">Build and publish time compilation of Razor files is enabled by default by the Razor Sdk.</span></span> <span data-ttu-id="c55c3-115">La modification des fichiers Razor une fois qu’ils sont mis à jour est prise en charge au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="c55c3-115">Editing Razor files after they are updated is supported at build time.</span></span> <span data-ttu-id="c55c3-116">Par défaut, seul le fichier *Views.dll* compilé est déployé avec votre application (aucun fichier cshtml n’est déployé).</span><span class="sxs-lookup"><span data-stu-id="c55c3-116">By default, only the compiled *Views.dll* and no cshtml files are deployed with your application.</span></span> 
    
> [!IMPORTANT]
> <span data-ttu-id="c55c3-117">Le SDK Razor est actif uniquement si aucune propriété de précompilation spécifique n’est définie dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="c55c3-117">The Razor Sdk is only effective when no precompilation specific properties are set in your project file.</span></span> <span data-ttu-id="c55c3-118">Par exemple, le fait de définir `MvcRazorCompileOnPublish` dans votre fichier *.csproj* désactive le SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="c55c3-118">For instance, setting `MvcRazorCompileOnPublish` in your *.csproj* file will disable the Razor Sdk.</span></span>

# <a name="aspnet-core-20tabaspnetcore20"></a>[<span data-ttu-id="c55c3-119">ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="c55c3-119">ASP.NET Core 2.0</span></span>](#tab/aspnetcore20/)

<span data-ttu-id="c55c3-120">Si votre projet cible .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :</span><span class="sxs-lookup"><span data-stu-id="c55c3-120">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="c55c3-121">Si votre projet cible .NET Core, aucune modification n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="c55c3-121">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="c55c3-122">Les modèles de projet ASP.NET Core 2.x définissent `MvcRazorCompileOnPublish` avec la valeur `true` par défaut, ce qui signifie que ce nœud peut être supprimé en toute sécurité du fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c55c3-122">The ASP.NET Core 2.x project templates implicitly sets `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span>
    
> [!IMPORTANT]
> <span data-ttu-id="c55c3-123">La précompilation de vue Razor n’est pas disponible quand vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c55c3-123">Razor view precompilation in not available when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> 

<span data-ttu-id="c55c3-124">Préparer l’application pour un [déploiement dépendant de l’infrastructure](/dotnet/core/deploying/#framework-dependent-deployments-fdd) avec la [commande de publication .NET Core CLI](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="c55c3-124">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="c55c3-125">Par exemple, exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="c55c3-125">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="c55c3-126">A *< nom_projet >. PrecompiledViews.dll* fichier, qui contient les vues Razor compilés, est généré lorsque la précompilation réussit.</span><span class="sxs-lookup"><span data-stu-id="c55c3-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="c55c3-127">Par exemple, la capture d’écran ci-dessous illustre le contenu de *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="c55c3-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c55c3-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c55c3-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="c55c3-130">Définissez `MvcRazorCompileOnPublish` avec la valeur `true` et incluez une référence de package à `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="c55c3-130">Set `MvcRazorCompileOnPublish` to `true` and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="c55c3-131">Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :</span><span class="sxs-lookup"><span data-stu-id="c55c3-131">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

