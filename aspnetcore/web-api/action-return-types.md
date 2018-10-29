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
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="72a01-103">Types de retour des actions du contrôleur dans l’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72a01-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="72a01-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="72a01-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="72a01-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="72a01-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="72a01-106">ASP.NET Core offre les options suivantes pour les types de retour des actions du contrôleur dans l’API web :</span><span class="sxs-lookup"><span data-stu-id="72a01-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="72a01-107">Type spécifique</span><span class="sxs-lookup"><span data-stu-id="72a01-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="72a01-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="72a01-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="72a01-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="72a01-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="72a01-110">Type spécifique</span><span class="sxs-lookup"><span data-stu-id="72a01-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="72a01-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="72a01-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="72a01-112">Ce document décrit le moment le plus approprié pour utiliser chaque type de retour.</span><span class="sxs-lookup"><span data-stu-id="72a01-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="72a01-113">Type spécifique</span><span class="sxs-lookup"><span data-stu-id="72a01-113">Specific type</span></span>

<span data-ttu-id="72a01-114">L’action la plus simple retourne un type de données primitif ou complexe (par exemple, `string` ou un type d’objet personnalisé).</span><span class="sxs-lookup"><span data-stu-id="72a01-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="72a01-115">Considérez l’action suivante, qui retourne une collection d’objets `Product` personnalisés :</span><span class="sxs-lookup"><span data-stu-id="72a01-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="72a01-116">Sans conditions connues contre lesquelles une protection est nécessaire pendant l’exécution de l’action, le retour d’un type spécifique peut suffire.</span><span class="sxs-lookup"><span data-stu-id="72a01-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="72a01-117">L’action précédente n’acceptant aucun paramètre, la validation des contraintes de paramètre n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="72a01-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="72a01-118">Quand des conditions connues doivent être prises en compte dans une action, plusieurs chemins de retour sont introduits.</span><span class="sxs-lookup"><span data-stu-id="72a01-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="72a01-119">Dans ce cas, il est courant de combiner un type de retour [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) avec le type de retour primitif ou complexe.</span><span class="sxs-lookup"><span data-stu-id="72a01-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="72a01-120">[IActionResult](#iactionresult-type) ou [ActionResult\<T>](#actionresultt-type) sont nécessaires pour prendre en charge ce type d’action.</span><span class="sxs-lookup"><span data-stu-id="72a01-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="72a01-121">Type IActionResult</span><span class="sxs-lookup"><span data-stu-id="72a01-121">IActionResult type</span></span>

<span data-ttu-id="72a01-122">Le type de retour [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) est approprié quand plusieurs types de retour [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) sont possibles dans une action.</span><span class="sxs-lookup"><span data-stu-id="72a01-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="72a01-123">Les types `ActionResult` représentent différents codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="72a01-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="72a01-124">Certains types de retour courants relevant de cette catégorie sont [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) et [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="72a01-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="72a01-125">Comme il existe plusieurs types de retour et chemins dans l’action, une utilisation répandue de l’attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="72a01-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="72a01-126">Cet attribut génère des détails plus descriptifs de la réponse pour les pages d’aide de l’API créées par des outils tels que [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="72a01-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="72a01-127">`[ProducesResponseType]` indique les types connus et les codes d’état HTTP que l’action doit retourner.</span><span class="sxs-lookup"><span data-stu-id="72a01-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="72a01-128">Action synchrone</span><span class="sxs-lookup"><span data-stu-id="72a01-128">Synchronous action</span></span>

<span data-ttu-id="72a01-129">Considérez l’action synchrone suivante pour laquelle il existe deux types de retour possibles :</span><span class="sxs-lookup"><span data-stu-id="72a01-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="72a01-130">Dans l’action précédente, un code d’état 404 est retourné quand le produit représenté par `id` n’existe pas dans le magasin de données sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="72a01-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="72a01-131">La méthode d’assistance [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) est appelée comme raccourci de `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="72a01-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="72a01-132">Si le produit existe, un objet `Product` représentant la charge utile est retourné avec un code d’état 200.</span><span class="sxs-lookup"><span data-stu-id="72a01-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="72a01-133">La méthode d’assistance [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) est appelée comme forme abrégée de `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="72a01-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="72a01-134">Action asynchrone</span><span class="sxs-lookup"><span data-stu-id="72a01-134">Asynchronous action</span></span>

<span data-ttu-id="72a01-135">Considérez l’action asynchrone suivante pour laquelle il existe deux types de retour possibles :</span><span class="sxs-lookup"><span data-stu-id="72a01-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="72a01-136">Dans l’action précédente, un code d’état 400 est retourné en cas d’échec de la validation du modèle et la méthode d’assistance [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) est appelée.</span><span class="sxs-lookup"><span data-stu-id="72a01-136">In the preceding action, a 400 status code is returned when model validation fails and the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) helper method is invoked.</span></span> <span data-ttu-id="72a01-137">Par exemple, le modèle suivant indique que les demandes doivent fournir la propriété `Name` et une valeur.</span><span class="sxs-lookup"><span data-stu-id="72a01-137">For example, the following model indicates that requests must provide the `Name` property and a value.</span></span> <span data-ttu-id="72a01-138">Par conséquent, si vous n’indiquez pas de propriété `Name` appropriée dans la demande, la validation du modèle échoue.</span><span class="sxs-lookup"><span data-stu-id="72a01-138">Therefore, failure to provide a proper `Name` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

<span data-ttu-id="72a01-139">L’autre code de retour connu de l’action précédente est le code 201, qui est généré par la méthode d’assistance [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="72a01-139">The preceding action's other known return code is a 201, which is generated by the [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) helper method.</span></span> <span data-ttu-id="72a01-140">Dans ce chemin, l’objet `Product` est retourné.</span><span class="sxs-lookup"><span data-stu-id="72a01-140">In this path, the `Product` object is returned.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="actionresultt-type"></a><span data-ttu-id="72a01-141">Type ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="72a01-141">ActionResult\<T> type</span></span>

<span data-ttu-id="72a01-142">ASP.NET Core 2.1 introduit le type de retour [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) pour les actions du contrôleur dans l’API web.</span><span class="sxs-lookup"><span data-stu-id="72a01-142">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="72a01-143">Il vous permet de retourner un type dérivant d’[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) ou de retourner un [type spécifique](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="72a01-143">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="72a01-144">`ActionResult<T>` offre les avantages suivants par rapport au [type IActionResult](#iactionresult-type) :</span><span class="sxs-lookup"><span data-stu-id="72a01-144">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="72a01-145">La propriété `Type` de l’attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) peut être exclue.</span><span class="sxs-lookup"><span data-stu-id="72a01-145">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="72a01-146">Par exemple, `[ProducesResponseType(200, Type = typeof(Product))]` est simplifié en `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="72a01-146">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="72a01-147">Le type de retour attendu pour l’action est déduit de `T` dans `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="72a01-147">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="72a01-148">Des [opérateurs de cast implicite](/dotnet/csharp/language-reference/keywords/implicit) prennent en charge la conversion de `T` et `ActionResult` en `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="72a01-148">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="72a01-149">`T` est converti en [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), ce qui signifie que `return new ObjectResult(T);` est simplifié en `return T;`.</span><span class="sxs-lookup"><span data-stu-id="72a01-149">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="72a01-150">C# ne prend pas en charge les opérateurs de cast implicite sur les interfaces.</span><span class="sxs-lookup"><span data-stu-id="72a01-150">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="72a01-151">Par conséquent, `ActionResult<T>` nécessite une conversion de l’interface en un type concret.</span><span class="sxs-lookup"><span data-stu-id="72a01-151">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="72a01-152">Par exemple, l’utilisation de `IEnumerable` dans l’exemple suivant ne fonctionne pas :</span><span class="sxs-lookup"><span data-stu-id="72a01-152">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="72a01-153">Pour réparer le code précédent, vous pouvez retourner `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="72a01-153">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="72a01-154">La plupart des actions ont un type de retour spécifique.</span><span class="sxs-lookup"><span data-stu-id="72a01-154">Most actions have a specific return type.</span></span> <span data-ttu-id="72a01-155">Des conditions inattendues peuvent se produire pendant l’exécution d’une action, auquel cas le type spécifique n’est pas retourné.</span><span class="sxs-lookup"><span data-stu-id="72a01-155">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="72a01-156">Par exemple, le paramètre d’entrée d’une action peut entraîner l’échec de la validation du modèle.</span><span class="sxs-lookup"><span data-stu-id="72a01-156">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="72a01-157">Dans ce cas, il est courant de retourner le type `ActionResult` approprié à la place du type spécifique.</span><span class="sxs-lookup"><span data-stu-id="72a01-157">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="72a01-158">Action synchrone</span><span class="sxs-lookup"><span data-stu-id="72a01-158">Synchronous action</span></span>

<span data-ttu-id="72a01-159">Considérez une action synchrone pour laquelle il existe deux types de retour possibles :</span><span class="sxs-lookup"><span data-stu-id="72a01-159">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="72a01-160">Dans le code précédent, un code d’état 404 est retourné quand le produit n’existe pas dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="72a01-160">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="72a01-161">Si le produit existe, l’objet `Product` correspondant est retourné.</span><span class="sxs-lookup"><span data-stu-id="72a01-161">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="72a01-162">Avant ASP.NET Core 2.1, la ligne `return product;` aurait indiqué `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="72a01-162">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="72a01-163">Depuis ASP.NET Core 2.1, l’inférence de la source de liaison de paramètre d’action est activée quand une classe de contrôleur est décorée avec l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="72a01-163">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="72a01-164">Un nom de paramètre correspondant à un nom dans le modèle de routage est automatiquement lié à l’aide des données de l’itinéraire de demande.</span><span class="sxs-lookup"><span data-stu-id="72a01-164">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="72a01-165">Par conséquent, le paramètre `id` de l’action précédente n’est pas explicitement annoté avec l’attribut [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).</span><span class="sxs-lookup"><span data-stu-id="72a01-165">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="72a01-166">Action asynchrone</span><span class="sxs-lookup"><span data-stu-id="72a01-166">Asynchronous action</span></span>

<span data-ttu-id="72a01-167">Considérez une action asynchrone pour laquelle il existe deux types de retour possibles :</span><span class="sxs-lookup"><span data-stu-id="72a01-167">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="72a01-168">En cas d’échec de la validation du modèle, la méthode [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) est appelée pour retourner un code d’état 400.</span><span class="sxs-lookup"><span data-stu-id="72a01-168">If model validation fails, the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) method is invoked to return a 400 status code.</span></span> <span data-ttu-id="72a01-169">La propriété [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) contenant les erreurs de validation spécifiques lui est transmise.</span><span class="sxs-lookup"><span data-stu-id="72a01-169">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property containing the specific validation errors is passed to it.</span></span> <span data-ttu-id="72a01-170">En cas de réussite de la validation du modèle, le produit est créé dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="72a01-170">If model validation succeeds, the product is created in the database.</span></span> <span data-ttu-id="72a01-171">Un code d’état 201 est retourné.</span><span class="sxs-lookup"><span data-stu-id="72a01-171">A 201 status code is returned.</span></span>

> [!TIP]
> <span data-ttu-id="72a01-172">Depuis ASP.NET Core 2.1, l’inférence de la source de liaison de paramètre d’action est activée quand une classe de contrôleur est décorée avec l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="72a01-172">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="72a01-173">Les paramètres de type complexe sont automatiquement liés à l’aide du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="72a01-173">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="72a01-174">Par conséquent, le paramètre `product` de l’action précédente n’est pas explicitement annoté avec l’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).</span><span class="sxs-lookup"><span data-stu-id="72a01-174">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="72a01-175">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72a01-175">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
