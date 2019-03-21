---
title: Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme une norme pour autoriser ou rejeter les demandes cross-origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 2cad26d0f61519f63888a2bc399bb7e8a0f1ee04
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210130"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="7801f-103">Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7801f-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="7801f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7801f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7801f-105">Cet article explique comment activer CORS dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7801f-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="7801f-106">Sécurité des navigateurs empêche une page web d’effectuer des requêtes vers un autre domaine que celui qui a servi la page web.</span><span class="sxs-lookup"><span data-stu-id="7801f-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="7801f-107">Cette restriction est appelée le *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="7801f-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="7801f-108">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="7801f-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="7801f-109">Parfois, vous souhaiterez autoriser de qu'autres sites apporter les demandes cross-origin à votre application.</span><span class="sxs-lookup"><span data-stu-id="7801f-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="7801f-110">Pour plus d’informations, consultez le [article de Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="7801f-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="7801f-111">[Cross-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) :</span><span class="sxs-lookup"><span data-stu-id="7801f-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="7801f-112">Est un W3C standard qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="7801f-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="7801f-113">Est **pas** une fonctionnalité de sécurité, CORS assouplit la sécurité.</span><span class="sxs-lookup"><span data-stu-id="7801f-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="7801f-114">Une API n’est pas plus sûre en autorisant CORS.</span><span class="sxs-lookup"><span data-stu-id="7801f-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="7801f-115">Pour plus d’informations, consultez [CORS comment fonctionne](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="7801f-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="7801f-116">Permet à un serveur autoriser explicitement certaines demandes cross-origin tout en refusant d’autres.</span><span class="sxs-lookup"><span data-stu-id="7801f-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="7801f-117">Est plus sûre et plus flexible que les techniques précédentes, telles que [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="7801f-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="7801f-118">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7801f-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="7801f-119">Même origine</span><span class="sxs-lookup"><span data-stu-id="7801f-119">Same origin</span></span>

<span data-ttu-id="7801f-120">Deux URL ont la même origine que s’ils ont des schémas identiques, les hôtes et les ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="7801f-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="7801f-121">Ces deux URL ayant la même origine :</span><span class="sxs-lookup"><span data-stu-id="7801f-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="7801f-122">Ces URL ont différentes origines que les deux URL précédentes :</span><span class="sxs-lookup"><span data-stu-id="7801f-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="7801f-123">`https://example.net` &ndash; Autre domaine</span><span class="sxs-lookup"><span data-stu-id="7801f-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="7801f-124">`https://www.example.com/foo.html` &ndash; Autre sous-domaine</span><span class="sxs-lookup"><span data-stu-id="7801f-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="7801f-125">`http://example.com/foo.html` &ndash; Schéma différent</span><span class="sxs-lookup"><span data-stu-id="7801f-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="7801f-126">`https://example.com:9000/foo.html` &ndash; Port différent</span><span class="sxs-lookup"><span data-stu-id="7801f-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="7801f-127">Internet Explorer ne considère pas le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="7801f-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="7801f-128">CORS avec la stratégie nommée et intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="7801f-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="7801f-129">Intergiciel (middleware) CORS gère les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="7801f-130">Le code suivant active CORS pour toute l’application avec l’origine spécifiée :</span><span class="sxs-lookup"><span data-stu-id="7801f-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="7801f-131">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="7801f-131">The preceding code:</span></span>

* <span data-ttu-id="7801f-132">Définit le nom de la stratégie «\_myAllowSpecificOrigins ».</span><span class="sxs-lookup"><span data-stu-id="7801f-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="7801f-133">Le nom de la stratégie est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="7801f-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="7801f-134">Appelle le <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> méthode d’extension, ce qui permet des cœurs.</span><span class="sxs-lookup"><span data-stu-id="7801f-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables cores.</span></span>
* <span data-ttu-id="7801f-135">Appels <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> avec un [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="7801f-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="7801f-136">L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="7801f-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="7801f-137">[Options de configuration](#cors-policy-options), tel que `WithOrigins`, sont décrits plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="7801f-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="7801f-138">Le <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> appel de méthode ajoute des services CORS au conteneur de service de l’application :</span><span class="sxs-lookup"><span data-stu-id="7801f-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="7801f-139">Pour plus d’informations, consultez [les options de stratégie CORS](#cpo) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="7801f-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="7801f-140">Le <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> méthode pouvez chaîner des méthodes, comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="7801f-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="7801f-141">Le code en surbrillance suivant applique les stratégies CORS pour toutes les applications points de terminaison via l’intergiciel (middleware) CORS :</span><span class="sxs-lookup"><span data-stu-id="7801f-141">The following highlighted code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

<span data-ttu-id="7801f-142">Consultez [activer de CORS dans les Pages Razor, contrôleurs et méthodes d’action](#ecors) pour appliquer la stratégie CORS au niveau de la page / / action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7801f-142">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="7801f-143">Remarque :</span><span class="sxs-lookup"><span data-stu-id="7801f-143">Note:</span></span>

* <span data-ttu-id="7801f-144">`UseCors` doit être appelé avant `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="7801f-144">`UseCors` must be called before `UseMvc`.</span></span>
* <span data-ttu-id="7801f-145">L’URL doit **pas** contiennent une barre oblique (`/`).</span><span class="sxs-lookup"><span data-stu-id="7801f-145">The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="7801f-146">Si l’URL se termine avec `/`, la comparaison retourne `false` et aucun en-tête n’est retourné.</span><span class="sxs-lookup"><span data-stu-id="7801f-146">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="7801f-147">Consultez [Test CORS](#test) pour obtenir des instructions sur le test du code précédent.</span><span class="sxs-lookup"><span data-stu-id="7801f-147">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="7801f-148">Activer CORS avec attributs</span><span class="sxs-lookup"><span data-stu-id="7801f-148">Enable CORS with attributes</span></span>

<span data-ttu-id="7801f-149">Le [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribut fournit une alternative à l’application de CORS dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="7801f-149">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="7801f-150">Le `[EnableCors]` attribut Active CORS pour les points de terminaison sélectionnés, plutôt que tous les points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="7801f-150">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="7801f-151">Utilisez `[EnableCors]` pour spécifier la stratégie par défaut et `[EnableCors("{Policy String}")]` pour spécifier une stratégie.</span><span class="sxs-lookup"><span data-stu-id="7801f-151">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="7801f-152">Le `[EnableCors]` attribut peut être appliqué aux :</span><span class="sxs-lookup"><span data-stu-id="7801f-152">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="7801f-153">Page Razor `PageModel`</span><span class="sxs-lookup"><span data-stu-id="7801f-153">Razor Page `PageModel`</span></span>
* <span data-ttu-id="7801f-154">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="7801f-154">Controller</span></span>
* <span data-ttu-id="7801f-155">Méthode d’action de contrôleur</span><span class="sxs-lookup"><span data-stu-id="7801f-155">Controller action method</span></span>

<span data-ttu-id="7801f-156">Vous pouvez appliquer des stratégies différentes à contrôleur/page-modèle/une action avec la `[EnableCors]` attribut.</span><span class="sxs-lookup"><span data-stu-id="7801f-156">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="7801f-157">Lorsque le `[EnableCors]` attribut est appliqué à une méthode d’action/modèle-page/contrôleurs et CORS est activé dans l’intergiciel (middleware), les deux stratégies sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="7801f-157">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="7801f-158">Nous déconseillons de combinaison de stratégies.</span><span class="sxs-lookup"><span data-stu-id="7801f-158">We recommend against combining policies.</span></span> <span data-ttu-id="7801f-159">Utilisez le `[EnableCors]` attribut ou un intergiciel (middleware), pas à la fois dans la même application.</span><span class="sxs-lookup"><span data-stu-id="7801f-159">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="7801f-160">Le code suivant applique une stratégie différente à chaque méthode :</span><span class="sxs-lookup"><span data-stu-id="7801f-160">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="7801f-161">Le code suivant crée une stratégie par défaut CORS et une stratégie nommée `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="7801f-161">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="7801f-162">Désactiver CORS</span><span class="sxs-lookup"><span data-stu-id="7801f-162">Disable CORS</span></span>

<span data-ttu-id="7801f-163">Le [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribut désactive CORS pour la contrôleur/page-modèle/une action.</span><span class="sxs-lookup"><span data-stu-id="7801f-163">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="7801f-164">Options de stratégie CORS</span><span class="sxs-lookup"><span data-stu-id="7801f-164">CORS policy options</span></span>

<span data-ttu-id="7801f-165">Cette section décrit les différentes options qui peuvent être définies dans une stratégie CORS :</span><span class="sxs-lookup"><span data-stu-id="7801f-165">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="7801f-166">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="7801f-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="7801f-167">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="7801f-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="7801f-168">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="7801f-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="7801f-169">Définir les en-têtes de réponse exposé</span><span class="sxs-lookup"><span data-stu-id="7801f-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="7801f-170">Informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="7801f-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="7801f-171">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="7801f-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="7801f-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> est appelé dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7801f-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7801f-173">Pour certaines options, il peut être utile de lire le [CORS comment fonctionne](#how-cors) section tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="7801f-173">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="7801f-174">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="7801f-174">Set the allowed origins</span></span>

<span data-ttu-id="7801f-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Autorise les demandes CORS à partir de toutes les origines avec n’importe quel schéma (`http` ou `https`).</span><span class="sxs-lookup"><span data-stu-id="7801f-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="7801f-176">`AllowAnyOrigin` n’est pas sécurisé, car *n’importe quel site Web* peut apporter les demandes cross-origin à l’application.</span><span class="sxs-lookup"><span data-stu-id="7801f-176">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="7801f-177">Spécification `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner la falsification de requête intersites.</span><span class="sxs-lookup"><span data-stu-id="7801f-177">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="7801f-178">Le service CORS retourne une réponse non valide de CORS lorsqu’une application est configurée avec les deux méthodes.</span><span class="sxs-lookup"><span data-stu-id="7801f-178">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="7801f-179">Spécification `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner la falsification de requête intersites.</span><span class="sxs-lookup"><span data-stu-id="7801f-179">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="7801f-180">Pour une application sécurisée, spécifiez une liste exacte des origines si le client doit autoriser lui-même pour accéder aux ressources du serveur.</span><span class="sxs-lookup"><span data-stu-id="7801f-180">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**.
-->

<span data-ttu-id="7801f-181">`AllowAnyOrigin` affecte des demandes de contrôle en amont et le `Access-Control-Allow-Origin` en-tête.</span><span class="sxs-lookup"><span data-stu-id="7801f-181">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="7801f-182">Pour plus d’informations, consultez le [demandes de contrôle en amont](#preflight-requests) section.</span><span class="sxs-lookup"><span data-stu-id="7801f-182">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7801f-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Définit le <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propriété de la stratégie comme étant une fonction qui autorise les origines correspondre à un domaine générique configuré lors de l’évaluation si l’origine est autorisée.</span><span class="sxs-lookup"><span data-stu-id="7801f-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="7801f-184">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="7801f-184">Set the allowed HTTP methods</span></span>

<span data-ttu-id="7801f-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="7801f-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="7801f-186">Permet de n’importe quelle méthode HTTP :</span><span class="sxs-lookup"><span data-stu-id="7801f-186">Allows any HTTP method:</span></span>
* <span data-ttu-id="7801f-187">Affecte des demandes de contrôle en amont et le `Access-Control-Allow-Methods` en-tête.</span><span class="sxs-lookup"><span data-stu-id="7801f-187">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="7801f-188">Pour plus d’informations, consultez le [demandes de contrôle en amont](#preflight-requests) section.</span><span class="sxs-lookup"><span data-stu-id="7801f-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="7801f-189">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="7801f-189">Set the allowed request headers</span></span>

<span data-ttu-id="7801f-190">Pour autoriser des en-têtes spécifiques à envoyer dans une demande CORS, appelé *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="7801f-190">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="7801f-191">Pour autoriser tous les créer des en-têtes de demande, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="7801f-191">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="7801f-192">Ce paramètre affecte les demandes préliminaires et `Access-Control-Request-Headers` en-tête.</span><span class="sxs-lookup"><span data-stu-id="7801f-192">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="7801f-193">Pour plus d’informations, consultez le [demandes de contrôle en amont](#preflight-requests) section.</span><span class="sxs-lookup"><span data-stu-id="7801f-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7801f-194">Une recherche de correspondance de stratégie intergiciel (middleware) CORS spécifiée par des en-têtes spécifiques `WithHeaders` n’est possible que lorsque les en-têtes envoyés `Access-Control-Request-Headers` correspondre exactement les en-têtes indiqués dans `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="7801f-194">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="7801f-195">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="7801f-195">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="7801f-196">Intergiciel (middleware) CORS refuse une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) n’est pas répertoriée dans `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="7801f-196">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="7801f-197">L’application retourne un *200 OK* réponse mais n’envoie pas les en-têtes CORS précédent.</span><span class="sxs-lookup"><span data-stu-id="7801f-197">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7801f-198">Par conséquent, le navigateur ne tente pas la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-198">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7801f-199">Intergiciel (middleware) CORS permet toujours quatre en-têtes dans le `Access-Control-Request-Headers` à envoyer, indépendamment des valeurs configurées dans CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="7801f-199">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="7801f-200">Cette liste d’en-têtes comprend :</span><span class="sxs-lookup"><span data-stu-id="7801f-200">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="7801f-201">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="7801f-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="7801f-202">Intergiciel (middleware) CORS répond correctement à une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` est toujours dans la liste verte :</span><span class="sxs-lookup"><span data-stu-id="7801f-202">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="7801f-203">Définir les en-têtes de réponse exposé</span><span class="sxs-lookup"><span data-stu-id="7801f-203">Set the exposed response headers</span></span>

<span data-ttu-id="7801f-204">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="7801f-204">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="7801f-205">Pour plus d’informations, consultez [W3C Cross-Origin Resource Sharing (terminologie) : En-tête de réponse simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="7801f-205">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="7801f-206">Les en-têtes de réponse sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="7801f-206">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="7801f-207">La spécification CORS appelle ces en-têtes *les en-têtes de réponse simple*.</span><span class="sxs-lookup"><span data-stu-id="7801f-207">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="7801f-208">Pour les autres en-têtes disponibles pour l’application, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="7801f-208">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="7801f-209">Informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="7801f-209">Credentials in cross-origin requests</span></span>

<span data-ttu-id="7801f-210">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="7801f-210">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="7801f-211">Par défaut, le navigateur n’envoie pas les informations d’identification avec une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-211">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="7801f-212">Informations d’identification incluent les cookies et les schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="7801f-212">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="7801f-213">Pour envoyer des informations d’identification avec une demande de cross-origin, le client doit définir `XMLHttpRequest.withCredentials` à `true`.</span><span class="sxs-lookup"><span data-stu-id="7801f-213">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="7801f-214">À l’aide de `XMLHttpRequest` directement :</span><span class="sxs-lookup"><span data-stu-id="7801f-214">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="7801f-215">À l’aide de jQuery :</span><span class="sxs-lookup"><span data-stu-id="7801f-215">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="7801f-216">À l’aide de la [obtenir l’API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="7801f-216">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="7801f-217">Le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="7801f-217">The server must allow the credentials.</span></span> <span data-ttu-id="7801f-218">Pour autoriser les informations d’identification de cross-origin, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="7801f-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="7801f-219">La réponse HTTP inclut un `Access-Control-Allow-Credentials` en-tête, qui indique au navigateur que le serveur autorise les informations d’identification pour une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="7801f-220">Si le navigateur envoie les informations d’identification, mais la réponse n’inclut pas une valide `Access-Control-Allow-Credentials` en-tête, le navigateur n’expose pas la réponse à l’application, et la demande cross-origin échoue.</span><span class="sxs-lookup"><span data-stu-id="7801f-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="7801f-221">Autoriser les informations d’identification de cross-origin est un risque de sécurité.</span><span class="sxs-lookup"><span data-stu-id="7801f-221">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="7801f-222">Un site Web à un autre domaine peut envoyer des informations d’identification d’un utilisateur de connecté à l’application sur l’utilisateur sans avoir connaissance de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7801f-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="7801f-223">La spécification CORS indique également ce paramètre pour les origines `"*"` (toutes les origines) n’est pas valide si le `Access-Control-Allow-Credentials` en-tête est présent.</span><span class="sxs-lookup"><span data-stu-id="7801f-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="7801f-224">Demandes préliminaires</span><span class="sxs-lookup"><span data-stu-id="7801f-224">Preflight requests</span></span>

<span data-ttu-id="7801f-225">Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="7801f-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="7801f-226">Cette requête est appelée un *demande préliminaire*.</span><span class="sxs-lookup"><span data-stu-id="7801f-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="7801f-227">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="7801f-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="7801f-228">La méthode de demande est GET, HEAD ou POST.</span><span class="sxs-lookup"><span data-stu-id="7801f-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="7801f-229">L’application ne définit pas les en-têtes de demande autre que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, ou `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="7801f-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="7801f-230">Le `Content-Type` en-tête, si définis, dispose d’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="7801f-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="7801f-231">La règle sur les en-têtes de requête définie pour la demande du client s’applique aux en-têtes de l’application définit en appelant `setRequestHeader` sur la `XMLHttpRequest` objet.</span><span class="sxs-lookup"><span data-stu-id="7801f-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="7801f-232">La spécification CORS appelle ces en-têtes *créer des en-têtes de demande*.</span><span class="sxs-lookup"><span data-stu-id="7801f-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="7801f-233">La règle ne s’applique pas aux en-têtes le navigateur peut définir comme `User-Agent`, `Host`, ou `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="7801f-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="7801f-234">Voici un exemple d’une requête préliminaire :</span><span class="sxs-lookup"><span data-stu-id="7801f-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="7801f-235">La demande de contrôle préliminaire utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="7801f-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="7801f-236">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="7801f-236">It includes two special headers:</span></span>

* <span data-ttu-id="7801f-237">`Access-Control-Request-Method`: La méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="7801f-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="7801f-238">`Access-Control-Request-Headers`: Liste des en-têtes de demande que l’application définit sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="7801f-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="7801f-239">Comme indiqué précédemment, cela n’inclut pas les en-têtes qui définit le navigateur, telles que `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="7801f-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="7801f-240">Une demande préliminaire CORS peut inclure un `Access-Control-Request-Headers` en-tête, qui indique au serveur les en-têtes qui sont envoyées avec la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="7801f-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="7801f-241">Pour autoriser des en-têtes spécifiques, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="7801f-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="7801f-242">Pour autoriser tous les créer des en-têtes de demande, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="7801f-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="7801f-243">Navigateurs ne sont pas entièrement cohérents dans la façon dont elles définies `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="7801f-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="7801f-244">Si vous définissez les en-têtes sur n’importe quelle autre que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`, et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="7801f-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="7801f-245">Voici un exemple de réponse à la demande préliminaire (en supposant que le serveur autorise la demande) :</span><span class="sxs-lookup"><span data-stu-id="7801f-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="7801f-246">La réponse inclut un `Access-Control-Allow-Methods` en-tête qui répertorie les méthodes autorisées et éventuellement un `Access-Control-Allow-Headers` en-tête qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="7801f-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="7801f-247">Si la demande préliminaire réussit, le navigateur envoie la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="7801f-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="7801f-248">Si la demande préliminaire est refusée, l’application retourne un *200 OK* réponse mais n’envoie pas les en-têtes CORS précédent.</span><span class="sxs-lookup"><span data-stu-id="7801f-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7801f-249">Par conséquent, le navigateur ne tente pas la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="7801f-250">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="7801f-250">Set the preflight expiration time</span></span>

<span data-ttu-id="7801f-251">Le `Access-Control-Max-Age` en-tête spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mis en cache.</span><span class="sxs-lookup"><span data-stu-id="7801f-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="7801f-252">Pour définir cet en-tête, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="7801f-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="7801f-253">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="7801f-253">How CORS works</span></span>

<span data-ttu-id="7801f-254">Cette section décrit ce qui se passe dans un [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) demande au niveau des messages HTTP.</span><span class="sxs-lookup"><span data-stu-id="7801f-254">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="7801f-255">CORS est **pas** une fonctionnalité de sécurité.</span><span class="sxs-lookup"><span data-stu-id="7801f-255">CORS is **not** a security feature.</span></span> <span data-ttu-id="7801f-256">CORS est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="7801f-256">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="7801f-257">Par exemple, un acteur malveillant pourrait utiliser [empêcher Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) par rapport à votre site avant d’exécuter une requête intersites à leur site CORS est activé pour dérober des informations.</span><span class="sxs-lookup"><span data-stu-id="7801f-257">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="7801f-258">Votre API n’est pas plus sûre en autorisant CORS.</span><span class="sxs-lookup"><span data-stu-id="7801f-258">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="7801f-259">C’est à appliquer CORS pour le client (navigateur).</span><span class="sxs-lookup"><span data-stu-id="7801f-259">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="7801f-260">Le serveur exécute la requête et renvoie la réponse, c’est le client qui renvoie une erreur et les blocs de la réponse.</span><span class="sxs-lookup"><span data-stu-id="7801f-260">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="7801f-261">Par exemple, un des outils suivants affiche la réponse du serveur :</span><span class="sxs-lookup"><span data-stu-id="7801f-261">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="7801f-262">Fiddler</span><span class="sxs-lookup"><span data-stu-id="7801f-262">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="7801f-263">Postman</span><span class="sxs-lookup"><span data-stu-id="7801f-263">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="7801f-264">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="7801f-264">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="7801f-265">Un navigateur web en entrant l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="7801f-265">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="7801f-266">C’est un moyen pour un serveur pour permettre des navigateurs pour exécuter une cross-origine [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) ou [API Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) demande sinon est interdite.</span><span class="sxs-lookup"><span data-stu-id="7801f-266">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="7801f-267">Navigateurs (sans CORS) ne peut pas faire des demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-267">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="7801f-268">Avant de CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) a été utilisé pour contourner cette restriction.</span><span class="sxs-lookup"><span data-stu-id="7801f-268">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="7801f-269">JSONP n’utilise pas XHR, il utilise le `<script>` balise pour recevoir la réponse.</span><span class="sxs-lookup"><span data-stu-id="7801f-269">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="7801f-270">Les scripts sont autorisés à être chargé cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-270">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="7801f-271">Le [spécification CORS](https://www.w3.org/TR/cors/) introduit plusieurs nouveaux en-têtes HTTP qui permettent les requêtes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-271">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="7801f-272">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-272">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="7801f-273">Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.</span><span class="sxs-lookup"><span data-stu-id="7801f-273">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="7801f-274">Voici un exemple d’une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7801f-274">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="7801f-275">Le `Origin` en-tête fournit le domaine du site qui effectue la demande :</span><span class="sxs-lookup"><span data-stu-id="7801f-275">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="7801f-276">Si le serveur autorise la demande, il définit le `Access-Control-Allow-Origin` en-tête dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="7801f-276">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="7801f-277">La valeur de cet en-tête correspond soit à la `Origin` en-tête à partir de la demande, soit la valeur de caractère générique `"*"`, ce qui signifie que toute origine est autorisée :</span><span class="sxs-lookup"><span data-stu-id="7801f-277">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="7801f-278">Si la réponse n’inclut pas le `Access-Control-Allow-Origin` en-tête, la demande cross-origin échoue.</span><span class="sxs-lookup"><span data-stu-id="7801f-278">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="7801f-279">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="7801f-279">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="7801f-280">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible dans l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="7801f-280">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="7801f-281">Testez CORS</span><span class="sxs-lookup"><span data-stu-id="7801f-281">Test CORS</span></span>

<span data-ttu-id="7801f-282">Pour tester CORS :</span><span class="sxs-lookup"><span data-stu-id="7801f-282">To test CORS:</span></span>

1. <span data-ttu-id="7801f-283">[Créer un projet d’API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="7801f-283">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="7801f-284">Vous pouvez également [télécharger l’exemple](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="7801f-284">Alternatively, you can [download the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="7801f-285">Activer CORS à l’aide d’une des approches dans ce document.</span><span class="sxs-lookup"><span data-stu-id="7801f-285">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="7801f-286">Exemple :</span><span class="sxs-lookup"><span data-stu-id="7801f-286">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="7801f-287">`WithOrigins("https://localhost:<port>");` doit uniquement être utilisé pour tester un exemple d’application similaire à la [télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="7801f-287">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="7801f-288">Créer un projet d’application web (Pages Razor ou MVC).</span><span class="sxs-lookup"><span data-stu-id="7801f-288">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="7801f-289">L’exemple utilise les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="7801f-289">The sample uses Razor Pages.</span></span> <span data-ttu-id="7801f-290">Vous pouvez créer l’application web dans la même solution que le projet d’API.</span><span class="sxs-lookup"><span data-stu-id="7801f-290">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="7801f-291">Ajoutez le code en surbrillance suivant à la *Index.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="7801f-291">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="7801f-292">Dans le code précédent, remplacez `url: 'https://<web app>.azurewebsites.net/api/values/1',` par l’URL de l’application déployée.</span><span class="sxs-lookup"><span data-stu-id="7801f-292">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="7801f-293">Déployer le projet d’API.</span><span class="sxs-lookup"><span data-stu-id="7801f-293">Deploy the API project.</span></span> <span data-ttu-id="7801f-294">Par exemple, [déployer vers Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="7801f-294">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="7801f-295">Exécutez l’application Pages Razor ou MVC à partir du bureau et cliquez sur le **Test** bouton.</span><span class="sxs-lookup"><span data-stu-id="7801f-295">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="7801f-296">Utiliser les outils F12 pour passer en revue les messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="7801f-296">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="7801f-297">Supprimer l’origine de localhost à partir de `WithOrigins` et déployer l’application.</span><span class="sxs-lookup"><span data-stu-id="7801f-297">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="7801f-298">Vous pouvez également exécuter l’application cliente avec un autre port.</span><span class="sxs-lookup"><span data-stu-id="7801f-298">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="7801f-299">Par exemple, exécutez à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7801f-299">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="7801f-300">Tester avec l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="7801f-300">Test with the client app.</span></span> <span data-ttu-id="7801f-301">Défaillances CORS renvoient une erreur, mais le message d’erreur n’est pas disponible à JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7801f-301">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="7801f-302">Utilisez l’onglet de la console dans les outils F12 pour afficher l’erreur.</span><span class="sxs-lookup"><span data-stu-id="7801f-302">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="7801f-303">Selon le navigateur, vous obtenez une erreur (dans la console Outils F12) similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="7801f-303">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="7801f-304">À l’aide de Microsoft Edge :</span><span class="sxs-lookup"><span data-stu-id="7801f-304">Using Microsoft Edge:</span></span>

     <span data-ttu-id="7801f-305">**SEC7120 : [CORS] l’origine `https://localhost:44375` n’a pas trouvé `https://localhost:44375` dans l’en-tête de réponse Access-Control-Allow-Origin pour des ressources cross-origin à `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="7801f-305">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="7801f-306">À l’aide de Chrome :</span><span class="sxs-lookup"><span data-stu-id="7801f-306">Using Chrome:</span></span>

     <span data-ttu-id="7801f-307">**Accès à XMLHttpRequest au `https://webapi.azurewebsites.net/api/values/1` à partir de l’origine `https://localhost:44375` a été bloqué par la stratégie CORS : Aucun en-tête 'Access-Control-Allow-Origin' n’est présent sur la ressource demandée.**</span><span class="sxs-lookup"><span data-stu-id="7801f-307">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7801f-308">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7801f-308">Additional resources</span></span>

* [<span data-ttu-id="7801f-309">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="7801f-309">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
