---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: "Création d’URL lisibles dans ASP.NET Web Pages (Razor) Sites | Documents Microsoft"
author: tfitzmac
description: "Cet article décrit le routage dans un site Web ASP.NET Web Pages (Razor), et comment vous pouvez ainsi utiliser des URL qui sont plus lisible et mieux pour les moteurs de recherche. Vous allez..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="d6e46-104">Création d’URL lisibles dans les Sites ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="d6e46-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="d6e46-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d6e46-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d6e46-106">Cet article décrit le routage dans un site Web ASP.NET Web Pages (Razor), et comment vous pouvez ainsi utiliser des URL qui sont plus lisible et mieux pour les moteurs de recherche.</span><span class="sxs-lookup"><span data-stu-id="d6e46-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="d6e46-107">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="d6e46-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d6e46-108">Comment ASP.NET utilise le routage pour vous permettre d’utiliser des URL plus lisibles et de recherches.</span><span class="sxs-lookup"><span data-stu-id="d6e46-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d6e46-109">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="d6e46-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d6e46-110">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d6e46-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d6e46-111">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="d6e46-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="d6e46-112">À propos du routage</span><span class="sxs-lookup"><span data-stu-id="d6e46-112">About Routing</span></span>

<span data-ttu-id="d6e46-113">Les URL pour les pages de votre site peuvent avoir un impact sur la manière dont le site fonctionne.</span><span class="sxs-lookup"><span data-stu-id="d6e46-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="d6e46-114">Une URL de &quot;convivial&quot; peut rendre plus faciles à utiliser le site.</span><span class="sxs-lookup"><span data-stu-id="d6e46-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="d6e46-115">Il peut également aider à l’optimisation de moteur de recherche (SEO) pour le site.</span><span class="sxs-lookup"><span data-stu-id="d6e46-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="d6e46-116">Les sites Web ASP.NET incluent la possibilité d’utiliser des URL conviviales automatiquement.</span><span class="sxs-lookup"><span data-stu-id="d6e46-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="d6e46-117">ASP.NET vous permet de créer des URL significatives qui décrivent les actions de l’utilisateur au lieu de simplement en pointant vers un fichier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="d6e46-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="d6e46-118">Prenons les URL pour un blog fictif :</span><span class="sxs-lookup"><span data-stu-id="d6e46-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="d6e46-119">Comparez les URL pour les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="d6e46-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="d6e46-120">Dans la première paire, un utilisateur est obligé de savoir que le blog est affiché à l’aide de la *blog.cshtml* page et puis auraient construire une chaîne de requête qui obtient la plage de catégorie ou la date à droite.</span><span class="sxs-lookup"><span data-stu-id="d6e46-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="d6e46-121">Le deuxième ensemble d’exemples est beaucoup plus facile à comprendre et à créer.</span><span class="sxs-lookup"><span data-stu-id="d6e46-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="d6e46-122">Les URL pour le premier exemple également pointent directement vers un fichier spécifique (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="d6e46-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="d6e46-123">Si pour une raison quelconque le blog ont été déplacé vers un autre dossier sur le serveur, ou si le blog ont été réécrites pour utiliser une autre page, les liens serait incorrects.</span><span class="sxs-lookup"><span data-stu-id="d6e46-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="d6e46-124">Le deuxième ensemble de l’URL ne pointe pas vers une page spécifique, par conséquent, même si l’emplacement ou le blog de l’implémentation change, l’URL serait toujours valides.</span><span class="sxs-lookup"><span data-stu-id="d6e46-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="d6e46-125">Dans ASP.NET Web Pages, vous pouvez créer une URL conviviale telles que celles dans les exemples ci-dessus, car ASP.NET utilise *routage*.</span><span class="sxs-lookup"><span data-stu-id="d6e46-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="d6e46-126">Routage de crée le mappage logique à partir d’une URL vers une page (ou de pages) qui peut répondre à la demande.</span><span class="sxs-lookup"><span data-stu-id="d6e46-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="d6e46-127">Étant donné que le mappage est logique (non physique, dans un fichier spécifique), le routage fournit une grande souplesse dans la façon dont vous définissez l’URL de votre site.</span><span class="sxs-lookup"><span data-stu-id="d6e46-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="d6e46-128">Fonctionnement du routage</span><span class="sxs-lookup"><span data-stu-id="d6e46-128">How Routing Works</span></span>

<span data-ttu-id="d6e46-129">Quand ASP.NET traite une demande, il lit l’URL pour déterminer comment router.</span><span class="sxs-lookup"><span data-stu-id="d6e46-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="d6e46-130">ASP.NET essaie de faire correspondre les segments individuels de l’URL à des fichiers sur disque, allant de gauche à droite.</span><span class="sxs-lookup"><span data-stu-id="d6e46-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="d6e46-131">S’il existe une correspondance, quoi que ce soit dans l’URL est passé à la page en tant que *les informations de chemin d’accès*.</span><span class="sxs-lookup"><span data-stu-id="d6e46-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="d6e46-132">Imaginez que quelqu'un effectue une demande à l’aide de cette URL :</span><span class="sxs-lookup"><span data-stu-id="d6e46-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="d6e46-133">La recherche est la suivante :</span><span class="sxs-lookup"><span data-stu-id="d6e46-133">The search goes like this:</span></span>

1. <span data-ttu-id="d6e46-134">Existe-t-il un fichier avec le chemin d’accès et le nom de */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="d6e46-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="d6e46-135">Dans ce cas, exécutez cette page et lui ne passer aucune information.</span><span class="sxs-lookup"><span data-stu-id="d6e46-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="d6e46-136">Sinon...</span><span class="sxs-lookup"><span data-stu-id="d6e46-136">Otherwise ...</span></span>
2. <span data-ttu-id="d6e46-137">Existe-t-il un fichier avec le chemin d’accès et le nom de */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="d6e46-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="d6e46-138">Si par conséquent, cette page d’exécution et transmettre la valeur `c` à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="d6e46-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="d6e46-139">Sinon...</span><span class="sxs-lookup"><span data-stu-id="d6e46-139">Otherwise …</span></span>
3. <span data-ttu-id="d6e46-140">Existe-t-il un fichier avec le chemin d’accès et le nom de */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="d6e46-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="d6e46-141">Si par conséquent, cette page d’exécution et transmettre la valeur `b/c` à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="d6e46-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="d6e46-142">Si la recherche a détecté exacte ne correspond pour *.cshtml* fichiers dans leurs dossiers spécifiés, ASP.NET continue à son tour la recherche de ces fichiers :</span><span class="sxs-lookup"><span data-stu-id="d6e46-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="d6e46-143">*/a/b/c/default.cshtml* (aucune information de chemin d’accès).</span><span class="sxs-lookup"><span data-stu-id="d6e46-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="d6e46-144">*/a/b/c/index.cshtml* (aucune information de chemin d’accès).</span><span class="sxs-lookup"><span data-stu-id="d6e46-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="d6e46-145">Pour être clair, demandes pour des pages spécifiques (autrement dit, les demandes qui incluent le *.cshtml* extension de nom de fichier) fonctionnent comme vous vous attendez.</span><span class="sxs-lookup"><span data-stu-id="d6e46-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="d6e46-146">Une demande comme `http://www.contoso.com/a/b.cshtml` exécutera la page *b.cshtml* tout à fait correctement.</span><span class="sxs-lookup"><span data-stu-id="d6e46-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="d6e46-147">À l’intérieur d’une page, vous pouvez obtenir les informations de chemin d’accès par le biais de la page `UrlData` propriété, qui est un dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="d6e46-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="d6e46-148">Imaginez que vous disposez d’un fichier nommé *ViewCustomers.cshtml* et votre site obtient cette demande :</span><span class="sxs-lookup"><span data-stu-id="d6e46-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="d6e46-149">Comme décrit dans les règles ci-dessus, la demande est transmise à votre page.</span><span class="sxs-lookup"><span data-stu-id="d6e46-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="d6e46-150">À l’intérieur de la page, vous pouvez utiliser le code suivant pour obtenir et afficher les informations de chemin d’accès (dans ce cas, la valeur &quot;1000&quot;) :</span><span class="sxs-lookup"><span data-stu-id="d6e46-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="d6e46-151">Étant donné que le routage n’implique pas les noms de fichier complet, il peut y avoir une ambiguïté si vous avez des pages qui ont le même nom mais des extensions de nom de fichier (par exemple, *MyPage.cshtml* et *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="d6e46-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="d6e46-152">Afin d’éviter des problèmes de routage, il est préférable de s’assurer que vous n’avez pas les pages de votre site dont les noms diffèrent uniquement par leur extension.</span><span class="sxs-lookup"><span data-stu-id="d6e46-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d6e46-153">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d6e46-153">Additional Resources</span></span>

<span data-ttu-id="d6e46-154">[WebMatrix - URL, de routage pour les moteurs de recherche et UrlData](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="d6e46-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="d6e46-155">Cette entrée de blog par Mike Brind fournit des informations supplémentaires sur le fonctionnement du routage dans ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="d6e46-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
