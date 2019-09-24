---
title: Journalisation et diagnostics dans gRPC sur .NET
author: jamesnk
description: Découvrez Comment collecter des diagnostics à partir de votre application gRPC sur .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: ce6ad96d9e26c9cd3844093536745f8f9bea4a76
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204344"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="2bb98-103">Journalisation et diagnostics dans gRPC sur .NET</span><span class="sxs-lookup"><span data-stu-id="2bb98-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="2bb98-104">Par [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="2bb98-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="2bb98-105">Cet article fournit des conseils pour la collecte des diagnostics à partir de votre application gRPC afin de vous aider à résoudre les problèmes.</span><span class="sxs-lookup"><span data-stu-id="2bb98-105">This article provides guidance for gathering diagnostics from your gRPC app to help troubleshoot issues.</span></span>

## <a name="grpc-services-logging"></a><span data-ttu-id="2bb98-106">journalisation des services gRPC</span><span class="sxs-lookup"><span data-stu-id="2bb98-106">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="2bb98-107">Les journaux côté serveur peuvent contenir des informations sensibles de votre application.</span><span class="sxs-lookup"><span data-stu-id="2bb98-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="2bb98-108">**Ne jamais** poster des journaux bruts à partir d’applications de production vers des forums publics tels que github.</span><span class="sxs-lookup"><span data-stu-id="2bb98-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="2bb98-109">Étant donné que les services gRPC sont hébergés sur ASP.NET Core, ils utilisent le système de journalisation ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bb98-109">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="2bb98-110">Dans la configuration par défaut, gRPC enregistre très peu d’informations, mais cela peut être configuré.</span><span class="sxs-lookup"><span data-stu-id="2bb98-110">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="2bb98-111">Pour plus d’informations sur la configuration de la journalisation des ASP.NET Core, consultez la documentation sur [ASP.net Core Logging](xref:fundamentals/logging/index#configuration) .</span><span class="sxs-lookup"><span data-stu-id="2bb98-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="2bb98-112">gRPC ajoute des journaux sous `Grpc` la catégorie.</span><span class="sxs-lookup"><span data-stu-id="2bb98-112">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="2bb98-113">Pour activer les journaux détaillés à partir de `Grpc` gRPC, configurez les préfixes sur le `Debug` niveau dans votre fichier *appSettings. JSON* en ajoutant les éléments `Logging`suivants à la `LogLevel` sous-section dans :</span><span class="sxs-lookup"><span data-stu-id="2bb98-113">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7)]

<span data-ttu-id="2bb98-114">Vous pouvez également configurer cette configuration dans Startup.cs `ConfigureLogging`avec :</span><span class="sxs-lookup"><span data-stu-id="2bb98-114">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5)]

<span data-ttu-id="2bb98-115">Si vous n’utilisez pas la configuration basée sur JSON, définissez la valeur de configuration suivante dans votre système de configuration :</span><span class="sxs-lookup"><span data-stu-id="2bb98-115">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="2bb98-116">Consultez la documentation de votre système de configuration pour déterminer comment spécifier les valeurs de configuration imbriquées.</span><span class="sxs-lookup"><span data-stu-id="2bb98-116">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="2bb98-117">Par exemple, lors de l’utilisation de variables `_` d’environnement, deux caractères sont `:` utilisés à la place `Logging__LogLevel__Grpc`de (par exemple,).</span><span class="sxs-lookup"><span data-stu-id="2bb98-117">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="2bb98-118">Nous vous recommandons d' `Debug` utiliser le niveau lorsque vous collectez des diagnostics plus détaillés pour votre application.</span><span class="sxs-lookup"><span data-stu-id="2bb98-118">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="2bb98-119">Le `Trace` niveau produit des diagnostics de bas niveau et est rarement nécessaire pour diagnostiquer les problèmes dans votre application.</span><span class="sxs-lookup"><span data-stu-id="2bb98-119">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

### <a name="sample-logging-output"></a><span data-ttu-id="2bb98-120">Exemple de sortie de la journalisation</span><span class="sxs-lookup"><span data-stu-id="2bb98-120">Sample logging output</span></span>

<span data-ttu-id="2bb98-121">Voici un exemple de sortie de console au `Debug` niveau d’un service gRPC :</span><span class="sxs-lookup"><span data-stu-id="2bb98-121">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

## <a name="access-server-side-logs"></a><span data-ttu-id="2bb98-122">Accéder aux journaux côté serveur</span><span class="sxs-lookup"><span data-stu-id="2bb98-122">Access server-side logs</span></span>

<span data-ttu-id="2bb98-123">La façon dont vous accédez aux journaux côté serveur dépend de l’environnement dans lequel vous exécutez.</span><span class="sxs-lookup"><span data-stu-id="2bb98-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app"></a><span data-ttu-id="2bb98-124">En tant qu’application console</span><span class="sxs-lookup"><span data-stu-id="2bb98-124">As a console app</span></span>

<span data-ttu-id="2bb98-125">Si vous exécutez dans une application console, l’enregistreur d’événements de [console](xref:fundamentals/logging/index#console-provider) doit être activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="2bb98-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="2bb98-126">les journaux gRPC s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="2bb98-126">gRPC logs will appear in the console.</span></span>

### <a name="other-environments"></a><span data-ttu-id="2bb98-127">Autres environnements</span><span class="sxs-lookup"><span data-stu-id="2bb98-127">Other environments</span></span>

<span data-ttu-id="2bb98-128">Si l’application est déployée dans un autre environnement (par exemple, docker, Kubernetes ou service Windows), <xref:fundamentals/logging/index> consultez pour plus d’informations sur la configuration des fournisseurs de journalisation appropriés pour l’environnement.</span><span class="sxs-lookup"><span data-stu-id="2bb98-128">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="grpc-client-logging"></a><span data-ttu-id="2bb98-129">journalisation du client gRPC</span><span class="sxs-lookup"><span data-stu-id="2bb98-129">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="2bb98-130">Les journaux côté client peuvent contenir des informations sensibles de votre application.</span><span class="sxs-lookup"><span data-stu-id="2bb98-130">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="2bb98-131">**Ne jamais** poster des journaux bruts à partir d’applications de production vers des forums publics tels que github.</span><span class="sxs-lookup"><span data-stu-id="2bb98-131">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="2bb98-132">Pour récupérer des journaux à partir du client .net, vous pouvez `GrpcChannelOptions.LoggerFactory` définir la propriété lorsque le canal du client est créé.</span><span class="sxs-lookup"><span data-stu-id="2bb98-132">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="2bb98-133">Si vous appelez un service gRPC à partir d’une application ASP.NET Core, la fabrique d’enregistreur peut être résolue à partir de l’injection de dépendances (DI) :</span><span class="sxs-lookup"><span data-stu-id="2bb98-133">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="2bb98-134">Une autre façon d’activer la journalisation du client consiste à utiliser la [fabrique de clients gRPC](xref:grpc/clientfactory) pour créer le client.</span><span class="sxs-lookup"><span data-stu-id="2bb98-134">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="2bb98-135">Un client gRPC inscrit auprès de la fabrique de clients et résolu à partir de DI utilisera automatiquement la journalisation configurée de l’application.</span><span class="sxs-lookup"><span data-stu-id="2bb98-135">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="2bb98-136">Si votre application n’utilise pas di, vous pouvez créer une `ILoggerFactory` nouvelle instance avec [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span><span class="sxs-lookup"><span data-stu-id="2bb98-136">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="2bb98-137">Pour accéder à cette méthode, ajoutez le package [Microsoft. extensions. logginging](https://www.nuget.org/packages/microsoft.extensions.logging/) à votre application.</span><span class="sxs-lookup"><span data-stu-id="2bb98-137">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a><span data-ttu-id="2bb98-138">Exemple de sortie de la journalisation</span><span class="sxs-lookup"><span data-stu-id="2bb98-138">Sample logging output</span></span>

<span data-ttu-id="2bb98-139">Voici un exemple de sortie de console au `Debug` niveau d’un client gRPC :</span><span class="sxs-lookup"><span data-stu-id="2bb98-139">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

```
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a><span data-ttu-id="2bb98-140">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2bb98-140">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
