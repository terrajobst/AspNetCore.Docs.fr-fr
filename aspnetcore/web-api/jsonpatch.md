---
title: JsonPatch dans l’API web ASP.NET Core
author: tdykstra
description: Découvrez comment gérer les demandes de correctifs JSON dans une API web ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: 14710e6431a2a7ce60fa7f190bef184da85281a0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64888414"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="e540b-103">JsonPatch dans l’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e540b-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="e540b-104">Par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e540b-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e540b-105">Cet article explique comment gérer les demandes de correctifs JSON dans une API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e540b-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="e540b-106">Méthode de demande HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="e540b-106">PATCH HTTP request method</span></span>

<span data-ttu-id="e540b-107">Les méthodes PUT et [PATCH](https://tools.ietf.org/html/rfc5789) sont utilisées pour mettre à jour une ressource existante.</span><span class="sxs-lookup"><span data-stu-id="e540b-107">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="e540b-108">La différence est que PUT remplace la ressource complète, tandis que PATCH spécifie uniquement les modifications.</span><span class="sxs-lookup"><span data-stu-id="e540b-108">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="e540b-109">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="e540b-109">JSON Patch</span></span>

<span data-ttu-id="e540b-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) est un format qui spécifie des mises à jour à appliquer à une ressource.</span><span class="sxs-lookup"><span data-stu-id="e540b-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="e540b-111">Un document JSON Patch possède un tableau des *opérations*.</span><span class="sxs-lookup"><span data-stu-id="e540b-111">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="e540b-112">Chaque opération identifie un type particulier de modification, tel qu’ajouter un élément de tableau ou remplacer une valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="e540b-112">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="e540b-113">Par exemple, les documents JSON suivants représentent une ressource, un document de correctif JSON pour la ressource et le résultat de l’application de l’application des correctifs.</span><span class="sxs-lookup"><span data-stu-id="e540b-113">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="e540b-114">Exemple de ressource</span><span class="sxs-lookup"><span data-stu-id="e540b-114">Resource example</span></span>

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="e540b-115">Exemple de correctif JSON</span><span class="sxs-lookup"><span data-stu-id="e540b-115">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="e540b-116">Dans le code JSON précédent :</span><span class="sxs-lookup"><span data-stu-id="e540b-116">In the preceding JSON:</span></span>

* <span data-ttu-id="e540b-117">La propriété `op` indique le type d’opération.</span><span class="sxs-lookup"><span data-stu-id="e540b-117">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="e540b-118">La propriété `path` indique l’élément à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="e540b-118">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="e540b-119">La propriété `value` fournit la nouvelle valeur.</span><span class="sxs-lookup"><span data-stu-id="e540b-119">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="e540b-120">Ressources après correction</span><span class="sxs-lookup"><span data-stu-id="e540b-120">Resource after patch</span></span>

<span data-ttu-id="e540b-121">Voici la ressource après l’application du document JSON Patch précédent :</span><span class="sxs-lookup"><span data-stu-id="e540b-121">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="e540b-122">Les modifications apportées en appliquant un document JSON Patch à une ressource sont atomiques : si une opération de la liste échoue, aucune opération de la liste n’est appliquée.</span><span class="sxs-lookup"><span data-stu-id="e540b-122">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="e540b-123">Syntaxe du chemin</span><span class="sxs-lookup"><span data-stu-id="e540b-123">Path syntax</span></span>

<span data-ttu-id="e540b-124">Les différents niveaux de la propriété [path](http://tools.ietf.org/html/rfc6901) d’un objet de l’opération sont séparés par des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="e540b-124">The [path](http://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="e540b-125">Par exemple, `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="e540b-125">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="e540b-126">Les index de base zéro sont utilisés pour spécifier les éléments du tableau.</span><span class="sxs-lookup"><span data-stu-id="e540b-126">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="e540b-127">Le premier élément du tableau `addresses` serait à `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="e540b-127">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="e540b-128">Pour `add` à la fin d’un tableau, utilisez un trait d’union (-) plutôt qu’un numéro d’index : `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="e540b-128">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="e540b-129">Opérations</span><span class="sxs-lookup"><span data-stu-id="e540b-129">Operations</span></span>

<span data-ttu-id="e540b-130">Le tableau suivant mentionne les opérations prises en charge telles qu’elles sont définies dans la [spécification JSON Patch](https://tools.ietf.org/html/rfc6902) :</span><span class="sxs-lookup"><span data-stu-id="e540b-130">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="e540b-131">Opération</span><span class="sxs-lookup"><span data-stu-id="e540b-131">Operation</span></span>  | <span data-ttu-id="e540b-132">Notes</span><span class="sxs-lookup"><span data-stu-id="e540b-132">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="e540b-133">Ajouter une propriété ou élément de tableau.</span><span class="sxs-lookup"><span data-stu-id="e540b-133">Add a property or array element.</span></span> <span data-ttu-id="e540b-134">Pour la propriété existante : définir la valeur.</span><span class="sxs-lookup"><span data-stu-id="e540b-134">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="e540b-135">Supprimer une propriété ou un élément de tableau.</span><span class="sxs-lookup"><span data-stu-id="e540b-135">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="e540b-136">Identique à `remove` suivi de `add` au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="e540b-136">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="e540b-137">Identique à `remove` de la source suivi de `add` à la destination à l’aide de la valeur de la source.</span><span class="sxs-lookup"><span data-stu-id="e540b-137">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="e540b-138">Identique à `add` à la destination à l’aide de la valeur de la source.</span><span class="sxs-lookup"><span data-stu-id="e540b-138">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="e540b-139">Retourne le code d’état de réussite si la valeur à `path` = `value` fournie.</span><span class="sxs-lookup"><span data-stu-id="e540b-139">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="e540b-140">JsonPatch dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e540b-140">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="e540b-141">L’implémentation ASP.NET Core de JSON Patch est fournie dans le package NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="e540b-141">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="e540b-142">Le package est inclus dans le métapaquet [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e540b-142">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="e540b-143">Code de méthode d’action</span><span class="sxs-lookup"><span data-stu-id="e540b-143">Action method code</span></span>

<span data-ttu-id="e540b-144">Dans un contrôleur d’API, une méthode d’action pour JSON Patch :</span><span class="sxs-lookup"><span data-stu-id="e540b-144">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="e540b-145">est annotée avec l’attribut `HttpPatch` ;</span><span class="sxs-lookup"><span data-stu-id="e540b-145">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="e540b-146">accepte un `JsonPatchDocument<T>`, généralement avec [FromBody] ;</span><span class="sxs-lookup"><span data-stu-id="e540b-146">Accepts a `JsonPatchDocument<T>`, typically with [FromBody].</span></span>
* <span data-ttu-id="e540b-147">appelle `ApplyTo` sur le document de correctif pour appliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="e540b-147">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="e540b-148">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="e540b-148">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="e540b-149">Ce code de l’exemple d’application fonctionne avec le modèle `Customer` suivant.</span><span class="sxs-lookup"><span data-stu-id="e540b-149">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="e540b-150">L’exemple de méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="e540b-150">The sample action method:</span></span>

* <span data-ttu-id="e540b-151">Construit un objet `Customer`.</span><span class="sxs-lookup"><span data-stu-id="e540b-151">Constructs a `Customer`.</span></span>
* <span data-ttu-id="e540b-152">Applique le correctif.</span><span class="sxs-lookup"><span data-stu-id="e540b-152">Applies the patch.</span></span>
* <span data-ttu-id="e540b-153">Retourne le résultat dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="e540b-153">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="e540b-154">Dans une application réelle, le code récupérerait les données dans un magasin tel qu’une base de données et mettrait à jour la base de données après avoir appliqué le correctif.</span><span class="sxs-lookup"><span data-stu-id="e540b-154">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="e540b-155">État du modèle</span><span class="sxs-lookup"><span data-stu-id="e540b-155">Model state</span></span>

<span data-ttu-id="e540b-156">L’exemple de méthode d’action précédent appelle une surcharge de `ApplyTo` qui prend l’état du modèle comme un de ses paramètres.</span><span class="sxs-lookup"><span data-stu-id="e540b-156">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="e540b-157">Avec cette option, vous pouvez obtenir des messages d’erreur dans les réponses.</span><span class="sxs-lookup"><span data-stu-id="e540b-157">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="e540b-158">L’exemple suivant montre le corps d’une demande-réponse incorrecte 400 pour une opération `test` :</span><span class="sxs-lookup"><span data-stu-id="e540b-158">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="e540b-159">Objets dynamiques</span><span class="sxs-lookup"><span data-stu-id="e540b-159">Dynamic objects</span></span>

<span data-ttu-id="e540b-160">L’exemple de méthode d’action suivant montre comment appliquer un correctif à un objet dynamique.</span><span class="sxs-lookup"><span data-stu-id="e540b-160">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="e540b-161">L’opération d’ajout</span><span class="sxs-lookup"><span data-stu-id="e540b-161">The add operation</span></span>

* <span data-ttu-id="e540b-162">Si `path` pointe vers un élément du tableau : insère un nouvel élément avant celui spécifié par `path`.</span><span class="sxs-lookup"><span data-stu-id="e540b-162">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="e540b-163">Si `path` pointe vers une propriété : définit la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="e540b-163">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="e540b-164">Si `path` pointe vers un emplacement qui n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="e540b-164">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="e540b-165">Si la ressource à corriger est un objet dynamique : ajoute une propriété.</span><span class="sxs-lookup"><span data-stu-id="e540b-165">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="e540b-166">Si la ressource à corriger est un objet statique : la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="e540b-166">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="e540b-167">L’exemple de document de correctif suivant définit la valeur de `CustomerName` et ajoute un objet `Order` à la fin du tableau `Orders`.</span><span class="sxs-lookup"><span data-stu-id="e540b-167">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="e540b-168">L’opération de suppression</span><span class="sxs-lookup"><span data-stu-id="e540b-168">The remove operation</span></span>

* <span data-ttu-id="e540b-169">Si `path` pointe vers un élément du tableau : supprime l’élément.</span><span class="sxs-lookup"><span data-stu-id="e540b-169">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="e540b-170">Si `path` pointe vers une propriété :</span><span class="sxs-lookup"><span data-stu-id="e540b-170">If `path` points to a property:</span></span>
  * <span data-ttu-id="e540b-171">Si la ressource à corriger est un objet dynamique : supprime la propriété.</span><span class="sxs-lookup"><span data-stu-id="e540b-171">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="e540b-172">Si la ressource à corriger est un objet statique :</span><span class="sxs-lookup"><span data-stu-id="e540b-172">If resource to patch is a static object:</span></span> 
    * <span data-ttu-id="e540b-173">Si la propriété est Nullable : met la valeur sur Null.</span><span class="sxs-lookup"><span data-stu-id="e540b-173">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="e540b-174">Si la propriété n’est pas Nullable : met la valeur sur `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="e540b-174">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="e540b-175">L’exemple suivant de document de correctif définit `CustomerName` sur Null et supprime `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e540b-175">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="e540b-176">L’opération de remplacement</span><span class="sxs-lookup"><span data-stu-id="e540b-176">The replace operation</span></span>

<span data-ttu-id="e540b-177">Cette opération est fonctionnellement identique à `remove` suivi de `add`.</span><span class="sxs-lookup"><span data-stu-id="e540b-177">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="e540b-178">L’exemple suivant de document de correctif définit la valeur de `CustomerName` et remplace `Orders[0]` avec un nouvel objet `Order`.</span><span class="sxs-lookup"><span data-stu-id="e540b-178">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="e540b-179">L’opération de déplacement</span><span class="sxs-lookup"><span data-stu-id="e540b-179">The move operation</span></span>

* <span data-ttu-id="e540b-180">Si `path` pointe vers un élément du tableau : copie l’élément `from` à l’emplacement de l’élément `path`, puis exécute une opération `remove` sur l’élément `from`.</span><span class="sxs-lookup"><span data-stu-id="e540b-180">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="e540b-181">Si `path` pointe vers une propriété : copie la valeur de la propriété `from` vers la propriété `path`, puis exécute une opération `remove` sur la propriété `from`.</span><span class="sxs-lookup"><span data-stu-id="e540b-181">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="e540b-182">Si `path` pointe vers une propriété qui n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="e540b-182">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="e540b-183">Si la ressource à corriger est un objet statique : la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="e540b-183">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="e540b-184">Si la ressource à corriger est un objet dynamique : copie la propriété `from` à un emplacement indiqué par `path`, puis exécute une opération `remove` sur la propriété `from`.</span><span class="sxs-lookup"><span data-stu-id="e540b-184">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="e540b-185">L’exemple de document de correctif suivant :</span><span class="sxs-lookup"><span data-stu-id="e540b-185">The following sample patch document:</span></span>

* <span data-ttu-id="e540b-186">Copie la valeur de `Orders[0].OrderName` vers `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="e540b-186">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="e540b-187">Met `Orders[0].OrderName` sur la valeur Null.</span><span class="sxs-lookup"><span data-stu-id="e540b-187">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="e540b-188">Déplace `Orders[1]` avant `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e540b-188">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="e540b-189">L’opération de copie</span><span class="sxs-lookup"><span data-stu-id="e540b-189">The copy operation</span></span>

<span data-ttu-id="e540b-190">Cette opération est fonctionnellement identique à une opération `move` sans l’étape `remove` finale.</span><span class="sxs-lookup"><span data-stu-id="e540b-190">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="e540b-191">L’exemple de document de correctif suivant :</span><span class="sxs-lookup"><span data-stu-id="e540b-191">The following sample patch document:</span></span>

* <span data-ttu-id="e540b-192">Copie la valeur de `Orders[0].OrderName` vers `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="e540b-192">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="e540b-193">Insère une copie de `Orders[1]` avant `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e540b-193">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="e540b-194">L’opération de test</span><span class="sxs-lookup"><span data-stu-id="e540b-194">The test operation</span></span>

<span data-ttu-id="e540b-195">Si la valeur à l’emplacement indiqué par `path` est différente de la valeur fournie dans `value`, la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="e540b-195">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="e540b-196">Dans ce cas, toute la demande PATCH échoue, même si toutes les autres opérations dans le document de correctif réussiraient autrement.</span><span class="sxs-lookup"><span data-stu-id="e540b-196">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="e540b-197">L’opération `test` est généralement utilisée pour empêcher une mise à jour lorsqu’il y a un conflit d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="e540b-197">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="e540b-198">L’exemple de document de correctif suivant n’a aucun effet si la valeur initiale de `CustomerName` est « John », car le test échoue :</span><span class="sxs-lookup"><span data-stu-id="e540b-198">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="e540b-199">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="e540b-199">Get the code</span></span>

<span data-ttu-id="e540b-200">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="e540b-200">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="e540b-201">([Guide pratique de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e540b-201">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="e540b-202">Pour tester l’exemple, exécutez l’application et envoyez des demandes HTTP avec les paramètres suivants :</span><span class="sxs-lookup"><span data-stu-id="e540b-202">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="e540b-203">URL : `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="e540b-203">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="e540b-204">Méthode HTTP : `PATCH`</span><span class="sxs-lookup"><span data-stu-id="e540b-204">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="e540b-205">En-tête : `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="e540b-205">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="e540b-206">Corps : Copiez et collez l’un des exemples de document de correctif JSON à partir du dossier de projet *JSON*.</span><span class="sxs-lookup"><span data-stu-id="e540b-206">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e540b-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e540b-207">Additional resources</span></span>

* [<span data-ttu-id="e540b-208">Spécification de méthode PATCH IETF RFC 5789 PATCH</span><span class="sxs-lookup"><span data-stu-id="e540b-208">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="e540b-209">Spécification JSON Patch IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="e540b-209">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="e540b-210">Spécification de format de chemin JSON Patch IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="e540b-210">IETF RFC 6901 JSON Patch path format spec</span></span>](http://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="e540b-211">[Documentation JSON Patch](http://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="e540b-211">[JSON Patch documentation](http://jsonpatch.com/).</span></span> <span data-ttu-id="e540b-212">Inclut des liens vers des ressources pour la création de documents JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="e540b-212">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="e540b-213">Code source ASP.NET Core JSON Patch</span><span class="sxs-lookup"><span data-stu-id="e540b-213">ASP.NET Core JSON Patch source code</span></span>](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
