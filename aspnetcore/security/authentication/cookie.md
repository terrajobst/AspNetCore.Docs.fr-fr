---
title: Utiliser l’authentification par cookie sans ASP.NET Core Identity
author: rick-anderson
description: Une explication de l’authentification par cookie sans ASP.NET Core Identity
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: f05e5b83359ec1739115293e092eaed0c811c046
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854378"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="62b44-103">Utiliser l’authentification par cookie sans ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="62b44-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="62b44-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="62b44-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="62b44-105">Comme vous l’avez vu dans les rubriques précédentes de l’authentification, [ASP.NET Core Identity](xref:security/authentication/identity) est un fournisseur d’authentification complète et complet pour créer et maintenir des connexions.</span><span class="sxs-lookup"><span data-stu-id="62b44-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="62b44-106">Toutefois, vous souhaiterez utiliser votre propre logique d’authentification personnalisée avec l’authentification basée sur les cookies dans certains cas.</span><span class="sxs-lookup"><span data-stu-id="62b44-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="62b44-107">Vous pouvez utiliser l’authentification par cookie comme un fournisseur d’authentification autonome sans ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="62b44-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="62b44-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="62b44-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="62b44-109">Fins de démonstration dans l’exemple d’application, le compte d’utilisateur pour l’utilisateur hypothétique, Maria Rodriguez, est codé en dur dans l’application.</span><span class="sxs-lookup"><span data-stu-id="62b44-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="62b44-110">Utilisez le nom d’utilisateur de messagerie «maria.rodriguez@contoso.com» et n’importe quel mot de passe pour vous connecter l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="62b44-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="62b44-111">L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="62b44-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="62b44-112">Dans un exemple réel, l’utilisateur serait être authentifié par rapport à une base de données.</span><span class="sxs-lookup"><span data-stu-id="62b44-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="62b44-113">Pour plus d’informations sur l’authentification basée sur les cookies de migration d’ASP.NET Core 1.x vers 2.0, consultez [migrer l’authentification et l’identité pour ASP.NET Core 2.0 rubrique (authentification basée sur les cookies)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="62b44-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="62b44-114">Pour utiliser ASP.NET Core Identity, consultez le [présentation d’Identity](xref:security/authentication/identity) rubrique.</span><span class="sxs-lookup"><span data-stu-id="62b44-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="62b44-115">Configuration</span><span class="sxs-lookup"><span data-stu-id="62b44-115">Configuration</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="62b44-116">Si l’application n’utilise pas le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app), créez une référence de package dans le fichier de projet pour le [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 ou plus tard).</span><span class="sxs-lookup"><span data-stu-id="62b44-116">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="62b44-117">Dans le `ConfigureServices` (méthode), créez le service de l’intergiciel d’authentification avec le `AddAuthentication` et `AddCookie` méthodes :</span><span class="sxs-lookup"><span data-stu-id="62b44-117">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="62b44-118">`AuthenticationScheme` passé à `AddAuthentication` définit le schéma d’authentification par défaut pour l’application.</span><span class="sxs-lookup"><span data-stu-id="62b44-118">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="62b44-119">`AuthenticationScheme` est utile quand il existe plusieurs instances de l’authentification des cookies et que vous souhaitez [autoriser avec un schéma spécifique](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="62b44-119">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="62b44-120">Définition de la `AuthenticationScheme` à `CookieAuthenticationDefaults.AuthenticationScheme` fournit une valeur de « Cookies » pour le schéma.</span><span class="sxs-lookup"><span data-stu-id="62b44-120">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="62b44-121">Vous pouvez fournir n’importe quelle valeur de chaîne qui distingue le schéma.</span><span class="sxs-lookup"><span data-stu-id="62b44-121">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="62b44-122">Schéma d’authentification de l’application est différent de schéma d’authentification de cookie de l’application.</span><span class="sxs-lookup"><span data-stu-id="62b44-122">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="62b44-123">Lorsqu’un schéma d’authentification de cookie n’est pas fourni pour <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, elle utilise [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (« Cookies »).</span><span class="sxs-lookup"><span data-stu-id="62b44-123">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="62b44-124">Dans le `Configure` (méthode), utilisez le `UseAuthentication` méthode à appeler l’intergiciel d’authentification qui définit le `HttpContext.User` propriété.</span><span class="sxs-lookup"><span data-stu-id="62b44-124">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="62b44-125">Appelez le `UseAuthentication` méthode avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="62b44-125">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="62b44-126">**AddCookie Options**</span><span class="sxs-lookup"><span data-stu-id="62b44-126">**AddCookie Options**</span></span>

<span data-ttu-id="62b44-127">Le [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe est utilisée pour configurer les options de fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="62b44-127">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="62b44-128">Option</span><span class="sxs-lookup"><span data-stu-id="62b44-128">Option</span></span> | <span data-ttu-id="62b44-129">Description</span><span class="sxs-lookup"><span data-stu-id="62b44-129">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="62b44-130">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="62b44-130">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="62b44-131">Fournit le chemin d’accès pour fournir un 302 trouvé (redirection d’URL) lorsque déclenchée par `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="62b44-131">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="62b44-132">La valeur par défaut est `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="62b44-132">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="62b44-133">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="62b44-133">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="62b44-134">L’émetteur à utiliser pour le [émetteur](/dotnet/api/system.security.claims.claim.issuer) propriété sur toutes les revendications créés par le service d’authentification de cookie.</span><span class="sxs-lookup"><span data-stu-id="62b44-134">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="62b44-135">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="62b44-135">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="62b44-136">Le nom de domaine dans lequel le cookie est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="62b44-136">The domain name where the cookie is served.</span></span> <span data-ttu-id="62b44-137">Par défaut, cela est le nom d’hôte de la demande.</span><span class="sxs-lookup"><span data-stu-id="62b44-137">By default, this is the host name of the request.</span></span> <span data-ttu-id="62b44-138">Le navigateur envoie uniquement le cookie dans les demandes à un nom d’hôte correspondant.</span><span class="sxs-lookup"><span data-stu-id="62b44-138">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="62b44-139">Vous pouvez souhaiter réglez cette option pour que les cookies disponibles pour n’importe quel hôte dans votre domaine.</span><span class="sxs-lookup"><span data-stu-id="62b44-139">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="62b44-140">Par exemple, si le domaine du cookie à `.contoso.com` met à disposition `contoso.com`, `www.contoso.com`, et `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="62b44-140">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="62b44-141">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="62b44-141">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="62b44-142">Un indicateur qui spécifie si le cookie doit être accessible uniquement aux serveurs.</span><span class="sxs-lookup"><span data-stu-id="62b44-142">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="62b44-143">Modification de cette valeur à `false` autorise les scripts côté client pour accéder au cookie et peut s’ouvrir à votre application au vol de cookies doit disposer de votre application un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnérabilité.</span><span class="sxs-lookup"><span data-stu-id="62b44-143">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="62b44-144">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="62b44-144">The default value is `true`.</span></span> |
| [<span data-ttu-id="62b44-145">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="62b44-145">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="62b44-146">Définit le nom du cookie.</span><span class="sxs-lookup"><span data-stu-id="62b44-146">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="62b44-147">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="62b44-147">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="62b44-148">Utilisé pour isoler les applications en cours d’exécution sur le même nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="62b44-148">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="62b44-149">Si vous avez une application en cours d’exécution à `/app1` et souhaitez limitent les cookies à cette application, définissez la `CookiePath` propriété `/app1`.</span><span class="sxs-lookup"><span data-stu-id="62b44-149">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="62b44-150">En procédant ainsi, le cookie est uniquement disponible sur les demandes à `/app1` et n’importe quelle application situé en dessous.</span><span class="sxs-lookup"><span data-stu-id="62b44-150">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="62b44-151">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="62b44-151">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="62b44-152">Indique si le navigateur doit autoriser le cookie à attacher aux demandes du même site uniquement (`SameSiteMode.Strict`) ou de requêtes inter-site à l’aide des méthodes HTTP sécurisés et requêtes same-site (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="62b44-152">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="62b44-153">Lorsque la valeur `SameSiteMode.None`, la valeur d’en-tête de cookie n’est pas définie.</span><span class="sxs-lookup"><span data-stu-id="62b44-153">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="62b44-154">Notez que [intergiciel (middleware) de Cookie stratégie](#cookie-policy-middleware) peut remplacer la valeur que vous fournissez.</span><span class="sxs-lookup"><span data-stu-id="62b44-154">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="62b44-155">Pour prendre en charge l’authentification OAuth, la valeur par défaut est `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="62b44-155">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="62b44-156">Pour plus d’informations, consultez [l’authentification OAuth divisée en raison de la stratégie de cookies SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="62b44-156">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="62b44-157">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="62b44-157">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="62b44-158">Un indicateur qui spécifie si le cookie créé doit être limité à HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou le même protocole que la demande (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="62b44-158">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="62b44-159">La valeur par défaut est `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="62b44-159">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="62b44-160">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="62b44-160">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="62b44-161">Définit le `DataProtectionProvider` qui est utilisé pour créer la valeur par défaut `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="62b44-161">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="62b44-162">Si le `TicketDataFormat` propriété est définie, la `DataProtectionProvider` option n’est pas utilisée.</span><span class="sxs-lookup"><span data-stu-id="62b44-162">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="62b44-163">Si ne pas fourni, le fournisseur de protection de données de l’application par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="62b44-163">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="62b44-164">Événements</span><span class="sxs-lookup"><span data-stu-id="62b44-164">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="62b44-165">Le gestionnaire appelle des méthodes sur le fournisseur qui donnent le contrôle de l’application à certains points de traitement.</span><span class="sxs-lookup"><span data-stu-id="62b44-165">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="62b44-166">Si `Events` ne sont pas fournies, une instance par défaut est fournie qui ne fait rien lorsque les méthodes sont appelées.</span><span class="sxs-lookup"><span data-stu-id="62b44-166">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="62b44-167">EventsType</span><span class="sxs-lookup"><span data-stu-id="62b44-167">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="62b44-168">Utilisé en tant que le type de service pour obtenir le `Events` instance au lieu de la propriété.</span><span class="sxs-lookup"><span data-stu-id="62b44-168">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="62b44-169">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="62b44-169">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="62b44-170">Le `TimeSpan` issue de laquelle le ticket d’authentification stocké dans le cookie expire.</span><span class="sxs-lookup"><span data-stu-id="62b44-170">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="62b44-171">`ExpireTimeSpan` est ajouté à l’heure actuelle pour créer le délai d’expiration du ticket.</span><span class="sxs-lookup"><span data-stu-id="62b44-171">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="62b44-172">Le `ExpiredTimeSpan` valeur prendra toujours chiffrée AuthTicket vérifié par le serveur.</span><span class="sxs-lookup"><span data-stu-id="62b44-172">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="62b44-173">Il peut également aller dans le [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) en-tête, mais uniquement si `IsPersistent` est défini.</span><span class="sxs-lookup"><span data-stu-id="62b44-173">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="62b44-174">Pour définir `IsPersistent` à `true`, configurer le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passé à `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="62b44-174">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="62b44-175">La valeur par défaut de `ExpireTimeSpan` est de 14 jours.</span><span class="sxs-lookup"><span data-stu-id="62b44-175">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="62b44-176">LoginPath</span><span class="sxs-lookup"><span data-stu-id="62b44-176">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="62b44-177">Fournit le chemin d’accès pour fournir un 302 trouvé (redirection d’URL) lorsque déclenchée par `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="62b44-177">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="62b44-178">L’URL actuelle qui a généré l’erreur 401 est ajoutée à la `LoginPath` comme paramètre de chaîne de requête nommé par le `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="62b44-178">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="62b44-179">Une fois une demande pour le `LoginPath` accorde une nouvelle identité de connexion, le `ReturnUrlParameter` valeur est utilisée pour rediriger le navigateur vers l’URL qui a provoqué le code d’état non autorisé d’origine.</span><span class="sxs-lookup"><span data-stu-id="62b44-179">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="62b44-180">La valeur par défaut est `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="62b44-180">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="62b44-181">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="62b44-181">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="62b44-182">Si le `LogoutPath` est fourni pour le gestionnaire, puis redirige une demande à ce chemin d’accès basé sur la valeur de la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="62b44-182">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="62b44-183">La valeur par défaut est `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="62b44-183">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="62b44-184">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="62b44-184">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="62b44-185">Détermine le nom du paramètre de chaîne de requête qui est ajouté par le gestionnaire pour une réponse 302 trouvé (redirection d’URL).</span><span class="sxs-lookup"><span data-stu-id="62b44-185">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="62b44-186">`ReturnUrlParameter` est utilisé lorsqu’une requête arrive sur le `LoginPath` ou `LogoutPath` pour retourner le navigateur vers l’URL d’origine après l’exécution de l’action de connexion ou déconnexion.</span><span class="sxs-lookup"><span data-stu-id="62b44-186">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="62b44-187">La valeur par défaut est `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="62b44-187">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="62b44-188">SessionStore</span><span class="sxs-lookup"><span data-stu-id="62b44-188">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="62b44-189">Un conteneur facultatif utilisé pour stocker l’identité entre les requêtes.</span><span class="sxs-lookup"><span data-stu-id="62b44-189">An optional container used to store identity across requests.</span></span> <span data-ttu-id="62b44-190">Lorsqu’il est utilisé, uniquement un identificateur de session est envoyé au client.</span><span class="sxs-lookup"><span data-stu-id="62b44-190">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="62b44-191">`SessionStore` peut être utilisé pour atténuer les problèmes potentiels liés aux identités de grande taille.</span><span class="sxs-lookup"><span data-stu-id="62b44-191">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="62b44-192">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="62b44-192">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="62b44-193">Un indicateur qui spécifie si un nouveau cookie avec un délai d’expiration de mise à jour doit être émis de manière dynamique.</span><span class="sxs-lookup"><span data-stu-id="62b44-193">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="62b44-194">Cela peut se produire sur toute requête dont la période d’expiration du cookie actuelle est supérieure à 50 % expiré.</span><span class="sxs-lookup"><span data-stu-id="62b44-194">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="62b44-195">La nouvelle date d’expiration est déplacée vers l’avant qu’ils doivent être la date actuelle plus la `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="62b44-195">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="62b44-196">Un [délai d’expiration de cookie absolu](xref:security/authentication/cookie#absolute-cookie-expiration) peut être définie à l’aide de la `AuthenticationProperties` lors de l’appel de la classe `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="62b44-196">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="62b44-197">Un délai d’expiration absolue peut améliorer la sécurité de votre application en limitant la durée pendant laquelle le cookie d’authentification est valide.</span><span class="sxs-lookup"><span data-stu-id="62b44-197">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="62b44-198">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="62b44-198">The default value is `true`.</span></span> |
| [<span data-ttu-id="62b44-199">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="62b44-199">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="62b44-200">Le `TicketDataFormat` est utilisé pour protéger et déprotéger l’identité et autres propriétés qui sont stockées dans la valeur du cookie.</span><span class="sxs-lookup"><span data-stu-id="62b44-200">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="62b44-201">Si n’est fourni, un `TicketDataFormat` est créé à l’aide de la [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="62b44-201">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="62b44-202">Validate</span><span class="sxs-lookup"><span data-stu-id="62b44-202">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="62b44-203">Méthode qui vérifie que les options sont valides.</span><span class="sxs-lookup"><span data-stu-id="62b44-203">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="62b44-204">Définissez `CookieAuthenticationOptions` dans la configuration du service pour l’authentification dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="62b44-204">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="62b44-205">ASP.NET Core 1.x utilise cookie [intergiciel (middleware)](xref:fundamentals/middleware/index) qui sérialise un principal d’utilisateur dans un cookie chiffré.</span><span class="sxs-lookup"><span data-stu-id="62b44-205">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="62b44-206">Pour les requêtes ultérieures, le cookie est validé, et le principal est recréé et affecté à la `HttpContext.User` propriété.</span><span class="sxs-lookup"><span data-stu-id="62b44-206">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="62b44-207">Installer le [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package NuGet dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="62b44-207">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="62b44-208">Ce package contient le middleware de cookie.</span><span class="sxs-lookup"><span data-stu-id="62b44-208">This package contains the cookie middleware.</span></span>

<span data-ttu-id="62b44-209">Utilisez le `UseCookieAuthentication` méthode dans le `Configure` méthode dans votre *Startup.cs* fichier avant `UseMvc` ou `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="62b44-209">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="62b44-210">**Options de CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="62b44-210">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="62b44-211">Le [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) classe est utilisée pour configurer les options de fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="62b44-211">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="62b44-212">Option</span><span class="sxs-lookup"><span data-stu-id="62b44-212">Option</span></span> | <span data-ttu-id="62b44-213">Description</span><span class="sxs-lookup"><span data-stu-id="62b44-213">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="62b44-214">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="62b44-214">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="62b44-215">Définit le schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="62b44-215">Sets the authentication scheme.</span></span> <span data-ttu-id="62b44-216">`AuthenticationScheme` est utile quand il existe plusieurs instances de l’authentification et que vous souhaitez [autoriser avec un schéma spécifique](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="62b44-216">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="62b44-217">Définition de la `AuthenticationScheme` à `CookieAuthenticationDefaults.AuthenticationScheme` fournit une valeur de « Cookies » pour le schéma.</span><span class="sxs-lookup"><span data-stu-id="62b44-217">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="62b44-218">Vous pouvez fournir n’importe quelle valeur de chaîne qui distingue le schéma.</span><span class="sxs-lookup"><span data-stu-id="62b44-218">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="62b44-219">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="62b44-219">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="62b44-220">Définit une valeur pour indiquer que l’authentification de cookie doit s’exécuter sur chaque demande et tentez de valider et de reconstruire n’importe quel principal sérialisé qu'il créé.</span><span class="sxs-lookup"><span data-stu-id="62b44-220">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="62b44-221">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="62b44-221">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="62b44-222">Si la valeur est true, le middleware d’authentification gère les défis automatique.</span><span class="sxs-lookup"><span data-stu-id="62b44-222">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="62b44-223">Si false, le middleware d’authentification modifie uniquement les réponses lorsque indiqué explicitement par le `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="62b44-223">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="62b44-224">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="62b44-224">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="62b44-225">L’émetteur à utiliser pour le [émetteur](/dotnet/api/system.security.claims.claim.issuer) propriété sur toutes les revendications créés par l’intergiciel d’authentification de cookie.</span><span class="sxs-lookup"><span data-stu-id="62b44-225">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="62b44-226">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="62b44-226">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="62b44-227">Le nom de domaine dans lequel le cookie est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="62b44-227">The domain name where the cookie is served.</span></span> <span data-ttu-id="62b44-228">Par défaut, cela est le nom d’hôte de la demande.</span><span class="sxs-lookup"><span data-stu-id="62b44-228">By default, this is the host name of the request.</span></span> <span data-ttu-id="62b44-229">Le navigateur sert uniquement le cookie à un nom d’hôte correspondant.</span><span class="sxs-lookup"><span data-stu-id="62b44-229">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="62b44-230">Vous pouvez souhaiter réglez cette option pour que les cookies disponibles pour n’importe quel hôte dans votre domaine.</span><span class="sxs-lookup"><span data-stu-id="62b44-230">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="62b44-231">Par exemple, si le domaine du cookie à `.contoso.com` met à disposition `contoso.com`, `www.contoso.com`, et `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="62b44-231">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="62b44-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="62b44-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="62b44-233">Un indicateur qui spécifie si le cookie doit être accessible uniquement aux serveurs.</span><span class="sxs-lookup"><span data-stu-id="62b44-233">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="62b44-234">Modification de cette valeur à `false` autorise les scripts côté client pour accéder au cookie et peut s’ouvrir à votre application au vol de cookies doit disposer de votre application un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnérabilité.</span><span class="sxs-lookup"><span data-stu-id="62b44-234">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="62b44-235">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="62b44-235">The default value is `true`.</span></span> |
| [<span data-ttu-id="62b44-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="62b44-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="62b44-237">Utilisé pour isoler les applications en cours d’exécution sur le même nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="62b44-237">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="62b44-238">Si vous avez une application en cours d’exécution à `/app1` et souhaitez limitent les cookies à cette application, définissez la `CookiePath` propriété `/app1`.</span><span class="sxs-lookup"><span data-stu-id="62b44-238">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="62b44-239">En procédant ainsi, le cookie est uniquement disponible sur les demandes à `/app1` et n’importe quelle application situé en dessous.</span><span class="sxs-lookup"><span data-stu-id="62b44-239">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="62b44-240">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="62b44-240">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="62b44-241">Un indicateur qui spécifie si le cookie créé doit être limité à HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou le même protocole que la demande (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="62b44-241">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="62b44-242">La valeur par défaut est `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="62b44-242">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="62b44-243">Description</span><span class="sxs-lookup"><span data-stu-id="62b44-243">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="62b44-244">Informations supplémentaires sur le type d’authentification qui est à votre disposition à l’application.</span><span class="sxs-lookup"><span data-stu-id="62b44-244">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="62b44-245">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="62b44-245">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="62b44-246">Le `TimeSpan` issue de laquelle le ticket d’authentification expire.</span><span class="sxs-lookup"><span data-stu-id="62b44-246">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="62b44-247">Il est ajouté à l’heure actuelle pour créer le délai d’expiration du ticket.</span><span class="sxs-lookup"><span data-stu-id="62b44-247">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="62b44-248">Pour utiliser `ExpireTimeSpan`, vous devez définir `IsPersistent` à `true` dans le `AuthenticationProperties` passé à `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="62b44-248">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="62b44-249">La valeur par défaut est de 14 jours.</span><span class="sxs-lookup"><span data-stu-id="62b44-249">The default value is 14 days.</span></span> |
| [<span data-ttu-id="62b44-250">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="62b44-250">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="62b44-251">Un indicateur qui spécifie si la date d’expiration du cookie réinitialise lorsque plusieurs la moitié de la `ExpireTimeSpan` est écoulé.</span><span class="sxs-lookup"><span data-stu-id="62b44-251">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="62b44-252">La nouvelle heure exipiration est déplacée vers la date actuelle plus la `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="62b44-252">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="62b44-253">Un [délai d’expiration de cookie absolu](xref:security/authentication/cookie#absolute-cookie-expiration) peut être définie à l’aide de la `AuthenticationProperties` lors de l’appel de la classe `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="62b44-253">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="62b44-254">Un délai d’expiration absolue peut améliorer la sécurité de votre application en limitant la durée pendant laquelle le cookie d’authentification est valide.</span><span class="sxs-lookup"><span data-stu-id="62b44-254">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="62b44-255">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="62b44-255">The default value is `true`.</span></span> |

<span data-ttu-id="62b44-256">Définissez `CookieAuthenticationOptions` du Middleware de Cookie d’authentification dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="62b44-256">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a><span data-ttu-id="62b44-257">Intergiciel (middleware) de cookie stratégie</span><span class="sxs-lookup"><span data-stu-id="62b44-257">Cookie Policy Middleware</span></span>

<span data-ttu-id="62b44-258">[Intergiciel (middleware) de cookie stratégie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) offre des fonctionnalités de stratégie de cookie dans une application.</span><span class="sxs-lookup"><span data-stu-id="62b44-258">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="62b44-259">Ajout de l’intergiciel (middleware) au pipeline de traitement d’application est l’ordre de la casse ; Il affecte uniquement les composants enregistrés après lui dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="62b44-259">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="62b44-260">Le [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fourni au Middleware Cookie stratégie vous permettent de contrôler les caractéristiques globales du traitement du cookie et le crochet dans les gestionnaires de traitement du cookie lorsque les cookies sont ajoutées ou supprimées.</span><span class="sxs-lookup"><span data-stu-id="62b44-260">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="62b44-261">Propriété</span><span class="sxs-lookup"><span data-stu-id="62b44-261">Property</span></span> | <span data-ttu-id="62b44-262">Description</span><span class="sxs-lookup"><span data-stu-id="62b44-262">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="62b44-263">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="62b44-263">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="62b44-264">Affecte si les cookies doivent être HttpOnly, qui est un indicateur indiquant si le cookie doit être accessible uniquement aux serveurs.</span><span class="sxs-lookup"><span data-stu-id="62b44-264">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="62b44-265">La valeur par défaut est `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="62b44-265">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="62b44-266">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="62b44-266">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="62b44-267">Affecte l’attribut du même site du cookie (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="62b44-267">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="62b44-268">La valeur par défaut est `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="62b44-268">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="62b44-269">Cette option est disponible pour ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="62b44-269">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="62b44-270">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="62b44-270">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="62b44-271">Appelé lorsqu’un cookie est ajouté.</span><span class="sxs-lookup"><span data-stu-id="62b44-271">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="62b44-272">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="62b44-272">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="62b44-273">Appelée lorsqu’un cookie est supprimé.</span><span class="sxs-lookup"><span data-stu-id="62b44-273">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="62b44-274">Sécuriser</span><span class="sxs-lookup"><span data-stu-id="62b44-274">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="62b44-275">Détermine si les cookies doivent être sécurisé.</span><span class="sxs-lookup"><span data-stu-id="62b44-275">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="62b44-276">La valeur par défaut est `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="62b44-276">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="62b44-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + uniquement)</span><span class="sxs-lookup"><span data-stu-id="62b44-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="62b44-278">La valeur par défaut `MinimumSameSitePolicy` valeur est `SameSiteMode.Lax` pour autoriser l’authentification OAuth2.</span><span class="sxs-lookup"><span data-stu-id="62b44-278">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="62b44-279">À strictement appliquer une stratégie de même site de `SameSiteMode.Strict`, définissez le `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="62b44-279">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="62b44-280">Bien que ce paramètre s’arrête OAuth2 et autres schémas d’authentification de cross-origin, elle élève le niveau de sécurité du cookie pour les autres types d’applications qui ne reposent pas sur le traitement de la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="62b44-280">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="62b44-281">Le paramètre d’intergiciel (middleware) stratégie de Cookie pour `MinimumSameSitePolicy` peuvent affecter votre valeur de `Cookie.SameSite` dans `CookieAuthenticationOptions` paramètres en fonction de la matrice ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="62b44-281">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="62b44-282">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="62b44-282">MinimumSameSitePolicy</span></span> | <span data-ttu-id="62b44-283">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="62b44-283">Cookie.SameSite</span></span> | <span data-ttu-id="62b44-284">Paramètre Cookie.SameSite résultante</span><span class="sxs-lookup"><span data-stu-id="62b44-284">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="62b44-285">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="62b44-285">SameSiteMode.None</span></span>     | <span data-ttu-id="62b44-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="62b44-286">SameSiteMode.None</span></span><br><span data-ttu-id="62b44-287">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="62b44-287">SameSiteMode.Lax</span></span><br><span data-ttu-id="62b44-288">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-288">SameSiteMode.Strict</span></span> | <span data-ttu-id="62b44-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="62b44-289">SameSiteMode.None</span></span><br><span data-ttu-id="62b44-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="62b44-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="62b44-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-291">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="62b44-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="62b44-292">SameSiteMode.Lax</span></span>      | <span data-ttu-id="62b44-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="62b44-293">SameSiteMode.None</span></span><br><span data-ttu-id="62b44-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="62b44-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="62b44-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="62b44-296">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="62b44-296">SameSiteMode.Lax</span></span><br><span data-ttu-id="62b44-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="62b44-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="62b44-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="62b44-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-299">SameSiteMode.Strict</span></span>   | <span data-ttu-id="62b44-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="62b44-300">SameSiteMode.None</span></span><br><span data-ttu-id="62b44-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="62b44-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="62b44-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="62b44-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-303">SameSiteMode.Strict</span></span><br><span data-ttu-id="62b44-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="62b44-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="62b44-305">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="62b44-306">Créer un cookie d’authentification</span><span class="sxs-lookup"><span data-stu-id="62b44-306">Create an authentication cookie</span></span>

<span data-ttu-id="62b44-307">Pour créer un cookie contenant les informations de l’utilisateur, vous devez construire un [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="62b44-307">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="62b44-308">Les informations de l’utilisateur sont sérialisées et stockées dans le cookie.</span><span class="sxs-lookup"><span data-stu-id="62b44-308">The user information is serialized and stored in the cookie.</span></span> 

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="62b44-309">Créer un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) avec n’importe quel requis [revendication](/dotnet/api/system.security.claims.claim)s et appelez [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) pour connecter l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="62b44-309">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="62b44-310">Appelez [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) pour connecter l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="62b44-310">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

<span data-ttu-id="62b44-311">`SignInAsync` Crée un cookie chiffré et l’ajoute à la réponse actuelle.</span><span class="sxs-lookup"><span data-stu-id="62b44-311">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="62b44-312">Si vous ne spécifiez pas un `AuthenticationScheme`, le schéma par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="62b44-312">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="62b44-313">En coulisses, du chiffrement utilisé est d’ASP.NET Core [Protection des données](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) système.</span><span class="sxs-lookup"><span data-stu-id="62b44-313">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="62b44-314">Si vous hébergez une application sur plusieurs ordinateurs, l’équilibrage de charge entre les applications ou à l’aide d’une batterie de serveurs web, vous devez [configurer la protection des données](xref:security/data-protection/configuration/overview) à utiliser le même porte-clés et l’identificateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="62b44-314">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="62b44-315">Déconnexion</span><span class="sxs-lookup"><span data-stu-id="62b44-315">Sign out</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="62b44-316">Pour déconnecter l’utilisateur actuel et supprimer leur cookie, appelez [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="62b44-316">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="62b44-317">Pour déconnecter l’utilisateur actuel et supprimer leur cookie, appelez [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="62b44-317">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

<span data-ttu-id="62b44-318">Si vous n’utilisez pas `CookieAuthenticationDefaults.AuthenticationScheme` (ou « Cookies ») en tant que le schéma (par exemple, « ContosoCookie »), fournir le schéma que vous avez utilisé lors de la configuration du fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="62b44-318">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="62b44-319">Sinon, le schéma par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="62b44-319">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="62b44-320">Réagir aux modifications du serveur principal</span><span class="sxs-lookup"><span data-stu-id="62b44-320">React to back-end changes</span></span>

<span data-ttu-id="62b44-321">Une fois qu’un cookie est créé, il devient la seule source d’identité.</span><span class="sxs-lookup"><span data-stu-id="62b44-321">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="62b44-322">Même si vous désactivez un utilisateur dans vos systèmes back-end, le système d’authentification de cookie n’a aucune connaissance de ce et un utilisateur reste connecté tant que leur cookie est valide.</span><span class="sxs-lookup"><span data-stu-id="62b44-322">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="62b44-323">Le [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) événements dans ASP.NET Core 2.x ou [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) méthode dans ASP.NET Core 1.x peut être utilisé pour intercepter et de remplacer la validation de l’identité de cookie.</span><span class="sxs-lookup"><span data-stu-id="62b44-323">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="62b44-324">Cette approche réduit le risque d’utilisateurs révoqués l’accès à l’application.</span><span class="sxs-lookup"><span data-stu-id="62b44-324">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="62b44-325">Une approche à la validation de cookie est basée sur le suivi des lorsque la base de données utilisateur a été modifiée.</span><span class="sxs-lookup"><span data-stu-id="62b44-325">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="62b44-326">Si la base de données n’a pas été modifié depuis que le cookie d’utilisateur a été émis, il n’est pas nécessaire de s’authentifier de nouveau l’utilisateur si le cookie est toujours valide.</span><span class="sxs-lookup"><span data-stu-id="62b44-326">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="62b44-327">Pour implémenter ce scénario, la base de données, qui est implémenté dans `IUserRepository` pour cet exemple, stocke un `LastChanged` valeur.</span><span class="sxs-lookup"><span data-stu-id="62b44-327">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="62b44-328">Mise à jour de n’importe quel utilisateur dans la base de données, le `LastChanged` a la valeur à l’heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="62b44-328">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="62b44-329">Afin d’invalider un cookie lorsque les modifications de base de données basée sur le `LastChanged` valeur, la création du cookie avec un `LastChanged` revendication contenant actuel `LastChanged` valeur à partir de la base de données :</span><span class="sxs-lookup"><span data-stu-id="62b44-329">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="62b44-330">Pour implémenter une substitution pour le `ValidatePrincipal` événement, écriture, une méthode avec la signature suivante dans une classe que vous dérivez de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="62b44-330">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="62b44-331">Un exemple se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="62b44-331">An example looks like the following:</span></span>

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

<span data-ttu-id="62b44-332">Enregistrer l’instance d’événements lors de l’inscription du service cookie dans le `ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="62b44-332">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="62b44-333">Fournir une inscription de service délimité pour votre `CustomCookieAuthenticationEvents` classe :</span><span class="sxs-lookup"><span data-stu-id="62b44-333">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="62b44-334">Pour implémenter une substitution pour la `ValidateAsync` événement, écriture, une méthode avec la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="62b44-334">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="62b44-335">ASP.NET Core Identity implémente cette vérification dans le cadre de son [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="62b44-335">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="62b44-336">Un exemple se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="62b44-336">An example looks like the following:</span></span>

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

<span data-ttu-id="62b44-337">Inscrire l’événement lors de la configuration de l’authentification de cookie dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="62b44-337">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

<span data-ttu-id="62b44-338">Considérez une situation dans laquelle le nom d’utilisateur est mise à jour &mdash; une décision qui n’affecte pas la sécurité en aucune façon.</span><span class="sxs-lookup"><span data-stu-id="62b44-338">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="62b44-339">Si vous souhaitez mettre à jour non-destructive de l’utilisateur principal, appelez `context.ReplacePrincipal` et définir le `context.ShouldRenew` propriété `true`.</span><span class="sxs-lookup"><span data-stu-id="62b44-339">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="62b44-340">L’approche décrite ici est déclenchée à chaque demande.</span><span class="sxs-lookup"><span data-stu-id="62b44-340">The approach described here is triggered on every request.</span></span> <span data-ttu-id="62b44-341">Cela peut entraîner une altération des performances pour l’application.</span><span class="sxs-lookup"><span data-stu-id="62b44-341">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="62b44-342">Cookies persistants</span><span class="sxs-lookup"><span data-stu-id="62b44-342">Persistent cookies</span></span>

<span data-ttu-id="62b44-343">Vous souhaiterez peut-être le cookie à conserver entre les sessions de navigateur.</span><span class="sxs-lookup"><span data-stu-id="62b44-343">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="62b44-344">Cette persistance doit uniquement être activé avec le consentement explicite de l’utilisateur avec une case à cocher « Mémoriser mes informations » sur la connexion ou un mécanisme similaire.</span><span class="sxs-lookup"><span data-stu-id="62b44-344">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on login or a similar mechanism.</span></span> 

<span data-ttu-id="62b44-345">L’extrait de code suivant crée une identité et le cookie correspondant qui persiste lors par le biais des fermetures de navigateur.</span><span class="sxs-lookup"><span data-stu-id="62b44-345">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="62b44-346">Tous les paramètres d’expiration décalée précédemment configurés sont respectées.</span><span class="sxs-lookup"><span data-stu-id="62b44-346">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="62b44-347">Si le cookie expire pendant que le navigateur est fermé, le navigateur supprime le cookie s’est arrêté.</span><span class="sxs-lookup"><span data-stu-id="62b44-347">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="62b44-348">Le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) classe réside dans le `Microsoft.AspNetCore.Authentication` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="62b44-348">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="62b44-349">Le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) classe réside dans le `Microsoft.AspNetCore.Http.Authentication` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="62b44-349">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

::: moniker-end

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="62b44-350">Expiration du cookie absolu</span><span class="sxs-lookup"><span data-stu-id="62b44-350">Absolute cookie expiration</span></span>

<span data-ttu-id="62b44-351">Vous pouvez définir une heure d’expiration absolue avec `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="62b44-351">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="62b44-352">Vous devez également définir `IsPersistent`; sinon, `ExpiresUtc` est ignoré et un cookie de session unique est créé.</span><span class="sxs-lookup"><span data-stu-id="62b44-352">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="62b44-353">Lorsque `ExpiresUtc` est définie sur `SignInAsync`, il remplace la valeur de la `ExpireTimeSpan` option de `CookieAuthenticationOptions`, si définie.</span><span class="sxs-lookup"><span data-stu-id="62b44-353">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="62b44-354">L’extrait de code suivant crée une identité et le cookie correspondant qui dure 20 minutes.</span><span class="sxs-lookup"><span data-stu-id="62b44-354">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="62b44-355">Il ignore tous les paramètres d’expiration décalée précédemment configurés.</span><span class="sxs-lookup"><span data-stu-id="62b44-355">This ignores any sliding expiration settings previously configured.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="62b44-356">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="62b44-356">Additional resources</span></span>

* [<span data-ttu-id="62b44-357">Modifications de AUTH 2.0 / annonce de la Migration</span><span class="sxs-lookup"><span data-stu-id="62b44-357">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="62b44-358">Vérifications de rôle de stratégie</span><span class="sxs-lookup"><span data-stu-id="62b44-358">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
