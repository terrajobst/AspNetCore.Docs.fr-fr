---
title: "Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.x et ultérieur"
author: Rick-Anderson
description: "Le métapackage Microsoft.AspNetCore.All comprend tous les packages ASP.NET Core et Entity Framework Core, ainsi que leurs dépendances."
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 07220fdae299723088fa85e452cedff5e5685bd7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="037ba-103">Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="037ba-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="037ba-104">Cette fonctionnalité nécessite ASP.NET Core 2.x ciblant .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="037ba-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="037ba-105">Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :</span><span class="sxs-lookup"><span data-stu-id="037ba-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="037ba-106">Tous les packages pris en charge par l’équipe ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="037ba-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="037ba-107">Tous les packages pris en charge par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="037ba-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="037ba-108">Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="037ba-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="037ba-109">Toutes les fonctionnalités d’ASP.NET Core 2.x et d’Entity Framework Core 2.x sont incluses dans le package `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="037ba-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="037ba-110">Les modèles de projet par défaut utilisent ce package.</span><span class="sxs-lookup"><span data-stu-id="037ba-110">The default project templates use this package.</span></span>

<span data-ttu-id="037ba-111">Le numéro de version du métapackage `Microsoft.AspNetCore.All` représente la version d’ASP.NET Core et la version d’Entity Framework Core (alignées sur la version de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="037ba-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="037ba-112">Les applications qui utilisent le métapackage `Microsoft.AspNetCore.All` tirent automatiquement parti du [magasin de packages de runtime de .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="037ba-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="037ba-113">Ce magasin Runtime contient toutes les ressources runtime nécessaires à l’exécution des applications ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="037ba-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="037ba-114">Quand vous utilisez le métapackage `Microsoft.AspNetCore.All`, **aucune** des ressources des packages NuGet ASP.NET Core référencés n’est déployée avec l’application. En effet, le magasin Runtime de .NET Core contient ces ressources.</span><span class="sxs-lookup"><span data-stu-id="037ba-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="037ba-115">Les ressources présentes dans le magasin Runtime sont précompilées afin d’améliorer la vitesse de démarrage des applications.</span><span class="sxs-lookup"><span data-stu-id="037ba-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="037ba-116">Vous pouvez utiliser le processus de suppression de package pour supprimer les packages que vous n’utilisez pas.</span><span class="sxs-lookup"><span data-stu-id="037ba-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="037ba-117">Les packages supprimés sont exclus de la sortie d’application publiée.</span><span class="sxs-lookup"><span data-stu-id="037ba-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="037ba-118">Le fichier *.csproj* suivant fait référence au métapackage `Microsoft.AspNetCore.All` pour ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="037ba-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
