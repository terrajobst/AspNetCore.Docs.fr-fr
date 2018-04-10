---
title: Intergiciel (middleware) ASP.NET Core
author: rick-anderson
description: Découvrez le middleware ASP.NET Core et le pipeline de requête.
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 3312b27f936340a73243224c1a716fe421f178bc
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="add2d-103">Intergiciel (middleware) ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="add2d-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="add2d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="add2d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="add2d-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="add2d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="add2d-106">Qu’est-ce qu’un intergiciel (middleware) ?</span><span class="sxs-lookup"><span data-stu-id="add2d-106">What is middleware?</span></span>

<span data-ttu-id="add2d-107">Un intergiciel est un logiciel qui est assemblé dans un pipeline d’application pour gérer les requêtes et les réponses.</span><span class="sxs-lookup"><span data-stu-id="add2d-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="add2d-108">Chaque composant :</span><span class="sxs-lookup"><span data-stu-id="add2d-108">Each component:</span></span>

* <span data-ttu-id="add2d-109">Choisit de passer la requête au composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="add2d-110">Peut travailler avant et après l’appel du composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="add2d-111">Les délégués de requête sont utilisés pour créer le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="add2d-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="add2d-112">Les délégués de requête gèrent chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="add2d-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="add2d-113">Les délégués de requête sont configurés à l’aide des méthodes d’extension [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) et [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="add2d-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="add2d-114">Chaque délégué de requête peut être spécifié inline comme méthode anonyme (appelée intergiciel inline) ou peut être défini dans une classe réutilisable.</span><span class="sxs-lookup"><span data-stu-id="add2d-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="add2d-115">Ces classes réutilisables et les méthodes anonymes inline sont des *intergiciels* ou des *composants d’intergiciel*.</span><span class="sxs-lookup"><span data-stu-id="add2d-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="add2d-116">Chaque composant d’intergiciel du pipeline de requête est chargé de l’appel du composant suivant dans le pipeline, ou du court-circuit de la chaîne, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="add2d-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="add2d-117">[Migrer des modules HTTP vers les intergiciels (middleware)](xref:migration/http-modules) explique la différence entre les pipelines de requêtes dans ASP.NET Core et ASP.NET 4.x, puis fournit d’autres exemples d’intergiciels.</span><span class="sxs-lookup"><span data-stu-id="add2d-117">[Migrate HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="add2d-118">Création d’un pipeline d’intergiciel (middleware) avec IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="add2d-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="add2d-119">Le pipeline de requête ASP.NET Core se compose d’une séquence de délégués de requête, appelés l’un après l’autre, comme le montre ce diagramme (le thread d’exécution suit les flèches noires) :</span><span class="sxs-lookup"><span data-stu-id="add2d-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Modèle de traitement des requêtes montrant une requête qui arrive et qui est traitée par trois intergiciels, puis la réponse qui quitte l’application.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="add2d-123">Chaque délégué peut effectuer des opérations avant et après le délégué suivant.</span><span class="sxs-lookup"><span data-stu-id="add2d-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="add2d-124">Un délégué peut également décider de ne pas passer une requête au délégué suivant. On parle alors de court-circuit du pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="add2d-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="add2d-125">Un court-circuit est souvent souhaitable car il évite le travail inutile.</span><span class="sxs-lookup"><span data-stu-id="add2d-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="add2d-126">Par exemple, l’intergiciel de fichiers statiques peut retourner une requête pour un fichier statique et court-circuiter le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="add2d-127">Les délégués de gestion des exceptions doivent être appelés tôt dans le pipeline pour qu’ils puissent intercepter les exceptions qui se produisent dans les phases ultérieures du pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="add2d-128">L’application ASP.NET Core la plus simple possible définit un seul délégué de requête qui gère toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="add2d-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="add2d-129">Ce cas ne fait pas appel à un pipeline de requête réel.</span><span class="sxs-lookup"><span data-stu-id="add2d-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="add2d-130">À la place, une seule fonction anonyme est appelée en réponse à chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="add2d-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="add2d-131">Le premier délégué [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) termine le pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="add2d-132">Vous pouvez chaîner plusieurs délégués de requête avec [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="add2d-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="add2d-133">Le paramètre `next` représente le délégué suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="add2d-134">(N’oubliez pas que vous pouvez court-circuiter le pipeline en n’appelant *pas* le paramètre *next*.) Vous pouvez généralement effectuer des actions à la fois avant et après le délégué suivant, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="add2d-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="add2d-135">N’appelez pas `next.Invoke` une fois que la réponse a été envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="add2d-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="add2d-136">Les changements apportés à `HttpResponse` après le démarrage de la réponse lèvent une exception.</span><span class="sxs-lookup"><span data-stu-id="add2d-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="add2d-137">Par exemple, les changements comme la définition des en-têtes, du code d’état, etc., lèvent une exception.</span><span class="sxs-lookup"><span data-stu-id="add2d-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="add2d-138">Écrire dans le corps de la réponse après avoir appelé `next` :</span><span class="sxs-lookup"><span data-stu-id="add2d-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="add2d-139">Peut entraîner une violation de protocole.</span><span class="sxs-lookup"><span data-stu-id="add2d-139">May cause a protocol violation.</span></span> <span data-ttu-id="add2d-140">Par exemple, écrire plus que le `content-length` indiqué.</span><span class="sxs-lookup"><span data-stu-id="add2d-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="add2d-141">Peut altérer le format du corps.</span><span class="sxs-lookup"><span data-stu-id="add2d-141">May corrupt the body format.</span></span> <span data-ttu-id="add2d-142">Par exemple, l’écriture d’un pied de page HTML dans un fichier CSS.</span><span class="sxs-lookup"><span data-stu-id="add2d-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="add2d-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) est un indice utile pour indiquer si les en-têtes ont été envoyés et/ou si le corps a fait l’objet d’écritures.</span><span class="sxs-lookup"><span data-stu-id="add2d-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="add2d-144">Classement</span><span class="sxs-lookup"><span data-stu-id="add2d-144">Ordering</span></span>

<span data-ttu-id="add2d-145">L’ordre dans lequel les composants d’intergiciel sont ajoutés dans la méthode `Configure` définit l’ordre dans lequel ils sont appelés sur les requêtes, et l’ordre inverse pour la réponse.</span><span class="sxs-lookup"><span data-stu-id="add2d-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="add2d-146">Cet ordre est critique pour la sécurité, les performances et le fonctionnement.</span><span class="sxs-lookup"><span data-stu-id="add2d-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="add2d-147">La méthode Configure (illustrée ci-dessous) ajoute les composants d’intergiciel suivants :</span><span class="sxs-lookup"><span data-stu-id="add2d-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="add2d-148">Gestion des erreurs/exceptions</span><span class="sxs-lookup"><span data-stu-id="add2d-148">Exception/error handling</span></span>
2. <span data-ttu-id="add2d-149">Serveur de fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="add2d-149">Static file server</span></span>
3. <span data-ttu-id="add2d-150">Authentification</span><span class="sxs-lookup"><span data-stu-id="add2d-150">Authentication</span></span>
4. <span data-ttu-id="add2d-151">MVC</span><span class="sxs-lookup"><span data-stu-id="add2d-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="add2d-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="add2d-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="add2d-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="add2d-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="add2d-154">Dans le code ci-dessus, `UseExceptionHandler` est le premier composant d’intergiciel ajouté au pipeline. Il intercepte donc toutes les exceptions qui se produisent dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="add2d-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="add2d-155">L’intergiciel de fichiers statiques est appelé tôt dans le pipeline pour qu’il puisse gérer les requêtes et procéder au court-circuit sans passer par les composants restants.</span><span class="sxs-lookup"><span data-stu-id="add2d-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="add2d-156">L’intergiciel de fichiers statiques ne fournit **aucune** vérification d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="add2d-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="add2d-157">Tous les fichiers qu’il sert, notamment ceux sous *wwwroot* sont accessibles à tous.</span><span class="sxs-lookup"><span data-stu-id="add2d-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="add2d-158">Consultez [Utiliser des fichiers statiques](xref:fundamentals/static-files) pour une approche permettant de sécuriser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="add2d-158">See [Work with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="add2d-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="add2d-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="add2d-160">Si la requête n’est pas gérée par l’intergiciel de fichiers statiques, elle est transmise à l’intergiciel Identité (`app.UseAuthentication`), qui effectue l’authentification.</span><span class="sxs-lookup"><span data-stu-id="add2d-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="add2d-161">L’intergiciel Identité ne court-circuite pas les requêtes non authentifiées.</span><span class="sxs-lookup"><span data-stu-id="add2d-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="add2d-162">Même s’il authentifie les requêtes, l’autorisation (et le refus) interviennent uniquement après que MVC sélectionne une page Razor/un contrôleur et une action spécifiques.</span><span class="sxs-lookup"><span data-stu-id="add2d-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="add2d-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="add2d-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="add2d-164">Si la requête n’est pas gérée par l’intergiciel de fichiers statiques, elle est transmise à l’intergiciel Identité (`app.UseIdentity`), qui effectue l’authentification.</span><span class="sxs-lookup"><span data-stu-id="add2d-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="add2d-165">L’intergiciel Identité ne court-circuite pas les requêtes non authentifiées.</span><span class="sxs-lookup"><span data-stu-id="add2d-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="add2d-166">Même s’il authentifie les requêtes, l’autorisation (et le refus) interviennent uniquement après que MVC sélectionne un contrôleur et une action spécifiques.</span><span class="sxs-lookup"><span data-stu-id="add2d-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="add2d-167">L’exemple suivant montre un ordre d’intergiciels où les requêtes pour les fichiers statiques sont gérées par l’intergiciel de fichiers statiques avant l’intergiciel de compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="add2d-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="add2d-168">Les fichiers statiques ne sont pas compressés avec cet ordre d’intergiciels.</span><span class="sxs-lookup"><span data-stu-id="add2d-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="add2d-169">Les réponses MVC de [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) peuvent être compressées.</span><span class="sxs-lookup"><span data-stu-id="add2d-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="add2d-170">Use, Run et Map</span><span class="sxs-lookup"><span data-stu-id="add2d-170">Use, Run, and Map</span></span>

<span data-ttu-id="add2d-171">Vous configurez le pipeline HTTP avec `Use`, `Run` et `Map`.</span><span class="sxs-lookup"><span data-stu-id="add2d-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="add2d-172">La méthode `Use` peut court-circuiter le pipeline (autrement dit, si elle n’appelle pas un délégué de requête `next`).</span><span class="sxs-lookup"><span data-stu-id="add2d-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="add2d-173">`Run` est une convention, et certains composants d’intergiciel peuvent exposer des méthodes `Run[Middleware]` qui s’exécutent à la fin du pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="add2d-174">Les extensions `Map*` sont utilisées comme convention pour créer une branche dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="add2d-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) crée une branche dans le pipeline de requête en fonction des correspondances du chemin de requête donné.</span><span class="sxs-lookup"><span data-stu-id="add2d-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="add2d-176">Si le chemin de requête commence par le chemin donné, la branche est exécutée.</span><span class="sxs-lookup"><span data-stu-id="add2d-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="add2d-177">Le tableau suivant présente les requêtes et les réponses de `http://localhost:1234` avec le code précédent :</span><span class="sxs-lookup"><span data-stu-id="add2d-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="add2d-178">Demande</span><span class="sxs-lookup"><span data-stu-id="add2d-178">Request</span></span> | <span data-ttu-id="add2d-179">Réponse</span><span class="sxs-lookup"><span data-stu-id="add2d-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="add2d-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="add2d-180">localhost:1234</span></span> | <span data-ttu-id="add2d-181">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="add2d-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="add2d-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="add2d-182">localhost:1234/map1</span></span> | <span data-ttu-id="add2d-183">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="add2d-183">Map Test 1</span></span> |
| <span data-ttu-id="add2d-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="add2d-184">localhost:1234/map2</span></span> | <span data-ttu-id="add2d-185">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="add2d-185">Map Test 2</span></span> |
| <span data-ttu-id="add2d-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="add2d-186">localhost:1234/map3</span></span> | <span data-ttu-id="add2d-187">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="add2d-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="add2d-188">Quand `Map` est utilisé, les segments de chemin mis en correspondance sont supprimés de `HttpRequest.Path` et ajoutés à `HttpRequest.PathBase` pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="add2d-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="add2d-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) crée une branche dans le pipeline de requête en fonction du résultat du prédicat donné.</span><span class="sxs-lookup"><span data-stu-id="add2d-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="add2d-190">Un prédicat de type `Func<HttpContext, bool>` peut être utilisé pour mapper les requêtes à une nouvelle branche du pipeline.</span><span class="sxs-lookup"><span data-stu-id="add2d-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="add2d-191">Dans l’exemple suivant, un prédicat est utilisé pour détecter la présence d’une variable de chaîne de requête `branch` :</span><span class="sxs-lookup"><span data-stu-id="add2d-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="add2d-192">Le tableau suivant présente les requêtes et les réponses de `http://localhost:1234` avec le code précédent :</span><span class="sxs-lookup"><span data-stu-id="add2d-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="add2d-193">Demande</span><span class="sxs-lookup"><span data-stu-id="add2d-193">Request</span></span> | <span data-ttu-id="add2d-194">Réponse</span><span class="sxs-lookup"><span data-stu-id="add2d-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="add2d-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="add2d-195">localhost:1234</span></span> | <span data-ttu-id="add2d-196">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="add2d-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="add2d-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="add2d-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="add2d-198">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="add2d-198">Branch used = master</span></span>|

<span data-ttu-id="add2d-199">`Map` prend en charge l’imbrication, par exemple :</span><span class="sxs-lookup"><span data-stu-id="add2d-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="add2d-200">`Map` peut également faire correspondre plusieurs segments à la fois, par exemple :</span><span class="sxs-lookup"><span data-stu-id="add2d-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="add2d-201">Intergiciels (middleware) intégrés</span><span class="sxs-lookup"><span data-stu-id="add2d-201">Built-in middleware</span></span>

<span data-ttu-id="add2d-202">ASP.NET Core est fourni avec les composants d’intergiciel suivants et une description de l’ordre dans lequel ils doivent être ajoutés :</span><span class="sxs-lookup"><span data-stu-id="add2d-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="add2d-203">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="add2d-203">Middleware</span></span> | <span data-ttu-id="add2d-204">Description</span><span class="sxs-lookup"><span data-stu-id="add2d-204">Description</span></span> | <span data-ttu-id="add2d-205">Trier</span><span class="sxs-lookup"><span data-stu-id="add2d-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="add2d-206">Authentification</span><span class="sxs-lookup"><span data-stu-id="add2d-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="add2d-207">Prend en charge l’authentification.</span><span class="sxs-lookup"><span data-stu-id="add2d-207">Provides authentication support.</span></span> | <span data-ttu-id="add2d-208">Avant que `HttpContext.User` ne soit nécessaire.</span><span class="sxs-lookup"><span data-stu-id="add2d-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="add2d-209">Terminal pour les rappels OAuth.</span><span class="sxs-lookup"><span data-stu-id="add2d-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="add2d-210">CORS</span><span class="sxs-lookup"><span data-stu-id="add2d-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="add2d-211">Configure le partage des ressources cross-origin (CORS).</span><span class="sxs-lookup"><span data-stu-id="add2d-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="add2d-212">Avant les composants qui utilisent CORS.</span><span class="sxs-lookup"><span data-stu-id="add2d-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="add2d-213">Diagnostics</span><span class="sxs-lookup"><span data-stu-id="add2d-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="add2d-214">Configure les diagnostics.</span><span class="sxs-lookup"><span data-stu-id="add2d-214">Configures diagnostics.</span></span> | <span data-ttu-id="add2d-215">Avant les composants qui génèrent des erreurs.</span><span class="sxs-lookup"><span data-stu-id="add2d-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="add2d-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="add2d-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="add2d-217">Transfère les en-têtes en proxy vers la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="add2d-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="add2d-218">Avant les composants qui consomment les champs mis à jour (exemples : Schéma, Hôte, IP client, Méthode).</span><span class="sxs-lookup"><span data-stu-id="add2d-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="add2d-219">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="add2d-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="add2d-220">Prend en charge la mise en cache des réponses.</span><span class="sxs-lookup"><span data-stu-id="add2d-220">Provides support for caching responses.</span></span> | <span data-ttu-id="add2d-221">Avant les composants qui nécessitent la mise en cache.</span><span class="sxs-lookup"><span data-stu-id="add2d-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="add2d-222">Compression des réponses</span><span class="sxs-lookup"><span data-stu-id="add2d-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="add2d-223">Prend en charge la compression des réponses.</span><span class="sxs-lookup"><span data-stu-id="add2d-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="add2d-224">Avant les composants qui nécessitent la compression.</span><span class="sxs-lookup"><span data-stu-id="add2d-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="add2d-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="add2d-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="add2d-226">Prend en charge la localisation.</span><span class="sxs-lookup"><span data-stu-id="add2d-226">Provides localization support.</span></span> | <span data-ttu-id="add2d-227">Avant la localisation des composants sensibles.</span><span class="sxs-lookup"><span data-stu-id="add2d-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="add2d-228">Le routage</span><span class="sxs-lookup"><span data-stu-id="add2d-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="add2d-229">Définit et contraint des routes de requête.</span><span class="sxs-lookup"><span data-stu-id="add2d-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="add2d-230">Terminal pour les routes correspondantes.</span><span class="sxs-lookup"><span data-stu-id="add2d-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="add2d-231">Session</span><span class="sxs-lookup"><span data-stu-id="add2d-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="add2d-232">Prend en charge la gestion des sessions utilisateur.</span><span class="sxs-lookup"><span data-stu-id="add2d-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="add2d-233">Avant les composants qui nécessitent la session.</span><span class="sxs-lookup"><span data-stu-id="add2d-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="add2d-234">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="add2d-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="add2d-235">Prend en charge le traitement des fichiers statiques et l’exploration des répertoires.</span><span class="sxs-lookup"><span data-stu-id="add2d-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="add2d-236">Terminal si une requête correspond à des fichiers.</span><span class="sxs-lookup"><span data-stu-id="add2d-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="add2d-237">Réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="add2d-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="add2d-238">Prend en charge la réécriture d’URL et la redirection des requêtes.</span><span class="sxs-lookup"><span data-stu-id="add2d-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="add2d-239">Avant les composants qui consomment l’URL.</span><span class="sxs-lookup"><span data-stu-id="add2d-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="add2d-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="add2d-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="add2d-241">Autorise le protocole WebSockets.</span><span class="sxs-lookup"><span data-stu-id="add2d-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="add2d-242">Avant les composants qui sont nécessaires pour accepter les requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="add2d-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="add2d-243">Intergiciel (middleware) d’écriture</span><span class="sxs-lookup"><span data-stu-id="add2d-243">Writing middleware</span></span>

<span data-ttu-id="add2d-244">Les intergiciels sont généralement encapsulés dans une classe et exposés avec une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="add2d-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="add2d-245">Prenez en compte l’intergiciel suivant, qui définit la culture de la requête actuelle à partir de la chaîne de requête :</span><span class="sxs-lookup"><span data-stu-id="add2d-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="add2d-246">Remarque : L’exemple de code ci-dessus est utilisé pour illustrer la création d’un composant d’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="add2d-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="add2d-247">Consultez [Globalisation et localisation](xref:fundamentals/localization) pour la prise en charge de la localisation intégrée d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="add2d-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="add2d-248">Vous pouvez tester l’intergiciel en passant la culture, par exemple `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="add2d-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="add2d-249">Le code suivant déplace le délégué de l’intergiciel dans une classe :</span><span class="sxs-lookup"><span data-stu-id="add2d-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> <span data-ttu-id="add2d-250">Dans ASP.NET Core 1.x, le nom de la méthode `Task` du middleware doit être `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="add2d-250">In ASP.NET Core 1.x, the middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="add2d-251">Dans ASP.NET Core 2.0 ou ultérieur, le nom peut être `Invoke` ou `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="add2d-251">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

<span data-ttu-id="add2d-252">La méthode d’extension suivante expose l’intergiciel à l’aide [d’IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) :</span><span class="sxs-lookup"><span data-stu-id="add2d-252">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="add2d-253">Le code suivant appelle l’intergiciel à partir de `Configure` :</span><span class="sxs-lookup"><span data-stu-id="add2d-253">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="add2d-254">L’intergiciel doit suivre le [principe de dépendances explicites](http://deviq.com/explicit-dependencies-principle/) en exposant ses dépendances dans son constructeur.</span><span class="sxs-lookup"><span data-stu-id="add2d-254">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="add2d-255">L’intergiciel est construit une fois par *durée de vie d’application*.</span><span class="sxs-lookup"><span data-stu-id="add2d-255">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="add2d-256">Consultez *Dépendances par requête* ci-dessous si vous avez besoin de partager des services avec un intergiciel au sein d’une requête.</span><span class="sxs-lookup"><span data-stu-id="add2d-256">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="add2d-257">Les composants d’intergiciel peuvent résoudre leurs dépendances à partir de l’injection de dépendances à l’aide des paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="add2d-257">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="add2d-258">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) peut également accepter des paramètres supplémentaires directement.</span><span class="sxs-lookup"><span data-stu-id="add2d-258">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="add2d-259">Dépendances par requête</span><span class="sxs-lookup"><span data-stu-id="add2d-259">Per-request dependencies</span></span>

<span data-ttu-id="add2d-260">Étant donné que l’intergiciel est construit au démarrage de l’application, et non par requête, les services de durée de vie *délimités* utilisés par les constructeurs de l’intergiciel ne sont pas partagés avec d’autres types injectés par des dépendances lors de chaque requête.</span><span class="sxs-lookup"><span data-stu-id="add2d-260">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="add2d-261">Si vous devez partager un service *délimité* entre votre intergiciel et d’autres types, ajoutez-le à la signature de la méthode `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="add2d-261">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="add2d-262">La méthode `Invoke` peut accepter des paramètres supplémentaires qui sont renseignés par injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="add2d-262">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="add2d-263">Exemple :</span><span class="sxs-lookup"><span data-stu-id="add2d-263">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="add2d-264">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="add2d-264">Additional resources</span></span>

* [<span data-ttu-id="add2d-265">Migrer des modules HTTP vers les intergiciels (middleware)</span><span class="sxs-lookup"><span data-stu-id="add2d-265">Migrate HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="add2d-266">Démarrage d’une application</span><span class="sxs-lookup"><span data-stu-id="add2d-266">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="add2d-267">Fonctionnalités de requête</span><span class="sxs-lookup"><span data-stu-id="add2d-267">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="add2d-268">Activation d’intergiciel (middleware) basée sur une fabrique</span><span class="sxs-lookup"><span data-stu-id="add2d-268">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="add2d-269">Activation d’intergiciel (middleware) avec un conteneur tiers</span><span class="sxs-lookup"><span data-stu-id="add2d-269">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
