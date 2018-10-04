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
# <a name="aspnet-webhooks-overview"></a>Vue d’ensemble des WebHooks ASP.NET

WebHooks est un modèle HTTP léger en fournissant un modèle pub/sub simple pour associer ensemble les API Web et des services SaaS. Quand un événement se produit dans un service, une notification est envoyée sous la forme d’une requête HTTP POST aux abonnés inscrits. La demande POST contient des informations sur l’événement qui rend possible pour le récepteur d’agir en conséquence.

En raison de leur simplicité, WebHooks sont déjà exposées par un grand nombre de services, y compris [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)et bien plus encore. Par exemple, un WebHook peut indiquer qu’un fichier a changé dans [Dropbox](http://dropbox.com/), une modification de code a été validée dans GitHub ou un paiement a été lancé dans [PayPal](http://www.paypal.com/), ou une carte a été créée dans [ Trello](http://www.trello.com/). Les possibilités sont infinies !

Microsoft ASP.NET WebHooks rend plus facile à envoyer et recevoir des WebHooks en tant que partie de votre application ASP.NET :

* Côté réception, il fournit un modèle commun pour recevoir et traiter des WebHooks à partir de n’importe quel nombre de fournisseurs de WebHook. Il s’agit dès le départ avec prise en charge de [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) et [Zendesk](https://www.zendesk.com/) , mais il est facile d’ajouter la prise en charge pour plus d’informations.

* Du côté envoi, il prend en charge pour la gestion et le stockage des abonnements ainsi que pour envoyer des notifications d’événements à l’ensemble approprié d’abonnés. Cela vous permet de définir votre propre jeu d’événements que les abonnés peuvent s’abonner à et les notifient lorsque les choses se produit.

Les deux parties peuvent être utilisées ensemble ou les unes des autres en fonction de votre scénario. Si vous devez uniquement recevoir des WebHooks à partir d’autres services, vous pouvez utiliser uniquement la partie destinataire ; Si vous souhaitez uniquement exposer des WebHooks pour d’autres personnes à utiliser, puis vous pouvez le faire simplement.

Le code cible ASP.NET Web API 2 et ASP.NET MVC 5 et est disponible en tant que [Open source sur GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Vue d’ensemble de WebHooks

WebHooks est un modèle, ce qui signifie qu’il varie comment elle est utilisée à partir de service au service, mais l’idée de base est le même. Vous pouvez considérer WebHooks comme un modèle pub/sub simple où un utilisateur peut s’abonner aux événements qui se produisent ailleurs. Les notifications d’événements sont propagées en tant que demandes HTTP POST contenant des informations sur l’événement lui-même.

En règle générale, la requête HTTP POST contient un objet JSON ou les données de formulaire HTML déterminées par l’expéditeur de WebHook, y compris des informations sur l’événement qui provoque le WebHook pour déclencher. Par exemple, un exemple d’un corps de demande POST du WebHook à partir de [GitHub](http://www.github.com/) ressemble à ceci en raison d’un problème en cours d’ouverture dans un référentiel particulier :

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

Pour vous assurer que le WebHook est en effet à partir de l’expéditeur initial, la requête POST est sécurisée d’une certaine façon et ensuite vérifiée par le récepteur. Par exemple, [GitHub WebHooks](https://developer.github.com/webhooks/) inclut un *X-Hub-Signature* en-tête HTTP avec un hachage du corps de la demande qui est vérifié par l’implémentation du récepteur sans que vous ayez à vous inquiéter.

Le flux de WebHook va généralement quelque chose comme ceci :

* L’expéditeur de WebHook expose des événements auxquels un client peut s’abonner. Les événements décrivent les modifications observables au système, par exemple un nouvel élément de données a été inséré, fin d’un processus ou autre chose.

* Le récepteur de WebHook s’abonne en inscrivant un WebHook composé de quatre éléments :

     1. Un URI pour lequel la notification d’événement doit être validée sous la forme d’une requête HTTP POST ;

     2. Un ensemble de filtres décrivant les événements pour lesquels le WebHook doit être déclenché ;

     3. Une clé secrète qui est utilisée pour signer la demande HTTP POST ;

     4. Données supplémentaires qui doit être inclus dans la requête HTTP POST. Cela peut par exemple être des champs d’en-tête HTTP supplémentaires ou des propriétés incluses dans le corps de la requête HTTP POST.

* Une fois qu’un événement se produit, les inscriptions de WebHook correspondantes sont trouvent et les requêtes HTTP POST sont envoyées. En règle générale, la génération des demandes HTTP POST sont retentées plusieurs fois si pour une raison quelconque le destinataire ne répond pas ou les résultats de la requête HTTP POST dans une réponse d’erreur.

## <a name="webhooks-processing-pipeline"></a>Pipeline de traitement de WebHooks

Le pipeline de traitement de Microsoft ASP.NET WebHooks pour WebHooks entrants ressemble à ceci :

![Pipeline de traitement de WebHooks ASP.NET](_static/WebHookReceivers.png)

Les deux concepts clés sont *récepteurs* et *gestionnaires*:

* *Récepteurs* sont responsables de gestion de la version particulière de WebHook à partir d’un expéditeur donné et pour mettre en œuvre des contrôles de sécurité pour vous assurer que la demande de WebHook est en effet à partir de l’expéditeur initial.

* *Gestionnaires* sont généralement où le code utilisateur exécute le traitement du WebHook particulier.

Dans les nœuds suivants, ces concepts sont décrits plus en détail.
