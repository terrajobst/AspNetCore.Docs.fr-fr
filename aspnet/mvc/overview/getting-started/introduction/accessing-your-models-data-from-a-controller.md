---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: L’accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817549"
---
<a name="accessing-your-models-data-from-a-controller"></a>L’accès aux données de votre modèle à partir d’un contrôleur
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Dans cette section, vous allez créer un nouveau `MoviesController` classe et d’écrire du code qui Récupère les données de film et l’affiche dans le navigateur à l’aide d’un modèle de vue.

**Générez l’application** avant de passer à l’étape suivante. Si vous ne générez l’application, vous obtiendrez une erreur d’ajout d’un contrôleur.

Dans l’Explorateur de solutions, cliquez sur le *contrôleurs* dossier, puis cliquez sur **ajouter**, puis **contrôleur**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

Dans le **ajouter une structure** boîte de dialogue, cliquez sur **contrôleur MVC 5 avec vues, utilisant Entity Framework**, puis cliquez sur **ajouter**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Sélectionnez **Movie (MvcMovie.Models)** pour la classe de modèle.
- Sélectionnez **MovieDBContext (MvcMovie.Models)** pour la classe de contexte de données.
- Entrez le nom du contrôleur **MoviesController**.

  L’image ci-dessous montre la boîte de dialogue terminé.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Cliquez sur **Ajouter**. (Si vous obtenez une erreur, vous probablement n’a pas généré l’application avant de commencer l’ajout du contrôleur.) Visual Studio crée les fichiers et dossiers suivants :

- *Un MoviesController.cs* de fichiers dans le *contrôleurs* dossier.
- Un *Views\Movies* dossier.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, et *Index.cshtml* dans le nouveau *Views\Movies* dossier.

Visual Studio créé automatiquement la [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) des méthodes d’action et des vues pour vous (la création automatique de méthodes d’action CRUD et de vues est appelée génération de modèles automatique). Vous disposez maintenant d’une application web entièrement fonctionnelles qui vous permet de créer, répertorier, modifier et supprimer des entrées de film.

Exécutez l’application et cliquez sur le **MVC Movie** lien (ou accédez à la `Movies` contrôleur en ajoutant */Movies* à l’URL dans la barre d’adresses de votre navigateur). Étant donné que l’application repose sur le routage par défaut (définie dans le *application\_start\routeconfig* fichier), la demande de navigateur `http://localhost:xxxxx/Movies` est acheminé vers la valeur par défaut `Index` méthode d’action de la `Movies` contrôleur. En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est en fait identique à la demande de navigateur `http://localhost:xxxxx/Movies/Index`. Le résultat est une liste vide de films, car vous n’avez pas ajouté.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Création d’un film

Sélectionnez le lien **Créer nouveau**. Entrez des détails sur un film, puis activez la **créer** bouton.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Vous n’êtes peut-être pas en mesure d’entrer des séparateurs décimaux ou des virgules dans le champ prix. Pour prendre en charge la validation jQuery pour les paramètres régionaux non anglais qui utilisent une virgule (&quot;,&quot;) pour une virgule décimale et les formats de date non anglais des États-Unis, vous devez inclure *globalize.js* et votre propre  *cultures/globalize.cultures.js* fichier (à partir de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`. Je montrerai comment effectuer cette opération dans le didacticiel suivant. Pour le moment, entrez simplement des nombres entiers tels que 10.


En cliquant sur le **créer** bouton provoque le formulaire à être publié sur le serveur, où les informations de film sont enregistrées dans la base de données. Vous êtes ensuite redirigé vers le */Movies* URL, où vous pouvez voir le film nouvellement créé dans la liste.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Créez deux ou trois autres entrées. Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.

## <a name="examining-the-generated-code"></a>Examen du Code généré

Ouvrez le *Controllers\MoviesController.cs* de fichiers et d’examiner le généré `Index` (méthode). Une partie du contrôleur de film avec la `Index` méthode est indiquée ci-dessous.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Une demande à la `Movies` contrôleur retourne toutes les entrées dans le `Movies` table, puis transmet les résultats à le `Index` vue. La ligne suivante à partir de la `MoviesController` classe instancie un contexte de base de données de film, comme décrit précédemment. Vous pouvez utiliser le contexte de base de données de film pour interroger, modifier et supprimer des films.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modèles fortement typés et @model mot clé

Précédemment dans ce didacticiel, vous l’avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue en utilisant le `ViewBag` objet. Le `ViewBag` est un objet dynamique qui offre un moyen pratique de la liaison tardive pour passer des informations à une vue.

MVC fournit également la possibilité de passer *fortement* des objets typés à un modèle de vue. Cette approche fortement typée permet une meilleure compilation au moment de la vérification de votre code et plus riche [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) dans l’éditeur Visual Studio. Le mécanisme de génération de modèles automatique dans Visual Studio utilisé cette approche (autrement dit, en passant un *fortement* modèle typé) avec le `MoviesController` modèles de classe et de la vue lorsqu’il créé les méthodes et les vues.

Dans le *Controllers\MoviesController.cs* fichier examiner généré `Details` (méthode). Le `Details` méthode est indiquée ci-dessous.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Le `id` paramètre est généralement passé en tant que données d’itinéraire, par exemple `http://localhost:1234/movies/details/1` définira le contrôleur au contrôleur de films, l’action à `details` et `id` à 1. Vous pouvez également transmettre l’id avec une chaîne de requête comme suit :

`http://localhost:1234/movies/details?id=1`

Si un `Movie` est trouvé, une instance de la `Movie` modèle est passé à la `Details` vue :

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Examinez le contenu de la *Views\Movies\Details.cshtml* fichier :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

En incluant un `@model` instruction en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet attendu par la vue. Quand vous avez créé le contrôleur pour les films, Visual Studio a inclus automatiquement l’instruction `@model` suivante en haut du fichier *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Cette directive `@model` vous permet d’accéder au film que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé. Par exemple, dans le *Details.cshtml* modèle, le code passe chaque champ du film à la `DisplayNameFor` et [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers avec fortement typée `Model` objet. Le `Create` et `Edit` méthodes et modèles de vue également passer un objet de modèle de film.

Examiner le *Index.cshtml* afficher le modèle et le `Index` méthode dans le *MoviesController.cs* fichier. Notez comment le code crée un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) de l’objet lorsqu’il appelle le `View` méthode d’assistance dans le `Index` méthode d’action. Le code transmet ensuite `Movies` liste à partir de la `Index` méthode d’action à la vue :

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Lorsque vous avez créé le contrôleur de films, Visual Studio inclus automatiquement ce qui suit `@model` instruction en haut de la *Index.cshtml* fichier :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Cela `@model` directive vous permet d’accéder à la liste de films que le contrôleur a passé à la vue en utilisant un `Model` objet fortement typé. Par exemple, dans le *Index.cshtml* modèle, le code effectue une itération sur les films en effectuant un `foreach` instruction sur fortement typée `Model` objet :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Étant donné que le `Model` objet est fortement typé (comme un `IEnumerable<Movie>` objet), chaque `item` objet dans la boucle est typé en tant que `Movie`. Entre autres avantages, cela signifie que vous obtenez la vérification au moment de la compilation du code et prise en charge d’IntelliSense dans l’éditeur de code complète :

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Utilisation de SQL Server LocalDB

Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie vers lequel pointe un `Movies` base de données qui n’existe pas encore, c’est pourquoi Code First créé la base de données automatiquement. Vous pouvez vérifier qu’il a été créé en consultant le *application\_données* dossier. Si vous ne voyez pas le *Movies.mdf* de fichiers, cliquez sur le **afficher tous les fichiers** situé dans le **l’Explorateur de solutions** barre d’outils, cliquez sur le **Actualiser** bouton, puis développez le *application\_données* dossier.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Double-cliquez sur *Movies.mdf* pour ouvrir **Explorateur de serveurs**, puis développez le **Tables** dossier pour afficher le tableau de films. Notez l’icône de clé en regard de code. Par défaut, EF fait d’une propriété nommée ID de la clé primaire. Pour plus d’informations sur EF et MVC, consultez Didacticiel excellente de Tom Dykstra sur [MVC et Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Avec le bouton droit le `Movies` de table et sélectionnez **afficher les données de Table** pour afficher les données que vous avez créé.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Avec le bouton droit le `Movies` de table et sélectionnez **ouvrir la définition de Table** pour afficher la table de structure que Entity Framework Code First créé pour vous.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Notez comment le schéma de la `Movies` table correspond à la `Movie` classe que vous avez créé précédemment. Entity Framework Code First automatiquement créé ce schéma pour vous en fonction de votre `Movie` classe.

Lorsque vous avez terminé, fermez la connexion en double-cliquant sur *MovieDBContext* et en sélectionnant **fermer la connexion**. (Si vous ne fermez pas la connexion, vous pouvez obtenir une erreur la prochaine fois que vous exécutez le projet).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Vous disposez maintenant d’une base de données et de pages pour afficher, modifier, mettre à jour et supprimer les données. Dans le didacticiel suivant, nous allons examiner le reste du code généré automatiquement et ajoutez un `SearchIndex` (méthode) et un `SearchIndex` vue qui vous permet de rechercher des films dans cette base de données. Pour plus d’informations sur l’utilisation d’Entity Framework avec MVC, consultez [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Précédent](creating-a-connection-string.md)
> [Suivant](examining-the-edit-methods-and-edit-view.md)
