---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans application web ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 48a25b7ba7affe84cfa6fe16096409239c510221
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652186"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="91590-103">Appliquer HTTPS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91590-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="91590-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="91590-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="91590-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="91590-105">This document shows how to:</span></span>

* <span data-ttu-id="91590-106">Exiger HTTPS pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="91590-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="91590-107">Rediriger toutes les demandes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="91590-108">Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="91590-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="91590-109">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="91590-110">Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="91590-111">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="91590-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="91590-112">Les API web doivent soit :</span><span class="sxs-lookup"><span data-stu-id="91590-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="91590-113">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="91590-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="91590-114">Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.</span><span class="sxs-lookup"><span data-stu-id="91590-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="91590-115">Exiger HTTPS</span><span class="sxs-lookup"><span data-stu-id="91590-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="91590-116">Nous vous recommandons de toutes les applications de web ASP.NET Core appellent l’intergiciel (middleware) de HTTPS Redirection ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les demandes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="91590-117">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="91590-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="91590-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="91590-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="91590-119">Le code suivant appelle [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="91590-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="91590-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="91590-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="91590-121">Le code en surbrillance précédent :</span><span class="sxs-lookup"><span data-stu-id="91590-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="91590-122">Jeux de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à `Status307TemporaryRedirect`, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="91590-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="91590-123">Les applications de production doivent appeler [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="91590-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="91590-124">Définit le port HTTPS 5001.</span><span class="sxs-lookup"><span data-stu-id="91590-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="91590-125">La valeur par défaut est 443.</span><span class="sxs-lookup"><span data-stu-id="91590-125">The default value is 443.</span></span>

<span data-ttu-id="91590-126">Les mécanismes suivants définir le port automatiquement :</span><span class="sxs-lookup"><span data-stu-id="91590-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="91590-127">L’intergiciel (middleware) peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :</span><span class="sxs-lookup"><span data-stu-id="91590-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="91590-128">Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="91590-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="91590-129">Uniquement **un port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="91590-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="91590-130">Visual Studio est utilisé :</span><span class="sxs-lookup"><span data-stu-id="91590-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="91590-131">IIS Express a activés HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="91590-132">*launchSettings.json* définit le `sslPort` pour IIS Express.</span><span class="sxs-lookup"><span data-stu-id="91590-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="91590-133">Quand une application est exécutée derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="91590-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="91590-134">Le port doit être configuré manuellement.</span><span class="sxs-lookup"><span data-stu-id="91590-134">The port must be manually configured.</span></span> <span data-ttu-id="91590-135">Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="91590-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="91590-136">Le port peut être configuré en définissant le :</span><span class="sxs-lookup"><span data-stu-id="91590-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="91590-137">La variable d’environnement `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="91590-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="91590-138">`http_port` clé de configuration d’hôte (par exemple, via *hostsettings.json* ou un argument de ligne de commande).</span><span class="sxs-lookup"><span data-stu-id="91590-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="91590-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="91590-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="91590-140">Consultez l’exemple précédent qui montre comment définir le port à 5001.</span><span class="sxs-lookup"><span data-stu-id="91590-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="91590-141">Le port peut être configuré indirectement par la définition de l’URL avec le `ASPNETCORE_URLS` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="91590-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="91590-142">La variable d’environnement configure le serveur, puis l’intergiciel (middleware) détecte indirectement le port HTTPS via `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="91590-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="91590-143">Si aucun port n’est définie :</span><span class="sxs-lookup"><span data-stu-id="91590-143">If no port is set:</span></span>

* <span data-ttu-id="91590-144">Les demandes ne sont pas redirigées.</span><span class="sxs-lookup"><span data-stu-id="91590-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="91590-145">L’intergiciel (middleware) consigne un avertissement.</span><span class="sxs-lookup"><span data-stu-id="91590-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="91590-146">Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser le Middleware de réécriture d’URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="91590-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="91590-147">`AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="91590-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="91590-148">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="91590-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="91590-149">Lors de la redirection vers HTTPS sans nécessité de règles de redirection supplémentaires, nous vous recommandons d’utiliser intergiciel (middleware) de HTTPS Redirection (`UseHttpsRedirection`) décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="91590-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="91590-150">Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="91590-151">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peut être appliqué globalement.</span><span class="sxs-lookup"><span data-stu-id="91590-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="91590-152">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="91590-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="91590-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="91590-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="91590-154">Le code en surbrillance précédent requiert que toutes les demandes utilisent `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="91590-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="91590-155">Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="91590-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="91590-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="91590-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="91590-157">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="91590-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="91590-158">L’intergiciel (middleware) permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="91590-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="91590-159">Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité,</span><span class="sxs-lookup"><span data-stu-id="91590-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="91590-160">car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`.</span><span class="sxs-lookup"><span data-stu-id="91590-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="91590-161">Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="91590-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="91590-162">Protocole de sécurité stricte de Transport HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="91590-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="91590-163">Par [OWASP avoir](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport sécurité (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de l’inclusion de sécurité qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse spécial.</span><span class="sxs-lookup"><span data-stu-id="91590-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="91590-164">Une fois qu’un navigateur pris en charge reçoit cet en-tête ce navigateur empêche les communications d’être envoyés via HTTP dans le domaine spécifié et l’envoie à la place toutes les communications via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="91590-165">Elle évite également les invites sur les navigateurs de clics HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="91590-166">ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="91590-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="91590-167">Le code suivant appelle `UseHsts` lorsque l’application n’est pas [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="91590-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="91590-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="91590-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="91590-169">`UseHsts` n’est pas recommandé dans le développement, car l’en-tête HSTS est hautement cachable par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="91590-169">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="91590-170">Par défaut, UseHsts exclut l’adresse de bouclage locale.</span><span class="sxs-lookup"><span data-stu-id="91590-170">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="91590-171">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="91590-171">The following code:</span></span>

<span data-ttu-id="91590-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="91590-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="91590-173">Définit le paramètre de préchargement de l’en-tête de sécurité de Transport Strict.</span><span class="sxs-lookup"><span data-stu-id="91590-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="91590-174">Le préchargement n’est pas dans le cadre de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur l’installation.</span><span class="sxs-lookup"><span data-stu-id="91590-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="91590-175">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="91590-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="91590-176">Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS de sous-domaines d’hôte.</span><span class="sxs-lookup"><span data-stu-id="91590-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="91590-177">Définit explicitement le paramètre de max-age de l’en-tête de sécurité de Transport Strict à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="91590-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="91590-178">Si la valeur n’est pas définie, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="91590-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="91590-179">Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="91590-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="91590-180">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="91590-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="91590-181">`UseHsts` exclut les ordinateurs hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="91590-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="91590-182">`localhost` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="91590-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="91590-183">`127.0.0.1` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="91590-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="91590-184">`[::1]` : L’adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="91590-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="91590-185">L’exemple précédent montre comment ajouter des hôtes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="91590-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="91590-186">Annulations de HTTPS sur la création du projet</span><span class="sxs-lookup"><span data-stu-id="91590-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="91590-187">Les modèles d’application ASP.NET Core web 2.1 ou version ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) [la redirection HTTPS](#require) et [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="91590-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="91590-188">Pour les déploiements qui n’ont pas besoin de HTTPS, vous pouvez annuler le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91590-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="91590-189">Par exemple, certains services principaux où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="91590-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="91590-190">Pour se désengager de HTTPS :</span><span class="sxs-lookup"><span data-stu-id="91590-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="91590-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91590-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="91590-192">Désactivez la case à cocher **configurer pour le protocole HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="91590-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramme des entités](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="91590-194">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="91590-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="91590-195">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="91590-195">Use the `--no-https` option.</span></span> <span data-ttu-id="91590-196">Exemple :</span><span class="sxs-lookup"><span data-stu-id="91590-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="91590-198">La configuration d’un certificat de développeur pour Docker</span><span class="sxs-lookup"><span data-stu-id="91590-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="91590-199">Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="91590-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
