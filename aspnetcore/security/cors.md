---
title: Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme une norme pour autoriser ou rejeter les demandes cross-origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045586"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="c6540-103">Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6540-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="c6540-104">Par [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c6540-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c6540-105">Sécurité des navigateurs empêche une page web d’effectuer des requêtes vers un autre domaine que celui qui a servi la page web.</span><span class="sxs-lookup"><span data-stu-id="c6540-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="c6540-106">Cette restriction est appelée le *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="c6540-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="c6540-107">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="c6540-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="c6540-108">Parfois, vous souhaiterez autoriser de qu'autres sites apporter les demandes cross-origin à votre application.</span><span class="sxs-lookup"><span data-stu-id="c6540-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="c6540-109">[Cross-origine partage de ressources](https://www.w3.org/TR/cors/) (CORS) est une norme W3C qui permet à un serveur d’abaisser la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="c6540-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="c6540-110">À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres.</span><span class="sxs-lookup"><span data-stu-id="c6540-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="c6540-111">CORS est plus sûre et plus flexible que les techniques précédentes, telles que [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="c6540-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="c6540-112">Cette rubrique montre comment activer CORS dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6540-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="c6540-113">Même origine</span><span class="sxs-lookup"><span data-stu-id="c6540-113">Same origin</span></span>

<span data-ttu-id="c6540-114">Deux URL ont la même origine que s’ils ont des schémas identiques, les hôtes et les ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="c6540-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="c6540-115">Ces deux URL ayant la même origine :</span><span class="sxs-lookup"><span data-stu-id="c6540-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="c6540-116">Ces URL ont différentes origines que les deux URL précédentes :</span><span class="sxs-lookup"><span data-stu-id="c6540-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="c6540-117">`https://example.net` &ndash; Autre domaine</span><span class="sxs-lookup"><span data-stu-id="c6540-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="c6540-118">`https://www.example.com/foo.html` &ndash; Autre sous-domaine</span><span class="sxs-lookup"><span data-stu-id="c6540-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="c6540-119">`http://example.com/foo.html` &ndash; Schéma différent</span><span class="sxs-lookup"><span data-stu-id="c6540-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="c6540-120">`https://example.com:9000/foo.html` &ndash; Port différent</span><span class="sxs-lookup"><span data-stu-id="c6540-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="c6540-121">Internet Explorer ne considère pas le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="c6540-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="c6540-122">Inscrire les services CORS</span><span class="sxs-lookup"><span data-stu-id="c6540-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c6540-123">Référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) ou ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span><span class="sxs-lookup"><span data-stu-id="c6540-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c6540-124">Référence le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ou ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span><span class="sxs-lookup"><span data-stu-id="c6540-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c6540-125">Ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span><span class="sxs-lookup"><span data-stu-id="c6540-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="c6540-126">Appelez <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> dans `Startup.ConfigureServices` pour ajouter des services CORS au conteneur de service de l’application :</span><span class="sxs-lookup"><span data-stu-id="c6540-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="c6540-127">Activer CORS</span><span class="sxs-lookup"><span data-stu-id="c6540-127">Enable CORS</span></span>

<span data-ttu-id="c6540-128">Après avoir inscrit des services CORS, utilisez une des approches suivantes pour activer CORS dans une application ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="c6540-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="c6540-129">[Intergiciel (middleware) CORS](#enable-cors-with-cors-middleware) &ndash; stratégies CORS s’appliquent globalement à l’application via l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="c6540-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="c6540-130">[CORS dans MVC](#enable-cors-in-mvc) &ndash; stratégies CORS s’appliquent par action ou par le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c6540-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="c6540-131">Intergiciel (middleware) CORS n’est pas utilisé.</span><span class="sxs-lookup"><span data-stu-id="c6540-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="c6540-132">Activer CORS avec un intergiciel (middleware) CORS</span><span class="sxs-lookup"><span data-stu-id="c6540-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="c6540-133">Intergiciel (middleware) CORS gère les demandes cross-origin à l’application.</span><span class="sxs-lookup"><span data-stu-id="c6540-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="c6540-134">Pour activer l’intergiciel (middleware) CORS dans le pipeline de traitement de la demande, appelez le <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> méthode d’extension dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c6540-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="c6540-135">Intergiciel (middleware) CORS doivent précéder n’importe quel point de terminaison défini dans votre application où vous souhaitez prendre en charge les demandes cross-origin (par exemple, avant l’appel à `UseMvc` intergiciel (middleware) de MVC/Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="c6540-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="c6540-136">Un *stratégie multi-origines* peut être spécifié lors de l’ajout de l’intergiciel (middleware) CORS à l’aide de la <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> classe.</span><span class="sxs-lookup"><span data-stu-id="c6540-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="c6540-137">Il existe deux approches permettant de définir une stratégie CORS :</span><span class="sxs-lookup"><span data-stu-id="c6540-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="c6540-138">Appelez `UseCors` avec une expression lambda :</span><span class="sxs-lookup"><span data-stu-id="c6540-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="c6540-139">L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c6540-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="c6540-140">[Options de configuration](#cors-policy-options), tel que `WithOrigins`, sont décrits plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="c6540-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="c6540-141">Dans l’exemple précédent, la stratégie autorise les demandes cross-origin à partir de `https://example.com` et aucune autre origine.</span><span class="sxs-lookup"><span data-stu-id="c6540-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="c6540-142">L’URL doit être spécifié sans barre oblique de fin (`/`).</span><span class="sxs-lookup"><span data-stu-id="c6540-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="c6540-143">Si l’URL se termine avec `/`, la comparaison retourne `false` et aucun en-tête n’est retourné.</span><span class="sxs-lookup"><span data-stu-id="c6540-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="c6540-144">`CorsPolicyBuilder` possède une API fluent, vous pouvez chaîner des appels de méthode :</span><span class="sxs-lookup"><span data-stu-id="c6540-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="c6540-145">Définir une ou plusieurs stratégies CORS nommées et sélectionnez la stratégie par nom lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="c6540-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="c6540-146">L’exemple suivant ajoute une stratégie CORS défini par l’utilisateur nommée *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="c6540-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="c6540-147">Pour sélectionner la stratégie, passez le nom à `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="c6540-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="c6540-148">Activer CORS dans MVC</span><span class="sxs-lookup"><span data-stu-id="c6540-148">Enable CORS in MVC</span></span>

<span data-ttu-id="c6540-149">Vous pouvez également utiliser MVC pour appliquer des stratégies CORS spécifiques par action ou par le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c6540-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="c6540-150">Lorsque vous utilisez MVC pour activer CORS, les services CORS inscrits sont utilisés.</span><span class="sxs-lookup"><span data-stu-id="c6540-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="c6540-151">L’intergiciel (middleware) CORS n’est pas utilisé.</span><span class="sxs-lookup"><span data-stu-id="c6540-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="c6540-152">Par action</span><span class="sxs-lookup"><span data-stu-id="c6540-152">Per action</span></span>

<span data-ttu-id="c6540-153">Pour spécifier une stratégie CORS pour une action spécifique, ajoutez le [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) d’attribut à l’action.</span><span class="sxs-lookup"><span data-stu-id="c6540-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="c6540-154">Spécifiez le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="c6540-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="c6540-155">Par contrôleur</span><span class="sxs-lookup"><span data-stu-id="c6540-155">Per controller</span></span>

<span data-ttu-id="c6540-156">Pour spécifier la stratégie CORS pour un contrôleur spécifique, ajoutez le [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) d’attribut à la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c6540-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="c6540-157">Spécifiez le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="c6540-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="c6540-158">L’ordre de priorité est :</span><span class="sxs-lookup"><span data-stu-id="c6540-158">The precedence order is:</span></span>

1. <span data-ttu-id="c6540-159">action</span><span class="sxs-lookup"><span data-stu-id="c6540-159">action</span></span>
1. <span data-ttu-id="c6540-160">contrôleur</span><span class="sxs-lookup"><span data-stu-id="c6540-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="c6540-161">Désactiver CORS</span><span class="sxs-lookup"><span data-stu-id="c6540-161">Disable CORS</span></span>

<span data-ttu-id="c6540-162">Pour désactiver CORS pour un contrôleur ou d’action, utilisez la [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribut :</span><span class="sxs-lookup"><span data-stu-id="c6540-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="c6540-163">Options de stratégie CORS</span><span class="sxs-lookup"><span data-stu-id="c6540-163">CORS policy options</span></span>

<span data-ttu-id="c6540-164">Cette section décrit les différentes options que vous pouvez définir dans une stratégie CORS.</span><span class="sxs-lookup"><span data-stu-id="c6540-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="c6540-165">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="c6540-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="c6540-166">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="c6540-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="c6540-167">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="c6540-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="c6540-168">Définir les en-têtes de réponse exposé</span><span class="sxs-lookup"><span data-stu-id="c6540-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="c6540-169">Informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="c6540-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="c6540-170">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="c6540-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="c6540-171">Pour certaines options, il peut être utile de lire le [CORS comment fonctionne](#how-cors-works) section tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="c6540-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="c6540-172">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="c6540-172">Set the allowed origins</span></span>

<span data-ttu-id="c6540-173">Pour permettre à un ou plusieurs origines spécifiques, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-173">To allow one or more specific origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

<span data-ttu-id="c6540-174">Pour autoriser toutes les origines, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-174">To allow all origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="c6540-175">Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine.</span><span class="sxs-lookup"><span data-stu-id="c6540-175">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="c6540-176">Autoriser les demandes à partir de toute origine signifie que *n’importe quel site Web* peut apporter les demandes cross-origin à votre application.</span><span class="sxs-lookup"><span data-stu-id="c6540-176">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

<span data-ttu-id="c6540-177">Ce paramètre affecte [préalable des demandes et l’en-tête Access-Control-Allow-Origin](#preflight-requests) (décrite plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="c6540-177">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="c6540-178">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="c6540-178">Set the allowed HTTP methods</span></span>

<span data-ttu-id="c6540-179">Pour autoriser toutes les méthodes HTTP, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-179">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="c6540-180">Ce paramètre affecte [préalable des demandes et l’en-tête Access-Control-Allow-Methods](#preflight-requests) (décrite plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="c6540-180">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="c6540-181">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="c6540-181">Set the allowed request headers</span></span>

<span data-ttu-id="c6540-182">Pour autoriser des en-têtes spécifiques à envoyer dans une demande CORS, appelé *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="c6540-182">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="c6540-183">Pour autoriser tous les créer des en-têtes de demande, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-183">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="c6540-184">Ce paramètre affecte [préalable des demandes et l’en-tête Access-Control-Request-Headers](#preflight-requests) (décrite plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="c6540-184">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c6540-185">Une recherche de correspondance de stratégie intergiciel (middleware) CORS spécifiée par des en-têtes spécifiques `WithHeaders` n’est possible que lorsque les en-têtes envoyés `Access-Control-Request-Headers` correspondre exactement les en-têtes indiqués dans `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="c6540-185">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="c6540-186">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="c6540-186">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="c6540-187">Intergiciel (middleware) CORS refuse une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) n’est pas répertoriée dans `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="c6540-187">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="c6540-188">L’application retourne un *200 OK* réponse mais n’envoie pas les en-têtes CORS précédent.</span><span class="sxs-lookup"><span data-stu-id="c6540-188">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="c6540-189">Par conséquent, le navigateur ne tente pas la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="c6540-189">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c6540-190">Intergiciel (middleware) CORS permet toujours quatre en-têtes dans le `Access-Control-Request-Headers` à envoyer, indépendamment des valeurs configurées dans CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="c6540-190">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="c6540-191">Cette liste d’en-têtes comprend :</span><span class="sxs-lookup"><span data-stu-id="c6540-191">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="c6540-192">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="c6540-192">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="c6540-193">Intergiciel (middleware) CORS répond correctement à une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` est toujours dans la liste verte :</span><span class="sxs-lookup"><span data-stu-id="c6540-193">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="c6540-194">Définir les en-têtes de réponse exposé</span><span class="sxs-lookup"><span data-stu-id="c6540-194">Set the exposed response headers</span></span>

<span data-ttu-id="c6540-195">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="c6540-195">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="c6540-196">Pour plus d’informations, consultez [W3C Cross-Origin Resource Sharing (terminologie) : en-tête de réponse Simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="c6540-196">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="c6540-197">Les en-têtes de réponse sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="c6540-197">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="c6540-198">La spécification CORS appelle ces en-têtes *les en-têtes de réponse simple*.</span><span class="sxs-lookup"><span data-stu-id="c6540-198">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="c6540-199">Pour les autres en-têtes disponibles pour l’application, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-199">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="c6540-200">Informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="c6540-200">Credentials in cross-origin requests</span></span>

<span data-ttu-id="c6540-201">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="c6540-201">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="c6540-202">Par défaut, le navigateur n’envoie pas les informations d’identification avec une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="c6540-202">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="c6540-203">Informations d’identification incluent les cookies et les schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6540-203">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="c6540-204">Pour envoyer des informations d’identification avec une demande de cross-origin, le client doit définir `XMLHttpRequest.withCredentials` à `true`.</span><span class="sxs-lookup"><span data-stu-id="c6540-204">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="c6540-205">À l’aide de `XMLHttpRequest` directement :</span><span class="sxs-lookup"><span data-stu-id="c6540-205">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="c6540-206">Dans jQuery :</span><span class="sxs-lookup"><span data-stu-id="c6540-206">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="c6540-207">En outre, le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="c6540-207">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="c6540-208">Pour autoriser les informations d’identification de cross-origin, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-208">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="c6540-209">La réponse HTTP inclut un `Access-Control-Allow-Credentials` en-tête, qui indique au navigateur que le serveur autorise les informations d’identification pour une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="c6540-209">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="c6540-210">Si le navigateur envoie les informations d’identification, mais la réponse n’inclut pas une valide `Access-Control-Allow-Credentials` en-tête, le navigateur n’expose pas la réponse à l’application, et la demande cross-origin échoue.</span><span class="sxs-lookup"><span data-stu-id="c6540-210">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="c6540-211">Soyez prudent lorsque vous autorisez les informations d’identification cross-origin.</span><span class="sxs-lookup"><span data-stu-id="c6540-211">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="c6540-212">Un site Web à un autre domaine peut envoyer des informations d’identification d’un utilisateur de connecté à l’application sur l’utilisateur sans avoir connaissance de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c6540-212">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="c6540-213">La spécification CORS indique également ce paramètre pour les origines `"*"` (toutes les origines) n’est pas valide si le `Access-Control-Allow-Credentials` en-tête est présent.</span><span class="sxs-lookup"><span data-stu-id="c6540-213">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="c6540-214">Demandes préliminaires</span><span class="sxs-lookup"><span data-stu-id="c6540-214">Preflight requests</span></span>

<span data-ttu-id="c6540-215">Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="c6540-215">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="c6540-216">Cette requête est appelée un *demande préliminaire*.</span><span class="sxs-lookup"><span data-stu-id="c6540-216">This request is called a *preflight request*.</span></span> <span data-ttu-id="c6540-217">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="c6540-217">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="c6540-218">La méthode de demande est GET, HEAD ou POST.</span><span class="sxs-lookup"><span data-stu-id="c6540-218">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="c6540-219">L’application ne définit pas les en-têtes de demande autre que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, ou `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="c6540-219">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="c6540-220">Le `Content-Type` en-tête, si définis, dispose d’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="c6540-220">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="c6540-221">La règle sur les en-têtes de requête définie pour la demande du client s’applique aux en-têtes de l’application définit en appelant `setRequestHeader` sur la `XMLHttpRequest` objet.</span><span class="sxs-lookup"><span data-stu-id="c6540-221">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="c6540-222">La spécification CORS appelle ces en-têtes *créer des en-têtes de demande*.</span><span class="sxs-lookup"><span data-stu-id="c6540-222">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="c6540-223">La règle ne s’applique pas aux en-têtes le navigateur peut définir comme `User-Agent`, `Host`, ou `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="c6540-223">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="c6540-224">Voici un exemple d’une requête préliminaire :</span><span class="sxs-lookup"><span data-stu-id="c6540-224">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="c6540-225">La demande de contrôle préliminaire utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="c6540-225">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="c6540-226">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="c6540-226">It includes two special headers:</span></span>

* <span data-ttu-id="c6540-227">`Access-Control-Request-Method`: La méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="c6540-227">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="c6540-228">`Access-Control-Request-Headers`: Liste des en-têtes de demande que l’application définit sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="c6540-228">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="c6540-229">Comme indiqué précédemment, cela n’inclut pas les en-têtes qui définit le navigateur, telles que `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="c6540-229">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="c6540-230">Une demande préliminaire CORS peut inclure un `Access-Control-Request-Headers` en-tête, qui indique au serveur les en-têtes qui sont envoyées avec la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="c6540-230">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="c6540-231">Pour autoriser des en-têtes spécifiques, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-231">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="c6540-232">Pour autoriser tous les créer des en-têtes de demande, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-232">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="c6540-233">Navigateurs ne sont pas entièrement cohérents dans la façon dont elles définies `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="c6540-233">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="c6540-234">Si vous définissez les en-têtes sur n’importe quelle autre que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`, et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="c6540-234">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="c6540-235">Voici un exemple de réponse à la demande préliminaire (en supposant que le serveur autorise la demande) :</span><span class="sxs-lookup"><span data-stu-id="c6540-235">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="c6540-236">La réponse inclut un `Access-Control-Allow-Methods` en-tête qui répertorie les méthodes autorisées et éventuellement un `Access-Control-Allow-Headers` en-tête qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="c6540-236">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="c6540-237">Si la demande préliminaire réussit, le navigateur envoie la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="c6540-237">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="c6540-238">Si la demande préliminaire est refusée, l’application retourne un *200 OK* réponse mais n’envoie pas les en-têtes CORS précédent.</span><span class="sxs-lookup"><span data-stu-id="c6540-238">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="c6540-239">Par conséquent, le navigateur ne tente pas la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="c6540-239">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="c6540-240">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="c6540-240">Set the preflight expiration time</span></span>

<span data-ttu-id="c6540-241">Le `Access-Control-Max-Age` en-tête spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mis en cache.</span><span class="sxs-lookup"><span data-stu-id="c6540-241">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="c6540-242">Pour définir cet en-tête, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="c6540-242">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="c6540-243">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="c6540-243">How CORS works</span></span>

<span data-ttu-id="c6540-244">Cette section décrit ce qui se passe dans une demande CORS au niveau des messages HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6540-244">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="c6540-245">Il est important de comprendre le fonctionnement de CORS afin que la stratégie CORS peut être configurée correctement et de débogage lorsque des comportements inattendus se produisent.</span><span class="sxs-lookup"><span data-stu-id="c6540-245">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="c6540-246">La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="c6540-246">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="c6540-247">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="c6540-247">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="c6540-248">Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.</span><span class="sxs-lookup"><span data-stu-id="c6540-248">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="c6540-249">Voici un exemple d’une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="c6540-249">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="c6540-250">Le `Origin` en-tête fournit le domaine du site qui effectue la demande :</span><span class="sxs-lookup"><span data-stu-id="c6540-250">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="c6540-251">Si le serveur autorise la demande, il définit le `Access-Control-Allow-Origin` en-tête dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="c6540-251">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="c6540-252">La valeur de cet en-tête correspond soit à la `Origin` en-tête à partir de la demande, soit la valeur de caractère générique `"*"`, ce qui signifie que toute origine est autorisée :</span><span class="sxs-lookup"><span data-stu-id="c6540-252">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="c6540-253">Si la réponse n’inclut pas le `Access-Control-Allow-Origin` en-tête, la demande cross-origin échoue.</span><span class="sxs-lookup"><span data-stu-id="c6540-253">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="c6540-254">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="c6540-254">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="c6540-255">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible dans l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="c6540-255">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6540-256">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c6540-256">Additional resources</span></span>

* [<span data-ttu-id="c6540-257">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="c6540-257">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
