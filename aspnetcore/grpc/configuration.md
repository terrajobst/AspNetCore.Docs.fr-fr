---
title: configuration de gRPC pour ASP.NET Core
author: jamesnk
description: Découvrez comment configurer gRPC pour les applications ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/21/2019
uid: grpc/configuration
ms.openlocfilehash: 34eb598211c87fbb2c68ae5e041da50d02f543f7
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310314"
---
# <a name="grpc-for-aspnet-core-configuration"></a>configuration de gRPC pour ASP.NET Core

## <a name="configure-services-options"></a>Configurer les options des services

Le tableau suivant décrit les options de configuration des services gRPC :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Taille maximale du message, en octets, qui peut être envoyée à partir du serveur. Toute tentative d’envoi d’un message qui dépasse la taille de message maximale configurée entraîne une exception. |
| `MaxReceiveMessageSize` | 4 MB | Taille maximale du message, en octets, qui peut être reçue par le serveur. Si le serveur reçoit un message qui dépasse cette limite, il lève une exception. L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire. |
| `EnableDetailedErrors` | `false` | Si `true`la valeur est, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de service. Par défaut, il s’agit de `false`. La `EnableDetailedErrors` définition `true` de sur peut entraîner une fuite d’informations sensibles. |
| `CompressionProviders` | gzip, deflate | Collection de fournisseurs de compression utilisée pour compresser et décompresser des messages. Des fournisseurs de compression personnalisés peuvent être créés et ajoutés à la collection. Les fournisseurs configurés par défaut prennent en charge la compression **gzip** et **deflate** . |
| `ResponseCompressionAlgorithm` | `null` | Algorithme de compression utilisé pour compresser les messages envoyés à partir du serveur. L’algorithme doit correspondre à un fournisseur `CompressionProviders`de compression dans. Pour que l’algorithme compresse une réponse, le client doit indiquer qu’il prend en charge l’algorithme en l’envoyant dans l’en-tête **GRPC-Accept-Encoding** . |
| `ResponseCompressionLevel` | `null` | Niveau de compression utilisé pour compresser les messages envoyés à partir du serveur. |

Les options peuvent être configurées pour tous les services en fournissant un délégué `AddGrpc` d’options `Startup.ConfigureServices`à l’appel dans :

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Les options pour un seul service remplacent les options globales `AddGrpc` fournies dans et peuvent être `AddServiceOptions<TService>`configurées à l’aide de :

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurer les options du client

Le tableau suivant décrit les options de configuration des canaux gRPC :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| `HttpClient` | Nouvelle instance | Utilisé `HttpClient` pour effectuer des appels gRPC. Un client peut être configuré pour configurer un personnalisé `HttpClientHandler`ou ajouter des gestionnaires supplémentaires au pipeline HTTP pour les appels gRPC. Par défaut, une `HttpClient` nouvelle instance est créée. |
| `MaxSendMessageSize` | `null` | Taille maximale du message, en octets, qui peut être envoyée à partir du client. Toute tentative d’envoi d’un message qui dépasse la taille de message maximale configurée entraîne une exception. |
| `MaxReceiveMessageSize` | 4 MB | Taille maximale du message, en octets, qui peut être reçue par le client. Si le serveur reçoit un message qui dépasse cette limite, il lève une exception. L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire. |
| `TransportOptions` | `null` | Options de transport configurer la manière dont le canal appelle le service gRPC. Actuellement, la seule implémentation `HttpClientTransport` est options vous permet de `HttpClient` spécifier le utilisé par gRPC. |
| `Credentials` | `null` | Instance de `ChannelCredentials`. Les informations d’identification sont utilisées pour ajouter des métadonnées d’authentification à des appels gRPC. |
| `CompressionProviders` | gzip, deflate | Collection de fournisseurs de compression utilisée pour compresser et décompresser des messages. Des fournisseurs de compression personnalisés peuvent être créés et ajoutés à la collection. Les fournisseurs configurés par défaut prennent en charge la compression **gzip** et **deflate** . |

L'exemple de code suivant :

* Définit la taille maximale des messages d’envoi et de réception sur le canal.
* Crée un client.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:tutorials/grpc/grpc-start>
