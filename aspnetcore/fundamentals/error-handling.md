---
title: Gérer les erreurs dans ASP.NET Core
author: tdykstra
description: Découvrez comment gérer les erreurs dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665361"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="0e0c1-103">Gérer les erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e0c1-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="0e0c1-104">Par [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0e0c1-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0e0c1-105">Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="0e0c1-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0e0c1-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="0e0c1-107">Page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="0e0c1-107">Developer Exception Page</span></span>

<span data-ttu-id="0e0c1-108">Pour configurer une application de sorte qu’elle affiche une page contenant des informations détaillées sur les exceptions des requêtes, utilisez la *page Exception pour les développeurs*.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-108">To configure an app to display a page that shows detailed information about request exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="0e0c1-109">Cette page est mise à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui est disponible dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0e0c1-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="0e0c1-110">Ajoutez une ligne à la méthode `Startup.Configure` lorsque l’application s’exécute dans [l’environnement](xref:fundamentals/environments) de développement :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-110">Add a line to the `Startup.Configure` method when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

<span data-ttu-id="0e0c1-111">Placez l’appel à <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> devant tout middleware où vous souhaitez intercepter des exceptions.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="0e0c1-112">Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="0e0c1-113">Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="0e0c1-114">Pour plus d’informations sur la configuration des environnements, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="0e0c1-115">Pour afficher la page d’exception de développeur, exécutez l’exemple d’application avec l’environnement défini sur `Development` et ajoutez `?throw=true` à l’URL de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="0e0c1-116">Cette page inclut les informations suivantes sur l’exception et la demande :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="0e0c1-117">Trace de pile</span><span class="sxs-lookup"><span data-stu-id="0e0c1-117">Stack trace</span></span>
* <span data-ttu-id="0e0c1-118">Paramètres de la chaîne de requête (le cas échéant)</span><span class="sxs-lookup"><span data-stu-id="0e0c1-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="0e0c1-119">Cookies (le cas échéant)</span><span class="sxs-lookup"><span data-stu-id="0e0c1-119">Cookies (if any)</span></span>
* <span data-ttu-id="0e0c1-120">En-têtes</span><span class="sxs-lookup"><span data-stu-id="0e0c1-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="0e0c1-121">Configurer une page personnalisée de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="0e0c1-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="0e0c1-122">Lorsque l’application n’est pas exécutée dans l’environnement de développement, appelez la méthode d’extension <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> pour ajouter le middleware de gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="0e0c1-123">Le middleware :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-123">The middleware:</span></span>

* <span data-ttu-id="0e0c1-124">Intercepte les exceptions.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-124">Catches exceptions.</span></span>
* <span data-ttu-id="0e0c1-125">Journalise les exceptions.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-125">Logs exceptions.</span></span>
* <span data-ttu-id="0e0c1-126">Réexécute la requête dans un autre pipeline pour la page ou le contrôleur indiqué(e).</span><span class="sxs-lookup"><span data-stu-id="0e0c1-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="0e0c1-127">La demande n’est pas réexécutée si la réponse a démarré.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="0e0c1-128">Dans l’exemple suivant à partir de l’exemple d’application, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ajoute le middleware de gestion des exceptions dans des environnements qui ne sont pas de développement.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="0e0c1-129">La méthode d’extension spécifie une page d’erreur ou un contrôleur au point de terminaison `/Error` pour les demandes réexécutées une fois que les exceptions sont interceptées et journalisées :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

<span data-ttu-id="0e0c1-130">Le modèle d’application Razor Pages fournit une page d’erreur (*.cshtml*) et une classe <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) dans le dossier Pages.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="0e0c1-131">Dans une application MVC, la méthode de gestionnaire d’erreurs suivante est incluse dans le modèle d’application MVC et apparaît dans le contrôleur Home :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="0e0c1-132">Ne décorez pas la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="0e0c1-133">Les verbes explicites empêchent certaines demandes d’atteindre la méthode.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="0e0c1-134">Autorisez l’accès anonyme à la méthode afin que les utilisateurs non authentifiés puissent recevoir la vue des erreurs.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="access-the-exception"></a><span data-ttu-id="0e0c1-135">Accéder à l'exception</span><span class="sxs-lookup"><span data-stu-id="0e0c1-135">Access the exception</span></span>

<span data-ttu-id="0e0c1-136">Utilisez <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> pour accéder à l’exception ou au chemin de la requête d’origine dans un contrôleur ou une page :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception or the original request path in a controller or page:</span></span>

* <span data-ttu-id="0e0c1-137">Le chemin est accessible avec la propriété <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path>.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-137">The path is available from the <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path> property.</span></span>
* <span data-ttu-id="0e0c1-138">Lisez <xref:System.Exception?displayProperty=fullName> à partir de la propriété [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) héritée.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-138">Read the <xref:System.Exception?displayProperty=fullName> from the inherited [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) property.</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> <span data-ttu-id="0e0c1-139">Ne distribuez **pas** d’informations sensibles sur les erreurs de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> ou de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> aux clients.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-139">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="0e0c1-140">Cela représenterait un risque de sécurité.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-140">Serving errors is a security risk.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="0e0c1-141">Configurer du code personnalisé de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="0e0c1-141">Configure custom exception handling code</span></span>

<span data-ttu-id="0e0c1-142">Pour éviter de distribuer les erreurs à un point de terminaison avec une [page personnalisée de gestion des exceptions](#configure-a-custom-exception-handling-page), il est possible de fournir une expression lambda à <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-142">An alternative to serving an endpoint for errors with a [custom exception handling page](#configure-a-custom-exception-handling-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="0e0c1-143">À l’aide d’une expression lambda avec <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> permet d’accéder à l’erreur avant de renvoyer la réponse.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-143">Using a lambda with <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> allows access to the error before returning the response.</span></span>

<span data-ttu-id="0e0c1-144">L’exemple d’application illustre le code personnalisé de gestion des exceptions dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-144">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="0e0c1-145">Déclenchez une exception avec le lien **Lever une exception** sur la page d’index.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-145">Trigger an exception with the **Throw Exception** link on the Index page.</span></span> <span data-ttu-id="0e0c1-146">L’expression lambda suivante s’exécute :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-146">The following lambda runs:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> <span data-ttu-id="0e0c1-147">Ne distribuez **pas** d’informations sensibles sur les erreurs de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> ou de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> aux clients.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-147">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="0e0c1-148">Cela représenterait un risque de sécurité.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-148">Serving errors is a security risk.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="0e0c1-149">Configurer des pages de codes d’état</span><span class="sxs-lookup"><span data-stu-id="0e0c1-149">Configure status code pages</span></span>

<span data-ttu-id="0e0c1-150">Par défaut, une application ASP.NET Core ne fournit pas une page de codes d’état pour les codes d’état HTTP, comme *404 - Introuvable*.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-150">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="0e0c1-151">L’application retourne un code d’état et un corps de réponse vide.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-151">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="0e0c1-152">Pour fournir des pages de codes d’état, utilisez le middleware (intergiciel) de pages de codes d’état.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-152">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="0e0c1-153">Le middleware est mis à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui est disponible dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0e0c1-153">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="0e0c1-154">Ajoutez une ligne à la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-154">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="0e0c1-155">Appelez la méthode <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> avant les middlewares de gestion des requêtes (comme le middleware de fichiers statiques et le middleware MVC).</span><span class="sxs-lookup"><span data-stu-id="0e0c1-155">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="0e0c1-156">Par défaut, le middleware de pages de codes d’état ajoute des gestionnaires de texte uniquement pour les codes d’état courants, comme *404 - Introuvable* :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-156">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="0e0c1-157">Le middleware prend en charge plusieurs méthodes d’extension qui vous permettent de personnaliser son comportement.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-157">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="0e0c1-158">Une surcharge de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> prend une expression lambda, que vous pouvez utiliser pour traiter la logique de gestion des erreurs personnalisée et écrire manuellement la réponse :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-158">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="0e0c1-159">Une surcharge de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> prend un type de contenu et une chaîne de format, que vous pouvez utiliser pour personnaliser le type de contenu et le texte de la réponse :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-159">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="0e0c1-160">Rediriger et réexécuter les méthodes d’extension</span><span class="sxs-lookup"><span data-stu-id="0e0c1-160">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="0e0c1-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="0e0c1-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="0e0c1-162">Envoie un code d’état *302 - Trouvé* au client.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="0e0c1-163">Redirige le client à l’emplacement fourni dans le modèle d’URL.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="0e0c1-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> est généralement utilisée lorsque l’application :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="0e0c1-165">Doit rediriger le client vers un autre point de terminaison, généralement dans les cas où une autre application traite l’erreur.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-165">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="0e0c1-166">Pour les applications web, la barre d’adresses du navigateur client reflète le point de terminaison redirigé.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-166">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="0e0c1-167">Ne doit pas conserver ni retourner le code d’état d’origine avec la réponse de redirection initiale.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-167">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="0e0c1-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="0e0c1-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="0e0c1-169">Retourne le code d’état d’origine au client.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-169">Returns the original status code to the client.</span></span>
* <span data-ttu-id="0e0c1-170">Génère le corps de la réponse en réexécutant le pipeline de requête avec un autre chemin.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-170">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="0e0c1-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> est généralement utilisée lorsque l’application doit :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="0e0c1-172">Traiter la demande sans la rediriger vers un autre point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-172">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="0e0c1-173">Pour les applications web, la barre d’adresses du navigateur client reflète le point de terminaison demandé à l’origine.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-173">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="0e0c1-174">Conserver et retourner le code d’état d’origine avec la réponse.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-174">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="0e0c1-175">Les modèles peuvent inclure un espace réservé (`{0}`) pour le code d’état.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-175">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="0e0c1-176">Le modèle doit commencer par une barre oblique (`/`).</span><span class="sxs-lookup"><span data-stu-id="0e0c1-176">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="0e0c1-177">Lorsque vous utilisez un espace réservé, vérifiez que le point de terminaison (page ou contrôleur) peut traiter le segment de chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-177">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="0e0c1-178">Par exemple, une Razor Page pour les erreurs doit accepter la valeur du segment de chemin d’accès facultatif avec la directive `@page` :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-178">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="0e0c1-179">Les pages de codes d’état peuvent être désactivées pour des requêtes spécifiques dans une méthode de gestionnaire de Pages Razor ou dans un contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-179">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="0e0c1-180">Pour désactiver des pages de codes d’état, essayez de récupérer le <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> auprès de la collection [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) de la requête et désactivez la fonctionnalité si elle est disponible :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-180">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="0e0c1-181">Pour utiliser une surcharge <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> qui pointe vers un point de terminaison dans l’application, créez une vue MVC ou Razor Page pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-181">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="0e0c1-182">Par exemple, le modèle d’application Razor Pages génère les page et classe de modèle de page suivantes :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-182">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="0e0c1-183">*Error.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-183">*Error.cshtml*:</span></span>

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

<span data-ttu-id="0e0c1-184">*Error.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-184">*Error.cshtml.cs*:</span></span>

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="0e0c1-185">*Error.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-185">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a><span data-ttu-id="0e0c1-186">Code de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="0e0c1-186">Exception-handling code</span></span>

<span data-ttu-id="0e0c1-187">Le code dans les pages de gestion des exceptions peut lever des exceptions.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-187">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="0e0c1-188">Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-188">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="0e0c1-189">En outre, n’oubliez pas qu’une fois que les en-têtes de réponse sont envoyés :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-189">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="0e0c1-190">L’application ne peut pas modifier le code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-190">The app can't change the response's status code.</span></span>
* <span data-ttu-id="0e0c1-191">Il est impossible d’exécuter les pages d’exception ou les gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-191">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="0e0c1-192">La réponse doit être accomplie ou la connexion abandonnée.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-192">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="0e0c1-193">Gestion des exceptions de serveur</span><span class="sxs-lookup"><span data-stu-id="0e0c1-193">Server exception handling</span></span>

<span data-ttu-id="0e0c1-194">En plus de la logique de gestion des exceptions dans votre application, [l’implémentation de serveur](xref:fundamentals/servers/index) peut gérer certaines exceptions.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-194">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="0e0c1-195">Si le serveur intercepte une exception avant que les en-têtes de réponse ne soient envoyés, le serveur envoie une réponse *500 Erreur interne du serveur* sans corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-195">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="0e0c1-196">Si le serveur intercepte une exception une fois que les en-têtes de réponse ont été envoyés, il ferme la connexion.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-196">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="0e0c1-197">Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-197">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="0e0c1-198">Toute exception qui se produit tandis que le serveur traite la demande est gérée par le dispositif de gestion des exceptions du serveur.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-198">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="0e0c1-199">Ni les pages d’erreur personnalisées de l’application, ni les intergiciels (middleware) de gestion des exceptions, ni les filtres n’ont d’incidence sur ce comportement.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-199">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="0e0c1-200">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="0e0c1-200">Startup exception handling</span></span>

<span data-ttu-id="0e0c1-201">Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-201">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="0e0c1-202">En utilisant [l’hôte web](xref:fundamentals/host/web-host), vous pouvez [configurer le comportement de l’hôte en réponse aux erreurs au démarrage](xref:fundamentals/host/web-host#detailed-errors) à l’aide des clés `captureStartupErrors` et `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-202">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="0e0c1-203">La couche d’hébergement ne peut afficher une page d’erreur pour une erreur de démarrage capturée que si celle-ci se produit une fois établie la liaison entre l’adresse de l’hôte et le port.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-203">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="0e0c1-204">Si une liaison échoue pour une raison quelconque :</span><span class="sxs-lookup"><span data-stu-id="0e0c1-204">If any binding fails for any reason:</span></span>

* <span data-ttu-id="0e0c1-205">La couche d’hébergement journalise une exception critique.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-205">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="0e0c1-206">Le processus dotnet tombe en panne.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-206">The dotnet process crashes.</span></span>
* <span data-ttu-id="0e0c1-207">Aucune page d’erreur ne s’affiche lorsque l’application s’exécute sur le serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="0e0c1-207">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="0e0c1-208">En cas d’exécution sur [IIS](/iis) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), une réponse *502.5 - Échec du processus* est retournée par le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) si le processus ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-208">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="0e0c1-209">Pour plus d'informations, consultez <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-209">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="0e0c1-210">Pour plus d’informations sur la résolution des problèmes de démarrage lors de l’hébergement avec Azure App Service, consultez le site <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-210">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="0e0c1-211">Gestion des erreurs MVC ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0e0c1-211">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="0e0c1-212">Les applications [MVC](xref:mvc/overview) ont des options supplémentaires pour la gestion des erreurs, telles que la configuration de filtres d’exception et l’exécution d’une validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-212">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="0e0c1-213">Filtres d’exceptions</span><span class="sxs-lookup"><span data-stu-id="0e0c1-213">Exception filters</span></span>

<span data-ttu-id="0e0c1-214">Vous pouvez configurer les filtres d’exception globalement ou en fonction d’un contrôleur ou d’une action dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-214">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="0e0c1-215">Ces filtres gèrent les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-215">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="0e0c1-216">Ils ne sont appelés dans aucun autre cas.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-216">These filters aren't called otherwise.</span></span> <span data-ttu-id="0e0c1-217">Pour plus d'informations, consultez <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-217">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="0e0c1-218">Les filtres d’exceptions sont utiles pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que le middleware de gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-218">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="0e0c1-219">Nous vous recommandons d’utiliser le middleware.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-219">We recommend using the middleware.</span></span> <span data-ttu-id="0e0c1-220">Utilisez uniquement des filtres si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-220">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="0e0c1-221">Gérer les erreurs d’état de modèle</span><span class="sxs-lookup"><span data-stu-id="0e0c1-221">Handle model state errors</span></span>

<span data-ttu-id="0e0c1-222">La [validation de modèle](xref:mvc/models/validation) se produit avant l’appel de chaque action du contrôleur ; il appartient à la méthode d’action d’inspecter [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-222">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="0e0c1-223">Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de[ validation de modèle](xref:mvc/models/validation). Dans ce cas, un [filtre](xref:mvc/controllers/filters) peut être un emplacement approprié pour implémenter une telle stratégie.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-223">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="0e0c1-224">Vous devez tester le comportement de vos actions avec les états de modèles non valide.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-224">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="0e0c1-225">Pour plus d'informations, consultez <xref:mvc/controllers/testing>.</span><span class="sxs-lookup"><span data-stu-id="0e0c1-225">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0e0c1-226">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0e0c1-226">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
