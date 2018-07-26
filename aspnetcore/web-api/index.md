---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les fonctionnalités disponibles pour la création d’une API web dans ASP.NET Core et quand il convient d’utiliser chaque fonctionnalité.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: ccee4f7bae0abe1b36088d58e5c1e1362d8de9f0
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243095"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="36150-103">Créer des API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36150-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="36150-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="36150-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="36150-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="36150-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="36150-106">Ce document explique comment créer une API web ASP.NET Core et quand il convient le plus d’utiliser chaque fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="36150-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="36150-107">Dériver la classe de ControllerBase</span><span class="sxs-lookup"><span data-stu-id="36150-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="36150-108">Héritez de la classe [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) dans un contrôleur destiné à servir d’API web.</span><span class="sxs-lookup"><span data-stu-id="36150-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="36150-109">Exemple :</span><span class="sxs-lookup"><span data-stu-id="36150-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="36150-110">La classe `ControllerBase` fournit l’accès à de plusieurs propriétés et méthodes.</span><span class="sxs-lookup"><span data-stu-id="36150-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="36150-111">Dans le code précédent, les exemples incluent [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) et [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="36150-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="36150-112">Ces méthodes sont appelées dans les méthodes d’action pour retourner les codes d’état HTTP 400 et 201, respectivement.</span><span class="sxs-lookup"><span data-stu-id="36150-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="36150-113">La propriété [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), également fournie par `ControllerBase`, permet de gérer la validation des modèles de demande.</span><span class="sxs-lookup"><span data-stu-id="36150-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="36150-114">Annoter la classe avec ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="36150-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="36150-115">ASP.NET Core 2.1 introduit l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) pour désigner une classe de contrôleur d’API web.</span><span class="sxs-lookup"><span data-stu-id="36150-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="36150-116">Exemple :</span><span class="sxs-lookup"><span data-stu-id="36150-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="36150-117">Une version de compatibilité 2.1 ou ultérieure, définie via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), est obligatoire pour utiliser cet attribut.</span><span class="sxs-lookup"><span data-stu-id="36150-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="36150-118">Par exemple, le code en surbrillance dans *Startup.ConfigureServices* définit l’indicateur de compatibilité 2.1 :</span><span class="sxs-lookup"><span data-stu-id="36150-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="36150-119">L’attribut `[ApiController]` est généralement associé à `ControllerBase` pour donner aux contrôleurs un comportement spécifique à REST.</span><span class="sxs-lookup"><span data-stu-id="36150-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="36150-120">`ControllerBase` permet d’accéder aux méthodes telles que [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) et [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="36150-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="36150-121">Une autre approche consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]` :</span><span class="sxs-lookup"><span data-stu-id="36150-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="36150-122">Les sections suivantes décrivent les fonctionnalités pratiques ajoutées par l’attribut.</span><span class="sxs-lookup"><span data-stu-id="36150-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="36150-123">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="36150-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="36150-124">Les erreurs de validation déclenchent automatiquement une réponse HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="36150-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="36150-125">Le code suivant devient inutile dans vos actions :</span><span class="sxs-lookup"><span data-stu-id="36150-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="36150-126">Le comportement par défaut est désactivé lorsque la propriété [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) est définie avec la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="36150-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="36150-127">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="36150-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="36150-128">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="36150-128">Binding source parameter inference</span></span>

<span data-ttu-id="36150-129">Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="36150-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="36150-130">Les attributs de source de liaison suivants existent :</span><span class="sxs-lookup"><span data-stu-id="36150-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="36150-131">Attribut</span><span class="sxs-lookup"><span data-stu-id="36150-131">Attribute</span></span>|<span data-ttu-id="36150-132">Source de liaison</span><span class="sxs-lookup"><span data-stu-id="36150-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="36150-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="36150-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="36150-134">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="36150-134">Request body</span></span> |
|<span data-ttu-id="36150-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="36150-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="36150-136">Données de formulaire dans le corps de la demande</span><span class="sxs-lookup"><span data-stu-id="36150-136">Form data in the request body</span></span> |
|<span data-ttu-id="36150-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="36150-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="36150-138">En-tête de demande</span><span class="sxs-lookup"><span data-stu-id="36150-138">Request header</span></span> |
|<span data-ttu-id="36150-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="36150-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="36150-140">Paramètre de la chaîne de requête de la demande</span><span class="sxs-lookup"><span data-stu-id="36150-140">Request query string parameter</span></span> |
|<span data-ttu-id="36150-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="36150-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="36150-142">Données d’itinéraire à partir de la demande actuelle</span><span class="sxs-lookup"><span data-stu-id="36150-142">Route data from the current request</span></span> |
|<span data-ttu-id="36150-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="36150-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="36150-144">Service de demande injecté comme paramètre d’action</span><span class="sxs-lookup"><span data-stu-id="36150-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="36150-145">N’utilisez pas `[FromRoute]` lorsque les valeurs risquent de contenir `%2f` (c’est-à-dire `/`).</span><span class="sxs-lookup"><span data-stu-id="36150-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="36150-146">`%2f` ne sera pas sans la séquence d’échappement `/`.</span><span class="sxs-lookup"><span data-stu-id="36150-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="36150-147">Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.</span><span class="sxs-lookup"><span data-stu-id="36150-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="36150-148">Sans l’attribut `[ApiController]`, les attributs de source de liaison sont définis explicitement.</span><span class="sxs-lookup"><span data-stu-id="36150-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="36150-149">Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :</span><span class="sxs-lookup"><span data-stu-id="36150-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="36150-150">Des règles d’inférence sont appliquées pour les sources de données par défaut des paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="36150-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="36150-151">Ces règles configurent les sources de liaison que vous seriez susceptible d’appliquer manuellement aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="36150-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="36150-152">Les attributs de source de liaison se comportent comme suit :</span><span class="sxs-lookup"><span data-stu-id="36150-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="36150-153">**[FromBody]** est déduit des paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="36150-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="36150-154">Une exception à cette règle est tout type complexe intégré ayant une signification spéciale, comme [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) et [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="36150-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="36150-155">Le code de l’inférence de la source de liaison ignore ces types spéciaux.</span><span class="sxs-lookup"><span data-stu-id="36150-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="36150-156">Quand une action a plusieurs paramètres explicitement spécifiés (par le biais de `[FromBody]`) ou déduits comme étant liés à partir du corps de la demande, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="36150-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="36150-157">Par exemple, les signatures d’action suivantes génèrent une exception :</span><span class="sxs-lookup"><span data-stu-id="36150-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="36150-158">**[FromForm]** est déduit pour les paramètres d’action de type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) et [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="36150-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="36150-159">Il n’est pas déduit pour les types simples ou définis par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="36150-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="36150-160">**[FromRoute]** est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="36150-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="36150-161">Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="36150-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="36150-162">**[FromQuery]** est déduit pour tous les autres paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="36150-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="36150-163">Les règles d’inférence par défaut sont désactivées quand la propriété [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) est définie avec la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="36150-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="36150-164">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="36150-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="36150-165">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="36150-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="36150-166">Quand un paramètre d’action est annoté avec l’attribut [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), le type de contenu de demande `multipart/form-data` est déduit.</span><span class="sxs-lookup"><span data-stu-id="36150-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="36150-167">Le comportement par défaut est désactivé quand la propriété [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) est définie avec la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="36150-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="36150-168">Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="36150-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="36150-169">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="36150-169">Attribute routing requirement</span></span>

<span data-ttu-id="36150-170">Le routage d’attribut devient une exigence.</span><span class="sxs-lookup"><span data-stu-id="36150-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="36150-171">Exemple :</span><span class="sxs-lookup"><span data-stu-id="36150-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="36150-172">Les actions sont inaccessibles par le biais des [itinéraires conventionnels](xref:mvc/controllers/routing#conventional-routing) définis dans [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ou par [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dans *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="36150-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="36150-173">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="36150-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
