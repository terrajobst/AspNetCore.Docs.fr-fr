---
title: Activer les requêtes Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme norme pour autoriser ou rejeter des demandes Cross-Origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 09cc296ebf605907371619124cac00883beb6abb
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511572"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="fb438-103">Activer les requêtes Cross-Origin (CORS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb438-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fb438-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb438-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fb438-105">Cet article explique comment activer CORS dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb438-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="fb438-106">La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web.</span><span class="sxs-lookup"><span data-stu-id="fb438-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="fb438-107">Cette restriction est appelée *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="fb438-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="fb438-108">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="fb438-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="fb438-109">Parfois, vous souhaiterez peut-être autoriser d’autres sites à effectuer des demandes Cross-Origin à votre application.</span><span class="sxs-lookup"><span data-stu-id="fb438-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="fb438-110">Pour plus d’informations, consultez l' [article Mozilla cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="fb438-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="fb438-111">[Partage des ressources Cross-Origin](https://www.w3.org/TR/cors/) (cors) :</span><span class="sxs-lookup"><span data-stu-id="fb438-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="fb438-112">Est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="fb438-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="fb438-113">N’est **pas** une fonctionnalité de sécurité, cors assouplit la sécurité.</span><span class="sxs-lookup"><span data-stu-id="fb438-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="fb438-114">Une API n’est pas plus sûre en autorisant CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="fb438-115">Pour plus d’informations, consultez fonctionnement de [cors](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="fb438-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="fb438-116">Permet à un serveur d’autoriser explicitement certaines demandes Cross-Origin tout en rejetant d’autres.</span><span class="sxs-lookup"><span data-stu-id="fb438-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="fb438-117">Est plus sûr et plus flexible que les techniques antérieures, telles que [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="fb438-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="fb438-118">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb438-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="fb438-119">Même origine</span><span class="sxs-lookup"><span data-stu-id="fb438-119">Same origin</span></span>

<span data-ttu-id="fb438-120">Deux URL ont la même origine si elles ont des schémas, des hôtes et des ports identiques ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="fb438-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="fb438-121">Ces deux URL ont le même origine :</span><span class="sxs-lookup"><span data-stu-id="fb438-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="fb438-122">Ces URL ont des origines différentes des deux précédentes URL :</span><span class="sxs-lookup"><span data-stu-id="fb438-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="fb438-123">`https://example.net` &ndash; domaine différent</span><span class="sxs-lookup"><span data-stu-id="fb438-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="fb438-124">`https://www.example.com/foo.html` &ndash; sous-domaine différent</span><span class="sxs-lookup"><span data-stu-id="fb438-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="fb438-125">`http://example.com/foo.html` &ndash; schéma différent</span><span class="sxs-lookup"><span data-stu-id="fb438-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="fb438-126">`https://example.com:9000/foo.html` &ndash; port différent</span><span class="sxs-lookup"><span data-stu-id="fb438-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="fb438-127">Internet Explorer ne prend pas en compte le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="fb438-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="fb438-128">CORS avec la stratégie et l’intergiciel (middleware) nommés</span><span class="sxs-lookup"><span data-stu-id="fb438-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="fb438-129">L’intergiciel (middleware) CORS gère les demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="fb438-130">Le code suivant active CORS pour l’application entière avec l’origine spécifiée :</span><span class="sxs-lookup"><span data-stu-id="fb438-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="fb438-131">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="fb438-131">The preceding code:</span></span>

* <span data-ttu-id="fb438-132">Définit le nom de la stratégie sur «\_myAllowSpecificOrigins ».</span><span class="sxs-lookup"><span data-stu-id="fb438-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="fb438-133">Le nom de la stratégie est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="fb438-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="fb438-134">Appelle la méthode d’extension <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, qui active CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="fb438-135">Appelle <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> avec une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="fb438-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="fb438-136">L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fb438-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="fb438-137">Les [options de configuration](#cors-policy-options), telles que `WithOrigins`, sont décrites plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="fb438-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="fb438-138">L’appel de la méthode <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> ajoute des services CORS au conteneur de services de l’application :</span><span class="sxs-lookup"><span data-stu-id="fb438-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="fb438-139">Pour plus d’informations, consultez les [options de stratégie cors](#cpo) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="fb438-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="fb438-140">La méthode <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> peut chaîner des méthodes, comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="fb438-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="fb438-141">Remarque : l’URL **ne doit pas** contenir de barre oblique de fin (`/`).</span><span class="sxs-lookup"><span data-stu-id="fb438-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="fb438-142">Si l’URL se termine par `/`, la comparaison retourne `false` et aucun en-tête n’est retourné.</span><span class="sxs-lookup"><span data-stu-id="fb438-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="fb438-143">Appliquer des stratégies CORS à tous les points de terminaison</span><span class="sxs-lookup"><span data-stu-id="fb438-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="fb438-144">Le code suivant applique des stratégies CORS à tous les points de terminaison des applications par le biais de l’intergiciel (middleware) CORS :</span><span class="sxs-lookup"><span data-stu-id="fb438-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code omitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code omitted.
}
```

> [!WARNING]
> <span data-ttu-id="fb438-145">Avec le routage de point de terminaison, l’intergiciel (middleware) CORS doit être configuré pour s’exécuter entre les appels à `UseRouting` et `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="fb438-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="fb438-146">Une configuration incorrecte entraîne l’arrêt du fonctionnement correct de l’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="fb438-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

<span data-ttu-id="fb438-147">Consultez [activer cors dans Razor pages, les contrôleurs et les méthodes d’action](#ecors) pour appliquer la stratégie cors au niveau de la page/du contrôleur/action.</span><span class="sxs-lookup"><span data-stu-id="fb438-147">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="fb438-148">Consultez la page [test cors](#test) pour obtenir des instructions sur le test du code précédent.</span><span class="sxs-lookup"><span data-stu-id="fb438-148">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="fb438-149">Activer cors avec routage du point de terminaison</span><span class="sxs-lookup"><span data-stu-id="fb438-149">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="fb438-150">Avec le routage de point de terminaison, CORS peut être activé sur une base par point de terminaison à l’aide de la `RequireCors` ensemble de méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="fb438-150">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="fb438-151">De même, CORS peut également être activé pour tous les contrôleurs :</span><span class="sxs-lookup"><span data-stu-id="fb438-151">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="fb438-152">Activer CORS avec des attributs</span><span class="sxs-lookup"><span data-stu-id="fb438-152">Enable CORS with attributes</span></span>

<span data-ttu-id="fb438-153">L’attribut [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fournit une alternative à l’application de cors globalement.</span><span class="sxs-lookup"><span data-stu-id="fb438-153">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="fb438-154">L’attribut `[EnableCors]` active CORS pour les points de terminaison sélectionnés, plutôt que tous les points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="fb438-154">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="fb438-155">Utilisez `[EnableCors]` pour spécifier la stratégie par défaut et `[EnableCors("{Policy String}")]` pour spécifier une stratégie.</span><span class="sxs-lookup"><span data-stu-id="fb438-155">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="fb438-156">L’attribut `[EnableCors]` peut être appliqué aux éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fb438-156">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="fb438-157">`PageModel` de page Razor</span><span class="sxs-lookup"><span data-stu-id="fb438-157">Razor Page `PageModel`</span></span>
* <span data-ttu-id="fb438-158">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="fb438-158">Controller</span></span>
* <span data-ttu-id="fb438-159">Méthode d’action du contrôleur</span><span class="sxs-lookup"><span data-stu-id="fb438-159">Controller action method</span></span>

<span data-ttu-id="fb438-160">Vous pouvez appliquer différentes stratégies à Controller/page-Model/action avec l’attribut `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="fb438-160">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="fb438-161">Lorsque l’attribut `[EnableCors]` est appliqué à une méthode Controllers/Model-Model/action et que CORS est activé dans l’intergiciel (middleware), les deux stratégies sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="fb438-161">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="fb438-162">Nous vous recommandons de combiner les stratégies.</span><span class="sxs-lookup"><span data-stu-id="fb438-162">We recommend against combining policies.</span></span> <span data-ttu-id="fb438-163">Utilisez l’attribut `[EnableCors]` ou l’intergiciel (middleware), pas les deux dans la même application.</span><span class="sxs-lookup"><span data-stu-id="fb438-163">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="fb438-164">Le code suivant applique une stratégie différente à chaque méthode :</span><span class="sxs-lookup"><span data-stu-id="fb438-164">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="fb438-165">Le code suivant crée une stratégie CORS par défaut et une stratégie nommée `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="fb438-165">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="fb438-166">Désactiver CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-166">Disable CORS</span></span>

<span data-ttu-id="fb438-167">L’attribut [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) désactive cors pour le contrôleur/page-Model/action.</span><span class="sxs-lookup"><span data-stu-id="fb438-167">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="fb438-168">Options de stratégie CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-168">CORS policy options</span></span>

<span data-ttu-id="fb438-169">Cette section décrit les différentes options qui peuvent être définies dans une stratégie CORS :</span><span class="sxs-lookup"><span data-stu-id="fb438-169">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="fb438-170">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="fb438-170">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="fb438-171">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="fb438-171">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="fb438-172">Définir les en-têtes de demande autorisés</span><span class="sxs-lookup"><span data-stu-id="fb438-172">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="fb438-173">Définir les en-têtes de réponse exposés</span><span class="sxs-lookup"><span data-stu-id="fb438-173">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="fb438-174">Informations d’identification dans les demandes Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="fb438-174">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="fb438-175">Définir le délai d’expiration du prévols</span><span class="sxs-lookup"><span data-stu-id="fb438-175">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="fb438-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> est appelé dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fb438-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fb438-177">Pour certaines options, il peut être utile de lire d’abord la section [How cors Works](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="fb438-177">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="fb438-178">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="fb438-178">Set the allowed origins</span></span>

<span data-ttu-id="fb438-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; autorise les requêtes CORS de toutes les origines avec n’importe quel schéma (`http` ou `https`).</span><span class="sxs-lookup"><span data-stu-id="fb438-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="fb438-180">`AllowAnyOrigin` n’est pas sécurisé, car *tout site Web* peut effectuer des demandes Cross-Origin à l’application.</span><span class="sxs-lookup"><span data-stu-id="fb438-180">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="fb438-181">La spécification de `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner une falsification de requête intersites.</span><span class="sxs-lookup"><span data-stu-id="fb438-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="fb438-182">Le service CORS retourne une réponse CORS non valide lorsqu’une application est configurée avec les deux méthodes.</span><span class="sxs-lookup"><span data-stu-id="fb438-182">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="fb438-183">`AllowAnyOrigin` affecte les demandes préliminaires et l’en-tête `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="fb438-183">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="fb438-184">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fb438-184">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="fb438-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; définit la propriété <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> de la stratégie pour qu’elle soit une fonction qui permet aux origines de correspondre à un domaine générique configuré lors de l’évaluation si l’origine est autorisée.</span><span class="sxs-lookup"><span data-stu-id="fb438-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="fb438-186">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="fb438-186">Set the allowed HTTP methods</span></span>

<span data-ttu-id="fb438-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="fb438-188">Autorise toute méthode HTTP :</span><span class="sxs-lookup"><span data-stu-id="fb438-188">Allows any HTTP method:</span></span>
* <span data-ttu-id="fb438-189">Affecte les demandes préliminaires et l’en-tête `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="fb438-189">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="fb438-190">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fb438-190">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="fb438-191">Définir les en-têtes de demande autorisés</span><span class="sxs-lookup"><span data-stu-id="fb438-191">Set the allowed request headers</span></span>

<span data-ttu-id="fb438-192">Pour autoriser l’envoi d’en-têtes spécifiques dans une demande CORS, appelée *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="fb438-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fb438-193">Pour autoriser tous les en-têtes de demande d’auteur, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fb438-194">Ce paramètre affecte les demandes préliminaires et l’en-tête de `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="fb438-194">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="fb438-195">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fb438-195">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>


<span data-ttu-id="fb438-196">Une stratégie d’intergiciel (middleware) CORS correspond aux en-têtes spécifiques spécifiés par `WithHeaders` est possible uniquement lorsque les en-têtes envoyés dans `Access-Control-Request-Headers` correspondent exactement aux en-têtes indiqués dans `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="fb438-196">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="fb438-197">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="fb438-197">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="fb438-198">L’intergiciel (middleware) CORS refuse une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) n’est pas listé dans `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="fb438-198">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="fb438-199">L’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors.</span><span class="sxs-lookup"><span data-stu-id="fb438-199">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fb438-200">Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-200">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="fb438-201">Définir les en-têtes de réponse exposés</span><span class="sxs-lookup"><span data-stu-id="fb438-201">Set the exposed response headers</span></span>

<span data-ttu-id="fb438-202">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="fb438-202">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="fb438-203">Pour plus d’informations, consultez la page [relative au partage des ressources Cross-Origin W3C (terminologie) : en-tête de réponse simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="fb438-203">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="fb438-204">Les en-têtes de réponse qui sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="fb438-204">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="fb438-205">La spécification CORS appelle ces en *-têtes de réponse simples*en-têtes.</span><span class="sxs-lookup"><span data-stu-id="fb438-205">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="fb438-206">Pour mettre d’autres en-têtes à la disposition de l’application, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-206">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="fb438-207">Informations d’identification dans les demandes Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="fb438-207">Credentials in cross-origin requests</span></span>

<span data-ttu-id="fb438-208">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-208">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="fb438-209">Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-209">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="fb438-210">Les informations d’identification incluent les cookies et les schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb438-210">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="fb438-211">Pour envoyer des informations d’identification avec une demande Cross-Origin, le client doit définir `XMLHttpRequest.withCredentials` sur `true`.</span><span class="sxs-lookup"><span data-stu-id="fb438-211">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="fb438-212">Utilisation directe de `XMLHttpRequest` :</span><span class="sxs-lookup"><span data-stu-id="fb438-212">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="fb438-213">Utilisation de jQuery :</span><span class="sxs-lookup"><span data-stu-id="fb438-213">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="fb438-214">Utilisation de l' [API FETCH](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="fb438-214">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="fb438-215">Le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="fb438-215">The server must allow the credentials.</span></span> <span data-ttu-id="fb438-216">Pour autoriser les informations d’identification Cross-Origin, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-216">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="fb438-217">La réponse HTTP comprend un en-tête `Access-Control-Allow-Credentials`, qui indique au navigateur que le serveur autorise les informations d’identification pour une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-217">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="fb438-218">Si le navigateur envoie des informations d’identification mais que la réponse n’inclut pas d’en-tête de `Access-Control-Allow-Credentials` valide, le navigateur n’expose pas la réponse à l’application, et la demande Cross-Origin échoue.</span><span class="sxs-lookup"><span data-stu-id="fb438-218">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="fb438-219">L’autorisation des informations d’identification Cross-Origin est un risque pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="fb438-219">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="fb438-220">Un site Web dans un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à l’application pour le compte de l’utilisateur sans la connaissance de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fb438-220">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="fb438-221">La spécification CORS indique également que le paramètre Origins to `"*"` (All Origins) n’est pas valide si l’en-tête `Access-Control-Allow-Credentials` est présent.</span><span class="sxs-lookup"><span data-stu-id="fb438-221">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="fb438-222">Demandes préliminaires</span><span class="sxs-lookup"><span data-stu-id="fb438-222">Preflight requests</span></span>

<span data-ttu-id="fb438-223">Pour certaines demandes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-223">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="fb438-224">Cette demande porte le nom de *demande préliminaire*.</span><span class="sxs-lookup"><span data-stu-id="fb438-224">This request is called a *preflight request*.</span></span> <span data-ttu-id="fb438-225">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="fb438-225">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="fb438-226">La méthode de demande est : obtenir, début ou publication.</span><span class="sxs-lookup"><span data-stu-id="fb438-226">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="fb438-227">L’application ne définit pas les en-têtes de requête autres que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`ou `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="fb438-227">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="fb438-228">L’en-tête `Content-Type`, s’il est défini, a l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="fb438-228">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="fb438-229">La règle sur les en-têtes de demande définie pour la demande du client s’applique aux en-têtes définis par l’application en appelant `setRequestHeader` sur l’objet `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="fb438-229">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="fb438-230">La spécification CORS appelle ces en-têtes *créer des en-* têtes de demande.</span><span class="sxs-lookup"><span data-stu-id="fb438-230">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="fb438-231">La règle ne s’applique pas aux en-têtes que le navigateur peut définir, par exemple `User-Agent`, `Host`ou `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="fb438-231">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="fb438-232">Voici un exemple de demande préliminaire :</span><span class="sxs-lookup"><span data-stu-id="fb438-232">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="fb438-233">La requête de pré-vol utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="fb438-233">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="fb438-234">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="fb438-234">It includes two special headers:</span></span>

* <span data-ttu-id="fb438-235">`Access-Control-Request-Method`: méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-235">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="fb438-236">`Access-Control-Request-Headers`: liste des en-têtes de requête que l’application définit sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-236">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="fb438-237">Comme indiqué précédemment, cela n’inclut pas les en-têtes définis par le navigateur, tels que les `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="fb438-237">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="fb438-238">Une demande préliminaire CORS peut inclure un en-tête `Access-Control-Request-Headers`, qui indique au serveur les en-têtes envoyés avec la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-238">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="fb438-239">Pour autoriser des en-têtes spécifiques, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-239">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fb438-240">Pour autoriser tous les en-têtes de demande d’auteur, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-240">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fb438-241">Les navigateurs ne sont pas entièrement cohérents dans la façon dont ils définissent `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="fb438-241">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="fb438-242">Si vous définissez les en-têtes sur une valeur autre que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="fb438-242">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="fb438-243">Voici un exemple de réponse à la demande préliminaire (en supposant que le serveur autorise la demande) :</span><span class="sxs-lookup"><span data-stu-id="fb438-243">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="fb438-244">La réponse comprend un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="fb438-244">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="fb438-245">Si la demande préliminaire est réussie, le navigateur envoie la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-245">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="fb438-246">Si la demande préliminaire est refusée, l’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors.</span><span class="sxs-lookup"><span data-stu-id="fb438-246">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fb438-247">Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-247">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="fb438-248">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="fb438-248">Set the preflight expiration time</span></span>

<span data-ttu-id="fb438-249">L’en-tête `Access-Control-Max-Age` spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mise en cache.</span><span class="sxs-lookup"><span data-stu-id="fb438-249">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="fb438-250">Pour définir cet en-tête, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-250">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="fb438-251">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-251">How CORS works</span></span>

<span data-ttu-id="fb438-252">Cette section décrit ce qui se produit dans une demande [cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) au niveau des messages http.</span><span class="sxs-lookup"><span data-stu-id="fb438-252">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="fb438-253">CORS n’est **pas** une fonctionnalité de sécurité.</span><span class="sxs-lookup"><span data-stu-id="fb438-253">CORS is **not** a security feature.</span></span> <span data-ttu-id="fb438-254">CORS est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="fb438-254">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="fb438-255">Par exemple, un acteur malveillant peut utiliser l’option [empêcher les scripts inter-sites (XSS)](xref:security/cross-site-scripting) sur votre site et exécuter une requête intersite vers son site Active cors pour voler des informations.</span><span class="sxs-lookup"><span data-stu-id="fb438-255">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="fb438-256">Votre API n’est pas plus sûre en autorisant CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-256">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="fb438-257">C’est au client (navigateur) d’appliquer CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-257">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="fb438-258">Le serveur exécute la requête et retourne la réponse, c’est le client qui retourne une erreur et bloque la réponse.</span><span class="sxs-lookup"><span data-stu-id="fb438-258">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="fb438-259">Par exemple, l’un des outils suivants affiche la réponse du serveur :</span><span class="sxs-lookup"><span data-stu-id="fb438-259">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="fb438-260">Fiddler</span><span class="sxs-lookup"><span data-stu-id="fb438-260">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="fb438-261">Postman</span><span class="sxs-lookup"><span data-stu-id="fb438-261">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="fb438-262">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="fb438-262">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="fb438-263">Un navigateur Web en entrant l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="fb438-263">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="fb438-264">C’est un moyen pour un serveur d’autoriser les navigateurs à exécuter une requête d’API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) ou [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) Cross-Origin qui serait sinon interdite.</span><span class="sxs-lookup"><span data-stu-id="fb438-264">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="fb438-265">Les navigateurs (sans CORS) ne peuvent pas effectuer de demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-265">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="fb438-266">Avant CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) était utilisé pour contourner cette restriction.</span><span class="sxs-lookup"><span data-stu-id="fb438-266">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="fb438-267">JSONP n’utilise pas XHR, il utilise la balise `<script>` pour recevoir la réponse.</span><span class="sxs-lookup"><span data-stu-id="fb438-267">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="fb438-268">Les scripts sont autorisés à être chargés sur plusieurs origines.</span><span class="sxs-lookup"><span data-stu-id="fb438-268">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="fb438-269">La [spécification cors](https://www.w3.org/TR/cors/) a introduit plusieurs nouveaux en-têtes HTTP qui permettent des demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-269">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="fb438-270">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-270">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="fb438-271">Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-271">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="fb438-272">Voici un exemple de demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-272">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="fb438-273">L’en-tête `Origin` fournit le domaine du site qui effectue la requête.</span><span class="sxs-lookup"><span data-stu-id="fb438-273">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="fb438-274">L’en-tête `Origin` est obligatoire et doit être différent de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="fb438-274">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="fb438-275">Si le serveur autorise la demande, il définit l’en-tête `Access-Control-Allow-Origin` dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="fb438-275">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="fb438-276">La valeur de cet en-tête correspond à l’en-tête `Origin` de la requête ou à la valeur de caractère générique `"*"`, ce qui signifie que toute origine est autorisée :</span><span class="sxs-lookup"><span data-stu-id="fb438-276">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="fb438-277">Si la réponse n’inclut pas l’en-tête `Access-Control-Allow-Origin`, la demande Cross-Origin échoue.</span><span class="sxs-lookup"><span data-stu-id="fb438-277">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="fb438-278">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="fb438-278">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="fb438-279">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible pour l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="fb438-279">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="fb438-280">Tester CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-280">Test CORS</span></span>

<span data-ttu-id="fb438-281">Pour tester CORS :</span><span class="sxs-lookup"><span data-stu-id="fb438-281">To test CORS:</span></span>

1. <span data-ttu-id="fb438-282">[Créez un projet d’API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="fb438-282">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="fb438-283">Vous pouvez également [Télécharger l’exemple](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="fb438-283">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="fb438-284">Activez CORS à l’aide de l’une des approches décrites dans ce document.</span><span class="sxs-lookup"><span data-stu-id="fb438-284">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="fb438-285">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="fb438-285">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="fb438-286">`WithOrigins("https://localhost:<port>");` ne doit être utilisé que pour tester un exemple d’application semblable à l' [exemple de code de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="fb438-286">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="fb438-287">Créez un projet d’application Web (Razor Pages ou MVC).</span><span class="sxs-lookup"><span data-stu-id="fb438-287">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="fb438-288">L’exemple utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fb438-288">The sample uses Razor Pages.</span></span> <span data-ttu-id="fb438-289">Vous pouvez créer l’application Web dans la même solution que le projet d’API.</span><span class="sxs-lookup"><span data-stu-id="fb438-289">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="fb438-290">Ajoutez le code en surbrillance suivant au fichier *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="fb438-290">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="fb438-291">Dans le code précédent, remplacez `url: 'https://<web app>.azurewebsites.net/api/values/1',` par l’URL de l’application déployée.</span><span class="sxs-lookup"><span data-stu-id="fb438-291">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="fb438-292">Déployez le projet API.</span><span class="sxs-lookup"><span data-stu-id="fb438-292">Deploy the API project.</span></span> <span data-ttu-id="fb438-293">Par exemple, [déployez sur Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="fb438-293">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="fb438-294">Exécutez l’Razor Pages ou l’application MVC à partir du bureau, puis cliquez sur le bouton **test** .</span><span class="sxs-lookup"><span data-stu-id="fb438-294">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="fb438-295">Utilisez les outils F12 pour passer en revue les messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="fb438-295">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="fb438-296">Supprimez l’origine localhost de `WithOrigins` et déployez l’application.</span><span class="sxs-lookup"><span data-stu-id="fb438-296">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="fb438-297">Vous pouvez également exécuter l’application cliente avec un autre port.</span><span class="sxs-lookup"><span data-stu-id="fb438-297">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="fb438-298">Par exemple, exécutez à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fb438-298">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="fb438-299">Testez avec l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="fb438-299">Test with the client app.</span></span> <span data-ttu-id="fb438-300">Les échecs CORS retournent une erreur, mais le message d’erreur n’est pas disponible pour JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fb438-300">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="fb438-301">Utilisez l’onglet Console des outils F12 pour afficher l’erreur.</span><span class="sxs-lookup"><span data-stu-id="fb438-301">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="fb438-302">En fonction du navigateur, vous recevez une erreur (dans la console outils F12) semblable à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="fb438-302">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="fb438-303">Utilisation de Microsoft Edge :</span><span class="sxs-lookup"><span data-stu-id="fb438-303">Using Microsoft Edge:</span></span>

     <span data-ttu-id="fb438-304">**SEC7120 : [CORS] l’origine `https://localhost:44375` n’a pas trouvé `https://localhost:44375` dans l’en-tête de réponse Access-Control-allow-Origin pour la ressource Cross-Origin à `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="fb438-304">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="fb438-305">Utilisation de chrome :</span><span class="sxs-lookup"><span data-stu-id="fb438-305">Using Chrome:</span></span>

     <span data-ttu-id="fb438-306">**L’accès à XMLHttpRequest à `https://webapi.azurewebsites.net/api/values/1` à partir de l’origine `https://localhost:44375` a été bloqué par la stratégie CORS : aucun en-tête « Access-Control-allow-Origin » n’est présent sur la ressource demandée.**</span><span class="sxs-lookup"><span data-stu-id="fb438-306">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="fb438-307">Les points de terminaison compatibles CORS peuvent être testés à l’aide d’un outil tel que [Fiddler](https://www.telerik.com/fiddler) ou [postal](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="fb438-307">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="fb438-308">Lors de l’utilisation d’un outil, l’origine de la demande spécifiée par l’en-tête `Origin` doit différer de l’hôte recevant la demande.</span><span class="sxs-lookup"><span data-stu-id="fb438-308">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="fb438-309">Si la requête n’est pas *une origine croisée* en fonction de la valeur de l’en-tête `Origin` :</span><span class="sxs-lookup"><span data-stu-id="fb438-309">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="fb438-310">Il n’est pas nécessaire d’utiliser un intergiciel (middleware) CORS pour traiter la requête.</span><span class="sxs-lookup"><span data-stu-id="fb438-310">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="fb438-311">Les en-têtes CORS ne sont pas retournés dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="fb438-311">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="fb438-312">CORS dans IIS</span><span class="sxs-lookup"><span data-stu-id="fb438-312">CORS in IIS</span></span>

<span data-ttu-id="fb438-313">Lors du déploiement sur IIS, CORS doit s’exécuter avant l’authentification Windows si le serveur n’est pas configuré pour autoriser l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="fb438-313">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="fb438-314">Pour prendre en charge ce scénario, le [module cors IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) doit être installé et configuré pour l’application.</span><span class="sxs-lookup"><span data-stu-id="fb438-314">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb438-315">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fb438-315">Additional resources</span></span>

* [<span data-ttu-id="fb438-316">Partage des ressources Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="fb438-316">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="fb438-317">Prise en main du module IIS CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-317">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fb438-318">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb438-318">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fb438-319">Cet article explique comment activer CORS dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb438-319">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="fb438-320">La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web.</span><span class="sxs-lookup"><span data-stu-id="fb438-320">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="fb438-321">Cette restriction est appelée *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="fb438-321">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="fb438-322">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="fb438-322">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="fb438-323">Parfois, vous souhaiterez peut-être autoriser d’autres sites à effectuer des demandes Cross-Origin à votre application.</span><span class="sxs-lookup"><span data-stu-id="fb438-323">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="fb438-324">Pour plus d’informations, consultez l' [article Mozilla cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="fb438-324">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="fb438-325">[Partage des ressources Cross-Origin](https://www.w3.org/TR/cors/) (cors) :</span><span class="sxs-lookup"><span data-stu-id="fb438-325">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="fb438-326">Est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="fb438-326">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="fb438-327">N’est **pas** une fonctionnalité de sécurité, cors assouplit la sécurité.</span><span class="sxs-lookup"><span data-stu-id="fb438-327">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="fb438-328">Une API n’est pas plus sûre en autorisant CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-328">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="fb438-329">Pour plus d’informations, consultez fonctionnement de [cors](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="fb438-329">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="fb438-330">Permet à un serveur d’autoriser explicitement certaines demandes Cross-Origin tout en rejetant d’autres.</span><span class="sxs-lookup"><span data-stu-id="fb438-330">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="fb438-331">Est plus sûr et plus flexible que les techniques antérieures, telles que [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="fb438-331">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="fb438-332">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb438-332">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="fb438-333">Même origine</span><span class="sxs-lookup"><span data-stu-id="fb438-333">Same origin</span></span>

<span data-ttu-id="fb438-334">Deux URL ont la même origine si elles ont des schémas, des hôtes et des ports identiques ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="fb438-334">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="fb438-335">Ces deux URL ont le même origine :</span><span class="sxs-lookup"><span data-stu-id="fb438-335">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="fb438-336">Ces URL ont des origines différentes des deux précédentes URL :</span><span class="sxs-lookup"><span data-stu-id="fb438-336">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="fb438-337">`https://example.net` &ndash; domaine différent</span><span class="sxs-lookup"><span data-stu-id="fb438-337">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="fb438-338">`https://www.example.com/foo.html` &ndash; sous-domaine différent</span><span class="sxs-lookup"><span data-stu-id="fb438-338">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="fb438-339">`http://example.com/foo.html` &ndash; schéma différent</span><span class="sxs-lookup"><span data-stu-id="fb438-339">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="fb438-340">`https://example.com:9000/foo.html` &ndash; port différent</span><span class="sxs-lookup"><span data-stu-id="fb438-340">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="fb438-341">Internet Explorer ne prend pas en compte le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="fb438-341">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="fb438-342">CORS avec la stratégie et l’intergiciel (middleware) nommés</span><span class="sxs-lookup"><span data-stu-id="fb438-342">CORS with named policy and middleware</span></span>

<span data-ttu-id="fb438-343">L’intergiciel (middleware) CORS gère les demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-343">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="fb438-344">Le code suivant active CORS pour l’application entière avec l’origine spécifiée :</span><span class="sxs-lookup"><span data-stu-id="fb438-344">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="fb438-345">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="fb438-345">The preceding code:</span></span>

* <span data-ttu-id="fb438-346">Définit le nom de la stratégie sur «\_myAllowSpecificOrigins ».</span><span class="sxs-lookup"><span data-stu-id="fb438-346">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="fb438-347">Le nom de la stratégie est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="fb438-347">The policy name is arbitrary.</span></span>
* <span data-ttu-id="fb438-348">Appelle la méthode d’extension <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, qui active CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-348">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="fb438-349">Appelle <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> avec une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="fb438-349">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="fb438-350">L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fb438-350">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="fb438-351">Les [options de configuration](#cors-policy-options), telles que `WithOrigins`, sont décrites plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="fb438-351">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="fb438-352">L’appel de la méthode <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> ajoute des services CORS au conteneur de services de l’application :</span><span class="sxs-lookup"><span data-stu-id="fb438-352">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="fb438-353">Pour plus d’informations, consultez les [options de stratégie cors](#cpo) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="fb438-353">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="fb438-354">La méthode <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> peut chaîner des méthodes, comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="fb438-354">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="fb438-355">Remarque : l’URL **ne doit pas** contenir de barre oblique de fin (`/`).</span><span class="sxs-lookup"><span data-stu-id="fb438-355">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="fb438-356">Si l’URL se termine par `/`, la comparaison retourne `false` et aucun en-tête n’est retourné.</span><span class="sxs-lookup"><span data-stu-id="fb438-356">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="fb438-357">Le code suivant applique des stratégies CORS à tous les points de terminaison des applications par le biais de l’intergiciel (middleware) CORS :</span><span class="sxs-lookup"><span data-stu-id="fb438-357">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="fb438-358">Remarque : `UseCors` doit être appelé avant `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="fb438-358">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="fb438-359">Consultez [activer cors dans Razor pages, les contrôleurs et les méthodes d’action](#ecors) pour appliquer la stratégie cors au niveau de la page/du contrôleur/action.</span><span class="sxs-lookup"><span data-stu-id="fb438-359">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="fb438-360">Consultez la page [test cors](#test) pour obtenir des instructions sur le test du code précédent.</span><span class="sxs-lookup"><span data-stu-id="fb438-360">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="fb438-361">Activer CORS avec des attributs</span><span class="sxs-lookup"><span data-stu-id="fb438-361">Enable CORS with attributes</span></span>

<span data-ttu-id="fb438-362">L’attribut [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fournit une alternative à l’application de cors globalement.</span><span class="sxs-lookup"><span data-stu-id="fb438-362">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="fb438-363">L’attribut `[EnableCors]` active CORS pour les points de terminaison sélectionnés, plutôt que tous les points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="fb438-363">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="fb438-364">Utilisez `[EnableCors]` pour spécifier la stratégie par défaut et `[EnableCors("{Policy String}")]` pour spécifier une stratégie.</span><span class="sxs-lookup"><span data-stu-id="fb438-364">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="fb438-365">L’attribut `[EnableCors]` peut être appliqué aux éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fb438-365">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="fb438-366">`PageModel` de page Razor</span><span class="sxs-lookup"><span data-stu-id="fb438-366">Razor Page `PageModel`</span></span>
* <span data-ttu-id="fb438-367">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="fb438-367">Controller</span></span>
* <span data-ttu-id="fb438-368">Méthode d’action du contrôleur</span><span class="sxs-lookup"><span data-stu-id="fb438-368">Controller action method</span></span>

<span data-ttu-id="fb438-369">Vous pouvez appliquer différentes stratégies à Controller/page-Model/action avec l’attribut `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="fb438-369">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="fb438-370">Lorsque l’attribut `[EnableCors]` est appliqué à une méthode Controllers/Model-Model/action et que CORS est activé dans l’intergiciel (middleware), les deux stratégies sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="fb438-370">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="fb438-371">Nous vous recommandons de combiner les stratégies.</span><span class="sxs-lookup"><span data-stu-id="fb438-371">We recommend against combining policies.</span></span> <span data-ttu-id="fb438-372">Utilisez l’attribut `[EnableCors]` ou l’intergiciel (middleware), pas les deux dans la même application.</span><span class="sxs-lookup"><span data-stu-id="fb438-372">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="fb438-373">Le code suivant applique une stratégie différente à chaque méthode :</span><span class="sxs-lookup"><span data-stu-id="fb438-373">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="fb438-374">Le code suivant crée une stratégie CORS par défaut et une stratégie nommée `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="fb438-374">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="fb438-375">Désactiver CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-375">Disable CORS</span></span>

<span data-ttu-id="fb438-376">L’attribut [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) désactive cors pour le contrôleur/page-Model/action.</span><span class="sxs-lookup"><span data-stu-id="fb438-376">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="fb438-377">Options de stratégie CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-377">CORS policy options</span></span>

<span data-ttu-id="fb438-378">Cette section décrit les différentes options qui peuvent être définies dans une stratégie CORS :</span><span class="sxs-lookup"><span data-stu-id="fb438-378">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="fb438-379">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="fb438-379">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="fb438-380">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="fb438-380">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="fb438-381">Définir les en-têtes de demande autorisés</span><span class="sxs-lookup"><span data-stu-id="fb438-381">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="fb438-382">Définir les en-têtes de réponse exposés</span><span class="sxs-lookup"><span data-stu-id="fb438-382">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="fb438-383">Informations d’identification dans les demandes Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="fb438-383">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="fb438-384">Définir le délai d’expiration du prévols</span><span class="sxs-lookup"><span data-stu-id="fb438-384">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="fb438-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> est appelé dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fb438-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fb438-386">Pour certaines options, il peut être utile de lire d’abord la section [How cors Works](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="fb438-386">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="fb438-387">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="fb438-387">Set the allowed origins</span></span>

<span data-ttu-id="fb438-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; autorise les requêtes CORS de toutes les origines avec n’importe quel schéma (`http` ou `https`).</span><span class="sxs-lookup"><span data-stu-id="fb438-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="fb438-389">`AllowAnyOrigin` n’est pas sécurisé, car *tout site Web* peut effectuer des demandes Cross-Origin à l’application.</span><span class="sxs-lookup"><span data-stu-id="fb438-389">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="fb438-390">La spécification de `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner une falsification de requête intersites.</span><span class="sxs-lookup"><span data-stu-id="fb438-390">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="fb438-391">Pour une application sécurisée, spécifiez une liste exacte d’origines si le client doit s’autoriser à accéder aux ressources du serveur.</span><span class="sxs-lookup"><span data-stu-id="fb438-391">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="fb438-392">`AllowAnyOrigin` affecte les demandes préliminaires et l’en-tête `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="fb438-392">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="fb438-393">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fb438-393">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="fb438-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; définit la propriété <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> de la stratégie pour qu’elle soit une fonction qui permet aux origines de correspondre à un domaine générique configuré lors de l’évaluation si l’origine est autorisée.</span><span class="sxs-lookup"><span data-stu-id="fb438-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="fb438-395">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="fb438-395">Set the allowed HTTP methods</span></span>

<span data-ttu-id="fb438-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="fb438-397">Autorise toute méthode HTTP :</span><span class="sxs-lookup"><span data-stu-id="fb438-397">Allows any HTTP method:</span></span>
* <span data-ttu-id="fb438-398">Affecte les demandes préliminaires et l’en-tête `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="fb438-398">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="fb438-399">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fb438-399">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="fb438-400">Définir les en-têtes de demande autorisés</span><span class="sxs-lookup"><span data-stu-id="fb438-400">Set the allowed request headers</span></span>

<span data-ttu-id="fb438-401">Pour autoriser l’envoi d’en-têtes spécifiques dans une demande CORS, appelée *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="fb438-401">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fb438-402">Pour autoriser tous les en-têtes de demande d’auteur, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-402">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fb438-403">Ce paramètre affecte les demandes préliminaires et l’en-tête de `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="fb438-403">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="fb438-404">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fb438-404">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="fb438-405">L’intergiciel (middleware) CORS autorise toujours l’envoi de quatre en-têtes dans le `Access-Control-Request-Headers` indépendamment des valeurs configurées dans les en-têtes CorsPolicy.</span><span class="sxs-lookup"><span data-stu-id="fb438-405">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="fb438-406">Cette liste d’en-têtes comprend les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fb438-406">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="fb438-407">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="fb438-407">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="fb438-408">L’intergiciel (middleware) CORS répond correctement à une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` est toujours la liste blanche :</span><span class="sxs-lookup"><span data-stu-id="fb438-408">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="fb438-409">Définir les en-têtes de réponse exposés</span><span class="sxs-lookup"><span data-stu-id="fb438-409">Set the exposed response headers</span></span>

<span data-ttu-id="fb438-410">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="fb438-410">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="fb438-411">Pour plus d’informations, consultez la page [relative au partage des ressources Cross-Origin W3C (terminologie) : en-tête de réponse simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="fb438-411">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="fb438-412">Les en-têtes de réponse qui sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="fb438-412">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="fb438-413">La spécification CORS appelle ces en *-têtes de réponse simples*en-têtes.</span><span class="sxs-lookup"><span data-stu-id="fb438-413">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="fb438-414">Pour mettre d’autres en-têtes à la disposition de l’application, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-414">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="fb438-415">Informations d’identification dans les demandes Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="fb438-415">Credentials in cross-origin requests</span></span>

<span data-ttu-id="fb438-416">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-416">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="fb438-417">Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-417">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="fb438-418">Les informations d’identification incluent les cookies et les schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb438-418">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="fb438-419">Pour envoyer des informations d’identification avec une demande Cross-Origin, le client doit définir `XMLHttpRequest.withCredentials` sur `true`.</span><span class="sxs-lookup"><span data-stu-id="fb438-419">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="fb438-420">Utilisation directe de `XMLHttpRequest` :</span><span class="sxs-lookup"><span data-stu-id="fb438-420">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="fb438-421">Utilisation de jQuery :</span><span class="sxs-lookup"><span data-stu-id="fb438-421">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="fb438-422">Utilisation de l' [API FETCH](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="fb438-422">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="fb438-423">Le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="fb438-423">The server must allow the credentials.</span></span> <span data-ttu-id="fb438-424">Pour autoriser les informations d’identification Cross-Origin, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-424">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="fb438-425">La réponse HTTP comprend un en-tête `Access-Control-Allow-Credentials`, qui indique au navigateur que le serveur autorise les informations d’identification pour une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-425">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="fb438-426">Si le navigateur envoie des informations d’identification mais que la réponse n’inclut pas d’en-tête de `Access-Control-Allow-Credentials` valide, le navigateur n’expose pas la réponse à l’application, et la demande Cross-Origin échoue.</span><span class="sxs-lookup"><span data-stu-id="fb438-426">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="fb438-427">L’autorisation des informations d’identification Cross-Origin est un risque pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="fb438-427">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="fb438-428">Un site Web dans un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à l’application pour le compte de l’utilisateur sans la connaissance de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fb438-428">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="fb438-429">La spécification CORS indique également que le paramètre Origins to `"*"` (All Origins) n’est pas valide si l’en-tête `Access-Control-Allow-Credentials` est présent.</span><span class="sxs-lookup"><span data-stu-id="fb438-429">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="fb438-430">Demandes préliminaires</span><span class="sxs-lookup"><span data-stu-id="fb438-430">Preflight requests</span></span>

<span data-ttu-id="fb438-431">Pour certaines demandes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-431">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="fb438-432">Cette demande porte le nom de *demande préliminaire*.</span><span class="sxs-lookup"><span data-stu-id="fb438-432">This request is called a *preflight request*.</span></span> <span data-ttu-id="fb438-433">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="fb438-433">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="fb438-434">La méthode de demande est : obtenir, début ou publication.</span><span class="sxs-lookup"><span data-stu-id="fb438-434">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="fb438-435">L’application ne définit pas les en-têtes de requête autres que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`ou `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="fb438-435">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="fb438-436">L’en-tête `Content-Type`, s’il est défini, a l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="fb438-436">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="fb438-437">La règle sur les en-têtes de demande définie pour la demande du client s’applique aux en-têtes définis par l’application en appelant `setRequestHeader` sur l’objet `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="fb438-437">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="fb438-438">La spécification CORS appelle ces en-têtes *créer des en-* têtes de demande.</span><span class="sxs-lookup"><span data-stu-id="fb438-438">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="fb438-439">La règle ne s’applique pas aux en-têtes que le navigateur peut définir, par exemple `User-Agent`, `Host`ou `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="fb438-439">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="fb438-440">Voici un exemple de demande préliminaire :</span><span class="sxs-lookup"><span data-stu-id="fb438-440">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="fb438-441">La requête de pré-vol utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="fb438-441">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="fb438-442">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="fb438-442">It includes two special headers:</span></span>

* <span data-ttu-id="fb438-443">`Access-Control-Request-Method`: méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-443">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="fb438-444">`Access-Control-Request-Headers`: liste des en-têtes de requête que l’application définit sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-444">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="fb438-445">Comme indiqué précédemment, cela n’inclut pas les en-têtes définis par le navigateur, tels que les `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="fb438-445">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="fb438-446">Une demande préliminaire CORS peut inclure un en-tête `Access-Control-Request-Headers`, qui indique au serveur les en-têtes envoyés avec la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-446">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="fb438-447">Pour autoriser des en-têtes spécifiques, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-447">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fb438-448">Pour autoriser tous les en-têtes de demande d’auteur, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-448">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fb438-449">Les navigateurs ne sont pas entièrement cohérents dans la façon dont ils définissent `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="fb438-449">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="fb438-450">Si vous définissez les en-têtes sur une valeur autre que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="fb438-450">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="fb438-451">Voici un exemple de réponse à la demande préliminaire (en supposant que le serveur autorise la demande) :</span><span class="sxs-lookup"><span data-stu-id="fb438-451">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="fb438-452">La réponse comprend un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="fb438-452">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="fb438-453">Si la demande préliminaire est réussie, le navigateur envoie la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="fb438-453">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="fb438-454">Si la demande préliminaire est refusée, l’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors.</span><span class="sxs-lookup"><span data-stu-id="fb438-454">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fb438-455">Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-455">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="fb438-456">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="fb438-456">Set the preflight expiration time</span></span>

<span data-ttu-id="fb438-457">L’en-tête `Access-Control-Max-Age` spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mise en cache.</span><span class="sxs-lookup"><span data-stu-id="fb438-457">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="fb438-458">Pour définir cet en-tête, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="fb438-458">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="fb438-459">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-459">How CORS works</span></span>

<span data-ttu-id="fb438-460">Cette section décrit ce qui se produit dans une demande [cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) au niveau des messages http.</span><span class="sxs-lookup"><span data-stu-id="fb438-460">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="fb438-461">CORS n’est **pas** une fonctionnalité de sécurité.</span><span class="sxs-lookup"><span data-stu-id="fb438-461">CORS is **not** a security feature.</span></span> <span data-ttu-id="fb438-462">CORS est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="fb438-462">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="fb438-463">Par exemple, un acteur malveillant peut utiliser l’option [empêcher les scripts inter-sites (XSS)](xref:security/cross-site-scripting) sur votre site et exécuter une requête intersite vers son site Active cors pour voler des informations.</span><span class="sxs-lookup"><span data-stu-id="fb438-463">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="fb438-464">Votre API n’est pas plus sûre en autorisant CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-464">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="fb438-465">C’est au client (navigateur) d’appliquer CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-465">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="fb438-466">Le serveur exécute la requête et retourne la réponse, c’est le client qui retourne une erreur et bloque la réponse.</span><span class="sxs-lookup"><span data-stu-id="fb438-466">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="fb438-467">Par exemple, l’un des outils suivants affiche la réponse du serveur :</span><span class="sxs-lookup"><span data-stu-id="fb438-467">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="fb438-468">Fiddler</span><span class="sxs-lookup"><span data-stu-id="fb438-468">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="fb438-469">Postman</span><span class="sxs-lookup"><span data-stu-id="fb438-469">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="fb438-470">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="fb438-470">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="fb438-471">Un navigateur Web en entrant l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="fb438-471">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="fb438-472">C’est un moyen pour un serveur d’autoriser les navigateurs à exécuter une requête d’API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) ou [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) Cross-Origin qui serait sinon interdite.</span><span class="sxs-lookup"><span data-stu-id="fb438-472">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="fb438-473">Les navigateurs (sans CORS) ne peuvent pas effectuer de demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-473">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="fb438-474">Avant CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) était utilisé pour contourner cette restriction.</span><span class="sxs-lookup"><span data-stu-id="fb438-474">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="fb438-475">JSONP n’utilise pas XHR, il utilise la balise `<script>` pour recevoir la réponse.</span><span class="sxs-lookup"><span data-stu-id="fb438-475">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="fb438-476">Les scripts sont autorisés à être chargés sur plusieurs origines.</span><span class="sxs-lookup"><span data-stu-id="fb438-476">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="fb438-477">La [spécification cors](https://www.w3.org/TR/cors/) a introduit plusieurs nouveaux en-têtes HTTP qui permettent des demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-477">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="fb438-478">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-478">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="fb438-479">Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.</span><span class="sxs-lookup"><span data-stu-id="fb438-479">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="fb438-480">Voici un exemple de demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="fb438-480">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="fb438-481">L’en-tête `Origin` fournit le domaine du site qui effectue la requête.</span><span class="sxs-lookup"><span data-stu-id="fb438-481">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="fb438-482">L’en-tête `Origin` est obligatoire et doit être différent de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="fb438-482">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="fb438-483">Si le serveur autorise la demande, il définit l’en-tête `Access-Control-Allow-Origin` dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="fb438-483">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="fb438-484">La valeur de cet en-tête correspond à l’en-tête `Origin` de la requête ou à la valeur de caractère générique `"*"`, ce qui signifie que toute origine est autorisée :</span><span class="sxs-lookup"><span data-stu-id="fb438-484">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="fb438-485">Si la réponse n’inclut pas l’en-tête `Access-Control-Allow-Origin`, la demande Cross-Origin échoue.</span><span class="sxs-lookup"><span data-stu-id="fb438-485">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="fb438-486">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="fb438-486">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="fb438-487">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible pour l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="fb438-487">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="fb438-488">Tester CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-488">Test CORS</span></span>

<span data-ttu-id="fb438-489">Pour tester CORS :</span><span class="sxs-lookup"><span data-stu-id="fb438-489">To test CORS:</span></span>

1. <span data-ttu-id="fb438-490">[Créez un projet d’API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="fb438-490">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="fb438-491">Vous pouvez également [Télécharger l’exemple](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="fb438-491">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="fb438-492">Activez CORS à l’aide de l’une des approches décrites dans ce document.</span><span class="sxs-lookup"><span data-stu-id="fb438-492">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="fb438-493">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="fb438-493">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="fb438-494">`WithOrigins("https://localhost:<port>");` ne doit être utilisé que pour tester un exemple d’application semblable à l' [exemple de code de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="fb438-494">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="fb438-495">Créez un projet d’application Web (Razor Pages ou MVC).</span><span class="sxs-lookup"><span data-stu-id="fb438-495">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="fb438-496">L’exemple utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fb438-496">The sample uses Razor Pages.</span></span> <span data-ttu-id="fb438-497">Vous pouvez créer l’application Web dans la même solution que le projet d’API.</span><span class="sxs-lookup"><span data-stu-id="fb438-497">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="fb438-498">Ajoutez le code en surbrillance suivant au fichier *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="fb438-498">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="fb438-499">Dans le code précédent, remplacez `url: 'https://<web app>.azurewebsites.net/api/values/1',` par l’URL de l’application déployée.</span><span class="sxs-lookup"><span data-stu-id="fb438-499">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="fb438-500">Déployez le projet API.</span><span class="sxs-lookup"><span data-stu-id="fb438-500">Deploy the API project.</span></span> <span data-ttu-id="fb438-501">Par exemple, [déployez sur Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="fb438-501">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="fb438-502">Exécutez l’Razor Pages ou l’application MVC à partir du bureau, puis cliquez sur le bouton **test** .</span><span class="sxs-lookup"><span data-stu-id="fb438-502">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="fb438-503">Utilisez les outils F12 pour passer en revue les messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="fb438-503">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="fb438-504">Supprimez l’origine localhost de `WithOrigins` et déployez l’application.</span><span class="sxs-lookup"><span data-stu-id="fb438-504">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="fb438-505">Vous pouvez également exécuter l’application cliente avec un autre port.</span><span class="sxs-lookup"><span data-stu-id="fb438-505">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="fb438-506">Par exemple, exécutez à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fb438-506">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="fb438-507">Testez avec l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="fb438-507">Test with the client app.</span></span> <span data-ttu-id="fb438-508">Les échecs CORS retournent une erreur, mais le message d’erreur n’est pas disponible pour JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fb438-508">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="fb438-509">Utilisez l’onglet Console des outils F12 pour afficher l’erreur.</span><span class="sxs-lookup"><span data-stu-id="fb438-509">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="fb438-510">En fonction du navigateur, vous recevez une erreur (dans la console outils F12) semblable à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="fb438-510">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="fb438-511">Utilisation de Microsoft Edge :</span><span class="sxs-lookup"><span data-stu-id="fb438-511">Using Microsoft Edge:</span></span>

     <span data-ttu-id="fb438-512">**SEC7120 : [CORS] l’origine `https://localhost:44375` n’a pas trouvé `https://localhost:44375` dans l’en-tête de réponse Access-Control-allow-Origin pour la ressource Cross-Origin à `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="fb438-512">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="fb438-513">Utilisation de chrome :</span><span class="sxs-lookup"><span data-stu-id="fb438-513">Using Chrome:</span></span>

     <span data-ttu-id="fb438-514">**L’accès à XMLHttpRequest à `https://webapi.azurewebsites.net/api/values/1` à partir de l’origine `https://localhost:44375` a été bloqué par la stratégie CORS : aucun en-tête « Access-Control-allow-Origin » n’est présent sur la ressource demandée.**</span><span class="sxs-lookup"><span data-stu-id="fb438-514">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="fb438-515">Les points de terminaison compatibles CORS peuvent être testés à l’aide d’un outil tel que [Fiddler](https://www.telerik.com/fiddler) ou [postal](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="fb438-515">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="fb438-516">Lors de l’utilisation d’un outil, l’origine de la demande spécifiée par l’en-tête `Origin` doit différer de l’hôte recevant la demande.</span><span class="sxs-lookup"><span data-stu-id="fb438-516">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="fb438-517">Si la requête n’est pas *une origine croisée* en fonction de la valeur de l’en-tête `Origin` :</span><span class="sxs-lookup"><span data-stu-id="fb438-517">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="fb438-518">Il n’est pas nécessaire d’utiliser un intergiciel (middleware) CORS pour traiter la requête.</span><span class="sxs-lookup"><span data-stu-id="fb438-518">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="fb438-519">Les en-têtes CORS ne sont pas retournés dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="fb438-519">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="fb438-520">CORS dans IIS</span><span class="sxs-lookup"><span data-stu-id="fb438-520">CORS in IIS</span></span>

<span data-ttu-id="fb438-521">Lors du déploiement sur IIS, CORS doit s’exécuter avant l’authentification Windows si le serveur n’est pas configuré pour autoriser l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="fb438-521">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="fb438-522">Pour prendre en charge ce scénario, le [module cors IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) doit être installé et configuré pour l’application.</span><span class="sxs-lookup"><span data-stu-id="fb438-522">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb438-523">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fb438-523">Additional resources</span></span>

* [<span data-ttu-id="fb438-524">Partage des ressources Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="fb438-524">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="fb438-525">Prise en main du module IIS CORS</span><span class="sxs-lookup"><span data-stu-id="fb438-525">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end