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
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Exécution côte à côte de différentes Versions de ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment exécuter des sites Web ASP.NET Web Pages (Razor) sur le même ordinateur ou serveur lorsque les sites Web sont configurés pour utiliser les différentes versions d’ASP.NET Web Pages.
> 
> Ce que vous allez apprendre :
> 
> - Le comportement par défaut Nouveautés dans ASP.NET lorsque vous avez des sites créés avec ASP.NET Web Pages.
> - Comment configurer un nouveau site pour exécuter avec une version antérieure d’ASP.NET Web Pages.
>   
> 
> Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :
> 
> - Le `webPages:Version` paramètre de configuration.
>   
> 
> ## <a name="software-versions"></a>Versions du logiciel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2 et la version 1.0 de ASP.NET Web Pages.


Les Pages Web ASP.NET prend en charge la capacité d’exécuter des sites Web côte à côte. Cela vous permet de continuer à exécuter vos anciennes applications ASP.NET Web Pages, de créer de nouvelles applications ASP.NET Web Pages et de tous les exécuter sur le même ordinateur.

Voici quelques points à retenir lorsque vous installez les Pages Web avec WebMatrix :

- Par défaut, les applications Web Pages existantes exécutera en tant que la version la plus récente sur votre ordinateur. (Les assemblys sont installés dans le global assembly cache (GAC) et sont automatiquement utilisés.)
- Si vous souhaitez exécuter un site à l’aide d’une autre version d’ASP.NET Web Pages, vous pouvez configurer le site pour ce faire. Si votre site n’a pas encore un *web.config* dans la racine du site, créez-en un et copiez-y le code XML suivant, remplacer le contenu existant. Si le site contient déjà un *web.config* , ajoutez une `<appSettings>` élément tel que le suivant à la `<configuration>` section.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-Si vous ne spécifiez pas une version dans le *web.config* fichier, un site est déployé en tant que la version la plus récente. (Les assemblys sont copiés dans le *bin* dossier dans le site déployé.)
- Nouvelles applications que vous créez dans les modèles de site Web Matrix incluent les assemblys de version des Pages Web dans la table *bin* dossier.

En règle générale, vous pouvez contrôler toujours la version de Pages Web à utiliser avec votre site à l’aide de NuGet pour installer les assemblys appropriés dans le site *bin* dossier. Pour rechercher les packages, visitez [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Ressources supplémentaires

[Les fonctionnalités principales dans les Pages Web ASP.NET 2](top-features-in-web-pages-2.md)
