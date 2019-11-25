---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Découvrez comment exiger le protocole HTTPs/TLS dans une application Web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/14/2019
uid: security/enforcing-ssl
ms.openlocfilehash: 82cd2e52f3bd929682b9eae24611ad04fd9f8682
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317368"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="9d918-103">Appliquer HTTPS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d918-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="9d918-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9d918-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9d918-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="9d918-105">This document shows how to:</span></span>

* <span data-ttu-id="9d918-106">Exiger le protocole HTTPs pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="9d918-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="9d918-107">Redirige toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d918-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="9d918-108">Aucune API ne peut empêcher un client d’envoyer des données sensibles à la première demande.</span><span class="sxs-lookup"><span data-stu-id="9d918-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="9d918-109">Projets d’API</span><span class="sxs-lookup"><span data-stu-id="9d918-109">API projects</span></span>
>
> <span data-ttu-id="9d918-110">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="9d918-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="9d918-111">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d918-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="9d918-112">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d918-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="9d918-113">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d918-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="9d918-114">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="9d918-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="9d918-115">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d918-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="9d918-116">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="9d918-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="9d918-117">Projets HSTS et API</span><span class="sxs-lookup"><span data-stu-id="9d918-117">HSTS and API projects</span></span>
>
> <span data-ttu-id="9d918-118">Les projets d’API par défaut n’incluent pas [HSTS](#hsts) , car HSTS est généralement une instruction de navigateur uniquement.</span><span class="sxs-lookup"><span data-stu-id="9d918-118">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="9d918-119">Les autres appelants, tels que les applications de bureau ou de téléphone, ne respectent **pas** l’instruction.</span><span class="sxs-lookup"><span data-stu-id="9d918-119">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="9d918-120">Même dans les navigateurs, un seul appel authentifié à une API sur HTTP présente des risques sur les réseaux non sécurisés.</span><span class="sxs-lookup"><span data-stu-id="9d918-120">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="9d918-121">L’approche sécurisée consiste à configurer des projets d’API pour écouter et répondre uniquement au protocole HTTPs.</span><span class="sxs-lookup"><span data-stu-id="9d918-121">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="9d918-122">Projets d’API</span><span class="sxs-lookup"><span data-stu-id="9d918-122">API projects</span></span>
>
> <span data-ttu-id="9d918-123">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="9d918-123">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="9d918-124">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d918-124">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="9d918-125">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d918-125">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="9d918-126">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d918-126">Such clients may send information over HTTP.</span></span> <span data-ttu-id="9d918-127">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="9d918-127">Web APIs should either:</span></span>
>
> * <span data-ttu-id="9d918-128">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d918-128">Not listen on HTTP.</span></span>
> * <span data-ttu-id="9d918-129">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="9d918-129">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="9d918-130">Exiger HTTPS</span><span class="sxs-lookup"><span data-stu-id="9d918-130">Require HTTPS</span></span>

<span data-ttu-id="9d918-131">Nous vous recommandons d’utiliser les applications Web de production ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="9d918-131">We recommend that production ASP.NET Core web apps use:</span></span>

* <span data-ttu-id="9d918-132">Intergiciel (middleware) de redirection HTTPs (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) pour rediriger les requêtes HTTP vers HTTPs.</span><span class="sxs-lookup"><span data-stu-id="9d918-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="9d918-133">Intergiciel (middleware) HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) pour envoyer des en-têtes HSTS (protocole de sécurité de transport strict) aux clients.</span><span class="sxs-lookup"><span data-stu-id="9d918-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="9d918-134">Les applications déployées dans une configuration de proxy inverse autorisent le proxy à gérer la sécurité de la connexion (HTTPs).</span><span class="sxs-lookup"><span data-stu-id="9d918-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="9d918-135">Si le proxy gère également la redirection HTTPs, il n’est pas nécessaire d’utiliser l’intergiciel (middleware) de redirection HTTPs.</span><span class="sxs-lookup"><span data-stu-id="9d918-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="9d918-136">Si le serveur proxy gère également l’écriture d’en-têtes HSTS (par exemple, la [prise en charge native de HSTS dans IIS 10,0 (1709) ou version ultérieure](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), l’application ne nécessite pas d’intergiciel HSTS.</span><span class="sxs-lookup"><span data-stu-id="9d918-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="9d918-137">Pour plus d’informations, consultez [opt-out de https/HSTS lors de la création du projet](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="9d918-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="9d918-138">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="9d918-138">UseHttpsRedirection</span></span>

<span data-ttu-id="9d918-139">Le code suivant appelle `UseHttpsRedirection` dans la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="9d918-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

<span data-ttu-id="9d918-140">Code mis en surbrillance précédent :</span><span class="sxs-lookup"><span data-stu-id="9d918-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="9d918-141">Utilise la valeur par défaut [HttpsRedirectionOptions. RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="9d918-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="9d918-142">Utilise la valeur par défaut [HttpsRedirectionOptions. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), sauf si elle est remplacée par la variable d’environnement `ASPNETCORE_HTTPS_PORT` ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="9d918-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="9d918-143">Nous vous recommandons d’utiliser des redirections temporaires plutôt que des redirections permanentes.</span><span class="sxs-lookup"><span data-stu-id="9d918-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="9d918-144">La mise en cache des liens peut provoquer un comportement instable dans les environnements de développement.</span><span class="sxs-lookup"><span data-stu-id="9d918-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="9d918-145">Si vous préférez envoyer un code d’état de redirection permanent lorsque l’application se trouve dans un environnement de non-développement, consultez la section [configurer des redirections permanentes en production](#configure-permanent-redirects-in-production) .</span><span class="sxs-lookup"><span data-stu-id="9d918-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="9d918-146">Nous vous recommandons d’utiliser [HSTS](#http-strict-transport-security-protocol-hsts) pour signaler aux clients que seules les demandes de ressources sécurisées doivent être envoyées à l’application (uniquement en production).</span><span class="sxs-lookup"><span data-stu-id="9d918-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="9d918-147">Configuration du port</span><span class="sxs-lookup"><span data-stu-id="9d918-147">Port configuration</span></span>

<span data-ttu-id="9d918-148">Un port doit être disponible pour que l’intergiciel redirige une requête non sécurisée vers HTTPs.</span><span class="sxs-lookup"><span data-stu-id="9d918-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="9d918-149">Si aucun port n’est disponible :</span><span class="sxs-lookup"><span data-stu-id="9d918-149">If no port is available:</span></span>

* <span data-ttu-id="9d918-150">La redirection vers HTTPs ne se produit pas.</span><span class="sxs-lookup"><span data-stu-id="9d918-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="9d918-151">L’intergiciel enregistre l’avertissement « échec de la détermination du port HTTPS pour la redirection ».</span><span class="sxs-lookup"><span data-stu-id="9d918-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="9d918-152">Spécifiez le port HTTPs à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d918-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="9d918-153">Définissez [HttpsRedirectionOptions. HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="9d918-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="9d918-154">Définissez le [paramètre d’hôte](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port)`https_port` :</span><span class="sxs-lookup"><span data-stu-id="9d918-154">Set the `https_port` [host setting](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port):</span></span>

  * <span data-ttu-id="9d918-155">Dans la configuration de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="9d918-155">In host configuration.</span></span>
  * <span data-ttu-id="9d918-156">En définissant la variable d’environnement `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="9d918-156">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="9d918-157">En ajoutant une entrée de niveau supérieur dans *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="9d918-157">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* <span data-ttu-id="9d918-158">Indiquez un port avec le schéma sécurisé à l’aide de la [variable d’environnement ASPNETCORE_URLS](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span><span class="sxs-lookup"><span data-stu-id="9d918-158">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span></span> <span data-ttu-id="9d918-159">La variable d’environnement configure le serveur.</span><span class="sxs-lookup"><span data-stu-id="9d918-159">The environment variable configures the server.</span></span> <span data-ttu-id="9d918-160">L’intergiciel Découvre indirectement le port HTTPs via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="9d918-160">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="9d918-161">Cette approche ne fonctionne pas dans les déploiements de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="9d918-161">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* <span data-ttu-id="9d918-162">Définissez le [paramètre d’hôte](xref:fundamentals/host/web-host#https-port)`https_port` :</span><span class="sxs-lookup"><span data-stu-id="9d918-162">Set the `https_port` [host setting](xref:fundamentals/host/web-host#https-port):</span></span>

  * <span data-ttu-id="9d918-163">Dans la configuration de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="9d918-163">In host configuration.</span></span>
  * <span data-ttu-id="9d918-164">En définissant la variable d’environnement `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="9d918-164">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="9d918-165">En ajoutant une entrée de niveau supérieur dans *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="9d918-165">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* <span data-ttu-id="9d918-166">Indiquez un port avec le schéma sécurisé à l’aide de la [variable d’environnement ASPNETCORE_URLS](xref:fundamentals/host/web-host#server-urls).</span><span class="sxs-lookup"><span data-stu-id="9d918-166">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](xref:fundamentals/host/web-host#server-urls).</span></span> <span data-ttu-id="9d918-167">La variable d’environnement configure le serveur.</span><span class="sxs-lookup"><span data-stu-id="9d918-167">The environment variable configures the server.</span></span> <span data-ttu-id="9d918-168">L’intergiciel Découvre indirectement le port HTTPs via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="9d918-168">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="9d918-169">Cette approche ne fonctionne pas dans les déploiements de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="9d918-169">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

* <span data-ttu-id="9d918-170">Dans développement, définissez une URL HTTPs dans *launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="9d918-170">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="9d918-171">Activez le protocole HTTPs lorsque IIS Express est utilisé.</span><span class="sxs-lookup"><span data-stu-id="9d918-171">Enable HTTPS when IIS Express is used.</span></span>

* <span data-ttu-id="9d918-172">Configurez un point de terminaison d’URL HTTPs pour un déploiement de périphérie public d’un serveur [Kestrel](xref:fundamentals/servers/kestrel) ou [http. sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="9d918-172">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="9d918-173">**Un seul port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="9d918-173">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="9d918-174">L’intergiciel Découvre le port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="9d918-174">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="9d918-175">Quand une application est exécutée dans une configuration de proxy inverse, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="9d918-175">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="9d918-176">Définissez le port à l’aide de l’une des autres approches décrites dans cette section.</span><span class="sxs-lookup"><span data-stu-id="9d918-176">Set the port using one of the other approaches described in this section.</span></span>

### <a name="edge-deployments"></a><span data-ttu-id="9d918-177">Déploiements de périphérie</span><span class="sxs-lookup"><span data-stu-id="9d918-177">Edge deployments</span></span> 

<span data-ttu-id="9d918-178">Quand Kestrel ou HTTP. sys est utilisé en tant que serveur Edge public, Kestrel ou HTTP. sys doivent être configurés pour écouter les deux :</span><span class="sxs-lookup"><span data-stu-id="9d918-178">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="9d918-179">Le port sécurisé où le client est redirigé (en général, 443 en production et 5001 en développement).</span><span class="sxs-lookup"><span data-stu-id="9d918-179">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="9d918-180">Le port non sécurisé (en général, 80 en production et 5000 en développement).</span><span class="sxs-lookup"><span data-stu-id="9d918-180">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="9d918-181">Le port non sécurisé doit être accessible par le client afin que l’application puisse recevoir une demande non sécurisée et rediriger le client vers le port sécurisé.</span><span class="sxs-lookup"><span data-stu-id="9d918-181">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="9d918-182">Pour plus d’informations, consultez [configuration du point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) ou <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="9d918-182">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="9d918-183">Scénarios de déploiement</span><span class="sxs-lookup"><span data-stu-id="9d918-183">Deployment scenarios</span></span>

<span data-ttu-id="9d918-184">Tout pare-feu entre le client et le serveur doit également disposer de ports de communication ouverts pour le trafic.</span><span class="sxs-lookup"><span data-stu-id="9d918-184">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="9d918-185">Si les demandes sont transférées dans une configuration de proxy inverse, utilisez l' [intergiciel d’en-tête transféré](xref:host-and-deploy/proxy-load-balancer) avant d’appeler l’intergiciel (middleware) de REdirection HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d918-185">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="9d918-186">L’intergiciel (middleware) des en-têtes transférés met à jour l' `Request.Scheme`à l’aide de l’en-tête `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="9d918-186">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="9d918-187">L’intergiciel permet de rediriger les URI et d’autres stratégies de sécurité pour fonctionner correctement.</span><span class="sxs-lookup"><span data-stu-id="9d918-187">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="9d918-188">Lorsque l’intergiciel d’en-têtes transférés n’est pas utilisé, l’application principale peut ne pas recevoir le schéma correct et se terminer dans une boucle de redirection.</span><span class="sxs-lookup"><span data-stu-id="9d918-188">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="9d918-189">Un message d’erreur de l’utilisateur final commun est qu’un trop grand nombre de redirections se sont produites.</span><span class="sxs-lookup"><span data-stu-id="9d918-189">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="9d918-190">Lors du déploiement sur Azure App Service, suivez les instructions du [Didacticiel : lier un certificat SSL personnalisé existant à Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="9d918-190">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="9d918-191">Options</span><span class="sxs-lookup"><span data-stu-id="9d918-191">Options</span></span>

<span data-ttu-id="9d918-192">Le code en surbrillance suivant appelle [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel :</span><span class="sxs-lookup"><span data-stu-id="9d918-192">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


<span data-ttu-id="9d918-193">L’appel de `AddHttpsRedirection` est nécessaire uniquement pour modifier les valeurs de `HttpsPort` ou `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="9d918-193">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="9d918-194">Code mis en surbrillance précédent :</span><span class="sxs-lookup"><span data-stu-id="9d918-194">The preceding highlighted code:</span></span>

* <span data-ttu-id="9d918-195">Définit [HttpsRedirectionOptions. RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) sur <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="9d918-195">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="9d918-196">Utilisez les champs de la classe <xref:Microsoft.AspNetCore.Http.StatusCodes> pour les assignations à `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="9d918-196">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="9d918-197">Définit le port HTTPs sur 5001.</span><span class="sxs-lookup"><span data-stu-id="9d918-197">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="9d918-198">La valeur par défaut est 443.</span><span class="sxs-lookup"><span data-stu-id="9d918-198">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="9d918-199">Configurer des redirections permanentes en production</span><span class="sxs-lookup"><span data-stu-id="9d918-199">Configure permanent redirects in production</span></span>

<span data-ttu-id="9d918-200">Par défaut, l’intergiciel envoie un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) avec toutes les redirections.</span><span class="sxs-lookup"><span data-stu-id="9d918-200">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="9d918-201">Si vous préférez envoyer un code d’état de redirection permanent lorsque l’application se trouve dans un environnement de non-développement, encapsulez la configuration des options d’intergiciel dans une vérification conditionnelle pour un environnement de non-développement.</span><span class="sxs-lookup"><span data-stu-id="9d918-201">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d918-202">Lors de la configuration des services dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d918-202">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="9d918-203">Lors de la configuration des services dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d918-203">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="9d918-204">Approche alternative de l’intergiciel (middleware) de redirection HTTPs</span><span class="sxs-lookup"><span data-stu-id="9d918-204">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="9d918-205">Une alternative à l’utilisation de l’intergiciel (middleware) de redirection HTTPs (`UseHttpsRedirection`) consiste à utiliser l’intergiciel (`AddRedirectToHttps`) de réécriture d’URL.</span><span class="sxs-lookup"><span data-stu-id="9d918-205">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="9d918-206">`AddRedirectToHttps` pouvez également définir le code et le port d’état lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="9d918-206">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="9d918-207">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="9d918-207">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="9d918-208">Lorsque vous redirigez vers HTTPs sans avoir besoin de règles de redirection supplémentaires, nous vous recommandons d’utiliser le middleware de redirection HTTPs (`UseHttpsRedirection`) décrit dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="9d918-208">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="9d918-209">Protocole HTTP strict transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="9d918-209">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="9d918-210">Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [http strict transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) est une amélioration de sécurité qui est spécifiée par une application Web via l’utilisation d’un en-tête de réponse.</span><span class="sxs-lookup"><span data-stu-id="9d918-210">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="9d918-211">Quand un [navigateur prenant en charge HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) reçoit cet en-tête :</span><span class="sxs-lookup"><span data-stu-id="9d918-211">When a [browser that supports HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) receives this header:</span></span>

* <span data-ttu-id="9d918-212">Le navigateur stocke la configuration pour le domaine qui empêche l’envoi de toute communication sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d918-212">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="9d918-213">Le navigateur force toutes les communications sur HTTPs.</span><span class="sxs-lookup"><span data-stu-id="9d918-213">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="9d918-214">Le navigateur empêche l’utilisateur d’utiliser des certificats non approuvés ou non valides.</span><span class="sxs-lookup"><span data-stu-id="9d918-214">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="9d918-215">Le navigateur désactive les invites qui permettent à un utilisateur d’approuver temporairement ce type de certificat.</span><span class="sxs-lookup"><span data-stu-id="9d918-215">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="9d918-216">Étant donné que HSTS est appliqué par le client, il présente certaines limitations :</span><span class="sxs-lookup"><span data-stu-id="9d918-216">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="9d918-217">Le client doit prendre en charge HSTS.</span><span class="sxs-lookup"><span data-stu-id="9d918-217">The client must support HSTS.</span></span>
* <span data-ttu-id="9d918-218">HSTS nécessite au moins une demande HTTPs réussie pour établir la stratégie HSTS.</span><span class="sxs-lookup"><span data-stu-id="9d918-218">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="9d918-219">L’application doit vérifier chaque requête HTTP et rediriger ou rejeter la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d918-219">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="9d918-220">ASP.NET Core 2,1 et versions ultérieures implémentent HSTS avec la méthode d’extension `UseHsts`.</span><span class="sxs-lookup"><span data-stu-id="9d918-220">ASP.NET Core 2.1 and later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="9d918-221">Le code suivant appelle `UseHsts` lorsque l’application n’est pas en [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="9d918-221">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

<span data-ttu-id="9d918-222">`UseHsts` n’est pas recommandé dans le développement, car les paramètres HSTS sont très facilement mis en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="9d918-222">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="9d918-223">Par défaut, `UseHsts` exclut l’adresse de bouclage locale.</span><span class="sxs-lookup"><span data-stu-id="9d918-223">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="9d918-224">Pour les environnements de production qui implémentent le protocole HTTPs pour la première fois, définissez [HstsOptions. MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) initiale sur une valeur faible à l’aide de l’une des méthodes <xref:System.TimeSpan>.</span><span class="sxs-lookup"><span data-stu-id="9d918-224">For production environments that are implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="9d918-225">Définissez la valeur des heures sur un seul jour si vous devez rétablir l’infrastructure HTTPs sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d918-225">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="9d918-226">Une fois que vous êtes certain de la durabilité de la configuration HTTPs, augmentez la valeur HSTS max-age. une valeur couramment utilisée est d’un an.</span><span class="sxs-lookup"><span data-stu-id="9d918-226">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="9d918-227">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="9d918-227">The following code:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* <span data-ttu-id="9d918-228">Définit le paramètre de préchargement de l’en-tête strict-transport-Security.</span><span class="sxs-lookup"><span data-stu-id="9d918-228">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="9d918-229">Le préchargement ne fait pas partie de la [spécification RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs Web pour précharger des sites HSTS sur une nouvelle installation.</span><span class="sxs-lookup"><span data-stu-id="9d918-229">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="9d918-230">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="9d918-230">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="9d918-231">Active [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), qui applique la stratégie HSTS pour héberger des sous-domaines.</span><span class="sxs-lookup"><span data-stu-id="9d918-231">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="9d918-232">Définit explicitement le paramètre max-age de l’en-tête strict-transport-Security sur 60 jours.</span><span class="sxs-lookup"><span data-stu-id="9d918-232">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="9d918-233">S’il n’est pas défini, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="9d918-233">If not set, defaults to 30 days.</span></span> <span data-ttu-id="9d918-234">Pour plus d’informations, consultez la [directive max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) .</span><span class="sxs-lookup"><span data-stu-id="9d918-234">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="9d918-235">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="9d918-235">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="9d918-236">`UseHsts` exclut les hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="9d918-236">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="9d918-237">`localhost` : adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="9d918-237">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="9d918-238">`127.0.0.1` : adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="9d918-238">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="9d918-239">`[::1]` : adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="9d918-239">`[::1]` : The IPv6 loopback address.</span></span>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="9d918-240">Désactiver HTTPs/HSTS lors de la création du projet</span><span class="sxs-lookup"><span data-stu-id="9d918-240">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="9d918-241">Dans certains scénarios de service backend où la sécurité de connexion est gérée au niveau du périmètre public du réseau, la configuration de la sécurité des connexions sur chaque nœud n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="9d918-241">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="9d918-242">Les applications Web générées à partir des modèles dans Visual Studio ou à partir de la commande [dotnet New](/dotnet/core/tools/dotnet-new) activent la [redirection https](#require-https) et [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="9d918-242">Web apps that are generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="9d918-243">Pour les déploiements qui n’ont pas besoin de ces scénarios, vous pouvez désactiver HTTPs/HSTS quand l’application est créée à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="9d918-243">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="9d918-244">Pour désactiver HTTPs/HSTS :</span><span class="sxs-lookup"><span data-stu-id="9d918-244">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d918-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d918-245">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="9d918-246">Désactivez la case à cocher **configurer pour HTTPS** .</span><span class="sxs-lookup"><span data-stu-id="9d918-246">Uncheck the **Configure for HTTPS** check box.</span></span>

::: moniker range=">= aspnetcore-3.0"

![Nouvelle boîte de dialogue d’application Web ASP.NET Core avec la case à cocher configurer pour HTTPs désélectionnée.](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![Nouvelle boîte de dialogue d’application Web ASP.NET Core avec la case à cocher configurer pour HTTPs désélectionnée.](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9d918-249">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="9d918-249">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="9d918-250">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="9d918-250">Use the `--no-https` option.</span></span> <span data-ttu-id="9d918-251">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9d918-251">For example</span></span>

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="9d918-252">Approuver le certificat de développement HTTPs ASP.NET Core sur Windows et macOS</span><span class="sxs-lookup"><span data-stu-id="9d918-252">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="9d918-253">Le kit SDK .NET Core comprend un certificat de développement HTTPs.</span><span class="sxs-lookup"><span data-stu-id="9d918-253">The .NET Core SDK includes an HTTPS development certificate.</span></span> <span data-ttu-id="9d918-254">Le certificat est installé dans le cadre de la première exécution.</span><span class="sxs-lookup"><span data-stu-id="9d918-254">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="9d918-255">Par exemple, `dotnet --info` produit une sortie similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="9d918-255">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="9d918-256">L’installation du kit SDK .NET Core installe le certificat de développement ASP.NET Core HTTPS dans le magasin de certificats de l’utilisateur local.</span><span class="sxs-lookup"><span data-stu-id="9d918-256">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="9d918-257">Le certificat a été installé, mais il n’est pas approuvé.</span><span class="sxs-lookup"><span data-stu-id="9d918-257">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="9d918-258">Pour faire confiance au certificat, effectuez l’étape unique pour exécuter l’outil dotnet `dev-certs` :</span><span class="sxs-lookup"><span data-stu-id="9d918-258">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="9d918-259">La commande suivante fournit de l’aide sur l’outil `dev-certs` :</span><span class="sxs-lookup"><span data-stu-id="9d918-259">The following command provides help on the `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="9d918-260">Comment configurer un certificat de développeur pour l’arrimeur</span><span class="sxs-lookup"><span data-stu-id="9d918-260">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="9d918-261">Consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="9d918-261">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="9d918-262">Approuver le certificat HTTPs à partir du sous-système Windows pour Linux</span><span class="sxs-lookup"><span data-stu-id="9d918-262">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="9d918-263">Le sous-système Windows pour Linux (WSL) génère un certificat auto-signé HTTPs. Pour configurer le magasin de certificats Windows de façon à ce qu’il approuve le certificat WSL :</span><span class="sxs-lookup"><span data-stu-id="9d918-263">The Windows Subsystem for Linux (WSL) generates a HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="9d918-264">Exécutez la commande suivante pour exporter le certificat généré par WSL : `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="9d918-264">Run the following command to export the WSL generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="9d918-265">Dans une fenêtre WSL, exécutez la commande suivante : `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="9d918-265">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="9d918-266">La commande précédente définit les variables d’environnement pour que Linux utilise le certificat de confiance Windows.</span><span class="sxs-lookup"><span data-stu-id="9d918-266">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="troubleshoot-certificate-problems"></a><span data-ttu-id="9d918-267">Résoudre les problèmes de certificat</span><span class="sxs-lookup"><span data-stu-id="9d918-267">Troubleshoot certificate problems</span></span>

<span data-ttu-id="9d918-268">Cette section fournit de l’aide lorsque le certificat de développement HTTPs ASP.NET Core a été [installé et approuvé](#trust), mais que vous avez encore des avertissements de navigateur indiquant que le certificat n’est pas approuvé.</span><span class="sxs-lookup"><span data-stu-id="9d918-268">This section provides help when the ASP.NET Core HTTPS development certificate has been [installed and trusted](#trust), but you still have browser warnings that the certificate is not trusted.</span></span> <span data-ttu-id="9d918-269">Le certificat de développement HTTPs ASP.NET Core est utilisé par [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="9d918-269">The ASP.NET Core HTTPS development certificate is used by [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

### <a name="all-platforms---certificate-not-trusted"></a><span data-ttu-id="9d918-270">Toutes les plateformes-certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="9d918-270">All platforms - certificate not trusted</span></span>

<span data-ttu-id="9d918-271">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d918-271">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="9d918-272">Fermez toutes les instances de navigateur ouvertes.</span><span class="sxs-lookup"><span data-stu-id="9d918-272">Close any browser instances open.</span></span> <span data-ttu-id="9d918-273">Ouvrez une nouvelle fenêtre de navigateur pour l’application.</span><span class="sxs-lookup"><span data-stu-id="9d918-273">Open a new browser window to app.</span></span> <span data-ttu-id="9d918-274">L’approbation de certificat est mise en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="9d918-274">Certificate trust is cached by browsers.</span></span>

<span data-ttu-id="9d918-275">Les commandes précédentes résolvent la plupart des problèmes d’approbation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="9d918-275">The preceding commands solve most browser trust issues.</span></span> <span data-ttu-id="9d918-276">Si le navigateur n’approuve toujours pas le certificat, suivez les suggestions spécifiques à la plateforme qui suivent.</span><span class="sxs-lookup"><span data-stu-id="9d918-276">If the browser is still not trusting the certificate, follow the platform specific suggestions that follow.</span></span>

### <a name="docker---certificate-not-trusted"></a><span data-ttu-id="9d918-277">Dockr-certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="9d918-277">Docker - certificate not trusted</span></span>

* <span data-ttu-id="9d918-278">Supprimez le dossier *C:\Users\{user} \AppData\Roaming\ASP.NET\Https* .</span><span class="sxs-lookup"><span data-stu-id="9d918-278">Delete the *C:\Users\{USER}\AppData\Roaming\ASP.NET\Https* folder.</span></span>
* <span data-ttu-id="9d918-279">Nettoyez la solution.</span><span class="sxs-lookup"><span data-stu-id="9d918-279">Clean the solution.</span></span> <span data-ttu-id="9d918-280">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="9d918-280">Delete the *bin* and *obj* folders.</span></span>
* <span data-ttu-id="9d918-281">Redémarrez l’outil de développement.</span><span class="sxs-lookup"><span data-stu-id="9d918-281">Restart the development tool.</span></span> <span data-ttu-id="9d918-282">Par exemple, Visual Studio, Visual Studio Code ou Visual Studio pour Mac.</span><span class="sxs-lookup"><span data-stu-id="9d918-282">For example, Visual Studio, Visual Studio Code, or Visual Studio for Mac.</span></span>

### <a name="windows---certificate-not-trusted"></a><span data-ttu-id="9d918-283">Windows-certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="9d918-283">Windows - certificate not trusted</span></span>

* <span data-ttu-id="9d918-284">Vérifiez les certificats dans le magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="9d918-284">Check the certificates in the certificate store.</span></span> <span data-ttu-id="9d918-285">Il doit y avoir un certificat de `localhost` avec le nom convivial `ASP.NET Core HTTPS development certificate` à la fois sous `Current User > Personal > Certificates` et `Current User > Trusted root certification authorities > Certificates`</span><span class="sxs-lookup"><span data-stu-id="9d918-285">There should be a `localhost` certificate with the `ASP.NET Core HTTPS development certificate` friendly name both under `Current User > Personal > Certificates` and `Current User > Trusted root certification authorities > Certificates`</span></span>
* <span data-ttu-id="9d918-286">Supprimez tous les certificats détectés des autorités de certification racines personnelles et de confiance.</span><span class="sxs-lookup"><span data-stu-id="9d918-286">Remove all the found certificates from both Personal and Trusted root certification authorities.</span></span> <span data-ttu-id="9d918-287">Ne supprimez **pas** le certificat IIS Express localhost.</span><span class="sxs-lookup"><span data-stu-id="9d918-287">Do **not** remove the IIS Express localhost certificate.</span></span>
* <span data-ttu-id="9d918-288">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d918-288">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="9d918-289">Fermez toutes les instances de navigateur ouvertes.</span><span class="sxs-lookup"><span data-stu-id="9d918-289">Close any browser instances open.</span></span> <span data-ttu-id="9d918-290">Ouvrez une nouvelle fenêtre de navigateur pour l’application.</span><span class="sxs-lookup"><span data-stu-id="9d918-290">Open a new browser window to app.</span></span>

### <a name="os-x---certificate-not-trusted"></a><span data-ttu-id="9d918-291">OS X-certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="9d918-291">OS X - certificate not trusted</span></span>

* <span data-ttu-id="9d918-292">Ouvrez le trousseau d’accès.</span><span class="sxs-lookup"><span data-stu-id="9d918-292">Open KeyChain Access.</span></span>
* <span data-ttu-id="9d918-293">Sélectionnez le trousseau système.</span><span class="sxs-lookup"><span data-stu-id="9d918-293">Select the System keychain.</span></span>
* <span data-ttu-id="9d918-294">Vérifiez la présence d’un certificat localhost.</span><span class="sxs-lookup"><span data-stu-id="9d918-294">Check for the presence of a localhost certificate.</span></span>
* <span data-ttu-id="9d918-295">Vérifiez qu’il contient un symbole `+` sur l’icône pour indiquer son approuvé pour tous les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="9d918-295">Check that it contains a `+` symbol on the icon to indicate its trusted for all users.</span></span>
* <span data-ttu-id="9d918-296">Supprimez le certificat du trousseau système.</span><span class="sxs-lookup"><span data-stu-id="9d918-296">Remove the certificate from the system keychain.</span></span>
* <span data-ttu-id="9d918-297">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d918-297">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="9d918-298">Fermez toutes les instances de navigateur ouvertes.</span><span class="sxs-lookup"><span data-stu-id="9d918-298">Close any browser instances open.</span></span> <span data-ttu-id="9d918-299">Ouvrez une nouvelle fenêtre de navigateur pour l’application.</span><span class="sxs-lookup"><span data-stu-id="9d918-299">Open a new browser window to app.</span></span>

<span data-ttu-id="9d918-300">Consultez [erreur HTTPS à l’aide de IIS Express (ASPNET/AspNetCore #16892)](https://github.com/aspnet/AspNetCore/issues/16892) pour résoudre les problèmes liés aux certificats dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d918-300">See [HTTPS Error using IIS Express (aspnet/AspNetCore #16892)](https://github.com/aspnet/AspNetCore/issues/16892) for troubleshooting certificate issues with Visual Studio.</span></span>

### <a name="iis-express-ssl-certificate-used-with-visual-studio"></a><span data-ttu-id="9d918-301">IIS Express certificat SSL utilisé avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d918-301">IIS Express SSL certificate used with Visual Studio</span></span>

<span data-ttu-id="9d918-302">Pour résoudre les problèmes liés au certificat de IIS Express, sélectionnez **réparer** dans le programme d’installation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d918-302">To fix problems with the IIS Express certificate, select **Repair** from the Visual Studio installer.</span></span>

## <a name="additional-information"></a><span data-ttu-id="9d918-303">Informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9d918-303">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="9d918-304">ASP.NET Core d’hôte sur Linux avec Apache : configuration HTTPs</span><span class="sxs-lookup"><span data-stu-id="9d918-304">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="9d918-305">ASP.NET Core d’hôte sur Linux avec Nginx : configuration HTTPs</span><span class="sxs-lookup"><span data-stu-id="9d918-305">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="9d918-306">Configuration du protocole SSL sur IIS</span><span class="sxs-lookup"><span data-stu-id="9d918-306">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="9d918-307">Prise en charge du navigateur OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="9d918-307">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
