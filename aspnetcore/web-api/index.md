---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les fonctionnalités disponibles pour la création d’une API web dans ASP.NET Core et quand il convient d’utiliser chaque fonctionnalité.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822138"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="bda5c-103">Créer des API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bda5c-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="bda5c-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="bda5c-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="bda5c-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bda5c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bda5c-106">Ce document explique comment créer une API web ASP.NET Core et quand il convient le plus d’utiliser chaque fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="bda5c-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="bda5c-107">Dériver la classe de ControllerBase</span><span class="sxs-lookup"><span data-stu-id="bda5c-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="bda5c-108">Héritez de la classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> dans un contrôleur destiné à servir d’API web.</span><span class="sxs-lookup"><span data-stu-id="bda5c-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="bda5c-109">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bda5c-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="bda5c-110">La classe `ControllerBase` fournit l’accès à de plusieurs propriétés et méthodes.</span><span class="sxs-lookup"><span data-stu-id="bda5c-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="bda5c-111">Dans le code précédent, les exemples incluent <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="bda5c-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="bda5c-112">Ces méthodes sont appelées dans les méthodes d’action pour retourner les codes d’état HTTP 400 et 201, respectivement.</span><span class="sxs-lookup"><span data-stu-id="bda5c-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="bda5c-113">La propriété <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, également fournie par `ControllerBase`, permet de gérer la validation des modèles de requêtes.</span><span class="sxs-lookup"><span data-stu-id="bda5c-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="bda5c-114">Annoter la classe avec ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="bda5c-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="bda5c-115">ASP.NET Core 2.1 introduit l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) pour désigner une classe de contrôleur d’API web.</span><span class="sxs-lookup"><span data-stu-id="bda5c-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="bda5c-116">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bda5c-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="bda5c-117">Une version de compatibilité 2.1 ou ultérieure, définie via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, est obligatoire pour utiliser cet attribut.</span><span class="sxs-lookup"><span data-stu-id="bda5c-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="bda5c-118">Par exemple, le code en surbrillance dans *Startup.ConfigureServices* définit l’indicateur de compatibilité 2.1 :</span><span class="sxs-lookup"><span data-stu-id="bda5c-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="bda5c-119">Pour plus d'informations, consultez <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="bda5c-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="bda5c-120">L’attribut `[ApiController]` est généralement associé à `ControllerBase` pour donner aux contrôleurs un comportement spécifique à REST.</span><span class="sxs-lookup"><span data-stu-id="bda5c-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="bda5c-121">`ControllerBase` permet d’accéder aux méthodes telles que <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="bda5c-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="bda5c-122">Une autre approche consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]` :</span><span class="sxs-lookup"><span data-stu-id="bda5c-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="bda5c-123">Les sections suivantes décrivent les fonctionnalités pratiques ajoutées par l’attribut.</span><span class="sxs-lookup"><span data-stu-id="bda5c-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="bda5c-124">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="bda5c-124">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="bda5c-125">Les erreurs de validation déclenchent automatiquement une réponse HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="bda5c-125">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="bda5c-126">Le code suivant devient inutile dans vos actions :</span><span class="sxs-lookup"><span data-stu-id="bda5c-126">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="bda5c-127">Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="bda5c-127">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="bda5c-128">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="bda5c-128">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="bda5c-129">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="bda5c-129">Binding source parameter inference</span></span>

<span data-ttu-id="bda5c-130">Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="bda5c-130">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="bda5c-131">Les attributs de source de liaison suivants existent :</span><span class="sxs-lookup"><span data-stu-id="bda5c-131">The following binding source attributes exist:</span></span>

|<span data-ttu-id="bda5c-132">Attribut</span><span class="sxs-lookup"><span data-stu-id="bda5c-132">Attribute</span></span>|<span data-ttu-id="bda5c-133">Source de liaison</span><span class="sxs-lookup"><span data-stu-id="bda5c-133">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="bda5c-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="bda5c-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="bda5c-135">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="bda5c-135">Request body</span></span> |
|<span data-ttu-id="bda5c-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="bda5c-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="bda5c-137">Données de formulaire dans le corps de la demande</span><span class="sxs-lookup"><span data-stu-id="bda5c-137">Form data in the request body</span></span> |
|<span data-ttu-id="bda5c-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="bda5c-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="bda5c-139">En-tête de demande</span><span class="sxs-lookup"><span data-stu-id="bda5c-139">Request header</span></span> |
|<span data-ttu-id="bda5c-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="bda5c-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="bda5c-141">Paramètre de la chaîne de requête de la demande</span><span class="sxs-lookup"><span data-stu-id="bda5c-141">Request query string parameter</span></span> |
|<span data-ttu-id="bda5c-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="bda5c-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="bda5c-143">Données d’itinéraire à partir de la demande actuelle</span><span class="sxs-lookup"><span data-stu-id="bda5c-143">Route data from the current request</span></span> |
|<span data-ttu-id="bda5c-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="bda5c-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="bda5c-145">Service de demande injecté comme paramètre d’action</span><span class="sxs-lookup"><span data-stu-id="bda5c-145">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="bda5c-146">N’utilisez pas `[FromRoute]` lorsque les valeurs risquent de contenir `%2f` (c’est-à-dire `/`).</span><span class="sxs-lookup"><span data-stu-id="bda5c-146">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="bda5c-147">`%2f` ne sera pas sans la séquence d’échappement `/`.</span><span class="sxs-lookup"><span data-stu-id="bda5c-147">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="bda5c-148">Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.</span><span class="sxs-lookup"><span data-stu-id="bda5c-148">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="bda5c-149">Sans l’attribut `[ApiController]`, les attributs de source de liaison sont définis explicitement.</span><span class="sxs-lookup"><span data-stu-id="bda5c-149">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="bda5c-150">Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :</span><span class="sxs-lookup"><span data-stu-id="bda5c-150">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="bda5c-151">Des règles d’inférence sont appliquées pour les sources de données par défaut des paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="bda5c-151">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="bda5c-152">Ces règles configurent les sources de liaison que vous seriez susceptible d’appliquer manuellement aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="bda5c-152">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="bda5c-153">Les attributs de source de liaison se comportent comme suit :</span><span class="sxs-lookup"><span data-stu-id="bda5c-153">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="bda5c-154">**[FromBody]** est déduit des paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="bda5c-154">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="bda5c-155">Une exception à cette règle est tout type complexe intégré ayant une signification spéciale, comme <xref:Microsoft.AspNetCore.Http.IFormCollection> et <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="bda5c-155">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="bda5c-156">Le code de l’inférence de la source de liaison ignore ces types spéciaux.</span><span class="sxs-lookup"><span data-stu-id="bda5c-156">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="bda5c-157">`[FromBody]` n’est pas déduit pour les types simples tels que `string` ou `int`.</span><span class="sxs-lookup"><span data-stu-id="bda5c-157">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="bda5c-158">Par conséquent, vous devez utiliser l’attribut `[FromBody]` pour les types simples quand cette fonctionnalité est souhaitée.</span><span class="sxs-lookup"><span data-stu-id="bda5c-158">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="bda5c-159">Quand une action a plusieurs paramètres explicitement spécifiés (par le biais de `[FromBody]`) ou déduits comme étant liés à partir du corps de la demande, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="bda5c-159">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="bda5c-160">Par exemple, les signatures d’action suivantes génèrent une exception :</span><span class="sxs-lookup"><span data-stu-id="bda5c-160">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="bda5c-161">**[FromForm]** est déduit pour les paramètres d’action de type <xref:Microsoft.AspNetCore.Http.IFormFile> et <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="bda5c-161">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="bda5c-162">Il n’est pas déduit pour les types simples ou définis par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bda5c-162">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="bda5c-163">**[FromRoute]** est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bda5c-163">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="bda5c-164">Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="bda5c-164">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="bda5c-165">**[FromQuery]** est déduit pour tous les autres paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="bda5c-165">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="bda5c-166">Les règles d’inférence par défaut sont désactivées quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="bda5c-166">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="bda5c-167">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="bda5c-167">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="bda5c-168">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="bda5c-168">Multipart/form-data request inference</span></span>

<span data-ttu-id="bda5c-169">Quand un paramètre d’action est annoté avec l’attribut [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), le type de contenu de demande `multipart/form-data` est déduit.</span><span class="sxs-lookup"><span data-stu-id="bda5c-169">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="bda5c-170">Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="bda5c-170">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="bda5c-171">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="bda5c-171">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="bda5c-172">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="bda5c-172">Attribute routing requirement</span></span>

<span data-ttu-id="bda5c-173">Le routage d’attribut devient une exigence.</span><span class="sxs-lookup"><span data-stu-id="bda5c-173">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="bda5c-174">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bda5c-174">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="bda5c-175">Les actions sont inaccessibles par le biais des [itinéraires conventionnels](xref:mvc/controllers/routing#conventional-routing) définis dans <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou par <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> dans *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="bda5c-175">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="bda5c-176">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bda5c-176">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
