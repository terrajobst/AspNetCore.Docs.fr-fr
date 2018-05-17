---
title: Intergiciel (middleware) de réécriture d’URL dans ASP.NET Core
author: guardrex
description: Découvrez la réécriture et la redirection d’URL avec l’intergiciel (middleware) de réécriture d’URL dans les applications ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: 336a097c2186bc195854bd54211d4554a577ed14
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="6a76e-103">Intergiciel (middleware) de réécriture d’URL dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a76e-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="6a76e-104">Par [Luke Latham](https://github.com/guardrex) et [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="6a76e-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="6a76e-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6a76e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6a76e-106">La réécriture d’URL consiste à modifier des URL de requête en fonction d’une ou de plusieurs règles prédéfinies.</span><span class="sxs-lookup"><span data-stu-id="6a76e-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="6a76e-107">La réécriture d’URL crée une abstraction entre les emplacements de ressources et leurs adresses afin que les emplacements et les adresses ne soient pas étroitement liés.</span><span class="sxs-lookup"><span data-stu-id="6a76e-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="6a76e-108">Il existe plusieurs scénarios dans lesquels la réécriture d’URL est utile :</span><span class="sxs-lookup"><span data-stu-id="6a76e-108">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="6a76e-109">Déplacement ou remplacement temporaire ou permanent de ressources serveur tout en maintenant les localisateurs stables pour ces ressources</span><span class="sxs-lookup"><span data-stu-id="6a76e-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="6a76e-110">Fractionnement du traitement des requêtes entre différentes applications ou entre des zones d’une application</span><span class="sxs-lookup"><span data-stu-id="6a76e-110">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="6a76e-111">Suppression, ajout ou réorganisation de segments d’URL sur des requêtes entrantes</span><span class="sxs-lookup"><span data-stu-id="6a76e-111">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="6a76e-112">Optimisation d’URL publiques pour l’optimisation du référencement d’un site auprès d’un moteur de recherche (SEO)</span><span class="sxs-lookup"><span data-stu-id="6a76e-112">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="6a76e-113">Autorisation d’utiliser une URL publique conviviale pour permettre aux utilisateurs de prévoir le contenu qu’ils trouveront en suivant un lien</span><span class="sxs-lookup"><span data-stu-id="6a76e-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="6a76e-114">Redirection des requêtes non sécurisées pour sécuriser des points de terminaison</span><span class="sxs-lookup"><span data-stu-id="6a76e-114">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="6a76e-115">Prévention des liens réactifs d’images</span><span class="sxs-lookup"><span data-stu-id="6a76e-115">Preventing image hotlinking</span></span>

<span data-ttu-id="6a76e-116">Vous pouvez définir des règles pour changer l’URL de plusieurs façons, notamment une expression régulière (regex), des règles de module mod_rewrite Apache, des règles du module de réécriture IIS et l’utilisation d’une logique de règles personnalisée.</span><span class="sxs-lookup"><span data-stu-id="6a76e-116">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="6a76e-117">Ce document présente la réécriture d’URL avec des instructions sur la façon d’utiliser l’intergiciel (middleware) de réécriture d’URL dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a76e-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="6a76e-118">La réécriture d’URL peut réduire les performances d’une application.</span><span class="sxs-lookup"><span data-stu-id="6a76e-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="6a76e-119">Si possible, vous devez limiter le nombre et la complexité des règles.</span><span class="sxs-lookup"><span data-stu-id="6a76e-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="6a76e-120">Redirection d’URL et réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="6a76e-121">La différence de formulation entre la *redirection d’URL* et la *réécriture d’URL* peut sembler subtile au premier abord, mais elle a des implications importantes sur la fourniture de ressources aux clients.</span><span class="sxs-lookup"><span data-stu-id="6a76e-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="6a76e-122">L’intergiciel de réécriture d’URL d’ASP.NET Core est capables de répondre aux besoins des deux.</span><span class="sxs-lookup"><span data-stu-id="6a76e-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="6a76e-123">Une *redirection d’URL* est une opération côté client, où le client est invité à accéder à une ressource à une autre adresse.</span><span class="sxs-lookup"><span data-stu-id="6a76e-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="6a76e-124">Cette opération exige un aller-retour au serveur.</span><span class="sxs-lookup"><span data-stu-id="6a76e-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="6a76e-125">L’URL de redirection retournée au client s’affiche dans la barre d’adresse du navigateur quand le client effectue une nouvelle requête pour la ressource.</span><span class="sxs-lookup"><span data-stu-id="6a76e-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="6a76e-126">Si `/resource` est *redirigé* vers `/different-resource`, le client demande `/resource`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="6a76e-127">Le serveur répond que le client doit obtenir la ressource à l’emplacement `/different-resource` avec un code d’état indiquant que la redirection est temporaire ou permanente.</span><span class="sxs-lookup"><span data-stu-id="6a76e-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="6a76e-128">Le client exécute une nouvelle requête pour la ressource à l’URL de redirection.</span><span class="sxs-lookup"><span data-stu-id="6a76e-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Un point de terminaison de service WebAPI a été changé temporairement de la version 1 (v1) à la version 2 (v2) sur le serveur.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="6a76e-134">Lors de la redirection des requêtes vers une URL différente, vous indiquez si la redirection est permanente ou temporaire.</span><span class="sxs-lookup"><span data-stu-id="6a76e-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="6a76e-135">Le code d’état 301 (Déplacé de façon permanente) est utilisé quand la ressource a une nouvelle URL permanente et que vous souhaitez indiquer au client que toutes les futures requêtes pour la ressource doivent utiliser la nouvelle URL.</span><span class="sxs-lookup"><span data-stu-id="6a76e-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="6a76e-136">*Le client peut mettre en cache la réponse quand un code d’état 301 est reçu.*</span><span class="sxs-lookup"><span data-stu-id="6a76e-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="6a76e-137">Le code d’état 302 (Trouvé) est utilisé quand la redirection est temporaire ou généralement sujette à changement, de sorte que le client n’ait pas à stocker et à réutiliser l’URL de redirection dans le futur.</span><span class="sxs-lookup"><span data-stu-id="6a76e-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="6a76e-138">Pour plus d’informations, consultez [RFC 2616 : Définitions de code d’état](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6a76e-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="6a76e-139">Une *réécriture d’URL* est une opération côté serveur pour fournir une ressource à partir d’une adresse de ressource différente.</span><span class="sxs-lookup"><span data-stu-id="6a76e-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="6a76e-140">La réécriture d’URL n’exige pas un aller-retour au serveur.</span><span class="sxs-lookup"><span data-stu-id="6a76e-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="6a76e-141">L’URL réécrite n’est pas retournée au client et n’apparaîtra pas dans la barre d’adresse du navigateur.</span><span class="sxs-lookup"><span data-stu-id="6a76e-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="6a76e-142">Quand `/resource` est *réécrite* en `/different-resource`, le client demande `/resource` et le serveur récupère la ressource *en interne* à l’emplacement `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="6a76e-143">Même si le client peut récupérer la ressource à l’URL réécrite, il n’est pas informé que la ressource existe à l’URL réécrite quand il effectue sa requête et reçoit la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a76e-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Un point de terminaison de service WebAPI a été changé de la version 1 (v1) à la version 2 (v2) sur le serveur.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="6a76e-148">Exemple d’application de réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-148">URL rewriting sample app</span></span>

<span data-ttu-id="6a76e-149">Vous pouvez explorer les fonctionnalités de l’intergiciel de réécriture d’URL avec [l’exemple d’application de réécriture d’URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="6a76e-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="6a76e-150">Cette application applique des règles de réécriture et de redirection, puis affiche l’URL réécrite ou redirigée.</span><span class="sxs-lookup"><span data-stu-id="6a76e-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="6a76e-151">Quand utiliser l’intergiciel (middleware) de réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="6a76e-152">Utilisez l’intergiciel de réécriture d’URL quand vous ne pouvez pas utiliser le [module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite) avec IIS sur Windows Server, le [module mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) sur Apache Server, la [réécriture d’URL sur Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) ou si votre application est hébergée sur un [serveur HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="6a76e-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="6a76e-153">Les principales raisons d’utiliser les technologies de réécriture d’URL basées sur les serveurs dans IIS, Apache ou Nginx sont que l’intergiciel ne prend pas en charge toutes les fonctionnalités de ces modules et que les performances de l’intergiciel ne correspondront probablement pas à celles des modules.</span><span class="sxs-lookup"><span data-stu-id="6a76e-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="6a76e-154">Toutefois, certaines fonctionnalités des modules serveur ne fonctionnent pas avec les projets ASP.NET Core, comme les contraintes `IsFile` et `IsDirectory` du module de réécriture IIS.</span><span class="sxs-lookup"><span data-stu-id="6a76e-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="6a76e-155">Dans ces scénarios, utilisez plutôt l’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="6a76e-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="6a76e-156">Package</span><span class="sxs-lookup"><span data-stu-id="6a76e-156">Package</span></span>

<span data-ttu-id="6a76e-157">Pour inclure l’intergiciel dans votre projet, ajoutez une référence au package [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/).</span><span class="sxs-lookup"><span data-stu-id="6a76e-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="6a76e-158">Cette fonctionnalité est disponible pour les applications qui ciblent ASP.NET Core 1.1 ou versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6a76e-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="6a76e-159">Extension et options</span><span class="sxs-lookup"><span data-stu-id="6a76e-159">Extension and options</span></span>

<span data-ttu-id="6a76e-160">Établissez vos règles de réécriture d’URL et de redirection en créant une instance de la classe `RewriteOptions` avec des méthodes d’extension pour chacune de vos règles.</span><span class="sxs-lookup"><span data-stu-id="6a76e-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="6a76e-161">Chaînez plusieurs règles dans l’ordre dans lequel vous voulez qu’elles soient traitées.</span><span class="sxs-lookup"><span data-stu-id="6a76e-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="6a76e-162">Les `RewriteOptions` sont passées dans l’intergiciel de réécriture d’URL quand il est ajouté au pipeline de requête avec `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a76e-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a76e-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

### <a name="url-redirect"></a><span data-ttu-id="6a76e-165">Redirection d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-165">URL redirect</span></span>

<span data-ttu-id="6a76e-166">Utilisez `AddRedirect` pour rediriger des requêtes.</span><span class="sxs-lookup"><span data-stu-id="6a76e-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="6a76e-167">Le premier paramètre contient votre expression régulière pour la mise en correspondance sur le chemin de l’URL entrante.</span><span class="sxs-lookup"><span data-stu-id="6a76e-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="6a76e-168">Le deuxième paramètre est la chaîne de remplacement.</span><span class="sxs-lookup"><span data-stu-id="6a76e-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="6a76e-169">Le troisième paramètre, le cas échéant, spécifie le code d’état.</span><span class="sxs-lookup"><span data-stu-id="6a76e-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="6a76e-170">Si vous ne spécifiez pas le code d’état, sa valeur par défaut est 302 (Trouvé), ce qui indique que la ressource est temporairement déplacée ou remplacée.</span><span class="sxs-lookup"><span data-stu-id="6a76e-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a76e-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a76e-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="6a76e-173">Dans un navigateur dans lequel les outils de développement sont activés, effectuez une requête à l’exemple d’application avec le chemin `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="6a76e-174">L’expression régulière établit une correspondance avec le chemin de la requête sur `redirect-rule/(.*)`, et le chemin est remplacé par `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="6a76e-175">L’URL de redirection est renvoyée au client avec le code d’état 302 (Trouvé).</span><span class="sxs-lookup"><span data-stu-id="6a76e-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="6a76e-176">Le navigateur effectue une nouvelle requête à l’URL de redirection, qui apparaît dans la barre d’adresse du navigateur.</span><span class="sxs-lookup"><span data-stu-id="6a76e-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="6a76e-177">Étant donné qu’aucune règle ne correspond à l’URL de redirection dans l’exemple d’application, la deuxième requête reçoit la réponse 200 (OK) de l’application et le corps de la réponse indique l’URL de redirection.</span><span class="sxs-lookup"><span data-stu-id="6a76e-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="6a76e-178">Un aller-retour est effectué vers le serveur quand une URL est *redirigée*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="6a76e-179">Soyez prudent lors de l’établissement de vos règles de redirection.</span><span class="sxs-lookup"><span data-stu-id="6a76e-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="6a76e-180">Vos règles de redirection sont évaluées à chaque requête à l’application, notamment après une redirection.</span><span class="sxs-lookup"><span data-stu-id="6a76e-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="6a76e-181">Il est facile de créer accidentellement une boucle de redirections infinies.</span><span class="sxs-lookup"><span data-stu-id="6a76e-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="6a76e-182">Requête d’origine : `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="6a76e-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![Fenêtre de navigateur avec les requêtes et les réponses suivies par les Outils de développement](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="6a76e-184">La partie de l’expression entre parenthèses est appelée *groupe de capture*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="6a76e-185">Le point (`.`) de l’expression signifie *mettre en correspondance n’importe quel caractère*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="6a76e-186">L’astérisque (`*`) indique *mettre en correspondance zéro occurrence ou plus du caractère précédent*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="6a76e-187">Par conséquent, les deux derniers segments de chemin de l’URL, `1234/5678`, sont capturés par le groupe de capture `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="6a76e-188">Toute valeur fournie dans l’URL de la requête après `redirect-rule/` est capturée par ce groupe de capture unique.</span><span class="sxs-lookup"><span data-stu-id="6a76e-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="6a76e-189">Dans la chaîne de remplacement, les groupes capturés sont injectés dans la chaîne avec le signe dollar (`$`) suivi du numéro de séquence de la capture.</span><span class="sxs-lookup"><span data-stu-id="6a76e-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="6a76e-190">La valeur du premier groupe de capture est obtenue avec `$1`, la deuxième avec `$2`, et ainsi de suite en séquence pour les groupes de capture de votre expression régulière.</span><span class="sxs-lookup"><span data-stu-id="6a76e-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="6a76e-191">Comme il n’y a qu’un seul groupe capturé dans l’expression régulière de la règle de redirection de l’exemple d’application, un seul groupe est injecté dans la chaîne de remplacement, à savoir `$1`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="6a76e-192">Quand la règle est appliquée, l’URL devient `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="6a76e-193">Redirection d’URL vers un point de terminaison sécurisé</span><span class="sxs-lookup"><span data-stu-id="6a76e-193">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="6a76e-194">Utilisez `AddRedirectToHttps` pour rediriger les requêtes HTTP vers le même hôte et chemin à l’aide de HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="6a76e-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="6a76e-195">Si le code d’état n’est pas fourni, la valeur par défaut de l’intergiciel est 302 (Trouvé).</span><span class="sxs-lookup"><span data-stu-id="6a76e-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="6a76e-196">Si le port n’est pas fourni, la valeur par défaut de l’intergiciel est `null`, ce qui signifie que le protocole devient `https://` et que le client accède à la ressource sur le port 443.</span><span class="sxs-lookup"><span data-stu-id="6a76e-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="6a76e-197">L’exemple montre comment définir le code d’état sur 301 (Déplacé de façon permanente) et remplacer le port par 5001.</span><span class="sxs-lookup"><span data-stu-id="6a76e-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="6a76e-198">Utilisez `AddRedirectToHttpsPermanent` pour rediriger les requêtes non sécurisées vers le même hôte et chemin avec le protocole HTTPS sécurisé (`https://` sur le port 443).</span><span class="sxs-lookup"><span data-stu-id="6a76e-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="6a76e-199">L’intergiciel définit le code d’état sur 301 (Déplacé de façon permanente).</span><span class="sxs-lookup"><span data-stu-id="6a76e-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

<span data-ttu-id="6a76e-200">L’exemple d’application peut montrer comment utiliser `AddRedirectToHttps` ou `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-200">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="6a76e-201">Ajoutez la méthode d’extension à `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-201">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="6a76e-202">Effectuez une requête non sécurisée à l’application à n’importe quelle URL.</span><span class="sxs-lookup"><span data-stu-id="6a76e-202">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="6a76e-203">Ignorez l’avertissement de sécurité du navigateur indiquant que le certificat auto-signé n’est pas approuvé.</span><span class="sxs-lookup"><span data-stu-id="6a76e-203">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="6a76e-204">Requête d’origine utilisant `AddRedirectToHttps(301, 5001)` : `/secure`</span><span class="sxs-lookup"><span data-stu-id="6a76e-204">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Fenêtre de navigateur avec les requêtes et les réponses suivies par les Outils de développement](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="6a76e-206">Requête d’origine utilisant `AddRedirectToHttpsPermanent` : `/secure`</span><span class="sxs-lookup"><span data-stu-id="6a76e-206">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Fenêtre de navigateur avec les requêtes et les réponses suivies par les Outils de développement](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="6a76e-208">Réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-208">URL rewrite</span></span>

<span data-ttu-id="6a76e-209">Utilisez `AddRewrite` pour créer une règle pour la réécriture d’URL.</span><span class="sxs-lookup"><span data-stu-id="6a76e-209">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="6a76e-210">Le premier paramètre contient votre expression régulière pour la mise en correspondance sur le chemin de l’URL entrante.</span><span class="sxs-lookup"><span data-stu-id="6a76e-210">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="6a76e-211">Le deuxième paramètre est la chaîne de remplacement.</span><span class="sxs-lookup"><span data-stu-id="6a76e-211">The second parameter is the replacement string.</span></span> <span data-ttu-id="6a76e-212">Le troisième paramètre, `skipRemainingRules: {true|false}`, indique à l’intergiciel d’ignorer, ou non, les règles de réécriture supplémentaires si la règle actuelle est appliquée.</span><span class="sxs-lookup"><span data-stu-id="6a76e-212">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a76e-213">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-213">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a76e-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="6a76e-215">Requête d’origine : `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="6a76e-215">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Fenêtre de navigateur avec la requête et la réponse suivies par les Outils de développement](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="6a76e-217">La première chose que vous remarquez dans l’expression régulière est le signe `^` (accent circonflexe) au début de l’expression.</span><span class="sxs-lookup"><span data-stu-id="6a76e-217">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="6a76e-218">Cela signifie que la correspondance commence au début du chemin de l’URL.</span><span class="sxs-lookup"><span data-stu-id="6a76e-218">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="6a76e-219">Dans l’exemple précédent, avec la règle de redirection, `redirect-rule/(.*)`, il n’y a pas d’accent circonflexe au début de l’expression régulière. Par conséquent, n’importe quels caractères peuvent précéder `redirect-rule/` dans le chemin pour une correspondance valide.</span><span class="sxs-lookup"><span data-stu-id="6a76e-219">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="6a76e-220">Chemin d’accès</span><span class="sxs-lookup"><span data-stu-id="6a76e-220">Path</span></span>                               | <span data-ttu-id="6a76e-221">Faire correspondre à</span><span class="sxs-lookup"><span data-stu-id="6a76e-221">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="6a76e-222">Oui</span><span class="sxs-lookup"><span data-stu-id="6a76e-222">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="6a76e-223">Oui</span><span class="sxs-lookup"><span data-stu-id="6a76e-223">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="6a76e-224">Oui</span><span class="sxs-lookup"><span data-stu-id="6a76e-224">Yes</span></span>   |

<span data-ttu-id="6a76e-225">La règle de réécriture, `^rewrite-rule/(\d+)/(\d+)`, établit une correspondance uniquement avec des chemins d’accès s’ils commencent par `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-225">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="6a76e-226">Notez la différence de correspondance entre la règle de réécriture ci-dessous et la règle de redirection ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="6a76e-226">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="6a76e-227">Chemin d’accès</span><span class="sxs-lookup"><span data-stu-id="6a76e-227">Path</span></span>                              | <span data-ttu-id="6a76e-228">Faire correspondre à</span><span class="sxs-lookup"><span data-stu-id="6a76e-228">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="6a76e-229">Oui</span><span class="sxs-lookup"><span data-stu-id="6a76e-229">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="6a76e-230">Non</span><span class="sxs-lookup"><span data-stu-id="6a76e-230">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="6a76e-231">Non</span><span class="sxs-lookup"><span data-stu-id="6a76e-231">No</span></span>    |

<span data-ttu-id="6a76e-232">À la suite de la partie `^rewrite-rule/` de l’expression se trouvent deux groupes de capture, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-232">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="6a76e-233">`\d` signifie *établir une correspondance avec un chiffre (nombre)*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-233">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="6a76e-234">Le signe plus (`+`) signifie *établir une correspondance avec une ou plusieurs occurrences du caractère précédent*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-234">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="6a76e-235">Par conséquent, l’URL doit contenir un nombre suivi d’une barre oblique, elle-même suivie d’un autre nombre.</span><span class="sxs-lookup"><span data-stu-id="6a76e-235">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="6a76e-236">Ces groupes sont injectés dans l’URL réécrite sous la forme `$1` et `$2`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-236">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="6a76e-237">La chaîne de remplacement de la règle de réécriture place les groupes capturés dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="6a76e-237">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="6a76e-238">Le chemin demandé `/rewrite-rule/1234/5678` est réécrit pour obtenir la ressource à l’emplacement `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-238">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="6a76e-239">Si une chaîne de requête est présente dans la requête d’origine, elle est conservée lors de la réécriture de l’URL.</span><span class="sxs-lookup"><span data-stu-id="6a76e-239">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="6a76e-240">Il n’y a pas d’aller-retour vers le serveur pour obtenir la ressource.</span><span class="sxs-lookup"><span data-stu-id="6a76e-240">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="6a76e-241">Si la ressource existe, il a récupérée et retournée au client avec le code d’état 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="6a76e-241">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="6a76e-242">Étant donné que le client n’est pas redirigé, l’URL dans la barre d’adresse du navigateur ne change pas.</span><span class="sxs-lookup"><span data-stu-id="6a76e-242">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="6a76e-243">En ce qui concerne le client, l’opération de réécriture de l’URL ne s’est jamais produite.</span><span class="sxs-lookup"><span data-stu-id="6a76e-243">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="6a76e-244">Utilisez `skipRemainingRules: true` autant que possible, car la mise en correspondance de règles est un processus coûteux et qui réduit le temps de réponse de l’application.</span><span class="sxs-lookup"><span data-stu-id="6a76e-244">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="6a76e-245">Pour obtenir la réponse d’application la plus rapide :</span><span class="sxs-lookup"><span data-stu-id="6a76e-245">For the fastest app response:</span></span>
> * <span data-ttu-id="6a76e-246">Ordonnez vos règles de réécriture de la règle la plus souvent mise en correspondance à la règle la moins souvent mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="6a76e-246">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="6a76e-247">Ignorez le traitement des règles restantes quand une correspondance est trouvée et qu’aucun traitement de règle supplémentaire n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="6a76e-247">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="6a76e-248">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="6a76e-248">Apache mod_rewrite</span></span>

<span data-ttu-id="6a76e-249">Appliquez des règles Apache mod_rewrite avec `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-249">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="6a76e-250">Vérifiez que le fichier de règles est déployé avec l’application.</span><span class="sxs-lookup"><span data-stu-id="6a76e-250">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="6a76e-251">Pour obtenir plus d’informations et des exemples de règles mod_rewrite, consultez [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="6a76e-251">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a76e-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="6a76e-253">Un `StreamReader` est utilisé pour lire les règles à partir du fichier de règles *ApacheModRewrite.txt*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-253">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a76e-254">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="6a76e-255">Le premier paramètre prend un `IFileProvider`, qui est fourni par le biais de [l’injection de dépendances](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="6a76e-255">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="6a76e-256">`IHostingEnvironment` est injecté pour fournir `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-256">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="6a76e-257">Le deuxième paramètre est le chemin à votre fichier de règles, qui est *ApacheModRewrite.txt* dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="6a76e-257">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="6a76e-258">L’exemple d’application redirige les requêtes de `/apache-mod-rules-redirect/(.\*)` vers `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-258">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="6a76e-259">Le code d’état de la réponse est 302 (Trouvé).</span><span class="sxs-lookup"><span data-stu-id="6a76e-259">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="6a76e-260">Requête d’origine : `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="6a76e-260">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Fenêtre de navigateur avec les requêtes et les réponses suivies par les Outils de développement](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="6a76e-262">Variables serveur prises en charge</span><span class="sxs-lookup"><span data-stu-id="6a76e-262">Supported server variables</span></span>

<span data-ttu-id="6a76e-263">L’intergiciel prend en charge les variables de serveur Apache mod_rewrite suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a76e-263">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="6a76e-264">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6a76e-264">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="6a76e-265">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="6a76e-265">HTTP_ACCEPT</span></span>
* <span data-ttu-id="6a76e-266">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="6a76e-266">HTTP_CONNECTION</span></span>
* <span data-ttu-id="6a76e-267">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="6a76e-267">HTTP_COOKIE</span></span>
* <span data-ttu-id="6a76e-268">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="6a76e-268">HTTP_FORWARDED</span></span>
* <span data-ttu-id="6a76e-269">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="6a76e-269">HTTP_HOST</span></span>
* <span data-ttu-id="6a76e-270">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="6a76e-270">HTTP_REFERER</span></span>
* <span data-ttu-id="6a76e-271">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="6a76e-271">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="6a76e-272">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6a76e-272">HTTPS</span></span>
* <span data-ttu-id="6a76e-273">IPV6</span><span class="sxs-lookup"><span data-stu-id="6a76e-273">IPV6</span></span>
* <span data-ttu-id="6a76e-274">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="6a76e-274">QUERY_STRING</span></span>
* <span data-ttu-id="6a76e-275">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6a76e-275">REMOTE_ADDR</span></span>
* <span data-ttu-id="6a76e-276">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="6a76e-276">REMOTE_PORT</span></span>
* <span data-ttu-id="6a76e-277">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6a76e-277">REQUEST_FILENAME</span></span>
* <span data-ttu-id="6a76e-278">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="6a76e-278">REQUEST_METHOD</span></span>
* <span data-ttu-id="6a76e-279">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="6a76e-279">REQUEST_SCHEME</span></span>
* <span data-ttu-id="6a76e-280">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="6a76e-280">REQUEST_URI</span></span>
* <span data-ttu-id="6a76e-281">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6a76e-281">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="6a76e-282">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="6a76e-282">SERVER_ADDR</span></span>
* <span data-ttu-id="6a76e-283">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="6a76e-283">SERVER_PORT</span></span>
* <span data-ttu-id="6a76e-284">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="6a76e-284">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="6a76e-285">TIME</span><span class="sxs-lookup"><span data-stu-id="6a76e-285">TIME</span></span>
* <span data-ttu-id="6a76e-286">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="6a76e-286">TIME_DAY</span></span>
* <span data-ttu-id="6a76e-287">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="6a76e-287">TIME_HOUR</span></span>
* <span data-ttu-id="6a76e-288">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="6a76e-288">TIME_MIN</span></span>
* <span data-ttu-id="6a76e-289">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="6a76e-289">TIME_MON</span></span>
* <span data-ttu-id="6a76e-290">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="6a76e-290">TIME_SEC</span></span>
* <span data-ttu-id="6a76e-291">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="6a76e-291">TIME_WDAY</span></span>
* <span data-ttu-id="6a76e-292">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="6a76e-292">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="6a76e-293">Règles du module de réécriture d’URL IIS</span><span class="sxs-lookup"><span data-stu-id="6a76e-293">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="6a76e-294">Pour utiliser les règles qui s’appliquent au module de réécriture d’URL IIS, utilisez `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-294">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="6a76e-295">Vérifiez que le fichier de règles est déployé avec l’application.</span><span class="sxs-lookup"><span data-stu-id="6a76e-295">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="6a76e-296">N’indiquez pas à l’intergiciel d’utiliser votre fichier *web.config* lors d’une exécution sur Windows Server IIS.</span><span class="sxs-lookup"><span data-stu-id="6a76e-296">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="6a76e-297">Avec IIS, ces règles doivent être stockées en dehors de votre fichier *web.config* pour éviter les conflits avec le module de réécriture IIS.</span><span class="sxs-lookup"><span data-stu-id="6a76e-297">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="6a76e-298">Pour obtenir plus d’informations et des exemples de règles du module de réécriture d’URL IIS, consultez [Utilisation du module de réécriture d’URL 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) et [Informations de référence sur la configuration du module de réécriture d’URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="6a76e-298">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a76e-299">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-299">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="6a76e-300">Un `StreamReader` est utilisé pour lire les règles à partir du fichier de règles *IISUrlRewrite.xml*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-300">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a76e-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="6a76e-302">Le premier paramètre prend un `IFileProvider`, tandis que le deuxième paramètre est le chemin à votre fichier de règles XML, qui est *IISUrlRewrite.xml* dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="6a76e-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="6a76e-303">L’exemple d’application réécrit les requêtes de `/iis-rules-rewrite/(.*)` vers `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="6a76e-304">La réponse est envoyée au client avec le code d’état 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="6a76e-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="6a76e-305">Requête d’origine : `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="6a76e-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Fenêtre de navigateur avec la requête et la réponse suivies par les Outils de développement](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="6a76e-307">Si vous avez un module de réécriture IIS actif pour lequel des règles au niveau du serveur qui affecteraient de façon non souhaitée votre application sont configurées, vous pouvez le désactiver pour une application.</span><span class="sxs-lookup"><span data-stu-id="6a76e-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="6a76e-308">Pour plus d’informations, consultez [Désactivation de modules IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="6a76e-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="6a76e-309">Fonctionnalités non prises en charge</span><span class="sxs-lookup"><span data-stu-id="6a76e-309">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a76e-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6a76e-311">L’intergiciel intégré à ASP.NET Core 2.x ne prend pas en charge les fonctionnalités de module de réécriture d’URL IIS suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a76e-311">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="6a76e-312">Règles de trafic sortant</span><span class="sxs-lookup"><span data-stu-id="6a76e-312">Outbound Rules</span></span>
* <span data-ttu-id="6a76e-313">Variables serveur personnalisées</span><span class="sxs-lookup"><span data-stu-id="6a76e-313">Custom Server Variables</span></span>
* <span data-ttu-id="6a76e-314">Caractères génériques</span><span class="sxs-lookup"><span data-stu-id="6a76e-314">Wildcards</span></span>
* <span data-ttu-id="6a76e-315">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="6a76e-315">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a76e-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a76e-317">L’intergiciel intégré à ASP.NET Core 1.x ne prend pas en charge les fonctionnalités de module de réécriture d’URL IIS suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a76e-317">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="6a76e-318">Règles globales</span><span class="sxs-lookup"><span data-stu-id="6a76e-318">Global Rules</span></span>
* <span data-ttu-id="6a76e-319">Règles de trafic sortant</span><span class="sxs-lookup"><span data-stu-id="6a76e-319">Outbound Rules</span></span>
* <span data-ttu-id="6a76e-320">Tables de réécriture</span><span class="sxs-lookup"><span data-stu-id="6a76e-320">Rewrite Maps</span></span>
* <span data-ttu-id="6a76e-321">Action CustomResponse</span><span class="sxs-lookup"><span data-stu-id="6a76e-321">CustomResponse Action</span></span>
* <span data-ttu-id="6a76e-322">Variables serveur personnalisées</span><span class="sxs-lookup"><span data-stu-id="6a76e-322">Custom Server Variables</span></span>
* <span data-ttu-id="6a76e-323">Caractères génériques</span><span class="sxs-lookup"><span data-stu-id="6a76e-323">Wildcards</span></span>
* <span data-ttu-id="6a76e-324">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="6a76e-324">Action:CustomResponse</span></span>
* <span data-ttu-id="6a76e-325">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="6a76e-325">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="6a76e-326">Variables serveur prises en charge</span><span class="sxs-lookup"><span data-stu-id="6a76e-326">Supported server variables</span></span>

<span data-ttu-id="6a76e-327">L’intergiciel prend en charge les variables serveur du module de réécriture d’URL IIS suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a76e-327">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="6a76e-328">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="6a76e-328">CONTENT_LENGTH</span></span>
* <span data-ttu-id="6a76e-329">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="6a76e-329">CONTENT_TYPE</span></span>
* <span data-ttu-id="6a76e-330">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="6a76e-330">HTTP_ACCEPT</span></span>
* <span data-ttu-id="6a76e-331">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="6a76e-331">HTTP_CONNECTION</span></span>
* <span data-ttu-id="6a76e-332">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="6a76e-332">HTTP_COOKIE</span></span>
* <span data-ttu-id="6a76e-333">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="6a76e-333">HTTP_HOST</span></span>
* <span data-ttu-id="6a76e-334">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="6a76e-334">HTTP_REFERER</span></span>
* <span data-ttu-id="6a76e-335">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-335">HTTP_URL</span></span>
* <span data-ttu-id="6a76e-336">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="6a76e-336">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="6a76e-337">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6a76e-337">HTTPS</span></span>
* <span data-ttu-id="6a76e-338">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="6a76e-338">LOCAL_ADDR</span></span>
* <span data-ttu-id="6a76e-339">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="6a76e-339">QUERY_STRING</span></span>
* <span data-ttu-id="6a76e-340">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6a76e-340">REMOTE_ADDR</span></span>
* <span data-ttu-id="6a76e-341">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="6a76e-341">REMOTE_PORT</span></span>
* <span data-ttu-id="6a76e-342">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6a76e-342">REQUEST_FILENAME</span></span>
* <span data-ttu-id="6a76e-343">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="6a76e-343">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="6a76e-344">Vous pouvez également obtenir un `IFileProvider` par le biais d’un `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-344">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="6a76e-345">Cette approche peut fournir davantage de flexibilité pour l’emplacement de vos fichiers de règles de réécriture.</span><span class="sxs-lookup"><span data-stu-id="6a76e-345">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="6a76e-346">Vérifiez que vos fichiers de règles de réécriture sont déployés sur le serveur dans le chemin que vous fournissez.</span><span class="sxs-lookup"><span data-stu-id="6a76e-346">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="6a76e-347">Règle basée sur une méthode</span><span class="sxs-lookup"><span data-stu-id="6a76e-347">Method-based rule</span></span>

<span data-ttu-id="6a76e-348">Utilisez `Add(Action<RewriteContext> applyRule)` pour implémenter votre propre logique de règle dans une méthode.</span><span class="sxs-lookup"><span data-stu-id="6a76e-348">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="6a76e-349">`RewriteContext` expose `HttpContext` pour qu’il soit utilisé dans votre méthode.</span><span class="sxs-lookup"><span data-stu-id="6a76e-349">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="6a76e-350">`context.Result` détermine la façon dont un traitement du pipeline supplémentaire est géré.</span><span class="sxs-lookup"><span data-stu-id="6a76e-350">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="6a76e-351">context.Result</span><span class="sxs-lookup"><span data-stu-id="6a76e-351">context.Result</span></span>                       | <span data-ttu-id="6a76e-352">Action</span><span class="sxs-lookup"><span data-stu-id="6a76e-352">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="6a76e-353">`RuleResult.ContinueRules` (par défaut)</span><span class="sxs-lookup"><span data-stu-id="6a76e-353">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="6a76e-354">Continuer à appliquer les règles</span><span class="sxs-lookup"><span data-stu-id="6a76e-354">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="6a76e-355">Cesser d’appliquer les règles et envoyer la réponse</span><span class="sxs-lookup"><span data-stu-id="6a76e-355">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="6a76e-356">Cesser d’appliquer les règles et envoyer le contexte à l’intergiciel suivant</span><span class="sxs-lookup"><span data-stu-id="6a76e-356">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a76e-357">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-357">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a76e-358">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-358">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="6a76e-359">L’exemple d’application présente une méthode qui redirige les requêtes de chemins qui se terminent par *.xml*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-359">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="6a76e-360">Si vous effectuez une requête pour `/file.xml`, elle est redirigée vers `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-360">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="6a76e-361">Le code d’état est défini sur 301 (Déplacé de façon permanente).</span><span class="sxs-lookup"><span data-stu-id="6a76e-361">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="6a76e-362">Pour une redirection, vous devez définir explicitement le code d’état de la réponse ; sinon, le code d’état 200 (OK) est retourné et la redirection ne se produit pas sur le client.</span><span class="sxs-lookup"><span data-stu-id="6a76e-362">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="6a76e-363">Requête d’origine : `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="6a76e-363">Original Request: `/file.xml`</span></span>

![Fenêtre de navigateur avec les requêtes et les réponses suivies par les Outils de développement pour file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="6a76e-365">Règle basée sur IRule</span><span class="sxs-lookup"><span data-stu-id="6a76e-365">IRule-based rule</span></span>

<span data-ttu-id="6a76e-366">Utilisez `Add(IRule)` pour implémenter votre propre logique de règle dans une classe qui dérive d’`IRule`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-366">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="6a76e-367">L’utilisation d’un `IRule` offre davantage de flexibilité par rapport à l’utilisation de l’approche de la règle basée sur une méthode.</span><span class="sxs-lookup"><span data-stu-id="6a76e-367">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="6a76e-368">Votre classe dérivée peut inclure un constructeur, où vous pouvez passer des paramètres pour la méthode `ApplyRule`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-368">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a76e-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a76e-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a76e-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="6a76e-371">Les valeurs des paramètres dans l’exemple d’application pour `extension` et `newPath` sont vérifiées afin de remplir plusieurs conditions.</span><span class="sxs-lookup"><span data-stu-id="6a76e-371">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="6a76e-372">`extension` doit contenir une valeur, laquelle doit être *.png*, *.jpg* ou *.gif*.</span><span class="sxs-lookup"><span data-stu-id="6a76e-372">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="6a76e-373">Si `newPath` n’est pas valide, un `ArgumentException` est levé.</span><span class="sxs-lookup"><span data-stu-id="6a76e-373">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="6a76e-374">Si vous effectuez une requête pour *image.png*, elle est redirigée vers `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-374">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="6a76e-375">Si vous effectuez une requête pour *image.jpg*, elle est redirigée vers `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="6a76e-375">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="6a76e-376">Le code d’état est défini sur 301 (Déplacé de façon permanente) et `context.Result` est défini pour cesser le traitement des règles et envoyer la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a76e-376">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="6a76e-377">Requête d’origine : `/image.png`</span><span class="sxs-lookup"><span data-stu-id="6a76e-377">Original Request: `/image.png`</span></span>

![Fenêtre de navigateur avec les requêtes et les réponses suivies par les Outils de développement pour image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="6a76e-379">Requête d’origine : `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="6a76e-379">Original Request: `/image.jpg`</span></span>

![Fenêtre de navigateur avec les requêtes et les réponses suivies par les Outils de développement pour image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="6a76e-381">Exemples d’expressions régulières</span><span class="sxs-lookup"><span data-stu-id="6a76e-381">Regex examples</span></span>

| <span data-ttu-id="6a76e-382">Goal</span><span class="sxs-lookup"><span data-stu-id="6a76e-382">Goal</span></span> | <span data-ttu-id="6a76e-383">Chaîne d’expression régulière et</span><span class="sxs-lookup"><span data-stu-id="6a76e-383">Regex String &</span></span><br><span data-ttu-id="6a76e-384">exemple de correspondance</span><span class="sxs-lookup"><span data-stu-id="6a76e-384">Match Example</span></span> | <span data-ttu-id="6a76e-385">Chaîne de remplacement et</span><span class="sxs-lookup"><span data-stu-id="6a76e-385">Replacement String &</span></span><br><span data-ttu-id="6a76e-386">exemple de sortie</span><span class="sxs-lookup"><span data-stu-id="6a76e-386">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="6a76e-387">Réécrire le chemin dans la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="6a76e-387">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="6a76e-388">Supprimer la barre oblique finale</span><span class="sxs-lookup"><span data-stu-id="6a76e-388">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="6a76e-389">Appliquer une barre oblique finale</span><span class="sxs-lookup"><span data-stu-id="6a76e-389">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="6a76e-390">Éviter la réécriture des requêtes spécifiques</span><span class="sxs-lookup"><span data-stu-id="6a76e-390">Avoid rewriting specific requests</span></span> | <span data-ttu-id="6a76e-391">`^(.*)(?<!\.axd)$` ou `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="6a76e-391">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="6a76e-392">Oui : `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="6a76e-392">Yes: `/resource.htm`</span></span><br><span data-ttu-id="6a76e-393">Non : `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="6a76e-393">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="6a76e-394">Réorganiser les segments d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-394">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="6a76e-395">Remplacer un segment d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-395">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="6a76e-396">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6a76e-396">Additional resources</span></span>

* [<span data-ttu-id="6a76e-397">Démarrage d’une application</span><span class="sxs-lookup"><span data-stu-id="6a76e-397">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="6a76e-398">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="6a76e-398">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="6a76e-399">Expressions régulières dans .NET</span><span class="sxs-lookup"><span data-stu-id="6a76e-399">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="6a76e-400">Langage des expressions régulières - Aide-mémoire</span><span class="sxs-lookup"><span data-stu-id="6a76e-400">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="6a76e-401">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="6a76e-401">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="6a76e-402">Utilisation du module de réécriture d’URL 2.0 (pour IIS)</span><span class="sxs-lookup"><span data-stu-id="6a76e-402">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="6a76e-403">Informations de référence sur la configuration du module de réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-403">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="6a76e-404">Forum du module de réécriture d’URL IIS</span><span class="sxs-lookup"><span data-stu-id="6a76e-404">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="6a76e-405">Maintenir une structure d’URL simple</span><span class="sxs-lookup"><span data-stu-id="6a76e-405">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="6a76e-406">10 conseils et astuces pour la réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="6a76e-406">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="6a76e-407">Mettre ou ne pas mettre une barre oblique</span><span class="sxs-lookup"><span data-stu-id="6a76e-407">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
