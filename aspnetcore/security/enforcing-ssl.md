---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Découvrez comment exiger HTTPS/TLS dans une application web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325599"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="796a8-103">Appliquer HTTPS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="796a8-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="796a8-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="796a8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="796a8-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="796a8-105">This document shows how to:</span></span>

* <span data-ttu-id="796a8-106">Exiger s-HTTP pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="796a8-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="796a8-107">Rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="796a8-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="796a8-108">Aucune API ne peut empêcher un client d’envoyer des données sensibles à la première demande.</span><span class="sxs-lookup"><span data-stu-id="796a8-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="796a8-109">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="796a8-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="796a8-110">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="796a8-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="796a8-111">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="796a8-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="796a8-112">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="796a8-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="796a8-113">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="796a8-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="796a8-114">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="796a8-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="796a8-115">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="796a8-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>

## <a name="require-https"></a><span data-ttu-id="796a8-116">Exiger s-http</span><span class="sxs-lookup"><span data-stu-id="796a8-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="796a8-117">Nous recommandons d’ASP.NET Core de production tous les appels d’applications web :</span><span class="sxs-lookup"><span data-stu-id="796a8-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="796a8-118">Le Middleware de Redirection HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="796a8-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="796a8-119">[UseHsts](#hsts), protocole de sécurité Strict Transport HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="796a8-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="796a8-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="796a8-120">UseHttpsRedirection</span></span>

<span data-ttu-id="796a8-121">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="796a8-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="796a8-122">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="796a8-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="796a8-123">Utilise la valeur par défaut [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="796a8-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="796a8-124">Utilise la valeur par défaut [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), sauf substitution par le `ASPNETCORE_HTTPS_PORT` variable d’environnement ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="796a8-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="796a8-125">Nous vous recommandons d’utiliser effectue une redirection temporaire plutôt que des redirections permanentes, comme la mise en cache de lien peut provoquer un comportement instable dans les environnements de développement.</span><span class="sxs-lookup"><span data-stu-id="796a8-125">We recommend using temporary redirects rather than permanent redirects, as link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="796a8-126">Nous vous recommandons d’utiliser [HSTS](#hsts) pour signaler aux clients qui sécurisent uniquement les ressources des demandes doivent être envoyés à l’application (uniquement en production).</span><span class="sxs-lookup"><span data-stu-id="796a8-126">We recommend using [HSTS](#hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

> [!WARNING]
> <span data-ttu-id="796a8-127">Un port doit être disponible pour l’intergiciel (middleware) pour rediriger vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="796a8-127">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="796a8-128">Si aucun port n’est disponible, la redirection vers HTTPS ne se produit.</span><span class="sxs-lookup"><span data-stu-id="796a8-128">If no port is available, redirection to HTTPS doesn't occur.</span></span> <span data-ttu-id="796a8-129">Le port HTTPS peut être spécifié en utilisant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="796a8-129">The HTTPS port can be specified using any of the following approaches:</span></span>
>
> * <span data-ttu-id="796a8-130">Définir `HttpsRedirectionOptions.HttpsPort`.</span><span class="sxs-lookup"><span data-stu-id="796a8-130">Set `HttpsRedirectionOptions.HttpsPort`.</span></span>
> * <span data-ttu-id="796a8-131">Définissez la variable d’environnement `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="796a8-131">Set the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
> * <span data-ttu-id="796a8-132">Dans le développement, définir une URL HTTPS dans *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="796a8-132">In development, set an HTTPS URL in *launchsettings.json*.</span></span>
> * <span data-ttu-id="796a8-133">Configurer un point de terminaison URL HTTPS pour [Kestrel](xref:fundamentals/servers/kestrel) ou [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="796a8-133">Configure an HTTPS URL endpoint for [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>
>
> <span data-ttu-id="796a8-134">Quand Kestrel ou HTTP.sys est utilisée comme un serveur edge de public, Kestrel ou HTTP.sys doit être configuré pour écouter sur les deux :</span><span class="sxs-lookup"><span data-stu-id="796a8-134">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>
>
> * <span data-ttu-id="796a8-135">Le port sécurisé où le client est redirigé (en règle générale, 443 dans 5001 dans le développement et de production).</span><span class="sxs-lookup"><span data-stu-id="796a8-135">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
> * <span data-ttu-id="796a8-136">Le port non sécurisé (en règle générale, 80 en production) et 5000 dans le développement.</span><span class="sxs-lookup"><span data-stu-id="796a8-136">The insecure port (typically, 80 in production and 5000 in development).</span></span>
>
> <span data-ttu-id="796a8-137">Le port non sécurisé doit être accessible par le client afin que l’application pour recevoir une demande non sécurisée et rediriger vers le port sécurisé.</span><span class="sxs-lookup"><span data-stu-id="796a8-137">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect it to the secure port.</span></span>
>
> <span data-ttu-id="796a8-138">N’importe quel pare-feu entre le client et le serveur doit également les ports sont ouverts pour le trafic.</span><span class="sxs-lookup"><span data-stu-id="796a8-138">Any firewall between the client and server must also have the ports open for traffic.</span></span>
>
> <span data-ttu-id="796a8-139">Pour plus d’informations, consultez [configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) ou <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="796a8-139">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

<span data-ttu-id="796a8-140">Les appels de code en surbrillance suivantes [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="796a8-140">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="796a8-141">Appel `AddHttpsRedirection` est uniquement nécessaire de modifier les valeurs de `HttpsPort` ou `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="796a8-141">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="796a8-142">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="796a8-142">The preceding highlighted code:</span></span>

* <span data-ttu-id="796a8-143">Jeux [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="796a8-143">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), which is the default value.</span></span> <span data-ttu-id="796a8-144">Utilisez les champs de la [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) classe les affectations à `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="796a8-144">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="796a8-145">Définit le port HTTPS par 5001.</span><span class="sxs-lookup"><span data-stu-id="796a8-145">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="796a8-146">La valeur par défaut est 443.</span><span class="sxs-lookup"><span data-stu-id="796a8-146">The default value is 443.</span></span>

<span data-ttu-id="796a8-147">Les mécanismes suivants définir automatiquement le port :</span><span class="sxs-lookup"><span data-stu-id="796a8-147">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="796a8-148">Le middleware peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :</span><span class="sxs-lookup"><span data-stu-id="796a8-148">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

  * <span data-ttu-id="796a8-149">Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur du Code Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="796a8-149">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  * <span data-ttu-id="796a8-150">Uniquement **un seul port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="796a8-150">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="796a8-151">Visual Studio est utilisé :</span><span class="sxs-lookup"><span data-stu-id="796a8-151">Visual Studio is used:</span></span>

  * <span data-ttu-id="796a8-152">IIS Express a HTTPS activé.</span><span class="sxs-lookup"><span data-stu-id="796a8-152">IIS Express has HTTPS enabled.</span></span>
  * <span data-ttu-id="796a8-153">*launchSettings.json* définit le `sslPort` pour IIS Express.</span><span class="sxs-lookup"><span data-stu-id="796a8-153">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="796a8-154">Quand une application s’exécute derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="796a8-154">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="796a8-155">Le port doit être configuré manuellement.</span><span class="sxs-lookup"><span data-stu-id="796a8-155">The port must be manually configured.</span></span> <span data-ttu-id="796a8-156">Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="796a8-156">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="796a8-157">Le port peut être configuré en définissant le [paramètre de configuration d’hôte Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="796a8-157">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="796a8-158">**Clé**: https_port</span><span class="sxs-lookup"><span data-stu-id="796a8-158">**Key**: https_port</span></span>  
<span data-ttu-id="796a8-159">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="796a8-159">**Type**: *string*</span></span>  
<span data-ttu-id="796a8-160">**Par défaut**: une valeur par défaut n’est pas définie.</span><span class="sxs-lookup"><span data-stu-id="796a8-160">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="796a8-161">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="796a8-161">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="796a8-162">**Variable d’environnement**: `<PREFIX_>HTTPS_PORT` (le préfixe est `ASPNETCORE_` lors de l’utilisation de l’hôte Web.)</span><span class="sxs-lookup"><span data-stu-id="796a8-162">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="796a8-163">Le port peut être configuré indirectement en définissant l’URL avec le `ASPNETCORE_URLS` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="796a8-163">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="796a8-164">La variable d’environnement configure le serveur, et détecte ensuite l’intergiciel (middleware) indirectement les le port HTTPS par le biais de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="796a8-164">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="796a8-165">Si aucun port n’est définie :</span><span class="sxs-lookup"><span data-stu-id="796a8-165">If no port is set:</span></span>

* <span data-ttu-id="796a8-166">Les requêtes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="796a8-166">Requests aren't redirected.</span></span>
* <span data-ttu-id="796a8-167">L’intergiciel (middleware) consigne l’avertissement « Échoué pour déterminer le port https pour la redirection ».</span><span class="sxs-lookup"><span data-stu-id="796a8-167">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="796a8-168">Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser l’intergiciel de réécriture d’URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="796a8-168">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="796a8-169">`AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="796a8-169">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="796a8-170">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="796a8-170">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="796a8-171">Lors de la redirection vers HTTPS sans la nécessité pour les règles de redirection supplémentaire, nous vous recommandons d’utiliser HTTPS Redirection Middleware (`UseHttpsRedirection`) décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="796a8-171">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="796a8-172">Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger s-HTTP.</span><span class="sxs-lookup"><span data-stu-id="796a8-172">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="796a8-173">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peuvent être appliquées globalement.</span><span class="sxs-lookup"><span data-stu-id="796a8-173">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="796a8-174">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="796a8-174">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="796a8-175">Le code précédent en surbrillance nécessite d’utilisent toutes les demandes `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="796a8-175">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="796a8-176">Le code en surbrillance suivant redirige toutes les requêtes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="796a8-176">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="796a8-177">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="796a8-177">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="796a8-178">Le middleware permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="796a8-178">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="796a8-179">Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité,</span><span class="sxs-lookup"><span data-stu-id="796a8-179">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="796a8-180">car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`.</span><span class="sxs-lookup"><span data-stu-id="796a8-180">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="796a8-181">Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="796a8-181">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="796a8-182">Protocole de sécurité Strict Transport HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="796a8-182">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="796a8-183">Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de sécurité à accepter qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse.</span><span class="sxs-lookup"><span data-stu-id="796a8-183">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="796a8-184">Quand un [navigateur qui prend en charge HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) reçoit cet en-tête :</span><span class="sxs-lookup"><span data-stu-id="796a8-184">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="796a8-185">Le navigateur stocke la configuration pour le domaine qui empêche l’envoi de toutes les communications via HTTP.</span><span class="sxs-lookup"><span data-stu-id="796a8-185">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="796a8-186">Le navigateur force toutes les communications via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="796a8-186">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="796a8-187">Le navigateur empêche l’utilisateur de l’utilisation de certificats non approuvés ou non valides.</span><span class="sxs-lookup"><span data-stu-id="796a8-187">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="796a8-188">Le navigateur désactive les invites qui permettent à un utilisateur de confiance à ce certificat.</span><span class="sxs-lookup"><span data-stu-id="796a8-188">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="796a8-189">Étant donné que HSTS est appliquée par le client, il présente certaines limitations :</span><span class="sxs-lookup"><span data-stu-id="796a8-189">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="796a8-190">Le client doit prendre en charge HSTS.</span><span class="sxs-lookup"><span data-stu-id="796a8-190">The client must support HSTS.</span></span>
* <span data-ttu-id="796a8-191">HSTS nécessite au moins une demande HTTPS réussie pour établir la stratégie HSTS.</span><span class="sxs-lookup"><span data-stu-id="796a8-191">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="796a8-192">L’application doit vérifier chaque requête HTTP et rediriger ou rejeter la demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="796a8-192">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="796a8-193">ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="796a8-193">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="796a8-194">Le code suivant appelle `UseHsts` lorsque l’application n’est pas dans [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="796a8-194">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="796a8-195">`UseHsts` n’est pas recommandée dans le développement, car les paramètres HSTS sont hautement mis en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="796a8-195">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="796a8-196">Par défaut, `UseHsts` exclut l’adresse de bouclage local.</span><span class="sxs-lookup"><span data-stu-id="796a8-196">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="796a8-197">Pour les environnements de production mise en œuvre HTTPS pour la première fois, définissez la valeur HSTS initiale sur une valeur faible.</span><span class="sxs-lookup"><span data-stu-id="796a8-197">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="796a8-198">Définissez la valeur des heures non plus d’une seule journée au cas où vous deviez restaurer l’infrastructure HTTPS vers HTTP.</span><span class="sxs-lookup"><span data-stu-id="796a8-198">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="796a8-199">Une fois que vous êtes ainsi certain de la durabilité de la configuration de HTTPS, augmentez la valeur de max-age HSTS ; une valeur couramment utilisée est un an.</span><span class="sxs-lookup"><span data-stu-id="796a8-199">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="796a8-200">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="796a8-200">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="796a8-201">Définit le paramètre de préchargement de l’en-tête Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="796a8-201">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="796a8-202">Préchargement ne fait pas partie de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur la nouvelle installation.</span><span class="sxs-lookup"><span data-stu-id="796a8-202">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="796a8-203">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="796a8-203">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="796a8-204">Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS à héberger des sous-domaines.</span><span class="sxs-lookup"><span data-stu-id="796a8-204">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="796a8-205">Définit explicitement le paramètre d’âge maximal de l’en-tête Strict-Transport-Security à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="796a8-205">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="796a8-206">Si ce n’est pas définie, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="796a8-206">If not set, defaults to 30 days.</span></span> <span data-ttu-id="796a8-207">Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="796a8-207">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="796a8-208">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="796a8-208">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="796a8-209">`UseHsts` exclut les hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="796a8-209">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="796a8-210">`localhost` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="796a8-210">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="796a8-211">`127.0.0.1` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="796a8-211">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="796a8-212">`[::1]` : L’adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="796a8-212">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="796a8-213">L’exemple précédent montre comment ajouter des hôtes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="796a8-213">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="796a8-214">Annulations de HTTPS/HSTS sur la création du projet</span><span class="sxs-lookup"><span data-stu-id="796a8-214">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="796a8-215">Dans certains scénarios de service back-end où la sécurité de la connexion est gérée en périphérie du réseau destinées au public, configuration de sécurité de la connexion au niveau de chaque nœud n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="796a8-215">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="796a8-216">Généré à partir des modèles dans Visual Studio ou à partir des applications Web le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande Activer [la redirection HTTPS](#require) et [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="796a8-216">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="796a8-217">Pour les déploiements ne nécessitant pas de ces scénarios, vous pouvez adhérer à de HTTPS/HSTS lorsque l’application est créée à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="796a8-217">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="796a8-218">Pour adhérer à de HTTPS/HSTS :</span><span class="sxs-lookup"><span data-stu-id="796a8-218">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="796a8-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="796a8-219">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="796a8-220">Désactivez la case à cocher **configurer pour le protocole HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="796a8-220">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramme des entités](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="796a8-222">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="796a8-222">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="796a8-223">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="796a8-223">Use the `--no-https` option.</span></span> <span data-ttu-id="796a8-224">Exemple :</span><span class="sxs-lookup"><span data-stu-id="796a8-224">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="796a8-225">Comment configurer un certificat de développeur pour Docker</span><span class="sxs-lookup"><span data-stu-id="796a8-225">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="796a8-226">Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="796a8-226">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="796a8-227">Informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="796a8-227">Additional information</span></span>

* [<span data-ttu-id="796a8-228">Prise en charge des navigateurs OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="796a8-228">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
