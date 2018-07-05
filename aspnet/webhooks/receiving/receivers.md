---
uid: webhooks/receiving/receivers
title: WebHooks ASP.NET récepteurs | Microsoft Docs
author: rick-anderson
description: Récepteurs de WebHooks ASP.NET
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.openlocfilehash: be1f7dcef60642231af9c593eb7329ede7c2094f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374741"
---
# <a name="aspnet-webhooks-receivers"></a>Récepteurs de WebHooks ASP.NET

Réception de WebHooks dépend de qui est l’expéditeur. Il y a parfois l’inscription d’un WebHook vérification que l’abonné est vraiment à l’écoute des étapes supplémentaires. Certains WebHooks fournissent un modèle de push et pull où la requête HTTP POST contient uniquement une référence pour les informations d’événement qui est ensuite être récupérés séparément. Fréquence à laquelle le modèle de sécurité varie un peu.

L’objectif de Microsoft ASP.NET WebHooks consiste à rendre plus simple et plus cohérente pour rattacher votre API sans perdre trop de temps à deviner comment gérer toute variante particulier de WebHooks.

Un récepteur de WebHook est chargé d’accepter et de vérification des WebHooks à partir d’un expéditeur particulier. Un récepteur de WebHook peut prendre en charge un nombre quelconque de WebHooks, chacun avec sa propre configuration. Par exemple, le récepteur de GitHub WebHook peut accepter des WebHooks à partir de n’importe quel nombre de dépôts GitHub.

## <a name="webhook-receiver-uris"></a>URI de récepteur de WebHook

En installant Microsoft ASP.NET WebHooks, vous obtenez un contrôleur de WebHook général qui accepte les requêtes de WebHook à partir d’un nombre très flexible de services. Lorsqu’une requête arrive, il choisit le destinataire approprié que vous avez installés pour gérer un expéditeur particulier de WebHook.

L’URI de ce contrôleur est l’URI du WebHook qui vous inscrivez auprès du service et du formulaire :

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Pour des raisons de sécurité, les nombreux destinataires WebHook nécessitent que l’URI est un *https* URI et dans certains cas, il doit également contenir un paramètre de requête supplémentaire qui est utilisé pour garantir que seul le destinataire prévu peut envoyer des WebHooks à l’URI ci-dessus .

Le <em> <receiver> </em> composant est le nom du récepteur, par exemple <em>github</em> ou <em>slack</em>.

Le *{id}* est un identificateur facultatif qui peut être utilisé pour identifier une configuration de récepteur de WebHook particulière. Cela peut servir à inscrire N WebHooks avec un destinataire particulier. Par exemple, les URI suivants trois utilisable pour vous inscrire à trois WebHooks indépendants :

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>L’installation d’un récepteur de WebHook

Pour recevoir des WebHooks à l’aide de Microsoft ASP.NET WebHooks, vous installez tout d’abord le package Nuget pour le fournisseur de WebHook ou les fournisseurs que vous souhaitez recevoir depuis. Les packages Nuget sont nommés [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) où la dernière partie indique le service pris en charge. Exemple :

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) prend en charge pour la réception des WebHooks à partir de GitHub et [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) prend en charge pour la réception générés par ASP des WebHooks. NET WebHooks.

Prêt à l’emploi, vous pouvez trouver la prise en charge pour Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello et WordPress, mais il est possible prendre en charge n’importe quel nombre d’autres fournisseurs.

## <a name="configuring-a-webhook-receiver"></a>Configuration d’un récepteur de WebHook

Les récepteurs de WebHook sont configurés via la [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interface et des implémentations particulières de cette interface peuvent être inscrit à l’aide de n’importe quel modèle d’injection de dépendance. L’implémentation par défaut utilise les paramètres d’Application qui peut être définie dans le fichier Web.config, ou, si vous utilisez Azure Web Apps, peut être définie via la [Azure Portal](https://portal.azure.com/).

![Paramètres d’application Azure](_static/AzureAppSettings.png)

Le format pour les clés de paramètre d’Application est le suivant :

```
MS_WebHookReceiverSecret_<receiver>
```

La valeur est une liste séparée par des virgules de valeurs correspondant à la *{id}* valeurs pour lesquelles WebHooks ont été enregistrées, par exemple :

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>L’initialisation d’un récepteur de WebHook

Les récepteurs de WebHook sont initialisés en les inscrivant, généralement dans le *WebApiConfig* classe statique, par exemple :

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
