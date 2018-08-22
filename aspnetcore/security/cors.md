---
title: Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme une norme pour autoriser ou rejeter les demandes cross-origin dans une application ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41828985"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="3a0d1-103">Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a0d1-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="3a0d1-104">Par [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3a0d1-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3a0d1-105">La sécurité du navigateur empêche une page web d’effectuer des demandes AJAX vers un autre domaine.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="3a0d1-106">Cette restriction est appelée la *stratégie de même origine* et empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="3a0d1-107">Toutefois, vous pouvez parfois permettre aux autres sites de faire des demandes cross-origin à votre API web.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="3a0d1-108">[Cross-origine partage de ressources](http://www.w3.org/TR/cors/) (CORS) est une norme W3C qui permet à un serveur d’abaisser la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="3a0d1-109">À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="3a0d1-110">CORS est plus sûre et plus flexible que des techniques antérieures telles que [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="3a0d1-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="3a0d1-111">Cette rubrique montre comment activer CORS dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="3a0d1-112">Que veut dire « même origine » ?</span><span class="sxs-lookup"><span data-stu-id="3a0d1-112">What is "same origin"?</span></span>

<span data-ttu-id="3a0d1-113">Deux URL ont la même origine que s’ils ont des ports, des hôtes et des schémas identiques.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="3a0d1-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="3a0d1-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="3a0d1-115">Ces deux URL ayant la même origine :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="3a0d1-116">Ces URL ont des origines différentes aux deux précédentes :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="3a0d1-117">`http://example.net` -Autre domaine</span><span class="sxs-lookup"><span data-stu-id="3a0d1-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="3a0d1-118">`http://www.example.com/foo.html` -Autre sous-domaine</span><span class="sxs-lookup"><span data-stu-id="3a0d1-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="3a0d1-119">`https://example.com/foo.html` -Schéma différent</span><span class="sxs-lookup"><span data-stu-id="3a0d1-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="3a0d1-120">`http://example.com:9000/foo.html` -Autre port</span><span class="sxs-lookup"><span data-stu-id="3a0d1-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="3a0d1-121">Internet Explorer ne considère pas le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="3a0d1-122">Activer CORS</span><span class="sxs-lookup"><span data-stu-id="3a0d1-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="3a0d1-123">Pour configurer des règles CORS pour votre application, ajoutez le package `Microsoft.AspNetCore.Cors` dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="3a0d1-124">Appelez [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3a0d1-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="3a0d1-125">Activation de CORS avec un intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="3a0d1-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="3a0d1-126">Pour activer CORS, ajoutez l’intergiciel (middleware) CORS au pipeline de demande à l’aide de la `UseCors` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="3a0d1-127">L’intergiciel (middleware) CORS doit précéder n’importe quel point de terminaison défini dans votre application où vous souhaitez prendre en charge les demandes cross-origin (par exemple, avant tout appel à `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="3a0d1-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="3a0d1-128">Une stratégie de cross-origin peut être spécifiée lors de l’ajout de l’intergiciel (middleware) CORS à l’aide du [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) classe.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="3a0d1-129">Il existe deux manières de procéder.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-129">There are two ways to do this.</span></span> <span data-ttu-id="3a0d1-130">La première consiste à appeler `UseCors` avec une expression lambda :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="3a0d1-131">**Remarque :** l’URL doit être spécifiée sans barre oblique de fin (`/`).</span><span class="sxs-lookup"><span data-stu-id="3a0d1-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="3a0d1-132">Si l’URL se termine par `/`, la comparaison retournera `false` et aucun en-tête n’est renvoyé.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="3a0d1-133">L’expression lambda prend un objet `CorsPolicyBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="3a0d1-134">Vous trouverez une liste des [options de configuration](#cors-policy-options) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="3a0d1-135">Dans cet exemple, la stratégie autorise les demandes cross-origin de `http://example.com` et aucune autre origine.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="3a0d1-136">CorsPolicyBuilder possède une API fluent, vous pouvez chaîner des appels de méthode :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="3a0d1-137">La deuxième approche consiste à définir un ou plusieurs stratégies CORS nommées, puis sélectionnez la stratégie par nom au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="3a0d1-138">Cet exemple ajoute une stratégie CORS nommée « AllowSpecificOrigin ».</span><span class="sxs-lookup"><span data-stu-id="3a0d1-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="3a0d1-139">Pour sélectionner la stratégie, passez le nom à `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="3a0d1-140">Activation de CORS dans MVC</span><span class="sxs-lookup"><span data-stu-id="3a0d1-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="3a0d1-141">Vous pouvez également utiliser MVC pour appliquer des CORS spécifiques par action, par contrôleur, ou pour tous les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="3a0d1-142">Lors de l’utilisation de MVC pour activer CORS, les mêmes services CORS sont employés, mais pas l’intergiciel (middleware) CORS</span><span class="sxs-lookup"><span data-stu-id="3a0d1-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="3a0d1-143">Par action</span><span class="sxs-lookup"><span data-stu-id="3a0d1-143">Per action</span></span>

<span data-ttu-id="3a0d1-144">Afin de spécifier une stratégie CORS pour une action spécifique, ajoutez l'attribut `[EnableCors]` à l’action.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="3a0d1-145">Spécifiez le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="3a0d1-146">Par contrôleur</span><span class="sxs-lookup"><span data-stu-id="3a0d1-146">Per controller</span></span>

<span data-ttu-id="3a0d1-147">Afin de spécifier la stratégie CORS pour un contrôleur spécifique, ajoutez l'attribut `[EnableCors]` à la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="3a0d1-148">Spécifiez le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="3a0d1-149">Dans le monde entier</span><span class="sxs-lookup"><span data-stu-id="3a0d1-149">Globally</span></span>

<span data-ttu-id="3a0d1-150">Vous pouvez activer CORS globalement pour tous les contrôleurs en ajoutant le filtre `CorsAuthorizationFilterFactory`à la collection de filtres globaux :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="3a0d1-151">L’ordre de priorité est : Action, contrôleur, global.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="3a0d1-152">Les stratégies au niveau de l’action sont prioritaires sur les stratégies au niveau du contrôleur et les stratégies au niveau du contrôleur sont prioritaires sur les stratégies globales.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="3a0d1-153">Désactiver CORS</span><span class="sxs-lookup"><span data-stu-id="3a0d1-153">Disable CORS</span></span>

<span data-ttu-id="3a0d1-154">Pour désactiver les CORS pour un contrôleur ou une action, utilisez l'attribut `[DisableCors]`.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="3a0d1-155">Options de stratégie CORS</span><span class="sxs-lookup"><span data-stu-id="3a0d1-155">CORS policy options</span></span>

<span data-ttu-id="3a0d1-156">Cette section décrit les différentes options que vous pouvez définir dans une stratégie CORS.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="3a0d1-157">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="3a0d1-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="3a0d1-158">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="3a0d1-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="3a0d1-159">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="3a0d1-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="3a0d1-160">Définir les en-têtes de réponse exposé</span><span class="sxs-lookup"><span data-stu-id="3a0d1-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="3a0d1-161">Informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="3a0d1-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="3a0d1-162">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="3a0d1-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="3a0d1-163">Pour certaines options, il peut être utile de lire [CORS comment fonctionne](#how-cors-works) première.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="3a0d1-164">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="3a0d1-164">Set the allowed origins</span></span>

<span data-ttu-id="3a0d1-165">Pour autoriser un ou plusieurs origines spécifiques :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="3a0d1-166">Pour autoriser toutes les origines :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="3a0d1-167">Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="3a0d1-168">Cela signifie que tout site Web peut effectuer des appels d’AJAX à votre API.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="3a0d1-169">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="3a0d1-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="3a0d1-170">Pour autoriser toutes les méthodes HTTP :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="3a0d1-171">Cela affecte les demandes en amont et en-tête de Access-Control-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="3a0d1-172">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="3a0d1-172">Set the allowed request headers</span></span>

<span data-ttu-id="3a0d1-173">Une demande préliminaire CORS peut inclure un en-tête Access-Control-Request-Headers, qui répertorie les en-têtes HTTP définis par l’application (appelés « en-têtes de demande d’auteur »).</span><span class="sxs-lookup"><span data-stu-id="3a0d1-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="3a0d1-174">À la liste verte des en-têtes spécifiques :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="3a0d1-175">Pour autoriser tous les en-têtes de demande d'auteur:</span><span class="sxs-lookup"><span data-stu-id="3a0d1-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="3a0d1-176">Les navigateurs ne sont pas entièrement cohérents dans leur définition Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="3a0d1-177">Si vous définissez les en-têtes sur tout élément autre que « \* », vous devez inclure au moins « accept », « content-type » et « origin », ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="3a0d1-178">Définir les en-têtes de réponse exposé</span><span class="sxs-lookup"><span data-stu-id="3a0d1-178">Set the exposed response headers</span></span>

<span data-ttu-id="3a0d1-179">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="3a0d1-180">(Consultez [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Les en-têtes de réponse sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="3a0d1-181">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="3a0d1-181">Cache-Control</span></span>

* <span data-ttu-id="3a0d1-182">Content-Language</span><span class="sxs-lookup"><span data-stu-id="3a0d1-182">Content-Language</span></span>

* <span data-ttu-id="3a0d1-183">Content-Type</span><span class="sxs-lookup"><span data-stu-id="3a0d1-183">Content-Type</span></span>

* <span data-ttu-id="3a0d1-184">Arrive à expiration</span><span class="sxs-lookup"><span data-stu-id="3a0d1-184">Expires</span></span>

* <span data-ttu-id="3a0d1-185">Dernière modification</span><span class="sxs-lookup"><span data-stu-id="3a0d1-185">Last-Modified</span></span>

* <span data-ttu-id="3a0d1-186">pragma</span><span class="sxs-lookup"><span data-stu-id="3a0d1-186">Pragma</span></span>

<span data-ttu-id="3a0d1-187">La spécification CORS appelle ces en-têtes : *les en-têtes de réponse simple*.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="3a0d1-188">Pour rendre d’autres en-têtes accessibles à l’application :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="3a0d1-189">Informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="3a0d1-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="3a0d1-190">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="3a0d1-191">Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="3a0d1-192">Les informations d’identification incluent les cookies, ainsi que des schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="3a0d1-193">Pour envoyer des informations d’identification avec une demande cross-origin, le client doit définir la propriété `XMLHttpRequest.withCredentials` sur true.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="3a0d1-194">À l’aide de XMLHttpRequest directement :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="3a0d1-195">Dans jQuery :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="3a0d1-196">En outre, le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="3a0d1-197">Pour autoriser les informations d’identification de cross-origine :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="3a0d1-198">La réponse HTTP inclut désormais un en-tête Access-Control-Allow-Credentials, lequel indique au navigateur que le serveur autorise les informations d’identification pour une demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="3a0d1-199">Si le navigateur envoie des informations d’identification, mais que la réponse n’inclut aucun en-tête Access-contrôle-Allow-Credentials valide, le navigateur n'exposera pas la réponse à l’application et la requête AJAX échouera.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="3a0d1-200">Soyez prudent lorsque vous autorisez les informations d’identification cross-origin.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="3a0d1-201">Un site web à un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à l’application de la part de l’utilisateur sans que celui-ci le sache.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="3a0d1-202">La spécification CORS indique également ce paramètre pour les origines `"*"` (toutes les origines) n’est pas valide si le `Access-Control-Allow-Credentials` en-tête est présent.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="3a0d1-203">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="3a0d1-203">Set the preflight expiration time</span></span>

<span data-ttu-id="3a0d1-204">L’en-tête `Access-contrôle-Max-Age` spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mise en cache. Pour définir cet en-tête :
</span><span class="sxs-lookup"><span data-stu-id="3a0d1-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="3a0d1-205">Pour définir cet en-tête :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="3a0d1-206">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="3a0d1-206">How CORS works</span></span>

<span data-ttu-id="3a0d1-207">Cette section décrit ce qui se passe dans une demande CORS au niveau des messages HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="3a0d1-208">Il est important de comprendre le fonctionnement de CORS afin que la stratégie CORS peut être configurée correctement et de débogage lorsque des comportements inattendus se produisent.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="3a0d1-209">La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="3a0d1-210">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="3a0d1-211">Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="3a0d1-212">Voici un exemple d’une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="3a0d1-213">Le `Origin` en-tête fournit le domaine du site qui effectue la demande :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="3a0d1-214">Si le serveur autorise la demande, il définit l’en-tête `Access-Control-Allow-Origin` dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="3a0d1-215">La valeur de cet en-tête correspond à l’en-tête Origin de la demande ou à la valeur de caractère générique « \* », ce qui signifie que toute origine est autorisée :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="3a0d1-216">Si la réponse n’inclut pas l’en-tête `Access-Control-Allow-Origin`, la requête AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="3a0d1-217">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="3a0d1-218">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible à l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="3a0d1-219">Demandes préliminaires</span><span class="sxs-lookup"><span data-stu-id="3a0d1-219">Preflight Requests</span></span>

<span data-ttu-id="3a0d1-220">Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire, appelée « demande préliminaire, » avant d’envoyer la demande réelle de la ressource.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="3a0d1-221">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="3a0d1-222">La méthode de demande est GET, HEAD ou POST</span><span class="sxs-lookup"><span data-stu-id="3a0d1-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="3a0d1-223">L’application ne définit aucun en-tête de demande autre que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type` ou Last-Event-ID, et</span><span class="sxs-lookup"><span data-stu-id="3a0d1-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="3a0d1-224">L’en-tête `Content-Type` (si défini) est une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="3a0d1-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="3a0d1-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="3a0d1-226">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="3a0d1-226">multipart/form-data</span></span>

  * <span data-ttu-id="3a0d1-227">texte/brut</span><span class="sxs-lookup"><span data-stu-id="3a0d1-227">text/plain</span></span>

<span data-ttu-id="3a0d1-228">La règle sur les en-têtes de demande s’applique aux en-têtes que l’application définit en appelant `setRequestHeader` sur l’objet XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="3a0d1-229">(La spécification CORS les appelle « en-têtes de demande auteur ».) La règle ne s’applique pas aux en-têtes que le navigateur peut définir, telles que User-Agent, Host ou Content-Length.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="3a0d1-230">Voici un exemple d’une requête préliminaire :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-230">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="3a0d1-231">La demande de contrôle préliminaire utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="3a0d1-232">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-232">It includes two special headers:</span></span>

* <span data-ttu-id="3a0d1-233">Access-Control-Request-Method : Méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="3a0d1-234">Access-Control-Request-Headers : Liste des en-têtes de demande que l’application est définie sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="3a0d1-235">(Là encore, cela n’inclut pas les en-têtes qui définit le navigateur.)</span><span class="sxs-lookup"><span data-stu-id="3a0d1-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="3a0d1-236">Voici un exemple de réponse, en supposant que le serveur autorise la demande :</span><span class="sxs-lookup"><span data-stu-id="3a0d1-236">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="3a0d1-237">La réponse inclut un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="3a0d1-238">Si la demande préliminaire réussit, le navigateur envoie la demande réelle, comme décrit précédemment.</span><span class="sxs-lookup"><span data-stu-id="3a0d1-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
