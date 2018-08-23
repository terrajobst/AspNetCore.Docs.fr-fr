---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Partie 8 : Panier d’achat avec des mises à jour Ajax | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 8 couvre le panier d’achat avec des mises à jour Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: cab338e56505c453532a26d794eb7bf4e94555a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831776"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Partie 8 : Panier d’achat avec des mises à jour Ajax
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.  
>   
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 8 couvre le panier d’achat avec des mises à jour Ajax.


Nous allons permettre aux utilisateurs de placer des albums dans leur panier d’achat sans l’enregistrer, mais ils doivent s’inscrire en tant qu’invités à l’extraction terminée. Le processus d’achat et l’extraction est divisée en deux contrôleurs : un contrôleur ShoppingCart qui permet d’ajouter des éléments anonymement à un panier d’achat et un contrôleur d’extraction qui gère le processus de validation. Nous allons commencer par le panier d’achat dans cette section, puis générez le processus de validation dans la section suivante.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Ajout de classes de modèle de panier, l’ordre et OrderDetail

Notre processus de panier d’achat et l’extraction rendra utiliser certaines nouvelles classes. Cliquez sur le dossier Modèles et ajouter une classe de panier (Cart.cs) avec le code suivant.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Cette classe est assez similaire à d’autres personnes que nous avons utilisé jusqu’ici, à l’exception de l’attribut [clé] pour la propriété RecordId. Nos articles du panier aura un identificateur de chaîne nommé CartID pour autoriser les achats anonymes, mais la table inclut une clé primaire entière nommée RecordId. Par convention, Entity Framework Code First attend que la clé primaire pour une table nommée panier sera CartId ou ID, mais nous pouvons facilement remplacer que via des annotations ou code si nous voulons. Il s’agit d’un exemple de comment nous pouvons utiliser les conventions simples dans le Code First Entity Framework lorsqu’ils sont adaptés à nous, mais nous ne sommes pas contraints par ces derniers lorsque ce n’est pas.

Ensuite, ajoutez une classe de commande (Order.cs) avec le code suivant.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Cette classe effectue le suivi des informations de résumé et la remise d’une commande. **Il n’est pas compilé encore**, car il a une propriété de navigation OrderDetails qui dépend d’une classe que nous n’avons pas encore créé. Nous allons corriger maintenant en ajoutant une classe nommée OrderDetail.cs, en ajoutant le code suivant.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Nous allons effectuer une dernière mise à jour notre classe MusicStoreEntities pour inclure les DbSets qui exposent ces nouvelles classes de modèle, également, y compris un DbSet&lt;artiste&gt;. La classe MusicStoreEntities mis à jour apparaît comme ci-dessous.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>La gestion de la logique métier de panier d’achat

Ensuite, nous allons créer la classe ShoppingCart dans le dossier Models. Le modèle ShoppingCart gère l’accès aux données à la table de panier. En outre, il gère la logique métier pour ajout et suppression d’éléments dans le panier d’achat.

Étant donné que nous ne voulons obliger les utilisateurs à s’inscrire pour un compte simplement à ajouter des éléments à leur panier d’achat, nous affecterons les utilisateurs un identificateur unique temporaire (à l’aide d’un GUID ou un identificateur global unique) lorsqu’ils accèdent au panier d’achat. Nous allons stocker cet ID à l’aide de la classe de Session ASP.NET.

*Remarque : La Session ASP.NET est un emplacement pratique pour stocker des informations spécifiques à l’utilisateur qui va expirer après qu’ils quittent le site. Tandis que l’utilisation incorrecte de l’état de session peut avoir des implications en matière de performances sur les sites de grande taille, notre utilisation claire fonctionnera bien à des fins de démonstration.*

La classe ShoppingCart expose les méthodes suivantes :

**AddToCart** prend un Album en tant que paramètre et l’ajoute au panier de l’utilisateur. Étant donné que la table de panier effectue le suivi de la quantité pour chaque album, il inclut une logique pour créer une nouvelle ligne, si nécessaire ou incrémenter seulement la quantité si l’utilisateur a déjà commandé une seule copie de l’album.

**RemoveFromCart** prend un ID de l’Album et le supprime de panier de l’utilisateur. Si l’utilisateur avait uniquement une seule copie de l’album dans leur panier d’achat, la ligne est supprimée.

**EmptyCart** supprime tous les éléments de panier d’achat d’un utilisateur.

**GetCartItems** récupère une liste de CartItems pour l’affichage ou de traitement.

**GetCount** récupère un le nombre total d’albums d’un utilisateur possède dans leur panier d’achat.

**GetTotal** calcule le coût total de tous les éléments dans le panier d’achat.

**CreateOrder** convertit le panier d’achat à une commande pendant la phase d’extraction.

**GetCart** est une méthode statique qui permet à nos contrôleurs obtenir un objet panier. Il utilise le **GetCartId** méthode pour gérer la lecture de la CartId à partir de la session utilisateur. La méthode GetCartId requiert HttpContextBase afin qu’il puisse lire CartId de l’utilisateur à partir de la session de l’utilisateur.

Voici l’ensemble **ShoppingCart classe**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Notre contrôleur de panier d’achat doit communiquer des informations complexes pour ses vues qui ne correspond pas parfaitement à nos objets de modèle. Nous ne voulons pas à modifier nos modèles pour l’adapter à nos vues ; Classes de modèle doivent représenter notre domaine, pas l’interface utilisateur. Une solution consisterait à passer des informations à nos vues à l’aide de la classe ViewBag, comme nous l’avons fait avec les informations de la liste déroulante Store Manager, mais en passant un grand nombre d’informations via ViewBag obtient difficile à gérer.

Une solution consiste à utiliser le *ViewModel* modèle. Lorsque vous utilisez ce modèle, que nous créons des classes fortement typées qui sont optimisés pour nos scénarios vue spécifique, et qui exposent des propriétés pour le valeurs/le contenu dynamique requises par nos modèles de vue. Nos classes de contrôleur peuvent remplir, puis transmettre ces classes d’affichage optimisé à notre modèle de vue à utiliser. Cela permet une sécurité de type, la vérification au moment de la compilation et éditeur IntelliSense dans les modèles de vue.

Nous allons créer deux modèles de vue pour une utilisation dans notre contrôleur de panier d’achat : le ShoppingCartViewModel contiendra le contenu du panier d’achat de l’utilisateur et le ShoppingCartRemoveViewModel doit être utilisée pour afficher les informations de confirmation lorsqu’un utilisateur supprime un élément à partir de leur panier d’achat.

Nous allons créer un nouveau dossier ViewModels à la racine de notre projet pour organiser les éléments. Cliquez sur le projet, sélectionnez Ajouter / nouveau dossier.

![](mvc-music-store-part-8/_static/image1.jpg)

Nommez le dossier ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

Ensuite, ajoutez la classe ShoppingCartViewModel dans le dossier ViewModels. Il a deux propriétés : une liste des éléments du panier et une valeur décimale pour contenir le prix total de tous les éléments dans le panier d’achat.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Ajoutez maintenant le ShoppingCartRemoveViewModel au dossier ViewModels, avec les quatre propriétés suivantes.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Le contrôleur de panier d’achat

Le contrôleur de panier d’achat a trois objectifs principaux : ajout d’éléments à un panier, supprimer des éléments du panier et afficher les éléments dans le panier d’achat. Il utilisera les trois classes nous venons de créer : ShoppingCartViewModel, ShoppingCartRemoveViewModel et ShoppingCart. Comme dans les StoreController et le StoreManagerController, nous allons ajouter un champ pour stocker une instance de MusicStoreEntities.

Ajoutez un nouveau contrôleur de panier d’achat pour le projet en utilisant le modèle de contrôleur vide.

![](mvc-music-store-part-8/_static/image2.png)

Voici le contrôleur ShoppingCart terminée. Les actions d’Index et d’ajouter un contrôleur doivent sembler très familière. Les actions de contrôleur Remove et CartSummary traiter les deux cas spéciaux, ce que nous aborderons dans la section suivante.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Mises à jour AJAX avec jQuery

Nous allons ensuite créer une page d’Index de panier d’achat qui est fortement typée à le ShoppingCartViewModel et utilise le modèle de vue de liste à l’aide de la même méthode comme avant.

![](mvc-music-store-part-8/_static/image3.png)

Toutefois, au lieu d’utiliser un Html.ActionLink pour supprimer des éléments du panier, nous allons utiliser jQuery pour « associer » l’événement click pour tous les liens dans cette vue ayant la classe HTML RemoveLink. Au lieu de renvoyer le formulaire, ce gestionnaire d’événements click fera simplement un rappel AJAX à notre action de contrôleur RemoveFromCart. Le RemoveFromCart retourne un résultat sérialisé au format JSON, qui notre rappel jQuery, puis analyse et effectue quatre mises à jour rapides à la page à l’aide de jQuery :

- 1. Supprime l’album supprimé de la liste
- 2. Met à jour le nombre de panier dans l’en-tête
- 3. Affiche un message de mise à jour à l’utilisateur
- 4. Met à jour le prix total du panier

Étant donné que le scénario de suppression est traité par un rappel Ajax au sein de la vue Index, nous n’avons besoin une vue supplémentaire pour l’action de RemoveFromCart. Voici le code complet pour la vue /ShoppingCart/Index :

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Pour tester, que nous devons être en mesure d’ajouter des éléments à notre panier d’achat. Nous mettrons à jour notre **Store détails** vue et inclure un bouton « Ajouter au panier ». Pendant que nous sommes à elle, nous pouvons inclure certaines informations supplémentaires Album dont nous avons ajouté dans la mesure où nous avons mis à jour cette vue : Genre, artiste, prix et pochette d’Album. Le code de vue Store détails mis à jour s’affiche comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Nous pouvons maintenant Cliquez sur via le magasin et tester l’ajout et la suppression des Albums vers et à partir de notre panier d’achat. Exécutez l’application et accédez à l’Index de Store.

![](mvc-music-store-part-8/_static/image4.png)

Ensuite, cliquez sur un Genre pour afficher une liste des albums.

![](mvc-music-store-part-8/_static/image5.png)

En cliquant sur un titre d’Album maintenant montre notre vue Détails de l’Album mis à jour, y compris le bouton « Ajouter au panier ».

![](mvc-music-store-part-8/_static/image6.png)

En cliquant sur le bouton « Ajouter au panier » montre notre vue Index de panier d’achat avec la liste de résumé de panier d’achat.

![](mvc-music-store-part-8/_static/image7.png)

Après avoir chargé votre panier d’achat, vous pouvez cliquer sur la supprimer à partir du lien du panier pour voir la mise à jour Ajax à votre panier d’achat.

![](mvc-music-store-part-8/_static/image8.png)

Nous avons développé un travail panier qui permet à des utilisateurs non inscrits à ajouter des éléments à leur panier d’achat. Dans la section suivante, nous allons pouvoir inscrire et terminer le processus de validation.


> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-7.md)
> [Suivant](mvc-music-store-part-9.md)
