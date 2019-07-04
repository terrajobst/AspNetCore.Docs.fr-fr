---
title: Facebook, Google et authentification de fournisseur externe sans ASP.NET Core Identity
author: rick-anderson
description: Une explication de l’utilisation de Facebook, Google, Twitter, etc. compte utilisateur l’authentification sans ASP.NET Core Identity.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 1e7124e8b07c0faf2d005ec3ef55c0414a697d64
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561570"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="527dc-103">Utiliser l’authentification de fournisseur de connexion réseau social sans ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="527dc-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="527dc-104"><xref:security/authentication/social/index> Décrit comment permettre aux utilisateurs pour se connecter à l’aide d’OAuth 2.0 avec les informations d’identification à partir de fournisseurs d’authentification externes.</span><span class="sxs-lookup"><span data-stu-id="527dc-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="527dc-105">L’approche décrite dans cette rubrique inclut ASP.NET Core Identity comme fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="527dc-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="527dc-106">Cet exemple montre comment utiliser un fournisseur d’authentification externe **sans** ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="527dc-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="527dc-107">Cela est utile pour les applications qui ne nécessitent pas toutes les fonctionnalités de ASP.NET Core Identity, mais nécessitent toujours une intégration avec un fournisseur d’authentification externe approuvé.</span><span class="sxs-lookup"><span data-stu-id="527dc-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="527dc-108">Cet exemple utilise [l’authentification Google](xref:security/authentication/google-logins) pour l’authentification des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="527dc-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="527dc-109">À l’aide de Google authentification transfère une grande partie des complexités de la gestion du processus de connexion à Google.</span><span class="sxs-lookup"><span data-stu-id="527dc-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="527dc-110">Pour intégrer à un fournisseur d’authentification externe, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="527dc-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="527dc-111">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="527dc-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="527dc-112">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="527dc-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="527dc-113">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="527dc-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="527dc-114">Autres fournisseurs</span><span class="sxs-lookup"><span data-stu-id="527dc-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="527dc-115">Configuration</span><span class="sxs-lookup"><span data-stu-id="527dc-115">Configuration</span></span>

<span data-ttu-id="527dc-116">Dans le `ConfigureServices` (méthode), configurer des schémas d’authentification de l’application avec le `AddAuthentication`, `AddCookie` et `AddGoogle` méthodes :</span><span class="sxs-lookup"><span data-stu-id="527dc-116">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie` and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="527dc-117">L’appel à [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) définit l’application [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span><span class="sxs-lookup"><span data-stu-id="527dc-117">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="527dc-118">Le `DefaultScheme` est le schéma par défaut utilisé par ce qui suit `HttpContext` méthodes d’extension d’authentification :</span><span class="sxs-lookup"><span data-stu-id="527dc-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="527dc-119">Définition de l’application `DefaultScheme` à [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (« Cookies ») configure l’application pour utiliser des Cookies en tant que le schéma par défaut pour ces méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="527dc-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="527dc-120">Définition de l’application <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> à [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) (« Google ») configure l’application pour utiliser Google en tant que le schéma par défaut pour les appels à `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="527dc-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="527dc-121">`DefaultChallengeScheme` substitue `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="527dc-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="527dc-122">Consultez <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> de propriétés supplémentaires qui substituent `DefaultScheme` lorsque la valeur.</span><span class="sxs-lookup"><span data-stu-id="527dc-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="527dc-123">Dans le `Configure` méthode, appelez le `UseAuthentication` méthode à appeler l’intergiciel d’authentification qui définit le `HttpContext.User` propriété.</span><span class="sxs-lookup"><span data-stu-id="527dc-123">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="527dc-124">Appelez le `UseAuthentication` méthode avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="527dc-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="527dc-125">Pour en savoir plus sur les schémas d’authentification et l’authentification des cookies, consultez <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="527dc-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="applying-authorization"></a><span data-ttu-id="527dc-126">Application d’autorisation</span><span class="sxs-lookup"><span data-stu-id="527dc-126">Applying authorization</span></span>

<span data-ttu-id="527dc-127">Tester la configuration de l’authentification de l’application en appliquant la `AuthorizeAttribute` d’attribut à un contrôleur, action ou page.</span><span class="sxs-lookup"><span data-stu-id="527dc-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="527dc-128">Le code suivant limite l’accès à la *confidentialité* page aux utilisateurs qui ont été authentifiés :</span><span class="sxs-lookup"><span data-stu-id="527dc-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="527dc-129">Déconnexion</span><span class="sxs-lookup"><span data-stu-id="527dc-129">Sign out</span></span>

<span data-ttu-id="527dc-130">Pour déconnecter l’utilisateur actuel et supprimer leur cookie, appelez [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="527dc-130">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span></span> <span data-ttu-id="527dc-131">Le code suivant ajoute un `Logout` Gestionnaire de page à la *Index* page :</span><span class="sxs-lookup"><span data-stu-id="527dc-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="527dc-132">Notez que l’appel à `SignOutAsync` ne spécifie pas un schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="527dc-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="527dc-133">L’application `DefaultScheme` de `CookieAuthenticationDefaults.AuthenticationScheme` est utilisé comme un secours.</span><span class="sxs-lookup"><span data-stu-id="527dc-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="527dc-134">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="527dc-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
