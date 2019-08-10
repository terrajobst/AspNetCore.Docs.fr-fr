# <a name="aspnet-core-response-caching-sample"></a>Exemple de mise en cache des réponses ASP.NET Core

Cet exemple illustre l’utilisation de l’intergiciel (middleware) de [mise en cache des réponses](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)ASP.net core.

L’application répond avec sa page d’index, y `Cache-Control` compris un en-tête pour configurer le comportement de mise en cache. L’application définit également l' `Vary` en-tête pour configurer le cache de façon à répondre à `Accept-Encoding` la réponse uniquement si l’en-tête des demandes suivantes correspond à celui de la demande d’origine.

Lors de l’exécution de l’exemple, la page d’index est traitée à partir du cache lorsqu’elle est stockée et mise en cache pendant 10 secondes.

Pour tester le comportement de mise en cache:

* N’utilisez pas de navigateur pour tester le comportement de mise en cache. Les navigateurs ajoutent souvent un en-tête de contrôle de cache lors du rechargement qui empêchent l’intergiciel de servir une page mise en cache. Par exemple, un `Cache-Control` en-tête avec la `max-age=0`valeur) peut être ajouté par le navigateur.
* Utilisez un outil de développement qui permet de définir explicitement les en-têtes de demande, tels que <a href="https://www.telerik.com/fiddler">Fiddler</a> ou <a href="https://www.getpostman.com/">poster</a>.
