---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Récupération et affichage des données avec les formulaires web et de la liaison de modèle | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: b05f3780d7c4e4734b35c0d9377a89d6f3edb0f8
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021337"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Récupération et affichage des données avec la liaison de modèle et les web forms
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend interaction des données plus simple que vous traitez des données des objets de source (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une partie introductive et progresse vers des concepts plus avancés dans les didacticiels suivants.
> 
> Le modèle de liaison de modèle fonctionne avec toute technologie d’accès aux données. Dans ce didacticiel, vous allez utiliser Entity Framework, mais vous pouvez utiliser la technologie d’accès aux données qui connaît plus. À partir d’un contrôle serveur lié aux données, tel qu’un contrôle GridView, ListView, DetailsView ou FormView, vous spécifiez les noms des méthodes à utiliser pour la sélection, la mise à jour, la suppression et la création de données. Dans ce didacticiel, vous spécifierez une valeur pour la méthode SelectMethod.
> 
> Dans cette méthode, vous fournissez la logique de récupération des données. Dans le didacticiel suivant, vous allez définir des valeurs pour UpdateMethod, DeleteMethod et InsertMethod.
> 
> Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 présentée dans ce didacticiel.
> 
> Dans le didacticiel, vous exécutez l’application dans Visual Studio. Vous pouvez également rendre l’application disponible sur Internet en la déployant sur un fournisseur d’hébergement. Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites web dans un  
>  [compte d’essai Azure gratuit](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur Azure App Service Web Apps, consultez le [le déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) série. Ce didacticiel montre également comment utiliser des Migrations Entity Framework Code First pour déployer votre base de données SQL Server vers Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - Microsoft Visual Studio 2013 ou Microsoft Visual Studio Express 2013 pour le Web
>   
> 
> Ce didacticiel fonctionne également avec Visual Studio 2012, mais il y aura des différences dans le modèle de projet et d’interface utilisateur.


## <a name="what-youll-build"></a>Vous allez générer

Dans ce didacticiel, vous allez :

1. Générer des objets de données qui reflètent une université avec étudiants inscrits aux cours
2. Créer des tables de base de données à partir des objets
3. Remplir la base de données de test
4. Afficher des données dans un formulaire web

## <a name="set-up-project"></a>Configuration de projet

Dans Visual Studio 2013, créez un nouveau **Application Web ASP.NET** appelé **ContosoUniversityModelBinding**.

![créer le projet](retrieving-data/_static/image2.png)

Sélectionnez le modèle Web Forms et laissez les autres options par défaut. Cliquez sur OK pour le projet d’installation.

![Sélectionnez les formulaires web](retrieving-data/_static/image3.png)

Tout d’abord, vous effectuerez quelques modifications mineures pour personnaliser l’apparence du site. Ouvrez le **Site.Master** de fichiers et remplacez le titre pour inclure de Contoso University, au lieu de mon Application ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Ensuite, modifiez le texte d’en-tête à partir de **nom de l’Application** à **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Également dans Site.Master, modifier les liens de navigation qui s’affichent dans l’en-tête pour refléter les pages qui s’appliquent à ce site. Vous devez soit le **sur** page ou le **Contact** page, ces liens peut être supprimée. Au lieu de cela, vous devez un lien vers une page appelée **étudiants**. Cette page n’a pas encore été créée.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Enregistrez et fermez Site.Master.

À présent, vous allez créer le formulaire web pour afficher les données de l’étudiant. Cliquez sur votre projet, et **ajouter** un **un nouvel élément**. Sélectionnez le **Web Form avec Page maître** modèle et nommez-le **Students.aspx**.

![créer la page](retrieving-data/_static/image5.png)

Sélectionnez **Site.Master** en tant que la page maître pour le nouveau formulaire web.

## <a name="create-the-data-models-and-database"></a>Créer les modèles de données et de la base de données

Vous allez utiliser les Migrations Code First pour créer des objets et les tables de base de données correspondante. Ces tables stocke les informations sur les étudiants et leurs cours.

Dans le dossier de modèles, ajoutez une nouvelle classe nommée **UniversityModels.cs**.

![créer la classe de modèle](retrieving-data/_static/image7.png)

Dans ce fichier, définissez les classes SchoolContext, étudiants, d’inscription et cours comme suit :

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

La classe SchoolContext dérive de DbContext, qui gère la connexion de base de données et les modifications des données.

Dans la classe Student, notez les attributs qui ont été appliqués à la **FirstName**, **LastName**, et **année** propriétés. Ces attributs sont utilisés pour la validation des données dans ce didacticiel. Pour simplifier le code pour ce projet de démonstration, seules ces propriétés ont été marquées avec des attributs de validation des données. Dans un projet réel, vous devez appliquer des attributs de validation à toutes les propriétés nécessitant des données validées telles que les propriétés dans les classes d’inscription et cours.

Enregistrer UniversityModels.cs.

Vous allez utiliser les outils pour les Migrations Code First pour configurer une base de données en fonction de ces classes.

Dans **Console du Gestionnaire de Package**, exécutez la commande :  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Si la commande se termine correctement, vous recevrez un message indiquant que les migrations ont été activées,

![Activer les migrations](retrieving-data/_static/image8.png)

Notez qu’un nouveau fichier nommé **Configuration.cs** a été créé. Dans Visual Studio, ce fichier s’ouvre automatiquement après sa création. La classe de Configuration contient un **Seed** méthode qui vous permet de préremplir les tables de base de données avec des données de test.

Ajoutez le code suivant à la méthode Seed. Vous devrez ajouter un **à l’aide de** instruction pour la **ContosoUniversityModelBinding.Models** espace de noms.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Enregistrer Configuration.cs.

Dans la Console du Gestionnaire de Package, exécutez la commande `add-migration initial`.

Ensuite, exécutez la commande `update-database`.

Si vous recevez une exception lors de l’exécution de cette commande, il est possible que les valeurs de StudentID et CourseID ont des divers à partir des valeurs dans la méthode Seed. Ouvrez ces tables dans la base de données et rechercher les valeurs existantes pour lequel StudentID et CourseID. Ajoutez ces valeurs dans le code pour l’amorçage de la table d’inscriptions.

Le fichier de base de données a été ajouté, mais il est actuellement masqué dans le projet. Cliquez sur **afficher tous les fichiers** pour afficher le fichier.

![afficher tous les fichiers](retrieving-data/_static/image10.png)

Notez que le fichier .mdf apparaît désormais dans l’application\_dossier de données.

![fichier de base de données](retrieving-data/_static/image12.png)

Double-cliquez sur le fichier .mdf et ouvrez l’Explorateur de serveurs. Désormais, les tables existent et sont remplis avec des données.

![tables de base de données](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Afficher les données d’étudiants et les tables associées

Avec des données dans la base de données, vous êtes maintenant prêt à récupérer ces données et les afficher dans une page web. Vous utiliserez un **GridView** contrôle pour afficher les données dans des colonnes et des lignes.

Ouvrez Students.aspx et recherchez le **MainContent** espace réservé. Dans cet espace réservé, ajoutez un **GridView** contrôle qui inclut le code suivant.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Il existe quelques concepts importants dans ce code de balisage pour vous faire remarquer que. Tout d’abord, notez qu’une valeur est définie pour le **SelectMethod** propriété dans l’élément de GridView. Cette valeur spécifie le nom de la méthode qui est utilisée pour récupérer des données pour le contrôle GridView. Vous allez créer cette méthode à l’étape suivante. En second lieu, notez que le **ItemType** propriété est définie sur la classe Student que vous avez créé précédemment. En définissant cette valeur, vous pouvez faire référence aux propriétés de cette classe dans le code de balisage. Par exemple, la classe Student contient une collection nommée d’inscriptions. Vous pouvez utiliser **Item.Enrollments** pour récupérer cette collection, puis utiliser la syntaxe LINQ pour récupérer la somme des crédits inscrits pour chaque étudiant.

Dans le fichier code-behind, vous devez ajouter la méthode qui est spécifiée pour le **SelectMethod** valeur. Ouvrez **Students.aspx.cs**et ajoutez **à l’aide de** instructions pour le **ContosoUniversityModelBinding.Models** et **System.Data.Entity**espaces de noms.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Ensuite, ajoutez la méthode suivante. Notez que le nom de cette méthode correspond au nom que vous avez fourni pour SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

Le **Include** clause améliore les performances de cette requête, mais n’est pas essentielle pour la requête fonctionne. Sans la clause Include, les données seraient récupérées à l’aide d’un chargement différé, ce qui implique l’envoi de qu'une requête distincte pour la base de données chaque fois liés de données sont récupérées. En fournissant la clause Include, les données sont récupérées à l’aide d’un chargement hâtif, ce qui signifie que toutes les données associées sont récupérées via une requête unique de la base de données. Dans le cas où la plupart des données connexes n’est pas utilisé, hâtif chargement peut être moins efficace, car davantage de données est récupérée. Toutefois, dans ce cas, le chargement hâtif fournit les meilleures performances car les données associées sont affichées pour chaque enregistrement.

Pour plus d’informations sur les questions de performances lors du chargement des données associées, consultez la section intitulée **Lazy, Eager et explicite du chargement de données associées** dans le [lecture des données associées avec Entity Framework dans une page ASP Application de MVC .NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) rubrique.

Par défaut, les données sont triées par les valeurs de la propriété marquée comme clé. Vous pouvez ajouter une clause ORDER BY pour spécifier une valeur différente pour le tri. Dans cet exemple, la propriété de StudentID par défaut est utilisée pour le tri. Dans le [tri, pagination et filtrage des données](sorting-paging-and-filtering-data.md) rubrique, vous allez activer l’utilisateur de sélectionner une colonne de tri.

Exécutez votre application web et accédez à la page des étudiants. La page des étudiants affiche les informations suivantes.

![afficher les données](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Génération automatique des méthodes de liaison de modèle

Lorsque vous travaillez via cette série de didacticiels, vous pouvez simplement copier le code à votre projet à partir du didacticiel. Toutefois, l’un des inconvénients de cette approche sont que vous ne pouvez pas avoir connaissance de la fonctionnalité fournie par Visual Studio pour générer automatiquement le code pour les méthodes de liaison de modèle. Lorsque vous travaillez sur vos propres projets, génération de code automatique peut gagner du temps et aide que vous avez une idée de la façon d’implémenter une opération. Cette section décrit la fonctionnalité de génération de code automatique. Cette section est uniquement informatif et ne contient-elle pas de code, que vous devez implémenter dans votre projet.

Lors de la définition d’une valeur pour le **SelectMethod**, **UpdateMethod**, **InsertMethod**, ou **DeleteMethod** propriétés dans le code de balisage Vous pouvez sélectionner le **créer une nouvelle méthode** option.

![créer la nouvelle méthode](retrieving-data/_static/image18.png)

Visual Studio crée une méthode dans le code-behind avec la signature appropriée mais génère également du code d’implémentation pour vous aider à effectuer l’opération. Si vous définissez tout d’abord le **ItemType** propriété avant d’utiliser la fonctionnalité de génération automatique de code, le code généré utilisera ce type pour les opérations. Par exemple, lorsque vous définissez la propriété UpdateMethod, le code suivant est généré automatiquement :

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Là encore, le code ci-dessus n’a pas besoin être ajouté à votre projet. Dans le didacticiel suivant, vous allez implémenter des méthodes pour la mise à jour, suppression et l’ajout de nouvelles données.

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous créé des classes de modèle de données et généré une base de données à partir de ces classes. Vous avez rempli les tables de base de données avec des données de test. Votre liaison de modèle permet de récupérer des données à partir de la base de données, puis affiche les données dans un GridView.

Dans la prochaine [didacticiel](updating-deleting-and-creating-data.md) dans cette série, vous allez activer la mise à jour, la suppression et la création de données.

> [!div class="step-by-step"]
> [Next](updating-deleting-and-creating-data.md)
