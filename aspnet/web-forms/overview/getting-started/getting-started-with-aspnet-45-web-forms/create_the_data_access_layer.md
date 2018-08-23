---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Créer la couche Data Access | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: e6ec385c6a4a5507ffae726157f7d52e9c5605da
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836246"
---
<a name="create-the-data-access-layer"></a>Créer la couche d’accès aux données
====================
par [Erik Reitan](https://github.com/Erikre)

[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web. Un Visual Studio 2013 [projet avec du code source c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.


Ce didacticiel explique comment créer, accéder à et passez en revue les données à partir d’une base de données à l’aide de Web Forms ASP.NET et Entity Framework Code First. Ce didacticiel s’appuie sur le didacticiel précédent, « Créer le projet » et fait partie de la série de didacticiels Wingtip Toys Store. Lorsque vous avez terminé ce didacticiel, vous aurez créé un groupe de classes d’accès aux données qui se trouvent dans le *modèles* dossier du projet.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment créer les modèles de données.
- Comment initialiser et amorcer la base de données.
- Comment mettre à jour et configurer l’application pour prendre en charge de la base de données.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Voici les fonctionnalités introduites dans le didacticiel :

- Entity Framework Code First
- LocalDB
- Annotations de données

## <a name="creating-the-data-models"></a>Création des modèles de données

[Entity Framework](https://msdn.microsoft.com/data/aa937723) est une infrastructure de mappage objet-relationnel (ORM). Il vous permet de travailler avec des données relationnelles en tant qu’objets, éliminant la plupart du code d’accès aux données que vous devrez généralement écrire. À l’aide d’Entity Framework, vous pouvez émettre des requêtes à l’aide de [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), puis extraire et manipuler les données en tant qu’objets fortement typés. LINQ fournit des modèles pour l’interrogation et la mise à jour des données. À l’aide d’Entity Framework vous permet de se concentrer sur la création du reste de votre application, plutôt que de se concentrer sur les données de principes de base de l’accès. Plus loin dans cette série de didacticiels, nous allons vous montrer comment utiliser les données pour remplir les requêtes de navigation et de produit.

Entity Framework prend en charge un paradigme de développement appelé *Code First*. Code First vous permet de définir vos modèles de données à l’aide de classes. Une classe est une construction qui vous permet de créer vos propres types personnalisés en regroupant des variables d’autres types, méthodes et des événements. Vous pouvez mapper des classes à une base de données existante ou les utiliser pour générer une base de données. Dans ce didacticiel, vous allez créer les modèles de données en écrivant des classes de modèle de données. Ensuite, vous laissez Entity Framework créer la base de données à la volée à partir de ces nouvelles classes.

Vous allez commencer par créer les classes d’entité qui définissent les modèles de données pour l’application Web Forms. Ensuite, vous allez créer une classe de contexte qui gère les classes d’entité et fournit l’accès à la base de données. Vous allez également créer une classe d’initialiseur que vous allez utiliser pour remplir la base de données.

### <a name="entity-framework-and-references"></a>Références et entity Framework

Par défaut, Entity Framework est inclus lorsque vous créez un nouveau **Application Web ASP.NET** à l’aide de la **Web Forms** modèle. Entity Framework peut être installée, désinstallée et mis à jour sous forme de package NuGet.

Ce package NuGet inclut les éléments suivants **runtime** assemblys au sein de votre projet :

- EntityFramework.dll – tout le code runtime courantes utilisé par Entity Framework
- EntityFramework.SqlServer.dll : le fournisseur de Microsoft SQL Server pour Entity Framework

### <a name="entity-classes"></a>Classes d’entité

Les classes que vous créez pour définir le schéma des données sont appelées classes d’entité. Si vous débutez avec la conception de base de données, considérez les classes d’entité comme des définitions de table de base de données. Chaque propriété de la classe spécifie une colonne dans la table de la base de données. Ces classes fournissent une interface légère, objet-relationnel entre code orienté objet et de la structure de table relationnelle de la base de données.

Dans ce didacticiel, vous commencerez en ajoutant des classes d’entité simple qui représentent les schémas pour les produits et les catégories. La classe products contient les définitions de chaque produit. Le nom de chacun des membres de la classe de produit sera `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, et `Category`. La classe de catégorie contient les définitions de chaque catégorie d’un produit peut appartenir à, telles que la voiture, bateau ou plan. Le nom de chacun des membres de la classe de catégorie sera `CategoryID`, `CategoryName`, `Description`, et `Products`. Chaque produit appartient à une des catégories. Ces classes d’entité seront ajoutés à un élément du projet existant *modèles* dossier.

1. Dans **l’Explorateur de solutions**, avec le bouton droit le *modèles* dossier, puis sélectionnez **ajouter**  - &gt; **un nouvel élément**. 

    ![Créer la couche d’accès aux données - Menu nouvel élément](create_the_data_access_layer/_static/image1.png)

   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sous **Visual C#** à partir de la **installé** volet de gauche, sélectionnez **Code**. 

    ![Créer la couche d’accès aux données - Menu nouvel élément](create_the_data_access_layer/_static/image2.png)
3. Sélectionnez **classe** dans le volet central et nommez cette nouvelle classe *Product.cs*.
4. Cliquez sur **Ajouter**.  
   Le nouveau fichier de classe s’affiche dans l’éditeur.
5. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Créer une autre classe en répétant les étapes 1 à 4, toutefois, nom de la nouvelle classe *Category.cs* et remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Comme mentionné précédemment, le `Category` classe représente le type de produit que l’application est conçue pour vendre (tel que <a id="a"> </a> &quot;voitures&quot;, &quot;bateaux&quot;, &quot;Roquettes&quot;, et ainsi de suite) et le `Product` classe représente les produits individuels (toys) dans la base de données. Chaque instance d’un `Product` objet correspond à une ligne dans une table de base de données relationnelle, et chaque propriété de la classe de produit doit être mappée à une colonne dans la table de base de données relationnelle. Plus loin dans ce didacticiel, vous allez consulter les données de produit contenues dans la base de données.

### <a name="data-annotations"></a>Annotations de données

Vous avez peut-être remarqué que certains membres des classes ont des attributs en spécifiant des détails sur le membre, tel que `[ScaffoldColumn(false)]`. Il s’agit de *annotations de données*. Les attributs d’annotation de données peuvent décrire comment valider l’entrée utilisateur pour ce membre, pour spécifier la mise en forme pour celle-ci et pour spécifier la façon dont elle est modélisée lors de la création de la base de données.

### <a name="context-class"></a>Context, classe

Pour commencer à utiliser les classes pour accéder aux données, vous devez définir une classe de contexte. Comme mentionné précédemment, la classe de contexte gère les classes d’entité (tels que le `Product` classe et la `Category` classe) et fournit l’accès aux données à la base de données.

Cette procédure ajoute une nouveau contexte classe c# à le *modèles* dossier.

1. Cliquez sur le *modèles* dossier, puis sélectionnez **ajouter**  - &gt; **un nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez **classe** dans le volet central, nommez-le *ProductContext.cs* et cliquez sur **ajouter**.
3. Remplacez le code par défaut contenu dans la classe par le code suivant :   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Ce code ajoute la `System.Data.Entity` espace de noms afin que vous avez accès à toutes les fonctionnalités principales d’Entity Framework, qui inclut la capacité à interroger, insérer, mettre à jour et supprimer des données en travaillant avec des objets fortement typés.

Le `ProductContext` classe représente le contexte de base de données produit Entity Framework, qui gère l’extraction, le stockage et la mise à jour `Product` instances dans la base de données de la classe. Le `ProductContext` classe dérive le `DbContext` fourni par Entity Framework de classe de base.

### <a name="initializer-class"></a>Classe d’initialiseur

Vous devrez exécuter une logique personnalisée pour initialiser la base de données la première utilisation le contexte. Ainsi, les données d’amorçage à ajouter à la base de données afin que vous pouvez afficher immédiatement les produits et des catégories.

Cette procédure ajoute une nouvel initialiseur classe c# à le *modèles* dossier.

1. Créer un autre `Class` dans le *modèles* dossier et nommez-le *ProductDatabaseInitializer.cs*.
2. Remplacez le code par défaut contenu dans la classe par le code suivant :   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Comme vous pouvez le voir dans le code ci-dessus, lors de la base de données est créé et initialisé, le `Seed` substitution et de set de propriété. Lorsque le `Seed` propriété est définie, les valeurs de catégories et des produits sont utilisées pour remplir la base de données. Si vous tentez de mettre à jour les données d’amorçage en modifiant le code ci-dessus, une fois que la base de données a été créé, vous ne voyez aucune mise à jour lorsque vous exécutez l’application Web. La raison est que le code ci-dessus utilise une implémentation de la `DropCreateDatabaseIfModelChanges` classe reconnaître si le modèle (schéma) a changé avant de réinitialiser les données d’amorçage. Si aucune modification n’est apportée à la `Category` et `Product` classes d’entité, la base de données ne seront pas réinitialisés avec les données d’amorçage.

> [!NOTE] 
> 
> Si vous souhaitez que la base de données des recréés chaque fois que vous avez exécuté l’application, vous pouvez utiliser la `DropCreateDatabaseAlways` classe au lieu du `DropCreateDatabaseIfModelChanges` classe. Toutefois, pour cette série de didacticiels, utilisez la `DropCreateDatabaseIfModelChanges` classe.


À ce stade dans ce didacticiel, vous aurez un *modèles* dossier avec quatre nouvelles classes et une classe par défaut :

![Créer la couche Data Access - dossier de modèles](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configuration de l’Application à utiliser le modèle de données

Maintenant que vous avez créé les classes qui représentent les données, vous devez configurer l’application pour utiliser les classes. Dans le *Global.asax* fichier, vous ajoutez le code qui initialise le modèle. Dans le *Web.config* fichier que vous ajoutez les informations qui indique à l’application de base de données que vous utiliserez pour stocker les données qui sont représentées par les nouvelles classes de données. Le *Global.asax* fichier peut être utilisé pour gérer les événements d’application ou de méthodes. Le *Web.config* fichier vous permet de contrôler la configuration de votre application web ASP.NET.

#### <a name="updating-the-globalasax-file"></a>La mise à jour le fichier Global.asax

Pour initialiser les modèles de données lorsque l’application démarre, vous mettrez à jour la `Application_Start` gestionnaire dans le *Global.asax.cs* fichier.

> [!NOTE] 
> 
> Dans l’Explorateur de solutions, vous pouvez sélectionner le *Global.asax* fichier ou le *Global.asax.cs* fichier pour modifier le *Global.asax.cs* fichier.


1. Ajoutez le code suivant mis en surbrillance en jaune pour le `Application_Start` méthode dans le *Global.asax.cs* fichier.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Votre navigateur doit prendre en charge HTML5 pour afficher le code mis en surbrillance en jaune, lors de l’affichage de cette série de didacticiels dans un navigateur.


Comme indiqué dans le code ci-dessus, lorsque l’application démarre, l’application spécifie l’initialiseur qui s’exécute pendant la première fois les données est accessible. Les deux espaces de noms supplémentaires sont nécessaires pour accéder à la `Database` objet et le `ProductDatabaseInitializer` objet.

 Modification du fichier Web.Config 

Bien que de Entity Framework Code First génère une base de données pour vous dans un emplacement par défaut lorsque la base de données est remplie avec les données d’amorçage, l’ajout de vos propres informations de connexion à votre application vous donne le contrôle de l’emplacement de la base de données. Vous spécifiez cette connexion de base de données à l’aide d’une chaîne de connexion dans l’application *Web.config* fichier à la racine du projet. En ajoutant une nouvelle chaîne de connexion, vous pouvez diriger l’emplacement de la base de données (*wingtiptoys.mdf*) à générer dans le répertoire de données de l’application (*application\_données*), plutôt que sa valeur par défaut emplacement. Cette modification vous permettra de trouver et d’inspecter le fichier de base de données plus loin dans ce didacticiel.

1. Dans **l’Explorateur de solutions**, recherchez et ouvrez le *Web.config* fichier.
2. Ajoutez la chaîne de connexion mises en surbrillance en jaune pour le `<connectionStrings>` section de la *Web.config* fichier comme suit :  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Lorsque l’application est exécutée pour la première fois, qu’il va générer la base de données à l’emplacement spécifié par la chaîne de connexion. Mais avant d’exécuter l’application, nous allons générer en premier.

## <a name="building-the-application"></a>Génération de l'application

Pour vous assurer que toutes les classes et les modifications apportées à votre application Web fonctionnent, vous devez créer l’application.

1. À partir de la **déboguer** menu, sélectionnez **WingtipToys Build**.  
 Le **sortie** fenêtre s’affiche, et si tout va bien, vous voyez un *a réussi* message.  

    ![Créer la couche Data Access - sortie Windows](create_the_data_access_layer/_static/image4.png)

Si vous rencontrez une erreur, vérifiez à nouveau les étapes ci-dessus. Les informations contenues dans le **sortie** fenêtre indique le fichier qui a un problème et où une modification est nécessaire dans le fichier. Ces informations permettent de déterminer quelle partie de la procédure ci-dessus doivent être examinés et corrigés dans votre projet.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel de la série vous avez créé le modèle de données, mais aussi, ajouté le code qui sera utilisé pour initialiser et amorcer la base de données. Vous avez également configuré l’application pour utiliser les modèles de données lorsque l’application est exécutée.

Dans le didacticiel suivant, vous allez mettre à jour de l’interface utilisateur, ajouter la navigation et récupérer des données à partir de la base de données. Ainsi, la base de données créée automatiquement selon les classes d’entité que vous avez créé dans ce didacticiel.

## <a name="additional-resources"></a>Ressources supplémentaires

[Présentation d’Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Guide du débutant pour ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Code premier développement avec Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (vidéo)   
[API Fluent Code First relations](https://msdn.microsoft.com/data/hh134698)   
[Annotations de données Code First](https://msdn.microsoft.com/data/gg193958)  
[Améliorations de productivité pour Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Précédent](create-the-project.md)
> [Suivant](ui_and_navigation.md)
