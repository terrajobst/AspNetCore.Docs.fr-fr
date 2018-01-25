---
title: "Fonctionnalités de demande dans ASP.NET Core"
author: ardalis
description: "En savoir plus sur les détails d’implémentation de serveur web liés aux requêtes HTTP et les réponses qui sont définies dans les interfaces pour ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: f0e371f5ea6c6688ef32adcacf667a412e4625e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="71e0a-103">Fonctionnalités de demande dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71e0a-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="71e0a-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="71e0a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="71e0a-105">Les détails d’implémentation d’un serveur web relatifs aux requêtes HTTP et aux réponses sont définis dans les interfaces.</span><span class="sxs-lookup"><span data-stu-id="71e0a-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="71e0a-106">Ces interfaces sont utilisées par les implémentations de serveur et d’intergiciel (middleware) pour créer et modifier le pipeline d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="71e0a-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="71e0a-107">Interfaces de fonctionnalité</span><span class="sxs-lookup"><span data-stu-id="71e0a-107">Feature interfaces</span></span>

<span data-ttu-id="71e0a-108">ASP.NET Core définit un nombre d’interfaces de fonctionnalité HTTP dans `Microsoft.AspNetCore.Http.Features` utilisés par les serveurs pour identifier les fonctionnalités prises en charge.</span><span class="sxs-lookup"><span data-stu-id="71e0a-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="71e0a-109">Les interfaces suivantes de la fonctionnalité gérer les demandes et renvoient des réponses :</span><span class="sxs-lookup"><span data-stu-id="71e0a-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="71e0a-110">`IHttpRequestFeature`Définit la structure d’une requête HTTP, y compris le protocole de chemin d’accès, chaîne de requête, en-têtes et corps.</span><span class="sxs-lookup"><span data-stu-id="71e0a-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="71e0a-111">`IHttpResponseFeature`Définit la structure d’une réponse HTTP, y compris le code d’état, en-têtes et corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="71e0a-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="71e0a-112">`IHttpAuthenticationFeature`Définit la prise en charge pour l’identification des utilisateurs basés sur un `ClaimsPrincipal` et en spécifiant un gestionnaire d’authentification.</span><span class="sxs-lookup"><span data-stu-id="71e0a-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="71e0a-113">`IHttpUpgradeFeature`Définit la prise en charge pour [mises à niveau HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), qui permettent au client de spécifier les autres protocoles qu’elle souhaite utiliser si le serveur souhaite faire basculer les protocoles.</span><span class="sxs-lookup"><span data-stu-id="71e0a-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="71e0a-114">`IHttpBufferingFeature`Définit les méthodes permettant de désactiver la mise en mémoire tampon des réponses aux demandes et/ou.</span><span class="sxs-lookup"><span data-stu-id="71e0a-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="71e0a-115">`IHttpConnectionFeature`Définit les propriétés pour les ports et les adresses locales et distantes.</span><span class="sxs-lookup"><span data-stu-id="71e0a-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="71e0a-116">`IHttpRequestLifetimeFeature`Définit la prise en charge de l’abandon de connexions, ou détecter si une demande a été arrêtée prématurément, tels que par une déconnexion du client.</span><span class="sxs-lookup"><span data-stu-id="71e0a-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="71e0a-117">`IHttpSendFileFeature`Définit une méthode pour envoyer des fichiers de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="71e0a-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="71e0a-118">`IHttpWebSocketFeature`Définit une API de prise en charge de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="71e0a-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="71e0a-119">`IHttpRequestIdentifierFeature`Ajoute une propriété qui peut être implémentée pour identifier les demandes de façon unique.</span><span class="sxs-lookup"><span data-stu-id="71e0a-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="71e0a-120">`ISessionFeature`Définit `ISessionFactory` et `ISession` abstractions pour prendre en charge les sessions utilisateur.</span><span class="sxs-lookup"><span data-stu-id="71e0a-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="71e0a-121">`ITlsConnectionFeature`Définit une API pour la récupération des certificats clients.</span><span class="sxs-lookup"><span data-stu-id="71e0a-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="71e0a-122">`ITlsTokenBindingFeature`Définit des méthodes pour travailler avec des paramètres de liaison de jeton TLS.</span><span class="sxs-lookup"><span data-stu-id="71e0a-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="71e0a-123">`ISessionFeature`n’est pas une fonctionnalité de serveur, mais est implémenté par le `SessionMiddleware` (consultez [gestion d’état Application](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="71e0a-123">`ISessionFeature` isn't a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="71e0a-124">Collections de fonctionnalité</span><span class="sxs-lookup"><span data-stu-id="71e0a-124">Feature collections</span></span>

<span data-ttu-id="71e0a-125">Le `Features` propriété du `HttpContext` fournit une interface pour l’obtention et définition des fonctionnalités HTTP disponibles pour la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="71e0a-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="71e0a-126">Étant donné que la collection de la fonctionnalité est mutable même dans le contexte d’une demande, intergiciel (middleware) peut servir à modifier la collection et d’ajouter la prise en charge des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="71e0a-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="71e0a-127">Fonctionnalités d’intergiciel (middleware) et de la demande</span><span class="sxs-lookup"><span data-stu-id="71e0a-127">Middleware and request features</span></span>

<span data-ttu-id="71e0a-128">Alors que les serveurs sont responsables de la création de la collection de fonctionnalité, intergiciel (middleware) peut ajouter à cette collection et utilisent les fonctionnalités de la collection.</span><span class="sxs-lookup"><span data-stu-id="71e0a-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="71e0a-129">Par exemple, le `StaticFileMiddleware` accède à la `IHttpSendFileFeature` fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="71e0a-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="71e0a-130">Si la fonction existe, il est utilisé pour envoyer le fichier statique demandé à partir de son chemin d’accès physique.</span><span class="sxs-lookup"><span data-stu-id="71e0a-130">If the feature exists, it's used to send the requested static file from its physical path.</span></span> <span data-ttu-id="71e0a-131">Sinon, une autre méthode plus lente est utilisée pour envoyer le fichier.</span><span class="sxs-lookup"><span data-stu-id="71e0a-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="71e0a-132">Lorsqu’il est disponible, le `IHttpSendFileFeature` permet au système d’exploitation ouvrir le fichier et effectuer une copie du mode noyau direct à la carte réseau.</span><span class="sxs-lookup"><span data-stu-id="71e0a-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="71e0a-133">En outre, intergiciel (middleware) peut ajouter à la collection de fonctionnalité établie par le serveur.</span><span class="sxs-lookup"><span data-stu-id="71e0a-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="71e0a-134">Fonctionnalités existantes peuvent même être remplacées par un intergiciel (middleware), ce qui permet de l’intergiciel (middleware) compléter les fonctionnalités du serveur.</span><span class="sxs-lookup"><span data-stu-id="71e0a-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="71e0a-135">Fonctionnalités ajoutées à la collection sont disponibles immédiatement pour les autres intergiciel (middleware) ou l’application sous-jacente plus loin dans le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="71e0a-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="71e0a-136">En combinant les implémentations de serveur personnalisé et des améliorations de l’intergiciel (middleware) spécifique, le jeu précis d’une application requiert des fonctionnalités peut être construit.</span><span class="sxs-lookup"><span data-stu-id="71e0a-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="71e0a-137">Ainsi, manquantes fonctionnalités pour être ajoutés sans nécessiter une modification de serveur et permet de s’assurer que la quantité minimale de fonctionnalités sont exposées, ce qui limite les attaques de surface zone et améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="71e0a-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="71e0a-138">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="71e0a-138">Summary</span></span>

<span data-ttu-id="71e0a-139">Fonctionnalité interfaces définissent des fonctionnalités HTTP spécifiques une demande donnée peut prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="71e0a-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="71e0a-140">Serveurs définissent des ensembles de fonctionnalités et l’ensemble initial des fonctionnalités prises en charge par ce serveur, mais l’intergiciel (middleware) peut être utilisé pour améliorer ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="71e0a-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71e0a-141">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="71e0a-141">Additional Resources</span></span>

* [<span data-ttu-id="71e0a-142">Serveurs</span><span class="sxs-lookup"><span data-stu-id="71e0a-142">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="71e0a-143">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="71e0a-143">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="71e0a-144">OWIN (Open Web Interface pour .NET)</span><span class="sxs-lookup"><span data-stu-id="71e0a-144">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
