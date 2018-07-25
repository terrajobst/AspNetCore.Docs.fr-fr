# <a name="cookie-sharing-sample-app"></a>Exemple d’application de cookie de partage

L’exemple illustre le partage de cookies entre trois applications qui utilisent l’authentification de cookie :

| Projet                             | Description |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core Razor Pages application sans utiliser ASP.NET Core Identity |
| CookieAuthWithIdentity.Core         | Application MVC ASP.NET Core avec ASP.NET Core Identity |
| CookieAuthWithIdentity.NETFramework | Application MVC ASP.NET Framework avec ASP.NET Identity |

Instructions :

1. Exécutez l’application CookieAuth.Core. Inscrire un utilisateur. L’application authentifie l’utilisateur lorsque l’utilisateur est inscrit. Déconnecter l’utilisateur.
1. Dans la même session de navigateur, exécutez l’application CookieAuthWithIdentity.Core. Inscrire le même utilisateur que celles utilisées avec l’application de base. L’application authentifie l’utilisateur lorsque l’utilisateur est inscrit. Déconnecter l’utilisateur.
1. Dans la même session de navigateur, exécutez l’application CookieAuthWithIdentity.NETFramework. Inscrire le même utilisateur que celles utilisées avec d’autres applications. L’application authentifie l’utilisateur lorsque l’utilisateur est inscrit. Déconnecter l’utilisateur.
1. Connecter l’utilisateur à l’un des trois applications. Le cookie d’authentification est partagé entre les applications. Notez que l’utilisateur est automatiquement connecté à deux autres applications.
1. Déconnecter l’utilisateur à partir d’une des applications. Notez que l’utilisateur se déconnecte automatiquement les deux autres applications.

Cet exemple illustre les fonctionnalités décrites dans le [partager des cookies entre applications](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) rubrique.
