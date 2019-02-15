---
title: Présentation d’ASP.NET Core
author: rick-anderson
description: Découvrez une introduction à ASP.NET Core, framework multiplateforme à hautes performances et open source qui permet de créer des applications cloud modernes et connectées à Internet.
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: index
ms.openlocfilehash: c3f07814bfab19a0f070e0b48b0d2ef6cfc1594e
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248158"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="69bbf-103">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69bbf-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="69bbf-104">Par [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="69bbf-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="69bbf-105">ASP.NET Core est un framework multiplateforme à hautes performances et [open source](https://github.com/aspnet/home) pour créer des applications cloud modernes et connectées à Internet.</span><span class="sxs-lookup"><span data-stu-id="69bbf-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="69bbf-106">Avec ASP.NET Core, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="69bbf-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="69bbf-107">Créer des applications et des services web, des applications [IoT](https://www.microsoft.com/internet-of-things/) et des back-ends mobiles.</span><span class="sxs-lookup"><span data-stu-id="69bbf-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="69bbf-108">Utiliser vos outils de développement préférés sur Windows, macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="69bbf-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="69bbf-109">Déployer dans le cloud ou localement.</span><span class="sxs-lookup"><span data-stu-id="69bbf-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="69bbf-110">Exécuter sur [.NET Core ou .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="69bbf-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="69bbf-111">Pourquoi utiliser ASP.NET Core ?</span><span class="sxs-lookup"><span data-stu-id="69bbf-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="69bbf-112">Des millions de développeurs ont utilisé (et continuent d’utiliser) [ASP.NET 4.x](/aspnet/overview) pour créer des applications web.</span><span class="sxs-lookup"><span data-stu-id="69bbf-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="69bbf-113">ASP.NET Core est une refonte d’ASP.NET 4.x, avec des modifications d’architecture qui aboutissent à un framework plus léger et modulaire.</span><span class="sxs-lookup"><span data-stu-id="69bbf-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="69bbf-114">Créer des API web et une interface utilisateur web en utilisant le modèle MVC d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69bbf-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="69bbf-115">Le modèle MVC d’ASP.NET Core fournit des fonctionnalités pour créer des [API web](xref:tutorials/first-web-api) et des [applications web](xref:tutorials/razor-pages/index) :</span><span class="sxs-lookup"><span data-stu-id="69bbf-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="69bbf-116">Le [modèle MVC (Modèle-Vue-Contrôleur)](xref:mvc/overview) permet de rendre vos API web et vos applications web testables.</span><span class="sxs-lookup"><span data-stu-id="69bbf-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="69bbf-117">[Razor Pages](xref:razor-pages/index) est un modèle de programmation basé sur les pages qui rend la création d’une interface utilisateur web plus facile et plus productive.</span><span class="sxs-lookup"><span data-stu-id="69bbf-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="69bbf-118">Le [balisage Razor](xref:mvc/views/razor) fournit une syntaxe efficace pour les [pages Razor](xref:razor-pages/index) et les [vues MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="69bbf-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="69bbf-119">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="69bbf-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="69bbf-120">La prise en charge intégrée de [plusieurs formats de données et de la négociation de contenu](xref:web-api/advanced/formatting) permet à vos API web d’être utilisées par un large éventail de clients, notamment des navigateurs et des appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="69bbf-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="69bbf-121">Le principe de la [liaison de modèle](xref:mvc/models/model-binding) permet le mappage automatiquement des données des requêtes HTTP aux paramètres des méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="69bbf-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="69bbf-122">La [validation de modèle](xref:mvc/models/validation) effectue automatiquement la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="69bbf-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="69bbf-123">Développement côté client</span><span class="sxs-lookup"><span data-stu-id="69bbf-123">Client-side development</span></span>

<span data-ttu-id="69bbf-124">ASP.NET Core s’intègre parfaitement avec les frameworks et les bibliothèques populaires côté client, notamment [Razor Components](xref:razor-components/index), [Angular](xref:spa/angular), [React](xref:spa/react) et [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="69bbf-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Razor Components](xref:razor-components/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="69bbf-125">Pour plus d’informations, consultez [Razor Components](xref:razor-components/index) et les rubriques connexes sous *Développement côté client*.</span><span class="sxs-lookup"><span data-stu-id="69bbf-125">For more information, see [Razor Components](xref:razor-components/index) and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="69bbf-126">ASP.NET Core ciblant .NET Framework</span><span class="sxs-lookup"><span data-stu-id="69bbf-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="69bbf-127">ASP.NET Core 2.x peut cibler .NET Core ou le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="69bbf-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="69bbf-128">Les applications ASP.NET Core ciblant .NET Framework ne sont pas multiplateformes : elles s’exécutent seulement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="69bbf-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="69bbf-129">D’une façon générale, ASP.NET Core 2.x est constitué de bibliothèques [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="69bbf-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="69bbf-130">Les applications écrites avec .NET Standard 2.0 s’exécutent partout où .NET Standard 2.0 est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="69bbf-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="69bbf-131">ASP.NET Core 2.x est pris en charge sur les versions de .NET Framework compatibles avec .NET Standard 2.0 :</span><span class="sxs-lookup"><span data-stu-id="69bbf-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="69bbf-132">.NET Framework 4.7.1 ou version ultérieure est vivement recommandé.</span><span class="sxs-lookup"><span data-stu-id="69bbf-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="69bbf-133">.NET Framework 4.6.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="69bbf-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="69bbf-134">ASP.NET Core 3.0 et ultérieur s’exécute uniquement sur .NET Core.</span><span class="sxs-lookup"><span data-stu-id="69bbf-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="69bbf-135">Pour plus de détails concernant ce changement, consultez [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="69bbf-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="69bbf-136">Le ciblage de .NET Core présente plusieurs avantages, qui sont plus nombreux à chaque version.</span><span class="sxs-lookup"><span data-stu-id="69bbf-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="69bbf-137">Voici certains avantages de .NET Core par rapport à .NET Framework :</span><span class="sxs-lookup"><span data-stu-id="69bbf-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="69bbf-138">Multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="69bbf-138">Cross-platform.</span></span> <span data-ttu-id="69bbf-139">S’exécute sur macOS, Linux et Windows</span><span class="sxs-lookup"><span data-stu-id="69bbf-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="69bbf-140">Performances améliorées</span><span class="sxs-lookup"><span data-stu-id="69bbf-140">Improved performance</span></span>
* <span data-ttu-id="69bbf-141">Gestion des versions côte à côte</span><span class="sxs-lookup"><span data-stu-id="69bbf-141">Side-by-side versioning</span></span>
* <span data-ttu-id="69bbf-142">Nouvelles API</span><span class="sxs-lookup"><span data-stu-id="69bbf-142">New APIs</span></span>
* <span data-ttu-id="69bbf-143">Open source</span><span class="sxs-lookup"><span data-stu-id="69bbf-143">Open source</span></span>

<span data-ttu-id="69bbf-144">Nous nous efforçons de combler l’écart d’API qui existe entre .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="69bbf-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="69bbf-145">Le [Pack de compatibilité Windows](/dotnet/core/porting/windows-compat-pack) a rendu disponible dans .NET Core des milliers d’API fonctionnant seulement dans Windows.</span><span class="sxs-lookup"><span data-stu-id="69bbf-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="69bbf-146">Ces API n’étaient pas disponibles dans .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="69bbf-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="69bbf-147">Comment télécharger un exemple</span><span class="sxs-lookup"><span data-stu-id="69bbf-147">How to download a sample</span></span>

<span data-ttu-id="69bbf-148">La plupart des articles et tutoriels contiennent des liens vers des exemples de code.</span><span class="sxs-lookup"><span data-stu-id="69bbf-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="69bbf-149">[Téléchargez le fichier zip du référentiel ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="69bbf-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="69bbf-150">Décompressez le fichier *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="69bbf-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="69bbf-151">Utilisez l’URL contenue dans l’exemple de lien pour vous aider à naviguer dans l’exemple de répertoire.</span><span class="sxs-lookup"><span data-stu-id="69bbf-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="69bbf-152">Directives de préprocesseur dans l’exemple de code</span><span class="sxs-lookup"><span data-stu-id="69bbf-152">Preprocessor directives in sample code</span></span>

<span data-ttu-id="69bbf-153">Pour illustrer plusieurs scénarios, les exemples d’applications utilisent les instructions C# `#define` et `#if-#else/#elif-#endif` pour compiler et exécuter différentes sections de l’exemple de code de manière sélective.</span><span class="sxs-lookup"><span data-stu-id="69bbf-153">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="69bbf-154">Pour ces exemples qui utilisent cette approche, définissez l’instruction `#define` en haut des fichiers C# pour le symbole associé au scénario que vous souhaitez exécuter.</span><span class="sxs-lookup"><span data-stu-id="69bbf-154">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="69bbf-155">Certains exemples exigent que vous définissiez le symbole en haut de plusieurs fichiers afin d’exécuter un scénario.</span><span class="sxs-lookup"><span data-stu-id="69bbf-155">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="69bbf-156">Par exemple, la liste des symboles `#define` suivante indique que les quatre scénarios sont disponibles (un scénario par symbole).</span><span class="sxs-lookup"><span data-stu-id="69bbf-156">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="69bbf-157">La configuration actuelle de l’exemple exécute le scénario `TemplateCode` :</span><span class="sxs-lookup"><span data-stu-id="69bbf-157">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="69bbf-158">Pour que l’exemple exécute le scénario `ExpandDefault`, définissez le symbole `ExpandDefault` et laissez les symboles restants commentés :</span><span class="sxs-lookup"><span data-stu-id="69bbf-158">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="69bbf-159">Pour plus d’informations sur l’utilisation des [directives de préprocesseur C#](/dotnet/csharp/language-reference/preprocessor-directives/) pour compiler de façon sélective des sections de code, consultez [#define (Référence C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) et [#if (Référence C#) ](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="69bbf-159">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="69bbf-160">Régions dans l’exemple de code</span><span class="sxs-lookup"><span data-stu-id="69bbf-160">Regions in sample code</span></span>

<span data-ttu-id="69bbf-161">Certains exemples d’applications contiennent des sections de code entourées d’instructions C# [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) et [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion).</span><span class="sxs-lookup"><span data-stu-id="69bbf-161">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="69bbf-162">Le système de génération de documentation injecte ces régions dans les rubriques de documentation affichées.</span><span class="sxs-lookup"><span data-stu-id="69bbf-162">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="69bbf-163">Les noms des régions contiennent généralement le mot « snippet ».</span><span class="sxs-lookup"><span data-stu-id="69bbf-163">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="69bbf-164">L’exemple suivant montre une région nommée `snippet_FilterInCode` :</span><span class="sxs-lookup"><span data-stu-id="69bbf-164">The following example shows a region named `snippet_FilterInCode`:</span></span>

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

<span data-ttu-id="69bbf-165">L’extrait de code C# précédent est référencé dans le fichier Markdown de la rubrique avec la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="69bbf-165">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="69bbf-166">Vous pouvez sans risque ignorer (ou supprimer) les instructions `#region` et `#endregion` qui entourent le code.</span><span class="sxs-lookup"><span data-stu-id="69bbf-166">You may safely ignore (or remove) the `#region` and `#endregion` statements that surround the code.</span></span> <span data-ttu-id="69bbf-167">Ne modifiez pas le code dans ces instructions si vous prévoyez d’exécuter les exemples de scénarios décrits dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="69bbf-167">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="69bbf-168">N’hésitez pas à modifier le code quand vous testez d’autres scénarios.</span><span class="sxs-lookup"><span data-stu-id="69bbf-168">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="69bbf-169">Pour plus d’informations, consultez [Contribuer à la documentation ASP.NET : extraits de code](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="69bbf-169">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="69bbf-170">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="69bbf-170">Next steps</span></span>

<span data-ttu-id="69bbf-171">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="69bbf-171">For more information, see the following resources:</span></span>

* [<span data-ttu-id="69bbf-172">Bien démarrer avec les pages Razor</span><span class="sxs-lookup"><span data-stu-id="69bbf-172">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="69bbf-173">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69bbf-173">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="69bbf-174">[Le point hebdomadaire de la communauté ASP.NET](https://live.asp.net/) couvre l’avancement et les plans des équipes de développement.</span><span class="sxs-lookup"><span data-stu-id="69bbf-174">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="69bbf-175">Il met en avant de nouveaux blogs et des logiciels de tiers.</span><span class="sxs-lookup"><span data-stu-id="69bbf-175">It features new blogs and third-party software.</span></span>
