---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les fonctionnalités disponibles pour la création d’une API web dans ASP.NET Core et quand il convient d’utiliser chaque fonctionnalité.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: e4615e5d416ba2433d55309b25ee3643c6c636ac
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207002"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="272be-103">Créer des API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="272be-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="272be-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="272be-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="272be-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="272be-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="272be-106">Ce document explique comment créer une API web ASP.NET Core et quand il convient le plus d’utiliser chaque fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="272be-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="272be-107">Dériver la classe de ControllerBase</span><span class="sxs-lookup"><span data-stu-id="272be-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="272be-108">Héritez de la classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> dans un contrôleur destiné à servir d’API web.</span><span class="sxs-lookup"><span data-stu-id="272be-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="272be-109">Exemple :</span><span class="sxs-lookup"><span data-stu-id="272be-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="272be-110">La classe `ControllerBase` fournit l’accès à de plusieurs propriétés et méthodes.</span><span class="sxs-lookup"><span data-stu-id="272be-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="272be-111">Dans le code précédent, les exemples incluent <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="272be-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="272be-112">Ces méthodes sont appelées dans les méthodes d’action pour retourner les codes d’état HTTP 400 et 201, respectivement.</span><span class="sxs-lookup"><span data-stu-id="272be-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="272be-113">La propriété <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, également fournie par `ControllerBase`, permet de gérer la validation des modèles de requêtes.</span><span class="sxs-lookup"><span data-stu-id="272be-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="272be-114">Annoter la classe avec ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="272be-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="272be-115">ASP.NET Core 2.1 introduit l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) pour désigner une classe de contrôleur d’API web.</span><span class="sxs-lookup"><span data-stu-id="272be-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="272be-116">Exemple :</span><span class="sxs-lookup"><span data-stu-id="272be-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="272be-117">Une version de compatibilité 2.1 ou ultérieure, définie via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, est obligatoire pour utiliser cet attribut.</span><span class="sxs-lookup"><span data-stu-id="272be-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="272be-118">Par exemple, le code en surbrillance dans *Startup.ConfigureServices* définit l’indicateur de compatibilité 2.2 :</span><span class="sxs-lookup"><span data-stu-id="272be-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="272be-119">Pour plus d'informations, consultez <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="272be-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="272be-120">L’attribut `[ApiController]` est généralement associé à `ControllerBase` pour donner aux contrôleurs un comportement spécifique à REST.</span><span class="sxs-lookup"><span data-stu-id="272be-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="272be-121">`ControllerBase` permet d’accéder aux méthodes telles que <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="272be-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="272be-122">Une autre approche consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]` :</span><span class="sxs-lookup"><span data-stu-id="272be-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="272be-123">Les sections suivantes décrivent les fonctionnalités pratiques ajoutées par l’attribut.</span><span class="sxs-lookup"><span data-stu-id="272be-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="272be-124">Réponses de la fonctionnalité Détails du problème pour les codes d’état erreur</span><span class="sxs-lookup"><span data-stu-id="272be-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="272be-125">ASP.NET Core 2.1 et ultérieur inclut [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), type basé sur la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="272be-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="272be-126">Le type `ProblemDetails` fournit un format standardisé pour transmettre les détails lisibles par la machine des erreurs dans une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="272be-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="272be-127">Dans ASP.NET Core 2.2 et ultérieur, MVC transforme les résultats de code d’état d’erreur (code d’état 400 et supérieur) en un résultat avec `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="272be-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="272be-128">Examinons le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="272be-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="272be-129">La réponse HTTP pour le résultat `NotFound` a un code d’état 404 avec un corps `ProblemDetails` semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="272be-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="272be-130">La fonctionnalité Détails du problème nécessite un indicateur de compatibilité de 2.2 ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="272be-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="272be-131">Le comportement par défaut est désactivé lorsque la propriété [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> est définie avec la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="272be-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="272be-132">Le code mis en surbrillance suivant de `Startup.ConfigureServices` désactive les détails du problème :</span><span class="sxs-lookup"><span data-stu-id="272be-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="272be-133">Utilisez la propriété [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> pour configurer le contenu de la réponse `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="272be-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="272be-134">Par exemple, le code suivant met à jour la propriété `type` pour les réponses 404 :</span><span class="sxs-lookup"><span data-stu-id="272be-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="272be-135">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="272be-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="272be-136">Les erreurs de validation déclenchent automatiquement une réponse HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="272be-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="272be-137">Le code suivant devient inutile dans vos actions :</span><span class="sxs-lookup"><span data-stu-id="272be-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="272be-138">Utilisez <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> pour personnaliser la sortie de la réponse résultante.</span><span class="sxs-lookup"><span data-stu-id="272be-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="272be-139">Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="272be-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="272be-140">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="272be-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="272be-141">Avec un indicateur de compatibilité 2.2 ou ultérieur, le type de réponse par défaut retourné pour les réponses 400 est <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="272be-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="272be-142">Utilisez la propriété [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> pour utiliser le format d’erreur ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="272be-142">Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="272be-143">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="272be-143">Binding source parameter inference</span></span>

<span data-ttu-id="272be-144">Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="272be-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="272be-145">Les attributs de source de liaison suivants existent :</span><span class="sxs-lookup"><span data-stu-id="272be-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="272be-146">Attribut</span><span class="sxs-lookup"><span data-stu-id="272be-146">Attribute</span></span>|<span data-ttu-id="272be-147">Source de liaison</span><span class="sxs-lookup"><span data-stu-id="272be-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="272be-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="272be-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="272be-149">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="272be-149">Request body</span></span> |
|<span data-ttu-id="272be-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="272be-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="272be-151">Données de formulaire dans le corps de la demande</span><span class="sxs-lookup"><span data-stu-id="272be-151">Form data in the request body</span></span> |
|<span data-ttu-id="272be-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="272be-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="272be-153">En-tête de demande</span><span class="sxs-lookup"><span data-stu-id="272be-153">Request header</span></span> |
|<span data-ttu-id="272be-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="272be-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="272be-155">Paramètre de la chaîne de requête de la demande</span><span class="sxs-lookup"><span data-stu-id="272be-155">Request query string parameter</span></span> |
|<span data-ttu-id="272be-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="272be-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="272be-157">Données d’itinéraire à partir de la demande actuelle</span><span class="sxs-lookup"><span data-stu-id="272be-157">Route data from the current request</span></span> |
|<span data-ttu-id="272be-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="272be-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="272be-159">Service de demande injecté comme paramètre d’action</span><span class="sxs-lookup"><span data-stu-id="272be-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="272be-160">N’utilisez pas `[FromRoute]` lorsque les valeurs risquent de contenir `%2f` (c’est-à-dire `/`).</span><span class="sxs-lookup"><span data-stu-id="272be-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="272be-161">`%2f` ne sera pas sans la séquence d’échappement `/`.</span><span class="sxs-lookup"><span data-stu-id="272be-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="272be-162">Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.</span><span class="sxs-lookup"><span data-stu-id="272be-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="272be-163">Sans l’attribut `[ApiController]`, les attributs de source de liaison sont définis explicitement.</span><span class="sxs-lookup"><span data-stu-id="272be-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="272be-164">Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :</span><span class="sxs-lookup"><span data-stu-id="272be-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="272be-165">Des règles d’inférence sont appliquées pour les sources de données par défaut des paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="272be-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="272be-166">Ces règles configurent les sources de liaison que vous seriez susceptible d’appliquer manuellement aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="272be-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="272be-167">Les attributs de source de liaison se comportent comme suit :</span><span class="sxs-lookup"><span data-stu-id="272be-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="272be-168">**[FromBody]** est déduit des paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="272be-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="272be-169">Une exception à cette règle est tout type complexe intégré ayant une signification spéciale, comme <xref:Microsoft.AspNetCore.Http.IFormCollection> et <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="272be-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="272be-170">Le code de l’inférence de la source de liaison ignore ces types spéciaux.</span><span class="sxs-lookup"><span data-stu-id="272be-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="272be-171">`[FromBody]` n’est pas déduit pour les types simples tels que `string` ou `int`.</span><span class="sxs-lookup"><span data-stu-id="272be-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="272be-172">Par conséquent, vous devez utiliser l’attribut `[FromBody]` pour les types simples quand cette fonctionnalité est souhaitée.</span><span class="sxs-lookup"><span data-stu-id="272be-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="272be-173">Quand une action a plusieurs paramètres explicitement spécifiés (par le biais de `[FromBody]`) ou déduits comme étant liés à partir du corps de la demande, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="272be-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="272be-174">Par exemple, les signatures d’action suivantes génèrent une exception :</span><span class="sxs-lookup"><span data-stu-id="272be-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="272be-175">**[FromForm]** est déduit pour les paramètres d’action de type <xref:Microsoft.AspNetCore.Http.IFormFile> et <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="272be-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="272be-176">Il n’est pas déduit pour les types simples ou définis par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="272be-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="272be-177">**[FromRoute]** est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="272be-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="272be-178">Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="272be-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="272be-179">**[FromQuery]** est déduit pour tous les autres paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="272be-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="272be-180">Les règles d’inférence par défaut sont désactivées quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="272be-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="272be-181">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="272be-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="272be-182">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="272be-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="272be-183">Quand un paramètre d’action est annoté avec l’attribut [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), le type de contenu de demande `multipart/form-data` est déduit.</span><span class="sxs-lookup"><span data-stu-id="272be-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="272be-184">Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="272be-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="272be-185">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="272be-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="272be-186">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="272be-186">Attribute routing requirement</span></span>

<span data-ttu-id="272be-187">Le routage d’attribut devient une exigence.</span><span class="sxs-lookup"><span data-stu-id="272be-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="272be-188">Exemple :</span><span class="sxs-lookup"><span data-stu-id="272be-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="272be-189">Les actions sont inaccessibles par le biais des [itinéraires conventionnels](xref:mvc/controllers/routing#conventional-routing) définis dans <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou par <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> dans *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="272be-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="272be-190">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="272be-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
