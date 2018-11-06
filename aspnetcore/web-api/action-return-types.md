---
title: Types de retour des actions du contrôleur dans l’API web ASP.NET Core
author: scottaddie
description: Découvrez comment utiliser les divers types de retour de méthode des actions du contrôleur dans une API Web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: web-api/action-return-types
ms.openlocfilehash: 84300eae4271c3ee4387be022c3576dc83e144eb
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207522"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Types de retour des actions du contrôleur dans l’API web ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

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

Ce document décrit le moment le plus approprié pour utiliser chaque type de retour.

## <a name="specific-type"></a>Type spécifique

L’action la plus simple retourne un type de données primitif ou complexe (par exemple, `string` ou un type d’objet personnalisé). Considérez l’action suivante, qui retourne une collection d’objets `Product` personnalisés :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Sans conditions connues contre lesquelles une protection est nécessaire pendant l’exécution de l’action, le retour d’un type spécifique peut suffire. L’action précédente n’acceptant aucun paramètre, la validation des contraintes de paramètre n’est pas nécessaire.

Quand des conditions connues doivent être prises en compte dans une action, plusieurs chemins de retour sont introduits. Dans ce cas, il est courant de combiner un type de retour [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) avec le type de retour primitif ou complexe. [IActionResult](#iactionresult-type) ou [ActionResult\<T>](#actionresultt-type) sont nécessaires pour prendre en charge ce type d’action.

## <a name="iactionresult-type"></a>Type IActionResult

Le type de retour [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) est approprié quand plusieurs types de retour [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) sont possibles dans une action. Les types `ActionResult` représentent différents codes d’état HTTP. Certains types de retour courants relevant de cette catégorie sont [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) et [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Comme il existe plusieurs types de retour et chemins dans l’action, une utilisation répandue de l’attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) est nécessaire. Cet attribut génère des détails plus descriptifs de la réponse pour les pages d’aide de l’API créées par des outils tels que [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` indique les types connus et les codes d’état HTTP que l’action doit retourner.

### <a name="synchronous-action"></a>Action synchrone

Considérez l’action synchrone suivante pour laquelle il existe deux types de retour possibles :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Dans l’action précédente, un code d’état 404 est retourné quand le produit représenté par `id` n’existe pas dans le magasin de données sous-jacent. La méthode d’assistance [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) est appelée comme raccourci de `return new NotFoundResult();`. Si le produit existe, un objet `Product` représentant la charge utile est retourné avec un code d’état 200. La méthode d’assistance [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) est appelée comme forme abrégée de `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Action asynchrone

Considérez l’action asynchrone suivante pour laquelle il existe deux types de retour possibles :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Dans l’action précédente, un code d’état 400 est retourné en cas d’échec de la validation du modèle et la méthode d’assistance [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) est appelée. Par exemple, le modèle suivant indique que les demandes doivent fournir la propriété `Name` et une valeur. Par conséquent, si vous n’indiquez pas de propriété `Name` appropriée dans la demande, la validation du modèle échoue.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

L’autre code de retour connu de l’action précédente est le code 201, qui est généré par la méthode d’assistance [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Dans ce chemin, l’objet `Product` est retourné.

::: moniker range=">= aspnetcore-2.1"

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

Pour réparer le code précédent, vous pouvez retourner `_repository.GetProducts().ToList();`.

La plupart des actions ont un type de retour spécifique. Des conditions inattendues peuvent se produire pendant l’exécution d’une action, auquel cas le type spécifique n’est pas retourné. Par exemple, le paramètre d’entrée d’une action peut entraîner l’échec de la validation du modèle. Dans ce cas, il est courant de retourner le type `ActionResult` approprié à la place du type spécifique.

### <a name="synchronous-action"></a>Action synchrone

Considérez une action synchrone pour laquelle il existe deux types de retour possibles :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Dans le code précédent, un code d’état 404 est retourné quand le produit n’existe pas dans la base de données. Si le produit existe, l’objet `Product` correspondant est retourné. Avant ASP.NET Core 2.1, la ligne `return product;` aurait indiqué `return Ok(product);`.

> [!TIP]
> Depuis ASP.NET Core 2.1, l’inférence de la source de liaison de paramètre d’action est activée quand une classe de contrôleur est décorée avec l’attribut `[ApiController]`. Un nom de paramètre correspondant à un nom dans le modèle de routage est automatiquement lié à l’aide des données de l’itinéraire de demande. Par conséquent, le paramètre `id` de l’action précédente n’est pas explicitement annoté avec l’attribut [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).

### <a name="asynchronous-action"></a>Action asynchrone

Considérez une action asynchrone pour laquelle il existe deux types de retour possibles :

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

En cas d’échec de la validation du modèle, la méthode [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) est appelée pour retourner un code d’état 400. La propriété [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) contenant les erreurs de validation spécifiques lui est transmise. En cas de réussite de la validation du modèle, le produit est créé dans la base de données. Un code d’état 201 est retourné.

> [!TIP]
> Depuis ASP.NET Core 2.1, l’inférence de la source de liaison de paramètre d’action est activée quand une classe de contrôleur est décorée avec l’attribut `[ApiController]`. Les paramètres de type complexe sont automatiquement liés à l’aide du corps de la demande. Par conséquent, le paramètre `product` de l’action précédente n’est pas explicitement annoté avec l’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
