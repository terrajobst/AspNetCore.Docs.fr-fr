---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Créer des objets de transfert de données (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 95075dd748f0fe4eb6d1c52d6bfe4a4576653b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833419"
---
<a name="create-data-transfer-objects-dtos"></a>Créer des objets de transfert de données (DTO)
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Droit à présent, notre API web expose les entités de base de données au client. Le client reçoit des données est directement mappé à votre base de données. Toutefois, qui n’est pas toujours une bonne idée. Parfois, vous souhaitez modifier la forme des données que vous envoyez au client. Vous pouvez, par exemple, décider d'effectuer les opérations suivantes :

- Supprimez les références circulaires (voir la section précédente).
- Masquer des propriétés particulières qui les clients ne sont pas censé pour afficher.
- Omettre certaines propriétés afin de réduire la taille de charge utile.
- Aplatir les graphiques d’objets qui contiennent des objets imbriqués, pour les rendre plus pratique pour les clients.
- Évitez « les vulnérabilités de survalidation ». (Consultez [Validation du modèle](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) pour une présentation de survalidation.)
- Découpler votre couche de service à partir de votre couche de base de données.

Pour ce faire, vous pouvez définir un *objet de transfert de données* (DTO). Un objet DTO est un objet qui définit comment les données seront être envoyées sur le réseau. Nous allons voir comment cela fonctionne avec l’entité de livre. Dans le dossier Models, ajoutez deux classes DTO :

[!code-csharp[Main](part-5/samples/sample1.cs)]

Le `BookDetailDTO` classe inclut toutes les propriétés du modèle de livre, à ceci près que `AuthorName` est une chaîne qui contienne le nom de l’auteur. Le `BookDTO` classe contient un sous-ensemble de propriétés à partir de `BookDetailDTO`.

Ensuite, remplacez les deux méthodes GET dans le `BooksController` (classe), avec les versions qui retournent des objets DTO. Nous allons utiliser LINQ **sélectionnez** instruction pour convertir les entités livre en objets DTO.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Voici le code SQL généré par le nouveau `GetBooks` (méthode). Vous pouvez voir qu’Entity Framework traduit la LINQ **sélectionnez** dans une instruction SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Enfin, modifiez le `PostBook` méthode pour retourner un objet DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> Dans ce didacticiel, nous convertissons à DTO manuellement dans le code. Une autre option consiste à utiliser une bibliothèque comme [AutoMapper](http://automapper.org/) qui gère la conversion automatiquement.
> 
> [!div class="step-by-step"]
> [Précédent](part-4.md)
> [Suivant](part-6.md)
