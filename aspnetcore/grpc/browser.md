---
title: Utiliser gRPC dans les applications de navigateur
author: jamesnk
description: Découvrez comment configurer les services gRPC sur ASP.NET Core à appeler à partir d’applications de navigateur à l’aide de gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/16/2020
uid: grpc/browser
ms.openlocfilehash: 3beeffc26ffd3c2dc85bfc22a46d97d5fd78d3d0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664198"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="386b9-103">Utiliser gRPC dans les applications de navigateur</span><span class="sxs-lookup"><span data-stu-id="386b9-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="386b9-104">Par [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="386b9-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="386b9-105">**gRPC-la prise en charge de Web dans .NET est expérimentale**</span><span class="sxs-lookup"><span data-stu-id="386b9-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="386b9-106">gRPC-Web pour .NET est un projet expérimental, et non un produit validé.</span><span class="sxs-lookup"><span data-stu-id="386b9-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="386b9-107">Nous voulons :</span><span class="sxs-lookup"><span data-stu-id="386b9-107">We want to:</span></span>
>
> * <span data-ttu-id="386b9-108">Testez que notre approche de l’implémentation de gRPC-Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="386b9-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="386b9-109">Recevez des commentaires sur si cette approche est utile pour les développeurs .NET par rapport à la méthode traditionnelle de configuration de gRPC-Web via un proxy.</span><span class="sxs-lookup"><span data-stu-id="386b9-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="386b9-110">N’hésitez pas à nous faire part de vos commentaires au [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) pour nous assurer que nous créons des éléments dont les développeurs aiment et sont productifs.</span><span class="sxs-lookup"><span data-stu-id="386b9-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="386b9-111">Il n’est pas possible d’appeler un service gRPC HTTP/2 à partir d’une application basée sur un navigateur.</span><span class="sxs-lookup"><span data-stu-id="386b9-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="386b9-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) est un protocole qui permet aux applications JavaScript et éblouissantes de navigateur d’appeler des services gRPC.</span><span class="sxs-lookup"><span data-stu-id="386b9-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="386b9-113">Cet article explique comment utiliser gRPC-Web dans .NET Core.</span><span class="sxs-lookup"><span data-stu-id="386b9-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="386b9-114">Configurer gRPC-Web dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="386b9-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="386b9-115">les services gRPC hébergés dans ASP.NET Core peuvent être configurés pour prendre en charge gRPC-Web parallèlement à HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="386b9-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="386b9-116">gRPC-Web ne nécessite aucune modification des services.</span><span class="sxs-lookup"><span data-stu-id="386b9-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="386b9-117">La seule modification concerne la configuration du démarrage.</span><span class="sxs-lookup"><span data-stu-id="386b9-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="386b9-118">Pour activer gRPC-Web avec un service ASP.NET Core gRPC :</span><span class="sxs-lookup"><span data-stu-id="386b9-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="386b9-119">Ajoutez une référence au package [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="386b9-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="386b9-120">Configurez l’application pour utiliser gRPC-Web en ajoutant `AddGrpcWeb` et `UseGrpcWeb` à *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="386b9-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="386b9-121">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="386b9-121">The preceding code:</span></span>

* <span data-ttu-id="386b9-122">Ajoute l’intergiciel gRPC-Web, `UseGrpcWeb`, après le routage et avant les points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="386b9-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="386b9-123">Spécifie que la méthode `endpoints.MapGrpcService<GreeterService>()` prend en charge gRPC-Web avec `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="386b9-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="386b9-124">Vous pouvez également configurer tous les services pour prendre en charge gRPC-Web en ajoutant des `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` à ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="386b9-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

### <a name="grpc-web-and-cors"></a><span data-ttu-id="386b9-125">gRPC-Web et CORS</span><span class="sxs-lookup"><span data-stu-id="386b9-125">gRPC-Web and CORS</span></span>

<span data-ttu-id="386b9-126">La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web.</span><span class="sxs-lookup"><span data-stu-id="386b9-126">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="386b9-127">Cette restriction s’applique à la création d’appels gRPC-Web avec des applications de navigateur.</span><span class="sxs-lookup"><span data-stu-id="386b9-127">This restriction applies to making gRPC-Web calls with browser apps.</span></span> <span data-ttu-id="386b9-128">Par exemple, une application de navigateur servie par `https://www.contoso.com` est bloquée lors de l’appel de gRPC-services Web hébergés sur `https://services.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="386b9-128">For example, a browser app served by `https://www.contoso.com` is blocked from calling gRPC-Web services hosted on `https://services.contoso.com`.</span></span> <span data-ttu-id="386b9-129">Le partage des ressources Cross-Origin (CORS) peut être utilisé pour assouplir cette restriction.</span><span class="sxs-lookup"><span data-stu-id="386b9-129">Cross Origin Resource Sharing (CORS) can be used to relax this restriction.</span></span>

<span data-ttu-id="386b9-130">Pour permettre à votre application de navigateur d’effectuer des appels gRPC-Web Cross-Origin, configurez [cors dans ASP.net Core](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="386b9-130">To allow your browser app to make cross-origin gRPC-Web calls, set up [CORS in ASP.NET Core](xref:security/cors).</span></span> <span data-ttu-id="386b9-131">Utilisez la prise en charge de CORS intégrée et exposez des en-têtes spécifiques à gRPC avec <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="386b9-131">Use the built-in CORS support, and expose gRPC-specific headers with <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span></span>

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

<span data-ttu-id="386b9-132">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="386b9-132">The preceding code:</span></span>

* <span data-ttu-id="386b9-133">Appelle `AddCors` pour ajouter des services CORS et configure une stratégie CORS qui expose des en-têtes spécifiques à gRPC.</span><span class="sxs-lookup"><span data-stu-id="386b9-133">Calls `AddCors` to add CORS services and configures a CORS policy that exposes gRPC-specific headers.</span></span>
* <span data-ttu-id="386b9-134">Appelle `UseCors` pour ajouter l’intergiciel (middleware) CORS après le routage et avant les points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="386b9-134">Calls `UseCors` to add the CORS middleware after routing and before endpoints.</span></span>
* <span data-ttu-id="386b9-135">Spécifie que la méthode `endpoints.MapGrpcService<GreeterService>()` prend en charge CORS avec `RequiresCors`.</span><span class="sxs-lookup"><span data-stu-id="386b9-135">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports CORS with `RequiresCors`.</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="386b9-136">Appeler gRPC-Web à partir du navigateur</span><span class="sxs-lookup"><span data-stu-id="386b9-136">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="386b9-137">Les applications de navigateur peuvent utiliser gRPC-Web pour appeler des services gRPC.</span><span class="sxs-lookup"><span data-stu-id="386b9-137">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="386b9-138">Il existe certaines exigences et limitations lors de l’appel de services gRPC avec gRPC-Web à partir du navigateur :</span><span class="sxs-lookup"><span data-stu-id="386b9-138">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="386b9-139">Le serveur doit avoir été configuré pour prendre en charge gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="386b9-139">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="386b9-140">Les appels de diffusion en continu et bidirectionnelle du client ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="386b9-140">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="386b9-141">La diffusion en continu du serveur est prise en charge.</span><span class="sxs-lookup"><span data-stu-id="386b9-141">Server streaming is supported.</span></span>
* <span data-ttu-id="386b9-142">L’appel de services gRPC sur un autre domaine requiert la configuration de [cors](xref:security/cors) sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="386b9-142">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="386b9-143">JavaScript gRPC-client Web</span><span class="sxs-lookup"><span data-stu-id="386b9-143">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="386b9-144">Il existe un client JavaScript gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="386b9-144">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="386b9-145">Pour obtenir des instructions sur l’utilisation de gRPC-Web à partir de JavaScript, consultez [écrire du code JavaScript client avec gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="386b9-145">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="386b9-146">Configurer gRPC-Web avec le client .NET gRPC</span><span class="sxs-lookup"><span data-stu-id="386b9-146">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="386b9-147">Le client .NET gRPC peut être configuré pour effectuer des appels gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="386b9-147">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="386b9-148">Cela est utile pour les applications de [Webassembly éblouissantes](xref:blazor/index#blazor-webassembly) , qui sont hébergées dans le navigateur et qui ont les mêmes limitations http du code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="386b9-148">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="386b9-149">L’appel de gRPC-Web avec un client .NET est [identique à http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="386b9-149">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="386b9-150">La seule modification est la manière dont le canal est créé.</span><span class="sxs-lookup"><span data-stu-id="386b9-150">The only modification is how the channel is created.</span></span>

<span data-ttu-id="386b9-151">Pour utiliser gRPC-Web :</span><span class="sxs-lookup"><span data-stu-id="386b9-151">To use gRPC-Web:</span></span>

* <span data-ttu-id="386b9-152">Ajoutez une référence au package [GRPC .net. client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="386b9-152">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="386b9-153">Assurez-vous que la référence au package [GRPC .net. client](https://www.nuget.org/packages/Grpc.Net.Client) est 2.27.0 ou supérieure.</span><span class="sxs-lookup"><span data-stu-id="386b9-153">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="386b9-154">Configurez le canal pour utiliser le `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="386b9-154">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="386b9-155">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="386b9-155">The preceding code:</span></span>

* <span data-ttu-id="386b9-156">Configure un canal pour utiliser gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="386b9-156">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="386b9-157">Crée un client et effectue un appel à l’aide du canal.</span><span class="sxs-lookup"><span data-stu-id="386b9-157">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="386b9-158">La `GrpcWebHandler` possède les options de configuration suivantes au moment de sa création :</span><span class="sxs-lookup"><span data-stu-id="386b9-158">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="386b9-159">**InnerHandler**: <xref:System.Net.Http.HttpMessageHandler> sous-jacente qui effectue la requête http gRPC, par exemple, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="386b9-159">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="386b9-160">**Mode**: type d’énumération qui spécifie si la demande de demande HTTP gRPC `Content-Type` est `application/grpc-web` ou `application/grpc-web-text`.</span><span class="sxs-lookup"><span data-stu-id="386b9-160">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="386b9-161">`GrpcWebMode.GrpcWeb` configure le contenu à envoyer sans encodage.</span><span class="sxs-lookup"><span data-stu-id="386b9-161">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="386b9-162">Valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="386b9-162">Default value.</span></span>
    * <span data-ttu-id="386b9-163">`GrpcWebMode.GrpcWebText` configure le contenu pour qu’il soit encodé en base64.</span><span class="sxs-lookup"><span data-stu-id="386b9-163">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="386b9-164">Requis pour les appels de diffusion en continu du serveur dans les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="386b9-164">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="386b9-165">**HttpVersion**: le protocole http `Version` utilisé pour définir [HttpRequestMessage. version](xref:System.Net.Http.HttpRequestMessage.Version) sur la requête http gRPC sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="386b9-165">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="386b9-166">gRPC-Web n’a pas besoin d’une version spécifique et ne remplace pas la valeur par défaut, sauf indication contraire.</span><span class="sxs-lookup"><span data-stu-id="386b9-166">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="386b9-167">Les clients gRPC générés ont des méthodes synchrones et asynchrones pour appeler des méthodes unaires.</span><span class="sxs-lookup"><span data-stu-id="386b9-167">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="386b9-168">Par exemple, `SayHello` est Sync et `SayHelloAsync` est Async.</span><span class="sxs-lookup"><span data-stu-id="386b9-168">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="386b9-169">L’appel d’une méthode Sync dans une application de webassembly éblouissante entraîne le blocage de l’application.</span><span class="sxs-lookup"><span data-stu-id="386b9-169">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="386b9-170">Les méthodes Async doivent toujours être utilisées dans le webassembly éblouissant.</span><span class="sxs-lookup"><span data-stu-id="386b9-170">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="386b9-171">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="386b9-171">Additional resources</span></span>

* [<span data-ttu-id="386b9-172">gRPC pour le projet GitHub des clients Web</span><span class="sxs-lookup"><span data-stu-id="386b9-172">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
