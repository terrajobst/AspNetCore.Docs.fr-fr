---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Ajoutez un nouveau champ | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 7326f3d80030cdc5db1c25efc31f59f5b51594f4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827006"
---
<a name="adding-a-new-field"></a>Ajout d’un nouveau champ
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Dans cette section, vous utiliserez des Migrations Entity Framework Code First pour migrer certaines modifications vers les classes de modèle pour la modification est appliquée à la base de données.

Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, Code First ajoute une table à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle qu'a été générée ultérieurement. Si elles ne sont pas synchronisés, Entity Framework génère une erreur. Cela rend plus facile à identifier les problèmes au moment du développement qui peut vous sinon uniquement (par des erreurs obscures) en cours d’exécution.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuration des Migrations Code First pour les modifications apportées au modèle

Accédez à l’Explorateur de solutions. Cliquez avec le bouton droit sur le *Movies.mdf* fichier et sélectionnez **supprimer** pour supprimer la base de données de films. Si vous ne voyez pas le *Movies.mdf* de fichiers, cliquez sur le **afficher tous les fichiers** icône ci-dessous dans le contour rouge.

![](adding-a-new-field/_static/image1.png)

Générez l’application pour vérifier qu’aucune erreur.

À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGet** , puis **Console du Gestionnaire de Package**.

![Ajouter le Pack Man](adding-a-new-field/_static/image2.png)

Dans le **Console du Gestionnaire de Package** fenêtre à la `PM>` invite Entrez

Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

Le **Enable-Migrations** (illustré ci-dessus) de commande crée un *Configuration.cs* fichier dans un nouveau *Migrations* dossier.

![](adding-a-new-field/_static/image4.png)

Visual Studio ouvre le *Configuration.cs* fichier. Remplacez le `Seed` méthode dans le *Configuration.cs* fichier par le code suivant :

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Placez le curseur sur la ligne ondulée rouge sous `Movie` et cliquez sur `Show Potential Fixes` puis cliquez sur **à l’aide de** **MvcMovie.Models ;**

![](adding-a-new-field/_static/image5.png)

Cela ajoute le code suivant à l’aide d’instruction :

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Code First Migrations appelle la `Seed` méthode après chaque migration (autrement dit, l’appel **mise à jour la base de données** dans la Console du Gestionnaire de Package), cette méthode met à jour les lignes qui ont déjà été insérés, ou les insère si elles n’existent pas encore.
> 
> Le [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) méthode dans le code suivant effectue une opération « upsert » :
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Étant donné que le [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) méthode s’exécute avec chaque migration, vous ne pouvez pas simplement insérer des données, car les lignes que vous essayez d’ajouter seront déjà il après la première migration qui crée la base de données. Le «[upsert](http://en.wikipedia.org/wiki/Upsert)« opération empêche les erreurs qui se passerait si vous essayez d’insérer une ligne qui existe déjà, mais elle remplace les modifications apportées aux données que vous avez apportées lors du test de l’application. Avec les données de test dans certaines tables vous ne souhaitez pas que cela se produise : dans certains cas lorsque vous modifiez des données lors du test vous voulez que vos modifications restant après les mises à jour de la base de données. Dans ce cas, vous souhaitez effectuer une opération d’insertion conditionnel : insérer une ligne uniquement si elle n’existe pas déjà.   
> 
> Le premier paramètre passé à la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) méthode spécifie la propriété à utiliser pour vérifier si une ligne existe déjà. Pour les données de film de test que vous fournissez, le `Title` propriété peut être utilisée à cet effet dans la mesure où chaque titre de la liste est unique :
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Ce code part du principe que les titres sont uniques. Si vous ajoutez manuellement un titre en double, vous obtiendrez l’exception suivante la prochaine fois que vous effectuez une migration.   
> 
>  *Séquence contient plusieurs éléments*  
> 
> Pour plus d’informations sur la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) (méthode), consultez [prendre en charge avec une méthode EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Appuyez sur CTRL-MAJ-B pour générer le projet.** (Les étapes suivantes échoue si vous ne créez pas à ce stade.)

L’étape suivante consiste à créer un `DbMigration` classe pour la migration initiale. Cette migration crée une base de données, c’est pourquoi vous avez supprimé le *movie.mdf* fichier à l’étape précédente.

Dans le **Console du Gestionnaire de Package** fenêtre, entrez la commande `add-migration Initial` pour créer la migration initiale. Le nom « Initial » est arbitraire et est utilisé pour nommer le fichier de migration créé.

![](adding-a-new-field/_static/image6.png)

Migrations Code First crée un autre fichier de classe dans le *Migrations* dossier (avec le nom *{date}\_Initial.cs* ), et cette classe contient le code qui crée le schéma de base de données. Le nom de fichier de migration est déjà résolu avec un horodatage pour faciliter le classement. Examiner le *{date}\_Initial.cs* fichier, il contient les instructions pour créer le `Movies` table pour la base de données de film. Lorsque vous mettez à jour la base de données dans les instructions ci-dessous, cela *{date}\_Initial.cs* fichier sera exécuté et créer le schéma de base de données. Le **Seed** méthode s’exécute pour remplir la base de données avec des données de test.

Dans le **Console du Gestionnaire de Package**, entrez la commande `update-database` pour créer la base de données et exécuter le `Seed` (méthode).

![](adding-a-new-field/_static/image7.png)

Si vous obtenez une erreur qui indique une table existe déjà et ne peut pas être créée, il est probablement parce que vous avez exécuté l’application une fois que vous avez supprimé la base de données et avant l’exécution de `update-database`. Dans ce cas, supprimez le *Movies.mdf* à nouveau de fichier et recommencez la `update-database` commande. Si vous obtenez toujours une erreur, supprimez le dossier migrations et le contenu, commencez par les instructions en haut de cette page (qui est de supprimer le *Movies.mdf* de fichier, puis passez à Enable-Migrations). Si vous obtenez encore une générant une erreur, ouvrez l’Explorateur d’objets SQL Server et supprimer la base de données de la liste.

Exécutez l’application et accédez à la */Movies* URL. Les données d’amorçage s’affiche.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Ajout d’une propriété Rating au modèle Movie

Commencez par ajouter un nouveau `Rating` propriété existant `Movie` classe. Ouvrez le *Models\Movie.cs* fichier, puis ajoutez le `Rating` propriété similaire à celle-ci :

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

L’ensemble `Movie` classe se présente comme le code suivant :

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Générez l’application (Ctrl + Maj + B).

Étant donné que vous avez ajouté un nouveau champ à la `Movie` (classe), vous devez également mettre à jour la liaison *liste verte* pour cette nouvelle propriété sera incluse. Mise à jour le `bind` attribut `Create` et `Edit` méthodes d’action à inclure le `Rating` propriété :

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Vous devez aussi mettre à jour les modèles de vue pour afficher, créer et modifier la nouvelle propriété `Rating` dans la vue du navigateur.

Ouvrez le *\Views\Movies\Index.cshtml* fichier, puis ajoutez un `<th>Rating</th>` juste après l’en-tête de colonne le **prix** colonne. Ajoutez ensuite une `<td>` colonne vers la fin du modèle pour restituer le `@item.Rating` valeur. Voici quelles mis à jour *Index.cshtml* ressemble à un modèle de vue :

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Ensuite, ouvrez le *\Views\Movies\Create.cshtml* fichier, puis ajoutez le `Rating` champ avec le balisage suivant distinctive. Cela restitue une zone de texte afin que vous pouvez spécifier un classement lors de la création d’un nouveau film.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Vous avez maintenant mis à jour le code d’application pour prendre en charge la nouvelle `Rating` propriété.

Exécutez l’application et accédez à la */Movies* URL. Lorsque vous effectuez cette opération, cependant, vous verrez les erreurs suivantes :

![](adding-a-new-field/_static/image9.png)  
  
Le modèle soutient le contexte 'MovieDBContext' a changé depuis la création de la base de données. Envisagez d’utiliser les Migrations Code First pour mettre à jour de la base de données (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Vous voyez cette erreur parce que la mise à jour `Movie` modèle classe dans l’application est maintenant différent de celui du schéma de la `Movie` table de base de données existante. (Il n’existe pas de colonne `Rating` dans la table de base de données.)


Plusieurs approches sont possibles pour résoudre l’erreur :

1. Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle. Cette approche est très utile au début du cycle de développement quand vous effectuez un développement actif sur une base de données de test. Elle permet de faire évoluer rapidement le schéma de modèle et de base de données ensemble. L’inconvénient, cependant, est que le que vous perdez les données existantes dans la base de données, donc vous *ne* à utiliser cette approche sur une base de données de production ! L’utilisation d’un initialiseur pour amorcer automatiquement une base de données avec des données de test est souvent un moyen efficace pour développer une application. Pour plus d’informations sur les initialiseurs de base de données Entity Framework, consultez [didacticiel d’ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle. L’avantage de cette approche est que vous conservez vos données. Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.
3. Utilisez Migrations Code First pour mettre à jour le schéma de base de données.


Pour ce didacticiel, nous allons utiliser les migrations Code First.

Mettre à jour la méthode Seed pour qu’elle fournisse une valeur pour la nouvelle colonne. Ouvrez le fichier de migrations\configuration.cs et ajoutez un champ d’évaluation pour chaque objet de film.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Générez la solution, puis ouvrez le **Console du Gestionnaire de Package** fenêtre et entrez la commande suivante :

`add-migration Rating`

Le `add-migration` commande indique à l’infrastructure de migration pour examiner le modèle de film actuel par le schéma actuel de la base de données de films et de créer le code nécessaire pour migrer la base de données vers le nouveau modèle. Le nom *évaluation* est arbitraire et est utilisé pour nommer le fichier de migration. Il est conseillé d’utiliser un nom explicite pour l’étape de migration.

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe dérivée, puis, dans le `Up` (méthode), vous pouvez voir le code qui crée la nouvelle colonne.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Générez la solution, puis entrez le `update-database` commande dans le **Console du Gestionnaire de Package** fenêtre.

L’illustration suivante montre la sortie dans le **Console du Gestionnaire de Package** fenêtre (le cachet de date ajoutant *évaluation* sera différent.)

![](adding-a-new-field/_static/image11.png)

Exécutez de nouveau l’application et accédez à l’URL /Movies. Vous pouvez voir le nouveau champ de contrôle d’accès.

![](adding-a-new-field/_static/image12.png)

Cliquez sur le **créer un nouveau** lien permettant d’ajouter un nouveau film. Notez que vous pouvez ajouter un contrôle d’accès.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Cliquez sur **Créer**. Ce nouveau film, y compris l’évaluation, figure désormais dans les liste de films :

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Maintenant que le projet est à l’aide des migrations, vous ne devez supprimer la base de données lorsque vous ajoutez un nouveau champ, ou sinon mettre à jour le schéma. Dans la section suivante, nous allons apporter d’autres modifications de schéma et utiliser des migrations pour mettre à jour de la base de données.

Vous devez également ajouter le `Rating` champ pour les modèles de vue Edit, détails et Delete.

Vous pouvez entrer la commande « update-database » dans le **Console du Gestionnaire de Package** fenêtre à nouveau et aucun code de migration exécuteriez, étant donné que le schéma correspond au modèle. Toutefois, la « mise à jour-base de données » en cours d’exécution sera exécuté le `Seed` méthode là encore, et si vous avez modifié les données de valeur initiale, les modifications seront perdues, car le `Seed` upserts les données de méthode. Vous pouvez en savoir plus sur la `Seed` méthode dans de Tom Dykstra populaires [didacticiel d’ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Dans cette section, vous avez vu comment vous pouvez modifier les objets de modèle et synchroniser la base de données avec les modifications. Vous avez également appris qu’une méthode pour remplir une base de données nouvellement créée avec les exemples de données, vous pouvez donc essayer des scénarios. Il s’agissait d’une introduction rapide à Code First, consultez [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pour obtenir un didacticiel plus complet sur le sujet. Ensuite, nous allons examiner comment vous pouvez ajouter une logique de validation plus riche pour les classes de modèle et activer des règles d’entreprise être appliqué.

> [!div class="step-by-step"]
> [Précédent](adding-search.md)
> [Suivant](adding-validation.md)
