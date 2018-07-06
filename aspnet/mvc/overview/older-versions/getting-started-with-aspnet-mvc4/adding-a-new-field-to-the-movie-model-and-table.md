---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Ajoutez un nouveau champ à la Table et le modèle Movie | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 2435bb50bb8124cce3150ba488ad76c012107b27
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840173"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Ajoutez un nouveau champ à la Table et le modèle Movie
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.


Dans cette section, vous utiliserez des Migrations Entity Framework Code First pour migrer certaines modifications vers les classes de modèle pour la modification est appliquée à la base de données.

Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, Code First ajoute une table à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle qu'a été générée ultérieurement. Si elles ne sont pas synchronisés, Entity Framework génère une erreur. Cela rend plus facile à identifier les problèmes au moment du développement qui peut vous sinon uniquement (par des erreurs obscures) en cours d’exécution.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuration des Migrations Code First pour les modifications apportées au modèle

Si vous utilisez Visual Studio 2012, double-cliquez sur le *Movies.mdf* fichier à partir de l’Explorateur de solutions pour ouvrir l’outil de base de données. Visual Studio Express pour Web affiche l’Explorateur de base de données, que Visual Studio 2012 affichera l’Explorateur de serveurs. Si vous utilisez Visual Studio 2010, utilisez l’Explorateur d’objets SQL Server.

Dans l’outil de base de données (Explorateur de base de données, l’Explorateur de serveurs ou Explorateur d’objets SQL Server), avec le bouton droit, cliquez sur `MovieDBContext` et sélectionnez **supprimer** pour supprimer la base de données de films.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Accédez à l’Explorateur de solutions. Cliquez avec le bouton droit sur le *Movies.mdf* fichier et sélectionnez **supprimer** pour supprimer la base de données de films.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Générez l’application pour vérifier qu’aucune erreur.

À partir de la **outils** menu, cliquez sur **Library Package Manager** , puis **Console du Gestionnaire de Package**.

![Ajouter le Pack Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

Dans le **Console du Gestionnaire de Package** fenêtre à la `PM>` invite Entrez « Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext ».

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

Le **Enable-Migrations** (illustré ci-dessus) de commande crée un *Configuration.cs* fichier dans un nouveau *Migrations* dossier.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio ouvre le *Configuration.cs* fichier. Remplacez le `Seed` méthode dans le *Configuration.cs* fichier par le code suivant :

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Cliquez avec le bouton droit sur la ligne ondulée rouge sous `Movie` et sélectionnez **résoudre** puis **à l’aide de** **MvcMovie.Models ;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Cela ajoute le code suivant à l’aide d’instruction :

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations appelle la `Seed` méthode après chaque migration (autrement dit, l’appel **mise à jour la base de données** dans la Console du Gestionnaire de Package), cette méthode met à jour les lignes qui ont déjà été insérés, ou les insère si elles n’existent pas encore.


**Appuyez sur CTRL-MAJ-B pour générer le projet.** (Les étapes suivantes échoue si votre ne créez pas à ce stade.)

L’étape suivante consiste à créer un `DbMigration` classe pour la migration initiale. Cette migration vers crée une base de données, c’est pourquoi vous avez supprimé le *movie.mdf* fichier à l’étape précédente.

Dans le **Console du Gestionnaire de Package** fenêtre, entrez la commande « add-migration Initial » pour créer la migration initiale. Le nom « Initial » est arbitraire et est utilisé pour nommer le fichier de migration créé.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrations Code First crée un autre fichier de classe dans le *Migrations* dossier (avec le nom *{date}\_Initial.cs* ), et cette classe contient le code qui crée le schéma de base de données. Le nom de fichier de migration est déjà résolu avec un horodatage pour faciliter le classement. Examiner le *{date}\_Initial.cs* fichier, il contient les instructions pour créer la table de films pour la base de données de film. Lorsque vous mettez à jour la base de données dans les instructions ci-dessous, cela *{date}\_Initial.cs* fichier sera exécuté et créer le schéma de base de données. Le **Seed** méthode s’exécute pour remplir la base de données avec des données de test.

Dans le **Console du Gestionnaire de Package**, entrez la commande « update-base de données » pour créer la base de données et exécuter le **Seed** (méthode).

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Si vous obtenez une erreur qui indique une table existe déjà et ne peut pas être créée, il est probablement parce que vous avez exécuté l’application une fois que vous avez supprimé la base de données et avant l’exécution de `update-database`. Dans ce cas, supprimez le *Movies.mdf* à nouveau de fichier et recommencez la `update-database` commande. Si vous obtenez toujours une erreur, supprimez le dossier migrations et le contenu, commencez par les instructions en haut de cette page (qui est de supprimer le *Movies.mdf* de fichier, puis passez à Enable-Migrations).

Exécutez l’application et accédez à la */Movies* URL. Les données d’amorçage s’affiche.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Ajout d’une propriété Rating au modèle Movie

Commencez par ajouter un nouveau `Rating` propriété existant `Movie` classe. Ouvrez le *Models\Movie.cs* fichier, puis ajoutez le `Rating` propriété similaire à celle-ci :

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

L’ensemble `Movie` classe se présente comme le code suivant :

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Générez l’application avec le **Build** &gt; **générer un film** menu de commande ou en appuyant sur CTRL-MAJ-B.

Maintenant que vous avez mis à jour le `Model` (classe), vous devez également mettre à jour le *\Views\Movies\Index.cshtml* et *\Views\Movies\Create.cshtml* afficher des modèles afin d’afficher la nouvelle `Rating`propriété dans la vue du navigateur.

Ouvrez le<em>\Views\Movies\Index.cshtml</em> fichier, puis ajoutez un `<th>Rating</th>` juste après l’en-tête de colonne le <strong>prix</strong> colonne. Ajoutez ensuite une `<td>` colonne vers la fin du modèle pour restituer le `@item.Rating` valeur. Voici quelles mis à jour <em>Index.cshtml</em> ressemble à un modèle de vue :

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Ensuite, ouvrez le *\Views\Movies\Create.cshtml* fichier, puis ajoutez le balisage suivant à la fin du formulaire. Cela restitue une zone de texte afin que vous pouvez spécifier un classement lors de la création d’un nouveau film.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Vous avez maintenant mis à jour le code d’application pour prendre en charge la nouvelle `Rating` propriété.

À présent exécuter l’application et accédez à la */Movies* URL. Lorsque vous effectuez cette opération, cependant, vous verrez les erreurs suivantes :

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Vous voyez cette erreur parce que la mise à jour `Movie` modèle classe dans l’application est maintenant différent de celui du schéma de la `Movie` table de base de données existante. (Il n’existe pas de colonne `Rating` dans la table de base de données.)


Plusieurs approches sont possibles pour résoudre l’erreur :

1. Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle. Cette approche est très pratique lorsque vous effectuez un développement actif sur une base de données de test ; Il vous permet de faire évoluer rapidement le schéma de modèle et de la base de données ensemble. L’inconvénient, cependant, est que le que vous perdez les données existantes dans la base de données, donc vous *ne* à utiliser cette approche sur une base de données de production ! À l’aide d’un initialiseur pour amorcer automatiquement une base de données avec des données de test est souvent un moyen efficace pour développer une application. Pour plus d’informations sur les initialiseurs de base de données Entity Framework, consultez de Tom Dykstra [didacticiel d’ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle. L’avantage de cette approche est que vous conservez vos données. Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.
3. Utilisez Migrations Code First pour mettre à jour le schéma de base de données.


Pour ce didacticiel, nous allons utiliser les migrations Code First.

Mettre à jour la méthode Seed pour qu’elle fournisse une valeur pour la nouvelle colonne. Ouvrez le fichier de migrations\configuration.cs et ajoutez un champ d’évaluation pour chaque objet de film.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Générez la solution, puis ouvrez le **Console du Gestionnaire de Package** fenêtre et entrez la commande suivante :

`add-migration AddRatingMig`

Le `add-migration` commande indique à l’infrastructure de migration pour examiner le modèle de film actuel par le schéma actuel de la base de données de films et de créer le code nécessaire pour migrer la base de données vers le nouveau modèle. Le AddRatingMig est arbitraire et est utilisé pour nommer le fichier de migration. Il est conseillé d’utiliser un nom explicite pour l’étape de migration.

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMIgration` classe dérivée, puis, dans le `Up` (méthode), vous pouvez voir le code qui crée la nouvelle colonne.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Générer la solution, puis entrez la commande « update-database » dans le **Console du Gestionnaire de Package** fenêtre.

L’illustration suivante montre la sortie dans le **Console du Gestionnaire de Package** fenêtre (le cachet de date ajoutant AddRatingMig sera différent.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Exécutez de nouveau l’application et accédez à l’URL /Movies. Vous pouvez voir le nouveau champ de contrôle d’accès.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Cliquez sur le **créer un nouveau** lien permettant d’ajouter un nouveau film. Notez que vous pouvez ajouter un contrôle d’accès.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Cliquez sur **Créer**. Ce nouveau film, y compris l’évaluation, figure désormais dans les liste de films :

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Vous devez également ajouter le `Rating` champ à modifier, les détails et les SearchIndex modèles de vue.

Vous pouvez entrer la commande « update-database » dans le **Console du Gestionnaire de Package** fenêtre à nouveau et aucune modification n’est effectuée, car le schéma correspond au modèle.

Dans cette section, vous avez vu comment vous pouvez modifier les objets de modèle et synchroniser la base de données avec les modifications. Vous avez également appris qu’une méthode pour remplir une base de données nouvellement créée avec les exemples de données, vous pouvez donc essayer des scénarios. Ensuite, nous allons examiner comment vous pouvez ajouter une logique de validation plus riche pour les classes de modèle et activer des règles d’entreprise être appliqué.

> [!div class="step-by-step"]
> [Précédent](examining-the-edit-methods-and-edit-view.md)
> [Suivant](adding-validation-to-the-model.md)
