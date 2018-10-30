---
title: Présentation d’ASP.NET Core
author: rick-anderson
description: Découvrez une introduction à ASP.NET Core, framework multiplateforme à hautes performances et open source qui permet de créer des applications cloud modernes et connectées à Internet.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: index
ms.openlocfilehash: 37448b1b3d0da4e3cb34b1cd51f663b7e53ddced
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207392"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="d94fe-103">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d94fe-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="d94fe-104">Par [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="d94fe-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="d94fe-105">ASP.NET Core est un framework multiplateforme à hautes performances et [open source](https://github.com/aspnet/home) pour créer des applications cloud modernes et connectées à Internet.</span><span class="sxs-lookup"><span data-stu-id="d94fe-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="d94fe-106">Avec ASP.NET Core, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="d94fe-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="d94fe-107">Créer des applications et des services web, des applications [IoT](https://www.microsoft.com/internet-of-things/) et des back-ends mobiles.</span><span class="sxs-lookup"><span data-stu-id="d94fe-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="d94fe-108">Utiliser vos outils de développement préférés sur Windows, macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="d94fe-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="d94fe-109">Déployer dans le cloud ou localement.</span><span class="sxs-lookup"><span data-stu-id="d94fe-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="d94fe-110">Exécuter sur [.NET Core ou .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="d94fe-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="d94fe-111">Pourquoi utiliser ASP.NET Core ?</span><span class="sxs-lookup"><span data-stu-id="d94fe-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="d94fe-112">Des millions de développeurs ont utilisé (et continuent d’utiliser) [ASP.NET 4.x](/aspnet/overview) pour créer des applications web.</span><span class="sxs-lookup"><span data-stu-id="d94fe-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="d94fe-113">ASP.NET Core est une refonte d’ASP.NET 4.x, avec des modifications d’architecture qui aboutissent à un framework plus léger et modulaire.</span><span class="sxs-lookup"><span data-stu-id="d94fe-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="d94fe-114">Créer des API web et une interface utilisateur web en utilisant le modèle MVC d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d94fe-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="d94fe-115">Le modèle MVC d’ASP.NET Core fournit des fonctionnalités pour créer des [API web](xref:tutorials/index#build-web-apis) et des [applications web](xref:tutorials/index#build-web-apps) :</span><span class="sxs-lookup"><span data-stu-id="d94fe-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="d94fe-116">Le [modèle MVC (Modèle-Vue-Contrôleur)](xref:mvc/overview) permet de rendre vos API web et vos applications web [testables](xref:test/index).</span><span class="sxs-lookup"><span data-stu-id="d94fe-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](xref:test/index).</span></span>
* <span data-ttu-id="d94fe-117">[Razor Pages](xref:razor-pages/index) (nouveauté dans ASP.NET Core 2.0) est un modèle de programmation basé sur les pages qui rend plus facile et plus productive la création d’une interface utilisateur web.</span><span class="sxs-lookup"><span data-stu-id="d94fe-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="d94fe-118">Le [balisage Razor](xref:mvc/views/razor) fournit une syntaxe efficace pour [Razor Pages](xref:razor-pages/index) et les [vues MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="d94fe-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="d94fe-119">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="d94fe-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="d94fe-120">La prise en charge intégrée de [plusieurs formats de données et de la négociation de contenu](xref:web-api/advanced/formatting) permet à vos API web d’être utilisées par un large éventail de clients, notamment des navigateurs et des appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="d94fe-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="d94fe-121">Le principe de la [liaison de modèle](xref:mvc/models/model-binding) permet le mappage automatiquement des données des requêtes HTTP aux paramètres des méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="d94fe-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="d94fe-122">La [validation de modèle](xref:mvc/models/validation) effectue automatiquement la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="d94fe-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="d94fe-123">Développement côté client</span><span class="sxs-lookup"><span data-stu-id="d94fe-123">Client-side development</span></span>

<span data-ttu-id="d94fe-124">ASP.NET Core s’intègre parfaitement avec les frameworks et les bibliothèques populaires côté client, notamment [Angular](xref:spa/angular), [React](xref:spa/react) et [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="d94fe-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="d94fe-125">Pour plus d’informations, consultez [Développement côté client](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="d94fe-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="d94fe-126">ASP.NET Core ciblant .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d94fe-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="d94fe-127">ASP.NET Core peut cibler .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d94fe-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="d94fe-128">Les applications ASP.NET Core ciblant .NET Framework ne sont pas multiplateformes : elles s’exécutent seulement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="d94fe-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="d94fe-129">Il n’est pas prévu de supprimer la prise en charge du ciblage de .NET Framework dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d94fe-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="d94fe-130">D’une façon générale, ASP.NET Core est constitué de bibliothèques [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="d94fe-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="d94fe-131">Les applications écrites avec .NET Standard 2.0 s’exécutent partout où .NET Standard 2.0 est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="d94fe-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="d94fe-132">ASP.NET Core 2.x est pris en charge sur les versions de .NET Framework compatibles avec .NET Standard 2.0 :</span><span class="sxs-lookup"><span data-stu-id="d94fe-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="d94fe-133">.NET Framework 4.7.1 ou version ultérieure est vivement recommandé.</span><span class="sxs-lookup"><span data-stu-id="d94fe-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="d94fe-134">.NET Framework 4.6.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="d94fe-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="d94fe-135">Le ciblage de .NET Core présente plusieurs avantages, qui sont plus nombreux à chaque version.</span><span class="sxs-lookup"><span data-stu-id="d94fe-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="d94fe-136">Voici certains avantages de .NET Core par rapport à .NET Framework :</span><span class="sxs-lookup"><span data-stu-id="d94fe-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="d94fe-137">Multiplateforme</span><span class="sxs-lookup"><span data-stu-id="d94fe-137">Cross-platform.</span></span> <span data-ttu-id="d94fe-138">S’exécute sur macOS, Linux et Windows</span><span class="sxs-lookup"><span data-stu-id="d94fe-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="d94fe-139">Performances améliorées</span><span class="sxs-lookup"><span data-stu-id="d94fe-139">Improved performance</span></span>
* <span data-ttu-id="d94fe-140">Gestion des versions côte à côte</span><span class="sxs-lookup"><span data-stu-id="d94fe-140">Side-by-side versioning</span></span>
* <span data-ttu-id="d94fe-141">Nouvelles API</span><span class="sxs-lookup"><span data-stu-id="d94fe-141">New APIs</span></span>
* <span data-ttu-id="d94fe-142">Ouvrir la source</span><span class="sxs-lookup"><span data-stu-id="d94fe-142">Open source</span></span>

<span data-ttu-id="d94fe-143">Nous nous efforçons de combler l’écart d’API qui existe entre .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d94fe-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="d94fe-144">Le [Pack de compatibilité Windows](/dotnet/core/porting/windows-compat-pack) a rendu disponible dans .NET Core des milliers d’API fonctionnant seulement dans Windows.</span><span class="sxs-lookup"><span data-stu-id="d94fe-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="d94fe-145">Ces API n’étaient pas disponibles dans .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d94fe-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="d94fe-146">Comment télécharger un exemple</span><span class="sxs-lookup"><span data-stu-id="d94fe-146">How to download a sample</span></span>

<span data-ttu-id="d94fe-147">La plupart des articles et tutoriels contiennent des liens vers des exemples de code.</span><span class="sxs-lookup"><span data-stu-id="d94fe-147">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="d94fe-148">[Téléchargez le fichier zip du référentiel ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="d94fe-148">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="d94fe-149">Décompressez le fichier *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="d94fe-149">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="d94fe-150">Utilisez l’URL contenue dans l’exemple de lien pour vous aider à naviguer dans l’exemple de répertoire.</span><span class="sxs-lookup"><span data-stu-id="d94fe-150">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d94fe-151">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d94fe-151">Next steps</span></span>

<span data-ttu-id="d94fe-152">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="d94fe-152">For more information, see the following resources:</span></span>

* [<span data-ttu-id="d94fe-153">Bien démarrer avec Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d94fe-153">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="d94fe-154">Tutoriels ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d94fe-154">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="d94fe-155">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d94fe-155">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="d94fe-156">[Le point hebdomadaire de la communauté ASP.NET](https://live.asp.net/) couvre l’avancement et les plans des équipes de développement.</span><span class="sxs-lookup"><span data-stu-id="d94fe-156">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="d94fe-157">Il comprend de nouveaux blogs et des logiciels de tiers.</span><span class="sxs-lookup"><span data-stu-id="d94fe-157">It features new blogs and third-party software.</span></span>
