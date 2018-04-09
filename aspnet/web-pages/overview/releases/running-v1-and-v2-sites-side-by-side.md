---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Exécutez différentes Versions des Pages Web ASP.NET (Razor) côte à côte | Documents Microsoft
author: tfitzmac
description: Cet article explique comment exécuter des sites Web ASP.NET Web Pages (Razor) sur le même ordinateur ou serveur lorsque les sites Web sont configurés pour utiliser des versions différentes...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="8e6e8-103">Exécution côte à côte de différentes Versions de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="8e6e8-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="8e6e8-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8e6e8-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8e6e8-105">Cet article explique comment exécuter des sites Web ASP.NET Web Pages (Razor) sur le même ordinateur ou serveur lorsque les sites Web sont configurés pour utiliser les différentes versions d’ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="8e6e8-106">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="8e6e8-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8e6e8-107">Le comportement par défaut Nouveautés dans ASP.NET lorsque vous avez des sites créés avec ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="8e6e8-108">Comment configurer un nouveau site pour exécuter avec une version antérieure d’ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="8e6e8-109">Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :</span><span class="sxs-lookup"><span data-stu-id="8e6e8-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="8e6e8-110">Le `webPages:Version` paramètre de configuration.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="8e6e8-111">Versions du logiciel</span><span class="sxs-lookup"><span data-stu-id="8e6e8-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="8e6e8-112">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="8e6e8-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="8e6e8-113">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2 et la version 1.0 de ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="8e6e8-114">Les Pages Web ASP.NET prend en charge la capacité d’exécuter des sites Web côte à côte.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="8e6e8-115">Cela vous permet de continuer à exécuter vos anciennes applications ASP.NET Web Pages, de créer de nouvelles applications ASP.NET Web Pages et de tous les exécuter sur le même ordinateur.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="8e6e8-116">Voici quelques points à retenir lorsque vous installez les Pages Web avec WebMatrix :</span><span class="sxs-lookup"><span data-stu-id="8e6e8-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="8e6e8-117">Par défaut, les applications Web Pages existantes exécutera en tant que la version la plus récente sur votre ordinateur.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="8e6e8-118">(Les assemblys sont installés dans le global assembly cache (GAC) et sont automatiquement utilisés.)</span><span class="sxs-lookup"><span data-stu-id="8e6e8-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="8e6e8-119">Si vous souhaitez exécuter un site à l’aide d’une autre version d’ASP.NET Web Pages, vous pouvez configurer le site pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="8e6e8-120">Si votre site n’a pas encore un *web.config* dans la racine du site, créez-en un et copiez-y le code XML suivant, remplacer le contenu existant.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="8e6e8-121">Si le site contient déjà un *web.config* , ajoutez une `<appSettings>` élément tel que le suivant à la `<configuration>` section.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="8e6e8-122">'-Si vous ne spécifiez pas une version dans le *web.config* fichier, un site est déployé en tant que la version la plus récente.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="8e6e8-123">(Les assemblys sont copiés dans le *bin* dossier dans le site déployé.)</span><span class="sxs-lookup"><span data-stu-id="8e6e8-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="8e6e8-124">Nouvelles applications que vous créez dans les modèles de site Web Matrix incluent les assemblys de version des Pages Web dans la table *bin* dossier.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="8e6e8-125">En règle générale, vous pouvez contrôler toujours la version de Pages Web à utiliser avec votre site à l’aide de NuGet pour installer les assemblys appropriés dans le site *bin* dossier.</span><span class="sxs-lookup"><span data-stu-id="8e6e8-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="8e6e8-126">Pour rechercher les packages, visitez [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="8e6e8-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e6e8-127">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8e6e8-127">Additional Resources</span></span>

[<span data-ttu-id="8e6e8-128">Les fonctionnalités principales dans les Pages Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="8e6e8-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
