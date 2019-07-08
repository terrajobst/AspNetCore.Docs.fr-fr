---
title: Utiliser l’authentification par cookie sans ASP.NET Core Identity
author: rick-anderson
description: Découvrez comment utiliser l’authentification par cookie sans ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622744"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="16d58-103">Utiliser l’authentification par cookie sans ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="16d58-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="16d58-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="16d58-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="16d58-105">ASP.NET Core Identity est un fournisseur d’authentification complète et complet pour créer et maintenir des connexions.</span><span class="sxs-lookup"><span data-stu-id="16d58-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="16d58-106">Toutefois, un fournisseur d’authentification de l’authentification par cookie sans ASP.NET Core Identity peut être utilisé.</span><span class="sxs-lookup"><span data-stu-id="16d58-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="16d58-107">Pour plus d'informations, consultez <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="16d58-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="16d58-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16d58-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="16d58-109">Fins de démonstration dans l’exemple d’application, le compte d’utilisateur pour l’utilisateur hypothétique, Maria Rodriguez, est codé en dur dans l’application.</span><span class="sxs-lookup"><span data-stu-id="16d58-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="16d58-110">Utilisez le **E-mail** nom d’utilisateur `maria.rodriguez@contoso.com` et n’importe quel mot de passe pour vous connecter l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16d58-110">Use the **Email** user name `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="16d58-111">L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="16d58-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="16d58-112">Dans un exemple réel, l’utilisateur serait être authentifié par rapport à une base de données.</span><span class="sxs-lookup"><span data-stu-id="16d58-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="16d58-113">Configuration</span><span class="sxs-lookup"><span data-stu-id="16d58-113">Configuration</span></span>

<span data-ttu-id="16d58-114">Si l’application n’utilise pas le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app), créez une référence de package dans le fichier de projet pour le [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span><span class="sxs-lookup"><span data-stu-id="16d58-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="16d58-115">Dans le `Startup.ConfigureServices` (méthode), créez le service de l’intergiciel d’authentification avec le <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> et <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> méthodes :</span><span class="sxs-lookup"><span data-stu-id="16d58-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="16d58-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passé à `AddAuthentication` définit le schéma d’authentification par défaut pour l’application.</span><span class="sxs-lookup"><span data-stu-id="16d58-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="16d58-117">`AuthenticationScheme` est utile quand il existe plusieurs instances de l’authentification des cookies et que vous souhaitez [autoriser avec un schéma spécifique](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="16d58-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="16d58-118">Définition de la `AuthenticationScheme` à [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) fournit une valeur de « Cookies » pour le schéma.</span><span class="sxs-lookup"><span data-stu-id="16d58-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="16d58-119">Vous pouvez fournir n’importe quelle valeur de chaîne qui distingue le schéma.</span><span class="sxs-lookup"><span data-stu-id="16d58-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="16d58-120">Schéma d’authentification de l’application est différent de schéma d’authentification de cookie de l’application.</span><span class="sxs-lookup"><span data-stu-id="16d58-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="16d58-121">Lorsqu’un schéma d’authentification de cookie n’est pas fourni pour <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, elle utilise `CookieAuthenticationDefaults.AuthenticationScheme` (« Cookies »).</span><span class="sxs-lookup"><span data-stu-id="16d58-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="16d58-122">Le cookie d’authentification <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> propriété a la valeur `true` par défaut.</span><span class="sxs-lookup"><span data-stu-id="16d58-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="16d58-123">Les cookies d’authentification sont autorisées lorsqu’un visiteur du site n’a pas donné son consentement pour la collecte de données.</span><span class="sxs-lookup"><span data-stu-id="16d58-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="16d58-124">Pour plus d'informations, consultez <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="16d58-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="16d58-125">Dans le `Startup.Configure` méthode, appelez le `UseAuthentication` méthode à appeler l’intergiciel d’authentification qui définit le `HttpContext.User` propriété.</span><span class="sxs-lookup"><span data-stu-id="16d58-125">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="16d58-126">Appelez le `UseAuthentication` méthode avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="16d58-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="16d58-127">Le <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> classe est utilisée pour configurer les options de fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="16d58-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="16d58-128">Définissez `CookieAuthenticationOptions` dans la configuration du service pour l’authentification dans le `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="16d58-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="16d58-129">Intergiciel (middleware) de cookie stratégie</span><span class="sxs-lookup"><span data-stu-id="16d58-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="16d58-130">[Intergiciel (middleware) de cookie stratégie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) offre des fonctionnalités de stratégie de cookie.</span><span class="sxs-lookup"><span data-stu-id="16d58-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="16d58-131">Ajout de l’intergiciel (middleware) au pipeline de traitement d’application est l’ordre sensible&mdash;il affecte uniquement les composants en aval enregistrés dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="16d58-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="16d58-132">Utilisez <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fourni à l’intergiciel de stratégie de Cookie pour contrôler des caractéristiques globales du traitement du cookie et de raccordement en gestionnaires de traitement du cookie lorsque les cookies sont ajoutées ou supprimées.</span><span class="sxs-lookup"><span data-stu-id="16d58-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="16d58-133">La valeur par défaut <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> valeur est `SameSiteMode.Lax` pour autoriser l’authentification OAuth2.</span><span class="sxs-lookup"><span data-stu-id="16d58-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="16d58-134">À strictement appliquer une stratégie de même site de `SameSiteMode.Strict`, définissez le `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="16d58-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="16d58-135">Bien que ce paramètre s’arrête OAuth2 et autres schémas d’authentification de cross-origin, elle élève le niveau de sécurité du cookie pour les autres types d’applications qui ne reposent pas sur le traitement de la demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="16d58-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="16d58-136">Le paramètre d’intergiciel (middleware) stratégie de Cookie pour `MinimumSameSitePolicy` peuvent affecter la valeur de `Cookie.SameSite` dans `CookieAuthenticationOptions` paramètres en fonction de la matrice ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="16d58-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="16d58-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="16d58-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="16d58-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="16d58-138">Cookie.SameSite</span></span> | <span data-ttu-id="16d58-139">Paramètre Cookie.SameSite résultante</span><span class="sxs-lookup"><span data-stu-id="16d58-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="16d58-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="16d58-140">SameSiteMode.None</span></span>     | <span data-ttu-id="16d58-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="16d58-141">SameSiteMode.None</span></span><br><span data-ttu-id="16d58-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="16d58-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="16d58-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="16d58-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="16d58-144">SameSiteMode.None</span></span><br><span data-ttu-id="16d58-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="16d58-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="16d58-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="16d58-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="16d58-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="16d58-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="16d58-148">SameSiteMode.None</span></span><br><span data-ttu-id="16d58-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="16d58-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="16d58-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="16d58-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="16d58-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="16d58-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="16d58-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="16d58-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="16d58-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="16d58-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="16d58-155">SameSiteMode.None</span></span><br><span data-ttu-id="16d58-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="16d58-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="16d58-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="16d58-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="16d58-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="16d58-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="16d58-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="16d58-161">Créer un cookie d’authentification</span><span class="sxs-lookup"><span data-stu-id="16d58-161">Create an authentication cookie</span></span>

<span data-ttu-id="16d58-162">Pour créer un cookie contenant les informations de l’utilisateur, construisez un <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="16d58-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="16d58-163">Les informations de l’utilisateur sont sérialisées et stockées dans le cookie.</span><span class="sxs-lookup"><span data-stu-id="16d58-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="16d58-164">Créer un <xref:System.Security.Claims.ClaimsIdentity> avec n’importe quel requis <xref:System.Security.Claims.Claim>s et appelez <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> pour connecter l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="16d58-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="16d58-165">`SignInAsync` Crée un cookie chiffré et l’ajoute à la réponse actuelle.</span><span class="sxs-lookup"><span data-stu-id="16d58-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="16d58-166">Si `AuthenticationScheme` n’est pas spécifié, le schéma par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="16d58-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="16d58-167">D’ASP.NET Core [Protection des données](xref:security/data-protection/using-data-protection) système est utilisé pour le chiffrement.</span><span class="sxs-lookup"><span data-stu-id="16d58-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="16d58-168">Pour une application hébergée sur plusieurs ordinateurs, charge équilibrage entre applications, ou à l’aide d’une batterie de serveurs web, [configurer la protection des données](xref:security/data-protection/configuration/overview) à utiliser le même porte-clés et l’identificateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="16d58-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="16d58-169">Déconnexion</span><span class="sxs-lookup"><span data-stu-id="16d58-169">Sign out</span></span>

<span data-ttu-id="16d58-170">Pour déconnecter l’utilisateur actuel et supprimer leur cookie, appelez <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="16d58-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="16d58-171">Si `CookieAuthenticationDefaults.AuthenticationScheme` (ou « Cookies ») n’est pas utilisé comme le schéma (par exemple, « ContosoCookie »), fournir le schéma utilisé lors de la configuration du fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="16d58-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="16d58-172">Sinon, le schéma par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="16d58-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="16d58-173">Réagir aux modifications du serveur principal</span><span class="sxs-lookup"><span data-stu-id="16d58-173">React to back-end changes</span></span>

<span data-ttu-id="16d58-174">Une fois qu’un cookie est créé, le cookie est la seule source d’identité.</span><span class="sxs-lookup"><span data-stu-id="16d58-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="16d58-175">Si un compte d’utilisateur est désactivé dans les systèmes back-end :</span><span class="sxs-lookup"><span data-stu-id="16d58-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="16d58-176">Système d’authentification de cookie de l’application continue à traiter les demandes basées sur le cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="16d58-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="16d58-177">L’utilisateur reste connecté à l’application tant que le cookie d’authentification est valid.</span><span class="sxs-lookup"><span data-stu-id="16d58-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="16d58-178">Le <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> événement peut être utilisé pour intercepter et de remplacer la validation de l’identité de cookie.</span><span class="sxs-lookup"><span data-stu-id="16d58-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="16d58-179">Valider le cookie à chaque demande d’atténue le risque des utilisateurs révoqués l’accès à l’application.</span><span class="sxs-lookup"><span data-stu-id="16d58-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="16d58-180">Une approche à la validation de cookie est basée sur le suivi des lorsque la base de données utilisateur est modifié.</span><span class="sxs-lookup"><span data-stu-id="16d58-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="16d58-181">Si la base de données n’a pas été modifié depuis que le cookie d’utilisateur a été émis, il n’est pas nécessaire de s’authentifier de nouveau l’utilisateur si le cookie est toujours valide.</span><span class="sxs-lookup"><span data-stu-id="16d58-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="16d58-182">Dans l’exemple d’application, la base de données est implémenté dans `IUserRepository` et stocke un `LastChanged` valeur.</span><span class="sxs-lookup"><span data-stu-id="16d58-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="16d58-183">Lorsqu’un utilisateur est mis à jour dans la base de données, le `LastChanged` a la valeur à l’heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="16d58-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="16d58-184">Afin d’invalider un cookie lorsque les modifications de base de données basée sur le `LastChanged` valeur, la création du cookie avec un `LastChanged` revendication contenant actuel `LastChanged` valeur à partir de la base de données :</span><span class="sxs-lookup"><span data-stu-id="16d58-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="16d58-185">Pour implémenter une substitution pour le `ValidatePrincipal` événement, écriture, une méthode avec la signature suivante dans une classe qui dérive de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="16d58-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="16d58-186">Voici un exemple d’implémentation de `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="16d58-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="16d58-187">Enregistrer l’instance d’événements lors de l’inscription du service cookie dans le `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="16d58-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="16d58-188">Fournir un [étendue de l’inscription du service](xref:fundamentals/dependency-injection#service-lifetimes) pour votre `CustomCookieAuthenticationEvents` classe :</span><span class="sxs-lookup"><span data-stu-id="16d58-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="16d58-189">Considérez une situation dans laquelle le nom d’utilisateur est mise à jour&mdash;une décision qui n’affecte pas la sécurité en aucune façon.</span><span class="sxs-lookup"><span data-stu-id="16d58-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="16d58-190">Si vous souhaitez mettre à jour non-destructive de l’utilisateur principal, appelez `context.ReplacePrincipal` et définir le `context.ShouldRenew` propriété `true`.</span><span class="sxs-lookup"><span data-stu-id="16d58-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="16d58-191">L’approche décrite ici est déclenchée à chaque demande.</span><span class="sxs-lookup"><span data-stu-id="16d58-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="16d58-192">Validation des cookies d’authentification pour tous les utilisateurs à chaque demande peut entraîner une altération des performances pour l’application.</span><span class="sxs-lookup"><span data-stu-id="16d58-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="16d58-193">Cookies persistants</span><span class="sxs-lookup"><span data-stu-id="16d58-193">Persistent cookies</span></span>

<span data-ttu-id="16d58-194">Vous souhaiterez peut-être le cookie à conserver entre les sessions de navigateur.</span><span class="sxs-lookup"><span data-stu-id="16d58-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="16d58-195">Cette persistance doit uniquement être activé avec le consentement explicite de l’utilisateur avec une case à cocher « Mémoriser mes informations » sur la connexion ou un mécanisme similaire.</span><span class="sxs-lookup"><span data-stu-id="16d58-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="16d58-196">L’extrait de code suivant crée une identité et le cookie correspondant qui persiste lors par le biais des fermetures de navigateur.</span><span class="sxs-lookup"><span data-stu-id="16d58-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="16d58-197">Tous les paramètres d’expiration décalée précédemment configurés sont respectées.</span><span class="sxs-lookup"><span data-stu-id="16d58-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="16d58-198">Si le cookie expire pendant que le navigateur est fermé, le navigateur supprime le cookie s’est arrêté.</span><span class="sxs-lookup"><span data-stu-id="16d58-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="16d58-199">Définissez <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> à `true` dans <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="16d58-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="16d58-200">Expiration du cookie absolu</span><span class="sxs-lookup"><span data-stu-id="16d58-200">Absolute cookie expiration</span></span>

<span data-ttu-id="16d58-201">Un délai d’expiration absolue peut être défini avec <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="16d58-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="16d58-202">Pour créer un cookie persistant, `IsPersistent` doit également être défini.</span><span class="sxs-lookup"><span data-stu-id="16d58-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="16d58-203">Sinon, le cookie est créé avec une durée de vie de session et peut expirer avant ou après le ticket d’authentification qu’il détient.</span><span class="sxs-lookup"><span data-stu-id="16d58-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="16d58-204">Lorsque `ExpiresUtc` est défini, il remplace la valeur de la <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option de <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, si définie.</span><span class="sxs-lookup"><span data-stu-id="16d58-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="16d58-205">L’extrait de code suivant crée une identité et le cookie correspondant qui dure 20 minutes.</span><span class="sxs-lookup"><span data-stu-id="16d58-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="16d58-206">Il ignore tous les paramètres d’expiration décalée précédemment configurés.</span><span class="sxs-lookup"><span data-stu-id="16d58-206">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a><span data-ttu-id="16d58-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="16d58-207">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="16d58-208">Vérifications de rôle de stratégie</span><span class="sxs-lookup"><span data-stu-id="16d58-208">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
