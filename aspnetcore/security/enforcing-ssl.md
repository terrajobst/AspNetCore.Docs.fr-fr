---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans application web ASP.NET Core.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: d8bf11d7d2df8d8b197f001570a8fab1f3262814
ms.sourcegitcommit: 4e34ce61e1e7f1317102b16012ce0742abf2cca6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514802"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="8c33f-103">Appliquer HTTPS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c33f-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="8c33f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8c33f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8c33f-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="8c33f-105">This document shows how to:</span></span>

* <span data-ttu-id="8c33f-106">Exiger s-HTTP pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="8c33f-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="8c33f-107">Rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8c33f-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="8c33f-108">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="8c33f-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="8c33f-109">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8c33f-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="8c33f-110">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8c33f-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="8c33f-111">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c33f-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="8c33f-112">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="8c33f-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="8c33f-113">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c33f-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="8c33f-114">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="8c33f-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="8c33f-115">Exiger s-http</span><span class="sxs-lookup"><span data-stu-id="8c33f-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8c33f-116">Nous vous recommandons de toutes les applications web de ASP.NET Core appellent intergiciel de la Redirection de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8c33f-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="8c33f-117">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="8c33f-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="8c33f-118">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="8c33f-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="8c33f-119">Utilise la valeur par défaut [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="8c33f-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="8c33f-120">Les applications de production doivent appeler [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="8c33f-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="8c33f-121">Utilise la valeur par défaut [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="8c33f-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="8c33f-122">Le code suivant appelle [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="8c33f-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="8c33f-123">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="8c33f-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="8c33f-124">Jeux [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à `Status307TemporaryRedirect`, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="8c33f-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="8c33f-125">Les applications de production doivent appeler [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="8c33f-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="8c33f-126">Définit le port HTTPS par 5001.</span><span class="sxs-lookup"><span data-stu-id="8c33f-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="8c33f-127">La valeur par défaut est 443.</span><span class="sxs-lookup"><span data-stu-id="8c33f-127">The default value is 443.</span></span>

<span data-ttu-id="8c33f-128">Les mécanismes suivants définir automatiquement le port :</span><span class="sxs-lookup"><span data-stu-id="8c33f-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="8c33f-129">Le middleware peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :</span><span class="sxs-lookup"><span data-stu-id="8c33f-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="8c33f-130">Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur du Code Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="8c33f-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="8c33f-131">Uniquement **un seul port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="8c33f-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="8c33f-132">Visual Studio est utilisé :</span><span class="sxs-lookup"><span data-stu-id="8c33f-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="8c33f-133">IIS Express a HTTPS activé.</span><span class="sxs-lookup"><span data-stu-id="8c33f-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="8c33f-134">*launchSettings.json* définit le `sslPort` pour IIS Express.</span><span class="sxs-lookup"><span data-stu-id="8c33f-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="8c33f-135">Quand une application s’exécute derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="8c33f-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="8c33f-136">Le port doit être configuré manuellement.</span><span class="sxs-lookup"><span data-stu-id="8c33f-136">The port must be manually configured.</span></span> <span data-ttu-id="8c33f-137">Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="8c33f-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="8c33f-138">Le port peut être configuré en définissant le [paramètre de configuration d’hôte Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="8c33f-138">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="8c33f-139">**Clé**: https_port **Type**: *chaîne*
**par défaut**: une valeur par défaut n’est pas définie.</span><span class="sxs-lookup"><span data-stu-id="8c33f-139">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="8c33f-140">**À l’aide de**: `UseSetting` 
 **variable d’environnement**: `<PREFIX_>HTTPS_PORT` (le préfixe est `ASPNETCORE_` lors de l’utilisation de l’hôte Web.)</span><span class="sxs-lookup"><span data-stu-id="8c33f-140">**Set using**: `UseSetting`
**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="8c33f-141">Le port peut être configuré indirectement en définissant l’URL avec le `ASPNETCORE_URLS` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="8c33f-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="8c33f-142">La variable d’environnement configure le serveur, et détecte ensuite l’intergiciel (middleware) indirectement les le port HTTPS par le biais de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="8c33f-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="8c33f-143">Si aucun port n’est définie :</span><span class="sxs-lookup"><span data-stu-id="8c33f-143">If no port is set:</span></span>

* <span data-ttu-id="8c33f-144">Les requêtes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="8c33f-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="8c33f-145">L’intergiciel (middleware) consigne un avertissement.</span><span class="sxs-lookup"><span data-stu-id="8c33f-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="8c33f-146">Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser l’intergiciel de réécriture d’URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="8c33f-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="8c33f-147">`AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="8c33f-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="8c33f-148">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="8c33f-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="8c33f-149">Lors de la redirection vers HTTPS sans la nécessité pour les règles de redirection supplémentaire, nous vous recommandons d’utiliser HTTPS Redirection Middleware (`UseHttpsRedirection`) décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="8c33f-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="8c33f-150">Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger s-HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c33f-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="8c33f-151">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peuvent être appliquées globalement.</span><span class="sxs-lookup"><span data-stu-id="8c33f-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="8c33f-152">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="8c33f-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="8c33f-153">Le code précédent en surbrillance nécessite d’utilisent toutes les demandes `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="8c33f-153">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="8c33f-154">Le code en surbrillance suivant redirige toutes les requêtes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="8c33f-154">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="8c33f-155">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="8c33f-155">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="8c33f-156">Le middleware permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="8c33f-156">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="8c33f-157">Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité,</span><span class="sxs-lookup"><span data-stu-id="8c33f-157">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="8c33f-158">car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`.</span><span class="sxs-lookup"><span data-stu-id="8c33f-158">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="8c33f-159">Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="8c33f-159">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="8c33f-160">Protocole de sécurité Strict Transport HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="8c33f-160">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="8c33f-161">Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de sécurité à accepter qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse spécial.</span><span class="sxs-lookup"><span data-stu-id="8c33f-161">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a special response header.</span></span> <span data-ttu-id="8c33f-162">Lorsqu’un navigateur qui prend en charge HSTS reçoit cet en-tête, il stocke la configuration pour le domaine qui empêche l’envoi de toutes les communications via HTTP et au lieu de cela force toutes les communications via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8c33f-162">When a browser that supports HSTS receives this header, it stores configuration for the domain that prevents sending any communication over HTTP and instead forces all communication over HTTPS.</span></span> <span data-ttu-id="8c33f-163">Il empêche également l’utilisateur à l’aide de certificats non approuvés ou non valides, désactiver les invites de navigateur qui permettent un utilisateur de confiance à ce certificat.</span><span class="sxs-lookup"><span data-stu-id="8c33f-163">It also prevents the user from using untrusted or invalid certificates, disabling the browser prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="8c33f-164">ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="8c33f-164">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="8c33f-165">Le code suivant appelle `UseHsts` lorsque l’application n’est pas dans [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="8c33f-165">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="8c33f-166">`UseHsts` n’est pas recommandée dans le développement, car l’en-tête HSTS est hautement mis en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="8c33f-166">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="8c33f-167">Par défaut, `UseHsts` exclut l’adresse de bouclage local.</span><span class="sxs-lookup"><span data-stu-id="8c33f-167">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="8c33f-168">Pour les environnements de production mise en œuvre HTTPS pour la première fois, définissez la valeur HSTS initiale sur une valeur faible.</span><span class="sxs-lookup"><span data-stu-id="8c33f-168">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="8c33f-169">Définissez la valeur des heures non plus d’une seule journée au cas où vous deviez restaurer l’infrastructure HTTPS vers HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c33f-169">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="8c33f-170">Une fois que vous êtes ainsi certain de la durabilité de la configuration de HTTPS, augmentez la valeur de max-age HSTS ; une valeur couramment utilisée est un an.</span><span class="sxs-lookup"><span data-stu-id="8c33f-170">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="8c33f-171">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="8c33f-171">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="8c33f-172">Définit le paramètre de préchargement de l’en-tête Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="8c33f-172">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="8c33f-173">Le préchargement n’est pas dans le cadre de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur la nouvelle installation.</span><span class="sxs-lookup"><span data-stu-id="8c33f-173">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="8c33f-174">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="8c33f-174">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="8c33f-175">Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS à héberger des sous-domaines.</span><span class="sxs-lookup"><span data-stu-id="8c33f-175">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="8c33f-176">Définit explicitement le paramètre d’âge maximal de l’en-tête Strict-Transport-Security à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="8c33f-176">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="8c33f-177">Si ce n’est pas définie, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="8c33f-177">If not set, defaults to 30 days.</span></span> <span data-ttu-id="8c33f-178">Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="8c33f-178">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="8c33f-179">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="8c33f-179">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="8c33f-180">`UseHsts` exclut les hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="8c33f-180">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="8c33f-181">`localhost` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="8c33f-181">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="8c33f-182">`127.0.0.1` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="8c33f-182">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="8c33f-183">`[::1]` : L’adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="8c33f-183">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="8c33f-184">L’exemple précédent montre comment ajouter des hôtes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="8c33f-184">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="8c33f-185">Annulations de HTTPS sur la création du projet</span><span class="sxs-lookup"><span data-stu-id="8c33f-185">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="8c33f-186">Activent les modèles d’application ASP.NET Core web 2.1 ou version ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) [la redirection HTTPS](#require) et [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="8c33f-186">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="8c33f-187">Pour les déploiements qui ne nécessitent pas HTTPS, vous pouvez ne pas adhérer du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8c33f-187">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="8c33f-188">Par exemple, certains services back-end où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="8c33f-188">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="8c33f-189">Pour se désengager de HTTPS :</span><span class="sxs-lookup"><span data-stu-id="8c33f-189">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c33f-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c33f-190">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="8c33f-191">Désactivez la case à cocher **configurer pour le protocole HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="8c33f-191">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramme des entités](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c33f-193">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c33f-193">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="8c33f-194">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="8c33f-194">Use the `--no-https` option.</span></span> <span data-ttu-id="8c33f-195">Exemple :</span><span class="sxs-lookup"><span data-stu-id="8c33f-195">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="8c33f-196">Comment configurer un certificat de développeur pour Docker</span><span class="sxs-lookup"><span data-stu-id="8c33f-196">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="8c33f-197">Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="8c33f-197">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
