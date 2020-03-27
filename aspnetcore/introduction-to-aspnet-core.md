---
title: Présentation d’ASP.NET Core
author: rick-anderson
description: Découvrez une introduction à ASP.NET Core, framework multiplateforme à hautes performances et open source qui permet de créer des applications cloud modernes et connectées à Internet.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: index
ms.openlocfilehash: fd7fa9dd70502f51222e457dd887ef668d377278
ms.sourcegitcommit: 4b166b49ec557a03f99f872dd069ca5e56faa524
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366658"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="2b6c3-103">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b6c3-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="2b6c3-104">Par [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="2b6c3-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="2b6c3-105">ASP.NET Core est un framework multiplateforme à hautes performances et [open source](https://github.com/aspnet/home) pour créer des applications cloud modernes et connectées à Internet.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="2b6c3-106">Avec ASP.NET Core, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="2b6c3-107">Créer des applications et des services web, des applications [IoT](https://www.microsoft.com/internet-of-things/) et des back-ends mobiles.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="2b6c3-108">Utilisez vos outils de développement préférés sur Windows, macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="2b6c3-109">Déployer sur le cloud ou localement.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="2b6c3-110">Exécuter sur [.NET Core ou .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="2b6c3-111">Pourquoi utiliser ASP.NET Core ?</span><span class="sxs-lookup"><span data-stu-id="2b6c3-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="2b6c3-112">Des millions de développeurs utilisent ou ont utilisé [ASP.net 4. x](/aspnet/overview) pour créer des applications Web.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-112">Millions of developers use or have used [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="2b6c3-113">ASP.NET Core est une reconception d’ASP.NET 4.x, avec des modifications d’architecture qui aboutissent à un framework plus léger et modulaire.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="2b6c3-114">Créer des API web et une interface utilisateur web en utilisant le modèle MVC d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b6c3-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="2b6c3-115">Le modèle MVC d’ASP.NET Core fournit des fonctionnalités pour créer des [API web](xref:tutorials/first-web-api) et des [applications web](xref:tutorials/razor-pages/index) :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="2b6c3-116">Le [modèle MVC (Modèle-Vue-Contrôleur)](xref:mvc/overview) permet de rendre vos API web et vos applications web testables.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="2b6c3-117">[Razor Pages](xref:razor-pages/index) est un modèle de programmation basé sur les pages qui rend la création d’une interface utilisateur web plus facile et plus productive.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="2b6c3-118">Le [balisage Razor](xref:mvc/views/razor) fournit une syntaxe efficace pour [Razor Pages](xref:razor-pages/index) et les [vues MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="2b6c3-119">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="2b6c3-120">La prise en charge intégrée de [plusieurs formats de données et de la négociation de contenu](xref:web-api/advanced/formatting) permet à vos API web d’être utilisées par un large éventail de clients, notamment des navigateurs et des appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="2b6c3-121">La [liaison de modèle](xref:mvc/models/model-binding) mappe automatiquement les données des requêtes HTTP aux paramètres des méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="2b6c3-122">La [validation de modèle](xref:mvc/models/validation) effectue automatiquement la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="2b6c3-123">Développement côté client</span><span class="sxs-lookup"><span data-stu-id="2b6c3-123">Client-side development</span></span>

<span data-ttu-id="2b6c3-124">ASP.NET Core s’intègre parfaitement avec les frameworks et les bibliothèques populaires côté client, notamment [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react) et [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="2b6c3-125">Pour plus d’informations, consultez <xref:blazor/index> et les rubriques connexes sous *Développement côté client*.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="2b6c3-126">ASP.NET Core ciblant .NET Framework</span><span class="sxs-lookup"><span data-stu-id="2b6c3-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="2b6c3-127">ASP.NET Core 2.x peut cibler .NET Core ou le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="2b6c3-128">Les applications ASP.NET Core ciblant .NET Framework ne sont pas multiplateformes : elles s’exécutent seulement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="2b6c3-129">D’une façon générale, ASP.NET Core 2.x est constitué de bibliothèques [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="2b6c3-130">Les bibliothèques écrites avec .NET Standard 2.0 s’exécutent sur [n’importe quelle plateforme .NET qui implémente .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="2b6c3-131">ASP.NET Core 2.x est pris en charge sur les versions .NET Framework compatibles avec .NET Standard 2.0 :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="2b6c3-132">La dernière version de .NET framework est fortement recommandée.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="2b6c3-133">.NET Framework 4.6.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="2b6c3-134">ASP.NET Core 3.0 et ultérieur s’exécute uniquement sur .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="2b6c3-135">Pour plus de détails concernant ce changement, consultez [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="2b6c3-136">Le ciblage de .NET Core présente plusieurs avantages, qui sont plus nombreux à chaque version.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="2b6c3-137">Voici certains avantages de .NET Core par rapport à .NET Framework :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="2b6c3-138">Multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-138">Cross-platform.</span></span> <span data-ttu-id="2b6c3-139">S’exécute sur macOS, Linux et Windows</span><span class="sxs-lookup"><span data-stu-id="2b6c3-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="2b6c3-140">performances améliorées</span><span class="sxs-lookup"><span data-stu-id="2b6c3-140">Improved performance</span></span>
* [<span data-ttu-id="2b6c3-141">Contrôle de version côte à côte</span><span class="sxs-lookup"><span data-stu-id="2b6c3-141">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#a-need-for-side-by-side-of-net-versions-per-application-level)
* <span data-ttu-id="2b6c3-142">Nouvelles API</span><span class="sxs-lookup"><span data-stu-id="2b6c3-142">New APIs</span></span>
* <span data-ttu-id="2b6c3-143">Open source</span><span class="sxs-lookup"><span data-stu-id="2b6c3-143">Open source</span></span>

<span data-ttu-id="2b6c3-144">Nous nous efforçons de combler l’écart d’API qui existe entre .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="2b6c3-145">Le [Pack de compatibilité Windows](/dotnet/core/porting/windows-compat-pack) a rendu disponible dans .NET Core des milliers d’API fonctionnant seulement dans Windows.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="2b6c3-146">Ces API n’étaient pas disponibles dans .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="2b6c3-147">Parcours d’apprentissage recommandé</span><span class="sxs-lookup"><span data-stu-id="2b6c3-147">Recommended learning path</span></span>

<span data-ttu-id="2b6c3-148">Nous vous recommandons la séquence de tutoriels et d’articles suivante comme introduction au développement des applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="2b6c3-149">Suivez un tutoriel pour le type d’application que vous souhaitez développer ou gérer :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="2b6c3-150">Type d’application</span><span class="sxs-lookup"><span data-stu-id="2b6c3-150">App type</span></span>  |<span data-ttu-id="2b6c3-151">Scénario</span><span class="sxs-lookup"><span data-stu-id="2b6c3-151">Scenario</span></span>  |<span data-ttu-id="2b6c3-152">Didacticiel</span><span class="sxs-lookup"><span data-stu-id="2b6c3-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="2b6c3-153">Application web</span><span class="sxs-lookup"><span data-stu-id="2b6c3-153">Web app</span></span>                   | <span data-ttu-id="2b6c3-154">Pour un nouveau développement</span><span class="sxs-lookup"><span data-stu-id="2b6c3-154">For new development</span></span>        |[<span data-ttu-id="2b6c3-155">Bien démarrer avec les pages Razor</span><span class="sxs-lookup"><span data-stu-id="2b6c3-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="2b6c3-156">Application web</span><span class="sxs-lookup"><span data-stu-id="2b6c3-156">Web app</span></span>                   | <span data-ttu-id="2b6c3-157">Pour maintenir une application MVC</span><span class="sxs-lookup"><span data-stu-id="2b6c3-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="2b6c3-158">Bien démarrer avec MVC</span><span class="sxs-lookup"><span data-stu-id="2b6c3-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="2b6c3-159">API Web</span><span class="sxs-lookup"><span data-stu-id="2b6c3-159">Web API</span></span>                   |                            |<span data-ttu-id="2b6c3-160">[Créer une API web](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="2b6c3-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="2b6c3-161">Application en temps réel</span><span class="sxs-lookup"><span data-stu-id="2b6c3-161">Real-time app</span></span>             |                            |[<span data-ttu-id="2b6c3-162">Bien démarrer avec SignalR</span><span class="sxs-lookup"><span data-stu-id="2b6c3-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |
   |<span data-ttu-id="2b6c3-163">Application éblouissante</span><span class="sxs-lookup"><span data-stu-id="2b6c3-163">Blazor app</span></span>                |                            |[<span data-ttu-id="2b6c3-164">Prise en main de éblouissant</span><span class="sxs-lookup"><span data-stu-id="2b6c3-164">Get started with Blazor</span></span>](xref:blazor/get-started) |
   |<span data-ttu-id="2b6c3-165">Application d’appel de procédure distante</span><span class="sxs-lookup"><span data-stu-id="2b6c3-165">Remote Procedure Call app</span></span> |                            |[<span data-ttu-id="2b6c3-166">Prise en main d’un service gRPC</span><span class="sxs-lookup"><span data-stu-id="2b6c3-166">Get started with a gRPC service</span></span>](xref:tutorials/grpc/grpc-start) |

1. <span data-ttu-id="2b6c3-167">Suivez un tutoriel qui montre comment exécuter l’accès aux données de base :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-167">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="2b6c3-168">Scénario</span><span class="sxs-lookup"><span data-stu-id="2b6c3-168">Scenario</span></span>  |<span data-ttu-id="2b6c3-169">Didacticiel</span><span class="sxs-lookup"><span data-stu-id="2b6c3-169">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="2b6c3-170">Pour un nouveau développement</span><span class="sxs-lookup"><span data-stu-id="2b6c3-170">For new development</span></span>        |[<span data-ttu-id="2b6c3-171">Pages Razor avec Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2b6c3-171">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="2b6c3-172">Pour maintenir une application MVC</span><span class="sxs-lookup"><span data-stu-id="2b6c3-172">For maintaining an MVC app</span></span> |[<span data-ttu-id="2b6c3-173">MVC avec Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2b6c3-173">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="2b6c3-174">Lisez une présentation des fonctionnalités d’ASP.NET Core qui s’appliquent à tous les types d’application :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-174">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="2b6c3-175">Fondamentaux</span><span class="sxs-lookup"><span data-stu-id="2b6c3-175">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="2b6c3-176">Parcourez la Table des matières pour d’autres rubriques qui vous intéressent.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-176">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="2b6c3-177">\* Il existe un nouveau [tutoriel sur l’API web que vous pouvez suivre entièrement dans le navigateur](https://docs.microsoft.com/learn/modules/build-web-api-net-core) (aucune installation d’IDE locale requise).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-177">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="2b6c3-178">Le code s’exécute dans [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), et [curl](https://curl.haxx.se/) est utilisé à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-178">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migration-from-the-net-framework"></a><span data-ttu-id="2b6c3-179">Migration à partir de la .NET Framework</span><span class="sxs-lookup"><span data-stu-id="2b6c3-179">Migration from the .NET Framework</span></span>

<span data-ttu-id="2b6c3-180">Pour obtenir un guide de référence sur la migration d’applications ASP.NET vers ASP.NET Core, consultez <xref:migration/proper-to-2x/index>.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-180">For a reference guide to migrating ASP.NET apps to ASP.NET Core, see <xref:migration/proper-to-2x/index>.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="2b6c3-181">Comment télécharger un exemple</span><span class="sxs-lookup"><span data-stu-id="2b6c3-181">How to download a sample</span></span>

<span data-ttu-id="2b6c3-182">La plupart des articles et tutoriels contiennent des liens vers des exemples de code.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-182">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="2b6c3-183">[Téléchargez le fichier zip du référentiel ASP.NET](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-183">[Download the ASP.NET repository zip file](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="2b6c3-184">Décompressez le fichier *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-184">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="2b6c3-185">Utilisez l’URL contenue dans l’exemple de lien pour vous aider à naviguer dans l’exemple de répertoire.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-185">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="2b6c3-186">Directives de préprocesseur dans l’exemple de code</span><span class="sxs-lookup"><span data-stu-id="2b6c3-186">Preprocessor directives in sample code</span></span>

<span data-ttu-id="2b6c3-187">Pour illustrer plusieurs scénarios, les exemples d’applications utilisent les directives de préprocesseur `#define` et `#if-#else/#elif-#endif` pour compiler et exécuter de manière sélective différentes sections d’exemples de code.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-187">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` preprocessor directives to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="2b6c3-188">Pour les exemples qui utilisent cette approche, définissez la directive `#define` en haut des C# fichiers pour définir le symbole associé au scénario que vous souhaitez exécuter.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-188">For those samples that make use of this approach, set the `#define` directive at the top of the C# files to define the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="2b6c3-189">Certains exemples requièrent la définition du symbole en haut de plusieurs fichiers afin d’exécuter un scénario.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-189">Some samples require defining the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="2b6c3-190">Par exemple, la liste des symboles `#define` suivante indique que les quatre scénarios sont disponibles (un scénario par symbole).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-190">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="2b6c3-191">La configuration actuelle de l’exemple exécute le scénario `TemplateCode` :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-191">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="2b6c3-192">Pour que l’exemple exécute le scénario `ExpandDefault`, définissez le symbole `ExpandDefault` et laissez les symboles restants commentés :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-192">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="2b6c3-193">Pour plus d’informations sur l’utilisation des [directives de préprocesseur C#](/dotnet/csharp/language-reference/preprocessor-directives/) pour compiler de façon sélective des sections de code, consultez [#define (Référence C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) et [#if (Référence C#) ](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-193">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="2b6c3-194">Régions dans l’exemple de code</span><span class="sxs-lookup"><span data-stu-id="2b6c3-194">Regions in sample code</span></span>

<span data-ttu-id="2b6c3-195">Certains exemples d’applications contiennent des sections de code entourées de [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) et de directives de [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# .</span><span class="sxs-lookup"><span data-stu-id="2b6c3-195">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# directives.</span></span> <span data-ttu-id="2b6c3-196">Le système de génération de documentation injecte ces régions dans les rubriques de documentation affichées.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-196">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="2b6c3-197">Les noms des régions contiennent généralement le mot « snippet ».</span><span class="sxs-lookup"><span data-stu-id="2b6c3-197">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="2b6c3-198">L’exemple suivant montre une région nommée `snippet_WebHostDefaults` :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-198">The following example shows a region named `snippet_WebHostDefaults`:</span></span>

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

<span data-ttu-id="2b6c3-199">L’extrait de code C# précédent est référencé dans le fichier Markdown de la rubrique avec la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-199">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

<span data-ttu-id="2b6c3-200">Vous pouvez ignorer (ou supprimer) en toute sécurité les directives `#region` et `#endregion` qui entourent le code.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-200">You may safely ignore (or remove) the `#region` and `#endregion` directives that surround the code.</span></span> <span data-ttu-id="2b6c3-201">Ne modifiez pas le code dans ces directives si vous envisagez d’exécuter les exemples de scénarios décrits dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-201">Don't alter the code within these directives if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="2b6c3-202">N’hésitez pas à modifier le code quand vous testez d’autres scénarios.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-202">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="2b6c3-203">Pour plus d’informations, consultez [Contribuer à la documentation ASP.NET : extraits de code](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="2b6c3-203">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b6c3-204">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="2b6c3-204">Next steps</span></span>

<span data-ttu-id="2b6c3-205">Pour plus d’informations, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="2b6c3-205">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="2b6c3-206">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b6c3-206">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="2b6c3-207">[Le point hebdomadaire de la communauté ASP.NET](https://live.asp.net/) couvre l’avancement et les plans de l’équipe.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-207">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="2b6c3-208">Il comprend de nouveaux blogs et des logiciels de tiers.</span><span class="sxs-lookup"><span data-stu-id="2b6c3-208">It features new blogs and third-party software.</span></span>
