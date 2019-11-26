---
title: Panier ReDim pour la montée en charge de ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment configurer un backplane ReDim pour permettre la montée en charge d’une application ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/redis-backplane
ms.openlocfilehash: 0461fc6a212ba78111bc2054cca74951721c5820
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289040"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a>Configurer un backplane ReDim pour ASP.NET Core SignalR montée en puissance parallèle

Par [Andrew Stanton-infirmière](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster)et [Tom Dykstra](https://github.com/tdykstra),

Cet article explique SignalRaspects spécifiques de la configuration d’un serveur [redims](https://redis.io/) à utiliser pour la montée en charge d’une application ASP.net Core SignalR.

## <a name="set-up-a-redis-backplane"></a>Configurer un backplane ReDim

* Déployez un serveur ReDim.

  > [!IMPORTANT] 
  > Pour une utilisation en production, un backplane ReDim est recommandé uniquement lorsqu’il s’exécute dans le même centre de données que l’application SignalR. Dans le cas contraire, la latence du réseau dégrade les performances. Si votre application SignalR s’exécute dans le Cloud Azure, nous vous recommandons Azure SignalR service au lieu d’un backplane ReDim. Vous pouvez utiliser le Cache Service Azure Redims pour les environnements de développement et de test.

  Pour plus d'informations, voir les ressources suivantes :

  * <xref:signalr/scale>
  * [Documentation redims](https://redis.io/)
  * [Documentation du cache Redims Azure](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* Dans l’application SignalR, installez le package NuGet `Microsoft.AspNetCore.SignalR.Redis`.
* Dans la méthode `Startup.ConfigureServices`, appelez `AddRedis` après `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Configurez les options selon vos besoins :
 
  La plupart des options peuvent être définies dans la chaîne de connexion ou dans l’objet [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) . Les options spécifiées dans `ConfigurationOptions` substituent celles définies dans la chaîne de connexion.

  L’exemple suivant montre comment définir les options de l’objet `ConfigurationOptions`. Cet exemple ajoute un préfixe de canal afin que plusieurs applications puissent partager la même instance de ReDim, comme expliqué à l’étape suivante.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* Dans l’application SignalR, installez l’un des packages NuGet suivants :

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`-dépend de StackExchange. Redims 2. X.X. Il s’agit du package recommandé pour ASP.NET Core 2,2 et versions ultérieures.
  * `Microsoft.AspNetCore.SignalR.Redis`-dépend de StackExchange. Redims 1. X.X. Ce package n’est pas inclus dans ASP.NET Core 3,0 et versions ultérieures.

* Dans la méthode `Startup.ConfigureServices`, appelez <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

 Quand vous utilisez `Microsoft.AspNetCore.SignalR.Redis`, appelez <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.

* Configurez les options selon vos besoins :
 
  La plupart des options peuvent être définies dans la chaîne de connexion ou dans l’objet [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) . Les options spécifiées dans `ConfigurationOptions` substituent celles définies dans la chaîne de connexion.

  L’exemple suivant montre comment définir les options de l’objet `ConfigurationOptions`. Cet exemple ajoute un préfixe de canal afin que plusieurs applications puissent partager la même instance de ReDim, comme expliqué à l’étape suivante.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

 Quand vous utilisez `Microsoft.AspNetCore.SignalR.Redis`, appelez <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.

  Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.

  Pour plus d’informations sur les options Redims, consultez la [documentation sur StackExchange](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

* Dans l’application SignalR, installez le package NuGet suivant :

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`
  
* Dans la méthode `Startup.ConfigureServices`, appelez <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```
  
* Configurez les options selon vos besoins :
 
  La plupart des options peuvent être définies dans la chaîne de connexion ou dans l’objet [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) . Les options spécifiées dans `ConfigurationOptions` substituent celles définies dans la chaîne de connexion.

  L’exemple suivant montre comment définir les options de l’objet `ConfigurationOptions`. Cet exemple ajoute un préfixe de canal afin que plusieurs applications puissent partager la même instance de ReDim, comme expliqué à l’étape suivante.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.

  Pour plus d’informations sur les options Redims, consultez la [documentation sur StackExchange](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Si vous utilisez un serveur Redims pour plusieurs applications de SignalR, utilisez un préfixe de canal différent pour chaque application SignalR.

  La définition d’un préfixe de canal isole une SignalR application des autres qui utilisent des préfixes de canal différents. Si vous n’assignez pas de préfixes différents, un message envoyé à partir d’une application à tous ses propres clients est envoyé à tous les clients de toutes les applications qui utilisent le serveur ReDim comme fond de panier.

* Configurez le logiciel d’équilibrage de charge de votre batterie de serveurs pour les sessions rémanentes. Voici quelques exemples de documentation sur la manière de procéder :

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Erreurs du serveur ReDim

Lorsqu’un serveur ReDim tombe en panne, SignalR lève des exceptions qui indiquent que les messages ne sont pas remis. Messages d’exception typiques :

* *Échec de l’écriture du message*
* *Échec de l’appel de la méthode de concentrateur’MethodName'*
* *Échec de la connexion aux ReDim*

SignalR ne met pas en mémoire tampon les messages pour les envoyer lorsque le serveur est sauvegardé. Tous les messages envoyés pendant que le serveur Redims est défaillant sont perdus.

SignalR se reconnecte automatiquement lorsque le serveur Redims est à nouveau disponible.

### <a name="custom-behavior-for-connection-failures"></a>Comportement personnalisé pour les échecs de connexion

Voici un exemple qui montre comment gérer les événements d’échec de connexion Redims.

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

## <a name="redis-clustering"></a>Clusters ReDim

Le [clustering redims](https://redis.io/topics/cluster-spec) est une méthode permettant d’obtenir une haute disponibilité à l’aide de plusieurs serveurs ReDim. Le clustering n’est pas officiellement pris en charge, mais il peut fonctionner.

## <a name="next-steps"></a>Étapes suivantes :

Pour plus d'informations, voir les ressources suivantes :

* <xref:signalr/scale>
* [Documentation redims](https://redis.io/documentation)
* [Documentation sur StackExchange ReDim](https://stackexchange.github.io/StackExchange.Redis/)
* [Documentation du cache Redims Azure](https://docs.microsoft.com/azure/redis-cache/)
