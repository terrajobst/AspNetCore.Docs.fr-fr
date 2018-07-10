# <a name="aspnet-core-web-api-controller-sample"></a>Exemple de contrôleur d’API web ASP.NET Core

Cet exemple d’application est composé des projets suivants :

- **WebApiSample.Api** : projet ASP.NET Core 2.1 ciblant .NET Core 2.1.
- **WebApiSample.Api.Pre21** : projet ASP.NET Core 2.0 ciblant .NET Core 2.0.
- **WebApiSample.DataAccess** : bibliothèque de classes .NET Standard 2.0 servant de couche d’accès aux données pour les deux projets d’API web.

Cet exemple illustre différentes façons de créer un contrôleur d’API web :

- [Dériver la classe de ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Annoter la classe avec ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
