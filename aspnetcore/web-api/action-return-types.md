---
title: Types de retour des actions du contrôleur dans l’API web ASP.NET Core
author: scottaddie
description: Découvrez comment utiliser les divers types de retour de méthode des actions du contrôleur dans une API Web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: b89ead55cd46ef62a3bc28b1cfc9077d3ce9aba2
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470410"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Types de retour des actions du contrôleur dans l’API web ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

ASP.NET Core offre les options suivantes pour les types de retour des actions du contrôleur dans l’API web :

::: moniker range=">= aspnetcore-2.1"

* [Type spécifique](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [Type spécifique](#specific-type)
* [IActionResult](#iactionresult-type)

::: moniker-end

Ce document décrit quel type de retour est le plus adapté en fonction de chaque cas.

## <a name="specific-type"></a>Type spécifique

L’action la plus simple retourne un type de données primitif ou complexe (par exemple, `string` ou un type d’objet personnalisé). Considérez l’action suivante, qui retourne une collection d’objets `Product` personnalisés :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Sans conditions connues contre lesquelles une protection est nécessaire pendant l’exécution de l’action, le retour d’un type spécifique peut suffire. L’action précédente n’acceptant aucun paramètre, la validation des contraintes de paramètre n’est pas nécessaire.

Quand des conditions connues doivent être prises en compte dans une action, plusieurs chemins de retour sont introduits. Dans ce cas, il est courant de combiner un type de retour [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) avec le type de retour primitif ou complexe. [IActionResult](#iactionresult-type) ou [ActionResult\<T>](#actionresultt-type) sont nécessaires pour prendre en charge ce type d’action.

## <a name="iactionresult-type"></a>Type IActionResult

Le type de retour [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) est adapté quand plusieurs types de retour [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) sont possibles dans une action. Les types `ActionResult` représentent différents codes d’état HTTP. Certains types de retour courants relevant de cette catégorie sont [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) et [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Comme il existe plusieurs types de retour et chemins dans l’action, une utilisation répandue de l’attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) est nécessaire. Cet attribut génère des détails plus descriptifs de la réponse pour les pages d’aide de l’API créées par des outils tels que [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` indique les types connus et les codes d’état HTTP que l’action doit retourner.

### <a name="synchronous-action"></a>Action synchrone

Considérez l’action synchrone suivante pour laquelle il existe deux types de retour possibles :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Dans l’action précédente, un code d’état 404 est retourné quand le produit représenté par `id` n’existe pas dans le magasin de données sous-jacent. La méthode d’assistance [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) est appelée comme raccourci de `return new NotFoundResult();`. Si le produit existe, un objet `Product` représentant la charge utile est retourné avec un code d’état 200. La méthode d’assistance [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) est appelée comme forme abrégée de `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Action asynchrone

Considérez l’action asynchrone suivante pour laquelle il existe deux types de retour possibles :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Dans le code précédent :

* Un code d’état 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) est retourné par le runtime ASP.NET Core lorsque la description de produit contient « XYZ Widget ».
* Un code d’état 201 est généré par la méthode [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) lors de la création d’un produit. Dans ce chemin de code, l’objet `Product` est retourné.

Par exemple, le modèle suivant indique que les requêtes doivent inclure les propriétés `Name` et `Description`. Par conséquent, si vous n’indiquez pas `Name` et `Description` dans la requête, la validation du modèle échoue.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

Si l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) dans ASP.NET Core 2.1 ou une version ultérieure est appliqué, les erreurs de validation de modèle entraînent un code d’état 400. Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).

## <a name="actionresultt-type"></a>Type ActionResult\<T>

ASP.NET Core 2.1 introduit le type de retour [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) pour les actions du contrôleur dans l’API web. Il vous permet de retourner un type dérivant d’[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) ou de retourner un [type spécifique](#specific-type). `ActionResult<T>` offre les avantages suivants par rapport au [type IActionResult](#iactionresult-type) :

* La propriété `Type` de l’attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) peut être exclue. Par exemple, `[ProducesResponseType(200, Type = typeof(Product))]` est simplifié en `[ProducesResponseType(200)]`. Le type de retour attendu pour l’action est déduit de `T` dans `ActionResult<T>`.
* Des [opérateurs de cast implicite](/dotnet/csharp/language-reference/keywords/implicit) prennent en charge la conversion de `T` et `ActionResult` en `ActionResult<T>`. `T` est converti en [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), ce qui signifie que `return new ObjectResult(T);` est simplifié en `return T;`.

C# ne prend pas en charge les opérateurs de cast implicite sur les interfaces. Par conséquent, `ActionResult<T>` nécessite une conversion de l’interface en un type concret. Par exemple, l’utilisation de `IEnumerable` dans l’exemple suivant ne fonctionne pas :

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

Pour corriger le code précédent, vous pouvez retourner `_repository.GetProducts().ToList();`.

La plupart des actions ont un type de retour spécifique. Des conditions inattendues peuvent se produire pendant l’exécution d’une action, auquel cas le type spécifique n’est pas retourné. Par exemple, le paramètre d’entrée d’une action peut entraîner l’échec de la validation du modèle. Dans ce cas, il est courant de retourner le type `ActionResult` approprié à la place du type spécifique.

### <a name="synchronous-action"></a>Action synchrone

Considérez une action synchrone pour laquelle il existe deux types de retour possibles :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=7,10)]

Dans le code précédent, un code d’état 404 est retourné quand le produit n’existe pas dans la base de données. Si le produit existe, l’objet `Product` correspondant est retourné. Avant ASP.NET Core 2.1, la ligne `return product;` aurait indiqué `return Ok(product);`.

> [!TIP]
> Depuis ASP.NET Core 2.1, l’inférence de la source de liaison de paramètre d’action est activée quand une classe de contrôleur est décorée avec l’attribut `[ApiController]`. Un nom de paramètre correspondant à un nom dans le modèle de routage est automatiquement lié à l’aide des données de l’itinéraire de demande. Par conséquent, le paramètre `id` de l’action précédente n’est pas explicitement annoté avec l’attribut [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).

### <a name="asynchronous-action"></a>Action asynchrone

Considérez une action asynchrone pour laquelle il existe deux types de retour possibles :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Dans le code précédent :

* Un code d’état 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) est retourné par le runtime ASP.NET Core dans les cas suivants :
  * L’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) a été appliqué et la validation du modèle échoue.
  * La description de produit contient « XYZ Widget ».
* Un code d’état 201 est généré par la méthode [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) lors de la création d’un produit. Dans ce chemin de code, l’objet `Product` est retourné.

> [!TIP]
> Depuis ASP.NET Core 2.1, l’inférence de la source de liaison de paramètre d’action est activée quand une classe de contrôleur est décorée avec l’attribut `[ApiController]`. Les paramètres de type complexe sont automatiquement liés à l’aide du corps de la demande. Par conséquent, le paramètre `product` de l’action précédente n’est pas explicitement annoté avec l’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
