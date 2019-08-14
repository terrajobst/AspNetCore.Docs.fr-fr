# <a name="aspnet-core-authorization-sample"></a>Exemple d’autorisation ASP.NET Core

Cet exemple illustre l’utilisation de Razor Pages autorisation par les conventions. Cet exemple illustre les fonctionnalités décrites dans la rubrique [Razor pages conventions d’autorisation](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) .

L’autorisation utilisateur dans cet exemple utilise les fonctionnalités d’authentification par cookie décrites dans la rubrique [utiliser l’authentification par cookie sans ASP.net Core identité](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) . Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core identité. Pour plus d’informations sur l’utilisation de ASP.NET Core identité, consultez [Présentation de l’identité sur ASP.net Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Utilisez l’adresse **maria.rodriguez@contoso.com** de messagerie pour authentifier l’utilisateur avec n’importe quel mot de passe. L’utilisateur est authentifié dans la `AuthenticateUser` méthode dans le fichier *pages/Account/login. cshtml. cs* . Dans un exemple réel, l’utilisateur est authentifié par rapport à une base de données.

## <a name="examples-in-this-sample"></a>Extraits de cet exemple

| Fonctionnalité | Description |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Ajoute un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page avec le chemin d’accès spécifié. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Ajoute un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à toutes les pages d’un dossier avec le chemin d’accès spécifié. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Ajoute un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à une page avec le chemin d’accès spécifié. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Ajoute un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à toutes les pages d’un dossier avec le chemin d’accès spécifié. |
