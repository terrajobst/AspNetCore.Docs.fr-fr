---
title: "État de session et d’application dans ASP.NET Core"
author: rick-anderson
description: "Approches de conservation d’application et l’état utilisateur (session) entre les demandes."
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e00960370fbe87ac0f81f8455526221fa992decd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="339d9-103">Introduction à l’état de session et d’application dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="339d9-103">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="339d9-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), et [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="339d9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="339d9-105">HTTP est un protocole sans état.</span><span class="sxs-lookup"><span data-stu-id="339d9-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="339d9-106">Un serveur web traite chaque demande HTTP comme une demande indépendante et ne conserve pas les valeurs d’utilisateur à partir de requêtes précédentes.</span><span class="sxs-lookup"><span data-stu-id="339d9-106">A web server treats each HTTP request as an independent request and doesn't retain user values from previous requests.</span></span> <span data-ttu-id="339d9-107">Cet article décrit les différentes façons de conserver l’application et l’état de session entre les demandes.</span><span class="sxs-lookup"><span data-stu-id="339d9-107">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="339d9-108">État de session</span><span class="sxs-lookup"><span data-stu-id="339d9-108">Session state</span></span>

<span data-ttu-id="339d9-109">L’état de session est une fonctionnalité ASP.NET Core que vous pouvez utiliser pour enregistrer et stocker des données utilisateur pendant que l’utilisateur navigue dans votre application web.</span><span class="sxs-lookup"><span data-stu-id="339d9-109">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="339d9-110">Consistant en une table de hachage ou de dictionnaire sur le serveur, l’état de session conserve les données entre les demandes à partir d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="339d9-110">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="339d9-111">Les données de session sont sauvegardées par un cache.</span><span class="sxs-lookup"><span data-stu-id="339d9-111">The session data is backed by a cache.</span></span>

<span data-ttu-id="339d9-112">ASP.NET Core maintient l’état de session en donnant le client un cookie qui contient l’ID de session, qui est envoyée au serveur avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="339d9-112">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="339d9-113">Le serveur utilise l’ID de session pour extraire les données de session.</span><span class="sxs-lookup"><span data-stu-id="339d9-113">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="339d9-114">Étant donné que le cookie de session est spécifique au navigateur, vous ne peuvent pas partager des sessions dans les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="339d9-114">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="339d9-115">Les cookies de session sont supprimées uniquement lorsque la session se termine.</span><span class="sxs-lookup"><span data-stu-id="339d9-115">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="339d9-116">Si un cookie est reçu pour une session a expiré, une nouvelle session qui utilise le même cookie de session est créée.</span><span class="sxs-lookup"><span data-stu-id="339d9-116">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="339d9-117">Le serveur conserve une session pendant une période limitée après la dernière demande.</span><span class="sxs-lookup"><span data-stu-id="339d9-117">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="339d9-118">Vous pouvez définir le délai d’expiration de session ou utiliser la valeur par défaut de 20 minutes.</span><span class="sxs-lookup"><span data-stu-id="339d9-118">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="339d9-119">État de session est idéal pour le stockage des données utilisateur qui sont spécifique à une session particulière, mais ne doivent pas être persistante de façon permanente.</span><span class="sxs-lookup"><span data-stu-id="339d9-119">Session state is ideal for storing user data that's specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="339d9-120">Données sont supprimées du magasin de stockage soit lorsque vous appelez `Session.Clear` ou l’expiration de la session dans le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="339d9-120">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="339d9-121">Le serveur ne sait pas lorsque le navigateur est fermé ou lorsque le cookie de session est supprimé.</span><span class="sxs-lookup"><span data-stu-id="339d9-121">The server doesn't know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="339d9-122">Ne pas stocker les données sensibles dans la session.</span><span class="sxs-lookup"><span data-stu-id="339d9-122">Don't store sensitive data in session.</span></span> <span data-ttu-id="339d9-123">Le client ne peut pas fermer le navigateur et effacer le cookie de session (et certains navigateurs maintenir les cookies de session sur windows).</span><span class="sxs-lookup"><span data-stu-id="339d9-123">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="339d9-124">En outre, une session ne peut pas être restreinte à un seul utilisateur ; l’utilisateur suivant risque de continuer avec la même session.</span><span class="sxs-lookup"><span data-stu-id="339d9-124">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="339d9-125">Le fournisseur de session en mémoire stocke les données de session sur le serveur local.</span><span class="sxs-lookup"><span data-stu-id="339d9-125">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="339d9-126">Si vous envisagez d’exécuter votre application web sur une batterie de serveurs, vous devez utiliser des sessions rémanentes pour lier chaque session à un serveur spécifique.</span><span class="sxs-lookup"><span data-stu-id="339d9-126">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="339d9-127">La plateforme Windows Azure Web Sites par défaut, les sessions rémanentes (Application Request Routing ou ARR).</span><span class="sxs-lookup"><span data-stu-id="339d9-127">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="339d9-128">Toutefois, les sessions rémanentes peuvent affecter l’évolutivité et compliquer la mise à jour des applications web.</span><span class="sxs-lookup"><span data-stu-id="339d9-128">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="339d9-129">Une meilleure option consiste à utiliser le Redis ou distribuées SQL Server met en cache, qui ne nécessitent pas sessions rémanentes.</span><span class="sxs-lookup"><span data-stu-id="339d9-129">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="339d9-130">Pour plus d’informations, consultez [fonctionne avec un Cache distribué de](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="339d9-130">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="339d9-131">Pour plus d’informations sur la configuration des fournisseurs de services, consultez [configuration de Session](#configuring-session) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="339d9-131">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="339d9-132">TempData</span><span class="sxs-lookup"><span data-stu-id="339d9-132">TempData</span></span>

<span data-ttu-id="339d9-133">ASP.NET MVC de base expose le [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) propriété sur un [contrôleur](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="339d9-133">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="339d9-134">Cette propriété stocke les données jusqu'à ce qu’il est en lecture.</span><span class="sxs-lookup"><span data-stu-id="339d9-134">This property stores data until it's read.</span></span> <span data-ttu-id="339d9-135">Vous pouvez utiliser les méthodes `Keep` et `Peek` pour examiner les données sans suppression.</span><span class="sxs-lookup"><span data-stu-id="339d9-135">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="339d9-136">`TempData`est particulièrement utile pour la redirection, lorsqu’il manque des données plus longtemps qu’une demande unique.</span><span class="sxs-lookup"><span data-stu-id="339d9-136">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="339d9-137">`TempData`est implémenté par les fournisseurs de TempData, par exemple, à l’aide de cookies ou l’état de session.</span><span class="sxs-lookup"><span data-stu-id="339d9-137">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="339d9-138">Fournisseurs de TempData</span><span class="sxs-lookup"><span data-stu-id="339d9-138">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="339d9-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="339d9-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="339d9-140">Dans ASP.NET Core 2.0 et versions ultérieures, le fournisseur TempData basé sur cookie est utilisé par défaut pour stocker TempData dans des cookies.</span><span class="sxs-lookup"><span data-stu-id="339d9-140">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="339d9-141">Les données de cookie sont codées avec le [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="339d9-141">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="339d9-142">Étant donné que le cookie est chiffré et mémorisé en bloc, le cookie unique trouvée dans ASP.NET Core 1.x ne s’applique pas de limite de taille.</span><span class="sxs-lookup"><span data-stu-id="339d9-142">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="339d9-143">Les données de cookie ne sont pas compressées, car la compression des données chiffrées peut entraîner des problèmes de sécurité telles que la [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) et [violation](https://wikipedia.org/wiki/BREACH_(security_exploit)) attaques.</span><span class="sxs-lookup"><span data-stu-id="339d9-143">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="339d9-144">Pour plus d’informations sur le fournisseur TempData basé sur cookie, consultez [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="339d9-144">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="339d9-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="339d9-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="339d9-146">Dans ASP.NET Core 1.0 et 1.1, le fournisseur TempData état de session est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="339d9-146">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="339d9-147">Choix d’un fournisseur TempData</span><span class="sxs-lookup"><span data-stu-id="339d9-147">Choosing a TempData provider</span></span>

<span data-ttu-id="339d9-148">Choix d’un fournisseur TempData implique de plusieurs considérations, telles que :</span><span class="sxs-lookup"><span data-stu-id="339d9-148">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="339d9-149">L’application a déjà utilise-t-il l’état de session à d’autres fins ?</span><span class="sxs-lookup"><span data-stu-id="339d9-149">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="339d9-150">Dans ce cas, à l’aide du fournisseur de TempData de l’état de session a sans coût supplémentaire à l’application (à l’exception de la taille des données).</span><span class="sxs-lookup"><span data-stu-id="339d9-150">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="339d9-151">L’application utilise-t-il TempData uniquement avec parcimonie pour relativement petites quantités de données (jusqu'à 500 octets) ?</span><span class="sxs-lookup"><span data-stu-id="339d9-151">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="339d9-152">Si, par conséquent, le fournisseur de TempData cookie ajoutera un petit coût à chaque demande qui achemine TempData.</span><span class="sxs-lookup"><span data-stu-id="339d9-152">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="339d9-153">Si ce n’est pas le cas, le fournisseur TempData état de session peut être utile éviter l’aller-retour une grande quantité de données dans chaque demande jusqu'à ce que le TempData est consommé.</span><span class="sxs-lookup"><span data-stu-id="339d9-153">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="339d9-154">L’application s’exécute dans une batterie de serveurs web (plusieurs serveurs) ?</span><span class="sxs-lookup"><span data-stu-id="339d9-154">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="339d9-155">Dans ce cas, aucune configuration supplémentaire n’est nécessaire pour utiliser le fournisseur de TempData de cookie.</span><span class="sxs-lookup"><span data-stu-id="339d9-155">If so, there's no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="339d9-156">La plupart des clients web (par exemple, les navigateurs web) renforce les limites de la taille maximale de chaque cookie, le nombre total de cookies ou les deux.</span><span class="sxs-lookup"><span data-stu-id="339d9-156">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="339d9-157">Par conséquent, lorsque vous utilisez le fournisseur de TempData cookie, vérifiez que l’application ne dépasse ces limites.</span><span class="sxs-lookup"><span data-stu-id="339d9-157">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="339d9-158">Prendre en compte la taille totale des données, gestion des comptes pour les charges de chiffrement et de segmentation.</span><span class="sxs-lookup"><span data-stu-id="339d9-158">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="339d9-159">Configurer le fournisseur TempData</span><span class="sxs-lookup"><span data-stu-id="339d9-159">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="339d9-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="339d9-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="339d9-161">Le fournisseur TempData basé sur cookie est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="339d9-161">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="339d9-162">Les éléments suivants `Startup` code de la classe configure le fournisseur TempData basé sur session :</span><span class="sxs-lookup"><span data-stu-id="339d9-162">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="339d9-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="339d9-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="339d9-164">Les éléments suivants `Startup` code de la classe configure le fournisseur TempData basé sur session :</span><span class="sxs-lookup"><span data-stu-id="339d9-164">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="339d9-165">Classement est essentiel pour les composants d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="339d9-165">Ordering is critical for middleware components.</span></span> <span data-ttu-id="339d9-166">Dans l’exemple précédent, une exception de type `InvalidOperationException` se produit lorsque `UseSession` est appelé après `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="339d9-166">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="339d9-167">Consultez [intergiciel (middleware) classement](xref:fundamentals/middleware#ordering) pour plus de détails.</span><span class="sxs-lookup"><span data-stu-id="339d9-167">See [Middleware Ordering](xref:fundamentals/middleware#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="339d9-168">Si ciblant .NET Framework et à l’aide du fournisseur basé sur session, ajoutez le [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) package NuGet à votre projet.</span><span class="sxs-lookup"><span data-stu-id="339d9-168">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="339d9-169">Chaînes de requête</span><span class="sxs-lookup"><span data-stu-id="339d9-169">Query strings</span></span>

<span data-ttu-id="339d9-170">Vous pouvez passer une quantité limitée de données à partir d’une demande à un autre en l’ajoutant à la chaîne de requête de la nouvelle demande.</span><span class="sxs-lookup"><span data-stu-id="339d9-170">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="339d9-171">Cela est utile pour capturer l’état de manière persistante qui autorise les liens avec état incorporée à partager par courrier électronique ou de réseaux sociaux.</span><span class="sxs-lookup"><span data-stu-id="339d9-171">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="339d9-172">Toutefois, pour cette raison, vous ne devez jamais utiliser des chaînes de requête pour les données sensibles.</span><span class="sxs-lookup"><span data-stu-id="339d9-172">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="339d9-173">En plus facilement partagé, y compris les données dans les chaînes de requête peut créer des opportunités pour [Cross-Site demande Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attaques, ce qui peuvent amener les utilisateurs à visiter les sites malveillants lors de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="339d9-173">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="339d9-174">Des personnes malveillantes peuvent ensuite dérober des données utilisateur à partir de votre application ou des actions malveillantes part de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="339d9-174">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="339d9-175">N’importe quel état de session ou application préservé doit protéger contre les attaques CSRF.</span><span class="sxs-lookup"><span data-stu-id="339d9-175">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="339d9-176">Pour plus d’informations sur les attaques CSRF, consultez [attaques empêchant Cross-Site Request Forgery (XSRF/CSRF) dans ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="339d9-176">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="339d9-177">Données de publication et les champs masqués</span><span class="sxs-lookup"><span data-stu-id="339d9-177">Post data and hidden fields</span></span>

<span data-ttu-id="339d9-178">Données peuvent être enregistrées dans les champs masqués et validées sur la demande suivante.</span><span class="sxs-lookup"><span data-stu-id="339d9-178">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="339d9-179">Cela est courant dans les formulaires de plusieurs pages.</span><span class="sxs-lookup"><span data-stu-id="339d9-179">This is common in multi-page forms.</span></span> <span data-ttu-id="339d9-180">Toutefois, étant donné que le client peut potentiellement falsifier des données, le serveur doit toujours revalider.</span><span class="sxs-lookup"><span data-stu-id="339d9-180">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="339d9-181">Cookies</span><span class="sxs-lookup"><span data-stu-id="339d9-181">Cookies</span></span>

<span data-ttu-id="339d9-182">Les cookies permettent de stocker des données spécifiques à l’utilisateur dans les applications web.</span><span class="sxs-lookup"><span data-stu-id="339d9-182">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="339d9-183">Étant donné que les cookies sont envoyés avec chaque demande, leur taille doit être conservée au minimum.</span><span class="sxs-lookup"><span data-stu-id="339d9-183">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="339d9-184">Dans l’idéal, doit être stocké dans un cookie uniquement un identificateur avec les données réelles stockées sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="339d9-184">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="339d9-185">La plupart des navigateurs limiter les cookies à 4 096 octets.</span><span class="sxs-lookup"><span data-stu-id="339d9-185">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="339d9-186">En outre, qu’un nombre limité de cookies est disponible pour chaque domaine.</span><span class="sxs-lookup"><span data-stu-id="339d9-186">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="339d9-187">Étant donné que les cookies sont au risque de falsification, ils doivent être validées sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="339d9-187">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="339d9-188">Bien que la durabilité du cookie sur un client est soumis à l’expiration et l’intervention de l’utilisateur, ils sont généralement de la forme la plus durable de persistance des données sur le client.</span><span class="sxs-lookup"><span data-stu-id="339d9-188">Although the durability of the cookie on a client is subject to user intervention and expiration, they're generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="339d9-189">Les cookies sont souvent utilisés pour la personnalisation, où le contenu est personnalisé pour un utilisateur connu.</span><span class="sxs-lookup"><span data-stu-id="339d9-189">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="339d9-190">Étant donné que l’utilisateur est identifié uniquement et pas authentifié dans la plupart des cas, vous pouvez généralement sécuriser un cookie en stockant le nom d’utilisateur, nom de compte ou un ID d’utilisateur unique (par exemple, un GUID) dans le cookie.</span><span class="sxs-lookup"><span data-stu-id="339d9-190">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="339d9-191">Vous pouvez ensuite utiliser le cookie pour accéder à l’infrastructure de personnalisation utilisateur d’un site.</span><span class="sxs-lookup"><span data-stu-id="339d9-191">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="339d9-192">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="339d9-192">HttpContext.Items</span></span>

<span data-ttu-id="339d9-193">Le `Items` collection est un bon emplacement pour stocker les données que nécessaire uniquement lors du traitement d’une requête particulière.</span><span class="sxs-lookup"><span data-stu-id="339d9-193">The `Items` collection is a good location to store data that's needed only while processing one particular request.</span></span> <span data-ttu-id="339d9-194">Contenu de la collection est supprimés après chaque demande.</span><span class="sxs-lookup"><span data-stu-id="339d9-194">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="339d9-195">Le `Items` collection est mieux utilisée comme un moyen pour les composants ou d’intergiciel (middleware) pour communiquer lorsqu’ils fonctionnent à différents moments dans le temps au cours d’une demande et n’ont aucun moyen direct pour passer des paramètres.</span><span class="sxs-lookup"><span data-stu-id="339d9-195">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="339d9-196">Pour plus d’informations, consultez [utilisation de HttpContext.Items](#working-with-httpcontextitems), plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="339d9-196">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="339d9-197">d'instance/de clé</span><span class="sxs-lookup"><span data-stu-id="339d9-197">Cache</span></span>

<span data-ttu-id="339d9-198">La mise en cache est un moyen efficace de stocker et récupérer des données.</span><span class="sxs-lookup"><span data-stu-id="339d9-198">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="339d9-199">Vous pouvez contrôler la durée de vie des éléments du cache en fonction de l’heure et d’autres considérations.</span><span class="sxs-lookup"><span data-stu-id="339d9-199">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="339d9-200">En savoir plus sur [mise en cache](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="339d9-200">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="339d9-201">Utilisation de l’état de Session</span><span class="sxs-lookup"><span data-stu-id="339d9-201">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="339d9-202">Configuration de Session</span><span class="sxs-lookup"><span data-stu-id="339d9-202">Configuring Session</span></span>

<span data-ttu-id="339d9-203">Le `Microsoft.AspNetCore.Session` package fournit un intergiciel (middleware) pour gérer l’état de session.</span><span class="sxs-lookup"><span data-stu-id="339d9-203">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="339d9-204">Pour activer l’intergiciel de session, `Startup` doit contenir :</span><span class="sxs-lookup"><span data-stu-id="339d9-204">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="339d9-205">Un de le [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) caches mémoire.</span><span class="sxs-lookup"><span data-stu-id="339d9-205">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="339d9-206">Le `IDistributedCache` implémentation est utilisée comme un magasin de stockage pour la session.</span><span class="sxs-lookup"><span data-stu-id="339d9-206">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="339d9-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) appeler, ce qui nécessite le package NuGet « Microsoft.AspNetCore.Session ».</span><span class="sxs-lookup"><span data-stu-id="339d9-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="339d9-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) appeler.</span><span class="sxs-lookup"><span data-stu-id="339d9-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="339d9-209">Le code suivant montre comment définir le fournisseur de session en mémoire.</span><span class="sxs-lookup"><span data-stu-id="339d9-209">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="339d9-210">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="339d9-210">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="339d9-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="339d9-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="339d9-212">Vous pouvez faire référence à la Session à partir de `HttpContext` une fois qu’il a installé et configuré.</span><span class="sxs-lookup"><span data-stu-id="339d9-212">You can reference Session from `HttpContext` once it's installed and configured.</span></span>

<span data-ttu-id="339d9-213">Si vous essayez d’accéder à `Session` avant `UseSession` a été appelée, l’exception `InvalidOperationException: Session has not been configured for this application or request` est levée.</span><span class="sxs-lookup"><span data-stu-id="339d9-213">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="339d9-214">Si vous essayez de créer un nouveau `Session` (autrement dit, aucun cookie de session n’a été créé) une fois que vous avez déjà commencé l’écriture dans le `Response` diffuser en continu, l’exception `InvalidOperationException: The session cannot be established after the response has started` est levée.</span><span class="sxs-lookup"><span data-stu-id="339d9-214">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="339d9-215">L’exception peut être trouvée dans le journal de serveur web ; Il ne sera pas affiché dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="339d9-215">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="339d9-216">Chargement asynchrone de Session</span><span class="sxs-lookup"><span data-stu-id="339d9-216">Loading Session asynchronously</span></span> 

<span data-ttu-id="339d9-217">Le fournisseur de session par défaut dans ASP.NET Core charge l’enregistrement de session sous-jacent [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) magasin de façon asynchrone uniquement si le [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) méthode est appelée explicitement avant  le `TryGetValue`, `Set`, ou `Remove` méthodes.</span><span class="sxs-lookup"><span data-stu-id="339d9-217">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="339d9-218">Si `LoadAsync` n’est pas appelée en premier, sous-jacent enregistrement de session est chargée de façon synchrone, ce qui pourrait avoir un impact sur la capacité de l’application à l’échelle.</span><span class="sxs-lookup"><span data-stu-id="339d9-218">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="339d9-219">Pour que les applications d’appliquer ce modèle, vous devez encapsuler le [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) et [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implémentations avec des versions qui lèvent une exception si le `LoadAsync` n’est pas de méthode appelé avant `TryGetValue`, `Set`, ou `Remove`.</span><span class="sxs-lookup"><span data-stu-id="339d9-219">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="339d9-220">Enregistrer les versions encapsulées dans le conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="339d9-220">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="339d9-221">Détails d’implémentation</span><span class="sxs-lookup"><span data-stu-id="339d9-221">Implementation Details</span></span>

<span data-ttu-id="339d9-222">Session utilise un cookie pour suivre et identifier les demandes à partir d’un seul navigateur.</span><span class="sxs-lookup"><span data-stu-id="339d9-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="339d9-223">Par défaut, ce cookie est nommé ». AspNet.Session » et qu’il utilise un chemin d’accès « / ».</span><span class="sxs-lookup"><span data-stu-id="339d9-223">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="339d9-224">Étant donné que la valeur par défaut du cookie ne spécifie pas un domaine, il n'est pas mis à disposition le script côté client dans la page (car `CookieHttpOnly` par défaut est `true`).</span><span class="sxs-lookup"><span data-stu-id="339d9-224">Because the cookie default doesn't specify a domain, it's not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="339d9-225">Pour remplacer les valeurs par défaut de la session, utilisez `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="339d9-225">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="339d9-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="339d9-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="339d9-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="339d9-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="339d9-228">Le serveur utilise le `IdleTimeout` propriété pour déterminer la durée pendant laquelle une session peut être inactive avant que son contenu est abandonné.</span><span class="sxs-lookup"><span data-stu-id="339d9-228">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="339d9-229">Cette propriété est indépendante de l’expiration du cookie.</span><span class="sxs-lookup"><span data-stu-id="339d9-229">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="339d9-230">Chaque demande qui passe par l’intergiciel de Session (lues ou écrites pour) réinitialise le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="339d9-230">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="339d9-231">Étant donné que `Session` est *sans verrouillage*, si deux demandes tentent de modifier le contenu de la session, il remplace le premier.</span><span class="sxs-lookup"><span data-stu-id="339d9-231">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="339d9-232">`Session`est implémenté comme un *session cohérente*, ce qui signifie que tout le contenu est stocké ensemble.</span><span class="sxs-lookup"><span data-stu-id="339d9-232">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="339d9-233">Deux requêtes qui sont modifier différentes parties de la session (clés différents) peuvent toujours avoir un impact sur eux.</span><span class="sxs-lookup"><span data-stu-id="339d9-233">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="339d9-234">Définition et l’obtention des valeurs de Session</span><span class="sxs-lookup"><span data-stu-id="339d9-234">Setting and getting Session values</span></span>

<span data-ttu-id="339d9-235">Session est accessible via la `Session` propriété sur `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="339d9-235">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="339d9-236">Cette propriété est un [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implémentation.</span><span class="sxs-lookup"><span data-stu-id="339d9-236">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="339d9-237">L’exemple suivant illustre la définition et l’obtention d’une valeur int et une chaîne :</span><span class="sxs-lookup"><span data-stu-id="339d9-237">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="339d9-238">Si vous ajoutez les méthodes d’extension, vous pouvez définir et obtenir des objets sérialisables à la Session :</span><span class="sxs-lookup"><span data-stu-id="339d9-238">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="339d9-239">L’exemple suivant montre comment définir et obtenir un objet sérialisable :</span><span class="sxs-lookup"><span data-stu-id="339d9-239">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="339d9-240">Utilisation de HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="339d9-240">Working with HttpContext.Items</span></span>

<span data-ttu-id="339d9-241">Le `HttpContext` abstraction fournit la prise en charge pour une collection de dictionnaires de type `IDictionary<object, object>`, appelé `Items`.</span><span class="sxs-lookup"><span data-stu-id="339d9-241">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="339d9-242">Cette collection est disponible depuis le début d’une *HttpRequest* et supprimées à la fin de chaque demande.</span><span class="sxs-lookup"><span data-stu-id="339d9-242">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="339d9-243">Il est accessible en affectant une valeur à une entrée de clé, ou en demandant la valeur d’une clé particulière.</span><span class="sxs-lookup"><span data-stu-id="339d9-243">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="339d9-244">Dans l’exemple ci-dessous, [intergiciel (middleware)](middleware.md) ajoute `isVerified` à la `Items` collection.</span><span class="sxs-lookup"><span data-stu-id="339d9-244">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="339d9-245">Plus tard dans le pipeline, un autre intergiciel (middleware) peut y accéder :</span><span class="sxs-lookup"><span data-stu-id="339d9-245">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="339d9-246">Pour un intergiciel (middleware) qui servira uniquement par une application unique, `string` clés sont acceptables.</span><span class="sxs-lookup"><span data-stu-id="339d9-246">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="339d9-247">Toutefois, intergiciel (middleware) qui est partagée entre les applications doit utiliser des clés de l’objet unique afin d’éviter tout risque de collision de clés.</span><span class="sxs-lookup"><span data-stu-id="339d9-247">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="339d9-248">Si vous développez un intergiciel (middleware) qui doit fonctionner sur plusieurs applications, utilisez une clé d’objet unique définie dans votre classe d’intergiciel (middleware) comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="339d9-248">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="339d9-249">Tout autre code peut accéder à la valeur stockée dans `HttpContext.Items` à l’aide de la clé exposée par la classe de l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="339d9-249">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="339d9-250">Cette approche a également l’avantage d’éliminer la répétition de « chaînes magiques » à plusieurs endroits dans le code.</span><span class="sxs-lookup"><span data-stu-id="339d9-250">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="339d9-251">Données de l’état</span><span class="sxs-lookup"><span data-stu-id="339d9-251">Application state data</span></span>

<span data-ttu-id="339d9-252">Utilisez [Injection de dépendance](xref:fundamentals/dependency-injection) pour rendre les données accessibles à tous les utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="339d9-252">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="339d9-253">Définir un service qui contient les données (par exemple, une classe nommée `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="339d9-253">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="339d9-254">Ajouter la classe de service `ConfigureServices` (par exemple `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="339d9-254">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="339d9-255">Consommer la classe de service de données dans chaque contrôleur :</span><span class="sxs-lookup"><span data-stu-id="339d9-255">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="339d9-256">Erreurs courantes lors de l’utilisation avec une session</span><span class="sxs-lookup"><span data-stu-id="339d9-256">Common errors when working with session</span></span>

* <span data-ttu-id="339d9-257">« Impossible de résoudre le service pour le type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' lors de la tentative d’activation 'Microsoft.AspNetCore.Session.DistributedSessionStore'. »</span><span class="sxs-lookup"><span data-stu-id="339d9-257">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="339d9-258">Cela est généralement dû au moins une configuration `IDistributedCache` implémentation.</span><span class="sxs-lookup"><span data-stu-id="339d9-258">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="339d9-259">Pour plus d’informations, consultez [fonctionne avec un Cache distribué de](xref:performance/caching/distributed) et [mise en mémoire cache](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="339d9-259">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="339d9-260">Dans l’événement que la session intergiciel (middleware) ne parvient pas à conserver une session (par exemple : si la base de données n’est pas disponible), il enregistre l’exception et avale.</span><span class="sxs-lookup"><span data-stu-id="339d9-260">In the event that the session middleware fails to persist a session (for example: if the database isn't available), it logs the exception and swallows it.</span></span> <span data-ttu-id="339d9-261">La demande continue ensuite normalement, ce qui aboutit à un comportement imprévisible très.</span><span class="sxs-lookup"><span data-stu-id="339d9-261">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="339d9-262">Voici un exemple typique :</span><span class="sxs-lookup"><span data-stu-id="339d9-262">A typical example:</span></span>

<span data-ttu-id="339d9-263">Un utilisateur stocke un panier d’achat dans la session.</span><span class="sxs-lookup"><span data-stu-id="339d9-263">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="339d9-264">L’utilisateur ajoute un élément, mais la validation échoue.</span><span class="sxs-lookup"><span data-stu-id="339d9-264">The user adds an item but the commit fails.</span></span> <span data-ttu-id="339d9-265">L’application ne connaît pas l’échec afin qu’il affiche le message « l’élément a été ajouté », ce qui n’est pas vrai.</span><span class="sxs-lookup"><span data-stu-id="339d9-265">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="339d9-266">La méthode recommandée pour rechercher les erreurs de ce type consiste à appeler `await feature.Session.CommitAsync();` à partir du code d’application lorsque vous avez terminé l’écriture dans la session.</span><span class="sxs-lookup"><span data-stu-id="339d9-266">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="339d9-267">Vous pouvez faire ce que vous aimez avec l’erreur.</span><span class="sxs-lookup"><span data-stu-id="339d9-267">Then you can do what you like with the error.</span></span> <span data-ttu-id="339d9-268">Il fonctionne de la même manière lors de l’appel `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="339d9-268">It works the same way when calling `LoadAsync`.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="339d9-269">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="339d9-269">Additional Resources</span></span>


* [<span data-ttu-id="339d9-270">ASP.NET Core 1.x : exemple de code utilisé dans ce document</span><span class="sxs-lookup"><span data-stu-id="339d9-270">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="339d9-271">ASP.NET Core 2.x : exemple de code utilisé dans ce document</span><span class="sxs-lookup"><span data-stu-id="339d9-271">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
