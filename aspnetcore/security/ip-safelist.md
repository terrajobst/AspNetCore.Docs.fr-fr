---
title: Client IP safelier pour ASP.NET Core
author: damienbod
description: Découvrez comment écrire des filtres d’intergiciel ou d’action pour valider des adresses IP distantes par rapport à une liste d’adresses IP approuvées.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: d25c375f7e659168ab8cc9d8e11753cb7dfde831
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659774"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="fb783-103">Client IP safelier pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb783-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="fb783-104">Par [Damien Bowden](https://twitter.com/damien_bod) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fb783-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="fb783-105">Cet article montre trois façons d’implémenter un safelist IP (également appelé liste verte) dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb783-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="fb783-106">Vous pouvez utiliser :</span><span class="sxs-lookup"><span data-stu-id="fb783-106">You can use:</span></span>

* <span data-ttu-id="fb783-107">Intergiciel pour vérifier l’adresse IP distante de chaque demande.</span><span class="sxs-lookup"><span data-stu-id="fb783-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="fb783-108">Filtres d’action pour vérifier l’adresse IP distante des demandes pour des contrôleurs ou des méthodes d’action spécifiques.</span><span class="sxs-lookup"><span data-stu-id="fb783-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="fb783-109">Razor Pages filtres pour vérifier l’adresse IP distante des requêtes pour les pages Razor.</span><span class="sxs-lookup"><span data-stu-id="fb783-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="fb783-110">Dans chaque cas, une chaîne contenant des adresses IP clientes approuvées est stockée dans un paramètre d’application.</span><span class="sxs-lookup"><span data-stu-id="fb783-110">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="fb783-111">L’intergiciel (middleware) ou le filtre analyse la chaîne dans une liste et vérifie si l’adresse IP distante figure dans la liste.</span><span class="sxs-lookup"><span data-stu-id="fb783-111">The middleware or filter parses the string into a list and checks if the remote IP is in the list.</span></span> <span data-ttu-id="fb783-112">Si ce n’est pas le cas, un code d’état HTTP 403 interdit est retourné.</span><span class="sxs-lookup"><span data-stu-id="fb783-112">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="fb783-113">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb783-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="fb783-114">Safelit</span><span class="sxs-lookup"><span data-stu-id="fb783-114">The safelist</span></span>

<span data-ttu-id="fb783-115">La liste est configurée dans le fichier *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="fb783-115">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="fb783-116">Il s’agit d’une liste délimitée par des points-virgules qui peut contenir des adresses IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="fb783-116">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="fb783-117">Middlewares</span><span class="sxs-lookup"><span data-stu-id="fb783-117">Middleware</span></span>

<span data-ttu-id="fb783-118">La méthode `Configure` ajoute l’intergiciel (middleware) et lui transmet la chaîne safeli dans un paramètre de constructeur.</span><span class="sxs-lookup"><span data-stu-id="fb783-118">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="fb783-119">L’intergiciel analyse la chaîne dans un tableau et recherche l’adresse IP distante dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="fb783-119">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="fb783-120">Si l’adresse IP distante est introuvable, l’intergiciel (middleware) renvoie HTTP 401 interdit.</span><span class="sxs-lookup"><span data-stu-id="fb783-120">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="fb783-121">Ce processus de validation est contourné pour les requêtes HTTP d’extraction.</span><span class="sxs-lookup"><span data-stu-id="fb783-121">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="fb783-122">Filtre d’action</span><span class="sxs-lookup"><span data-stu-id="fb783-122">Action filter</span></span>

<span data-ttu-id="fb783-123">Si vous souhaitez un safelir uniquement pour des contrôleurs ou des méthodes d’action spécifiques, utilisez un filtre d’action.</span><span class="sxs-lookup"><span data-stu-id="fb783-123">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="fb783-124">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="fb783-124">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckFilter.cs)]

<span data-ttu-id="fb783-125">Le filtre d’action est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="fb783-125">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="fb783-126">Le filtre peut ensuite être utilisé sur un contrôleur ou une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="fb783-126">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="fb783-127">Dans l’exemple d’application, le filtre est appliqué à la méthode `Get`.</span><span class="sxs-lookup"><span data-stu-id="fb783-127">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="fb783-128">Ainsi, lorsque vous testez l’application en envoyant une demande d’API `Get`, l’attribut valide l’adresse IP du client.</span><span class="sxs-lookup"><span data-stu-id="fb783-128">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="fb783-129">Lorsque vous testez en appelant l’API avec une autre méthode HTTP, l’intergiciel (middleware) valide l’adresse IP du client.</span><span class="sxs-lookup"><span data-stu-id="fb783-129">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="fb783-130">Filtre de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="fb783-130">Razor Pages filter</span></span> 

<span data-ttu-id="fb783-131">Si vous souhaitez obtenir une application Razor Pages, utilisez un filtre Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fb783-131">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="fb783-132">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="fb783-132">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckPageFilter.cs)]

<span data-ttu-id="fb783-133">Ce filtre est activé en l’ajoutant à la collection de filtres MVC.</span><span class="sxs-lookup"><span data-stu-id="fb783-133">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="fb783-134">Lorsque vous exécutez l’application et demandez une page Razor, le filtre de Razor Pages valide l’adresse IP du client.</span><span class="sxs-lookup"><span data-stu-id="fb783-134">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb783-135">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fb783-135">Next steps</span></span>

<span data-ttu-id="fb783-136">[En savoir plus sur les intergiciels (middleware) ASP.net Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="fb783-136">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
