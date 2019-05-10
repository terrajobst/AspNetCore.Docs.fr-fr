---
title: Types de retour des actions du contrôleur dans l’API web ASP.NET Core
author: scottaddie
description: Découvrez comment utiliser les divers types de retour de méthode des actions du contrôleur dans une API Web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: 180d76c2c2e53dbf64b8fcc5cdc6d2b6f4dab6eb
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887114"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="cc79f-103">Types de retour des actions du contrôleur dans l’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc79f-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="cc79f-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="cc79f-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="cc79f-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cc79f-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cc79f-106">ASP.NET Core offre les options suivantes pour les types de retour des actions du contrôleur dans l’API web :</span><span class="sxs-lookup"><span data-stu-id="cc79f-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="cc79f-107">Type spécifique</span><span class="sxs-lookup"><span data-stu-id="cc79f-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="cc79f-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="cc79f-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="cc79f-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="cc79f-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="cc79f-110">Type spécifique</span><span class="sxs-lookup"><span data-stu-id="cc79f-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="cc79f-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="cc79f-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="cc79f-112">Ce document décrit quel type de retour est le plus adapté en fonction de chaque cas.</span><span class="sxs-lookup"><span data-stu-id="cc79f-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="cc79f-113">Type spécifique</span><span class="sxs-lookup"><span data-stu-id="cc79f-113">Specific type</span></span>

<span data-ttu-id="cc79f-114">L’action la plus simple retourne un type de données primitif ou complexe (par exemple, `string` ou un type d’objet personnalisé).</span><span class="sxs-lookup"><span data-stu-id="cc79f-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="cc79f-115">Considérez l’action suivante, qui retourne une collection d’objets `Product` personnalisés :</span><span class="sxs-lookup"><span data-stu-id="cc79f-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="cc79f-116">Sans conditions connues contre lesquelles une protection est nécessaire pendant l’exécution de l’action, le retour d’un type spécifique peut suffire.</span><span class="sxs-lookup"><span data-stu-id="cc79f-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="cc79f-117">L’action précédente n’acceptant aucun paramètre, la validation des contraintes de paramètre n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="cc79f-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="cc79f-118">Quand des conditions connues doivent être prises en compte dans une action, plusieurs chemins de retour sont introduits.</span><span class="sxs-lookup"><span data-stu-id="cc79f-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="cc79f-119">Dans ce cas, il est courant de combiner un type de retour [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) avec le type de retour primitif ou complexe.</span><span class="sxs-lookup"><span data-stu-id="cc79f-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="cc79f-120">[IActionResult](#iactionresult-type) ou [ActionResult\<T>](#actionresultt-type) sont nécessaires pour prendre en charge ce type d’action.</span><span class="sxs-lookup"><span data-stu-id="cc79f-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="cc79f-121">Type IActionResult</span><span class="sxs-lookup"><span data-stu-id="cc79f-121">IActionResult type</span></span>

<span data-ttu-id="cc79f-122">Le type de retour [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) est adapté quand plusieurs types de retour [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) sont possibles dans une action.</span><span class="sxs-lookup"><span data-stu-id="cc79f-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="cc79f-123">Les types `ActionResult` représentent différents codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc79f-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="cc79f-124">Certains types de retour courants relevant de cette catégorie sont [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) et [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="cc79f-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="cc79f-125">Comme il existe plusieurs types de retour et chemins dans l’action, une utilisation répandue de l’attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="cc79f-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="cc79f-126">Cet attribut génère des détails plus descriptifs de la réponse pour les pages d’aide de l’API créées par des outils tels que [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="cc79f-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="cc79f-127">`[ProducesResponseType]` indique les types connus et les codes d’état HTTP que l’action doit retourner.</span><span class="sxs-lookup"><span data-stu-id="cc79f-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="cc79f-128">Action synchrone</span><span class="sxs-lookup"><span data-stu-id="cc79f-128">Synchronous action</span></span>

<span data-ttu-id="cc79f-129">Considérez l’action synchrone suivante pour laquelle il existe deux types de retour possibles :</span><span class="sxs-lookup"><span data-stu-id="cc79f-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="cc79f-130">Dans l’action précédente, un code d’état 404 est retourné quand le produit représenté par `id` n’existe pas dans le magasin de données sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="cc79f-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="cc79f-131">La méthode d’assistance [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) est appelée comme raccourci de `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="cc79f-132">Si le produit existe, un objet `Product` représentant la charge utile est retourné avec un code d’état 200.</span><span class="sxs-lookup"><span data-stu-id="cc79f-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="cc79f-133">La méthode d’assistance [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) est appelée comme forme abrégée de `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="cc79f-134">Action asynchrone</span><span class="sxs-lookup"><span data-stu-id="cc79f-134">Asynchronous action</span></span>

<span data-ttu-id="cc79f-135">Considérez l’action asynchrone suivante pour laquelle il existe deux types de retour possibles :</span><span class="sxs-lookup"><span data-stu-id="cc79f-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="cc79f-136">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="cc79f-136">In the preceding code:</span></span>

* <span data-ttu-id="cc79f-137">Un code d’état 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) est retourné par le runtime ASP.NET Core lorsque la description de produit contient « XYZ Widget ».</span><span class="sxs-lookup"><span data-stu-id="cc79f-137">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when the product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="cc79f-138">Un code d’état 201 est généré par la méthode [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) lors de la création d’un produit.</span><span class="sxs-lookup"><span data-stu-id="cc79f-138">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="cc79f-139">Dans ce chemin de code, l’objet `Product` est retourné.</span><span class="sxs-lookup"><span data-stu-id="cc79f-139">In this code path, the `Product` object is returned.</span></span>

<span data-ttu-id="cc79f-140">Par exemple, le modèle suivant indique que les requêtes doivent inclure les propriétés `Name` et `Description`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-140">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="cc79f-141">Par conséquent, si vous n’indiquez pas `Name` et `Description` dans la requête, la validation du modèle échoue.</span><span class="sxs-lookup"><span data-stu-id="cc79f-141">Therefore, failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cc79f-142">Si l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) dans ASP.NET Core 2.1 ou une version ultérieure est appliqué, les erreurs de validation de modèle entraînent un code d’état 400.</span><span class="sxs-lookup"><span data-stu-id="cc79f-142">If the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later  is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="cc79f-143">Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="cc79f-143">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="cc79f-144">Type ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="cc79f-144">ActionResult\<T> type</span></span>

<span data-ttu-id="cc79f-145">ASP.NET Core 2.1 introduit le type de retour [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) pour les actions du contrôleur dans l’API web.</span><span class="sxs-lookup"><span data-stu-id="cc79f-145">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="cc79f-146">Il vous permet de retourner un type dérivant d’[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) ou de retourner un [type spécifique](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="cc79f-146">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="cc79f-147">`ActionResult<T>` offre les avantages suivants par rapport au [type IActionResult](#iactionresult-type) :</span><span class="sxs-lookup"><span data-stu-id="cc79f-147">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="cc79f-148">La propriété `Type` de l’attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) peut être exclue.</span><span class="sxs-lookup"><span data-stu-id="cc79f-148">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="cc79f-149">Par exemple, `[ProducesResponseType(200, Type = typeof(Product))]` est simplifié en `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-149">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="cc79f-150">Le type de retour attendu pour l’action est déduit de `T` dans `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-150">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="cc79f-151">Des [opérateurs de cast implicite](/dotnet/csharp/language-reference/keywords/implicit) prennent en charge la conversion de `T` et `ActionResult` en `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-151">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="cc79f-152">`T` est converti en [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), ce qui signifie que `return new ObjectResult(T);` est simplifié en `return T;`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-152">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="cc79f-153">C# ne prend pas en charge les opérateurs de cast implicite sur les interfaces.</span><span class="sxs-lookup"><span data-stu-id="cc79f-153">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="cc79f-154">Par conséquent, `ActionResult<T>` nécessite une conversion de l’interface en un type concret.</span><span class="sxs-lookup"><span data-stu-id="cc79f-154">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="cc79f-155">Par exemple, l’utilisation de `IEnumerable` dans l’exemple suivant ne fonctionne pas :</span><span class="sxs-lookup"><span data-stu-id="cc79f-155">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="cc79f-156">Pour corriger le code précédent, vous pouvez retourner `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-156">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="cc79f-157">La plupart des actions ont un type de retour spécifique.</span><span class="sxs-lookup"><span data-stu-id="cc79f-157">Most actions have a specific return type.</span></span> <span data-ttu-id="cc79f-158">Des conditions inattendues peuvent se produire pendant l’exécution d’une action, auquel cas le type spécifique n’est pas retourné.</span><span class="sxs-lookup"><span data-stu-id="cc79f-158">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="cc79f-159">Par exemple, le paramètre d’entrée d’une action peut entraîner l’échec de la validation du modèle.</span><span class="sxs-lookup"><span data-stu-id="cc79f-159">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="cc79f-160">Dans ce cas, il est courant de retourner le type `ActionResult` approprié à la place du type spécifique.</span><span class="sxs-lookup"><span data-stu-id="cc79f-160">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="cc79f-161">Action synchrone</span><span class="sxs-lookup"><span data-stu-id="cc79f-161">Synchronous action</span></span>

<span data-ttu-id="cc79f-162">Considérez une action synchrone pour laquelle il existe deux types de retour possibles :</span><span class="sxs-lookup"><span data-stu-id="cc79f-162">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="cc79f-163">Dans le code précédent, un code d’état 404 est retourné quand le produit n’existe pas dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cc79f-163">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="cc79f-164">Si le produit existe, l’objet `Product` correspondant est retourné.</span><span class="sxs-lookup"><span data-stu-id="cc79f-164">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="cc79f-165">Avant ASP.NET Core 2.1, la ligne `return product;` aurait indiqué `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-165">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="cc79f-166">Depuis ASP.NET Core 2.1, l’inférence de la source de liaison de paramètre d’action est activée quand une classe de contrôleur est décorée avec l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-166">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="cc79f-167">Un nom de paramètre correspondant à un nom dans le modèle de routage est automatiquement lié à l’aide des données de l’itinéraire de demande.</span><span class="sxs-lookup"><span data-stu-id="cc79f-167">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="cc79f-168">Par conséquent, le paramètre `id` de l’action précédente n’est pas explicitement annoté avec l’attribut [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).</span><span class="sxs-lookup"><span data-stu-id="cc79f-168">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="cc79f-169">Action asynchrone</span><span class="sxs-lookup"><span data-stu-id="cc79f-169">Asynchronous action</span></span>

<span data-ttu-id="cc79f-170">Considérez une action asynchrone pour laquelle il existe deux types de retour possibles :</span><span class="sxs-lookup"><span data-stu-id="cc79f-170">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="cc79f-171">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="cc79f-171">In the preceding code:</span></span>

* <span data-ttu-id="cc79f-172">Un code d’état 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) est retourné par le runtime ASP.NET Core dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="cc79f-172">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="cc79f-173">L’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) a été appliqué et la validation du modèle échoue.</span><span class="sxs-lookup"><span data-stu-id="cc79f-173">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="cc79f-174">La description de produit contient « XYZ Widget ».</span><span class="sxs-lookup"><span data-stu-id="cc79f-174">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="cc79f-175">Un code d’état 201 est généré par la méthode [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) lors de la création d’un produit.</span><span class="sxs-lookup"><span data-stu-id="cc79f-175">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="cc79f-176">Dans ce chemin de code, l’objet `Product` est retourné.</span><span class="sxs-lookup"><span data-stu-id="cc79f-176">In this code path, the `Product` object is returned.</span></span>

> [!TIP]
> <span data-ttu-id="cc79f-177">Depuis ASP.NET Core 2.1, l’inférence de la source de liaison de paramètre d’action est activée quand une classe de contrôleur est décorée avec l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="cc79f-177">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="cc79f-178">Les paramètres de type complexe sont automatiquement liés à l’aide du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="cc79f-178">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="cc79f-179">Par conséquent, le paramètre `product` de l’action précédente n’est pas explicitement annoté avec l’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).</span><span class="sxs-lookup"><span data-stu-id="cc79f-179">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="cc79f-180">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cc79f-180">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
