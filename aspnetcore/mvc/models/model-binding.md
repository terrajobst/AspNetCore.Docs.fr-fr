---
title: Liaison de données dans ASP.NET Core
author: rick-anderson
description: Découvrez comment fonctionne la liaison de modèle avec ASP.NET Core, et comment personnaliser son comportement.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 298e305cf918117ec2d313060a7420a1e721a365
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975295"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="5f8a9-103">Liaison de données dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f8a9-103">Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="5f8a9-104">Cet article explique ce qu’est la liaison de modèle, comment elle fonctionne et comment personnaliser son comportement.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="5f8a9-105">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="5f8a9-106">Description de la liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="5f8a9-106">What is Model binding</span></span>

<span data-ttu-id="5f8a9-107">Les contrôleurs et Razor Pages utilisent des données provenant de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="5f8a9-108">Par exemple, les données de routage peuvent fournir une clé d’enregistrement, et les champs de formulaire posté peuvent fournir des valeurs pour les propriétés du modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="5f8a9-109">L’écriture du code permettant de récupérer chacune de ces valeurs et de les convertir en types .NET à partir de chaînes est fastidieuse et source d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="5f8a9-110">La liaison de modèle automatise ce processus.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-110">Model binding automates this process.</span></span> <span data-ttu-id="5f8a9-111">Le système de liaison de modèle :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-111">The model binding system:</span></span>

* <span data-ttu-id="5f8a9-112">Récupère les données de diverses sources telles que les données de routage, les champs de formulaire et les chaînes de requête</span><span class="sxs-lookup"><span data-stu-id="5f8a9-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="5f8a9-113">Fournit les données aux contrôleurs et à Razor Pages dans les paramètres de méthode et les propriétés publiques</span><span class="sxs-lookup"><span data-stu-id="5f8a9-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="5f8a9-114">Convertit les données de chaîne en types .NET</span><span class="sxs-lookup"><span data-stu-id="5f8a9-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="5f8a9-115">Met à jour les propriétés des types complexes</span><span class="sxs-lookup"><span data-stu-id="5f8a9-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="5f8a9-116">Exemples</span><span class="sxs-lookup"><span data-stu-id="5f8a9-116">Example</span></span>

<span data-ttu-id="5f8a9-117">Supposons que vous ayez la méthode d’action suivante :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="5f8a9-118">Et que l’application reçoive une requête avec l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="5f8a9-119">La liaison de modèle suit les étapes ci-après une fois que le système de routage a sélectionné la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-119">Model binding goes though the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="5f8a9-120">Elle recherche le premier paramètre de `GetByID`, un entier nommé `id`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="5f8a9-121">Elle parcourt les sources disponibles dans la requête HTTP et trouve `id` = « 2 » dans les données de routage.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="5f8a9-122">Elle convertit la chaîne « 2 » en entier 2.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="5f8a9-123">Elle recherche le paramètre suivant de `GetByID`, un booléen nommé `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="5f8a9-124">Elle parcourt les sources et trouve « DogsOnly=true » dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="5f8a9-125">La mise en correspondance des noms ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="5f8a9-126">Elle convertit la chaîne « true » en booléen `true`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="5f8a9-127">Le framework appelle ensuite la méthode `GetById`, en passant 2 pour le paramètre `id`, et `true` pour le paramètre `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="5f8a9-128">Dans l’exemple précédent, les cibles de liaison de modèle sont des paramètres de méthode qui sont des types simples.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="5f8a9-129">Les cibles peuvent être également les propriétés d’un type complexe.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="5f8a9-130">Une fois chaque propriété correctement liée, la [validation du modèle](xref:mvc/models/validation) est effectuée pour la propriété concernée.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="5f8a9-131">L’enregistrement des données liées au modèle (ainsi que des erreurs de liaison ou de validation) est stocké dans [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) ou [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="5f8a9-132">Pour savoir si ce processus a abouti, l’application vérifie l’indicateur [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="5f8a9-133">Cibles</span><span class="sxs-lookup"><span data-stu-id="5f8a9-133">Targets</span></span>

<span data-ttu-id="5f8a9-134">La liaison de modèle tente de trouver des valeurs pour les genres de cible suivants :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="5f8a9-135">Paramètres de la méthode d’action de contrôleur vers laquelle une requête est routée.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="5f8a9-136">Paramètres de la méthode de gestionnaire Razor Pages vers laquelle une requête est routée.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="5f8a9-137">Propriétés publiques d’un contrôleur ou d’une classe `PageModel`, si elles sont spécifiées par des attributs.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="5f8a9-138">Attribut [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="5f8a9-138">[BindProperty] attribute</span></span>

<span data-ttu-id="5f8a9-139">Peut être appliqué à une propriété publique d’un contrôleur ou à une classe `PageModel` pour que la liaison de modèle cible cette propriété :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="5f8a9-140">Attribut [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="5f8a9-140">[BindProperties] attribute</span></span>

<span data-ttu-id="5f8a9-141">Disponible avec ASP.NET Core 2.1 et les versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="5f8a9-142">Peut être appliqué à un contrôleur ou à une classe `PageModel` pour indiquer à la liaison de modèle de cibler toutes les propriétés publiques de la classe :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="5f8a9-143">Liaison de modèle pour les requêtes HTTP GET</span><span class="sxs-lookup"><span data-stu-id="5f8a9-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="5f8a9-144">Par défaut, les propriétés ne sont pas liées pour les requêtes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="5f8a9-145">En règle générale, le paramètre ID d’un enregistrement est tout ce dont vous avez besoin pour une requête GET.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="5f8a9-146">L’ID d’enregistrement est utilisé pour rechercher l’élément dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="5f8a9-147">Il n’est donc pas nécessaire de lier une propriété qui contient une instance du modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="5f8a9-148">Pour les scénarios dans lesquels vous souhaitez que les propriétés soient liées aux données provenant de requêtes GET, affectez à la propriété `SupportsGet` la valeur `true` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="5f8a9-149">Sources</span><span class="sxs-lookup"><span data-stu-id="5f8a9-149">Sources</span></span>

<span data-ttu-id="5f8a9-150">Par défaut, la liaison de modèle obtient des données sous la forme de paires clé-valeur à partir des sources suivantes dans une requête HTTP :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="5f8a9-151">Champs de formulaire</span><span class="sxs-lookup"><span data-stu-id="5f8a9-151">Form fields</span></span> 
1. <span data-ttu-id="5f8a9-152">Corps de la requête (pour les [contrôleurs ayant l’attribut [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="5f8a9-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="5f8a9-153">Données de route</span><span class="sxs-lookup"><span data-stu-id="5f8a9-153">Route data</span></span>
1. <span data-ttu-id="5f8a9-154">Paramètres de la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="5f8a9-154">Query string parameters</span></span>
1. <span data-ttu-id="5f8a9-155">Fichiers chargés</span><span class="sxs-lookup"><span data-stu-id="5f8a9-155">Uploaded files</span></span> 

<span data-ttu-id="5f8a9-156">Pour chaque paramètre ou propriété cible, les sources sont analysées dans l’ordre indiqué dans cette liste.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-156">For each target parameter or property, the sources are scanned in the order indicated in this list.</span></span> <span data-ttu-id="5f8a9-157">Il existe quelques exceptions :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-157">There are a few exceptions:</span></span>

* <span data-ttu-id="5f8a9-158">Les données de routage et les valeurs de chaîne de requête sont utilisées uniquement pour les types simples.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="5f8a9-159">Les fichiers chargés sont liés uniquement aux types cibles qui implémentent `IFormFile` ou `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="5f8a9-160">Si le comportement par défaut ne donne pas les bons résultats, vous pouvez vous servir de l’un des attributs suivants pour spécifier la source à utiliser pour une cible donnée.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-160">If the default behavior doesn't give the right results, you can use one of the following attributes to specify the source to use for any given target.</span></span> 

* <span data-ttu-id="5f8a9-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Obtient les valeurs à partir de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="5f8a9-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Obtient les valeurs à partir des données de routage.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="5f8a9-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Obtient les valeurs à partir des champs de formulaire postés.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="5f8a9-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Obtient les valeurs à partir du corps de la requête.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="5f8a9-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Obtient les valeurs à partir des en-têtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="5f8a9-166">Ces attributs :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-166">These attributes:</span></span>

* <span data-ttu-id="5f8a9-167">Sont ajoutés aux propriétés du modèle individuellement (et non à la classe de modèle), comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="5f8a9-168">Acceptent éventuellement une valeur de nom de modèle dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="5f8a9-169">Cette option est fournie au cas où le nom de propriété ne correspondrait pas à la valeur de la requête.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="5f8a9-170">Par exemple, la valeur de la requête peut être un en-tête avec un trait d’union dans son nom, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="5f8a9-171">Attribut [FromBody]</span><span class="sxs-lookup"><span data-stu-id="5f8a9-171">[FromBody] attribute</span></span>

<span data-ttu-id="5f8a9-172">Les données du corps de la requête sont analysées à l’aide de formateurs d’entrée spécifiques au type de contenu de la requête.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-172">The request body data is parsed by using input formatters specific to the content type of the request.</span></span> <span data-ttu-id="5f8a9-173">Les formateurs d’entrée sont décrits [plus loin dans cet article](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-173">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="5f8a9-174">N’appliquez pas `[FromBody]` à plus d’un paramètre par méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-174">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="5f8a9-175">Le runtime ASP.NET Core délègue la responsabilité de la lecture du flux de requête au formateur d’entrée.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-175">The ASP.NET Core runtime delegates the responsibility of reading the request stream to the input formatter.</span></span> <span data-ttu-id="5f8a9-176">Une fois le flux de requête lu, il ne peut plus être relu pour lier d’autres paramètres `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-176">Once the request stream is read, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="5f8a9-177">Sources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5f8a9-177">Additional sources</span></span>

<span data-ttu-id="5f8a9-178">Les données sources sont fournies au système de liaison de modèle par les *fournisseurs de valeurs*.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-178">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="5f8a9-179">Vous pouvez écrire et inscrire des fournisseurs de valeurs personnalisés qui obtiennent des données de liaison de modèle à partir d’autres sources.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-179">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="5f8a9-180">Par exemple, vous pouvez obtenir des données provenant de cookies ou de l’état de session.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-180">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="5f8a9-181">Pour obtenir des données provenant d’une nouvelle source :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-181">To get data from a new source:</span></span>

* <span data-ttu-id="5f8a9-182">Créez une classe qui implémente `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-182">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="5f8a9-183">Créez une classe qui implémente `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-183">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="5f8a9-184">Inscrivez la classe de fabrique dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-184">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="5f8a9-185">L’exemple d’application comprend un exemple de [fournisseur de valeurs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) et de [fabrique](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs), qui permet de récupérer les valeurs provenant des cookies.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-185">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="5f8a9-186">Voici le code d’inscription dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-186">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="5f8a9-187">Le code affiché place le fournisseur de valeurs personnalisé après tous les fournisseurs de valeurs intégrés.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-187">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="5f8a9-188">Pour en faire le premier fournisseur de la liste, appelez `Insert(0, new CookieValueProviderFactory())` à la place de `Add`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-188">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="5f8a9-189">Aucune source pour une propriété de modèle</span><span class="sxs-lookup"><span data-stu-id="5f8a9-189">No source for a model property</span></span>

<span data-ttu-id="5f8a9-190">Par défaut, aucune erreur d’état de modèle n’est créée, s’il n’existe aucune valeur de propriété de modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-190">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="5f8a9-191">La propriété a une valeur null ou une valeur par défaut :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-191">The property is set to null or a default value:</span></span>

* <span data-ttu-id="5f8a9-192">Les types simples Nullable ont une valeur `null`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-192">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="5f8a9-193">Les types valeur non Nullable ont la valeur `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-193">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="5f8a9-194">Par exemple, un paramètre `int id` a la valeur 0.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-194">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="5f8a9-195">Pour les types complexes, la liaison de modèle crée une instance à l’aide du constructeur par défaut, sans définir de propriétés.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-195">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="5f8a9-196">Les tableaux ont la valeur `Array.Empty<T>()`, sauf les tableaux `byte[]` qui ont une valeur `null`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-196">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="5f8a9-197">Si l’état de modèle doit être rendu non valide quand les champs de formulaire d’une propriété de modèle ne contiennent rien, utilisez l’attribut [[BindRequired]](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-197">If model state should be invalidated when nothing is found in form fields for a model property, use the [[BindRequired] attribute](#bindrequired-attribute).</span></span>

<span data-ttu-id="5f8a9-198">Notez que ce comportement de `[BindRequired]` s’applique à la liaison de modèle des données de formulaire postées, et non aux données JSON ou XML d’un corps de requête.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-198">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="5f8a9-199">Les données du corps de requête sont prises en charge par les [formateurs d’entrée](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-199">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="5f8a9-200">Erreurs de conversion de type</span><span class="sxs-lookup"><span data-stu-id="5f8a9-200">Type conversion errors</span></span>

<span data-ttu-id="5f8a9-201">Si une source est localisée mais qu’elle ne peut pas être convertie vers le type cible, l’état du modèle est marqué comme étant non valide.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-201">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="5f8a9-202">Le paramètre ou la propriété cible a une valeur null ou une valeur par défaut, comme indiqué dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-202">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="5f8a9-203">Dans un contrôleur d’API ayant l’attribut `[ApiController]`, un état de modèle non valide entraîne une réponse HTTP 400 automatique.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-203">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="5f8a9-204">Dans une page Razor Pages, réaffichez la page avec un message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-204">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="5f8a9-205">La validation côté client intercepte la plupart des données incorrectes qui sont envoyées à un formulaire Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-205">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="5f8a9-206">Cette validation rend difficile le déclenchement du code en surbrillance indiqué plus haut.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-206">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="5f8a9-207">L’exemple d’application comprend un bouton **Submit with Invalid Date** (Envoyer avec une date non valide), qui place les données incorrectes dans le champ **Hire Date** (Date d’embauche) et envoie le formulaire.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-207">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="5f8a9-208">Ce bouton montre comment fonctionne le code permettant de réafficher la page quand des erreurs de conversion de données se produisent.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-208">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="5f8a9-209">Quand la page est réaffichée par le code précédent, l’entrée non valide n’est pas visible dans le champ de formulaire.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-209">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="5f8a9-210">En effet, la propriété de modèle à une valeur null ou une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-210">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="5f8a9-211">L’entrée non valide apparaît dans un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-211">The invalid input does appear in an error message.</span></span> <span data-ttu-id="5f8a9-212">Toutefois, si vous souhaitez réafficher les données incorrectes dans le champ de formulaire, transformez la propriété de modèle en chaîne et procédez à la conversion des données manuellement.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-212">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="5f8a9-213">La même stratégie est recommandée si vous ne souhaitez pas que les erreurs de conversion de type entraînent des erreurs d’état de modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-213">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="5f8a9-214">Dans ce cas, transformez la propriété de modèle en chaîne.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-214">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="5f8a9-215">Types simples</span><span class="sxs-lookup"><span data-stu-id="5f8a9-215">Simple types</span></span>

<span data-ttu-id="5f8a9-216">Les types simples que le lieur de modèle peut convertir en chaînes sources sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-216">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="5f8a9-217">Boolean</span><span class="sxs-lookup"><span data-stu-id="5f8a9-217">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="5f8a9-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="5f8a9-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="5f8a9-219">Char</span><span class="sxs-lookup"><span data-stu-id="5f8a9-219">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="5f8a9-220">DateTime</span><span class="sxs-lookup"><span data-stu-id="5f8a9-220">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="5f8a9-221">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="5f8a9-221">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="5f8a9-222">Decimal</span><span class="sxs-lookup"><span data-stu-id="5f8a9-222">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="5f8a9-223">Double</span><span class="sxs-lookup"><span data-stu-id="5f8a9-223">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="5f8a9-224">Enum</span><span class="sxs-lookup"><span data-stu-id="5f8a9-224">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="5f8a9-225">Guid</span><span class="sxs-lookup"><span data-stu-id="5f8a9-225">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="5f8a9-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="5f8a9-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="5f8a9-227">Single</span><span class="sxs-lookup"><span data-stu-id="5f8a9-227">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="5f8a9-228">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="5f8a9-228">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="5f8a9-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="5f8a9-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="5f8a9-230">Uri</span><span class="sxs-lookup"><span data-stu-id="5f8a9-230">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="5f8a9-231">Version</span><span class="sxs-lookup"><span data-stu-id="5f8a9-231">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="5f8a9-232">Types complexes</span><span class="sxs-lookup"><span data-stu-id="5f8a9-232">Complex types</span></span>

<span data-ttu-id="5f8a9-233">Un type complexe doit avoir un constructeur public par défaut et des propriétés publiques accessibles en écriture à lier.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-233">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="5f8a9-234">Quand la liaison de modèle se produit, la classe est instanciée à l’aide du constructeur public par défaut.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-234">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="5f8a9-235">Pour chaque propriété du type complexe, la liaison de modèle recherche dans les sources le modèle de nom *préfixe.nom_propriété*.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-235">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="5f8a9-236">Si rien n’est trouvé, elle recherche uniquement *nom_propriété* sans le préfixe.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-236">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="5f8a9-237">Dans le cas d’une liaison à un paramètre, le préfixe représente le nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-237">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="5f8a9-238">Dans le cas d’une liaison à une propriété publique `PageModel`, le préfixe représente le nom de la propriété publique.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-238">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="5f8a9-239">Certains attributs ont une propriété `Prefix` qui vous permet de remplacer l’utilisation par défaut du nom de paramètre ou de propriété.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-239">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="5f8a9-240">Par exemple, supposons que le type complexe corresponde à la classe `Instructor` suivante :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-240">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="5f8a9-241">Préfixe = nom de paramètre</span><span class="sxs-lookup"><span data-stu-id="5f8a9-241">Prefix = parameter name</span></span>

<span data-ttu-id="5f8a9-242">Si le modèle à lier est un paramètre nommé `instructorToUpdate` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-242">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="5f8a9-243">La liaison de modèle commence par rechercher dans les sources la clé `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-243">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="5f8a9-244">Si elle est introuvable, elle recherche `ID` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-244">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="5f8a9-245">Préfixe = nom de propriété</span><span class="sxs-lookup"><span data-stu-id="5f8a9-245">Prefix = property name</span></span>

<span data-ttu-id="5f8a9-246">Si le modèle à lier est une propriété nommée `Instructor` du contrôleur ou de la classe `PageModel` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-246">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="5f8a9-247">La liaison de modèle commence par rechercher dans les sources la clé `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-247">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="5f8a9-248">Si elle est introuvable, elle recherche `ID` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-248">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="5f8a9-249">Préfixe personnalisé</span><span class="sxs-lookup"><span data-stu-id="5f8a9-249">Custom prefix</span></span>

<span data-ttu-id="5f8a9-250">Si le modèle à lier est un paramètre nommé `instructorToUpdate` et si un attribut `Bind` spécifie `Instructor` en tant que préfixe :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-250">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="5f8a9-251">La liaison de modèle commence par rechercher dans les sources la clé `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-251">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="5f8a9-252">Si elle est introuvable, elle recherche `ID` sans préfixe.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="5f8a9-253">Attributs des cibles de type complexe</span><span class="sxs-lookup"><span data-stu-id="5f8a9-253">Attributes for complex type targets</span></span>

<span data-ttu-id="5f8a9-254">Plusieurs attributs intégrés sont disponibles pour contrôler la liaison de modèle des types complexes :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-254">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="5f8a9-255">Ces attributs affectent la liaison de modèle quand les données de formulaire postées représentent la source des valeurs.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-255">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="5f8a9-256">Ils n’affectent pas les formateurs d’entrée, qui traitent les corps de requête JSON et XML postés.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-256">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="5f8a9-257">Les formateurs d’entrée sont décrits [plus loin dans cet article](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-257">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="5f8a9-258">Consultez également la discussion sur l’attribut `[Required]` dans [Validation de modèle](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-258">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="5f8a9-259">Attribut [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="5f8a9-259">[BindRequired] attribute</span></span>

<span data-ttu-id="5f8a9-260">Il s’applique uniquement aux propriétés de modèle, pas aux paramètres de méthode.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-260">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="5f8a9-261">Il oblige la liaison de modèle à ajouter une erreur d’état de modèle si la liaison est impossible pour la propriété d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-261">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="5f8a9-262">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-262">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="5f8a9-263">Attribut [BindNever]</span><span class="sxs-lookup"><span data-stu-id="5f8a9-263">[BindNever] attribute</span></span>

<span data-ttu-id="5f8a9-264">Il s’applique uniquement aux propriétés de modèle, pas aux paramètres de méthode.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-264">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="5f8a9-265">Il empêche la liaison de modèle de définir la propriété d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-265">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="5f8a9-266">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-266">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="5f8a9-267">Attribut [Bind]</span><span class="sxs-lookup"><span data-stu-id="5f8a9-267">[Bind] attribute</span></span>

<span data-ttu-id="5f8a9-268">Il peut être appliqué à une classe ou à un paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-268">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="5f8a9-269">Il spécifie les propriétés d’un modèle à inclure dans la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-269">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="5f8a9-270">Dans l’exemple suivant, seules les propriétés spécifiées du modèle `Instructor` sont liées quand une méthode de gestionnaire ou une méthode d’action est appelée :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-270">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="5f8a9-271">Dans l’exemple suivant, seules les propriétés spécifiées du modèle `Instructor` sont liées quand la méthode `OnPost` est appelée :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-271">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="5f8a9-272">Vous pouvez utiliser l’attribut `[Bind]` pour éviter le surpostage dans les scénarios de *création*.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-272">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="5f8a9-273">Il ne fonctionne pas bien dans les scénarios de modification, car les propriétés exclues ont une valeur null ou une valeur par défaut au lieu de rester inchangées.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-273">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="5f8a9-274">Pour empêcher le surpostage, il est recommandé d’utiliser des modèles de vues à la place de l’attribut `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-274">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="5f8a9-275">Pour plus d’informations, consultez [Remarque sur la sécurité concernant le surpostage](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-275">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="5f8a9-276">Collections</span><span class="sxs-lookup"><span data-stu-id="5f8a9-276">Collections</span></span>

<span data-ttu-id="5f8a9-277">Pour les cibles qui sont des collections de types simples, la liaison de modèle recherche les correspondances avec *nom_paramètre* ou *nom_propriété*.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-277">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="5f8a9-278">Si aucune correspondance n’est localisée, elle recherche l’un des formats pris en charge sans le préfixe.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-278">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="5f8a9-279">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-279">For example:</span></span>

* <span data-ttu-id="5f8a9-280">Supposons que le paramètre à lier soit un tableau nommé `selectedCourses` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-280">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="5f8a9-281">Les données de formulaire ou de chaîne de requête peuvent avoir l’un des formats suivants :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-281">Form or query string data can be in one of the following formats:</span></span>
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* <span data-ttu-id="5f8a9-282">Le format suivant est pris en charge uniquement dans les données de formulaire :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-282">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="5f8a9-283">Pour tous les exemples de formats précédents, la liaison de modèle passe un tableau de deux éléments au paramètre `selectedCourses` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-283">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="5f8a9-284">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="5f8a9-284">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="5f8a9-285">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="5f8a9-285">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="5f8a9-286">Les formats de données qui utilisent des nombres en indice (... [0] ... [1] ...) doivent être impérativement numérotés de manière séquentielle à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-286">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="5f8a9-287">S’il existe des vides dans la numérotation en indice, tous les éléments suivants sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-287">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="5f8a9-288">Par exemple, si les indices sont 0 et 2 au lieu de 0 et 1, le second élément est ignoré.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-288">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="5f8a9-289">Dictionnaires</span><span class="sxs-lookup"><span data-stu-id="5f8a9-289">Dictionaries</span></span>

<span data-ttu-id="5f8a9-290">Pour les cibles `Dictionary`, la liaison de modèle recherche les correspondances avec *nom_paramètre* ou *nom_propriété*.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-290">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="5f8a9-291">Si aucune correspondance n’est localisée, elle recherche l’un des formats pris en charge sans le préfixe.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-291">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="5f8a9-292">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-292">For example:</span></span>

* <span data-ttu-id="5f8a9-293">Supposons que le paramètre cible soit un `Dictionary<int, string>` nommé `selectedCourses` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-293">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="5f8a9-294">Les données de chaîne de requête ou de formulaire posté peuvent ressembler à l’un des exemples suivants :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-294">The posted form or query string data can look like one of the following examples:</span></span>

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* <span data-ttu-id="5f8a9-295">Pour tous les exemples de formats précédents, la liaison de modèle passe un dictionnaire de deux éléments au paramètre `selectedCourses` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-295">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="5f8a9-296">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="5f8a9-296">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="5f8a9-297">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="5f8a9-297">selectedCourses["2000"]="Economics"</span></span>

## <a name="special-data-types"></a><span data-ttu-id="5f8a9-298">Types de données spéciaux</span><span class="sxs-lookup"><span data-stu-id="5f8a9-298">Special data types</span></span>

<span data-ttu-id="5f8a9-299">Certains types de données spéciaux peuvent être pris en charge par la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-299">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="5f8a9-300">IFormFile et IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="5f8a9-300">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="5f8a9-301">Fichier chargé inclus dans la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-301">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="5f8a9-302">`IEnumerable<IFormFile>` est également pris en charge pour plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-302">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="5f8a9-303">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="5f8a9-303">CancellationToken</span></span>

<span data-ttu-id="5f8a9-304">utilisé pour annuler l’activité dans les contrôleurs asynchrones.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-304">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="5f8a9-305">FormCollection</span><span class="sxs-lookup"><span data-stu-id="5f8a9-305">FormCollection</span></span>

<span data-ttu-id="5f8a9-306">Permet de récupérer toutes les valeurs des données de formulaire posté.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-306">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="5f8a9-307">Formateurs d’entrée</span><span class="sxs-lookup"><span data-stu-id="5f8a9-307">Input formatters</span></span>

<span data-ttu-id="5f8a9-308">Les données contenues dans le corps de la requête peuvent être au format JSON, XML ou tout autre format.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-308">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="5f8a9-309">Pour analyser ces données, la liaison de modèle utilise un *formateur d’entrée* configuré pour prendre en charge un type de contenu particulier.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-309">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="5f8a9-310">Par défaut, ASP.NET Core inclut des formateurs d’entrée basés sur le format JSON pour prendre en charge les données JSON.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-310">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="5f8a9-311">Vous pouvez ajouter d’autres formateurs pour d’autres types de contenu.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-311">You can add other formatters for other content types.</span></span>

<span data-ttu-id="5f8a9-312">ASP.NET Core sélectionne les formateurs d’entrée en fonction de l’attribut [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-312">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="5f8a9-313">Si aucun attribut n’est présent, il utilise l’[en-tête Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-313">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="5f8a9-314">Pour utiliser les formateurs d’entrée XML intégrés :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-314">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="5f8a9-315">Installez le package NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-315">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="5f8a9-316">Dans `Startup.ConfigureServices`, appelez <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> ou <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-316">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="5f8a9-317">Appliquez l’attribut `Consumes` aux classes de contrôleur ou aux méthodes d’action devant contenir des données XML dans le corps de la requête.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-317">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="5f8a9-318">Pour plus d’informations, consultez [Introduction à la sérialisation XML](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-318">For more information, see [Introducing XML Serialization](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="5f8a9-319">Exclure les types spécifiés de la liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="5f8a9-319">Exclude specified types from model binding</span></span>

<span data-ttu-id="5f8a9-320">Le comportement de la liaison de modèle et du système de validation est régi par [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-320">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="5f8a9-321">Vous pouvez personnaliser `ModelMetadata` en ajoutant un fournisseur de détails à [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-321">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="5f8a9-322">Des fournisseurs de détails intégrés sont disponibles pour désactiver la liaison de modèle ou la validation des types spécifiés.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-322">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="5f8a9-323">Pour désactiver la liaison de modèle sur tous les modèles d’un type spécifique, ajoutez <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-323">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5f8a9-324">Par exemple, pour désactiver la liaison de modèle sur tous les modèles de type `System.Version` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-324">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="5f8a9-325">Pour désactiver la validation des propriétés d’un type spécifique, ajoutez <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-325">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5f8a9-326">Par exemple, pour désactiver la validation sur les propriétés de type `System.Guid` :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-326">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="5f8a9-327">Lieurs de modèles personnalisés</span><span class="sxs-lookup"><span data-stu-id="5f8a9-327">Custom model binders</span></span>

<span data-ttu-id="5f8a9-328">Vous pouvez étendre la liaison de modèle en écrivant un lieur de modèle personnalisé et en utilisant l’attribut `[ModelBinder]` afin de le sélectionner pour une cible donnée.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-328">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="5f8a9-329">Découvrez plus d’informations sur la [liaison de modèle personnalisée](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-329">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="5f8a9-330">Liaison de modèle manuelle</span><span class="sxs-lookup"><span data-stu-id="5f8a9-330">Manual model binding</span></span>

<span data-ttu-id="5f8a9-331">Vous pouvez appeler la liaison de modèle manuellement à l’aide de la méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-331">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="5f8a9-332">La méthode est définie sur les classes `ControllerBase` et `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-332">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="5f8a9-333">Les surcharges de méthode vous permettent de spécifier le préfixe et le fournisseur de valeurs à utiliser.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-333">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="5f8a9-334">La méthode retourne `false` en cas d’échec de la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-334">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="5f8a9-335">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="5f8a9-335">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="5f8a9-336">Attribut [FromServices]</span><span class="sxs-lookup"><span data-stu-id="5f8a9-336">[FromServices] attribute</span></span>

<span data-ttu-id="5f8a9-337">Le nom de cet attribut suit le modèle des attributs de liaison de modèle qui spécifient une source de données.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-337">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="5f8a9-338">Toutefois, il ne permet pas de lier les données d’un fournisseur de valeurs.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-338">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="5f8a9-339">Il obtient une instance d’un type à partir du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5f8a9-339">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="5f8a9-340">Son objectif est de fournir une alternative à l’injection de constructeurs quand vous avez besoin d’un service uniquement si une méthode particulière est appelée.</span><span class="sxs-lookup"><span data-stu-id="5f8a9-340">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5f8a9-341">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5f8a9-341">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
