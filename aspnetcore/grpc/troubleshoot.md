---
title: Résoudre les problèmes de gRPC sur .NET Core
author: jamesnk
description: Résoudre les erreurs lors de l’utilisation de gRPC sur .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/12/2019
uid: grpc/troubleshoot
ms.openlocfilehash: ad74bfa57d2dde316734d55d86075f463e78ee56
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029032"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="b0dfa-103">Résoudre les problèmes de gRPC sur .NET Core</span><span class="sxs-lookup"><span data-stu-id="b0dfa-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="b0dfa-104">Par [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="b0dfa-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="b0dfa-105">Incompatibilité entre la configuration SSL/TLS du client et du service</span><span class="sxs-lookup"><span data-stu-id="b0dfa-105">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="b0dfa-106">Le modèle gRPC et les exemples utilisent le protocole [TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246) pour sécuriser les services gRPC par défaut.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-106">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="b0dfa-107">les clients gRPC doivent utiliser une connexion sécurisée pour appeler correctement les services gRPC sécurisés.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-107">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="b0dfa-108">Vous pouvez vérifier que ASP.NET Core le service gRPC utilise TLS dans les journaux écrits au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-108">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="b0dfa-109">Le service est à l’écoute sur un point de terminaison HTTPs:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-109">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="b0dfa-110">Le client .net Core doit utiliser `https` dans l’adresse du serveur pour effectuer des appels avec une connexion sécurisée:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-110">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

<span data-ttu-id="b0dfa-111">Toutes les implémentations du client gRPC prennent en charge TLS.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-111">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="b0dfa-112">les clients gRPC d’autres langages requièrent généralement le `SslCredentials`canal configuré avec.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-112">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="b0dfa-113">`SslCredentials`Spécifie le certificat que le client utilisera, et il doit être utilisé à la place d’informations d’identification non sécurisées.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-113">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="b0dfa-114">Pour obtenir des exemples de configuration des différentes implémentations du client gRPC pour utiliser TLS, consultez [authentification gRPC](https://www.grpc.io/docs/guides/auth/).</span><span class="sxs-lookup"><span data-stu-id="b0dfa-114">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="b0dfa-115">Appeler des services gRPC non sécurisés avec un client .NET Core</span><span class="sxs-lookup"><span data-stu-id="b0dfa-115">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="b0dfa-116">Une configuration supplémentaire est requise pour appeler des services gRPC non sécurisés avec le client .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-116">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="b0dfa-117">Le client gRPC doit définir le `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` commutateur sur `true` et l' `http` utiliser dans l’adresse du serveur:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-117">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="b0dfa-118">Impossible de démarrer ASP.NET Core application gRPC sur macOS</span><span class="sxs-lookup"><span data-stu-id="b0dfa-118">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="b0dfa-119">Kestrel ne prend pas en charge HTTP/2 avec TLS sur macOS et les anciennes versions de Windows telles que Windows 7.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-119">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="b0dfa-120">Le modèle et les exemples de ASP.NET Core gRPC utilisent TLS par défaut.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-120">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="b0dfa-121">Le message d’erreur suivant s’affiche lorsque vous essayez de démarrer le serveur gRPC:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-121">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="b0dfa-122">Impossible de lier à https://localhost:5001 sur l’interface de bouclage IPv4: «HTTP/2 sur TLS n’est pas pris en charge sur macOS en raison de la prise en charge ALPN manquante.».</span><span class="sxs-lookup"><span data-stu-id="b0dfa-122">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="b0dfa-123">Pour contourner ce problème, configurez Kestrel et le client gRPC pour utiliser HTTP/2 *sans* TLS.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-123">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="b0dfa-124">Vous ne devez effectuer cette opération qu’au cours du développement.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-124">You should only do this during development.</span></span> <span data-ttu-id="b0dfa-125">Si vous n’utilisez pas TLS, les messages gRPC sont envoyés sans chiffrement.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-125">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="b0dfa-126">Kestrel doit configurer un point de terminaison HTTP/2 sans TLS dans *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-126">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="b0dfa-127">Le client gRPC doit également être configuré de façon à ne pas utiliser TLS.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-127">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="b0dfa-128">Pour plus d’informations, consultez [appeler des services gRPC non sécurisés avec un client .net Core](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="b0dfa-128">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="b0dfa-129">HTTP/2 sans TLS doit être utilisé uniquement pendant le développement de l’application.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-129">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="b0dfa-130">Les applications de production doivent toujours utiliser la sécurité de transport.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-130">Production apps should always use transport security.</span></span> <span data-ttu-id="b0dfa-131">Pour plus d’informations, consultez Considérations sur la [sécurité dans gRPC pour ASP.net Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="b0dfa-131">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="b0dfa-132">les C# ressources gRPC ne sont pas générées par du code à partir de  *\*fichiers. proto*</span><span class="sxs-lookup"><span data-stu-id="b0dfa-132">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="b0dfa-133">la génération de code gRPC des clients concrets et des classes de base de service requiert que les fichiers et les outils protobuf soient référencés à partir d’un projet.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-133">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="b0dfa-134">Vous devez inclure les éléments suivants:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-134">You must include:</span></span>

* <span data-ttu-id="b0dfa-135">fichiers *. proto* que vous souhaitez utiliser dans le `<Protobuf>` groupe d’éléments.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-135">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="b0dfa-136">Les [fichiers *. proto* ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) importés doivent être référencés par le projet.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-136">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="b0dfa-137">Référence de package au package d’outils gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="b0dfa-137">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="b0dfa-138">Pour plus d’informations sur la C# génération de ressources <xref:grpc/basics>gRPC, consultez.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-138">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="b0dfa-139">Par défaut, une `<Protobuf>` référence génère un client concret et une classe de base de service.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-139">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="b0dfa-140">L’attribut de l' `GrpcServices` élément de référence peut être utilisé C# pour limiter la génération d’éléments multimédias.</span><span class="sxs-lookup"><span data-stu-id="b0dfa-140">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="b0dfa-141">Les `GrpcServices` options valides sont les suivantes:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-141">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="b0dfa-142">`Both`(valeur par défaut absente)</span><span class="sxs-lookup"><span data-stu-id="b0dfa-142">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="b0dfa-143">Une application Web ASP.NET Core hébergeant gRPC services a uniquement besoin de la classe de base de service générée:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-143">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="b0dfa-144">Une application cliente gRPC qui effectue des appels gRPC a uniquement besoin du client concret généré:</span><span class="sxs-lookup"><span data-stu-id="b0dfa-144">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
