---
title: Gérer les erreurs dans ASP.NET Core
author: tdykstra
description: Découvrez comment gérer les erreurs dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/07/2019
uid: fundamentals/error-handling
ms.openlocfilehash: cbb9462a3c6010e074dc391aa128ac2cbb901456
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705576"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="1ddb4-103">Gérer les erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ddb4-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="1ddb4-104">Par [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1ddb4-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1ddb4-105">Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="1ddb4-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="1ddb4-107">([Guide pratique de téléchargement](xref:index#how-to-download-a-sample).) L’article présente les instructions à suivre pour définir des directives de préprocesseur (`#if`, `#endif`, `#define`) dans l’exemple d’application afin de gérer différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-107">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="1ddb4-108">Page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="1ddb4-108">Developer Exception Page</span></span>

<span data-ttu-id="1ddb4-109">La *Page d’exceptions du développeur* affiche des informations détaillées sur les exceptions des demandes.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-109">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="1ddb4-110">Elle est mise à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-110">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="1ddb4-111">Ajoutez du code à la méthode `Startup.Configure` pour activer la page lorsque l’application s’exécute dans [l’environnement](xref:fundamentals/environments) de développement :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-111">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="1ddb4-112">Placez l’appel à <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> avant tous les middlewares dont vous souhaitez intercepter les exceptions.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-112">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="1ddb4-113">Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-113">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="1ddb4-114">Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-114">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="1ddb4-115">Pour plus d’informations sur la configuration des environnements, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-115">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="1ddb4-116">Cette page inclut les informations suivantes sur l’exception et la demande :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="1ddb4-117">Trace de pile</span><span class="sxs-lookup"><span data-stu-id="1ddb4-117">Stack trace</span></span>
* <span data-ttu-id="1ddb4-118">Paramètres de la chaîne de requête (le cas échéant)</span><span class="sxs-lookup"><span data-stu-id="1ddb4-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="1ddb4-119">Cookies (le cas échéant)</span><span class="sxs-lookup"><span data-stu-id="1ddb4-119">Cookies (if any)</span></span>
* <span data-ttu-id="1ddb4-120">En-têtes</span><span class="sxs-lookup"><span data-stu-id="1ddb4-120">Headers</span></span>

<span data-ttu-id="1ddb4-121">Pour afficher la Page d’exceptions du développeur dans [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), utilisez la directive de préprocesseur `DevEnvironment` et sélectionnez **Déclencher une exception** sur la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-121">To see the Developer Exception Page in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="1ddb4-122">Page Gestionnaire d’exceptions</span><span class="sxs-lookup"><span data-stu-id="1ddb4-122">Exception handler page</span></span>

<span data-ttu-id="1ddb4-123">Pour configurer une page de gestion des erreurs personnalisée en fonction de l’environnement de production, utilisez le middleware de gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-123">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="1ddb4-124">Le middleware :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-124">The middleware:</span></span>

* <span data-ttu-id="1ddb4-125">Intercepte et consigne les exceptions.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-125">Catches and logs exceptions.</span></span>
* <span data-ttu-id="1ddb4-126">Réexécute la requête dans un autre pipeline pour la page ou le contrôleur indiqué(e).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="1ddb4-127">La demande n’est pas réexécutée si la réponse a démarré.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="1ddb4-128">Dans l’exemple suivant, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ajoute le middleware de gestion des exceptions dans des environnements autres que les environnements de développement :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-128">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="1ddb4-129">Le modèle d’application Razor Pages fournit une page d’erreur (*.cshtml*) et une classe <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) dans le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-129">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="1ddb4-130">Pour une application MVC, le modèle de projet comprend une méthode d’action d’erreur et un affichage correspondant.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-130">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="1ddb4-131">Voici la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-131">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="1ddb4-132">Ne décorez pas la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="1ddb4-133">Les verbes explicites empêchent certaines demandes d’atteindre la méthode.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="1ddb4-134">Autorisez l’accès anonyme à la méthode afin que les utilisateurs non authentifiés puissent recevoir la vue des erreurs.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="1ddb4-135">Accéder à l'exception</span><span class="sxs-lookup"><span data-stu-id="1ddb4-135">Access the exception</span></span>

<span data-ttu-id="1ddb4-136">Utilisez <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> pour accéder à l’exception et au chemin de la demande d’origine dans une page ou un contrôleur de gestionnaire d’erreurs :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="1ddb4-137">Ne communiquez **pas** d’informations sensibles sur les erreurs aux clients.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-137">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="1ddb4-138">Cela représenterait un risque de sécurité.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-138">Serving errors is a security risk.</span></span>

<span data-ttu-id="1ddb4-139">Pour afficher la page de gestion des exceptions dans [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), utilisez les directive de préprocesseur `ProdEnvironment` et `ErrorHandlerPage` et sélectionnez **Déclencher une exception** sur la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-139">To see the exception handling page in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="1ddb4-140">Expression lambda Gestionnaire d’exceptions</span><span class="sxs-lookup"><span data-stu-id="1ddb4-140">Exception handler lambda</span></span>

<span data-ttu-id="1ddb4-141">En dehors d’une [page de gestionnaire d’exceptions personnalisée](#exception-handler-page), il est possible de fournir une expression lambda à <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>,</span><span class="sxs-lookup"><span data-stu-id="1ddb4-141">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="1ddb4-142">ce qui permet d’accéder à l’erreur avant de retourner la réponse.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-142">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="1ddb4-143">Voici un exemple d’utilisation d’une expression lambda pour la gestion des exceptions :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-143">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> <span data-ttu-id="1ddb4-144">Ne distribuez **pas** d’informations sensibles sur les erreurs de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> ou de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> aux clients.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-144">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="1ddb4-145">Cela représenterait un risque de sécurité.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-145">Serving errors is a security risk.</span></span>

<span data-ttu-id="1ddb4-146">Pour afficher le résultat de l’expression lambda de gestion des exceptions dans [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), utilisez les directive de préprocesseur `ProdEnvironment` et `ErrorHandlerLambda` et sélectionnez **Déclencher une exception** sur la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-146">To see the result of the exception handling lambda in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="1ddb4-147">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="1ddb4-147">UseStatusCodePages</span></span>

<span data-ttu-id="1ddb4-148">Par défaut, une application ASP.NET Core ne fournit pas une page de codes d’état pour les codes d’état HTTP, comme *404 - Introuvable*.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-148">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="1ddb4-149">L’application retourne un code d’état et un corps de réponse vide.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-149">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="1ddb4-150">Pour fournir des pages de codes d’état, utilisez le middleware Pages de codes d’état.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-150">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="1ddb4-151">Le middleware est mis à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-151">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ddb4-152">Pour activer les gestionnaires exclusivement textuels par défaut des codes d’état d’erreur courants, appelez <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> dans la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-152">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="1ddb4-153">Appelez `UseStatusCodePages` avant les middlewares de gestion des demandes (par exemple, le middleware de fichiers statiques et le middleware MVC).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-153">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="1ddb4-154">Voici un exemple du texte affiché par les gestionnaires par défaut :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-154">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="1ddb4-155">Pour voir l’un des formats de pages de codes d’état dans [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), utilisez une des directives de préprocesseur qui commencent par `StatusCodePages`, puis sélectionnez **Déclencher une erreur 404** sur la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-155">To see one of the various status code page formats in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="1ddb4-156">UseStatusCodePages avec chaîne de format</span><span class="sxs-lookup"><span data-stu-id="1ddb4-156">UseStatusCodePages with format string</span></span>

<span data-ttu-id="1ddb4-157">Pour personnaliser le texte et le type de contenu de la réponse, utilisez la surcharge de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> qui prend un type de contenu et une chaîne de format :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-157">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="1ddb4-158">UseStatusCodePages avec expression lambda</span><span class="sxs-lookup"><span data-stu-id="1ddb4-158">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="1ddb4-159">Pour spécifier un code personnalisé de gestion des erreurs et d’écriture de la réponse, utilisez la surcharge de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> qui prend une expression lambda :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-159">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirect"></a><span data-ttu-id="1ddb4-160">UseStatusCodePagesWithRedirect</span><span class="sxs-lookup"><span data-stu-id="1ddb4-160">UseStatusCodePagesWithRedirect</span></span>

<span data-ttu-id="1ddb4-161">La méthode d’extension <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-161">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="1ddb4-162">Envoie un code d’état *302 - Trouvé* au client.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="1ddb4-163">Redirige le client à l’emplacement fourni dans le modèle d’URL.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="1ddb4-164">Le modèle d’URL peut comporter un espace réservé `{0}` pour le code d’état, comme dans l’exemple.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-164">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="1ddb4-165">S’il commence par un tilde (~), celui-ci est remplacé par le `PathBase` de l’application.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-165">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="1ddb4-166">Si vous pointez vers un point de terminaison dans l’application, créez une vue MVC ou une page Razor pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-166">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="1ddb4-167">Pour consulter un exemple Razor Pages, voir [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) dans [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-167">For a Razor Pages example, see [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="1ddb4-168">Cette méthode est généralement utilisée lorsque l’application :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-168">This method is commonly used when the app:</span></span>

* <span data-ttu-id="1ddb4-169">Doit rediriger le client vers un autre point de terminaison, généralement dans les cas où une autre application traite l’erreur.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-169">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="1ddb4-170">Pour les applications web, la barre d’adresses du navigateur client reflète le point de terminaison redirigé.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-170">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="1ddb4-171">Ne doit pas conserver ni retourner le code d’état d’origine avec la réponse de redirection initiale.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-171">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="1ddb4-172">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="1ddb4-172">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="1ddb4-173">La méthode d’extension <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-173">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="1ddb4-174">Retourne le code d’état d’origine au client.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-174">Returns the original status code to the client.</span></span>
* <span data-ttu-id="1ddb4-175">Génère le corps de la réponse en réexécutant le pipeline de requête avec un autre chemin.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-175">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="1ddb4-176">Si vous pointez vers un point de terminaison dans l’application, créez une vue MVC ou une page Razor pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-176">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="1ddb4-177">Pour consulter un exemple Razor Pages, voir [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) dans [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-177">For a Razor Pages example, see [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="1ddb4-178">Cette méthode est généralement utilisée lorsque l’application doit :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-178">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="1ddb4-179">Traiter la demande sans la rediriger vers un autre point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-179">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="1ddb4-180">Pour les applications web, la barre d’adresses du navigateur client reflète le point de terminaison demandé à l’origine.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-180">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="1ddb4-181">Conserver et retourner le code d’état d’origine avec la réponse.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-181">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="1ddb4-182">Les modèles d’URL et de chaîne de requête peuvent comporter un espace réservé (`{0}`) pour le code d’état.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-182">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="1ddb4-183">Le modèle d’URL doit commencer par une barre oblique (`/`).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-183">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="1ddb4-184">Si vous utilisez un espace réservé dans le chemin, vérifiez que le point de terminaison (page ou contrôleur) peut traiter le segment de chemin.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-184">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="1ddb4-185">Par exemple, une Razor Page pour les erreurs doit accepter la valeur du segment de chemin d’accès facultatif avec la directive `@page` :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-185">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="1ddb4-186">Le point de terminaison qui traite l’erreur peut récupérer l’URL d’origine qui a généré l’erreur, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-186">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="1ddb4-187">Désactiver les pages de codes d’état</span><span class="sxs-lookup"><span data-stu-id="1ddb4-187">Disable status code pages</span></span>

<span data-ttu-id="1ddb4-188">Les pages de codes d’état peuvent être désactivées pour des requêtes spécifiques dans une méthode de gestionnaire de Pages Razor ou dans un contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-188">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="1ddb4-189">Pour désactiver les pages de codes d’état, utilisez <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-189">To disable status code pages, use the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="1ddb4-190">Code de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="1ddb4-190">Exception-handling code</span></span>

<span data-ttu-id="1ddb4-191">Le code dans les pages de gestion des exceptions peut lever des exceptions.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-191">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="1ddb4-192">Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-192">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="1ddb4-193">En-têtes de réponse</span><span class="sxs-lookup"><span data-stu-id="1ddb4-193">Response headers</span></span>

<span data-ttu-id="1ddb4-194">Une fois les en-têtes d’une réponse envoyés :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-194">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="1ddb4-195">L’application ne peut pas modifier le code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-195">The app can't change the response's status code.</span></span>
* <span data-ttu-id="1ddb4-196">Il est impossible d’exécuter les pages d’exception ou les gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-196">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="1ddb4-197">La réponse doit être accomplie ou la connexion abandonnée.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-197">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="1ddb4-198">Gestion des exceptions de serveur</span><span class="sxs-lookup"><span data-stu-id="1ddb4-198">Server exception handling</span></span>

<span data-ttu-id="1ddb4-199">En plus de la logique de gestion des exceptions de votre application, [l’implémentation de serveur HTTP](xref:fundamentals/servers/index) peut gérer certaines exceptions.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-199">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="1ddb4-200">Si le serveur intercepte une exception avant que les en-têtes de réponse ne soient envoyés, le serveur envoie une réponse *500 Erreur interne du serveur* sans corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-200">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="1ddb4-201">Si le serveur intercepte une exception une fois que les en-têtes de réponse ont été envoyés, il ferme la connexion.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-201">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="1ddb4-202">Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-202">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="1ddb4-203">Toute exception qui se produit tandis que le serveur traite la demande est gérée par le dispositif de gestion des exceptions du serveur.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-203">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="1ddb4-204">Ni les pages d’erreur personnalisées de l’application, ni les intergiciels (middleware) de gestion des exceptions, ni les filtres n’ont d’incidence sur ce comportement.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-204">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="1ddb4-205">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="1ddb4-205">Startup exception handling</span></span>

<span data-ttu-id="1ddb4-206">Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-206">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="1ddb4-207">L’hôte peut être configuré pour [capturer les erreurs de démarrage](xref:fundamentals/host/web-host#capture-startup-errors) et [capturer les erreurs détaillées](xref:fundamentals/host/web-host#detailed-errors).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-207">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="1ddb4-208">La couche d’hébergement ne peut afficher la page d’une erreur de démarrage capturée que si celle-ci se produit une fois la liaison établie entre l’adresse d’hôte et le port.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-208">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="1ddb4-209">Si la liaison échoue :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-209">If binding fails:</span></span>

* <span data-ttu-id="1ddb4-210">La couche d’hébergement journalise une exception critique.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-210">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="1ddb4-211">Le processus dotnet tombe en panne.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-211">The dotnet process crashes.</span></span>
* <span data-ttu-id="1ddb4-212">Aucune page d’erreur ne s’affiche si le serveur HTTP est [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-212">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="1ddb4-213">En cas d’exécution sur [IIS](/iis) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), une réponse *502.5 - Échec du processus* est retournée par le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) si le processus ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-213">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="1ddb4-214">Pour plus d'informations, consultez <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-214">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="1ddb4-215">Pour plus d’informations sur la résolution des problèmes de démarrage lors de l’hébergement avec Azure App Service, consultez le site <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-215">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="1ddb4-216">Page d’erreur de base de données</span><span class="sxs-lookup"><span data-stu-id="1ddb4-216">Database error page</span></span>

<span data-ttu-id="1ddb4-217">Le middleware [Page d’erreur de base de données](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) capture les exceptions liées aux bases de données qui peuvent être résolues par des migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-217">The [Database Error Page](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="1ddb4-218">Lorsque ces exceptions se produisent, une réponse HTML comportant le détail des actions possibles pour résoudre le problème est générée.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-218">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="1ddb4-219">Cette page ne doit être activée que dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-219">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="1ddb4-220">Pour cela, ajoutez du code à `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="1ddb4-220">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

## <a name="exception-filters"></a><span data-ttu-id="1ddb4-221">Filtres d’exceptions</span><span class="sxs-lookup"><span data-stu-id="1ddb4-221">Exception filters</span></span>

<span data-ttu-id="1ddb4-222">Dans les applications MVC, vous pouvez configurer les filtres d’exception globalement, contrôleur par contrôleur ou action par action.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-222">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="1ddb4-223">Dans les applications Razor Pages, ils peuvent être configurés globalement ou modèle de page par modèle de page.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-223">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="1ddb4-224">Ces filtres gèrent les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-224">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="1ddb4-225">Pour plus d'informations, consultez <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-225">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="1ddb4-226">Les filtres d’exceptions sont utiles pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que le middleware de gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-226">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="1ddb4-227">Nous vous recommandons d’utiliser le middleware.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-227">We recommend using the middleware.</span></span> <span data-ttu-id="1ddb4-228">N’utilisez des filtres que si vous devez gérer les erreurs différemment en fonction de l’action MVC choisie.</span><span class="sxs-lookup"><span data-stu-id="1ddb4-228">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="1ddb4-229">Erreurs d’état de modèle</span><span class="sxs-lookup"><span data-stu-id="1ddb4-229">Model state errors</span></span>

<span data-ttu-id="1ddb4-230">Pour plus d’informations sur la gestion des erreurs d’état de modèle, voir [Liaison de modèles](xref:mvc/models/model-binding) et [Validation de modèles](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="1ddb4-230">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ddb4-231">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1ddb4-231">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
