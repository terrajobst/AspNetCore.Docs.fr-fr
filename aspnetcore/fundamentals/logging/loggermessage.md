---
title: Journalisation avancée avec LoggerMessage dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser LoggerMessage pour créer des délégués pouvant être mis en cache et nécessitant moins d’allocations d’objets pour les scénarios de journalisation à hautes performances.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 7a030b4bb754f65f8d93e51f203344c2dc02a634
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809261"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="dd8e3-103">Journalisation avancée avec LoggerMessage dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd8e3-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="dd8e3-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dd8e3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dd8e3-105">Les fonctionnalités <xref:Microsoft.Extensions.Logging.LoggerMessage> créent des délégués pouvant être mis en cache qui nécessitent moins d’allocations d’objet et de charge de calcul par rapport aux [méthodes d’extension de journaliseur](xref:Microsoft.Extensions.Logging.LoggerExtensions), telles que <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> et <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-105"><xref:Microsoft.Extensions.Logging.LoggerMessage> features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](xref:Microsoft.Extensions.Logging.LoggerExtensions), such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span></span> <span data-ttu-id="dd8e3-106">Pour les scénarios de journalisation hautes performances, utilisez le modèle <xref:Microsoft.Extensions.Logging.LoggerMessage>.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-106">For high-performance logging scenarios, use the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

<span data-ttu-id="dd8e3-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> procure les avantages suivants en termes de performances par rapport aux méthodes d’extension de journaliseur :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="dd8e3-108">Les méthodes d’extension de journaliseur nécessitent la conversion (« boxing ») de types de valeur, tels que `int`, en `object`.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="dd8e3-109">Utilisant des champs <xref:System.Action> statiques et des méthodes d’extension avec des paramètres fortement typés, le modèle <xref:Microsoft.Extensions.Logging.LoggerMessage> évite le boxing.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-109">The <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern avoids boxing by using static <xref:System.Action> fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="dd8e3-110">Les méthodes d’extension de journaliseur doivent analyser le modèle de message (chaîne de format nommé) chaque fois qu’un message de journal est écrit.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="dd8e3-111"><xref:Microsoft.Extensions.Logging.LoggerMessage> requiert l’analyse d’un modèle une seule fois quand le message est défini.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-111"><xref:Microsoft.Extensions.Logging.LoggerMessage> only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="dd8e3-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/2.x/LoggerMessageSamples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dd8e3-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/2.x/LoggerMessageSamples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dd8e3-113">L’exemple d’application illustre les fonctionnalités <xref:Microsoft.Extensions.Logging.LoggerMessage> avec un système de suivi de citations de base.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-113">The sample app demonstrates <xref:Microsoft.Extensions.Logging.LoggerMessage> features with a basic quote tracking system.</span></span> <span data-ttu-id="dd8e3-114">L’application ajoute et supprime des citations à l’aide d’une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="dd8e3-115">À mesure que ces opérations se produisent, des messages de journal sont générés à l’aide du modèle <xref:Microsoft.Extensions.Logging.LoggerMessage>.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-115">As these operations occur, log messages are generated using the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="dd8e3-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="dd8e3-116">LoggerMessage.Define</span></span>

<span data-ttu-id="dd8e3-117">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) crée un délégué <xref:System.Action> pour la journalisation d’un message.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-117">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) creates an <xref:System.Action> delegate for logging a message.</span></span> <span data-ttu-id="dd8e3-118">Les surcharges <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> permettent de passer jusqu’à six paramètres de type à une chaîne de format nommée (modèle).</span><span class="sxs-lookup"><span data-stu-id="dd8e3-118"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="dd8e3-119">La chaîne fournie à la méthode <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> est un modèle et non pas une chaîne interpolée.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-119">The string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="dd8e3-120">Les espaces réservés sont remplis dans l’ordre dans lequel les types sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="dd8e3-121">Les noms d’espace réservé dans le modèle doivent être descriptifs et cohérents d’un modèle à l’autre.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="dd8e3-122">Ils servent de noms de propriété dans les données de journal structurées.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="dd8e3-123">Nous vous recommandons d’utiliser la [casse Pascal](/dotnet/standard/design-guidelines/capitalization-conventions) pour les noms d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="dd8e3-124">Par exemple, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="dd8e3-125">Chaque message de journal est une <xref:System.Action> contenue dans un champ statique créé par [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span><span class="sxs-lookup"><span data-stu-id="dd8e3-125">Each log message is an <xref:System.Action> held in a static field created by [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span></span> <span data-ttu-id="dd8e3-126">Par exemple, l’exemple d’application crée un champ afin de décrire un message de journal pour une demande GET pour la page Index (*Internal/LoggerExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="dd8e3-127">Pour l’<xref:System.Action>, spécifiez :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-127">For the <xref:System.Action>, specify:</span></span>

* <span data-ttu-id="dd8e3-128">Le niveau du journal</span><span class="sxs-lookup"><span data-stu-id="dd8e3-128">The log level.</span></span>
* <span data-ttu-id="dd8e3-129">Un identificateur d’événement unique (<xref:Microsoft.Extensions.Logging.EventId>) avec le nom de la méthode d’extension statique</span><span class="sxs-lookup"><span data-stu-id="dd8e3-129">A unique event identifier (<xref:Microsoft.Extensions.Logging.EventId>) with the name of the static extension method.</span></span>
* <span data-ttu-id="dd8e3-130">Le modèle de message (chaîne de format nommée)</span><span class="sxs-lookup"><span data-stu-id="dd8e3-130">The message template (named format string).</span></span> 

<span data-ttu-id="dd8e3-131">Une demande pour la page Index de l’exemple d’application définit :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="dd8e3-132">Le niveau de journal sur `Information`</span><span class="sxs-lookup"><span data-stu-id="dd8e3-132">Log level to `Information`.</span></span>
* <span data-ttu-id="dd8e3-133">L’ID d’événement sur `1` avec le nom de la méthode `IndexPageRequested`</span><span class="sxs-lookup"><span data-stu-id="dd8e3-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="dd8e3-134">Le modèle de message (chaîne de format nommée) sur une chaîne</span><span class="sxs-lookup"><span data-stu-id="dd8e3-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="dd8e3-135">Des magasins de journalisation structurée peuvent utiliser le nom d’événement quand il est fourni avec l’ID d’événement pour enrichir la journalisation.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="dd8e3-136">Par exemple, [Serilog](https://github.com/serilog/serilog-extensions-logging) utilise le nom d’événement.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="dd8e3-137">L’<xref:System.Action> est appelée par le biais d’une méthode d’extension fortement typée.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-137">The <xref:System.Action> is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="dd8e3-138">La méthode `IndexPageRequested` journalise un message pour une demande GET pour la page Index dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="dd8e3-139">`IndexPageRequested` est appelé sur le journaliseur dans la méthode `OnGetAsync` dans *Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="dd8e3-140">Examinez la sortie de la console de l’application :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="dd8e3-141">Pour passer des paramètres à un message de journal, définissez jusqu’à six types au moment de la création du champ statique.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="dd8e3-142">L’exemple d’application journalise une chaîne au moment de l’ajout d’une citation en définissant un type `string` pour le champ <xref:System.Action> :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-142">The sample app logs a string when adding a quote by defining a `string` type for the <xref:System.Action> field:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="dd8e3-143">Le modèle de message de journal du délégué reçoit ses valeurs d’espace réservé des types fournis.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="dd8e3-144">L’exemple d’application définit un délégué pour l’ajout d’une citation, où le paramètre Quote est de type `string` :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="dd8e3-145">La méthode d’extension statique pour l’ajout d’une citation, `QuoteAdded`, reçoit la valeur d’argument quote et la passe au délégué <xref:System.Action> :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the <xref:System.Action> delegate:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="dd8e3-146">Dans le modèle de page de la page Index (*Pages/Index.cshtml.cs*), `QuoteAdded` est appelé pour journaliser le message :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="dd8e3-147">Examinez la sortie de la console de l’application :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="dd8e3-148">L’exemple d’application implémente un modèle [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) pour la suppression de citations.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-148">The sample app implements a [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) pattern for quote deletion.</span></span> <span data-ttu-id="dd8e3-149">Un message d’information est journalisé chaque fois qu’une opération de suppression réussit.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="dd8e3-150">Un message d’erreur est journalisé chaque fois qu’une opération de suppression donne lieu à la levée d’une exception.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="dd8e3-151">Le message de journal lié à l’échec d’une opération de suppression inclut la trace des exceptions (*Internal/LoggerExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="dd8e3-152">Notez la manière dont l’exception est passée au délégué dans `QuoteDeleteFailed` :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="dd8e3-153">Dans le modèle de page pour la page Index, la réussite de la suppression d’une citation se traduit par l’appel de la méthode `QuoteDeleted` sur le journaliseur.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="dd8e3-154">Quand une citation à supprimer n’est pas trouvée, une <xref:System.ArgumentNullException> est levée.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-154">When a quote isn't found for deletion, an <xref:System.ArgumentNullException> is thrown.</span></span> <span data-ttu-id="dd8e3-155">L’exception est interceptée par l’instruction [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) et journalisée par le biais de l’appel de la méthode `QuoteDeleteFailed` sur le journaliseur dans le bloc [catch](/dotnet/csharp/language-reference/keywords/try-catch) (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-155">The exception is trapped by the [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) statement and logged by calling the `QuoteDeleteFailed` method on the logger in the [catch](/dotnet/csharp/language-reference/keywords/try-catch) block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="dd8e3-156">Quand une citation est correctement supprimée, voici à quoi ressemble la sortie de la console de l’application :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="dd8e3-157">Quand la suppression d’une citation échoue, voici à quoi ressemble la sortie de la console de l’application.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="dd8e3-158">Notez que l’exception est incluse dans le message de journal :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-158">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T]
       (T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() 
      in <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="dd8e3-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="dd8e3-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="dd8e3-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) crée un délégué <xref:System.Func%601> pour la définition d’une [étendue de journal](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="dd8e3-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) creates a <xref:System.Func%601> delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="dd8e3-161">Les surcharges <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> permettent de passer jusqu’à trois paramètres de type à une chaîne de format nommée (modèle).</span><span class="sxs-lookup"><span data-stu-id="dd8e3-161"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="dd8e3-162">Comme c’est le cas avec la méthode <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*>, la chaîne fournie à la méthode <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> est un modèle et non pas une chaîne interpolée.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-162">As is the case with the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method, the string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="dd8e3-163">Les espaces réservés sont remplis dans l’ordre dans lequel les types sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="dd8e3-164">Les noms d’espace réservé dans le modèle doivent être descriptifs et cohérents d’un modèle à l’autre.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="dd8e3-165">Ils servent de noms de propriété dans les données de journal structurées.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="dd8e3-166">Nous vous recommandons d’utiliser la [casse Pascal](/dotnet/standard/design-guidelines/capitalization-conventions) pour les noms d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="dd8e3-167">Par exemple, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="dd8e3-168">Définissez une [étendue de journal](xref:fundamentals/logging/index#log-scopes) à appliquer à une série de messages de journal à l’aide de la méthode <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method.</span></span>

<span data-ttu-id="dd8e3-169">L’exemple d’application a un bouton **Clear All** (Effacer tout) pour supprimer toutes les citations de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="dd8e3-170">Les citations sont supprimées une par une.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="dd8e3-171">Chaque fois qu’une citation est supprimée, la méthode `QuoteDeleted` est appelée sur le journaliseur.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="dd8e3-172">Une étendue de journal est ajoutée à ces messages de journal.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="dd8e3-173">Activez `IncludeScopes` dans la section du journaliseur de console *d’appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

<span data-ttu-id="dd8e3-174">Pour créer une étendue de journal, ajoutez un champ destiné à contenir un délégué <xref:System.Func%601> pour l’étendue.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-174">To create a log scope, add a field to hold a <xref:System.Func%601> delegate for the scope.</span></span> <span data-ttu-id="dd8e3-175">L’exemple d’application crée un champ intitulé `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="dd8e3-176">Utilisez <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> pour créer le délégué.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-176">Use <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> to create the delegate.</span></span> <span data-ttu-id="dd8e3-177">Vous pouvez spécifier jusqu’à trois types à utiliser comme arguments de modèle quand le délégué est appelé.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="dd8e3-178">L’exemple d’application utilise un modèle de message qui inclut le nombre de citations supprimées (un type `int`) :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="dd8e3-179">Fournissez une méthode d’extension statique pour le message de journal.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="dd8e3-180">Incluez tous les paramètres de type pour les propriétés nommées qui s’affichent dans le modèle de message.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="dd8e3-181">L’exemple d’application prend un `count` de citations à supprimer et retourne `_allQuotesDeletedScope` :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="dd8e3-182">L’étendue inclut les appels d’extension de journalisation dans un bloc [using](/dotnet/csharp/language-reference/keywords/using-statement) :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-182">The scope wraps the logging extension calls in a [using](/dotnet/csharp/language-reference/keywords/using-statement) block:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="dd8e3-183">Examinez les messages de journal dans la sortie de la console de l’application.</span><span class="sxs-lookup"><span data-stu-id="dd8e3-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="dd8e3-184">Le résultat suivant montre trois citations supprimées, message d’étendue de journal compris :</span><span class="sxs-lookup"><span data-stu-id="dd8e3-184">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a><span data-ttu-id="dd8e3-185">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dd8e3-185">Additional resources</span></span>

* [<span data-ttu-id="dd8e3-186">Journalisation</span><span class="sxs-lookup"><span data-stu-id="dd8e3-186">Logging</span></span>](xref:fundamentals/logging/index)
