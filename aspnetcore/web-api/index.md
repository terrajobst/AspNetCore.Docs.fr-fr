---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les fonctionnalités disponibles pour la création d’une API web dans ASP.NET Core et quand il convient d’utiliser chaque fonctionnalité.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
ms.openlocfilehash: 8ba20c51f38a43adca4133a402c6d741379a4c54
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341611"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="a2b1c-103">Créer des API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2b1c-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="a2b1c-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="a2b1c-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="a2b1c-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a2b1c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a2b1c-106">Ce document explique comment créer une API web ASP.NET Core et quand il convient le plus d’utiliser chaque fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="a2b1c-107">Dériver la classe de ControllerBase</span><span class="sxs-lookup"><span data-stu-id="a2b1c-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="a2b1c-108">Héritez de la classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> dans un contrôleur destiné à servir d’API web.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="a2b1c-109">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="a2b1c-110">La classe `ControllerBase` fournit l’accès à de plusieurs propriétés et méthodes.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="a2b1c-111">Dans le code précédent, les exemples incluent <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="a2b1c-112">Ces méthodes sont appelées dans les méthodes d’action pour retourner les codes d’état HTTP 400 et 201, respectivement.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="a2b1c-113">La propriété <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, également fournie par `ControllerBase`, permet de gérer la validation des modèles de requêtes.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a><span data-ttu-id="a2b1c-114">Annotation avec attribut ApiController</span><span class="sxs-lookup"><span data-stu-id="a2b1c-114">Annotation with ApiController attribute</span></span>

<span data-ttu-id="a2b1c-115">ASP.NET Core 2.1 introduit l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) pour désigner une classe de contrôleur d’API web.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="a2b1c-116">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="a2b1c-117">Une version de compatibilité 2.1 ou ultérieure, définie par le biais de <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, est obligatoire pour utiliser cet attribut au niveau du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="a2b1c-118">Par exemple, le code en surbrillance dans `Startup.ConfigureServices` définit l’indicateur de compatibilité 2.1 :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="a2b1c-119">Pour plus d'informations, consultez <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2b1c-120">Dans ASP.NET Core 2.2 ou ultérieur, l’attribut `[ApiController]` peut être appliqué à un assembly.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="a2b1c-121">De cette manière, l’annotation applique le comportement de l’API web à tous les contrôleurs de l’assembly.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="a2b1c-122">Notez que les contrôleurs individuels n’ont aucun moyen de refuser.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="a2b1c-123">Il est recommandé d’appliquer des attributs de niveau assembly à la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="a2b1c-124">Une version de compatibilité 2.2 ou ultérieure, définie par le biais de <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, est obligatoire pour utiliser cet attribut au niveau de l’assembly.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a2b1c-125">L’attribut `[ApiController]` est généralement associé à `ControllerBase` pour donner aux contrôleurs un comportement spécifique à REST.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="a2b1c-126">`ControllerBase` permet d’accéder aux méthodes telles que <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="a2b1c-127">Une autre approche consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]` :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="a2b1c-128">Les sections suivantes décrivent les fonctionnalités pratiques ajoutées par l’attribut.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="a2b1c-129">Réponses HTTP 400 automatiques</span><span class="sxs-lookup"><span data-stu-id="a2b1c-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="a2b1c-130">Les erreurs de validation de modèles déclenchent automatiquement une réponse HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-130">Model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="a2b1c-131">Par conséquent, le code suivant devient inutile dans vos actions :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-131">Consequently, the following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="a2b1c-132">Utilisez <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> pour personnaliser la sortie de la réponse résultante.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="a2b1c-133">La désactivation du comportement par défaut est utile lorsque votre action peut récupérer suite à une erreur de validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-133">Disabling the default behavior is useful when your action can recover from a model validation error.</span></span> <span data-ttu-id="a2b1c-134">Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-134">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="a2b1c-135">Ajoutez le code suivant dans `Startup.ConfigureServices` après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-135">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2b1c-136">Avec un indicateur de compatibilité 2.2 ou ultérieur, le type de réponse par défaut pour les réponses HTTP 400 est <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-136">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="a2b1c-137">Le type `ValidationProblemDetails` est conforme à la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="a2b1c-137">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="a2b1c-138">Définissez la propriété `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` avec la valeur `true` pour retourner à la place le format d’erreur ASP.NET Core 2.1 de <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-138">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="a2b1c-139">Ajoutez le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-139">Add the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="a2b1c-140">Inférence de paramètre de source de liaison</span><span class="sxs-lookup"><span data-stu-id="a2b1c-140">Binding source parameter inference</span></span>

<span data-ttu-id="a2b1c-141">Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-141">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="a2b1c-142">Les attributs de source de liaison suivants existent :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-142">The following binding source attributes exist:</span></span>

|<span data-ttu-id="a2b1c-143">Attribut</span><span class="sxs-lookup"><span data-stu-id="a2b1c-143">Attribute</span></span>|<span data-ttu-id="a2b1c-144">Source de liaison</span><span class="sxs-lookup"><span data-stu-id="a2b1c-144">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="a2b1c-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="a2b1c-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="a2b1c-146">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="a2b1c-146">Request body</span></span> |
|<span data-ttu-id="a2b1c-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="a2b1c-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="a2b1c-148">Données de formulaire dans le corps de la demande</span><span class="sxs-lookup"><span data-stu-id="a2b1c-148">Form data in the request body</span></span> |
|<span data-ttu-id="a2b1c-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="a2b1c-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="a2b1c-150">En-tête de demande</span><span class="sxs-lookup"><span data-stu-id="a2b1c-150">Request header</span></span> |
|<span data-ttu-id="a2b1c-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="a2b1c-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="a2b1c-152">Paramètre de la chaîne de requête de la demande</span><span class="sxs-lookup"><span data-stu-id="a2b1c-152">Request query string parameter</span></span> |
|<span data-ttu-id="a2b1c-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="a2b1c-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="a2b1c-154">Données d’itinéraire à partir de la demande actuelle</span><span class="sxs-lookup"><span data-stu-id="a2b1c-154">Route data from the current request</span></span> |
|<span data-ttu-id="a2b1c-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="a2b1c-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="a2b1c-156">Service de demande injecté comme paramètre d’action</span><span class="sxs-lookup"><span data-stu-id="a2b1c-156">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="a2b1c-157">N’utilisez pas `[FromRoute]` lorsque les valeurs risquent de contenir `%2f` (c’est-à-dire `/`).</span><span class="sxs-lookup"><span data-stu-id="a2b1c-157">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="a2b1c-158">`%2f` ne sera pas sans la séquence d’échappement `/`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-158">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="a2b1c-159">Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-159">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="a2b1c-160">Sans l’attribut `[ApiController]`, les attributs de source de liaison sont définis explicitement.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-160">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="a2b1c-161">Sans `[ApiController]` ou autres attributs de source de liaison comme `[FromQuery]`, le runtime ASP.NET Core tente d’utiliser le classeur de modèles objet complexe.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-161">Without `[ApiController]` or other binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="a2b1c-162">Le classeur de modèles objet complexe extrait des données à partir de fournisseurs de valeurs (qui ont un ordre défini).</span><span class="sxs-lookup"><span data-stu-id="a2b1c-162">The complex object model binder pulls data from value providers (which have a defined order).</span></span> <span data-ttu-id="a2b1c-163">Par exemple,le « classeur de modèles body » est toujours activé.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-163">For instance, the 'body model binder' is always opt in.</span></span>

<span data-ttu-id="a2b1c-164">Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="a2b1c-165">Des règles d’inférence sont appliquées pour les sources de données par défaut des paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="a2b1c-166">Ces règles configurent les sources de liaison que vous seriez susceptible d’appliquer manuellement aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="a2b1c-167">Les attributs de source de liaison se comportent comme suit :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="a2b1c-168">**[FromBody]** est déduit des paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="a2b1c-169">Une exception à cette règle est tout type complexe intégré ayant une signification spéciale, comme <xref:Microsoft.AspNetCore.Http.IFormCollection> et <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="a2b1c-170">Le code de l’inférence de la source de liaison ignore ces types spéciaux.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="a2b1c-171">`[FromBody]` n’est pas déduit pour les types simples tels que `string` ou `int`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="a2b1c-172">Vous devez donc utiliser l’attribut `[FromBody]` pour les types simples quand cette fonctionnalité est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="a2b1c-173">Quand une action a plusieurs paramètres explicitement spécifiés (par le biais de `[FromBody]`) ou déduits comme étant liés à partir du corps de la demande, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="a2b1c-174">Par exemple, les signatures d’action suivantes génèrent une exception :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-174">For example, the following action signatures cause an exception:</span></span>

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > <span data-ttu-id="a2b1c-175">Dans ASP.NET Core 2.1, les paramètres de type collection comme les listes et les tableaux sont déduits de manière incorrecte en tant que [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span><span class="sxs-lookup"><span data-stu-id="a2b1c-175">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span></span> <span data-ttu-id="a2b1c-176">[[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) doit être utilisé pour ces paramètres s’ils doivent être liés à partir du corps de la requête.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-176">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="a2b1c-177">Ce problème est résolu dans ASP.NET Core 2.2 ou version ultérieure, où les paramètres de type collection sont déduits pour être liés à partir du corps par défaut.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-177">This behavior is fixed in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

* <span data-ttu-id="a2b1c-178">**[FromForm]** est déduit pour les paramètres d’action de type <xref:Microsoft.AspNetCore.Http.IFormFile> et <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-178">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="a2b1c-179">Il n’est pas déduit pour les types simples ou définis par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-179">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="a2b1c-180">**[FromRoute]** est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-180">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="a2b1c-181">Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-181">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="a2b1c-182">**[FromQuery]** est déduit pour tous les autres paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-182">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="a2b1c-183">Les règles d’inférence par défaut sont désactivées quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-183">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="a2b1c-184">Ajoutez le code suivant dans `Startup.ConfigureServices` après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-184">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="a2b1c-185">Inférence de demande multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="a2b1c-185">Multipart/form-data request inference</span></span>

<span data-ttu-id="a2b1c-186">Quand un paramètre d’action est annoté avec l’attribut [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), le type de contenu de demande `multipart/form-data` est déduit.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-186">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="a2b1c-187">Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-187">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2b1c-188">Ajoutez le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-188">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="a2b1c-189">Ajoutez le code suivant dans `Startup.ConfigureServices` après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-189">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="a2b1c-190">Exigence du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="a2b1c-190">Attribute routing requirement</span></span>

<span data-ttu-id="a2b1c-191">Le routage d’attribut devient une exigence.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-191">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="a2b1c-192">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-192">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="a2b1c-193">Les actions sont inaccessibles par le biais de [routes conventionnelles](xref:mvc/controllers/routing#conventional-routing) définies dans <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou par <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-193">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="a2b1c-194">Réponses de la fonctionnalité Détails du problème pour les codes d’état erreur</span><span class="sxs-lookup"><span data-stu-id="a2b1c-194">Problem details responses for error status codes</span></span>

<span data-ttu-id="a2b1c-195">Dans ASP.NET Core 2.2 ou ultérieur, MVC transforme un résultat d’erreur (résultat avec un code d’état égal ou supérieur à 400) en résultat avec <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-195">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="a2b1c-196">`ProblemDetails` est :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-196">`ProblemDetails` is:</span></span>

* <span data-ttu-id="a2b1c-197">Un type basé sur la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="a2b1c-197">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="a2b1c-198">Un format standardisé pour spécifier les détails des erreurs lisibles par machine dans une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-198">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="a2b1c-199">Prenons le code suivant dans une action de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-199">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="a2b1c-200">La réponse HTTP pour `NotFound` a le code d’état 404 avec le corps `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-200">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="a2b1c-201">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-201">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="a2b1c-202">La fonctionnalité Détails du problème nécessite un indicateur de compatibilité de 2.2 ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-202">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="a2b1c-203">Le comportement par défaut est désactivé quand la propriété `SuppressMapClientErrors` a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-203">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="a2b1c-204">Ajoutez le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-204">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="a2b1c-205">Utilisez la propriété `ClientErrorMapping` pour configurer le contenu de la réponse `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="a2b1c-205">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="a2b1c-206">Par exemple, le code suivant met à jour la propriété `type` pour les réponses 404 :</span><span class="sxs-lookup"><span data-stu-id="a2b1c-206">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a2b1c-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a2b1c-207">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
****
