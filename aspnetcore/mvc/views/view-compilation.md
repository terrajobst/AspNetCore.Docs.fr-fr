---
title: Précompilation et compilation de vues Razor
author: rick-anderson
description: Document de référence expliquant comment activer la précompilation et la compilation de vues Razor MVC dans les applications ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="e2e6c-103">Précompilation et compilation de vues Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2e6c-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="e2e6c-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e2e6c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e2e6c-105">Les vues Razor sont compilées à l'exécution lors de leur premier appel.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="e2e6c-106">A partir de  ASP.NET Core 1.1.0 il est possible de compiler les vues Razor et les déployer avec l’application &mdash; (précompilation). </span><span class="sxs-lookup"><span data-stu-id="e2e6c-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="e2e6c-107">Les modèles de projet ASP.NET Core 2.x activent la précompilation par défaut.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2e6c-108">La précompilation de vue Razor est actuellement pas disponible lorsque vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET 2.0 de base.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="e2e6c-109">La fonctionnalité sera disponible les applications SCD (Self-Contained Deployment) lorsque Asp.Net Core 2.1 sera publié. </span><span class="sxs-lookup"><span data-stu-id="e2e6c-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="e2e6c-110">Pour plus d’informations, consultez [la compilation de la vue échoue lors de la compilation croisée pour Linux sur Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="e2e6c-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="e2e6c-111">Considérations relatives à la précompilation :</span><span class="sxs-lookup"><span data-stu-id="e2e6c-111">Precompilation considerations:</span></span>

* <span data-ttu-id="e2e6c-112">La précompilation des vues réduit la taille du bundle publié et le temps de démarrage.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="e2e6c-113">Vous ne pouvez pas modifier les vues précompilées.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="e2e6c-114">Elles ne font pas partie de l’application publiée.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="e2e6c-115">Pour déployer des vues précompilées :</span><span class="sxs-lookup"><span data-stu-id="e2e6c-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e2e6c-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e2e6c-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e2e6c-117">Si votre projet cible .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :</span><span class="sxs-lookup"><span data-stu-id="e2e6c-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="e2e6c-118">Si votre projet cible .NET Core, aucune modification n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="e2e6c-119">Les modèles de projet ASP.NET Core 2.x définissent la valeur `MvcRazorCompileOnPublish` à `true` par défaut, ce qui signifie que ce nœud peut être supprimé en toute sécurité le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="e2e6c-120">Si vous préférez être explicite, il n’existe aucun risque à définir le paramètre de la propriété `MvcRazorCompileOnPublish` à `true`.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="e2e6c-121">Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ce paramètre :</span><span class="sxs-lookup"><span data-stu-id="e2e6c-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e2e6c-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e2e6c-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e2e6c-123">Définissez `MvcRazorCompileOnPublish` à `true` et inclure une référence de package à `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="e2e6c-124">Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :</span><span class="sxs-lookup"><span data-stu-id="e2e6c-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="e2e6c-125">Préparez l’application pour un [déploiement dépendant du framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd) en exécutant une commande similaire à la suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="e2e6c-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="e2e6c-126">A *< nom_projet >. PrecompiledViews.dll* fichier, qui contient les vues Razor compilés, est généré lorsque la précompilation réussit.</span><span class="sxs-lookup"><span data-stu-id="e2e6c-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="e2e6c-127">Par exemple, la capture d’écran ci-dessous illustre le contenu de *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="e2e6c-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)
