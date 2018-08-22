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
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="17232-102">Gestionnaires de messages HttpClient dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="17232-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="17232-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="17232-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="17232-104">Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et renvoie une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="17232-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="17232-105">En règle générale, une série de gestionnaires de messages sont chaînées ensemble.</span><span class="sxs-lookup"><span data-stu-id="17232-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="17232-106">Le premier gestionnaire reçoit une requête HTTP effectue un traitement et donne la demande au gestionnaire suivant.</span><span class="sxs-lookup"><span data-stu-id="17232-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="17232-107">À un moment donné, la réponse est créée et remonte la chaîne.</span><span class="sxs-lookup"><span data-stu-id="17232-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="17232-108">Ce modèle est appelé un *délégation* gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="17232-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="17232-109">Du côté client, le **HttpClient** classe utilise un gestionnaire de messages pour traiter les demandes.</span><span class="sxs-lookup"><span data-stu-id="17232-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="17232-110">Le gestionnaire par défaut est **HttpClientHandler**, qui envoie la demande sur le réseau et obtient la réponse à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="17232-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="17232-111">Vous pouvez insérer des gestionnaires de messages personnalisés dans le pipeline client :</span><span class="sxs-lookup"><span data-stu-id="17232-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="17232-112">API Web ASP.NET utilise également des gestionnaires de messages côté serveur.</span><span class="sxs-lookup"><span data-stu-id="17232-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="17232-113">Pour plus d’informations, consultez [gestionnaires de messages HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="17232-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="17232-114">Gestionnaires de messages personnalisés</span><span class="sxs-lookup"><span data-stu-id="17232-114">Custom Message Handlers</span></span>

<span data-ttu-id="17232-115">Pour écrire un gestionnaire de messages personnalisés, dérivez de **System.Net.Http.DelegatingHandler** et remplacer le **SendAsync** (méthode).</span><span class="sxs-lookup"><span data-stu-id="17232-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="17232-116">Voici la signature de méthode :</span><span class="sxs-lookup"><span data-stu-id="17232-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="17232-117">La méthode accepte un **HttpRequestMessage** comme entrée et retourne de façon asynchrone un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="17232-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="17232-118">Une implémentation classique effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="17232-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="17232-119">Traiter le message de demande.</span><span class="sxs-lookup"><span data-stu-id="17232-119">Process the request message.</span></span>
2. <span data-ttu-id="17232-120">Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.</span><span class="sxs-lookup"><span data-stu-id="17232-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="17232-121">Le gestionnaire interne retourne un message de réponse.</span><span class="sxs-lookup"><span data-stu-id="17232-121">The inner handler returns a response message.</span></span> <span data-ttu-id="17232-122">(Cette étape est asynchrone).</span><span class="sxs-lookup"><span data-stu-id="17232-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="17232-123">Traiter la réponse et le renvoyer à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="17232-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="17232-124">L’exemple suivant montre un gestionnaire de messages qui ajoute un en-tête personnalisé à la demande sortante :</span><span class="sxs-lookup"><span data-stu-id="17232-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="17232-125">L’appel à `base.SendAsync` est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="17232-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="17232-126">Si le gestionnaire effectue tout travail après cet appel, utilisez le **await** mot clé pour reprendre l’exécution une fois la méthode terminée.</span><span class="sxs-lookup"><span data-stu-id="17232-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="17232-127">L’exemple suivant montre un gestionnaire qui enregistre les codes d’erreur.</span><span class="sxs-lookup"><span data-stu-id="17232-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="17232-128">La journalisation proprement dit n’est pas très intéressante, mais l’exemple montre comment procéder à la réponse dans le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="17232-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="17232-129">Ajout de gestionnaires de messages au Pipeline Client</span><span class="sxs-lookup"><span data-stu-id="17232-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="17232-130">Pour ajouter des gestionnaires personnalisés pour **HttpClient**, utilisez le **HttpClientFactory.Create** méthode :</span><span class="sxs-lookup"><span data-stu-id="17232-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="17232-131">Gestionnaires de messages sont appelés dans l’ordre dans lequel vous les passez dans le **créer** (méthode).</span><span class="sxs-lookup"><span data-stu-id="17232-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="17232-132">Étant donné que les gestionnaires sont imbriqués, le message de réponse sont transmises dans l’autre direction.</span><span class="sxs-lookup"><span data-stu-id="17232-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="17232-133">Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="17232-133">That is, the last handler is the first to get the response message.</span></span>
