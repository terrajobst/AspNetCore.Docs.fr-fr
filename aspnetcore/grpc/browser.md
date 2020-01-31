---
title: gRPC dans les applications de navigateur
author: jamesnk
description: Découvrez comment configurer les services gRPC sur ASP.NET Core à appeler à partir d’applications de navigateur à l’aide de gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830659"
---
# <a name="grpc-in-browser-apps"></a><span data-ttu-id="228e0-103">gRPC dans les applications de navigateur</span><span class="sxs-lookup"><span data-stu-id="228e0-103">gRPC in browser apps</span></span>

<span data-ttu-id="228e0-104">Par [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="228e0-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="228e0-105">**gRPC-la prise en charge de Web dans .NET est expérimentale**</span><span class="sxs-lookup"><span data-stu-id="228e0-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="228e0-106">gRPC-Web pour .NET est un projet expérimental, et non un produit validé.</span><span class="sxs-lookup"><span data-stu-id="228e0-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="228e0-107">Nous voulons :</span><span class="sxs-lookup"><span data-stu-id="228e0-107">We want to:</span></span>
>
> * <span data-ttu-id="228e0-108">Testez que notre approche de l’implémentation de gRPC-Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="228e0-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="228e0-109">Recevez des commentaires sur si cette approche est utile pour les développeurs .NET par rapport à la méthode traditionnelle de configuration de gRPC-Web via un proxy.</span><span class="sxs-lookup"><span data-stu-id="228e0-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="228e0-110">N’hésitez pas à nous faire part de vos commentaires au [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) pour nous assurer que nous créons des éléments dont les développeurs aiment et sont productifs.</span><span class="sxs-lookup"><span data-stu-id="228e0-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="228e0-111">Il n’est pas possible d’appeler un service gRPC HTTP/2 à partir d’une application basée sur un navigateur.</span><span class="sxs-lookup"><span data-stu-id="228e0-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="228e0-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) est un protocole qui permet aux applications JavaScript et éblouissantes de navigateur d’appeler des services gRPC.</span><span class="sxs-lookup"><span data-stu-id="228e0-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="228e0-113">Cet article explique comment utiliser gRPC-Web dans .NET Core.</span><span class="sxs-lookup"><span data-stu-id="228e0-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="228e0-114">Configurer gRPC-Web dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="228e0-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="228e0-115">les services gRPC hébergés dans ASP.NET Core peuvent être configurés pour prendre en charge gRPC-Web parallèlement à HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="228e0-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="228e0-116">gRPC-Web ne nécessite aucune modification des services.</span><span class="sxs-lookup"><span data-stu-id="228e0-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="228e0-117">La seule modification concerne la configuration du démarrage.</span><span class="sxs-lookup"><span data-stu-id="228e0-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="228e0-118">Pour activer gRPC-Web avec un service ASP.NET Core gRPC :</span><span class="sxs-lookup"><span data-stu-id="228e0-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="228e0-119">Ajoutez une référence au package [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="228e0-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="228e0-120">Configurez l’application pour utiliser gRPC-Web en ajoutant `AddGrpcWeb` et `UseGrpcWeb` à *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="228e0-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

<span data-ttu-id="228e0-121">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="228e0-121">The preceding code:</span></span>

* <span data-ttu-id="228e0-122">Ajoute l’intergiciel gRPC-Web, `UseGrpcWeb`, après le routage et avant les points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="228e0-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="228e0-123">Spécifie que la méthode `endpoints.MapGrpcService<GreeterService>()` prend en charge gRPC-Web avec `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="228e0-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="228e0-124">Vous pouvez également configurer tous les services pour prendre en charge gRPC-Web en ajoutant des `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` à ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="228e0-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

<span data-ttu-id="228e0-125">Une configuration supplémentaire peut être nécessaire pour appeler gRPC-Web à partir du navigateur, par exemple la configuration de ASP.NET Core pour prendre en charge CORS.</span><span class="sxs-lookup"><span data-stu-id="228e0-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="228e0-126">Pour plus d’informations, consultez [prise en charge de cors](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="228e0-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="228e0-127">Appeler gRPC-Web à partir du navigateur</span><span class="sxs-lookup"><span data-stu-id="228e0-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="228e0-128">Les applications de navigateur peuvent utiliser gRPC-Web pour appeler des services gRPC.</span><span class="sxs-lookup"><span data-stu-id="228e0-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="228e0-129">Il existe certaines exigences et limitations lors de l’appel de services gRPC avec gRPC-Web à partir du navigateur :</span><span class="sxs-lookup"><span data-stu-id="228e0-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="228e0-130">Le serveur doit avoir été configuré pour prendre en charge gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="228e0-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="228e0-131">Les appels de diffusion en continu et bidirectionnelle du client ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="228e0-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="228e0-132">La diffusion en continu du serveur est prise en charge.</span><span class="sxs-lookup"><span data-stu-id="228e0-132">Server streaming is supported.</span></span>
* <span data-ttu-id="228e0-133">L’appel de services gRPC sur un autre domaine requiert la configuration de [cors](xref:security/cors) sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="228e0-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="228e0-134">JavaScript gRPC-client Web</span><span class="sxs-lookup"><span data-stu-id="228e0-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="228e0-135">Il existe un client JavaScript gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="228e0-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="228e0-136">Pour obtenir des instructions sur l’utilisation de gRPC-Web à partir de JavaScript, consultez [écrire du code JavaScript client avec gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="228e0-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="228e0-137">Configurer gRPC-Web avec le client .NET gRPC</span><span class="sxs-lookup"><span data-stu-id="228e0-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="228e0-138">Le client .NET gRPC peut être configuré pour effectuer des appels gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="228e0-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="228e0-139">Cela est utile pour les applications de [Webassembly éblouissantes](xref:blazor/index#blazor-webassembly) , qui sont hébergées dans le navigateur et qui ont les mêmes limitations http du code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="228e0-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="228e0-140">L’appel de gRPC-Web avec un client .NET est [identique à http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="228e0-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="228e0-141">La seule modification est la manière dont le canal est créé.</span><span class="sxs-lookup"><span data-stu-id="228e0-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="228e0-142">Pour utiliser gRPC-Web :</span><span class="sxs-lookup"><span data-stu-id="228e0-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="228e0-143">Ajoutez une référence au package [GRPC .net. client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="228e0-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="228e0-144">Configurez le canal pour utiliser le `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="228e0-144">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="228e0-145">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="228e0-145">The preceding code:</span></span>

* <span data-ttu-id="228e0-146">Configure un canal pour utiliser gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="228e0-146">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="228e0-147">Crée un client et effectue un appel à l’aide du canal.</span><span class="sxs-lookup"><span data-stu-id="228e0-147">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="228e0-148">La `GrpcWebHandler` possède les options de configuration suivantes au moment de sa création :</span><span class="sxs-lookup"><span data-stu-id="228e0-148">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="228e0-149">**InnerHandler**: <xref:System.Net.Http.HttpMessageHandler> sous-jacent qui effectue l’appel http, par exemple, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="228e0-149">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the HTTP call, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="228e0-150">**Mode**: `GrpcWebMode` Enum.</span><span class="sxs-lookup"><span data-stu-id="228e0-150">**Mode**: `GrpcWebMode` enum.</span></span> <span data-ttu-id="228e0-151">`GrpcWebMode.GrpcWebText` configure le contenu pour qu’il soit encodé en base64, ce qui est nécessaire pour prendre en charge les appels de diffusion en continu du serveur.</span><span class="sxs-lookup"><span data-stu-id="228e0-151">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded, which is required to support server streaming calls.</span></span>
* <span data-ttu-id="228e0-152">**HttpVersion**: `Version`de protocole http.</span><span class="sxs-lookup"><span data-stu-id="228e0-152">**HttpVersion**: HTTP protocol `Version`.</span></span> <span data-ttu-id="228e0-153">gRPC-Web ne nécessite pas de protocole spécifique et n’en spécifie pas pour effectuer une demande, sauf si elle est configurée.</span><span class="sxs-lookup"><span data-stu-id="228e0-153">gRPC-Web doesn't require a specific protocol and won't specify one when making a request unless configured.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="228e0-154">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="228e0-154">Additional resources</span></span>

* [<span data-ttu-id="228e0-155">gRPC pour le projet GitHub des clients Web</span><span class="sxs-lookup"><span data-stu-id="228e0-155">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
