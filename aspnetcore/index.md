---
title: Présentation d’ASP.NET Core
author: rick-anderson
description: Découvrez une introduction à ASP.NET Core, framework multiplateforme à hautes performances et open source qui permet de créer des applications cloud modernes et connectées à Internet.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: index
ms.openlocfilehash: ec2b5b72eeff21d183397284b0d538681282d45b
ms.sourcegitcommit: 47cc13ab90913af9a2887cef0896bb4e9aba4dd5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67399160"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="079d2-103">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="079d2-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="079d2-104">Par [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="079d2-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="079d2-105">ASP.NET Core est un framework multiplateforme à hautes performances et [open source](https://github.com/aspnet/home) pour créer des applications cloud modernes et connectées à Internet.</span><span class="sxs-lookup"><span data-stu-id="079d2-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="079d2-106">Avec ASP.NET Core, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="079d2-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="079d2-107">Créer des applications et des services web, des applications [IoT](https://www.microsoft.com/internet-of-things/) et des back-ends mobiles.</span><span class="sxs-lookup"><span data-stu-id="079d2-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="079d2-108">Utiliser vos outils de développement préférés sur Windows, macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="079d2-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="079d2-109">Déployer dans le cloud ou localement.</span><span class="sxs-lookup"><span data-stu-id="079d2-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="079d2-110">Exécuter sur [.NET Core ou .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="079d2-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="079d2-111">Pourquoi utiliser ASP.NET Core ?</span><span class="sxs-lookup"><span data-stu-id="079d2-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="079d2-112">Des millions de développeurs ont utilisé (et continuent d’utiliser) [ASP.NET 4.x](/aspnet/overview) pour créer des applications web.</span><span class="sxs-lookup"><span data-stu-id="079d2-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="079d2-113">ASP.NET Core est une refonte d’ASP.NET 4.x, avec des modifications d’architecture qui aboutissent à un framework plus léger et modulaire.</span><span class="sxs-lookup"><span data-stu-id="079d2-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="079d2-114">Créer des API web et une interface utilisateur web en utilisant le modèle MVC d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="079d2-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="079d2-115">Le modèle MVC d’ASP.NET Core fournit des fonctionnalités pour créer des [API web](xref:tutorials/first-web-api) et des [applications web](xref:tutorials/razor-pages/index) :</span><span class="sxs-lookup"><span data-stu-id="079d2-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="079d2-116">Le [modèle MVC (Modèle-Vue-Contrôleur)](xref:mvc/overview) permet de rendre vos API web et vos applications web testables.</span><span class="sxs-lookup"><span data-stu-id="079d2-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="079d2-117">[Razor Pages](xref:razor-pages/index) est un modèle de programmation basé sur les pages qui rend la création d’une interface utilisateur web plus facile et plus productive.</span><span class="sxs-lookup"><span data-stu-id="079d2-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="079d2-118">Le [balisage Razor](xref:mvc/views/razor) fournit une syntaxe efficace pour les [pages Razor](xref:razor-pages/index) et les [vues MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="079d2-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="079d2-119">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="079d2-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="079d2-120">La prise en charge intégrée de [plusieurs formats de données et de la négociation de contenu](xref:web-api/advanced/formatting) permet à vos API web d’être utilisées par un large éventail de clients, notamment des navigateurs et des appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="079d2-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="079d2-121">Le principe de la [liaison de modèle](xref:mvc/models/model-binding) permet le mappage automatiquement des données des requêtes HTTP aux paramètres des méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="079d2-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="079d2-122">La [validation de modèle](xref:mvc/models/validation) effectue automatiquement la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="079d2-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="079d2-123">Développement côté client</span><span class="sxs-lookup"><span data-stu-id="079d2-123">Client-side development</span></span>

<span data-ttu-id="079d2-124">ASP.NET Core s’intègre parfaitement avec les frameworks et les bibliothèques populaires côté client, notamment [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react) et [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="079d2-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="079d2-125">Pour plus d’informations, consultez <xref:blazor/index> et les rubriques connexes sous *Développement côté client*.</span><span class="sxs-lookup"><span data-stu-id="079d2-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="079d2-126">ASP.NET Core ciblant .NET Framework</span><span class="sxs-lookup"><span data-stu-id="079d2-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="079d2-127">ASP.NET Core 2.x peut cibler .NET Core ou le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="079d2-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="079d2-128">Les applications ASP.NET Core ciblant .NET Framework ne sont pas multiplateformes : elles s’exécutent seulement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="079d2-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="079d2-129">D’une façon générale, ASP.NET Core 2.x est constitué de bibliothèques [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="079d2-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="079d2-130">Les bibliothèques écrites avec .NET Standard 2.0 s’exécutent sur [n’importe quelle plateforme .NET qui implémente .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span><span class="sxs-lookup"><span data-stu-id="079d2-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="079d2-131">ASP.NET Core 2.x est pris en charge sur les versions .NET Framework compatibles avec .NET Standard 2.0 :</span><span class="sxs-lookup"><span data-stu-id="079d2-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="079d2-132">La dernière version de .NET framework est fortement recommandée.</span><span class="sxs-lookup"><span data-stu-id="079d2-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="079d2-133">.NET Framework 4.6.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="079d2-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="079d2-134">ASP.NET Core 3.0 et ultérieur s’exécute uniquement sur .NET Core.</span><span class="sxs-lookup"><span data-stu-id="079d2-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="079d2-135">Pour plus de détails concernant ce changement, consultez [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="079d2-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="079d2-136">Le ciblage de .NET Core présente plusieurs avantages, qui sont plus nombreux à chaque version.</span><span class="sxs-lookup"><span data-stu-id="079d2-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="079d2-137">Voici certains avantages de .NET Core par rapport à .NET Framework :</span><span class="sxs-lookup"><span data-stu-id="079d2-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="079d2-138">Multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="079d2-138">Cross-platform.</span></span> <span data-ttu-id="079d2-139">S’exécute sur macOS, Linux et Windows</span><span class="sxs-lookup"><span data-stu-id="079d2-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="079d2-140">Performances améliorées</span><span class="sxs-lookup"><span data-stu-id="079d2-140">Improved performance</span></span>
* <span data-ttu-id="079d2-141">Gestion des versions côte à côte</span><span class="sxs-lookup"><span data-stu-id="079d2-141">Side-by-side versioning</span></span>
* <span data-ttu-id="079d2-142">Nouvelles API</span><span class="sxs-lookup"><span data-stu-id="079d2-142">New APIs</span></span>
* <span data-ttu-id="079d2-143">Open source</span><span class="sxs-lookup"><span data-stu-id="079d2-143">Open source</span></span>

<span data-ttu-id="079d2-144">Nous nous efforçons de combler l’écart d’API qui existe entre .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="079d2-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="079d2-145">Le [Pack de compatibilité Windows](/dotnet/core/porting/windows-compat-pack) a rendu disponible dans .NET Core des milliers d’API fonctionnant seulement dans Windows.</span><span class="sxs-lookup"><span data-stu-id="079d2-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="079d2-146">Ces API n’étaient pas disponibles dans .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="079d2-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="079d2-147">Parcours d’apprentissage recommandé</span><span class="sxs-lookup"><span data-stu-id="079d2-147">Recommended learning path</span></span>

<span data-ttu-id="079d2-148">Nous vous recommandons la séquence de tutoriels et d’articles suivante comme introduction au développement des applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="079d2-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="079d2-149">Suivez un tutoriel pour le type d’application que vous souhaitez développer ou gérer :</span><span class="sxs-lookup"><span data-stu-id="079d2-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="079d2-150">Type d'application</span><span class="sxs-lookup"><span data-stu-id="079d2-150">App type</span></span>  |<span data-ttu-id="079d2-151">Scénario</span><span class="sxs-lookup"><span data-stu-id="079d2-151">Scenario</span></span>  |<span data-ttu-id="079d2-152">Didacticiel</span><span class="sxs-lookup"><span data-stu-id="079d2-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="079d2-153">Application web</span><span class="sxs-lookup"><span data-stu-id="079d2-153">Web app</span></span>       | <span data-ttu-id="079d2-154">Pour un nouveau développement</span><span class="sxs-lookup"><span data-stu-id="079d2-154">For new development</span></span>        |[<span data-ttu-id="079d2-155">Bien démarrer avec les pages Razor</span><span class="sxs-lookup"><span data-stu-id="079d2-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="079d2-156">Application web</span><span class="sxs-lookup"><span data-stu-id="079d2-156">Web app</span></span>       | <span data-ttu-id="079d2-157">Pour maintenir une application MVC</span><span class="sxs-lookup"><span data-stu-id="079d2-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="079d2-158">Bien démarrer avec MVC</span><span class="sxs-lookup"><span data-stu-id="079d2-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="079d2-159">API web</span><span class="sxs-lookup"><span data-stu-id="079d2-159">Web API</span></span>       |                            |<span data-ttu-id="079d2-160">[Créer une API web](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="079d2-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="079d2-161">Application en temps réel</span><span class="sxs-lookup"><span data-stu-id="079d2-161">Real-time app</span></span> |                            |[<span data-ttu-id="079d2-162">Bien démarrer avec SignalR</span><span class="sxs-lookup"><span data-stu-id="079d2-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |

1. <span data-ttu-id="079d2-163">Suivez un tutoriel qui montre comment exécuter l’accès aux données de base :</span><span class="sxs-lookup"><span data-stu-id="079d2-163">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="079d2-164">Scénario</span><span class="sxs-lookup"><span data-stu-id="079d2-164">Scenario</span></span>  |<span data-ttu-id="079d2-165">Didacticiel</span><span class="sxs-lookup"><span data-stu-id="079d2-165">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="079d2-166">Pour un nouveau développement</span><span class="sxs-lookup"><span data-stu-id="079d2-166">For new development</span></span>        |[<span data-ttu-id="079d2-167">Pages Razor avec Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="079d2-167">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="079d2-168">Pour maintenir une application MVC</span><span class="sxs-lookup"><span data-stu-id="079d2-168">For maintaining an MVC app</span></span> |[<span data-ttu-id="079d2-169">MVC avec Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="079d2-169">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="079d2-170">Lisez une présentation des fonctionnalités d’ASP.NET Core qui s’appliquent à tous les types d’application :</span><span class="sxs-lookup"><span data-stu-id="079d2-170">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="079d2-171">Notions de base</span><span class="sxs-lookup"><span data-stu-id="079d2-171">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="079d2-172">Parcourez la Table des matières pour d’autres rubriques qui vous intéressent.</span><span class="sxs-lookup"><span data-stu-id="079d2-172">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="079d2-173">\* Il existe un nouveau [tutoriel sur l’API web que vous pouvez suivre entièrement dans le navigateur](https://docs.microsoft.com/learn/modules/build-web-api-net-core) (aucune installation d’IDE locale requise).</span><span class="sxs-lookup"><span data-stu-id="079d2-173">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="079d2-174">Le code s’exécute dans [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), et [curl](https://curl.haxx.se/) est utilisé à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="079d2-174">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="079d2-175">Comment télécharger un exemple</span><span class="sxs-lookup"><span data-stu-id="079d2-175">How to download a sample</span></span>

<span data-ttu-id="079d2-176">La plupart des articles et tutoriels contiennent des liens vers des exemples de code.</span><span class="sxs-lookup"><span data-stu-id="079d2-176">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="079d2-177">[Téléchargez le fichier zip du référentiel ASP.NET](https://codeload.github.com/aspnet/AspNetCore.Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="079d2-177">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="079d2-178">Décompressez le fichier *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="079d2-178">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="079d2-179">Utilisez l’URL contenue dans l’exemple de lien pour vous aider à naviguer dans l’exemple de répertoire.</span><span class="sxs-lookup"><span data-stu-id="079d2-179">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="079d2-180">Directives de préprocesseur dans l’exemple de code</span><span class="sxs-lookup"><span data-stu-id="079d2-180">Preprocessor directives in sample code</span></span>

<span data-ttu-id="079d2-181">Pour illustrer plusieurs scénarios, les exemples d’applications utilisent les instructions C# `#define` et `#if-#else/#elif-#endif` pour compiler et exécuter différentes sections de l’exemple de code de manière sélective.</span><span class="sxs-lookup"><span data-stu-id="079d2-181">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="079d2-182">Pour ces exemples qui utilisent cette approche, définissez l’instruction `#define` en haut des fichiers C# pour le symbole associé au scénario que vous souhaitez exécuter.</span><span class="sxs-lookup"><span data-stu-id="079d2-182">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="079d2-183">Certains exemples exigent que vous définissiez le symbole en haut de plusieurs fichiers afin d’exécuter un scénario.</span><span class="sxs-lookup"><span data-stu-id="079d2-183">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="079d2-184">Par exemple, la liste des symboles `#define` suivante indique que les quatre scénarios sont disponibles (un scénario par symbole).</span><span class="sxs-lookup"><span data-stu-id="079d2-184">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="079d2-185">La configuration actuelle de l’exemple exécute le scénario `TemplateCode` :</span><span class="sxs-lookup"><span data-stu-id="079d2-185">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="079d2-186">Pour que l’exemple exécute le scénario `ExpandDefault`, définissez le symbole `ExpandDefault` et laissez les symboles restants commentés :</span><span class="sxs-lookup"><span data-stu-id="079d2-186">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="079d2-187">Pour plus d’informations sur l’utilisation des [directives de préprocesseur C#](/dotnet/csharp/language-reference/preprocessor-directives/) pour compiler de façon sélective des sections de code, consultez [#define (Référence C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) et [#if (Référence C#) ](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="079d2-187">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="079d2-188">Régions dans l’exemple de code</span><span class="sxs-lookup"><span data-stu-id="079d2-188">Regions in sample code</span></span>

<span data-ttu-id="079d2-189">Certains exemples d’applications contiennent des sections de code entourées d’instructions C# [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) et [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion).</span><span class="sxs-lookup"><span data-stu-id="079d2-189">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="079d2-190">Le système de génération de documentation injecte ces régions dans les rubriques de documentation affichées.</span><span class="sxs-lookup"><span data-stu-id="079d2-190">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="079d2-191">Les noms des régions contiennent généralement le mot « snippet ».</span><span class="sxs-lookup"><span data-stu-id="079d2-191">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="079d2-192">L’exemple suivant montre une région nommée `snippet_FilterInCode` :</span><span class="sxs-lookup"><span data-stu-id="079d2-192">The following example shows a region named `snippet_FilterInCode`:</span></span>

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

<span data-ttu-id="079d2-193">L’extrait de code C# précédent est référencé dans le fichier Markdown de la rubrique avec la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="079d2-193">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="079d2-194">Vous pouvez sans risque ignorer (ou supprimer) les instructions `#region` et `#endregion` qui entourent le code.</span><span class="sxs-lookup"><span data-stu-id="079d2-194">You may safely ignore (or remove) the `#region` and `#endregion` statements that surround the code.</span></span> <span data-ttu-id="079d2-195">Ne modifiez pas le code dans ces instructions si vous prévoyez d’exécuter les exemples de scénarios décrits dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="079d2-195">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="079d2-196">N’hésitez pas à modifier le code quand vous testez d’autres scénarios.</span><span class="sxs-lookup"><span data-stu-id="079d2-196">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="079d2-197">Pour plus d’informations, consultez [Contribuer à la documentation ASP.NET : extraits de code](https://github.com/aspnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="079d2-197">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="079d2-198">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="079d2-198">Next steps</span></span>

<span data-ttu-id="079d2-199">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="079d2-199">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="079d2-200">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="079d2-200">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="079d2-201">[Le point hebdomadaire de la communauté ASP.NET](https://live.asp.net/) couvre l’avancement et les plans des équipes de développement.</span><span class="sxs-lookup"><span data-stu-id="079d2-201">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="079d2-202">Il met en avant de nouveaux blogs et des logiciels de tiers.</span><span class="sxs-lookup"><span data-stu-id="079d2-202">It features new blogs and third-party software.</span></span>
