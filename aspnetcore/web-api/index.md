---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les fonctionnalités disponibles pour la création d’une API web dans ASP.NET Core et quand il convient d’utiliser chaque fonctionnalité.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274964"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="583f5-103">Créer des API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="583f5-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="583f5-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="583f5-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="583f5-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="583f5-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="583f5-106">Ce document explique comment créer une API web ASP.NET Core et quand il convient le plus d’utiliser chaque fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="583f5-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="583f5-107">Dériver la classe de ControllerBase</span><span class="sxs-lookup"><span data-stu-id="583f5-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="583f5-108">Héritez de la classe [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) dans un contrôleur destiné à servir d’API web.</span><span class="sxs-lookup"><span data-stu-id="583f5-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="583f5-109">Exemple :</span><span class="sxs-lookup"><span data-stu-id="583f5-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="583f5-110">La classe `ControllerBase` fournit l’accès à de nombreuses propriétés et méthodes.</span><span class="sxs-lookup"><span data-stu-id="583f5-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="583f5-111">Dans l’exemple précédent, certaines de ces méthodes incluent [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) et [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="583f5-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="583f5-112">Ces méthodes sont appelées dans les méthodes d’action pour retourner les codes d’état HTTP 400 et 201, respectivement.</span><span class="sxs-lookup"><span data-stu-id="583f5-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="583f5-113">La propriété [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), également fournie par `ControllerBase`, permet d’effectuer la validation des modèles de demande.</span><span class="sxs-lookup"><span data-stu-id="583f5-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="583f5-114">Annoter la classe avec ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="583f5-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="583f5-115">ASP.NET Core 2.1 introduit l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) pour désigner une classe de contrôleur d’API web.</span><span class="sxs-lookup"><span data-stu-id="583f5-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="583f5-116">Exemple :</span><span class="sxs-lookup"><span data-stu-id="583f5-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="583f5-117">Généralement associé à `ControllerBase`, cet attribut donne accès à des méthodes et propriétés utiles.</span><span class="sxs-lookup"><span data-stu-id="583f5-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="583f5-118">`ControllerBase` permet d’accéder aux méthodes telles que [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) et [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="583f5-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="583f5-119">Une autre approche consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]` :</span><span class="sxs-lookup"><span data-stu-id="583f5-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="583f5-120">Les sections suivantes décrivent les fonctionnalités pratiques ajoutées par l’attribut.</span><span class="sxs-lookup"><span data-stu-id="583f5-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="583f5-121">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="583f5-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="583f5-122">Les erreurs de validation déclenchent automatiquement une réponse HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="583f5-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="583f5-123">Le code suivant devient inutile dans vos actions :</span><span class="sxs-lookup"><span data-stu-id="583f5-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="583f5-124">Ce comportement par défaut est désactivé avec le code suivant dans *Startup.ConfigureServices* :</span><span class="sxs-lookup"><span data-stu-id="583f5-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="583f5-125">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="583f5-125">Binding source parameter inference</span></span>

<span data-ttu-id="583f5-126">Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="583f5-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="583f5-127">Les attributs de source de liaison suivants existent :</span><span class="sxs-lookup"><span data-stu-id="583f5-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="583f5-128">Attribut</span><span class="sxs-lookup"><span data-stu-id="583f5-128">Attribute</span></span>|<span data-ttu-id="583f5-129">Source de liaison</span><span class="sxs-lookup"><span data-stu-id="583f5-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="583f5-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="583f5-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="583f5-131">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="583f5-131">Request body</span></span> |
|<span data-ttu-id="583f5-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="583f5-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="583f5-133">Données de formulaire dans le corps de la demande</span><span class="sxs-lookup"><span data-stu-id="583f5-133">Form data in the request body</span></span> |
|<span data-ttu-id="583f5-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="583f5-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="583f5-135">En-tête de demande</span><span class="sxs-lookup"><span data-stu-id="583f5-135">Request header</span></span> |
|<span data-ttu-id="583f5-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="583f5-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="583f5-137">Paramètre de la chaîne de requête de la demande</span><span class="sxs-lookup"><span data-stu-id="583f5-137">Request query string parameter</span></span> |
|<span data-ttu-id="583f5-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="583f5-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="583f5-139">Données d’itinéraire à partir de la demande actuelle</span><span class="sxs-lookup"><span data-stu-id="583f5-139">Route data from the current request</span></span> |
|<span data-ttu-id="583f5-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="583f5-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="583f5-141">Service de demande injecté comme paramètre d’action</span><span class="sxs-lookup"><span data-stu-id="583f5-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="583f5-142">N’**utilisez** pas `[FromRoute]` lorsque les valeurs peuvent contenir `%2f` (c’est-à dire `/`), car `%2f` ne sera pas sans séquence d’échappement vers `/`.</span><span class="sxs-lookup"><span data-stu-id="583f5-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="583f5-143">Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.</span><span class="sxs-lookup"><span data-stu-id="583f5-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="583f5-144">Sans l’attribut `[ApiController]`, les attributs de source de liaison sont définis explicitement.</span><span class="sxs-lookup"><span data-stu-id="583f5-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="583f5-145">Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :</span><span class="sxs-lookup"><span data-stu-id="583f5-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="583f5-146">Des règles d’inférence sont appliquées pour les sources de données par défaut des paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="583f5-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="583f5-147">Ces règles configurent les sources de liaison que vous seriez susceptible d’appliquer manuellement aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="583f5-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="583f5-148">Les attributs de source de liaison se comportent comme suit :</span><span class="sxs-lookup"><span data-stu-id="583f5-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="583f5-149">**[FromBody]** est déduit des paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="583f5-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="583f5-150">Une exception à cette règle est tout type complexe intégré ayant une signification spéciale, comme [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) et [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="583f5-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="583f5-151">Le code de l’inférence de la source de liaison ignore ces types spéciaux.</span><span class="sxs-lookup"><span data-stu-id="583f5-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="583f5-152">Quand une action a plusieurs paramètres explicitement spécifiés (par le biais de `[FromBody]`) ou déduits comme étant liés à partir du corps de la demande, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="583f5-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="583f5-153">Par exemple, les signatures d’action suivantes génèrent une exception :</span><span class="sxs-lookup"><span data-stu-id="583f5-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="583f5-154">**[FromForm]** est déduit pour les paramètres d’action de type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) et [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="583f5-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="583f5-155">Il n’est pas déduit pour les types simples ou définis par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="583f5-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="583f5-156">**[FromRoute]** est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="583f5-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="583f5-157">Quand plusieurs itinéraires correspondent à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="583f5-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="583f5-158">**[FromQuery]** est déduit pour tous les autres paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="583f5-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="583f5-159">Les règles d’inférence par défaut sont désactivées avec le code suivant dans *Startup.ConfigureServices* :</span><span class="sxs-lookup"><span data-stu-id="583f5-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="583f5-160">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="583f5-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="583f5-161">Quand un paramètre d’action est annoté avec l’attribut [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), le type de contenu de demande `multipart/form-data` est déduit.</span><span class="sxs-lookup"><span data-stu-id="583f5-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="583f5-162">Le comportement par défaut est désactivé avec le code suivant dans *Startup.ConfigureServices* :</span><span class="sxs-lookup"><span data-stu-id="583f5-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="583f5-163">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="583f5-163">Attribute routing requirement</span></span>

<span data-ttu-id="583f5-164">Le routage d’attribut devient une exigence.</span><span class="sxs-lookup"><span data-stu-id="583f5-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="583f5-165">Exemple :</span><span class="sxs-lookup"><span data-stu-id="583f5-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="583f5-166">Les actions sont inaccessibles par le biais des [itinéraires conventionnels](xref:mvc/controllers/routing#conventional-routing) définis dans [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ou par [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dans *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="583f5-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="583f5-167">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="583f5-167">Additional resources</span></span>

* [<span data-ttu-id="583f5-168">Types de retour des actions du contrôleur</span><span class="sxs-lookup"><span data-stu-id="583f5-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="583f5-169">Formateurs personnalisés</span><span class="sxs-lookup"><span data-stu-id="583f5-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="583f5-170">Mettre en forme les données de la réponse</span><span class="sxs-lookup"><span data-stu-id="583f5-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="583f5-171">Pages d’aide avec Swagger</span><span class="sxs-lookup"><span data-stu-id="583f5-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="583f5-172">Routage vers les actions du contrôleur</span><span class="sxs-lookup"><span data-stu-id="583f5-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
