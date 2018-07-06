---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Partie 3 : Création d’un contrôleur d’administration | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: caf7649f2e420b4063966fd1a371c3d9a5eb4d04
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815455"
---
<a name="part-3-creating-an-admin-controller"></a>Partie 3 : Création d’un contrôleur d’administration
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Ajouter un contrôleur d’administration

Dans cette section, nous allons ajouter un contrôleur d’API Web qui prend en charge CRUD (créer, lire, mettre à jour et supprimer) des opérations sur les produits. Le contrôleur utilisera Entity Framework pour communiquer avec la couche de base de données. Seuls les administrateurs seront en mesure d’utiliser ce contrôleur. Les clients auront accès les produits par un autre contrôleur.

Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs. Sélectionnez **ajouter** , puis **contrôleur**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

Dans le **ajouter un contrôleur** boîte de dialogue, nommez le contrôleur `AdminController`. Sous **modèle**, sélectionnez &quot;contrôleur d’API avec actions en lecture/écriture, à l’aide d’Entity Framework&quot;. Sous **classe de modèle**, sélectionnez « Product (ProductStore.Models) ». Sous **contexte de données**, sélectionnez «&lt;nouveau contexte de données&gt;».

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Si le **classe de modèle** liste déroulante n’affiche pas les classes de modèle, assurez-vous que vous avez compilé le projet. Entity Framework utilise la réflexion, par conséquent, il doit l’assembly compilé.


En sélectionnant «&lt;nouveau contexte de données&gt;» ouvre le **nouveau contexte de données** boîte de dialogue. Nommez le contexte de données `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Cliquez sur **OK** pour faire disparaître le **nouveau contexte de données** boîte de dialogue. Dans le **ajouter un contrôleur** boîte de dialogue, cliquez sur **ajouter**.

Voici ce qui a été ajouté au projet :

- Une classe nommée `OrdersContext` qui dérive de **DbContext**. Cette classe fournit le liant entre les modèles POCO et de la base de données.
- Un contrôleur d’API Web nommé `AdminController`. Ce contrôleur prend en charge les opérations CRUD sur `Product` instances. Il utilise le `OrdersContext` classe pour communiquer avec Entity Framework.
- Une nouvelle chaîne de connexion de base de données dans le fichier Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Ouvrez le fichier OrdersContext.cs. Notez que le constructeur spécifie le nom de la chaîne de connexion de base de données. Ce nom fait référence à la chaîne de connexion qui a été ajoutée au fichier Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Ajoutez les propriétés suivantes à la classe `OrdersContext` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Un **DbSet** représente un jeu d’entités qui peuvent être interrogées. Voici l’intégralité de la `OrdersContext` classe :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

Le `AdminController` classe définit cinq méthodes qui implémentent les fonctionnalités de CRUD de base. Chaque méthode correspond à un URI que le client peut appeler :

| Méthode de contrôleur | Description | URI | Méthode HTTP |
| --- | --- | --- | --- |
| GetProducts | Obtient tous les produits. | API/produits | GET |
| GetProduct | Recherche d’un produit par ID. | API/products/*id* | GET |
| PutProduct | Met à jour un produit. | API/products/*id* | PUT |
| PostProduct | Crée un nouveau produit. | API/produits | PUBLIER |
| DeleteProduct | Supprime un produit. | API/products/*id* | SUPPR |

Chaque méthode appelle `OrdersContext` pour interroger la base de données. Appellent des méthodes qui modifient la collection (PUT, POST et DELETE) `db.SaveChanges` pour rendre persistantes les modifications apportées à la base de données. Contrôleurs sont créés par la requête HTTP et puis supprimés, il est donc nécessaire de conserver les modifications avant d’une méthode est retournée.

## <a name="add-a-database-initializer"></a>Ajouter un initialiseur de base de données

Entity Framework a une fonctionnalité très utile qui vous permet de remplir la base de données au démarrage et recréer automatiquement la base de données chaque fois que les modèles change. Cette fonctionnalité est utile lors du développement, car vous avez toujours des données de test, même si vous modifiez les modèles.

Dans l’Explorateur de solutions, cliquez sur le dossier Modèles et créez une nouvelle classe nommée `OrdersContextInitializer`. Collez l’implémentation suivante :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

En héritant de la **DropCreateDatabaseIfModelChanges** (classe), nous indiquons à Entity Framework pour supprimer la base de données chaque fois que nous modifions les classes de modèle. Lorsque Entity Framework crée ou recrée la base de données, il appelle le **Seed** méthode pour remplir les tables. Nous utilisons le **Seed** méthode pour ajouter certains produits exemple ainsi qu’une commande de l’exemple.

Cette fonctionnalité est très utile pour tester, mais n’utilisez pas le **DropCreateDatabaseIfModelChanges** classe en production, car vous risquez de perdre vos données si un utilisateur modifie une classe de modèle.

Ensuite, ouvrir Global.asax et ajoutez le code suivant à la **Application\_Démarrer** méthode :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Envoyer une demande au contrôleur

À ce stade, nous n’avons pas écrit de code de client, mais vous pouvez appeler le web API à l’aide d’un navigateur web ou un débogage HTTP outil tel que [Fiddler](http://www.fiddler2.com/fiddler2/). Dans Visual Studio, appuyez sur F5 pour démarrer le débogage. Votre navigateur web s’ouvre sur `http://localhost:*portnum*/`, où *portnum* correspond à un numéro de port.

Envoyer une requête HTTP à «`http://localhost:*portnum*/api/admin`. La première requête peut être lente, car Entify Framework a besoin créer et alimenter la base de données. La réponse doit quelque chose de similaire à ce qui suit :

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-2.md)
> [Suivant](using-web-api-with-entity-framework-part-4.md)
