---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Partie 2 : Création de modèles de domaine | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: cb98f42df411a7ba12ff4566c30ddfbf253253d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836191"
---
<a name="part-2-creating-the-domain-models"></a>Partie 2 : Création de modèles de domaine
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Ajouter des modèles

Il existe trois façons de l’approche Entity Framework :

- Base de données en premier : commencer avec une base de données et Entity Framework génère le code.
- Model first : vous démarrez avec un modèle visual et Entity Framework génère la base de données et le code.
- Code first : vous démarrez avec le code et Entity Framework génère la base de données.

Nous utilisons l’approche code first, donc nous commençons par définir nos objets de domaine en tant qu’Oct (plain-objets CLR). Avec l’approche code first, objets de domaine n’avez pas besoin tout code supplémentaire pour prendre en charge de la couche de base de données, telles que les transactions ou la persistance. (Plus précisément, ils n’avez pas besoin d’hériter de la [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) classe.) Vous pouvez toujours utiliser des annotations de données pour contrôler la façon dont Entity Framework crée le schéma de base de données.

Étant donné qu’oct ne transmettent pas de toutes les propriétés supplémentaires qui décrivent [état de base de données](https://msdn.microsoft.com/library/system.data.entitystate.aspx), ils peuvent facilement être sérialisés vers JSON ou XML. Toutefois, cela ne signifie pas vous devez toujours exposer vos modèles Entity Framework directement aux clients, comme nous le verrons plus loin dans le didacticiel.

Nous allons créer les objets oct suivantes :

- Produit
- Trier
- OrderDetail

Pour créer chaque classe, cliquez sur le dossier de modèles dans l’Explorateur de solutions. Dans le menu contextuel, sélectionnez **ajouter** , puis sélectionnez **classe.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Ajouter un `Product` classe avec l’implémentation suivante :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Par convention, Entity Framework utilise le `Id` propriété comme clé primaire et le mappe à une colonne d’identité dans la table de base de données. Lorsque vous créez un nouveau `Product` instance, vous ne définissez une valeur pour `Id`, car la base de données génère la valeur.

Le **ScaffoldColumn** attribut indique à ASP.NET MVC pour ignorer le `Id` propriété lors de la génération d’un éditeur de formulaire. Le **requis** attribut est utilisé pour valider le modèle. Il spécifie que le `Name` propriété doit être une chaîne non vide.

Ajouter la `Order` classe :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Ajouter la `OrderDetail` classe :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relations de clé étrangères

Une commande contient de nombreux détails de commande, et chaque détail de la commande fait référence à un seul produit. Pour représenter ces relations, la `OrderDetail` classe définit des propriétés nommées `OrderId` et `ProductId`. Entity Framework déduira que ces propriétés représentent les clés étrangères et ajoutera les contraintes foreign key à la base de données.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Le `Order` et `OrderDetail` classes incluent également des propriétés de « navigation », qui contiennent des références aux objets connexes. Une commande donnée, vous pouvez naviguer vers les produits dans l’ordre en suivant les propriétés de navigation.

Compilez le projet maintenant. Entity Framework utilise la réflexion pour découvrir les propriétés des modèles, de sorte qu’elle nécessite un assembly compilé créer le schéma de base de données.

## <a name="configure-the-media-type-formatters"></a>Configurer les formateurs de Type de média

Un [formateur de type de média](../../formats-and-model-binding/media-formatters.md) est un objet qui sérialise vos données lors de l’API Web écrit le corps de réponse HTTP. Les formateurs intégrés prennent en charge la sortie JSON et XML. Par défaut, les deux de ces formateurs sérialiser tous les objets par valeur.

Sérialisation par valeur crée un problème si un graphique d’objet contient des références circulaires. C’est exactement le cas avec le `Order` et `OrderDetail` des classes, étant donné que chacun conserve une référence à l’autre. Le formateur est suivez les références, écriture de chaque objet par valeur et vous accédez dans les cercles. Par conséquent, nous devons modifier le comportement par défaut.

Dans l’Explorateur de solutions, développez l’application\_dossier Démarrer et d’ouvrir le fichier WebApiConfig.cs. Ajoutez le code suivant à la classe `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Ce code définit le formateur JSON pour conserver les références d’objet et supprime le formateur XML provenant du pipeline entièrement. (Vous pouvez configurer le formateur XML pour conserver les références d’objet, mais il est un peu plus de travail, et il nous suffit de JSON pour cette application. Pour plus d’informations, consultez [gestion des références d’objet circulaires](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-1.md)
> [Suivant](using-web-api-with-entity-framework-part-3.md)
