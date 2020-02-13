---
title: Utiliser gRPC dans les applications de navigateur
author: jamesnk
description: Découvrez comment configurer les services gRPC sur ASP.NET Core à appeler à partir d’applications de navigateur à l’aide de gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/10/2020
uid: grpc/browser
ms.openlocfilehash: 333fc8c4277bbac47042d4904c276e963186914a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172278"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="cde17-103">Utiliser gRPC dans les applications de navigateur</span><span class="sxs-lookup"><span data-stu-id="cde17-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="cde17-104">Par [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="cde17-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cde17-105">**gRPC-la prise en charge de Web dans .NET est expérimentale**</span><span class="sxs-lookup"><span data-stu-id="cde17-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="cde17-106">gRPC-Web pour .NET est un projet expérimental, et non un produit validé.</span><span class="sxs-lookup"><span data-stu-id="cde17-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="cde17-107">Nous voulons :</span><span class="sxs-lookup"><span data-stu-id="cde17-107">We want to:</span></span>
>
> * <span data-ttu-id="cde17-108">Testez que notre approche de l’implémentation de gRPC-Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="cde17-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="cde17-109">Recevez des commentaires sur si cette approche est utile pour les développeurs .NET par rapport à la méthode traditionnelle de configuration de gRPC-Web via un proxy.</span><span class="sxs-lookup"><span data-stu-id="cde17-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="cde17-110">N’hésitez pas à nous faire part de vos commentaires au [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) pour nous assurer que nous créons des éléments dont les développeurs aiment et sont productifs.</span><span class="sxs-lookup"><span data-stu-id="cde17-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="cde17-111">Il n’est pas possible d’appeler un service gRPC HTTP/2 à partir d’une application basée sur un navigateur.</span><span class="sxs-lookup"><span data-stu-id="cde17-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="cde17-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) est un protocole qui permet aux applications JavaScript et éblouissantes de navigateur d’appeler des services gRPC.</span><span class="sxs-lookup"><span data-stu-id="cde17-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="cde17-113">Cet article explique comment utiliser gRPC-Web dans .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cde17-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="cde17-114">Configurer gRPC-Web dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cde17-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="cde17-115">les services gRPC hébergés dans ASP.NET Core peuvent être configurés pour prendre en charge gRPC-Web parallèlement à HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="cde17-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="cde17-116">gRPC-Web ne nécessite aucune modification des services.</span><span class="sxs-lookup"><span data-stu-id="cde17-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="cde17-117">La seule modification concerne la configuration du démarrage.</span><span class="sxs-lookup"><span data-stu-id="cde17-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="cde17-118">Pour activer gRPC-Web avec un service ASP.NET Core gRPC :</span><span class="sxs-lookup"><span data-stu-id="cde17-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="cde17-119">Ajoutez une référence au package [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="cde17-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="cde17-120">Configurez l’application pour utiliser gRPC-Web en ajoutant `AddGrpcWeb` et `UseGrpcWeb` à *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cde17-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="cde17-121">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="cde17-121">The preceding code:</span></span>

* <span data-ttu-id="cde17-122">Ajoute l’intergiciel gRPC-Web, `UseGrpcWeb`, après le routage et avant les points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="cde17-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="cde17-123">Spécifie que la méthode `endpoints.MapGrpcService<GreeterService>()` prend en charge gRPC-Web avec `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="cde17-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="cde17-124">Vous pouvez également configurer tous les services pour prendre en charge gRPC-Web en ajoutant des `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` à ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="cde17-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

<span data-ttu-id="cde17-125">Une configuration supplémentaire peut être nécessaire pour appeler gRPC-Web à partir du navigateur, par exemple la configuration de ASP.NET Core pour prendre en charge CORS.</span><span class="sxs-lookup"><span data-stu-id="cde17-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="cde17-126">Pour plus d’informations, consultez [prise en charge de cors](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="cde17-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="cde17-127">Appeler gRPC-Web à partir du navigateur</span><span class="sxs-lookup"><span data-stu-id="cde17-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="cde17-128">Les applications de navigateur peuvent utiliser gRPC-Web pour appeler des services gRPC.</span><span class="sxs-lookup"><span data-stu-id="cde17-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="cde17-129">Il existe certaines exigences et limitations lors de l’appel de services gRPC avec gRPC-Web à partir du navigateur :</span><span class="sxs-lookup"><span data-stu-id="cde17-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="cde17-130">Le serveur doit avoir été configuré pour prendre en charge gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="cde17-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="cde17-131">Les appels de diffusion en continu et bidirectionnelle du client ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="cde17-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="cde17-132">La diffusion en continu du serveur est prise en charge.</span><span class="sxs-lookup"><span data-stu-id="cde17-132">Server streaming is supported.</span></span>
* <span data-ttu-id="cde17-133">L’appel de services gRPC sur un autre domaine requiert la configuration de [cors](xref:security/cors) sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="cde17-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="cde17-134">JavaScript gRPC-client Web</span><span class="sxs-lookup"><span data-stu-id="cde17-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="cde17-135">Il existe un client JavaScript gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="cde17-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="cde17-136">Pour obtenir des instructions sur l’utilisation de gRPC-Web à partir de JavaScript, consultez [écrire du code JavaScript client avec gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="cde17-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="cde17-137">Configurer gRPC-Web avec le client .NET gRPC</span><span class="sxs-lookup"><span data-stu-id="cde17-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="cde17-138">Le client .NET gRPC peut être configuré pour effectuer des appels gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="cde17-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="cde17-139">Cela est utile pour les applications de [Webassembly éblouissantes](xref:blazor/index#blazor-webassembly) , qui sont hébergées dans le navigateur et qui ont les mêmes limitations http du code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cde17-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="cde17-140">L’appel de gRPC-Web avec un client .NET est [identique à http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="cde17-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="cde17-141">La seule modification est la manière dont le canal est créé.</span><span class="sxs-lookup"><span data-stu-id="cde17-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="cde17-142">Pour utiliser gRPC-Web :</span><span class="sxs-lookup"><span data-stu-id="cde17-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="cde17-143">Ajoutez une référence au package [GRPC .net. client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="cde17-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="cde17-144">Assurez-vous que la référence au package [GRPC .net. client](https://www.nuget.org/packages/Grpc.Net.Client) est 2.27.0 ou supérieure.</span><span class="sxs-lookup"><span data-stu-id="cde17-144">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="cde17-145">Configurez le canal pour utiliser le `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="cde17-145">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="cde17-146">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="cde17-146">The preceding code:</span></span>

* <span data-ttu-id="cde17-147">Configure un canal pour utiliser gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="cde17-147">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="cde17-148">Crée un client et effectue un appel à l’aide du canal.</span><span class="sxs-lookup"><span data-stu-id="cde17-148">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="cde17-149">La `GrpcWebHandler` possède les options de configuration suivantes au moment de sa création :</span><span class="sxs-lookup"><span data-stu-id="cde17-149">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="cde17-150">**InnerHandler**: <xref:System.Net.Http.HttpMessageHandler> sous-jacente qui effectue la requête http gRPC, par exemple, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="cde17-150">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="cde17-151">**Mode**: type d’énumération qui spécifie si la demande de demande HTTP gRPC `Content-Type` est `application/grpc-web` ou `application/grpc-web-text`.</span><span class="sxs-lookup"><span data-stu-id="cde17-151">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="cde17-152">`GrpcWebMode.GrpcWeb` configure le contenu à envoyer sans encodage.</span><span class="sxs-lookup"><span data-stu-id="cde17-152">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="cde17-153">Valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="cde17-153">Default value.</span></span>
    * <span data-ttu-id="cde17-154">`GrpcWebMode.GrpcWebText` configure le contenu pour qu’il soit encodé en base64.</span><span class="sxs-lookup"><span data-stu-id="cde17-154">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="cde17-155">Requis pour les appels de diffusion en continu du serveur dans les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="cde17-155">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="cde17-156">**HttpVersion**: le protocole http `Version` utilisé pour définir [HttpRequestMessage. version](xref:System.Net.Http.HttpRequestMessage.Version) sur la requête http gRPC sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cde17-156">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="cde17-157">gRPC-Web n’a pas besoin d’une version spécifique et ne remplace pas la valeur par défaut, sauf indication contraire.</span><span class="sxs-lookup"><span data-stu-id="cde17-157">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cde17-158">Les clients gRPC générés ont des méthodes synchrones et asynchrones pour appeler des méthodes unaires.</span><span class="sxs-lookup"><span data-stu-id="cde17-158">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="cde17-159">Par exemple, `SayHello` est Sync et `SayHelloAsync` est Async.</span><span class="sxs-lookup"><span data-stu-id="cde17-159">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="cde17-160">L’appel d’une méthode Sync dans une application de webassembly éblouissante entraîne le blocage de l’application.</span><span class="sxs-lookup"><span data-stu-id="cde17-160">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="cde17-161">Les méthodes Async doivent toujours être utilisées dans le webassembly éblouissant.</span><span class="sxs-lookup"><span data-stu-id="cde17-161">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cde17-162">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cde17-162">Additional resources</span></span>

* [<span data-ttu-id="cde17-163">gRPC pour le projet GitHub des clients Web</span><span class="sxs-lookup"><span data-stu-id="cde17-163">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
