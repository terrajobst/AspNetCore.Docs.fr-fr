# <a name="aspnet-core-distributed-cache-sample"></a>Exemple de Cache distribué d’ASP.NET Core

Cet exemple illustre l’utilisation d’un cache distribué. Cet exemple illustre le scénario décrit dans la [fonctionne avec un cache distribué dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/distributed) rubrique.

Dans l’environnement de Production, l’exemple d’application est configuré pour utiliser un cache distribué de SQL Server. Pour reconfigurer l’application pour utiliser un cache Redis distribué, modifiez la directive de préprocesseur en haut de la *Startup.cs* fichier à utiliser Redis (`#define Redis // SQLServer`). Pour plus d’informations, consultez [directives de préprocesseur dans l’exemple de code](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code).
