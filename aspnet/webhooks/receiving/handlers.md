---
uid: webhooks/receiving/handlers
title: "Gestionnaires d’ASP.NET WebHooks | Documents Microsoft"
author: rick-anderson
description: "Comment gérer les demandes de WebHooks de ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 12acae0883c12698a8f9c2150623ba792303e7ef
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="82e7c-103">Gestionnaires d’ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="82e7c-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="82e7c-104">Une fois que les demandes de WebHooks a été validée par un récepteur de WebHook, il est prêt à être traité par le code utilisateur.</span><span class="sxs-lookup"><span data-stu-id="82e7c-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="82e7c-105">C’est là *gestionnaires* sont disponibles dans.</span><span class="sxs-lookup"><span data-stu-id="82e7c-105">This is where *handlers* come in.</span></span> <span data-ttu-id="82e7c-106">Gestionnaires dérivent la [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface, mais généralement utilise le [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe au lieu de dériver directement à partir de l’interface.</span><span class="sxs-lookup"><span data-stu-id="82e7c-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="82e7c-107">Une demande de WebHook peut être traitée par un ou plusieurs gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="82e7c-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="82e7c-108">Gestionnaires sont appelés dans l’ordre en fonction de leurs détenteurs respectifs *commande* propriété allant de la plus basse à la plus élevée dont est un entier simple (suggéré pour être compris entre 1 et 100) :</span><span class="sxs-lookup"><span data-stu-id="82e7c-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Schéma de propriété WebHook Gestionnaire ordre](_static/Handlers.png)

<span data-ttu-id="82e7c-110">Un gestionnaire peut éventuellement définir le *réponse* propriété sur le WebHookHandlerContext, ce qui entraînera le traitement à l’arrêt et la réponse à envoyer comme la réponse HTTP pour le WebHook.</span><span class="sxs-lookup"><span data-stu-id="82e7c-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="82e7c-111">Dans le cas ci-dessus, gestionnaire C ne sera pas appelée, car il a un ordre plus élevé que B et B définit la réponse.</span><span class="sxs-lookup"><span data-stu-id="82e7c-111">In the case above, Handler C won’t get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="82e7c-112">Définition de la réponse est généralement uniquement pertinente pour WebHooks où la réponse peut contenir des informations sur l’API d’origine.</span><span class="sxs-lookup"><span data-stu-id="82e7c-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="82e7c-113">Il s’agit par exemple le cas avec le WebHooks Slack où la réponse est publiée sur le canal d'où provenance le WebHook.</span><span class="sxs-lookup"><span data-stu-id="82e7c-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="82e7c-114">Les gestionnaires peuvent définir la propriété de récepteur s’ils souhaitent uniquement recevoir WebHooks ce destinataire particulier.</span><span class="sxs-lookup"><span data-stu-id="82e7c-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="82e7c-115">Si elles ne définissent pas le récepteur, ils sont appelés pour chacun d’eux.</span><span class="sxs-lookup"><span data-stu-id="82e7c-115">If they don’t set the receiver they are called for all of them.</span></span>

<span data-ttu-id="82e7c-116">Une autre utilisation courante d’une réponse consiste à utiliser un *Gone 410* réponse pour indiquer que le WebHook n’est plus actif et aucune demande supplémentaire ne doit être soumis.</span><span class="sxs-lookup"><span data-stu-id="82e7c-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="82e7c-117">Par défaut, un gestionnaire sera appelé par tous les destinataires de WebHook.</span><span class="sxs-lookup"><span data-stu-id="82e7c-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="82e7c-118">Toutefois, si le *récepteur* est définie sur le nom d’un gestionnaire, puis ce gestionnaire reçoive uniquement les demandes de WebHook à partir de ce dernier.</span><span class="sxs-lookup"><span data-stu-id="82e7c-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="82e7c-119">Le traitement d’un WebHook</span><span class="sxs-lookup"><span data-stu-id="82e7c-119">Processing a WebHook</span></span>

<span data-ttu-id="82e7c-120">Lorsqu’un gestionnaire est appelé, il obtient un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenant des informations sur la demande de WebHook.</span><span class="sxs-lookup"><span data-stu-id="82e7c-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="82e7c-121">Les données, en général, le corps de demande HTTP, sont disponibles à partir de la *données* propriété.</span><span class="sxs-lookup"><span data-stu-id="82e7c-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="82e7c-122">Le type de données est généralement les données de formulaire JSON ou HTML, mais il est possible d’effectuer un cast en un type plus spécifique si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="82e7c-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="82e7c-123">Par exemple, le WebHooks personnalisées générées par des WebHooks de ASP.NET peut être converti en type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) comme suit :</span><span class="sxs-lookup"><span data-stu-id="82e7c-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="82e7c-124">Traitement en file d’attente</span><span class="sxs-lookup"><span data-stu-id="82e7c-124">Queued Processing</span></span>

<span data-ttu-id="82e7c-125">La plupart des expéditeurs de WebHook renverra un WebHook si une réponse n’est pas générée dans un certain nombre de secondes.</span><span class="sxs-lookup"><span data-stu-id="82e7c-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="82e7c-126">Cela signifie que votre gestionnaire doit effectuer le traitement dans ce laps de temps dans l’ordre, pas pour pouvoir être appelée à nouveau.</span><span class="sxs-lookup"><span data-stu-id="82e7c-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="82e7c-127">Si le traitement prend plus de temps, ou il est mieux géré séparément le [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) peut être utilisé pour envoyer la demande de WebHook dans une file d’attente, par exemple [file d’attente de stockage Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="82e7c-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="82e7c-128">Un plan d’un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implémentation est fournie ici :</span><span class="sxs-lookup"><span data-stu-id="82e7c-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
