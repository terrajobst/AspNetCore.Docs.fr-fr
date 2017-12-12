---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: "Test unitaire contrôleurs dans ASP.NET Web API 2 | Documents Microsoft"
author: MikeWasson
description: "Cette rubrique décrit certaines techniques spécifiques pour l’unité de contrôleurs de test dans l’API Web 2. Avant de lire cette rubrique, vous pouvez souhaiter lire le didacticiel unité..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 167cd24d27977c3652f6a8903054654f5edf7756
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="86f16-104">Test unitaire contrôleurs dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="86f16-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="86f16-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="86f16-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="86f16-106">Cette rubrique décrit certaines techniques spécifiques pour l’unité de contrôleurs de test dans l’API Web 2.</span><span class="sxs-lookup"><span data-stu-id="86f16-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="86f16-107">Avant de lire cette rubrique, vous pouvez souhaiter lire le didacticiel [unité test ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), qui montre comment ajouter un projet de test unitaire à votre solution.</span><span class="sxs-lookup"><span data-stu-id="86f16-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="86f16-108">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="86f16-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="86f16-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="86f16-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="86f16-110">API Web 2</span><span class="sxs-lookup"><span data-stu-id="86f16-110">Web API 2</span></span>
> - <span data-ttu-id="86f16-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="86f16-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="86f16-112">J’ai utilisé Moq, mais le même principe s’applique à n’importe quelle infrastructure factices.</span><span class="sxs-lookup"><span data-stu-id="86f16-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="86f16-113">Moq 4.5.30 (et versions ultérieures) prend en charge de Visual Studio 2017, Roslyn et .NET 4.5 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="86f16-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="86f16-114">Il est courant dans les tests unitaires &quot;réorganiser act-assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="86f16-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="86f16-115">Réorganiser : Configurer les composants requis pour le test à exécuter.</span><span class="sxs-lookup"><span data-stu-id="86f16-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="86f16-116">ACT : Effectuer le test.</span><span class="sxs-lookup"><span data-stu-id="86f16-116">Act: Perform the test.</span></span>
- <span data-ttu-id="86f16-117">Assertion : Vérifiez que le test a réussi.</span><span class="sxs-lookup"><span data-stu-id="86f16-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="86f16-118">Dans l’étape d’organisation, vous souvent utiliser fictifs ou objets du stub.</span><span class="sxs-lookup"><span data-stu-id="86f16-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="86f16-119">Qui réduit le nombre de dépendances, donc le test se concentre sur un point de test.</span><span class="sxs-lookup"><span data-stu-id="86f16-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="86f16-120">Voici quelques éléments que vous devez le test unitaire sur vos contrôleurs de l’API Web :</span><span class="sxs-lookup"><span data-stu-id="86f16-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="86f16-121">L’action retourne le type de réponse approprié.</span><span class="sxs-lookup"><span data-stu-id="86f16-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="86f16-122">Paramètres non valides renvoient la réponse d’erreur approprié.</span><span class="sxs-lookup"><span data-stu-id="86f16-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="86f16-123">L’action appelle la méthode correcte sur la couche du référentiel ou un service.</span><span class="sxs-lookup"><span data-stu-id="86f16-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="86f16-124">Si la réponse inclut un modèle de domaine, vérifiez le type de modèle.</span><span class="sxs-lookup"><span data-stu-id="86f16-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="86f16-125">Voici quelques-unes des choses générales pour tester, mais les spécificités dépendent de votre implémentation de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="86f16-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="86f16-126">En particulier, il fait une grande différence si vos actions de contrôleur retournent **HttpResponseMessage** ou **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86f16-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="86f16-127">Pour plus d’informations sur ces types de résultats, consultez [résultats d’Action dans l’Api Web 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="86f16-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="86f16-128">Test des Actions qui retournent des objets HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="86f16-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="86f16-129">Voici un exemple d’un contrôleur dont retour actions **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="86f16-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="86f16-130">Notez le contrôleur utilise l’injection de dépendances pour injecter un `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="86f16-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="86f16-131">Ainsi, le contrôleur des tests, car vous pouvez injecter un référentiel fictif.</span><span class="sxs-lookup"><span data-stu-id="86f16-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="86f16-132">Le test unitaire suivant vérifie que la `Get` méthode écrit un `Product` dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="86f16-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="86f16-133">Supposons que `repository` est un fictifs `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="86f16-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="86f16-134">Il est important de définir **demande** et **Configuration** sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="86f16-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="86f16-135">Dans le cas contraire, le test échoue avec une **ArgumentNullException** ou **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="86f16-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="86f16-136">Génération du lien de test</span><span class="sxs-lookup"><span data-stu-id="86f16-136">Testing Link Generation</span></span>

<span data-ttu-id="86f16-137">Le `Post` les appels de méthode **UrlHelper.Link** pour créer des liens dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="86f16-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="86f16-138">Cela nécessite un peu plus de programme d’installation dans le test unitaire :</span><span class="sxs-lookup"><span data-stu-id="86f16-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="86f16-139">Le **UrlHelper** classe a besoin des requête URL et données d’itinéraire, par conséquent, le test a définir des valeurs pour ces.</span><span class="sxs-lookup"><span data-stu-id="86f16-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="86f16-140">Une autre option consiste fictifs ou stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="86f16-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="86f16-141">Avec cette approche, vous remplacez la valeur par défaut [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) avec une version fictifs ou stub qui retourne une valeur fixe.</span><span class="sxs-lookup"><span data-stu-id="86f16-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="86f16-142">Nous allons réécrire le test à l’aide de la [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="86f16-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="86f16-143">Installer le `Moq` package NuGet dans le projet de test.</span><span class="sxs-lookup"><span data-stu-id="86f16-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="86f16-144">Dans cette version, vous n’avez pas besoin de configurer les données d’itinéraire, car la fictifs **UrlHelper** retourne une chaîne constante.</span><span class="sxs-lookup"><span data-stu-id="86f16-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="86f16-145">Test des Actions qui retournent IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="86f16-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="86f16-146">Dans l’API Web 2, une action de contrôleur peut retourner **IHttpActionResult**, qui est analogue à **ActionResult** dans ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="86f16-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="86f16-147">Le **IHttpActionResult** interface définit un modèle de commande pour créer des réponses HTTP.</span><span class="sxs-lookup"><span data-stu-id="86f16-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="86f16-148">Au lieu de créer directement à la réponse, le contrôleur renvoie un **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86f16-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="86f16-149">Une version ultérieure, le pipeline appelle la **IHttpActionResult** pour créer la réponse.</span><span class="sxs-lookup"><span data-stu-id="86f16-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="86f16-150">Cette approche facilite écrire des tests unitaires, car vous pouvez ignorer le programme d’installation n’est nécessaire pour de nombreuses **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="86f16-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="86f16-151">Voici un exemple controller dont retour actions **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86f16-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="86f16-152">Cet exemple illustre des modèles communs à l’aide de **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86f16-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="86f16-153">Voyons comment à unité les tester.</span><span class="sxs-lookup"><span data-stu-id="86f16-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="86f16-154">Action renvoie 200 (OK) avec un corps de réponse</span><span class="sxs-lookup"><span data-stu-id="86f16-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="86f16-155">Le `Get` les appels de méthode `Ok(product)` si le produit est trouvé.</span><span class="sxs-lookup"><span data-stu-id="86f16-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="86f16-156">Dans le test unitaire, assurez-vous que le type de retour est **OkNegotiatedContentResult** et le produit retourné a l’ID de droite.</span><span class="sxs-lookup"><span data-stu-id="86f16-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="86f16-157">Notez que le test unitaire n’exécute pas le résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="86f16-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="86f16-158">Vous pouvez supposer que le résultat d’action crée la réponse HTTP correctement.</span><span class="sxs-lookup"><span data-stu-id="86f16-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="86f16-159">(C’est pourquoi l’infrastructure API Web a son propre tests unitaires !)</span><span class="sxs-lookup"><span data-stu-id="86f16-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="86f16-160">Action retourne 404 (introuvable)</span><span class="sxs-lookup"><span data-stu-id="86f16-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="86f16-161">Le `Get` les appels de méthode `NotFound()` si le produit est introuvable.</span><span class="sxs-lookup"><span data-stu-id="86f16-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="86f16-162">Dans ce cas, le test unitaire vérifie uniquement si le type de retour est **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="86f16-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="86f16-163">Action renvoie 200 (OK) avec aucun corps de réponse</span><span class="sxs-lookup"><span data-stu-id="86f16-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="86f16-164">Le `Delete` les appels de méthode `Ok()` pour renvoyer une réponse HTTP 200 vide.</span><span class="sxs-lookup"><span data-stu-id="86f16-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="86f16-165">Comme l’exemple précédent, le test unitaire vérifie le type de retour, dans ce cas **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="86f16-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="86f16-166">Action renvoie 201 (créé) avec un en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="86f16-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="86f16-167">Le `Post` les appels de méthode `CreatedAtRoute` pour renvoyer une réponse HTTP 201 avec un URI dans l’en-tête Location.</span><span class="sxs-lookup"><span data-stu-id="86f16-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="86f16-168">Dans le test unitaire, vérifiez que l’action définit les valeurs de routage corrects.</span><span class="sxs-lookup"><span data-stu-id="86f16-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="86f16-169">Action retourne un autre 2xx avec un corps de réponse</span><span class="sxs-lookup"><span data-stu-id="86f16-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="86f16-170">Le `Put` les appels de méthode `Content` pour renvoyer une réponse HTTP 202 (accepté) avec un corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="86f16-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="86f16-171">Ce cas est semblable au retour de 200 (OK), mais le test unitaire doit également vérifier le code d’état.</span><span class="sxs-lookup"><span data-stu-id="86f16-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="86f16-172">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="86f16-172">Additional Resources</span></span>

- [<span data-ttu-id="86f16-173">Simulation d’Entity Framework lorsque l’API Web ASP.NET 2 de tests unitaires</span><span class="sxs-lookup"><span data-stu-id="86f16-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="86f16-174">[Écrivez des tests pour un service ASP.NET Web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (billet de blog de Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="86f16-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="86f16-175">Débogage API Web ASP.NET avec le débogueur d’itinéraires</span><span class="sxs-lookup"><span data-stu-id="86f16-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
