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
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a><span data-ttu-id="88a3e-103">Configurer un backplane ReDim pour ASP.NET Core SignalR montée en puissance parallèle</span><span class="sxs-lookup"><span data-stu-id="88a3e-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="88a3e-104">Par [Andrew Stanton-infirmière](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster)et [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="88a3e-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="88a3e-105">Cet article explique SignalRaspects spécifiques de la configuration d’un serveur [redims](https://redis.io/) à utiliser pour la montée en charge d’une application ASP.net Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="88a3e-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="88a3e-106">Configurer un backplane ReDim</span><span class="sxs-lookup"><span data-stu-id="88a3e-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="88a3e-107">Déployez un serveur ReDim.</span><span class="sxs-lookup"><span data-stu-id="88a3e-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="88a3e-108">Pour une utilisation en production, un backplane ReDim est recommandé uniquement lorsqu’il s’exécute dans le même centre de données que l’application SignalR.</span><span class="sxs-lookup"><span data-stu-id="88a3e-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="88a3e-109">Dans le cas contraire, la latence du réseau dégrade les performances.</span><span class="sxs-lookup"><span data-stu-id="88a3e-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="88a3e-110">Si votre application SignalR s’exécute dans le Cloud Azure, nous vous recommandons Azure SignalR service au lieu d’un backplane ReDim.</span><span class="sxs-lookup"><span data-stu-id="88a3e-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="88a3e-111">Vous pouvez utiliser le Cache Service Azure Redims pour les environnements de développement et de test.</span><span class="sxs-lookup"><span data-stu-id="88a3e-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="88a3e-112">Pour plus d'informations, voir les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="88a3e-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="88a3e-113">Documentation redims</span><span class="sxs-lookup"><span data-stu-id="88a3e-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="88a3e-114">Documentation du cache Redims Azure</span><span class="sxs-lookup"><span data-stu-id="88a3e-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="88a3e-115">Dans l’application SignalR, installez le package NuGet `Microsoft.AspNetCore.SignalR.Redis`.</span><span class="sxs-lookup"><span data-stu-id="88a3e-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>
* <span data-ttu-id="88a3e-116">Dans la méthode `Startup.ConfigureServices`, appelez `AddRedis` après `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="88a3e-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="88a3e-117">Configurez les options selon vos besoins :</span><span class="sxs-lookup"><span data-stu-id="88a3e-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="88a3e-118">La plupart des options peuvent être définies dans la chaîne de connexion ou dans l’objet [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="88a3e-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="88a3e-119">Les options spécifiées dans `ConfigurationOptions` substituent celles définies dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="88a3e-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="88a3e-120">L’exemple suivant montre comment définir les options de l’objet `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="88a3e-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="88a3e-121">Cet exemple ajoute un préfixe de canal afin que plusieurs applications puissent partager la même instance de ReDim, comme expliqué à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="88a3e-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="88a3e-122">Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="88a3e-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="88a3e-123">Dans l’application SignalR, installez l’un des packages NuGet suivants :</span><span class="sxs-lookup"><span data-stu-id="88a3e-123">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="88a3e-124">`Microsoft.AspNetCore.SignalR.StackExchangeRedis`-dépend de StackExchange. Redims 2. X.X.</span><span class="sxs-lookup"><span data-stu-id="88a3e-124">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="88a3e-125">Il s’agit du package recommandé pour ASP.NET Core 2,2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="88a3e-125">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="88a3e-126">`Microsoft.AspNetCore.SignalR.Redis`-dépend de StackExchange. Redims 1. X.X.</span><span class="sxs-lookup"><span data-stu-id="88a3e-126">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="88a3e-127">Ce package n’est pas inclus dans ASP.NET Core 3,0 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="88a3e-127">This package isn't included in ASP.NET Core 3.0 and later.</span></span>

* <span data-ttu-id="88a3e-128">Dans la méthode `Startup.ConfigureServices`, appelez <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:</span><span class="sxs-lookup"><span data-stu-id="88a3e-128">In the `Startup.ConfigureServices` method, call <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

 <span data-ttu-id="88a3e-129">Quand vous utilisez `Microsoft.AspNetCore.SignalR.Redis`, appelez <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.</span><span class="sxs-lookup"><span data-stu-id="88a3e-129">When using `Microsoft.AspNetCore.SignalR.Redis`, call <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.</span></span>

* <span data-ttu-id="88a3e-130">Configurez les options selon vos besoins :</span><span class="sxs-lookup"><span data-stu-id="88a3e-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="88a3e-131">La plupart des options peuvent être définies dans la chaîne de connexion ou dans l’objet [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="88a3e-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="88a3e-132">Les options spécifiées dans `ConfigurationOptions` substituent celles définies dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="88a3e-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="88a3e-133">L’exemple suivant montre comment définir les options de l’objet `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="88a3e-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="88a3e-134">Cet exemple ajoute un préfixe de canal afin que plusieurs applications puissent partager la même instance de ReDim, comme expliqué à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="88a3e-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

 <span data-ttu-id="88a3e-135">Quand vous utilisez `Microsoft.AspNetCore.SignalR.Redis`, appelez <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.</span><span class="sxs-lookup"><span data-stu-id="88a3e-135">When using `Microsoft.AspNetCore.SignalR.Redis`, call <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.</span></span>

  <span data-ttu-id="88a3e-136">Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="88a3e-136">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="88a3e-137">Pour plus d’informations sur les options Redims, consultez la [documentation sur StackExchange](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="88a3e-137">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="88a3e-138">Dans l’application SignalR, installez le package NuGet suivant :</span><span class="sxs-lookup"><span data-stu-id="88a3e-138">In the SignalR app, install the following NuGet package:</span></span>

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`
  
* <span data-ttu-id="88a3e-139">Dans la méthode `Startup.ConfigureServices`, appelez <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:</span><span class="sxs-lookup"><span data-stu-id="88a3e-139">In the `Startup.ConfigureServices` method, call <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```
  
* <span data-ttu-id="88a3e-140">Configurez les options selon vos besoins :</span><span class="sxs-lookup"><span data-stu-id="88a3e-140">Configure options as needed:</span></span>
 
  <span data-ttu-id="88a3e-141">La plupart des options peuvent être définies dans la chaîne de connexion ou dans l’objet [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="88a3e-141">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="88a3e-142">Les options spécifiées dans `ConfigurationOptions` substituent celles définies dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="88a3e-142">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="88a3e-143">L’exemple suivant montre comment définir les options de l’objet `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="88a3e-143">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="88a3e-144">Cet exemple ajoute un préfixe de canal afin que plusieurs applications puissent partager la même instance de ReDim, comme expliqué à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="88a3e-144">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="88a3e-145">Dans le code précédent, `options.Configuration` est initialisé avec tout ce qui a été spécifié dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="88a3e-145">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="88a3e-146">Pour plus d’informations sur les options Redims, consultez la [documentation sur StackExchange](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="88a3e-146">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="88a3e-147">Si vous utilisez un serveur Redims pour plusieurs applications de SignalR, utilisez un préfixe de canal différent pour chaque application SignalR.</span><span class="sxs-lookup"><span data-stu-id="88a3e-147">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="88a3e-148">La définition d’un préfixe de canal isole une SignalR application des autres qui utilisent des préfixes de canal différents.</span><span class="sxs-lookup"><span data-stu-id="88a3e-148">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="88a3e-149">Si vous n’assignez pas de préfixes différents, un message envoyé à partir d’une application à tous ses propres clients est envoyé à tous les clients de toutes les applications qui utilisent le serveur ReDim comme fond de panier.</span><span class="sxs-lookup"><span data-stu-id="88a3e-149">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="88a3e-150">Configurez le logiciel d’équilibrage de charge de votre batterie de serveurs pour les sessions rémanentes.</span><span class="sxs-lookup"><span data-stu-id="88a3e-150">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="88a3e-151">Voici quelques exemples de documentation sur la manière de procéder :</span><span class="sxs-lookup"><span data-stu-id="88a3e-151">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="88a3e-152">IIS</span><span class="sxs-lookup"><span data-stu-id="88a3e-152">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="88a3e-153">HAProxy</span><span class="sxs-lookup"><span data-stu-id="88a3e-153">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="88a3e-154">Nginx</span><span class="sxs-lookup"><span data-stu-id="88a3e-154">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="88a3e-155">pfSense</span><span class="sxs-lookup"><span data-stu-id="88a3e-155">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="88a3e-156">Erreurs du serveur ReDim</span><span class="sxs-lookup"><span data-stu-id="88a3e-156">Redis server errors</span></span>

<span data-ttu-id="88a3e-157">Lorsqu’un serveur ReDim tombe en panne, SignalR lève des exceptions qui indiquent que les messages ne sont pas remis.</span><span class="sxs-lookup"><span data-stu-id="88a3e-157">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="88a3e-158">Messages d’exception typiques :</span><span class="sxs-lookup"><span data-stu-id="88a3e-158">Some typical exception messages:</span></span>

* <span data-ttu-id="88a3e-159">*Échec de l’écriture du message*</span><span class="sxs-lookup"><span data-stu-id="88a3e-159">*Failed writing message*</span></span>
* <span data-ttu-id="88a3e-160">*Échec de l’appel de la méthode de concentrateur’MethodName'*</span><span class="sxs-lookup"><span data-stu-id="88a3e-160">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="88a3e-161">*Échec de la connexion aux ReDim*</span><span class="sxs-lookup"><span data-stu-id="88a3e-161">*Connection to Redis failed*</span></span>

SignalR<span data-ttu-id="88a3e-162"> ne met pas en mémoire tampon les messages pour les envoyer lorsque le serveur est sauvegardé.</span><span class="sxs-lookup"><span data-stu-id="88a3e-162"> doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="88a3e-163">Tous les messages envoyés pendant que le serveur Redims est défaillant sont perdus.</span><span class="sxs-lookup"><span data-stu-id="88a3e-163">Any messages sent while the Redis server is down are lost.</span></span>

SignalR<span data-ttu-id="88a3e-164"> se reconnecte automatiquement lorsque le serveur Redims est à nouveau disponible.</span><span class="sxs-lookup"><span data-stu-id="88a3e-164"> automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="88a3e-165">Comportement personnalisé pour les échecs de connexion</span><span class="sxs-lookup"><span data-stu-id="88a3e-165">Custom behavior for connection failures</span></span>

<span data-ttu-id="88a3e-166">Voici un exemple qui montre comment gérer les événements d’échec de connexion Redims.</span><span class="sxs-lookup"><span data-stu-id="88a3e-166">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="redis-clustering"></a><span data-ttu-id="88a3e-167">Clusters ReDim</span><span class="sxs-lookup"><span data-stu-id="88a3e-167">Redis Clustering</span></span>

<span data-ttu-id="88a3e-168">Le [clustering redims](https://redis.io/topics/cluster-spec) est une méthode permettant d’obtenir une haute disponibilité à l’aide de plusieurs serveurs ReDim.</span><span class="sxs-lookup"><span data-stu-id="88a3e-168">[Redis Clustering](https://redis.io/topics/cluster-spec) is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="88a3e-169">Le clustering n’est pas officiellement pris en charge, mais il peut fonctionner.</span><span class="sxs-lookup"><span data-stu-id="88a3e-169">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="88a3e-170">Étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="88a3e-170">Next steps</span></span>

<span data-ttu-id="88a3e-171">Pour plus d'informations, voir les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="88a3e-171">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="88a3e-172">Documentation redims</span><span class="sxs-lookup"><span data-stu-id="88a3e-172">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="88a3e-173">Documentation sur StackExchange ReDim</span><span class="sxs-lookup"><span data-stu-id="88a3e-173">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="88a3e-174">Documentation du cache Redims Azure</span><span class="sxs-lookup"><span data-stu-id="88a3e-174">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)
