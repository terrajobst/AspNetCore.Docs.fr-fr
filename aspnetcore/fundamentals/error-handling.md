---
title: Gestion des erreurs dans ASP.NET Core
author: ardalis
description: "Découvrez comment gérer les erreurs dans les applications ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="5e042-103">Présentation de la gestion des erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e042-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="5e042-104">Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="5e042-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="5e042-105">Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e042-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="5e042-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e042-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="5e042-107">Page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="5e042-107">The developer exception page</span></span>

<span data-ttu-id="5e042-108">Pour configurer une application afin qu’elle affiche une page qui indique des informations détaillées sur les exceptions, installez le package NuGet `Microsoft.AspNetCore.Diagnostics` et ajoutez une ligne à la [méthode Configure dans la classe Startup](startup.md) :</span><span class="sxs-lookup"><span data-stu-id="5e042-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="5e042-109">Placez `UseDeveloperExceptionPage` avant tout intergiciel (middleware) dans lequel vous souhaitez intercepter des exceptions, tel que `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="5e042-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="5e042-110">Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**.</span><span class="sxs-lookup"><span data-stu-id="5e042-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="5e042-111">Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production.</span><span class="sxs-lookup"><span data-stu-id="5e042-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="5e042-112">[Découvrez en plus sur la configuration d’environnements](environments.md).</span><span class="sxs-lookup"><span data-stu-id="5e042-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="5e042-113">Pour afficher la page d’exception de développeur, exécutez l’exemple d’application avec l’environnement défini sur `Development` et ajoutez `?throw=true` à l’URL de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="5e042-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="5e042-114">Cette page inclut plusieurs onglets avec des informations sur l’exception et la demande.</span><span class="sxs-lookup"><span data-stu-id="5e042-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="5e042-115">Le premier onglet inclut une trace de pile.</span><span class="sxs-lookup"><span data-stu-id="5e042-115">The first tab includes a stack trace.</span></span> 

![Trace de pile](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="5e042-117">L’onglet suivant montre les paramètres de la chaîne de requête, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="5e042-117">The next tab shows the query string parameters, if any.</span></span>

![Paramètres de la chaîne de requête](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="5e042-119">Cette demande ne contenait pas de cookies, sinon ils apparaîtraient sous l’onglet **Cookies**. Vous pouvez voir les en-têtes qui ont été passés sous le dernier onglet.</span><span class="sxs-lookup"><span data-stu-id="5e042-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![En-têtes](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="5e042-121">Configuration d’une page de gestion des exceptions personnalisée</span><span class="sxs-lookup"><span data-stu-id="5e042-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="5e042-122">Il est judicieux de configurer une page de gestionnaire d’exceptions à utiliser quand l’application n’est pas en cours d’exécution dans l’environnement `Development`.</span><span class="sxs-lookup"><span data-stu-id="5e042-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="5e042-123">Dans une application MVC, ne décorez pas explicitement la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="5e042-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="5e042-124">Utiliser des verbes explicites peut empêcher certaines demandes d’atteindre la méthode.</span><span class="sxs-lookup"><span data-stu-id="5e042-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="5e042-125">Configuration des pages de codes d’état</span><span class="sxs-lookup"><span data-stu-id="5e042-125">Configuring status code pages</span></span>

<span data-ttu-id="5e042-126">Par défaut, votre application ne fournit pas une page de codes d’état complète pour les codes d’état HTTP tels que 500 (erreur interne du serveur) ou 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="5e042-126">By default, your app won't provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="5e042-127">Vous pouvez configurer le `StatusCodePagesMiddleware` en ajoutant une ligne à la méthode `Configure` :</span><span class="sxs-lookup"><span data-stu-id="5e042-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="5e042-128">Par défaut, cet intergiciel (middleware) ajoute des gestionnaires simples de texte uniquement pour les codes d’état courants, tels que 404 :</span><span class="sxs-lookup"><span data-stu-id="5e042-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![Page 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="5e042-130">L’intergiciel (middleware) prend en charge plusieurs méthodes d’extension différentes.</span><span class="sxs-lookup"><span data-stu-id="5e042-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="5e042-131">L’une prend une expression lambda, une autre une chaîne de format et un type de contenu.</span><span class="sxs-lookup"><span data-stu-id="5e042-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="5e042-132">Il existe également des méthodes d’extension de redirection.</span><span class="sxs-lookup"><span data-stu-id="5e042-132">There are also redirect extension methods.</span></span> <span data-ttu-id="5e042-133">L’une d’elles envoie un code d’état 302 au client, tandis qu’une autre retourne le code d’état d’origine au client, mais exécute également le gestionnaire pour l’URL de redirection.</span><span class="sxs-lookup"><span data-stu-id="5e042-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="5e042-134">Si vous devez désactiver les pages de codes d’état pour certaines demandes, vous pouvez le faire :</span><span class="sxs-lookup"><span data-stu-id="5e042-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="5e042-135">Code de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="5e042-135">Exception-handling code</span></span>

<span data-ttu-id="5e042-136">Le code dans les pages de gestion des exceptions peut lever des exceptions.</span><span class="sxs-lookup"><span data-stu-id="5e042-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="5e042-137">Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.</span><span class="sxs-lookup"><span data-stu-id="5e042-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="5e042-138">En outre, sachez qu’une fois que les en-têtes d’une réponse ont été envoyés, vous ne pouvez pas changer le code d’état de la réponse, et aucun gestionnaire ou page d’exception ne peut s’exécuter.</span><span class="sxs-lookup"><span data-stu-id="5e042-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="5e042-139">La réponse doit être accomplie ou la connexion abandonnée.</span><span class="sxs-lookup"><span data-stu-id="5e042-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="5e042-140">Gestion des exceptions de serveur</span><span class="sxs-lookup"><span data-stu-id="5e042-140">Server exception handling</span></span>

<span data-ttu-id="5e042-141">En plus de la logique de gestion des exceptions dans votre application, le [serveur](servers/index.md) qui héberge votre application effectue certaines tâches de gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="5e042-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="5e042-142">Si le serveur intercepte une exception avant que les en-têtes ne soient envoyés, le serveur envoie une réponse d’erreur de serveur interne 500 sans corps.</span><span class="sxs-lookup"><span data-stu-id="5e042-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="5e042-143">Si le serveur intercepte une exception une fois que les en-têtes ont été envoyés, il ferme la connexion.</span><span class="sxs-lookup"><span data-stu-id="5e042-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="5e042-144">Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="5e042-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="5e042-145">Toute exception qui se produit est gérée par le dispositif de gestion des exceptions du serveur.</span><span class="sxs-lookup"><span data-stu-id="5e042-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="5e042-146">Aucune page d’erreur personnalisée configurée, ni aucun filtre ou intergiciel (middleware) de gestion des exceptions, n’affecte ce comportement.</span><span class="sxs-lookup"><span data-stu-id="5e042-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="5e042-147">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="5e042-147">Startup exception handling</span></span>

<span data-ttu-id="5e042-148">Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="5e042-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="5e042-149">Vous pouvez [configurer le comportement de l’hôte en réponse aux erreurs au moment du démarrage](hosting.md#detailed-errors) à l’aide de `captureStartupErrors` et de la clé `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="5e042-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="5e042-150">La couche d’hébergement ne peut afficher une page d’erreur pour une erreur de démarrage capturée que si celle-ci se produit une fois établie la liaison entre l’adresse de l’hôte et le port.</span><span class="sxs-lookup"><span data-stu-id="5e042-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="5e042-151">Si une liaison échoue pour une raison quelconque, la couche d’hébergement journalise une exception critique, le processus dotnet échoue et aucune page d’erreur ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="5e042-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="5e042-152">Gestion des erreurs MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5e042-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="5e042-153">Les applications [MVC](xref:mvc/overview) ont des options supplémentaires pour la gestion des erreurs, telles que la configuration de filtres d’exception et l’exécution d’une validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="5e042-153">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="5e042-154">Filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="5e042-154">Exception Filters</span></span>

<span data-ttu-id="5e042-155">Vous pouvez configurer les filtres d’exception globalement ou en fonction d’un contrôleur ou d’une action dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="5e042-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="5e042-156">Ces filtres sont uniquement appelés pour gérer les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="5e042-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="5e042-157">Découvrez-en plus sur les filtres d’exception dans [Filtres](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="5e042-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="5e042-158">Les filtres d’exception sont adaptés pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que l’intergiciel (middleware) de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="5e042-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="5e042-159">En règle générale, préférez l’intergiciel (middleware), et n’utilisez des filtres que si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie.</span><span class="sxs-lookup"><span data-stu-id="5e042-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="5e042-160">Gestion des erreurs d’état de modèle</span><span class="sxs-lookup"><span data-stu-id="5e042-160">Handling Model State Errors</span></span>

<span data-ttu-id="5e042-161">La [validation de modèle](../mvc/models/validation.md) se produit avant l’appel de chaque action du contrôleur ; il appartient à la méthode d’action d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="5e042-161">[Model validation](../mvc/models/validation.md) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="5e042-162">Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de validation de modèle ; dans ce cas, un [filtre](../mvc/controllers/filters.md) peut être un emplacement approprié pour implémenter une telle stratégie.</span><span class="sxs-lookup"><span data-stu-id="5e042-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="5e042-163">Vous devez tester le comportement de vos actions avec les états de modèles non valide.</span><span class="sxs-lookup"><span data-stu-id="5e042-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="5e042-164">Pour en savoir plus, consultez [Tester la logique du contrôleur](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="5e042-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



