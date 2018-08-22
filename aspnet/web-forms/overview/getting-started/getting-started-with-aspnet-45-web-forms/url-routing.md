---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Routage d’URL | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: d9f0779d560d6ec7796a16dc2996b959dd171c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825112"
---
<a name="url-routing"></a>Routage d’URL
====================
par [Erik Reitan](https://github.com/Erikre)

[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web. Un Visual Studio 2013 [projet avec du code source c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.


Dans ce didacticiel, vous allez modifier l’exemple d’application Wingtip Toys pour prendre en charge le routage d’URL. Routage permet à votre application web utiliser des URL qui sont mieux pris en charge par les moteurs de recherche, plus facile à mémoriser et convivial. Ce didacticiel s’appuie sur le didacticiel précédent, « L’appartenance et Administration » et fait partie de la série de didacticiels Wingtip Toys.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment inscrire des itinéraires pour une application ASP.NET Web Forms.
- Comment ajouter des itinéraires à une page web.
- Comment sélectionner des données dans une base de données pour prendre en charge des itinéraires.

## <a name="aspnet-routing-overview"></a>Vue d’ensemble du routage ASP.NET

Routage d’URL vous permet de configurer une application pour accepter les URL qui ne sont pas mappent à des fichiers physiques de la requête. Une URL de demande est simplement l’URL un utilisateur entre dans son navigateur pour rechercher une page sur votre site web. Utiliser le routage pour définir des URL qui sont sémantiquement explicites pour les utilisateurs et qui peut faciliter l’optimisation de moteur de recherche (SEO).

Par défaut, le modèle Web Forms inclut [les URL conviviales ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Grande partie du travail de routage de base sera implémentée à l’aide de *URL conviviales*. Toutefois, dans ce didacticiel, vous allez ajouter des fonctionnalités de routage personnalisées.

Avant de personnaliser le routage d’URL, l’exemple d’application Wingtip Toys peut lier à un produit à l’aide de l’URL suivante :

`https://localhost:44300/ProductDetails.aspx?productID=2`

En personnalisant le routage d’URL, l’exemple d’application Wingtip Toys sera liée à la même produit à l’aide d’un plus facile à lire l’URL :

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Routes

Un itinéraire est un modèle d’URL qui est mappé à un gestionnaire. Le gestionnaire peut être un fichier physique, tel qu’un fichier .aspx dans une application Web Forms. Un gestionnaire peut également être une classe qui traite la demande. Pour définir un itinéraire, vous créez une instance de la classe d’itinéraire en spécifiant le modèle d’URL, le gestionnaire et éventuellement un nom pour l’itinéraire.

Ajouter de l’itinéraire à l’application en ajoutant le `Route` objet à la méthode statique `Routes` propriété de la `RouteTable` classe. La propriété d’itinéraires est une `RouteCollection` objet qui stocke tous les itinéraires de l’application.

### <a name="url-patterns"></a>Modèles d’URL

Un modèle d’URL peut contenir des valeurs littérales et divers emplacements réservés (également appelés paramètres d’URL). Les littéraux et les espaces réservés sont situés dans des segments de l’URL qui sont délimités par une barre oblique (`/`) caractères.

Lorsqu’une demande à votre application web est effectuée, l’URL est analysée dans les segments et des espaces réservés, et les valeurs des variables sont fournies pour le Gestionnaire de requête. Ce processus est similaire à la façon dont les données dans une chaîne de requête sont analysées et transmies au Gestionnaire de requêtes. Dans les deux cas, les informations sur les variables sont incluses dans l’URL et passées au gestionnaire sous la forme de paires clé-valeur. Pour les chaînes de requête, les clés et les valeurs sont dans l’URL. Pour les itinéraires, les clés sont les noms d’espace réservé définis dans le modèle d’URL, et seules les valeurs figurent dans l’URL.

Dans un modèle d’URL, vous définissez des espaces réservés en les plaçant entre accolades ( `{` et `}` ). Vous pouvez définir plusieurs espaces réservés dans un segment, mais les espaces réservés doivent être séparés par une valeur littérale. Par exemple, `{language}-{country}/{action}` est un modèle d’itinéraire valide. Toutefois, `{language}{country}/{action}` n’est pas un modèle valide, car il n’existe aucune valeur littérale ou un séparateur entre les espaces réservés. Par conséquent, le routage ne peut pas déterminer où séparer la valeur pour l’espace réservé de langue à partir de la valeur de l’espace réservé de pays.

### <a name="mapping-and-registering-routes"></a>Mappage et l’inscription d’itinéraires

Avant que vous pouvez inclure des itinéraires vers des pages de l’exemple d’application Wingtip Toys, vous devez inscrire les itinéraires lorsque l’application démarre. Pour inscrire les itinéraires, vous allez modifier le `Application_Start` Gestionnaire d’événements.

1. Dans **l’Explorateur de solutions**de Visual Studio, recherchez et ouvrez le *Global.asax.cs* fichier.
2. Ajoutez le code mis en surbrillance en jaune pour le *Global.asax.cs* fichier comme suit :   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Lorsque le Wingtip Toys exemple d’application a démarré, il appelle le `Application_Start` Gestionnaire d’événements. À la fin de ce gestionnaire d’événements, le `RegisterCustomRoutes` méthode est appelée. Le `RegisterCustomRoutes` méthode ajoute chaque itinéraire en appelant le `MapPageRoute` méthode de la `RouteCollection` objet. Les routes sont définies à l’aide d’un nom d’itinéraire, une URL de routage et une URL physique.

Le premier paramètre («`ProductsByCategoryRoute`») est le nom d’itinéraire. Il est utilisé pour appeler l’itinéraire lorsqu’il est nécessaire. Le deuxième paramètre («`Category/{categoryName}`») définit l’URL qui peut être dynamiquement basé sur le code de remplacement convivial. Utilisez cet itinéraire lorsque vous remplissez un contrôle de données avec des liens qui sont générés en fonction des données. Un itinéraire est la suivante :

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Le deuxième paramètre de l’itinéraire inclut une valeur dynamique spécifiée par des accolades (`{ }`). Dans ce cas, le `categoryName` est une variable qui sera utilisée pour déterminer l’itinéraire de routage approprié.

> [!NOTE] 
> 
> **Optional**
> 
> Il peut s’avérer plus facile à gérer votre code en déplaçant le `RegisterCustomRoutes` méthode à une classe distincte. Dans le *logique* dossier, créez un distinct `RouteActions` classe. Déplacer la méthode ci-dessus `RegisterCustomRoutes` méthode à partir de la *Global.asax.cs* fichier dans le nouvel `RoutesActions` classe. Utilisez le `RoleActions` classe et le `createAdmin` un exemple montrant comment appeler la méthode le `RegisterCustomRoutes` méthode à partir de la *Global.asax.cs* fichier.


Vous peut-être également remarqué le `RegisterRoutes` appel de méthode à l’aide du `RouteConfig` objet situé au début de la `Application_Start` Gestionnaire d’événements. Cet appel est effectué pour implémenter le routage par défaut. Il a été inclus en tant que code par défaut lorsque vous avez créé l’application à l’aide du modèle de Web Forms de Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Récupération et à l’aide des données d’itinéraire

Comme mentionné ci-dessus, les itinéraires peuvent être définies. Le code que vous avez ajouté à la `Application_Start` Gestionnaire d’événements dans le *Global.asax.cs* fichier charge les itinéraires définis par le.

### <a name="setting-routes"></a>Définition des itinéraires

Itinéraires vous obligent à ajouter du code. Dans ce didacticiel, vous allez utiliser la liaison de modèle pour récupérer un `RouteValueDictionary` objet qui est utilisé lors de la génération les itinéraires à l’aide de données à partir d’un contrôle de données. Le `RouteValueDictionary` objet contiendra une liste des noms de produits qui appartiennent à une catégorie spécifique de produits. Un lien est créé pour chaque produit basé sur les données et l’itinéraire.

#### <a name="enable-routes-for-categories-and-products"></a>Activer les itinéraires pour les catégories et produits

Ensuite, vous allez mettre à jour l’application pour utiliser le `ProductsByCategoryRoute` pour déterminer l’itinéraire correct à inclure pour chaque lien de catégorie de produit. Vous allez également mettre à jour le *ProductList.aspx* page à inclure un lien routé pour chaque produit. Les liens seront affiche comme ils l’étaient avant la modification, mais les liens seront désormais utiliser le routage d’URL.

1. Dans **l’Explorateur de solutions**, ouvrez le *Site.Master* page si elle n’est pas déjà ouverte.
2. Mise à jour le **ListView** contrôle nommé «`categoryList`» avec les modifications mises en surbrillance en jaune, par conséquent, le balisage se présente comme suit :   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. Dans **l’Explorateur de solutions**, ouvrez le *ProductList.aspx* page.
4. Mise à jour le `ItemTemplate` élément de la *ProductList.aspx* page avec les mises à jour en surbrillance en jaune, donc le balisage se présente comme suit :   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Ouvrez le fichier code-behind de *ProductList.aspx.cs* et ajoutez l’espace de noms suivante en tant que mis en surbrillance en jaune :  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Remplacez le `GetProducts` méthode de code-behind (*ProductList.aspx.cs*) avec le code suivant :   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Ajoutez le Code pour les détails du produit

À présent, mettez à jour le code-behind (*ProductDetails.aspx.cs*) pour le *ProductDetails.aspx* page à utiliser les données d’itinéraire. Notez que la nouvelle `GetProduct` méthode accepte également une valeur de chaîne de requête pour le cas où l’utilisateur a un lien contenant un signet qui utilise l’URL non conviviales, non routé plus anciens.

1. Remplacez le `GetProduct` méthode de code-behind (*ProductDetails.aspx.cs*) avec le code suivant :   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Exécution de l'application

Vous pouvez exécuter l’application maintenant pour afficher les itinéraires de mise à jour.

1. Appuyez sur **F5** pour exécuter l’exemple d’application Wingtip Toys.  
 Le navigateur s’ouvre et affiche le *Default.aspx* page.
2. Cliquez sur le **produits** lien en haut de la page.  
 Tous les produits sont affichés sur le *ProductList.aspx* page. L’URL suivante (à l’aide de votre numéro de port) s’affiche pour le navigateur :  
    `https://localhost:44300/ProductList`
3. Ensuite, cliquez sur le **voitures** lien vers le haut de la page de la catégorie.  
 Uniquement les voitures sont affichées sur le *ProductList.aspx* page. L’URL suivante (à l’aide de votre numéro de port) s’affiche pour le navigateur :  
    `https://localhost:44300/Category/Cars`
4. Cliquez sur le lien contenant le nom de la première voiture répertorié dans la page («**convertibles en voiture**») pour afficher les détails du produit.  
 L’URL suivante (à l’aide de votre numéro de port) s’affiche pour le navigateur :  
    `https://localhost:44300/Product/Convertible%20Car`
5. Ensuite, entrez l’URL suivante non routé (à l’aide de votre numéro de port) dans le navigateur :  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Le code reconnaît toujours une URL qui inclut une chaîne de requête, dans le cas où un utilisateur a un lien de signet.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez ajouté des itinéraires pour les catégories et produits. Vous avez appris comment les itinéraires peuvent être intégrées avec les contrôles de données qui utilisent la liaison de modèle. Dans le didacticiel suivant, vous allez implémenter la gestion des erreurs globales.

## <a name="additional-resources"></a>Ressources supplémentaires

[URL conviviales ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Déployer une application de formulaires Web ASP.NET sécurisée avec appartenance, OAuth et base de données SQL dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - version d’évaluation gratuite](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Précédent](membership-and-administration.md)
> [Suivant](aspnet-error-handling.md)
