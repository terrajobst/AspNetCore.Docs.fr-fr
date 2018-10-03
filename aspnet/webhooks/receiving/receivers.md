---
uid: webhooks/receiving/receivers
title: WebHooks ASP.NET récepteurs | Microsoft Docs
author: rick-anderson
description: Récepteurs de WebHooks ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: d771a588b23abcd7b1b33e694af17b219683fc48
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860912"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="14ca8-103">Récepteurs de WebHooks ASP.NET</span><span class="sxs-lookup"><span data-stu-id="14ca8-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="14ca8-104">Réception de WebHooks dépend de qui est l’expéditeur.</span><span class="sxs-lookup"><span data-stu-id="14ca8-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="14ca8-105">Il y a parfois l’inscription d’un WebHook vérification que l’abonné est vraiment à l’écoute des étapes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="14ca8-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="14ca8-106">Certains WebHooks fournissent un modèle de push et pull où la requête HTTP POST contient uniquement une référence pour les informations d’événement qui est ensuite être récupérés séparément.</span><span class="sxs-lookup"><span data-stu-id="14ca8-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="14ca8-107">Fréquence à laquelle le modèle de sécurité varie un peu.</span><span class="sxs-lookup"><span data-stu-id="14ca8-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="14ca8-108">L’objectif de Microsoft ASP.NET WebHooks consiste à rendre plus simple et plus cohérente pour rattacher votre API sans perdre trop de temps à deviner comment gérer toute variante particulier de WebHooks.</span><span class="sxs-lookup"><span data-stu-id="14ca8-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="14ca8-109">Un récepteur de WebHook est chargé d’accepter et de vérification des WebHooks à partir d’un expéditeur particulier.</span><span class="sxs-lookup"><span data-stu-id="14ca8-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="14ca8-110">Un récepteur de WebHook peut prendre en charge un nombre quelconque de WebHooks, chacun avec sa propre configuration.</span><span class="sxs-lookup"><span data-stu-id="14ca8-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="14ca8-111">Par exemple, le récepteur de GitHub WebHook peut accepter des WebHooks à partir de n’importe quel nombre de dépôts GitHub.</span><span class="sxs-lookup"><span data-stu-id="14ca8-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="14ca8-112">URI de récepteur de WebHook</span><span class="sxs-lookup"><span data-stu-id="14ca8-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="14ca8-113">En installant Microsoft ASP.NET WebHooks, vous obtenez un contrôleur de WebHook général qui accepte les requêtes de WebHook à partir d’un nombre très flexible de services.</span><span class="sxs-lookup"><span data-stu-id="14ca8-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="14ca8-114">Lorsqu’une requête arrive, il choisit le destinataire approprié que vous avez installés pour gérer un expéditeur particulier de WebHook.</span><span class="sxs-lookup"><span data-stu-id="14ca8-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="14ca8-115">L’URI de ce contrôleur est l’URI du WebHook qui vous inscrivez auprès du service et du formulaire :</span><span class="sxs-lookup"><span data-stu-id="14ca8-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="14ca8-116">Pour des raisons de sécurité, les nombreux destinataires WebHook nécessitent que l’URI est un *https* URI et dans certains cas, il doit également contenir un paramètre de requête supplémentaire qui est utilisé pour garantir que seul le destinataire prévu peut envoyer des WebHooks à l’URI ci-dessus .</span><span class="sxs-lookup"><span data-stu-id="14ca8-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="14ca8-117">Le `<receiver>` composant est le nom du récepteur, par exemple `github` ou `slack`.</span><span class="sxs-lookup"><span data-stu-id="14ca8-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="14ca8-118">Le *{id}* est un identificateur facultatif qui peut être utilisé pour identifier une configuration de récepteur de WebHook particulière.</span><span class="sxs-lookup"><span data-stu-id="14ca8-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="14ca8-119">Cela peut servir à inscrire N WebHooks avec un destinataire particulier.</span><span class="sxs-lookup"><span data-stu-id="14ca8-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="14ca8-120">Par exemple, les URI suivants trois utilisable pour vous inscrire à trois WebHooks indépendants :</span><span class="sxs-lookup"><span data-stu-id="14ca8-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="14ca8-121">L’installation d’un récepteur de WebHook</span><span class="sxs-lookup"><span data-stu-id="14ca8-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="14ca8-122">Pour recevoir des WebHooks à l’aide de Microsoft ASP.NET WebHooks, vous installez tout d’abord le package Nuget pour le fournisseur de WebHook ou les fournisseurs que vous souhaitez recevoir depuis.</span><span class="sxs-lookup"><span data-stu-id="14ca8-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="14ca8-123">Les packages Nuget sont nommés [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) où la dernière partie indique le service pris en charge.</span><span class="sxs-lookup"><span data-stu-id="14ca8-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="14ca8-124">Exemple :</span><span class="sxs-lookup"><span data-stu-id="14ca8-124">For example</span></span>

<span data-ttu-id="14ca8-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) prend en charge pour la réception des WebHooks à partir de GitHub et [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) prend en charge pour la réception générés par ASP des WebHooks. NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="14ca8-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="14ca8-126">Prêt à l’emploi, vous pouvez trouver la prise en charge pour Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello et WordPress, mais il est possible prendre en charge n’importe quel nombre d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="14ca8-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="14ca8-127">Configuration d’un récepteur de WebHook</span><span class="sxs-lookup"><span data-stu-id="14ca8-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="14ca8-128">Les récepteurs de WebHook sont configurés via la [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interface et des implémentations particulières de cette interface peuvent être inscrit à l’aide de n’importe quel modèle d’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="14ca8-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="14ca8-129">L’implémentation par défaut utilise les paramètres d’Application qui peut être définie dans le fichier Web.config, ou, si vous utilisez Azure Web Apps, peut être définie via la [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="14ca8-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Paramètres d’application Azure](_static/AzureAppSettings.png)

<span data-ttu-id="14ca8-131">Le format pour les clés de paramètre d’Application est le suivant :</span><span class="sxs-lookup"><span data-stu-id="14ca8-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="14ca8-132">La valeur est une liste séparée par des virgules de valeurs correspondant à la *{id}* valeurs pour lesquelles WebHooks ont été enregistrées, par exemple :</span><span class="sxs-lookup"><span data-stu-id="14ca8-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="14ca8-133">L’initialisation d’un récepteur de WebHook</span><span class="sxs-lookup"><span data-stu-id="14ca8-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="14ca8-134">Les récepteurs de WebHook sont initialisés en les inscrivant, généralement dans le *WebApiConfig* classe statique, par exemple :</span><span class="sxs-lookup"><span data-stu-id="14ca8-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
