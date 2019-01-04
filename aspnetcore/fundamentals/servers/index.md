---
title: Implémentations de serveurs web dans ASP.NET Core
author: guardrex
description: Découvrez les serveurs web Kestrel et HTTP.sys pour ASP.NET Core. Découvrez comment choisir un serveur et quand utiliser un serveur proxy inverse.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 2c209942ed219b6d6ca309d8aba94b264d421158
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637740"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="67da6-104">Implémentations de serveurs web dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67da6-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="67da6-105">De [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) et [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="67da6-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="67da6-106">Une application ASP.NET Core s’exécute avec une implémentation de serveur HTTP in-process.</span><span class="sxs-lookup"><span data-stu-id="67da6-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="67da6-107">L’implémentation du serveur écoute les requêtes HTTP et les expose à l’application sous forme d’ensemble de [fonctionnalités de requêtes](xref:fundamentals/request-features) composées dans un <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="67da6-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="67da6-108">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="67da6-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="67da6-109">ASP.NET Core est fourni avec les composants suivants :</span><span class="sxs-lookup"><span data-stu-id="67da6-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="67da6-110">[Serveur Kestrel](xref:fundamentals/servers/kestrel) : implémentation de serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="67da6-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="67da6-111">Le serveur HTTP IIS est un [serveur in-process](#in-process-hosting-model) pour IIS.</span><span class="sxs-lookup"><span data-stu-id="67da6-111">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="67da6-112">[Serveur HTTP.sys](xref:fundamentals/servers/httpsys) : serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="67da6-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="67da6-113">Lorsque vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l’application s’exécute :</span><span class="sxs-lookup"><span data-stu-id="67da6-113">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="67da6-114">dans le même processus que le processus Worker IIS (le [modèle d’hébergement in-process](#in-process-hosting-model)) avec le [serveur HTTP IIS](#iis-http-server).</span><span class="sxs-lookup"><span data-stu-id="67da6-114">In the same process as the IIS worker process (the [in-process hosting model](#in-process-hosting-model)) with the [IIS HTTP Server](#iis-http-server).</span></span> <span data-ttu-id="67da6-115">*In-process* est la configuration recommandée.</span><span class="sxs-lookup"><span data-stu-id="67da6-115">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="67da6-116">Ou dans un processus séparé du processus Worker IIS (le [mode d’hébergement out-of-process](#out-of-process-hosting-model)) avec le [serveur Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="67da6-116">In a process separate from the IIS worker process (the [out-of-process hosting model](#out-of-process-hosting-model)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="67da6-117">Le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) est un module IIS natif qui gère les requêtes IIS natives entre IIS et le serveur HTTP IIS in-process ou Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67da6-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="67da6-118">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="67da6-118">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="67da6-119">Modèles d'hébergement</span><span class="sxs-lookup"><span data-stu-id="67da6-119">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="67da6-120">Modèle d’hébergement in-process</span><span class="sxs-lookup"><span data-stu-id="67da6-120">In-process hosting model</span></span>

<span data-ttu-id="67da6-121">En utilisant l’hébergement in-process, une application ASP.NET Core s’exécute dans le même processus que son processus de travail IIS.</span><span class="sxs-lookup"><span data-stu-id="67da6-121">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="67da6-122">Cette opération supprime la baisse des performances out-of-process des requêtes proxy sur l’adaptateur de bouclage, une interface réseau qui renvoie du trafic réseau sortant à la même machine.</span><span class="sxs-lookup"><span data-stu-id="67da6-122">This removes the out-of-process performance penalty of proxying requests over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="67da6-123">IIS s’occupe de la gestion des processus par l’intermédiaire du [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="67da6-123">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="67da6-124">Le module ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="67da6-124">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="67da6-125">Effectue l’initialisation de l’application.</span><span class="sxs-lookup"><span data-stu-id="67da6-125">Performs app initialization.</span></span>
  * <span data-ttu-id="67da6-126">Charge le [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="67da6-126">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="67da6-127">Appelle `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="67da6-127">Calls `Program.Main`.</span></span>
* <span data-ttu-id="67da6-128">Gère la durée de vie de la requête native IIS.</span><span class="sxs-lookup"><span data-stu-id="67da6-128">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="67da6-129">Le schéma suivant montre la relation entre IIS, le module ASP.NET Core et une application hébergée dans un processus :</span><span class="sxs-lookup"><span data-stu-id="67da6-129">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Module ASP.NET Core](_static/ancm-inprocess.png)

<span data-ttu-id="67da6-131">Une requête arrive du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="67da6-131">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="67da6-132">Le pilote achemine la requête native vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="67da6-132">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="67da6-133">Le module reçoit la requête native et la passe à IIS HTTP Server (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="67da6-133">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="67da6-134">Le serveur HTTP IIS est une implémentation du serveur in-process pour IIS qui convertit la requête native en requête managée.</span><span class="sxs-lookup"><span data-stu-id="67da6-134">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="67da6-135">Une fois que IIS HTTP Server a traité la requête, celle-ci est envoyée (push) dans le pipeline de middleware (intergiciel) d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67da6-135">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="67da6-136">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="67da6-136">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="67da6-137">La réponse de l’application est repassée à IIS, qui la renvoie au client à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="67da6-137">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

<span data-ttu-id="67da6-138">L’hébergement in-process est au choix pour les applications existantes, mais les modèles [dotnet new](/dotnet/core/tools/dotnet-new) sont définis par défaut sur le modèle d’hébergement in-process pour tous les scénarios IIS et IIS Express.</span><span class="sxs-lookup"><span data-stu-id="67da6-138">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="67da6-139">Modèle d’hébergement out-of-process</span><span class="sxs-lookup"><span data-stu-id="67da6-139">Out-of-process hosting model</span></span>

<span data-ttu-id="67da6-140">Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module s’occupe de la gestion du processus.</span><span class="sxs-lookup"><span data-stu-id="67da6-140">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="67da6-141">Le module démarre le processus pour l’application ASP.NET Core quand la première requête arrive, et il redémarre l’application si elle s’arrête ou se bloque.</span><span class="sxs-lookup"><span data-stu-id="67da6-141">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="67da6-142">Il s’agit essentiellement du même comportement que celui des applications s’exécutant in-process, et qui sont gérées par le [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="67da6-142">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="67da6-143">Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application hébergée hors processus :</span><span class="sxs-lookup"><span data-stu-id="67da6-143">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Module ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="67da6-145">Les requêtes arrivent du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="67da6-145">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="67da6-146">Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="67da6-146">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="67da6-147">Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est ni le port 80 ni le port 443.</span><span class="sxs-lookup"><span data-stu-id="67da6-147">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="67da6-148">Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{PORT}`.</span><span class="sxs-lookup"><span data-stu-id="67da6-148">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="67da6-149">Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées.</span><span class="sxs-lookup"><span data-stu-id="67da6-149">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="67da6-150">Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="67da6-150">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="67da6-151">Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67da6-151">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="67da6-152">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="67da6-152">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="67da6-153">Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67da6-153">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="67da6-154">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="67da6-154">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="67da6-155">Pour des conseils de configuration d’IIS et du module ASP.NET Core, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="67da6-155">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="67da6-156">macOS</span><span class="sxs-lookup"><span data-stu-id="67da6-156">macOS</span></span>](#tab/macos)

<span data-ttu-id="67da6-157">ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="67da6-157">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="67da6-158">Linux</span><span class="sxs-lookup"><span data-stu-id="67da6-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="67da6-159">ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="67da6-159">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="67da6-160">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="67da6-160">Windows</span></span>](#tab/windows)

<span data-ttu-id="67da6-161">ASP.NET Core est fourni avec les composants suivants :</span><span class="sxs-lookup"><span data-stu-id="67da6-161">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="67da6-162">[Serveur Kestrel](xref:fundamentals/servers/kestrel) : serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="67da6-162">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="67da6-163">[Serveur HTTP.sys](xref:fundamentals/servers/httpsys) : serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="67da6-163">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="67da6-164">Lorsqu’[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) sont utilisés, l’application s’exécute dans un processus séparé du processus Worker IIS (le mode d’hébergement *out-of-process*) avec le [serveur Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="67da6-164">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="67da6-165">Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module s’occupe de la gestion du processus.</span><span class="sxs-lookup"><span data-stu-id="67da6-165">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="67da6-166">Le module démarre le processus pour l’application ASP.NET Core quand la première requête arrive, et il redémarre l’application si elle s’arrête ou se bloque.</span><span class="sxs-lookup"><span data-stu-id="67da6-166">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="67da6-167">Il s’agit essentiellement du même comportement que celui des applications s’exécutant in-process, et qui sont gérées par le [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="67da6-167">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="67da6-168">Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application hébergée hors processus :</span><span class="sxs-lookup"><span data-stu-id="67da6-168">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Module ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="67da6-170">Les requêtes arrivent du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="67da6-170">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="67da6-171">Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="67da6-171">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="67da6-172">Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est ni le port 80 ni le port 443.</span><span class="sxs-lookup"><span data-stu-id="67da6-172">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="67da6-173">Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="67da6-173">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="67da6-174">Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées.</span><span class="sxs-lookup"><span data-stu-id="67da6-174">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="67da6-175">Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="67da6-175">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="67da6-176">Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67da6-176">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="67da6-177">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="67da6-177">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="67da6-178">Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67da6-178">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="67da6-179">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="67da6-179">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="67da6-180">Pour des conseils de configuration d’IIS et du module ASP.NET Core, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="67da6-180">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="67da6-181">macOS</span><span class="sxs-lookup"><span data-stu-id="67da6-181">macOS</span></span>](#tab/macos)

<span data-ttu-id="67da6-182">ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="67da6-182">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="67da6-183">Linux</span><span class="sxs-lookup"><span data-stu-id="67da6-183">Linux</span></span>](#tab/linux)

<span data-ttu-id="67da6-184">ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="67da6-184">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="67da6-185">Kestrel</span><span class="sxs-lookup"><span data-stu-id="67da6-185">Kestrel</span></span>

<span data-ttu-id="67da6-186">Kestrel est le serveur web par défaut inclus dans les modèles de projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67da6-186">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="67da6-187">Kestrel peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="67da6-187">Kestrel can be used:</span></span>

* <span data-ttu-id="67da6-188">Par lui même, c’est-à-dire en tant que serveur de périphérie traitant les requêtes en provenance directe d’un réseau, notamment d’Internet.</span><span class="sxs-lookup"><span data-stu-id="67da6-188">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="67da6-190">Avec un *serveur proxy inverse*, comme [IIS (Internet Information Services)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="67da6-190">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="67da6-191">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67da6-191">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="67da6-193">Les deux configurations d’hébergement, &mdash;avec ou sans serveur proxy inverse&mdash;, sont prises en charge pour les applications ASP.NET Core 2.1 ou des versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="67da6-193">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="67da6-194">Si l’application accepte seulement les requêtes provenant d’un réseau interne, Kestrel peut être utilisé par lui-même.</span><span class="sxs-lookup"><span data-stu-id="67da6-194">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel communique directement avec le réseau interne](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="67da6-196">Si l’application est exposée à Internet, Kestrel doit utiliser un *serveur proxy inverse* comme[IIS (Internet Information Services)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="67da6-196">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="67da6-197">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67da6-197">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="67da6-199">La raison la plus importante de l’utilisation d’un proxy inversé pour les déploiements de serveurs de périphérie publics directement exposés à Internet est la sécurité.</span><span class="sxs-lookup"><span data-stu-id="67da6-199">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="67da6-200">Les versions 1.x de Kestrel n’incluent pas de fonctionnalités de sécurité suffisantes pour vous défendre contre les attaques provenant d’Internet.</span><span class="sxs-lookup"><span data-stu-id="67da6-200">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="67da6-201">Ceci inclut, entre autres choses, des délais d’attente appropriés, des limites de taille des requêtes et des limites du nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="67da6-201">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="67da6-202">Pour des conseils de configuration de Kestrel et des informations sur le moment d’utiliser Kestrel dans une configuration de proxy inverse, consultez <xref:fundamentals/servers/kestrel>.</span><span class="sxs-lookup"><span data-stu-id="67da6-202">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

### <a name="nginx-with-kestrel"></a><span data-ttu-id="67da6-203">Nginx avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="67da6-203">Nginx with Kestrel</span></span>

<span data-ttu-id="67da6-204">Pour plus d’informations sur l’utilisation de Nginx sur Linux comme serveur proxy inverse pour Kestrel, consultez <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="67da6-204">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="67da6-205">Apache avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="67da6-205">Apache with Kestrel</span></span>

<span data-ttu-id="67da6-206">Pour plus d’informations sur l’utilisation d’Apache sur Linux comme serveur proxy inverse pour Kestrel, consultez <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="67da6-206">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a><span data-ttu-id="67da6-207">Serveur HTTP IIS</span><span class="sxs-lookup"><span data-stu-id="67da6-207">IIS HTTP Server</span></span>

<span data-ttu-id="67da6-208">Le serveur HTTP IIS est un [serveur in-process](#in-process-hosting-model) pour IIS requis pour les déploiements in-process.</span><span class="sxs-lookup"><span data-stu-id="67da6-208">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS and required for in-process deployments.</span></span> <span data-ttu-id="67da6-209">Le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) gère des requêtes IIS natives entre IIS et le serveur HTTP IIS.</span><span class="sxs-lookup"><span data-stu-id="67da6-209">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) handles native IIS requests between IIS and IIS HTTP Server.</span></span> <span data-ttu-id="67da6-210">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="67da6-210">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="67da6-211">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="67da6-211">HTTP.sys</span></span>

<span data-ttu-id="67da6-212">Si vos applications ASP.NET Core sont exécutées sur Windows, HTTP.sys est une alternative à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67da6-212">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="67da6-213">Kestrel est généralement recommandé pour de meilleures performances.</span><span class="sxs-lookup"><span data-stu-id="67da6-213">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="67da6-214">HTTP.sys peut être utilisé dans les scénarios où l’application est exposée à Internet et où des fonctionnalités requises sont prises en charge par HTTP.sys, mais pas par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67da6-214">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="67da6-215">Pour plus d'informations, consultez <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="67da6-215">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="67da6-217">HTTP.sys peut également être utilisé pour les applications qui sont exposées seulement à un réseau interne.</span><span class="sxs-lookup"><span data-stu-id="67da6-217">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys communique directement avec le réseau interne](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="67da6-219">Pour obtenir des conseils de configuration de HTTP.sys, consultez <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="67da6-219">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="67da6-220">Infrastructure serveur d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67da6-220">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="67da6-221">Le [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible dans la méthode `Startup.Configure` expose la propriété [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="67da6-221">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="67da6-222">Kestrel et HTTP.sys n’exposent qu’une seule fonctionnalité chacun, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mais des implémentations serveur différentes peuvent exposer des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="67da6-222">Kestrel and HTTP.sys only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="67da6-223">`IServerAddressesFeature` permet de déterminer quel est le port lié à l’implémentation serveur au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="67da6-223">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="67da6-224">Serveurs personnalisés</span><span class="sxs-lookup"><span data-stu-id="67da6-224">Custom servers</span></span>

<span data-ttu-id="67da6-225">Si les serveurs intégrés ne répondent pas aux spécifications de l’application, vous pouvez créer une implémentation serveur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="67da6-225">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="67da6-226">Le [guide OWIN (Open Web Interface pour .NET)](xref:fundamentals/owin) montre comment écrire une implémentation [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basée sur [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="67da6-226">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="67da6-227">Seules les interfaces des fonctionnalités utilisées par l’application nécessitent une implémentation, bien qu’au minimum, [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) et [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) doivent être pris en charge.</span><span class="sxs-lookup"><span data-stu-id="67da6-227">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="67da6-228">Démarrage du serveur</span><span class="sxs-lookup"><span data-stu-id="67da6-228">Server startup</span></span>

<span data-ttu-id="67da6-229">Le serveur est lancé lorsque l’environnement de développement intégré (IDE) ou l’éditeur démarre l’application :</span><span class="sxs-lookup"><span data-stu-id="67da6-229">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="67da6-230">Des &ndash;profils de lancement [Visual Studio](https://www.visualstudio.com/vs/) peuvent être utilisés pour démarrer l’application et le serveur avec [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) ou avec la console.</span><span class="sxs-lookup"><span data-stu-id="67da6-230">[Visual Studio](https://www.visualstudio.com/vs/) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="67da6-231">[Visual Studio Code](https://code.visualstudio.com/) &ndash;L’application et le serveur sont démarrés par [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), qui active le débogueur CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="67da6-231">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="67da6-232">[Visual Studio pour Mac](https://www.visualstudio.com/vs/mac/) &ndash; L’application et le serveur sont démarrés par le [débogueur Mono Soft-Mode](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="67da6-232">[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="67da6-233">Lors du lancement de l’application à partir d’une invite de commandes dans le dossier du projet, [dotnet run](/dotnet/core/tools/dotnet-run) lance l’application et le serveur (Kestrel et HTTP.sys uniquement).</span><span class="sxs-lookup"><span data-stu-id="67da6-233">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="67da6-234">La configuration est spécifiée par l’option `-c|--configuration`, qui est définie sur `Debug` (par défaut) ou sur `Release`.</span><span class="sxs-lookup"><span data-stu-id="67da6-234">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="67da6-235">Si les profils de lancement sont présents dans un fichier *launchSettings.json*, utilisez l’option `--launch-profile <NAME>` pour définir le profil de lancement (par exemple `Development` ou `Production`).</span><span class="sxs-lookup"><span data-stu-id="67da6-235">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="67da6-236">Pour plus d’informations, consultez les rubriques [dotnet run](/dotnet/core/tools/dotnet-run) et [Package de distribution de .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="67da6-236">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="67da6-237">Prise en charge de HTTP/2</span><span class="sxs-lookup"><span data-stu-id="67da6-237">HTTP/2 support</span></span>

<span data-ttu-id="67da6-238">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est pris en charge avec ASP.NET Core dans les scénarios de déploiement suivants :</span><span class="sxs-lookup"><span data-stu-id="67da6-238">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="67da6-239">Kestrel</span><span class="sxs-lookup"><span data-stu-id="67da6-239">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="67da6-240">Système d'exploitation</span><span class="sxs-lookup"><span data-stu-id="67da6-240">Operating system</span></span>
    * <span data-ttu-id="67da6-241">Windows Server 2016/Windows 10 ou version ultérieure&dagger;</span><span class="sxs-lookup"><span data-stu-id="67da6-241">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="67da6-242">Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple, Ubuntu 16.04 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="67da6-242">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="67da6-243">HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="67da6-243">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="67da6-244">Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="67da6-244">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="67da6-245">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="67da6-245">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="67da6-246">Windows Server 2016/Windows 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="67da6-246">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="67da6-247">Framework cible : non applicable aux déploiements HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="67da6-247">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="67da6-248">IIS (in-process)</span><span class="sxs-lookup"><span data-stu-id="67da6-248">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="67da6-249">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="67da6-249">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="67da6-250">Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="67da6-250">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="67da6-251">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="67da6-251">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="67da6-252">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="67da6-252">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="67da6-253">Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="67da6-253">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="67da6-254">Framework cible : non applicable aux déploiements IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="67da6-254">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="67da6-255">&dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="67da6-255">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="67da6-256">La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée.</span><span class="sxs-lookup"><span data-stu-id="67da6-256">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="67da6-257">Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.</span><span class="sxs-lookup"><span data-stu-id="67da6-257">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="67da6-258">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="67da6-258">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="67da6-259">Windows Server 2016/Windows 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="67da6-259">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="67da6-260">Framework cible : non applicable aux déploiements HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="67da6-260">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="67da6-261">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="67da6-261">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="67da6-262">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="67da6-262">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="67da6-263">Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="67da6-263">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="67da6-264">Framework cible : non applicable aux déploiements IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="67da6-264">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="67da6-265">Une connexion HTTP/2 doit utiliser la [négociation du protocole de la couche d’application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) et TLS 1.2 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="67da6-265">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="67da6-266">Pour plus d’informations, consultez les rubriques qui se rapportent à vos scénarios de déploiement de serveur.</span><span class="sxs-lookup"><span data-stu-id="67da6-266">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="67da6-267">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="67da6-267">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
