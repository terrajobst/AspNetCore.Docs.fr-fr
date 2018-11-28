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
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="538d0-103">Configurer un fond de panier de Redis pour ASP.NET Core SignalR scale-out</span><span class="sxs-lookup"><span data-stu-id="538d0-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="538d0-104">Par [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), et [Nowak](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="538d0-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="538d0-105">Cet article explique les aspects de SignalR spécifiques de la configuration d’un [Redis](https://redis.io/) serveur à utiliser pour la montée en puissance une application ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="538d0-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="538d0-106">Configurer un fond de panier de Redis</span><span class="sxs-lookup"><span data-stu-id="538d0-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="538d0-107">Déployer un serveur Redis.</span><span class="sxs-lookup"><span data-stu-id="538d0-107">Deploy a Redis server.</span></span>

  <span data-ttu-id="538d0-108">À des fins de production, un fond de panier de Redis est recommandé uniquement pour l’infrastructure locale.</span><span class="sxs-lookup"><span data-stu-id="538d0-108">For production use, a Redis backplane is recommended only for on-premises infrastructure.</span></span> <span data-ttu-id="538d0-109">Pour réduire la latence, le serveur Redis doit être dans le même centre de données que l’application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="538d0-109">To minimize latency, the Redis server should be in the same data center as the SignalR app.</span></span> <span data-ttu-id="538d0-110">Si votre application SignalR est en cours d’exécution dans le cloud Azure, nous recommandons le Service Azure SignalR au lieu de fond de panier de Redis.</span><span class="sxs-lookup"><span data-stu-id="538d0-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="538d0-111">Vous pouvez utiliser le Service de Cache Redis Azure pour le développement et les environnements de test.</span><span class="sxs-lookup"><span data-stu-id="538d0-111">You can use the Azure Redis Cache Service for development and test environments.</span></span> <span data-ttu-id="538d0-112">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="538d0-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="538d0-113">Documentation redis</span><span class="sxs-lookup"><span data-stu-id="538d0-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="538d0-114">Documentation Cache Redis Azure</span><span class="sxs-lookup"><span data-stu-id="538d0-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="538d0-115">Dans l’application SignalR, installer le `Microsoft.AspNetCore.SignalR.Redis` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="538d0-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>

* <span data-ttu-id="538d0-116">Dans le `Startup.ConfigureServices` méthode, appelez `AddRedis` après `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="538d0-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="538d0-117">Configurer les options en fonction des besoins :</span><span class="sxs-lookup"><span data-stu-id="538d0-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="538d0-118">La plupart des options peuvent être définies dans la chaîne de connexion ou dans le [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objet.</span><span class="sxs-lookup"><span data-stu-id="538d0-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="538d0-119">Options spécifiées dans `ConfigurationOptions` remplacent celles définies dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="538d0-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="538d0-120">L’exemple suivant montre comment définir les options dans la `ConfigurationOptions` objet.</span><span class="sxs-lookup"><span data-stu-id="538d0-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="538d0-121">Cet exemple ajoute un préfixe de canal afin que plusieurs applications peuvent partager la même instance Redis, comme expliqué dans l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="538d0-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="538d0-122">Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="538d0-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="538d0-123">Dans l’application SignalR, installer le `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="538d0-123">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.StackExchangeRedis` NuGet package.</span></span>

* <span data-ttu-id="538d0-124">Dans le `Startup.ConfigureServices` méthode, appelez `AddStackExchangeRedis` après `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="538d0-124">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="538d0-125">Configurer les options en fonction des besoins :</span><span class="sxs-lookup"><span data-stu-id="538d0-125">Configure options as needed:</span></span>
 
  <span data-ttu-id="538d0-126">La plupart des options peuvent être définies dans la chaîne de connexion ou dans le [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objet.</span><span class="sxs-lookup"><span data-stu-id="538d0-126">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="538d0-127">Options spécifiées dans `ConfigurationOptions` remplacent celles définies dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="538d0-127">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="538d0-128">L’exemple suivant montre comment définir les options dans la `ConfigurationOptions` objet.</span><span class="sxs-lookup"><span data-stu-id="538d0-128">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="538d0-129">Cet exemple ajoute un préfixe de canal afin que plusieurs applications peuvent partager la même instance Redis, comme expliqué dans l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="538d0-129">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="538d0-130">Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="538d0-130">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="538d0-131">Pour plus d’informations sur les options de Redis, consultez le [documentation StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="538d0-131">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="538d0-132">Si vous utilisez un serveur Redis pour plusieurs applications SignalR, utiliser un préfixe de canal différent pour chaque application SignalR.</span><span class="sxs-lookup"><span data-stu-id="538d0-132">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="538d0-133">Définir un préfixe de canal isole une seule application SignalR à partir d’autres personnes qui utilisent des préfixes de canal différent.</span><span class="sxs-lookup"><span data-stu-id="538d0-133">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="538d0-134">Si vous n’affectez pas des préfixes différents, un message envoyé à partir d’une application à l’ensemble de ses propres clients passera à tous les clients de toutes les applications qui utilisent le serveur Redis comme un fond de panier.</span><span class="sxs-lookup"><span data-stu-id="538d0-134">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="538d0-135">Configurer votre batterie de serveurs équilibrage du serveur logiciel des sessions rémanentes.</span><span class="sxs-lookup"><span data-stu-id="538d0-135">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="538d0-136">Voici quelques exemples de documentation sur la marche à suivre :</span><span class="sxs-lookup"><span data-stu-id="538d0-136">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="538d0-137">IIS</span><span class="sxs-lookup"><span data-stu-id="538d0-137">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="538d0-138">HAProxy</span><span class="sxs-lookup"><span data-stu-id="538d0-138">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="538d0-139">Nginx</span><span class="sxs-lookup"><span data-stu-id="538d0-139">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="538d0-140">pfSense</span><span class="sxs-lookup"><span data-stu-id="538d0-140">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="538d0-141">Redis des erreurs de serveur</span><span class="sxs-lookup"><span data-stu-id="538d0-141">Redis server errors</span></span>

<span data-ttu-id="538d0-142">Lorsqu’un serveur Redis tombe en panne, SignalR lève des exceptions qui indiquent les messages ne seront pas remis.</span><span class="sxs-lookup"><span data-stu-id="538d0-142">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="538d0-143">Certains messages d’exception standard :</span><span class="sxs-lookup"><span data-stu-id="538d0-143">Some typical exception messages:</span></span>

* <span data-ttu-id="538d0-144">*Échec d’écriture de messages*</span><span class="sxs-lookup"><span data-stu-id="538d0-144">*Failed writing message*</span></span>
* <span data-ttu-id="538d0-145">*Échec d’appel de méthode de concentrateur 'Nom_méthode'*</span><span class="sxs-lookup"><span data-stu-id="538d0-145">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="538d0-146">*Échec de la connexion à Redis*</span><span class="sxs-lookup"><span data-stu-id="538d0-146">*Connection to Redis failed*</span></span>

<span data-ttu-id="538d0-147">SignalR ne mémoires tampons de messages à envoyer les lorsque le serveur redevient opérationnel.</span><span class="sxs-lookup"><span data-stu-id="538d0-147">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="538d0-148">Tous les messages envoyés quand le serveur Redis est en panne sont perdues.</span><span class="sxs-lookup"><span data-stu-id="538d0-148">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="538d0-149">SignalR se reconnecte automatiquement lorsque le serveur Redis est à nouveau disponible.</span><span class="sxs-lookup"><span data-stu-id="538d0-149">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="538d0-150">Comportement personnalisé pour les échecs de connexion</span><span class="sxs-lookup"><span data-stu-id="538d0-150">Custom behavior for connection failures</span></span>

<span data-ttu-id="538d0-151">Voici un exemple qui montre comment gérer les événements d’échec de connexion Redis.</span><span class="sxs-lookup"><span data-stu-id="538d0-151">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="clustering"></a><span data-ttu-id="538d0-152">Clustering</span><span class="sxs-lookup"><span data-stu-id="538d0-152">Clustering</span></span>

<span data-ttu-id="538d0-153">Le clustering est une méthode pour la haute disponibilité à l’aide de plusieurs serveurs Redis.</span><span class="sxs-lookup"><span data-stu-id="538d0-153">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="538d0-154">Clustering n’est pas officiellement pris en charge, mais elle peut fonctionner.</span><span class="sxs-lookup"><span data-stu-id="538d0-154">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="538d0-155">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="538d0-155">Next steps</span></span>

<span data-ttu-id="538d0-156">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="538d0-156">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="538d0-157">Documentation redis</span><span class="sxs-lookup"><span data-stu-id="538d0-157">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="538d0-158">Documentation de StackExchange Redis</span><span class="sxs-lookup"><span data-stu-id="538d0-158">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="538d0-159">Documentation Cache Redis Azure</span><span class="sxs-lookup"><span data-stu-id="538d0-159">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
