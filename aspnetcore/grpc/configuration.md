---
title: gRPC pour la configuration d’ASP.NET Core
author: jamesnk
description: Découvrez comment configurer gRPC pour les applications ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 05/30/2019
uid: grpc/configuration
ms.openlocfilehash: e269d701f45c0b852a9006107f0162cc5af2c38a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814922"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="78e7d-103">gRPC pour la configuration d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78e7d-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="78e7d-104">Configurer les options de services</span><span class="sxs-lookup"><span data-stu-id="78e7d-104">Configure services options</span></span>

<span data-ttu-id="78e7d-105">Le tableau suivant décrit les options de configuration des services de gRPC :</span><span class="sxs-lookup"><span data-stu-id="78e7d-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="78e7d-106">Option</span><span class="sxs-lookup"><span data-stu-id="78e7d-106">Option</span></span> | <span data-ttu-id="78e7d-107">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="78e7d-107">Default Value</span></span> | <span data-ttu-id="78e7d-108">Description</span><span class="sxs-lookup"><span data-stu-id="78e7d-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="78e7d-109">La taille maximale en octets qui peuvent être envoyés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="78e7d-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="78e7d-110">Essayez d’envoyer un message qui dépasse les résultats de la taille maximale de message dans une exception.</span><span class="sxs-lookup"><span data-stu-id="78e7d-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="78e7d-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="78e7d-111">4 MB</span></span> | <span data-ttu-id="78e7d-112">La taille maximale en octets qui peuvent être reçus par le serveur.</span><span class="sxs-lookup"><span data-stu-id="78e7d-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="78e7d-113">Si le serveur reçoit un message qui dépasse cette limite, elle lève une exception.</span><span class="sxs-lookup"><span data-stu-id="78e7d-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="78e7d-114">Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="78e7d-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="78e7d-115">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de service.</span><span class="sxs-lookup"><span data-stu-id="78e7d-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="78e7d-116">Par défaut, il s’agit de `false`.</span><span class="sxs-lookup"><span data-stu-id="78e7d-116">The default is `false`.</span></span> <span data-ttu-id="78e7d-117">Paramètre `EnableDetailedErrors` à `true` peut entraîner une fuite des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="78e7d-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="78e7d-118">gzip</span><span class="sxs-lookup"><span data-stu-id="78e7d-118">gzip</span></span> | <span data-ttu-id="78e7d-119">Une collection de fournisseurs de compression utilisé pour compresser et décompresser des messages.</span><span class="sxs-lookup"><span data-stu-id="78e7d-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="78e7d-120">Fournisseurs de compression personnalisé peuvent être créés et ajoutés à la collection.</span><span class="sxs-lookup"><span data-stu-id="78e7d-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="78e7d-121">La valeur par défaut configuré le fournisseur prend en charge **gzip** la compression.</span><span class="sxs-lookup"><span data-stu-id="78e7d-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="78e7d-122">L’algorithme de compression utilisé pour compresser les messages envoyés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="78e7d-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="78e7d-123">L’algorithme doit correspondre à un fournisseur de la compression dans `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="78e7d-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="78e7d-124">Pour l’algorithme compresser une réponse, le client doit indiquer qu’il prend en charge l’algorithme en lui envoyant le **grpc-encodage** en-tête.</span><span class="sxs-lookup"><span data-stu-id="78e7d-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="78e7d-125">Le niveau de compression utilisé pour compresser les messages envoyés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="78e7d-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="78e7d-126">Options peuvent être configurées pour tous les services en fournissant un délégué d’options pour le `AddGrpc` appeler dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="78e7d-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="78e7d-127">Options pour un seul service remplacent les options globales fournies dans `AddGrpc` et peuvent être configurés à l’aide de `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="78e7d-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="78e7d-128">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="78e7d-128">Configure client options</span></span>

<span data-ttu-id="78e7d-129">Le code suivant définit l’envoi maximal du client et la taille de message de réception :</span><span class="sxs-lookup"><span data-stu-id="78e7d-129">The following code sets the client maximum send and receive message size:</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a><span data-ttu-id="78e7d-130">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="78e7d-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
