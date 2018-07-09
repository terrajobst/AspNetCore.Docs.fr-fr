---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Partie 10 : Mises à jour finales Navigation et de conception de Site, Conclusion | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 10 couvre les mises à jour finales de Navigation et S....
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 40ed7c337e097675199ab66229095bd3315d8c8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836804"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Partie 10 : Mises à jour finales Navigation et de conception de Site, Conclusion
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.  
>   
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 10 couvre les mises à jour finales de Navigation et de conception de Site, Conclusion.


Nous avons terminé toutes les fonctionnalités principales pour notre site, mais nous avons toujours certaines fonctionnalités à ajouter à la navigation du site, la page d’accueil et la page Rechercher un Store.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Création de la vue partielle résumé panier d’achat

Nous devions présenter le nombre d’éléments dans le panier d’achat de l’utilisateur sur l’ensemble du site.

![](mvc-music-store-part-10/_static/image1.png)

Nous pouvons facilement implémenter ceci en créant une vue partielle qui est ajoutée à notre Site.master.

Comme indiqué précédemment, le contrôleur ShoppingCart inclut une méthode d’action CartSummary qui retourne une vue partielle :

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Pour créer la vue partielle CartSummary, avec le bouton droit sur le dossier vues/ShoppingCart et sélectionnez Ajouter une vue. Nom de la vue CartSummary et cochez la case à cocher « Créer une vue partielle » comme indiqué ci-dessous.

![](mvc-music-store-part-10/_static/image2.png)

La vue partielle CartSummary est très simple : il est simplement un lien vers la vue de ShoppingCart Index qui indique le nombre d’éléments dans le panier d’achat. Le code complet pour CartSummary.cshtml est comme suit :

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Nous pouvons inclure une vue partielle dans n’importe quelle page dans le site, y compris le maître du Site, à l’aide de la méthode Html.RenderAction. RenderAction nous oblige à spécifier le nom d’Action (« CartSummary ») et le nom du contrôleur (« ShoppingCart ») comme ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Avant d’ajouter cela à la disposition du site, nous créerons également le Menu de Genre afin de pouvoir effectuer toutes nos mises à jour Site.master en même temps.

## <a name="creating-the-genre-menu-partial-view"></a>Création de la vue partielle du Menu Genre

Nous pouvons faire beaucoup plus facile pour nos utilisateurs de naviguer dans le magasin en ajoutant un Menu de Genre qui répertorie tous les Genres disponibles dans notre magasin.

![](mvc-music-store-part-10/_static/image3.png)

Nous allons suivre les mêmes étapes permettent également de créer une vue partielle GenreMenu, et puis nous pouvons ajouter les deux pour le Site maître. Tout d’abord, ajoutez l’action de contrôleur GenreMenu suivante à la StoreController :

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Cette action renvoie une liste de Genres qui sera affiché par la vue partielle, nous allons créer ensuite.

*Remarque : Nous avons ajouté l’attribut [ChildActionOnly] à cette action de contrôleur, ce qui indique que nous voulons uniquement cette action pour être utilisée à partir d’une vue partielle. Cet attribut empêche l’action du contrôleur à partir d’en cours d’exécution en accédant à /Store/GenreMenu. Cela n’est pas nécessaire pour les vues partielles, mais il est judicieux, car nous voulons s’assurer que nos actions de contrôleur sont utilisées comme nous avons l’intention. Nous avons également retourner PartialView plutôt que vue, qui informe le moteur d’affichage qu’il ne doit pas utiliser la mise en page pour cette vue, comme il est inclus dans les autres modes.*

Avec le bouton droit sur l’action du contrôleur GenreMenu et créer une vue partielle nommée GenreMenu qui est fortement typée à l’aide de la classe de données d’affichage Genre comme indiqué ci-dessous.

![](mvc-music-store-part-10/_static/image4.png)

Mettre à jour le code de vue de la vue partielle GenreMenu afficher les éléments à l’aide d’une liste non triée comme suit.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>La mise à jour de disposition de Site pour afficher les vues partielles

Nous pouvons ajouter notre vues partielles à la disposition du Site (/vues/Shared/\_Layout.cshtml) en appelant Html.RenderAction(). Nous allons ajouter les deux dans, ainsi que des balises supplémentaires pour les afficher, comme indiqué ci-dessous :

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Lorsque nous exécutons l’application, nous allons maintenant voir le Genre dans la zone de navigation gauche et le résumé du panier en haut.

## <a name="update-to-the-store-browse-page"></a>Mettre à jour vers la page Rechercher un Store

La page Rechercher un Store est fonctionnelle, mais ne semble pas très bien. Nous pouvons mettre à jour la page pour afficher des albums dans une meilleure mise en page en mettant à jour le code de vue (trouvé dans /Views/Store/Browse.cshtml) comme suit :

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Ici, nous effectuons utiliser Url.Action plutôt que Html.ActionLink afin que nous pouvons appliquer la mise en forme spéciale pour le lien pour inclure l’illustration de l’album.

*Remarque : Nous affichons une couverture d’album générique pour ces albums. Ces informations sont stockées dans la base de données et peut être modifiées via le Gestionnaire de Store. Vous pouvez ajouter votre propre signature graphique.*

Maintenant, lorsque nous accédez à un Genre, nous verrons albums affichées dans une grille avec l’illustration de l’album.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>La mise à jour de la Page d’accueil pour afficher des Albums de vente de haut

Nous souhaitons nos meilleures ventes albums sur la page d’accueil pour augmenter les ventes de fonctionnalités. Nous allons effectuer certaines mises à jour notre HomeController pour gérer cela, ajoutez dans certains graphiques supplémentaires également.

Tout d’abord, nous allons ajouter une propriété de navigation à notre classe Album afin que EntityFramework sache qu’elles sont associées. Les dernières lignes de notre **Album** classe doit maintenant ressembler à ceci :

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Remarque : Cela nécessite l’ajout d’un à l’aide de l’instruction à afficher dans l’espace de noms System.Collections.Generic.*

Tout d’abord, nous allons ajouter un champ storeDB et le MvcMusicStore.Models à l’aide des instructions, comme dans nos autres contrôleurs. Ensuite, nous allons ajouter la méthode suivante au HomeController qui interroge notre base de données pour trouver les meilleurs albums de ventes en fonction de OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Il s’agit d’une méthode privée, étant donné que nous ne voulons pas pour le rendre disponible en tant qu’une action de contrôleur. Nous allons l’inclure dans le HomeController par souci de simplicité, mais il est conseillé de déplacer votre logique métier dans des classes de service distinct comme il convient.

Ceci en place, nous pouvons mettre à jour l’action du contrôleur pour interroger les 5 vendant des albums et les retourner à la vue Index.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Le code complet pour le HomeController mis à jour est comme indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Enfin, nous allons devoir mettre à jour notre vue d’accueil d’Index pour qu’il puisse afficher une liste d’albums par la mise à jour le type de modèle et l’ajout de la liste des albums vers le bas. Nous allons cette occasion pour également ajouter un titre et une section de promotion à la page.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Maintenant, lorsque nous exécutons l’application, nous verrons notre page d’accueil mises à jour avec les meilleurs albums de ventes et nos messages promotionnels.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusion

Nous avons vu que ASP.NET MVC facilite la création d’un site Web sophistiqué avec un accès de base de données, l’appartenance, AJAX, etc. assez rapidement. J’espère que ce didacticiel vous a donné les outils que vous avez besoin pour commencer à créer votre propre MVC ASP.NET applications !


> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-9.md)
