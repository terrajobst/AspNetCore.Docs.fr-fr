---
title: Héberger et déployer ASP.NET Core serveur Blazor
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor Server à l’aide de ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: b928296c45ddb11efcd2c8912cc595c799e65037
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447254"
---
# <a name="host-and-deploy-opno-locblazor-server"></a>Héberger et déployer Blazor serveur

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

les [applicationsBlazor Server](xref:blazor/hosting-models#blazor-server) peuvent accepter des [valeurs de configuration d’hôte génériques](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Déploiement

À l’aide du [modèle d’hébergement de serveurBlazor](xref:blazor/hosting-models#blazor-server), Blazor est exécutée sur le serveur à partir d’une application ASP.net core. Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés via une connexion [SignalR](xref:signalr/introduction) .

Un serveur web capable d’héberger une application ASP.NET Core est nécessaire. Visual Studio comprend le modèle de projet d' **applicationBlazor Server** (`blazorserverside` modèle lors de l’utilisation de la commande [dotnet New](/dotnet/core/tools/dotnet-new) ).

## <a name="scalability"></a>Extensibilité

Planifiez un déploiement pour tirer le meilleur parti de l’infrastructure disponible pour une application Blazor Server. Consultez les ressources suivantes pour résoudre Blazor extensibilité des applications serveur :

* [Notions de base des applications Blazor Server](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>Serveur de déploiement

Lorsque vous envisagez l’évolutivité d’un serveur unique (montée en puissance), la mémoire disponible pour une application est probablement la première ressource que l’application épuisera en fonction des demandes des utilisateurs. La mémoire disponible sur le serveur affecte les éléments suivants :

* Nombre de circuits actifs qu’un serveur peut prendre en charge.
* Latence de l’interface utilisateur sur le client.

Pour obtenir des conseils sur la création d’applications Blazor Server sécurisées et évolutives, consultez <xref:security/blazor/server>.

Chaque circuit utilise environ 250 Ko de mémoire pour une application de type *Hello World*minimale. La taille d’un circuit dépend du code de l’application et des exigences de maintenance d’état associées à chaque composant. Nous vous recommandons de mesurer les demandes de ressources pendant le développement de votre application et de votre infrastructure, mais la ligne de base suivante peut être un point de départ pour la planification de votre cible de déploiement : Si vous vous attendez à ce que votre application prenne en charge 5 000 utilisateurs simultanés, pensez à la budgétisation à moins de 1,3 Go de mémoire serveur pour l’application (ou ~ 273 Ko par utilisateur).

### <a name="opno-locsignalr-configuration"></a>configuration de SignalR

les applications Blazor Server utilisent ASP.NET Core SignalR pour communiquer avec le navigateur. [les conditions d’hébergement et de mise à l’échelle deSignalR](xref:signalr/publish-to-azure-web-app) s’appliquent aux applications Blazor Server.

Blazor fonctionne mieux lorsque vous utilisez WebSockets comme transport SignalR en raison d’une latence, d’une fiabilité et d’une [sécurité](xref:signalr/security)moindres. L’interrogation longue est utilisée par les SignalR lorsque WebSocket n’est pas disponible ou lorsque l’application est configurée explicitement pour utiliser l’interrogation longue. Lors du déploiement sur Azure App Service, configurez l’application pour qu’elle utilise WebSockets dans les paramètres Portail Azure pour le service. Pour plus d’informations sur la configuration de l’application pour Azure App Service, consultez les [instructions de publicationSignalR](xref:signalr/publish-to-azure-web-app).

#### <a name="azure-opno-locsignalr-service"></a>Service SignalR Azure

Nous vous recommandons d’utiliser le [service Azure SignalR](/azure/azure-signalr) pour les applications Blazor Server. Le service permet la mise à l’échelle d’une application Blazor Server vers un grand nombre de connexions SignalR simultanées. En outre, la portée mondiale et les centres de données haute performance du service SignalR contribuent de manière significative à réduire la latence en raison de la géographie. Pour configurer une application (et éventuellement approvisionner) le service Azure SignalR :

1. Activez le service pour prendre en charge les *sessions rémanentes*, où les clients sont [redirigés vers le même serveur lors du prérendu](xref:blazor/hosting-models#connection-to-the-server). Définissez l’option `ServerStickyMode` ou la valeur de configuration sur `Required`. En règle générale, une application crée la configuration à l’aide de l' **une** des approches suivantes :

   * `Startup.ConfigureServices`:
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * Configuration (utilisez l' **une** des approches suivantes) :
  
     * *appsettings.json* :

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * La **configuration** d’app service > **paramètres d’Application** dans le Portail Azure (**nom**: `Azure:SignalR:ServerStickyMode`, **valeur**: `Required`).

1. Créez un profil de publication Azure Apps dans Visual Studio pour l’application Blazor Server.
1. Ajoutez la dépendance du **service Azure SignalR** au profil. Si l’abonnement Azure n’a pas d’instance de service Azure SignalR existante à attribuer à l’application, sélectionnez **créer une nouvelle instance azure SignalR service** pour approvisionner une nouvelle instance de service.
1. Publiez l’application dans Azure.

#### <a name="iis"></a>IIS

Lorsque vous utilisez IIS, activez :

* [WebSocket sur IIS](xref:fundamentals/websockets#enabling-websockets-on-iis).
* [Sessions rémanentes avec application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

#### <a name="kubernetes"></a>Kubernetes

Créez une définition d’entrée avec les [Annotations Kubernetes suivantes pour les sessions rémanentes](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <ingress-name>
  annotations:
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/session-cookie-name: "affinity"
    nginx.ingress.kubernetes.io/session-cookie-expires: "14400"
    nginx.ingress.kubernetes.io/session-cookie-max-age: "14400"
```

#### <a name="linux-with-nginx"></a>Linux avec Nginx

Pour que SignalR WebSocket fonctionne correctement, définissez les en-têtes `Upgrade` et `Connection` du proxy comme suit :

```
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $connection_upgrade;
```

Pour plus d’informations, consultez [Nginx comme proxy WebSocket](https://www.nginx.com/blog/websocket-nginx/).

### <a name="measure-network-latency"></a>Mesurer la latence du réseau

L' [interopérabilité js](xref:blazor/javascript-interop) peut être utilisée pour mesurer la latence du réseau, comme le montre l’exemple suivant :

```razor
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

Pour une expérience d’interface utilisateur raisonnable, nous recommandons une latence d’interface utilisateur soutenue de 250 ms ou moins.
