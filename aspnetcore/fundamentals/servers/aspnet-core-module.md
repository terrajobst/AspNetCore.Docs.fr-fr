---
title: Module ASP.NET Core
author: rick-anderson
description: Découvrez comment le module ASP.NET Core permet au serveur web Kestrel d’utiliser IIS ou IIS Express en tant que serveur proxy inverse.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-module"></a><span data-ttu-id="ecc2b-103">Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecc2b-103">ASP.NET Core Module</span></span>

<span data-ttu-id="ecc2b-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) et [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ecc2b-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="ecc2b-105">Le module ASP.NET Core permet d’exécuter les applications ASP.NET Core derrière IIS dans une configuration de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="ecc2b-106">IIS fournit des fonctionnalités avancées de sécurité et de gestion des applications web.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="ecc2b-107">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="ecc2b-107">Supported Windows versions:</span></span>

* <span data-ttu-id="ecc2b-108">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ecc2b-108">Windows 7 or later</span></span>
* <span data-ttu-id="ecc2b-109">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ecc2b-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="ecc2b-110">Le module ASP.NET Core fonctionne seulement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="ecc2b-111">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="ecc2b-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="ecc2b-112">Description du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecc2b-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="ecc2b-113">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour rediriger les requêtes web aux applications ASP.NET Core du backend.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="ecc2b-114">De nombreux modules natifs, comme l’authentification Windows, restent actifs.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="ecc2b-115">Pour plus d’informations sur les modules IIS actifs avec le module, consultez [Modules IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="ecc2b-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="ecc2b-116">Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module traite également la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="ecc2b-117">Le module démarre le processus de l’application ASP.NET Core quand la première requête arrive et redémarre l’application si elle plante.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="ecc2b-118">Il s’agit essentiellement du même comportement que celui des applications ASP.NET 4.x qui s’exécutent in-process dans IIS et sont gérées par le [service WAS (Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="ecc2b-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ecc2b-119">Le diagramme suivant montre la relation entre IIS, le module ASP.NET Core et les applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="ecc2b-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![Module ASP.NET Core](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="ecc2b-121">Les requêtes arrivent du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="ecc2b-122">Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ecc2b-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="ecc2b-123">Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est pas le port 80/443.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="ecc2b-124">Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="ecc2b-125">Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="ecc2b-126">Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="ecc2b-127">Une fois que Kestrel prend une requête dans le module, cette requête est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ecc2b-128">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ecc2b-129">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="ecc2b-130">Le module ASP.NET Core a quelques autres fonctions.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="ecc2b-131">Le module peut :</span><span class="sxs-lookup"><span data-stu-id="ecc2b-131">The module can:</span></span>

* <span data-ttu-id="ecc2b-132">Définir des variables d’environnement pour le processus de travail.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="ecc2b-133">Journaliser la sortie de stdout dans un stockage de fichiers pour résoudre les problèmes de démarrage.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="ecc2b-134">Transférer des jetons d’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="ecc2b-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="ecc2b-135">Comment installer et utiliser le module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecc2b-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="ecc2b-136">Pour obtenir des instructions détaillées sur l’installation et l’utilisation du module ASP.NET Core, consultez [Héberger sur Windows avec IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="ecc2b-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="ecc2b-137">Pour plus d’informations sur la configuration du module, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="ecc2b-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ecc2b-138">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ecc2b-138">Additional resources</span></span>

* [<span data-ttu-id="ecc2b-139">Héberger sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="ecc2b-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="ecc2b-140">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecc2b-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="ecc2b-141">Dépôt GitHub du module ASP.NET Core (code source)</span><span class="sxs-lookup"><span data-stu-id="ecc2b-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
