# <a name="aspnet-core-response-caching-sample"></a>Exemple de la mise en cache de réponse ASP.NET Core

Cet exemple illustre l’utilisation d’ASP.NET Core [intergiciel de mise en cache des réponses](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

L’application répond avec sa page d’Index, y compris un `Cache-Control` en-tête pour configurer le comportement de mise en cache. L’application définit également la `Vary` en-tête pour configurer le cache pour répondre à la réponse uniquement si le `Accept-Encoding` en-tête des demandes suivantes correspondent à ceux de la demande d’origine.

Lorsque vous exécutez l’exemple, la page d’Index est pris en charge à partir du cache lorsque stockés et mis en cache pendant 10 secondes.

Pour tester le comportement de mise en cache :

* N’utilisez pas un navigateur pour tester le comportement de mise en cache. Navigateurs ajoutent souvent un en-tête de contrôle de cache lors du rechargement qui empêchent l’intergiciel (middleware) prennent en charge une page mise en cache. Par exemple, un `Cache-Control` en-tête avec la valeur `max-age=0`) pouvant être ajoutés par le navigateur.
* Utiliser un outil de développement qui permet de définir les en-têtes de demande explicitement, tels que <a href="https://www.telerik.com/fiddler">Fiddler</a> ou <a href="https://www.getpostman.com/">Postman</a>.
