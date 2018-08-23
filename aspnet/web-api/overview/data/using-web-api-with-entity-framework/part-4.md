---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Gestion des Relations d’entité | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 5d05a2e5d4380a15078317545325bd20fde3f83c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826575"
---
<a name="handling-entity-relations"></a>Gestion des Relations d’entité
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Cette section décrit certains détails de comment Entity Framework charge les entités associées et comment gérer les propriétés de navigation circulaire dans vos classes de modèle. (Cette section fournit des connaissances générales et n’est pas nécessaire pour suivre le didacticiel. Si vous préférez, passez à [partie 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Chargement et le chargement différé hâtif

Lorsque vous utilisez EF avec une base de données relationnelle, il est important de comprendre comment Entity Framework charge les données associées.

Il est également utile de visualiser les requêtes SQL généré par EF. Pour tracer le code SQL, ajoutez la ligne suivante de code pour le `BookServiceContext` constructeur :

[!code-csharp[Main](part-4/samples/sample1.cs)]

Si vous envoyez une demande GET à /api/books, elle retourne JSON comme suit :

[!code-console[Main](part-4/samples/sample2.cmd)]

Vous pouvez voir que la propriété Author est null, même si le livre contient un AuthorId valide. C’est parce que EF ne se charge pas les entités auteur associées. Le journal des traces de la requête SQL le confirme :

[!code-console[Main](part-4/samples/sample3.sql)]

L’instruction SELECT prend de la table de la documentation et ne fait pas référence à la table de l’auteur.

Pour référence, voici la méthode dans la `BooksController` classe qui retourne la liste de livres.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Nous allons voir comment nous pouvons renvoyer l’auteur en tant que partie des données JSON. Il existe trois façons de charger les données associées dans Entity Framework : le chargement hâtif, le chargement différé et le chargement explicite. Il existe des compromis avec chaque technique, il est donc important de comprendre comment elles fonctionnent.

### <a name="eager-loading"></a>Chargement hâtif

Avec *chargement hâtif*, EF charge les entités associées dans le cadre de la requête de base de données initiale. Pour effectuer un chargement hâtif, utilisez le **System.Data.Entity.Include** méthode d’extension.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Ce code indique à Entity Framework pour inclure les données de l’auteur dans la requête. Si vous apportez cette modification, exécutez l’application, les données JSON ressemblent maintenant à ceci :

[!code-console[Main](part-4/samples/sample6.cmd)]

Le journal des traces montre qu’EF effectuée une jointure sur les tables de livre et auteur.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Chargement différé

Lorsque le chargement différé, EF charge automatiquement une entité associée lors de la propriété de navigation pour cette entité est déréférencée. Pour activer le chargement différé, vérifiez la propriété de navigation virtuelle. Par exemple, dans la classe Book :

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Examinons à présent le code suivant :

[!code-csharp[Main](part-4/samples/sample9.cs)]

Lorsque le chargement différé est activé, l’accès à la `Author` propriété sur `books[0]` provoque EF interroger la base de données de l’auteur.

Le chargement différé requiert plusieurs allers-retours de base de données, étant donné que EF envoie une requête chaque fois qu’il récupère une entité associée. En règle générale, vous souhaitez que le chargement différé désactivé pour les objets qui vous sérialisez. Le sérialiseur doit lire toutes les propriétés sur le modèle, ce qui déclenche le chargement des entités connexes. Par exemple, voici les requêtes SQL lorsqu’EF sérialise la liste des livres avec le chargement différé est activé. Vous pouvez voir que EF effectue trois requêtes distinctes pour les auteurs de trois.

[!code-console[Main](part-4/samples/sample10.sql)]

Il existe toujours une fois lorsque vous souhaiterez utiliser le chargement différé. Le chargement hâtif peut entraîner d’EF générer une jointure très complexe. Ou vous devrez peut-être les entités associées pour un petit sous-ensemble des données, et le chargement différé serait plus efficace.

Pour éviter les problèmes de sérialisation consiste à sérialiser des objets de transfert de données (DTO) au lieu d’objets d’entité. Je vais expliquer cette approche plus loin dans l’article.

### <a name="explicit-loading"></a>Chargement explicite

Chargement explicite est similaire au chargement différé, sauf que vous obtenez explicitement les données associées dans le code ; Il ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation. Le chargement explicite vous donne davantage de contrôle sur le moment charger les données associées, mais nécessite du code supplémentaire. Pour plus d’informations sur le chargement explicite, consultez [le chargement des entités associées](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Propriétés de navigation et des références circulaires

Lorsque j’ai défini les modèles de livre et auteur, j’ai défini une propriété de navigation sur le `Book` classe pour la relation de l’auteur du livre, mais je n’ont pas défini une propriété de navigation dans l’autre direction.

Que se passe-t-il si vous ajoutez la propriété de navigation correspondant à la `Author` classe ?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Malheureusement, ceci pose un problème lorsque vous sérialisez les modèles. Si vous chargez les données associées, il crée un graphique d’objet circulaires.

![](part-4/_static/image1.png)

Lorsque le module de formatage JSON ou XML tente de sérialiser le graphique, une exception sera levée. Les deux formateurs générer des messages d’exception différent. Voici un exemple pour le formateur JSON :

[!code-console[Main](part-4/samples/sample12.cmd)]

Voici le formateur XML :

[!code-xml[Main](part-4/samples/sample13.xml)]

Une solution consiste à utiliser des DTO, que je décrirai la section suivante. Vous pouvez également configurer les formateurs JSON et XML pour gérer les cycles de graphique. Pour plus d’informations, consultez [gestion des références d’objet circulaires](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Pour ce didacticiel, vous n’avez pas besoin du `Author.Book` propriété de navigation, vous pouvez donc laisser il.

> [!div class="step-by-step"]
> [Précédent](part-3.md)
> [Suivant](part-5.md)
