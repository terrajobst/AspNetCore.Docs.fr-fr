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
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC pour la configuration d’ASP.NET Core

## <a name="configure-services-options"></a>Configurer les options de services

Le tableau suivant décrit les options de configuration des services de gRPC :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | La taille maximale en octets qui peuvent être envoyés à partir du serveur. Essayez d’envoyer un message qui dépasse les résultats de la taille maximale de message dans une exception. |
| `ReceiveMaxMessageSize` | 4 MB | La taille maximale en octets qui peuvent être reçus par le serveur. Si le serveur reçoit un message qui dépasse cette limite, elle lève une exception. Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire. |
| `EnableDetailedErrors` | `false` | Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de service. Par défaut, il s’agit de `false`. Paramètre `EnableDetailedErrors` à `true` peut entraîner une fuite des informations sensibles. |
| `CompressionProviders` | gzip | Une collection de fournisseurs de compression utilisé pour compresser et décompresser des messages. Fournisseurs de compression personnalisé peuvent être créés et ajoutés à la collection. La valeur par défaut configuré le fournisseur prend en charge **gzip** la compression. |
| `ResponseCompressionAlgorithm` | `null` | L’algorithme de compression utilisé pour compresser les messages envoyés à partir du serveur. L’algorithme doit correspondre à un fournisseur de la compression dans `CompressionProviders`. Pour l’algorithme compresser une réponse, le client doit indiquer qu’il prend en charge l’algorithme en lui envoyant le **grpc-encodage** en-tête. |
| `ResponseCompressionLevel` | `null` | Le niveau de compression utilisé pour compresser les messages envoyés à partir du serveur. |

Options peuvent être configurées pour tous les services en fournissant un délégué d’options pour le `AddGrpc` appeler dans `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Options pour un seul service remplacent les options globales fournies dans `AddGrpc` et peuvent être configurés à l’aide de `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurer les options du client

Le code suivant définit l’envoi maximal du client et la taille de message de réception :

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
