---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans application web ASP.NET Core.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 06ff89d30fb3586c69274cc7cb3e6c75065b098a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011324"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="a631b-103">Appliquer HTTPS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a631b-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="a631b-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a631b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a631b-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="a631b-105">This document shows how to:</span></span>

* <span data-ttu-id="a631b-106">Exiger s-HTTP pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="a631b-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="a631b-107">Rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a631b-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="a631b-108">Aucune API ne peut empêcher un client d’envoyer des données sensibles à la première demande.</span><span class="sxs-lookup"><span data-stu-id="a631b-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="a631b-109">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="a631b-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="a631b-110">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a631b-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="a631b-111">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a631b-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="a631b-112">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="a631b-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="a631b-113">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="a631b-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="a631b-114">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="a631b-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="a631b-115">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="a631b-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="a631b-116">Exiger s-http</span><span class="sxs-lookup"><span data-stu-id="a631b-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a631b-117">Nous recommandons d’ASP.NET Core de production tous les appels d’applications web :</span><span class="sxs-lookup"><span data-stu-id="a631b-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="a631b-118">Le Middleware de Redirection HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a631b-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="a631b-119">[UseHsts](#hsts), protocole de sécurité Strict Transport HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="a631b-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="a631b-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="a631b-120">UseHttpsRedirection</span></span>

<span data-ttu-id="a631b-121">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="a631b-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="a631b-122">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="a631b-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="a631b-123">Utilise la valeur par défaut [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="a631b-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="a631b-124">Utilise la valeur par défaut [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), sauf substitution par le `ASPNETCORE_HTTPS_PORT` variable d’environnement ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="a631b-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="a631b-125">Un port doit être disponible pour l’intergiciel (middleware) pour rediriger vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a631b-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="a631b-126">Si aucun port n’est disponible, la redirection vers HTTPS ne se produit pas.</span><span class="sxs-lookup"><span data-stu-id="a631b-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="a631b-127">Le port HTTPS peut être spécifié par un du paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="a631b-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="a631b-128">Le `ASPNETCORE_HTTPS_PORT` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="a631b-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="a631b-129">Dans le développement, une url HTTPS dans *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a631b-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="a631b-130">Une url HTTPS configurées directement sur Kestrel ou HttpSys.</span><span class="sxs-lookup"><span data-stu-id="a631b-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="a631b-131">Les appels de code en surbrillance suivantes [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="a631b-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="a631b-132">Appel `AddHttpsRedirection` est uniquement nécessaire de modifier les valeurs de ` HttpsPort` ou ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="a631b-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="a631b-133">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="a631b-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="a631b-134">Jeux [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à `Status307TemporaryRedirect`, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="a631b-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="a631b-135">Définit le port HTTPS par 5001.</span><span class="sxs-lookup"><span data-stu-id="a631b-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="a631b-136">La valeur par défaut est 443.</span><span class="sxs-lookup"><span data-stu-id="a631b-136">The default value is 443.</span></span>

<span data-ttu-id="a631b-137">Les mécanismes suivants définir automatiquement le port :</span><span class="sxs-lookup"><span data-stu-id="a631b-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="a631b-138">Le middleware peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :</span><span class="sxs-lookup"><span data-stu-id="a631b-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="a631b-139">Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur du Code Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="a631b-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="a631b-140">Uniquement **un seul port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="a631b-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="a631b-141">Visual Studio est utilisé :</span><span class="sxs-lookup"><span data-stu-id="a631b-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="a631b-142">IIS Express a HTTPS activé.</span><span class="sxs-lookup"><span data-stu-id="a631b-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="a631b-143">*launchSettings.json* définit le `sslPort` pour IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a631b-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="a631b-144">Quand une application s’exécute derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="a631b-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="a631b-145">Le port doit être configuré manuellement.</span><span class="sxs-lookup"><span data-stu-id="a631b-145">The port must be manually configured.</span></span> <span data-ttu-id="a631b-146">Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="a631b-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="a631b-147">Le port peut être configuré en définissant le [paramètre de configuration d’hôte Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="a631b-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="a631b-148">**Clé**: https_port</span><span class="sxs-lookup"><span data-stu-id="a631b-148">**Key**: https_port</span></span>  
<span data-ttu-id="a631b-149">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="a631b-149">**Type**: *string*</span></span>  
<span data-ttu-id="a631b-150">**Par défaut**: une valeur par défaut n’est pas définie.</span><span class="sxs-lookup"><span data-stu-id="a631b-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="a631b-151">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a631b-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a631b-152">**Variable d’environnement**: `<PREFIX_>HTTPS_PORT` (le préfixe est `ASPNETCORE_` lors de l’utilisation de l’hôte Web.)</span><span class="sxs-lookup"><span data-stu-id="a631b-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="a631b-153">Le port peut être configuré indirectement en définissant l’URL avec le `ASPNETCORE_URLS` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="a631b-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="a631b-154">La variable d’environnement configure le serveur, et détecte ensuite l’intergiciel (middleware) indirectement les le port HTTPS par le biais de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="a631b-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="a631b-155">Si aucun port n’est définie :</span><span class="sxs-lookup"><span data-stu-id="a631b-155">If no port is set:</span></span>

* <span data-ttu-id="a631b-156">Les requêtes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="a631b-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="a631b-157">L’intergiciel (middleware) consigne l’avertissement « Échoué pour déterminer le port https pour la redirection ».</span><span class="sxs-lookup"><span data-stu-id="a631b-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="a631b-158">Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser l’intergiciel de réécriture d’URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="a631b-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="a631b-159">`AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="a631b-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="a631b-160">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a631b-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="a631b-161">Lors de la redirection vers HTTPS sans la nécessité pour les règles de redirection supplémentaire, nous vous recommandons d’utiliser HTTPS Redirection Middleware (`UseHttpsRedirection`) décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="a631b-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="a631b-162">Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger s-HTTP.</span><span class="sxs-lookup"><span data-stu-id="a631b-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="a631b-163">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peuvent être appliquées globalement.</span><span class="sxs-lookup"><span data-stu-id="a631b-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="a631b-164">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="a631b-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="a631b-165">Le code précédent en surbrillance nécessite d’utilisent toutes les demandes `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="a631b-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="a631b-166">Le code en surbrillance suivant redirige toutes les requêtes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="a631b-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="a631b-167">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a631b-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="a631b-168">Le middleware permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="a631b-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="a631b-169">Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité,</span><span class="sxs-lookup"><span data-stu-id="a631b-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a631b-170">car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`.</span><span class="sxs-lookup"><span data-stu-id="a631b-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a631b-171">Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="a631b-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="a631b-172">Protocole de sécurité Strict Transport HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a631b-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="a631b-173">Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de sécurité à accepter qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse.</span><span class="sxs-lookup"><span data-stu-id="a631b-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="a631b-174">Quand un [navigateur qui prend en charge HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) reçoit cet en-tête :</span><span class="sxs-lookup"><span data-stu-id="a631b-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="a631b-175">Le navigateur stocke la configuration pour le domaine qui empêche l’envoi de toutes les communications via HTTP.</span><span class="sxs-lookup"><span data-stu-id="a631b-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="a631b-176">Le navigateur force toutes les communications via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a631b-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="a631b-177">Le navigateur empêche l’utilisateur de l’utilisation de certificats non approuvés ou non valides.</span><span class="sxs-lookup"><span data-stu-id="a631b-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="a631b-178">Le navigateur désactive les invites qui permettent à un utilisateur de confiance à ce certificat.</span><span class="sxs-lookup"><span data-stu-id="a631b-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="a631b-179">Étant donné que HSTS est appliquée par le client, il présente certaines limitations :</span><span class="sxs-lookup"><span data-stu-id="a631b-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="a631b-180">Le client doit prendre en charge HSTS.</span><span class="sxs-lookup"><span data-stu-id="a631b-180">The client must support HSTS.</span></span>
* <span data-ttu-id="a631b-181">HSTS nécessite au moins une demande HTTPS réussie pour établir la stratégie HSTS.</span><span class="sxs-lookup"><span data-stu-id="a631b-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="a631b-182">L’application doit vérifier chaque requête HTTP et rediriger ou rejeter la demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="a631b-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="a631b-183">ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="a631b-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="a631b-184">Le code suivant appelle `UseHsts` lorsque l’application n’est pas dans [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="a631b-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="a631b-185">`UseHsts` n’est pas recommandée dans le développement, car les paramètres HSTS sont hautement mis en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="a631b-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="a631b-186">Par défaut, `UseHsts` exclut l’adresse de bouclage local.</span><span class="sxs-lookup"><span data-stu-id="a631b-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="a631b-187">Pour les environnements de production mise en œuvre HTTPS pour la première fois, définissez la valeur HSTS initiale sur une valeur faible.</span><span class="sxs-lookup"><span data-stu-id="a631b-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="a631b-188">Définissez la valeur des heures non plus d’une seule journée au cas où vous deviez restaurer l’infrastructure HTTPS vers HTTP.</span><span class="sxs-lookup"><span data-stu-id="a631b-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="a631b-189">Une fois que vous êtes ainsi certain de la durabilité de la configuration de HTTPS, augmentez la valeur de max-age HSTS ; une valeur couramment utilisée est un an.</span><span class="sxs-lookup"><span data-stu-id="a631b-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="a631b-190">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="a631b-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="a631b-191">Définit le paramètre de préchargement de l’en-tête Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="a631b-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="a631b-192">Préchargement ne fait pas partie de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur la nouvelle installation.</span><span class="sxs-lookup"><span data-stu-id="a631b-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="a631b-193">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="a631b-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="a631b-194">Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS à héberger des sous-domaines.</span><span class="sxs-lookup"><span data-stu-id="a631b-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="a631b-195">Définit explicitement le paramètre d’âge maximal de l’en-tête Strict-Transport-Security à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="a631b-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="a631b-196">Si ce n’est pas définie, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="a631b-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="a631b-197">Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="a631b-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="a631b-198">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="a631b-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="a631b-199">`UseHsts` exclut les hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="a631b-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="a631b-200">`localhost` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="a631b-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a631b-201">`127.0.0.1` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="a631b-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a631b-202">`[::1]` : L’adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="a631b-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="a631b-203">L’exemple précédent montre comment ajouter des hôtes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="a631b-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="a631b-204">Annulations de HTTPS sur la création du projet</span><span class="sxs-lookup"><span data-stu-id="a631b-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="a631b-205">Activent les modèles d’application ASP.NET Core web 2.1 ou version ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) [la redirection HTTPS](#require) et [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="a631b-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="a631b-206">Pour les déploiements qui ne nécessitent pas HTTPS, vous pouvez ne pas adhérer du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a631b-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="a631b-207">Par exemple, certains services back-end où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="a631b-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="a631b-208">Pour se désengager de HTTPS :</span><span class="sxs-lookup"><span data-stu-id="a631b-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a631b-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a631b-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="a631b-210">Désactivez la case à cocher **configurer pour le protocole HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="a631b-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramme des entités](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a631b-212">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="a631b-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="a631b-213">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="a631b-213">Use the `--no-https` option.</span></span> <span data-ttu-id="a631b-214">Exemple :</span><span class="sxs-lookup"><span data-stu-id="a631b-214">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="a631b-215">Comment configurer un certificat de développeur pour Docker</span><span class="sxs-lookup"><span data-stu-id="a631b-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="a631b-216">Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="a631b-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="a631b-217">Informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a631b-217">Additional information</span></span>

* [<span data-ttu-id="a631b-218">Prise en charge des navigateurs OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="a631b-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
