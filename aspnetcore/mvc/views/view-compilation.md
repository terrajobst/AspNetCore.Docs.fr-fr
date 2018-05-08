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
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="a5fbe-103">Précompilation et compilation de vues Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5fbe-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="a5fbe-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5fbe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5fbe-105">Les vues Razor sont compilées à l'exécution lors de leur premier appel.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="a5fbe-106">A partir de  ASP.NET Core 1.1.0 il est possible de compiler les vues Razor et les déployer avec l’application &mdash; (précompilation). </span><span class="sxs-lookup"><span data-stu-id="a5fbe-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="a5fbe-107">Les modèles de projet ASP.NET Core 2.x activent la précompilation par défaut.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a5fbe-108">La précompilation de vue Razor est actuellement pas disponible lorsque vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET 2.0 de base.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="a5fbe-109">La fonctionnalité sera disponible les applications SCD (Self-Contained Deployment) lorsque Asp.Net Core 2.1 sera publié. </span><span class="sxs-lookup"><span data-stu-id="a5fbe-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="a5fbe-110">Pour plus d’informations, consultez [la compilation de la vue échoue lors de la compilation croisée pour Linux sur Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="a5fbe-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="a5fbe-111">Considérations relatives à la précompilation :</span><span class="sxs-lookup"><span data-stu-id="a5fbe-111">Precompilation considerations:</span></span>

* <span data-ttu-id="a5fbe-112">La précompilation des vues réduit la taille du bundle publié et le temps de démarrage.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="a5fbe-113">Vous ne pouvez pas modifier les vues précompilées.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="a5fbe-114">Elles ne font pas partie de l’application publiée.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="a5fbe-115">Pour déployer des vues précompilées :</span><span class="sxs-lookup"><span data-stu-id="a5fbe-115">To deploy precompiled views:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5fbe-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5fbe-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="a5fbe-117">Si votre projet cible .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :</span><span class="sxs-lookup"><span data-stu-id="a5fbe-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="a5fbe-118">Si votre projet cible .NET Core, aucune modification n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="a5fbe-119">Les modèles de projet ASP.NET Core 2.x définissent la valeur `MvcRazorCompileOnPublish` à `true` par défaut, ce qui signifie que ce nœud peut être supprimé en toute sécurité le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="a5fbe-120">Si vous préférez être explicite, il n’existe aucun risque à définir le paramètre de la propriété `MvcRazorCompileOnPublish` à `true`.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="a5fbe-121">Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ce paramètre :</span><span class="sxs-lookup"><span data-stu-id="a5fbe-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5fbe-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5fbe-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="a5fbe-123">Définissez `MvcRazorCompileOnPublish` à `true` et inclure une référence de package à `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="a5fbe-124">Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :</span><span class="sxs-lookup"><span data-stu-id="a5fbe-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

<span data-ttu-id="a5fbe-125">Préparer l’application pour un [déploiement dépendant de l’infrastructure](/dotnet/core/deploying/#framework-dependent-deployments-fdd) avec la [commande de publication .NET Core CLI](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="a5fbe-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="a5fbe-126">Par exemple, exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="a5fbe-126">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="a5fbe-127">A *< nom_projet >. PrecompiledViews.dll* fichier, qui contient les vues Razor compilés, est généré lorsque la précompilation réussit.</span><span class="sxs-lookup"><span data-stu-id="a5fbe-127">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="a5fbe-128">Par exemple, la capture d’écran ci-dessous illustre le contenu de *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="a5fbe-128">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)
