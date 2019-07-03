---
title: Facebook, Google et authentification de fournisseur externe sans ASP.NET Core Identity
author: rick-anderson
description: Une explication de l’utilisation de Facebook, Google, Twitter, etc. compte utilisateur l’authentification sans ASP.NET Core Identity.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: e67da513fef1ce453110c465b08e9c7965e71df5
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557648"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Utiliser l’authentification de fournisseur de connexion réseau social sans ASP.NET Core Identity

<xref:security/authentication/social/index> Décrit comment permettre aux utilisateurs pour se connecter à l’aide d’OAuth 2.0 avec les informations d’identification à partir de fournisseurs d’authentification externes. L’approche décrite dans cette rubrique inclut ASP.NET Core Identity comme fournisseur d’authentification.

Cet exemple montre comment utiliser un fournisseur d’authentification externe **sans** ASP.NET Core Identity. Cela est utile pour les applications qui ne nécessitent pas toutes les fonctionnalités de ASP.NET Core Identity, mais nécessitent toujours une intégration avec un fournisseur d’authentification externe approuvé.

Cet exemple utilise [l’authentification Google](xref:security/authentication/google-logins) pour l’authentification des utilisateurs. À l’aide de Google authentification transfère une grande partie des complexités de la gestion du processus de connexion à Google. Pour intégrer à un fournisseur d’authentification externe, consultez les rubriques suivantes :

* [Authentification Facebook](xref:security/authentication/facebook-logins)
* [Authentification Microsoft](xref:security/authentication/microsoft-logins)
* [Authentification Twitter](xref:security/authentication/twitter-logins)
* [Autres fournisseurs](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configuration

Dans le `ConfigureServices` (méthode), configurer des schémas d’authentification de l’application avec le `AddAuthentication`, `AddCookie` et `AddGoogle` méthodes :

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

L’appel à [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) définit l’application [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme). Le `DefaultScheme` est le schéma par défaut utilisé par ce qui suit `HttpContext` méthodes d’extension d’authentification :

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Définition de l’application `DefaultScheme` à [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (« Cookies ») configure l’application pour utiliser des Cookies en tant que le schéma par défaut pour ces méthodes d’extension. Définition de l’application <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> à [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) (« Google ») configure l’application pour utiliser Google en tant que le schéma par défaut pour les appels à `ChallengeAsync`. `DefaultChallengeScheme` substitue `DefaultScheme`. Consultez <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> de propriétés supplémentaires qui substituent `DefaultScheme` lorsque la valeur.

Dans le `Configure` méthode, appelez le `UseAuthentication` méthode à appeler l’intergiciel d’authentification qui définit le `HttpContext.User` propriété. Appelez le `UseAuthentication` méthode avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Pour en savoir plus sur les schémas d’authentification et l’authentification des cookies, consultez <xref:security/authentication/cookie>.

## <a name="applying-basic-authorization"></a>Appliquer l’autorisation de base

Tester la configuration de l’authentification de l’application en appliquant la `AuthorizeAttribute` d’attribut à un contrôleur, action ou page. Le code suivant limite l’accès à la *confidentialité* page aux utilisateurs qui ont été authentifiés :

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Déconnexion

Pour déconnecter l’utilisateur actuel et supprimer leur cookie, appelez [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0). Le code suivant ajoute un `Logout` Gestionnaire de page à la *Index* page :

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

Notez que l’appel à `SignOutAsync` ne spécifie pas un schéma d’authentification. L’application `DefaultScheme` de `CookieAuthenticationDefaults.AuthenticationScheme` est utilisé comme un secours.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
