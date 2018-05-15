---
title: Appliquer HTTPS dans une base de ASP.NET
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans un cœur d’ASP.NET application web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: b324dbcd6d28c1a8505f96da333874728e2e6a18
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="0043a-103">Appliquer HTTPS dans une base de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0043a-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="0043a-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0043a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0043a-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="0043a-105">This document shows how to:</span></span>

- <span data-ttu-id="0043a-106">Exiger HTTPS pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="0043a-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="0043a-107">Rediriger toutes les demandes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0043a-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="0043a-108">Faire **pas** utiliser `RequireHttpsAttribute` sur les API Web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="0043a-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="0043a-109">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0043a-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="0043a-110">Les clients d’API ne peuvent pas comprendre ou obéissent aux règles de redirections HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0043a-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="0043a-111">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="0043a-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="0043a-112">API Web doit soit :</span><span class="sxs-lookup"><span data-stu-id="0043a-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="0043a-113">Pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="0043a-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="0043a-114">Fermer la connexion avec le code d’état 400 (demande incorrecte) et pas desservir la demande.</span><span class="sxs-lookup"><span data-stu-id="0043a-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="0043a-115">Exiger HTTPS</span><span class="sxs-lookup"><span data-stu-id="0043a-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="0043a-116">Nous vous recommandons de tous les principaux ASP.NET web applications appel `UseHttpsRedirection` pour rediriger toutes les demandes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0043a-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="0043a-117">Si `UseHsts` est appelée dans l’application, il doit être appelé avant `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="0043a-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="0043a-118">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="0043a-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="0043a-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="0043a-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="0043a-120">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="0043a-120">The following code:</span></span>

<span data-ttu-id="0043a-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="0043a-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="0043a-122">Jeux de `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="0043a-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="0043a-123">Définit le port HTTPS 5001.</span><span class="sxs-lookup"><span data-stu-id="0043a-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="0043a-124">Le [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) est utilisée pour exiger HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0043a-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="0043a-125">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peut être appliqué globalement.</span><span class="sxs-lookup"><span data-stu-id="0043a-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="0043a-126">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="0043a-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="0043a-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="0043a-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="0043a-128">Le code en surbrillance précédent requiert que toutes les demandes utilisent `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="0043a-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="0043a-129">Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="0043a-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="0043a-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="0043a-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="0043a-131">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="0043a-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="0043a-132">Exiger le protocol HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité.</span><span class="sxs-lookup"><span data-stu-id="0043a-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="0043a-133">car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutées à votre application il faudra penser à appliquer le `[RequireHttps]` attribut.</span><span class="sxs-lookup"><span data-stu-id="0043a-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="0043a-134">Vous ne pouvez pas garantir la `[RequireHttps]` attribut est appliqué lors de l’ajout de nouveaux contrôleurs et les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="0043a-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="0043a-135">Protocole de sécurité stricte de Transport HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="0043a-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="0043a-136">Par [OWASP avoir](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport sécurité (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de l’inclusion de sécurité qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse spécial.</span><span class="sxs-lookup"><span data-stu-id="0043a-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="0043a-137">Une fois qu’un navigateur pris en charge reçoit cet en-tête ce navigateur empêche les communications d’être envoyés via HTTP dans le domaine spécifié et l’envoie à la place toutes les communications via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0043a-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="0043a-138">Elle évite également les invites sur les navigateurs de clics HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0043a-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="0043a-139">ASP.NET Core preview1 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="0043a-139">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="0043a-140">Le code suivant appelle `UseHsts` lorsque l’application n’est pas [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="0043a-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="0043a-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="0043a-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="0043a-142">`UseHsts` n’est pas recommandé dans le développement, car l’en-tête HSTS est hautement cachable par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="0043a-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="0043a-143">Par défaut, UseHsts exclut l’adresse de bouclage locale.</span><span class="sxs-lookup"><span data-stu-id="0043a-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="0043a-144">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="0043a-144">The following code:</span></span>

<span data-ttu-id="0043a-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="0043a-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="0043a-146">Définit le paramètre de préchargement de l’en-tête de sécurité de Transport Strict.</span><span class="sxs-lookup"><span data-stu-id="0043a-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="0043a-147">Le préchargement n’est pas dans le cadre de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur l’installation.</span><span class="sxs-lookup"><span data-stu-id="0043a-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="0043a-148">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="0043a-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="0043a-149">Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS de sous-domaines d’hôte.</span><span class="sxs-lookup"><span data-stu-id="0043a-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="0043a-150">Définit explicitement le paramètre de max-age de l’en-tête de sécurité de Transport Strict à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="0043a-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="0043a-151">Si la valeur n’est pas définie, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="0043a-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="0043a-152">Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="0043a-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="0043a-153">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="0043a-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="0043a-154">`UseHsts` exclut les ordinateurs hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="0043a-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="0043a-155">`localhost` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="0043a-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0043a-156">`127.0.0.1` : L’adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="0043a-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0043a-157">`[::1]` : L’adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="0043a-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="0043a-158">L’exemple précédent montre comment ajouter des hôtes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="0043a-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="0043a-159">Annulations de HTTPS sur la création du projet</span><span class="sxs-lookup"><span data-stu-id="0043a-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="0043a-160">Les ASP.NET Core 2.1 et les modèles d’application web ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) permettent [la redirection HTTPS](#require) et [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="0043a-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="0043a-161">Pour les déploiements qui n’ont pas besoin de HTTPS, vous pouvez annuler le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0043a-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="0043a-162">Par exemple, certains services principaux où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="0043a-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="0043a-163">Pour s’en désengager de HTTPS :</span><span class="sxs-lookup"><span data-stu-id="0043a-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0043a-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0043a-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0043a-165">Désactivez le **configurer pour le protocole HTTPS** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="0043a-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramme des entités](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0043a-167">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="0043a-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0043a-168">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="0043a-168">Use the `--no-https` option.</span></span> <span data-ttu-id="0043a-169">Exemple :</span><span class="sxs-lookup"><span data-stu-id="0043a-169">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="0043a-170">La configuration d’un certificat de développeur pour Docker</span><span class="sxs-lookup"><span data-stu-id="0043a-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="0043a-171">Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="0043a-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
