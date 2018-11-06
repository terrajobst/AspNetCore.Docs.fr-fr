---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: En cours d’exécution différentes Versions d’ASP.NET Web Pages (Razor) côte à côte | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment exécuter des sites Web ASP.NET Web Pages (Razor) sur le même ordinateur ou serveur lorsque les sites Web sont configurés pour utiliser différentes versions...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: e587398b430795c12a1dcee394852b4e2b8a0e44
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021171"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Exécution côte à côte de différentes Versions d’ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment exécuter des sites Web ASP.NET Web Pages (Razor) sur le même ordinateur ou serveur lorsque les sites Web sont configurés pour utiliser différentes versions d’ASP.NET Web Pages.
> 
> Ce que vous allez apprendre :
> 
> - Le comportement par défaut Nouveautés dans ASP.NET lorsque vous avez des sites créés avec ASP.NET Web Pages.
> - Comment configurer un nouveau site pour s’exécuter avec une version antérieure d’ASP.NET Web Pages.
>   
> 
> Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :
> 
> - Le `webPages:Version` paramètre de configuration.
>   
> 
> ## <a name="software-versions"></a>Versions des logiciels
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2 et ASP.NET Web Pages 1.0.


Les Pages Web ASP.NET prend en charge la possibilité d’exécuter des sites Web côte à côte. Cela vous permet de continuer à exécuter vos anciennes applications ASP.NET Web Pages, créer de nouvelles applications ASP.NET Web Pages et toutes les exécuter sur le même ordinateur.

Voici quelques points à retenir lorsque vous installez les Pages Web avec WebMatrix :

- Par défaut, les applications de Pages Web existantes seront exécutera comme la dernière version sur votre ordinateur. (Les assemblys sont installés dans le global assembly cache (GAC) et sont utilisées automatiquement.)
- Si vous souhaitez exécuter un site à l’aide d’une version différente d’ASP.NET Web Pages, vous pouvez configurer le site pour ce faire. Si votre site n’a pas encore un *web.config* dans la racine du site, créez-en un et copiez-y le code XML suivant, en remplaçant le contenu existant. Si le site contient déjà un *web.config* , ajoutez un `<appSettings>` élément semblable à celui-ci à le `<configuration>` section.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-Si vous ne spécifiez pas une version dans le *web.config* fichier, un site est déployé en tant que la dernière version. (Les assemblys sont copiés dans le *bin* dossier dans le site déployé.)
- Nouvelles applications que vous créez à l’aide des modèles de site dans Web Matrix incluent les assemblys de version des Pages Web dans le site *bin* dossier.

En règle générale, vous pouvez toujours de contrôler quelle version des Pages Web à utiliser avec votre site à l’aide de NuGet pour installer les assemblys appropriés dans le site *bin* dossier. Pour rechercher les packages, visitez [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Ressources supplémentaires

[Les fonctionnalités principales dans les Pages Web ASP.NET 2](top-features-in-web-pages-2.md)
