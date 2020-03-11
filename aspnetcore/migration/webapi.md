---
title: Migrer de API Web ASP.NET vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer une implémentation d’API Web à partir de l’API Web ASP.NET 4. x vers ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/webapi
ms.openlocfilehash: 7f61b78c589fc9d01061b50554e5a639e372c3d8
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661846"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrer de API Web ASP.NET vers ASP.NET Core

Par [Scott Addie](https://twitter.com/scott_addie) et [Steve Smith](https://ardalis.com/)

Une API Web ASP.NET 4. x est un service HTTP qui atteint un large éventail de clients, y compris des navigateurs et des appareils mobiles. ASP.NET Core unifie les modèles d’application MVC et API Web de ASP.NET 4. x en un modèle de programmation plus simple, connu sous le nom de ASP.NET Core MVC. Cet article décrit les étapes nécessaires à la migration de l’API Web ASP.NET 4. x vers ASP.NET Core MVC.

[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Conditions préalables requises

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>Examiner le projet d’API Web ASP.NET 4. x

En guise de point de départ, cet article utilise le projet *ProductsApp* créé dans [prise en main avec API Web ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). Dans ce projet, un simple projet d’API Web ASP.NET 4. x est configuré comme suit.

Dans *global.asax.cs*, un appel est effectué pour `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

La classe `WebApiConfig` se trouve dans le dossier *App_Start* et a une méthode `Register` statique :

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

Cette classe configure le [routage des attributs](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), bien qu’elle ne soit pas réellement utilisée dans le projet. Elle configure également la table de routage, qui est utilisée par API Web ASP.NET. Dans ce cas, l’API Web ASP.NET 4. x s’attend à ce que les URL correspondent au format `/api/{controller}/{id}`, `{id}` étant facultatif.

Le projet *ProductsApp* comprend un contrôleur. Le contrôleur hérite de `ApiController` et contient deux actions :

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

Le modèle de `Product` utilisé par `ProductsController` est une classe simple :

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Les sections suivantes montrent comment migrer le projet d’API Web vers ASP.NET Core MVC.

## <a name="create-destination-project"></a>Créer un projet de destination

Effectuez les étapes suivantes dans Visual Studio :

* Accédez à **fichier** > **nouveau** **projet** >  > **autres types de projets** > **solutions Visual Studio**. Sélectionnez **solution vide**et nommez la solution *WebAPIMigration*. Cliquez sur le bouton **OK** .
* Ajoutez le projet *ProductsApp* existant à la solution.
* Ajoutez un nouveau **ASP.net Core projet d’application Web** à la solution. Sélectionnez le Framework cible **.net Core** dans la liste déroulante, puis sélectionnez le modèle de projet **API** . Nommez le projet *ProductsCore*, puis cliquez sur le bouton **OK** .

La solution contient maintenant deux projets. Les sections suivantes expliquent comment migrer le contenu du projet *ProductsApp* vers le projet *ProductsCore* .

## <a name="migrate-configuration"></a>Migrer la configuration

ASP.NET Core n’utilise pas le dossier *App_Start* ou le fichier *global. asax* , et le fichier *Web. config* est ajouté au moment de la publication. *Startup.cs* est le remplacement de *global. asax* et se trouve à la racine du projet. La classe `Startup` gère toutes les tâches de démarrage de l’application. Pour plus d’informations, consultez <xref:fundamentals/startup>.

Dans ASP.NET Core MVC, le routage des attributs est inclus par défaut lors de l’appel de <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> dans `Startup.Configure`. L’appel App_Start de `UseMvc` suivant remplace le fichier */webapiconfig.cs* du projet *ProductsApp* :

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Migrer les modèles et les contrôleurs

Copiez sur le contrôleur du projet *ProductApp* et le modèle qu’il utilise. Procédez comme suit :

1. Copiez les *contrôleurs/ProductsController. cs* du projet d’origine vers le nouveau.
1. Copiez le dossier *Models* entier du projet d’origine vers le nouveau.
1. Modifiez les espaces de noms des fichiers copiés pour qu’ils correspondent au nouveau nom du projet (*ProductsCore*). Ajustez également l’instruction `using ProductsApp.Models;` dans *ProductsController.cs* .

À ce stade, la génération de l’application génère un certain nombre d’erreurs de compilation. Les erreurs se produisent parce que les composants suivants n’existent pas dans ASP.NET Core :

* Classe `ApiController`
* Espace de noms `System.Web.Http`
* interface `IHttpActionResult`

Corrigez les erreurs comme suit :

1. Remplacez `ApiController` par <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Ajoutez `using Microsoft.AspNetCore.Mvc;` pour résoudre la référence `ControllerBase`.
1. Supprimez `using System.Web.Http;`.
1. Remplacez le type de retour de l’action `GetProduct` `IHttpActionResult` par `ActionResult<Product>`.

Simplifiez l’instruction `return` de l’action `GetProduct` comme suit :

```csharp
return product;
```

## <a name="configure-routing"></a>Configurer le routage

Configurez le routage comme suit :

1. Marquez la classe `ProductsController` avec les attributs suivants :

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    L’attribut [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) précédent configure le modèle de routage des attributs du contrôleur. L’attribut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) rend le routage des attributs obligatoire pour toutes les actions dans ce contrôleur.

    Le routage d’attributs prend en charge les jetons, tels que `[controller]` et `[action]`. Lors de l’exécution, chaque jeton est remplacé par le nom du contrôleur ou de l’action, respectivement, auquel l’attribut a été appliqué. Les jetons réduisent le nombre de chaînes magiques dans le projet. Les jetons garantissent également que les itinéraires restent synchronisés avec les contrôleurs et actions correspondants lorsque les refactorisations de renommage automatique sont appliquées.
1. Définissez le mode de compatibilité du projet sur ASP.NET Core 2,2 :

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    Modification précédente :

    * Est requis pour utiliser l’attribut `[ApiController]` au niveau du contrôleur.
    * Opte pour l’interruption potentielle des comportements introduits dans ASP.NET Core 2,2.
1. Activez les demandes HTTP d’extraction aux actions de `ProductController` :
    * Appliquez l’attribut [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) à l’action `GetAllProducts`.
    * Appliquez l’attribut `[HttpGet("{id}")]` à l’action `GetProduct`.

Après les modifications précédentes et la suppression des instructions `using` inutilisées, le fichier *ProductsController.cs* ressemble à ceci :

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Exécutez le projet migré et accédez à `/api/products`. La liste complète des trois produits s’affiche. Accédez à `/api/products/1`. Le premier produit s’affiche.

## <a name="compatibility-shim"></a>Shim de compatibilité

La bibliothèque [Microsoft. AspNetCore. Mvc. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) fournit un shim de compatibilité pour déplacer des projets d’API Web ASP.net 4. x vers ASP.net core. Le shim de compatibilité étend ASP.NET Core pour prendre en charge un certain nombre de conventions de l’API Web 2 de ASP.NET 4. x. L’exemple porté précédemment dans ce document est suffisamment basique pour que le shim de compatibilité soit inutile. Pour les projets de grande taille, l’utilisation du shim de compatibilité peut être utile pour combler temporairement le fossé entre les API ASP.NET Core et ASP.NET 4. x Web API 2.

Le shim de compatibilité de l’API Web est destiné à être utilisé comme mesure temporaire pour prendre en charge la migration de grands projets d’API Web ASP.NET 4. x vers ASP.NET Core. Au fil du temps, les projets doivent être mis à jour pour utiliser des modèles de ASP.NET Core au lieu de s’appuyer sur le shim de compatibilité.

Les fonctionnalités de compatibilité incluses dans `Microsoft.AspNetCore.Mvc.WebApiCompatShim` sont les suivantes :

* Ajoute un type de `ApiController` afin que les types de base des contrôleurs ne doivent pas être mis à jour.
* Active la liaison de modèle de style API Web. ASP.NET Core liaison de modèle MVC fonctionne de façon similaire à celle de ASP.NET 4. x MVC 5, par défaut. Le shim de compatibilité modifie la liaison de modèle pour qu’elle soit plus semblable aux conventions de liaison de modèle d’API Web 2 ASP.NET 4. x. Par exemple, les types complexes sont automatiquement liés à partir du corps de la demande.
* Étend la liaison de modèle afin que les actions du contrôleur puissent prendre des paramètres de type `HttpRequestMessage`.
* Ajoute des formateurs de message permettant aux actions de retourner des résultats de type `HttpResponseMessage`.
* Ajoute des méthodes de réponse supplémentaires que les actions de l’API Web 2 peuvent avoir utilisées pour répondre aux réponses :
  * générateurs de `HttpResponseMessage` :
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Méthodes de résultat d’action :
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Ajoute une instance de `IContentNegotiator` au conteneur d’injection de dépendances de l’application et met à disposition les types liés à la négociation de contenu à partir de [Microsoft. Aspnet. WebApi. client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). `DefaultContentNegotiator` et `MediaTypeFormatter`sont des exemples de ces types.

Pour utiliser le shim de compatibilité :

1. Installez le package NuGet [Microsoft. AspNetCore. Mvc. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) .
1. Enregistrez les services du shim de compatibilité avec le conteneur DI de l’application en appelant `services.AddMvc().AddWebApiConventions()` dans `Startup.ConfigureServices`.
1. Définissez des itinéraires spécifiques à l’API Web à l’aide de `MapWebApiRoute` sur le `IRouteBuilder` dans l’appel de `IApplicationBuilder.UseMvc` de l’application.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
