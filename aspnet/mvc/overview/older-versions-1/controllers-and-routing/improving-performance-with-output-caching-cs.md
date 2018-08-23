---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Amélioration des performances avec la sortie mise en cache (c#) | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, vous découvrez comment vous pouvez améliorer considérablement les performances des applications web ASP.NET MVC en tirant parti de la mise en cache de sortie. Vous...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 516c370941b8f7e5f3528953491057973679586d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838378"
---
<a name="improving-performance-with-output-caching-c"></a>Amélioration des performances avec la sortie de la mise en cache (c#)
====================
par [Microsoft](https://github.com/microsoft)

> Dans ce didacticiel, vous découvrez comment vous pouvez améliorer considérablement les performances des applications web ASP.NET MVC en tirant parti de la mise en cache de sortie. Vous allez apprendre à mettre en cache le résultat retourné par une action de contrôleur afin que le même contenu n’ait pas besoin soit créée chaque fois qu'un nouvel utilisateur appelle l’action.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez améliorer considérablement les performances d’une application ASP.NET MVC en tirant parti du cache de sortie. Le cache de sortie vous permet de mettre en cache le contenu retourné par une action de contrôleur. De cette façon, le même contenu dispense doit être généré chaque fois que la même action de contrôleur est appelée.

Par exemple, imaginez que votre application ASP.NET MVC affiche une liste d’enregistrements de base de données dans une vue nommée des Index. Normalement, chaque fois un utilisateur appelle l’action du contrôleur qui retourne la vue Index, de l’ensemble d’enregistrements de base de données doit être récupérée à partir de la base de données en exécutant une requête de base de données.

Si, en revanche, vous tirez parti du cache de sortie vous pouvez éviter l’exécution d’une requête de base de données chaque fois qu’un utilisateur appelle la même action de contrôleur. La vue peut être récupérée à partir du cache au lieu d’être régénérée à partir de l’action du contrôleur. La mise en cache permet de vous éviter d’exécuter redondant travailler sur le serveur.

## <a name="enabling-output-caching"></a>L’activation de la mise en cache de sortie

Vous activez la mise en cache de sortie en ajoutant un attribut [OutputCache] à une action de contrôleur individuel ou une classe d’ensemble du contrôleur. Par exemple, le contrôleur dans le Listing 1 expose une action nommée Index(). La sortie de l’action Index() est mis en cache pendant 10 secondes.

**Liste 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

Dans les versions bêta d’ASP.NET MVC, la mise en cache de sortie ne fonctionne pas pour une URL telle que [ http://www.MySite.com/ ](http://www.mysite.com/). Au lieu de cela, vous devez entrer une URL telle que [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index). 

Dans la liste 1, la sortie de l’action Index() est mis en cache pendant 10 secondes. Si vous préférez, vous pouvez spécifier une durée de cache beaucoup plus de temps. Par exemple, si vous souhaitez mettre en cache la sortie d’une action de contrôleur pendant une journée vous pouvez spécifier une durée de cache de 86 400 secondes (60 secondes \* 60 minutes \* des dernières 24 heures).

Il existe aucune garantie que le contenu ne sera mis en cache pendant la durée que vous spécifiez. Lorsque les ressources de mémoire sont faibles, le cache démarre automatiquement l’éviction du contenu.

Le contrôleur Home dans le Listing 1 retourne la vue Index dans le Listing 2. Il n’a rien de spécial sur cette vue. La vue Index affiche simplement l’heure actuelle (voir Figure 1).

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figure 1 – vue d’Index de la mise en cache**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Si vous appelez l’action Index() plusieurs fois en entrant l’URL de base/Index dans la barre d’adresses de votre navigateur et en appuyant sur le bouton d’actualisation/rechargement dans votre navigateur à plusieurs reprises, puis l’heure affichée par la vue Index ne changera pas pendant 10 secondes. Le même temps s’affiche, car la vue est mis en cache.

Il est important de comprendre que la même vue est mis en cache pour tous les utilisateurs qui visitent votre application. Toute personne qui appelle l’action Index() obtiendra la même version de mise en cache de la vue Index. Cela signifie que la quantité de travail que le serveur web doit effectuer pour répondre à la vue Index est considérablement réduite.

La vue dans la liste 2 se trouve être faire quelque chose de très simple. La vue affiche simplement l’heure actuelle. Toutefois, vous pouvez tout aussi facilement cache une vue qui affiche un ensemble d’enregistrements de base de données. Dans ce cas, le jeu d’enregistrements de base de données serait inutile à récupérer à partir de la base de données chaque fois que l’action du contrôleur qui retourne la vue est appelée. Elle peut réduire la quantité de travail que votre serveur web et le serveur de base de données doivent effectuer.

N’utilisez pas la page &lt;% @ OutputCache %&gt; directive dans une vue MVC. Cette directive est saigner au fil du monde de Web Forms et ne doit pas être utilisée dans une application ASP.NET MVC.

## <a name="where-content-is-cached"></a>Où le contenu est mis en cache

Par défaut, lorsque vous utilisez l’attribut [OutputCache], contenu est mis en cache dans trois emplacements : le serveur web, tous les serveurs proxy et le navigateur web. Vous pouvez contrôler exactement où contenu est mis en cache en modifiant la propriété d’emplacement de l’attribut [OutputCache].

Vous pouvez définir la propriété d’emplacement à l’une des valeurs suivantes :

> · N’importe quel
> 
> · Client
> 
> · En aval
> 
> · Serveur
> 
> · None
> 
> · ServerAndClient


Par défaut, la propriété d’emplacement a la valeur Any. Toutefois, il existe des situations dans lesquelles vous souhaiterez cache uniquement sur le navigateur ou uniquement sur le serveur. Par exemple, si vous mettez en cache d’informations personnalisé pour chaque utilisateur puis vous ne devez pas mettre en cache les informations sur le serveur. Si vous affichez des informations différentes à différents utilisateurs puis vous devez mettre en cache les informations uniquement sur le client.

Par exemple, le contrôleur dans le Listing 3 expose une action nommée GetName() qui retourne le nom d’utilisateur actuel. Si Jack se connecte au site Web et appelle l’action GetName() l’action retourne la chaîne « Hi Jack ». Si, par la suite, Jill se connecte au site Web et appelle l’action GetName() puis elle également obtenez la chaîne « Hi Jack ». La chaîne est mis en cache sur le serveur web pour tous les utilisateurs après que Jack appelle initialement l’action du contrôleur.

**Liste 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Très probablement, le contrôleur dans le Listing 3 ne fonctionne pas comme vous le souhaitez. Vous ne souhaitez pas afficher le message « Hi Jack » pour Jill.

Vous devez jamais mettre en cache un contenu personnalisé dans le cache du serveur. Toutefois, vous souhaiterez peut-être mettre en cache le contenu personnalisé dans le cache du navigateur pour améliorer les performances. Si vous mettez en cache le contenu dans le navigateur, et un utilisateur appelle l’action du contrôleur même plusieurs fois, le contenu peut être récupéré à partir du cache de navigateur au lieu du serveur.

Le contrôleur modifié sur la liste 4 met en cache la sortie de l’action GetName(). Toutefois, le contenu est mis en cache uniquement sur le navigateur et non sur le serveur. Ainsi, lorsque plusieurs utilisateurs appeler la méthode GetName(), chaque personne obtient son propre nom d’utilisateur et nom d’utilisateur pas à une autre personne.

**Liste 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Notez que l’attribut [OutputCache] sur la liste 4 inclut une propriété d’emplacement définie sur la valeur OutputCacheLocation.Client. L’attribut [OutputCache] inclut également une propriété NoStore. La propriété NoStore est utilisée pour informer navigateur et les serveurs proxy qu’ils ne doivent pas stocker une copie permanente du contenu mis en cache.

## <a name="varying-the-output-cache"></a>Faire varier le Cache de sortie

Dans certaines situations, vous pouvez effectuer différentes versions de mise en cache du contenu très même. Par exemple, imaginez que vous créez une page maître/détail. La page maître affiche une liste de titres de films. Lorsque vous cliquez sur un titre, vous obtenez des détails pour le film sélectionné.

Si vous mettez en cache la page de détails, puis les détails du même film seront affichera, quel que soit le film que vous cliquez sur. Le premier film sélectionné par le premier utilisateur s’affichera à tous les utilisateurs futurs.

Vous pouvez résoudre ce problème en tirant parti de la propriété VaryByParam de l’attribut [OutputCache]. Cette propriété vous permet de créer différentes versions de mise en cache du contenu très même lorsqu’un paramètre de formulaire ou varie en fonction du paramètre de chaîne de requête.

Par exemple, le contrôleur dans la liste 5 expose deux actions nommées Master() et Details(). L’action Master() retourne une liste de titres de films et l’action Details() renvoie les détails de la séquence sélectionnée.

**Liste 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

L’action Master() inclut une propriété VaryByParam avec la valeur « none ». Lorsque le Master() action est appelée, la même version de mise en cache du serveur principal affichez est retournée. Les paramètres de formulaire ou la chaîne de requête paramètres sont ignorés (voir Figure 2).

**Figure 2 – la vue /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figure 3 – la vue Détails/Movies /**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

L’action Details() inclut une propriété VaryByParam avec la valeur « Id ». Lorsque des valeurs différentes pour le paramètre Id sont transmis à l’action du contrôleur, différentes versions de mise en cache de l’affichage des détails sont générées.

Il est important de comprendre qu’à l’aide de la propriété VaryByParam entraîne la mise en cache plus et pas moins. Une autre version de mise en cache de l’affichage des détails est créée pour chaque version du paramètre Id.

Vous pouvez définir la propriété VaryByParam les valeurs suivantes :

> \* = Créer une version mise en cache différents chaque fois qu’un paramètre de chaîne de requête ou de formulaire varie.
> 
> None = jamais créer différentes versions de mise en cache
> 
> Liste de point-virgule de paramètres = créer différentes versions de mise en cache chaque fois que les paramètres de chaîne de formulaire ou d’une requête dans la liste varie


## <a name="creating-a-cache-profile"></a>Création d’un profil de Cache

Comme alternative à la configuration des propriétés de cache de sortie en modifiant les propriétés de l’attribut [OutputCache], vous pouvez créer un profil de cache dans le fichier de configuration (web.config) web. Création d’un profil de cache dans le fichier de configuration web offre deux avantages importants.

Tout d’abord, en configurant la mise en cache de sortie dans le fichier de configuration web, vous pouvez contrôler comment les actions de contrôleur mettre en cache le contenu dans un emplacement central. Vous pouvez créer un profil de cache et appliquez le profil à plusieurs contrôleurs ou les actions de contrôleur.

Ensuite, vous pouvez modifier le fichier de configuration web sans avoir à recompiler votre application. Si vous devez désactiver la mise en cache pour une application qui a déjà été déployée en production, vous pouvez simplement modifier les profils de cache définies dans le fichier de configuration web. Toutes les modifications au fichier de configuration web seront détectées automatiquement et appliquées.

Par exemple, le &lt;la mise en cache&gt; section de configuration web dans la liste 6 définit un profil de cache nommé Cache1Hour. Le &lt;la mise en cache&gt; section doit apparaître dans le &lt;system.web&gt; section d’un fichier de configuration web.

**Liste 6 – section mise en cache pour web.config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Le contrôleur dans la liste 7 illustre comment vous pouvez appliquer le profil Cache1Hour à une action de contrôleur avec l’attribut [OutputCache].

**Liste 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Si vous appelez l’action Index() exposée par le contrôleur dans la liste 7, le même temps sera retourné pour 1 heure.

## <a name="summary"></a>Récapitulatif

La mise en cache de sortie vous offre une méthode très facile d’améliorer considérablement les performances de vos applications ASP.NET MVC. Dans ce didacticiel, vous avez appris à utiliser l’attribut [OutputCache] pour mettre en cache la sortie d’actions de contrôleur. Vous avez également appris comment modifier les propriétés de l’attribut [OutputCache] telles que les propriétés de durée et VaryByParam pour modifier la façon dont le contenu est mis en cache. Enfin, vous avez appris à définir des profils de cache dans le fichier de configuration web.

> [!div class="step-by-step"]
> [Précédent](understanding-action-filters-cs.md)
> [Suivant](adding-dynamic-content-to-a-cached-page-cs.md)
