---
title: Activer les requêtes Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme norme pour autoriser ou rejeter des demandes Cross-Origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a02b3497684979c1a9e792437f9f1a4c467600f0
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187255"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="828bb-103">Activer les requêtes Cross-Origin (CORS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="828bb-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="828bb-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="828bb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="828bb-105">Cet article explique comment activer CORS dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="828bb-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="828bb-106">La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web.</span><span class="sxs-lookup"><span data-stu-id="828bb-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="828bb-107">Cette restriction est appelée *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="828bb-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="828bb-108">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="828bb-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="828bb-109">Parfois, vous souhaiterez peut-être autoriser d’autres sites à effectuer des demandes Cross-Origin à votre application.</span><span class="sxs-lookup"><span data-stu-id="828bb-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="828bb-110">Pour plus d’informations, consultez l' [article Mozilla cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="828bb-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="828bb-111">[Partage des ressources Cross-Origin](https://www.w3.org/TR/cors/) (CORS) :</span><span class="sxs-lookup"><span data-stu-id="828bb-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="828bb-112">Est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="828bb-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="828bb-113">N’est **pas** une fonctionnalité de sécurité, cors assouplit la sécurité.</span><span class="sxs-lookup"><span data-stu-id="828bb-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="828bb-114">Une API n’est pas plus sûre en autorisant CORS.</span><span class="sxs-lookup"><span data-stu-id="828bb-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="828bb-115">Pour plus d’informations, consultez fonctionnement de [cors](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="828bb-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="828bb-116">Permet à un serveur d’autoriser explicitement certaines demandes Cross-Origin tout en rejetant d’autres.</span><span class="sxs-lookup"><span data-stu-id="828bb-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="828bb-117">Est plus sûr et plus flexible que les techniques antérieures, telles que [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="828bb-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="828bb-118">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="828bb-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="828bb-119">Même origine</span><span class="sxs-lookup"><span data-stu-id="828bb-119">Same origin</span></span>

<span data-ttu-id="828bb-120">Deux URL ont la même origine si elles ont des schémas, des hôtes et des ports identiques ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="828bb-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="828bb-121">Ces deux URL ont le même origine :</span><span class="sxs-lookup"><span data-stu-id="828bb-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="828bb-122">Ces URL ont des origines différentes des deux précédentes URL :</span><span class="sxs-lookup"><span data-stu-id="828bb-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="828bb-123">`https://example.net`&ndash; Domaine différent</span><span class="sxs-lookup"><span data-stu-id="828bb-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="828bb-124">`https://www.example.com/foo.html`&ndash; Autre sous-domaine</span><span class="sxs-lookup"><span data-stu-id="828bb-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="828bb-125">`http://example.com/foo.html`&ndash; Schéma différent</span><span class="sxs-lookup"><span data-stu-id="828bb-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="828bb-126">`https://example.com:9000/foo.html`&ndash; Autre port</span><span class="sxs-lookup"><span data-stu-id="828bb-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="828bb-127">Internet Explorer ne prend pas en compte le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="828bb-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="828bb-128">CORS avec la stratégie et l’intergiciel (middleware) nommés</span><span class="sxs-lookup"><span data-stu-id="828bb-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="828bb-129">L’intergiciel (middleware) CORS gère les demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="828bb-130">Le code suivant active CORS pour l’application entière avec l’origine spécifiée :</span><span class="sxs-lookup"><span data-stu-id="828bb-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="828bb-131">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="828bb-131">The preceding code:</span></span>

* <span data-ttu-id="828bb-132">Définit le nom de la stratégie\_sur « myAllowSpecificOrigins ».</span><span class="sxs-lookup"><span data-stu-id="828bb-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="828bb-133">Le nom de la stratégie est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="828bb-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="828bb-134">Appelle la <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> méthode d’extension, qui active cors.</span><span class="sxs-lookup"><span data-stu-id="828bb-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="828bb-135">Appelle <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> avec une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="828bb-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="828bb-136">L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="828bb-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="828bb-137">Les [options de configuration](#cors-policy-options), `WithOrigins`telles que, sont décrites plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="828bb-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="828bb-138">L' <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> appel de méthode ajoute des services cors au conteneur de services de l’application :</span><span class="sxs-lookup"><span data-stu-id="828bb-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="828bb-139">Pour plus d’informations, consultez les [options de stratégie cors](#cpo) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="828bb-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="828bb-140">La <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> méthode peut chaîner des méthodes, comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="828bb-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="828bb-141">Remarque : L’URL **ne doit pas** contenir de barre oblique finale`/`().</span><span class="sxs-lookup"><span data-stu-id="828bb-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="828bb-142">Si l’URL se termine `/`par, la comparaison `false` retourne et aucun en-tête n’est retourné.</span><span class="sxs-lookup"><span data-stu-id="828bb-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="828bb-143">Le code suivant applique des stratégies CORS à tous les points de terminaison des applications par le biais de l’intergiciel (middleware) CORS :</span><span class="sxs-lookup"><span data-stu-id="828bb-143">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> <span data-ttu-id="828bb-144">Avec le routage de point de terminaison, l’intergiciel (middleware) cors doit être configuré `UseRouting` pour `UseEndpoints`s’exécuter entre les appels à et.</span><span class="sxs-lookup"><span data-stu-id="828bb-144">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="828bb-145">Une configuration incorrecte entraîne l’arrêt du fonctionnement correct de l’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="828bb-145">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="828bb-146">Le code suivant applique des stratégies CORS à tous les points de terminaison des applications par le biais de l’intergiciel (middleware) CORS :</span><span class="sxs-lookup"><span data-stu-id="828bb-146">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="828bb-147">Remarque : `UseCors` doit être appelé avant `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="828bb-147">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="828bb-148">Consultez [activer cors dans Razor pages, les contrôleurs et les méthodes d’action](#ecors) pour appliquer la stratégie cors au niveau de la page/du contrôleur/action.</span><span class="sxs-lookup"><span data-stu-id="828bb-148">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="828bb-149">Consultez la page [test cors](#test) pour obtenir des instructions sur le test du code précédent.</span><span class="sxs-lookup"><span data-stu-id="828bb-149">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="828bb-150">Activer cors avec routage du point de terminaison</span><span class="sxs-lookup"><span data-stu-id="828bb-150">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="828bb-151">Avec le routage de point de terminaison, cors peut être activé sur une base par `RequireCors` point de terminaison à l’aide de l’ensemble de méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="828bb-151">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="828bb-152">De même, CORS peut également être activé pour tous les contrôleurs :</span><span class="sxs-lookup"><span data-stu-id="828bb-152">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="828bb-153">Activer CORS avec des attributs</span><span class="sxs-lookup"><span data-stu-id="828bb-153">Enable CORS with attributes</span></span>

<span data-ttu-id="828bb-154">L' [ &lbrack;attributEnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fournit une alternative à l’application de cors globalement.</span><span class="sxs-lookup"><span data-stu-id="828bb-154">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="828bb-155">L' `[EnableCors]` attribut Active cors pour les points de terminaison sélectionnés, plutôt que tous les points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="828bb-155">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="828bb-156">Utilisez `[EnableCors]` pour spécifier la stratégie par défaut `[EnableCors("{Policy String}")]` et spécifier une stratégie.</span><span class="sxs-lookup"><span data-stu-id="828bb-156">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="828bb-157">L' `[EnableCors]` attribut peut être appliqué aux éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="828bb-157">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="828bb-158">Page Razor`PageModel`</span><span class="sxs-lookup"><span data-stu-id="828bb-158">Razor Page `PageModel`</span></span>
* <span data-ttu-id="828bb-159">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="828bb-159">Controller</span></span>
* <span data-ttu-id="828bb-160">Méthode d’action du contrôleur</span><span class="sxs-lookup"><span data-stu-id="828bb-160">Controller action method</span></span>

<span data-ttu-id="828bb-161">Vous pouvez appliquer différentes stratégies à Controller/page-Model/action avec l' `[EnableCors]` attribut.</span><span class="sxs-lookup"><span data-stu-id="828bb-161">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="828bb-162">Lorsque l' `[EnableCors]` attribut est appliqué à une méthode Controllers/Model-Model/action et que cors est activé dans l’intergiciel (middleware), les deux stratégies sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="828bb-162">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="828bb-163">Nous vous recommandons de combiner les stratégies.</span><span class="sxs-lookup"><span data-stu-id="828bb-163">We recommend against combining policies.</span></span> <span data-ttu-id="828bb-164">Utilisez l' `[EnableCors]` attribut ou l’intergiciel (middleware), pas les deux dans la même application.</span><span class="sxs-lookup"><span data-stu-id="828bb-164">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="828bb-165">Le code suivant applique une stratégie différente à chaque méthode :</span><span class="sxs-lookup"><span data-stu-id="828bb-165">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="828bb-166">Le code suivant crée une stratégie CORS par défaut et une stratégie `"AnotherPolicy"`nommée :</span><span class="sxs-lookup"><span data-stu-id="828bb-166">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="828bb-167">Désactiver CORS</span><span class="sxs-lookup"><span data-stu-id="828bb-167">Disable CORS</span></span>

<span data-ttu-id="828bb-168">[ L'&lbrack;attributDisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) désactive cors pour le contrôleur/page-Model/action.</span><span class="sxs-lookup"><span data-stu-id="828bb-168">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="828bb-169">Options de stratégie CORS</span><span class="sxs-lookup"><span data-stu-id="828bb-169">CORS policy options</span></span>

<span data-ttu-id="828bb-170">Cette section décrit les différentes options qui peuvent être définies dans une stratégie CORS :</span><span class="sxs-lookup"><span data-stu-id="828bb-170">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="828bb-171">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="828bb-171">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="828bb-172">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="828bb-172">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="828bb-173">Définir les en-têtes de demande autorisés</span><span class="sxs-lookup"><span data-stu-id="828bb-173">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="828bb-174">Définir les en-têtes de réponse exposés</span><span class="sxs-lookup"><span data-stu-id="828bb-174">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="828bb-175">Informations d’identification dans les demandes Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="828bb-175">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="828bb-176">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="828bb-176">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="828bb-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>est appelé dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="828bb-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="828bb-178">Pour certaines options, il peut être utile de lire d’abord la section [How cors Works](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="828bb-178">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="828bb-179">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="828bb-179">Set the allowed origins</span></span>

<span data-ttu-id="828bb-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Autorise les requêtes cors de toutes les origines avec n'`http` importe `https`quel schéma (ou). &ndash;</span><span class="sxs-lookup"><span data-stu-id="828bb-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="828bb-181">`AllowAnyOrigin`n’est pas sécurisé, car *un site Web* peut effectuer des demandes Cross-Origin à l’application.</span><span class="sxs-lookup"><span data-stu-id="828bb-181">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="828bb-182">La `AllowAnyOrigin` spécification `AllowCredentials` de et de est une configuration non sécurisée et peut entraîner une falsification de requête intersites.</span><span class="sxs-lookup"><span data-stu-id="828bb-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="828bb-183">Le service CORS retourne une réponse CORS non valide lorsqu’une application est configurée avec les deux méthodes.</span><span class="sxs-lookup"><span data-stu-id="828bb-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="828bb-184">La `AllowAnyOrigin` spécification `AllowCredentials` de et de est une configuration non sécurisée et peut entraîner une falsification de requête intersites.</span><span class="sxs-lookup"><span data-stu-id="828bb-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="828bb-185">Pour une application sécurisée, spécifiez une liste exacte d’origines si le client doit s’autoriser à accéder aux ressources du serveur.</span><span class="sxs-lookup"><span data-stu-id="828bb-185">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="828bb-186">`AllowAnyOrigin`affecte les demandes préliminaires et `Access-Control-Allow-Origin` l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="828bb-186">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="828bb-187">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="828bb-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="828bb-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Définit la<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propriété de la stratégie pour qu’elle soit une fonction qui permet aux origines de correspondre à un domaine générique configuré lors de l’évaluation si l’origine est autorisée.</span><span class="sxs-lookup"><span data-stu-id="828bb-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="828bb-189">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="828bb-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="828bb-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="828bb-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="828bb-191">Autorise toute méthode HTTP :</span><span class="sxs-lookup"><span data-stu-id="828bb-191">Allows any HTTP method:</span></span>
* <span data-ttu-id="828bb-192">Affecte les demandes préliminaires et `Access-Control-Allow-Methods` l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="828bb-192">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="828bb-193">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="828bb-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="828bb-194">Définir les en-têtes de demande autorisés</span><span class="sxs-lookup"><span data-stu-id="828bb-194">Set the allowed request headers</span></span>

<span data-ttu-id="828bb-195">Pour autoriser l’envoi d’en-têtes spécifiques dans une demande cors, appelée *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="828bb-195">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="828bb-196">Pour autoriser tous les en-têtes de demande <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>d’auteur, appelez :</span><span class="sxs-lookup"><span data-stu-id="828bb-196">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="828bb-197">Ce paramètre affecte les demandes préliminaires et `Access-Control-Request-Headers` l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="828bb-197">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="828bb-198">Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="828bb-198">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="828bb-199">Une stratégie d’intergiciel (middleware) cors correspondant à des en `WithHeaders` -têtes spécifiques spécifiés par n’est possible `Access-Control-Request-Headers` que lorsque les en-têtes envoyés `WithHeaders`dans correspondent exactement aux en-têtes indiqués dans.</span><span class="sxs-lookup"><span data-stu-id="828bb-199">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="828bb-200">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="828bb-200">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="828bb-201">L’intergiciel (middleware) cors refuse une demande préliminaire avec l’en-tête `Content-Language` de demande suivant, car ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) n’est pas listé dans `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="828bb-201">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="828bb-202">L’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors.</span><span class="sxs-lookup"><span data-stu-id="828bb-202">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="828bb-203">Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-203">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="828bb-204">L’intergiciel (middleware) cors autorise toujours l’envoi `Access-Control-Request-Headers` de quatre en-têtes dans le, quelles que soient les valeurs configurées dans les en-têtes CorsPolicy.</span><span class="sxs-lookup"><span data-stu-id="828bb-204">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="828bb-205">Cette liste d’en-têtes comprend les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="828bb-205">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="828bb-206">Par exemple, considérez une application configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="828bb-206">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="828bb-207">L’intergiciel (middleware) cors répond correctement à une demande préliminaire avec l’en- `Content-Language` tête de demande suivant, car est toujours ajouté à la liste blanche :</span><span class="sxs-lookup"><span data-stu-id="828bb-207">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="828bb-208">Définir les en-têtes de réponse exposés</span><span class="sxs-lookup"><span data-stu-id="828bb-208">Set the exposed response headers</span></span>

<span data-ttu-id="828bb-209">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="828bb-209">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="828bb-210">Pour plus d’informations, [consultez partage des ressources Cross-Origin W3C (terminologie) : En-tête](https://www.w3.org/TR/cors/#simple-response-header)de réponse simple.</span><span class="sxs-lookup"><span data-stu-id="828bb-210">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="828bb-211">Les en-têtes de réponse qui sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="828bb-211">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="828bb-212">La spécification CORS appelle ces en *-têtes de réponse simples*en-têtes.</span><span class="sxs-lookup"><span data-stu-id="828bb-212">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="828bb-213">Pour mettre d’autres en-têtes à la disposition de <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>l’application, appelez :</span><span class="sxs-lookup"><span data-stu-id="828bb-213">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="828bb-214">Informations d’identification dans les demandes Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="828bb-214">Credentials in cross-origin requests</span></span>

<span data-ttu-id="828bb-215">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="828bb-215">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="828bb-216">Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-216">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="828bb-217">Les informations d’identification incluent les cookies et les schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="828bb-217">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="828bb-218">Pour envoyer des informations d’identification avec une demande Cross-Origin, le client `XMLHttpRequest.withCredentials` doit `true`affecter à la valeur.</span><span class="sxs-lookup"><span data-stu-id="828bb-218">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="828bb-219">Utilisation `XMLHttpRequest` directe :</span><span class="sxs-lookup"><span data-stu-id="828bb-219">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="828bb-220">Utilisation de jQuery :</span><span class="sxs-lookup"><span data-stu-id="828bb-220">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="828bb-221">Utilisation de l' [API FETCH](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="828bb-221">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="828bb-222">Le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="828bb-222">The server must allow the credentials.</span></span> <span data-ttu-id="828bb-223">Pour autoriser les informations d’identification Cross-Origin <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>, appelez :</span><span class="sxs-lookup"><span data-stu-id="828bb-223">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="828bb-224">La réponse http comprend un `Access-Control-Allow-Credentials` en-tête qui indique au navigateur que le serveur autorise les informations d’identification pour une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-224">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="828bb-225">Si le navigateur envoie des informations d’identification mais que la réponse n' `Access-Control-Allow-Credentials` inclut pas d’en-tête valide, le navigateur n’expose pas la réponse à l’application, et la demande Cross-Origin échoue.</span><span class="sxs-lookup"><span data-stu-id="828bb-225">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="828bb-226">L’autorisation des informations d’identification Cross-Origin est un risque pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="828bb-226">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="828bb-227">Un site Web dans un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à l’application pour le compte de l’utilisateur sans la connaissance de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="828bb-227">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="828bb-228">La spécification cors indique également que le paramètre Origins to `"*"` (All Origins) n’est pas valide si l' `Access-Control-Allow-Credentials` en-tête est présent.</span><span class="sxs-lookup"><span data-stu-id="828bb-228">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="828bb-229">Demandes préliminaires</span><span class="sxs-lookup"><span data-stu-id="828bb-229">Preflight requests</span></span>

<span data-ttu-id="828bb-230">Pour certaines demandes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="828bb-230">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="828bb-231">Cette demande porte le nom de *demande préliminaire*.</span><span class="sxs-lookup"><span data-stu-id="828bb-231">This request is called a *preflight request*.</span></span> <span data-ttu-id="828bb-232">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="828bb-232">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="828bb-233">La méthode de demande est : obtenir, début ou publication.</span><span class="sxs-lookup"><span data-stu-id="828bb-233">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="828bb-234">L’application ne définit pas les en-têtes `Accept`de `Accept-Language`requête autres `Content-Type`que, `Last-Event-ID`, `Content-Language`, ou.</span><span class="sxs-lookup"><span data-stu-id="828bb-234">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="828bb-235">L' `Content-Type` en-tête, s’il est défini, a l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="828bb-235">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="828bb-236">La règle sur les en-têtes de demande définie pour la demande du client s’applique aux en-têtes `setRequestHeader` définis par `XMLHttpRequest` l’application en appelant sur l’objet.</span><span class="sxs-lookup"><span data-stu-id="828bb-236">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="828bb-237">La spécification CORS appelle ces en-têtes *créer des en-* têtes de demande.</span><span class="sxs-lookup"><span data-stu-id="828bb-237">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="828bb-238">La règle ne s’applique pas aux en-têtes que le navigateur peut `User-Agent`définir `Host`, tels `Content-Length`que, ou.</span><span class="sxs-lookup"><span data-stu-id="828bb-238">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="828bb-239">Voici un exemple de demande préliminaire :</span><span class="sxs-lookup"><span data-stu-id="828bb-239">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="828bb-240">La requête de pré-vol utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="828bb-240">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="828bb-241">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="828bb-241">It includes two special headers:</span></span>

* <span data-ttu-id="828bb-242">`Access-Control-Request-Method`: Méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="828bb-242">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="828bb-243">`Access-Control-Request-Headers`: Liste des en-têtes de requête que l’application définit sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="828bb-243">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="828bb-244">Comme indiqué précédemment, cela n’inclut pas les en-têtes définis par le navigateur `User-Agent`, tels que.</span><span class="sxs-lookup"><span data-stu-id="828bb-244">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="828bb-245">Une demande préliminaire cors peut inclure un `Access-Control-Request-Headers` en-tête, qui indique au serveur les en-têtes envoyés avec la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="828bb-245">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="828bb-246">Pour autoriser des en-têtes spécifiques <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>, appelez :</span><span class="sxs-lookup"><span data-stu-id="828bb-246">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="828bb-247">Pour autoriser tous les en-têtes de demande <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>d’auteur, appelez :</span><span class="sxs-lookup"><span data-stu-id="828bb-247">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="828bb-248">Les navigateurs ne sont pas entièrement cohérents `Access-Control-Request-Headers`dans la façon dont ils sont définis.</span><span class="sxs-lookup"><span data-stu-id="828bb-248">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="828bb-249">Si vous définissez des en-têtes autres que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`, et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="828bb-249">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="828bb-250">Voici un exemple de réponse à la demande préliminaire (en supposant que le serveur autorise la demande) :</span><span class="sxs-lookup"><span data-stu-id="828bb-250">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="828bb-251">La réponse comprend un `Access-Control-Allow-Methods` en-tête qui répertorie les méthodes autorisées et `Access-Control-Allow-Headers` éventuellement un en-tête, qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="828bb-251">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="828bb-252">Si la demande préliminaire est réussie, le navigateur envoie la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="828bb-252">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="828bb-253">Si la demande préliminaire est refusée, l’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors.</span><span class="sxs-lookup"><span data-stu-id="828bb-253">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="828bb-254">Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-254">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="828bb-255">Définir en amont le délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="828bb-255">Set the preflight expiration time</span></span>

<span data-ttu-id="828bb-256">L' `Access-Control-Max-Age` en-tête spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mise en cache.</span><span class="sxs-lookup"><span data-stu-id="828bb-256">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="828bb-257">Pour définir cet en-tête <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>, appelez :</span><span class="sxs-lookup"><span data-stu-id="828bb-257">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="828bb-258">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="828bb-258">How CORS works</span></span>

<span data-ttu-id="828bb-259">Cette section décrit ce qui se produit dans une demande [cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) au niveau des messages http.</span><span class="sxs-lookup"><span data-stu-id="828bb-259">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="828bb-260">CORS n’est **pas** une fonctionnalité de sécurité.</span><span class="sxs-lookup"><span data-stu-id="828bb-260">CORS is **not** a security feature.</span></span> <span data-ttu-id="828bb-261">CORS est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="828bb-261">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="828bb-262">Par exemple, un acteur malveillant peut utiliser l’option [empêcher les scripts inter-sites (XSS)](xref:security/cross-site-scripting) sur votre site et exécuter une requête intersite vers son site Active cors pour voler des informations.</span><span class="sxs-lookup"><span data-stu-id="828bb-262">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="828bb-263">Votre API n’est pas plus sûre en autorisant CORS.</span><span class="sxs-lookup"><span data-stu-id="828bb-263">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="828bb-264">C’est au client (navigateur) d’appliquer CORS.</span><span class="sxs-lookup"><span data-stu-id="828bb-264">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="828bb-265">Le serveur exécute la requête et retourne la réponse, c’est le client qui retourne une erreur et bloque la réponse.</span><span class="sxs-lookup"><span data-stu-id="828bb-265">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="828bb-266">Par exemple, l’un des outils suivants affiche la réponse du serveur :</span><span class="sxs-lookup"><span data-stu-id="828bb-266">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="828bb-267">Fiddler</span><span class="sxs-lookup"><span data-stu-id="828bb-267">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="828bb-268">Postman</span><span class="sxs-lookup"><span data-stu-id="828bb-268">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="828bb-269">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="828bb-269">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="828bb-270">Un navigateur Web en entrant l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="828bb-270">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="828bb-271">C’est un moyen pour un serveur d’autoriser les navigateurs à exécuter une requête d’API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) ou [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) Cross-Origin qui serait sinon interdite.</span><span class="sxs-lookup"><span data-stu-id="828bb-271">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="828bb-272">Les navigateurs (sans CORS) ne peuvent pas effectuer de demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-272">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="828bb-273">Avant CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) était utilisé pour contourner cette restriction.</span><span class="sxs-lookup"><span data-stu-id="828bb-273">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="828bb-274">JSONP n’utilise pas XHR, elle utilise `<script>` la balise pour recevoir la réponse.</span><span class="sxs-lookup"><span data-stu-id="828bb-274">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="828bb-275">Les scripts sont autorisés à être chargés sur plusieurs origines.</span><span class="sxs-lookup"><span data-stu-id="828bb-275">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="828bb-276">La [spécification cors](https://www.w3.org/TR/cors/) a introduit plusieurs nouveaux en-têtes HTTP qui permettent des demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-276">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="828bb-277">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-277">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="828bb-278">Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.</span><span class="sxs-lookup"><span data-stu-id="828bb-278">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="828bb-279">Voici un exemple de demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="828bb-279">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="828bb-280">L' `Origin` en-tête fournit le domaine du site qui effectue la requête :</span><span class="sxs-lookup"><span data-stu-id="828bb-280">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="828bb-281">Si le serveur autorise la demande, il définit l' `Access-Control-Allow-Origin` en-tête dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="828bb-281">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="828bb-282">La valeur de cet en-tête correspond `Origin` à l’en-tête de la demande ou `"*"`à la valeur du caractère générique, ce qui signifie que toute origine est autorisée :</span><span class="sxs-lookup"><span data-stu-id="828bb-282">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="828bb-283">Si la réponse n’inclut pas `Access-Control-Allow-Origin` l’en-tête, la demande Cross-Origin échoue.</span><span class="sxs-lookup"><span data-stu-id="828bb-283">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="828bb-284">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="828bb-284">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="828bb-285">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible pour l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="828bb-285">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="828bb-286">Test CORS</span><span class="sxs-lookup"><span data-stu-id="828bb-286">Test CORS</span></span>

<span data-ttu-id="828bb-287">Pour tester CORS :</span><span class="sxs-lookup"><span data-stu-id="828bb-287">To test CORS:</span></span>

1. <span data-ttu-id="828bb-288">[Créez un projet d’API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="828bb-288">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="828bb-289">Vous pouvez également [Télécharger l’exemple](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="828bb-289">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="828bb-290">Activez CORS à l’aide de l’une des approches décrites dans ce document.</span><span class="sxs-lookup"><span data-stu-id="828bb-290">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="828bb-291">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="828bb-291">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="828bb-292">`WithOrigins("https://localhost:<port>");`doit uniquement être utilisé pour tester un exemple d’application semblable à l' [exemple de code de téléchargement](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="828bb-292">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="828bb-293">Créez un projet d’application Web (Razor Pages ou MVC).</span><span class="sxs-lookup"><span data-stu-id="828bb-293">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="828bb-294">L’exemple utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="828bb-294">The sample uses Razor Pages.</span></span> <span data-ttu-id="828bb-295">Vous pouvez créer l’application Web dans la même solution que le projet d’API.</span><span class="sxs-lookup"><span data-stu-id="828bb-295">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="828bb-296">Ajoutez le code en surbrillance suivant au fichier *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="828bb-296">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="828bb-297">Dans le code précédent, remplacez `url: 'https://<web app>.azurewebsites.net/api/values/1',` par l’URL de l’application déployée.</span><span class="sxs-lookup"><span data-stu-id="828bb-297">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="828bb-298">Déployez le projet API.</span><span class="sxs-lookup"><span data-stu-id="828bb-298">Deploy the API project.</span></span> <span data-ttu-id="828bb-299">Par exemple, [déployez sur Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="828bb-299">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="828bb-300">Exécutez l’Razor Pages ou l’application MVC à partir du bureau, puis cliquez sur le bouton **test** .</span><span class="sxs-lookup"><span data-stu-id="828bb-300">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="828bb-301">Utilisez les outils F12 pour passer en revue les messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="828bb-301">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="828bb-302">Supprimez l’origine localhost `WithOrigins` de et déployez l’application.</span><span class="sxs-lookup"><span data-stu-id="828bb-302">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="828bb-303">Vous pouvez également exécuter l’application cliente avec un autre port.</span><span class="sxs-lookup"><span data-stu-id="828bb-303">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="828bb-304">Par exemple, exécutez à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="828bb-304">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="828bb-305">Testez avec l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="828bb-305">Test with the client app.</span></span> <span data-ttu-id="828bb-306">Les échecs CORS retournent une erreur, mais le message d’erreur n’est pas disponible pour JavaScript.</span><span class="sxs-lookup"><span data-stu-id="828bb-306">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="828bb-307">Utilisez l’onglet Console des outils F12 pour afficher l’erreur.</span><span class="sxs-lookup"><span data-stu-id="828bb-307">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="828bb-308">En fonction du navigateur, vous recevez une erreur (dans la console outils F12) semblable à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="828bb-308">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="828bb-309">Utilisation de Microsoft Edge :</span><span class="sxs-lookup"><span data-stu-id="828bb-309">Using Microsoft Edge:</span></span>

     <span data-ttu-id="828bb-310">**SEC7120 : [cors] l’origine `https://localhost:44375` n’a pas `https://localhost:44375` été trouvée dans l’en-tête de réponse Access-Control-allow-Origin pour la ressource Cross-Origin à l’adresse`https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="828bb-310">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="828bb-311">Utilisation de chrome :</span><span class="sxs-lookup"><span data-stu-id="828bb-311">Using Chrome:</span></span>

     <span data-ttu-id="828bb-312">**L’accès à XMLHttpRequest `https://webapi.azurewebsites.net/api/values/1` à partir `https://localhost:44375` de l’origine a été bloqué par la stratégie cors : Aucun en-tête « Access-Control-allow-Origin » n’est présent sur la ressource demandée.**</span><span class="sxs-lookup"><span data-stu-id="828bb-312">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="828bb-313">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="828bb-313">Additional resources</span></span>

* [<span data-ttu-id="828bb-314">Partage des ressources Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="828bb-314">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
