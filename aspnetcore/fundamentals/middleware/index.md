---
title: Intergiciel (middleware) ASP.NET Core
author: rick-anderson
description: Découvrez le middleware ASP.NET Core et le pipeline de requête.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 84e79df7fcf5790e658a20c80f21d73cdc76c054
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46483007"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="06ec6-103">Intergiciel (middleware) ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06ec6-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="06ec6-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="06ec6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="06ec6-105">Un middleware est un logiciel qui est assemblé dans un pipeline d’application pour gérer les requêtes et les réponses.</span><span class="sxs-lookup"><span data-stu-id="06ec6-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="06ec6-106">Chaque composant :</span><span class="sxs-lookup"><span data-stu-id="06ec6-106">Each component:</span></span>

* <span data-ttu-id="06ec6-107">Choisit de passer la requête au composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="06ec6-108">Peut travailler avant et après l’appel du composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="06ec6-109">Les délégués de requête sont utilisés pour créer le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="06ec6-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="06ec6-110">Les délégués de requête gèrent chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="06ec6-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="06ec6-111">Les délégués de requête sont configurés à l’aide des méthodes d’extension <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> et <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="06ec6-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="06ec6-112">Chaque délégué de requête peut être spécifié inline comme méthode anonyme (appelée intergiciel inline) ou peut être défini dans une classe réutilisable.</span><span class="sxs-lookup"><span data-stu-id="06ec6-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="06ec6-113">Ces classes réutilisables et les méthodes anonymes inline sont des *middlewares*, également appelés *composants de middleware*.</span><span class="sxs-lookup"><span data-stu-id="06ec6-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="06ec6-114">Chaque composant de middleware du pipeline de requête est chargé d’appeler le composant suivant du pipeline ou de court-circuiter le pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="06ec6-115"><xref:migration/http-modules> explique la différence entre les pipelines de requêtes dans ASP.NET Core et ASP.NET 4.x, puis fournit d’autres exemples de middlewares.</span><span class="sxs-lookup"><span data-stu-id="06ec6-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="06ec6-116">Créer un pipeline de middleware avec IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="06ec6-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="06ec6-117">Le pipeline de requête ASP.NET Core est composé d’une séquence de délégués de requête, appelés l’un après l’autre.</span><span class="sxs-lookup"><span data-stu-id="06ec6-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="06ec6-118">Le diagramme suivant illustre le concept.</span><span class="sxs-lookup"><span data-stu-id="06ec6-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="06ec6-119">Le thread d’exécution suit les flèches noires.</span><span class="sxs-lookup"><span data-stu-id="06ec6-119">The thread of execution follows the black arrows.</span></span>

![Modèle de traitement des requêtes montrant une requête qui arrive et qui est traitée par trois middlewares, puis la réponse qui quitte l’application.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="06ec6-123">Chaque délégué peut effectuer des opérations avant et après le délégué suivant.</span><span class="sxs-lookup"><span data-stu-id="06ec6-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="06ec6-124">Un délégué peut également décider de ne pas passer une requête au délégué suivant. On parle alors de *court-circuit du pipeline de requête*.</span><span class="sxs-lookup"><span data-stu-id="06ec6-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="06ec6-125">Un court-circuit est souvent souhaitable car il évite le travail inutile.</span><span class="sxs-lookup"><span data-stu-id="06ec6-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="06ec6-126">Par exemple, le middleware Fihiers statiques peut retourner une requête pour un fichier statique et court-circuiter le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="06ec6-127">Les délégués de gestion des exceptions sont appelés tôt dans le pipeline pour qu’ils puissent intercepter les exceptions qui se produisent dans les phases ultérieures du pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="06ec6-128">L’application ASP.NET Core la plus simple possible définit un seul délégué de requête qui gère toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="06ec6-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="06ec6-129">Ce cas ne fait pas appel à un pipeline de requête réel.</span><span class="sxs-lookup"><span data-stu-id="06ec6-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="06ec6-130">À la place, une seule fonction anonyme est appelée en réponse à chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="06ec6-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="06ec6-131">Le premier délégué <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> termine le pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="06ec6-132">Chaînez plusieurs délégués de requête avec <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="06ec6-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="06ec6-133">Le paramètre `next` représente le délégué suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="06ec6-134">Vous pouvez court-circuiter le pipeline en n’appelant *pas* le paramètre *next*.</span><span class="sxs-lookup"><span data-stu-id="06ec6-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="06ec6-135">Vous pouvez généralement effectuer des actions à la fois avant et après le délégué suivant, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="06ec6-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="06ec6-136">N’appelez pas `next.Invoke` une fois que la réponse a été envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="06ec6-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="06ec6-137">Les changements apportés à <xref:Microsoft.AspNetCore.Http.HttpResponse> après le démarrage de la réponse lèvent une exception.</span><span class="sxs-lookup"><span data-stu-id="06ec6-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="06ec6-138">Par exemple, les changements comme la définition des en-têtes et du code d’état lèvent une exception.</span><span class="sxs-lookup"><span data-stu-id="06ec6-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="06ec6-139">Écrire dans le corps de la réponse après avoir appelé `next` :</span><span class="sxs-lookup"><span data-stu-id="06ec6-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="06ec6-140">Peut entraîner une violation de protocole.</span><span class="sxs-lookup"><span data-stu-id="06ec6-140">May cause a protocol violation.</span></span> <span data-ttu-id="06ec6-141">Par exemple, écrire plus que le `Content-Length` indiqué.</span><span class="sxs-lookup"><span data-stu-id="06ec6-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="06ec6-142">Peut altérer le format du corps.</span><span class="sxs-lookup"><span data-stu-id="06ec6-142">May corrupt the body format.</span></span> <span data-ttu-id="06ec6-143">Par exemple, l’écriture d’un pied de page HTML dans un fichier CSS.</span><span class="sxs-lookup"><span data-stu-id="06ec6-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="06ec6-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> est un indice utile pour indiquer si les en-têtes ont été envoyés ou si le corps a fait l’objet d’écritures.</span><span class="sxs-lookup"><span data-stu-id="06ec6-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="06ec6-145">Trier</span><span class="sxs-lookup"><span data-stu-id="06ec6-145">Order</span></span>

<span data-ttu-id="06ec6-146">L’ordre dans lequel les composants de middleware sont ajoutés dans la méthode `Startup.Configure` définit l’ordre dans lequel ils sont appelés sur les requêtes et l’ordre inverse pour la réponse.</span><span class="sxs-lookup"><span data-stu-id="06ec6-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="06ec6-147">L’ordre est critique pour la sécurité, les performances et le fonctionnement.</span><span class="sxs-lookup"><span data-stu-id="06ec6-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="06ec6-148">La méthode `Startup.Configure` suivante ajoute des composants middleware utiles pour les scénarios d’application courants :</span><span class="sxs-lookup"><span data-stu-id="06ec6-148">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="06ec6-149">Gestion des erreurs/exceptions</span><span class="sxs-lookup"><span data-stu-id="06ec6-149">Exception/error handling</span></span>
1. <span data-ttu-id="06ec6-150">Protocole HSTS (HTTP Strict Transport Security)</span><span class="sxs-lookup"><span data-stu-id="06ec6-150">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="06ec6-151">Redirection HTTPS</span><span class="sxs-lookup"><span data-stu-id="06ec6-151">HTTPS redirection</span></span>
1. <span data-ttu-id="06ec6-152">Serveur de fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="06ec6-152">Static file server</span></span>
1. <span data-ttu-id="06ec6-153">Application d’une stratégie de cookies</span><span class="sxs-lookup"><span data-stu-id="06ec6-153">Cookie policy enforcement</span></span>
1. <span data-ttu-id="06ec6-154">Authentification</span><span class="sxs-lookup"><span data-stu-id="06ec6-154">Authentication</span></span>
1. <span data-ttu-id="06ec6-155">Session</span><span class="sxs-lookup"><span data-stu-id="06ec6-155">Session</span></span>
1. <span data-ttu-id="06ec6-156">MVC</span><span class="sxs-lookup"><span data-stu-id="06ec6-156">MVC</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. <span data-ttu-id="06ec6-157">Gestion des erreurs/exceptions</span><span class="sxs-lookup"><span data-stu-id="06ec6-157">Exception/error handling</span></span>
1. <span data-ttu-id="06ec6-158">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="06ec6-158">Static files</span></span>
1. <span data-ttu-id="06ec6-159">Authentification</span><span class="sxs-lookup"><span data-stu-id="06ec6-159">Authentication</span></span>
1. <span data-ttu-id="06ec6-160">Session</span><span class="sxs-lookup"><span data-stu-id="06ec6-160">Session</span></span>
1. <span data-ttu-id="06ec6-161">MVC</span><span class="sxs-lookup"><span data-stu-id="06ec6-161">MVC</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="06ec6-162">Dans l’exemple de code précédent, chaque méthode d’extension d’intergiciel (middleware) est exposée sur <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> à travers l’espace de noms <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="06ec6-162">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="06ec6-163"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> est le premier composant d’intergiciel ajouté au pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-163"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="06ec6-164">Par conséquent, le middleware Gestion des exceptions intercepte toutes les exceptions qui se produisent dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="06ec6-164">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="06ec6-165">Le middleware Fichiers statiques est appelé tôt dans le pipeline pour qu’il puisse gérer les requêtes et procéder au court-circuit sans passer par les composants restants.</span><span class="sxs-lookup"><span data-stu-id="06ec6-165">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="06ec6-166">Le middleware Fichiers statiques ne fournit **aucune** vérification d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="06ec6-166">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="06ec6-167">Tous les fichiers qu’il sert, notamment ceux sous *wwwroot* sont accessibles à tous.</span><span class="sxs-lookup"><span data-stu-id="06ec6-167">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="06ec6-168">Pour obtenir une approche permettant de sécuriser les fichiers statiques, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="06ec6-168">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="06ec6-169">Si la requête n’est pas gérée par le middleware Fichiers statiques, elle est transmise au middleware Authentification (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), qui effectue l’authentification.</span><span class="sxs-lookup"><span data-stu-id="06ec6-169">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="06ec6-170">Le middleware Authentification ne court-circuite pas les requêtes non authentifiées.</span><span class="sxs-lookup"><span data-stu-id="06ec6-170">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="06ec6-171">Même s’il authentifie les requêtes, l’autorisation (et le refus) interviennent uniquement après que MVC a sélectionné une page Razor/un contrôleur MVC et une action spécifiques.</span><span class="sxs-lookup"><span data-stu-id="06ec6-171">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="06ec6-172">Si la requête n’est pas gérée par le middleware Fichiers statiques, elle est transmise au middleware Identité (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), qui effectue l’authentification.</span><span class="sxs-lookup"><span data-stu-id="06ec6-172">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="06ec6-173">L’intergiciel Identité ne court-circuite pas les requêtes non authentifiées.</span><span class="sxs-lookup"><span data-stu-id="06ec6-173">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="06ec6-174">Même s’il authentifie les requêtes, l’autorisation (et le refus) interviennent uniquement après que MVC a sélectionné un contrôleur et une action spécifiques.</span><span class="sxs-lookup"><span data-stu-id="06ec6-174">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="06ec6-175">L’exemple suivant montre un ordre de middlewares où les requêtes pour les fichiers statiques sont gérées par le middleware Fichiers statiques avant le middleware Compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="06ec6-175">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="06ec6-176">Les fichiers statiques ne sont pas compressés avec cet ordre de middlewares.</span><span class="sxs-lookup"><span data-stu-id="06ec6-176">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="06ec6-177">Les réponses MVC de <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> peuvent être compressées.</span><span class="sxs-lookup"><span data-stu-id="06ec6-177">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="06ec6-178">Use, Run et Map</span><span class="sxs-lookup"><span data-stu-id="06ec6-178">Use, Run, and Map</span></span>

<span data-ttu-id="06ec6-179">Configurez le pipeline HTTP avec `Use`, `Run` et `Map`.</span><span class="sxs-lookup"><span data-stu-id="06ec6-179">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="06ec6-180">La méthode `Use` peut court-circuiter le pipeline (autrement dit, si elle n’appelle pas un délégué de requête `next`).</span><span class="sxs-lookup"><span data-stu-id="06ec6-180">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="06ec6-181">`Run` est une convention, et certains composants d’intergiciel peuvent exposer des méthodes `Run[Middleware]` qui s’exécutent à la fin du pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-181">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="06ec6-182">Les extensions <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> sont utilisées comme convention pour créer une branche dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-182"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="06ec6-183">`Map*` crée une branche dans le pipeline de requête en fonction des correspondances du chemin de requête donné.</span><span class="sxs-lookup"><span data-stu-id="06ec6-183">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="06ec6-184">Si le chemin de requête commence par le chemin donné, la branche est exécutée.</span><span class="sxs-lookup"><span data-stu-id="06ec6-184">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="06ec6-185">Le tableau suivant présente les requêtes et les réponses de `http://localhost:1234` avec le code précédent.</span><span class="sxs-lookup"><span data-stu-id="06ec6-185">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="06ec6-186">Demande</span><span class="sxs-lookup"><span data-stu-id="06ec6-186">Request</span></span>             | <span data-ttu-id="06ec6-187">Réponse</span><span class="sxs-lookup"><span data-stu-id="06ec6-187">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="06ec6-188">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="06ec6-188">localhost:1234</span></span>      | <span data-ttu-id="06ec6-189">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="06ec6-189">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="06ec6-190">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="06ec6-190">localhost:1234/map1</span></span> | <span data-ttu-id="06ec6-191">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="06ec6-191">Map Test 1</span></span>                   |
| <span data-ttu-id="06ec6-192">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="06ec6-192">localhost:1234/map2</span></span> | <span data-ttu-id="06ec6-193">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="06ec6-193">Map Test 2</span></span>                   |
| <span data-ttu-id="06ec6-194">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="06ec6-194">localhost:1234/map3</span></span> | <span data-ttu-id="06ec6-195">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="06ec6-195">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="06ec6-196">Quand `Map` est utilisé, les segments de chemin mis en correspondance sont supprimés de `HttpRequest.Path` et ajoutés à `HttpRequest.PathBase` pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="06ec6-196">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="06ec6-197">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crée une branche dans le pipeline de requête en fonction du résultat du prédicat donné.</span><span class="sxs-lookup"><span data-stu-id="06ec6-197">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="06ec6-198">Un prédicat de type `Func<HttpContext, bool>` peut être utilisé pour mapper les requêtes à une nouvelle branche du pipeline.</span><span class="sxs-lookup"><span data-stu-id="06ec6-198">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="06ec6-199">Dans l’exemple suivant, un prédicat est utilisé pour détecter la présence d’une variable de chaîne de requête `branch` :</span><span class="sxs-lookup"><span data-stu-id="06ec6-199">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="06ec6-200">Le tableau suivant présente les requêtes et les réponses de `http://localhost:1234` avec le code précédent.</span><span class="sxs-lookup"><span data-stu-id="06ec6-200">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="06ec6-201">Demande</span><span class="sxs-lookup"><span data-stu-id="06ec6-201">Request</span></span>                       | <span data-ttu-id="06ec6-202">Réponse</span><span class="sxs-lookup"><span data-stu-id="06ec6-202">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="06ec6-203">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="06ec6-203">localhost:1234</span></span>                | <span data-ttu-id="06ec6-204">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="06ec6-204">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="06ec6-205">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="06ec6-205">localhost:1234/?branch=master</span></span> | <span data-ttu-id="06ec6-206">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="06ec6-206">Branch used = master</span></span>         |

<span data-ttu-id="06ec6-207">`Map` prend en charge l’imbrication, par exemple :</span><span class="sxs-lookup"><span data-stu-id="06ec6-207">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="06ec6-208">`Map` peut également faire correspondre plusieurs segments à la fois :</span><span class="sxs-lookup"><span data-stu-id="06ec6-208">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="06ec6-209">Intergiciels (middleware) intégrés</span><span class="sxs-lookup"><span data-stu-id="06ec6-209">Built-in middleware</span></span>

<span data-ttu-id="06ec6-210">ASP.NET Core est fourni avec les composants de middleware suivant.</span><span class="sxs-lookup"><span data-stu-id="06ec6-210">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="06ec6-211">La colonne *Ordre* fournit des notes sur l’emplacement du middleware dans le pipeline de requête et sur les conditions dans lesquelles le middleware peut mettre fin à la requête et empêcher les autres middlewares de traiter une requête.</span><span class="sxs-lookup"><span data-stu-id="06ec6-211">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="06ec6-212">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="06ec6-212">Middleware</span></span> | <span data-ttu-id="06ec6-213">Description</span><span class="sxs-lookup"><span data-stu-id="06ec6-213">Description</span></span> | <span data-ttu-id="06ec6-214">Trier</span><span class="sxs-lookup"><span data-stu-id="06ec6-214">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="06ec6-215">Authentification</span><span class="sxs-lookup"><span data-stu-id="06ec6-215">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="06ec6-216">Prend en charge l’authentification.</span><span class="sxs-lookup"><span data-stu-id="06ec6-216">Provides authentication support.</span></span> | <span data-ttu-id="06ec6-217">Avant que `HttpContext.User` ne soit nécessaire.</span><span class="sxs-lookup"><span data-stu-id="06ec6-217">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="06ec6-218">Terminal pour les rappels OAuth.</span><span class="sxs-lookup"><span data-stu-id="06ec6-218">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="06ec6-219">Stratégie de cookies</span><span class="sxs-lookup"><span data-stu-id="06ec6-219">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="06ec6-220">Effectue le suivi de consentement des utilisateurs pour le stockage des informations personnelles et applique des normes minimales pour les champs de cookie, comme `secure` et `SameSite`.</span><span class="sxs-lookup"><span data-stu-id="06ec6-220">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="06ec6-221">Avant le middleware qui émet les cookies.</span><span class="sxs-lookup"><span data-stu-id="06ec6-221">Before middleware that issues cookies.</span></span> <span data-ttu-id="06ec6-222">Exemples : authentification, session, MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="06ec6-222">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="06ec6-223">CORS</span><span class="sxs-lookup"><span data-stu-id="06ec6-223">CORS</span></span>](xref:security/cors) | <span data-ttu-id="06ec6-224">Configure le partage des ressources cross-origin (CORS).</span><span class="sxs-lookup"><span data-stu-id="06ec6-224">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="06ec6-225">Avant les composants qui utilisent CORS.</span><span class="sxs-lookup"><span data-stu-id="06ec6-225">Before components that use CORS.</span></span> |
| [<span data-ttu-id="06ec6-226">Diagnostics</span><span class="sxs-lookup"><span data-stu-id="06ec6-226">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="06ec6-227">Configure les diagnostics.</span><span class="sxs-lookup"><span data-stu-id="06ec6-227">Configures diagnostics.</span></span> | <span data-ttu-id="06ec6-228">Avant les composants qui génèrent des erreurs.</span><span class="sxs-lookup"><span data-stu-id="06ec6-228">Before components that generate errors.</span></span> |
| [<span data-ttu-id="06ec6-229">En-têtes transférés</span><span class="sxs-lookup"><span data-stu-id="06ec6-229">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="06ec6-230">Transfère les en-têtes en proxy vers la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="06ec6-230">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="06ec6-231">Avant les composants qui consomment les champs mis à jour.</span><span class="sxs-lookup"><span data-stu-id="06ec6-231">Before components that consume the updated fields.</span></span> <span data-ttu-id="06ec6-232">Exemples : schéma, hôte, IP du client, méthode.</span><span class="sxs-lookup"><span data-stu-id="06ec6-232">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="06ec6-233">Remplacement de la méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="06ec6-233">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="06ec6-234">Autorise une requête POST entrante à remplacer la méthode.</span><span class="sxs-lookup"><span data-stu-id="06ec6-234">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="06ec6-235">Avant les composants qui consomment la méthode mise à jour.</span><span class="sxs-lookup"><span data-stu-id="06ec6-235">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="06ec6-236">Redirection HTTPS</span><span class="sxs-lookup"><span data-stu-id="06ec6-236">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="06ec6-237">Redirige toutes les requêtes HTTP vers HTTPS (ASP.NET Core 2.1 ou ultérieur).</span><span class="sxs-lookup"><span data-stu-id="06ec6-237">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="06ec6-238">Avant les composants qui consomment l’URL.</span><span class="sxs-lookup"><span data-stu-id="06ec6-238">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="06ec6-239">HSTS (HTTP Strict Transport Security)</span><span class="sxs-lookup"><span data-stu-id="06ec6-239">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="06ec6-240">Middleware d’amélioration de la sécurité qui ajoute un en-tête de réponse spécial (ASP.NET Core 2.1 ou ultérieur).</span><span class="sxs-lookup"><span data-stu-id="06ec6-240">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="06ec6-241">Avant l’envoi des réponses et après les composants qui modifient les requêtes.</span><span class="sxs-lookup"><span data-stu-id="06ec6-241">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="06ec6-242">Exemples : en-têtes transférés, réécriture d’URL.</span><span class="sxs-lookup"><span data-stu-id="06ec6-242">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="06ec6-243">MVC</span><span class="sxs-lookup"><span data-stu-id="06ec6-243">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="06ec6-244">Traite les requêtes avec MVC/Razor Pages (ASP.NET Core 2.0 ou version ultérieure).</span><span class="sxs-lookup"><span data-stu-id="06ec6-244">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="06ec6-245">Terminal si une requête correspond à un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="06ec6-245">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="06ec6-246">OWIN</span><span class="sxs-lookup"><span data-stu-id="06ec6-246">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="06ec6-247">Interopérabilité avec le middleware, les serveurs et les applications OWIN.</span><span class="sxs-lookup"><span data-stu-id="06ec6-247">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="06ec6-248">Terminal si le middleware OWIN traite entièrement la requête.</span><span class="sxs-lookup"><span data-stu-id="06ec6-248">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="06ec6-249">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="06ec6-249">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="06ec6-250">Prend en charge la mise en cache des réponses.</span><span class="sxs-lookup"><span data-stu-id="06ec6-250">Provides support for caching responses.</span></span> | <span data-ttu-id="06ec6-251">Avant les composants qui nécessitent la mise en cache.</span><span class="sxs-lookup"><span data-stu-id="06ec6-251">Before components that require caching.</span></span> |
| [<span data-ttu-id="06ec6-252">Compression des réponses</span><span class="sxs-lookup"><span data-stu-id="06ec6-252">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="06ec6-253">Prend en charge la compression des réponses.</span><span class="sxs-lookup"><span data-stu-id="06ec6-253">Provides support for compressing responses.</span></span> | <span data-ttu-id="06ec6-254">Avant les composants qui nécessitent la compression.</span><span class="sxs-lookup"><span data-stu-id="06ec6-254">Before components that require compression.</span></span> |
| [<span data-ttu-id="06ec6-255">Localisation des requêtes</span><span class="sxs-lookup"><span data-stu-id="06ec6-255">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="06ec6-256">Prend en charge la localisation.</span><span class="sxs-lookup"><span data-stu-id="06ec6-256">Provides localization support.</span></span> | <span data-ttu-id="06ec6-257">Avant la localisation des composants sensibles.</span><span class="sxs-lookup"><span data-stu-id="06ec6-257">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="06ec6-258">Le routage</span><span class="sxs-lookup"><span data-stu-id="06ec6-258">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="06ec6-259">Définit et contraint des routes de requête.</span><span class="sxs-lookup"><span data-stu-id="06ec6-259">Defines and constrains request routes.</span></span> | <span data-ttu-id="06ec6-260">Terminal pour les routes correspondantes.</span><span class="sxs-lookup"><span data-stu-id="06ec6-260">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="06ec6-261">Session</span><span class="sxs-lookup"><span data-stu-id="06ec6-261">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="06ec6-262">Prend en charge la gestion des sessions utilisateur.</span><span class="sxs-lookup"><span data-stu-id="06ec6-262">Provides support for managing user sessions.</span></span> | <span data-ttu-id="06ec6-263">Avant les composants qui nécessitent la session.</span><span class="sxs-lookup"><span data-stu-id="06ec6-263">Before components that require Session.</span></span> |
| [<span data-ttu-id="06ec6-264">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="06ec6-264">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="06ec6-265">Prend en charge le traitement des fichiers statiques et l’exploration des répertoires.</span><span class="sxs-lookup"><span data-stu-id="06ec6-265">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="06ec6-266">Terminal si une requête correspond à un fichier.</span><span class="sxs-lookup"><span data-stu-id="06ec6-266">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="06ec6-267">Réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="06ec6-267">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="06ec6-268">Prend en charge la réécriture d’URL et la redirection des requêtes.</span><span class="sxs-lookup"><span data-stu-id="06ec6-268">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="06ec6-269">Avant les composants qui consomment l’URL.</span><span class="sxs-lookup"><span data-stu-id="06ec6-269">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="06ec6-270">WebSockets</span><span class="sxs-lookup"><span data-stu-id="06ec6-270">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="06ec6-271">Autorise le protocole WebSockets.</span><span class="sxs-lookup"><span data-stu-id="06ec6-271">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="06ec6-272">Avant les composants qui sont nécessaires pour accepter les requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="06ec6-272">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="06ec6-273">Middleware Écriture</span><span class="sxs-lookup"><span data-stu-id="06ec6-273">Write middleware</span></span>

<span data-ttu-id="06ec6-274">Les intergiciels sont généralement encapsulés dans une classe et exposés avec une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="06ec6-274">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="06ec6-275">Prenez en compte le middleware suivant, qui définit la culture de la requête actuelle à partir d’une chaîne de requête :</span><span class="sxs-lookup"><span data-stu-id="06ec6-275">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="06ec6-276">L’exemple de code précédent est utilisé pour illustrer la création d’un composant de middleware.</span><span class="sxs-lookup"><span data-stu-id="06ec6-276">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="06ec6-277">Pour plus d’informations sur la prise en charge intégrée de la localisation par ASP.NET Core, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="06ec6-277">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="06ec6-278">Vous pouvez tester l’intergiciel en passant la culture, par exemple `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="06ec6-278">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="06ec6-279">Le code suivant déplace le délégué de l’intergiciel dans une classe :</span><span class="sxs-lookup"><span data-stu-id="06ec6-279">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="06ec6-280">Le nom de la méthode `Task` du middleware doit être `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="06ec6-280">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="06ec6-281">Dans ASP.NET Core 2.0 ou ultérieur, le nom peut être `Invoke` ou `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="06ec6-281">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="06ec6-282">La méthode d’extension suivante expose le middleware par le biais de <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> :</span><span class="sxs-lookup"><span data-stu-id="06ec6-282">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="06ec6-283">Le code suivant appelle l’intergiciel à partir de `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="06ec6-283">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="06ec6-284">L’intergiciel doit suivre le [principe de dépendances explicites](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) en exposant ses dépendances dans son constructeur.</span><span class="sxs-lookup"><span data-stu-id="06ec6-284">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="06ec6-285">L’intergiciel est construit une fois par *durée de vie d’application*.</span><span class="sxs-lookup"><span data-stu-id="06ec6-285">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="06ec6-286">Consultez la section [Dépendances par requête](#per-request-dependencies) si vous avez besoin de partager des services avec un middleware au sein d’une requête.</span><span class="sxs-lookup"><span data-stu-id="06ec6-286">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="06ec6-287">Les composants de middleware peuvent résoudre leurs dépendances à partir de l’[injection de dépendances](xref:fundamentals/dependency-injection) à l’aide des paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="06ec6-287">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="06ec6-288">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) peut également accepter des paramètres supplémentaires directement.</span><span class="sxs-lookup"><span data-stu-id="06ec6-288">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="06ec6-289">Dépendances par requête</span><span class="sxs-lookup"><span data-stu-id="06ec6-289">Per-request dependencies</span></span>

<span data-ttu-id="06ec6-290">Étant donné que le middleware est construit au démarrage de l’application, et non par requête, les services de durée de vie *délimités* utilisés par les constructeurs du middleware ne sont pas partagés avec d’autres types injectés par des dépendances lors de chaque requête.</span><span class="sxs-lookup"><span data-stu-id="06ec6-290">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="06ec6-291">Si vous devez partager un service *délimité* entre votre intergiciel et d’autres types, ajoutez-le à la signature de la méthode `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="06ec6-291">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="06ec6-292">La méthode `Invoke` peut accepter des paramètres supplémentaires qui sont renseignés par injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="06ec6-292">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="06ec6-293">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="06ec6-293">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
