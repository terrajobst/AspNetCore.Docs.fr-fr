---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Création d’URL lisibles dans ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: tfitzmac
description: Cet article décrit le routage dans un site Web ASP.NET Web Pages (Razor), et comment vous pouvez ainsi utiliser des URL qui sont plus lisible et une meilleure pratique pour le référencement. Vous allez...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d027be2924f24c1080deb4ee2deacc235bbd1001
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385746"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Création d’URL lisibles dans les Sites ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit le routage dans un site Web ASP.NET Web Pages (Razor), et comment vous pouvez ainsi utiliser des URL qui sont plus lisible et une meilleure pratique pour le référencement.
> 
> Ce que vous allez apprendre :
> 
> - Comment ASP.NET utilise le routage pour vous permettent d’utiliser des URL plus lisibles et consultable.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


## <a name="about-routing"></a>À propos du routage

Les URL pour les pages de votre site peuvent avoir un impact sur la manière dont le site fonctionne. Une URL qui est &quot;convivial&quot; peut rendre plus faciles à utiliser le site. Cela peut également améliorer l’optimisation des moteurs de recherche (SEO) pour le site. Sites Web ASP.NET incluent la possibilité d’utiliser automatiquement les URL conviviales.

ASP.NET vous permet de créer des URL significatives qui décrivent des actions de l’utilisateur au lieu de simplement pointer vers un fichier sur le serveur. Prendre en compte ces URL pour un blog fictif :

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Comparez ces URL pour les options suivantes :

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Dans la première paire, un utilisateur a à savoir que le blog est affiché à l’aide de la *blog.cshtml* page et puis aurait construire une chaîne de requête qui obtient la bonne catégorie ou plage de dates. Le deuxième ensemble d’exemples est beaucoup plus facile à comprendre et à créer.

Les URL pour le premier exemple également pointent directement vers un fichier spécifique (*blog.cshtml*). Si pour une raison quelconque le blog ont été déplacé vers un autre dossier sur le serveur, ou si le blog ayant été réécrites pour utiliser une autre page, les liens serait incorrects. Le deuxième ensemble d’URL ne pointe pas vers une page spécifique, par conséquent, même si l’implémentation de blog ou un emplacement change, l’URL serait toujours valides.

Dans ASP.NET Web Pages, vous pouvez créer une URL plus conviviale, comme celles figurant dans les exemples ci-dessus, car ASP.NET utilise *routage*. Routage de crée le mappage logique à partir d’une URL vers une page (ou de pages) qui peut répondre à la demande. Étant donné que le mappage est logique (non pas physiques à un fichier spécifique), le routage fournit une grande souplesse dans la façon dont vous définissez l’URL de votre site.

## <a name="how-routing-works"></a>Le fonctionnement du routage

Quand ASP.NET traite une demande, il lit l’URL pour déterminer comment l’acheminer. ASP.NET tente de faire correspondre des segments individuels de l’URL à des fichiers sur disque, allant de gauche à droite. S’il existe une correspondance, tout élément restant dans l’URL est passée à la page en tant que *les informations de chemin d’accès*.

Imaginez que quelqu'un fait une demande à l’aide de cette URL :

`http://www.contoso.com/a/b/c`

La recherche se déroule comme suit :

1. Existe-t-il un fichier avec le chemin d’accès et le nom de */a/b/c.cshtml*? Dans ce cas, exécutez cette page et lui ne passer aucune information. Sinon...
2. Existe-t-il un fichier avec le chemin d’accès et le nom de */a/b.cshtml*? Si cela, exécutez cette page et passez la valeur `c` à celui-ci. Sinon...
3. Existe-t-il un fichier avec le chemin d’accès et le nom de */a.cshtml*? Si cela, exécutez cette page et passez la valeur `b/c` à celui-ci.

Si la recherche trouvée exacte ne correspond à *.cshtml* fichiers dans leurs dossiers spécifiés, ASP.NET continue de rechercher ces fichiers à son tour :

1. */a/b/c/default.cshtml* (aucune information de chemin d’accès).
2. */a/b/c/index.cshtml* (aucune information de chemin d’accès).

> [!NOTE]
> En fait, les demandes pour des pages spécifiques (autrement dit, les requêtes qui incluent le *.cshtml* extension de nom de fichier) fonctionnent comme prévu. Une requête semblable à `http://www.contoso.com/a/b.cshtml` exécutera la page *b.cshtml* parfaitement.


À l’intérieur d’une page, vous pouvez obtenir les informations de chemin d’accès par le biais de la page `UrlData` propriété, qui est un dictionnaire. Imaginez que vous disposez d’un fichier nommé *ViewCustomers.cshtml* et votre site obtient cette demande :

`http://mysite.com/myWebSite/ViewCustomers/1000`

Comme décrit dans les règles ci-dessus, la demande est transmise à votre page. À l’intérieur de la page, vous pouvez utiliser du code semblable au suivant pour obtenir et afficher les informations de chemin d’accès (dans ce cas, la valeur &quot;1000&quot;) :

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Étant donné que le routage n’implique pas les noms de fichier complet, il peut y avoir une ambiguïté si vous avez des pages qui ont le même nom mais des extensions de nom de fichier (par exemple, *MyPage.cshtml* et *MyPage.html*) . Afin d’éviter des problèmes de routage, il est préférable de s’assurer que vous n’avez des pages de votre site dont les noms diffèrent uniquement par leur extension.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[WebMatrix - URL, de routage pour les moteurs de recherche et UrlData](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Cette entrée de blog par Mike Brind fournit des informations supplémentaires sur le fonctionnement du routage dans ASP.NET Web Pages.
