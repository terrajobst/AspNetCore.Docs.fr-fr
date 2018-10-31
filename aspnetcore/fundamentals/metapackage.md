---
title: Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.0
author: Rick-Anderson
description: Le métapackage Microsoft.AspNetCore.All n’est pas recommandé pour ASP.NET Core 2.1 et versions ultérieures.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2018
uid: fundamentals/metapackage
ms.openlocfilehash: b1924e07acd2b4feb25c69b8c4674002e6ba0464
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325677"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="0f711-103">Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="0f711-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="0f711-104">Nous recommandons que les applications qui ciblent ASP.NET Core 2.1 et ultérieur utilisent [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) au lieu de ce package.</span><span class="sxs-lookup"><span data-stu-id="0f711-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="0f711-105">Consultez [Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App](#migrate) dans cet article.</span><span class="sxs-lookup"><span data-stu-id="0f711-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="0f711-106">Cette fonctionnalité nécessite ASP.NET Core 2.x ciblant .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0f711-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="0f711-107">Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :</span><span class="sxs-lookup"><span data-stu-id="0f711-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="0f711-108">Tous les packages pris en charge par l’équipe ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0f711-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="0f711-109">Tous les packages pris en charge par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0f711-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="0f711-110">Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0f711-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="0f711-111">Toutes les fonctionnalités d’ASP.NET Core 2.x et d’Entity Framework Core 2.x sont incluses dans le package `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="0f711-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="0f711-112">Les modèles de projet par défaut ciblant ASP.NET Core 2.0 utilisent ce package.</span><span class="sxs-lookup"><span data-stu-id="0f711-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="0f711-113">Le numéro de version du métapackage `Microsoft.AspNetCore.All` représente la version d’ASP.NET Core et la version d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0f711-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="0f711-114">Les applications qui utilisent le métapackage `Microsoft.AspNetCore.All` tirent automatiquement parti du [magasin de packages de runtime de .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="0f711-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="0f711-115">Ce magasin Runtime contient toutes les ressources runtime nécessaires à l’exécution des applications ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0f711-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="0f711-116">Quand vous utilisez le métapackage `Microsoft.AspNetCore.All`, **aucune** des ressources des packages NuGet ASP.NET Core référencés n’est déployée avec l’application. En effet, le magasin Runtime de .NET Core contient ces ressources.</span><span class="sxs-lookup"><span data-stu-id="0f711-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="0f711-117">Les ressources présentes dans le magasin Runtime sont précompilées afin d’améliorer la vitesse de démarrage des applications.</span><span class="sxs-lookup"><span data-stu-id="0f711-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="0f711-118">Vous pouvez utiliser le processus de suppression de package pour supprimer les packages que vous n’utilisez pas.</span><span class="sxs-lookup"><span data-stu-id="0f711-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="0f711-119">Les packages supprimés sont exclus de la sortie d’application publiée.</span><span class="sxs-lookup"><span data-stu-id="0f711-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="0f711-120">Le fichier *.csproj* suivant fait référence au métapackage `Microsoft.AspNetCore.All` pour ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="0f711-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="0f711-121">Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="0f711-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="0f711-122">Les packages suivants sont inclus dans `Microsoft.AspNetCore.All` mais pas le package `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="0f711-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="0f711-123">Pour passer de `Microsoft.AspNetCore.All` à `Microsoft.AspNetCore.App`, si votre application utilise les API des packages ci-dessus, ou les packages apportés par ces packages, ajoutez des références à ces packages dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="0f711-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="0f711-124">Toutes les dépendances des packages précédents qui ne sont pas des dépendances de `Microsoft.AspNetCore.App` ne sont pas incluses de manière implicite.</span><span class="sxs-lookup"><span data-stu-id="0f711-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="0f711-125">Exemple :</span><span class="sxs-lookup"><span data-stu-id="0f711-125">For example:</span></span>

* <span data-ttu-id="0f711-126">`StackExchange.Redis` comme dépendance de `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="0f711-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="0f711-127">`Microsoft.ApplicationInsights` comme dépendance de `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="0f711-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="0f711-128">Mettre à jour ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="0f711-128">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="0f711-129">Nous vous recommandons de migrer vers le métapackage `Microsoft.AspNetCore.App` pour 2.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="0f711-129">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="0f711-130">Pour continuer à utiliser le métapackage `Microsoft.AspNetCore.All` et vérifier que la dernière version corrective est déployée :</span><span class="sxs-lookup"><span data-stu-id="0f711-130">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="0f711-131">Sur les machines de développement et les serveurs de builds, installez le [kit SDK .NET Core](https://www.microsoft.com/net/download) le plus récent.</span><span class="sxs-lookup"><span data-stu-id="0f711-131">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="0f711-132">Sur les serveurs de déploiement, installez le [Runtime .NET Core](https://www.microsoft.com/net/download) le plus récent.</span><span class="sxs-lookup"><span data-stu-id="0f711-132">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="0f711-133">Votre application est restaurée par progression jusqu’à la version installée la plus récente au moment de son redémarrage.</span><span class="sxs-lookup"><span data-stu-id="0f711-133">Your app will roll forward to the latest installed version on an application restart.</span></span>
