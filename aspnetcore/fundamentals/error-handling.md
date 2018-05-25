---
title: Gérer les erreurs dans ASP.NET Core
author: ardalis
description: Découvrez comment gérer les erreurs dans les applications ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 3ff3a17d14d9ed7c438399191ffe3cf93d555d49
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="aa044-103">Gérer les erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa044-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="aa044-104">Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="aa044-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="aa044-105">Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa044-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="aa044-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aa044-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="aa044-107">Page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="aa044-107">The developer exception page</span></span>

<span data-ttu-id="aa044-108">Pour configurer une application afin qu’elle affiche une page qui indique des informations détaillées sur les exceptions, installez le package NuGet `Microsoft.AspNetCore.Diagnostics` et ajoutez une ligne à la [méthode Configure dans la classe Startup](xref:fundamentals/startup) :</span><span class="sxs-lookup"><span data-stu-id="aa044-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="aa044-109">Placez `UseDeveloperExceptionPage` avant tout intergiciel (middleware) dans lequel vous souhaitez intercepter des exceptions, tel que `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="aa044-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="aa044-110">Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**.</span><span class="sxs-lookup"><span data-stu-id="aa044-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="aa044-111">Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production.</span><span class="sxs-lookup"><span data-stu-id="aa044-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="aa044-112">[Découvrez en plus sur la configuration d’environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="aa044-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="aa044-113">Pour afficher la page d’exception de développeur, exécutez l’exemple d’application avec l’environnement défini sur `Development` et ajoutez `?throw=true` à l’URL de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="aa044-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="aa044-114">Cette page inclut plusieurs onglets avec des informations sur l’exception et la demande.</span><span class="sxs-lookup"><span data-stu-id="aa044-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="aa044-115">Le premier onglet inclut une trace de pile.</span><span class="sxs-lookup"><span data-stu-id="aa044-115">The first tab includes a stack trace.</span></span> 

![Trace de pile](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="aa044-117">L’onglet suivant montre les paramètres de la chaîne de requête, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="aa044-117">The next tab shows the query string parameters, if any.</span></span>

![Paramètres de la chaîne de requête](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="aa044-119">Cette demande ne contenait pas de cookies, sinon ils apparaîtraient sous l’onglet **Cookies**. Vous pouvez voir les en-têtes qui ont été passés sous le dernier onglet.</span><span class="sxs-lookup"><span data-stu-id="aa044-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![En-têtes](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="aa044-121">Configuration d’une page de gestion des exceptions personnalisée</span><span class="sxs-lookup"><span data-stu-id="aa044-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="aa044-122">Il est judicieux de configurer une page de gestionnaire d’exceptions à utiliser quand l’application n’est pas en cours d’exécution dans l’environnement `Development`.</span><span class="sxs-lookup"><span data-stu-id="aa044-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="aa044-123">Dans une application MVC, ne décorez pas explicitement la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="aa044-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="aa044-124">Utiliser des verbes explicites peut empêcher certaines demandes d’atteindre la méthode.</span><span class="sxs-lookup"><span data-stu-id="aa044-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="aa044-125">Configuration des pages de codes d’état</span><span class="sxs-lookup"><span data-stu-id="aa044-125">Configuring status code pages</span></span>

<span data-ttu-id="aa044-126">Par défaut, une application ne fournit pas de page de codes d’état très complète pour les codes d’état HTTP, comme *404 (introuvable)*.</span><span class="sxs-lookup"><span data-stu-id="aa044-126">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="aa044-127">Pour fournir des pages de codes d’état, configurez le middleware de pages de codes d’état en ajoutant une ligne à la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="aa044-127">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="aa044-128">Par défaut, le middleware de pages de codes d’état ajoute des gestionnaires simples de texte uniquement pour les codes d’état courants, comme 404 :</span><span class="sxs-lookup"><span data-stu-id="aa044-128">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![Page 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="aa044-130">Le middleware prend en charge plusieurs méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="aa044-130">The middleware supports several extension methods.</span></span> <span data-ttu-id="aa044-131">Une méthode prend une expression lambda :</span><span class="sxs-lookup"><span data-stu-id="aa044-131">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="aa044-132">Une autre méthode prend un type de contenu et une chaîne de format :</span><span class="sxs-lookup"><span data-stu-id="aa044-132">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="aa044-133">Il existe également des méthodes d’extension de redirection et de réexécution.</span><span class="sxs-lookup"><span data-stu-id="aa044-133">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="aa044-134">La méthode de redirection envoie un code d’état 302 au client :</span><span class="sxs-lookup"><span data-stu-id="aa044-134">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="aa044-135">La méthode de réexécution retourne le code d’état d’origine au client, mais exécute également le gestionnaire pour l’URL de redirection :</span><span class="sxs-lookup"><span data-stu-id="aa044-135">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="aa044-136">Les pages de codes d’état peuvent être désactivées pour des requêtes spécifiques dans une méthode de gestionnaire de Pages Razor ou dans un contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="aa044-136">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="aa044-137">Pour désactiver des pages de codes d’état, essayez de récupérer le [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) auprès de la collection [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) de la requête et désactivez la fonctionnalité si elle est disponible :</span><span class="sxs-lookup"><span data-stu-id="aa044-137">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="aa044-138">Code de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="aa044-138">Exception-handling code</span></span>

<span data-ttu-id="aa044-139">Le code dans les pages de gestion des exceptions peut lever des exceptions.</span><span class="sxs-lookup"><span data-stu-id="aa044-139">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="aa044-140">Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.</span><span class="sxs-lookup"><span data-stu-id="aa044-140">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="aa044-141">En outre, sachez qu’une fois que les en-têtes d’une réponse ont été envoyés, vous ne pouvez pas changer le code d’état de la réponse, et aucun gestionnaire ou page d’exception ne peut s’exécuter.</span><span class="sxs-lookup"><span data-stu-id="aa044-141">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="aa044-142">La réponse doit être accomplie ou la connexion abandonnée.</span><span class="sxs-lookup"><span data-stu-id="aa044-142">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="aa044-143">Gestion des exceptions de serveur</span><span class="sxs-lookup"><span data-stu-id="aa044-143">Server exception handling</span></span>

<span data-ttu-id="aa044-144">En plus de la logique de gestion des exceptions dans votre application, le [serveur](xref:fundamentals/servers/index) qui héberge votre application effectue certaines tâches de gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="aa044-144">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="aa044-145">Si le serveur intercepte une exception avant que les en-têtes ne soient envoyés, le serveur envoie une réponse *500 Erreur interne du serveur* sans corps.</span><span class="sxs-lookup"><span data-stu-id="aa044-145">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="aa044-146">Si le serveur intercepte une exception une fois que les en-têtes ont été envoyés, il ferme la connexion.</span><span class="sxs-lookup"><span data-stu-id="aa044-146">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="aa044-147">Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="aa044-147">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="aa044-148">Toute exception qui se produit est gérée par le dispositif de gestion des exceptions du serveur.</span><span class="sxs-lookup"><span data-stu-id="aa044-148">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="aa044-149">Aucune page d’erreur personnalisée configurée, ni aucun filtre ou intergiciel (middleware) de gestion des exceptions, n’affecte ce comportement.</span><span class="sxs-lookup"><span data-stu-id="aa044-149">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="aa044-150">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="aa044-150">Startup exception handling</span></span>

<span data-ttu-id="aa044-151">Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="aa044-151">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="aa044-152">En utilisant l’[hôte web](xref:fundamentals/host/web-host), vous pouvez [configurer le comportement de l’hôte en réponse aux erreurs au démarrage](xref:fundamentals/host/web-host#detailed-errors) à l’aide des clés `captureStartupErrors` et `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="aa044-152">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="aa044-153">La couche d’hébergement ne peut afficher une page d’erreur pour une erreur de démarrage capturée que si celle-ci se produit une fois établie la liaison entre l’adresse de l’hôte et le port.</span><span class="sxs-lookup"><span data-stu-id="aa044-153">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="aa044-154">Si une liaison échoue pour une raison quelconque, la couche d’hébergement journalise une exception critique, le processus dotnet plante et aucune page d’erreur n’est affichée quand l’application s’exécute sur le serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="aa044-154">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="aa044-155">En cas d’exécution sur [IIS](/iis) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), une réponse *502.5 Échec du processus* est retournée par le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) si le processus ne peut pas être a démarré.</span><span class="sxs-lookup"><span data-stu-id="aa044-155">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="aa044-156">Suivez les conseils de dépannage de la rubrique [Résoudre les problèmes liés à ASP.NET Core sur IIS](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="aa044-156">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="aa044-157">Gestion des erreurs MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aa044-157">ASP.NET MVC error handling</span></span>

<span data-ttu-id="aa044-158">Les applications [MVC](xref:mvc/overview) ont des options supplémentaires pour la gestion des erreurs, telles que la configuration de filtres d’exception et l’exécution d’une validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="aa044-158">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="aa044-159">Filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="aa044-159">Exception Filters</span></span>

<span data-ttu-id="aa044-160">Vous pouvez configurer les filtres d’exception globalement ou en fonction d’un contrôleur ou d’une action dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="aa044-160">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="aa044-161">Ces filtres sont uniquement appelés pour gérer les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="aa044-161">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="aa044-162">Découvrez-en plus sur les filtres d’exception dans [Filtres](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="aa044-162">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

>[!TIP]
> <span data-ttu-id="aa044-163">Les filtres d’exception sont adaptés pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que l’intergiciel (middleware) de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="aa044-163">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="aa044-164">En règle générale, préférez l’intergiciel (middleware), et n’utilisez des filtres que si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie.</span><span class="sxs-lookup"><span data-stu-id="aa044-164">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="aa044-165">Gestion des erreurs d’état de modèle</span><span class="sxs-lookup"><span data-stu-id="aa044-165">Handling Model State Errors</span></span>

<span data-ttu-id="aa044-166">La [validation de modèle](xref:mvc/models/validation) se produit avant l’appel de chaque action du contrôleur ; il appartient à la méthode d’action d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="aa044-166">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="aa044-167">Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de validation de modèle ; dans ce cas, un [filtre](xref:mvc/controllers/filters) peut être un emplacement approprié pour implémenter une telle stratégie.</span><span class="sxs-lookup"><span data-stu-id="aa044-167">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="aa044-168">Vous devez tester le comportement de vos actions avec les états de modèles non valide.</span><span class="sxs-lookup"><span data-stu-id="aa044-168">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="aa044-169">Pour plus d’informations, consultez [Tester la logique du contrôleur](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="aa044-169">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>



