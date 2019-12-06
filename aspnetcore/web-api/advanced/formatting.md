---
title: Mettre en forme les données des réponses dans l’API web ASP.NET Core
author: ardalis
description: Découvrez comment mettre en forme les données des réponses dans l’API web ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/05/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 4433ed11dad7522962ebeed411c4bef88e07e7af
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881358"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="d0e0e-103">Mettre en forme les données des réponses dans l’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0e0e-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="d0e0e-104">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d0e0e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d0e0e-105">ASP.NET Core MVC prend en charge la mise en forme des données de réponse.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="d0e0e-106">Les données de réponse peuvent être formatées à l’aide de formats spécifiques ou en réponse au format demandé par le client.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="d0e0e-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d0e0e-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="d0e0e-108">Résultats des actions spécifiques au format</span><span class="sxs-lookup"><span data-stu-id="d0e0e-108">Format-specific Action Results</span></span>

<span data-ttu-id="d0e0e-109">Certains types de résultats d’action sont spécifiques à un format particulier, comme <xref:Microsoft.AspNetCore.Mvc.JsonResult> et <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="d0e0e-110">Les actions peuvent retourner des résultats mis en forme dans un format particulier, indépendamment des préférences du client.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="d0e0e-111">Par exemple, le retour de `JsonResult` retourne des données au format JSON.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="d0e0e-112">Le renvoi d' `ContentResult` ou d’une chaîne retourne des données de chaîne au format texte brut.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="d0e0e-113">Une action n’est pas requise pour retourner un type spécifique.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="d0e0e-114">ASP.NET Core prend en charge toute valeur de retour d’objet.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="d0e0e-115">Les résultats des actions qui retournent des objets qui ne sont pas des types <xref:Microsoft.AspNetCore.Mvc.IActionResult> sont sérialisés à l’aide de l’implémentation de <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> appropriée.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="d0e0e-116">Pour plus d'informations, consultez <xref:web-api/action-return-types>.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="d0e0e-117">La méthode d’assistance intégrée <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> retourne des données au format JSON : [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="d0e0e-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="d0e0e-118">L’exemple de téléchargement retourne la liste des auteurs.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="d0e0e-119">À l’aide des outils de développement du navigateur F12 ou du [poster](https://www.getpostman.com/tools) avec le code précédent :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="d0e0e-120">L’en-tête de réponse contenant **Content-type :** `application/json; charset=utf-8` est affiché.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="d0e0e-121">Les en-têtes de demande s’affichent.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-121">The request headers are displayed.</span></span> <span data-ttu-id="d0e0e-122">Par exemple, l’en-tête `Accept`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-122">For example, the `Accept` header.</span></span> <span data-ttu-id="d0e0e-123">L’en-tête `Accept` est ignoré par le code précédent.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="d0e0e-124">Pour retourner des données mises en forme en texte brut, utilisez <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> et le helper <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="d0e0e-125">Dans le code précédent, le `Content-Type` retourné est `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="d0e0e-126">Le retour d’une chaîne `Content-Type` de `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="d0e0e-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="d0e0e-127">Pour les actions avec plusieurs types de retour, retournez `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="d0e0e-128">Par exemple, le retour de codes d’état HTTP différents en fonction du résultat des opérations effectuées.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="d0e0e-129">Négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="d0e0e-129">Content negotiation</span></span>

<span data-ttu-id="d0e0e-130">La négociation de contenu se produit lorsque le client spécifie un [en-tête Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="d0e0e-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="d0e0e-131">Le format par défaut utilisé par ASP.NET Core est [JSON](https://json.org/).</span><span class="sxs-lookup"><span data-stu-id="d0e0e-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="d0e0e-132">La négociation de contenu est :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-132">Content negotiation is:</span></span>

* <span data-ttu-id="d0e0e-133">Implémenté par <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="d0e0e-134">Intégré aux résultats d’action spécifiques au code d’État retournés par les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="d0e0e-135">Les méthodes d’assistance des résultats d’action sont basées sur `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="d0e0e-136">Quand un type de modèle est retourné, le type de retour est `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="d0e0e-137">La méthode d’action suivante utilise les méthodes helper `Ok` et `NotFound` :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="d0e0e-138">Par défaut, ASP.NET Core prend en charge les types de média `application/json`, `text/json`et `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-138">By default, ASP.NET Core supports `application/json`, `text/json`, and `text/plain` media types.</span></span> <span data-ttu-id="d0e0e-139">Les outils tels que [Fiddler](https://www.telerik.com/fiddler) ou [postal](https://www.getpostman.com/tools) peuvent définir l’en-tête de demande `Accept` pour spécifier le format de retour.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` request header to specify the return format.</span></span> <span data-ttu-id="d0e0e-140">Lorsque l’en-tête `Accept` contient un type pris en charge par le serveur, ce type est retourné.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-140">When the `Accept` header contains a type the server supports, that type is returned.</span></span> <span data-ttu-id="d0e0e-141">La section suivante montre comment ajouter des formateurs supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-141">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="d0e0e-142">Les actions de contrôleur peuvent retourner des POCO (Plain Old CLR Objects).</span><span class="sxs-lookup"><span data-stu-id="d0e0e-142">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="d0e0e-143">Lorsqu’un POCO est retourné, le runtime crée automatiquement un `ObjectResult` qui encapsule l’objet.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-143">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="d0e0e-144">Le client obtient l’objet sérialisé mis en forme.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-144">The client gets the formatted serialized object.</span></span> <span data-ttu-id="d0e0e-145">Si l’objet retourné est `null`, une réponse `204 No Content` est retournée.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-145">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="d0e0e-146">Retour d’un type d’objet :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="d0e0e-147">Dans le code précédent, une demande d’alias d’auteur valide retourne une réponse `200 OK` avec les données de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-147">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="d0e0e-148">Une demande d’alias non valide retourne une réponse `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-148">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="d0e0e-149">En-tête Accept</span><span class="sxs-lookup"><span data-stu-id="d0e0e-149">The Accept header</span></span>

<span data-ttu-id="d0e0e-150">La *négociation* de contenu a lieu lorsqu’un en-tête de `Accept` apparaît dans la demande.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-150">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="d0e0e-151">Quand une demande contient un en-tête Accept, ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-151">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="d0e0e-152">Énumère les types de médias dans l’en-tête Accept dans l’ordre de préférence.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-152">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="d0e0e-153">Tente de trouver un formateur capable de produire une réponse dans l’un des formats spécifiés.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-153">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="d0e0e-154">Si aucun formateur trouvé pouvant satisfaire la demande du client, ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-154">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="d0e0e-155">Retourne `406 Not Acceptable` si <xref:Microsoft.AspNetCore.Mvc.MvcOptions> a été défini, ou-</span><span class="sxs-lookup"><span data-stu-id="d0e0e-155">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="d0e0e-156">Tente de trouver le premier formateur qui peut produire une réponse.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-156">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="d0e0e-157">Si aucun formateur n’est configuré pour le format demandé, le premier formateur qui peut mettre en forme l’objet est utilisé.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-157">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="d0e0e-158">Si aucun en-tête de `Accept` n’apparaît dans la requête :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-158">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="d0e0e-159">Le premier formateur qui peut gérer l’objet est utilisé pour sérialiser la réponse.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-159">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="d0e0e-160">Aucune négociation n’a lieu.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-160">There isn't any negotiation taking place.</span></span> <span data-ttu-id="d0e0e-161">Le serveur détermine le format à retourner.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-161">The server is determining what format to return.</span></span>

<span data-ttu-id="d0e0e-162">Si l’en-tête Accept contient `*/*`, l’en-tête est ignoré, sauf si `RespectBrowserAcceptHeader` a la valeur true sur <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-162">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="d0e0e-163">Navigateurs et négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="d0e0e-163">Browsers and content negotiation</span></span>

<span data-ttu-id="d0e0e-164">Contrairement aux clients d’API typiques, les navigateurs Web fournissent des en-têtes de `Accept`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-164">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="d0e0e-165">Le navigateur Web spécifie de nombreux formats, y compris des caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-165">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="d0e0e-166">Par défaut, lorsque le Framework détecte que la demande provient d’un navigateur :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-166">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="d0e0e-167">L’en-tête `Accept` est ignoré.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-167">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="d0e0e-168">Le contenu est renvoyé au format JSON, sauf s’il a été configuré autrement.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-168">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="d0e0e-169">Cela offre une expérience plus cohérente entre les navigateurs lors de l’utilisation des API.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-169">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="d0e0e-170">Pour configurer une application pour honorer les en-têtes Accept du navigateur, définissez <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> sur `true`:</span><span class="sxs-lookup"><span data-stu-id="d0e0e-170">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="d0e0e-171">Configurer les formateurs</span><span class="sxs-lookup"><span data-stu-id="d0e0e-171">Configure formatters</span></span>

<span data-ttu-id="d0e0e-172">Les applications qui doivent prendre en charge des formats supplémentaires peuvent ajouter les packages NuGet appropriés et configurer la prise en charge.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-172">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="d0e0e-173">Il existe des formateurs distincts pour les entrées et pour les sorties.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-173">There are separate formatters for input and output.</span></span> <span data-ttu-id="d0e0e-174">Les formateurs d’entrée sont utilisés par la [liaison de modèle](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="d0e0e-174">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="d0e0e-175">Les formateurs de sortie sont utilisés pour mettre en forme les réponses.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-175">Output formatters are used to format responses.</span></span> <span data-ttu-id="d0e0e-176">Pour plus d’informations sur la création d’un formateur personnalisé, consultez [formateurs personnalisés](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="d0e0e-176">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="d0e0e-177">Ajouter la prise en charge du format XML</span><span class="sxs-lookup"><span data-stu-id="d0e0e-177">Add XML format support</span></span>

<span data-ttu-id="d0e0e-178">Les formateurs XML implémentés à l’aide de <xref:System.Xml.Serialization.XmlSerializer> sont configurés en appelant <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span><span class="sxs-lookup"><span data-stu-id="d0e0e-178">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="d0e0e-179">Le code précédent sérialise les résultats à l’aide de `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-179">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="d0e0e-180">Lorsque vous utilisez le code précédent, les méthodes de contrôleur doivent retourner le format approprié en fonction de l’en-tête `Accept` de la requête.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-180">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="d0e0e-181">Configurer des formateurs basés sur System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="d0e0e-181">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="d0e0e-182">Les fonctionnalités pour les formateurs basés sur `System.Text.Json` peuvent être configurées avec `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-182">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="d0e0e-183">Les options de sérialisation de sortie, en fonction de l’action, peuvent être configurées à l’aide de `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-183">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="d0e0e-184">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-184">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="d0e0e-185">Ajouter la prise en charge du format JSON basé sur Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="d0e0e-185">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="d0e0e-186">Avant ASP.NET Core 3,0, les formateurs JSON utilisés par défaut ont été implémentés à l’aide du package `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-186">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="d0e0e-187">Dans ASP.NET Core 3.0 ou version ultérieure, les formateurs JSON par défaut sont basés sur `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-187">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="d0e0e-188">La prise en charge des formateurs et fonctionnalités basés sur `Newtonsoft.Json` est disponible en installant le package NuGet [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) et en le configurant dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-188">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="d0e0e-189">Certaines fonctionnalités peuvent ne pas fonctionner correctement avec les formateurs de `System.Text.Json`et nécessitent une référence aux formateurs basés sur `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-189">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="d0e0e-190">Continuez à utiliser les formateurs basés sur `Newtonsoft.Json`si l’application :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-190">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="d0e0e-191">Utilise `Newtonsoft.Json` attributs.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-191">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="d0e0e-192">Par exemple, `[JsonProperty]` ou `[JsonIgnore]`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-192">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="d0e0e-193">Personnalise les paramètres de sérialisation.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-193">Customizes the serialization settings.</span></span>
* <span data-ttu-id="d0e0e-194">S’appuie sur les fonctionnalités fournies par `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-194">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="d0e0e-195">Configure `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-195">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="d0e0e-196">Avant ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepte une instance de `JsonSerializerSettings` spécifique à `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-196">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="d0e0e-197">Génère une documentation [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>).</span><span class="sxs-lookup"><span data-stu-id="d0e0e-197">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="d0e0e-198">Les fonctionnalités des formateurs basés sur `Newtonsoft.Json`peuvent être configurées à l’aide de `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span><span class="sxs-lookup"><span data-stu-id="d0e0e-198">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="d0e0e-199">Les options de sérialisation de sortie, en fonction de l’action, peuvent être configurées à l’aide de `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-199">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="d0e0e-200">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-200">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a><span data-ttu-id="d0e0e-201">Ajouter la prise en charge du format XML</span><span class="sxs-lookup"><span data-stu-id="d0e0e-201">Add XML format support</span></span>

<span data-ttu-id="d0e0e-202">La mise en forme XML requiert le package NuGet [Microsoft. AspNetCore. Mvc. Formatters. xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) .</span><span class="sxs-lookup"><span data-stu-id="d0e0e-202">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="d0e0e-203">Les formateurs XML implémentés à l’aide de <xref:System.Xml.Serialization.XmlSerializer> sont configurés en appelant <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span><span class="sxs-lookup"><span data-stu-id="d0e0e-203">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="d0e0e-204">Le code précédent sérialise les résultats à l’aide de `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-204">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="d0e0e-205">Lorsque vous utilisez le code précédent, les méthodes de contrôleur doivent retourner le format approprié en fonction de l’en-tête `Accept` de la requête.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-205">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="d0e0e-206">Spécifier un format</span><span class="sxs-lookup"><span data-stu-id="d0e0e-206">Specify a format</span></span>

<span data-ttu-id="d0e0e-207">Pour limiter les formats de réponse, appliquez le filtre [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) .</span><span class="sxs-lookup"><span data-stu-id="d0e0e-207">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="d0e0e-208">Comme la plupart des [filtres](xref:mvc/controllers/filters), `[Produces]` peut être appliqué au niveau de l’action, du contrôleur ou de l’étendue globale :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-208">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="d0e0e-209">Le filtre de [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) précédent :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-209">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="d0e0e-210">Force toutes les actions du contrôleur à retourner des réponses au format JSON.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-210">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="d0e0e-211">Si d’autres formateurs sont configurés et que le client spécifie un format différent, JSON est retourné.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-211">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="d0e0e-212">Pour plus d’informations, consultez [filtres](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="d0e0e-212">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="d0e0e-213">Formateurs de cas spéciaux</span><span class="sxs-lookup"><span data-stu-id="d0e0e-213">Special case formatters</span></span>

<span data-ttu-id="d0e0e-214">Certains cas spéciaux sont implémentés avec des formateurs intégrés.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-214">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="d0e0e-215">Par défaut, les `string` types de retour sont mis en forme en tant que *texte/brut* (*texte/html* si demandé via l’en-tête `Accept`).</span><span class="sxs-lookup"><span data-stu-id="d0e0e-215">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="d0e0e-216">Ce comportement peut être supprimé en supprimant l' <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-216">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span></span> <span data-ttu-id="d0e0e-217">Les formateurs sont supprimés de la méthode `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-217">Formatters are removed in the `ConfigureServices` method.</span></span> <span data-ttu-id="d0e0e-218">Les actions qui ont un type de retour d’objet de modèle retournent `204 No Content` lors du retour de `null`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-218">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="d0e0e-219">Ce comportement peut être supprimé en supprimant l' <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-219">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="d0e0e-220">Le code suivant supprime `StringOutputFormatter` et `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-220">The following code removes the `StringOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="d0e0e-221">Sans la `StringOutputFormatter`, le format de formateur JSON intégré `string` les types de retour.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-221">Without the `StringOutputFormatter`, the built-in JSON formatter formats `string` return types.</span></span> <span data-ttu-id="d0e0e-222">Si le formateur JSON intégré est supprimé et qu’un formateur XML est disponible, le formateur XML met en forme `string` types de retour.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-222">If the built-in JSON formatter is removed and an XML formatter is available, the XML formatter formats `string` return types.</span></span> <span data-ttu-id="d0e0e-223">Sinon, les types de retour `string` retournent `406 Not Acceptable`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-223">Otherwise, `string` return types return `406 Not Acceptable`.</span></span>

<span data-ttu-id="d0e0e-224">Sans `HttpNoContentOutputFormatter`, les objets null sont mis en forme avec le formateur configuré.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-224">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="d0e0e-225">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-225">For example:</span></span>

* <span data-ttu-id="d0e0e-226">Le formateur JSON retourne une réponse avec un corps de `null`.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-226">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="d0e0e-227">Le formateur XML retourne un élément XML vide avec l’attribut `xsi:nil="true"` Set.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-227">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="d0e0e-228">Mappages d’URL de format de réponse</span><span class="sxs-lookup"><span data-stu-id="d0e0e-228">Response format URL mappings</span></span>

<span data-ttu-id="d0e0e-229">Les clients peuvent demander un format particulier dans le cadre de l’URL, par exemple :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-229">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="d0e0e-230">Dans la chaîne de requête ou dans une partie du chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-230">In the query string or part of the path.</span></span>
* <span data-ttu-id="d0e0e-231">En utilisant une extension de fichier spécifique au format, par exemple. XML ou. JSON.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-231">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="d0e0e-232">Le mappage du chemin de la requête doit être spécifié dans la route utilisée par l’API.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-232">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="d0e0e-233">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="d0e0e-233">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="d0e0e-234">L’itinéraire précédent permet de spécifier le format demandé en tant qu’extension de fichier facultative.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-234">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="d0e0e-235">L’attribut [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) vérifie l’existence de la valeur de format dans le `RouteData` et mappe le format de réponse au formateur approprié lors de la création de la réponse.</span><span class="sxs-lookup"><span data-stu-id="d0e0e-235">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="d0e0e-236">Route</span><span class="sxs-lookup"><span data-stu-id="d0e0e-236">Route</span></span>        |             <span data-ttu-id="d0e0e-237">Formatter</span><span class="sxs-lookup"><span data-stu-id="d0e0e-237">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="d0e0e-238">Le formateur de sortie par défaut</span><span class="sxs-lookup"><span data-stu-id="d0e0e-238">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="d0e0e-239">Le formateur JSON (s’il est configuré)</span><span class="sxs-lookup"><span data-stu-id="d0e0e-239">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="d0e0e-240">Le formateur XML (s’il est configuré)</span><span class="sxs-lookup"><span data-stu-id="d0e0e-240">The XML formatter (if configured)</span></span>  |
