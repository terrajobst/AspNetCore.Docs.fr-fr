---
title: "Implémentation du serveur web Kestrel dans ASP.NET Core"
author: tdykstra
description: "Présente Kestrel, serveur web multiplateforme pour ASP.NET Core sur libuv."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Présentation de l’implémentation du serveur web Kestrel dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) et [Stephen Halter](https://twitter.com/halter73)

[Kestrel](index.md) est un serveur web multiplateforme pour ASP.NET Core basé sur [libuv](https://github.com/libuv/libuv), bibliothèque d’E/S asynchrones multiplateforme. Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core. 

Kestrel prend en charge les fonctionnalités suivantes :

  * HTTPS
  * Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)
  * Sockets UNIX pour des performances élevées derrière Nginx 

Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Affichez ou téléchargez l’exemple de code pour 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Affichez ou téléchargez l’exemple de code pour 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Quand utiliser Kestrel avec un proxy inverse ?

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Vous pouvez utiliser uniquement Kestrel ou l’associer à un *serveur proxy inverse*, par exemple IIS, Nginx ou Apache. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

L’une ou l’autre des configurations, &mdash;avec ou sans serveur proxy inverse&mdash;, peut également être utilisée si Kestrel est exposé uniquement à un réseau interne.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si votre application accepte uniquement les requêtes d’un réseau interne, vous pouvez utiliser Kestrel de manière autonome.

![Kestrel communique directement avec votre réseau interne](kestrel/_static/kestrel-to-internal.png)

Si vous exposez votre application à Internet, vous devez utiliser IIS, Nginx ou Apache en tant que *serveur proxy inverse*. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

Un proxy inverse est requis pour les déploiements Edge (exposés au trafic en provenance d’Internet) pour des raisons de sécurité. Les versions 1.x de Kestrel n’ont pas un ensemble complet de mesures de défense contre les attaques. Cela inclut, entre autres choses, les délais d’attente appropriés, les limites de taille et les limites de connexions simultanées.

---

Par exemple, un proxy inverse est nécessaire si plusieurs applications qui partagent les mêmes adresse IP et port s’exécutent sur un seul serveur. Cette configuration ne fonctionne pas avec Kestrel directement, car ce dernier ne prend pas en charge le partage des mêmes adresse IP et port entre plusieurs processus. Quand vous configurez Kestrel pour qu’il écoute sur un port, il gère tout le trafic pour ce port, quel que soit l’en-tête d’hôte. Un proxy inverse qui peut partager des ports doit ensuite transférer les données à Kestrel sur une adresse IP et un port uniques.

Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix pour d’autres raisons :

* Il peut limiter votre surface d’exposition.
* Il fournit une couche de configuration et de défense supplémentaire facultative.
* Il peut mieux s’intégrer à l’infrastructure existante.
* Il simplifie l’équilibrage de charge la configuration SSL. Seul votre serveur proxy inverse requiert un certificat SSL ; ce serveur peut donc communiquer avec vos serveurs d’applications sur le réseau interne à l’aide de requêtes HTTP normales.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Comment utiliser Kestrel dans les applications ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Le package [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

Les modèles de projet ASP.NET Core utilisent Kestrel par défaut. Dans *Program.cs*, le modèle de code appelle `CreateDefaultBuilder`, qui appelle [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en arrière-plan.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Si vous avez besoin de configurer des options Kestrel, appelez `UseKestrel` dans *Program.cs*, comme indiqué dans l’exemple suivant :

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installez le package NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Appelez la méthode d’extension [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) sur `WebHostBuilder` dans votre méthode `Main`, en spécifiant les [options Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) dont vous avez besoin, comme indiqué dans la prochaine section.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Options Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet. Voici quelques-unes des limites que vous pouvez définir :

- Nombre maximale de connexions client
- Taille maximale du corps de la requête
- Débit données minimal du corps de la requête

Vous définissez ces contraintes et d’autres dans la propriété `Limits` de la classe [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs). La propriété `Limits` conserve une instance de la classe [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs). 

**Nombre maximal de connexions client**

Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket). Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`. 

Le nombre maximal de connexions est illimité (null) par défaut.

**Taille maximale du corps de la demande**

La taille maximale par défaut du corps de la demande est de 30 000 000 octets, soit environ 28,6 Mo. 

Pour remplacer la limite dans une application MVC ASP.NET Core, nous vous recommandons d’utiliser l’attribut [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) sur une méthode d’action :

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Voici un exemple qui montre comment configurer la contrainte pour l’application entière et chaque demande :

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Vous pouvez remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Une exception est levée si vous essayez de configurer la limite sur une demande dont l’application a commencé la lecture. Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.

**Débit données minimal du corps de la demande**

Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde. Si le débit est inférieur au minimum, la connexion expire. La période de grâce est la durée que Kestrel accorde au client pour augmenter sa vitesse de transmission jusqu’à la limite minimale ; pendant cette période, le débit n’est pas vérifié. La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.

Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.

Un débit minimal s’applique également à la réponse. Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface. 

Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Vous pouvez configurer les débits par demande dans l’intergiciel (middleware) :

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Pour plus d’informations sur les autres options Kestrel, consultez les classes suivantes :

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pour plus d’informations sur les options de , consultez [Classe KestrelServerOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Configuration de point de terminaison

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Par défaut, ASP.NET Core est lié à `http://localhost:5000`. Vous configurez des préfixes d’URL et des ports d’écoute pour Kestrel en appelant les méthodes `Listen` ou `ListenUnixSocket` sur `KestrelServerOptions`. (`UseUrls`, l’argument de ligne de commande `urls` et la variable d’environnement ASPNETCORE_URLS fonctionnent également, mais ils présentent les limitations indiquées [plus loin dans cet article](#useurls-limitations).)

**Lier à un socket TCP**

La méthode `Listen` lie à un socket TCP, et une expression lambda options vous permet de configurer un certificat SSL :

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Notez la façon dont cet exemple configure SSL pour un point de terminaison particulier à l’aide de [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Vous pouvez utiliser la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison particuliers.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Lier à un socket Unix**

Vous pouvez écouter sur un socket Unix pour améliorer les performances avec Nginx, comme illustré dans cet exemple :

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Si vous spécifiez le numéro de port 0, Kestrel lie dynamiquement à un port disponible. L’exemple suivant montre comment déterminer le port auquel Kestrel a effectué la liaison à l’exécution :

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Limitations de UseUrls**

Vous pouvez configurer des points de terminaison en appelant la méthode `UseUrls` ou à l’aide de l’argument de ligne de commande `urls` ou de la variable d’environnement ASPNETCORE_URLS. Ces méthodes sont utiles si vous souhaitez que votre code fonctionne avec les serveurs autres que Kestrel. Toutefois, tenez compte des limitations suivantes :

* Vous ne pouvez pas utiliser SSL avec ces méthodes.
* Si vous utilisez à la fois la méthode `Listen` et `UseUrls`, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.

**Configuration de point de terminaison pour IIS**

Si vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons que vous définissez en appelant `Listen` ou `UseUrls`. Pour plus d’informations, consultez [Introduction au module ASP.NET Core](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Par défaut, ASP.NET Core est lié à `http://localhost:5000`. Vous pouvez configurer les préfixes d’URL et les ports d’écoute de Kestrel en utilisant la méthode d’extension `UseUrls`, l’argument de ligne de commande `urls` ou le système de configuration ASP.NET Core. Pour plus d’informations sur ces méthodes, consultez [Hébergement](../../fundamentals/hosting.md). Pour plus d’informations sur le fonctionnement de la liaison d’URL quand vous utilisez IIS comme proxy inverse, consultez [Module ASP.NET Core](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Préfixes d’URL

Si vous appelez `UseUrls` ou utilisez l’argument de ligne de commande `urls` ou la variable d’environnement ASPNETCORE_URLS, les préfixes d’URL peuvent être dans l’un des formats suivants. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Seuls les préfixes d’URL HTTP sont valides ; Kestrel ne prend pas en charge SSL quand vous configurez les liaisons d’URL à l’aide de `UseUrls`.

* Adresse IPv4 avec numéro de port

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 est un cas spécial qui est lié à toutes les adresses IPv4.


* Adresse IPv6 avec numéro de port

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] est l’équivalent IPv6 de IPv4 0.0.0.0.


* Nom d’hôte avec numéro de port

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Les noms d’hôte, * et + ne sont pas spéciaux. Tout ce qui n’est pas une adresse IP reconnue ou « localhost » est lié à toutes les adresses IP IPv4 et IPv6. Si vous devez lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](httpsys.md) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.

* Nom «localhost » avec numéro de port ou adresse IP de bouclage avec numéro de port

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quand `localhost` est spécifié, Kestrel essaie de lier aux interfaces de bouclage IPv4 et IPv6. Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas. Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* Adresse IPv4 avec numéro de port

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 est un cas spécial qui est lié à toutes les adresses IPv4.


* Adresse IPv6 avec numéro de port

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] est l’équivalent IPv6 de IPv4 0.0.0.0.


* Nom d’hôte avec numéro de port

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Les noms d’hôte, \* et + ne sont pas spéciaux. Tout ce qui n’est pas une adresse IP reconnue ou « localhost » est lié à toutes les adresses IP IPv4 et IPv6. Si vous devez lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [WebListener](weblistener.md) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.

* Nom «localhost » avec numéro de port ou adresse IP de bouclage avec numéro de port

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quand `localhost` est spécifié, Kestrel essaie de lier aux interfaces de bouclage IPv4 et IPv6. Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas. Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement. 

* Socket UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Port 0**

Si vous spécifiez le numéro de port 0, Kestrel lie dynamiquement à un port disponible. La liaison au port 0 est autorisée pour n’importe quel nom d’hôte ou adresse IP à l’exception du nom `localhost`.

L’exemple suivant montre comment déterminer le port auquel Kestrel a effectué la liaison à l’exécution :

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Préfixes d’URL pour SSL**

Veillez à inclure les préfixes d’URL avec `https:` si vous appelez la méthode d’extension `UseHttps`, comme indiqué ci-dessous.

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> HTTPS et HTTP ne peuvent pas être hébergés sur le même port.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, reportez-vous aux ressources suivantes :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Exemple d’application pour 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Code source Kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Exemple d’application pour 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Code source Kestrel](https://github.com/aspnet/KestrelHttpServer)

---
