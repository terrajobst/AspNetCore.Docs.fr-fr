---
uid: webhooks/receiving/handlers
title: Les gestionnaires ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Comment gérer les demandes dans ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824294"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="31233-103">Gestionnaires ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="31233-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="31233-104">Une fois que les demandes de WebHooks a été validée par un récepteur de WebHook, il est prêt à être traité par le code utilisateur.</span><span class="sxs-lookup"><span data-stu-id="31233-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="31233-105">C’est là où *gestionnaires* se présentent sous.</span><span class="sxs-lookup"><span data-stu-id="31233-105">This is where *handlers* come in.</span></span> <span data-ttu-id="31233-106">Gestionnaires dérivent le [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface, mais elle généralement utilise le [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe au lieu de dériver directement à partir de l’interface.</span><span class="sxs-lookup"><span data-stu-id="31233-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="31233-107">Une demande de WebHook peut être traitée par un ou plusieurs gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="31233-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="31233-108">Gestionnaires sont appelés dans l’ordre en fonction de leurs détenteurs respectifs *ordre* propriété allant de la plus basse à la plus élevée dont est un entier simple (suggéré pour être comprise entre 1 et 100) :</span><span class="sxs-lookup"><span data-stu-id="31233-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagramme de propriété de commande Gestionnaire de WebHook](_static/Handlers.png)

<span data-ttu-id="31233-110">Un gestionnaire peut éventuellement définir le *réponse* propriété sur le WebHookHandlerContext, ce qui entraînera le traitement arrêt et la réponse à envoyer comme la réponse HTTP au WebHook.</span><span class="sxs-lookup"><span data-stu-id="31233-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="31233-111">Dans le cas ci-dessus, gestionnaire C ne sont pas appelée, car il a un ordre plus élevé que B et B définit la réponse.</span><span class="sxs-lookup"><span data-stu-id="31233-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="31233-112">Définition de la réponse est généralement uniquement pertinent pour WebHooks où la réponse peut transporter des informations sur l’API d’origine.</span><span class="sxs-lookup"><span data-stu-id="31233-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="31233-113">C’est par exemple le cas avec des WebHooks Slack où la réponse est publiée sur le canal d'où provenance le WebHook.</span><span class="sxs-lookup"><span data-stu-id="31233-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="31233-114">Gestionnaires peuvent définir la propriété de récepteur s’ils souhaitent uniquement recevoir des WebHooks à partir de ce récepteur particulière.</span><span class="sxs-lookup"><span data-stu-id="31233-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="31233-115">Si vous ne définissez pas le destinataire, ils sont appelés pour chacun d'entre eux.</span><span class="sxs-lookup"><span data-stu-id="31233-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="31233-116">Une autre utilisation courante d’une réponse consiste à utiliser un *410 supprimé* réponse pour indiquer que le WebHook n’est plus actif, et aucuns autres demandes ne doivent être soumises.</span><span class="sxs-lookup"><span data-stu-id="31233-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="31233-117">Par défaut un gestionnaire sera appelé par tous les récepteurs de WebHook.</span><span class="sxs-lookup"><span data-stu-id="31233-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="31233-118">Toutefois, si le *récepteur* propriété est définie sur le nom d’un gestionnaire ensuite ce gestionnaire reçoivent uniquement les requêtes de WebHook à partir de ce récepteur.</span><span class="sxs-lookup"><span data-stu-id="31233-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="31233-119">Traitement d’un WebHook</span><span class="sxs-lookup"><span data-stu-id="31233-119">Processing a WebHook</span></span>

<span data-ttu-id="31233-120">Lorsqu’un gestionnaire est appelé, elle obtient un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenant des informations sur la demande de WebHook.</span><span class="sxs-lookup"><span data-stu-id="31233-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="31233-121">Les données, en général, le corps de demande HTTP, sont disponibles à partir de la *données* propriété.</span><span class="sxs-lookup"><span data-stu-id="31233-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="31233-122">Le type de données est généralement les données de formulaire JSON ou HTML, mais il est possible d’effectuer un cast en un type plus spécifique si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="31233-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="31233-123">Par exemple, les WebHooks personnalisés générés par ASP.NET WebHooks peut être castés au type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) comme suit :</span><span class="sxs-lookup"><span data-stu-id="31233-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="31233-124">Traitement en file d’attente</span><span class="sxs-lookup"><span data-stu-id="31233-124">Queued Processing</span></span>

<span data-ttu-id="31233-125">La plupart des expéditeurs WebHook renverra un WebHook si une réponse n’est pas générée au sein d’un certain nombre de secondes.</span><span class="sxs-lookup"><span data-stu-id="31233-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="31233-126">Cela signifie que votre gestionnaire doit terminer le traitement dans ce laps de temps dans l’ordre pas pour qu’elle soit invoquée à nouveau.</span><span class="sxs-lookup"><span data-stu-id="31233-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="31233-127">Si le traitement prend plus de temps, ou il est mieux géré séparément le [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) peut être utilisé pour envoyer la demande de WebHook à une file d’attente, par exemple [file d’attente de stockage Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="31233-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="31233-128">Un plan d’un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implémentation est fournie ici :</span><span class="sxs-lookup"><span data-stu-id="31233-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
