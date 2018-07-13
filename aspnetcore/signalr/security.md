---
title: Considérations de sécurité dans ASP.NET Core SignalR
author: rachelappel
description: Découvrez comment utiliser l’authentification et autorisation dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: eff4542b88f24dd6c1c0675f56874e368d441fdd
ms.sourcegitcommit: 32626efaa7316c9b283c96be6516e637d548c5e5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39028481"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Considérations de sécurité dans ASP.NET Core SignalR

Par [Andrew Stanton-Nurse](https://twitter.com/anurse)

## <a name="overview"></a>Vue d'ensemble

SignalR fournit un nombre de protections de sécurité par défaut. Il est important de comprendre comment configurer ces protections.

### <a name="cross-origin-resource-sharing"></a>Partage des ressources cross-origin

[Ressources cross-origin CORS (partage)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) peut être utilisé pour autoriser les connexions SignalR cross-origine dans le navigateur. Si votre code JavaScript est hébergé sur un nom de domaine différent à partir de votre application SignalR, vous devez activer le [intergiciel (middleware) ASP.NET Core CORS](xref:security/cors) afin de permettre la connexion. En règle générale, autoriser les demandes cross-origin uniquement à partir de domaines que vous contrôlez. Par exemple, si votre site est hébergé sur `http://www.example.com` et votre application SignalR est hébergée sur `http://signalr.example.com`, vous devez configurer CORS dans votre application de SignalR pour autoriser uniquement l’origine `www.example.com`.

Pour plus d’informations sur la configuration de CORS, consultez [la documentation sur ASP.NET Core CORS](xref:security/cors). SignalR requiert que les stratégies CORS suivantes afin de fonctionner correctement :

* La stratégie doit autoriser les origines spécifiques que vous escomptez ou autoriser toute origine (non recommandé).
* Méthodes HTTP `GET` et `POST` doivent être autorisés.
* Informations d’identification doivent être activées, même lorsque vous n’utilisez pas l’authentification.

Par exemple, la stratégie CORS suivante permet à un client de navigateur de SignalR hébergé sur `http://example.com` pour accéder à votre application SignalR :

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR n’est pas compatible avec la fonctionnalité CORS intégrée dans Azure App Service.

### <a name="access-token-logging"></a>Connexion de jeton d’accès

Lorsque vous utilisez des WebSockets ou les événements, le navigateur client envoie le jeton d’accès dans la chaîne de requête. Il s’agit généralement aussi sécurisé qu’à l’aide de la norme `Authorization` en-tête, mais de nombreux serveurs web connecter à l’URL pour chaque demande, y compris la chaîne de requête. Cela signifie que le jeton d’accès peut être inclus dans les journaux. Examinez les paramètres de journalisation du serveur web pour éviter l’enregistrement de ces informations.

### <a name="exceptions"></a>Exceptions

Messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client. Par défaut, les détails d’une exception levée par une méthode de concentrateur au client n’envoie pas de SignalR. Au lieu de cela, le client reçoit un message générique indiquant qu'une erreur s’est produite. Vous pouvez substituer ce comportement en définissant le [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) paramètre.

### <a name="buffer-management"></a>Gestion des tampons

SignalR utilise des mémoires tampons de chaque connexion afin de gérer les messages entrants et sortants. Par défaut, SignalR limite ces mémoires tampons à 32 Ko. Cela signifie que le plus grand message possible qu'un client ou le serveur peut envoyer est de 32 Ko. Cela signifie également la quantité maximale de mémoire consommée par une connexion pour les messages est 32 Ko. Si vous connaissez que vos messages sont toujours plus petits que cette limite, vous pouvez réduire cette taille pour empêcher un client capable d’envoyer un message plus volumineux et de forcer le serveur à allouer de la mémoire pour l’accepter. De même, si vous connaissez que vos messages sont supérieures à cette limite, vous pouvez l’augmenter. Toutefois, n’oubliez pas que si vous augmentez cette limite, que le client est en mesure de provoquer le serveur d’allocation de mémoire supplémentaire et peut réduire le nombre de connexions simultanées que votre application puisse les traiter.

Il existe des limites distincts pour les messages entrants et sortants, les deux peuvent être configurées sur le [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objet configuré dans `MapHub`:

* `ApplicationMaxBufferSize` représente le nombre maximal d’octets à partir du client qui les mémoires tampons de serveur. Si le client tente d’envoyer un message dépasse cette limite, la connexion peut être fermée.
* `TransportMaxBufferSize` représente le nombre maximal d’octets que le serveur peut envoyer. Si le serveur tente d’envoyer un message (inclut les valeurs de retour des méthodes de concentrateur) dépasse cette limite, une exception sera levée.

Définition de la limite `0` permet de désactiver complètement la limite. Toutefois, cela doit être fait avec une extrême prudence. Suppression d’une limite permet à un client envoyer un message de n’importe quelle taille. Il peut être utilisé par un client malveillant de provoquer l’excès de mémoire à allouer, ce qui pourrait réduire considérablement le nombre de connexions simultanées que votre application peut prendre en charge.
