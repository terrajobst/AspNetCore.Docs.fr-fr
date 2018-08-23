---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: L’accès aux données de votre modèle à partir d’un contrôleur (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 526f3b532a0b5c1c5c6d0cf8a9e17899300ca237
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832632"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>L’accès aux données de votre modèle à partir d’un contrôleur (VB)
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez c#, basculez vers le [c# version](../cs/accessing-your-models-data-from-a-controller.md) de ce didacticiel.


Dans cette section, vous allez créer un nouveau `MoviesController` classe et d’écrire du code qui Récupère les données de film et l’affiche dans le navigateur à l’aide d’un modèle de vue. Veillez à générer votre application avant de continuer.

Avec le bouton droit le *contrôleurs* dossier et créez un nouveau `MoviesController` contrôleur. Sélectionnez les options suivantes :

- Nom du contrôleur : **MoviesController**. (C’est la valeur par défaut).
- Modèle : **contrôleur avec actions en lecture/écriture et de vues, utilisant Entity Framework**.
- Classe de modèle : **Movie (MvcMovie.Models)**.
- Classe de contexte de données : **MovieDBContext (MvcMovie.Models)**.
- Vues : **Razor (CSHTML)**. (La valeur par défaut.)

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Cliquez sur **Ajouter**. Visual Web Developer crée les fichiers et dossiers suivants :

- *Un MoviesController.vb* fichier dans le projet *contrôleurs* dossier.
- Un *films* dossier dans le projet *vues* dossier.
- *Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, et *Index.vbhtml* dans le nouveau *Views\Movies* dossier.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Le mécanisme de génération de modèles automatique ASP.NET MVC 3 créé automatiquement la CRUD (créer, lire, mettre à jour et supprimer) des méthodes d’action et des vues pour vous. Vous disposez maintenant d’une application web entièrement fonctionnelles qui vous permet de créer, répertorier, modifier et supprimer des entrées de film.

Exécutez l’application et accédez à la `Movies` contrôleur en ajoutant */Movies* à l’URL dans la barre d’adresses de votre navigateur. Étant donné que l’application repose sur le routage par défaut (définie dans le *Global.asax* fichier), la demande de navigateur `http://localhost:xxxxx/Movies` est acheminé vers la valeur par défaut `Index` méthode d’action de la `Movies` contrôleur. En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est en fait identique à la demande de navigateur `http://localhost:xxxxx/Movies/Index`. Le résultat est une liste vide de films, car vous n’avez pas ajouté.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Création d’un film

Sélectionnez le lien **Créer nouveau**. Entrez des détails sur un film, puis activez la **créer** bouton.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

En cliquant sur le **créer** bouton provoque le formulaire à être publié sur le serveur, où les informations de film sont enregistrées dans la base de données. Vous êtes ensuite redirigé vers le */Movies* URL, où vous pouvez voir le film nouvellement créé dans la liste.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Créez deux ou trois autres entrées. Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.

## <a name="examining-the-generated-code"></a>Examen du Code généré

Ouvrez le *Controllers\MoviesController.vb* de fichiers et d’examiner le généré `Index` (méthode). Une partie du contrôleur de film avec la `Index` méthode est indiquée ci-dessous.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

La ligne suivante à partir de la `MoviesController` classe instancie un contexte de base de données de film, comme décrit précédemment. Vous pouvez utiliser le contexte de base de données de film pour interroger, modifier et supprimer des films.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Une demande pour le `Movies` contrôleur retourne toutes les entrées dans le `Movies` table de la base de données de film, puis transmet les résultats à le `Index` vue.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modèles fortement typés et @model mot clé

Précédemment dans ce didacticiel, vous l’avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue en utilisant le `ViewBag` objet. Le `ViewBag` est un objet dynamique qui offre un moyen pratique de la liaison tardive pour passer des informations à une vue.

ASP.NET MVC fournit également la possibilité de passer fortement typée des données ou des objets à un modèle de vue. Cela fortement typées approche permet une meilleure compilation la vérification de votre code et l’expérience IntelliSense plus riche dans l’éditeur de Visual Web Developer. Nous utilisons cette approche avec la `MoviesController` classe et *Index.vbhtml* afficher le modèle.

Notez comment le code crée un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) de l’objet lorsqu’il appelle le `View` méthode d’assistance dans le `Index` méthode d’action. Le code transmet ensuite `Movies` liste à partir du contrôleur à la vue :

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

En incluant un `@ModelType` instruction en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet attendu par la vue. Lorsque vous avez créé le contrôleur de films, Visual Web Developer inclus automatiquement ce qui suit `@model` instruction en haut de la *Index.vbhtml* fichier :

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Cela `@ModelType` directive vous permet d’accéder à la liste de films que le contrôleur a passé à la vue en utilisant un `Model` objet fortement typé. Par exemple, dans le *Index.vbhtml* modèle, le code effectue une itération sur les films en effectuant un `foreach` instruction sur fortement typée `Model` objet :

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Étant donné que le `Model` objet est fortement typé (comme un `IEnumerable<Movie>` objet), chaque `item` objet dans la boucle est typé en tant que `Movie`. Entre autres avantages, cela signifie que vous obtenez la vérification au moment de la compilation du code et prise en charge d’IntelliSense dans l’éditeur de code complète :

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Utilisation de SQL Server Compact

Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie vers lequel pointe un `Movies` base de données qui n’existe pas encore, c’est pourquoi Code First créé la base de données automatiquement. Vous pouvez vérifier qu’il a été créé en consultant le *application\_données* dossier. Si vous ne voyez pas le *Movies.sdf* de fichiers, cliquez sur le **afficher tous les fichiers** situé dans le **l’Explorateur de solutions** barre d’outils, cliquez sur le **Actualiser** bouton, puis développez le *application\_données* dossier.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Double-cliquez sur *Movies.sdf* pour ouvrir **Explorateur de serveurs**. Puis développez le **Tables** dossier pour afficher les tables qui ont été créés dans la base de données.

> [!NOTE]
> Si vous obtenez une erreur lorsque vous double-cliquez sur *Movies.sdf*, assurez-vous que vous avez installé **Visual Studio 2010 SP1 Tools pour SQL Server Compact 4.0**. (Pour obtenir des liens vers le logiciel, consultez la liste des conditions préalables dans la partie 1 de cette série de didacticiels). Si vous installez la version maintenant, vous devrez fermer et rouvrir Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Il existe deux tables, une pour le `Movie` jeu d’entités, puis le `EdmMetadata` table. Le `EdmMetadata` table est utilisée par Entity Framework pour déterminer quand le modèle et la base de données sont désynchronisés.

Avec le bouton droit le `Movies` de table et sélectionnez **afficher les données de Table** pour afficher les données que vous avez créé.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Cliquez sur le `Movies` de table et sélectionnez **modifier le schéma de Table**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Notez comment le schéma de la `Movies` table correspond à la `Movie` classe que vous avez créé précédemment. Entity Framework Code First automatiquement créé ce schéma pour vous en fonction de votre `Movie` classe.

Lorsque vous avez terminé, fermez la connexion. (Si vous ne fermez pas la connexion, vous pouvez obtenir une erreur la prochaine fois que vous exécutez le projet).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Vous avez maintenant la base de données et une page de liste simple d’afficher le contenu à partir de celui-ci. Dans le didacticiel suivant, nous allons examiner le reste du code généré automatiquement et ajoutez un `SearchIndex` (méthode) et un `SearchIndex` vue qui vous permet de rechercher des films dans cette base de données.

> [!div class="step-by-step"]
> [Précédent](adding-a-model.md)
> [Suivant](examining-the-edit-methods-and-edit-view.md)
