---
title: Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme une norme pour autoriser ou rejeter les demandes cross-origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: security/cors
ms.openlocfilehash: 8e5056b448d47d75272e9394b03ce8a58b05a0f4
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191319"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="da0cb-103">Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="da0cb-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="da0cb-104">Par [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="da0cb-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="da0cb-105">Sécurité des navigateurs empêche une page web d’effectuer des requêtes vers un autre domaine que celui qui a servi la page web.</span><span class="sxs-lookup"><span data-stu-id="da0cb-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="da0cb-106">Cette restriction est appelée le *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="da0cb-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="da0cb-107">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="da0cb-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="da0cb-108">Parfois, vous souhaiterez autoriser de qu'autres sites apporter les demandes cross-origin à votre application.</span><span class="sxs-lookup"><span data-stu-id="da0cb-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="da0cb-109">[Cross-origine partage de ressources](https://www.w3.org/TR/cors/) (CORS) est une norme W3C qui permet à un serveur d’abaisser la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="da0cb-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="da0cb-110">À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres.</span><span class="sxs-lookup"><span data-stu-id="da0cb-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="da0cb-111">CORS est plus sûre et plus flexible que les techniques précédentes, telles que [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="da0cb-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="da0cb-112">Cette rubrique montre comment activer CORS dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="da0cb-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="da0cb-113">Même origine</span><span class="sxs-lookup"><span data-stu-id="da0cb-113">Same origin</span></span>

<span data-ttu-id="da0cb-114">Deux URL ont la même origine que s’ils ont des schémas identiques, les hôtes et les ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="da0cb-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="da0cb-115">Ces deux URL ayant la même origine :</span><span class="sxs-lookup"><span data-stu-id="da0cb-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="da0cb-116">Ces URL ont différentes origines que les deux URL précédentes :</span><span class="sxs-lookup"><span data-stu-id="da0cb-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="da0cb-117">`https://example.net` &ndash; Autre domaine</span><span class="sxs-lookup"><span data-stu-id="da0cb-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="da0cb-118">`https://www.example.com/foo.html` &ndash; Autre sous-domaine</span><span class="sxs-lookup"><span data-stu-id="da0cb-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="da0cb-119">`http://example.com/foo.html` &ndash; Schéma différent</span><span class="sxs-lookup"><span data-stu-id="da0cb-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="da0cb-120">`https://example.com:9000/foo.html` &ndash; Port différent</span><span class="sxs-lookup"><span data-stu-id="da0cb-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="da0cb-121">Internet Explorer ne considère pas le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="da0cb-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="da0cb-122">Inscrire les services CORS</span><span class="sxs-lookup"><span data-stu-id="da0cb-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="da0cb-123">Référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) ou ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span><span class="sxs-lookup"><span data-stu-id="da0cb-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="da0cb-124">Référence le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ou ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span><span class="sxs-lookup"><span data-stu-id="da0cb-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="da0cb-125">Ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span><span class="sxs-lookup"><span data-stu-id="da0cb-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="da0cb-126">Appelez <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> dans `Startup.ConfigureServices` pour ajouter des services CORS au conteneur de service de l’application :</span><span class="sxs-lookup"><span data-stu-id="da0cb-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="da0cb-127">Activer CORS</span><span class="sxs-lookup"><span data-stu-id="da0cb-127">Enable CORS</span></span>

<span data-ttu-id="da0cb-128">Après avoir inscrit des services CORS, utilisez une des approches suivantes pour activer CORS dans une application ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="da0cb-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="da0cb-129">[Intergiciel (middleware) CORS](#enable-cors-with-cors-middleware) &ndash; stratégies CORS s’appliquent globalement à l’application via l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="da0cb-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="da0cb-130">[CORS dans MVC](#enable-cors-in-mvc) &ndash; stratégies CORS s’appliquent par action ou par le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="da0cb-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="da0cb-131">Intergiciel (middleware) CORS n’est pas utilisé.</span><span class="sxs-lookup"><span data-stu-id="da0cb-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="da0cb-132">Activer CORS avec un intergiciel (middleware) CORS</span><span class="sxs-lookup"><span data-stu-id="da0cb-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="da0cb-133">Intergiciel (middleware) CORS gère les demandes cross-origin à l’application.</span><span class="sxs-lookup"><span data-stu-id="da0cb-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="da0cb-134">Pour activer l’intergiciel (middleware) CORS dans le pipeline de traitement de la demande, appelez le <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> méthode d’extension dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="da0cb-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="da0cb-135">Intergiciel (middleware) CORS doivent précéder n’importe quel point de terminaison défini dans votre application où vous souhaitez prendre en charge les demandes cross-origin (par exemple, avant l’appel à `UseMvc` intergiciel (middleware) de MVC/Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="da0cb-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="da0cb-136">Un *stratégie multi-origines* peut être spécifié lors de l’ajout de l’intergiciel (middleware) CORS à l’aide de la <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> classe.</span><span class="sxs-lookup"><span data-stu-id="da0cb-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="da0cb-137">Il existe deux approches permettant de définir une stratégie CORS :</span><span class="sxs-lookup"><span data-stu-id="da0cb-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="da0cb-138">Appelez `UseCors` avec une expression lambda :</span><span class="sxs-lookup"><span data-stu-id="da0cb-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="da0cb-139">L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="da0cb-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="da0cb-140">[Options de configuration](#cors-policy-options), tel que `WithOrigins`, sont décrits plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="da0cb-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="da0cb-141">Dans l’exemple précédent, la stratégie autorise les demandes cross-origin à partir de `https://example.com` et aucune autre origine.</span><span class="sxs-lookup"><span data-stu-id="da0cb-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="da0cb-142">L’URL doit être spécifié sans barre oblique de fin (`/`).</span><span class="sxs-lookup"><span data-stu-id="da0cb-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="da0cb-143">Si l’URL se termine avec `/`, la comparaison retourne `false` et aucun en-tête n’est retourné.</span><span class="sxs-lookup"><span data-stu-id="da0cb-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="da0cb-144">`CorsPolicyBuilder` possède une API fluent, vous pouvez chaîner des appels de méthode :</span><span class="sxs-lookup"><span data-stu-id="da0cb-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="da0cb-145">Définir une ou plusieurs stratégies CORS nommées et sélectionnez la stratégie par nom lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="da0cb-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="da0cb-146">L’exemple suivant ajoute une stratégie CORS défini par l’utilisateur nommée *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="da0cb-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="da0cb-147">Pour sélectionner la stratégie, passez le nom à `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="da0cb-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="da0cb-148">Activer CORS dans MVC</span><span class="sxs-lookup"><span data-stu-id="da0cb-148">Enable CORS in MVC</span></span>

<span data-ttu-id="da0cb-149">Vous pouvez également utiliser MVC pour appliquer des stratégies CORS spécifiques par action ou par le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="da0cb-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="da0cb-150">Lorsque vous utilisez MVC pour activer CORS, les services CORS inscrits sont utilisés.</span><span class="sxs-lookup"><span data-stu-id="da0cb-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="da0cb-151">L’intergiciel (middleware) CORS n’est pas utilisé.</span><span class="sxs-lookup"><span data-stu-id="da0cb-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="da0cb-152">Par action</span><span class="sxs-lookup"><span data-stu-id="da0cb-152">Per action</span></span>

<span data-ttu-id="da0cb-153">Pour spécifier une stratégie CORS pour une action spécifique, ajoutez le [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) d’attribut à l’action.</span><span class="sxs-lookup"><span data-stu-id="da0cb-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="da0cb-154">Spécifiez le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="da0cb-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="da0cb-155">Par contrôleur</span><span class="sxs-lookup"><span data-stu-id="da0cb-155">Per controller</span></span>

<span data-ttu-id="da0cb-156">Pour spécifier la stratégie CORS pour un contrôleur spécifique, ajoutez le [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) d’attribut à la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="da0cb-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="da0cb-157">Spécifiez le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="da0cb-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="da0cb-158">L’ordre de priorité est :</span><span class="sxs-lookup"><span data-stu-id="da0cb-158">The precedence order is:</span></span>

1. <span data-ttu-id="da0cb-159">action</span><span class="sxs-lookup"><span data-stu-id="da0cb-159">action</span></span>
1. <span data-ttu-id="da0cb-160">contrôleur</span><span class="sxs-lookup"><span data-stu-id="da0cb-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="da0cb-161">Désactiver CORS</span><span class="sxs-lookup"><span data-stu-id="da0cb-161">Disable CORS</span></span>

<span data-ttu-id="da0cb-162">Pour désactiver CORS pour un contrôleur ou d’action, utilisez la [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribut :</span><span class="sxs-lookup"><span data-stu-id="da0cb-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="da0cb-163">Options de stratégie CORS</span><span class="sxs-lookup"><span data-stu-id="da0cb-163">CORS policy options</span></span>

<span data-ttu-id="da0cb-164">Cette section décrit les différentes options que vous pouvez définir dans une stratégie CORS.</span><span class="sxs-lookup"><span data-stu-id="da0cb-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="da0cb-165">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="da0cb-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="da0cb-166">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="da0cb-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="da0cb-167">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="da0cb-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="da0cb-168">Définir les en-têtes de réponse exposé</span><span class="sxs-lookup"><span data-stu-id="da0cb-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="da0cb-169">Informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="da0cb-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="da0cb-170">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="da0cb-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="da0cb-171">Pour certaines options, il peut être utile de lire le [CORS comment fonctionne](#how-cors-works) section tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="da0cb-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="da0cb-172">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="da0cb-172">Set the allowed origins</span></span>

<span data-ttu-id="da0cb-173">L’intergiciel (middleware) CORS dans ASP.NET Core MVC présente quelques façons de spécifier les origines autorisées :</span><span class="sxs-lookup"><span data-stu-id="da0cb-173">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="da0cb-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Permet de spécifier une ou plusieurs URL.</span><span class="sxs-lookup"><span data-stu-id="da0cb-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Allows specifying one or more URLs.</span></span> <span data-ttu-id="da0cb-175">L’URL peut inclure le schéma, le nom d’hôte et le port sans informations de chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="da0cb-175">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="da0cb-176">Par exemple, `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="da0cb-176">For example, `https://example.com`.</span></span> <span data-ttu-id="da0cb-177">L’URL doit être spécifié sans barre oblique de fin (`/`).</span><span class="sxs-lookup"><span data-stu-id="da0cb-177">The URL must be specified without a trailing slash (`/`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

* <span data-ttu-id="da0cb-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Autorise les demandes CORS à partir de toutes les origines avec n’importe quel schéma (`http` ou `https`).</span><span class="sxs-lookup"><span data-stu-id="da0cb-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="da0cb-179">Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine.</span><span class="sxs-lookup"><span data-stu-id="da0cb-179">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="da0cb-180">Autoriser les demandes à partir de toute origine signifie que *n’importe quel site Web* peut apporter les demandes cross-origin à votre application.</span><span class="sxs-lookup"><span data-stu-id="da0cb-180">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="da0cb-181">Spécification `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner la falsification de requête intersites.</span><span class="sxs-lookup"><span data-stu-id="da0cb-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="da0cb-182">Le service CORS retourne une réponse non valide de CORS lorsqu’une application est configurée avec les deux.</span><span class="sxs-lookup"><span data-stu-id="da0cb-182">The CORS service returns an invalid CORS response when an app is configured with the two.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="da0cb-183">Spécification `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner la falsification de requête intersites.</span><span class="sxs-lookup"><span data-stu-id="da0cb-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="da0cb-184">Envisagez de spécifier une liste exacte des origines, si votre client doit accorder une autorisation d’accéder aux ressources du serveur.</span><span class="sxs-lookup"><span data-stu-id="da0cb-184">Consider specifying an exact list of origins if your client needs to authorize to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="da0cb-185">Ce paramètre affecte [préalable des demandes et l’en-tête Access-Control-Allow-Origin](#preflight-requests) (décrite plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="da0cb-185">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

* <span data-ttu-id="da0cb-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> -Permet aux demandes CORS à partir de n’importe quel sous-domaine d’un domaine donné.</span><span class="sxs-lookup"><span data-stu-id="da0cb-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> - Allows CORS requests from any sub-domain of a given domain.</span></span> <span data-ttu-id="da0cb-187">Le schéma ne peut pas être un caractère générique.</span><span class="sxs-lookup"><span data-stu-id="da0cb-187">The scheme cannot be a wildcard.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=98-104&highlight=4)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="da0cb-188">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="da0cb-188">Set the allowed HTTP methods</span></span>

<span data-ttu-id="da0cb-189">Pour autoriser toutes les méthodes HTTP, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="da0cb-189">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="da0cb-190">Ce paramètre affecte [préalable des demandes et l’en-tête Access-Control-Allow-Methods](#preflight-requests) (décrite plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="da0cb-190">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="da0cb-191">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="da0cb-191">Set the allowed request headers</span></span>

<span data-ttu-id="da0cb-192">Pour autoriser des en-têtes spécifiques à envoyer dans une demande CORS, appelé *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="da0cb-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="da0cb-193">Pour autoriser tous les créer des en-têtes de demande, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="da0cb-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="da0cb-194">Ce paramètre affecte [préalable des demandes et l’en-tête Access-Control-Request-Headers](#preflight-requests) (décrite plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="da0cb-194">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="da0cb-195">Une recherche de correspondance de stratégie intergiciel (middleware) CORS spécifiée par des en-têtes spécifiques `WithHeaders` n’est possible que lorsque les en-têtes envoyés `Access-Control-Request-Headers` correspondre exactement les en-têtes indiqués dans `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="da0cb-195">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="da0cb-196">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="da0cb-196">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="da0cb-197">Intergiciel (middleware) CORS refuse une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) n’est pas répertoriée dans `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="da0cb-197">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="da0cb-198">L’application retourne un *200 OK* réponse mais n’envoie pas les en-têtes CORS précédent.</span><span class="sxs-lookup"><span data-stu-id="da0cb-198">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="da0cb-199">Par conséquent, le navigateur ne tente pas la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="da0cb-199">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="da0cb-200">Intergiciel (middleware) CORS permet toujours quatre en-têtes dans le `Access-Control-Request-Headers` à envoyer, indépendamment des valeurs configurées dans CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="da0cb-200">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="da0cb-201">Cette liste d’en-têtes comprend :</span><span class="sxs-lookup"><span data-stu-id="da0cb-201">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="da0cb-202">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="da0cb-202">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="da0cb-203">Intergiciel (middleware) CORS répond correctement à une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` est toujours dans la liste verte :</span><span class="sxs-lookup"><span data-stu-id="da0cb-203">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="da0cb-204">Définir les en-têtes de réponse exposé</span><span class="sxs-lookup"><span data-stu-id="da0cb-204">Set the exposed response headers</span></span>

<span data-ttu-id="da0cb-205">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="da0cb-205">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="da0cb-206">Pour plus d’informations, consultez [W3C Cross-Origin Resource Sharing (terminologie) : en-tête de réponse Simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="da0cb-206">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="da0cb-207">Les en-têtes de réponse sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="da0cb-207">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="da0cb-208">La spécification CORS appelle ces en-têtes *les en-têtes de réponse simple*.</span><span class="sxs-lookup"><span data-stu-id="da0cb-208">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="da0cb-209">Pour les autres en-têtes disponibles pour l’application, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="da0cb-209">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="da0cb-210">Informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="da0cb-210">Credentials in cross-origin requests</span></span>

<span data-ttu-id="da0cb-211">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="da0cb-211">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="da0cb-212">Par défaut, le navigateur n’envoie pas les informations d’identification avec une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="da0cb-212">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="da0cb-213">Informations d’identification incluent les cookies et les schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="da0cb-213">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="da0cb-214">Pour envoyer des informations d’identification avec une demande de cross-origin, le client doit définir `XMLHttpRequest.withCredentials` à `true`.</span><span class="sxs-lookup"><span data-stu-id="da0cb-214">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="da0cb-215">À l’aide de `XMLHttpRequest` directement :</span><span class="sxs-lookup"><span data-stu-id="da0cb-215">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="da0cb-216">Dans jQuery :</span><span class="sxs-lookup"><span data-stu-id="da0cb-216">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="da0cb-217">En outre, le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="da0cb-217">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="da0cb-218">Pour autoriser les informations d’identification de cross-origin, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="da0cb-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="da0cb-219">La réponse HTTP inclut un `Access-Control-Allow-Credentials` en-tête, qui indique au navigateur que le serveur autorise les informations d’identification pour une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="da0cb-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="da0cb-220">Si le navigateur envoie les informations d’identification, mais la réponse n’inclut pas une valide `Access-Control-Allow-Credentials` en-tête, le navigateur n’expose pas la réponse à l’application, et la demande cross-origin échoue.</span><span class="sxs-lookup"><span data-stu-id="da0cb-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="da0cb-221">Soyez prudent lorsque vous autorisez les informations d’identification cross-origin.</span><span class="sxs-lookup"><span data-stu-id="da0cb-221">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="da0cb-222">Un site Web à un autre domaine peut envoyer des informations d’identification d’un utilisateur de connecté à l’application sur l’utilisateur sans avoir connaissance de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="da0cb-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="da0cb-223">La spécification CORS indique également ce paramètre pour les origines `"*"` (toutes les origines) n’est pas valide si le `Access-Control-Allow-Credentials` en-tête est présent.</span><span class="sxs-lookup"><span data-stu-id="da0cb-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="da0cb-224">Demandes préliminaires</span><span class="sxs-lookup"><span data-stu-id="da0cb-224">Preflight requests</span></span>

<span data-ttu-id="da0cb-225">Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="da0cb-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="da0cb-226">Cette requête est appelée un *demande préliminaire*.</span><span class="sxs-lookup"><span data-stu-id="da0cb-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="da0cb-227">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="da0cb-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="da0cb-228">La méthode de demande est GET, HEAD ou POST.</span><span class="sxs-lookup"><span data-stu-id="da0cb-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="da0cb-229">L’application ne définit pas les en-têtes de demande autre que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, ou `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="da0cb-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="da0cb-230">Le `Content-Type` en-tête, si définis, dispose d’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="da0cb-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="da0cb-231">La règle sur les en-têtes de requête définie pour la demande du client s’applique aux en-têtes de l’application définit en appelant `setRequestHeader` sur la `XMLHttpRequest` objet.</span><span class="sxs-lookup"><span data-stu-id="da0cb-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="da0cb-232">La spécification CORS appelle ces en-têtes *créer des en-têtes de demande*.</span><span class="sxs-lookup"><span data-stu-id="da0cb-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="da0cb-233">La règle ne s’applique pas aux en-têtes le navigateur peut définir comme `User-Agent`, `Host`, ou `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="da0cb-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="da0cb-234">Voici un exemple d’une requête préliminaire :</span><span class="sxs-lookup"><span data-stu-id="da0cb-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="da0cb-235">La demande de contrôle préliminaire utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="da0cb-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="da0cb-236">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="da0cb-236">It includes two special headers:</span></span>

* <span data-ttu-id="da0cb-237">`Access-Control-Request-Method`: La méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="da0cb-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="da0cb-238">`Access-Control-Request-Headers`: Liste des en-têtes de demande que l’application définit sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="da0cb-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="da0cb-239">Comme indiqué précédemment, cela n’inclut pas les en-têtes qui définit le navigateur, telles que `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="da0cb-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="da0cb-240">Une demande préliminaire CORS peut inclure un `Access-Control-Request-Headers` en-tête, qui indique au serveur les en-têtes qui sont envoyées avec la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="da0cb-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="da0cb-241">Pour autoriser des en-têtes spécifiques, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="da0cb-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="da0cb-242">Pour autoriser tous les créer des en-têtes de demande, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="da0cb-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="da0cb-243">Navigateurs ne sont pas entièrement cohérents dans la façon dont elles définies `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="da0cb-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="da0cb-244">Si vous définissez les en-têtes sur n’importe quelle autre que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`, et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="da0cb-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="da0cb-245">Voici un exemple de réponse à la demande préliminaire (en supposant que le serveur autorise la demande) :</span><span class="sxs-lookup"><span data-stu-id="da0cb-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="da0cb-246">La réponse inclut un `Access-Control-Allow-Methods` en-tête qui répertorie les méthodes autorisées et éventuellement un `Access-Control-Allow-Headers` en-tête qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="da0cb-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="da0cb-247">Si la demande préliminaire réussit, le navigateur envoie la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="da0cb-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="da0cb-248">Si la demande préliminaire est refusée, l’application retourne un *200 OK* réponse mais n’envoie pas les en-têtes CORS précédent.</span><span class="sxs-lookup"><span data-stu-id="da0cb-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="da0cb-249">Par conséquent, le navigateur ne tente pas la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="da0cb-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="da0cb-250">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="da0cb-250">Set the preflight expiration time</span></span>

<span data-ttu-id="da0cb-251">Le `Access-Control-Max-Age` en-tête spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mis en cache.</span><span class="sxs-lookup"><span data-stu-id="da0cb-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="da0cb-252">Pour définir cet en-tête, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="da0cb-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="da0cb-253">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="da0cb-253">How CORS works</span></span>

<span data-ttu-id="da0cb-254">Cette section décrit ce qui se passe dans une demande CORS au niveau des messages HTTP.</span><span class="sxs-lookup"><span data-stu-id="da0cb-254">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="da0cb-255">Il est important de comprendre le fonctionnement de CORS afin que la stratégie CORS peut être configurée correctement et de débogage lorsque des comportements inattendus se produisent.</span><span class="sxs-lookup"><span data-stu-id="da0cb-255">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="da0cb-256">La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="da0cb-256">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="da0cb-257">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="da0cb-257">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="da0cb-258">Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.</span><span class="sxs-lookup"><span data-stu-id="da0cb-258">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="da0cb-259">Voici un exemple d’une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="da0cb-259">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="da0cb-260">Le `Origin` en-tête fournit le domaine du site qui effectue la demande :</span><span class="sxs-lookup"><span data-stu-id="da0cb-260">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="da0cb-261">Si le serveur autorise la demande, il définit le `Access-Control-Allow-Origin` en-tête dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="da0cb-261">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="da0cb-262">La valeur de cet en-tête correspond soit à la `Origin` en-tête à partir de la demande, soit la valeur de caractère générique `"*"`, ce qui signifie que toute origine est autorisée :</span><span class="sxs-lookup"><span data-stu-id="da0cb-262">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="da0cb-263">Si la réponse n’inclut pas le `Access-Control-Allow-Origin` en-tête, la demande cross-origin échoue.</span><span class="sxs-lookup"><span data-stu-id="da0cb-263">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="da0cb-264">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="da0cb-264">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="da0cb-265">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible dans l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="da0cb-265">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da0cb-266">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="da0cb-266">Additional resources</span></span>

* [<span data-ttu-id="da0cb-267">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="da0cb-267">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
