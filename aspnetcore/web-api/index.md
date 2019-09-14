---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les principes fondamentaux de la création d’une API web dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2019
uid: web-api/index
ms.openlocfilehash: 6e1868690a2c384307a23c89467505d3ed8916db
ms.sourcegitcommit: 805f625d16d74e77f02f5f37326e5aceafcb78e3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985460"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="35d7a-103">Créer des API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35d7a-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="35d7a-104">De [Scott Addie](https://github.com/scottaddie) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="35d7a-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="35d7a-105">ASP.NET Core prend en charge la création de services RESTful, également appelés API web, à l’aide de C#.</span><span class="sxs-lookup"><span data-stu-id="35d7a-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="35d7a-106">Pour traiter les demandes, une API web utilise des contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="35d7a-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="35d7a-107">Les *contrôleurs* dans une API web sont des classes qui dérivent de `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="35d7a-108">Cet article explique comment utiliser des contrôleurs pour gérer les demandes d’API Web.</span><span class="sxs-lookup"><span data-stu-id="35d7a-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="35d7a-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="35d7a-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="35d7a-110">([Guide pratique de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="35d7a-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="35d7a-111">Classe ControllerBase</span><span class="sxs-lookup"><span data-stu-id="35d7a-111">ControllerBase class</span></span>

<span data-ttu-id="35d7a-112">Une API Web se compose d’une ou de plusieurs classes de contrôleur <xref:Microsoft.AspNetCore.Mvc.ControllerBase>qui dérivent de.</span><span class="sxs-lookup"><span data-stu-id="35d7a-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="35d7a-113">Le modèle de projet d’API Web fournit un contrôleur de démarrage :</span><span class="sxs-lookup"><span data-stu-id="35d7a-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="35d7a-114">Ne créez pas de contrôleur d’API web en effectuant une dérivation de la classe <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="35d7a-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="35d7a-115">`Controller` dérive de `ControllerBase` et ajoute la prise en charge pour les vues ; ainsi, il est destiné à la gestion des pages web, pas des demandes d’API web.</span><span class="sxs-lookup"><span data-stu-id="35d7a-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="35d7a-116">Il existe une exception à cette règle : Si vous envisagez d’utiliser le même contrôleur pour les vues et les API Web, `Controller`dérivez-le de.</span><span class="sxs-lookup"><span data-stu-id="35d7a-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="35d7a-117">La classe `ControllerBase` fournit de nombreuses propriétés et méthodes qui sont utiles pour gérer les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="35d7a-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="35d7a-118">Par exemple, `ControllerBase.CreatedAtAction` retourne un code d’état 201 :</span><span class="sxs-lookup"><span data-stu-id="35d7a-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="35d7a-119">Voici d’autres exemples de méthodes fournies par `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="35d7a-120">Méthode</span><span class="sxs-lookup"><span data-stu-id="35d7a-120">Method</span></span>   |<span data-ttu-id="35d7a-121">Notes</span><span class="sxs-lookup"><span data-stu-id="35d7a-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="35d7a-122">Retourne le code d’état 400.</span><span class="sxs-lookup"><span data-stu-id="35d7a-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>|<span data-ttu-id="35d7a-123">Retourne le code d’état 404.</span><span class="sxs-lookup"><span data-stu-id="35d7a-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="35d7a-124">Retourne un fichier.</span><span class="sxs-lookup"><span data-stu-id="35d7a-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="35d7a-125">Appelle la [liaison de modèle](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="35d7a-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="35d7a-126">Appelle la [validation de modèle](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="35d7a-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="35d7a-127">Pour obtenir la liste complète des méthodes et propriétés disponibles, consultez <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="35d7a-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="35d7a-128">Attributs</span><span class="sxs-lookup"><span data-stu-id="35d7a-128">Attributes</span></span>

<span data-ttu-id="35d7a-129">L’espace de noms <xref:Microsoft.AspNetCore.Mvc> fournit des attributs qui peuvent être utilisés pour configurer le comportement des contrôleurs d’API web et des méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="35d7a-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="35d7a-130">L’exemple suivant utilise des attributs pour spécifier le verbe d’action HTTP pris en charge et tous les codes d’état HTTP connus qui peuvent être retournés :</span><span class="sxs-lookup"><span data-stu-id="35d7a-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="35d7a-131">Voici d’autres exemples d’attributs disponibles.</span><span class="sxs-lookup"><span data-stu-id="35d7a-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="35d7a-132">Attribut</span><span class="sxs-lookup"><span data-stu-id="35d7a-132">Attribute</span></span>|<span data-ttu-id="35d7a-133">Notes</span><span class="sxs-lookup"><span data-stu-id="35d7a-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="35d7a-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="35d7a-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="35d7a-135">Spécifie le modèle d’URL pour un contrôleur ou une action.</span><span class="sxs-lookup"><span data-stu-id="35d7a-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="35d7a-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="35d7a-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="35d7a-137">Spécifie le préfixe et les propriétés à inclure pour la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="35d7a-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="35d7a-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="35d7a-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="35d7a-139">Identifie une action qui prend en charge le verbe d’action HTTP.</span><span class="sxs-lookup"><span data-stu-id="35d7a-139">Identifies an action that supports the HTTP GET action verb.</span></span>|
|<span data-ttu-id="35d7a-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="35d7a-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="35d7a-141">Spécifie les types de données acceptés par une action.</span><span class="sxs-lookup"><span data-stu-id="35d7a-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="35d7a-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="35d7a-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="35d7a-143">Spécifie les types de données retournés par une action.</span><span class="sxs-lookup"><span data-stu-id="35d7a-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="35d7a-144">Pour obtenir la liste des attributs disponibles, consultez l’espace de noms <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="35d7a-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="35d7a-145">Attribut ApiController</span><span class="sxs-lookup"><span data-stu-id="35d7a-145">ApiController attribute</span></span>

<span data-ttu-id="35d7a-146">L’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) peut être appliqué à une classe de contrôleur pour activer les consignes strictes suivants, comportements spécifiques aux API :</span><span class="sxs-lookup"><span data-stu-id="35d7a-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="35d7a-147">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="35d7a-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="35d7a-148">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="35d7a-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="35d7a-149">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="35d7a-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="35d7a-150">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="35d7a-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="35d7a-151">Fonctionnalité Détails du problème pour les codes d’état erreur</span><span class="sxs-lookup"><span data-stu-id="35d7a-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="35d7a-152">Ces fonctionnalités nécessitent une [version de compatibilité](xref:mvc/compatibility-version) 2.1 ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="35d7a-152">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="35d7a-153">Attribut sur des contrôleurs spécifiques</span><span class="sxs-lookup"><span data-stu-id="35d7a-153">Attribute on specific controllers</span></span>

<span data-ttu-id="35d7a-154">L’attribut `[ApiController]` peut être appliqué à des contrôleurs spécifiques, comme dans l’exemple suivant à partir du modèle de projet :</span><span class="sxs-lookup"><span data-stu-id="35d7a-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="35d7a-155">Attribut sur plusieurs contrôleurs</span><span class="sxs-lookup"><span data-stu-id="35d7a-155">Attribute on multiple controllers</span></span>

<span data-ttu-id="35d7a-156">Une façon d’utiliser l’attribut sur plusieurs contrôleurs consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="35d7a-157">L’exemple suivant montre une classe de base personnalisée et un contrôleur qui dérive de celle-ci :</span><span class="sxs-lookup"><span data-stu-id="35d7a-157">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="35d7a-158">Attribut sur un assembly</span><span class="sxs-lookup"><span data-stu-id="35d7a-158">Attribute on an assembly</span></span>

<span data-ttu-id="35d7a-159">Si la [version de compatibilité](xref:mvc/compatibility-version) est définie sur 2.2 ou une version ultérieure, l’attribut `[ApiController]` peut être appliqué à un assembly.</span><span class="sxs-lookup"><span data-stu-id="35d7a-159">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="35d7a-160">De cette manière, l’annotation applique le comportement de l’API web à tous les contrôleurs de l’assembly.</span><span class="sxs-lookup"><span data-stu-id="35d7a-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="35d7a-161">Les contrôleurs individuels n’ont aucun moyen de refuser.</span><span class="sxs-lookup"><span data-stu-id="35d7a-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="35d7a-162">Appliquez l’attribut assembly au niveau de l’assembly à `Startup` la déclaration d’espace de noms qui entoure la classe :</span><span class="sxs-lookup"><span data-stu-id="35d7a-162">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

::: moniker-end

## <a name="attribute-routing-requirement"></a><span data-ttu-id="35d7a-163">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="35d7a-163">Attribute routing requirement</span></span>

<span data-ttu-id="35d7a-164">L’attribut `[ApiController]` rend nécessaire le routage d’attributs.</span><span class="sxs-lookup"><span data-stu-id="35d7a-164">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="35d7a-165">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="35d7a-165">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="35d7a-166">Les actions sont inaccessibles par le biais `UseEndpoints`des <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> [itinéraires conventionnels](xref:mvc/controllers/routing#conventional-routing) définis par, ou <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="35d7a-167">Les actions sont inaccessibles par le biais de [routes conventionnelles](xref:mvc/controllers/routing#conventional-routing) définies par <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-167">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="35d7a-168">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="35d7a-168">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="35d7a-169">Grâce à l’attribut `[ApiController]`, les erreurs de validation de modèles déclenchent automatiquement une réponse HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="35d7a-169">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="35d7a-170">Ainsi, le code suivant est inutile dans une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="35d7a-170">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="35d7a-171">ASP.net Core MVC utilise le <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> filtre d’action pour effectuer la vérification précédente.</span><span class="sxs-lookup"><span data-stu-id="35d7a-171">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="35d7a-172">Réponse BadRequest par défaut</span><span class="sxs-lookup"><span data-stu-id="35d7a-172">Default BadRequest response</span></span> 

<span data-ttu-id="35d7a-173">Avec une version de compatibilité de 2,1, le type de réponse par défaut pour une réponse <xref:Microsoft.AspNetCore.Mvc.SerializableError>http 400 est.</span><span class="sxs-lookup"><span data-stu-id="35d7a-173">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="35d7a-174">Le corps de la requête suivant est un exemple de type sérialisé :</span><span class="sxs-lookup"><span data-stu-id="35d7a-174">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="35d7a-175">Avec une version de compatibilité 2,2 ou ultérieure, le type de réponse par défaut pour une réponse HTTP <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>400 est.</span><span class="sxs-lookup"><span data-stu-id="35d7a-175">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="35d7a-176">Le corps de la requête suivant est un exemple de type sérialisé :</span><span class="sxs-lookup"><span data-stu-id="35d7a-176">The following request body is an example of the serialized type:</span></span>

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

<span data-ttu-id="35d7a-177">`ValidationProblemDetails` Type :</span><span class="sxs-lookup"><span data-stu-id="35d7a-177">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="35d7a-178">Fournit un format lisible par l’ordinateur pour spécifier les erreurs dans les réponses de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="35d7a-178">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="35d7a-179">Est conforme à la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="35d7a-179">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="35d7a-180">Pour modifier le type de `SerializableError`réponse par défaut, appliquez les modifications en surbrillance dans : `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="35d7a-180">To change the default response type to `SerializableError`, apply the highlighted changes in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

### <a name="customize-badrequest-response"></a><span data-ttu-id="35d7a-181">Personnaliser la réponse BadRequest</span><span class="sxs-lookup"><span data-stu-id="35d7a-181">Customize BadRequest response</span></span>

<span data-ttu-id="35d7a-182">Pour personnaliser la réponse qui résulte d’une erreur de validation, utilisez <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span><span class="sxs-lookup"><span data-stu-id="35d7a-182">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="35d7a-183">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="35d7a-183">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=4-20)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=5-21)]

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="35d7a-184">Consigner automatiquement 400 réponses</span><span class="sxs-lookup"><span data-stu-id="35d7a-184">Log automatic 400 responses</span></span>

<span data-ttu-id="35d7a-185">Voir [Guide pratique pour consigner automatiquement 400 réponses en cas d’erreurs de validation de modèle (aspnet/AspNetCore.Docs no 12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="35d7a-185">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="35d7a-186">Désactiver la réponse automatique 400</span><span class="sxs-lookup"><span data-stu-id="35d7a-186">Disable automatic 400 response</span></span>

<span data-ttu-id="35d7a-187">Pour désactiver le comportement 400 automatique, définissez la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> sur `true`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-187">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="35d7a-188">Ajoutez le code en surbrillance suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="35d7a-188">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="35d7a-189">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="35d7a-189">Binding source parameter inference</span></span>

<span data-ttu-id="35d7a-190">Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="35d7a-190">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="35d7a-191">Les attributs de source de liaison suivants existent :</span><span class="sxs-lookup"><span data-stu-id="35d7a-191">The following binding source attributes exist:</span></span>

|<span data-ttu-id="35d7a-192">Attribut</span><span class="sxs-lookup"><span data-stu-id="35d7a-192">Attribute</span></span>|<span data-ttu-id="35d7a-193">Source de liaison</span><span class="sxs-lookup"><span data-stu-id="35d7a-193">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="35d7a-194">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="35d7a-194">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="35d7a-195">Corps de la demande</span><span class="sxs-lookup"><span data-stu-id="35d7a-195">Request body</span></span> |
|<span data-ttu-id="35d7a-196">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="35d7a-196">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="35d7a-197">Données de formulaire dans le corps de la demande</span><span class="sxs-lookup"><span data-stu-id="35d7a-197">Form data in the request body</span></span> |
|<span data-ttu-id="35d7a-198">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="35d7a-198">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="35d7a-199">En-tête de requête</span><span class="sxs-lookup"><span data-stu-id="35d7a-199">Request header</span></span> |
|<span data-ttu-id="35d7a-200">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="35d7a-200">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="35d7a-201">Paramètre de la chaîne de requête de la demande</span><span class="sxs-lookup"><span data-stu-id="35d7a-201">Request query string parameter</span></span> |
|<span data-ttu-id="35d7a-202">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="35d7a-202">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="35d7a-203">Données d’itinéraire à partir de la demande actuelle</span><span class="sxs-lookup"><span data-stu-id="35d7a-203">Route data from the current request</span></span> |
|<span data-ttu-id="35d7a-204">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="35d7a-204">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="35d7a-205">Service de demande injecté comme paramètre d’action</span><span class="sxs-lookup"><span data-stu-id="35d7a-205">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="35d7a-206">N’utilisez pas `[FromRoute]` lorsque les valeurs risquent de contenir `%2f` (c’est-à-dire `/`).</span><span class="sxs-lookup"><span data-stu-id="35d7a-206">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="35d7a-207">`%2f` ne sera pas sans la séquence d’échappement `/`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-207">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="35d7a-208">Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-208">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="35d7a-209">Sans l’attribut `[ApiController]` ou des attributs de source de liaison comme `[FromQuery]`, le runtime ASP.NET Core tente d’utiliser le classeur de modèles objet complexe.</span><span class="sxs-lookup"><span data-stu-id="35d7a-209">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="35d7a-210">Le classeur de modèles objet complexe extrait des données à partir de fournisseurs de valeurs dans un ordre défini.</span><span class="sxs-lookup"><span data-stu-id="35d7a-210">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="35d7a-211">Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :</span><span class="sxs-lookup"><span data-stu-id="35d7a-211">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="35d7a-212">L’attribut `[ApiController]` applique des règles d’inférence pour les sources de données par défaut des paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="35d7a-212">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="35d7a-213">Ces règles vous évitent d’avoir à identifier les sources de liaison manuellement en appliquant des attributs aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="35d7a-213">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="35d7a-214">Les règles d’inférence de source de liaison se comportent comme suit :</span><span class="sxs-lookup"><span data-stu-id="35d7a-214">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="35d7a-215">`[FromBody]` est déduit des paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="35d7a-215">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="35d7a-216">Une exception à cette règle d’inférence `[FromBody]` est tout type complexe intégré ayant une signification spéciale, comme <xref:Microsoft.AspNetCore.Http.IFormCollection> et <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="35d7a-216">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="35d7a-217">Le code de l’inférence de la source de liaison ignore ces types spéciaux.</span><span class="sxs-lookup"><span data-stu-id="35d7a-217">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="35d7a-218">`[FromForm]` est déduit pour les paramètres d’action de type <xref:Microsoft.AspNetCore.Http.IFormFile> et <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="35d7a-218">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="35d7a-219">Il n’est pas déduit pour les types simples ou définis par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="35d7a-219">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="35d7a-220">`[FromRoute]` est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="35d7a-220">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="35d7a-221">Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-221">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="35d7a-222">`[FromQuery]` est déduit pour tous les autres paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="35d7a-222">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="35d7a-223">Notes sur l’inférence FromBody</span><span class="sxs-lookup"><span data-stu-id="35d7a-223">FromBody inference notes</span></span>

<span data-ttu-id="35d7a-224">`[FromBody]` n’est pas déduit pour les types simples tels que `string` ou `int`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-224">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="35d7a-225">Vous devez donc utiliser l’attribut `[FromBody]` pour les types simples quand cette fonctionnalité est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="35d7a-225">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="35d7a-226">Quand une action a plusieurs paramètres liés à partir du corps de la demande, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="35d7a-226">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="35d7a-227">Par exemple, toutes les signatures de méthode d’action suivantes génèrent une exception :</span><span class="sxs-lookup"><span data-stu-id="35d7a-227">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="35d7a-228">`[FromBody]` est déduit sur les deux paramètres, car ce sont des types complexes.</span><span class="sxs-lookup"><span data-stu-id="35d7a-228">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="35d7a-229">Attribut `[FromBody]` sur un paramètre, déduit sur l’autre, car c’est un type complexe.</span><span class="sxs-lookup"><span data-stu-id="35d7a-229">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="35d7a-230">Attribut `[FromBody]` sur les deux paramètres.</span><span class="sxs-lookup"><span data-stu-id="35d7a-230">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="35d7a-231">Dans ASP.NET Core 2.1, les paramètres de type collection comme les listes et les tableaux sont déduits de manière incorrecte en tant que `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-231">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="35d7a-232">L’attribut `[FromBody]` doit être utilisé pour ces paramètres s’ils doivent être liés à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="35d7a-232">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="35d7a-233">Ce problème est corrigé dans ASP.NET Core version 2.2 ou ultérieure, où les paramètres de type collection sont déduits pour être liés à partir du corps par défaut.</span><span class="sxs-lookup"><span data-stu-id="35d7a-233">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="35d7a-234">Désactiver les règles d’inférence</span><span class="sxs-lookup"><span data-stu-id="35d7a-234">Disable inference rules</span></span>

<span data-ttu-id="35d7a-235">Pour désactiver l’inférence de la source de liaison, définissez <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> sur `true`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-235">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="35d7a-236">Ajoutez le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="35d7a-236">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="35d7a-237">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="35d7a-237">Multipart/form-data request inference</span></span>

<span data-ttu-id="35d7a-238">L' `[ApiController]` attribut applique une règle d’inférence lorsqu’un paramètre d’action est annoté avec l’attribut [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) .</span><span class="sxs-lookup"><span data-stu-id="35d7a-238">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="35d7a-239">Le `multipart/form-data` type de contenu de la demande est déduit.</span><span class="sxs-lookup"><span data-stu-id="35d7a-239">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="35d7a-240">Pour désactiver le comportement par défaut, affectez à `true` la `Startup.ConfigureServices`propriété la <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> valeur dans :</span><span class="sxs-lookup"><span data-stu-id="35d7a-240">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="35d7a-241">Fonctionnalité Détails du problème pour les codes d’état erreur</span><span class="sxs-lookup"><span data-stu-id="35d7a-241">Problem details for error status codes</span></span>

<span data-ttu-id="35d7a-242">Quand la version de compatibilité est 2.2 ou ultérieure, MVC transforme un résultat d’erreur (résultat avec un code d’état égal ou supérieur à 400) en résultat avec <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="35d7a-242">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="35d7a-243">Le type `ProblemDetails` est basé sur la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807) pour fournir des détails de l’erreur lisibles par machine dans une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="35d7a-243">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="35d7a-244">Prenons le code suivant dans une action de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="35d7a-244">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="35d7a-245">La `NotFound` méthode génère un code d’état HTTP 404 avec `ProblemDetails` un corps.</span><span class="sxs-lookup"><span data-stu-id="35d7a-245">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="35d7a-246">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="35d7a-246">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="35d7a-247">Personnaliser la réponse ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="35d7a-247">Customize ProblemDetails response</span></span>

<span data-ttu-id="35d7a-248">Utilisez la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> pour configurer le contenu de la réponse `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="35d7a-248">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="35d7a-249">Par exemple, le code suivant met à jour la propriété `type` pour les réponses 404 :</span><span class="sxs-lookup"><span data-stu-id="35d7a-249">For example, the following code updates the `type` property for 404 responses:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end

### <a name="disable-problemdetails-response"></a><span data-ttu-id="35d7a-250">Désactiver la réponse ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="35d7a-250">Disable ProblemDetails response</span></span>

<span data-ttu-id="35d7a-251">La création automatique d’une `ProblemDetails` instance est désactivée <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> lorsque la propriété a `true`la valeur.</span><span class="sxs-lookup"><span data-stu-id="35d7a-251">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> property is set to `true`.</span></span> <span data-ttu-id="35d7a-252">Ajoutez le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="35d7a-252">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="35d7a-253">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="35d7a-253">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
