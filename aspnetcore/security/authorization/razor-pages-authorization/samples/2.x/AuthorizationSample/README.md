# <a name="aspnet-core-authorization-sample"></a>Exemple de l’autorisation ASP.NET Core

Cet exemple illustre l’utilisation de l’autorisation de Pages Razor par des conventions. Cet exemple illustre les fonctionnalités décrites dans le [conventions d’autorisation des Pages Razor](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) rubrique.

Autorisation de l’utilisateur dans cet exemple utilise l’authentification de cookie les fonctionnalités décrites dans le [utiliser l’authentification de cookie sans ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) rubrique. Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core Identity. Pour plus d’informations sur l’utilisation d’ASP.NET Core Identity, consultez [présentation d’Identity dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Utiliser l’adresse de messagerie **maria.rodriguez@contoso.com** pour authentifier l’utilisateur avec n’importe quel mot de passe. L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier. Dans un exemple réel, l’utilisateur serait être authentifié par rapport à une base de données.

## <a name="examples-in-this-sample"></a>Extraits de cet exemple

| Fonctionnalité | Description |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Ajoute un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page avec le chemin d’accès spécifié. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Ajoute un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à toutes les pages dans un dossier avec le chemin d’accès spécifié. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Ajoute un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à une page avec le chemin d’accès spécifié. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Ajoute un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à toutes les pages dans un dossier avec le chemin d’accès spécifié. |
