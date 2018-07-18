---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans application web ASP.NET Core.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 331c17de33b5c13221385ffb4282bc16bde32289
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095715"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="8aab2-103">Appliquer HTTPS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8aab2-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="8aab2-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8aab2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8aab2-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="8aab2-105">This document shows how to:</span></span>

* <span data-ttu-id="8aab2-106">Exiger s-HTTP pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="8aab2-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="8aab2-107">Rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8aab2-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="8aab2-108">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="8aab2-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="8aab2-109">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8aab2-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="8aab2-110">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8aab2-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="8aab2-111">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="8aab2-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="8aab2-112">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="8aab2-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="8aab2-113">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="8aab2-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="8aab2-114">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="8aab2-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="8aab2-115">Exiger s-http</span><span class="sxs-lookup"><span data-stu-id="8aab2-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8aab2-116">Nous vous recommandons de toutes les applications web de ASP.NET Core appellent intergiciel de la Redirection de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8aab2-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="8aab2-117">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="8aab2-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="8aab2-118">Le code suivant appelle [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="8aab2-118">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="8aab2-119">Le code précédent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="8aab2-119">The preceding highlighted code:</span></span>

* <span data-ttu-id="8aab2-120">Jeux [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à `Status307TemporaryRedirect`, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="8aab2-120">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="8aab2-121">Les applications de production doivent appeler [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="8aab2-121">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="8aab2-122">Définit le port HTTPS par 5001.</span><span class="sxs-lookup"><span data-stu-id="8aab2-122">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="8aab2-123">La valeur par défaut est 443.</span><span class="sxs-lookup"><span data-stu-id="8aab2-123">The default value is 443.</span></span>

<span data-ttu-id="8aab2-124">Les mécanismes suivants définir automatiquement le port :</span><span class="sxs-lookup"><span data-stu-id="8aab2-124">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="8aab2-125">Le middleware peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :</span><span class="sxs-lookup"><span data-stu-id="8aab2-125">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="8aab2-126">Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur du Code Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="8aab2-126">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="8aab2-127">Uniquement **un seul port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="8aab2-127">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="8aab2-128">Visual Studio est utilisé :</span><span class="sxs-lookup"><span data-stu-id="8aab2-128">Visual Studio is used:</span></span>
  - <span data-ttu-id="8aab2-129">IIS Express a HTTPS activé.</span><span class="sxs-lookup"><span data-stu-id="8aab2-129">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="8aab2-130">*launchSettings.json* définit le `sslPort` pour IIS Express.</span><span class="sxs-lookup"><span data-stu-id="8aab2-130">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="8aab2-131">Quand une application s’exécute derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="8aab2-131">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="8aab2-132">Le port doit être configuré manuellement.</span><span class="sxs-lookup"><span data-stu-id="8aab2-132">The port must be manually configured.</span></span> <span data-ttu-id="8aab2-133">Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="8aab2-133">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="8aab2-134">Le port peut être configuré en définissant le :</span><span class="sxs-lookup"><span data-stu-id="8aab2-134">The port can be configured by setting the:</span></span>

* <span data-ttu-id="8aab2-135">La variable d’environnement `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="8aab2-135">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="8aab2-136">`http_port` clé de configuration d’hôte (par exemple, via *hostsettings.json* ou un argument de ligne de commande).</span><span class="sxs-lookup"><span data-stu-id="8aab2-136">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="8aab2-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="8aab2-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="8aab2-138">Consultez l’exemple précédent qui montre comment définir le port par 5001.</span><span class="sxs-lookup"><span data-stu-id="8aab2-138">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="8aab2-139">Le port peut être configuré indirectement en définissant l’URL avec le `ASPNETCORE_URLS` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="8aab2-139">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="8aab2-140">La variable d’environnement configure le serveur, et détecte ensuite l’intergiciel (middleware) indirectement les le port HTTPS par le biais de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="8aab2-140">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="8aab2-141">Si aucun port n’est définie :</span><span class="sxs-lookup"><span data-stu-id="8aab2-141">If no port is set:</span></span>

* <span data-ttu-id="8aab2-142">Les requêtes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="8aab2-142">Requests aren't redirected.</span></span>
* <span data-ttu-id="8aab2-143">L’intergiciel (middleware) consigne un avertissement.</span><span class="sxs-lookup"><span data-stu-id="8aab2-143">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="8aab2-144">Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser l’intergiciel de réécriture d’URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="8aab2-144">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="8aab2-145">`AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="8aab2-145">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="8aab2-146">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="8aab2-146">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="8aab2-147">Lors de la redirection vers HTTPS sans la nécessité pour les règles de redirection supplémentaire, nous vous recommandons d’utiliser HTTPS Redirection Middleware (`UseHttpsRedirection`) décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="8aab2-147">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="8aab2-148">Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger s-HTTP.</span><span class="sxs-lookup"><span data-stu-id="8aab2-148">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="8aab2-149">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peuvent être appliquées globalement.</span><span class="sxs-lookup"><span data-stu-id="8aab2-149">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="8aab2-150">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="8aab2-150">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="8aab2-151">Le code précédent en surbrillance nécessite d’utilisent toutes les demandes `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="8aab2-151">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="8aab2-152">Le code en surbrillance suivant redirige toutes les requêtes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="8aab2-152">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="8aab2-153">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="8aab2-153">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="8aab2-154">Le middleware permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="8aab2-154">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="8aab2-155">Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité,</span><span class="sxs-lookup"><span data-stu-id="8aab2-155">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="8aab2-156">car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`.</span><span class="sxs-lookup"><span data-stu-id="8aab2-156">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="8aab2-157">Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="8aab2-157">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="8aab2-158">Protocole de sécurité Strict Transport HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="8aab2-158">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="8aab2-159">Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de sécurité à accepter qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse spécial.</span><span class="sxs-lookup"><span data-stu-id="8aab2-159">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="8aab2-160">Une fois qu’un navigateur pris en charge reçoit cet en-tête ce navigateur empêchera toutes les communications d’être envoyés sur HTTP pour le domaine spécifié et enverra à la place toutes les communications via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8aab2-160">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="8aab2-161">Il empêche également cliquez sur HTTPS invites sur les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="8aab2-161">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="8aab2-162">ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="8aab2-162">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="8aab2-163">Le code suivant appelle `UseHsts` lorsque l’application n’est pas dans [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="8aab2-163">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="8aab2-164">`UseHsts` n’est pas recommandée dans le développement, car l’en-tête HSTS est hautement mis en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="8aab2-164">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="8aab2-165">Par défaut, `UseHsts` exclut l’adresse de bouclage local.</span><span class="sxs-lookup"><span data-stu-id="8aab2-165">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="8aab2-166">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="8aab2-166">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="8aab2-167">Définit le paramètre de préchargement de l’en-tête Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="8aab2-167">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="8aab2-168">Le préchargement n’est pas dans le cadre de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur la nouvelle installation.</span><span class="sxs-lookup"><span data-stu-id="8aab2-168">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="8aab2-169">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="8aab2-169">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="8aab2-170">Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS à héberger des sous-domaines.</span><span class="sxs-lookup"><span data-stu-id="8aab2-170">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="8aab2-171">Définit explicitement le paramètre d’âge maximal de l’en-tête Strict-Transport-Security à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="8aab2-171">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="8aab2-172">Si ce n’est pas définie, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="8aab2-172">If not set, defaults to 30 days.</span></span> <span data-ttu-id="8aab2-173">Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="8aab2-173">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="8aab2-174">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="8aab2-174">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="8aab2-175">`UseHsts` exclut les hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="8aab2-175">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="8aab2-176">`localhost` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="8aab2-176">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="8aab2-177">`127.0.0.1` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="8aab2-177">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="8aab2-178">`[::1]` : L’adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="8aab2-178">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="8aab2-179">L’exemple précédent montre comment ajouter des hôtes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="8aab2-179">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="8aab2-180">Annulations de HTTPS sur la création du projet</span><span class="sxs-lookup"><span data-stu-id="8aab2-180">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="8aab2-181">Activent les modèles d’application ASP.NET Core web 2.1 ou version ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) [la redirection HTTPS](#require) et [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="8aab2-181">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="8aab2-182">Pour les déploiements qui ne nécessitent pas HTTPS, vous pouvez ne pas adhérer du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8aab2-182">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="8aab2-183">Par exemple, certains services back-end où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="8aab2-183">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="8aab2-184">Pour se désengager de HTTPS :</span><span class="sxs-lookup"><span data-stu-id="8aab2-184">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8aab2-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8aab2-185">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="8aab2-186">Désactivez la case à cocher **configurer pour le protocole HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="8aab2-186">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramme des entités](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8aab2-188">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="8aab2-188">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="8aab2-189">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="8aab2-189">Use the `--no-https` option.</span></span> <span data-ttu-id="8aab2-190">Exemple :</span><span class="sxs-lookup"><span data-stu-id="8aab2-190">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="8aab2-191">Comment configurer un certificat de développeur pour Docker</span><span class="sxs-lookup"><span data-stu-id="8aab2-191">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="8aab2-192">Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="8aab2-192">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
