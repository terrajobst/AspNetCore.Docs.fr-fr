---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relations d’entité dans OData v4 à l’aide d’ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: 'La plupart des jeux de données définissent les relations entre des entités : les clients ont passé des commandes ; la documentation ont auteurs ; produits ont des fournisseurs. L’utilisation d’OData, les clients peuvent les parcourir...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d07ddab83462ee1bc84ba8ab15fe906937f506e6
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795390"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relations d’entité dans OData v4 à l’aide d’ASP.NET Web API 2.2
====================
par [Mike Wasson](https://github.com/MikeWasson)

> La plupart des jeux de données définissent les relations entre des entités : les clients ont passé des commandes ; la documentation ont auteurs ; produits ont des fournisseurs. L’utilisation d’OData, les clients peuvent naviguer sur les relations d’entité. Étant donné un produit, vous pouvez trouver le fournisseur. Vous pouvez également créer ou supprimer des relations. Par exemple, vous pouvez définir le fournisseur pour un produit.
>
> Ce didacticiel montre comment prendre en charge ces opérations dans OData v4 à l’aide d’API Web ASP.NET. Le didacticiel s’appuie sur le didacticiel [créer une Using ASP.NET Web API 2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
> - Web API 2.1
> - OData v4
> - Visual Studio 2013 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versions de didacticiels
>
> Pour la Version 3 d’OData, consultez [prenant en charge les Relations d’entité dans OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).

## <a name="add-a-supplier-entity"></a>Ajouter une entité de fournisseur

> [!NOTE]
> Le didacticiel s’appuie sur le didacticiel [créer une Using ASP.NET Web API 2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md).

Tout d’abord, nous avons besoin d’une entité associée. Ajoutez une classe nommée `Supplier` dans le dossier Models.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Ajoutez une propriété de navigation à la `Product` classe :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Ajouter un nouveau **DbSet** à la `ProductsContext` classe, afin que Entity Framework inclura la table de fournisseur dans la base de données.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

Dans WebApiConfig.cs, ajoutez un &quot;fournisseurs&quot; du jeu d’entités à l’entity data model :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Ajouter un contrôleur de fournisseurs

Ajouter un `SuppliersController` classe dans le dossier contrôleurs.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Je ne montrent comment ajouter des opérations CRUD pour ce contrôleur. Les étapes sont les mêmes que pour le contrôleur de produits (consultez [créer un point de terminaison OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Obtention des entités associées

Pour obtenir le fournisseur pour un produit, le client envoie une demande GET :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Pour prendre en charge de cette demande, ajoutez la méthode suivante à la `ProductsController` classe :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Cette méthode utilise une convention d’affectation de noms par défaut

- Nom de la méthode : GetX, où X est la propriété de navigation.
- Nom du paramètre : *clé*

Si vous suivez cette convention d’affectation de noms, les API Web mappe automatiquement la requête HTTP à la méthode de contrôleur.

Exemple HTTP de demande :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Exemple de réponse HTTP :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Obtention d’une collection connexe

Dans l’exemple précédent, un produit est associé à un seul fournisseur. Une propriété de navigation peut également retourner une collection. Le code suivant obtient les produits pour un fournisseur :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

Dans ce cas, la méthode retourne un **IQueryable** au lieu d’un **SingleResult&lt;T&gt;**

Exemple HTTP de demande :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Exemple de réponse HTTP :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Création d’une relation entre des entités

OData prend en charge la création ou la suppression des relations entre deux entités existantes. Dans la terminologie d’OData v4, la relation est une &quot;référence&quot;. (Dans OData v3, la relation a été appelée une *lien*. Les différences de protocole n’a pas d’importance pour ce didacticiel.)

Une référence a son propre URI, le formulaire `/Entity/NavigationProperty/$ref`. Par exemple, voici l’URI pour résoudre la référence entre un produit et son fournisseur :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Pour ajouter une relation, le client envoie une demande POST ou PUT à cette adresse.

- Si la propriété de navigation est une entité unique, tel que `Product.Supplier`.
- VALIDER si la propriété de navigation est une collection, tel que `Supplier.Products`.

Le corps de la requête contient l’URI de l’autre entité dans la relation. Voici un exemple de demande :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

Dans cet exemple, le client envoie une demande PUT pour `/Products(6)/Supplier/$ref`, qui est l’URI $ref pour la `Supplier` du produit avec l’ID = 6. Si la demande aboutit, le serveur envoie une réponse 204 (aucun contenu) :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Voici la méthode de contrôleur pour ajouter une relation à un `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

Le *navigationProperty* paramètre spécifie quant à la relation à définir. (S’il existe plus d’une propriété de navigation sur l’entité, vous pouvez ajouter d’autres `case` instructions.)

Le *lien* paramètre contient l’URI du fournisseur. API Web analyse automatiquement le corps de la requête pour obtenir la valeur de ce paramètre.

Pour rechercher le fournisseur, nous avons besoin de l’ID (ou clé), qui fait partie de la *lien* paramètre. Pour ce faire, utilisez la méthode d’assistance suivante :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

En fait, cette méthode utilise la bibliothèque OData pour fractionner le chemin d’accès URI en segments, recherchez le segment qui contient la clé et convertir la clé en type correct.

## <a name="deleting-a-relationship-between-entities"></a>Suppression d’une relation entre des entités

Pour supprimer une relation, le client envoie une requête HTTP DELETE à l’URI de $ref :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Voici la méthode de contrôleur pour supprimer la relation entre un produit et un fournisseur :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

Dans ce cas, `Product.Supplier` est la &quot;1&quot; fin d’une relation 1-à-plusieurs, afin de pouvoir supprimer la relation en définissant simplement `Product.Supplier` à `null`.

Dans le &quot;nombreux&quot; fin d’une relation, le client doit spécifier quels connexes d’entité à supprimer. Pour ce faire, le client envoie l’URI de l’entité associée dans la chaîne de requête de la demande. Par exemple, pour supprimer « Product 1 » de « fournisseur 1 » :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Pour ce faire dans l’API Web, nous devons inclure un paramètre supplémentaire dans le `DeleteRef` (méthode). Voici la méthode de contrôleur pour supprimer un produit à partir de la `Supplier.Products` relation.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

Le *clé* paramètre est la clé pour le fournisseur et le *relatedKey* paramètre est la clé du produit à supprimer de la `Products` relation. Notez que les API Web obtient automatiquement la clé de la chaîne de requête.
