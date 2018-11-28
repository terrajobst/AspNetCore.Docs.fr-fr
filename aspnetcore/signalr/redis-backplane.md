---
title: Redis fond de panier de montée d’ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment configurer un fond de panier de Redis pour activer la montée en puissance pour une application ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c8b09c0d482da344b54d167c0c9757167eaa6186
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52452952"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>Configurer un fond de panier de Redis pour ASP.NET Core SignalR scale-out

Par [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), et [Nowak](https://github.com/tdykstra),

Cet article explique les aspects de SignalR spécifiques de la configuration d’un [Redis](https://redis.io/) serveur à utiliser pour la montée en puissance une application ASP.NET Core SignalR.

## <a name="set-up-a-redis-backplane"></a>Configurer un fond de panier de Redis

* Déployer un serveur Redis.

  À des fins de production, un fond de panier de Redis est recommandé uniquement pour l’infrastructure locale. Pour réduire la latence, le serveur Redis doit être dans le même centre de données que l’application de SignalR. Si votre application SignalR est en cours d’exécution dans le cloud Azure, nous recommandons le Service Azure SignalR au lieu de fond de panier de Redis. Vous pouvez utiliser le Service de Cache Redis Azure pour le développement et les environnements de test. Pour plus d'informations, reportez-vous aux ressources suivantes :

  * <xref:signalr/scale>
  * [Documentation redis](https://redis.io/)
  * [Documentation Cache Redis Azure](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* Dans l’application SignalR, installer le `Microsoft.AspNetCore.SignalR.Redis` package NuGet.

* Dans le `Startup.ConfigureServices` méthode, appelez `AddRedis` après `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Configurer les options en fonction des besoins :
 
  La plupart des options peuvent être définies dans la chaîne de connexion ou dans le [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objet. Options spécifiées dans `ConfigurationOptions` remplacent celles définies dans la chaîne de connexion.

  L’exemple suivant montre comment définir les options dans la `ConfigurationOptions` objet. Cet exemple ajoute un préfixe de canal afin que plusieurs applications peuvent partager la même instance Redis, comme expliqué dans l’étape suivante.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* Dans l’application SignalR, installer le `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package NuGet.

* Dans le `Startup.ConfigureServices` méthode, appelez `AddStackExchangeRedis` après `AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* Configurer les options en fonction des besoins :
 
  La plupart des options peuvent être définies dans la chaîne de connexion ou dans le [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objet. Options spécifiées dans `ConfigurationOptions` remplacent celles définies dans la chaîne de connexion.

  L’exemple suivant montre comment définir les options dans la `ConfigurationOptions` objet. Cet exemple ajoute un préfixe de canal afin que plusieurs applications peuvent partager la même instance Redis, comme expliqué dans l’étape suivante.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.

  Pour plus d’informations sur les options de Redis, consultez le [documentation StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Si vous utilisez un serveur Redis pour plusieurs applications SignalR, utiliser un préfixe de canal différent pour chaque application SignalR.

  Définir un préfixe de canal isole une seule application SignalR à partir d’autres personnes qui utilisent des préfixes de canal différent. Si vous n’affectez pas des préfixes différents, un message envoyé à partir d’une application à l’ensemble de ses propres clients passera à tous les clients de toutes les applications qui utilisent le serveur Redis comme un fond de panier.

* Configurer votre batterie de serveurs équilibrage du serveur logiciel des sessions rémanentes. Voici quelques exemples de documentation sur la marche à suivre :

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Redis des erreurs de serveur

Lorsqu’un serveur Redis tombe en panne, SignalR lève des exceptions qui indiquent les messages ne seront pas remis. Certains messages d’exception standard :

* *Échec d’écriture de messages*
* *Échec d’appel de méthode de concentrateur 'Nom_méthode'*
* *Échec de la connexion à Redis*

SignalR ne mémoires tampons de messages à envoyer les lorsque le serveur redevient opérationnel. Tous les messages envoyés quand le serveur Redis est en panne sont perdues.

SignalR se reconnecte automatiquement lorsque le serveur Redis est à nouveau disponible.

### <a name="custom-behavior-for-connection-failures"></a>Comportement personnalisé pour les échecs de connexion

Voici un exemple qui montre comment gérer les événements d’échec de connexion Redis.

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="clustering"></a>Clustering

Le clustering est une méthode pour la haute disponibilité à l’aide de plusieurs serveurs Redis. Clustering n’est pas officiellement pris en charge, mais elle peut fonctionner.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, reportez-vous aux ressources suivantes :

* <xref:signalr/scale>
* [Documentation redis](https://redis.io/documentation)
* [Documentation de StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/)
* [Documentation Cache Redis Azure](https://docs.microsoft.com/en-us/azure/redis-cache/)
