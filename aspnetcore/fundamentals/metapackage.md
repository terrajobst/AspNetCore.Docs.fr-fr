---
title: Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.0
author: Rick-Anderson
description: Le métapackage Microsoft.AspNetCore.All n’est pas recommandé pour ASP.NET Core 2.1 et versions ultérieures.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148848"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="e6547-103">Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="e6547-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="e6547-104">Nous recommandons que les applications qui ciblent ASP.NET Core 2.1 et ultérieur utilisent le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) au lieu de ce package.</span><span class="sxs-lookup"><span data-stu-id="e6547-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="e6547-105">Consultez [Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App](#migrate) dans cet article.</span><span class="sxs-lookup"><span data-stu-id="e6547-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="e6547-106">Cette fonctionnalité nécessite ASP.NET Core 2.x ciblant .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="e6547-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="e6547-107">Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :</span><span class="sxs-lookup"><span data-stu-id="e6547-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="e6547-108">Tous les packages pris en charge par l’équipe ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6547-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="e6547-109">Tous les packages pris en charge par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e6547-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="e6547-110">Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e6547-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="e6547-111">Toutes les fonctionnalités d’ASP.NET Core 2.x et d’Entity Framework Core 2.x sont incluses dans le package `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="e6547-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="e6547-112">Les modèles de projet par défaut ciblant ASP.NET Core 2.0 utilisent ce package.</span><span class="sxs-lookup"><span data-stu-id="e6547-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="e6547-113">Le numéro de version du métapackage `Microsoft.AspNetCore.All` représente la version d’ASP.NET Core et la version d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e6547-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="e6547-114">Les applications qui utilisent le métapackage `Microsoft.AspNetCore.All` tirent automatiquement parti du [magasin de packages de runtime de .NET Core](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="e6547-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="e6547-115">Ce magasin Runtime contient toutes les ressources runtime nécessaires à l’exécution des applications ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="e6547-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="e6547-116">Quand vous utilisez le métapackage `Microsoft.AspNetCore.All`, **aucune** des ressources des packages NuGet ASP.NET Core référencés n’est déployée avec l’application. En effet, le magasin Runtime de .NET Core contient ces ressources.</span><span class="sxs-lookup"><span data-stu-id="e6547-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="e6547-117">Les ressources présentes dans le magasin Runtime sont précompilées afin d’améliorer la vitesse de démarrage des applications.</span><span class="sxs-lookup"><span data-stu-id="e6547-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="e6547-118">Vous pouvez utiliser le processus de suppression de package pour supprimer les packages que vous n’utilisez pas.</span><span class="sxs-lookup"><span data-stu-id="e6547-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="e6547-119">Les packages supprimés sont exclus de la sortie d’application publiée.</span><span class="sxs-lookup"><span data-stu-id="e6547-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="e6547-120">Le fichier *.csproj* suivant fait référence au métapackage `Microsoft.AspNetCore.All` pour ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="e6547-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="e6547-121">Gestion des versions implicites</span><span class="sxs-lookup"><span data-stu-id="e6547-121">Implicit versioning</span></span>

<span data-ttu-id="e6547-122">Dans ASP.NET Core 2.1 ou ultérieur, vous pouvez spécifier la référence de package `Microsoft.AspNetCore.All` sans version.</span><span class="sxs-lookup"><span data-stu-id="e6547-122">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="e6547-123">Lorsque la version n’est pas spécifiée, une version implicite est spécifiée par le SDK (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="e6547-123">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="e6547-124">Nous vous recommandons de garder la version implicite spécifiée par le kit SDK et de ne pas définir de façon explicite le numéro de version sur la référence de package.</span><span class="sxs-lookup"><span data-stu-id="e6547-124">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="e6547-125">Si vous avez des questions au sujet de cette approche, envoyez vos commentaires GitHub dans la [Discussion concernant la version implicite de Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="e6547-125">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="e6547-126">La version implicite est définie sur `major.minor.0` pour les applications portables.</span><span class="sxs-lookup"><span data-stu-id="e6547-126">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="e6547-127">Le mécanisme de restauration par progression des frameworks partagés exécute l’application sur la dernière version compatible parmi les frameworks partagés installés.</span><span class="sxs-lookup"><span data-stu-id="e6547-127">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="e6547-128">Pour garantir l’utilisation de la même version en développement, test et production, vérifiez que la même version du framework partagé est installée dans tous les environnements.</span><span class="sxs-lookup"><span data-stu-id="e6547-128">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="e6547-129">Pour les applications autonomes, le numéro de version implicite est défini sur le `major.minor.patch` du framework partagé inclus dans le SDK installé.</span><span class="sxs-lookup"><span data-stu-id="e6547-129">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="e6547-130">La spécification d’un numéro de version sur la référence de package `Microsoft.AspNetCore.All` ne garantit **pas** que la version du framework partagé sera choisie.</span><span class="sxs-lookup"><span data-stu-id="e6547-130">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="e6547-131">Par exemple, supposons que la version « 2.1.1 » est spécifiée, mais que la version « 2.1.3 » est installée.</span><span class="sxs-lookup"><span data-stu-id="e6547-131">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="e6547-132">Dans ce cas, l’application utilise « 2.1.3 ».</span><span class="sxs-lookup"><span data-stu-id="e6547-132">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="e6547-133">Même si ce n’est pas recommandé, vous pouvez désactiver la restauration par progression (correctif et/ou mineur).</span><span class="sxs-lookup"><span data-stu-id="e6547-133">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="e6547-134">Pour plus d’informations sur la restauration par progression de l’hôte dotnet et sur la configuration de son comportement, consultez [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span><span class="sxs-lookup"><span data-stu-id="e6547-134">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="e6547-135">Le SDK du projet doit être défini sur `Microsoft.NET.Sdk.Web` dans le fichier projet pour utiliser la version implicite de `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="e6547-135">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="e6547-136">Lorsque le SDK `Microsoft.NET.Sdk` est spécifié (`<Project Sdk="Microsoft.NET.Sdk">` en haut du fichier projet), l’avertissement suivant est généré :</span><span class="sxs-lookup"><span data-stu-id="e6547-136">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="e6547-137">*Avertissement NU1604 : La dépendance du projet Microsoft.AspNetCore.All ne contient pas de limite inférieure inclusive. Incluez une limite inférieure dans la version de dépendance pour assurer des résultats de restauration cohérents.*</span><span class="sxs-lookup"><span data-stu-id="e6547-137">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="e6547-138">C’est un problème connu avec le kit SDK .NET Core 2.1 ; il sera corrigé dans le kit SDK .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="e6547-138">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="e6547-139">Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="e6547-139">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="e6547-140">Les packages suivants sont inclus dans `Microsoft.AspNetCore.All` mais pas le package `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e6547-140">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

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

<span data-ttu-id="e6547-141">Pour passer de `Microsoft.AspNetCore.All` à `Microsoft.AspNetCore.App`, si votre application utilise les API des packages ci-dessus, ou les packages apportés par ces packages, ajoutez des références à ces packages dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="e6547-141">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="e6547-142">Toutes les dépendances des packages précédents qui ne sont pas des dépendances de `Microsoft.AspNetCore.App` ne sont pas incluses de manière implicite.</span><span class="sxs-lookup"><span data-stu-id="e6547-142">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="e6547-143">Exemple :</span><span class="sxs-lookup"><span data-stu-id="e6547-143">For example:</span></span>

* <span data-ttu-id="e6547-144">`StackExchange.Redis` comme dépendance de `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="e6547-144">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="e6547-145">`Microsoft.ApplicationInsights` comme dépendance de `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="e6547-145">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="e6547-146">Mettre à jour ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="e6547-146">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="e6547-147">Nous vous recommandons de migrer vers le métapackage `Microsoft.AspNetCore.App` pour 2.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="e6547-147">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="e6547-148">Pour continuer à utiliser le métapackage `Microsoft.AspNetCore.All` et vérifier que la dernière version corrective est déployée :</span><span class="sxs-lookup"><span data-stu-id="e6547-148">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="e6547-149">Sur les machines de développement et les serveurs de builds, installez le [kit SDK .NET Core](https://www.microsoft.com/net/download) le plus récent.</span><span class="sxs-lookup"><span data-stu-id="e6547-149">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="e6547-150">Sur les serveurs de déploiement, installez le [Runtime .NET Core](https://www.microsoft.com/net/download) le plus récent.</span><span class="sxs-lookup"><span data-stu-id="e6547-150">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="e6547-151">Votre application est restaurée par progression jusqu’à la version installée la plus récente au moment de son redémarrage.</span><span class="sxs-lookup"><span data-stu-id="e6547-151">Your app will roll forward to the latest installed version on an application restart.</span></span>
