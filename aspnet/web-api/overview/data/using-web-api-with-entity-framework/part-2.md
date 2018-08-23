---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Ajouter des modèles et des contrôleurs | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 162ef2cd4ba11040e1bc938617a36495489ba5bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831817"
---
<a name="add-models-and-controllers"></a>Ajouter des modèles et des contrôleurs
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez ajouter des classes de modèle qui définissent les entités de base de données. Ensuite, vous allez ajouter des contrôleurs d’API Web qui effectuent des opérations CRUD sur ces entités.

## <a name="add-model-classes"></a>Ajouter des Classes de modèle

Dans ce didacticiel, nous allons créer la base de données à l’aide de l’approche « Code First » à Entity Framework (EF). Avec Code First, vous écrivez des classes c# qui correspondent aux tables de base de données et Entity Framework crée la base de données. (Pour plus d’informations, consultez [approches de développement Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Commencez par définir nos objets de domaine en tant qu’Oct (plain-objets CLR). Nous allons créer les objets oct suivantes :

- Auteur
- Livre

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier de modèles. Sélectionnez **ajouter**, puis sélectionnez **classe**. Nommez la classe `Author`.

![](part-2/_static/image1.png)

Remplacez tout le code réutilisable dans Author.cs par le code suivant.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Ajouter une autre classe nommée `Book`, avec le code suivant.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework utilisera ces modèles pour créer des tables de base de données. Pour chaque modèle, le `Id` propriété devient la colonne de clé primaire de la table de base de données.

Dans la classe Book, la `AuthorId` définit une clé étrangère dans la `Author` table. (Par souci de simplicité, je suppose que chaque livre possède un auteur unique.) La classe de livre contient également une propriété de navigation connexe `Author`. Vous pouvez utiliser la propriété de navigation pour accéder aux connexe `Author` dans le code. Je dis plus sur les propriétés de navigation dans la partie 4, [gestion des Relations d’entité](part-4.md).

## <a name="add-web-api-controllers"></a>Ajouter des contrôleurs d’API Web

Dans cette section, nous allons ajouter des contrôleurs d’API Web qui prennent en charge les opérations CRUD (créer, lire, mettre à jour et supprimer). Les contrôleurs utilisera Entity Framework pour communiquer avec la couche de base de données.

Tout d’abord, vous pouvez supprimer le fichier Controllers/ValuesController.cs. Ce fichier contient un exemple de contrôleur d’API Web, mais vous n’en avez pas besoin pour ce didacticiel.

![](part-2/_static/image2.png)

Ensuite, générez le projet. La structure de l’API Web utilise la réflexion pour trouver les classes de modèle, par conséquent, il doit l’assembly compilé.

Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs. Sélectionnez **ajouter**, puis sélectionnez **contrôleur**.

![](part-2/_static/image3.png)

Dans le **ajouter une structure** boîte de dialogue, sélectionnez « Web API 2 contrôleur avec actions, utilisant Entity Framework ». Cliquez sur **Ajouter**.

![](part-2/_static/image4.png)

Dans le **ajouter un contrôleur** boîte de dialogue, procédez comme suit :

1. Dans le **classe de modèle** liste déroulante, sélectionnez le `Author` classe. (Si vous ne voyez pas répertoriés dans la liste déroulante, assurez-vous que vous avez créé le projet.)
2. Consultez « Utiliser des actions de contrôleur asynchrones ».
3. Laissez le nom du contrôleur en tant que &quot;AuthorsController&quot;.
4. Cliquez sur plus (+) situé en regard **classe de contexte de données**.

![](part-2/_static/image5.png)

Dans le **nouveau contexte de données** boîte de dialogue, laissez le nom par défaut et cliquez sur **ajouter**.

![](part-2/_static/image6.png)

Cliquez sur **ajouter** pour terminer la **ajouter un contrôleur** boîte de dialogue. La boîte de dialogue ajoute deux classes à votre projet :

- `AuthorsController` définit un contrôleur d’API Web. Le contrôleur implémente l’API REST que les clients utilisent pour effectuer des opérations CRUD sur la liste des auteurs.
- `BookServiceContext` gère des objets d’entité pendant l’exécution, ce qui inclut le remplissage des objets avec des données à partir d’une base de données, le suivi des modifications et la conservation des données pour la base de données. Il hérite de `DbContext`.

![](part-2/_static/image7.png)

À ce stade, regénérez le projet. Accédez maintenant à travers les mêmes étapes pour ajouter un contrôleur d’API pour `Book` entités. Cette fois-ci, sélectionnez `Book` pour la classe de modèle, puis sélectionnez existant `BookServiceContext` classe pour la classe de contexte de données. (Ne créez pas un nouveau contexte de données). Cliquez sur **ajouter** pour ajouter le contrôleur.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Précédent](part-1.md)
> [Suivant](part-3.md)
