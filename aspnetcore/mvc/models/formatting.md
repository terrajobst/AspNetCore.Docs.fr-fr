---
title: "Mise en forme des données de réponse dans ASP.NET MVC de base"
author: ardalis
description: "Découvrez comment mettre en forme les données de réponse dans ASP.NET MVC de base."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85398928164e75ec27c91870f03ee1c81725e080
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="dc86f-103">Introduction à la mise en forme des données de réponse dans ASP.NET MVC de base</span><span class="sxs-lookup"><span data-stu-id="dc86f-103">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="dc86f-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="dc86f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="dc86f-105">ASP.NET Core MVC prend en charge pour la mise en forme des données de réponse, au format fixe ou en réponse aux spécifications du client.</span><span class="sxs-lookup"><span data-stu-id="dc86f-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="dc86f-106">[Afficher ou télécharger l’exemple à partir de GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="dc86f-106">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="dc86f-107">Résultats de l’Action de format spécifiques</span><span class="sxs-lookup"><span data-stu-id="dc86f-107">Format-Specific Action Results</span></span>

<span data-ttu-id="dc86f-108">Certains types de résultats d’action sont spécifiques à un format particulier, tel que `JsonResult` et `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="dc86f-109">Actions peuvent retourner des résultats spécifiques qui sont toujours mis en forme d’une manière particulière.</span><span class="sxs-lookup"><span data-stu-id="dc86f-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="dc86f-110">Par exemple, retourner un `JsonResult` retourne des données au format JSON, indépendamment des préférences du client.</span><span class="sxs-lookup"><span data-stu-id="dc86f-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="dc86f-111">De même, en retournant un `ContentResult` retournent des données de chaîne au format texte brut (comme sera simplement retourner une chaîne).</span><span class="sxs-lookup"><span data-stu-id="dc86f-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="dc86f-112">Une action n’est pas nécessaire pour retourner un type particulier ; MVC prend en charge la valeur de retour de n’importe quel objet.</span><span class="sxs-lookup"><span data-stu-id="dc86f-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="dc86f-113">Si une action retourne un `IActionResult` mise en œuvre et le contrôleur hérite `Controller`, les développeurs ont de nombreuses méthodes d’assistance correspondant à la plupart des options.</span><span class="sxs-lookup"><span data-stu-id="dc86f-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="dc86f-114">Résultats des actions qui retournent des objets qui ne sont pas `IActionResult` types seront sérialisées à l’aide de la commande appropriée `IOutputFormatter` implémentation.</span><span class="sxs-lookup"><span data-stu-id="dc86f-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="dc86f-115">Pour retourner des données dans un format spécifique à partir d’un contrôleur qui hérite de la `Controller` classe de base, utilisez la méthode d’assistance intégrée `Json` à retourner JSON et `Content` pour le texte brut.</span><span class="sxs-lookup"><span data-stu-id="dc86f-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="dc86f-116">Votre méthode d’action doit retourner le type de résultat spécifique (par exemple, `JsonResult`) ou `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="dc86f-117">Recherche de données au format JSON :</span><span class="sxs-lookup"><span data-stu-id="dc86f-117">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="dc86f-118">Exemple de réponse à partir de cette action :</span><span class="sxs-lookup"><span data-stu-id="dc86f-118">Sample response from this action:</span></span>

![Onglet réseau des outils de développement dans Microsoft Edge indiquant le type de contenu de la réponse est application/json](formatting/_static/json-response.png)

<span data-ttu-id="dc86f-120">Notez que le type de contenu de la réponse est `application/json`, comme illustré dans la liste des demandes du réseau et dans la section en-têtes de réponse.</span><span class="sxs-lookup"><span data-stu-id="dc86f-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="dc86f-121">Notez également la liste des options présentées par le navigateur (dans ce cas, Microsoft Edge) dans l’en-tête Accept dans la section en-têtes de la requête.</span><span class="sxs-lookup"><span data-stu-id="dc86f-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="dc86f-122">La technique actuelle ignore cet en-tête ; Il s’est présenté ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="dc86f-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="dc86f-123">Pour renvoyer les données mises en forme en texte brut, utilisez `ContentResult` et `Content` assistance :</span><span class="sxs-lookup"><span data-stu-id="dc86f-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="dc86f-124">Une réponse à partir de cette action :</span><span class="sxs-lookup"><span data-stu-id="dc86f-124">A response from this action:</span></span>

![Onglet réseau des outils de développement dans Microsoft Edge indiquant le type de contenu de la réponse est le texte brut](formatting/_static/text-response.png)

<span data-ttu-id="dc86f-126">Notez dans ce cas le `Content-Type` retournée est `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="dc86f-127">Vous pouvez également obtenir ce comportement à l’aide d’un type de réponse de chaîne :</span><span class="sxs-lookup"><span data-stu-id="dc86f-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="dc86f-128">Pour les actions non trivial avec plusieurs options (par exemple, les codes d’état HTTP différents en fonction du résultat des opérations effectuées) ou des types de retour, préférez `IActionResult` comme type de retour.</span><span class="sxs-lookup"><span data-stu-id="dc86f-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="dc86f-129">Négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="dc86f-129">Content Negotiation</span></span>

<span data-ttu-id="dc86f-130">Négociation de contenu (*conneg* en abrégé) se produit lorsque le client spécifie un [en-tête Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="dc86f-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="dc86f-131">Le format par défaut utilisé par ASP.NET MVC de base est JSON.</span><span class="sxs-lookup"><span data-stu-id="dc86f-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="dc86f-132">Négociation de contenu est implémentée par `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="dc86f-133">Il est également créé dans le code d’état retournés de résultats d’action spécifique à partir des méthodes d’assistance (qui sont basées sur `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="dc86f-133">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="dc86f-134">Vous pouvez également retourner un type de modèle (une classe que vous avez défini comme type de transfert de données) et le framework encapsule automatiquement dans un `ObjectResult` pour vous.</span><span class="sxs-lookup"><span data-stu-id="dc86f-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="dc86f-135">La méthode d’action suivante utilise le `Ok` et `NotFound` méthodes d’assistance :</span><span class="sxs-lookup"><span data-stu-id="dc86f-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="dc86f-136">Une réponse au format JSON s’affichera, sauf si un autre format a été demandé et le serveur peut retourner le format demandé.</span><span class="sxs-lookup"><span data-stu-id="dc86f-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="dc86f-137">Vous pouvez utiliser un outil tel que [Fiddler](http://www.telerik.com/fiddler) pour créer une demande qui inclut un en-tête Accept et spécifiez un autre format.</span><span class="sxs-lookup"><span data-stu-id="dc86f-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="dc86f-138">Dans ce cas, si le serveur possède un *formateur* qui peut produire une réponse dans le format demandé, le résultat est renvoyé dans le format préféré au client.</span><span class="sxs-lookup"><span data-stu-id="dc86f-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Affichage créé manuellement dans la console Fiddler obtenir une demande avec une valeur d’en-tête Accept d’application/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="dc86f-140">Dans la capture d’écran ci-dessus, l’éditeur de Fiddler a été utilisé pour générer une demande, en spécifiant `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="dc86f-141">Par défaut, ASP.NET MVC de base prend uniquement en charge JSON, donc, même si un autre format est spécifié, le résultat retourné est toujours au format JSON.</span><span class="sxs-lookup"><span data-stu-id="dc86f-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="dc86f-142">Vous verrez comment ajouter des modules de formatage supplémentaires dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="dc86f-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="dc86f-143">Actions de contrôleur peuvent retourner POCOs (Plain Old CLR Objects), auquel cas ASP.NET MVC crée automatiquement un `ObjectResult` qui encapsule l’objet.</span><span class="sxs-lookup"><span data-stu-id="dc86f-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="dc86f-144">Le client obtiendra l’objet sérialisé au format (format JSON est la valeur par défaut ; vous pouvez configurer XML ou autres formats).</span><span class="sxs-lookup"><span data-stu-id="dc86f-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="dc86f-145">Si l’objet retourné est `null`, le framework retournera un `204 No Content` réponse.</span><span class="sxs-lookup"><span data-stu-id="dc86f-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="dc86f-146">Retourner un type d’objet :</span><span class="sxs-lookup"><span data-stu-id="dc86f-146">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="dc86f-147">Dans l’exemple, une demande pour un alias valide auteur reçoit une réponse 200 OK avec des données de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="dc86f-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="dc86f-148">Une demande d’un alias non valide répondra 204 pas de contenu.</span><span class="sxs-lookup"><span data-stu-id="dc86f-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="dc86f-149">Captures d’écran montrant la réponse dans les formats XML et JSON sont indiquées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="dc86f-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="dc86f-150">Processus de négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="dc86f-150">Content Negotiation Process</span></span>

<span data-ttu-id="dc86f-151">Contenu *négociation* intervient uniquement si une `Accept` en-tête s’affiche dans la demande.</span><span class="sxs-lookup"><span data-stu-id="dc86f-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="dc86f-152">Lorsqu’une demande contient un en-tête accept, le framework énumère les types de médias dans l’en-tête accept par ordre de préférence et tente de trouver un formateur capable de générer une réponse dans un des formats spécifiés par l’en-tête accept.</span><span class="sxs-lookup"><span data-stu-id="dc86f-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="dc86f-153">Si aucun formateur n’est trouvée qui peut satisfaire la demande du client, le framework va tenter de trouver le premier formateur capable de générer une réponse (à moins que le développeur a configuré l’option sur `MvcOptions` pour retourner 406 non Acceptable à la place).</span><span class="sxs-lookup"><span data-stu-id="dc86f-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="dc86f-154">Si la demande spécifie XML, mais le formateur XML n’a pas été configuré, le module de formatage JSON être utilisé.</span><span class="sxs-lookup"><span data-stu-id="dc86f-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="dc86f-155">En règle générale, si aucun formateur n’est configuré, qui peut fournir le format demandé, puis le premier formateur peut mettre en forme l’objet est utilisé.</span><span class="sxs-lookup"><span data-stu-id="dc86f-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="dc86f-156">Si aucun en-tête n’est spécifié, le premier formateur capable de gérer l’objet doit être retournée sera utilisé pour sérialiser la réponse.</span><span class="sxs-lookup"><span data-stu-id="dc86f-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="dc86f-157">Dans ce cas, il n’existe pas de n’importe quel lieu de négociation - le serveur consiste à déterminer quel format, il utilisera.</span><span class="sxs-lookup"><span data-stu-id="dc86f-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="dc86f-158">Si l’en-tête Accept contient `*/*`, l’en-tête sera ignoré, sauf si `RespectBrowserAcceptHeader` a la valeur true sur `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="dc86f-159">Navigateurs et la négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="dc86f-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="dc86f-160">Contrairement aux clients d’API par défaut, les navigateurs web ont tendance à fournir `Accept` en-têtes qui incluent un large éventail de formats, y compris les caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="dc86f-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="dc86f-161">Par défaut, lorsque le framework détecte que la demande provient d’un navigateur, il ignore le `Accept` en-tête et le retour à la place le contenu de l’application configurée format par défaut (JSON, sauf si configuré dans le cas contraire).</span><span class="sxs-lookup"><span data-stu-id="dc86f-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="dc86f-162">Cela fournit une expérience plus cohérente lors de l’utilisation de différents navigateurs pour consommer des API.</span><span class="sxs-lookup"><span data-stu-id="dc86f-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="dc86f-163">Si vous préférez en-têtes accept de votre navigateur de honorer l’application, vous pouvez le configurer en tant que partie de la configuration de MVC en définissant `RespectBrowserAcceptHeader` à `true` dans les `ConfigureServices` méthode dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc86f-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="dc86f-164">Formateurs de configuration</span><span class="sxs-lookup"><span data-stu-id="dc86f-164">Configuring Formatters</span></span>

<span data-ttu-id="dc86f-165">Si votre application doit prendre en charge des formats supplémentaires au-delà de la valeur par défaut de JSON, vous pouvez ajouter des packages NuGet et configurer MVC pour les prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="dc86f-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="dc86f-166">Il existe des formateurs distincts pour l’entrée et de sortie.</span><span class="sxs-lookup"><span data-stu-id="dc86f-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="dc86f-167">Les formateurs d’entrée sont utilisés par [liaison de modèle](model-binding.md); les formateurs de sortie sont utilisés pour le formatage de réponses.</span><span class="sxs-lookup"><span data-stu-id="dc86f-167">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="dc86f-168">Vous pouvez également configurer [personnalisé formateurs](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="dc86f-168">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="dc86f-169">Ajout de prise en charge du Format XML</span><span class="sxs-lookup"><span data-stu-id="dc86f-169">Adding XML Format Support</span></span>

<span data-ttu-id="dc86f-170">Pour ajouter la prise en charge de la mise en forme XML, vous devez installer le `Microsoft.AspNetCore.Mvc.Formatters.Xml` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="dc86f-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="dc86f-171">Ajouter le XmlSerializerFormatters à la configuration de MVC à *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="dc86f-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="dc86f-172">Ou bien, vous pouvez ajouter simplement le formateur de sortie :</span><span class="sxs-lookup"><span data-stu-id="dc86f-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="dc86f-173">Ces deux approches sérialise les résultats à l’aide de `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="dc86f-174">Si vous préférez, vous pouvez utiliser la `System.Runtime.Serialization.DataContractSerializer` en ajoutant le formateur associé :</span><span class="sxs-lookup"><span data-stu-id="dc86f-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="dc86f-175">Une fois que vous avez ajouté la prise en charge de la mise en forme XML, vos méthodes de contrôleur doivent retourner le format approprié en fonction de la demande `Accept` en-tête, comme cette Fiddler montre :</span><span class="sxs-lookup"><span data-stu-id="dc86f-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Console de Fiddler : brut de l’onglet de la requête affiche la valeur d’en-tête Accept est application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="dc86f-178">Vous pouvez voir dans l’onglet inspecteurs qui la demande GET brute a été effectuée avec un `Accept: application/xml` ensemble d’en-têtes.</span><span class="sxs-lookup"><span data-stu-id="dc86f-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="dc86f-179">Le volet de réponse affiche le `Content-Type: application/xml` en-tête et le `Author` objet a été sérialisé en XML.</span><span class="sxs-lookup"><span data-stu-id="dc86f-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="dc86f-180">Utilisez l’onglet pour modifier la requête pour spécifier `application/json` dans le `Accept` en-tête.</span><span class="sxs-lookup"><span data-stu-id="dc86f-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="dc86f-181">Exécutez la requête et la réponse est au format JSON :</span><span class="sxs-lookup"><span data-stu-id="dc86f-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Console de Fiddler : brut de l’onglet de la requête affiche la valeur d’en-tête Accept est application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="dc86f-184">Dans cette capture d’écran, vous pouvez voir la demande définit un en-tête de `Accept: application/json` et la réponse indique que le même que son `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="dc86f-185">Le `Author` objet est affiché dans le corps de la réponse, au format JSON.</span><span class="sxs-lookup"><span data-stu-id="dc86f-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="dc86f-186">Forcer un Format particulier</span><span class="sxs-lookup"><span data-stu-id="dc86f-186">Forcing a Particular Format</span></span>

<span data-ttu-id="dc86f-187">Si vous souhaitez limiter les formats de réponse pour une action spécifique que vous pouvez, vous pouvez appliquer la `[Produces]` filtre.</span><span class="sxs-lookup"><span data-stu-id="dc86f-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="dc86f-188">Le `[Produces]` filtre spécifie les formats de réponse pour une action spécifique (ou un contrôleur).</span><span class="sxs-lookup"><span data-stu-id="dc86f-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="dc86f-189">Comme la plupart [filtres](../controllers/filters.md), cela peut être appliqué à l’action, le contrôleur ou la portée globale.</span><span class="sxs-lookup"><span data-stu-id="dc86f-189">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="dc86f-190">Le `[Produces]` filtre force toutes les actions au sein de la `AuthorsController` pour renvoyer des réponses au format JSON, même si d’autres modules de formatage a été configurés pour l’application et le client fourni un `Accept` en-tête de demande d’une autre disponibles format.</span><span class="sxs-lookup"><span data-stu-id="dc86f-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="dc86f-191">Consultez [filtres](../controllers/filters.md) pour en savoir plus, y compris comment appliquer des filtres globaux.</span><span class="sxs-lookup"><span data-stu-id="dc86f-191">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="dc86f-192">Formateurs de cas spéciaux</span><span class="sxs-lookup"><span data-stu-id="dc86f-192">Special Case Formatters</span></span>

<span data-ttu-id="dc86f-193">Certains cas sont implémentées à l’aide de formateurs intégrés.</span><span class="sxs-lookup"><span data-stu-id="dc86f-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="dc86f-194">Par défaut, `string` seront formatées en tant que types de retour *texte/brut* (*texte/html* si demandé `Accept` en-tête).</span><span class="sxs-lookup"><span data-stu-id="dc86f-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="dc86f-195">Ce comportement peut être supprimé en supprimant le `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="dc86f-196">Supprimer des formateurs dans le `Configure` méthode dans *Startup.cs* (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="dc86f-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="dc86f-197">Les actions qui ont un objet de modèle retournent type ne retournera un 204 aucun réponse contenu lors du retour `null`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="dc86f-198">Ce comportement peut être supprimé en supprimant le `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="dc86f-199">Le code suivant supprime le `TextOutputFormatter` et `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="dc86f-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="dc86f-200">Sans le `TextOutputFormatter`, `string` retournent des types de retour 406 non Acceptable, par exemple.</span><span class="sxs-lookup"><span data-stu-id="dc86f-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="dc86f-201">Notez que si un formateur XML existe, elle mettra en forme `string` types de retour si la `TextOutputFormatter` est supprimé.</span><span class="sxs-lookup"><span data-stu-id="dc86f-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="dc86f-202">Sans le `HttpNoContentOutputFormatter`, objets null sont mis en forme à l’aide du formateur configuré.</span><span class="sxs-lookup"><span data-stu-id="dc86f-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="dc86f-203">Par exemple, le module de formatage JSON retourne simplement une réponse avec un corps de `null`, tandis que le formateur XML retourne un élément XML vide avec l’attribut `xsi:nil="true"` défini.</span><span class="sxs-lookup"><span data-stu-id="dc86f-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="dc86f-204">Mappages d’URL de Format de réponse</span><span class="sxs-lookup"><span data-stu-id="dc86f-204">Response Format URL Mappings</span></span>

<span data-ttu-id="dc86f-205">Les clients peuvent demander un format particulier dans le cadre de l’URL, comme dans la chaîne de requête ou une partie du chemin d’accès, ou à l’aide d’une extension de fichier de format spécifiques telles que .xml ou .json.</span><span class="sxs-lookup"><span data-stu-id="dc86f-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="dc86f-206">Le mappage de chemin d’accès de la demande doit être spécifié dans l’itinéraire à l’aide de l’API.</span><span class="sxs-lookup"><span data-stu-id="dc86f-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="dc86f-207">Exemple :</span><span class="sxs-lookup"><span data-stu-id="dc86f-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="dc86f-208">Cet itinéraire permettrait le format demandé en tant qu’une extension de fichier facultatif.</span><span class="sxs-lookup"><span data-stu-id="dc86f-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="dc86f-209">Le `[FormatFilter]` attribut vérifie l’existence de la valeur de format dans le `RouteData` et mappe le format de réponse au formateur approprié lors de la création de la réponse.</span><span class="sxs-lookup"><span data-stu-id="dc86f-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="dc86f-210">Route</span><span class="sxs-lookup"><span data-stu-id="dc86f-210">Route</span></span>                      | <span data-ttu-id="dc86f-211">Formateur</span><span class="sxs-lookup"><span data-stu-id="dc86f-211">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="dc86f-212">Le formateur de sortie par défaut</span><span class="sxs-lookup"><span data-stu-id="dc86f-212">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="dc86f-213">Le module de formatage JSON (si configuré)</span><span class="sxs-lookup"><span data-stu-id="dc86f-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="dc86f-214">Le formateur XML (si configuré)</span><span class="sxs-lookup"><span data-stu-id="dc86f-214">The XML formatter (if configured)</span></span>  |
