---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Partie 6 : Création de produit et les contrôleurs d’ordre | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830968"
---
<a name="part-6-creating-product-and-order-controllers"></a>Partie 6 : Création d’un produit et les contrôleurs de commande
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Ajouter un contrôleur de produits

Le contrôleur de l’administrateur est pour les utilisateurs disposant des privilèges d’administrateur. Les clients, quant à eux, peuvent afficher les produits, mais ne peut pas créer, mettre à jour ou supprimez-les.

Nous pouvons facilement restreindre l’accès aux méthodes Post, Put et Delete, en laissant les méthodes Get ouvert. Mais, examiner les données retournées pour un produit :

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

Le `ActualCost` propriété ne doit pas être visible par les clients ! La solution consiste à définir un *objet de transfert de données* (DTO) qui inclut un sous-ensemble de propriétés qui doivent être visibles aux clients. Nous allons utiliser LINQ pour projet `Product` instances à `ProductDTO` instances.

Ajoutez une classe nommée `ProductDTO` dans le dossier Modèles.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Ajoutez maintenant le contrôleur. Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs. Sélectionnez **ajouter**, puis sélectionnez **contrôleur**. Dans le **ajouter un contrôleur** boîte de dialogue, nommez le contrôleur &quot;ProductsController&quot;. Sous **modèle**, sélectionnez **contrôleur d’API vide**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Remplacez tous les éléments dans le fichier source avec le code suivant :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Le contrôleur utilise toujours le `OrdersContext` pour interroger la base de données. Mais au lieu de retourner `Product` instances directement, nous appelons `MapProducts` pour projeter les sur `ProductDTO` instances :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

Le `MapProducts` méthode retourne un **IQueryable**, de sorte que nous pouvons composer le résultat avec d’autres paramètres de requête. Vous pouvez le constater dans le `GetProduct` (méthode), qui ajoute un **où** clause à la requête :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Ajouter un contrôleur de commandes

Ensuite, ajoutez un contrôleur qui permet aux utilisateurs de créer et afficher les commandes.

Nous allons commencer par un autre objet DTO. Dans l’Explorateur de solutions, cliquez sur le dossier Models et ajoutez une classe nommée `OrderDTO` utiliser l’implémentation suivante :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Ajoutez maintenant le contrôleur. Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs. Sélectionnez **ajouter**, puis sélectionnez **contrôleur**. Dans le **ajouter un contrôleur** boîte de dialogue, définissez les options suivantes :

- Sous **nom du contrôleur**, entrez « OrdersController ».
- Sous **modèle**, sélectionnez « Contrôleur d’API avec actions en lecture/écriture, à l’aide d’Entity Framework ».
- Sous **classe de modèle**, sélectionnez &quot;ordre (ProductStore.Models)&quot;.
- Sous **classe de contexte de données**, sélectionnez &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Cliquez sur **Ajouter**. Cette opération ajoute un fichier nommé OrdersController.cs. Ensuite, nous devons modifier l’implémentation par défaut du contrôleur.

Tout d’abord, supprimez le `PutOrder` et `DeleteOrder` méthodes. Pour cet exemple, les clients ne peuvent pas modifier ou supprimer des commandes existantes. Dans une application réelle, vous devez beaucoup de logique back-end pour gérer ces cas. (Par exemple, l’ordre déjà expédié ?)

Modifier la `GetOrders` méthode pour retourner uniquement les commandes qui appartiennent à l’utilisateur :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Modifier la `GetOrder` méthode comme suit :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Voici les modifications que nous avons apportées à la méthode :

- La valeur de retour est un `OrderDTO` instance, au lieu d’un `Order`.
- Lorsque nous interroger la base de données pour la commande, nous utilisons le [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) méthode pour extraire les informations connexes `OrderDetail` et `Product` entités.
- Nous aplatir le résultat à l’aide d’une projection.

La réponse HTTP contient un tableau de produits avec des quantités :

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Ce format est plus facile pour les clients à utiliser que le graphique d’objet d’origine, qui contient des entités imbriquées (ordre, les détails et produits).

La dernière méthode de prendre en compte `PostOrder`. Pour l’instant, cette méthode prend un `Order` instance. Mais que se passe-t-il si un client envoie un corps de demande comme suit :

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Il s’agit d’un ordre bien structuré et Entity Framework est heureusement insérer dans la base de données. Mais il contient une entité de produit qui n’existait pas précédemment. Le client venez de créer un nouveau produit dans notre base de données ! Il s’agit une surprise pour le département d’imprimerie ordre lorsqu’ils voient une commande pour koala ours. Est la morale, soyez très prudent sur les données que vous acceptez une demande POST ou PUT.

Pour éviter ce problème, modifiez le `PostOrder` méthode entrent en un `OrderDTO` instance. Utilisez le `OrderDTO` pour créer le `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Notez que nous utilisons le `ProductID` et `Quantity` propriétés et nous ignorer toutes les valeurs envoyés par le client pour le nom de produit ou de prix. Si l’ID de produit n’est pas valide, il enfreint la contrainte de clé étrangère dans la base de données, et l’insertion échoue, comme il le devrait.

Voici l’ensemble `PostOrder` méthode :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Enfin, ajoutez le **Authorize** attribut au contrôleur :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Maintenant, seuls les utilisateurs inscrits peuvent créer ou afficher les commandes.

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-5.md)
> [Suivant](using-web-api-with-entity-framework-part-7.md)
