---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les principes fondamentaux de la création d’une API web dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/11/2019
uid: web-api/index
ms.openlocfilehash: d804a7f1b4f0e89f433a3674116c97804705f7cc
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64882954"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="7b6d5-103">Créer des API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b6d5-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="7b6d5-104">De [Scott Addie](https://github.com/scottaddie) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7b6d5-105">ASP.NET Core prend en charge la création de services RESTful, également appelés API web, à l’aide de C#.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="7b6d5-106">Pour traiter les demandes, une API web utilise des contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="7b6d5-107">Les *contrôleurs* dans une API web sont des classes qui dérivent de `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="7b6d5-108">Cet article montre comment utiliser des contrôleurs pour gérer des demandes d’API.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-108">This article shows how to use controllers for handling API requests.</span></span>

<span data-ttu-id="7b6d5-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="7b6d5-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="7b6d5-110">([Guide pratique de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7b6d5-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="7b6d5-111">Classe ControllerBase</span><span class="sxs-lookup"><span data-stu-id="7b6d5-111">ControllerBase class</span></span>

<span data-ttu-id="7b6d5-112">Une API web a une ou plusieurs classes de contrôleur qui dérivent de <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-112">A web API has one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="7b6d5-113">Par exemple, le modèle de projet d’API web crée un contrôleur Values :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-113">For example, the web API project template creates a Values controller:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

<span data-ttu-id="7b6d5-114">Ne créez pas de contrôleur d’API web en effectuant une dérivation de la classe de base <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class.</span></span> <span data-ttu-id="7b6d5-115">`Controller` dérive de `ControllerBase` et ajoute la prise en charge pour les vues ; ainsi, il est destiné à la gestion des pages web, pas des demandes d’API web.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span>  <span data-ttu-id="7b6d5-116">Il existe une exception à cette règle : si vous envisagez d’utiliser le même contrôleur pour les vues et les API, dérivez-le de `Controller`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-116">There's an exception to this rule: if you plan to use the same controller for both views and APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="7b6d5-117">La classe `ControllerBase` fournit de nombreuses propriétés et méthodes qui sont utiles pour gérer les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="7b6d5-118">Par exemple, `ControllerBase.CreatedAtAction` retourne un code d’état 201 :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 <span data-ttu-id="7b6d5-119">Voici d’autres exemples de méthodes fournies par `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="7b6d5-120">Méthode</span><span class="sxs-lookup"><span data-stu-id="7b6d5-120">Method</span></span>  |<span data-ttu-id="7b6d5-121">Notes</span><span class="sxs-lookup"><span data-stu-id="7b6d5-121">Notes</span></span>  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="7b6d5-122">Retourne le code d’état 400.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |<span data-ttu-id="7b6d5-123">Retourne le code d’état 404.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="7b6d5-124">Retourne un fichier.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="7b6d5-125">Appelle la [liaison de modèle](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="7b6d5-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="7b6d5-126">Appelle la [validation de modèle](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="7b6d5-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="7b6d5-127">Pour obtenir la liste complète des méthodes et propriétés disponibles, consultez <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="7b6d5-128">Attributs</span><span class="sxs-lookup"><span data-stu-id="7b6d5-128">Attributes</span></span>

<span data-ttu-id="7b6d5-129">L’espace de noms <xref:Microsoft.AspNetCore.Mvc> fournit des attributs qui peuvent être utilisés pour configurer le comportement des contrôleurs d’API web et des méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="7b6d5-130">L’exemple suivant utilise des attributs pour spécifier la méthode HTTP acceptée et les codes d’état retournés :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-130">The following example uses attributes to specify the HTTP method accepted and the status codes returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="7b6d5-131">Voici d’autres exemples d’attributs disponibles.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="7b6d5-132">Attribut</span><span class="sxs-lookup"><span data-stu-id="7b6d5-132">Attribute</span></span>|<span data-ttu-id="7b6d5-133">Notes</span><span class="sxs-lookup"><span data-stu-id="7b6d5-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="7b6d5-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="7b6d5-135">Spécifie le modèle d’URL pour un contrôleur ou une action.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="7b6d5-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="7b6d5-137">Spécifie le préfixe et les propriétés à inclure pour la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="7b6d5-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="7b6d5-139">Identifie une action qui prend en charge la méthode HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-139">Identifies an action that supports the HTTP GET method.</span></span>|
|<span data-ttu-id="7b6d5-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="7b6d5-141">Spécifie les types de données acceptés par une action.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="7b6d5-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="7b6d5-143">Spécifie les types de données retournés par une action.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="7b6d5-144">Pour obtenir la liste des attributs disponibles, consultez l’espace de noms <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="7b6d5-145">Attribut ApiController</span><span class="sxs-lookup"><span data-stu-id="7b6d5-145">ApiController attribute</span></span>

<span data-ttu-id="7b6d5-146">L’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) peut être appliqué à une classe de contrôleur pour activer les comportements spécifiques à une API :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable API-specific behaviors:</span></span>

* [<span data-ttu-id="7b6d5-147">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="7b6d5-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="7b6d5-148">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="7b6d5-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="7b6d5-149">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="7b6d5-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="7b6d5-150">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="7b6d5-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="7b6d5-151">Fonctionnalité Détails du problème pour les codes d’état erreur</span><span class="sxs-lookup"><span data-stu-id="7b6d5-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="7b6d5-152">Ces fonctionnalités nécessitent une [version de compatibilité](<xref:mvc/compatibility-version>) 2.1 ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-152">These features require a [compatibility version](<xref:mvc/compatibility-version>) of 2.1 or later.</span></span>

### <a name="apicontroller-on-specific-controllers"></a><span data-ttu-id="7b6d5-153">ApiController sur des contrôleurs spécifiques</span><span class="sxs-lookup"><span data-stu-id="7b6d5-153">ApiController on specific controllers</span></span>

<span data-ttu-id="7b6d5-154">L’attribut `[ApiController]` peut être appliqué à des contrôleurs spécifiques, comme dans l’exemple suivant à partir du modèle de projet :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a><span data-ttu-id="7b6d5-155">ApiController sur plusieurs contrôleurs</span><span class="sxs-lookup"><span data-stu-id="7b6d5-155">ApiController on multiple controllers</span></span>

<span data-ttu-id="7b6d5-156">Une façon d’utiliser l’attribut sur plusieurs contrôleurs consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="7b6d5-157">Voici un exemple illustrant une classe de base personnalisée et un contrôleur qui dérive de celle-ci :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-157">Here's an example showing a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a><span data-ttu-id="7b6d5-158">ApiController sur un assembly</span><span class="sxs-lookup"><span data-stu-id="7b6d5-158">ApiController on an assembly</span></span>

<span data-ttu-id="7b6d5-159">Si la [version de compatibilité](<xref:mvc/compatibility-version>) est définie sur 2.2 ou une version ultérieure, l’attribut `[ApiController]` peut être appliqué à un assembly.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-159">If [compatibility version](<xref:mvc/compatibility-version>) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="7b6d5-160">De cette manière, l’annotation applique le comportement de l’API web à tous les contrôleurs de l’assembly.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="7b6d5-161">Les contrôleurs individuels n’ont aucun moyen de refuser.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="7b6d5-162">Appliquez l’attribut de niveau assembly à la classe `Startup`, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-162">Apply the assembly-level attribute to the `Startup` class as shown in the following example:</span></span>

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

## <a name="attribute-routing-requirement"></a><span data-ttu-id="7b6d5-163">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="7b6d5-163">Attribute routing requirement</span></span>

<span data-ttu-id="7b6d5-164">L’attribut `ApiController` rend nécessaire le routage d’attributs.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-164">The `ApiController` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="7b6d5-165">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-165">For example:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

<span data-ttu-id="7b6d5-166">Les actions sont inaccessibles par le biais de [routes conventionnelles](xref:mvc/controllers/routing#conventional-routing) définies par <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

## <a name="automatic-http-400-responses"></a><span data-ttu-id="7b6d5-167">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="7b6d5-167">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="7b6d5-168">Grâce à l’attribut `ApiController`, les erreurs de validation de modèles déclenchent automatiquement une réponse HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-168">The `ApiController` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="7b6d5-169">Ainsi, le code suivant est inutile dans une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-169">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a><span data-ttu-id="7b6d5-170">Réponse BadRequest par défaut</span><span class="sxs-lookup"><span data-stu-id="7b6d5-170">Default BadRequest response</span></span> 

<span data-ttu-id="7b6d5-171">Avec une version de compatibilité 2.2 ou ultérieure, le type de réponse par défaut pour les réponses HTTP 400 est <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-171">With a compatibility version of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="7b6d5-172">Le type `ValidationProblemDetails` est conforme à la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="7b6d5-172">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="7b6d5-173">Pour remplacer la réponse par défaut par <xref:Microsoft.AspNetCore.Mvc.SerializableError>, définissez la propriété `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` sur `true` dans `Startup.ConfigureServices`, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-173">To change the default response to <xref:Microsoft.AspNetCore.Mvc.SerializableError>, set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a><span data-ttu-id="7b6d5-174">Personnaliser la réponse BadRequest</span><span class="sxs-lookup"><span data-stu-id="7b6d5-174">Customize BadRequest response</span></span>

<span data-ttu-id="7b6d5-175">Pour personnaliser la réponse qui résulte d’une erreur de validation, utilisez <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-175">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="7b6d5-176">Ajoutez le code en surbrillance suivant après `services.AddMvc().SetCompatibilityVersion` :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-176">Add the following highlighted code after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="disable-automatic-400"></a><span data-ttu-id="7b6d5-177">Désactiver le comportement 400 automatique</span><span class="sxs-lookup"><span data-stu-id="7b6d5-177">Disable automatic 400</span></span>

<span data-ttu-id="7b6d5-178">Pour désactiver le comportement 400 automatique, définissez la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> sur `true`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-178">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="7b6d5-179">Ajoutez le code en surbrillance suivant dans `Startup.ConfigureServices` après `services.AddMvc().SetCompatibilityVersion` :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-179">Add the following highlighted code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="7b6d5-180">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="7b6d5-180">Binding source parameter inference</span></span>

<span data-ttu-id="7b6d5-181">Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-181">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="7b6d5-182">Les attributs de source de liaison suivants existent :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-182">The following binding source attributes exist:</span></span>

|<span data-ttu-id="7b6d5-183">Attribut</span><span class="sxs-lookup"><span data-stu-id="7b6d5-183">Attribute</span></span>|<span data-ttu-id="7b6d5-184">Source de liaison</span><span class="sxs-lookup"><span data-stu-id="7b6d5-184">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="7b6d5-185">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-185">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="7b6d5-186">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="7b6d5-186">Request body</span></span> |
|<span data-ttu-id="7b6d5-187">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-187">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="7b6d5-188">Données de formulaire dans le corps de la demande</span><span class="sxs-lookup"><span data-stu-id="7b6d5-188">Form data in the request body</span></span> |
|<span data-ttu-id="7b6d5-189">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-189">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="7b6d5-190">En-tête de demande</span><span class="sxs-lookup"><span data-stu-id="7b6d5-190">Request header</span></span> |
|<span data-ttu-id="7b6d5-191">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-191">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="7b6d5-192">Paramètre de la chaîne de requête de la demande</span><span class="sxs-lookup"><span data-stu-id="7b6d5-192">Request query string parameter</span></span> |
|<span data-ttu-id="7b6d5-193">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-193">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="7b6d5-194">Données d’itinéraire à partir de la demande actuelle</span><span class="sxs-lookup"><span data-stu-id="7b6d5-194">Route data from the current request</span></span> |
|<span data-ttu-id="7b6d5-195">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="7b6d5-195">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="7b6d5-196">Service de demande injecté comme paramètre d’action</span><span class="sxs-lookup"><span data-stu-id="7b6d5-196">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="7b6d5-197">N’utilisez pas `[FromRoute]` lorsque les valeurs risquent de contenir `%2f` (c’est-à-dire `/`).</span><span class="sxs-lookup"><span data-stu-id="7b6d5-197">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="7b6d5-198">`%2f` ne sera pas sans la séquence d’échappement `/`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-198">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="7b6d5-199">Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-199">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="7b6d5-200">Sans l’attribut `[ApiController]` ou des attributs de source de liaison comme `[FromQuery]`, le runtime ASP.NET Core tente d’utiliser le classeur de modèles objet complexe.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-200">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="7b6d5-201">Le classeur de modèles objet complexe extrait des données à partir de fournisseurs de valeurs dans un ordre défini.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-201">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="7b6d5-202">Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-202">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="7b6d5-203">L’attribut `[ApiController]` applique des règles d’inférence pour les sources de données par défaut des paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-203">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="7b6d5-204">Ces règles vous évitent d’avoir à identifier les sources de liaison manuellement en appliquant des attributs aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-204">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="7b6d5-205">Les règles d’inférence de source de liaison se comportent comme suit :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-205">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="7b6d5-206">`[FromBody]` est déduit des paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-206">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="7b6d5-207">Une exception à cette règle d’inférence `[FromBody]` est tout type complexe intégré ayant une signification spéciale, comme <xref:Microsoft.AspNetCore.Http.IFormCollection> et <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-207">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="7b6d5-208">Le code de l’inférence de la source de liaison ignore ces types spéciaux.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-208">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="7b6d5-209">`[FromForm]` est déduit pour les paramètres d’action de type <xref:Microsoft.AspNetCore.Http.IFormFile> et <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-209">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="7b6d5-210">Il n’est pas déduit pour les types simples ou définis par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-210">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="7b6d5-211">`[FromRoute]` est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-211">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="7b6d5-212">Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-212">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="7b6d5-213">`[FromQuery]` est déduit pour tous les autres paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-213">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="7b6d5-214">Notes sur l’inférence FromBody</span><span class="sxs-lookup"><span data-stu-id="7b6d5-214">FromBody inference notes</span></span>

<span data-ttu-id="7b6d5-215">`[FromBody]` n’est pas déduit pour les types simples tels que `string` ou `int`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-215">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="7b6d5-216">Vous devez donc utiliser l’attribut `[FromBody]` pour les types simples quand cette fonctionnalité est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-216">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="7b6d5-217">Quand une action a plusieurs paramètres liés à partir du corps de la demande, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-217">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="7b6d5-218">Par exemple, toutes les signatures de méthode d’action suivantes génèrent une exception :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-218">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="7b6d5-219">`[FromBody]` est déduit sur les deux paramètres, car ce sont des types complexes.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-219">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="7b6d5-220">Attribut `[FromBody]` sur un paramètre, déduit sur l’autre, car c’est un type complexe.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-220">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="7b6d5-221">Attribut `[FromBody]` sur les deux paramètres.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-221">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> <span data-ttu-id="7b6d5-222">Dans ASP.NET Core 2.1, les paramètres de type collection comme les listes et les tableaux sont déduits de manière incorrecte en tant que `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-222">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="7b6d5-223">L’attribut `[FromBody]` doit être utilisé pour ces paramètres s’ils doivent être liés à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-223">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="7b6d5-224">Ce problème est corrigé dans ASP.NET Core version 2.2 ou ultérieure, où les paramètres de type collection sont déduits pour être liés à partir du corps par défaut.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-224">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

### <a name="disable-inference-rules"></a><span data-ttu-id="7b6d5-225">Désactiver les règles d’inférence</span><span class="sxs-lookup"><span data-stu-id="7b6d5-225">Disable inference rules</span></span>

<span data-ttu-id="7b6d5-226">Pour désactiver l’inférence de la source de liaison, définissez <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> sur `true`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-226">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="7b6d5-227">Ajoutez le code suivant dans `Startup.ConfigureServices` après `services.AddMvc().SetCompatibilityVersion` :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-227">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="7b6d5-228">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="7b6d5-228">Multipart/form-data request inference</span></span>

<span data-ttu-id="7b6d5-229">L’attribut `[ApiController]` applique une règle d’inférence quand un paramètre d’action est annoté avec l’attribut [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) : le type de contenu de demande `multipart/form-data` est déduit.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-229">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute: the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="7b6d5-230">Pour désactiver le comportement par défaut, définissez <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> sur `true` dans `Startup.ConfigureServices`, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-230">To disable the default behavior, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters>  to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="7b6d5-231">Fonctionnalité Détails du problème pour les codes d’état erreur</span><span class="sxs-lookup"><span data-stu-id="7b6d5-231">Problem details for error status codes</span></span>

<span data-ttu-id="7b6d5-232">Quand la version de compatibilité est 2.2 ou ultérieure, MVC transforme un résultat d’erreur (résultat avec un code d’état égal ou supérieur à 400) en résultat avec <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-232">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="7b6d5-233">Le type `ProblemDetails` est basé sur la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807) pour fournir des détails de l’erreur lisibles par machine dans une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-233">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="7b6d5-234">Prenons le code suivant dans une action de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-234">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="7b6d5-235">La réponse HTTP pour `NotFound` a le code d’état 404 avec le corps `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-235">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="7b6d5-236">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-236">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="7b6d5-237">Personnaliser la réponse ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="7b6d5-237">Customize ProblemDetails response</span></span>

<span data-ttu-id="7b6d5-238">Utilisez la propriété `ClientErrorMapping` pour configurer le contenu de la réponse `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-238">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="7b6d5-239">Par exemple, le code suivant met à jour la propriété `type` pour les réponses 404 :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-239">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a><span data-ttu-id="7b6d5-240">Désactiver la réponse ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="7b6d5-240">Disable ProblemDetails response</span></span>

<span data-ttu-id="7b6d5-241">La création automatique de `ProblemDetails` est désactivée quand la propriété `SuppressMapClientErrors` est définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="7b6d5-241">The automatic creation of `ProblemDetails` is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="7b6d5-242">Ajoutez le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="7b6d5-242">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a><span data-ttu-id="7b6d5-243">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7b6d5-243">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
