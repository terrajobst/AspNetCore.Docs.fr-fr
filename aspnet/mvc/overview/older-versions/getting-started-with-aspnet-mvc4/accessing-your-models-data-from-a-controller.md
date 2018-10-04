---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: L’accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6d4d2a5f30e55cc876632f9f1bf73ab534a702ad
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577403"
---
<a name="accessing-your-models-data-from-a-controller"></a>L’accès aux données de votre modèle à partir d’un contrôleur
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.


Dans cette section, vous allez créer un nouveau `MoviesController` classe et d’écrire du code qui Récupère les données de film et l’affiche dans le navigateur à l’aide d’un modèle de vue.

**Générez l’application** avant de passer à l’étape suivante.

Avec le bouton droit le *contrôleurs* dossier et créez un nouveau `MoviesController` contrôleur. Les options ci-dessous seront affiche pas jusqu'à ce que vous générez votre application. Sélectionnez les options suivantes :

- Nom du contrôleur : **MoviesController**. (Il s’agit de la valeur par défaut. )
- Modèle : **contrôleur MVC avec des actions de lecture/écriture et des vues, utilisant Entity Framework**.
- Classe de modèle : **Movie (MvcMovie.Models)**.
- Classe de contexte de données : **MovieDBContext (MvcMovie.Models)**.
- Vues : **Razor (CSHTML)**. (La valeur par défaut.)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Cliquez sur **Ajouter**. Visual Studio Express crée les fichiers et dossiers suivants :

- *Un MoviesController.cs* fichier dans le projet *contrôleurs* dossier.
- Un *films* dossier dans le projet *vues* dossier.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, et *Index.cshtml* dans le nouveau *Views\Movies* dossier.

ASP.NET MVC 4 créée automatiquement le CRUD (créer, lire, mettre à jour et supprimer) des méthodes d’action et des vues pour vous (la création automatique de méthodes d’action CRUD et de vues est appelée génération de modèles automatique). Vous disposez maintenant d’une application web entièrement fonctionnelles qui vous permet de créer, répertorier, modifier et supprimer des entrées de film.

Exécutez l’application et accédez à la `Movies` contrôleur en ajoutant */Movies* à l’URL dans la barre d’adresses de votre navigateur. Étant donné que l’application repose sur le routage par défaut (définie dans le *Global.asax* fichier), la demande de navigateur `http://localhost:xxxxx/Movies` est acheminé vers la valeur par défaut `Index` méthode d’action de la `Movies` contrôleur. En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est en fait identique à la demande de navigateur `http://localhost:xxxxx/Movies/Index`. Le résultat est une liste vide de films, car vous n’avez pas ajouté.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Création d’un film

Sélectionnez le lien **Créer nouveau**. Entrez des détails sur un film, puis activez la **créer** bouton.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

En cliquant sur le **créer** bouton provoque le formulaire à être publié sur le serveur, où les informations de film sont enregistrées dans la base de données. Vous êtes ensuite redirigé vers le */Movies* URL, où vous pouvez voir le film nouvellement créé dans la liste.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Créez deux ou trois autres entrées. Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.

## <a name="examining-the-generated-code"></a>Examen du Code généré

Ouvrez le *Controllers\MoviesController.cs* de fichiers et d’examiner le généré `Index` (méthode). Une partie du contrôleur de film avec la `Index` méthode est indiquée ci-dessous.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La ligne suivante à partir de la `MoviesController` classe instancie un contexte de base de données de film, comme décrit précédemment. Vous pouvez utiliser le contexte de base de données de film pour interroger, modifier et supprimer des films.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Une demande pour le `Movies` contrôleur retourne toutes les entrées dans le `Movies` table de la base de données de film, puis transmet les résultats à le `Index` vue.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modèles fortement typés et @model mot clé

Précédemment dans ce didacticiel, vous l’avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue en utilisant le `ViewBag` objet. Le `ViewBag` est un objet dynamique qui offre un moyen pratique de la liaison tardive pour passer des informations à une vue.

ASP.NET MVC fournit également la possibilité de passer fortement typée des données ou des objets à un modèle de vue. Cela fortement typées approche permet une meilleure compilation la vérification de votre code et l’expérience IntelliSense plus riche dans l’éditeur Visual Studio. Le mécanisme de génération de modèles automatique dans Visual Studio utilisé cette approche avec la `MoviesController` modèles de classe et de la vue lorsqu’il créé les méthodes et les vues.

Dans le *Controllers\MoviesController.cs* fichier examiner généré `Details` (méthode). Une partie du contrôleur de film avec la `Details` méthode est indiquée ci-dessous.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Si un `Movie` est trouvé, une instance de la `Movie` modèle est passé à l’affichage des détails. Examinez le contenu de la *Views\Movies\Details.cshtml* fichier.

En incluant un `@model` instruction en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet attendu par la vue. Quand vous avez créé le contrôleur pour les films, Visual Studio a inclus automatiquement l’instruction `@model` suivante en haut du fichier *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Cette directive `@model` vous permet d’accéder au film que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé. Par exemple, dans le *Details.cshtml* modèle, le code passe chaque champ du film à la `DisplayNameFor` et [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers avec fortement typée `Model` objet. Les méthodes Create et Edit et les modèles de vue également passer un objet de modèle de film.

Examiner le *Index.cshtml* afficher le modèle et le `Index` méthode dans le *MoviesController.cs* fichier. Notez comment le code crée un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) de l’objet lorsqu’il appelle le `View` méthode d’assistance dans le `Index` méthode d’action. Le code transmet ensuite `Movies` liste à partir du contrôleur à la vue :

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Lorsque vous avez créé le contrôleur de films, Visual Studio Express inclus automatiquement ce qui suit `@model` instruction en haut de la *Index.cshtml* fichier :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Cela `@model` directive vous permet d’accéder à la liste de films que le contrôleur a passé à la vue en utilisant un `Model` objet fortement typé. Par exemple, dans le *Index.cshtml* modèle, le code effectue une itération sur les films en effectuant un `foreach` instruction sur fortement typée `Model` objet :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Étant donné que le `Model` objet est fortement typé (comme un `IEnumerable<Movie>` objet), chaque `item` objet dans la boucle est typé en tant que `Movie`. Entre autres avantages, cela signifie que vous obtenez la vérification au moment de la compilation du code et prise en charge d’IntelliSense dans l’éditeur de code complète :

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Utilisation de SQL Server LocalDB

Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie vers lequel pointe un `Movies` base de données qui n’existe pas encore, c’est pourquoi Code First créé la base de données automatiquement. Vous pouvez vérifier qu’il a été créé en consultant le *application\_données* dossier. Si vous ne voyez pas le *Movies.mdf* de fichiers, cliquez sur le **afficher tous les fichiers** situé dans le **l’Explorateur de solutions** barre d’outils, cliquez sur le **Actualiser** bouton, puis développez le *application\_données* dossier.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Double-cliquez sur *Movies.mdf* pour ouvrir **DATABASE EXPLORER**, puis développez le **Tables** dossier pour afficher le tableau de films.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Si l’Explorateur de base de données n’apparaît pas, à partir de la **outils** menu, sélectionnez **se connecter à la base de données**, puis annuler le **choisir la Source de données** boîte de dialogue. Cela forcera ouvrir l’Explorateur de base de données.


> [!NOTE]
> Si vous utilisez VWD ou Visual Studio 2010 et que vous obtenez une erreur similaire à une des opérations suivantes :
> 
> - La base de données ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' ne peut pas être ouvert, car il s’agit version 706. Ce serveur prend en charge la version 655 et les versions antérieure. Un chemin d’accès à une version antérieure n’est pas pris en charge.
> - &quot;Exception InvalidOperation a pas été gérée par le code utilisateur&quot; le SqlConnection fourni ne spécifie pas un catalogue initial.
> 
> Vous devez installer le [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) et [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Vérifiez le `MovieDBContext` chaîne de connexion spécifiée dans la page précédente.


Avec le bouton droit le `Movies` de table et sélectionnez **afficher les données de Table** pour afficher les données que vous avez créé.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Avec le bouton droit le `Movies` de table et sélectionnez **ouvrir la définition de Table** pour afficher la table de structure que Entity Framework Code First créé pour vous.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Notez comment le schéma de la `Movies` table correspond à la `Movie` classe que vous avez créé précédemment. Entity Framework Code First automatiquement créé ce schéma pour vous en fonction de votre `Movie` classe.

Lorsque vous avez terminé, fermez la connexion en double-cliquant sur *MovieDBContext* et en sélectionnant **fermer la connexion**. (Si vous ne fermez pas la connexion, vous pouvez obtenir une erreur la prochaine fois que vous exécutez le projet).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Vous avez maintenant la base de données et une page de liste simple d’afficher le contenu à partir de celui-ci. Dans le didacticiel suivant, nous allons examiner le reste du code généré automatiquement et ajoutez un `SearchIndex` (méthode) et un `SearchIndex` vue qui vous permet de rechercher des films dans cette base de données.

> [!div class="step-by-step"]
> [Précédent](adding-a-model.md)
> [Suivant](examining-the-edit-methods-and-edit-view.md)
