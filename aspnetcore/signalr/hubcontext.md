---
title: SignalR HubContext
author: tdykstra
description: Découvrez comment utiliser le service ASP.NET Core SignalR HubContext pour envoyer des notifications aux clients à partir en dehors d’un concentrateur.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: a02588dc98283a375e9deb7c8561c59f6d886eb0
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835876"
---
# <a name="send-messages-from-outside-a-hub"></a>Envoyer des messages depuis en dehors d’un concentrateur

Par [Mikael Mengistu](https://twitter.com/MikaelM_12)

Le concentrateur SignalR est l’abstraction core pour envoyer des messages aux clients connectés au serveur SignalR. Il est également possible d’envoyer des messages à partir d’autres emplacements dans votre application en utilisant la `IHubContext` service. Cet article explique comment accéder à un SignalR `IHubContext` pour envoyer des notifications aux clients à partir en dehors d’un concentrateur.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obtenir une instance de `IHubContext`

ASP.NET Core SignalR, vous pouvez accéder à une instance de `IHubContext` via l’injection de dépendance. Vous pouvez injecter une instance de `IHubContext` dans un contrôleur, intergiciel (middleware) ou un autre service de l’injection de dépendances. Utilisez l’instance pour envoyer des messages aux clients.

> [!NOTE]
> Cela diffère d’ASP.NET 4.x SignalR qui permet de fournir un accès aux GlobalHost le `IHubContext`. ASP.NET Core offre une infrastructure d’injection de dépendance qui n’est plus nécessaire pour ce singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Injecter une instance de `IHubContext` dans un contrôleur

Vous pouvez injecter une instance de `IHubContext` dans un contrôleur en l’ajoutant à votre constructeur :

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Maintenant, avec accès à une instance de `IHubContext`, vous pouvez appeler des méthodes de concentrateur comme si vous étiez dans le hub lui-même.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obtenir une instance de `IHubContext` dans l’intergiciel (middleware)

Accès le `IHubContext` dans le pipeline d’intergiciel (middleware) comme suit :

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
> Lorsque les méthodes de concentrateur sont appelées à partir d’en dehors de la `Hub` classe, il n’existe aucun appelant associé à l’appel. Par conséquent, il n’existe aucun accès à la `ConnectionId`, `Caller`, et `Others` propriétés.

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
