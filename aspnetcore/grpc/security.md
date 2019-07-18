---
title: Considérations sur la sécurité dans gRPC pour ASP.NET Core
author: jamesnk
description: En savoir plus sur les considérations de sécurité pour gRPC pour ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/07/2019
uid: grpc/security
ms.openlocfilehash: 4a70cb16d8397dbc69a626435fb72a0512788b14
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308752"
---
# <a name="security-considerations-in-grpc-for-aspnet-core"></a>Considérations sur la sécurité dans gRPC pour ASP.NET Core

Par [James Newton-King](https://twitter.com/jamesnk)

Cet article fournit des informations sur la sécurisation de gRPC avec .NET Core.

## <a name="transport-security"></a>Sécurité de transport

les messages gRPC sont envoyés et reçus à l’aide de HTTP/2. Nous vous recommandons d’utiliser les éléments suivants:

* [TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246) permet de sécuriser les messages dans les applications gRPC de production.
* les services gRPC doivent uniquement écouter les ports sécurisés et y répondre.

TLS est configuré dans Kestrel. Pour plus d’informations sur la configuration des points de terminaison Kestrel, consultez [configuration du point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

## <a name="exceptions"></a>Exceptions

Les messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client. Par défaut, gRPC n’envoie pas les détails d’une exception levée par un service gRPC au client. Au lieu de cela, le client reçoit un message générique indiquant qu’une erreur s’est produite. La remise de message d’exception au client peut être remplacée (par exemple, dans le développement ou le test) par [EnableDetailedErrors](xref:grpc/configuration#configure-services-options). Les messages d’exception ne doivent pas être exposés au client dans les applications de production.

## <a name="message-size-limits"></a>Limites de taille des messages

Les messages entrants aux clients et services gRPC sont chargés en mémoire. Les limites de taille des messages sont un mécanisme qui permet d’empêcher gRPC de consommer trop de ressources.

gRPC utilise des limites de taille par message pour gérer les messages entrants et sortants. Par défaut, gRPC limite les messages entrants à 4 Mo. Il n’existe aucune limite pour les messages sortants.

Sur le serveur, les limites de messages gRPC peuvent être configurées pour tous les services `AddGrpc`d’une application avec:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 1 * 1024 * 1024;  // 1 megabyte
        options.SendMaxMessageSize = 1 * 1024 * 1024;     // 1 megabyte
    });
}
```

Les limites peuvent également être configurées pour un service `AddServiceOptions<TService>`individuel à l’aide de. Pour plus d’informations sur la configuration des limites de taille des messages, consultez [configuration de gRPC](xref:grpc/configuration).

## <a name="client-certificate-validation"></a>Validation du certificat client

Les [certificats clients](https://tools.ietf.org/html/rfc5246#section-7.4.4) sont initialement validés lorsque la connexion est établie. Par défaut, Kestrel n’effectue pas de validation supplémentaire du certificat client d’une connexion.

Nous recommandons que les services gRPC sécurisés par les certificats clients utilisent le package [Microsoft. AspNetCore. Authentication. Certificate](xref:security/authentication/certauth) . ASP.NET Core l’authentification de certification effectue une validation supplémentaire sur un certificat client, notamment:

* Le certificat a une utilisation avancée de la clé (EKU) valide
* Est dans sa période de validité
* Vérifier la révocation des certificats
