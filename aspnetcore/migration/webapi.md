---
title: Migrer à partir de l’API Web ASP.NET vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer une implémentation de l’API Web à partir de l’API Web ASP.NET vers ASP.NET MVC de base.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 9385805d548bc87f4a50b87f2c06aa74abdaf8af
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272528"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrer à partir de l’API Web ASP.NET vers ASP.NET Core

Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)

API Web est des services HTTP qui atteignent un large éventail de clients, y compris les navigateurs et périphériques mobiles. Base d’ASP.NET MVC prend en charge les API Web offre un moyen unique et homogène de la création d’applications web de génération. Dans cet article, nous allons montrer les étapes requises pour migrer une implémentation de l’API Web à partir de l’API Web ASP.NET vers ASP.NET MVC de base.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Projet d’API Web ASP.NET révision

Cet article utilise l’exemple de projet *ProductsApp*, créé dans l’article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) comme point de départ. Dans ce projet, un projet d’API Web ASP.NET simple est configuré comme suit.

Dans *Global.asax.cs*, un appel est fait à `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` est défini dans *App_Start*, et a simplement une ligne statique `Register` méthode :

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Cette classe configure [attribut routage](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), bien qu’il n’est pas réellement utilisé dans le projet. Il configure également la table de routage, qui est utilisée par l’API Web ASP.NET. Dans ce cas, API Web ASP.NET s’attend à recevoir les URL au format */api/ {controller} / {id}*, avec *{id}* est facultatif.

Le *ProductsApp* projet comprend qu’un seul contrôleur simple, qui hérite de `ApiController` et expose deux méthodes :

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Enfin, le modèle, *produit*, utilisée par le *ProductsApp*, est une classe simple :

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Maintenant que nous avons un projet simple à partir duquel commencer, nous pouvons montrent comment migrer ce projet d’API Web pour ASP.NET MVC de base.

## <a name="create-the-destination-project"></a>Créer le projet de Destination

À l’aide de Visual Studio, créez une solution vide et nommez-la *WebAPIMigration*. Ajouter existant *ProductsApp* de projet à ce dernier, puis ajouter un nouveau projet d’Application Web ASP.NET Core à la solution. Nommez le nouveau projet *ProductsCore*.

![Boîte de dialogue Nouveau projet ouvrir avec des modèles Web](webapi/_static/add-web-project.png)

Ensuite, choisissez le modèle de projet d’API Web. Nous allons migrer le *ProductsApp* le contenu pour ce nouveau projet.

![Boîte de dialogue nouvelle Application Web avec le modèle de projet d’API Web sélectionné dans la liste des modèles ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Supprimer le `Project_Readme.html` le fichier à partir du nouveau projet. Votre solution doit maintenant ressembler à ceci :

![Solution d’application ouverte dans l’Explorateur de solutions affichant les fichiers et dossiers des projets ProductsApp et ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migration de Configuration

ASP.NET Core n’utilise plus *Global.asax*, *web.config*, ou *App_Start* dossiers. Au lieu de cela, toutes les tâches de démarrage sont effectuées en *Startup.cs* à la racine du projet (consultez [démarrage de l’Application](../fundamentals/startup.md)). Dans ASP.NET MVC de base, le routage basé sur l’attribut est à présent inclus par défaut lorsque `UseMvc()` est appelé ; et, il s’agit de l’approche recommandée pour la configuration des itinéraires de l’API Web (et comment le projet de démarrage des API Web gère le routage).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

En supposant que vous souhaitez utiliser le routage d’attributs dans votre projet à l’avenir, aucune configuration supplémentaire n’est nécessaire. Simplement appliquer les attributs en fonction des besoins de vos contrôleurs et vos actions, comme dans l’exemple `ValuesController` classe qui est inclus dans le projet de démarrage des API Web :

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Notez la présence de *[controller]* sur la ligne 8. Le routage basé sur l’attribut maintenant prend en charge certains jetons, tels que *[controller]* et *[action]*. Ces jetons sont remplacés lors de l’exécution avec le nom du contrôleur ou action, respectivement, à laquelle l’attribut a été appliqué. Ceci permet de réduire le nombre de chaînes magiques dans le projet, et il garantit que les itinéraires restent synchronisés avec leurs contrôleurs correspondants et leurs actions lorsque refactorisations de changement de nom automatique sont appliquées.

Pour migrer le contrôleur d’API de produits, nous devons d’abord copier *ProductsController* vers le nouveau projet. Insérez ensuite simplement l’attribut d’itinéraire sur le contrôleur :

```csharp
[Route("api/[controller]")]
```

Vous devez également ajouter le `[HttpGet]` d’attributs pour les deux méthodes, étant donné que les deux doivent être appelés via HTTP Get. Inclure l’attente d’un paramètre « id » dans l’attribut pour `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

À ce stade, le routage est correcte ; Toutefois, nous ne pouvons pas encore le tester. Des modifications supplémentaires doivent être effectuées avant *ProductsController* peut être compilé.

## <a name="migrate-models-and-controllers"></a>Migrer des modèles et des contrôleurs

La dernière étape du processus de migration pour ce projet d’API Web simple consiste à copier sur les contrôleurs et les modèles qu’ils utilisent. Dans ce cas, il suffit de copier *Controllers/ProductsController.cs* à partir du projet d’origine vers le nouveau. Ensuite, copiez la totalité du dossier des modèles de projet d’origine vers le nouveau. Ajuster les espaces de noms pour le nouveau nom de projet (*ProductsCore*).  À ce stade, vous pouvez générer l’application, et vous trouverez un nombre d’erreurs de compilation. Il doivent généralement être comprises dans les catégories suivantes :

* *ApiController* n’existe pas

* *System.Web.Http* espace de noms n’existe pas

* *IHttpActionResult* n’existe pas

Heureusement, il s’agit très faciles à corriger :

* Modification *ApiController* à *contrôleur* (il se pouvez que vous deviez ajouter *à l’aide de Microsoft.AspNetCore.Mvc*)

* Supprimer tout à l’aide d’instruction faisant référence à *System.Web.Http*

* Modifier toute méthode retournant *IHttpActionResult* pour renvoyer un *IActionResult*

Une fois ces modifications ont été apportées et inutilisés à l’aide d’instructions supprimés, migrées *ProductsController* classe ressemble à ceci :

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Vous devez maintenant être en mesure d’exécuter le projet migré et accédez à */api/produits*; et, vous devez voir la liste complète des 3 produits. Accédez à */api/products/1* et vous devez voir le premier produit.

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a>Microsoft.AspNetCore.Mvc.WebApiCompatShim

Est un outil utile lors de la migration d’ASP.NET Web API des projets vers ASP.NET Core le [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) bibliothèque. Le shim de compatibilité étend ASP.NET Core pour autoriser un nombre de différentes conventions d’API Web 2 à utiliser. L’exemple déplacée précédemment dans ce document est suffisamment base pour que le shim de compatibilité n’est pas nécessaire. Pour les projets de grande taille, le shim de compatibilité peut s’avérer utile pour le pontage temporairement l’écart de l’API entre ASP.NET et ASP.NET Web API 2.

Le shim de compatibilité d’API Web est destiné à être utilisé comme une mesure temporaire pour faciliter la migration de grands projets d’API Web pour ASP.NET Core. Au fil du temps, les projets doivent être mis à jour pour utiliser les modèles ASP.NET Core au lieu d’utiliser le shim de compatibilité. 

Les fonctionnalités de compatibilité incluses dans Microsoft.AspNetCore.Mvc.WebApiCompatShim :

* Ajoute un `ApiController` type afin que les types de base des contrôleurs n’avez pas besoin d’être mis à jour.
* Active la liaison de modèle de style Web API. Fonctions de liaison du modèle MVC ASP.NET Core même à MVC 5, par défaut. Les modifications de shim de compatibilité de liaison sont plus semblables aux conventions de liaison de modèle Web API 2 modèle. Par exemple, les types complexes sont automatiquement liés à partir du corps de la demande.
* Étend la liaison de modèle afin que les actions de contrôleur peuvent prendre de paramètres de type `HttpRequestMessage`.
* Ajoute des formateurs de messages permettant d’actions pour retourner des résultats de type `HttpResponseMessage`.
* Ajoute des méthodes supplémentaires de réponse Web API 2 actions avez peut-être utilisé pour traiter les réponses :
    * Générateurs de HttpResponseMessage :
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * Méthodes de résultat d’action :
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* Ajoute une instance de `IContentNegotiator` au conteneur DI de l’application et liées à la négociation des types à partir du contenu est [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponible. Cela inclut les types comme `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.

Pour utiliser le shim de compatibilité, vous devez :

* Référence le [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) package NuGet.
* Inscrire les services du shim de compatibilité avec le conteneur DI de l’application en appelant `services.AddWebApiConventions()` dans l’application `Startup.ConfigureServices` (méthode).
* Définir des itinéraires propre à l’API Web à l’aide de `MapWebApiRoute` sur la `IRouteBuilder` dans l’application `IApplicationBuilder.UseMvc` appeler.

## <a name="summary"></a>Récapitulatif

Migration d’un projet d’API Web ASP.NET simple vers ASP.NET MVC de base est assez simple, grâce à la prise en charge intégrée pour les API Web dans ASP.NET MVC de base. Le principal que doivent faire migrer tous les projets API Web ASP.NET sont itinéraires, les contrôleurs et les modèles, ainsi que des mises à jour pour les types utilisés par les contrôleurs et les actions.
