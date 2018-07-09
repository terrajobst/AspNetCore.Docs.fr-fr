---
title: Migrer à partir de l’API Web ASP.NET vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer d’une implémentation de l’API Web à partir de l’API Web ASP.NET vers ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894190"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrer à partir de l’API Web ASP.NET vers ASP.NET Core

Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)

API Web sont des services HTTP qui atteignent une large gamme de clients, y compris les navigateurs et appareils mobiles. ASP.NET Core MVC prend en charge pour la création d’API Web offre un moyen unique et homogène de la création d’applications web. Dans cet article, nous montrons les étapes nécessaires pour migrer d’une implémentation de l’API Web à partir de l’API Web ASP.NET vers ASP.NET Core MVC.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Projet d’API Web ASP.NET de révision

Cet article utilise l’exemple de projet *ProductsApp*, créé dans l’article [mise en route avec ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) comme point de départ. Dans ce projet, un projet d’API Web ASP.NET simple est configuré comme suit.

Dans *Global.asax.cs*, un appel est effectué pour `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` est défini dans *App_Start*, et a simplement une ligne statique `Register` méthode :

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Cette classe configure [routage par attributs](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), bien qu’il n’est pas réellement utilisé dans le projet. Il configure également la table de routage, qui est utilisée par l’API Web ASP.NET. Dans ce cas, les API Web ASP.NET s’attendent des URL pour correspondre au format */api/ {controller} / {id}*, avec *{id}* est facultatif.

Le *ProductsApp* projet comprend qu’un seul contrôleur simple, qui hérite de `ApiController` et expose deux méthodes :

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Enfin, le modèle, *produit*, utilisé par le *ProductsApp*, est une classe simple :

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Maintenant que nous avons un projet simple à partir duquel commencer, nous pouvons illustrer comment migrer ce projet d’API Web vers ASP.NET Core MVC.

## <a name="create-the-destination-project"></a>Créer le projet de Destination

À l’aide de Visual Studio, créez une solution vide et nommez-la *WebAPIMigration*. Ajouter l’existant *ProductsApp* de projet à ce dernier, puis ajouter un nouveau projet d’Application Web ASP.NET Core à la solution. Nommez le nouveau projet *ProductsCore*.

![Boîte de dialogue Nouveau projet ouvrir aux modèles Web](webapi/_static/add-web-project.png)

Ensuite, choisissez le modèle de projet API Web. Nous allons migrer le *ProductsApp* le contenu pour ce nouveau projet.

![Boîte de dialogue Nouveau Application Web avec le modèle de projet API Web sélectionné dans la liste des modèles ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Supprimer le `Project_Readme.html` fichier à partir du nouveau projet. Votre solution doit maintenant ressembler à ceci :

![Solution d’application ouverte dans l’Explorateur de solutions affichant les fichiers et dossiers des projets ProductsApp et ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migrer la Configuration

ASP.NET Core n’utilise plus *Global.asax*, *web.config*, ou *App_Start* dossiers. Au lieu de cela, toutes les tâches de démarrage sont effectuent dans *Startup.cs* à la racine du projet (consultez [démarrage de l’Application](../fundamentals/startup.md)). Dans ASP.NET Core MVC, le routage basé sur l’attribut est désormais inclus par défaut lorsque `UseMvc()` est appelée ; et, il s’agit de l’approche recommandée pour la configuration des itinéraires de l’API Web (et comment le projet de démarrage des API Web gère le routage).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

En supposant que vous souhaitez utiliser le routage par attributs dans votre projet à l’avenir, aucune configuration supplémentaire est nécessaire. Simplement appliquer les attributs en fonction des besoins de vos contrôleurs et vos actions, comme dans l’exemple `ValuesController` classe qui est inclus dans le projet de démarrage des API Web :

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Notez la présence de *[controller]* sur la ligne 8. Le routage basé sur l’attribut maintenant prend en charge certains jetons, tels que *[controller]* et *[action]*. Ces jetons sont remplacés lors de l’exécution avec le nom du contrôleur ou de l’action, respectivement, à laquelle l’attribut a été appliqué. Ceci permet de réduire le nombre de chaînes magiques dans le projet, et elle garantit que les itinéraires restent synchronisés avec leurs contrôleurs correspondants et les actions lorsque les refactorisations de changement de nom automatique sont appliquées.

Pour migrer le contrôleur d’API de produits, nous devons tout d’abord copier *ProductsController* vers le nouveau projet. Puis simplement inclure l’attribut d’itinéraire sur le contrôleur :

```csharp
[Route("api/[controller]")]
```

Vous devez également ajouter le `[HttpGet]` d’attribut pour les deux méthodes, dans la mesure où ils sont tous deux doivent être appelées par le biais de HTTP Get. Inclure l’attente d’un paramètre « id » dans l’attribut pour `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

Routage à ce stade, est configuré correctement ; Toutefois, nous ne pouvons pas encore le tester. Modifications supplémentaires doivent être effectuées avant *ProductsController* compilera.

## <a name="migrate-models-and-controllers"></a>Migrer des modèles et des contrôleurs

La dernière étape du processus de migration pour ce projet d’API Web simple consiste à copier sur les contrôleurs et tous les modèles qu’ils utilisent. Dans ce cas, il suffit de copier *Controllers/ProductsController.cs* du projet d’origine vers le nouveau. Ensuite, copiez la totalité du dossier des modèles à partir du projet d’origine vers le nouveau. Ajuster les espaces de noms pour correspondre au nouveau nom de projet (*ProductsCore*).  À ce stade, vous pouvez générer l’application, et vous trouverez un nombre d’erreurs de compilation. Il doivent généralement être comprises dans les catégories suivantes :

* *ApiController* n’existe pas

* *System.Web.Http* espace de noms n’existe pas

* *IHttpActionResult* n’existe pas

Heureusement, il s’agit très faciles à corriger :

* Modification *ApiController* à *contrôleur* (vous devrez peut-être ajouter *à l’aide de Microsoft.AspNetCore.Mvc*)

* Supprimer l’un à l’aide d’instruction qui fait référence à *System.Web.Http*

* Modifier toute méthode retournant *IHttpActionResult* pour retourner un *IActionResult*

Une fois que ces modifications ont été apportées et inutilisées à l’aide d’instructions supprimé, migrées *ProductsController* classe ressemble à ceci :

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Vous devez maintenant être en mesure d’exécuter le projet migré et accédez à */api/produits*; et, vous devez voir la liste complète des 3 produits. Accédez à */api/products/1* et vous devriez voir le premier produit.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>Correctif de compatibilité ASP.NET 4.x Web API 2

Est un outil utile lors de la migration d’ASP.NET Web API projets vers ASP.NET Core la [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) bibliothèque. Le shim de compatibilité étend ASP.NET Core pour autoriser un nombre de différentes conventions d’API Web 2 à utiliser. L’exemple porté précédemment dans ce document est assez basique pour que le shim de compatibilité n’était pas nécessaire. Pour les grands projets, le shim de compatibilité peut être utile pour temporairement combler le fossé de API entre ASP.NET Core et ASP.NET Web API 2.

Le shim de compatibilité d’API Web est destiné à être utilisé en tant que mesure temporaire afin de faciliter la migration de grands projets d’API Web vers ASP.NET Core. Au fil du temps, les projets doivent être mis à jour pour utiliser les modèles ASP.NET Core au lieu d’utiliser le shim de compatibilité.

Les fonctionnalités de compatibilité incluses dans `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluent :

* Ajoute un `ApiController` tapez afin que les types de base des contrôleurs n’aient à mettre à jour.
* Active la liaison de modèle de style Web API. Fonctions de liaison du modèle MVC ASP.NET Core même vers MVC 5, par défaut. Les modifications de shim de compatibilité liaison de données pour être plus similaire aux conventions de liaison de modèle API Web 2. Par exemple, les types complexes sont liés automatiquement à partir du corps de demande.
* Étend la liaison de modèle afin que les actions de contrôleur peuvent prendre des paramètres de type `HttpRequestMessage`.
* Ajoute des formateurs de messages autorisant des actions pour retourner des résultats de type `HttpResponseMessage`.
* Ajoute des méthodes de réponse supplémentaires qui ont utilisés pour traiter les réponses Web API 2 actions :
  * Générateurs de HttpResponseMessage :
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Méthodes de résultat d’action :
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Ajoute une instance de `IContentNegotiator` au conteneur d’injection de dépendances de l’application et les types liés à la négociation à partir du contenu est [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponible. Cela inclut les types tels que `DefaultContentNegotiator`, `MediaTypeFormatter`, etc..

Pour utiliser le shim de compatibilité, vous devez :

* Installer le [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) package NuGet.
* Inscrire les services du shim de compatibilité avec le conteneur d’injection de dépendances de l’application en appelant `services.AddMvc().AddWebApiConventions()` dans l’application `Startup.ConfigureServices` (méthode).
* Définir des itinéraires spécifiques de l’API Web à l’aide de `MapWebApiRoute` sur le `IRouteBuilder` dans l’application `IApplicationBuilder.UseMvc` appeler.

## <a name="summary"></a>Récapitulatif

Migration d’un projet d’API Web ASP.NET simple à ASP.NET Core MVC est assez simple, grâce à la prise en charge intégrée pour les API Web dans ASP.NET Core MVC. Les éléments principaux que doivent faire migrer chaque projet d’API Web ASP.NET sont des itinéraires, contrôleurs et modèles, ainsi que des mises à jour pour les types utilisés par les contrôleurs et les actions.
