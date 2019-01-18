---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Afficher les données éléments et les détails | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous montrera les principes fondamentaux de la création d’une application Web Forms ASP.NET avec ASP.NET 4.7 et Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396219"
---
<a name="display-data-items-and-details"></a>Afficher les éléments de données et les détails
====================
par [Erik Reitan](https://github.com/Erikre)

> Cette série de didacticiels vous enseigne les principes fondamentaux de la création d’une application Web Forms ASP.NET avec ASP.NET 4.7 et Microsoft Visual Studio 2017.

Dans ce didacticiel, vous allez apprendre à afficher les éléments de données et les détails des éléments de données avec ASP.NET Web Forms et Entity Framework Code First. Ce didacticiel s’appuie sur le didacticiel précédent de « L’interface utilisateur et Navigation » dans le cadre de la série de didacticiels Wingtip Toys Store. À l’issue de ce didacticiel, vous verrez les produits sur le *ProductsList.aspx* page et les détails d’un produit sur le *ProductDetails.aspx* page.

## <a name="youll-learn-how-to"></a>Vous allez apprendre à :

- Ajouter un contrôle de données pour afficher les produits à partir de la base de données
- Connecter un contrôle de données pour les données sélectionnées
- Ajouter un contrôle de données pour afficher les détails du produit à partir de la base de données
- Récupérer une valeur dans la chaîne de requête et utilisez cette valeur pour limiter les données sont récupérées à partir de la base de données

### <a name="features-introduced-in-this-tutorial"></a>Fonctionnalités introduites dans ce didacticiel :

- Liaison de données
- Fournisseurs de valeurs

## <a name="add-a-data-control"></a>Ajouter un contrôle de données

Vous pouvez utiliser plusieurs options pour lier des données à un contrôle serveur. Les plus courantes sont les suivantes :

 * Ajout d’un contrôle de source de données
 * Ajout de code manuellement
 * À l’aide de la liaison de modèle

### <a name="use-a-data-source-control-to-bind-data"></a>Utiliser un contrôle de source de données pour lier des données

Ajout d’un contrôle de source de données vous permet de lier le contrôle de source de données au contrôle qui affiche les données. Avec cette approche, vous pouvez de façon déclarative, plutôt que par programmation, connectez les contrôles côté serveur aux sources de données.

### <a name="code-by-hand-to-bind-data"></a>Code manuellement pour lier des données

En codant manuellement implique :

1. Lecture d’une valeur
2. Vérifier si elle est null
3. Convertir en un type approprié
4. Vérification de la réussite de la conversion
5. À l’aide de la valeur dans la requête 

Cette approche vous permet d’avoir un contrôle total sur votre logique d’accès aux données.

### <a name="use-model-binding-to-bind-data"></a>Utilisez la liaison de modèle pour lier des données

Liaison de modèle vous permet de lier les résultats avec beaucoup moins de code et vous donne la possibilité de réutiliser les fonctionnalités dans votre application. Il simplifie l’utilisation avec la logique d’accès aux données orientés code tout en fournissant une infrastructure riche, la liaison de données.

## <a name="display-products"></a>Afficher les produits

Dans ce didacticiel, vous allez utiliser la liaison de modèle pour lier des données. Pour configurer un contrôle de données pour utiliser la liaison de modèle pour sélectionner les données, vous définissez le contrôle `SelectMethod` propriété à un nom de méthode dans le code de la page. Le contrôle de données appelle la méthode au moment opportun dans le cycle de vie de page et lie automatiquement les données retournées. Il est inutile d’appeler explicitement la `DataBind` (méthode).

1. Dans **l’Explorateur de solutions**, ouvrez *ProductList.aspx*.
2. Remplacez le balisage existant par ce balisage :   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Ce code utilise un **ListView** contrôle nommé `productList` pour afficher les produits.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Avec les styles et modèles, vous définissez la **ListView** contrôle affiche les données. Il est utile pour les données dans une structure à répétition. Bien que cela **ListView** exemple affiche simplement la base de données, vous pouvez également, sans code, permettre aux utilisateurs à modifier, insérer et supprimer des données et de trier et paginer des données.

En définissant le `ItemType` propriété dans le **ListView** contrôler, l’expression de liaison de données `Item` est disponible et le contrôle devienne fortement typé. Comme mentionné dans le didacticiel précédent, vous pouvez sélectionner les détails de l’objet élément avec IntelliSense, telles que la spécification du `ProductName`:

![Afficher les données des éléments et des détails - IntelliSense](display_data_items_and_details/_static/image1.png)

Vous utilisez également une liaison de modèle pour spécifier un `SelectMethod` valeur. Cette valeur (`GetProducts`) correspond à la méthode que vous allez ajouter du code derrière pour afficher les produits à l’étape suivante.

### <a name="add-code-to-display-products"></a>Ajoutez du code pour afficher les produits

Dans cette étape, vous ajouterez du code pour remplir le **ListView** contrôle avec les données de produit à partir de la base de données. Le code prend en charge montrant tous les produits et les produits de catégorie individuelle.

1. Dans **l’Explorateur de solutions**, avec le bouton droit *ProductList.aspx* , puis sélectionnez **afficher le Code**.
2. Remplacez le code existant dans le *ProductList.aspx.cs* fichier avec ce :   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Ce code montre le `GetProducts` méthode qui le **ListView** du contrôle `ItemType` références de propriété dans le *ProductList.aspx* page. Pour limiter les résultats à une catégorie spécifique de la base de données, le code définit le `categoryId` valeur à partir de la valeur de chaîne de requête passée à la *ProductList.aspx* page lorsque le *ProductList.aspx* page est cible de la navigation. Le `QueryStringAttribute` classe dans le `System.Web.ModelBinding` espace de noms est utilisé pour récupérer la valeur de la variable de chaîne de requête `id`. Cela indique à la liaison de modèle pour essayer de lier une valeur de la chaîne de requête à la `categoryId` paramètre en cours d’exécution.

Lorsqu’une catégorie valide est passée comme une chaîne de requête à la page, les résultats de la requête sont limités à ces produits dans la base de données qui correspondent à la `categoryId` valeur. Par exemple, si le *ProductsList.aspx* s’agit-il d’URL de la page :


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

La page affiche uniquement les produits où le `categoryId` est égal à `1`.

Tous les produits sont affichés si aucune chaîne de requête n’est inclus lorsque le *ProductList.aspx* page est appelée.

Les sources des valeurs pour ces méthodes sont appelées *valeur fournisseurs* (tel que *QueryString*), et les attributs de paramètre qui indiquent le fournisseur de valeur à utiliser sont appelés *les attributs de fournisseur de valeur* (tel que `id`). ASP.NET inclut les fournisseurs de valeurs et des attributs correspondants pour toutes les sources d’entrée d’utilisateur standard dans une application Web Forms, telles que la chaîne de requête, les cookies, les valeurs de formulaire, contrôles, état d’affichage, l’état de session et les propriétés de profil. Vous pouvez également écrire des fournisseurs de valeurs personnalisés.

### <a name="run-the-application"></a>Exécuter l'application

Exécutez maintenant l’application pour afficher tous les produits ou des produits d’une catégorie.

1. Appuyez sur **F5** tandis que dans Visual Studio pour exécuter l’application.  
   Le navigateur s’ouvre et affiche le *Default.aspx* page.

2. Sélectionnez **voitures** dans le menu de navigation de catégorie de produit.  
   Le *ProductList.aspx* page affiche uniquement **voitures** produits de la catégorie. Plus loin dans ce didacticiel, vous allez afficher les détails du produit.  

    ![Afficher les données des éléments et des détails - voitures](display_data_items_and_details/_static/image2.png)

3. Sélectionnez **produits** dans le menu de navigation en haut.  
   Là encore, le *ProductList.aspx* page s’affiche, mais cette fois, il indique la liste complète des produits.   

    ![Afficher les données des éléments et des détails - produits](display_data_items_and_details/_static/image3.png)

4. Fermez le navigateur et revenir à Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Ajouter un contrôle de données pour afficher les détails du produit

Ensuite, vous allez modifier le balisage dans le *ProductDetails.aspx* page que vous avez ajouté dans le didacticiel précédent pour afficher des informations de produit spécifique.

1. Dans **l’Explorateur de solutions**, ouvrez *ProductDetails.aspx*.

2. Remplacez le balisage existant par ce balisage :

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Ce code utilise un **FormView** contrôle pour afficher les détails de produit spécifique. Ce balisage utilise des méthodes, telles que les méthodes utilisées pour afficher des données dans le *ProductList.aspx* page. Le **FormView** contrôle est utilisé pour afficher un seul enregistrement à la fois à partir d’une source de données. Lorsque vous utilisez le **FormView** contrôle, vous créez des modèles pour afficher et modifier des valeurs liées aux données. Ces modèles contiennent des contrôles, les expressions de liaison, et mise en forme qui définissent apparence et du fonctionnement du formulaire.

Connexion le balisage précédent à la base de données nécessite du code supplémentaire.

1. Dans **l’Explorateur de solutions**, avec le bouton droit *ProductDetails.aspx* puis cliquez sur **afficher le Code**.  
   Le *ProductDetails.aspx.cs* fichier s’affiche.

2. Remplacez le code existant par ce code :   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Ce code vérifie pour un «`productID`« valeur de chaîne de requête. Si une valeur de chaîne de requête valide est trouvée, le produit correspondant s’affiche. Si la chaîne de requête n’est pas trouvée, ou sa valeur n’est pas valide, aucun produit ne s’affiche.

### <a name="run-the-application"></a>Exécuter l'application

Vous pouvez maintenant exécuter l’application pour voir un produit individuel affiché en fonction des ID de produit.

1. Appuyez sur **F5** tandis que dans Visual Studio pour exécuter l’application.  
   Le navigateur s’ouvre et affiche le *Default.aspx* page.

2. Sélectionnez **bateaux** dans le menu de navigation de catégorie.  
   Le *ProductList.aspx* page s’affiche.

3. Sélectionnez **livre bateau** à partir de la liste des produits.
   Le *ProductDetails.aspx* page s’affiche.

    ![Afficher les données des éléments et des détails - produits](display_data_items_and_details/_static/image4.png)
    
4. Fermez le navigateur.


## <a name="additional-resources"></a>Ressources supplémentaires

[Récupération et affichage des données avec la liaison de modèle et les web forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez ajouté le balisage et le code pour afficher les produits et les détails du produit. Vous avez appris à des contrôles de données fortement typées, de liaison de modèle et de fournisseurs de valeurs. Dans le didacticiel suivant, vous allez ajouter un panier d’achat pour l’exemple d’application Wingtip Toys. 

> [!div class="step-by-step"]
> [Précédent](ui_and_navigation.md)
> [Suivant](shopping-cart.md)
