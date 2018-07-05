---
uid: web-api/overview/advanced/http-cookies
title: Les Cookies HTTP dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 5885586df1d0f67d4e7e04ad88bc4fd1af71dc80
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368403"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="11af5-102">Cookies HTTP dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="11af5-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="11af5-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="11af5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="11af5-104">Cette rubrique décrit comment envoyer et recevoir des cookies HTTP dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="11af5-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="11af5-105">En arrière-plan sur les Cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="11af5-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="11af5-106">Cette section donne un bref aperçu de la façon dont les cookies sont implémentés au niveau HTTP.</span><span class="sxs-lookup"><span data-stu-id="11af5-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="11af5-107">Pour plus d’informations, consultez [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="11af5-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="11af5-108">Un cookie est un élément de données qui envoie par un serveur dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="11af5-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="11af5-109">Le client stocke le cookie (facultatif) et le retourne sur subsequet demandes.</span><span class="sxs-lookup"><span data-stu-id="11af5-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="11af5-110">Ainsi, le client et le serveur partager l’état.</span><span class="sxs-lookup"><span data-stu-id="11af5-110">This allows the client and server to share state.</span></span> <span data-ttu-id="11af5-111">Pour définir un cookie, le serveur inclut un en-tête Set-Cookie dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="11af5-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="11af5-112">Le format d’un cookie est une paire nom-valeur, avec des attributs facultatifs.</span><span class="sxs-lookup"><span data-stu-id="11af5-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="11af5-113">Exemple :</span><span class="sxs-lookup"><span data-stu-id="11af5-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="11af5-114">Voici un exemple avec des attributs :</span><span class="sxs-lookup"><span data-stu-id="11af5-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="11af5-115">Pour retourner un cookie au serveur, le client inclut un en-tête de Cookie dans les demandes ultérieures.</span><span class="sxs-lookup"><span data-stu-id="11af5-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="11af5-116">Une réponse HTTP peut inclure plusieurs en-têtes Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="11af5-117">Le client retourne plusieurs cookies à l’aide d’un en-tête de Cookie unique.</span><span class="sxs-lookup"><span data-stu-id="11af5-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="11af5-118">L’étendue et la durée d’un cookie sont contrôlées par les attributs suivants dans l’en-tête Set-Cookie :</span><span class="sxs-lookup"><span data-stu-id="11af5-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="11af5-119">**Domaine**: indique au client de domaine qui doit recevoir le cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="11af5-120">Par exemple, si le domaine est « example.com », le client retourne le cookie à chaque sous-domaine de example.com.</span><span class="sxs-lookup"><span data-stu-id="11af5-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="11af5-121">Si non spécifié, le domaine est le serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="11af5-121">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="11af5-122">**Chemin d’accès**: limite le cookie pour le chemin d’accès spécifié au sein du domaine.</span><span class="sxs-lookup"><span data-stu-id="11af5-122">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="11af5-123">Si non spécifié, le chemin d’accès de l’URI de demande est utilisé.</span><span class="sxs-lookup"><span data-stu-id="11af5-123">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="11af5-124">**Expiration**: définit une date d’expiration du cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-124">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="11af5-125">Le client supprime le cookie expiré.</span><span class="sxs-lookup"><span data-stu-id="11af5-125">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="11af5-126">**Max-Age**: définit l’âge maximal pour le cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-126">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="11af5-127">Le client supprime le cookie lorsqu’il atteint l’âge maximal.</span><span class="sxs-lookup"><span data-stu-id="11af5-127">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="11af5-128">Si les deux `Expires` et `Max-Age` sont définies, `Max-Age` est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="11af5-128">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="11af5-129">Si aucun n’est défini, le client supprime le cookie lorsque la session active se termine.</span><span class="sxs-lookup"><span data-stu-id="11af5-129">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="11af5-130">(La signification exacte de « session » est déterminée par l’agent utilisateur).</span><span class="sxs-lookup"><span data-stu-id="11af5-130">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="11af5-131">Toutefois, n’oubliez pas que les clients peuvent ignorer les cookies.</span><span class="sxs-lookup"><span data-stu-id="11af5-131">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="11af5-132">Par exemple, un utilisateur peut désactiver les cookies pour des raisons de confidentialité.</span><span class="sxs-lookup"><span data-stu-id="11af5-132">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="11af5-133">Les clients peuvent supprimer les cookies avant leur expiration ou limitent le nombre de cookies stockés.</span><span class="sxs-lookup"><span data-stu-id="11af5-133">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="11af5-134">Pour des raisons de confidentialité, les clients rejettent souvent des cookies de « tiers », où le domaine ne correspond pas au serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="11af5-134">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="11af5-135">En bref, le serveur ne doit pas s’appuient sur retour des cookies qu’il définit.</span><span class="sxs-lookup"><span data-stu-id="11af5-135">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="11af5-136">Cookies dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="11af5-136">Cookies in Web API</span></span>

<span data-ttu-id="11af5-137">Pour ajouter un cookie à une réponse HTTP, créez un **CookieHeaderValue** instance qui représente le cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-137">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="11af5-138">Appelez ensuite la **AddCookies** méthode d’extension, qui est défini dans le **System.Net.Http. HttpResponseHeadersExtensions** (classe), pour ajouter le cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-138">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="11af5-139">Par exemple, le code suivant ajoute un cookie au sein d’une action de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="11af5-139">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="11af5-140">Notez que **AddCookies** prend un tableau de **CookieHeaderValue** instances.</span><span class="sxs-lookup"><span data-stu-id="11af5-140">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="11af5-141">Pour extraire les cookies à partir d’une demande du client, appelez le **GetCookies** méthode :</span><span class="sxs-lookup"><span data-stu-id="11af5-141">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="11af5-142">Un **CookieHeaderValue** contient une collection de **CookieState** instances.</span><span class="sxs-lookup"><span data-stu-id="11af5-142">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="11af5-143">Chaque **CookieState** représente un seul petit gâteau.</span><span class="sxs-lookup"><span data-stu-id="11af5-143">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="11af5-144">Utilisez la méthode de l’indexeur pour obtenir un **CookieState** par nom, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="11af5-144">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="11af5-145">Données de Cookie structurée</span><span class="sxs-lookup"><span data-stu-id="11af5-145">Structured Cookie Data</span></span>

<span data-ttu-id="11af5-146">De nombreux navigateurs limitent le nombre de cookies qu’ils stockera&#8212;à la fois le nombre total et le nombre par domaine.</span><span class="sxs-lookup"><span data-stu-id="11af5-146">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="11af5-147">Par conséquent, il peut être utile placer des données structurées dans un cookie unique, au lieu de définir plusieurs cookies.</span><span class="sxs-lookup"><span data-stu-id="11af5-147">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="11af5-148">RFC 6265 ne définit pas la structure des données de cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-148">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="11af5-149">À l’aide de la **CookieHeaderValue** (classe), vous pouvez passer une liste de paires nom-valeur pour les données de cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-149">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="11af5-150">Ces paires nom-valeur sont encodés en tant que données de forme codée URL dans l’en-tête Set-Cookie :</span><span class="sxs-lookup"><span data-stu-id="11af5-150">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="11af5-151">Le code précédent génère l’en-tête Set-Cookie suivant :</span><span class="sxs-lookup"><span data-stu-id="11af5-151">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="11af5-152">Le **CookieState** classe fournit une méthode de l’indexeur pour lire les valeurs de sous-élément à partir d’un cookie dans le message de demande :</span><span class="sxs-lookup"><span data-stu-id="11af5-152">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="11af5-153">Exemple : Définir et récupérer des Cookies dans un gestionnaire de messages</span><span class="sxs-lookup"><span data-stu-id="11af5-153">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="11af5-154">Les exemples précédents ont montré comment utiliser des cookies à partir d’un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="11af5-154">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="11af5-155">Une autre option consiste à utiliser [gestionnaires de messages](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="11af5-155">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="11af5-156">Gestionnaires de messages sont appelées plus tôt dans le pipeline de contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="11af5-156">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="11af5-157">Un gestionnaire de messages pour lire les cookies à partir de la demande avant que la demande n’atteigne le contrôleur, ou ajouter des cookies à la réponse après que le contrôleur a généré la réponse.</span><span class="sxs-lookup"><span data-stu-id="11af5-157">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="11af5-158">Le code suivant montre un gestionnaire de messages pour la création d’ID de session.</span><span class="sxs-lookup"><span data-stu-id="11af5-158">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="11af5-159">L’ID de session est stocké dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="11af5-159">The session ID is stored in a cookie.</span></span> <span data-ttu-id="11af5-160">Le gestionnaire vérifie la demande pour le cookie de session.</span><span class="sxs-lookup"><span data-stu-id="11af5-160">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="11af5-161">Si la demande n’inclut pas le cookie, le gestionnaire génère un ID de la nouvelle session.</span><span class="sxs-lookup"><span data-stu-id="11af5-161">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="11af5-162">Dans les deux cas, le gestionnaire stocke l’ID de session dans le **HttpRequestMessage.Properties** sac de propriétés.</span><span class="sxs-lookup"><span data-stu-id="11af5-162">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="11af5-163">Il ajoute également le cookie de session à la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="11af5-163">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="11af5-164">Cette implémentation ne valide pas que l’ID de session à partir du client a été effectivement émis par le serveur.</span><span class="sxs-lookup"><span data-stu-id="11af5-164">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="11af5-165">Ne l’utilisez comme un formulaire d’authentification !</span><span class="sxs-lookup"><span data-stu-id="11af5-165">Don't use it as a form of authentication!</span></span> <span data-ttu-id="11af5-166">Le but de cet exemple est de montrer de gestion des cookies HTTP.</span><span class="sxs-lookup"><span data-stu-id="11af5-166">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="11af5-167">Un contrôleur peut obtenir l’ID de session à partir de la **HttpRequestMessage.Properties** sac de propriétés.</span><span class="sxs-lookup"><span data-stu-id="11af5-167">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
