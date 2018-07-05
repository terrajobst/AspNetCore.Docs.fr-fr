---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Résultats de l’action dans Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 7726829ac9eba339ff3ac1c94c86132cb1090783
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395527"
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="59cda-102">Résultats d’action dans Web API 2</span><span class="sxs-lookup"><span data-stu-id="59cda-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="59cda-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="59cda-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="59cda-104">Cette rubrique décrit comment les API Web ASP.NET convertit la valeur de retour à partir d’une action de contrôleur dans un message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="59cda-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="59cda-105">Une action de contrôleur d’API Web peut renvoyer la valeur d’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="59cda-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="59cda-106">void</span><span class="sxs-lookup"><span data-stu-id="59cda-106">void</span></span>
2. <span data-ttu-id="59cda-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="59cda-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="59cda-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="59cda-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="59cda-109">Un autre type</span><span class="sxs-lookup"><span data-stu-id="59cda-109">Some other type</span></span>

<span data-ttu-id="59cda-110">En fonction de ces est retourné, API Web utilise un mécanisme différent pour créer la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="59cda-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="59cda-111">Type de retour</span><span class="sxs-lookup"><span data-stu-id="59cda-111">Return type</span></span> | <span data-ttu-id="59cda-112">Comment les API Web crée la réponse</span><span class="sxs-lookup"><span data-stu-id="59cda-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="59cda-113">void</span><span class="sxs-lookup"><span data-stu-id="59cda-113">void</span></span> | <span data-ttu-id="59cda-114">Retour 204 vide (aucun contenu)</span><span class="sxs-lookup"><span data-stu-id="59cda-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="59cda-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="59cda-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="59cda-116">Convertir directement en un message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="59cda-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="59cda-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="59cda-117">**IHttpActionResult**</span></span> | <span data-ttu-id="59cda-118">Appelez **ExecuteAsync** pour créer un **HttpResponseMessage**, puis convertir un message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="59cda-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="59cda-119">Autre type</span><span class="sxs-lookup"><span data-stu-id="59cda-119">Other type</span></span> | <span data-ttu-id="59cda-120">Écrire la valeur de retour sérialisée dans le corps de réponse ; retourner 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="59cda-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="59cda-121">Le reste de cette rubrique décrit chaque option plus en détail.</span><span class="sxs-lookup"><span data-stu-id="59cda-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="59cda-122">void</span><span class="sxs-lookup"><span data-stu-id="59cda-122">void</span></span>

<span data-ttu-id="59cda-123">Si le type de retour est `void`, API Web renvoie simplement une réponse HTTP vide avec le code d’état 204 (aucun contenu).</span><span class="sxs-lookup"><span data-stu-id="59cda-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="59cda-124">Exemple de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="59cda-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="59cda-125">Réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="59cda-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="59cda-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="59cda-126">HttpResponseMessage</span></span>

<span data-ttu-id="59cda-127">Si l’action retourne un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), API Web convertit la valeur de retour directement dans un message de réponse HTTP, en utilisant les propriétés de la **HttpResponseMessage** objet à remplir le réponse.</span><span class="sxs-lookup"><span data-stu-id="59cda-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="59cda-128">Cette option vous donne un contrôle important sur le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="59cda-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="59cda-129">Par exemple, l’action du contrôleur suivant définit l’en-tête Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="59cda-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="59cda-130">Réponse :</span><span class="sxs-lookup"><span data-stu-id="59cda-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="59cda-131">Si vous passez d’un modèle de domaine pour le **CreateResponse** (méthode), API Web utilise un [formateur média](../formats-and-model-binding/media-formatters.md) pour écrire le modèle sérialisé dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="59cda-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="59cda-132">API Web utilise l’en-tête Accept dans la demande de choisir le formateur.</span><span class="sxs-lookup"><span data-stu-id="59cda-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="59cda-133">Pour plus d’informations, consultez [négociation de contenu](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="59cda-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="59cda-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="59cda-134">IHttpActionResult</span></span>

<span data-ttu-id="59cda-135">Le **IHttpActionResult** interface a été introduite dans Web API 2.</span><span class="sxs-lookup"><span data-stu-id="59cda-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="59cda-136">En fait, il définit un **HttpResponseMessage** factory.</span><span class="sxs-lookup"><span data-stu-id="59cda-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="59cda-137">Voici quelques avantages de la **IHttpActionResult** interface :</span><span class="sxs-lookup"><span data-stu-id="59cda-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="59cda-138">Simplifie la [tests unitaires](../testing-and-debugging/unit-testing-controllers-in-web-api.md) vos contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="59cda-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="59cda-139">Déplace une logique commune pour la création de réponses HTTP dans des classes séparées.</span><span class="sxs-lookup"><span data-stu-id="59cda-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="59cda-140">Rend l’objectif de l’action de contrôleur plus claire, en masquant les détails de bas niveau de la construction de la réponse.</span><span class="sxs-lookup"><span data-stu-id="59cda-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="59cda-141">**IHttpActionResult** contient une méthode unique, **ExecuteAsync**, ce qui crée de façon asynchrone un **HttpResponseMessage** instance.</span><span class="sxs-lookup"><span data-stu-id="59cda-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="59cda-142">Si une action de contrôleur retourne un **IHttpActionResult**, API Web appelle le **ExecuteAsync** méthode pour créer un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="59cda-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="59cda-143">Il convertit le **HttpResponseMessage** dans un message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="59cda-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="59cda-144">Voici une simple implémentation n’a de **IHttpActionResult** qui crée une réponse de texte brut :</span><span class="sxs-lookup"><span data-stu-id="59cda-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="59cda-145">Action de contrôleur d’exemple :</span><span class="sxs-lookup"><span data-stu-id="59cda-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="59cda-146">Réponse :</span><span class="sxs-lookup"><span data-stu-id="59cda-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="59cda-147">Plus souvent, vous utiliserez le **IHttpActionResult** implémentations définies dans le **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="59cda-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="59cda-148">Le **ApiController** classe définit des méthodes d’assistance qui retournent des résultats de ces action intégrée.</span><span class="sxs-lookup"><span data-stu-id="59cda-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="59cda-149">Dans l’exemple suivant, si la demande ne correspond pas à un ID de produit existant, le contrôleur appelle [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) pour créer une réponse 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="59cda-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="59cda-150">Sinon, le contrôleur appelle [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), ce qui crée une réponse HTTP 200 (OK) qui contient le produit.</span><span class="sxs-lookup"><span data-stu-id="59cda-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="59cda-151">Autres Types de retour</span><span class="sxs-lookup"><span data-stu-id="59cda-151">Other Return Types</span></span>

<span data-ttu-id="59cda-152">Pour tous les autres types de retour, les API Web utilise un [formateur média](../formats-and-model-binding/media-formatters.md) pour sérialiser la valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="59cda-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="59cda-153">API Web écrit la valeur sérialisée dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="59cda-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="59cda-154">Le code d’état de réponse est 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="59cda-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="59cda-155">L’inconvénient de cette approche est que vous ne pouvez pas retourner directement un code d’erreur, tels que 404.</span><span class="sxs-lookup"><span data-stu-id="59cda-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="59cda-156">Toutefois, vous pouvez lever une **HttpResponseException** pour les codes d’erreur.</span><span class="sxs-lookup"><span data-stu-id="59cda-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="59cda-157">Pour plus d’informations, consultez [exceptions dans l’API Web ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="59cda-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="59cda-158">API Web utilise l’en-tête Accept dans la demande de choisir le formateur.</span><span class="sxs-lookup"><span data-stu-id="59cda-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="59cda-159">Pour plus d’informations, consultez [négociation de contenu](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="59cda-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="59cda-160">Exemple de demande</span><span class="sxs-lookup"><span data-stu-id="59cda-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="59cda-161">Exemple de réponse :</span><span class="sxs-lookup"><span data-stu-id="59cda-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
