---
title: configuration de gRPC pour .NET
author: jamesnk
description: Découvrez comment configurer gRPC pour les applications .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 02/26/2020
uid: grpc/configuration
ms.openlocfilehash: cabe2d86f535bf3063dd7ede9e8a3bc5de70e244
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666900"
---
# <a name="grpc-for-net-configuration"></a>configuration de gRPC pour .NET

## <a name="configure-services-options"></a>Configurer les options des services

les services gRPC sont configurés avec `AddGrpc` dans *Startup.cs*. Le tableau suivant décrit les options de configuration des services gRPC :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| MaxSendMessageSize | `null` | Taille maximale du message, en octets, qui peut être envoyée à partir du serveur. Toute tentative d’envoi d’un message qui dépasse la taille de message maximale configurée entraîne une exception. |
| MaxReceiveMessageSize | 4 Mo | Taille maximale du message, en octets, qui peut être reçue par le serveur. Si le serveur reçoit un message qui dépasse cette limite, il lève une exception. L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire. |
| EnableDetailedErrors | `false` | Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de service. Par défaut, il s’agit de `false`. La définition de `EnableDetailedErrors` sur `true` peut entraîner des fuites d’informations sensibles. |
| CompressionProviders | gzip | Collection de fournisseurs de compression utilisée pour compresser et décompresser des messages. Des fournisseurs de compression personnalisés peuvent être créés et ajoutés à la collection. Les fournisseurs configurés par défaut prennent en charge la compression **gzip** . |
| <span style="word-break:normal;word-wrap:normal">ResponseCompressionAlgorithm</span> | `null` | Algorithme de compression utilisé pour compresser les messages envoyés à partir du serveur. L’algorithme doit correspondre à un fournisseur de compression dans `CompressionProviders`. Pour que l’algorithme compresse une réponse, le client doit indiquer qu’il prend en charge l’algorithme en l’envoyant dans l’en-tête **GRPC-Accept-Encoding** . |
| ResponseCompressionLevel | `null` | Niveau de compression utilisé pour compresser les messages envoyés à partir du serveur. |
| Intercepteurs | None | Collection d’intercepteurs exécutés avec chaque appel gRPC. Les intercepteurs sont exécutés dans l’ordre dans lequel ils sont inscrits. Les intercepteurs configurés globalement sont exécutés avant les intercepteurs configurés pour un service unique. Pour plus d’informations sur les intercepteurs gRPC, consultez [intercepteurs gRPC et intergiciel](xref:grpc/migration#grpc-interceptors-vs-middleware). |

Les options peuvent être configurées pour tous les services en fournissant un délégué options à l’appel `AddGrpc` dans `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Les options pour un seul service remplacent les options globales fournies dans `AddGrpc` et peuvent être configurées à l’aide de `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurer les options du client

la configuration du client gRPC est définie sur `GrpcChannelOptions`. Le tableau suivant décrit les options de configuration des canaux gRPC :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| HttpClient | Nouvelle instance | `HttpClient` utilisé pour effectuer des appels gRPC. Un client peut être configuré pour configurer un `HttpClientHandler`personnalisé ou ajouter des gestionnaires supplémentaires au pipeline HTTP pour les appels gRPC. Si aucun `HttpClient` n’est spécifié, une nouvelle instance de `HttpClient` est créée pour le canal. Elle est automatiquement supprimée. |
| DisposeHttpClient | `false` | Si `true`et qu’un `HttpClient` est spécifié, l’instance `HttpClient` est supprimée lors de la suppression de l' `GrpcChannel`. |
| LoggerFactory | `null` | `LoggerFactory` utilisé par le client pour enregistrer des informations sur les appels gRPC. Une instance de `LoggerFactory` peut être résolue à partir de l’injection de dépendances ou créée à l’aide de `LoggerFactory.Create`. Pour obtenir des exemples de configuration de la journalisation, consultez <xref:grpc/diagnostics#grpc-client-logging>. |
| MaxSendMessageSize | `null` | Taille maximale du message, en octets, qui peut être envoyée à partir du client. Toute tentative d’envoi d’un message qui dépasse la taille de message maximale configurée entraîne une exception. |
| <span style="word-break:normal;word-wrap:normal">MaxReceiveMessageSize</span> | 4 Mo | Taille maximale du message, en octets, qui peut être reçue par le client. Si le client reçoit un message qui dépasse cette limite, il lève une exception. L’augmentation de cette valeur permet au client de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire. |
| Informations d'identification | `null` | Une instance de `ChannelCredentials`. Les informations d’identification sont utilisées pour ajouter des métadonnées d’authentification à des appels gRPC. |
| CompressionProviders | gzip | Collection de fournisseurs de compression utilisée pour compresser et décompresser des messages. Des fournisseurs de compression personnalisés peuvent être créés et ajoutés à la collection. Les fournisseurs configurés par défaut prennent en charge la compression **gzip** . |

Le code suivant :

* Définit la taille maximale des messages d’envoi et de réception sur le canal.
* Crée un client.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
