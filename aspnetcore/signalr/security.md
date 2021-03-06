---
title: Considérations relatives à la sécurité dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment utiliser l’authentification et l’autorisation dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: f92b56132d0fa55665568416d0760430cb698f8b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78668146"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a>Considérations relatives à la sécurité dans ASP.NET Core SignalR

Par [Andrew Stanton-infirmière](https://twitter.com/anurse)

Cet article fournit des informations sur la sécurisation des SignalR.

## <a name="cross-origin-resource-sharing"></a>Partage de ressources cross-origin

Le [partage des ressources Cross-Origin (cors)](https://www.w3.org/TR/cors/) peut être utilisé pour autoriser les connexions de SignalR Cross-Origin dans le navigateur. Si le code JavaScript est hébergé sur un domaine différent de l’application SignalR, l' [intergiciel (middleware) cors](xref:security/cors) doit être activé pour permettre à JavaScript de se connecter à l’application SignalR. Autorisez les requêtes Cross-Origin uniquement à partir des domaines que vous approuvez ou contrôlez. Par exemple :

* Votre site est hébergé sur `http://www.example.com`
* Votre application SignalR est hébergée sur `http://signalr.example.com`

CORS doit être configuré dans l’application SignalR pour autoriser uniquement le `www.example.com`d’origine.

Pour plus d’informations sur la configuration de CORS, consultez [activer les demandes Cross-Origin (cors)](xref:security/cors). SignalR **requiert** les stratégies cors suivantes :

* Autorisez les origines attendues spécifiques. L’autorisation d’une origine est possible, mais elle n’est **pas** sécurisée ou recommandée.
* Les méthodes HTTP `GET` et `POST` doivent être autorisées.
* Les informations d’identification doivent être autorisées afin que les sessions rémanentes basées sur des cookies fonctionnent correctement. Ils doivent être activés même lorsque l’authentification n’est pas utilisée.

<!--
::: moniker range=">= aspnetcore-5.0"  // Moniker here just to make sure this doesn't get missed in the 5.0 version update.
However, in 5.0 we have provided an option in the TypeScript client to not use credentials.
The not to use credentials option should only be used when you know 100% that credentials like Cookies are not needed in your app (cookies are used by azure app service when using multiple servers)

For more info, see https://github.com/dotnet/AspNetCore.Docs/issues/16003
.-->

Par exemple, la stratégie CORS suivante permet à un client de navigateur SignalR hébergé sur `https://example.com` d’accéder à l’application SignalR hébergée sur `https://signalr.example.com`:

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> SignalR n’est pas compatible avec la fonctionnalité CORS intégrée dans Azure App Service.

## <a name="websocket-origin-restriction"></a>Restriction d’origine WebSocket

::: moniker range=">= aspnetcore-2.2"

Les protections fournies par CORS ne s’appliquent pas aux WebSockets. Pour une restriction d’origine sur les WebSockets, consultez [restriction d’origine des WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Les protections fournies par CORS ne s’appliquent pas aux WebSockets. Les navigateurs **:**

* n’effectuent pas de requêtes préalables CORS ;
* respectent les restrictions spécifiées dans les en-têtes `Access-Control` quand ils effectuent des requêtes WebSocket.

Toutefois, les navigateurs envoient l’en-tête `Origin` au moment de l’émission des requêtes WebSocket. Les applications doivent être configurées de manière à valider ces en-têtes, le but étant de vérifier que seuls les WebSockets provenant des origines attendues sont autorisés.

Dans ASP.NET Core 2,1 et versions ultérieures, la validation d’en-tête peut être obtenue à l’aide d’un intergiciel (middleware) personnalisé placé **avant `UseSignalR`et de l’intergiciel (middleware) d’authentification** dans `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> L’en-tête `Origin` est contrôlé par le client et, comme l’en-tête `Referer`, peut être falsifié. Ces en-têtes ne doivent **pas** être utilisés en tant que mécanisme d’authentification.

::: moniker-end

## <a name="connectionid"></a>ConnectionId

L’exposition de `ConnectionId` peut entraîner un emprunt d’identité malveillant si la version du client ou du serveur SignalR est ASP.NET Core 2,2 ou une version antérieure. Si le serveur SignalR et la version du client sont ASP.NET Core 3,0 ou version ultérieure, le `ConnectionToken` plutôt que le `ConnectionId` doit être gardé secret. L' `ConnectionToken` n’est volontairement pas exposée dans une API.  Il peut être difficile de s’assurer que les anciens clients SignalR ne se connectent pas au serveur. par conséquent, même si votre version de SignalR Server est ASP.NET Core 3,0 ou une version ultérieure, le `ConnectionId` ne doit pas être exposé.

## <a name="access-token-logging"></a>Journalisation des jetons d’accès

Lorsque vous utilisez des WebSockets ou des événements envoyés par le serveur, le client du navigateur envoie le jeton d’accès dans la chaîne de requête. La réception du jeton d’accès via la chaîne de requête est généralement sécurisée à l’aide de l’en-tête `Authorization` standard. Utilisez toujours le protocole HTTPs pour garantir une connexion sécurisée de bout en bout entre le client et le serveur. De nombreux serveurs Web consignent l’URL pour chaque demande, y compris la chaîne de requête. La journalisation des URL peut enregistrer le jeton d’accès. ASP.NET Core enregistre l’URL pour chaque demande par défaut, qui inclut la chaîne de requête. Par exemple :

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Si vous avez des doutes quant à la journalisation de ces données avec les journaux de votre serveur, vous pouvez désactiver cette journalisation en configurant l’enregistreur d' `Microsoft.AspNetCore.Hosting` au niveau `Warning` ou au-dessus (ces messages sont écrits au niveau `Info`). Pour plus d’informations, consultez [filtrage des journaux](xref:fundamentals/logging/index#log-filtering) pour plus d’informations. Si vous souhaitez toujours enregistrer certaines informations de demande, vous pouvez [écrire un intergiciel (middleware](xref:fundamentals/middleware/write) ) pour enregistrer les données dont vous avez besoin et filtrer les `access_token` valeur de chaîne de requête (le cas échéant).

## <a name="exceptions"></a>Exceptions

Les messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client. Par défaut, SignalR n’envoie pas les détails d’une exception levée par une méthode de concentrateur au client. Au lieu de cela, le client reçoit un message générique indiquant qu’une erreur s’est produite. La remise de message d’exception au client peut être remplacée (par exemple, dans le développement ou le test) par [EnableDetailedErrors](xref:signalr/configuration#configure-server-options). Les messages d’exception ne doivent pas être exposés au client dans les applications de production.

## <a name="buffer-management"></a>Gestion des tampons

SignalR utilise des tampons par connexion pour gérer les messages entrants et sortants. Par défaut, SignalR limite ces mémoires tampon à 32 Ko. Le plus grand message qu’un client ou un serveur peut envoyer est de 32 Ko. La mémoire maximale consommée par une connexion pour les messages est de 32 Ko. Si vos messages sont toujours inférieurs à 32 Ko, vous pouvez réduire la limite, qui :

* Empêche un client d’envoyer un message plus volumineux.
* Le serveur n’a jamais besoin d’allouer de grandes mémoires tampons pour accepter des messages.

Si vos messages sont supérieurs à 32 Ko, vous pouvez augmenter la limite. L’amélioration de cette limite signifie :

* Le client peut forcer le serveur à allouer de grandes mémoires tampons de mémoire.
* L’allocation de serveurs de mémoires tampons de grande taille peut réduire le nombre de connexions simultanées.

Il existe des limites pour les messages entrants et sortants, qui peuvent être configurés sur l’objet [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) configuré dans `MapHub`:

* `ApplicationMaxBufferSize` représente le nombre maximal d’octets du client que le serveur met en mémoire tampon. Si le client tente d’envoyer un message d’une taille supérieure à cette limite, la connexion peut être fermée.
* `TransportMaxBufferSize` représente le nombre maximal d’octets que le serveur peut envoyer. Si le serveur tente d’envoyer un message (y compris les valeurs de retour des méthodes de concentrateur) supérieures à cette limite, une exception est levée.

Définir la limite sur `0` désactive la limite. La suppression de la limite permet à un client d’envoyer un message de toute taille. Les clients malveillants qui envoient des messages volumineux peuvent entraîner l’allocation de mémoire supplémentaire. L’utilisation excessive de la mémoire peut réduire de manière significative le nombre de connexions simultanées.
