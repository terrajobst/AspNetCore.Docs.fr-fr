---
uid: webhooks/index
title: Vue d’ensemble des WebHooks ASP.NET | Microsoft Docs
author: rick-anderson
description: Introduction à ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "48253679"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="13ae5-103">Vue d’ensemble des WebHooks ASP.NET</span><span class="sxs-lookup"><span data-stu-id="13ae5-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="13ae5-104">WebHooks est un modèle HTTP léger en fournissant un modèle pub/sub simple pour associer ensemble les API Web et des services SaaS.</span><span class="sxs-lookup"><span data-stu-id="13ae5-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="13ae5-105">Quand un événement se produit dans un service, une notification est envoyée sous la forme d’une requête HTTP POST aux abonnés inscrits.</span><span class="sxs-lookup"><span data-stu-id="13ae5-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="13ae5-106">La demande POST contient des informations sur l’événement qui rend possible pour le récepteur d’agir en conséquence.</span><span class="sxs-lookup"><span data-stu-id="13ae5-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="13ae5-107">En raison de leur simplicité, WebHooks sont déjà exposées par un grand nombre de services, y compris [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="13ae5-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="13ae5-108">Par exemple, un WebHook peut indiquer qu’un fichier a changé dans [Dropbox](http://dropbox.com/), une modification de code a été validée dans GitHub ou un paiement a été lancé dans [PayPal](http://www.paypal.com/), ou une carte a été créée dans [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="13ae5-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="13ae5-109">Les possibilités sont infinies !</span><span class="sxs-lookup"><span data-stu-id="13ae5-109">The possibilities are endless!</span></span>

<span data-ttu-id="13ae5-110">Microsoft ASP.NET WebHooks rend plus facile à envoyer et recevoir des WebHooks en tant que partie de votre application ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="13ae5-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="13ae5-111">Côté réception, il fournit un modèle commun pour recevoir et traiter des WebHooks à partir de n’importe quel nombre de fournisseurs de WebHook.</span><span class="sxs-lookup"><span data-stu-id="13ae5-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="13ae5-112">Il s’agit dès le départ avec prise en charge de [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) et [Zendesk](https://www.zendesk.com/) , mais il est facile d’ajouter la prise en charge pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="13ae5-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="13ae5-113">Du côté envoi, il prend en charge pour la gestion et le stockage des abonnements ainsi que pour envoyer des notifications d’événements à l’ensemble approprié d’abonnés.</span><span class="sxs-lookup"><span data-stu-id="13ae5-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="13ae5-114">Cela vous permet de définir votre propre jeu d’événements que les abonnés peuvent s’abonner à et les notifient lorsque les choses se produit.</span><span class="sxs-lookup"><span data-stu-id="13ae5-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="13ae5-115">Les deux parties peuvent être utilisées ensemble ou les unes des autres en fonction de votre scénario.</span><span class="sxs-lookup"><span data-stu-id="13ae5-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="13ae5-116">Si vous devez uniquement recevoir des WebHooks à partir d’autres services, vous pouvez utiliser uniquement la partie destinataire ; Si vous souhaitez uniquement exposer des WebHooks pour d’autres personnes à utiliser, puis vous pouvez le faire simplement.</span><span class="sxs-lookup"><span data-stu-id="13ae5-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="13ae5-117">Le code cible ASP.NET Web API 2 et ASP.NET MVC 5 et est disponible en tant que [Open source sur GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="13ae5-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="13ae5-118">Vue d’ensemble de WebHooks</span><span class="sxs-lookup"><span data-stu-id="13ae5-118">WebHooks Overview</span></span>

<span data-ttu-id="13ae5-119">WebHooks est un modèle, ce qui signifie qu’il varie comment elle est utilisée à partir de service au service, mais l’idée de base est le même.</span><span class="sxs-lookup"><span data-stu-id="13ae5-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="13ae5-120">Vous pouvez considérer WebHooks comme un modèle pub/sub simple où un utilisateur peut s’abonner aux événements qui se produisent ailleurs.</span><span class="sxs-lookup"><span data-stu-id="13ae5-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="13ae5-121">Les notifications d’événements sont propagées en tant que demandes HTTP POST contenant des informations sur l’événement lui-même.</span><span class="sxs-lookup"><span data-stu-id="13ae5-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="13ae5-122">En règle générale, la requête HTTP POST contient un objet JSON ou les données de formulaire HTML déterminées par l’expéditeur de WebHook, y compris des informations sur l’événement qui provoque le WebHook pour déclencher.</span><span class="sxs-lookup"><span data-stu-id="13ae5-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="13ae5-123">Par exemple, un exemple d’un corps de demande POST du WebHook à partir de [GitHub](http://www.github.com/) ressemble à ceci en raison d’un problème en cours d’ouverture dans un référentiel particulier :</span><span class="sxs-lookup"><span data-stu-id="13ae5-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="13ae5-124">Pour vous assurer que le WebHook est en effet à partir de l’expéditeur initial, la requête POST est sécurisée d’une certaine façon et ensuite vérifiée par le récepteur.</span><span class="sxs-lookup"><span data-stu-id="13ae5-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="13ae5-125">Par exemple, [GitHub WebHooks](https://developer.github.com/webhooks/) inclut un *X-Hub-Signature* en-tête HTTP avec un hachage du corps de la demande qui est vérifié par l’implémentation du récepteur sans que vous ayez à vous inquiéter.</span><span class="sxs-lookup"><span data-stu-id="13ae5-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="13ae5-126">Le flux de WebHook va généralement quelque chose comme ceci :</span><span class="sxs-lookup"><span data-stu-id="13ae5-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="13ae5-127">L’expéditeur de WebHook expose des événements auxquels un client peut s’abonner.</span><span class="sxs-lookup"><span data-stu-id="13ae5-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="13ae5-128">Les événements décrivent les modifications observables au système, par exemple un nouvel élément de données a été inséré, fin d’un processus ou autre chose.</span><span class="sxs-lookup"><span data-stu-id="13ae5-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="13ae5-129">Le récepteur de WebHook s’abonne en inscrivant un WebHook composé de quatre éléments :</span><span class="sxs-lookup"><span data-stu-id="13ae5-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="13ae5-130">Un URI pour lequel la notification d’événement doit être validée sous la forme d’une requête HTTP POST ;</span><span class="sxs-lookup"><span data-stu-id="13ae5-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="13ae5-131">Un ensemble de filtres décrivant les événements pour lesquels le WebHook doit être déclenché ;</span><span class="sxs-lookup"><span data-stu-id="13ae5-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="13ae5-132">Une clé secrète qui est utilisée pour signer la demande HTTP POST ;</span><span class="sxs-lookup"><span data-stu-id="13ae5-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="13ae5-133">Données supplémentaires qui doit être inclus dans la requête HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="13ae5-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="13ae5-134">Cela peut par exemple être des champs d’en-tête HTTP supplémentaires ou des propriétés incluses dans le corps de la requête HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="13ae5-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="13ae5-135">Une fois qu’un événement se produit, les inscriptions de WebHook correspondantes sont trouvent et les requêtes HTTP POST sont envoyées.</span><span class="sxs-lookup"><span data-stu-id="13ae5-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="13ae5-136">En règle générale, la génération des demandes HTTP POST sont retentées plusieurs fois si pour une raison quelconque le destinataire ne répond pas ou les résultats de la requête HTTP POST dans une réponse d’erreur.</span><span class="sxs-lookup"><span data-stu-id="13ae5-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="13ae5-137">Pipeline de traitement de WebHooks</span><span class="sxs-lookup"><span data-stu-id="13ae5-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="13ae5-138">Le pipeline de traitement de Microsoft ASP.NET WebHooks pour WebHooks entrants ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="13ae5-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Pipeline de traitement de WebHooks ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="13ae5-140">Les deux concepts clés sont *récepteurs* et *gestionnaires*:</span><span class="sxs-lookup"><span data-stu-id="13ae5-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="13ae5-141">*Récepteurs* sont responsables de gestion de la version particulière de WebHook à partir d’un expéditeur donné et pour mettre en œuvre des contrôles de sécurité pour vous assurer que la demande de WebHook est en effet à partir de l’expéditeur initial.</span><span class="sxs-lookup"><span data-stu-id="13ae5-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="13ae5-142">*Gestionnaires* sont généralement où le code utilisateur exécute le traitement du WebHook particulier.</span><span class="sxs-lookup"><span data-stu-id="13ae5-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="13ae5-143">Dans les nœuds suivants, ces concepts sont décrits plus en détail.</span><span class="sxs-lookup"><span data-stu-id="13ae5-143">In the following nodes these concepts are described in more details.</span></span>
