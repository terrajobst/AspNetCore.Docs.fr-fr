---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Découvrez comment exiger HTTPS/TLS dans une application web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: ab407436afb16687fa285a836b608ad2e6a4802f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894556"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="56cd2-103">Appliquer HTTPS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56cd2-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="56cd2-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="56cd2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="56cd2-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="56cd2-105">This document shows how to:</span></span>

* <span data-ttu-id="56cd2-106">Exiger s-HTTP pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="56cd2-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="56cd2-107">Rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="56cd2-108">Aucune API ne peut empêcher un client d’envoyer des données sensibles à la première demande.</span><span class="sxs-lookup"><span data-stu-id="56cd2-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="56cd2-109">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="56cd2-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="56cd2-110">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="56cd2-111">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="56cd2-112">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="56cd2-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="56cd2-113">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="56cd2-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="56cd2-114">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="56cd2-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="56cd2-115">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="56cd2-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="56cd2-116">Exiger HTTPS</span><span class="sxs-lookup"><span data-stu-id="56cd2-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="56cd2-117">Nous recommandons que la production ASP.NET Core appel d’applications web :</span><span class="sxs-lookup"><span data-stu-id="56cd2-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="56cd2-118">Intergiciel (middleware) la Redirection du protocole HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) pour rediriger les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="56cd2-119">Intergiciel (middleware) HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) pour envoyer des en-têtes de protocole HTTP Strict Transport Security (HSTS) aux clients.</span><span class="sxs-lookup"><span data-stu-id="56cd2-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="56cd2-120">Les applications déployées dans une configuration de proxy inverse autoriser le proxy à gérer la sécurité de connexion (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="56cd2-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="56cd2-121">Si le proxy gère également la redirection HTTPS, il est inutile d’utiliser l’intergiciel (middleware) la Redirection de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="56cd2-122">Si le serveur proxy gère également l’écriture des en-têtes HSTS (par exemple, [HSTS natifs prend en charge dans IIS 10.0 (1709) ou version ultérieure](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS intergiciel (middleware) n’est pas requis par l’application.</span><span class="sxs-lookup"><span data-stu-id="56cd2-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="56cd2-123">Pour plus d’informations, consultez [de refus de HTTPS/HSTS sur la création du projet](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="56cd2-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="56cd2-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="56cd2-124">UseHttpsRedirection</span></span>

<span data-ttu-id="56cd2-125">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="56cd2-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="56cd2-126">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="56cd2-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="56cd2-127">Utilise la valeur par défaut [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="56cd2-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="56cd2-128">Utilise la valeur par défaut [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), sauf substitution par le `ASPNETCORE_HTTPS_PORT` variable d’environnement ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="56cd2-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="56cd2-129">Nous vous recommandons d’utiliser des redirections temporaires au lieu des redirections permanentes.</span><span class="sxs-lookup"><span data-stu-id="56cd2-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="56cd2-130">Lien de mise en cache peut provoquer un comportement instable dans les environnements de développement.</span><span class="sxs-lookup"><span data-stu-id="56cd2-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="56cd2-131">Si vous préférez envoyer un code d’état de redirection permanente lorsque l’application est dans un environnement non liées au développement, consultez le [configurer des redirections permanentes en production](#configure-permanent-redirects-in-production) section.</span><span class="sxs-lookup"><span data-stu-id="56cd2-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="56cd2-132">Nous vous recommandons d’utiliser [HSTS](#http-strict-transport-security-protocol-hsts) pour signaler aux clients qui sécurisent uniquement les ressources des demandes doivent être envoyés à l’application (uniquement en production).</span><span class="sxs-lookup"><span data-stu-id="56cd2-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="56cd2-133">Configuration du port</span><span class="sxs-lookup"><span data-stu-id="56cd2-133">Port configuration</span></span>

<span data-ttu-id="56cd2-134">Un port doit être disponible pour l’intergiciel (middleware) pour rediriger une requête non sécurisée vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="56cd2-135">Si aucun port n’est disponible :</span><span class="sxs-lookup"><span data-stu-id="56cd2-135">If no port is available:</span></span>

* <span data-ttu-id="56cd2-136">Redirection vers HTTPS ne se produit.</span><span class="sxs-lookup"><span data-stu-id="56cd2-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="56cd2-137">L’intergiciel (middleware) consigne l’avertissement « Échoué pour déterminer le port https pour la redirection ».</span><span class="sxs-lookup"><span data-stu-id="56cd2-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="56cd2-138">Spécifiez le port HTTPS en utilisant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="56cd2-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="56cd2-139">Définissez [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="56cd2-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="56cd2-140">Définir le `ASPNETCORE_HTTPS_PORT` variable d’environnement ou [paramètre de configuration d’hôte Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="56cd2-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="56cd2-141">**Clé**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="56cd2-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="56cd2-142">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="56cd2-142">**Type**: *string*</span></span>  
  <span data-ttu-id="56cd2-143">**Par défaut** : Aucune valeur par défaut n’est définie.</span><span class="sxs-lookup"><span data-stu-id="56cd2-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="56cd2-144">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="56cd2-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="56cd2-145">**Variable d’environnement**: `<PREFIX_>HTTPS_PORT` (Le préfixe est `ASPNETCORE_` lorsque vous utilisez le [hôte Web](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="56cd2-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="56cd2-146">Lorsque vous configurez un <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> dans `Program`:</span><span class="sxs-lookup"><span data-stu-id="56cd2-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="56cd2-147">Indiquer un port avec le schéma sécurisé à l’aide de la `ASPNETCORE_URLS` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="56cd2-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="56cd2-148">La variable d’environnement configure le serveur.</span><span class="sxs-lookup"><span data-stu-id="56cd2-148">The environment variable configures the server.</span></span> <span data-ttu-id="56cd2-149">L’intergiciel (middleware) détecte indirectement le port HTTPS par le biais de <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="56cd2-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="56cd2-150">Cette approche ne fonctionne pas dans les déploiements de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="56cd2-150">This approach doesn't work in reverse proxy deployments.</span></span>
* <span data-ttu-id="56cd2-151">Dans le développement, définir une URL HTTPS dans *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="56cd2-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="56cd2-152">Activer le protocole HTTPS lorsque IIS Express est utilisé.</span><span class="sxs-lookup"><span data-stu-id="56cd2-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="56cd2-153">Configurer un point de terminaison URL HTTPS pour un déploiement de périphérie destinées au public de [Kestrel](xref:fundamentals/servers/kestrel) server ou [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span><span class="sxs-lookup"><span data-stu-id="56cd2-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="56cd2-154">Uniquement **un seul port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="56cd2-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="56cd2-155">L’intergiciel (middleware) détecte le port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="56cd2-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="56cd2-156">Quand une application est exécutée dans une configuration de proxy inverse, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="56cd2-156">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="56cd2-157">Définissez le port à l’aide d’une des autres approches décrites dans cette section.</span><span class="sxs-lookup"><span data-stu-id="56cd2-157">Set the port using one of the other approaches described in this section.</span></span>

<span data-ttu-id="56cd2-158">Quand Kestrel ou HTTP.sys est utilisée comme un serveur edge de public, Kestrel ou HTTP.sys doit être configuré pour écouter sur les deux :</span><span class="sxs-lookup"><span data-stu-id="56cd2-158">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="56cd2-159">Le port sécurisé où le client est redirigé (en règle générale, 443 dans 5001 dans le développement et de production).</span><span class="sxs-lookup"><span data-stu-id="56cd2-159">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="56cd2-160">Le port non sécurisé (en règle générale, 80 en production) et 5000 dans le développement.</span><span class="sxs-lookup"><span data-stu-id="56cd2-160">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="56cd2-161">Le port non sécurisé doit être accessible par le client afin que l’application pour recevoir une requête non sécurisée et de rediriger le client vers le port sécurisé.</span><span class="sxs-lookup"><span data-stu-id="56cd2-161">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="56cd2-162">Pour plus d’informations, consultez [configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) ou <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="56cd2-162">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="56cd2-163">Scénarios de déploiement</span><span class="sxs-lookup"><span data-stu-id="56cd2-163">Deployment scenarios</span></span>

<span data-ttu-id="56cd2-164">N’importe quel pare-feu entre le client et le serveur doit également communication ports sont ouverts pour le trafic.</span><span class="sxs-lookup"><span data-stu-id="56cd2-164">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="56cd2-165">Si les demandes sont transmises dans une configuration de proxy inverse, utilisez [intergiciel des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) avant d’appeler intergiciel (middleware) la Redirection de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-165">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="56cd2-166">Transféré les mises à jour de l’intergiciel des en-têtes le `Request.Scheme`, en utilisant le `X-Forwarded-Proto` en-tête.</span><span class="sxs-lookup"><span data-stu-id="56cd2-166">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="56cd2-167">Les autorisations de l’intergiciel (middleware) rediriger URI et autres stratégies de sécurité fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="56cd2-167">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="56cd2-168">Lors de l’intergiciel des en-têtes transférés n’est pas utilisé, l’application back-end ne peut pas recevoir le schéma correct et finir dans une boucle de redirection.</span><span class="sxs-lookup"><span data-stu-id="56cd2-168">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="56cd2-169">Un message d’erreur utilisateur final commun est que trop de redirections ont eu lieu.</span><span class="sxs-lookup"><span data-stu-id="56cd2-169">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="56cd2-170">Lorsque vous déployez sur Azure App Service, suivez les instructions de [didacticiel : Lier un certificat SSL personnalisé existant à Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="56cd2-170">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="56cd2-171">Options</span><span class="sxs-lookup"><span data-stu-id="56cd2-171">Options</span></span>

<span data-ttu-id="56cd2-172">Les appels de code en surbrillance suivantes [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="56cd2-172">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="56cd2-173">Appel `AddHttpsRedirection` est uniquement nécessaire de modifier les valeurs de `HttpsPort` ou `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="56cd2-173">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="56cd2-174">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="56cd2-174">The preceding highlighted code:</span></span>

* <span data-ttu-id="56cd2-175">Jeux [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) à <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="56cd2-175">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="56cd2-176">Utilisez les champs de la <xref:Microsoft.AspNetCore.Http.StatusCodes> classe les affectations à `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="56cd2-176">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="56cd2-177">Définit le port HTTPS par 5001.</span><span class="sxs-lookup"><span data-stu-id="56cd2-177">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="56cd2-178">La valeur par défaut est 443.</span><span class="sxs-lookup"><span data-stu-id="56cd2-178">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="56cd2-179">Configurer des redirections permanentes en production</span><span class="sxs-lookup"><span data-stu-id="56cd2-179">Configure permanent redirects in production</span></span>

<span data-ttu-id="56cd2-180">Valeur par défaut est de l’intergiciel (middleware) d’envoyer un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) avec toutes les redirections.</span><span class="sxs-lookup"><span data-stu-id="56cd2-180">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="56cd2-181">Si vous préférez envoyer un code d’état de redirection permanente lorsque l’application est dans un environnement non liées au développement, encapsulez la configuration des options intergiciel (middleware) dans un contrôle conditionnel pour un environnement de développement non.</span><span class="sxs-lookup"><span data-stu-id="56cd2-181">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="56cd2-182">Lorsque vous configurez un `IWebHostBuilder` dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="56cd2-182">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

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

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="56cd2-183">Intergiciel (middleware) de HTTPS Redirection autre approche</span><span class="sxs-lookup"><span data-stu-id="56cd2-183">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="56cd2-184">Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser l’intergiciel de réécriture d’URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="56cd2-184">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="56cd2-185">`AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="56cd2-185">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="56cd2-186">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="56cd2-186">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="56cd2-187">Lors de la redirection vers HTTPS sans la nécessité pour les règles de redirection supplémentaire, nous vous recommandons d’utiliser HTTPS Redirection Middleware (`UseHttpsRedirection`) décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="56cd2-187">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="56cd2-188">Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger s-HTTP.</span><span class="sxs-lookup"><span data-stu-id="56cd2-188">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="56cd2-189">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peuvent être appliquées globalement.</span><span class="sxs-lookup"><span data-stu-id="56cd2-189">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="56cd2-190">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="56cd2-190">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="56cd2-191">Le code précédent en surbrillance nécessite d’utilisent toutes les demandes `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="56cd2-191">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="56cd2-192">Le code en surbrillance suivant redirige toutes les requêtes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="56cd2-192">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="56cd2-193">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="56cd2-193">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="56cd2-194">Le middleware permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="56cd2-194">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="56cd2-195">Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité,</span><span class="sxs-lookup"><span data-stu-id="56cd2-195">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="56cd2-196">car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`.</span><span class="sxs-lookup"><span data-stu-id="56cd2-196">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="56cd2-197">Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="56cd2-197">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="56cd2-198">Protocole de sécurité Strict Transport HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="56cd2-198">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="56cd2-199">Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de sécurité à accepter qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse.</span><span class="sxs-lookup"><span data-stu-id="56cd2-199">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="56cd2-200">Quand un [navigateur qui prend en charge HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) reçoit cet en-tête :</span><span class="sxs-lookup"><span data-stu-id="56cd2-200">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="56cd2-201">Le navigateur stocke la configuration pour le domaine qui empêche l’envoi de toutes les communications via HTTP.</span><span class="sxs-lookup"><span data-stu-id="56cd2-201">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="56cd2-202">Le navigateur force toutes les communications via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-202">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="56cd2-203">Le navigateur empêche l’utilisateur de l’utilisation de certificats non approuvés ou non valides.</span><span class="sxs-lookup"><span data-stu-id="56cd2-203">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="56cd2-204">Le navigateur désactive les invites qui permettent à un utilisateur de confiance à ce certificat.</span><span class="sxs-lookup"><span data-stu-id="56cd2-204">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="56cd2-205">Étant donné que HSTS est appliquée par le client, il présente certaines limitations :</span><span class="sxs-lookup"><span data-stu-id="56cd2-205">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="56cd2-206">Le client doit prendre en charge HSTS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-206">The client must support HSTS.</span></span>
* <span data-ttu-id="56cd2-207">HSTS nécessite au moins une demande HTTPS réussie pour établir la stratégie HSTS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-207">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="56cd2-208">L’application doit vérifier chaque requête HTTP et rediriger ou rejeter la demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="56cd2-208">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="56cd2-209">ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="56cd2-209">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="56cd2-210">Le code suivant appelle `UseHsts` lorsque l’application n’est pas dans [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="56cd2-210">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="56cd2-211">`UseHsts` n’est pas recommandée dans le développement, car les paramètres HSTS sont hautement mis en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="56cd2-211">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="56cd2-212">Par défaut, `UseHsts` exclut l’adresse de bouclage local.</span><span class="sxs-lookup"><span data-stu-id="56cd2-212">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="56cd2-213">Pour les environnements de production mise en œuvre HTTPS pour la première fois, définissez initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) sur une valeur faible à l’aide d’une de la <xref:System.TimeSpan> méthodes.</span><span class="sxs-lookup"><span data-stu-id="56cd2-213">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="56cd2-214">Définissez la valeur des heures non plus d’une seule journée au cas où vous deviez restaurer l’infrastructure HTTPS vers HTTP.</span><span class="sxs-lookup"><span data-stu-id="56cd2-214">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="56cd2-215">Une fois que vous êtes ainsi certain de la durabilité de la configuration de HTTPS, augmentez la valeur de max-age HSTS ; une valeur couramment utilisée est un an.</span><span class="sxs-lookup"><span data-stu-id="56cd2-215">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="56cd2-216">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="56cd2-216">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="56cd2-217">Définit le paramètre de préchargement de l’en-tête Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="56cd2-217">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="56cd2-218">Préchargement ne fait pas partie de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur la nouvelle installation.</span><span class="sxs-lookup"><span data-stu-id="56cd2-218">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="56cd2-219">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="56cd2-219">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="56cd2-220">Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS à héberger des sous-domaines.</span><span class="sxs-lookup"><span data-stu-id="56cd2-220">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="56cd2-221">Définit explicitement le paramètre d’âge maximal de l’en-tête Strict-Transport-Security à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="56cd2-221">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="56cd2-222">Si ce n’est pas définie, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="56cd2-222">If not set, defaults to 30 days.</span></span> <span data-ttu-id="56cd2-223">Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="56cd2-223">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="56cd2-224">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="56cd2-224">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="56cd2-225">`UseHsts` exclut les hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="56cd2-225">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="56cd2-226">`localhost` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="56cd2-226">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="56cd2-227">`127.0.0.1` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="56cd2-227">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="56cd2-228">`[::1]` : L’adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="56cd2-228">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="56cd2-229">Annulations de HTTPS/HSTS sur la création du projet</span><span class="sxs-lookup"><span data-stu-id="56cd2-229">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="56cd2-230">Dans certains scénarios de service back-end où la sécurité de la connexion est gérée en périphérie du réseau destinées au public, configuration de sécurité de la connexion au niveau de chaque nœud n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="56cd2-230">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="56cd2-231">Généré à partir des modèles dans Visual Studio ou à partir des applications Web le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande Activer [la redirection HTTPS](#require-https) et [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="56cd2-231">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="56cd2-232">Pour les déploiements ne nécessitant pas de ces scénarios, vous pouvez adhérer à de HTTPS/HSTS lorsque l’application est créée à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="56cd2-232">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="56cd2-233">Pour adhérer à de HTTPS/HSTS :</span><span class="sxs-lookup"><span data-stu-id="56cd2-233">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="56cd2-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56cd2-234">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="56cd2-235">Désactivez le **configurer pour le protocole HTTPS** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="56cd2-235">Uncheck the **Configure for HTTPS** check box.</span></span>

![Nouvelle Application Web ASP.NET Core boîte de dialogue affichant la configurer pour la case à cocher HTTPS non sélectionné.](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="56cd2-237">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="56cd2-237">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="56cd2-238">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="56cd2-238">Use the `--no-https` option.</span></span> <span data-ttu-id="56cd2-239">Exemple :</span><span class="sxs-lookup"><span data-stu-id="56cd2-239">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="56cd2-240">Approuver le certificat de développement ASP.NET Core HTTPS sur Windows et Mac OS</span><span class="sxs-lookup"><span data-stu-id="56cd2-240">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="56cd2-241">SDK .NET core inclut un certificat de développement de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56cd2-241">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="56cd2-242">Le certificat est installé en tant que partie de l’expérience de première exécution.</span><span class="sxs-lookup"><span data-stu-id="56cd2-242">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="56cd2-243">Par exemple, `dotnet --info` produit une sortie similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="56cd2-243">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="56cd2-244">L’installation du kit SDK .NET Core installe le certificat de développement ASP.NET Core HTTPS dans le magasin de certificats de l’utilisateur local.</span><span class="sxs-lookup"><span data-stu-id="56cd2-244">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="56cd2-245">Le certificat a été installé, mais il n’a pas été approuvé.</span><span class="sxs-lookup"><span data-stu-id="56cd2-245">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="56cd2-246">Pour approuver le certificat procédez à usage unique pour exécuter le dotnet `dev-certs` outil :</span><span class="sxs-lookup"><span data-stu-id="56cd2-246">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="56cd2-247">La commande suivante fournit de l’aide sur l’outil `dev-certs` :</span><span class="sxs-lookup"><span data-stu-id="56cd2-247">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="56cd2-248">Comment configurer un certificat de développeur pour Docker</span><span class="sxs-lookup"><span data-stu-id="56cd2-248">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="56cd2-249">Consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="56cd2-249">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="56cd2-250">Informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="56cd2-250">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="56cd2-251">Héberger ASP.NET Core sur Linux avec Apache : Configuration de HTTPS</span><span class="sxs-lookup"><span data-stu-id="56cd2-251">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="56cd2-252">Héberger ASP.NET Core sur Linux avec Nginx : Configuration de HTTPS</span><span class="sxs-lookup"><span data-stu-id="56cd2-252">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="56cd2-253">Procédure pour configurer SSL sur IIS</span><span class="sxs-lookup"><span data-stu-id="56cd2-253">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="56cd2-254">Prise en charge des navigateurs OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="56cd2-254">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
