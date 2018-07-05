---
uid: webhooks/receiving/handlers
title: Les gestionnaires ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Comment gérer les demandes dans ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.openlocfilehash: 7e45a97ac9d61b2d046984e5ede3be158b741b7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372697"
---
# <a name="aspnet-webhooks-handlers"></a>Gestionnaires ASP.NET WebHooks

Une fois que les demandes de WebHooks a été validée par un récepteur de WebHook, il est prêt à être traité par le code utilisateur. C’est là où *gestionnaires* se présentent sous. Gestionnaires dérivent le [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface, mais elle généralement utilise le [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe au lieu de dériver directement à partir de l’interface.

Une demande de WebHook peut être traitée par un ou plusieurs gestionnaires. Gestionnaires sont appelés dans l’ordre en fonction de leurs détenteurs respectifs *ordre* propriété allant de la plus basse à la plus élevée dont est un entier simple (suggéré pour être comprise entre 1 et 100) :

![Diagramme de propriété de commande Gestionnaire de WebHook](_static/Handlers.png)

Un gestionnaire peut éventuellement définir le *réponse* propriété sur le WebHookHandlerContext, ce qui entraînera le traitement arrêt et la réponse à envoyer comme la réponse HTTP au WebHook. Dans le cas ci-dessus, gestionnaire C ne sont pas appelée, car il a un ordre plus élevé que B et B définit la réponse.

Définition de la réponse est généralement uniquement pertinent pour WebHooks où la réponse peut transporter des informations sur l’API d’origine. C’est par exemple le cas avec des WebHooks Slack où la réponse est publiée sur le canal d'où provenance le WebHook. Gestionnaires peuvent définir la propriété de récepteur s’ils souhaitent uniquement recevoir des WebHooks à partir de ce récepteur particulière. Si vous ne définissez pas le destinataire, ils sont appelés pour chacun d'entre eux.

Une autre utilisation courante d’une réponse consiste à utiliser un *410 supprimé* réponse pour indiquer que le WebHook n’est plus actif, et aucuns autres demandes ne doivent être soumises.

Par défaut un gestionnaire sera appelé par tous les récepteurs de WebHook. Toutefois, si le *récepteur* propriété est définie sur le nom d’un gestionnaire ensuite ce gestionnaire reçoivent uniquement les requêtes de WebHook à partir de ce récepteur.

## <a name="processing-a-webhook"></a>Traitement d’un WebHook

Lorsqu’un gestionnaire est appelé, elle obtient un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenant des informations sur la demande de WebHook. Les données, en général, le corps de demande HTTP, sont disponibles à partir de la *données* propriété.

Le type de données est généralement les données de formulaire JSON ou HTML, mais il est possible d’effectuer un cast en un type plus spécifique si vous le souhaitez. Par exemple, les WebHooks personnalisés générés par ASP.NET WebHooks peut être castés au type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) comme suit :

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

  ## <a name="queued-processing"></a>Traitement en file d’attente

La plupart des expéditeurs WebHook renverra un WebHook si une réponse n’est pas générée au sein d’un certain nombre de secondes. Cela signifie que votre gestionnaire doit terminer le traitement dans ce laps de temps dans l’ordre pas pour qu’elle soit invoquée à nouveau.

Si le traitement prend plus de temps, ou il est mieux géré séparément le [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) peut être utilisé pour envoyer la demande de WebHook à une file d’attente, par exemple [file d’attente de stockage Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Un plan d’un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implémentation est fournie ici :

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
