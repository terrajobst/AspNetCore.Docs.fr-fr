---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans application web ASP.NET Core.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: c3d92994c0331b1408e246953454910ca1f4dc43
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254829"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="e2c62-103">Appliquer HTTPS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2c62-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="e2c62-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e2c62-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e2c62-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="e2c62-105">This document shows how to:</span></span>

* <span data-ttu-id="e2c62-106">Exiger s-HTTP pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="e2c62-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="e2c62-107">Rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2c62-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="e2c62-108">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="e2c62-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="e2c62-109">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2c62-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="e2c62-110">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2c62-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="e2c62-111">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2c62-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="e2c62-112">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="e2c62-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="e2c62-113">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2c62-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="e2c62-114">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="e2c62-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="e2c62-115">Exiger s-http</span><span class="sxs-lookup"><span data-stu-id="e2c62-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e2c62-116">Nous vous recommandons de toutes les applications web de ASP.NET Core appellent intergiciel de la Redirection de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2c62-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="e2c62-117">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="e2c62-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="e2c62-118">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="e2c62-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="e2c62-119">Utilise la valeur par défaut [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="e2c62-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="e2c62-120">Les applications de production doivent appeler [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="e2c62-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="e2c62-121">Utilise la valeur par défaut [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="e2c62-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="e2c62-122">Le code suivant appelle [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="e2c62-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="e2c62-123">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="e2c62-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="e2c62-124">Jeux [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à `Status307TemporaryRedirect`, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="e2c62-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="e2c62-125">Les applications de production doivent appeler [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="e2c62-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="e2c62-126">Définit le port HTTPS par 5001.</span><span class="sxs-lookup"><span data-stu-id="e2c62-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="e2c62-127">La valeur par défaut est 443.</span><span class="sxs-lookup"><span data-stu-id="e2c62-127">The default value is 443.</span></span>

<span data-ttu-id="e2c62-128">Les mécanismes suivants définir automatiquement le port :</span><span class="sxs-lookup"><span data-stu-id="e2c62-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="e2c62-129">Le middleware peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :</span><span class="sxs-lookup"><span data-stu-id="e2c62-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="e2c62-130">Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur du Code Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="e2c62-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="e2c62-131">Uniquement **un seul port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c62-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="e2c62-132">Visual Studio est utilisé :</span><span class="sxs-lookup"><span data-stu-id="e2c62-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="e2c62-133">IIS Express a HTTPS activé.</span><span class="sxs-lookup"><span data-stu-id="e2c62-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="e2c62-134">*launchSettings.json* définit le `sslPort` pour IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e2c62-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c62-135">Quand une application s’exécute derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="e2c62-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="e2c62-136">Le port doit être configuré manuellement.</span><span class="sxs-lookup"><span data-stu-id="e2c62-136">The port must be manually configured.</span></span> <span data-ttu-id="e2c62-137">Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="e2c62-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="e2c62-138">Le port peut être configuré en définissant le :</span><span class="sxs-lookup"><span data-stu-id="e2c62-138">The port can be configured by setting the:</span></span>

* <span data-ttu-id="e2c62-139">La variable d’environnement `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="e2c62-139">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="e2c62-140">`http_port` clé de configuration d’hôte (par exemple, via *hostsettings.json* ou un argument de ligne de commande).</span><span class="sxs-lookup"><span data-stu-id="e2c62-140">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="e2c62-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="e2c62-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="e2c62-142">Consultez l’exemple précédent qui montre comment définir le port par 5001.</span><span class="sxs-lookup"><span data-stu-id="e2c62-142">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c62-143">Le port peut être configuré indirectement en définissant l’URL avec le `ASPNETCORE_URLS` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e2c62-143">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="e2c62-144">La variable d’environnement configure le serveur, et détecte ensuite l’intergiciel (middleware) indirectement les le port HTTPS par le biais de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="e2c62-144">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="e2c62-145">Si aucun port n’est définie :</span><span class="sxs-lookup"><span data-stu-id="e2c62-145">If no port is set:</span></span>

* <span data-ttu-id="e2c62-146">Les requêtes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="e2c62-146">Requests aren't redirected.</span></span>
* <span data-ttu-id="e2c62-147">L’intergiciel (middleware) consigne un avertissement.</span><span class="sxs-lookup"><span data-stu-id="e2c62-147">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c62-148">Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser l’intergiciel de réécriture d’URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="e2c62-148">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="e2c62-149">`AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="e2c62-149">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="e2c62-150">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e2c62-150">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="e2c62-151">Lors de la redirection vers HTTPS sans la nécessité pour les règles de redirection supplémentaire, nous vous recommandons d’utiliser HTTPS Redirection Middleware (`UseHttpsRedirection`) décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e2c62-151">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="e2c62-152">Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger s-HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2c62-152">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="e2c62-153">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peuvent être appliquées globalement.</span><span class="sxs-lookup"><span data-stu-id="e2c62-153">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="e2c62-154">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="e2c62-154">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="e2c62-155">Le code précédent en surbrillance nécessite d’utilisent toutes les demandes `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="e2c62-155">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="e2c62-156">Le code en surbrillance suivant redirige toutes les requêtes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="e2c62-156">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="e2c62-157">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e2c62-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="e2c62-158">Le middleware permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="e2c62-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="e2c62-159">Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité,</span><span class="sxs-lookup"><span data-stu-id="e2c62-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="e2c62-160">car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`.</span><span class="sxs-lookup"><span data-stu-id="e2c62-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="e2c62-161">Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="e2c62-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="e2c62-162">Protocole de sécurité Strict Transport HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="e2c62-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="e2c62-163">Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de sécurité à accepter qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse spécial.</span><span class="sxs-lookup"><span data-stu-id="e2c62-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="e2c62-164">Une fois qu’un navigateur pris en charge reçoit cet en-tête ce navigateur empêchera toutes les communications d’être envoyés sur HTTP pour le domaine spécifié et enverra à la place toutes les communications via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2c62-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="e2c62-165">Il empêche également cliquez sur HTTPS invites sur les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="e2c62-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="e2c62-166">ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="e2c62-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="e2c62-167">Le code suivant appelle `UseHsts` lorsque l’application n’est pas dans [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="e2c62-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="e2c62-168">`UseHsts` n’est pas recommandée dans le développement, car l’en-tête HSTS est hautement mis en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="e2c62-168">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="e2c62-169">Par défaut, `UseHsts` exclut l’adresse de bouclage local.</span><span class="sxs-lookup"><span data-stu-id="e2c62-169">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="e2c62-170">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2c62-170">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="e2c62-171">Définit le paramètre de préchargement de l’en-tête Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="e2c62-171">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="e2c62-172">Le préchargement n’est pas dans le cadre de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur la nouvelle installation.</span><span class="sxs-lookup"><span data-stu-id="e2c62-172">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="e2c62-173">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="e2c62-173">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="e2c62-174">Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS à héberger des sous-domaines.</span><span class="sxs-lookup"><span data-stu-id="e2c62-174">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="e2c62-175">Définit explicitement le paramètre d’âge maximal de l’en-tête Strict-Transport-Security à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="e2c62-175">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="e2c62-176">Si ce n’est pas définie, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="e2c62-176">If not set, defaults to 30 days.</span></span> <span data-ttu-id="e2c62-177">Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="e2c62-177">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="e2c62-178">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="e2c62-178">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="e2c62-179">`UseHsts` exclut les hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="e2c62-179">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="e2c62-180">`localhost` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="e2c62-180">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="e2c62-181">`127.0.0.1` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="e2c62-181">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="e2c62-182">`[::1]` : L’adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="e2c62-182">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="e2c62-183">L’exemple précédent montre comment ajouter des hôtes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="e2c62-183">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="e2c62-184">Annulations de HTTPS sur la création du projet</span><span class="sxs-lookup"><span data-stu-id="e2c62-184">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="e2c62-185">Activent les modèles d’application ASP.NET Core web 2.1 ou version ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) [la redirection HTTPS](#require) et [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="e2c62-185">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="e2c62-186">Pour les déploiements qui ne nécessitent pas HTTPS, vous pouvez ne pas adhérer du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2c62-186">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="e2c62-187">Par exemple, certains services back-end où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e2c62-187">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="e2c62-188">Pour se désengager de HTTPS :</span><span class="sxs-lookup"><span data-stu-id="e2c62-188">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2c62-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2c62-189">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e2c62-190">Désactivez la case à cocher **configurer pour le protocole HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="e2c62-190">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramme des entités](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2c62-192">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2c62-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="e2c62-193">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="e2c62-193">Use the `--no-https` option.</span></span> <span data-ttu-id="e2c62-194">Exemple :</span><span class="sxs-lookup"><span data-stu-id="e2c62-194">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="e2c62-195">Comment configurer un certificat de développeur pour Docker</span><span class="sxs-lookup"><span data-stu-id="e2c62-195">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="e2c62-196">Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="e2c62-196">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
