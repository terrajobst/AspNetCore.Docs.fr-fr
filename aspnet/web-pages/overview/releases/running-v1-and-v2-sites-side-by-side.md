---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: En cours d’exécution différentes Versions d’ASP.NET Web Pages (Razor) côte à côte | Microsoft Docs
author: tfitzmac
description: Cet article explique comment exécuter des sites Web ASP.NET Web Pages (Razor) sur le même ordinateur ou serveur lorsque les sites Web sont configurés pour utiliser différentes versions...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 86b1d5babac747eb4fa7ba8abde0e6155c8b17fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810054"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="8ec79-103">Exécution côte à côte de différentes Versions d’ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="8ec79-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="8ec79-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8ec79-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8ec79-105">Cet article explique comment exécuter des sites Web ASP.NET Web Pages (Razor) sur le même ordinateur ou serveur lorsque les sites Web sont configurés pour utiliser différentes versions d’ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="8ec79-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="8ec79-106">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="8ec79-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8ec79-107">Le comportement par défaut Nouveautés dans ASP.NET lorsque vous avez des sites créés avec ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="8ec79-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="8ec79-108">Comment configurer un nouveau site pour s’exécuter avec une version antérieure d’ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="8ec79-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="8ec79-109">Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :</span><span class="sxs-lookup"><span data-stu-id="8ec79-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="8ec79-110">Le `webPages:Version` paramètre de configuration.</span><span class="sxs-lookup"><span data-stu-id="8ec79-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="8ec79-111">Versions des logiciels</span><span class="sxs-lookup"><span data-stu-id="8ec79-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="8ec79-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="8ec79-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="8ec79-113">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2 et ASP.NET Web Pages 1.0.</span><span class="sxs-lookup"><span data-stu-id="8ec79-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="8ec79-114">Les Pages Web ASP.NET prend en charge la possibilité d’exécuter des sites Web côte à côte.</span><span class="sxs-lookup"><span data-stu-id="8ec79-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="8ec79-115">Cela vous permet de continuer à exécuter vos anciennes applications ASP.NET Web Pages, créer de nouvelles applications ASP.NET Web Pages et toutes les exécuter sur le même ordinateur.</span><span class="sxs-lookup"><span data-stu-id="8ec79-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="8ec79-116">Voici quelques points à retenir lorsque vous installez les Pages Web avec WebMatrix :</span><span class="sxs-lookup"><span data-stu-id="8ec79-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="8ec79-117">Par défaut, les applications de Pages Web existantes seront exécutera comme la dernière version sur votre ordinateur.</span><span class="sxs-lookup"><span data-stu-id="8ec79-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="8ec79-118">(Les assemblys sont installés dans le global assembly cache (GAC) et sont utilisées automatiquement.)</span><span class="sxs-lookup"><span data-stu-id="8ec79-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="8ec79-119">Si vous souhaitez exécuter un site à l’aide d’une version différente d’ASP.NET Web Pages, vous pouvez configurer le site pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="8ec79-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="8ec79-120">Si votre site n’a pas encore un *web.config* dans la racine du site, créez-en un et copiez-y le code XML suivant, en remplaçant le contenu existant.</span><span class="sxs-lookup"><span data-stu-id="8ec79-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="8ec79-121">Si le site contient déjà un *web.config* , ajoutez un `<appSettings>` élément semblable à celui-ci à le `<configuration>` section.</span><span class="sxs-lookup"><span data-stu-id="8ec79-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="8ec79-122">'-Si vous ne spécifiez pas une version dans le *web.config* fichier, un site est déployé en tant que la dernière version.</span><span class="sxs-lookup"><span data-stu-id="8ec79-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="8ec79-123">(Les assemblys sont copiés dans le *bin* dossier dans le site déployé.)</span><span class="sxs-lookup"><span data-stu-id="8ec79-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="8ec79-124">Nouvelles applications que vous créez à l’aide des modèles de site dans Web Matrix incluent les assemblys de version des Pages Web dans le site *bin* dossier.</span><span class="sxs-lookup"><span data-stu-id="8ec79-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="8ec79-125">En règle générale, vous pouvez toujours de contrôler quelle version des Pages Web à utiliser avec votre site à l’aide de NuGet pour installer les assemblys appropriés dans le site *bin* dossier.</span><span class="sxs-lookup"><span data-stu-id="8ec79-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="8ec79-126">Pour rechercher les packages, visitez [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="8ec79-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ec79-127">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8ec79-127">Additional Resources</span></span>

[<span data-ttu-id="8ec79-128">Les fonctionnalités principales dans les Pages Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="8ec79-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
