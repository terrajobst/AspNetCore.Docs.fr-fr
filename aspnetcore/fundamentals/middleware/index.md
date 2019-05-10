---
title: Intergiciel (middleware) ASP.NET Core
author: rick-anderson
description: Découvrez le middleware ASP.NET Core et le pipeline de requête.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: bac121441d6856ca79affe1a3130e5cbc76debd9
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64882334"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="509c7-103">Intergiciel (middleware) ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="509c7-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="509c7-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="509c7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="509c7-105">Un middleware est un logiciel qui est assemblé dans un pipeline d’application pour gérer les requêtes et les réponses.</span><span class="sxs-lookup"><span data-stu-id="509c7-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="509c7-106">Chaque composant :</span><span class="sxs-lookup"><span data-stu-id="509c7-106">Each component:</span></span>

* <span data-ttu-id="509c7-107">Choisit de passer la requête au composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="509c7-108">Peut travailler avant et après le composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="509c7-109">Les délégués de requête sont utilisés pour créer le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="509c7-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="509c7-110">Les délégués de requête gèrent chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="509c7-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="509c7-111">Les délégués de requête sont configurés à l’aide des méthodes d’extension <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> et <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="509c7-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="509c7-112">Chaque délégué de requête peut être spécifié inline comme méthode anonyme (appelée intergiciel inline) ou peut être défini dans une classe réutilisable.</span><span class="sxs-lookup"><span data-stu-id="509c7-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="509c7-113">Ces classes réutilisables et les méthodes anonymes inline sont des *middlewares*, également appelés *composants de middleware*.</span><span class="sxs-lookup"><span data-stu-id="509c7-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="509c7-114">Chaque composant de middleware du pipeline de requête est chargé d’appeler le composant suivant du pipeline ou de court-circuiter le pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="509c7-115">Lorsqu’un middleware effectue un court-circuit, on parle de *middleware terminal*, car il empêche tout autre middleware de traiter la requête.</span><span class="sxs-lookup"><span data-stu-id="509c7-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="509c7-116"><xref:migration/http-modules> explique la différence entre les pipelines de requêtes dans ASP.NET Core et ASP.NET 4.x, puis fournit d’autres exemples de middlewares.</span><span class="sxs-lookup"><span data-stu-id="509c7-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="509c7-117">Créer un pipeline de middleware avec IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="509c7-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="509c7-118">Le pipeline de requête ASP.NET Core est composé d’une séquence de délégués de requête, appelés l’un après l’autre.</span><span class="sxs-lookup"><span data-stu-id="509c7-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="509c7-119">Le diagramme suivant illustre le concept.</span><span class="sxs-lookup"><span data-stu-id="509c7-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="509c7-120">Le thread d’exécution suit les flèches noires.</span><span class="sxs-lookup"><span data-stu-id="509c7-120">The thread of execution follows the black arrows.</span></span>

![Modèle de traitement des requêtes montrant une requête qui arrive et qui est traitée par trois middlewares, puis la réponse qui quitte l’application.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="509c7-124">Chaque délégué peut effectuer des opérations avant et après le délégué suivant.</span><span class="sxs-lookup"><span data-stu-id="509c7-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="509c7-125">Les délégués de gestion des exceptions doivent être appelés tôt dans le pipeline pour qu’ils puissent intercepter les exceptions qui se produisent dans les phases ultérieures du pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="509c7-126">L’application ASP.NET Core la plus simple possible définit un seul délégué de requête qui gère toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="509c7-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="509c7-127">Ce cas ne fait pas appel à un pipeline de requête réel.</span><span class="sxs-lookup"><span data-stu-id="509c7-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="509c7-128">À la place, une seule fonction anonyme est appelée en réponse à chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="509c7-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="509c7-129">Le premier délégué <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> termine le pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-129">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="509c7-130">Chaînez plusieurs délégués de requête avec <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="509c7-130">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="509c7-131">Le paramètre `next` représente le délégué suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-131">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="509c7-132">Vous pouvez court-circuiter le pipeline en n’appelant *pas* le paramètre *next*.</span><span class="sxs-lookup"><span data-stu-id="509c7-132">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="509c7-133">Vous pouvez généralement effectuer des actions à la fois avant et après le délégué suivant, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="509c7-133">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

<span data-ttu-id="509c7-134">Quand un délégué ne passe pas une requête au délégué suivant, on parle alors de *court-circuit du pipeline de requête*.</span><span class="sxs-lookup"><span data-stu-id="509c7-134">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="509c7-135">Un court-circuit est souvent souhaitable car il évite le travail inutile.</span><span class="sxs-lookup"><span data-stu-id="509c7-135">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="509c7-136">Par exemple, le [middleware Fichier statique](xref:fundamentals/static-files) peut agir en tant que *middleware terminal* en traitant une requête pour un fichier statique et en court-circuitant le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-136">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="509c7-137">Le middleware ajouté au pipeline avant de le middleware qui met fin à la poursuite du traitement traite tout de même le code après les instructions `next.Invoke`.</span><span class="sxs-lookup"><span data-stu-id="509c7-137">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="509c7-138">Toutefois, consultez l’avertissement suivant à propos de la tentative d’écriture sur une réponse qui a déjà été envoyée.</span><span class="sxs-lookup"><span data-stu-id="509c7-138">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="509c7-139">N’appelez pas `next.Invoke` une fois que la réponse a été envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="509c7-139">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="509c7-140">Les changements apportés à <xref:Microsoft.AspNetCore.Http.HttpResponse> après le démarrage de la réponse lèvent une exception.</span><span class="sxs-lookup"><span data-stu-id="509c7-140">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="509c7-141">Par exemple, les changements comme la définition des en-têtes et du code d’état lèvent une exception.</span><span class="sxs-lookup"><span data-stu-id="509c7-141">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="509c7-142">Écrire dans le corps de la réponse après avoir appelé `next` :</span><span class="sxs-lookup"><span data-stu-id="509c7-142">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="509c7-143">Peut entraîner une violation de protocole.</span><span class="sxs-lookup"><span data-stu-id="509c7-143">May cause a protocol violation.</span></span> <span data-ttu-id="509c7-144">Par exemple, écrire plus que le `Content-Length` indiqué.</span><span class="sxs-lookup"><span data-stu-id="509c7-144">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="509c7-145">Peut altérer le format du corps.</span><span class="sxs-lookup"><span data-stu-id="509c7-145">May corrupt the body format.</span></span> <span data-ttu-id="509c7-146">Par exemple, l’écriture d’un pied de page HTML dans un fichier CSS.</span><span class="sxs-lookup"><span data-stu-id="509c7-146">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="509c7-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> est un indice utile pour indiquer si les en-têtes ont été envoyés ou si le corps a fait l’objet d’écritures.</span><span class="sxs-lookup"><span data-stu-id="509c7-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="509c7-148">Trier</span><span class="sxs-lookup"><span data-stu-id="509c7-148">Order</span></span>

<span data-ttu-id="509c7-149">L’ordre dans lequel les composants de middleware sont ajoutés dans la méthode `Startup.Configure` définit l’ordre dans lequel ils sont appelés sur les requêtes et l’ordre inverse pour la réponse.</span><span class="sxs-lookup"><span data-stu-id="509c7-149">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="509c7-150">L’ordre est critique pour la sécurité, les performances et le fonctionnement.</span><span class="sxs-lookup"><span data-stu-id="509c7-150">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="509c7-151">La méthode `Startup.Configure` suivante ajoute des composants middleware utiles pour les scénarios d’application courants :</span><span class="sxs-lookup"><span data-stu-id="509c7-151">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="509c7-152">Gestion des erreurs/exceptions</span><span class="sxs-lookup"><span data-stu-id="509c7-152">Exception/error handling</span></span>
1. <span data-ttu-id="509c7-153">Protocole HSTS (HTTP Strict Transport Security)</span><span class="sxs-lookup"><span data-stu-id="509c7-153">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="509c7-154">Redirection HTTPS</span><span class="sxs-lookup"><span data-stu-id="509c7-154">HTTPS redirection</span></span>
1. <span data-ttu-id="509c7-155">Serveur de fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="509c7-155">Static file server</span></span>
1. <span data-ttu-id="509c7-156">Application d’une stratégie de cookies</span><span class="sxs-lookup"><span data-stu-id="509c7-156">Cookie policy enforcement</span></span>
1. <span data-ttu-id="509c7-157">Authentification</span><span class="sxs-lookup"><span data-stu-id="509c7-157">Authentication</span></span>
1. <span data-ttu-id="509c7-158">Session</span><span class="sxs-lookup"><span data-stu-id="509c7-158">Session</span></span>
1. <span data-ttu-id="509c7-159">MVC</span><span class="sxs-lookup"><span data-stu-id="509c7-159">MVC</span></span>

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

1. <span data-ttu-id="509c7-160">Gestion des erreurs/exceptions</span><span class="sxs-lookup"><span data-stu-id="509c7-160">Exception/error handling</span></span>
1. <span data-ttu-id="509c7-161">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="509c7-161">Static files</span></span>
1. <span data-ttu-id="509c7-162">Authentification</span><span class="sxs-lookup"><span data-stu-id="509c7-162">Authentication</span></span>
1. <span data-ttu-id="509c7-163">Session</span><span class="sxs-lookup"><span data-stu-id="509c7-163">Session</span></span>
1. <span data-ttu-id="509c7-164">MVC</span><span class="sxs-lookup"><span data-stu-id="509c7-164">MVC</span></span>

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

<span data-ttu-id="509c7-165">Dans l’exemple de code précédent, chaque méthode d’extension d’intergiciel (middleware) est exposée sur <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> à travers l’espace de noms <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="509c7-165">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="509c7-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> est le premier composant d’intergiciel ajouté au pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="509c7-167">Par conséquent, le middleware Gestion des exceptions intercepte toutes les exceptions qui se produisent dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="509c7-167">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="509c7-168">Le middleware Fichier statique est appelé tôt dans le pipeline pour qu’il puisse gérer les requêtes et procéder au court-circuit sans passer par les composants restants.</span><span class="sxs-lookup"><span data-stu-id="509c7-168">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="509c7-169">Le middleware Fichier statique ne fournit **aucune** vérification d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="509c7-169">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="509c7-170">Tous les fichiers qu’il sert, notamment ceux sous *wwwroot* sont accessibles à tous.</span><span class="sxs-lookup"><span data-stu-id="509c7-170">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="509c7-171">Pour obtenir une approche permettant de sécuriser les fichiers statiques, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="509c7-171">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="509c7-172">Si la requête n’est pas gérée par le middleware Fichier statique, elle est transmise au middleware Authentification (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), qui effectue l’authentification.</span><span class="sxs-lookup"><span data-stu-id="509c7-172">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="509c7-173">Le middleware Authentification ne court-circuite pas les requêtes non authentifiées.</span><span class="sxs-lookup"><span data-stu-id="509c7-173">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="509c7-174">Même s’il authentifie les requêtes, l’autorisation (et le refus) interviennent uniquement après que MVC a sélectionné une page Razor/un contrôleur MVC et une action spécifiques.</span><span class="sxs-lookup"><span data-stu-id="509c7-174">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="509c7-175">Si la requête n’est pas gérée par le middleware Fichier statique, elle est transmise au middleware Identité (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), qui effectue l’authentification.</span><span class="sxs-lookup"><span data-stu-id="509c7-175">If the request isn't handled by Static File Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="509c7-176">L’intergiciel Identité ne court-circuite pas les requêtes non authentifiées.</span><span class="sxs-lookup"><span data-stu-id="509c7-176">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="509c7-177">Même s’il authentifie les requêtes, l’autorisation (et le refus) interviennent uniquement après que MVC a sélectionné un contrôleur et une action spécifiques.</span><span class="sxs-lookup"><span data-stu-id="509c7-177">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="509c7-178">L’exemple suivant montre un ordre de middlewares où les requêtes pour les fichiers statiques sont gérées par le middleware Fichier statique avant le middleware Compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="509c7-178">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="509c7-179">Les fichiers statiques ne sont pas compressés avec cet ordre de middlewares.</span><span class="sxs-lookup"><span data-stu-id="509c7-179">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="509c7-180">Les réponses MVC de <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> peuvent être compressées.</span><span class="sxs-lookup"><span data-stu-id="509c7-180">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="509c7-181">Use, Run et Map</span><span class="sxs-lookup"><span data-stu-id="509c7-181">Use, Run, and Map</span></span>

<span data-ttu-id="509c7-182">Configurez le pipeline HTTP avec `Use`, `Run` et `Map`.</span><span class="sxs-lookup"><span data-stu-id="509c7-182">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="509c7-183">La méthode `Use` peut court-circuiter le pipeline (autrement dit, si elle n’appelle pas un délégué de requête `next`).</span><span class="sxs-lookup"><span data-stu-id="509c7-183">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="509c7-184">`Run` est une convention, et certains composants d’intergiciel peuvent exposer des méthodes `Run[Middleware]` qui s’exécutent à la fin du pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-184">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="509c7-185">Les extensions <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> sont utilisées comme convention pour créer une branche dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-185"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="509c7-186">`Map*` crée une branche dans le pipeline de requête en fonction des correspondances du chemin de requête donné.</span><span class="sxs-lookup"><span data-stu-id="509c7-186">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="509c7-187">Si le chemin de requête commence par le chemin donné, la branche est exécutée.</span><span class="sxs-lookup"><span data-stu-id="509c7-187">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="509c7-188">Le tableau suivant présente les requêtes et les réponses de `http://localhost:1234` avec le code précédent.</span><span class="sxs-lookup"><span data-stu-id="509c7-188">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="509c7-189">Requête</span><span class="sxs-lookup"><span data-stu-id="509c7-189">Request</span></span>             | <span data-ttu-id="509c7-190">Réponse</span><span class="sxs-lookup"><span data-stu-id="509c7-190">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="509c7-191">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="509c7-191">localhost:1234</span></span>      | <span data-ttu-id="509c7-192">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="509c7-192">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="509c7-193">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="509c7-193">localhost:1234/map1</span></span> | <span data-ttu-id="509c7-194">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="509c7-194">Map Test 1</span></span>                   |
| <span data-ttu-id="509c7-195">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="509c7-195">localhost:1234/map2</span></span> | <span data-ttu-id="509c7-196">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="509c7-196">Map Test 2</span></span>                   |
| <span data-ttu-id="509c7-197">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="509c7-197">localhost:1234/map3</span></span> | <span data-ttu-id="509c7-198">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="509c7-198">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="509c7-199">Quand `Map` est utilisé, les segments de chemin mis en correspondance sont supprimés de `HttpRequest.Path` et ajoutés à `HttpRequest.PathBase` pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="509c7-199">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="509c7-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crée une branche dans le pipeline de requête en fonction du résultat du prédicat donné.</span><span class="sxs-lookup"><span data-stu-id="509c7-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="509c7-201">Un prédicat de type `Func<HttpContext, bool>` peut être utilisé pour mapper les requêtes à une nouvelle branche du pipeline.</span><span class="sxs-lookup"><span data-stu-id="509c7-201">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="509c7-202">Dans l’exemple suivant, un prédicat est utilisé pour détecter la présence d’une variable de chaîne de requête `branch` :</span><span class="sxs-lookup"><span data-stu-id="509c7-202">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="509c7-203">Le tableau suivant présente les requêtes et les réponses de `http://localhost:1234` avec le code précédent.</span><span class="sxs-lookup"><span data-stu-id="509c7-203">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="509c7-204">Requête</span><span class="sxs-lookup"><span data-stu-id="509c7-204">Request</span></span>                       | <span data-ttu-id="509c7-205">Réponse</span><span class="sxs-lookup"><span data-stu-id="509c7-205">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="509c7-206">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="509c7-206">localhost:1234</span></span>                | <span data-ttu-id="509c7-207">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="509c7-207">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="509c7-208">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="509c7-208">localhost:1234/?branch=master</span></span> | <span data-ttu-id="509c7-209">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="509c7-209">Branch used = master</span></span>         |

<span data-ttu-id="509c7-210">`Map` prend en charge l’imbrication, par exemple :</span><span class="sxs-lookup"><span data-stu-id="509c7-210">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="509c7-211">`Map` peut également faire correspondre plusieurs segments à la fois :</span><span class="sxs-lookup"><span data-stu-id="509c7-211">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="509c7-212">Intergiciels (middleware) intégrés</span><span class="sxs-lookup"><span data-stu-id="509c7-212">Built-in middleware</span></span>

<span data-ttu-id="509c7-213">ASP.NET Core est fourni avec les composants de middleware suivant.</span><span class="sxs-lookup"><span data-stu-id="509c7-213">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="509c7-214">La colonne *Ordre* fournit des notes sur l’emplacement du middleware dans le pipeline de traitement de la requête et sur les conditions dans lesquelles le middleware peut mettre fin au traitement de la requête.</span><span class="sxs-lookup"><span data-stu-id="509c7-214">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="509c7-215">Lorsqu’un middleware court-circuite le pipeline de traitement de la requête et empêche tout middleware en aval de traiter une requête, on parle de *middleware terminal*.</span><span class="sxs-lookup"><span data-stu-id="509c7-215">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="509c7-216">Pour plus d’informations sur le court-circuit, consultez la section [Créer un pipeline de middlewares avec IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="509c7-216">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="509c7-217">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="509c7-217">Middleware</span></span> | <span data-ttu-id="509c7-218">Description</span><span class="sxs-lookup"><span data-stu-id="509c7-218">Description</span></span> | <span data-ttu-id="509c7-219">Trier</span><span class="sxs-lookup"><span data-stu-id="509c7-219">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="509c7-220">Authentification</span><span class="sxs-lookup"><span data-stu-id="509c7-220">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="509c7-221">Prend en charge l’authentification.</span><span class="sxs-lookup"><span data-stu-id="509c7-221">Provides authentication support.</span></span> | <span data-ttu-id="509c7-222">Avant que `HttpContext.User` ne soit nécessaire.</span><span class="sxs-lookup"><span data-stu-id="509c7-222">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="509c7-223">Terminal pour les rappels OAuth.</span><span class="sxs-lookup"><span data-stu-id="509c7-223">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="509c7-224">Stratégie de cookies</span><span class="sxs-lookup"><span data-stu-id="509c7-224">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="509c7-225">Effectue le suivi de consentement des utilisateurs pour le stockage des informations personnelles et applique des normes minimales pour les champs de cookie, comme `secure` et `SameSite`.</span><span class="sxs-lookup"><span data-stu-id="509c7-225">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="509c7-226">Avant le middleware qui émet les cookies.</span><span class="sxs-lookup"><span data-stu-id="509c7-226">Before middleware that issues cookies.</span></span> <span data-ttu-id="509c7-227">Exemples : authentification, session, MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="509c7-227">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="509c7-228">CORS</span><span class="sxs-lookup"><span data-stu-id="509c7-228">CORS</span></span>](xref:security/cors) | <span data-ttu-id="509c7-229">Configure le partage des ressources cross-origin (CORS).</span><span class="sxs-lookup"><span data-stu-id="509c7-229">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="509c7-230">Avant les composants qui utilisent CORS.</span><span class="sxs-lookup"><span data-stu-id="509c7-230">Before components that use CORS.</span></span> |
| [<span data-ttu-id="509c7-231">Gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="509c7-231">Exception Handling</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="509c7-232">Gère les exceptions.</span><span class="sxs-lookup"><span data-stu-id="509c7-232">Handles exceptions.</span></span> | <span data-ttu-id="509c7-233">Avant les composants qui génèrent des erreurs.</span><span class="sxs-lookup"><span data-stu-id="509c7-233">Before components that generate errors.</span></span> |
| [<span data-ttu-id="509c7-234">En-têtes transférés</span><span class="sxs-lookup"><span data-stu-id="509c7-234">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="509c7-235">Transfère les en-têtes en proxy vers la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="509c7-235">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="509c7-236">Avant les composants qui consomment les champs mis à jour.</span><span class="sxs-lookup"><span data-stu-id="509c7-236">Before components that consume the updated fields.</span></span> <span data-ttu-id="509c7-237">Exemples : schéma, hôte, IP du client, méthode.</span><span class="sxs-lookup"><span data-stu-id="509c7-237">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="509c7-238">Contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="509c7-238">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="509c7-239">Contrôle l’intégrité d’une application ASP.NET Core et de ses dépendances, notamment la disponibilité de la base de données.</span><span class="sxs-lookup"><span data-stu-id="509c7-239">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="509c7-240">Terminal si une requête correspond à un point de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="509c7-240">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="509c7-241">Remplacement de la méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="509c7-241">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="509c7-242">Autorise une requête POST entrante à remplacer la méthode.</span><span class="sxs-lookup"><span data-stu-id="509c7-242">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="509c7-243">Avant les composants qui consomment la méthode mise à jour.</span><span class="sxs-lookup"><span data-stu-id="509c7-243">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="509c7-244">Redirection HTTPS</span><span class="sxs-lookup"><span data-stu-id="509c7-244">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="509c7-245">Redirige toutes les requêtes HTTP vers HTTPS (ASP.NET Core 2.1 ou ultérieur).</span><span class="sxs-lookup"><span data-stu-id="509c7-245">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="509c7-246">Avant les composants qui consomment l’URL.</span><span class="sxs-lookup"><span data-stu-id="509c7-246">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="509c7-247">HSTS (HTTP Strict Transport Security)</span><span class="sxs-lookup"><span data-stu-id="509c7-247">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="509c7-248">Middleware d’amélioration de la sécurité qui ajoute un en-tête de réponse spécial (ASP.NET Core 2.1 ou ultérieur).</span><span class="sxs-lookup"><span data-stu-id="509c7-248">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="509c7-249">Avant l’envoi des réponses et après les composants qui modifient les requêtes.</span><span class="sxs-lookup"><span data-stu-id="509c7-249">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="509c7-250">Exemples : en-têtes transférés, réécriture d’URL.</span><span class="sxs-lookup"><span data-stu-id="509c7-250">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="509c7-251">MVC</span><span class="sxs-lookup"><span data-stu-id="509c7-251">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="509c7-252">Traite les requêtes avec MVC/Razor Pages (ASP.NET Core 2.0 ou version ultérieure).</span><span class="sxs-lookup"><span data-stu-id="509c7-252">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="509c7-253">Terminal si une requête correspond à un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="509c7-253">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="509c7-254">OWIN</span><span class="sxs-lookup"><span data-stu-id="509c7-254">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="509c7-255">Interopérabilité avec le middleware, les serveurs et les applications OWIN.</span><span class="sxs-lookup"><span data-stu-id="509c7-255">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="509c7-256">Terminal si le middleware OWIN traite entièrement la requête.</span><span class="sxs-lookup"><span data-stu-id="509c7-256">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="509c7-257">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="509c7-257">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="509c7-258">Prend en charge la mise en cache des réponses.</span><span class="sxs-lookup"><span data-stu-id="509c7-258">Provides support for caching responses.</span></span> | <span data-ttu-id="509c7-259">Avant les composants qui nécessitent la mise en cache.</span><span class="sxs-lookup"><span data-stu-id="509c7-259">Before components that require caching.</span></span> |
| [<span data-ttu-id="509c7-260">Compression des réponses</span><span class="sxs-lookup"><span data-stu-id="509c7-260">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="509c7-261">Prend en charge la compression des réponses.</span><span class="sxs-lookup"><span data-stu-id="509c7-261">Provides support for compressing responses.</span></span> | <span data-ttu-id="509c7-262">Avant les composants qui nécessitent la compression.</span><span class="sxs-lookup"><span data-stu-id="509c7-262">Before components that require compression.</span></span> |
| [<span data-ttu-id="509c7-263">Localisation des requêtes</span><span class="sxs-lookup"><span data-stu-id="509c7-263">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="509c7-264">Prend en charge la localisation.</span><span class="sxs-lookup"><span data-stu-id="509c7-264">Provides localization support.</span></span> | <span data-ttu-id="509c7-265">Avant la localisation des composants sensibles.</span><span class="sxs-lookup"><span data-stu-id="509c7-265">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="509c7-266">Le routage</span><span class="sxs-lookup"><span data-stu-id="509c7-266">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="509c7-267">Définit et contraint des routes de requête.</span><span class="sxs-lookup"><span data-stu-id="509c7-267">Defines and constrains request routes.</span></span> | <span data-ttu-id="509c7-268">Terminal pour les routes correspondantes.</span><span class="sxs-lookup"><span data-stu-id="509c7-268">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="509c7-269">Session</span><span class="sxs-lookup"><span data-stu-id="509c7-269">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="509c7-270">Prend en charge la gestion des sessions utilisateur.</span><span class="sxs-lookup"><span data-stu-id="509c7-270">Provides support for managing user sessions.</span></span> | <span data-ttu-id="509c7-271">Avant les composants qui nécessitent la session.</span><span class="sxs-lookup"><span data-stu-id="509c7-271">Before components that require Session.</span></span> |
| [<span data-ttu-id="509c7-272">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="509c7-272">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="509c7-273">Prend en charge le traitement des fichiers statiques et l’exploration des répertoires.</span><span class="sxs-lookup"><span data-stu-id="509c7-273">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="509c7-274">Terminal si une requête correspond à un fichier.</span><span class="sxs-lookup"><span data-stu-id="509c7-274">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="509c7-275">Réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="509c7-275">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="509c7-276">Prend en charge la réécriture d’URL et la redirection des requêtes.</span><span class="sxs-lookup"><span data-stu-id="509c7-276">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="509c7-277">Avant les composants qui consomment l’URL.</span><span class="sxs-lookup"><span data-stu-id="509c7-277">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="509c7-278">WebSockets</span><span class="sxs-lookup"><span data-stu-id="509c7-278">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="509c7-279">Autorise le protocole WebSockets.</span><span class="sxs-lookup"><span data-stu-id="509c7-279">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="509c7-280">Avant les composants qui sont nécessaires pour accepter les requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="509c7-280">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="509c7-281">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="509c7-281">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
