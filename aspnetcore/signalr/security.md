---
title: Considérations de sécurité dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment utiliser l’authentification et les autorisations dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: 6e9f849ed856cf1cbf989b8b16cab5209c465471
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743785"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Considérations de sécurité dans ASP.NET Core SignalR

Par [Andrew Stanton-Nurse](https://twitter.com/anurse)

Cet article fournit des informations sur la sécurisation de SignalR.

## <a name="cross-origin-resource-sharing"></a>Partage des ressources cross-origin

[Ressources cross-origin CORS (partage)](https://www.w3.org/TR/cors/) peut être utilisé pour autoriser les connexions SignalR cross-origine dans le navigateur. Si le code JavaScript est hébergé sur un domaine différent de l’application SignalR, [intergiciel (middleware) CORS](xref:security/cors) doit être activée pour autoriser le code JavaScript pour se connecter à l’application de SignalR. Autoriser les demandes cross-origin uniquement à partir de domaines de que confiance ou un contrôle. Exemple :

* Votre site est hébergé sur `http://www.example.com`
* Votre application SignalR est hébergée sur `http://signalr.example.com`

CORS doit être configuré dans l’application de SignalR pour autoriser uniquement l’origine `www.example.com`.

Pour plus d’informations sur la configuration de CORS, consultez [activer de demandes de Cross-Origin (CORS)](xref:security/cors). SignalR **requiert** les stratégies CORS suivantes :

* Autoriser les origines attendus spécifiques. Autoriser toute origine est possible mais est **pas** sécurisée ou recommandée.
* Méthodes HTTP `GET` et `POST` doivent être autorisés.
* Informations d’identification doivent être activées, même lorsque l’authentification n’est pas utilisée.

Par exemple, la stratégie CORS suivante permet à un client de navigateur de SignalR hébergé sur `https://example.com` pour accéder à l’application de SignalR hébergée sur `https://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR n’est pas compatible avec la fonctionnalité CORS intégrée dans Azure App Service.

## <a name="websocket-origin-restriction"></a>Restriction de l’origine de WebSocket

::: moniker range=">= aspnetcore-2.2"

Les protections fournies par CORS ne s’appliquent pas aux WebSockets. Pour la restriction de l’origine sur WebSockets, lisez [restriction de l’origine WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Les protections fournies par CORS ne s’appliquent pas aux WebSockets. Les navigateurs **:**

* n’effectuent pas de requêtes préalables CORS ;
* respectent les restrictions spécifiées dans les en-têtes `Access-Control` quand ils effectuent des requêtes WebSocket.

Toutefois, les navigateurs envoient l’en-tête `Origin` au moment de l’émission des requêtes WebSocket. Les applications doivent être configurées de manière à valider ces en-têtes, le but étant de vérifier que seuls les WebSockets provenant des origines attendues sont autorisés.

Dans ASP.NET Core 2.1 et versions ultérieures, validation de l’en-tête peut être obtenue à l’aide d’un intergiciel (middleware) personnalisé placé **avant `UseSignalR`et l’intergiciel d’authentification** dans `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> L’en-tête `Origin` est contrôlé par le client et, comme l’en-tête `Referer`, peut être falsifié. Ces en-têtes doivent **pas** être utilisé comme mécanisme d’authentification.

::: moniker-end

## <a name="access-token-logging"></a>Connexion de jeton d’accès

Lorsque vous utilisez des WebSockets ou les événements, le navigateur client envoie le jeton d’accès dans la chaîne de requête. Reçoit le jeton d’accès via la chaîne de requête est généralement aussi sécurisé qu’à l’aide de la norme `Authorization` en-tête. Vous devez toujours utiliser HTTPS pour garantir une connexion sécurisée de bout en bout entre le client et le serveur. Nombre de serveurs web connecter à l’URL pour chaque demande, y compris la chaîne de requête. Journalisation de l’URL peut enregistrer le jeton d’accès. ASP.NET Core se connecte à l’URL pour chaque demande par défaut, ce qui inclut la chaîne de requête. Exemple :

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Si vous avez des questions sur l’enregistrement de ces données avec les journaux de votre serveur, vous pouvez désactiver cette journalisation entièrement en configurant le `Microsoft.AspNetCore.Hosting` enregistreur d’événements à la `Warning` niveau ou version ultérieure (ces messages sont écrits au `Info` niveau). Consultez la documentation sur [filtrage de journal](xref:fundamentals/logging/index#log-filtering) pour plus d’informations. Si vous souhaitez toujours enregistrer certaines informations de demande, vous pouvez [écrire un intergiciel (middleware)](xref:fundamentals/middleware/write) pour journaliser les données que vous avez besoin et éliminer les `access_token` valeur de chaîne de requête (le cas échéant).

## <a name="exceptions"></a>Exceptions

Messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client. Par défaut, les détails d’une exception levée par une méthode de concentrateur au client n’envoie pas de SignalR. Au lieu de cela, le client reçoit un message générique indiquant qu'une erreur s’est produite. Remise de messages d’exception pour le client peut être remplacée (par exemple dans le développement ou test) par [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options). Messages d’exception ne doivent pas être exposées au client dans les applications de production.

## <a name="buffer-management"></a>Gestion des tampons

SignalR utilise des mémoires tampons de chaque connexion pour gérer les messages entrants et sortants. Par défaut, SignalR limite ces mémoires tampons à 32 Ko. Un client ou le serveur peut envoyer message maximale est de 32 Ko. La mémoire maximale utilisée par une connexion pour les messages est de 32 Ko. Si vos messages sont toujours inférieures à 32 Ko, vous pouvez réduire la limite, dont :

* Empêche un client d’être en mesure d’envoyer un message plus volumineux.
* Le serveur devra jamais allouer des tampons de grande taille pour accepter les messages.

Si vos messages sont supérieur à 32 Ko, vous pouvez augmenter la limite. Si vous augmentez cette limite :

* Le client peut entraîner le serveur d’allouer des mémoires tampons volumineuses.
* Allocation de serveur de tampons de grande taille peut réduire le nombre de connexions simultanées.

Il existe des limites pour les messages entrants et sortants, les deux peuvent être configurées sur le [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objet configuré dans `MapHub`:

* `ApplicationMaxBufferSize` représente le nombre maximal d’octets à partir du client qui les mémoires tampons de serveur. Si le client tente d’envoyer un message dépasse cette limite, la connexion peut être fermée.
* `TransportMaxBufferSize` représente le nombre maximal d’octets que le serveur peut envoyer. Si le serveur tente d’envoyer un message (y compris les valeurs de retour des méthodes de concentrateur) dépasse cette limite, une exception sera levée.

Définition de la limite `0` désactive la limite. Suppression d’une limite permet à un client envoyer un message de n’importe quelle taille. Clients malveillants envoi de messages volumineux peuvent entraîner de trop de mémoire à allouer. Utilisation excessive de la mémoire peut réduire considérablement le nombre de connexions simultanées.
