---
title: gRPC pour la configuration d’ASP.NET Core
author: jamesnk
description: Découvrez comment configurer gRPC pour les applications ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087342"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="3b81c-103">gRPC pour la configuration d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b81c-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="3b81c-104">Configurer les options de services</span><span class="sxs-lookup"><span data-stu-id="3b81c-104">Configure services options</span></span>

<span data-ttu-id="3b81c-105">Le tableau suivant décrit les options de configuration des services de gRPC :</span><span class="sxs-lookup"><span data-stu-id="3b81c-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="3b81c-106">Option</span><span class="sxs-lookup"><span data-stu-id="3b81c-106">Option</span></span> | <span data-ttu-id="3b81c-107">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="3b81c-107">Default Value</span></span> | <span data-ttu-id="3b81c-108">Description</span><span class="sxs-lookup"><span data-stu-id="3b81c-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="3b81c-109">La taille maximale en octets qui peuvent être envoyés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="3b81c-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="3b81c-110">Essayez d’envoyer un message qui dépasse les résultats de la taille maximale de message dans une exception.</span><span class="sxs-lookup"><span data-stu-id="3b81c-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="3b81c-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="3b81c-111">4 MB</span></span> | <span data-ttu-id="3b81c-112">La taille maximale en octets qui peuvent être reçus par le serveur.</span><span class="sxs-lookup"><span data-stu-id="3b81c-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="3b81c-113">Si le serveur reçoit un message qui dépasse cette limite, elle lève une exception.</span><span class="sxs-lookup"><span data-stu-id="3b81c-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="3b81c-114">Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="3b81c-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="3b81c-115">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de service.</span><span class="sxs-lookup"><span data-stu-id="3b81c-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="3b81c-116">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="3b81c-116">The default is `false`.</span></span> <span data-ttu-id="3b81c-117">Définir cette valeur sur `true` peut entraîner une fuite des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="3b81c-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="3b81c-118">gzip</span><span class="sxs-lookup"><span data-stu-id="3b81c-118">gzip</span></span> | <span data-ttu-id="3b81c-119">Une collection de fournisseurs de compression utilisé pour compresser et décompresser des messages.</span><span class="sxs-lookup"><span data-stu-id="3b81c-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="3b81c-120">Fournisseurs de compression personnalisé peuvent être créés et ajoutés à la collection.</span><span class="sxs-lookup"><span data-stu-id="3b81c-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="3b81c-121">La valeur par défaut configuré le fournisseur prend en charge **gzip** la compression.</span><span class="sxs-lookup"><span data-stu-id="3b81c-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="3b81c-122">L’algorithme de compression utilisé pour compresser les messages envoyés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="3b81c-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="3b81c-123">L’algorithme doit correspondre à un fournisseur de la compression dans `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="3b81c-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="3b81c-124">Pour l’algorthm compresser une réponse le client doit indiquer qu’il prend en charge l’algorithme en lui envoyant le **grpc-encodage** en-tête.</span><span class="sxs-lookup"><span data-stu-id="3b81c-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="3b81c-125">Le niveau de compression utilisé pour compresser les messages envoyés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="3b81c-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="3b81c-126">Options peuvent être configurées pour tous les services en fournissant un délégué d’options pour le `AddGrpc` appeler dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3b81c-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="3b81c-127">Options pour un seul service remplacent les options globales fournies dans `AddGrpc` et peuvent être configurés à l’aide de `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="3b81c-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a><span data-ttu-id="3b81c-128">Configurer des options Kestrel</span><span class="sxs-lookup"><span data-stu-id="3b81c-128">Configure Kestrel options</span></span>

<span data-ttu-id="3b81c-129">Serveur kestrel a des options de configuration qui affectent le comportement de gRPC pour ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3b81c-129">Kestrel server has configuration options that affect the behavior of gRPC for ASP.NET.</span></span>

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="3b81c-130">Limite de débit de données de corps de la demande</span><span class="sxs-lookup"><span data-stu-id="3b81c-130">Request body data rate limit</span></span>

<span data-ttu-id="3b81c-131">Par défaut, le serveur Kestrel impose un [débit de données du corps de demande minimale](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span><span class="sxs-lookup"><span data-stu-id="3b81c-131">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="3b81c-132">Pour le client de diffusion en continu et le mode duplex de la diffusion en continu des appels, ce taux ne peut pas être satisfait, et la connexion peut être expiré. La valeur minimale de la demande du corps de la limite de débit doit être désactivé lorsque le service gRPC inclut le client de diffusion en continu et duplex de diffusion en continu des appels :</span><span class="sxs-lookup"><span data-stu-id="3b81c-132">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a><span data-ttu-id="3b81c-133">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3b81c-133">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
