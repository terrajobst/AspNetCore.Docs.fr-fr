---
title: Implémentation du serveur web HTTP.sys dans ASP.NET Core
author: guardrex
description: Découvrez HTTP.sys, un serveur web pour ASP.NET Core sous Windows. Basé sur le pilote en mode noyau HTTP.sys, HTTP.sys est une solution qui permet d’établir une connexion directe à Internet sans IIS ni Kestrel.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/27/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d91a0f7b83fb073147978319edfbed0bda9a1c44
ms.sourcegitcommit: 06c4f2910dd54ded25e1b8750e09c66578748bc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66395954"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="c29bf-104">Implémentation du serveur web HTTP.sys dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c29bf-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="c29bf-105">[Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c29bf-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c29bf-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) est un [serveur web pour ASP.NET Core](xref:fundamentals/servers/index) qui s’exécute uniquement sous Windows.</span><span class="sxs-lookup"><span data-stu-id="c29bf-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="c29bf-107">HTTP.sys est une alternative au serveur [Kestrel](xref:fundamentals/servers/kestrel) et offre des fonctionnalités supplémentaires par rapport à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="c29bf-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c29bf-108">HTTP.sys est incompatible avec le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et n’est pas utilisable avec IIS ni IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c29bf-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="c29bf-109">HTTP.sys prend en charge les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="c29bf-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="c29bf-110">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="c29bf-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="c29bf-111">Partage de port</span><span class="sxs-lookup"><span data-stu-id="c29bf-111">Port sharing</span></span>
* <span data-ttu-id="c29bf-112">HTTPS avec SNI</span><span class="sxs-lookup"><span data-stu-id="c29bf-112">HTTPS with SNI</span></span>
* <span data-ttu-id="c29bf-113">HTTP/2 sur TLS (Windows 10 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="c29bf-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="c29bf-114">Transmission de fichier directe</span><span class="sxs-lookup"><span data-stu-id="c29bf-114">Direct file transmission</span></span>
* <span data-ttu-id="c29bf-115">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="c29bf-115">Response caching</span></span>
* <span data-ttu-id="c29bf-116">WebSockets (Windows 8 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="c29bf-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="c29bf-117">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="c29bf-117">Supported Windows versions:</span></span>

* <span data-ttu-id="c29bf-118">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="c29bf-118">Windows 7 or later</span></span>
* <span data-ttu-id="c29bf-119">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="c29bf-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="c29bf-120">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c29bf-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="c29bf-121">Quand utiliser HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c29bf-121">When to use HTTP.sys</span></span>

<span data-ttu-id="c29bf-122">HTTP.sys est utile pour les déploiements lorsque :</span><span class="sxs-lookup"><span data-stu-id="c29bf-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="c29bf-123">Il est nécessaire d’exposer directement le serveur à Internet sans passer par IIS.</span><span class="sxs-lookup"><span data-stu-id="c29bf-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="c29bf-125">Un déploiement interne implique une fonctionnalité qui n’est pas disponible dans Kestrel, par exemple, [l’authentification Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="c29bf-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys communique directement avec le réseau interne](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="c29bf-127">HTTP.sys est une technologie aboutie qui assure une protection contre de nombreux types d’attaques et offre la robustesse, la sécurité et l’extensibilité d’un serveur web haut de gamme.</span><span class="sxs-lookup"><span data-stu-id="c29bf-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="c29bf-128">Pour sa part, IIS s’exécute en tant qu’écouteur HTTP sur HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c29bf-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="c29bf-129">Prise en charge de HTTP/2</span><span class="sxs-lookup"><span data-stu-id="c29bf-129">HTTP/2 support</span></span>

<span data-ttu-id="c29bf-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est activé pour les applications ASP.NET Core si les conditions de base suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="c29bf-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="c29bf-131">Windows Server 2016/Windows 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="c29bf-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="c29bf-132">Connexion [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="c29bf-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="c29bf-133">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="c29bf-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c29bf-134">Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c29bf-135">Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="c29bf-136">HTTP/2 est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="c29bf-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="c29bf-137">Si une connexion HTTP/2 n’est pas établie, la connexion revient à HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="c29bf-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="c29bf-138">Dans une prochaine version de Windows, les indicateurs de configuration HTTP/2 seront disponibles, y compris la possibilité de désactivation de HTTP/2 avec HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c29bf-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="c29bf-139">Authentification en mode noyau avec Kerberos</span><span class="sxs-lookup"><span data-stu-id="c29bf-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="c29bf-140">HTTP.sys délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos.</span><span class="sxs-lookup"><span data-stu-id="c29bf-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="c29bf-141">L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c29bf-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="c29bf-142">Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="c29bf-143">Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="c29bf-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="c29bf-144">Comment utiliser HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c29bf-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="c29bf-145">Configurer l’application ASP.NET Core afin d’utiliser HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c29bf-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="c29bf-146">Il n’est pas nécessaire d’ajouter une référence de package dans le fichier projet dès lors que le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) est utilisé (ASP.NET Core 2.1 ou ultérieur).</span><span class="sxs-lookup"><span data-stu-id="c29bf-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="c29bf-147">Si vous n'utilisez pas le métapaquet `Microsoft.AspNetCore.App`, ajoutez une référence de package à [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span><span class="sxs-lookup"><span data-stu-id="c29bf-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="c29bf-148">Appelez la méthode d’extension <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> au moment de générer l’hôte web en spécifiant les <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions> nécessaires :</span><span class="sxs-lookup"><span data-stu-id="c29bf-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building Web Host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="c29bf-149">Le reste de la configuration de HTTP.sys est géré par le biais des [paramètres du Registre](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span><span class="sxs-lookup"><span data-stu-id="c29bf-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="c29bf-150">**Options HTTP.sys**</span><span class="sxs-lookup"><span data-stu-id="c29bf-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="c29bf-151">Property</span><span class="sxs-lookup"><span data-stu-id="c29bf-151">Property</span></span> | <span data-ttu-id="c29bf-152">Description</span><span class="sxs-lookup"><span data-stu-id="c29bf-152">Description</span></span> | <span data-ttu-id="c29bf-153">Par défaut</span><span class="sxs-lookup"><span data-stu-id="c29bf-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="c29bf-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="c29bf-154">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="c29bf-155">Contrôle si l’entrée/sortie synchrone est autorisée pour le `HttpContext.Request.Body` et le `HttpContext.Response.Body`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="c29bf-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="c29bf-156">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="c29bf-157">Autorise les requêtes anonymes.</span><span class="sxs-lookup"><span data-stu-id="c29bf-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="c29bf-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="c29bf-158">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="c29bf-159">Spécifie les schémas d’authentification autorisés.</span><span class="sxs-lookup"><span data-stu-id="c29bf-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="c29bf-160">Peut être modifié à tout moment avant la suppression de l’écouteur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="c29bf-161">Les valeurs sont fournies par [l’enum AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) : `Basic`, `Kerberos`, `Negotiate`, `None` et `NTLM`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-161">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="c29bf-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="c29bf-162">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="c29bf-163">Tente la mise en cache [en mode noyau](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) pour les réponses comportant un en-tête compatible.</span><span class="sxs-lookup"><span data-stu-id="c29bf-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="c29bf-164">La réponse peut ne pas inclure d’en-tête `Set-Cookie`, `Vary` ou `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="c29bf-165">Elle doit comporter un en-tête `Cache-Control` `public` et soit une valeur `shared-max-age` ou `max-age`, soit un en-tête `Expires`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="c29bf-166">Nombre maximal d'acceptations simultanées.</span><span class="sxs-lookup"><span data-stu-id="c29bf-166">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="c29bf-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="c29bf-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="c29bf-168">Nombre maximum de connexions simultanées à accepter.</span><span class="sxs-lookup"><span data-stu-id="c29bf-168">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="c29bf-169">Utilisez `-1` pour signifier l’infini,</span><span class="sxs-lookup"><span data-stu-id="c29bf-169">Use `-1` for infinite.</span></span> <span data-ttu-id="c29bf-170">et `null` pour utiliser le paramètre du Registre qui s’applique à l’ordinateur dans son ensemble.</span><span class="sxs-lookup"><span data-stu-id="c29bf-170">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="c29bf-171">(illimité)</span><span class="sxs-lookup"><span data-stu-id="c29bf-171">(unlimited)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="c29bf-172">Consultez la section <a href="#maxrequestbodysize">MaxRequestBodySize</a>.</span><span class="sxs-lookup"><span data-stu-id="c29bf-172">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="c29bf-173">30 000 000 octets</span><span class="sxs-lookup"><span data-stu-id="c29bf-173">30000000 bytes</span></span><br><span data-ttu-id="c29bf-174">(env. 28,6 Mo)</span><span class="sxs-lookup"><span data-stu-id="c29bf-174">(~28.6 MB)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="c29bf-175">Nombre maximal de demandes pouvant être placées en file d'attente.</span><span class="sxs-lookup"><span data-stu-id="c29bf-175">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="c29bf-176">1000</span><span class="sxs-lookup"><span data-stu-id="c29bf-176">1000</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="c29bf-177">Indique si les écritures dans le corps de la réponse qui échouent en raison d’une déconnexion du client doivent lever des exceptions ou se terminer normalement.</span><span class="sxs-lookup"><span data-stu-id="c29bf-177">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="c29bf-178">(se terminer normalement)</span><span class="sxs-lookup"><span data-stu-id="c29bf-178">(complete normally)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="c29bf-179">Expose la configuration <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> de HTTP.sys, également paramétrable dans le Registre.</span><span class="sxs-lookup"><span data-stu-id="c29bf-179">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="c29bf-180">Suivez les liens de l’API pour en savoir plus sur chaque paramètre, y compris les valeurs par défaut :</span><span class="sxs-lookup"><span data-stu-id="c29bf-180">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="c29bf-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Temps alloué à l’API de serveur HTTP pour décharger le corps de l’entité sur une connexion persistante.</span><span class="sxs-lookup"><span data-stu-id="c29bf-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="c29bf-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Temps alloué pour que le corps de l'entité de la requête arrive.</span><span class="sxs-lookup"><span data-stu-id="c29bf-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="c29bf-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Temps alloué à l’API de serveur HTTP pour analyser l’en-tête de la requête.</span><span class="sxs-lookup"><span data-stu-id="c29bf-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="c29bf-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Temps alloué pour une connexion inactive.</span><span class="sxs-lookup"><span data-stu-id="c29bf-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="c29bf-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; Taux d’envoi minimal de la réponse.</span><span class="sxs-lookup"><span data-stu-id="c29bf-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="c29bf-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Temps alloué à la demande pour rester dans la file d’attente des requêtes avant que l’application ne la récupère.</span><span class="sxs-lookup"><span data-stu-id="c29bf-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="c29bf-187">Spécifiez <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> à inscrire auprès de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c29bf-187">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="c29bf-188">La plus utile est [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), qui permet d’ajouter un préfixe à la collection.</span><span class="sxs-lookup"><span data-stu-id="c29bf-188">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="c29bf-189">Ces choix peuvent être modifiés à tout moment avant la suppression de l’écouteur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-189">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>

   <span data-ttu-id="c29bf-190">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="c29bf-190">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="c29bf-191">Taille maximale autorisée pour le corps d’une demande, en octets.</span><span class="sxs-lookup"><span data-stu-id="c29bf-191">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="c29bf-192">Lorsque la valeur est `null`, elle est illimitée.</span><span class="sxs-lookup"><span data-stu-id="c29bf-192">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="c29bf-193">Cette limite est sans effet sur les connexions mises à niveau, qui sont illimitées.</span><span class="sxs-lookup"><span data-stu-id="c29bf-193">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="c29bf-194">Pour remplacer la limite d’un seul `IActionResult` dans une application MVC ASP.NET Core, nous vous recommandons d’utiliser l’attribut <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="c29bf-194">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="c29bf-195">Une exception est levée si l’application tente de configurer la limite sur une demande dont l’application a commencé la lecture.</span><span class="sxs-lookup"><span data-stu-id="c29bf-195">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="c29bf-196">La propriété `IsReadOnly` indique si la propriété `MaxRequestBodySize` est en lecture seule, et donc s’il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="c29bf-196">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="c29bf-197">Si l’application doit remplacer <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> par requête, utilisez <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature> :</span><span class="sxs-lookup"><span data-stu-id="c29bf-197">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="c29bf-198">Si vous utilisez Visual Studio, vérifiez que l’application n’est pas configurée pour exécuter IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c29bf-198">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="c29bf-199">Dans Visual Studio, le profil de démarrage par défaut est destiné à IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c29bf-199">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="c29bf-200">Pour exécuter le projet en tant qu’application console, changez manuellement le profil sélectionné, comme dans la capture d’écran suivante :</span><span class="sxs-lookup"><span data-stu-id="c29bf-200">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![Sélectionner le profil d’application console](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="c29bf-202">Configurer Windows Server</span><span class="sxs-lookup"><span data-stu-id="c29bf-202">Configure Windows Server</span></span>

1. <span data-ttu-id="c29bf-203">Identifiez les ports à ouvrir pour l’application et utilisez le [pare-feu Windows](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) ou les applets de commande PowerShell [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) pour ouvrir les ports du pare-feu et ainsi permettre au trafic d’atteindre HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c29bf-203">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="c29bf-204">Dans les commandes et la configuration de l’application suivantes, le port 443 est utilisé.</span><span class="sxs-lookup"><span data-stu-id="c29bf-204">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="c29bf-205">Lors du déploiement sur une machine virtuelle Azure, ouvrez les ports dans le [groupe de sécurité réseau](/azure/virtual-machines/windows/nsg-quickstart-portal).</span><span class="sxs-lookup"><span data-stu-id="c29bf-205">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="c29bf-206">Dans les commandes et la configuration de l’application suivantes, le port 443 est utilisé.</span><span class="sxs-lookup"><span data-stu-id="c29bf-206">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="c29bf-207">Obtenez et installez des certificats X.509, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="c29bf-207">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="c29bf-208">Sous Windows, créez des certificats auto-signés à l’aide de la [cmdlet PowerShell New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="c29bf-208">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="c29bf-209">Vous trouverez un exemple non pris en charge sur la page [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span><span class="sxs-lookup"><span data-stu-id="c29bf-209">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="c29bf-210">Installez des certificats auto-signés ou signés par l’autorité de certification dans le magasin **Ordinateur Local** > **Personnel** du serveur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-210">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="c29bf-211">Si l’application est un [déploiement dépendant de .NET Framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), installez .NET Core, .NET Framework ou les deux (si l’application est une application .NET Core ciblant .NET Framework).</span><span class="sxs-lookup"><span data-stu-id="c29bf-211">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="c29bf-212">**.NET Core** &ndash; Si l’application nécessite .NET Core, procurez-vous et exécutez le programme d’installation de **.NET Core Runtime** à partir de [Téléchargements .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="c29bf-212">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="c29bf-213">N’installez pas le Kit SDK complet sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-213">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="c29bf-214">**.NET framework** &ndash; Si l’application nécessite .NET Framework, consultez le [Guide d’installation de .NET Framework](/dotnet/framework/install/).</span><span class="sxs-lookup"><span data-stu-id="c29bf-214">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="c29bf-215">Installez la version requise de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c29bf-215">Install the required .NET Framework.</span></span> <span data-ttu-id="c29bf-216">Le programme d’installation du .NET Framework le plus récent est disponible depuis la page [Téléchargements .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="c29bf-216">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="c29bf-217">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#framework-dependent-deployments-scd), elle inclut le runtime dans son déploiement.</span><span class="sxs-lookup"><span data-stu-id="c29bf-217">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="c29bf-218">Aucune installation de framework n’est requise sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-218">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="c29bf-219">Configurez les ports et les URL de l’application.</span><span class="sxs-lookup"><span data-stu-id="c29bf-219">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="c29bf-220">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-220">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="c29bf-221">Pour configurer les ports et les préfixes d’URL, il existe plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="c29bf-221">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="c29bf-222">Arguments de ligne de commande `urls`</span><span class="sxs-lookup"><span data-stu-id="c29bf-222">`urls` command-line argument</span></span>
   * <span data-ttu-id="c29bf-223">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="c29bf-223">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="c29bf-224">L’exemple de code suivant montre comment utiliser <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> avec l’adresse IP locale du serveur `10.0.0.4` sur le port 443 :</span><span class="sxs-lookup"><span data-stu-id="c29bf-224">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="c29bf-225">`UrlPrefixes` présente l’avantage de générer immédiatement un message d’erreur en cas de préfixe mal formé.</span><span class="sxs-lookup"><span data-stu-id="c29bf-225">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="c29bf-226">Les paramètres de `UrlPrefixes` remplacent les paramètres `UseUrls`/`urls`/`ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-226">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="c29bf-227">Par conséquent, avec `UseUrls`, `urls` et la variable d’environnement `ASPNETCORE_URLS`, il est plus facile de basculer entre Kestrel et HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c29bf-227">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="c29bf-228">Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="c29bf-228">For more information, see <xref:fundamentals/host/web-host>.</span></span>

   <span data-ttu-id="c29bf-229">HTTP.sys utilise les [formats de chaîne UrlPrefix de l’API de serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="c29bf-229">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="c29bf-230">Les liaisons génériques de niveau supérieur (`http://*:80/` et `http://+:80`) ne doivent **pas** être utilisées.</span><span class="sxs-lookup"><span data-stu-id="c29bf-230">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="c29bf-231">Les liaisons génériques de niveau supérieur créent des failles de sécurité dans l’application.</span><span class="sxs-lookup"><span data-stu-id="c29bf-231">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="c29bf-232">Cela s’applique aux caractères génériques forts et faibles.</span><span class="sxs-lookup"><span data-stu-id="c29bf-232">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="c29bf-233">Utilisez des noms d’hôte ou des adresses IP explicites plutôt que des caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="c29bf-233">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="c29bf-234">Une liaison générique de sous-domaine (par exemple, `*.mysub.com`) ne constitue pas un risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="c29bf-234">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="c29bf-235">Pour plus d’informations, consultez [RFC 7230 : Section 5.4 : Hôte](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="c29bf-235">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="c29bf-236">Préenregistrez les préfixes d’URL sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-236">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="c29bf-237">L’outil intégré pour configurer HTTP.sys est *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="c29bf-237">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="c29bf-238">*netsh.exe* permet de réserver des préfixes d’URL et d’assigner des certificats X.509.</span><span class="sxs-lookup"><span data-stu-id="c29bf-238">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="c29bf-239">L’outil requiert des privilèges Administrateur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-239">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="c29bf-240">Utilisez l’outil *netsh.exe* pour enregistrer les URL pour l’application :</span><span class="sxs-lookup"><span data-stu-id="c29bf-240">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="c29bf-241">`<URL>` &ndash; L’URL (Uniform Resource Locator) complète.</span><span class="sxs-lookup"><span data-stu-id="c29bf-241">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="c29bf-242">N’utilisez pas une liaison de caractère générique.</span><span class="sxs-lookup"><span data-stu-id="c29bf-242">Don't use a wildcard binding.</span></span> <span data-ttu-id="c29bf-243">Utilisez un nom d’hôte ou une adresse IP locale valide.</span><span class="sxs-lookup"><span data-stu-id="c29bf-243">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="c29bf-244">*L’URL doit inclure une barre oblique de fin.*</span><span class="sxs-lookup"><span data-stu-id="c29bf-244">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="c29bf-245">`<USER>` &ndash; Spécifie le nom de l’utilisateur ou du groupe d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="c29bf-245">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="c29bf-246">Dans l’exemple suivant, l’adresse IP locale du serveur est `10.0.0.4` :</span><span class="sxs-lookup"><span data-stu-id="c29bf-246">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="c29bf-247">Lorsqu’une URL est inscrite, l’outil répond avec `URL reservation successfully added`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-247">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="c29bf-248">Pour supprimer une URL inscrite, utilisez la commande `delete urlacl` :</span><span class="sxs-lookup"><span data-stu-id="c29bf-248">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="c29bf-249">Inscrivez les certificats X.509 sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-249">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="c29bf-250">Utilisez l’outil *netsh.exe* pour enregistrer les certificats pour l’application :</span><span class="sxs-lookup"><span data-stu-id="c29bf-250">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="c29bf-251">`<IP>` &ndash; Spécifie l’adresse IP locale pour la liaison.</span><span class="sxs-lookup"><span data-stu-id="c29bf-251">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="c29bf-252">N’utilisez pas une liaison de caractère générique.</span><span class="sxs-lookup"><span data-stu-id="c29bf-252">Don't use a wildcard binding.</span></span> <span data-ttu-id="c29bf-253">Utilisez une adresse IP valide.</span><span class="sxs-lookup"><span data-stu-id="c29bf-253">Use a valid IP address.</span></span>
   * <span data-ttu-id="c29bf-254">`<PORT>` &ndash; Spécifie le port pour la liaison.</span><span class="sxs-lookup"><span data-stu-id="c29bf-254">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="c29bf-255">`<THUMBPRINT>`&ndash; Empreinte du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="c29bf-255">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="c29bf-256">`<GUID>` &ndash; Un GUID généré par le développeur pour représenter l’application à titre d’information.</span><span class="sxs-lookup"><span data-stu-id="c29bf-256">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="c29bf-257">À titre de référence, stockez le GUID dans l’application en tant que balise de package :</span><span class="sxs-lookup"><span data-stu-id="c29bf-257">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="c29bf-258">Dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="c29bf-258">In Visual Studio:</span></span>
     * <span data-ttu-id="c29bf-259">Ouvrez les propriétés du projet de l’application en cliquant avec le bouton droit sur l’application dans **l’Explorateur de solutions** et en sélectionnant **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="c29bf-259">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="c29bf-260">Sélectionnez l’onglet **Package**.</span><span class="sxs-lookup"><span data-stu-id="c29bf-260">Select the **Package** tab.</span></span>
     * <span data-ttu-id="c29bf-261">Entrez le GUID que vous avez créé dans le champ **Balises**.</span><span class="sxs-lookup"><span data-stu-id="c29bf-261">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="c29bf-262">Si vous n’utilisez pas Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="c29bf-262">When not using Visual Studio:</span></span>
     * <span data-ttu-id="c29bf-263">Ouvrez le chemin d’accès vers le fichier de projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="c29bf-263">Open the app's project file.</span></span>
     * <span data-ttu-id="c29bf-264">Ajoutez une propriété `<PackageTags>` à un `<PropertyGroup>` nouveau ou existant avec le GUID que vous avez créé :</span><span class="sxs-lookup"><span data-stu-id="c29bf-264">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="c29bf-265">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="c29bf-265">In the following example:</span></span>

   * <span data-ttu-id="c29bf-266">L’adresse IP locale du serveur est `10.0.0.4`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-266">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="c29bf-267">Un générateur de GUID aléatoire en ligne fournit la valeur `appid`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-267">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="c29bf-268">Lorsqu’un certificat est inscrit, l’outil répond avec `SSL Certificate successfully added`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-268">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="c29bf-269">Pour supprimer l’inscription d’un certificat, utilisez la commande `delete sslcert` :</span><span class="sxs-lookup"><span data-stu-id="c29bf-269">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="c29bf-270">Documentation de référence de *netsh.exe* :</span><span class="sxs-lookup"><span data-stu-id="c29bf-270">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="c29bf-271">Commandes netsh pour HTTP (Hypertext Transfer Protocol)</span><span class="sxs-lookup"><span data-stu-id="c29bf-271">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="c29bf-272">Chaînes UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="c29bf-272">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="c29bf-273">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="c29bf-273">Run the app.</span></span>

   <span data-ttu-id="c29bf-274">Les privilèges administrateur ne sont pas requis pour exécuter l’application lors d’une liaison à localhost à l’aide de HTTP (pas HTTPS) avec un numéro de port supérieur à 1024.</span><span class="sxs-lookup"><span data-stu-id="c29bf-274">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="c29bf-275">Pour les autres configurations (par exemple, l’utilisation d’une adresse IP locale ou la liaison au port 443), exécutez l’application avec des privilèges administrateur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-275">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="c29bf-276">L’application répond à l’adresse IP publique du serveur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-276">The app responds at the server's public IP address.</span></span> <span data-ttu-id="c29bf-277">Dans cet exemple, le serveur est contacté à partir d’Internet à son adresse IP publique `104.214.79.47`.</span><span class="sxs-lookup"><span data-stu-id="c29bf-277">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="c29bf-278">Un certificat de développement est utilisé dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="c29bf-278">A development certificate is used in this example.</span></span> <span data-ttu-id="c29bf-279">La page est chargée en toute sécurité après le contournement de l’avertissement de certificat non approuvé du navigateur.</span><span class="sxs-lookup"><span data-stu-id="c29bf-279">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![Fenêtre de navigateur affichant la page Index de l’application chargée](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="c29bf-281">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="c29bf-281">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="c29bf-282">Pour les applications hébergées par HTTP.sys qui interagissent avec les demandes provenant d’Internet ou d’un réseau d’entreprise, une configuration supplémentaire peut être nécessaire en cas d’hébergement derrière des serveurs proxy et des équilibreurs de charge.</span><span class="sxs-lookup"><span data-stu-id="c29bf-282">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="c29bf-283">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="c29bf-283">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c29bf-284">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c29bf-284">Additional resources</span></span>

* [<span data-ttu-id="c29bf-285">Activer l’authentification Windows avec HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c29bf-285">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#httpsys)
* [<span data-ttu-id="c29bf-286">API de serveur HTTP</span><span class="sxs-lookup"><span data-stu-id="c29bf-286">HTTP Server API</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [<span data-ttu-id="c29bf-287">Référentiel GitHub aspnet/HttpSysServer (code source)</span><span class="sxs-lookup"><span data-stu-id="c29bf-287">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="c29bf-288">L’hôte</span><span class="sxs-lookup"><span data-stu-id="c29bf-288">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
