---
title: Composants de Razor modèles d’hébergement
author: guardrex
description: Comprendre les Blazor côté client et côté serveur ASP.NET Core Razor composants, modèles d’hébergement.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515528"
---
# <a name="razor-components-hosting-models"></a><span data-ttu-id="08a49-103">Composants de Razor modèles d’hébergement</span><span class="sxs-lookup"><span data-stu-id="08a49-103">Razor Components hosting models</span></span>

<span data-ttu-id="08a49-104">Par [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="08a49-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="08a49-105">Composants de Razor est une infrastructure web conçue pour s’exécuter côté client dans le navigateur sur un [WebAssembly](http://webassembly.org/)-en fonction du runtime .NET (*Blazor*) ou côté serveur dans ASP.NET Core (*ASP.NET Core Razor Composants*).</span><span class="sxs-lookup"><span data-stu-id="08a49-105">Razor Components is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="08a49-106">Quel que soit les modèles d’hébergement modèle, l’application et le composant *restent les mêmes*.</span><span class="sxs-lookup"><span data-stu-id="08a49-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="08a49-107">Cet article aborde les modèles d’hébergement disponibles :</span><span class="sxs-lookup"><span data-stu-id="08a49-107">This article discusses the available hosting models:</span></span>

* [<span data-ttu-id="08a49-108">Blazor côté client</span><span class="sxs-lookup"><span data-stu-id="08a49-108">Client-side Blazor</span></span>](#client-side-hosting-model)
* [<span data-ttu-id="08a49-109">Composants Razor ASP.NET Core côté serveur</span><span class="sxs-lookup"><span data-stu-id="08a49-109">Server-side ASP.NET Core Razor Components</span></span>](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a><span data-ttu-id="08a49-110">Modèle d’hébergement côté client</span><span class="sxs-lookup"><span data-stu-id="08a49-110">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="08a49-111">Le modèle d’hébergement principal pour Blazor est en cours d’exécution côté client dans le navigateur sur WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="08a49-111">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="08a49-112">L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="08a49-112">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="08a49-113">L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="08a49-113">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="08a49-114">Mises à jour de l’interface utilisateur et de gestion des événements se produisent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="08a49-114">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="08a49-115">Ressources de l’application sont déployés en tant que fichiers statiques à un serveur web ou un service capable de fournir du contenu statique à des clients.</span><span class="sxs-lookup"><span data-stu-id="08a49-115">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor côté client : L’application Blazor s’exécute sur un thread d’interface utilisateur dans le navigateur.](hosting-models/_static/client-side.png)

<span data-ttu-id="08a49-117">Pour créer une application de Blazor en utilisant le modèle d’hébergement côté client, utilisez un des modèles suivants :</span><span class="sxs-lookup"><span data-stu-id="08a49-117">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="08a49-118">**Blazor** ([blazor nouveau dotnet](/dotnet/core/tools/dotnet-new)) &ndash; déployé comme un ensemble de fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="08a49-118">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="08a49-119">**Blazor (ASP.NET Core hébergées)** ([blazorhosted nouveau dotnet](/dotnet/core/tools/dotnet-new)) &ndash; hébergées par un serveur d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08a49-119">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="08a49-120">L’application ASP.NET Core sert l’application Blazor aux clients.</span><span class="sxs-lookup"><span data-stu-id="08a49-120">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="08a49-121">L’application de Blazor côté client peut interagir avec le serveur sur le réseau à l’aide d’appels d’API web ou [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="08a49-121">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="08a49-122">Les modèles incluent les *components.webassembly.js* script gère :</span><span class="sxs-lookup"><span data-stu-id="08a49-122">The templates include the *components.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="08a49-123">Télécharger le runtime .NET, l’application et les dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="08a49-123">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="08a49-124">Initialisation du runtime pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="08a49-124">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="08a49-125">Le modèle d’hébergement côté client offre les avantages suivants.</span><span class="sxs-lookup"><span data-stu-id="08a49-125">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="08a49-126">Côté client Blazor :</span><span class="sxs-lookup"><span data-stu-id="08a49-126">Client-side Blazor:</span></span>

* <span data-ttu-id="08a49-127">N’a aucune dépendance de côté serveur .NET.</span><span class="sxs-lookup"><span data-stu-id="08a49-127">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="08a49-128">Entièrement tire parti des fonctionnalités et les ressources client.</span><span class="sxs-lookup"><span data-stu-id="08a49-128">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="08a49-129">Déchargements fonctionnent à partir du serveur au client.</span><span class="sxs-lookup"><span data-stu-id="08a49-129">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="08a49-130">Prend en charge les scénarios hors connexion.</span><span class="sxs-lookup"><span data-stu-id="08a49-130">Supports offline scenarios.</span></span>

<span data-ttu-id="08a49-131">Il existe des inconvénients à l’hébergement du côté client.</span><span class="sxs-lookup"><span data-stu-id="08a49-131">There are downsides to client-side hosting.</span></span> <span data-ttu-id="08a49-132">Côté client Blazor :</span><span class="sxs-lookup"><span data-stu-id="08a49-132">Client-side Blazor:</span></span>

* <span data-ttu-id="08a49-133">Restreint l’application aux fonctionnalités du navigateur.</span><span class="sxs-lookup"><span data-stu-id="08a49-133">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="08a49-134">Nécessite le matériel capable de client et logiciels (par exemple, WebAssembly de prise en charge).</span><span class="sxs-lookup"><span data-stu-id="08a49-134">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="08a49-135">A une plus grande taille de téléchargement et l’application plue le temps de chargement.</span><span class="sxs-lookup"><span data-stu-id="08a49-135">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="08a49-136">A moins de maturité runtime .NET et les outils de prise en charge (par exemple, les limitations dans [.NET Standard](/dotnet/standard/net-standard) prise en charge et le débogage).</span><span class="sxs-lookup"><span data-stu-id="08a49-136">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="08a49-137">Modèle d’hébergement côté serveur</span><span class="sxs-lookup"><span data-stu-id="08a49-137">Server-side hosting model</span></span>

<span data-ttu-id="08a49-138">Avec le modèle d’hébergement ASP.NET Core Razor composants côté serveur, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08a49-138">With the ASP.NET Core Razor Components server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="08a49-139">Mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont traités sur un [SignalR](xref:signalr/introduction) connexion.</span><span class="sxs-lookup"><span data-stu-id="08a49-139">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![ASP.NET Core Razor composants côté serveur : Le navigateur interagit avec l’application (hébergée à l’intérieur d’une application ASP.NET Core) sur le serveur via une connexion SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="08a49-141">Pour créer une application de composants de Razor en utilisant le modèle d’hébergement côté serveur, utilisez ASP.NET Core **Razor composants** modèle ([razorcomponents nouveau dotnet](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="08a49-141">To create a Razor Components app using the server-side hosting model, use the ASP.NET Core **Razor Components** template ([dotnet new razorcomponents](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="08a49-142">L’application ASP.NET Core héberge l’application côté serveur de composants de Razor et configure le point de terminaison SignalR où les clients se connectent.</span><span class="sxs-lookup"><span data-stu-id="08a49-142">The ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="08a49-143">L’application fait référence à l’application ASP.NET Core `Startup` classe à ajouter :</span><span class="sxs-lookup"><span data-stu-id="08a49-143">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="08a49-144">Services de composants de Razor côté serveur.</span><span class="sxs-lookup"><span data-stu-id="08a49-144">Server-side Razor Components services.</span></span>
* <span data-ttu-id="08a49-145">L’application pour le pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="08a49-145">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="08a49-146">Le *components.server.js* script&dagger; établit la connexion cliente.</span><span class="sxs-lookup"><span data-stu-id="08a49-146">The *components.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="08a49-147">Il est responsable de l’application pour rendre persistantes et restaurer l’état de l’application en fonction des besoins (par exemple, en cas d’une connexion réseau perdues).</span><span class="sxs-lookup"><span data-stu-id="08a49-147">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="08a49-148">Le modèle d’hébergement côté serveur offre plusieurs avantages.</span><span class="sxs-lookup"><span data-stu-id="08a49-148">The server-side hosting model offers several benefits.</span></span> <span data-ttu-id="08a49-149">Composants côté serveur Razor :</span><span class="sxs-lookup"><span data-stu-id="08a49-149">Server-side Razor Components:</span></span>

* <span data-ttu-id="08a49-150">Avoir une taille d’application beaucoup plus petite qu’une application de Blazor côté client et chargées beaucoup plus rapidement.</span><span class="sxs-lookup"><span data-stu-id="08a49-150">Have a significantly smaller app size than a client-side Blazor app and load much faster.</span></span>
* <span data-ttu-id="08a49-151">Peut tirer pleinement parti des fonctionnalités de serveur, notamment l’utilisation des API compatibles de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="08a49-151">Can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="08a49-152">Exécuter sur .NET Core sur le serveur, afin que .NET existantes des outils, telles que le débogage, fonctionne comme prévu.</span><span class="sxs-lookup"><span data-stu-id="08a49-152">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="08a49-153">Fonctionne avec les clients légers (par exemple, les navigateurs qui ne prennent pas en charge le WebAssembly et ressources limité des appareils).</span><span class="sxs-lookup"><span data-stu-id="08a49-153">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="08a49-154">.NET /C# base de code, y compris le code du composant de l’application, n’est pas prise en charge pour les clients.</span><span class="sxs-lookup"><span data-stu-id="08a49-154">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="08a49-155">Il existe des inconvénients à l’hébergement du côté serveur.</span><span class="sxs-lookup"><span data-stu-id="08a49-155">There are downsides to server-side hosting.</span></span> <span data-ttu-id="08a49-156">Composants côté serveur Razor :</span><span class="sxs-lookup"><span data-stu-id="08a49-156">Server-side Razor Components:</span></span>

* <span data-ttu-id="08a49-157">Avoir une latence plus élevée : Chaque interaction utilisateur implique un tronçon réseau.</span><span class="sxs-lookup"><span data-stu-id="08a49-157">Have higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="08a49-158">N’offre aucune prise en charge hors connexion : Si la connexion du client échoue, l’application cesse de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="08a49-158">Offer no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="08a49-159">Ont réduit l’évolutivité : Le serveur doit gérer plusieurs connexions de client et gérer l’état du client.</span><span class="sxs-lookup"><span data-stu-id="08a49-159">Have reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="08a49-160">Exiger un serveur d’ASP.NET Core pour servir de l’application.</span><span class="sxs-lookup"><span data-stu-id="08a49-160">Require an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="08a49-161">Déploiement sans serveur (par exemple, à partir d’un CDN) n’est pas possible.</span><span class="sxs-lookup"><span data-stu-id="08a49-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="08a49-162">&dagger;Le *components.server.js* script est publié dans le chemin d’accès suivant : *bin / {déboguer | Mise en production} / {FRAMEWORK cible} /publish/ {nom de l’APPLICATION}. Dist/application/_framework*.</span><span class="sxs-lookup"><span data-stu-id="08a49-162">&dagger;The *components.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
