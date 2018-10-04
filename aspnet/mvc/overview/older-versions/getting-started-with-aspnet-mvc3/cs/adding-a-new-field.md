---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Ajoutez un nouveau champ à la Table (c#) et le modèle Movie | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 3767ca5f0f32c2a93217d902e200fa2dd3dd262a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578287"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Ajoutez un nouveau champ à la Table (c#) et le modèle Movie
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.
> 
> 
> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec code source c# est disponible pour accompagner cette rubrique. [Téléchargez la version c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez Visual Basic, basculez vers le [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.


Dans cette section, vous allez apporter des modifications pour les classes de modèle et apprendre comment vous pouvez mettre à jour le schéma de base de données pour refléter les modifications de modèle.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Ajout d’une propriété Rating au modèle Movie

Commencez par ajouter un nouveau `Rating` propriété existant `Movie` classe. Ouvrez le *Movie.cs* fichier, puis ajoutez le `Rating` propriété similaire à celle-ci :

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

L’ensemble `Movie` classe se présente comme le code suivant :

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Recompiler l’application à l’aide de la **déboguer** &gt; **générer un film** commande de menu.

Maintenant que vous avez mis à jour le `Model` (classe), vous devez également mettre à jour le *\Views\Movies\Index.cshtml* et *\Views\Movies\Create.cshtml* afficher des modèles pour prendre en charge la nouvelle `Rating`propriété.

Ouvrez le *\Views\Movies\Index.cshtml* fichier, puis ajoutez un `<th>Rating</th>` juste après l’en-tête de colonne le **prix** colonne. Ajoutez ensuite une `<td>` colonne vers la fin du modèle pour restituer le `@item.Rating` valeur. Voici quelles mis à jour *Index.cshtml* ressemble à un modèle de vue :

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Ensuite, ouvrez le *\Views\Movies\Create.cshtml* fichier, puis ajoutez le balisage suivant à la fin du formulaire. Cela restitue une zone de texte afin que vous pouvez spécifier un classement lors de la création d’un nouveau film.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>La gestion de modèle et les différences de schéma de base de données

Vous avez maintenant mis à jour le code d’application pour prendre en charge la nouvelle `Rating` propriété.

À présent exécuter l’application et accédez à la */Movies* URL. Lorsque vous effectuez cette opération, cependant, vous verrez l’erreur suivante :

![](adding-a-new-field/_static/image1.png)

Vous voyez cette erreur parce que la mise à jour `Movie` modèle classe dans l’application est maintenant différent de celui du schéma de la `Movie` table de base de données existante. (Il n’existe pas de colonne `Rating` dans la table de base de données.)

Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, Code First ajoute une table à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle qu'a été générée ultérieurement. Si elles ne sont pas synchronisés, Entity Framework génère une erreur. Cela rend plus facile à identifier les problèmes au moment du développement qui peut vous sinon uniquement (par des erreurs obscures) en cours d’exécution. La fonctionnalité de vérification de la synchronisation est ce qui provoque le message d’erreur à afficher que vous venez de voir.

Il existe deux approches pour résoudre l’erreur :

1. Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle. Cette approche est très pratique lorsque vous effectuez un développement actif sur une base de données de test, car il vous permet de faire évoluer rapidement le schéma de modèle et de la base de données ensemble. L’inconvénient, cependant, est que le que vous perdez les données existantes dans la base de données, donc vous *ne* à utiliser cette approche sur une base de données de production !
2. Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle. L’avantage de cette approche est que vous conservez vos données. Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.

Pour ce didacticiel, nous allons utiliser la première approche, vous aurez Entity Framework Code First recréer automatiquement la base de données chaque fois que le modèle change.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Recréer automatiquement les modifications du modèle de la base de données

Nous allons mettre à jour l’application afin que le Code First automatiquement supprime et recrée la base de données chaque fois que vous modifiez le modèle d’application.

> [!NOTE] 
> 
> **Avertissement** vous devez activer cette approche d’automatiquement et recréer la base de données uniquement lorsque vous utilisez une base de données de test ou de développement et *jamais* sur une base de données de production qui contient des données réelles. L’aide sur un serveur de production peut entraîner une perte de données.


Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le *modèles* dossier, sélectionnez **ajouter**, puis sélectionnez **classe**.

![](adding-a-new-field/_static/image2.png)

Nommez la classe « MovieInitializer ». Mise à jour le `MovieInitializer` classe permettant de contenir le code suivant :

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Le `MovieInitializer` classe spécifie que la base de données utilisée par le modèle doit être supprimée et recréée automatiquement si les classes de modèle sont modifient. Le code inclut un `Seed` méthode pour spécifier des données par défaut pour ajouter automatiquement à la base de données une fois qu’il a créé (ou recréé). Cela fournit un moyen utile pour remplir la base de données avec des exemples de données, sans avoir à remplir manuellement chaque fois que vous modifiez un modèle.

Maintenant que vous avez défini le `MovieInitializer` (classe), vous voudrez raccordées afin que chaque fois que l’application s’exécute, il vérifie si les classes de modèle sont différents du schéma dans la base de données. S’ils sont, vous pouvez exécuter l’initialiseur pour recréer la base de données correspond au modèle et ensuite renseigner la base de données avec les exemples de données.

Ouvrez le *Global.asax* fichier qui est à la racine de la `MvcMovies` projet :

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

Le *Global.asax* fichier contient la classe qui définit l’ensemble de l’application pour le projet et contienne un `Application_Start` Gestionnaire d’événements qui s’exécute lors du premier démarrage de l’application.

Nous allons ajouter deux à l’aide d’instructions au début du fichier. La première fait référence à l’espace de noms Entity Framework, et le second fait référence à l’espace de noms où notre `MovieInitializer` classe vie :

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Recherchez le `Application_Start` (méthode) et ajoutez un appel à `Database.SetInitializer` au début de la méthode, comme indiqué ci-dessous :

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

Le `Database.SetInitializer` instruction que vous venez d’ajouter indique que la base de données utilisée par le `MovieDBContext` instance doit être automatiquement supprimée et recréée si le schéma et la base de données ne correspondent pas. Et comme vous l’avez vu, il remplit également la base de données avec les exemples de données qui sont spécifiés dans le `MovieInitializer` classe.

Fermer le *Global.asax* fichier.

Exécutez de nouveau l’application et accédez à la */Movies* URL. Lorsque l’application démarre, il détecte que la structure du modèle ne correspond plus au schéma de base de données. Automatiquement, il recrée la base de données pour correspondre à la nouvelle structure de modèle et remplit la base de données avec les films d’exemple :

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Cliquez sur le **créer un nouveau** lien permettant d’ajouter un nouveau film. Notez que vous pouvez ajouter un contrôle d’accès.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Cliquez sur **Créer**. Ce nouveau film, y compris l’évaluation, figure désormais dans les liste de films :

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

Dans cette section, vous avez vu comment vous pouvez modifier les objets de modèle et synchroniser la base de données avec les modifications. Vous avez également appris qu’une méthode pour remplir une base de données nouvellement créée avec les exemples de données, vous pouvez donc essayer des scénarios. Ensuite, nous allons examiner comment vous pouvez ajouter une logique de validation plus riche pour les classes de modèle et activer des règles d’entreprise être appliqué.

> [!div class="step-by-step"]
> [Précédent](examining-the-edit-methods-and-edit-view.md)
> [Suivant](adding-validation-to-the-model.md)
