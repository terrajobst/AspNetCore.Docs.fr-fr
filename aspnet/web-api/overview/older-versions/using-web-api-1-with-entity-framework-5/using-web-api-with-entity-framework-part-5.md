---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Partie 5 : Création d’une interface utilisateur dynamique avec Knockout.js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: d019941700992e173a5812b11b558b6726dfd809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832570"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Partie 5 : Création d’une interface utilisateur dynamique avec Knockout.js
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Création d’une interface utilisateur dynamique avec Knockout.js

Dans cette section, nous allons utiliser Knockout.js pour ajouter des fonctionnalités à la vue de l’administrateur.

[Knockout.js](http://knockoutjs.com/) est une bibliothèque Javascript qui facilite la liaison de contrôles HTML aux données. Knockout.js utilise le modèle Model-View-ViewModel (MVVM).

- Le *modèle* est la représentation sous forme de côté serveur des données dans le domaine d’entreprise (dans notre cas, les produits et les commandes).
- Le *vue* est la couche de présentation (HTML).
- Le *modèle de vue* est un objet Javascript qui contient les données de modèle. Le modèle de vue est une abstraction de code de l’interface utilisateur. Elle n’a aucune connaissance de la représentation HTML. Au lieu de cela, il représente les fonctionnalités abstraites de la vue, telles que « il s’agit d’une liste d’éléments ».

La vue est lié aux données au modèle de vue. Mises à jour le modèle de vue sont automatiquement reflétées dans la vue. Le modèle de vue également Obtient les événements à partir de la vue, telles que des clics de bouton et effectue des opérations sur le modèle, telles que la création d’une commande.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Tout d’abord, nous allons définir le modèle de vue. Après cela, nous liera le balisage HTML pour le modèle de vue.

Ajoutez la section suivante de Razor à Admin.cshtml :

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Vous pouvez ajouter cette section n’importe où dans le fichier. Lorsque la vue est restituée, la section apparaît au bas de la page HTML, avec le bouton droit avant de fermer le &lt;/corps&gt; balise.

Tous le script pour cette page passera à l’intérieur de la balise de script indiquée par le commentaire :

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Tout d’abord, définissez une classe de modèle de vue :

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** est un type spécial d’objet dans Knockout, appelé un *observable*. À partir de la [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): un observable est un « objet JavaScript qui peut avertir les abonnés sur les modifications apportées. » Lorsque le contenu d’un observable change, la vue est automatiquement mis à jour.

Pour remplir le `products` de tableau, d’effectuer une requête AJAX à l’API web. Rappelez-vous que nous avons stocké l’URI de base pour l’API dans le sac d’affichage (voir [partie 4](using-web-api-with-entity-framework-part-4.md) du didacticiel).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Ensuite, ajoutez des fonctions pour le modèle de vue à créer, mettre à jour et supprimer des produits. Ces fonctions soumettre les appels AJAX à l’API web et utilisent les résultats pour mettre à jour le modèle de vue.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Maintenant la partie la plus importante : lorsque le DOM est fulled appel chargé, le **ko.applyBindings** de fonction et le passer à une nouvelle instance de la `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

Le **ko.applyBindings** méthode active Knockout et associe le modèle de vue à la vue.

Maintenant que nous avons un modèle de vue, nous pouvons créer les liaisons. Dans Knockout.js, vous faire en ajoutant `data-bind` attributs aux éléments HTML. Par exemple, pour lier une liste HTML à un tableau, utilisez le `foreach` liaison :

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

Le `foreach` liaison effectue une itération dans le tableau et crée des éléments pour chaque objet enfant dans le tableau. Liaisons sur les éléments enfants peuvent faire référence aux propriétés sur les objets du tableau.

Ajoutez les liaisons suivantes à la liste « produits de mise à jour » :

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

Le `<li>` élément se produit dans l’étendue de la **foreach** liaison. Que signifie sera de Knockout restitue l’élément une fois pour chaque produit dans le `products` tableau. Toutes les liaisons dans le `<li>` élément font référence à cette instance de produit. Par exemple, `$data.Name` fait référence à la `Name` propriété sur le produit.

Pour définir les valeurs des entrées de texte, utilisez le `value` liaison. Les boutons sont liés aux fonctions sur la vue de modèle, à l’aide de la `click` liaison. L’instance de produit est passé en tant que paramètre à chaque fonction. Pour plus d’informations, le [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) a bonne descriptions des liaisons différentes.

Ensuite, ajoutez une liaison pour le **soumettre** événement sur le formulaire Ajouter un produit :

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Cette liaison appelle le `create` fonction sur le modèle de vue pour créer un nouveau produit.

Voici le code complet pour l’affichage de l’administrateur :

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Exécutez l’application, connectez-vous avec le compte d’administrateur, cliquez sur le lien « Admin ». Vous devez voir la liste des produits et être en mesure de créer, mettre à jour ou supprimer des produits.

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-4.md)
> [Suivant](using-web-api-with-entity-framework-part-6.md)
