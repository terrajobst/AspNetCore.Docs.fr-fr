---
title: Listes d’IP client fiables pour ASP.NET Core
author: damienbod
description: Apprenez à écrire des filtres d’intergiciel (middleware) ou une action pour valider les adresses IP distantes par rapport à une liste des adresses IP approuvées.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040108"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="4ca51-103">Listes d’IP client fiables pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ca51-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="4ca51-104">Par [Damien Bowden](https://twitter.com/damien_bod) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4ca51-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="4ca51-105">Cet article montre deux façons d’implémenter un safelist IP (également appelé liste blanche) :</span><span class="sxs-lookup"><span data-stu-id="4ca51-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="4ca51-106">En utilisant des intergiciels (middleware) ASP.NET Core pour vérifier l’adresse IP distante de chaque demande.</span><span class="sxs-lookup"><span data-stu-id="4ca51-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="4ca51-107">En utilisant des filtres d’action ASP.NET Core pour vérifier l’adresse IP distante de requêtes pour les méthodes d’action spécifique.</span><span class="sxs-lookup"><span data-stu-id="4ca51-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="4ca51-108">L’exemple d’application illustre les deux approches.</span><span class="sxs-lookup"><span data-stu-id="4ca51-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="4ca51-109">Dans chaque cas, une chaîne contenant les adresses IP de client approuvé est stockée dans un paramètre d’application.</span><span class="sxs-lookup"><span data-stu-id="4ca51-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="4ca51-110">L’intergiciel (middleware) ou le filtre analyse la chaîne dans une liste et vérifie si l’adresse IP distante est dans la liste.</span><span class="sxs-lookup"><span data-stu-id="4ca51-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="4ca51-111">Si ce n’est pas le cas, un code d’état HTTP 403 Interdit retourné.</span><span class="sxs-lookup"><span data-stu-id="4ca51-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="4ca51-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ca51-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="4ca51-113">Les listes fiables</span><span class="sxs-lookup"><span data-stu-id="4ca51-113">The safelist</span></span>

<span data-ttu-id="4ca51-114">La liste est configurée dans le *appsettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="4ca51-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="4ca51-115">Il est une liste délimitée par des points-virgules et peut contenir des adresses IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="4ca51-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="4ca51-116">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="4ca51-116">Middleware</span></span>

<span data-ttu-id="4ca51-117">Le `Configure` méthode ajoute l’intergiciel (middleware) et lui passe la chaîne de listes fiables dans un paramètre de constructeur.</span><span class="sxs-lookup"><span data-stu-id="4ca51-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="4ca51-118">L’intergiciel (middleware) analyse la chaîne en un tableau et recherche de l’adresse IP distante dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="4ca51-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="4ca51-119">Si l’adresse IP distante n’est pas trouvée, le middleware renvoie HTTP 401 interdit.</span><span class="sxs-lookup"><span data-stu-id="4ca51-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="4ca51-120">Ce processus de validation est ignoré pour les requêtes HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="4ca51-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="4ca51-121">Filtre d’action</span><span class="sxs-lookup"><span data-stu-id="4ca51-121">Action filter</span></span>

<span data-ttu-id="4ca51-122">Si vous souhaitez un safelist uniquement pour les contrôleurs spécifiques ou des méthodes d’action, utilisez un filtre d’action.</span><span class="sxs-lookup"><span data-stu-id="4ca51-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="4ca51-123">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="4ca51-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="4ca51-124">Le filtre d’action est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="4ca51-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="4ca51-125">Le filtre peut ensuite être utilisé sur une méthode de contrôleur ou d’action.</span><span class="sxs-lookup"><span data-stu-id="4ca51-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="4ca51-126">Dans l’exemple d’application, le filtre est appliqué à la `Get` (méthode).</span><span class="sxs-lookup"><span data-stu-id="4ca51-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="4ca51-127">Par conséquent, lorsque vous testez l’application en envoyant un `Get` demande d’API, l’attribut est validation de l’adresse IP du client.</span><span class="sxs-lookup"><span data-stu-id="4ca51-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="4ca51-128">Lorsque vous testez en appelant l’API avec toute autre méthode HTTP, le middleware valide l’adresse IP du client.</span><span class="sxs-lookup"><span data-stu-id="4ca51-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="4ca51-129">Filtrer les Pages Razor</span><span class="sxs-lookup"><span data-stu-id="4ca51-129">Razor Pages filter</span></span> 

<span data-ttu-id="4ca51-130">Si vous souhaitez une listes fiables pour une application Pages Razor, utilisez un filtre de Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="4ca51-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="4ca51-131">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="4ca51-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="4ca51-132">Ce filtre est activé en l’ajoutant à la collection de filtres MVC.</span><span class="sxs-lookup"><span data-stu-id="4ca51-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="4ca51-133">Lorsque vous exécutez l’application et demandez une page Razor, le filtre de Pages Razor est validation de l’adresse IP du client.</span><span class="sxs-lookup"><span data-stu-id="4ca51-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ca51-134">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="4ca51-134">Next steps</span></span>

<span data-ttu-id="4ca51-135">[En savoir plus sur ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="4ca51-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
