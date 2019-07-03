---
title: Migrer à partir de l’API Web ASP.NET vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer d’une implémentation de l’API web à partir de l’API Web ASP.NET 4.x vers ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/10/2018
uid: migration/webapi
ms.openlocfilehash: 74c7730a667ebc979241489733cdace149cacdf2
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555748"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrer à partir de l’API Web ASP.NET vers ASP.NET Core

Par [Scott Addie](https://twitter.com/scott_addie) et [Steve Smith](https://ardalis.com/)

API Web ASP.NET 4.x est un service HTTP qui atteigne un large éventail de clients, y compris les navigateurs et appareils mobiles. ASP.NET Core unifie ASP.NET du 4.x MVC et modèles de l’application API Web dans un modèle de programmation plus simple, appelé ASP.NET Core MVC. Cet article illustre les étapes nécessaires pour migrer à partir de l’API Web ASP.NET 4.x vers ASP.NET Core MVC.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>Passez en revue le projet d’API Web ASP.NET 4.x

En tant que point de départ, cet article utilise le *ProductsApp* projet créé dans [mise en route avec ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). Dans ce projet, un projet d’API Web ASP.NET 4.x simple est configuré comme suit.

Dans *Global.asax.cs*, un appel est effectué pour `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

Le `WebApiConfig` classe se trouve dans le *App_Start* dossier et a statique `Register` méthode :

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

Cette classe configure [routage par attributs](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), bien qu’il n’est pas réellement utilisé dans le projet. Il configure également la table de routage, qui est utilisée par l’API Web ASP.NET. Dans ce cas, les API Web ASP.NET 4.x attend les URL pour correspondre au format `/api/{controller}/{id}`, avec `{id}` est facultatif.

Le *ProductsApp* projet comprend un seul contrôleur. Le contrôleur hérite `ApiController` et contient deux actions :

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

Le `Product` modèle utilisé par `ProductsController` est une classe simple :

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Les sections suivantes expliquent la migration du projet API Web ASP.NET Core MVC.

## <a name="create-destination-project"></a>Créer le projet de destination

Procédez comme suit dans Visual Studio :

* Accédez à **fichier** > **nouveau** > **projet** > **autres Types de projets**  >  **Solutions visual Studio**. Sélectionnez **nouvelle Solution**et nommez la solution *WebAPIMigration*. Cliquez sur le **OK** bouton.
* Ajouter l’existant *ProductsApp* projet à la solution.
* Ajouter un nouveau **Application Web ASP.NET Core** projet à la solution. Sélectionnez le **.NET Core** ciblent framework à partir de la liste déroulante, puis sélectionnez le **API** modèle de projet. Nommez le projet *ProductsCore*, puis cliquez sur le **OK** bouton.

La solution contient maintenant deux projets. Les sections suivantes expliquent de migration le *ProductsApp* le contenu du projet pour le *ProductsCore* projet.

## <a name="migrate-configuration"></a>Migrer la configuration

ASP.NET Core n’utilise pas le *App_Start* dossier ou le *Global.asax* fichier et le *web.config* fichier est ajouté au moment de la publication. *Startup.cs* remplace *Global.asax* et se trouve dans la racine du projet. Le `Startup` classe gère toutes les tâches de démarrage d’application. Pour plus d'informations, consultez <xref:fundamentals/startup>.

Dans ASP.NET Core MVC, le routage par attributs est inclus par défaut lorsque <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> est appelée `Startup.Configure`. Ce qui suit `UseMvc` appeler remplace le *ProductsApp* du projet *app_start/webapiconfig.cs* fichier :

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Migrer des modèles et des contrôleurs

Recopiez les *ProductApp* contrôleur du projet et le modèle utilisé. Procédez comme suit :

1. Copie *Controllers/ProductsController.cs* du projet d’origine vers le nouveau.
1. Copiez l’intégralité de *modèles* dossier du projet d’origine vers le nouveau.
1. Modifier les espaces de noms des fichiers copiés pour correspondre au nouveau nom de projet (*ProductsCore*). Ajuster la `using ProductsApp.Models;` instruction dans *ProductsController.cs* trop.

Génération à ce stade, les résultats de l’application dans un nombre d’erreurs de compilation. Les erreurs se produisent, car les composants suivants n’existent pas dans ASP.NET Core :

* Classe `ApiController`
* Espace de noms `System.Web.Http`
* `IHttpActionResult` Interface

Corrigez les erreurs comme suit :

1. Modification `ApiController` à <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Ajouter `using Microsoft.AspNetCore.Mvc;` pour résoudre le `ControllerBase` référence.
1. Supprimez `using System.Web.Http;`.
1. Modifier le `GetProduct` type de retour de l’action à partir de `IHttpActionResult` à `ActionResult<Product>`.

Simplifier le `GetProduct` l’action `return` instruction à ce qui suit :

```csharp
return product;
```

## <a name="configure-routing"></a>Configurer le routage

Configurer le routage comme suit :

1. Décorer la `ProductsController` classe avec les attributs suivants :

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    L’exemple précédent [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribut configure le modèle de routage attribut du contrôleur. Le [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribut rend le routage d’attributs requis pour toutes les actions dans ce contrôleur.

    Routage par attributs prend en charge les jetons, tels que `[controller]` et `[action]`. Lors de l’exécution, chaque jeton est remplacé par le nom du contrôleur ou de l’action, respectivement, à laquelle l’attribut a été appliqué. Les jetons de réduisent le nombre de chaînes magiques dans le projet. Les jetons Vérifiez également les itinéraires restent synchronisés avec les contrôleurs de correspondants et les actions lorsque automatique renommez refactorisations sont appliquées.
1. Définissez le mode de compatibilité du projet à ASP.NET Core 2.2 :

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    La modification précédente :

    * Est requis pour utiliser le `[ApiController]` attribut au niveau du contrôleur.
    * Adhère à rompre les comportements introduits dans ASP.NET Core 2.2.
1. Activer les demandes HTTP Get pour le `ProductController` actions :
    * Appliquer le [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribut le `GetAllProducts` action.
    * Appliquer le `[HttpGet("{id}")]` attribut le `GetProduct` action.

Après les modifications précédentes et la suppression d’inutilisé `using` instructions, *ProductsController.cs* fichier ressemble à ceci :

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Exécutez le projet migré et accédez à `/api/products`. Une liste complète des trois produits s’affiche. Accédez à `/api/products/1`. Le premier produit s’affiche.

## <a name="compatibility-shim"></a>Correctif de compatibilité

Le [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) bibliothèque fournit un shim de compatibilité pour déplacer les projets d’API Web ASP.NET 4.x vers ASP.NET Core. Le shim de compatibilité étend ASP.NET Core pour prendre en charge un nombre de conventions d’ASP.NET 4.x Web API 2. L’exemple porté précédemment dans ce document est assez basique pour que le shim de compatibilité était inutile. Pour les grands projets, le shim de compatibilité peut être utile pour temporairement combler le fossé de API entre ASP.NET Core et ASP.NET 4.x Web API 2.

Le shim de compatibilité d’API Web est destiné à être utilisé en tant que mesure temporaire pour prendre en charge la migration volumineux API Web les projets ASP.NET 4.x vers ASP.NET Core. Au fil du temps, les projets doivent être mis à jour pour utiliser les modèles ASP.NET Core au lieu d’utiliser le shim de compatibilité.

Les fonctionnalités de compatibilité incluses dans `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluent :

* Ajoute un `ApiController` tapez afin que les types de base des contrôleurs n’aient à mettre à jour.
* Active la liaison de modèle de style Web API. ASP.NET Core MVC de modèle des fonctions de liaison de la même façon à celui d’ASP.NET 4.x MVC 5, par défaut. Les modifications de shim de compatibilité liaison de données pour être plus similaire aux conventions de liaison de modèle de 4.x API Web 2 ASP.NET. Par exemple, les types complexes sont liés automatiquement à partir du corps de demande.
* Étend la liaison de modèle afin que les actions de contrôleur peuvent prendre des paramètres de type `HttpRequestMessage`.
* Ajoute des formateurs de messages autorisant des actions pour retourner des résultats de type `HttpResponseMessage`.
* Ajoute des méthodes de réponse supplémentaires qui ont utilisés pour traiter les réponses Web API 2 actions :
  * `HttpResponseMessage` générateurs de :
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Méthodes de résultat d’action :
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Ajoute une instance de `IContentNegotiator` à l’application de conteneur d’injection (DI) et rend disponibles les types liés à la négociation de contenu à partir de [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). Exemples de ces types de `DefaultContentNegotiator` et `MediaTypeFormatter`.

Pour utiliser le shim de compatibilité :

1. Installer le [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) package NuGet.
1. Inscrire les services du shim de compatibilité avec le conteneur d’injection de dépendances de l’application en appelant `services.AddMvc().AddWebApiConventions()` dans `Startup.ConfigureServices`.
1. Définir achemine les API spécifiques à l’aide de web `MapWebApiRoute` sur le `IRouteBuilder` dans l’application `IApplicationBuilder.UseMvc` appeler.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
