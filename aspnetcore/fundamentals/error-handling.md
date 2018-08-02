---
title: Gérer les erreurs dans ASP.NET Core
author: ardalis
description: Découvrez comment gérer les erreurs dans les applications ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: d7e60c0f615841461a17b093bffe5fb3f82f8616
ms.sourcegitcommit: 506a199274e9fe5fb4070b273ba94f29f14cb619
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2018
ms.locfileid: "39332273"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="56d4b-103">Gérer les erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56d4b-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="56d4b-104">Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="56d4b-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="56d4b-105">Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56d4b-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="56d4b-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56d4b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="56d4b-107">Page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="56d4b-107">The developer exception page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="56d4b-108">Pour configurer une application pour afficher une page qui fournit des informations détaillées sur les exceptions, utilisez la *page d’exception de développeur*.</span><span class="sxs-lookup"><span data-stu-id="56d4b-108">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="56d4b-109">Cette page est mise à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui est disponible dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="56d4b-109">The page made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="56d4b-110">Ajoutez une ligne à la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="56d4b-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="56d4b-111">Pour configurer une application pour afficher une page qui fournit des informations détaillées sur les exceptions, utilisez la *page d’exception de développeur*.</span><span class="sxs-lookup"><span data-stu-id="56d4b-111">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="56d4b-112">Cette page est mise à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui est disponible dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="56d4b-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="56d4b-113">Ajoutez une ligne à la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="56d4b-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="56d4b-114">Pour configurer une application pour afficher une page qui fournit des informations détaillées sur les exceptions, utilisez la *page d’exception de développeur*.</span><span class="sxs-lookup"><span data-stu-id="56d4b-114">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="56d4b-115">Cette page est accessible en ajoutant une référence de package pour le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="56d4b-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="56d4b-116">Ajoutez une ligne à la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="56d4b-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="56d4b-117">Placez l’appel à [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) devant tout intergiciel (middleware) où vous souhaitez intercepter des exceptions, tel que `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="56d4b-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="56d4b-118">Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**.</span><span class="sxs-lookup"><span data-stu-id="56d4b-118">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="56d4b-119">Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production.</span><span class="sxs-lookup"><span data-stu-id="56d4b-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="56d4b-120">[Découvrez en plus sur la configuration d’environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="56d4b-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="56d4b-121">Pour afficher la page d’exception de développeur, exécutez l’exemple d’application avec l’environnement défini sur `Development` et ajoutez `?throw=true` à l’URL de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="56d4b-121">To see the developer exception page, run the sample app with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="56d4b-122">Cette page inclut plusieurs onglets avec des informations sur l’exception et la demande.</span><span class="sxs-lookup"><span data-stu-id="56d4b-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="56d4b-123">Le premier onglet inclut une trace de pile :</span><span class="sxs-lookup"><span data-stu-id="56d4b-123">The first tab includes a stack trace:</span></span>

![Trace de pile](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="56d4b-125">L’onglet suivant montre les paramètres de la chaîne de requête, le cas échéant :</span><span class="sxs-lookup"><span data-stu-id="56d4b-125">The next tab shows the query string parameters, if any:</span></span>

![Paramètres de la chaîne de requête](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="56d4b-127">Si la requête a des cookies, ceux-ci apparaissent sous l’onglet **Cookies**. Les en-têtes sont visibles sous le dernier onglet :</span><span class="sxs-lookup"><span data-stu-id="56d4b-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![En-têtes](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="56d4b-129">Configuration d’une page personnalisée de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="56d4b-129">Configuring a custom exception handling page</span></span>

<span data-ttu-id="56d4b-130">Configurez une page de gestionnaire d’exceptions à utiliser quand l’application ne s’exécute pas dans l’environnement `Development` :</span><span class="sxs-lookup"><span data-stu-id="56d4b-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="56d4b-131">Dans une application Razor Pages, le modèle Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) fournit une page Error et une classe de modèle de page `ErrorModel` dans le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="56d4b-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="56d4b-132">Dans une application MVC, ne décorez pas la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="56d4b-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="56d4b-133">Les verbes explicites empêchent certaines demandes d’atteindre la méthode.</span><span class="sxs-lookup"><span data-stu-id="56d4b-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="56d4b-134">Autorisez l’accès anonyme à la méthode afin que les utilisateurs non authentifiés puissent recevoir la vue des erreurs.</span><span class="sxs-lookup"><span data-stu-id="56d4b-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="56d4b-135">Par exemple, la méthode de gestionnaire d’erreurs suivante est fournie par le modèle MVC [dotnet new](/dotnet/core/tools/dotnet-new) et apparaît dans le contrôleur Home :</span><span class="sxs-lookup"><span data-stu-id="56d4b-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="56d4b-136">Configuration des pages de codes d’état</span><span class="sxs-lookup"><span data-stu-id="56d4b-136">Configuring status code pages</span></span>

<span data-ttu-id="56d4b-137">Par défaut, une application ne fournit pas de page de codes d’état très complète pour les codes d’état HTTP, comme *404 (introuvable)*.</span><span class="sxs-lookup"><span data-stu-id="56d4b-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="56d4b-138">Pour fournir des pages de codes d’état, configurez le middleware de pages de codes d’état en ajoutant une ligne à la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="56d4b-138">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="56d4b-139">Par défaut, le middleware de pages de codes d’état ajoute des gestionnaires de texte uniquement pour les codes d’état courants, comme 404 :</span><span class="sxs-lookup"><span data-stu-id="56d4b-139">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![Page 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="56d4b-141">Le middleware prend en charge plusieurs méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="56d4b-141">The middleware supports several extension methods.</span></span> <span data-ttu-id="56d4b-142">Une méthode prend une expression lambda :</span><span class="sxs-lookup"><span data-stu-id="56d4b-142">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="56d4b-143">Une autre méthode prend un type de contenu et une chaîne de format :</span><span class="sxs-lookup"><span data-stu-id="56d4b-143">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="56d4b-144">Il existe également des méthodes d’extension de redirection et de réexécution.</span><span class="sxs-lookup"><span data-stu-id="56d4b-144">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="56d4b-145">La méthode de redirection envoie un code d’état *302 Trouvé* au client et redirige ce dernier vers le modèle d’URL d’emplacement fourni.</span><span class="sxs-lookup"><span data-stu-id="56d4b-145">The redirect method sends a *302 Found* status code to the client and redirects the client to the provided location URL template.</span></span> <span data-ttu-id="56d4b-146">Le modèle peut inclure un espace réservé `{0}` pour le code d’état.</span><span class="sxs-lookup"><span data-stu-id="56d4b-146">The template may include a `{0}` placeholder for the status code.</span></span> <span data-ttu-id="56d4b-147">Le chemin de base est ajouté aux URL commençant par `~`.</span><span class="sxs-lookup"><span data-stu-id="56d4b-147">URLs starting with `~` have the base path prepended.</span></span> <span data-ttu-id="56d4b-148">Une URL qui ne commence pas par `~` est utilisée en l’état.</span><span class="sxs-lookup"><span data-stu-id="56d4b-148">A URL that doesn't start with `~` is used as is.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="56d4b-149">La méthode de réexécution retourne le code d’état d’origine au client et indique que le corps de la réponse doit être généré en réexécutant le pipeline de requête avec un autre chemin.</span><span class="sxs-lookup"><span data-stu-id="56d4b-149">The re-execute method returns the original status code to the client and specifies that the response body should be generated by re-executing the request pipeline using an alternate path.</span></span> <span data-ttu-id="56d4b-150">Ce chemin peut contenir un espace réservé `{0}` pour le code d’état :</span><span class="sxs-lookup"><span data-stu-id="56d4b-150">This path may contain a `{0}` placeholder for the status code:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="56d4b-151">Les pages de codes d’état peuvent être désactivées pour des requêtes spécifiques dans une méthode de gestionnaire de Pages Razor ou dans un contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="56d4b-151">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="56d4b-152">Pour désactiver des pages de codes d’état, essayez de récupérer le [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) auprès de la collection [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) de la requête et désactivez la fonctionnalité si elle est disponible :</span><span class="sxs-lookup"><span data-stu-id="56d4b-152">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="56d4b-153">Si vous utilisez une surcharge `UseStatusCodePages*` qui pointe vers un point de terminaison dans l’application, créez une vue MVC ou Razor Page pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="56d4b-153">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="56d4b-154">Par exemple, le modèle [dotnet new](/dotnet/core/tools/dotnet-new) pour une application Razor Pages génère les page et classe de modèle de page suivantes :</span><span class="sxs-lookup"><span data-stu-id="56d4b-154">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="56d4b-155">*Error.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="56d4b-155">*Error.cshtml*:</span></span>

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

<span data-ttu-id="56d4b-156">*Error.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="56d4b-156">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="56d4b-157">Code de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="56d4b-157">Exception-handling code</span></span>

<span data-ttu-id="56d4b-158">Le code dans les pages de gestion des exceptions peut lever des exceptions.</span><span class="sxs-lookup"><span data-stu-id="56d4b-158">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="56d4b-159">Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.</span><span class="sxs-lookup"><span data-stu-id="56d4b-159">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="56d4b-160">En outre, sachez qu’une fois que les en-têtes d’une réponse ont été envoyés, vous ne pouvez pas changer le code d’état de la réponse, et aucun gestionnaire ou page d’exception ne peut s’exécuter.</span><span class="sxs-lookup"><span data-stu-id="56d4b-160">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="56d4b-161">La réponse doit être accomplie ou la connexion abandonnée.</span><span class="sxs-lookup"><span data-stu-id="56d4b-161">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="56d4b-162">Gestion des exceptions de serveur</span><span class="sxs-lookup"><span data-stu-id="56d4b-162">Server exception handling</span></span>

<span data-ttu-id="56d4b-163">En plus de la logique de gestion des exceptions dans votre application, le [serveur](xref:fundamentals/servers/index) qui héberge votre application effectue certaines tâches de gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="56d4b-163">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="56d4b-164">Si le serveur intercepte une exception avant que les en-têtes ne soient envoyés, le serveur envoie une réponse *500 Erreur interne du serveur* sans corps.</span><span class="sxs-lookup"><span data-stu-id="56d4b-164">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="56d4b-165">Si le serveur intercepte une exception une fois que les en-têtes ont été envoyés, il ferme la connexion.</span><span class="sxs-lookup"><span data-stu-id="56d4b-165">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="56d4b-166">Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="56d4b-166">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="56d4b-167">Toute exception qui se produit est gérée par le dispositif de gestion des exceptions du serveur.</span><span class="sxs-lookup"><span data-stu-id="56d4b-167">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="56d4b-168">Aucune page d’erreur personnalisée configurée, ni aucun filtre ou intergiciel (middleware) de gestion des exceptions, n’affecte ce comportement.</span><span class="sxs-lookup"><span data-stu-id="56d4b-168">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="56d4b-169">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="56d4b-169">Startup exception handling</span></span>

<span data-ttu-id="56d4b-170">Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="56d4b-170">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="56d4b-171">En utilisant l’[hôte web](xref:fundamentals/host/web-host), vous pouvez [configurer le comportement de l’hôte en réponse aux erreurs au démarrage](xref:fundamentals/host/web-host#detailed-errors) à l’aide des clés `captureStartupErrors` et `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="56d4b-171">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="56d4b-172">La couche d’hébergement ne peut afficher une page d’erreur pour une erreur de démarrage capturée que si celle-ci se produit une fois établie la liaison entre l’adresse de l’hôte et le port.</span><span class="sxs-lookup"><span data-stu-id="56d4b-172">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="56d4b-173">Si une liaison échoue pour une raison quelconque, la couche d’hébergement journalise une exception critique, le processus dotnet plante et aucune page d’erreur n’est affichée quand l’application s’exécute sur le serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="56d4b-173">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="56d4b-174">En cas d’exécution sur [IIS](/iis) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), une réponse *502.5 Échec du processus* est retournée par le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) si le processus ne peut pas être a démarré.</span><span class="sxs-lookup"><span data-stu-id="56d4b-174">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="56d4b-175">Pour plus d’informations sur la résolution des problèmes de démarrage lors de l’hébergement avec IIS, consultez le site <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="56d4b-175">For information on troubleshooting startup issues when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="56d4b-176">Pour plus d’informations sur la résolution des problèmes de démarrage lors de l’hébergement avec Azure App Service, consultez le site <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="56d4b-176">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="56d4b-177">Gestion des erreurs MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="56d4b-177">ASP.NET MVC error handling</span></span>

<span data-ttu-id="56d4b-178">Les applications [MVC](xref:mvc/overview) ont des options supplémentaires pour la gestion des erreurs, telles que la configuration de filtres d’exception et l’exécution d’une validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="56d4b-178">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="56d4b-179">Filtres d’exceptions</span><span class="sxs-lookup"><span data-stu-id="56d4b-179">Exception filters</span></span>

<span data-ttu-id="56d4b-180">Vous pouvez configurer les filtres d’exception globalement ou en fonction d’un contrôleur ou d’une action dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="56d4b-180">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="56d4b-181">Ces filtres gèrent les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="56d4b-181">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="56d4b-182">Ils ne sont appelés dans aucun autre cas.</span><span class="sxs-lookup"><span data-stu-id="56d4b-182">These filters aren't called otherwise.</span></span> <span data-ttu-id="56d4b-183">Pour en savoir plus, consultez [Filtres](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="56d4b-183">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="56d4b-184">Les filtres d’exceptions sont adaptés pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que l’intergiciel de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="56d4b-184">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="56d4b-185">En règle générale, privilégiez l’utilisation de middleware et n’utilisez des filtres que si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie.</span><span class="sxs-lookup"><span data-stu-id="56d4b-185">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="56d4b-186">Gestion des erreurs d’état de modèle</span><span class="sxs-lookup"><span data-stu-id="56d4b-186">Handling model state errors</span></span>

<span data-ttu-id="56d4b-187">La [validation de modèle](xref:mvc/models/validation) se produit avant l’appel de chaque action du contrôleur ; il appartient à la méthode d’action d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="56d4b-187">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="56d4b-188">Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de validation de modèle. Dans ce cas, un [filtre](xref:mvc/controllers/filters) peut être un emplacement approprié pour implémenter une telle stratégie.</span><span class="sxs-lookup"><span data-stu-id="56d4b-188">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="56d4b-189">Vous devez tester le comportement de vos actions avec les états de modèles non valide.</span><span class="sxs-lookup"><span data-stu-id="56d4b-189">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="56d4b-190">Pour plus d’informations, consultez [Tester la logique du contrôleur](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="56d4b-190">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56d4b-191">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="56d4b-191">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
