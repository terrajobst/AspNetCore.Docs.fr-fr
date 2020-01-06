---
title: Authentification Facebook, Google et fournisseur externe sans ASP.NET Core d’identité
author: rick-anderson
description: Explication de l’utilisation de l’authentification utilisateur Facebook, Google, Twitter, etc. sans ASP.NET Core identité.
ms.author: riande
ms.date: 12/10/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 612964ec9ed4975cdc81780dda3bac6cce96037f
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75359056"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Utiliser l’authentification du fournisseur de connexion sociale sans ASP.NET Core d’identité

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index> explique comment permettre aux utilisateurs de se connecter à l’aide d’OAuth 2,0 avec les informations d’identification des fournisseurs d’authentification externes. L’approche décrite dans cette rubrique comprend ASP.NET Core identité comme fournisseur d’authentification.

Cet exemple montre comment utiliser un fournisseur d’authentification externe **sans** ASP.net Core identité. Cela est utile pour les applications qui n’ont pas besoin de toutes les fonctionnalités de ASP.NET Core identité, mais qui nécessitent toujours une intégration avec un fournisseur d’authentification externe approuvé.

Cet exemple utilise [l’authentification Google](xref:security/authentication/google-logins) pour authentifier les utilisateurs. L’utilisation de l’authentification Google décale un grand nombre des complexités liées à la gestion du processus de connexion à Google. Pour une intégration avec un autre fournisseur d’authentification externe, consultez les rubriques suivantes :

* [Authentification Facebook](xref:security/authentication/facebook-logins)
* [Authentification Microsoft](xref:security/authentication/microsoft-logins)
* [Authentification Twitter](xref:security/authentication/twitter-logins)
* [Autres fournisseurs](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configuration

Dans la méthode `ConfigureServices`, configurez les schémas d’authentification de l’application avec les méthodes <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>et <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> :

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

L’appel à <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> définit le <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>de l’application. Le `DefaultScheme` est le schéma par défaut utilisé par les méthodes d’extension d’authentification `HttpContext` suivantes :

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

La définition de la `DefaultScheme` de l’application sur [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (« cookies ») configure l’application pour utiliser les cookies comme schéma par défaut pour ces méthodes d’extension. La définition de la <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> de l’application sur [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) (« Google ») configure l’application pour utiliser Google comme modèle par défaut pour les appels à `ChallengeAsync`. `DefaultChallengeScheme` remplace `DefaultScheme`. Consultez <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> pour obtenir des propriétés supplémentaires qui remplacent `DefaultScheme` quand elles sont définies.

Dans `Startup.Configure`, appelez `UseAuthentication` et `UseAuthorization` entre l’appel de `UseRouting` et `UseEndpoints`. Cela définit la propriété `HttpContext.User` et exécute l’intergiciel (middleware) des autorisations pour les demandes :

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

Pour en savoir plus sur les schémas d’authentification, consultez [concepts d’authentification](xref:security/authentication/index#authentication-concepts). Pour en savoir plus sur l’authentification par cookie, consultez <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Appliquer l’autorisation

Testez la configuration d’authentification de l’application en appliquant l’attribut `AuthorizeAttribute` à un contrôleur, une action ou une page. Le code suivant limite l’accès à la page de *confidentialité* aux utilisateurs qui ont été authentifiés :

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Déconnexion

Pour déconnecter l’utilisateur actuel et supprimer son cookie, appelez [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Le code suivant ajoute une `Logout` gestionnaire de page à la page d' *index* :

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

Notez que l’appel à `SignOutAsync` ne spécifie pas de schéma d’authentification. Le `DefaultScheme` de `CookieAuthenticationDefaults.AuthenticationScheme` de l’application est utilisé comme un recul.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index> explique comment permettre aux utilisateurs de se connecter à l’aide d’OAuth 2,0 avec les informations d’identification des fournisseurs d’authentification externes. L’approche décrite dans cette rubrique comprend ASP.NET Core identité comme fournisseur d’authentification.

Cet exemple montre comment utiliser un fournisseur d’authentification externe **sans** ASP.net Core identité. Cela est utile pour les applications qui n’ont pas besoin de toutes les fonctionnalités de ASP.NET Core identité, mais qui nécessitent toujours une intégration avec un fournisseur d’authentification externe approuvé.

Cet exemple utilise [l’authentification Google](xref:security/authentication/google-logins) pour authentifier les utilisateurs. L’utilisation de l’authentification Google décale un grand nombre des complexités liées à la gestion du processus de connexion à Google. Pour une intégration avec un autre fournisseur d’authentification externe, consultez les rubriques suivantes :

* [Authentification Facebook](xref:security/authentication/facebook-logins)
* [Authentification Microsoft](xref:security/authentication/microsoft-logins)
* [Authentification Twitter](xref:security/authentication/twitter-logins)
* [Autres fournisseurs](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configuration

Dans la méthode `ConfigureServices`, configurez les schémas d’authentification de l’application avec les méthodes `AddAuthentication`, `AddCookie`et `AddGoogle` :

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

L’appel à [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) définit le [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)de l’application. Le `DefaultScheme` est le schéma par défaut utilisé par les méthodes d’extension d’authentification `HttpContext` suivantes :

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

La définition de la `DefaultScheme` de l’application sur [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (« cookies ») configure l’application pour utiliser les cookies comme schéma par défaut pour ces méthodes d’extension. La définition de la <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> de l’application sur [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) (« Google ») configure l’application pour utiliser Google comme modèle par défaut pour les appels à `ChallengeAsync`. `DefaultChallengeScheme` remplace `DefaultScheme`. Consultez <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> pour obtenir des propriétés supplémentaires qui remplacent `DefaultScheme` quand elles sont définies.

Dans la méthode `Configure`, appelez la méthode `UseAuthentication` pour appeler l’intergiciel (middleware) d’authentification qui définit la propriété `HttpContext.User`. Appelez la méthode `UseAuthentication` avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

Pour en savoir plus sur les schémas d’authentification, consultez [concepts d’authentification](xref:security/authentication/index#authentication-concepts). Pour en savoir plus sur l’authentification par cookie, consultez <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Appliquer l’autorisation

Testez la configuration d’authentification de l’application en appliquant l’attribut `AuthorizeAttribute` à un contrôleur, une action ou une page. Le code suivant limite l’accès à la page de *confidentialité* aux utilisateurs qui ont été authentifiés :

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Déconnexion

Pour déconnecter l’utilisateur actuel et supprimer son cookie, appelez [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Le code suivant ajoute une `Logout` gestionnaire de page à la page d' *index* :

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

Notez que l’appel à `SignOutAsync` ne spécifie pas de schéma d’authentification. Le `DefaultScheme` de `CookieAuthenticationDefaults.AuthenticationScheme` de l’application est utilisé comme un recul.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
