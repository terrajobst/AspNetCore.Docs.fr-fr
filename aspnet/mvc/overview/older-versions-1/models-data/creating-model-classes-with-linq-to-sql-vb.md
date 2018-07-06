---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Création de Classes de modèle avec LINQ to SQL (VB) | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est d’expliquer une méthode de création de classes de modèle pour une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à créer le modèle c...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 2073ef716763f746f315a2131c4aa049bbdeec22
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841352"
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>Création de Classes de modèle avec LINQ to SQL (VB)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> L’objectif de ce didacticiel est d’expliquer une méthode de création de classes de modèle pour une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à créer des classes de modèle et effectuer des accès de base de données en tirant parti de Microsoft LINQ to SQL.


L’objectif de ce didacticiel est d’expliquer une méthode de création de classes de modèle pour une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à créer des classes de modèle et effectuer des accès de base de données en tirant parti de Microsoft LINQ to SQL.

Dans ce didacticiel, nous créer une application de base de données de film base. Nous commençons par la création de l’application de base de données de film dans le plus rapide et le plus simple possible. Nous effectuons tous nos accès aux données directement à partir de nos actions de contrôleur.

Ensuite, vous allez apprendre à utiliser le modèle de référentiel. À l’aide du modèle de référentiel nécessite un peu plus de travail. Toutefois, l’avantage de l’adoption de ce modèle est qu’il vous permet de créer des applications adaptables à modifier et peuvent être facilement testées.

## <a name="what-is-a-model-class"></a>Qu’est une classe de modèle ?

Un modèle MVC contient toute la logique d’application qui n’est pas contenue dans une vue MVC ou d’un contrôleur MVC. En particulier, un modèle MVC contient toutes vos professionnels de l’application et la logique d’accès aux données.

Vous pouvez utiliser une variété de technologies différentes pour implémenter votre logique d’accès aux données. Par exemple, vous pouvez créer vos classes d’accès aux données en utilisant les classes Microsoft Entity Framework, NHibernate, Subsonic ou ADO.NET.

Dans ce didacticiel, j’utilise LINQ to SQL pour interroger et mettre à jour de la base de données. LINQ to SQL vous offre une méthode très facile de l’interaction avec une base de données Microsoft SQL Server. Toutefois, il est important de comprendre que l’infrastructure ASP.NET MVC n’est pas lié à LINQ to SQL en aucune façon. ASP.NET MVC est compatible avec toute technologie d’accès aux données.

## <a name="create-a-movie-database"></a>Créer une base de données de film

Dans ce didacticiel--afin d’illustrer comment vous pouvez créer des classes de modèle--nous générer une application de base de données de film simple. La première étape consiste à créer une base de données. Avec le bouton droit de l’application\_dossier de données dans la fenêtre Explorateur de solutions, puis sélectionnez l’option de menu **ajouter, nouvel élément**. Sélectionnez le modèle de base de données SQL Server, attribuez-lui le nom MoviesDB.mdf, puis cliquez sur le **ajouter** bouton (voir Figure 1).


[![Ajout d’une nouvelle base de données de SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Figure 01**: ajout d’une nouvelle base de données de SQL Server ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


Après avoir créé la nouvelle base de données, vous pouvez ouvrir la base de données en double-cliquant sur le fichier MoviesDB.mdf dans l’application\_dossier de données. Double-cliquez sur le fichier MoviesDB.mdf pour ouvrir la fenêtre Explorateur de serveurs (voir Figure 2).


|   | La fenêtre Explorateur de serveurs est appelée la fenêtre Explorateur de base de données lors de l’utilisation de Visual Web Developer. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![À l’aide de la fenêtre Explorateur de serveurs](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Figure 02**: à l’aide de la fenêtre Explorateur de serveurs ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


Nous devons ajouter une table à notre base de données qui représente notre films. Cliquez sur le dossier Tables et sélectionnez l’option de menu **ajouter une nouvelle Table**. Cette option de menu ouvre le Concepteur de tables (voir Figure 3).


[![À l’aide de la fenêtre Explorateur de serveurs](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Figure 03**: le Concepteur de tables ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


Nous devons ajouter les colonnes suivantes à la table de base de données :

| **Nom de colonne** | **Type de données** | **Autoriser les valeurs null** |
| --- | --- | --- |
| Id | Int | False |
| Titre | Nvarchar (200) | False |
| Directeur | nvarchar (50) | False |

Vous devez effectuer deux tâches spéciales pour la colonne Id. Tout d’abord, vous devez marquer la colonne Id en tant que colonne clé primaire en sélectionnant la colonne dans le Concepteur de tables et en cliquant sur l’icône d’une clé. LINQ to SQL vous oblige à spécifier votre clé primaire lorsque l’exécution insère ou met à jour par rapport à la base de données.

Ensuite, vous devez marquer la colonne Id comme une colonne d’identité, affectez la valeur Oui à la **est d’identité** propriété (voir Figure 3). Une colonne d’identité est une colonne qui est un nouveau numéro affectée automatiquement chaque fois que vous ajoutez une nouvelle ligne de données à une table.

Après avoir apporté ces modifications, enregistrez la table avec le nom tblMovie. Vous pouvez enregistrer la table en cliquant sur le bouton Enregistrer.

## <a name="create-linq-to-sql-classes"></a>Créer des Classes LINQ to SQL

Notre modèle MVC contiendra LINQ aux classes SQL représentant la table de base de données tblMovie. Le moyen le plus simple de créer ces classes LINQ to SQL consiste à cliquez sur le dossier Modèles, sélectionnez **ajouter, nouvel élément**, sélectionnez le LINQ au modèle de Classes SQL, nommez les classes Movie.dbml, puis cliquez sur le **ajouter**bouton (voir Figure 4).


[![Création de LINQ aux classes SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Figure 04**: création de classes LINQ to SQL ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


Immédiatement après avoir créé le film Classes LINQ to SQL, le concepteur objet/relationnel s’affiche. Vous pouvez faire glisser des tables de base de données de la fenêtre de l’Explorateur de serveurs vers le concepteur objet/relationnel pour créer des Classes LINQ to SQL qui représentent des tables de base de données particulière. Nous devons ajouter la table de base de données tblMovie sur le Concepteur Objet/Relationnel (voir Figure 4).


[![À l’aide du concepteur objet/relationnel](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Figure 05**: à l’aide du concepteur objet/relationnel ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


Par défaut, le concepteur objet/relationnel crée une classe avec très identique à celui de la table de base de données que vous faites glisser sur le concepteur. Toutefois, nous ne voulons pas appeler notre tblMovie de classe. Par conséquent, cliquez sur le nom de la classe dans le concepteur et remplacez le nom de la classe Movie.

Enfin, n’oubliez pas de cliquer sur le **enregistrer** bouton (l’image de disquette) pour enregistrer le LINQ aux Classes SQL. Sinon, le Classes LINQ to SQL ne sont pas généré par le concepteur objet/relationnel.

## <a name="using-linq-to-sql-in-a-controller-action"></a>À l’aide de LINQ to SQL dans une Action de contrôleur

Maintenant que nous avons nos classes LINQ to SQL, nous pouvons utiliser ces classes pour récupérer des données à partir de la base de données. Dans cette section, vous allez apprendre à utiliser LINQ aux classes SQL directement dans une action de contrôleur. Nous allons afficher la liste de films à partir de la table de base de données tblMovies dans une vue MVC.

Tout d’abord, nous devons modifier la classe HomeController. Cette classe peut être trouvée dans le dossier contrôleurs de votre application. Modifiez la classe afin qu’il ressemble à la classe dans le Listing 1.

**Liste 1 : `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

L’action Index() dans le Listing 1 utilise une classe LINQ to SQL DataContext (la MovieDataContext) pour représenter la base de données MoviesDB. La classe MoveDataContext a été générée par Visual Studio concepteur objet/relationnel.

Une requête LINQ est effectuée contre le DataContext pour récupérer tous les films à partir de la table de base de données tblMovies. La liste de films est affectée à une variable locale nommée films. Enfin, la liste de films est transmise à la vue de données d’affichage.

Pour afficher les films, nous devons ensuite modifier la vue Index. Vous trouverez la vue Index dans le dossier Views\Home\. Mettre à jour la vue Index afin qu’il ressemble à la vue dans la liste 2.

**Listing 2 : `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Notez que la vue Index modifiée inclut un &lt;% @ % d’espace de noms import&gt; directive en haut de la vue. Cette directive importe l’espace de noms MvcApplication1. Nous avons besoin de cet espace de noms pour pouvoir fonctionner avec les classes de modèle – en particulier, la classe Movie--dans la vue.

La vue dans la liste 2 contient un pour chaque boucle qui itère au sein de tous les éléments représentés par la propriété ViewData.Model. La valeur de la propriété Title est affichée pour chaque film.

Notez que la valeur de la propriété ViewData.Model est convertie en un IEnumerable. Cela est nécessaire pour parcourir le contenu de ViewData.Model. Une autre option consiste à créer une vue fortement typée. Lorsque vous créez une vue fortement typée, vous caster la propriété ViewData.Model à un type particulier dans une classe de la vue code-behind.

Si vous exécutez l’application après la modification de la classe HomeController et la vue Index, vous obtiendrez une page vierge. Vous obtiendrez une page vierge, car il n’existe aucun enregistrement de film dans la table de base de données tblMovies.

Pour ajouter des enregistrements à la table de base de données tblMovies, avec le bouton droit de la table de base de données tblMovies dans la fenêtre Explorateur de serveurs (fenêtre de l’Explorateur de base de données dans Visual Web Developer) et sélectionnez l’option de menu **afficher les données de Table**. Vous pouvez insérer des enregistrements de film à l’aide de la grille qui s’affiche (voir Figure 5).


[![Insertion de films](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Figure 06**: insertion de films ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


Une fois que vous ajoutez des enregistrements de base de données à la table tblMovies, et vous exécutez l’application, vous verrez la page dans la Figure 7. Tous les enregistrements de base de données de film sont affichés dans une liste à puces.


[![Affichage des films avec la vue Index](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Figure 07**: affichage des films avec la vue Index ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>À l’aide du modèle de référentiel

Dans la section précédente, nous avons utilisé LINQ aux classes SQL directement dans une action de contrôleur. Nous avons utilisé la classe MovieDataContext directement à partir de l’action du contrôleur Index(). Il n’existe pas de mal à cela dans le cas d’une application simple. Toutefois, l’utilisation directe LINQ to SQL dans une classe de contrôleur crée des problèmes lorsque vous avez besoin créer une application plus complexe.

À l’aide de LINQ to SQL au sein d’une classe de contrôleur rend difficile de passer à l’avenir des technologies d’accès aux données. Par exemple, vous pouvez décider de passer de l’utilisation de Microsoft LINQ to SQL à l’aide de Microsoft Entity Framework en tant que votre technologie d’accès aux données. Dans ce cas, vous devrez réécrire chaque contrôleur qui accède à la base de données au sein de votre application.

À l’aide de LINQ to SQL au sein d’une classe de contrôleur rend également difficile de créer des tests unitaires pour votre application. Normalement, vous ne souhaitez pas interagir avec une base de données lors de l’exécution des tests unitaires. Vous souhaitez utiliser vos tests unitaires pour tester votre logique d’application et pas votre serveur de base de données.

Pour générer une application MVC modification plus adaptable à venir et qui peut être testé plus facilement, vous devez envisager d’utiliser le modèle de référentiel. Lorsque vous utilisez le modèle de référentiel, vous créez une classe de référentiel distinct qui contient l’ensemble de votre logique d’accès de base de données.

Lorsque vous créez la classe de dépôt, vous créez une interface qui représente toutes les méthodes utilisées par la classe de dépôt. Au sein de vos contrôleurs, vous écrivez votre code par rapport à l’interface au lieu du référentiel. De cette façon, vous pouvez implémenter le référentiel à l’aide de technologies d’accès aux données différents dans le futur.

L’interface dans le Listing 3 est nommé IMovieRepository et il représente une méthode unique nommée ListAll().

**Liste 3 : `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

La classe de dépôt dans la liste 4 implémente l’interface IMovieRepository. Notez qu’il contient une méthode nommée ListAll() qui correspond à la méthode requise par l’interface IMovieRepository.

**Liste 4 – `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Enfin, la classe MoviesController dans la liste 5 utilise le modèle de référentiel. Il n’utilise plus LINQ aux classes SQL directement.

**Liste 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Notez que la classe MoviesController dans la liste 5 possède deux constructeurs. Le premier constructeur, le constructeur sans paramètre, est appelé lors de l’exécution de votre application. Ce constructeur crée une instance de la classe MovieRepository et passe à la deuxième constructeur.

Le deuxième constructeur a un seul paramètre : un paramètre IMovieRepository. Ce constructeur affecte simplement la valeur du paramètre à un champ de niveau classe nommé \_référentiel.

La classe MoviesController tire parti d’un modèle de conception de logiciel appelé le modèle d’Injection de dépendance. En particulier, il est à l’aide de ce que l'on appelle l’Injection de dépendances de constructeur. Vous trouverez plus d’informations sur ce modèle, lisez l’article suivant de Martin Fowler :

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Notez que tout le code dans la classe MoviesController (à l’exception du premier constructeur) interagit avec l’interface de IMovieRepository au lieu de la classe MovieRepository réelle. Le code interagit avec une interface abstraite au lieu d’une implémentation concrète de l’interface.

Si vous souhaitez modifier la technologie d’accès aux données utilisée par l’application vous pouvez simplement implémenter l’interface IMovieRepository avec une classe qui utilise la technologie d’accès de base de données alternative. Par exemple, vous pouvez créer une classe EntityFrameworkMovieRepository ou une classe SubSonicMovieRepository. Étant donné que la classe de contrôleur est programmée par rapport à l’interface, vous pouvez transmettre une nouvelle implémentation de IMovieRepository à la classe de contrôleur et la classe continuera de fonctionner.

En outre, si vous souhaitez tester la classe MoviesController, vous pouvez passer une classe de dépôt de film factice à le MoviesController. Vous pouvez implémenter la classe IMovieRepository avec une classe qui n’accède pas réellement à la base de données, mais contient toutes les méthodes de l’interface IMovieRepository requis. De cette façon, vous pouvez le test unitaire la classe MoviesController sans réellement l’accès à une véritable base de données.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel a été pour illustrer comment vous pouvez créer des classes de modèle MVC en tirant parti de Microsoft LINQ to SQL. Nous avons examiné les deux stratégies pour afficher les données de la base de données dans une application ASP.NET MVC. Tout d’abord, nous avons créé LINQ aux classes SQL et utilisé les classes directement au sein d’une action de contrôleur. À l’aide de LINQ aux classes SQL au sein d’un contrôleur vous permet de rapidement et facilement afficher des données de base de données dans une application MVC.

Ensuite, nous avons exploré un peu plus difficile, mais sans aucun doute plus vertueux, le chemin d’accès pour afficher les données de la base de données. Nous a tiré parti du modèle dépôt et toute notre logique d’accès de base de données placée dans une classe de référentiel distinct. Dans notre contrôleur, nous avons écrit tout notre code par rapport à une interface au lieu d’une classe concrète. L’avantage du modèle dépôt est qu’il nous permet de modifier facilement les technologies d’accès de base de données à l’avenir, et il nous permet de tester facilement nos classes de contrôleur.

> [!div class="step-by-step"]
> [Précédent](creating-model-classes-with-the-entity-framework-vb.md)
> [Suivant](displaying-a-table-of-database-data-vb.md)
