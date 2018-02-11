---
title: "Précompilation et compilation de vues Razor"
author: rick-anderson
description: "Document de référence expliquant comment activer la précompilation et la compilation de vues Razor MVC dans les applications ASP.NET Core."
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
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="6a652-103">Précompilation et compilation de vues Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a652-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="6a652-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6a652-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6a652-105">Les vues Razor sont compilées au moment de l’exécution quand une vue est appelée.</span><span class="sxs-lookup"><span data-stu-id="6a652-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="6a652-106">Dans ASP.NET Core 1.1.0 et ultérieur, les vues Razor peuvent éventuellement être compilées et déployées avec l’application &mdash; ce processus est appelé précompilation.</span><span class="sxs-lookup"><span data-stu-id="6a652-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="6a652-107">Les modèles de projet ASP.NET Core 2.x activent la précompilation par défaut.</span><span class="sxs-lookup"><span data-stu-id="6a652-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a652-108">La précompilation de vues Razor n’est pas disponible quand vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6a652-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="6a652-109">Cette fonctionnalité sera disponible pour les déploiements SCD dans la future version 2.1.</span><span class="sxs-lookup"><span data-stu-id="6a652-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="6a652-110">Pour plus d’informations, consultez [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="6a652-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="6a652-111">Considérations relatives à la précompilation :</span><span class="sxs-lookup"><span data-stu-id="6a652-111">Precompilation considerations:</span></span>

* <span data-ttu-id="6a652-112">La précompilation des vues réduit la taille du bundle publié et le temps de démarrage.</span><span class="sxs-lookup"><span data-stu-id="6a652-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="6a652-113">Vous ne pouvez pas modifier les fichiers Razor après avoir précompilé des vues.</span><span class="sxs-lookup"><span data-stu-id="6a652-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="6a652-114">Les vues modifiées ne seraient pas présentes dans le bundle publié.</span><span class="sxs-lookup"><span data-stu-id="6a652-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="6a652-115">Pour déployer des vues précompilées :</span><span class="sxs-lookup"><span data-stu-id="6a652-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a652-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a652-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6a652-117">Si votre projet cible .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :</span><span class="sxs-lookup"><span data-stu-id="6a652-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="6a652-118">Si votre projet cible .NET Core, aucune modification n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="6a652-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="6a652-119">Les modèles de projet ASP.NET Core 2.x définissent implicitement `MvcRazorCompileOnPublish` à la valeur `true` par défaut. Ce nœud peut donc être supprimé en toute sécurité dans le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="6a652-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="6a652-120">Si vous préférez une définition explicite, vous pouvez sans problème définir la propriété `MvcRazorCompileOnPublish` à la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="6a652-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="6a652-121">L’exemple *.csproj* suivant illustre ce paramètre :</span><span class="sxs-lookup"><span data-stu-id="6a652-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a652-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a652-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a652-123">Définissez `MvcRazorCompileOnPublish` avec la valeur `true` et ajoutez une référence de package à `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="6a652-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="6a652-124">L’exemple *.csproj* suivant illustre ces paramètres :</span><span class="sxs-lookup"><span data-stu-id="6a652-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="6a652-125">Préparez l’application pour un [déploiement dépendant du framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd) en exécutant une commande similaire à la suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="6a652-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="6a652-126">Quand la précompilation réussit, un fichier *<nom_projet>.PrecompiledViews.dll* contenant les vues Razor compilées est créé.</span><span class="sxs-lookup"><span data-stu-id="6a652-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="6a652-127">Par exemple, la capture d’écran ci-dessous illustre le contenu des vues *Index.cshtml* dans le fichier *WebApplication1.PrecompiledViews.dll* :</span><span class="sxs-lookup"><span data-stu-id="6a652-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)
