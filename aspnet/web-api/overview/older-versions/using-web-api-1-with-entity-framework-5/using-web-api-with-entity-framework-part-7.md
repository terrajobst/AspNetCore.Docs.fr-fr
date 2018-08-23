---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Partie 7 : Création de la Main Page | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb4704e7f4f13fab04acdbdd642174884517e18a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825945"
---
<a name="part-7-creating-the-main-page"></a>Partie 7 : Création de la Main Page
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Création de la Main Page

Dans cette section, vous allez créer la page principale de l’application. Cette page sera plus complexe que la page d’administration, nous allons l’approche en plusieurs étapes. Tout au long du processus, vous verrez certaines techniques plus avancées de Knockout.js. Voici la disposition de base de la page :

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- « Produits » contient un tableau de produits.
- « Panier » conserve un tableau de produits avec des quantités. En cliquant sur « Add to Cart » met à jour le panier d’achat.
- « Orders » contient un tableau d’ID de commande.
- « Détails » contient un détail de commande, qui est un tableau d’éléments (produits avec des quantités)

Nous allons commencer par définir une disposition de base en HTML, sans liaison de données ou d’un script. Ouvrez le fichier Views/Home/Index.cshtml et remplacez tout le contenu avec les éléments suivants :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Ensuite, ajoutez une section de Scripts et créer un modèle de vue vide :

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Selon la conception décrit plus haut, notre modèle de vue doit observables pour les produits, le panier, commandes et détails. Ajoutez les variables suivantes pour le `AppViewModel` objet :

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Les utilisateurs peuvent ajouter des éléments dans la liste de produits dans le panier et supprimer des éléments de panier. Pour encapsuler ces fonctions, nous allons créer une autre classe de modèle de vue qui représente un produit. Ajoutez le code suivant à `AppViewModel` :

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

Le `ProductViewModel` classe contient deux fonctions qui sont utilisées pour déplacer le produit vers et depuis le panier : `addItemToCart` ajoute une unité du produit au panier d’achat, et `removeAllFromCart` supprime toutes les quantités du produit.

Les utilisateurs peuvent sélectionner une commande existante et obtenir les détails de commande. Nous allons encapsuler cette fonctionnalité dans un autre modèle de vue :

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

Le `OrderDetailsViewModel` est initialisé avec une commande, et il extrait les détails de commande en envoyant une requête AJAX au serveur.

En outre, notez le `total` propriété sur le `OrderDetailsViewModel`. Cette propriété est un type spécial d’observable appelé un [calculée observable](http://knockoutjs.com/documentation/computedObservables.html). Comme son nom l’indique, un observable calculée vous permet de lier les données à une valeur calculée&#8212;dans ce cas, le coût total de l’ordre.

Ensuite, ajoutez ces fonctions à `AppViewModel`:

- `resetCart` Supprime tous les éléments du panier.
- `getDetails` Obtient les détails d’une commande (par pusing un nouveau `OrderDetailsViewModel` sur la `details` liste).
- `createOrder` Crée un nouvel ordre et vide le panier d’achat.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Enfin, initialisez le modèle de vue en effectuant des demandes AJAX pour les produits et les commandes :

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK, cela représente beaucoup de code, mais nous avons pas à pas, donc j’espère que la conception est clair. Maintenant, nous pouvons ajouter certaines liaisons Knockout.js dans le code HTML.

**Produits**

Voici les liaisons pour la liste des produits :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Il effectue une itération sur le tableau de produits et affiche le nom et le prix. Le bouton « Ajouter à la commande » est visible uniquement lorsque l’utilisateur est connecté.

Les appels de bouton « Ajouter à la commande » `addItemToCart` sur la `ProductViewModel` instance pour le produit. Cela montre une fonctionnalité intéressante de Knockout.js : lorsqu’un modèle de vue contient d’autres modèles de vue, vous pouvez appliquer ces liaisons au modèle interne. Dans cet exemple, les liaisons dans le `foreach` sont appliquées à chaque le `ProductViewModel` instances. Cette approche est beaucoup plus claire que placer toutes les fonctionnalités dans un modèle de vue unique.

**Panier**

Voici les liaisons pour le panier d’achat :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Il effectue une itération sur le tableau de panier et affiche le nom, le prix et la quantité. Notez que le lien « Supprimer » et le bouton « Créer une commande » sont liés aux fonctions de modèle de vue.

**Commandes**

Voici les liaisons pour obtenir la liste de commandes :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Il effectue une itération sur les commandes et qui affiche l’ID de commande. L’événement de clic sur le lien est lié à la `getDetails` (fonction).

**Détails de la commande**

Voici les liaisons pour les détails de commande :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Il effectue une itération sur les éléments dans l’ordre et affiche le produit, le prix et la quantité. La balise div environnante est visible uniquement si le tableau de détails contient un ou plusieurs éléments.

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez créé une application qui utilise Entity Framework pour communiquer avec la base de données et ASP.NET Web API pour fournir une interface publique sur la couche de données. ASP.NET MVC 4 nous permettent de restituer les pages HTML et Knockout.js ainsi que jQuery pour fournir des interactions sans rechargements de page dynamiques.

Ressources supplémentaires :

- [ASP.NET Data Access Content Map](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centre de développement Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-6.md)
