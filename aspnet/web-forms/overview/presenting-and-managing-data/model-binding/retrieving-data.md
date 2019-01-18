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
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396283"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Récupération et affichage des données avec la liaison de modèle et les web forms
====================

> Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend interaction des données plus simple que vous traitez des données des objets de source (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une partie introductive et progresse vers des concepts plus avancés dans les didacticiels suivants.
> 
>  Le modèle de liaison de modèle fonctionne avec toute technologie d’accès aux données. Dans ce didacticiel, vous allez utiliser Entity Framework, mais vous pouvez utiliser la technologie d’accès aux données qui connaît plus. À partir d’un contrôle serveur lié aux données, tel qu’un contrôle GridView, ListView, DetailsView ou FormView, vous spécifiez les noms des méthodes à utiliser pour la sélection, la mise à jour, la suppression et la création de données. Dans ce didacticiel, vous spécifierez une valeur pour la méthode SelectMethod. 
> 
> Dans cette méthode, vous fournissez la logique de récupération des données. Dans le didacticiel suivant, vous allez définir des valeurs pour UpdateMethod, DeleteMethod et InsertMethod.
>
> Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou Visual Basic. Le code téléchargeable fonctionne avec Visual Studio 2012 et versions ultérieures. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2017 présentée dans ce didacticiel.
> 
> Dans le didacticiel, vous exécutez l’application dans Visual Studio. Vous pouvez également déployer l’application sur un fournisseur d’hébergement et rendez-le accessible sur internet. Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites web dans un  
>  [compte d’essai Azure gratuit](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur Azure App Service Web Apps, consultez le [le déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) série. Ce didacticiel montre également comment utiliser des Migrations Entity Framework Code First pour déployer votre base de données SQL Server vers Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> - Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017
>   
> Ce didacticiel fonctionne également avec Visual Studio 2012 et Visual Studio 2013, mais il existe des différences dans le modèle de projet et d’interface utilisateur.


## <a name="what-youll-build"></a>Vous allez générer

Dans ce didacticiel, vous allez :

* Générer des objets de données qui reflètent une université avec étudiants inscrits aux cours
* Créer des tables de base de données à partir des objets
* Remplir la base de données de test
* Afficher des données dans un formulaire web

## <a name="create-the-project"></a>Créer le projet

1. Dans Visual Studio 2017, créez un **Application Web ASP.NET (.NET Framework)** projet appelé **ContosoUniversityModelBinding**.

   ![créer le projet](retrieving-data/_static/image19.png)

2. Sélectionnez **OK**. La boîte de dialogue Sélectionner un modèle s’affiche.

   ![Sélectionnez les formulaires web](retrieving-data/_static/image3.png)

3. Sélectionnez le **Web Forms** modèle. 

4. Si nécessaire, définissez l’authentification sur **comptes d’utilisateur individuels**. 

5. Sélectionnez **OK** pour créer le projet.

## <a name="modify-site-appearance"></a>Modifier l’apparence du site

   Apporter quelques modifications pour personnaliser l’apparence du site. 
   
   1. Ouvrez le fichier Site.Master.
   
   2. Modifier le titre à afficher **Contoso University** et non **mon Application ASP.NET**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Modifier le texte d’en-tête à partir de **nom de l’Application** à **Contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Modifier les liens d’en-tête de navigation pour les adapter de site. 
   
      Supprimer les liens pour **sur** et **Contact** et, au lieu de cela, lier à un **étudiants** page, que vous allez créer.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Enregistrez Site.Master.

## <a name="add-a-web-form-to-display-student-data"></a>Ajoutez un formulaire web pour afficher les données des étudiants

   1. Dans **l’Explorateur de solutions**, cliquez sur votre projet, sélectionnez **ajouter** , puis **un nouvel élément**. 
   
   2. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **Web Form avec Page maître** modèle et nommez-le **Students.aspx**.

      ![créer la page](retrieving-data/_static/image5.png)

   3. Sélectionnez **Ajouter**.
   
   4. Pour la page maître du formulaire web, sélectionnez **Site.Master**.
   
   5. Sélectionnez **OK**.
   

## <a name="add-the-data-model"></a>Ajouter le modèle de données

Dans le **modèles** dossier, ajoutez une classe nommée **UniversityModels.cs**.

   1. Avec le bouton droit **modèles**, sélectionnez **ajouter**, puis **un nouvel élément**. La boîte de dialogue **Ajouter un nouvel élément** s’affiche.

   2. Dans le menu de navigation de gauche, sélectionnez **Code**, puis **classe**.

      ![créer la classe de modèle](retrieving-data/_static/image20.png)

   3. Nommez la classe **UniversityModels.cs** et sélectionnez **ajouter**.

      Dans ce fichier, définissez la `SchoolContext`, `Student`, `Enrollment`, et `Course` classes comme suit :

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      Le `SchoolContext` dérive de la classe `DbContext`, qui gère la connexion de base de données et les modifications dans les données.

      Dans le `Student` class, notez les attributs appliqués à la `FirstName`, `LastName`, et `Year` propriétés. Ce didacticiel utilise ces attributs pour la validation de données. Pour simplifier le code, que ces propriétés sont marquées avec des attributs de validation des données. Dans un projet réel, vous devez appliquer des attributs de validation à toutes les propriétés ayant besoin de validation.

   4. Enregistrer UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Configurer la base de données basée sur des classes

Ce didacticiel utilise [Migrations Code First](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) pour créer des objets et tables de base de données. Ces tables stockent des informations sur les étudiants et leurs cours.

   1. Sélectionnez **outils** > **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**.

   2. Dans **Console du Gestionnaire de Package**, exécutez la commande suivante :  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Si la commande se termine correctement, un message indiquant que les migrations qui ont été activées s’affiche.

      ![Activer les migrations](retrieving-data/_static/image8.png)

      Notez qu’un fichier nommé *Configuration.cs* a été créé. Le `Configuration` classe a un `Seed` (méthode), qui permettre préremplir les tables de base de données avec des données de test.

## <a name="pre-populate-the-database"></a>Préremplir la base de données

   1. Ouvrez Configuration.cs.
   
   2. Ajoutez le code suivant à la méthode `Seed` . En outre, ajouter un `using` instruction pour la `ContosoUniversityModelBinding. Models` espace de noms.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Enregistrer Configuration.cs.

   4. Dans la Console du Gestionnaire de Package, exécutez la commande **ajouter-migration initial**.

   5. Exécutez la commande **mise à jour la base de données**.

      Si vous recevez une exception lors de l’exécution de cette commande, le `StudentID` et `CourseID` valeurs peuvent être différentes de la `Seed` valeurs de la méthode. Ouvrez ces tables de base de données et rechercher les valeurs existantes pour `StudentID` et `CourseID`. Ajoutez ces valeurs pour le code pour l’amorçage le `Enrollments` table.

## <a name="add-a-gridview-control"></a>Ajouter un contrôle GridView

Avec les données de base de données remplis, vous êtes maintenant prêt à récupérer ces données et les afficher. 

1. Ouvrez Students.aspx.

2. Recherchez le `MainContent` espace réservé. Dans cet espace réservé, ajoutez un **GridView** contrôle qui contient ce code.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Points à noter :
   * Notez que la valeur définie pour le `SelectMethod` propriété dans l’élément de GridView. Cette valeur spécifie la méthode utilisée pour récupérer les données de GridView, que vous créez à l’étape suivante. 
   
   * Le `ItemType` propriété est définie sur la `Student` classe créé précédemment. Ce paramètre vous permet de référencer les propriétés de la classe dans le balisage. Par exemple, le `Student` classe a une collection nommée `Enrollments`. Vous pouvez utiliser `Item.Enrollments` pour récupérer cette collection, puis utilisez [syntaxe LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) pour récupérer chaque étudiant d’inscrits somme de crédits.
   
3. Enregistrer Students.aspx.

## <a name="add-code-to-retrieve-data"></a>Ajoutez du code pour récupérer des données

   Dans le fichier de code-behind Students.aspx, ajoutez la méthode spécifiée pour le `SelectMethod` valeur. 
   
   1. Ouvrez Students.aspx.cs.
   
   2. Ajouter `using` instructions pour le `ContosoUniversityModelBinding. Models` et `System.Data.Entity` espaces de noms.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Ajoutez la méthode que vous avez spécifié pour `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      Le `Include` clause améliore les performances des requêtes mais n’est pas nécessaire. Sans le `Include` clause, les données est récupérée à l’aide de [ *le chargement différé*](https://en.wikipedia.org/wiki/Lazy_loading), ce qui implique l’envoi d’une requête distincte pour la base de données chaque fois liés sont extraites. Avec le `Include` clause, les données est récupérée à l’aide de *chargement hâtif*, ce qui signifie qu’une requête de base de données unique récupère toutes les données liées. Si les données associées n’est pas utilisées, le chargement hâtif est moins efficace, car davantage de données est récupérée. Toutefois, dans ce cas, le chargement hâtif vous donne les meilleures performances car les données associées sont affichées pour chaque enregistrement.

      Pour plus d’informations sur les performances lors du chargement des données associées, consultez le **Lazy, Eager et explicite du chargement de données associées** section dans le [lecture des données associées avec Entity Framework dans ASP.NET Application MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.

      Par défaut, les données sont triées par les valeurs de la propriété marquée comme clé. Vous pouvez ajouter un `OrderBy` clause pour spécifier une valeur de tri différent. Dans cet exemple, la valeur par défaut `StudentID` propriété est utilisée pour le tri. Dans le [tri, pagination et filtrage des données](sorting-paging-and-filtering-data.md) article, l’utilisateur est activé pour sélectionner une colonne de tri.
 
   4. Enregistrer Students.aspx.cs.

## <a name="run-your-application"></a>Exécutez votre application 

Exécuter votre application web (**F5**) et accédez à la **étudiants** page, qui affiche les éléments suivants :

   ![afficher les données](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Génération automatique des méthodes de liaison de modèle

Lorsque vous travaillez via cette série de didacticiels, vous pouvez simplement copier le code à votre projet à partir du didacticiel. Toutefois, l’un des inconvénients de cette approche sont que vous ne pouvez pas avoir connaissance de la fonctionnalité fournie par Visual Studio pour générer automatiquement le code pour les méthodes de liaison de modèle. Lorsque vous travaillez sur vos propres projets, génération de code automatique peut gagner du temps et aide que vous avez une idée de la façon d’implémenter une opération. Cette section décrit la fonctionnalité de génération de code automatique. Cette section est uniquement informatif et ne contient-elle pas de code, que vous devez implémenter dans votre projet. 

Lors de la définition d’une valeur pour le `SelectMethod`, `UpdateMethod`, `InsertMethod`, ou `DeleteMethod` propriétés dans le code de balisage, vous pouvez sélectionner le **créer une nouvelle méthode** option.

![Créez une méthode](retrieving-data/_static/image18.png)

Visual Studio crée une méthode dans le code-behind avec la signature appropriée mais génère également du code d’implémentation pour effectuer l’opération. Si vous définissez tout d’abord le `ItemType` propriété avant d’utiliser la génération de code automatique des fonctionnalités, les utilisations de code généré type pour les opérations. Par exemple, lors de la définition du `UpdateMethod` propriété, le code suivant est générée automatiquement :

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Là encore, ce code n’a pas besoin être ajouté à votre projet. Dans le didacticiel suivant, vous allez implémenter des méthodes pour la mise à jour, la suppression et l’ajout de nouvelles données.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous créé des classes de modèle de données et généré une base de données à partir de ces classes. Vous avez rempli les tables de base de données avec des données de test. Votre liaison de modèle permet de récupérer des données à partir de la base de données, puis affiche les données dans un GridView.

Dans la prochaine [didacticiel](updating-deleting-and-creating-data.md) dans cette série, vous allez activer la mise à jour, la suppression et la création de données.

> [!div class="step-by-step"]
> [Next](updating-deleting-and-creating-data.md)
