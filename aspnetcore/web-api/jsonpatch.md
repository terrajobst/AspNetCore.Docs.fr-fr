---
title: JsonPatch dans l’API web ASP.NET Core
author: rick-anderson
description: Découvrez comment gérer les demandes de correctifs JSON dans une API web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: cf1a00c1928652bf5210b2442087209e23b8868e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661783"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="93456-103">JsonPatch dans l’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93456-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="93456-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="93456-104">By [Tom Dykstra](https://github.com/tdykstra) and [Kirk Larkin](https://github.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="93456-105">Cet article explique comment gérer les demandes de correctifs JSON dans une API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93456-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="package-installation"></a><span data-ttu-id="93456-106">Installation de package</span><span class="sxs-lookup"><span data-stu-id="93456-106">Package installation</span></span>

<span data-ttu-id="93456-107">La prise en charge de JsonPatch est activée à l’aide du package `Microsoft.AspNetCore.Mvc.NewtonsoftJson`.</span><span class="sxs-lookup"><span data-stu-id="93456-107">Support for JsonPatch is enabled using the `Microsoft.AspNetCore.Mvc.NewtonsoftJson` package.</span></span> <span data-ttu-id="93456-108">Pour activer cette fonctionnalité, les applications doivent :</span><span class="sxs-lookup"><span data-stu-id="93456-108">To enable this feature, apps must:</span></span>

* <span data-ttu-id="93456-109">Installez le package NuGet [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) .</span><span class="sxs-lookup"><span data-stu-id="93456-109">Install the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package.</span></span>
* <span data-ttu-id="93456-110">Mettre à jour la méthode `Startup.ConfigureServices` du projet pour inclure un appel à `AddNewtonsoftJson` :</span><span class="sxs-lookup"><span data-stu-id="93456-110">Update the project's `Startup.ConfigureServices` method to include a call to `AddNewtonsoftJson`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

<span data-ttu-id="93456-111">`AddNewtonsoftJson` est compatible avec les méthodes d’inscription du service MVC :</span><span class="sxs-lookup"><span data-stu-id="93456-111">`AddNewtonsoftJson` is compatible with the MVC service registration methods:</span></span>

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a><span data-ttu-id="93456-112">JsonPatch, AddNewtonsoftJson et System. Text. JSON</span><span class="sxs-lookup"><span data-stu-id="93456-112">JsonPatch, AddNewtonsoftJson, and System.Text.Json</span></span>
  
<span data-ttu-id="93456-113">`AddNewtonsoftJson` remplace les formateurs d’entrée et de sortie basés sur `System.Text.Json` utilisés pour mettre en forme **tout** le contenu JSON.</span><span class="sxs-lookup"><span data-stu-id="93456-113">`AddNewtonsoftJson` replaces the `System.Text.Json` based input and output formatters used for formatting **all** JSON content.</span></span> <span data-ttu-id="93456-114">Pour ajouter la prise en charge de `JsonPatch` à l’aide de `Newtonsoft.Json`, tout en laissant les autres formateurs inchangés, mettez à jour le `Startup.ConfigureServices` du projet comme suit :</span><span class="sxs-lookup"><span data-stu-id="93456-114">To add support for `JsonPatch` using `Newtonsoft.Json`, while leaving the other formatters unchanged, update the project's `Startup.ConfigureServices` as follows:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="93456-115">Le code précédent requiert une référence à [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) et les instructions using suivantes :</span><span class="sxs-lookup"><span data-stu-id="93456-115">The preceding code requires a reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) and the following using statements:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a><span data-ttu-id="93456-116">Méthode de demande HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="93456-116">PATCH HTTP request method</span></span>

<span data-ttu-id="93456-117">Les méthodes PUT et [PATCH](https://tools.ietf.org/html/rfc5789) sont utilisées pour mettre à jour une ressource existante.</span><span class="sxs-lookup"><span data-stu-id="93456-117">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="93456-118">La différence est que PUT remplace la ressource complète, tandis que PATCH spécifie uniquement les modifications.</span><span class="sxs-lookup"><span data-stu-id="93456-118">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="93456-119">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="93456-119">JSON Patch</span></span>

<span data-ttu-id="93456-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) est un format qui spécifie des mises à jour à appliquer à une ressource.</span><span class="sxs-lookup"><span data-stu-id="93456-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="93456-121">Un document JSON Patch possède un tableau des *opérations*.</span><span class="sxs-lookup"><span data-stu-id="93456-121">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="93456-122">Chaque opération identifie un type particulier de modification, tel qu’ajouter un élément de tableau ou remplacer une valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="93456-122">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="93456-123">Par exemple, les documents JSON suivants représentent une ressource, un document de correctif JSON pour la ressource et le résultat de l’application de l’application des correctifs.</span><span class="sxs-lookup"><span data-stu-id="93456-123">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="93456-124">Exemple de ressource</span><span class="sxs-lookup"><span data-stu-id="93456-124">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="93456-125">Exemple de correctif JSON</span><span class="sxs-lookup"><span data-stu-id="93456-125">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="93456-126">Dans le code JSON précédent :</span><span class="sxs-lookup"><span data-stu-id="93456-126">In the preceding JSON:</span></span>

* <span data-ttu-id="93456-127">La propriété `op` indique le type d’opération.</span><span class="sxs-lookup"><span data-stu-id="93456-127">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="93456-128">La propriété `path` indique l’élément à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="93456-128">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="93456-129">La propriété `value` fournit la nouvelle valeur.</span><span class="sxs-lookup"><span data-stu-id="93456-129">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="93456-130">Ressources après correction</span><span class="sxs-lookup"><span data-stu-id="93456-130">Resource after patch</span></span>

<span data-ttu-id="93456-131">Voici la ressource après l’application du document JSON Patch précédent :</span><span class="sxs-lookup"><span data-stu-id="93456-131">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="93456-132">Les modifications apportées en appliquant un document JSON Patch à une ressource sont atomiques : si une opération de la liste échoue, aucune opération de la liste n’est appliquée.</span><span class="sxs-lookup"><span data-stu-id="93456-132">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="93456-133">Syntaxe du chemin</span><span class="sxs-lookup"><span data-stu-id="93456-133">Path syntax</span></span>

<span data-ttu-id="93456-134">Les différents niveaux de la propriété [path](https://tools.ietf.org/html/rfc6901) d’un objet de l’opération sont séparés par des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="93456-134">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="93456-135">Par exemple : `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="93456-135">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="93456-136">Les index de base zéro sont utilisés pour spécifier les éléments du tableau.</span><span class="sxs-lookup"><span data-stu-id="93456-136">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="93456-137">Le premier élément du tableau `addresses` serait à `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="93456-137">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="93456-138">Pour `add` à la fin d’un tableau, utilisez un trait d’union (-) plutôt qu’un numéro d’index : `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="93456-138">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="93456-139">Opérations</span><span class="sxs-lookup"><span data-stu-id="93456-139">Operations</span></span>

<span data-ttu-id="93456-140">Le tableau suivant mentionne les opérations prises en charge telles qu’elles sont définies dans la [spécification JSON Patch](https://tools.ietf.org/html/rfc6902) :</span><span class="sxs-lookup"><span data-stu-id="93456-140">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="93456-141">Opération</span><span class="sxs-lookup"><span data-stu-id="93456-141">Operation</span></span>  | <span data-ttu-id="93456-142">Notes</span><span class="sxs-lookup"><span data-stu-id="93456-142">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="93456-143">Ajouter une propriété ou élément de tableau.</span><span class="sxs-lookup"><span data-stu-id="93456-143">Add a property or array element.</span></span> <span data-ttu-id="93456-144">Pour la propriété existante : définir la valeur.</span><span class="sxs-lookup"><span data-stu-id="93456-144">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="93456-145">Supprimer une propriété ou un élément de tableau.</span><span class="sxs-lookup"><span data-stu-id="93456-145">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="93456-146">Identique à `remove` suivi de `add` au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="93456-146">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="93456-147">Identique à `remove` de la source suivi de `add` à la destination à l’aide de la valeur de la source.</span><span class="sxs-lookup"><span data-stu-id="93456-147">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="93456-148">Identique à `add` à la destination à l’aide de la valeur de la source.</span><span class="sxs-lookup"><span data-stu-id="93456-148">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="93456-149">Retourne le code d’état de réussite si la valeur à `path` = `value` fournie.</span><span class="sxs-lookup"><span data-stu-id="93456-149">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="93456-150">JsonPatch dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93456-150">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="93456-151">L’implémentation ASP.NET Core de JSON Patch est fournie dans le package NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="93456-151">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="93456-152">Code de méthode d’action</span><span class="sxs-lookup"><span data-stu-id="93456-152">Action method code</span></span>

<span data-ttu-id="93456-153">Dans un contrôleur d’API, une méthode d’action pour JSON Patch :</span><span class="sxs-lookup"><span data-stu-id="93456-153">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="93456-154">est annotée avec l’attribut `HttpPatch` ;</span><span class="sxs-lookup"><span data-stu-id="93456-154">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="93456-155">Accepte un `JsonPatchDocument<T>`, en général avec `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="93456-155">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="93456-156">appelle `ApplyTo` sur le document de correctif pour appliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="93456-156">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="93456-157">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="93456-157">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="93456-158">Ce code de l’exemple d’application fonctionne avec le modèle `Customer` suivant.</span><span class="sxs-lookup"><span data-stu-id="93456-158">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="93456-159">L’exemple de méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="93456-159">The sample action method:</span></span>

* <span data-ttu-id="93456-160">Construit un objet `Customer`.</span><span class="sxs-lookup"><span data-stu-id="93456-160">Constructs a `Customer`.</span></span>
* <span data-ttu-id="93456-161">Applique le correctif.</span><span class="sxs-lookup"><span data-stu-id="93456-161">Applies the patch.</span></span>
* <span data-ttu-id="93456-162">Retourne le résultat dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="93456-162">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="93456-163">Dans une application réelle, le code récupérerait les données dans un magasin tel qu’une base de données et mettrait à jour la base de données après avoir appliqué le correctif.</span><span class="sxs-lookup"><span data-stu-id="93456-163">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="93456-164">État du modèle</span><span class="sxs-lookup"><span data-stu-id="93456-164">Model state</span></span>

<span data-ttu-id="93456-165">L’exemple de méthode d’action précédent appelle une surcharge de `ApplyTo` qui prend l’état du modèle comme un de ses paramètres.</span><span class="sxs-lookup"><span data-stu-id="93456-165">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="93456-166">Avec cette option, vous pouvez obtenir des messages d’erreur dans les réponses.</span><span class="sxs-lookup"><span data-stu-id="93456-166">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="93456-167">L’exemple suivant montre le corps d’une demande-réponse incorrecte 400 pour une opération `test` :</span><span class="sxs-lookup"><span data-stu-id="93456-167">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="93456-168">Objets dynamiques</span><span class="sxs-lookup"><span data-stu-id="93456-168">Dynamic objects</span></span>

<span data-ttu-id="93456-169">L’exemple de méthode d’action suivant montre comment appliquer un correctif à un objet dynamique.</span><span class="sxs-lookup"><span data-stu-id="93456-169">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="93456-170">L’opération d’ajout</span><span class="sxs-lookup"><span data-stu-id="93456-170">The add operation</span></span>

* <span data-ttu-id="93456-171">Si `path` pointe vers un élément du tableau : insère un nouvel élément avant celui spécifié par `path`.</span><span class="sxs-lookup"><span data-stu-id="93456-171">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="93456-172">Si `path` pointe vers une propriété : définit la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="93456-172">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="93456-173">Si `path` pointe vers un emplacement qui n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="93456-173">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="93456-174">Si la ressource à corriger est un objet dynamique : ajoute une propriété.</span><span class="sxs-lookup"><span data-stu-id="93456-174">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="93456-175">Si la ressource à corriger est un objet statique : la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="93456-175">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="93456-176">L’exemple de document de correctif suivant définit la valeur de `CustomerName` et ajoute un objet `Order` à la fin du tableau `Orders`.</span><span class="sxs-lookup"><span data-stu-id="93456-176">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="93456-177">L’opération de suppression</span><span class="sxs-lookup"><span data-stu-id="93456-177">The remove operation</span></span>

* <span data-ttu-id="93456-178">Si `path` pointe vers un élément du tableau : supprime l’élément.</span><span class="sxs-lookup"><span data-stu-id="93456-178">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="93456-179">Si `path` pointe vers une propriété :</span><span class="sxs-lookup"><span data-stu-id="93456-179">If `path` points to a property:</span></span>
  * <span data-ttu-id="93456-180">Si la ressource à corriger est un objet dynamique : supprime la propriété.</span><span class="sxs-lookup"><span data-stu-id="93456-180">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="93456-181">Si la ressource à corriger est un objet statique :</span><span class="sxs-lookup"><span data-stu-id="93456-181">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="93456-182">Si la propriété est Nullable : met la valeur sur Null.</span><span class="sxs-lookup"><span data-stu-id="93456-182">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="93456-183">Si la propriété n’est pas Nullable : met la valeur sur `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="93456-183">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="93456-184">L’exemple suivant de document de correctif définit `CustomerName` sur Null et supprime `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="93456-184">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="93456-185">L’opération de remplacement</span><span class="sxs-lookup"><span data-stu-id="93456-185">The replace operation</span></span>

<span data-ttu-id="93456-186">Cette opération est fonctionnellement identique à `remove` suivi de `add`.</span><span class="sxs-lookup"><span data-stu-id="93456-186">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="93456-187">L’exemple suivant de document de correctif définit la valeur de `CustomerName` et remplace `Orders[0]` avec un nouvel objet `Order`.</span><span class="sxs-lookup"><span data-stu-id="93456-187">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="93456-188">L’opération de déplacement</span><span class="sxs-lookup"><span data-stu-id="93456-188">The move operation</span></span>

* <span data-ttu-id="93456-189">Si `path` pointe vers un élément du tableau : copie l’élément `from` à l’emplacement de l’élément `path`, puis exécute une opération `remove` sur l’élément `from`.</span><span class="sxs-lookup"><span data-stu-id="93456-189">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="93456-190">Si `path` pointe vers une propriété : copie la valeur de la propriété `from` vers la propriété `path`, puis exécute une opération `remove` sur la propriété `from`.</span><span class="sxs-lookup"><span data-stu-id="93456-190">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="93456-191">Si `path` pointe vers une propriété qui n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="93456-191">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="93456-192">Si la ressource à corriger est un objet statique : la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="93456-192">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="93456-193">Si la ressource à corriger est un objet dynamique : copie la propriété `from` à un emplacement indiqué par `path`, puis exécute une opération `remove` sur la propriété `from`.</span><span class="sxs-lookup"><span data-stu-id="93456-193">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="93456-194">L’exemple de document de correctif suivant :</span><span class="sxs-lookup"><span data-stu-id="93456-194">The following sample patch document:</span></span>

* <span data-ttu-id="93456-195">Copie la valeur de `Orders[0].OrderName` vers `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="93456-195">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="93456-196">Met `Orders[0].OrderName` sur la valeur Null.</span><span class="sxs-lookup"><span data-stu-id="93456-196">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="93456-197">Déplace `Orders[1]` avant `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="93456-197">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="93456-198">L’opération de copie</span><span class="sxs-lookup"><span data-stu-id="93456-198">The copy operation</span></span>

<span data-ttu-id="93456-199">Cette opération est fonctionnellement identique à une opération `move` sans l’étape `remove` finale.</span><span class="sxs-lookup"><span data-stu-id="93456-199">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="93456-200">L’exemple de document de correctif suivant :</span><span class="sxs-lookup"><span data-stu-id="93456-200">The following sample patch document:</span></span>

* <span data-ttu-id="93456-201">Copie la valeur de `Orders[0].OrderName` vers `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="93456-201">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="93456-202">Insère une copie de `Orders[1]` avant `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="93456-202">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="93456-203">L’opération de test</span><span class="sxs-lookup"><span data-stu-id="93456-203">The test operation</span></span>

<span data-ttu-id="93456-204">Si la valeur à l’emplacement indiqué par `path` est différente de la valeur fournie dans `value`, la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="93456-204">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="93456-205">Dans ce cas, toute la demande PATCH échoue, même si toutes les autres opérations dans le document de correctif réussiraient autrement.</span><span class="sxs-lookup"><span data-stu-id="93456-205">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="93456-206">L’opération `test` est généralement utilisée pour empêcher une mise à jour lorsqu’il y a un conflit d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="93456-206">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="93456-207">L’exemple de document de correctif suivant n’a aucun effet si la valeur initiale de `CustomerName` est « John », car le test échoue :</span><span class="sxs-lookup"><span data-stu-id="93456-207">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="93456-208">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="93456-208">Get the code</span></span>

<span data-ttu-id="93456-209">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="93456-209">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="93456-210">([Guide pratique de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="93456-210">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="93456-211">Pour tester l’exemple, exécutez l’application et envoyez des demandes HTTP avec les paramètres suivants :</span><span class="sxs-lookup"><span data-stu-id="93456-211">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="93456-212">URL : `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="93456-212">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="93456-213">Méthode HTTP : `PATCH`</span><span class="sxs-lookup"><span data-stu-id="93456-213">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="93456-214">En-tête : `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="93456-214">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="93456-215">Corps : copiez et collez l’un des exemples de document de correctif JSON à partir du dossier de projet *JSON* .</span><span class="sxs-lookup"><span data-stu-id="93456-215">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93456-216">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="93456-216">Additional resources</span></span>

* [<span data-ttu-id="93456-217">Spécification de méthode PATCH IETF RFC 5789 PATCH</span><span class="sxs-lookup"><span data-stu-id="93456-217">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="93456-218">Spécification JSON Patch IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="93456-218">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="93456-219">Spécification de format de chemin JSON Patch IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="93456-219">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="93456-220">[Documentation JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="93456-220">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="93456-221">Inclut des liens vers des ressources pour la création de documents JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="93456-221">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="93456-222">Code source ASP.NET Core JSON Patch</span><span class="sxs-lookup"><span data-stu-id="93456-222">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="93456-223">Cet article explique comment gérer les demandes de correctifs JSON dans une API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93456-223">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="93456-224">Méthode de demande HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="93456-224">PATCH HTTP request method</span></span>

<span data-ttu-id="93456-225">Les méthodes PUT et [PATCH](https://tools.ietf.org/html/rfc5789) sont utilisées pour mettre à jour une ressource existante.</span><span class="sxs-lookup"><span data-stu-id="93456-225">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="93456-226">La différence est que PUT remplace la ressource complète, tandis que PATCH spécifie uniquement les modifications.</span><span class="sxs-lookup"><span data-stu-id="93456-226">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="93456-227">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="93456-227">JSON Patch</span></span>

<span data-ttu-id="93456-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) est un format qui spécifie des mises à jour à appliquer à une ressource.</span><span class="sxs-lookup"><span data-stu-id="93456-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="93456-229">Un document JSON Patch possède un tableau des *opérations*.</span><span class="sxs-lookup"><span data-stu-id="93456-229">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="93456-230">Chaque opération identifie un type particulier de modification, tel qu’ajouter un élément de tableau ou remplacer une valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="93456-230">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="93456-231">Par exemple, les documents JSON suivants représentent une ressource, un document de correctif JSON pour la ressource et le résultat de l’application de l’application des correctifs.</span><span class="sxs-lookup"><span data-stu-id="93456-231">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="93456-232">Exemple de ressource</span><span class="sxs-lookup"><span data-stu-id="93456-232">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="93456-233">Exemple de correctif JSON</span><span class="sxs-lookup"><span data-stu-id="93456-233">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="93456-234">Dans le code JSON précédent :</span><span class="sxs-lookup"><span data-stu-id="93456-234">In the preceding JSON:</span></span>

* <span data-ttu-id="93456-235">La propriété `op` indique le type d’opération.</span><span class="sxs-lookup"><span data-stu-id="93456-235">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="93456-236">La propriété `path` indique l’élément à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="93456-236">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="93456-237">La propriété `value` fournit la nouvelle valeur.</span><span class="sxs-lookup"><span data-stu-id="93456-237">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="93456-238">Ressources après correction</span><span class="sxs-lookup"><span data-stu-id="93456-238">Resource after patch</span></span>

<span data-ttu-id="93456-239">Voici la ressource après l’application du document JSON Patch précédent :</span><span class="sxs-lookup"><span data-stu-id="93456-239">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="93456-240">Les modifications apportées en appliquant un document JSON Patch à une ressource sont atomiques : si une opération de la liste échoue, aucune opération de la liste n’est appliquée.</span><span class="sxs-lookup"><span data-stu-id="93456-240">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="93456-241">Syntaxe du chemin</span><span class="sxs-lookup"><span data-stu-id="93456-241">Path syntax</span></span>

<span data-ttu-id="93456-242">Les différents niveaux de la propriété [path](https://tools.ietf.org/html/rfc6901) d’un objet de l’opération sont séparés par des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="93456-242">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="93456-243">Par exemple : `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="93456-243">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="93456-244">Les index de base zéro sont utilisés pour spécifier les éléments du tableau.</span><span class="sxs-lookup"><span data-stu-id="93456-244">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="93456-245">Le premier élément du tableau `addresses` serait à `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="93456-245">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="93456-246">Pour `add` à la fin d’un tableau, utilisez un trait d’union (-) plutôt qu’un numéro d’index : `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="93456-246">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="93456-247">Opérations</span><span class="sxs-lookup"><span data-stu-id="93456-247">Operations</span></span>

<span data-ttu-id="93456-248">Le tableau suivant mentionne les opérations prises en charge telles qu’elles sont définies dans la [spécification JSON Patch](https://tools.ietf.org/html/rfc6902) :</span><span class="sxs-lookup"><span data-stu-id="93456-248">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="93456-249">Opération</span><span class="sxs-lookup"><span data-stu-id="93456-249">Operation</span></span>  | <span data-ttu-id="93456-250">Notes</span><span class="sxs-lookup"><span data-stu-id="93456-250">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="93456-251">Ajouter une propriété ou élément de tableau.</span><span class="sxs-lookup"><span data-stu-id="93456-251">Add a property or array element.</span></span> <span data-ttu-id="93456-252">Pour la propriété existante : définir la valeur.</span><span class="sxs-lookup"><span data-stu-id="93456-252">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="93456-253">Supprimer une propriété ou un élément de tableau.</span><span class="sxs-lookup"><span data-stu-id="93456-253">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="93456-254">Identique à `remove` suivi de `add` au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="93456-254">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="93456-255">Identique à `remove` de la source suivi de `add` à la destination à l’aide de la valeur de la source.</span><span class="sxs-lookup"><span data-stu-id="93456-255">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="93456-256">Identique à `add` à la destination à l’aide de la valeur de la source.</span><span class="sxs-lookup"><span data-stu-id="93456-256">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="93456-257">Retourne le code d’état de réussite si la valeur à `path` = `value` fournie.</span><span class="sxs-lookup"><span data-stu-id="93456-257">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="93456-258">JsonPatch dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93456-258">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="93456-259">L’implémentation ASP.NET Core de JSON Patch est fournie dans le package NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="93456-259">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="93456-260">Le package est inclus dans le métapaquet [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="93456-260">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="93456-261">Code de méthode d’action</span><span class="sxs-lookup"><span data-stu-id="93456-261">Action method code</span></span>

<span data-ttu-id="93456-262">Dans un contrôleur d’API, une méthode d’action pour JSON Patch :</span><span class="sxs-lookup"><span data-stu-id="93456-262">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="93456-263">est annotée avec l’attribut `HttpPatch` ;</span><span class="sxs-lookup"><span data-stu-id="93456-263">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="93456-264">Accepte un `JsonPatchDocument<T>`, en général avec `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="93456-264">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="93456-265">appelle `ApplyTo` sur le document de correctif pour appliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="93456-265">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="93456-266">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="93456-266">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="93456-267">Ce code de l’exemple d’application fonctionne avec le modèle `Customer` suivant.</span><span class="sxs-lookup"><span data-stu-id="93456-267">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="93456-268">L’exemple de méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="93456-268">The sample action method:</span></span>

* <span data-ttu-id="93456-269">Construit un objet `Customer`.</span><span class="sxs-lookup"><span data-stu-id="93456-269">Constructs a `Customer`.</span></span>
* <span data-ttu-id="93456-270">Applique le correctif.</span><span class="sxs-lookup"><span data-stu-id="93456-270">Applies the patch.</span></span>
* <span data-ttu-id="93456-271">Retourne le résultat dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="93456-271">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="93456-272">Dans une application réelle, le code récupérerait les données dans un magasin tel qu’une base de données et mettrait à jour la base de données après avoir appliqué le correctif.</span><span class="sxs-lookup"><span data-stu-id="93456-272">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="93456-273">État du modèle</span><span class="sxs-lookup"><span data-stu-id="93456-273">Model state</span></span>

<span data-ttu-id="93456-274">L’exemple de méthode d’action précédent appelle une surcharge de `ApplyTo` qui prend l’état du modèle comme un de ses paramètres.</span><span class="sxs-lookup"><span data-stu-id="93456-274">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="93456-275">Avec cette option, vous pouvez obtenir des messages d’erreur dans les réponses.</span><span class="sxs-lookup"><span data-stu-id="93456-275">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="93456-276">L’exemple suivant montre le corps d’une demande-réponse incorrecte 400 pour une opération `test` :</span><span class="sxs-lookup"><span data-stu-id="93456-276">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="93456-277">Objets dynamiques</span><span class="sxs-lookup"><span data-stu-id="93456-277">Dynamic objects</span></span>

<span data-ttu-id="93456-278">L’exemple de méthode d’action suivant montre comment appliquer un correctif à un objet dynamique.</span><span class="sxs-lookup"><span data-stu-id="93456-278">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="93456-279">L’opération d’ajout</span><span class="sxs-lookup"><span data-stu-id="93456-279">The add operation</span></span>

* <span data-ttu-id="93456-280">Si `path` pointe vers un élément du tableau : insère un nouvel élément avant celui spécifié par `path`.</span><span class="sxs-lookup"><span data-stu-id="93456-280">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="93456-281">Si `path` pointe vers une propriété : définit la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="93456-281">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="93456-282">Si `path` pointe vers un emplacement qui n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="93456-282">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="93456-283">Si la ressource à corriger est un objet dynamique : ajoute une propriété.</span><span class="sxs-lookup"><span data-stu-id="93456-283">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="93456-284">Si la ressource à corriger est un objet statique : la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="93456-284">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="93456-285">L’exemple de document de correctif suivant définit la valeur de `CustomerName` et ajoute un objet `Order` à la fin du tableau `Orders`.</span><span class="sxs-lookup"><span data-stu-id="93456-285">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="93456-286">L’opération de suppression</span><span class="sxs-lookup"><span data-stu-id="93456-286">The remove operation</span></span>

* <span data-ttu-id="93456-287">Si `path` pointe vers un élément du tableau : supprime l’élément.</span><span class="sxs-lookup"><span data-stu-id="93456-287">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="93456-288">Si `path` pointe vers une propriété :</span><span class="sxs-lookup"><span data-stu-id="93456-288">If `path` points to a property:</span></span>
  * <span data-ttu-id="93456-289">Si la ressource à corriger est un objet dynamique : supprime la propriété.</span><span class="sxs-lookup"><span data-stu-id="93456-289">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="93456-290">Si la ressource à corriger est un objet statique :</span><span class="sxs-lookup"><span data-stu-id="93456-290">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="93456-291">Si la propriété est Nullable : met la valeur sur Null.</span><span class="sxs-lookup"><span data-stu-id="93456-291">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="93456-292">Si la propriété n’est pas Nullable : met la valeur sur `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="93456-292">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="93456-293">L’exemple suivant de document de correctif définit `CustomerName` sur Null et supprime `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="93456-293">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="93456-294">L’opération de remplacement</span><span class="sxs-lookup"><span data-stu-id="93456-294">The replace operation</span></span>

<span data-ttu-id="93456-295">Cette opération est fonctionnellement identique à `remove` suivi de `add`.</span><span class="sxs-lookup"><span data-stu-id="93456-295">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="93456-296">L’exemple suivant de document de correctif définit la valeur de `CustomerName` et remplace `Orders[0]` avec un nouvel objet `Order`.</span><span class="sxs-lookup"><span data-stu-id="93456-296">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="93456-297">L’opération de déplacement</span><span class="sxs-lookup"><span data-stu-id="93456-297">The move operation</span></span>

* <span data-ttu-id="93456-298">Si `path` pointe vers un élément du tableau : copie l’élément `from` à l’emplacement de l’élément `path`, puis exécute une opération `remove` sur l’élément `from`.</span><span class="sxs-lookup"><span data-stu-id="93456-298">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="93456-299">Si `path` pointe vers une propriété : copie la valeur de la propriété `from` vers la propriété `path`, puis exécute une opération `remove` sur la propriété `from`.</span><span class="sxs-lookup"><span data-stu-id="93456-299">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="93456-300">Si `path` pointe vers une propriété qui n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="93456-300">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="93456-301">Si la ressource à corriger est un objet statique : la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="93456-301">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="93456-302">Si la ressource à corriger est un objet dynamique : copie la propriété `from` à un emplacement indiqué par `path`, puis exécute une opération `remove` sur la propriété `from`.</span><span class="sxs-lookup"><span data-stu-id="93456-302">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="93456-303">L’exemple de document de correctif suivant :</span><span class="sxs-lookup"><span data-stu-id="93456-303">The following sample patch document:</span></span>

* <span data-ttu-id="93456-304">Copie la valeur de `Orders[0].OrderName` vers `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="93456-304">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="93456-305">Met `Orders[0].OrderName` sur la valeur Null.</span><span class="sxs-lookup"><span data-stu-id="93456-305">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="93456-306">Déplace `Orders[1]` avant `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="93456-306">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="93456-307">L’opération de copie</span><span class="sxs-lookup"><span data-stu-id="93456-307">The copy operation</span></span>

<span data-ttu-id="93456-308">Cette opération est fonctionnellement identique à une opération `move` sans l’étape `remove` finale.</span><span class="sxs-lookup"><span data-stu-id="93456-308">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="93456-309">L’exemple de document de correctif suivant :</span><span class="sxs-lookup"><span data-stu-id="93456-309">The following sample patch document:</span></span>

* <span data-ttu-id="93456-310">Copie la valeur de `Orders[0].OrderName` vers `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="93456-310">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="93456-311">Insère une copie de `Orders[1]` avant `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="93456-311">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="93456-312">L’opération de test</span><span class="sxs-lookup"><span data-stu-id="93456-312">The test operation</span></span>

<span data-ttu-id="93456-313">Si la valeur à l’emplacement indiqué par `path` est différente de la valeur fournie dans `value`, la demande échoue.</span><span class="sxs-lookup"><span data-stu-id="93456-313">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="93456-314">Dans ce cas, toute la demande PATCH échoue, même si toutes les autres opérations dans le document de correctif réussiraient autrement.</span><span class="sxs-lookup"><span data-stu-id="93456-314">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="93456-315">L’opération `test` est généralement utilisée pour empêcher une mise à jour lorsqu’il y a un conflit d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="93456-315">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="93456-316">L’exemple de document de correctif suivant n’a aucun effet si la valeur initiale de `CustomerName` est « John », car le test échoue :</span><span class="sxs-lookup"><span data-stu-id="93456-316">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="93456-317">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="93456-317">Get the code</span></span>

<span data-ttu-id="93456-318">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="93456-318">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="93456-319">([Guide pratique de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="93456-319">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="93456-320">Pour tester l’exemple, exécutez l’application et envoyez des demandes HTTP avec les paramètres suivants :</span><span class="sxs-lookup"><span data-stu-id="93456-320">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="93456-321">URL : `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="93456-321">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="93456-322">Méthode HTTP : `PATCH`</span><span class="sxs-lookup"><span data-stu-id="93456-322">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="93456-323">En-tête : `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="93456-323">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="93456-324">Corps : copiez et collez l’un des exemples de document de correctif JSON à partir du dossier de projet *JSON* .</span><span class="sxs-lookup"><span data-stu-id="93456-324">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93456-325">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="93456-325">Additional resources</span></span>

* [<span data-ttu-id="93456-326">Spécification de méthode PATCH IETF RFC 5789 PATCH</span><span class="sxs-lookup"><span data-stu-id="93456-326">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="93456-327">Spécification JSON Patch IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="93456-327">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="93456-328">Spécification de format de chemin JSON Patch IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="93456-328">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="93456-329">[Documentation JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="93456-329">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="93456-330">Inclut des liens vers des ressources pour la création de documents JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="93456-330">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="93456-331">Code source ASP.NET Core JSON Patch</span><span class="sxs-lookup"><span data-stu-id="93456-331">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
