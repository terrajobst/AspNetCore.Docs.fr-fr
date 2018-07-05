---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Utiliser des Migrations Code First pour amorcer la base de données | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 0d753ea52c57af2cbffff9e1e8741bbe49bc6d7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370524"
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Utiliser des Migrations Code First pour amorcer la base de données
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez utiliser [Migrations Code First](https://msdn.microsoft.com/data/jj591621) dans EF pour amorcer la base de données de test.

À partir de la **outils** menu, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

[!code-console[Main](part-3/samples/sample1.cmd)]

Cette commande ajoute un dossier nommé Migrations à votre projet, ainsi qu’un fichier de code nommé Configuration.cs dans le dossier Migrations.

![](part-3/_static/image1.png)

Ouvrez le fichier Configuration.cs. Ajoutez le code suivant **à l’aide de** instruction.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Puis ajoutez le code suivant à la **Configuration.Seed** méthode :

[!code-csharp[Main](part-3/samples/sample3.cs)]

Dans la fenêtre de Console du Gestionnaire de Package, tapez les commandes suivantes :

[!code-console[Main](part-3/samples/sample4.cmd)]

La première commande génère du code qui crée la base de données, et la deuxième commande exécute ce code. La base de données est créé localement, à l’aide de [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Explorer l’API (facultatif)

Appuyez sur F5 pour exécuter l’application en mode débogage. Visual Studio démarre IIS Express et exécute votre application web. Ensuite, Visual Studio lance un navigateur et ouvre la page d’accueil de l’application.

Lorsque Visual Studio s’exécute à un projet web, il affecte un numéro de port. Dans l’image ci-dessous, le numéro de port est 50524. Lorsque vous exécutez l’application, vous verrez un numéro de port différent.

![](part-3/_static/image3.png)

La page d’accueil est implémentée à l’aide d’ASP.NET MVC. En haut de la page, il existe un lien indiquant que « API ». Ce lien vous dirige vers une page d’aide générée automatiquement pour l’API web. (Pour savoir comment cette page d’aide est générée, et comment vous pouvez ajouter votre propre documentation à la page, consultez [création de Pages d’aide pour l’API Web ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Vous pouvez cliquer sur l’aide des liens de page pour afficher les détails sur l’API, y compris le format de demande et de réponse.

![](part-3/_static/image4.png)

L’API permet les opérations CRUD sur la base de données. Voici un résumé de l’API.

| Auteurs |  |
| --- | -- |
| OBTENIR l’api/authors | Obtenir tous les auteurs. |
| GET api/authors / {id} | Obtenir un auteur par ID. |
| / Api/authors POST | Créer un nouvel auteur. |
| PUT/API/authors / {id} | Mettre à jour un auteur existant. |
| Supprimer/API/authors / {id} | Supprimer un auteur. |

| Livres |  |
| --- | -- |
| OBTENIR /api/books | Obtenir tous les livres. |
| OBTENIR/API/books / {id} | Obtenir un livre par ID. |
| PUBLIER/api/la documentation | Créer un nouveau livre. |
| PUT/API/books / {id} | Mettre à jour un livre existant. |
| Supprimer/API/books / {id} | Supprimer un livre. |

## <a name="view-the-database-optional"></a>Afficher la base de données (facultatif)

Lorsque vous avez exécuté la commande Update-Database, EF créé la base de données et appelé le `Seed` (méthode). Lorsque vous exécutez l’application localement, EF utilise [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Vous pouvez afficher la base de données dans Visual Studio. À partir de la **vue** menu, sélectionnez **Explorateur d’objets SQL Server**.

![](part-3/_static/image5.png)

Dans le **se connecter au serveur** boîte de dialogue, dans le **nom du serveur** zone d’édition, tapez « (localdb) \v11.0 ». Laissez le **authentification** option en tant que « Authentification Windows ». Cliquez sur **Connexion**.

![](part-3/_static/image6.png)

Visual Studio se connecte à la base de données locale et affiche les bases de données existantes dans la fenêtre Explorateur d’objets SQL Server. Vous pouvez développer les nœuds pour afficher les tables EF créé.

![](part-3/_static/image7.png)

Pour afficher les données, cliquez sur une table et sélectionnez **afficher les données**.

![](part-3/_static/image8.png)

La capture d’écran suivante montre les résultats de la table de la documentation. Notez que EF rempli la base de données avec les données d’amorçage, et le tableau contient la clé étrangère à la table auteurs.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Précédent](part-2.md)
> [Suivant](part-4.md)
