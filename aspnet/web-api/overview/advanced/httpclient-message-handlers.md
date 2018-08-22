---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Gestionnaires de messages HttpClient dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 764244d1299d8cfcb59c3f15d63b42ebff4f6ac0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829282"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Gestionnaires de messages HttpClient dans l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et renvoie une réponse HTTP.

En règle générale, une série de gestionnaires de messages sont chaînées ensemble. Le premier gestionnaire reçoit une requête HTTP effectue un traitement et donne la demande au gestionnaire suivant. À un moment donné, la réponse est créée et remonte la chaîne. Ce modèle est appelé un *délégation* gestionnaire.

![](httpclient-message-handlers/_static/image1.png)

Du côté client, le **HttpClient** classe utilise un gestionnaire de messages pour traiter les demandes. Le gestionnaire par défaut est **HttpClientHandler**, qui envoie la demande sur le réseau et obtient la réponse à partir du serveur. Vous pouvez insérer des gestionnaires de messages personnalisés dans le pipeline client :

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> API Web ASP.NET utilise également des gestionnaires de messages côté serveur. Pour plus d’informations, consultez [gestionnaires de messages HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Gestionnaires de messages personnalisés

Pour écrire un gestionnaire de messages personnalisés, dérivez de **System.Net.Http.DelegatingHandler** et remplacer le **SendAsync** (méthode). Voici la signature de méthode :

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

La méthode accepte un **HttpRequestMessage** comme entrée et retourne de façon asynchrone un **HttpResponseMessage**. Une implémentation classique effectue les opérations suivantes :

1. Traiter le message de demande.
2. Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.
3. Le gestionnaire interne retourne un message de réponse. (Cette étape est asynchrone).
4. Traiter la réponse et le renvoyer à l’appelant.

L’exemple suivant montre un gestionnaire de messages qui ajoute un en-tête personnalisé à la demande sortante :

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

L’appel à `base.SendAsync` est asynchrone. Si le gestionnaire effectue tout travail après cet appel, utilisez le **await** mot clé pour reprendre l’exécution une fois la méthode terminée. L’exemple suivant montre un gestionnaire qui enregistre les codes d’erreur. La journalisation proprement dit n’est pas très intéressante, mais l’exemple montre comment procéder à la réponse dans le gestionnaire.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Ajout de gestionnaires de messages au Pipeline Client

Pour ajouter des gestionnaires personnalisés pour **HttpClient**, utilisez le **HttpClientFactory.Create** méthode :

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Gestionnaires de messages sont appelés dans l’ordre dans lequel vous les passez dans le **créer** (méthode). Étant donné que les gestionnaires sont imbriqués, le message de réponse sont transmises dans l’autre direction. Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.
