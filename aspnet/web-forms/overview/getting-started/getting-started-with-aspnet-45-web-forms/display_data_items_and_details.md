---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Afficher les données éléments et les détails | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET avec ASP.NET 4.7 et Microsoft Visual Studio Community 2017 pour le Web
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207432"
---
<a name="display-data-items-and-details"></a>Afficher les éléments de données et les détails
====================
par [Erik Reitan](https://github.com/Erikre)

> Cette série de didacticiels vous enseigne les principes fondamentaux de la création d’une application Web Forms ASP.NET avec ASP.NET 4.7 et Microsoft Visual Studio Community 2017 pour le Web.

Dans ce didacticiel, vous allez apprendre à afficher les éléments de données et les détails des éléments de données avec ASP.NET Web Forms et Entity Framework Code First. Ce didacticiel s’appuie sur le didacticiel précédent de « L’interface utilisateur et Navigation » dans le cadre de la série de didacticiels Wingtip Toys Store. Dans le didacticiel terminé, les produits sur le *ProductsList.aspx* page et les détails d’un produit sur le *ProductDetails.aspx* page sont affichés.

## <a name="what-you-learn"></a>Vous découvrez

- Ajoutez un contrôle de données pour afficher des produits de base de données.
- Se connecter à un contrôle de données vers les données sélectionnées.
- Ajoutez un contrôle de données pour afficher les détails du produit.
- Analyser une valeur de chaîne de requête et l’utiliser pour filtrer les données de la base de données récupérée.

Les fonctionnalités présentées dans ce didacticiel incluent la liaison de modèle et les fournisseurs de valeurs.

## <a name="add-a-data-control-to-display-products"></a>Ajouter un contrôle de données pour afficher des produits
 
Vous avez quelques options pour lier des données à un contrôle serveur. Les plus courantes sont les suivantes :

 * Ajout d’un contrôle de source de données
 * Ajout de code manuellement
 * Implémentation de liaison de modèle

### <a name="use-a-data-source-control-to-bind-data"></a>Utiliser un contrôle de source de données pour lier des données

Ajout d’un contrôle de source de données lie le contrôle de source de données au contrôle qui affiche les données. Avec cette approche, vous pouvez de façon déclarative, plutôt que par programmation, connectez les contrôles côté serveur aux sources de données.

### <a name="code-by-hand-to-bind-data"></a>Code manuellement pour lier des données

En codant manuellement implique :

1. Lecture d’une valeur
2. Vérifier si elle est null
3. Convertir en un type approprié
4. Vérification de la réussite de la conversion
5. Création d’une requête avec la valeur convertie 

Avec cette approche, vous avez un contrôle total sur votre logique d’accès aux données.

### <a name="use-model-binding-to-bind-data"></a>Utilisez la liaison de modèle pour lier des données

Avec la liaison de modèle, vous liez des résultats avec beaucoup moins de code et il vous donne la possibilité de réutiliser les fonctionnalités dans votre application. Il simplifie l’utilisation avec la logique d’accès aux données orientés code tout en fournissant une infrastructure riche, la liaison de données.

## <a name="display-products"></a>Afficher les produits

Dans ce didacticiel, vous utilisez la liaison de modèle pour lier des données. Pour configurer un contrôle de données pour utiliser la liaison de modèle pour sélectionner les données, vous définissez le contrôle `SelectMethod` propriété à une méthode dans le code de la page. Le contrôle de données appelle la méthode au moment opportun dans le cycle de vie de page et lie automatiquement les données retournées. Il est inutile d’appeler explicitement la `DataBind` (méthode).

Utilisez les étapes suivantes, vous modifiez *ProductList.aspx* balisage pour afficher les produits.

1. Dans **l’Explorateur de solutions**, ouvrez *ProductList.aspx*.

2. Remplacez le balisage existant par le balisage suivant : 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Le balisage précédent utilise un **ListView** contrôle nommé `productList` pour afficher les produits.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Avec les styles et modèles, vous définissez la **ListView** contrôle affiche les données. Il est utile pour les données dans une structure à répétition. Bien que cela **ListView** exemple affiche simplement la base de données, vous pouvez également, sans code, permettre aux utilisateurs à modifier, insérer et supprimer des données et de trier et paginer des données.

Lorsque vous définissez la `ItemType` propriété dans le **ListView** contrôler, l’expression de liaison de données `Item` est disponible et le contrôle devienne fortement typé. Comme mentionné dans le didacticiel précédent, vous pouvez sélectionner les détails de l’objet élément avec IntelliSense, telles que la spécification du `ProductName`:

![Afficher les données des éléments et des détails - IntelliSense](display_data_items_and_details/_static/image1.png)

Avec la liaison de modèle, vous spécifiez un `SelectMethod` valeur (`GetProducts`). C’est la méthode que vous ajoutez du code derrière pour afficher les produits à l’étape suivante.

### <a name="add-code-to-display-products"></a>Ajoutez du code pour afficher les produits

Dans cette étape, vous ajoutez le code pour remplir le **ListView** contrôle avec les données de produit de base de données. Le code prend en charge montrant tous les produits et les produits de catégorie individuelle.

1. Dans **l’Explorateur de solutions**, avec le bouton droit *ProductList.aspx* , puis sélectionnez **afficher le Code**.
2. Remplacez le code existant dans le *ProductList.aspx.cs* fichier avec ce :   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Ce code montre le `GetProducts` méthode qui le **ListView** du contrôle `ItemType` références de propriété dans *ProductList.aspx*. Pour limiter les résultats à une catégorie spécifique de la base de données, le code définit le `categoryId` valeur à partir de la chaîne de requête transmise à *ProductList.aspx*. Le `QueryStringAttribute` classe dans le `System.Web.ModelBinding` espace de noms est utilisée pour récupérer la variable de chaîne de requête `id`de valeur. Cela indique à la liaison de modèle à, au moment de l’exécution, lier une valeur de chaîne de requête à la `categoryId` paramètre.

Lorsqu’une catégorie valide (`categoryId`) est passée, les résultats sont limités aux produits de base de données de cette catégorie. Par exemple, si le *ProductsList.aspx* s’agit-il d’URL de la page :

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

La page affiche uniquement les produits où le `categoryId` est égal à `1`.

Tous les produits sont affichés si aucune chaîne de requête n’est passée.

Les sources de valeur pour ces méthodes sont appelées *valeur fournisseurs* (tel que `QueryString`), et les attributs de paramètre qui indiquent le fournisseur de valeur à utiliser sont appelés *les attributs de fournisseur de valeur* () comme `id`). ASP.NET inclut les fournisseurs de valeurs et les attributs de tous les classique Web Forms application utilisateur sources d’entrée. Citons notamment la chaîne de requête, les cookies, valeurs de formulaire, contrôles, état d’affichage, l’état de session et les propriétés de profil. Vous pouvez également écrire des fournisseurs de valeurs personnalisés.

### <a name="run-the-application"></a>Exécuter l'application

Exécutez maintenant l’application pour afficher tous les produits ou des produits d’une catégorie.

1. Dans Visual Studio, appuyez sur **F5** pour exécuter l’application.
 Le navigateur s’ouvre et affiche le *Default.aspx* page.

2. Dans le menu de catégorie de produit, sélectionnez **voitures**.

   Le *ProductList.aspx* page apparaît, affichant uniquement les produits à partir de la **voitures** catégorie. Plus loin dans ce didacticiel, vous afficher les détails du produit.

    ![Afficher les données des éléments et des détails - voitures](display_data_items_and_details/_static/image2.png)

3. Sélectionnez **produits** dans le menu supérieur.
 Le *ProductList.aspx* page affiche maintenant tous les produits. 

    ![Afficher les données des éléments et des détails - produits](display_data_items_and_details/_static/image3.png)

4. Fermez le navigateur et revenir à Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Ajouter un contrôle de données pour afficher les détails du produit

Modifier le *ProductDetails.aspx* balisage que vous avez ajouté dans le didacticiel précédent pour afficher des informations de produit spécifique :

1. Dans **l’Explorateur de solutions**, ouvrez *ProductDetails.aspx*.

2. Remplacez le balisage existant par ce balisage :

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

Ce balisage utilise un **FormView** contrôle pour afficher les détails de produit spécifique. Il utilise des méthodes telles que celles utilisées pour afficher des données dans *ProductList.aspx*. Le **FormView** contrôle est utilisé pour afficher un seul enregistrement à la fois à partir d’une source de données. Lorsque vous utilisez le **FormView** contrôle, vous créez des modèles pour afficher et modifier des valeurs liées aux données. Ces modèles contiennent des contrôles, les expressions de liaison, et mise en forme qui définissent apparence et du fonctionnement du formulaire.

Connexion le balisage précédent à la base de données nécessite du code supplémentaire.

1. Dans **l’Explorateur de solutions**, avec le bouton droit *ProductDetails.aspx* , puis sélectionnez **afficher le Code**.  
   Le *ProductDetails.aspx.cs* fichier s’affiche.

2. Remplacez le code existant par ceci :   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Ce code vérifie pour un «`productID`« valeur de chaîne de requête. Si une valeur valide est trouvée, le produit correspondant s’affiche. Si la chaîne de requête n’est pas trouvée, ou sa valeur n’est pas valide, aucun produit ne s’affiche.

### <a name="run-the-application"></a>Exécuter l'application

Vous pouvez maintenant exécuter l’application pour afficher les détails de produit spécifique en fonction de l’ID de produit.

1. Dans Visual Studio, appuyez sur **F5** pour exécuter l’application.  
 Le navigateur ouvre *Default.aspx*.

2. Dans le menu de la catégorie, sélectionnez **bateaux**.  
 Le *ProductList.aspx* page s’affiche.

3. Sélectionnez **papier bateau**.  
 Le *ProductDetails.aspx* page s’affiche.   

    ![Afficher les données des éléments et des détails - produits](display_data_items_and_details/_static/image4.png)

Dans le didacticiel suivant, vous ajoutez un panier d’achat à l’application Wingtip Toys.

## <a name="additional-resources"></a>Ressources supplémentaires

[Récupération et affichage des données avec la liaison de modèle et les web forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Précédent](ui_and_navigation.md)
> [Suivant](shopping-cart.md)
