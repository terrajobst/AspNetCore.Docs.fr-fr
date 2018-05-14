---
title: Implémentation du serveur web Kestrel dans ASP.NET Core
author: tdykstra
description: Découvrez plus d’informations sur Kestrel, le serveur web multiplateforme pour ASP.NET Core basé sur libuv.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 1709d26f5dfe40d178da70c286d328982f2c39a0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Implémentation du serveur web Kestrel dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) et [Stephen Halter](https://twitter.com/halter73)

[Kestrel](xref:fundamentals/servers/index) est un serveur web multiplateforme pour ASP.NET Core basé sur [libuv](https://github.com/libuv/libuv), bibliothèque d’E/S asynchrones multiplateforme. Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.

Kestrel prend en charge les fonctionnalités suivantes :

* HTTPS
* Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)
* Sockets UNIX pour des performances élevées derrière Nginx

Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Quand utiliser Kestrel avec un proxy inverse ?

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Vous pouvez utiliser uniquement Kestrel ou l’associer à un *serveur proxy inverse*, par exemple IIS, Nginx ou Apache. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

Nous vous recommandons d’utiliser Kestrel avec un serveur proxy inverse, sauf si Kestrel est exposé seulement à un réseau interne.

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si une application accepte seulement les requêtes provenant d’un réseau interne, Kestrel peut être utilisé directement comme serveur de l’application.

![Kestrel communique directement avec votre réseau interne](kestrel/_static/kestrel-to-internal.png)

Si vous exposez votre application à Internet, vous devez utiliser IIS, Nginx ou Apache comme *serveur proxy inverse*. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

Un proxy inverse est requis pour les déploiements Edge (exposés au trafic en provenance d’Internet) pour des raisons de sécurité. Les versions 1.x de Kestrel ne disposent pas d’un ensemble complet de défenses contre les attaques, comme des délais d’expiration, des limites de taille et des limites du nombre de connexions simultanées.

* * *

Il existe un scénario de proxy inverse quand plusieurs applications partageant la même adresse IP et le même port s’exécutent sur un même serveur. Kestrel ne prend pas en charge ce scénario, car le partage de la même adresse IP et du même port entre plusieurs processus n’est pas pris en charge par Kestrel. Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit l’en-tête de l’hôte des requêtes. Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.

Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix :

* Il peut limiter la surface publique exposée des applications qu’il héberge.
* Il fournit une couche supplémentaire de configuration et de défense.
* Il peut mieux s’intégrer à l’infrastructure existante.
* Il simplifie l’équilibrage de charge et la configuration de SSL. Seul le serveur proxy inverse nécessite un certificat SSL : ce serveur peut donc communiquer avec vos serveurs d’applications sur le réseau interne avec du HTTP normal.

> [!WARNING]
> Si vous n’utilisez pas un proxy inverse avec le filtrage d’hôtes activé, le [filtrage d’hôtes](#host-filtering) doit être activé.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Comment utiliser Kestrel dans les applications ASP.NET Core

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Le package [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

Les modèles de projet ASP.NET Core utilisent Kestrel par défaut. Dans *Program.cs*, le code du modèle appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), qui appelle [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) en arrière-plan.

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Installez le package NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Appelez la méthode d’extension [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) sur [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) dans la méthode `Main`, en spécifiant les [options Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) dont vous avez besoin, comme indiqué dans la section suivante.

[!code-csharp[](kestrel/samples/1.x/Program.cs?name=snippet_Main&highlight=13-19)]

* * *

### <a name="kestrel-options"></a>Options Kestrel

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet. Vous pouvez personnaliser quelques limites importantes :

* Nombre maximale de connexions client
* Taille maximale du corps de la requête
* Débit données minimal du corps de la requête

Définissez ces limites et d’autres contraintes sur la propriété [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) de la classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions). La propriété `Limits` conserve une instance de la classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).

**Nombre maximal de connexions client**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=3-4)]

Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket). Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.

Le nombre maximal de connexions est illimité (null) par défaut.

**Taille maximale du corps de la demande**

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.

Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) sur une méthode d’action :

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=5)]

Vous pouvez remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=3-4)]

Une exception est levée vous tentez de configurer la limite sur une requête une fois que l’application a commencé à la lire. Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.

**Débit données minimal du corps de la demande**

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde. Si le débit est inférieur au minimum, la connexion expire. La période de grâce est la durée que Kestrel accorde au client pour augmenter sa vitesse de transmission jusqu’à la limite minimale ; pendant cette période, le débit n’est pas vérifié. La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.

Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.

Un débit minimal s’applique également à la réponse. Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.

Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=6-9)]

Vous pouvez configurer les débits par demande dans l’intergiciel (middleware) :

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=5-8)]

Pour plus d’informations sur les autres options et limites de Kestrel, consultez :

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Pour plus d’informations sur les options et limites de Kestrel, consultez :

* [KestrelServerOptions, classe](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

* * *

### <a name="endpoint-configuration"></a>Configuration de point de terminaison

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
Par défaut, ASP.NET Core est lié à `http://localhost:5000`. Appelez les méthodes [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) sur [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) pour configurer les préfixes d’URL et les ports pour Kestrel. `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section.

La clé de configuration d’hôte `urls` doit provenir de la configuration de l’hôte, et non pas de la configuration de l’application. L’ajout d’une clé et d’une valeur de `urls` à *appsettings.json* n’affecte pas la configuration de l’hôte, car l’hôte est complètement initialisé au moment où la configuration est lue à partir du fichier de configuration. Cependant, une clé `urls` dans *appsettings.json* peut être utilisée avec [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) sur le créateur d’hôte pour configurer l’hôte :

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]

Par défaut, ASP.NET Core se lie à :

* `http://localhost:5000`
* `https://localhost:5001` (quand un certificat de développement local est présent)

Un certificat de développement est créé :

* Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.
* [L’outil dev-certs](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) est utilisé pour créer un certificat.

Vous devez accorder à certains navigateurs l’autorisation explicite d’approuver le certificat de développement local.

Les modèles de projet ASP.NET Core 2.1 et ultérieur configurent les applications pour une exécution par défaut sur HTTPS et pour l’inclusion de [la redirection HTTPS et de la prise en charge de HSTS](xref:security/enforcing-ssl).

Appelez les méthodes [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) sur [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) pour configurer les préfixes d’URL et les ports pour Kestrel.

`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).

Configuration de `KestrelServerOptions` pour ASP.NET Core 2.1 :

**ConfigureEndpointDefaults(Action<ListenOptions>)**  
Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié. Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.

**ConfigureHttpsDefaults(Action<HttpsConnectionAdapterOptions>)**  
Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS. Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.

**Configure(IConfiguration)**  
Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration). La configuration doit être limitée à la section de configuration pour Kestrel.

**ListenOptions.UseHttps**  
Configure Kestrel pour l’utilisation de HTTPS.

Extensions de `ListenOptions.UseHttps` :

* `UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut. Lève une exception si aucun certificat par défaut n’est configuré.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

Paramètres de `ListenOptions.UseHttps` :

* `filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.
* `password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.
* `configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`. Retourne l'`ListenOptions`.
* `storeName` est le magasin de certificats à partir duquel charger le certificat.
* `subject` est le nom du sujet du certificat.
* `allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.
* `location` est l’emplacement du magasin à partir duquel charger le certificat.
* `serverCertificate` est le certificat X.509.

En production, HTTPS doit être explicitement configuré. Au minimum, un certificat par défaut doit être fourni.

Configurations prises en charge décrites dans la suite :

* Pas de configuration
* Remplacer le certificat par défaut dans la configuration
* Changer les valeurs par défaut dans le code

*Pas de configuration*

Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).

Spécifiez les URL avec :

* La variable d’environnement `ASPNETCORE_URLS`.
* L’argument de ligne de commande `--urls`.
* La clé de configuration d’hôte `urls`.
* La méthode d’extension `UseUrls`.

Pour plus d’informations, consultez [URL de serveur](xref:fundamentals/hosting#server-urls) et [Remplacement de la configuration](xref:fundamentals/hosting#overriding-configuration).

La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible). Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000;http://localhost:8001"`).

*Remplacer le certificat par défaut dans la configuration*

Par défaut, [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) appelle `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel. Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel. Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.

Dans l’exemple de fichier *appsettings.json* suivant :

* Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).
* Les points de terminaison HTTPS qui ne spécifient pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) utilise comme alternative de secours le certificat défini sous **Certificats** > **Par défaut**  ou bien le certificat de développement.

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats. Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Notes de schéma :

* Les noms des points de terminaison ne respectent pas la casse. Par exemple, `HTTPS` et `Https` sont valides.
* Le paramètre `Url` est obligatoire pour chaque point de terminaison. Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.
* Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter. Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.
* La section `Certificate` est facultative. Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées. Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.
* La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.
* Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, options => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Vous pouvez également accéder directement à `KestrelServerOptions.ConfigurationLoader` pour continuer à boucler sur le chargeur existant, comme celui fourni par `WebHost.CreatedDeafaultBuilder`.

* La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint`, ce qui permet de lire les paramètres personnalisés.
* Plusieurs configurations peuvent être chargées en rappelant `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` avec une autre section. Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes. Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.
* `KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement. Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.

*Changer les valeurs par défaut dans le code*

`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent. `ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Prise en charge de SNI par Kestrel*

[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port. Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct. Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.

Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`. Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.

La prise en charge de SNI nécessite une exécution sur le framework cible `netcoreapp2.1`. Sur `netcoreapp2.0` et `net461`, le rappel est appelé, mais `name` est toujours `null`. `name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```
::: moniker-end

**Lier à un socket TCP**

La méthode [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat SSL :

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Notez la façon dont cet exemple configure SSL pour un point de terminaison particulier à l’aide de [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**Lier à un socket Unix**

Écoutez sur un socket Unix avec [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) pour améliorer les performances avec Nginx, comme illustré dans cet exemple :

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible. L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Port0&highlight=3)]

Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :

```console
Now listening on: http://127.0.0.1:48508
```

**UseUrls, argument de ligne de commande --urls, clé de configuration d’hôte urls et limitations de la variable d’environnement ASPNETCORE_URLS**

Configurez des points de terminaison avec les approches suivantes :

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* Arguments de ligne de commande `--urls`
* Clé de configuration d’hôte `urls`.
* Variable d’environnement `ASPNETCORE_URLS`

Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel. Toutefois, tenez compte des limitations suivantes :

* SSL ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).
* Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.

**Configuration des points de terminaison IIS**

Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`. Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Par défaut, ASP.NET Core est lié à `http://localhost:5000`. Configurez les préfixes d’URL et les ports pour Kestrel avec :

* La méthode d’extension [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)
* Arguments de ligne de commande `--urls`
* La clé de configuration d’hôte `urls`
* Le système de configuration d’ASP.NET Core, notamment la variable d’environnement `ASPNETCORE_URLS`

Pour plus d’informations sur ces méthodes, consultez [Hébergement](xref:fundamentals/hosting).

**Configuration des points de terminaison IIS**

Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons définies par `UseUrls`. Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

* * *

### <a name="url-prefixes"></a>Préfixes d’URL

Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Seuls les préfixes d’URL HTTP sont valides. Kestrel ne prend pas en charge SSL lors de la configuration de liaisons d’URL avec `UseUrls`.

* Adresse IPv4 avec numéro de port

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.


* Adresse IPv6 avec numéro de port

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.


* Nom d’hôte avec numéro de port

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Les noms d’hôte, `*` et `+` ne sont pas spéciaux. Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6. Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.

  > [!WARNING]
  > Si vous n’utilisez pas un proxy inverse avec le filtrage d’hôtes activé, activez le [filtrage d’hôtes](#host-filtering).

* Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6. Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas. Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Adresse IPv4 avec numéro de port

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.

* Adresse IPv6 avec numéro de port

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.

* Nom d’hôte avec numéro de port

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Les noms d’hôte, `*` et `+` ne sont pas spéciaux. Tout ce qui n’est pas une adresse IP reconnue ou `localhost` est lié à toutes les adresses IP IPv4 et IPv6. Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [WebListener](xref:fundamentals/servers/weblistener) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.

* Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6. Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas. Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.

* Socket UNIX

  ```
  http://unix:/run/dan-live.sock
  ```

**Port 0**

Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible. La liaison au port `0` est autorisée pour n’importe quel nom d’hôte ou adresse IP, excepté pour `localhost`.

Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :

```console
Now listening on: http://127.0.0.1:48508
```

**Préfixes d’URL pour SSL**

Si vous appelez la méthode d’extension `UseHttps`, veillez à inclure les préfixes d’URL avec `https:` :

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

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

* * *

## <a name="host-filtering"></a>Filtrage d’hôtes

Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte. L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage. Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques. Aucune de ces informations n’est utilisée pour valider les en-têtes `Host` des requêtes.

Il y a deux manières d’y remédier :

* Un hôte derrière un proxy inverse avec filtrage des en-têtes d’hôte. Il s’agissait du seul scénario pris en charge pour Kestrel dans ASP.NET Core 1.x.
* Utilisez un middleware pour filtrer les requêtes d’après l’en-tête `Host`. Voici un exemple de middleware :

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Inscrivez le `HostFilteringMiddleware` précédent dans `Startup.Configure`. Notez que [l’ordre d’inscription des middlewares](xref:fundamentals/middleware/index#ordering) est important. L’inscription doit se produire immédiatement après l’inscription du middleware de diagnostic (par exemple `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

Le middleware précédent attend une clé `AllowedHosts` dans *appsettings.\<nom_environnement>.json*. La valeur de cette clé est une liste délimitée par des points-virgules des noms d’hôte sans les numéros de port. Ajoutez la paire clé-valeur `AllowedHosts` dans *appsettings.Production.json* :

```json
{
  "AllowedHosts": "example.com"
}
```

*appsettings.Development.json* (fichier de configuration de localhost) :

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Appliquer HTTPS](xref:security/enforcing-ssl)
* [Code source Kestrel](https://github.com/aspnet/KestrelHttpServer)
