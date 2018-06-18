---
title: SignalR HubContext
author: rachelappel
description: Découvrez comment utiliser le service ASP.NET Core SignalR HubContext pour envoyer des notifications aux clients à partir d’en dehors d’un concentrateur.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubcontext
ms.openlocfilehash: 79b91a776a38a2e6810cc89ff0b8d15fe836ce66
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726071"
---
# <a name="send-messages-from-outside-a-hub"></a>Envoi des messages en dehors d’un concentrateur

Par [Mikael Mengistu](https://twitter.com/MikaelM_12)

Le concentrateur SignalR est l’abstraction de base pour envoyer des messages aux clients connectés au serveur SignalR. Il est également possible d’envoyer des messages à partir d’autres emplacements dans votre application à l’aide de la `IHubContext` service. Cet article explique comment accéder à un SignalR `IHubContext` pour envoyer des notifications aux clients à partir d’en dehors d’un concentrateur.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obtenir une instance de `IHubContext`

ASP.NET Core SignalR, vous pouvez accéder à une instance de `IHubContext` par injection de dépendances. Vous pouvez injecter une instance de `IHubContext` dans un autre service DI, intergiciel (middleware) ou un contrôleur. Utilisez l’instance pour envoyer des messages aux clients.

> [!NOTE]
> Cela diffère de ASP.NET SignalR qui permet de fournir l’accès aux GlobalHost le `IHubContext`. ASP.NET Core possède une infrastructure d’injection de dépendance qui supprime la nécessité pour ce singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Injecter une instance de `IHubContext` dans un contrôleur

Vous pouvez injecter une instance de `IHubContext` dans un contrôleur en l’ajoutant à votre constructeur :

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Désormais, avec un accès à une instance de `IHubContext`, vous pouvez appeler des méthodes de concentrateur comme si vous étiez dans le hub lui-même.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obtenir une instance de `IHubContext` dans l’intergiciel (middleware)

Accès le `IHubContext` dans le pipeline de l’intergiciel (middleware) comme suit :

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Lorsque les méthodes de concentrateur sont appelées à partir d’en dehors de la `Hub` de classe, il n’existe aucun appelant associé à l’appel. Par conséquent, il n’existe aucun accès à la `ConnectionId`, `Caller`, et `Others` propriétés.

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:signalr/get-started)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
