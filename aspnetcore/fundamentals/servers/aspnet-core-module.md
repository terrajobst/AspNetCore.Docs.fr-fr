---
title: Module ASP.NET Core
author: rick-anderson
description: Découvrez comment le module ASP.NET Core permet au serveur web Kestrel d’utiliser IIS ou IIS Express en tant que serveur proxy inverse.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910887"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="e8750-103">Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8750-103">ASP.NET Core Module</span></span>

<span data-ttu-id="e8750-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) et [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="e8750-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e8750-105">Le module ASP.NET Core permet d’exécuter les applications ASP.NET Core dans un processus de travail IIS (*in-process*), ou derrière IIS dans une configuration de proxy inverse (*out-of-process*).</span><span class="sxs-lookup"><span data-stu-id="e8750-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="e8750-106">IIS fournit des fonctionnalités avancées de sécurité et de gestion des applications web.</span><span class="sxs-lookup"><span data-stu-id="e8750-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e8750-107">Le module ASP.NET Core permet d’exécuter les applications ASP.NET Core derrière IIS dans une configuration de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="e8750-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="e8750-108">IIS fournit des fonctionnalités avancées de sécurité et de gestion des applications web.</span><span class="sxs-lookup"><span data-stu-id="e8750-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="e8750-109">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="e8750-109">Supported Windows versions:</span></span>

* <span data-ttu-id="e8750-110">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="e8750-110">Windows 7 or later</span></span>
* <span data-ttu-id="e8750-111">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="e8750-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e8750-112">Lors de l’hébergement in-process, le module dispose de sa propre implémentation de serveur, `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="e8750-112">When hosting in-process, the module has its own server implementation, `IISHttpServer`.</span></span>

<span data-ttu-id="e8750-113">Lors de l’hébergement out-of-process, le module fonctionne uniquement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e8750-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="e8750-114">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="e8750-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e8750-115">Le module fonctionne seulement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e8750-115">The module only works with Kestrel.</span></span> <span data-ttu-id="e8750-116">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="e8750-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="e8750-117">Description du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8750-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e8750-118">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour effectuer l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8750-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="e8750-119">Héberger une application ASP.NET Core à l’intérieur du processus de travail IIS (`w3wp.exe`), appelé [modèle d’hébergement in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="e8750-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="e8750-120">Transférer les requêtes web à une application ASP.NET Core du back-end exécutant le [serveur Kestrel](xref:fundamentals/servers/kestrel), appelé [modèle d’hébergement out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="e8750-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="e8750-121">Modèle d’hébergement in-process</span><span class="sxs-lookup"><span data-stu-id="e8750-121">In-process hosting model</span></span>

<span data-ttu-id="e8750-122">En utilisant l’hébergement in-process, une application ASP.NET Core s’exécute dans le même processus que son processus de travail IIS.</span><span class="sxs-lookup"><span data-stu-id="e8750-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="e8750-123">Une baisse des performances est ainsi évitée dans le traitement par proxy des requêtes sur la carte de bouclage lorsque vous utilisez le modèle d’hébergement out-of-process.</span><span class="sxs-lookup"><span data-stu-id="e8750-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="e8750-124">IIS s’occupe de la gestion des processus par l’intermédiaire du [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="e8750-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="e8750-125">Le module ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="e8750-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="e8750-126">Effectue l’initialisation de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8750-126">Performs app initialization.</span></span>
  * <span data-ttu-id="e8750-127">Charge le [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="e8750-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="e8750-128">Appelle `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="e8750-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="e8750-129">Gère la durée de vie de la requête native IIS.</span><span class="sxs-lookup"><span data-stu-id="e8750-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="e8750-130">Le schéma suivant montre la relation entre IIS, le module ASP.NET Core et une application hébergée dans un processus :</span><span class="sxs-lookup"><span data-stu-id="e8750-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Module ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="e8750-132">Une requête arrive du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="e8750-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="e8750-133">Le pilote achemine la requête native vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e8750-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="e8750-134">Le module reçoit la requête native et passe le contrôle à `IISHttpServer`, ce qui par le fait transforme la requête native en requête managée.</span><span class="sxs-lookup"><span data-stu-id="e8750-134">The module receives the native request and passes control to `IISHttpServer`, which is what converts the request from native to managed.</span></span>

<span data-ttu-id="e8750-135">Dès que `IISHttpServer` sélectionne la requête, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8750-135">After `IISHttpServer` picks up the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="e8750-136">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8750-136">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="e8750-137">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="e8750-137">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="e8750-138">Modèle d’hébergement out-of-process</span><span class="sxs-lookup"><span data-stu-id="e8750-138">Out-of-process hosting model</span></span>

<span data-ttu-id="e8750-139">Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module s’occupe de la gestion du processus.</span><span class="sxs-lookup"><span data-stu-id="e8750-139">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="e8750-140">Le module démarre le processus pour l’application ASP.NET Core quand la première requête arrive, et il redémarre l’application si elle s’arrête ou se bloque.</span><span class="sxs-lookup"><span data-stu-id="e8750-140">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="e8750-141">Il s’agit essentiellement du même comportement que celui des applications s’exécutant in-process, et qui sont gérées par le [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="e8750-141">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="e8750-142">Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application hébergée hors processus :</span><span class="sxs-lookup"><span data-stu-id="e8750-142">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Module ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="e8750-144">Les requêtes arrivent du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="e8750-144">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="e8750-145">Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e8750-145">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="e8750-146">Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est pas le port 80/443.</span><span class="sxs-lookup"><span data-stu-id="e8750-146">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="e8750-147">Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="e8750-147">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="e8750-148">Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées.</span><span class="sxs-lookup"><span data-stu-id="e8750-148">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="e8750-149">Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8750-149">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="e8750-150">Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8750-150">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="e8750-151">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8750-151">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="e8750-152">L’intergiciel (middleware) ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e8750-152">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="e8750-153">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="e8750-153">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e8750-154">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour transférer les requêtes web aux applications ASP.NET Core du back-end.</span><span class="sxs-lookup"><span data-stu-id="e8750-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="e8750-155">Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module traite également la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="e8750-155">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="e8750-156">Le module démarre le processus de l’application ASP.NET Core quand la première requête arrive et redémarre l’application si elle plante.</span><span class="sxs-lookup"><span data-stu-id="e8750-156">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="e8750-157">Il s’agit essentiellement du même comportement que celui des applications ASP.NET 4.x qui s’exécutent in-process dans IIS et sont gérées par le [service WAS (Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="e8750-157">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="e8750-158">Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application :</span><span class="sxs-lookup"><span data-stu-id="e8750-158">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Module ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="e8750-160">Les requêtes arrivent du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="e8750-160">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="e8750-161">Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e8750-161">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="e8750-162">Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est pas le port 80/443.</span><span class="sxs-lookup"><span data-stu-id="e8750-162">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="e8750-163">Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="e8750-163">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="e8750-164">Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées.</span><span class="sxs-lookup"><span data-stu-id="e8750-164">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="e8750-165">Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8750-165">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="e8750-166">Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8750-166">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="e8750-167">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8750-167">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="e8750-168">Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e8750-168">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="e8750-169">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="e8750-169">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="e8750-170">De nombreux modules natifs, comme l’authentification Windows, restent actifs.</span><span class="sxs-lookup"><span data-stu-id="e8750-170">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="e8750-171">Pour obtenir plus d’informations sur les modules IIS actifs avec le module, consultez <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="e8750-171">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="e8750-172">Le module ASP.NET Core a quelques autres fonctions.</span><span class="sxs-lookup"><span data-stu-id="e8750-172">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="e8750-173">Le module peut :</span><span class="sxs-lookup"><span data-stu-id="e8750-173">The module can:</span></span>

* <span data-ttu-id="e8750-174">Définir des variables d’environnement pour le processus de travail.</span><span class="sxs-lookup"><span data-stu-id="e8750-174">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="e8750-175">Journaliser la sortie de stdout dans un stockage de fichiers pour résoudre les problèmes de démarrage.</span><span class="sxs-lookup"><span data-stu-id="e8750-175">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="e8750-176">Transférer des jetons d’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="e8750-176">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="e8750-177">Comment installer et utiliser le module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8750-177">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="e8750-178">Pour obtenir des instructions détaillées sur l’installation et l’utilisation du module ASP.NET Core, consultez <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="e8750-178">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="e8750-179">Pour plus d’informations sur la configuration du module, consultez <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="e8750-179">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8750-180">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e8750-180">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="e8750-181">Dépôt GitHub du module ASP.NET Core (code source)</span><span class="sxs-lookup"><span data-stu-id="e8750-181">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
