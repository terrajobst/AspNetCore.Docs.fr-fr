---
title: "À l’aide de l’authentification de Cookie sans ASP.NET Core identité"
author: rick-anderson
description: "Une explication de l’utilisation de l’authentification de cookie sans ASP.NET Core Identity"
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: 5b2f70b1f1fc9805025d6fa2256fcfcecf808b9a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="63cff-103">À l’aide de l’authentification de Cookie sans ASP.NET Core identité</span><span class="sxs-lookup"><span data-stu-id="63cff-103">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="63cff-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="63cff-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="63cff-105">Comme vous l’avez vu dans les rubriques précédentes de l’authentification, [ASP.NET Core Identity](xref:security/authentication/identity) est un fournisseur d’authentification terminé et complet pour créer et maintenir les connexions.</span><span class="sxs-lookup"><span data-stu-id="63cff-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="63cff-106">Toutefois, il pouvez que vous souhaitez utiliser votre propre logique d’authentification personnalisée avec l’authentification basée sur les cookies à des moments.</span><span class="sxs-lookup"><span data-stu-id="63cff-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="63cff-107">Vous pouvez utiliser l’authentification par cookie comme un fournisseur d’authentification autonome sans ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="63cff-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="63cff-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="63cff-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="63cff-109">Pour plus d’informations sur l’authentification basée sur le cookie de migration à partir de ASP.NET 1.x vers la version 2.0, consultez [migration de l’authentification et d’identité ASP.NET Core 2.0 rubrique (basée sur le Cookie d’authentification)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="63cff-109">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="63cff-110">Configuration</span><span class="sxs-lookup"><span data-stu-id="63cff-110">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63cff-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63cff-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="63cff-112">Si vous n’utilisez pas le [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), installez la version 2.0 + de le [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="63cff-112">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="63cff-113">Dans le `ConfigureServices` (méthode), créer le service de l’intergiciel (middleware) d’authentification avec le `AddAuthentication` et `AddCookie` méthodes :</span><span class="sxs-lookup"><span data-stu-id="63cff-113">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="63cff-114">`AuthenticationScheme`passé à `AddAuthentication` définit le schéma d’authentification par défaut pour l’application.</span><span class="sxs-lookup"><span data-stu-id="63cff-114">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="63cff-115">`AuthenticationScheme`est utile lorsqu’il existe plusieurs instances de l’authentification de cookie et vous souhaitez [autoriser avec un schéma spécifique](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="63cff-115">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="63cff-116">Définition de la `AuthenticationScheme` à `CookieAuthenticationDefaults.AuthenticationScheme` fournit une valeur de « Cookies » pour le schéma.</span><span class="sxs-lookup"><span data-stu-id="63cff-116">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="63cff-117">Vous pouvez fournir une valeur de chaîne qui distingue le schéma.</span><span class="sxs-lookup"><span data-stu-id="63cff-117">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="63cff-118">Dans le `Configure` méthode, utilisez la `UseAuthentication` méthode à appeler l’intergiciel (middleware) d’authentification qui définit la `HttpContext.User` propriété.</span><span class="sxs-lookup"><span data-stu-id="63cff-118">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="63cff-119">Appelez le `UseAuthentication` méthode avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="63cff-119">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="63cff-120">**Options de AddCookie**</span><span class="sxs-lookup"><span data-stu-id="63cff-120">**AddCookie Options**</span></span>

<span data-ttu-id="63cff-121">Le [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe est utilisée pour configurer les options de fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="63cff-121">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="63cff-122">Option</span><span class="sxs-lookup"><span data-stu-id="63cff-122">Option</span></span> | <span data-ttu-id="63cff-123">Description</span><span class="sxs-lookup"><span data-stu-id="63cff-123">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="63cff-124">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="63cff-124">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="63cff-125">Fournit le chemin d’accès à fournir une détection 302 (redirection d’URL) lorsque déclenchée par `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="63cff-125">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="63cff-126">La valeur par défaut est `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="63cff-126">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="63cff-127">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="63cff-127">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="63cff-128">L’émetteur à utiliser pour le [émetteur](/dotnet/api/system.security.claims.claim.issuer) propriété sur toutes les revendications créés par le service d’authentification de cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-128">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="63cff-129">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="63cff-129">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="63cff-130">Le nom de domaine dans lequel le cookie est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="63cff-130">The domain name where the cookie is served.</span></span> <span data-ttu-id="63cff-131">Par défaut, il est le nom d’hôte de la demande.</span><span class="sxs-lookup"><span data-stu-id="63cff-131">By default, this is the host name of the request.</span></span> <span data-ttu-id="63cff-132">Le navigateur envoie uniquement le cookie dans les demandes à un nom d’hôte correspondant.</span><span class="sxs-lookup"><span data-stu-id="63cff-132">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="63cff-133">Vous pouvez souhaiter ajuster ici pour que les cookies disponibles pour tous les hôtes de votre domaine.</span><span class="sxs-lookup"><span data-stu-id="63cff-133">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="63cff-134">Par exemple, si le domaine du cookie à `.contoso.com` met à disposition `contoso.com`, `www.contoso.com`, et `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="63cff-134">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="63cff-135">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="63cff-135">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="63cff-136">Obtient ou définit la durée de vie d’un cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-136">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="63cff-137">Actuellement, cette option non-ops et deviendra obsolète dans ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="63cff-137">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="63cff-138">Utilisez la `ExpireTimeSpan` option permettant de définir l’expiration du cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-138">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="63cff-139">Pour plus d’informations, consultez [préciser le comportement de CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="63cff-139">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="63cff-140">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="63cff-140">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="63cff-141">Indicateur qui indique si le cookie doit être accessible uniquement aux serveurs.</span><span class="sxs-lookup"><span data-stu-id="63cff-141">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="63cff-142">Modification de cette valeur à `false` autorise les scripts côté client pour accéder au cookie et peuvent s’ouvrir votre application au vol de cookie doit disposer de votre application un [inter-site (XSS)](xref:security/cross-site-scripting) vulnérabilité.</span><span class="sxs-lookup"><span data-stu-id="63cff-142">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="63cff-143">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="63cff-143">The default value is `true`.</span></span> |
| [<span data-ttu-id="63cff-144">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="63cff-144">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="63cff-145">Définit le nom du cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-145">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="63cff-146">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="63cff-146">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="63cff-147">Permet d’isoler les applications qui s’exécutent sur le même nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="63cff-147">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="63cff-148">Si vous avez une application en cours d’exécution à `/app1` et vous souhaitez restreindre les cookies pour cette application, définissez la `CookiePath` propriété `/app1`.</span><span class="sxs-lookup"><span data-stu-id="63cff-148">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="63cff-149">En procédant ainsi, le cookie est uniquement disponible sur les demandes à `/app1` et n’importe quelle application situés en dessous.</span><span class="sxs-lookup"><span data-stu-id="63cff-149">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="63cff-150">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="63cff-150">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="63cff-151">Indique si le navigateur doit autoriser le cookie à attacher aux demandes du même site uniquement (`SameSiteMode.Strict`) ou les demandes entre sites à l’aide des méthodes HTTP sécurisés et les demandes du même site (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="63cff-151">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="63cff-152">Lorsque la valeur `SameSiteMode.None`, la valeur d’en-tête cookie n’est pas définie.</span><span class="sxs-lookup"><span data-stu-id="63cff-152">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="63cff-153">Notez que [intergiciel (middleware) de Cookie stratégie](#cookie-policy-middleware) peut remplacer la valeur que vous fournissez.</span><span class="sxs-lookup"><span data-stu-id="63cff-153">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="63cff-154">Pour prendre en charge l’authentification OAuth, la valeur par défaut est `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="63cff-154">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="63cff-155">Pour plus d’informations, consultez [authentification OAuth interrompue en raison d’une stratégie de cookies SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="63cff-155">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="63cff-156">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="63cff-156">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="63cff-157">Un indicateur qui indique si le cookie créé doit être limité à HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou le même protocole que la demande (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="63cff-157">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="63cff-158">La valeur par défaut est `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="63cff-158">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="63cff-159">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="63cff-159">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="63cff-160">Définit le `DataProtectionProvider` qui est utilisé pour créer la valeur par défaut `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="63cff-160">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="63cff-161">Si le `TicketDataFormat` propriété est définie, la `DataProtectionProvider` option n’est pas utilisée.</span><span class="sxs-lookup"><span data-stu-id="63cff-161">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="63cff-162">Si n’est fourni, le fournisseur de protection des données de l’application par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="63cff-162">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="63cff-163">Événements</span><span class="sxs-lookup"><span data-stu-id="63cff-163">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="63cff-164">Le gestionnaire appelle des méthodes sur le fournisseur qui donnent le contrôle de l’application à certains points de traitement.</span><span class="sxs-lookup"><span data-stu-id="63cff-164">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="63cff-165">Si `Events` ne sont pas fournies, une instance par défaut est fournie qui ne fait rien lorsque les méthodes sont appelées.</span><span class="sxs-lookup"><span data-stu-id="63cff-165">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="63cff-166">EventsType</span><span class="sxs-lookup"><span data-stu-id="63cff-166">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="63cff-167">Utilisé en tant que le type de service pour obtenir le `Events` instance au lieu de la propriété.</span><span class="sxs-lookup"><span data-stu-id="63cff-167">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="63cff-168">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="63cff-168">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="63cff-169">Le `TimeSpan` après laquelle le ticket d’authentification stocké dans le cookie expire.</span><span class="sxs-lookup"><span data-stu-id="63cff-169">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="63cff-170">`ExpireTimeSpan`est ajouté à l’heure actuelle pour créer le délai d’expiration pour le ticket.</span><span class="sxs-lookup"><span data-stu-id="63cff-170">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="63cff-171">Le `ExpiredTimeSpan` la valeur est toujours en AuthTicket chiffrée vérifié par le serveur.</span><span class="sxs-lookup"><span data-stu-id="63cff-171">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="63cff-172">Il peut également aller dans le [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) en-tête, mais uniquement si `IsPersistent` est défini.</span><span class="sxs-lookup"><span data-stu-id="63cff-172">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="63cff-173">Pour définir `IsPersistent` à `true`, configurer le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passé à `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="63cff-173">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="63cff-174">La valeur par défaut de `ExpireTimeSpan` est de 14 jours.</span><span class="sxs-lookup"><span data-stu-id="63cff-174">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="63cff-175">LoginPath</span><span class="sxs-lookup"><span data-stu-id="63cff-175">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="63cff-176">Fournit le chemin d’accès à fournir une détection 302 (redirection d’URL) lorsque déclenchée par `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="63cff-176">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="63cff-177">L’URL actuelle qui a généré le code 401 est ajoutée à la `LoginPath` comme paramètre de chaîne de requête nommé par le `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="63cff-177">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="63cff-178">Une fois une demande pour le `LoginPath` accorde une nouvelle identité de connexion, le `ReturnUrlParameter` valeur est utilisée pour rediriger le navigateur vers l’URL qui a provoqué le code d’état non autorisé d’origine.</span><span class="sxs-lookup"><span data-stu-id="63cff-178">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="63cff-179">La valeur par défaut est `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="63cff-179">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="63cff-180">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="63cff-180">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="63cff-181">Si le `LogoutPath` est fournie pour le gestionnaire, ensuite une demande à ce chemin d’accès redirige en fonction de la valeur de la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="63cff-181">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="63cff-182">La valeur par défaut est `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="63cff-182">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="63cff-183">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="63cff-183">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="63cff-184">Détermine le nom du paramètre de chaîne de requête qui est ajouté par le Gestionnaire de réponse 302 trouvé (redirection d’URL).</span><span class="sxs-lookup"><span data-stu-id="63cff-184">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="63cff-185">`ReturnUrlParameter`est utilisé lorsqu’une demande arrive sur le `LoginPath` ou `LogoutPath` pour renvoyer le navigateur vers l’URL d’origine après l’exécution de l’action de connexion ou de déconnexion.</span><span class="sxs-lookup"><span data-stu-id="63cff-185">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="63cff-186">La valeur par défaut est `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="63cff-186">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="63cff-187">SessionStore</span><span class="sxs-lookup"><span data-stu-id="63cff-187">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="63cff-188">Conteneur facultatif utilisé pour stocker l’identité entre les demandes.</span><span class="sxs-lookup"><span data-stu-id="63cff-188">An optional container used to store identity across requests.</span></span> <span data-ttu-id="63cff-189">Lorsqu’il est utilisé, uniquement un identificateur de session est envoyé au client.</span><span class="sxs-lookup"><span data-stu-id="63cff-189">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="63cff-190">`SessionStore`peut être utilisé pour atténuer les problèmes potentiels avec les identités de grande taille.</span><span class="sxs-lookup"><span data-stu-id="63cff-190">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="63cff-191">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="63cff-191">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="63cff-192">Indicateur signalant si un nouveau cookie avec un délai d’expiration de mise à jour doit être émis de manière dynamique.</span><span class="sxs-lookup"><span data-stu-id="63cff-192">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="63cff-193">Cela peut se produire sur toute demande dont le délai d’expiration du cookie actuel est supérieur à 50 % expiré.</span><span class="sxs-lookup"><span data-stu-id="63cff-193">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="63cff-194">La nouvelle date d’expiration est déplacée vers la date actuelle plus la `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="63cff-194">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="63cff-195">Un [délai d’expiration de cookie absolu](xref:security/authentication/cookie#absolute-cookie-expiration) peut être définie à l’aide de la `AuthenticationProperties` lors de l’appel de la classe `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="63cff-195">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="63cff-196">Un délai d’expiration absolue peut améliorer la sécurité de votre application en limitant la durée pendant laquelle le cookie d’authentification est valide.</span><span class="sxs-lookup"><span data-stu-id="63cff-196">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="63cff-197">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="63cff-197">The default value is `true`.</span></span> |
| [<span data-ttu-id="63cff-198">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="63cff-198">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="63cff-199">Le `TicketDataFormat` est utilisé pour protéger et déprotéger l’identité et autres propriétés qui sont stockées dans la valeur du cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-199">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="63cff-200">Si n’est fourni, un `TicketDataFormat` est créé à l’aide de la [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="63cff-200">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="63cff-201">Validate</span><span class="sxs-lookup"><span data-stu-id="63cff-201">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="63cff-202">Méthode qui vérifie que les options sont valides.</span><span class="sxs-lookup"><span data-stu-id="63cff-202">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="63cff-203">Définissez `CookieAuthenticationOptions` dans la configuration du service pour l’authentification dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="63cff-203">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63cff-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63cff-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="63cff-205">ASP.NET Core 1.x utilise cookie [intergiciel (middleware)](xref:fundamentals/middleware) qui sérialise un principal d’utilisateur dans un cookie chiffré.</span><span class="sxs-lookup"><span data-stu-id="63cff-205">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="63cff-206">Pour les demandes suivantes, le cookie est validé, et le principal est recréé et affecté à la `HttpContext.User` propriété.</span><span class="sxs-lookup"><span data-stu-id="63cff-206">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="63cff-207">Installer le [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package NuGet dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="63cff-207">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="63cff-208">Ce package contient le middleware du cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-208">This package contains the cookie middleware.</span></span>

<span data-ttu-id="63cff-209">Utilisez le `UseCookieAuthentication` méthode dans le `Configure` méthode dans votre *Startup.cs* avant de fichiers `UseMvc` ou `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="63cff-209">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="63cff-210">**Options de CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="63cff-210">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="63cff-211">Le [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) classe est utilisée pour configurer les options de fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="63cff-211">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="63cff-212">Option</span><span class="sxs-lookup"><span data-stu-id="63cff-212">Option</span></span> | <span data-ttu-id="63cff-213">Description</span><span class="sxs-lookup"><span data-stu-id="63cff-213">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="63cff-214">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="63cff-214">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="63cff-215">Définit le schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="63cff-215">Sets the authentication scheme.</span></span> <span data-ttu-id="63cff-216">`AuthenticationScheme`est utile lorsqu’il existe plusieurs instances de l’authentification et vous souhaitez [autoriser avec un schéma spécifique](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="63cff-216">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="63cff-217">Définition de la `AuthenticationScheme` à `CookieAuthenticationDefaults.AuthenticationScheme` fournit une valeur de « Cookies » pour le schéma.</span><span class="sxs-lookup"><span data-stu-id="63cff-217">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="63cff-218">Vous pouvez fournir une valeur de chaîne qui distingue le schéma.</span><span class="sxs-lookup"><span data-stu-id="63cff-218">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="63cff-219">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="63cff-219">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="63cff-220">Définit une valeur pour indiquer que l’authentification de cookie doit s’exécuter sur chaque demande et tentez de valider et de reconstruire toute entité sérialisée de que sa création.</span><span class="sxs-lookup"><span data-stu-id="63cff-220">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="63cff-221">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="63cff-221">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="63cff-222">Si la valeur est true, l’intergiciel (middleware) d’authentification gère les défis automatique.</span><span class="sxs-lookup"><span data-stu-id="63cff-222">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="63cff-223">Si la valeur est false, l’intergiciel (middleware) d’authentification modifie uniquement les réponses lorsque explicitement indiqué par le `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="63cff-223">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="63cff-224">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="63cff-224">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="63cff-225">L’émetteur à utiliser pour le [émetteur](/dotnet/api/system.security.claims.claim.issuer) propriété sur toutes les revendications créés par l’intergiciel (middleware) d’authentification de cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-225">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="63cff-226">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="63cff-226">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="63cff-227">Le nom de domaine dans lequel le cookie est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="63cff-227">The domain name where the cookie is served.</span></span> <span data-ttu-id="63cff-228">Par défaut, il est le nom d’hôte de la demande.</span><span class="sxs-lookup"><span data-stu-id="63cff-228">By default, this is the host name of the request.</span></span> <span data-ttu-id="63cff-229">Le navigateur ne sert que le cookie à un nom d’hôte correspondant.</span><span class="sxs-lookup"><span data-stu-id="63cff-229">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="63cff-230">Vous pouvez souhaiter ajuster ici pour que les cookies disponibles pour tous les hôtes de votre domaine.</span><span class="sxs-lookup"><span data-stu-id="63cff-230">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="63cff-231">Par exemple, si le domaine du cookie à `.contoso.com` met à disposition `contoso.com`, `www.contoso.com`, et `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="63cff-231">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="63cff-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="63cff-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="63cff-233">Indicateur qui indique si le cookie doit être accessible uniquement aux serveurs.</span><span class="sxs-lookup"><span data-stu-id="63cff-233">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="63cff-234">Modification de cette valeur à `false` autorise les scripts côté client pour accéder au cookie et peuvent s’ouvrir votre application au vol de cookie doit disposer de votre application un [inter-site (XSS)](xref:security/cross-site-scripting) vulnérabilité.</span><span class="sxs-lookup"><span data-stu-id="63cff-234">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="63cff-235">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="63cff-235">The default value is `true`.</span></span> |
| [<span data-ttu-id="63cff-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="63cff-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="63cff-237">Permet d’isoler les applications qui s’exécutent sur le même nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="63cff-237">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="63cff-238">Si vous avez une application en cours d’exécution à `/app1` et vous souhaitez restreindre les cookies pour cette application, définissez la `CookiePath` propriété `/app1`.</span><span class="sxs-lookup"><span data-stu-id="63cff-238">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="63cff-239">En procédant ainsi, le cookie est uniquement disponible sur les demandes à `/app1` et n’importe quelle application situés en dessous.</span><span class="sxs-lookup"><span data-stu-id="63cff-239">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="63cff-240">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="63cff-240">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="63cff-241">Un indicateur qui indique si le cookie créé doit être limité à HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou le même protocole que la demande (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="63cff-241">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="63cff-242">La valeur par défaut est `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="63cff-242">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="63cff-243">Description</span><span class="sxs-lookup"><span data-stu-id="63cff-243">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="63cff-244">Informations supplémentaires sur le type d’authentification qui est rendue disponible à l’application.</span><span class="sxs-lookup"><span data-stu-id="63cff-244">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="63cff-245">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="63cff-245">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="63cff-246">Le `TimeSpan` après laquelle le ticket d’authentification expire.</span><span class="sxs-lookup"><span data-stu-id="63cff-246">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="63cff-247">Il est ajouté à l’heure actuelle pour créer le délai d’expiration pour le ticket.</span><span class="sxs-lookup"><span data-stu-id="63cff-247">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="63cff-248">Pour utiliser `ExpireTimeSpan`, vous devez définir `IsPersistent` à `true` dans les `AuthenticationProperties` passé à `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="63cff-248">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="63cff-249">La valeur par défaut est de 14 jours.</span><span class="sxs-lookup"><span data-stu-id="63cff-249">The default value is 14 days.</span></span> |
| [<span data-ttu-id="63cff-250">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="63cff-250">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="63cff-251">Indicateur spécifiant si la date d’expiration du cookie réinitialise lorsque plus de la moitié de la `ExpireTimeSpan` est écoulé.</span><span class="sxs-lookup"><span data-stu-id="63cff-251">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="63cff-252">La nouvelle heure exipiration est déplacée vers la date actuelle plus la `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="63cff-252">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="63cff-253">Un [délai d’expiration de cookie absolu](xref:security/authentication/cookie#absolute-cookie-expiration) peut être définie à l’aide de la `AuthenticationProperties` lors de l’appel de la classe `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="63cff-253">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="63cff-254">Un délai d’expiration absolue peut améliorer la sécurité de votre application en limitant la durée pendant laquelle le cookie d’authentification est valide.</span><span class="sxs-lookup"><span data-stu-id="63cff-254">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="63cff-255">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="63cff-255">The default value is `true`.</span></span> |

<span data-ttu-id="63cff-256">Définissez `CookieAuthenticationOptions` pour l’intergiciel (middleware) d’authentification de Cookie dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="63cff-256">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="63cff-257">Intergiciel (middleware) de cookie stratégie</span><span class="sxs-lookup"><span data-stu-id="63cff-257">Cookie Policy Middleware</span></span>

<span data-ttu-id="63cff-258">[Intergiciel (middleware) de cookie stratégie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) Active les fonctionnalités de stratégie de cookie dans une application.</span><span class="sxs-lookup"><span data-stu-id="63cff-258">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="63cff-259">Ajout de l’intergiciel (middleware) au pipeline de traitement d’application est l’ordre de la casse ; Il affecte uniquement les composants inscrits après lui dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="63cff-259">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="63cff-260">Le [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fourni à l’intergiciel de stratégie de Cookie vous permettent de contrôler des caractéristiques globales du traitement du cookie et le crochet en gestionnaires de traitement du cookie lorsque les cookies sont ajoutées ou supprimées.</span><span class="sxs-lookup"><span data-stu-id="63cff-260">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="63cff-261">Propriété</span><span class="sxs-lookup"><span data-stu-id="63cff-261">Property</span></span> | <span data-ttu-id="63cff-262">Description</span><span class="sxs-lookup"><span data-stu-id="63cff-262">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="63cff-263">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="63cff-263">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="63cff-264">Affecte si les cookies doivent être HttpOnly, qui est un indicateur indiquant si le cookie doit être accessible uniquement aux serveurs.</span><span class="sxs-lookup"><span data-stu-id="63cff-264">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="63cff-265">La valeur par défaut est `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="63cff-265">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="63cff-266">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="63cff-266">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="63cff-267">Affecte un attribut du même site du cookie (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="63cff-267">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="63cff-268">La valeur par défaut est `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="63cff-268">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="63cff-269">Cette option est disponible pour les principaux d’ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="63cff-269">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="63cff-270">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="63cff-270">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="63cff-271">Appelé lorsqu’un cookie est ajouté.</span><span class="sxs-lookup"><span data-stu-id="63cff-271">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="63cff-272">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="63cff-272">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="63cff-273">Appelé lorsqu’un cookie est supprimé.</span><span class="sxs-lookup"><span data-stu-id="63cff-273">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="63cff-274">Secure</span><span class="sxs-lookup"><span data-stu-id="63cff-274">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="63cff-275">Détermine si les cookies doivent être sécurisés.</span><span class="sxs-lookup"><span data-stu-id="63cff-275">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="63cff-276">La valeur par défaut est `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="63cff-276">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="63cff-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + uniquement)</span><span class="sxs-lookup"><span data-stu-id="63cff-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="63cff-278">La valeur par défaut `MinimumSameSitePolicy` valeur est `SameSiteMode.Lax` pour permettre l’authentification OAuth2.</span><span class="sxs-lookup"><span data-stu-id="63cff-278">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="63cff-279">Pour impose une stratégie du même site de `SameSiteMode.Strict`, définissez le `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="63cff-279">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="63cff-280">Bien que ce paramètre s’arrête OAuth2 et autres schémas d’authentification de cross-origine, elle élève le niveau de sécurité des cookies pour d’autres types d’applications qui ne reposent sur le traitement de la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="63cff-280">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="63cff-281">Le paramètre de stratégie Middleware du Cookie de `MinimumSameSitePolicy` peuvent affecter votre paramètre de `Cookie.SameSite` dans `CookieAuthenticationOptions` paramètres en fonction de la matrice ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="63cff-281">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="63cff-282">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="63cff-282">MinimumSameSitePolicy</span></span> | <span data-ttu-id="63cff-283">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="63cff-283">Cookie.SameSite</span></span> | <span data-ttu-id="63cff-284">Paramètre Cookie.SameSite résultante</span><span class="sxs-lookup"><span data-stu-id="63cff-284">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="63cff-285">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="63cff-285">SameSiteMode.None</span></span>     | <span data-ttu-id="63cff-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="63cff-286">SameSiteMode.None</span></span><br><span data-ttu-id="63cff-287">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="63cff-287">SameSiteMode.Lax</span></span><br><span data-ttu-id="63cff-288">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-288">SameSiteMode.Strict</span></span> | <span data-ttu-id="63cff-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="63cff-289">SameSiteMode.None</span></span><br><span data-ttu-id="63cff-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="63cff-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="63cff-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-291">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="63cff-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="63cff-292">SameSiteMode.Lax</span></span>      | <span data-ttu-id="63cff-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="63cff-293">SameSiteMode.None</span></span><br><span data-ttu-id="63cff-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="63cff-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="63cff-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="63cff-296">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="63cff-296">SameSiteMode.Lax</span></span><br><span data-ttu-id="63cff-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="63cff-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="63cff-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="63cff-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-299">SameSiteMode.Strict</span></span>   | <span data-ttu-id="63cff-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="63cff-300">SameSiteMode.None</span></span><br><span data-ttu-id="63cff-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="63cff-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="63cff-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="63cff-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-303">SameSiteMode.Strict</span></span><br><span data-ttu-id="63cff-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="63cff-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="63cff-305">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="63cff-306">Création d’un cookie d’authentification</span><span class="sxs-lookup"><span data-stu-id="63cff-306">Creating an authentication cookie</span></span>

<span data-ttu-id="63cff-307">Pour créer un cookie contenant les informations de l’utilisateur, vous devez construire une [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="63cff-307">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="63cff-308">Les informations de l’utilisateur sont sérialisées et stockées dans le cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-308">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63cff-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63cff-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="63cff-310">Appelez [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) connecter l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="63cff-310">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63cff-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63cff-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="63cff-312">Appelez [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) connecter l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="63cff-312">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="63cff-313">`SignInAsync`Crée un cookie chiffré et l’ajoute à la réponse actuelle.</span><span class="sxs-lookup"><span data-stu-id="63cff-313">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="63cff-314">Si vous ne spécifiez pas un `AuthenticationScheme`, le schéma par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="63cff-314">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="63cff-315">En arrière-plan, le chiffrement utilisé est de ASP.NET Core [Protection des données](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) système.</span><span class="sxs-lookup"><span data-stu-id="63cff-315">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="63cff-316">Si vous hébergez une application sur plusieurs ordinateurs, l’équilibrage de charge entre les applications ou à l’aide d’une batterie de serveurs web, vous devez [configurer la protection des données](xref:security/data-protection/configuration/overview) pour utiliser le même anneau de clé et l’identificateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="63cff-316">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="63cff-317">Déconnecter</span><span class="sxs-lookup"><span data-stu-id="63cff-317">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63cff-318">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63cff-318">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="63cff-319">Pour déconnecter l’utilisateur actuel et supprimer les cookies, appelez [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="63cff-319">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63cff-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63cff-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="63cff-321">Pour déconnecter l’utilisateur actuel et supprimer les cookies, appelez [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="63cff-321">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="63cff-322">Si vous n’utilisez pas `CookieAuthenticationDefaults.AuthenticationScheme` (ou « Cookies ») que le schéma (par exemple, « ContosoCookie »), indiquez le schéma que vous avez utilisé lors de la configuration du fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="63cff-322">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="63cff-323">Dans le cas contraire, le schéma par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="63cff-323">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="63cff-324">Réagir aux modifications du serveur principal</span><span class="sxs-lookup"><span data-stu-id="63cff-324">Reacting to back-end changes</span></span>

<span data-ttu-id="63cff-325">Une fois qu’un cookie est créé, il devient la seule source d’identité.</span><span class="sxs-lookup"><span data-stu-id="63cff-325">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="63cff-326">Même si vous désactivez un utilisateur dans vos systèmes principaux, le système d’authentification de cookie n’a aucune connaissance de ce, et un utilisateur reste connecté en tant que cookie est valide.</span><span class="sxs-lookup"><span data-stu-id="63cff-326">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="63cff-327">Le [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) événement dans ASP.NET Core 2.x ou [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) méthode dans ASP.NET Core 1.x peut être utilisé pour intercepter et de remplacer la validation de l’identité de cookie.</span><span class="sxs-lookup"><span data-stu-id="63cff-327">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="63cff-328">Cette approche réduit le risque d’utilisateurs révoqués l’accès à l’application.</span><span class="sxs-lookup"><span data-stu-id="63cff-328">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="63cff-329">Une méthode de validation de cookie est basée sur le suivi de lorsque la base de données utilisateur a été modifiée.</span><span class="sxs-lookup"><span data-stu-id="63cff-329">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="63cff-330">Si la base de données n’a pas été modifié depuis que le cookie d’utilisateur a été émis, il n’est pas nécessaire pour authentifier l’utilisateur si le cookie est toujours valide.</span><span class="sxs-lookup"><span data-stu-id="63cff-330">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="63cff-331">Pour implémenter ce scénario, la base de données, qui est implémenté dans `IUserRepository` pour cet exemple, stocke un `LastChanged` valeur.</span><span class="sxs-lookup"><span data-stu-id="63cff-331">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="63cff-332">Lorsqu’un utilisateur est mise à jour dans la base de données, le `LastChanged` a la valeur à l’heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="63cff-332">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="63cff-333">Pour invalider un cookie lorsque les modifications de la base de données basée sur le `LastChanged` valeur, créer le cookie avec une `LastChanged` contenant actuel de la revendication `LastChanged` valeur à partir de la base de données :</span><span class="sxs-lookup"><span data-stu-id="63cff-333">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63cff-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63cff-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="63cff-335">Pour implémenter un remplacement pour le `ValidatePrincipal` événement, écriture, une méthode avec la signature suivante dans une classe que vous dérivez de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="63cff-335">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="63cff-336">Un exemple ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="63cff-336">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="63cff-337">Enregistrer l’instance d’événements lors de l’inscription du service de cookie dans le `ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="63cff-337">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="63cff-338">Fournir l’inscription d’une service avec étendue pour votre `CustomCookieAuthenticationEvents` classe :</span><span class="sxs-lookup"><span data-stu-id="63cff-338">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63cff-339">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63cff-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="63cff-340">Pour implémenter un remplacement pour le `ValidateAsync` événement, écriture, une méthode avec la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="63cff-340">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="63cff-341">ASP.NET Core Identity implémente cette vérification dans le cadre de son [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="63cff-341">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="63cff-342">Un exemple ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="63cff-342">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="63cff-343">Inscrire l’événement lors de la configuration de l’authentification de cookie dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="63cff-343">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="63cff-344">Prenons le cas dans lequel le nom d’utilisateur est mise à jour &mdash; une décision qui n’affecte pas la sécurité de toute façon.</span><span class="sxs-lookup"><span data-stu-id="63cff-344">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="63cff-345">Si vous souhaitez mettre à jour de façon non destructrice le principal de l’utilisateur, appelez `context.ReplacePrincipal` et définir le `context.ShouldRenew` propriété `true`.</span><span class="sxs-lookup"><span data-stu-id="63cff-345">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="63cff-346">L’approche décrite ici est déclenché à chaque demande.</span><span class="sxs-lookup"><span data-stu-id="63cff-346">The approach described here is triggered on every request.</span></span> <span data-ttu-id="63cff-347">Cela peut entraîner une baisse des performances de l’application.</span><span class="sxs-lookup"><span data-stu-id="63cff-347">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="63cff-348">Cookies persistants</span><span class="sxs-lookup"><span data-stu-id="63cff-348">Persistent cookies</span></span>

<span data-ttu-id="63cff-349">Vous souhaiterez peut-être le cookie pour conserver dans les sessions de navigateur.</span><span class="sxs-lookup"><span data-stu-id="63cff-349">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="63cff-350">Cette persistance doit être activée avec le consentement explicite de l’utilisateur avec une case à cocher « Mémoriser mes informations » sur la connexion ou un mécanisme similaire.</span><span class="sxs-lookup"><span data-stu-id="63cff-350">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="63cff-351">L’extrait de code suivant crée une identité et le cookie correspondant qui persiste via la fermeture du navigateur.</span><span class="sxs-lookup"><span data-stu-id="63cff-351">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="63cff-352">Tous les paramètres d’expiration décalée précédemment configurées sont respectées.</span><span class="sxs-lookup"><span data-stu-id="63cff-352">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="63cff-353">Si le cookie expire pendant que le navigateur est fermé, le navigateur efface le cookie s’est arrêté.</span><span class="sxs-lookup"><span data-stu-id="63cff-353">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63cff-354">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63cff-354">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="63cff-355">Le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) classe se trouve dans le `Microsoft.AspNetCore.Authentication` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="63cff-355">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63cff-356">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63cff-356">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="63cff-357">Le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) classe se trouve dans le `Microsoft.AspNetCore.Http.Authentication` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="63cff-357">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="63cff-358">Expiration du cookie absolu</span><span class="sxs-lookup"><span data-stu-id="63cff-358">Absolute cookie expiration</span></span>

<span data-ttu-id="63cff-359">Vous pouvez définir une heure d’expiration absolue avec `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="63cff-359">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="63cff-360">Vous devez également définir `IsPersistent`; sinon, `ExpiresUtc` est ignorée et un cookie de session unique est créé.</span><span class="sxs-lookup"><span data-stu-id="63cff-360">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="63cff-361">Lorsque `ExpiresUtc` est définie sur `SignInAsync`, il remplace la valeur de la `ExpireTimeSpan` option de `CookieAuthenticationOptions`, si définie.</span><span class="sxs-lookup"><span data-stu-id="63cff-361">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="63cff-362">L’extrait de code suivant crée une identité et le cookie correspondant qui dure 20 minutes.</span><span class="sxs-lookup"><span data-stu-id="63cff-362">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="63cff-363">Il ignore tous les paramètres d’expiration décalée précédemment configurés.</span><span class="sxs-lookup"><span data-stu-id="63cff-363">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63cff-364">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63cff-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63cff-365">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63cff-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a><span data-ttu-id="63cff-366">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="63cff-366">See also</span></span>

* [<span data-ttu-id="63cff-367">Les modifications d’authentification 2.0 / annonce de Migration</span><span class="sxs-lookup"><span data-stu-id="63cff-367">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="63cff-368">Limitation d’identité par schéma</span><span class="sxs-lookup"><span data-stu-id="63cff-368">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
