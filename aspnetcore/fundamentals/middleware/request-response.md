---
title: Opérations de demande et de réponse dans ASP.NET Core
author: jkotalik
description: Découvrez comment lire le corps de demande et écrire le corps de la réponse dans ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jkotalik
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b6e3cd4b79e0c062b271c65cd5ecbdb4ef80c3a1
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208055"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="05d0b-103">Opérations de demande et de réponse dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05d0b-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="05d0b-104">Par [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="05d0b-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="05d0b-105">Cet article explique comment lire à partir du corps de la demande et écrire dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="05d0b-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="05d0b-106">Vous devrez peut-être écrire du code pour ces opérations lorsque vous écrivez le middleware.</span><span class="sxs-lookup"><span data-stu-id="05d0b-106">You might need to write code for these operations when you're writing middleware.</span></span> <span data-ttu-id="05d0b-107">Sinon, il est en général inutile d’écrire ce code, car les opérations sont gérées par MVC et Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05d0b-107">Otherwise, you typically don't have to write this code because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="05d0b-108">Dans ASP.NET Core 3.0, il existe deux abstractions pour les corps de la demande et de la réponse : <xref:System.IO.Stream> et <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="05d0b-108">In ASP.NET Core 3.0, there are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="05d0b-109">Pour la lecture de la demande, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) est un <xref:System.IO.Stream>, et `HttpRequest.BodyPipe` est un <xref:System.IO.Pipelines.PipeReader>.</span><span class="sxs-lookup"><span data-stu-id="05d0b-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyPipe` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="05d0b-110">Pour l’écriture de la réponse, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) est un `HttpResponse.BodyPipe` est un <xref:System.IO.Pipelines.PipeWriter>.</span><span class="sxs-lookup"><span data-stu-id="05d0b-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a `HttpResponse.BodyPipe` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="05d0b-111">Nous recommandons d’utiliser des pipelines plutôt que des flux.</span><span class="sxs-lookup"><span data-stu-id="05d0b-111">We recommend pipelines over streams.</span></span> <span data-ttu-id="05d0b-112">Les flux peuvent être plus faciles à utiliser pour des opérations simples, mais les pipelines présentent un avantage de performances et sont plus faciles à utiliser dans la plupart des scénarios.</span><span class="sxs-lookup"><span data-stu-id="05d0b-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="05d0b-113">Dans 3.0, ASP.NET Core commence à utiliser des pipelines au lieu de flux en interne.</span><span class="sxs-lookup"><span data-stu-id="05d0b-113">In 3.0, ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="05d0b-114">En voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="05d0b-114">Examples include:</span></span>

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

<span data-ttu-id="05d0b-115">Les flux ne sont pas amenée à disparaître.</span><span class="sxs-lookup"><span data-stu-id="05d0b-115">Streams aren't going away.</span></span> <span data-ttu-id="05d0b-116">Ils continuent à être utilisés dans .NET et de nombreux types de flux n’ont d’équivalents en termes de canal, comme `FileStreams` et `ResponseCompression`.</span><span class="sxs-lookup"><span data-stu-id="05d0b-116">They continue to be used throughout .NET, and many stream types don't have pipe equivalents, like `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="05d0b-117">Exemples de flux</span><span class="sxs-lookup"><span data-stu-id="05d0b-117">Stream examples</span></span>

<span data-ttu-id="05d0b-118">Supposons que vous souhaitez créer un middleware qui lit le corps de la demande dans sa totalité comme une liste de chaînes, avec un fractionnement sur les nouvelles lignes.</span><span class="sxs-lookup"><span data-stu-id="05d0b-118">Suppose you want to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="05d0b-119">Une implémentation simple de flux peut se présenter comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="05d0b-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="05d0b-120">Ce code fonctionne, mais il existe certains problèmes :</span><span class="sxs-lookup"><span data-stu-id="05d0b-120">This code works, but there are some issues:</span></span>

- <span data-ttu-id="05d0b-121">Avant d’ajouter à `StringBuilder`, l’exemple crée une autre chaîne (`encodedString`) qui est immédiatement rejetée.</span><span class="sxs-lookup"><span data-stu-id="05d0b-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="05d0b-122">Ce processus se produit pour tous les octets dans le flux, il en résulte une allocation de mémoire supplémentaire de la taille de la totalité du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="05d0b-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
- <span data-ttu-id="05d0b-123">L’exemple lit la chaîne entière avant de fractionner sur les nouvelles lignes.</span><span class="sxs-lookup"><span data-stu-id="05d0b-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="05d0b-124">Il serait plus efficace de rechercher les nouvelles lignes dans le tableau d’octets.</span><span class="sxs-lookup"><span data-stu-id="05d0b-124">It would be more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="05d0b-125">Voici un exemple qui résout certains de ces problèmes :</span><span class="sxs-lookup"><span data-stu-id="05d0b-125">Here's an example that fixes some of these issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="05d0b-126">Cet exemple :</span><span class="sxs-lookup"><span data-stu-id="05d0b-126">This example:</span></span>

- <span data-ttu-id="05d0b-127">Ne met pas en mémoire tampon le corps entier de la demande dans un `StringBuilder` , sauf s’il n’y a pas de caractère de nouvelle ligne.</span><span class="sxs-lookup"><span data-stu-id="05d0b-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
- <span data-ttu-id="05d0b-128">N’appelle pas `Split` sur la chaîne.</span><span class="sxs-lookup"><span data-stu-id="05d0b-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="05d0b-129">Toutefois, il existe toujours quelques problèmes :</span><span class="sxs-lookup"><span data-stu-id="05d0b-129">However, there are still are a few issues:</span></span>

- <span data-ttu-id="05d0b-130">Si les caractères de saut de ligne sont épars, une grande partie du corps de la demande est mis en mémoire tampon dans la chaîne.</span><span class="sxs-lookup"><span data-stu-id="05d0b-130">If newline characters are sparse, much of the request body is buffered in the string .</span></span>
- <span data-ttu-id="05d0b-131">Cela crée toujours des chaînes (`remainingString`) et les ajoute à la mémoire tampon de chaîne, ce qui entraîne une allocation supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="05d0b-131">It still creates strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="05d0b-132">Ces problèmes sont réparables, mais le code est de plus en plus compliqué avec peu d’amélioration.</span><span class="sxs-lookup"><span data-stu-id="05d0b-132">These issues are fixable, but the code is becoming more and more complicated with little improvement.</span></span> <span data-ttu-id="05d0b-133">Les pipelines permettent de résoudre ces problèmes avec un code peu compliqué.</span><span class="sxs-lookup"><span data-stu-id="05d0b-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="05d0b-134">Pipelines</span><span class="sxs-lookup"><span data-stu-id="05d0b-134">Pipelines</span></span>

<span data-ttu-id="05d0b-135">L’exemple suivant indique comment le même scénario peut être géré à l’aide d’un `PipeReader` :</span><span class="sxs-lookup"><span data-stu-id="05d0b-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="05d0b-136">Cet exemple résout de nombreux problèmes trouvés dans les implémentations de flux :</span><span class="sxs-lookup"><span data-stu-id="05d0b-136">This example fixes many issues that the streams implementations had:</span></span>

- <span data-ttu-id="05d0b-137">Un mémoire tampon de chaîne est inutile, car le `PipeReader` gère des octets qui n’ont pas été utilisés.</span><span class="sxs-lookup"><span data-stu-id="05d0b-137">There is no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
- <span data-ttu-id="05d0b-138">Les chaînes codées sont ajoutées directement à la liste des chaînes retournées.</span><span class="sxs-lookup"><span data-stu-id="05d0b-138">Encoded strings are directly added to the list of returned strings.</span></span>
- <span data-ttu-id="05d0b-139">La création de chaînes est libre d’allocation en dehors de la mémoire utilisée par la chaîne (à l’exception de l’appel `ToArray()`).</span><span class="sxs-lookup"><span data-stu-id="05d0b-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="05d0b-140">Adaptateurs</span><span class="sxs-lookup"><span data-stu-id="05d0b-140">Adapters</span></span>

<span data-ttu-id="05d0b-141">Maintenant que les propriétés `Body` et `BodyPipe` sont disponibles pour `HttpRequest` et `HttpResponse`, que se passe-t-il lorsque vous définissez `Body` sur un autre flux ?</span><span class="sxs-lookup"><span data-stu-id="05d0b-141">Now that both `Body` and `BodyPipe` properties are available for `HttpRequest` and `HttpResponse`, what happens when you set `Body` to a different stream?</span></span> <span data-ttu-id="05d0b-142">Dans 3.0, un nouvel ensemble d’adaptateurs adaptent automatiquement chaque type à l’autre.</span><span class="sxs-lookup"><span data-stu-id="05d0b-142">In 3.0, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="05d0b-143">Par exemple, si vous définissez `HttpRequest.Body` sur un nouveau flux, `HttpRequest.BodyPipe` est automatiquement défini sur un nouveau `PipeReader` qui encapsule `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="05d0b-143">For example, if you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyPipe` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span> <span data-ttu-id="05d0b-144">Le même comportement s’applique au paramètre de la propriété `BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="05d0b-144">The same behavior applies to setting the `BodyPipe` property.</span></span> <span data-ttu-id="05d0b-145">Si `HttpResponse.BodyPipe` est défini sur un nouveau `PipeWriter`, le `HttpResponse.Body` est automatiquement défini sur un flux qui encapsule `HttpResponse.BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="05d0b-145">If `HttpResponse.BodyPipe` is set to a new `PipeWriter`, the `HttpResponse.Body` is automatically set to a new stream that wraps `HttpResponse.BodyPipe`.</span></span>

## <a name="startasync"></a><span data-ttu-id="05d0b-146">StartAsync</span><span class="sxs-lookup"><span data-stu-id="05d0b-146">StartAsync</span></span>

<span data-ttu-id="05d0b-147">`HttpResponse.StartAsync` est nouveau dans 3.0.</span><span class="sxs-lookup"><span data-stu-id="05d0b-147">`HttpResponse.StartAsync` is new in 3.0.</span></span> <span data-ttu-id="05d0b-148">Il est utilisé pour indiquer que les en-têtes sont non modifiables et pour exécuter des rappels `OnStarting`.</span><span class="sxs-lookup"><span data-stu-id="05d0b-148">It is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="05d0b-149">Dans 3.0-preview3, vous devez appeler `StartAsync` avant d’utiliser `HttpRequest.BodyPipe`, et dans les versions futures, ce sera une recommandation.</span><span class="sxs-lookup"><span data-stu-id="05d0b-149">In 3.0-preview3, you have to call `StartAsync` before using `HttpRequest.BodyPipe`, and in future releases, it will be a recommendation.</span></span> <span data-ttu-id="05d0b-150">Lors de l’utilisation de Kestrel en tant que serveur, l’appel de StartAsync avant d’utiliser le `PipeReader` garantit que la mémoire retournée par `GetMemory` appartiendra au <xref:System.IO.Pipelines.Pipe> interne de Kestrel au lieu d’une mémoire tampon externe.</span><span class="sxs-lookup"><span data-stu-id="05d0b-150">When using Kestrel as a server, calling StartAsync before using the `PipeReader` guarantees that memory returned by `GetMemory` will belong to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="05d0b-151">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="05d0b-151">Additional resources</span></span>

- [<span data-ttu-id="05d0b-152">Présentation de System.IO.Pipelines</span><span class="sxs-lookup"><span data-stu-id="05d0b-152">Introducing System.IO.Pipelines</span></span>](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
- <xref:fundamentals/middleware/write>