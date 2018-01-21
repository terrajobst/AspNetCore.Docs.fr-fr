---
title: "Metapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.x et versions ultérieures"
author: Rick-Anderson
description: "Le metapackage Microsoft.AspNetCore.All inclut tous les packages ASP.NET Core et Entity Framework Core, ainsi que leurs dépendances."
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="5dc25-103">Metapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5dc25-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="5dc25-104">Cette fonctionnalité nécessite .NET de ciblage ASP.NET Core 2.x 2.x de base.</span><span class="sxs-lookup"><span data-stu-id="5dc25-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="5dc25-105">Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :</span><span class="sxs-lookup"><span data-stu-id="5dc25-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="5dc25-106">Tous les packages pris en charge par l’équipe ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5dc25-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="5dc25-107">Tous les packages pris en charge par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5dc25-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="5dc25-108">Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5dc25-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="5dc25-109">Toutes les fonctionnalités ASP.NET Core 2.x et Entity Framework Core 2.x sont inclus dans le `Microsoft.AspNetCore.All` package.</span><span class="sxs-lookup"><span data-stu-id="5dc25-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="5dc25-110">Les modèles de projet par défaut utilisent ce package.</span><span class="sxs-lookup"><span data-stu-id="5dc25-110">The default project templates use this package.</span></span>

<span data-ttu-id="5dc25-111">Le numéro de version de la `Microsoft.AspNetCore.All` metapackage représente la version de ASP.NET Core et Entity Framework Core (aligné avec la version de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="5dc25-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="5dc25-112">Les applications qui utilisent la `Microsoft.AspNetCore.All` metapackage tirer parti automatiquement de la [magasin du Runtime .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="5dc25-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="5dc25-113">Le magasin de Runtime contient tous les composants runtime nécessaires à l’exécution des applications 2.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5dc25-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="5dc25-114">Lorsque vous utilisez la `Microsoft.AspNetCore.All` metapackage, **aucun** des ressources depuis les packages ASP.NET Core NuGet référencés sont déployés avec l’application &mdash; le magasin du Runtime .NET Core contient ces ressources.</span><span class="sxs-lookup"><span data-stu-id="5dc25-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="5dc25-115">Les éléments multimédias dans le magasin d’exécution sont précompilés pour améliorer les temps de démarrage d’application.</span><span class="sxs-lookup"><span data-stu-id="5dc25-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="5dc25-116">Vous pouvez utiliser le processus de suppression du package à supprimer les packages que vous n’utilisez pas.</span><span class="sxs-lookup"><span data-stu-id="5dc25-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="5dc25-117">Packages découpés sont exclus de la sortie de l’application publiée.</span><span class="sxs-lookup"><span data-stu-id="5dc25-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="5dc25-118">Les éléments suivants *.csproj* références de fichiers le `Microsoft.AspNetCore.All` metapackage pour ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="5dc25-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
