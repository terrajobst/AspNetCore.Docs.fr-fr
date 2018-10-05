---
title: SignalR HubContext
author: tdykstra
description: Découvrez comment utiliser le service ASP.NET Core SignalR HubContext pour envoyer des notifications aux clients à partir en dehors d’un concentrateur.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 2d7d37b655bf7dbb71b321919314bbb8bef8db17
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44339976"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="668dd-103">Envoyer des messages depuis en dehors d’un concentrateur</span><span class="sxs-lookup"><span data-stu-id="668dd-103">Send messages from outside a hub</span></span>

<span data-ttu-id="668dd-104">Par [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="668dd-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="668dd-105">Le hub SignalR est l’abstraction pour envoyer des messages aux clients connectés au serveur SignalR.</span><span class="sxs-lookup"><span data-stu-id="668dd-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="668dd-106">Il est également possible d’envoyer des messages à partir d’autres emplacements dans votre application en utilisant le service `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="668dd-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="668dd-107">Cet article explique comment accéder à un `IHubContext` SignalR pour envoyer des notifications aux clients en dehors d’un hub.</span><span class="sxs-lookup"><span data-stu-id="668dd-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="668dd-108">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="668dd-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="668dd-109">Obtenir une instance de `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="668dd-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="668dd-110">ASP.NET Core SignalR, vous pouvez accéder à une instance de `IHubContext` via l’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="668dd-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="668dd-111">Vous pouvez injecter une instance de `IHubContext` dans un contrôleur, intergiciel (middleware) ou un autre service de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="668dd-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="668dd-112">Utilisez l’instance pour envoyer des messages aux clients.</span><span class="sxs-lookup"><span data-stu-id="668dd-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="668dd-113">Cela diffère d’ASP.NET 4.x SignalR qui utilisait GlobalHost pour accéder au `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="668dd-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="668dd-114">ASP.NET Core offre une infrastructure d’injection de dépendance qui supprime le besoin de ce singleton global.</span><span class="sxs-lookup"><span data-stu-id="668dd-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="668dd-115">Injecter une instance de `IHubContext` dans un contrôleur</span><span class="sxs-lookup"><span data-stu-id="668dd-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="668dd-116">Vous pouvez injecter une instance de `IHubContext` dans un contrôleur en l’ajoutant à votre constructeur :</span><span class="sxs-lookup"><span data-stu-id="668dd-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="668dd-117">Maintenant, avec accès à une instance de `IHubContext`, vous pouvez appeler des méthodes de concentrateur comme si vous étiez dans le hub lui-même.</span><span class="sxs-lookup"><span data-stu-id="668dd-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="668dd-118">Obtenir une instance de `IHubContext` dans l’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="668dd-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="668dd-119">Accès le `IHubContext` dans le pipeline d’intergiciel (middleware) comme suit :</span><span class="sxs-lookup"><span data-stu-id="668dd-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="668dd-120">Lorsque les méthodes du hub sont appelées d’en dehors de la classe `Hub`, il n'y a aucun appelant associé à l’appel.</span><span class="sxs-lookup"><span data-stu-id="668dd-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="668dd-121">Par conséquent, il n'y a aucun accès aux propriétés `ConnectionId`, `Caller` et `Others`.</span><span class="sxs-lookup"><span data-stu-id="668dd-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="668dd-122">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="668dd-122">Related resources</span></span>

* [<span data-ttu-id="668dd-123">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="668dd-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="668dd-124">Hubs</span><span class="sxs-lookup"><span data-stu-id="668dd-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="668dd-125">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="668dd-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
