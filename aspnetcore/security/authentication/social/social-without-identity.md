---
title: Authentification Facebook, Google et fournisseur externe sans ASP.NET Core d’identité
author: rick-anderson
description: Explication de l’utilisation de l’authentification utilisateur Facebook, Google, Twitter, etc. sans ASP.NET Core identité.
ms.author: riande
ms.date: 11/19/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 680ea091dcc5ed7f94879b5d277e8be7e5abeb7b
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289113"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="63568-103">Utiliser l’authentification du fournisseur de connexion sociale sans ASP.NET Core d’identité</span><span class="sxs-lookup"><span data-stu-id="63568-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="63568-104"><xref:security/authentication/social/index> explique comment permettre aux utilisateurs de se connecter à l’aide d’OAuth 2,0 avec les informations d’identification des fournisseurs d’authentification externes.</span><span class="sxs-lookup"><span data-stu-id="63568-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="63568-105">L’approche décrite dans cette rubrique comprend ASP.NET Core identité comme fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="63568-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="63568-106">Cet exemple montre comment utiliser un fournisseur d’authentification externe **sans** ASP.net Core identité.</span><span class="sxs-lookup"><span data-stu-id="63568-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="63568-107">Cela est utile pour les applications qui n’ont pas besoin de toutes les fonctionnalités de ASP.NET Core identité, mais qui nécessitent toujours une intégration avec un fournisseur d’authentification externe approuvé.</span><span class="sxs-lookup"><span data-stu-id="63568-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="63568-108">Cet exemple utilise [l’authentification Google](xref:security/authentication/google-logins) pour authentifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="63568-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="63568-109">L’utilisation de l’authentification Google décale un grand nombre des complexités liées à la gestion du processus de connexion à Google.</span><span class="sxs-lookup"><span data-stu-id="63568-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="63568-110">Pour une intégration avec un autre fournisseur d’authentification externe, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="63568-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="63568-111">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="63568-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="63568-112">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="63568-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="63568-113">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="63568-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="63568-114">Autres fournisseurs</span><span class="sxs-lookup"><span data-stu-id="63568-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="63568-115">Configuration</span><span class="sxs-lookup"><span data-stu-id="63568-115">Configuration</span></span>

<span data-ttu-id="63568-116">Dans la méthode `ConfigureServices`, configurez les schémas d’authentification de l’application avec les méthodes <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>et <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> :</span><span class="sxs-lookup"><span data-stu-id="63568-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

<span data-ttu-id="63568-117">L’appel à <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> définit le <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>de l’application.</span><span class="sxs-lookup"><span data-stu-id="63568-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="63568-118">Le `DefaultScheme` est le schéma par défaut utilisé par les méthodes d’extension d’authentification `HttpContext` suivantes :</span><span class="sxs-lookup"><span data-stu-id="63568-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="63568-119">La définition de la `DefaultScheme` de l’application sur [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (« cookies ») configure l’application pour utiliser les cookies comme schéma par défaut pour ces méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="63568-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="63568-120">La définition de la <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> de l’application sur [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) (« Google ») configure l’application pour utiliser Google comme modèle par défaut pour les appels à `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="63568-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="63568-121">`DefaultChallengeScheme` remplace `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="63568-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="63568-122">Consultez <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> pour obtenir des propriétés supplémentaires qui remplacent `DefaultScheme` quand elles sont définies.</span><span class="sxs-lookup"><span data-stu-id="63568-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="63568-123">Dans `Startup.Configure`, appelez `UseAuthentication` et `UseAuthorization` entre l’appel de `UseRouting` et `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="63568-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` between calling `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="63568-124">Cela définit la propriété `HttpContext.User` et exécute l’intergiciel (middleware) des autorisations pour les demandes :</span><span class="sxs-lookup"><span data-stu-id="63568-124">This sets the `HttpContext.User` property and runs the Authorization Middleware for requests:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

<span data-ttu-id="63568-125">Pour en savoir plus sur les schémas d’authentification et l’authentification des cookies, consultez <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="63568-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="63568-126">Appliquer l’autorisation</span><span class="sxs-lookup"><span data-stu-id="63568-126">Apply authorization</span></span>

<span data-ttu-id="63568-127">Testez la configuration d’authentification de l’application en appliquant l’attribut `AuthorizeAttribute` à un contrôleur, une action ou une page.</span><span class="sxs-lookup"><span data-stu-id="63568-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="63568-128">Le code suivant limite l’accès à la page de *confidentialité* aux utilisateurs qui ont été authentifiés :</span><span class="sxs-lookup"><span data-stu-id="63568-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="63568-129">Déconnexion</span><span class="sxs-lookup"><span data-stu-id="63568-129">Sign out</span></span>

<span data-ttu-id="63568-130">Pour déconnecter l’utilisateur actuel et supprimer son cookie, appelez [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="63568-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="63568-131">Le code suivant ajoute une `Logout` gestionnaire de page à la page d' *index* :</span><span class="sxs-lookup"><span data-stu-id="63568-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="63568-132">Notez que l’appel à `SignOutAsync` ne spécifie pas de schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="63568-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="63568-133">Le `DefaultScheme` de `CookieAuthenticationDefaults.AuthenticationScheme` de l’application est utilisé comme un recul.</span><span class="sxs-lookup"><span data-stu-id="63568-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63568-134">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="63568-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="63568-135"><xref:security/authentication/social/index> explique comment permettre aux utilisateurs de se connecter à l’aide d’OAuth 2,0 avec les informations d’identification des fournisseurs d’authentification externes.</span><span class="sxs-lookup"><span data-stu-id="63568-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="63568-136">L’approche décrite dans cette rubrique comprend ASP.NET Core identité comme fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="63568-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="63568-137">Cet exemple montre comment utiliser un fournisseur d’authentification externe **sans** ASP.net Core identité.</span><span class="sxs-lookup"><span data-stu-id="63568-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="63568-138">Cela est utile pour les applications qui n’ont pas besoin de toutes les fonctionnalités de ASP.NET Core identité, mais qui nécessitent toujours une intégration avec un fournisseur d’authentification externe approuvé.</span><span class="sxs-lookup"><span data-stu-id="63568-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="63568-139">Cet exemple utilise [l’authentification Google](xref:security/authentication/google-logins) pour authentifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="63568-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="63568-140">L’utilisation de l’authentification Google décale un grand nombre des complexités liées à la gestion du processus de connexion à Google.</span><span class="sxs-lookup"><span data-stu-id="63568-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="63568-141">Pour une intégration avec un autre fournisseur d’authentification externe, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="63568-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="63568-142">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="63568-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="63568-143">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="63568-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="63568-144">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="63568-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="63568-145">Autres fournisseurs</span><span class="sxs-lookup"><span data-stu-id="63568-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="63568-146">Configuration</span><span class="sxs-lookup"><span data-stu-id="63568-146">Configuration</span></span>

<span data-ttu-id="63568-147">Dans la méthode `ConfigureServices`, configurez les schémas d’authentification de l’application avec les méthodes `AddAuthentication`, `AddCookie`et `AddGoogle` :</span><span class="sxs-lookup"><span data-stu-id="63568-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

<span data-ttu-id="63568-148">L’appel à [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) définit le [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)de l’application.</span><span class="sxs-lookup"><span data-stu-id="63568-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="63568-149">Le `DefaultScheme` est le schéma par défaut utilisé par les méthodes d’extension d’authentification `HttpContext` suivantes :</span><span class="sxs-lookup"><span data-stu-id="63568-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="63568-150">La définition de la `DefaultScheme` de l’application sur [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (« cookies ») configure l’application pour utiliser les cookies comme schéma par défaut pour ces méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="63568-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="63568-151">La définition de la <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> de l’application sur [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) (« Google ») configure l’application pour utiliser Google comme modèle par défaut pour les appels à `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="63568-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="63568-152">`DefaultChallengeScheme` remplace `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="63568-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="63568-153">Consultez <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> pour obtenir des propriétés supplémentaires qui remplacent `DefaultScheme` quand elles sont définies.</span><span class="sxs-lookup"><span data-stu-id="63568-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="63568-154">Dans la méthode `Configure`, appelez la méthode `UseAuthentication` pour appeler l’intergiciel (middleware) d’authentification qui définit la propriété `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="63568-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="63568-155">Appelez la méthode `UseAuthentication` avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="63568-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

<span data-ttu-id="63568-156">Pour en savoir plus sur les schémas d’authentification et l’authentification des cookies, consultez <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="63568-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="63568-157">Appliquer l’autorisation</span><span class="sxs-lookup"><span data-stu-id="63568-157">Apply authorization</span></span>

<span data-ttu-id="63568-158">Testez la configuration d’authentification de l’application en appliquant l’attribut `AuthorizeAttribute` à un contrôleur, une action ou une page.</span><span class="sxs-lookup"><span data-stu-id="63568-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="63568-159">Le code suivant limite l’accès à la page de *confidentialité* aux utilisateurs qui ont été authentifiés :</span><span class="sxs-lookup"><span data-stu-id="63568-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="63568-160">Déconnexion</span><span class="sxs-lookup"><span data-stu-id="63568-160">Sign out</span></span>

<span data-ttu-id="63568-161">Pour déconnecter l’utilisateur actuel et supprimer son cookie, appelez [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="63568-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="63568-162">Le code suivant ajoute une `Logout` gestionnaire de page à la page d' *index* :</span><span class="sxs-lookup"><span data-stu-id="63568-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="63568-163">Notez que l’appel à `SignOutAsync` ne spécifie pas de schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="63568-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="63568-164">Le `DefaultScheme` de `CookieAuthenticationDefaults.AuthenticationScheme` de l’application est utilisé comme un recul.</span><span class="sxs-lookup"><span data-stu-id="63568-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63568-165">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="63568-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
