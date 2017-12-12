---
title: Journalisation de hautes performances avec LoggerMessage dans ASP.NET Core
author: guardrex
description: "Découvrez comment utiliser les fonctionnalités de LoggerMessage pour créer des délégués pouvant être nécessitant moins d’allocations objet celle des méthodes d’extension d’enregistreur d’événements pour les scénarios de journalisation de hautes performances."
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="b71b2-103">Journalisation de hautes performances avec LoggerMessage dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b71b2-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="b71b2-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b71b2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b71b2-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) fonctionnalités créer pouvant être délégués qui nécessitent moins d’allocations objets et réduit la charge de calcul à [les méthodes d’extension d’enregistreur d’événements](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), tel que `LogInformation`, `LogDebug`et `LogError`.</span><span class="sxs-lookup"><span data-stu-id="b71b2-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="b71b2-106">Pour les scénarios de journalisation de hautes performances, utilisez la `LoggerMessage` modèle.</span><span class="sxs-lookup"><span data-stu-id="b71b2-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="b71b2-107">`LoggerMessage`fournit les avantages de performances suivants sur les méthodes d’extension d’enregistreur d’événements :</span><span class="sxs-lookup"><span data-stu-id="b71b2-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="b71b2-108">Méthodes d’extension d’enregistreur d’événements nécessitent des types de valeur « conversion boxing » (conversion), tel que `int`, dans `object`.</span><span class="sxs-lookup"><span data-stu-id="b71b2-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="b71b2-109">Le `LoggerMessage` boxing évite de modèle à l’aide de static `Action` champs et méthodes d’extension avec des paramètres fortement typés.</span><span class="sxs-lookup"><span data-stu-id="b71b2-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="b71b2-110">Méthodes d’extension d’enregistreur d’événements doivent analyser le modèle de message (chaîne de format nommé) chaque fois qu’un message de journal est écrit.</span><span class="sxs-lookup"><span data-stu-id="b71b2-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="b71b2-111">`LoggerMessage`ne nécessite que l’analyse d’un modèle une fois lorsque le message est défini.</span><span class="sxs-lookup"><span data-stu-id="b71b2-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="b71b2-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b71b2-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b71b2-113">L’exemple d’application montre `LoggerMessage` fonctionnalités avec une demande de base système de suivi.</span><span class="sxs-lookup"><span data-stu-id="b71b2-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="b71b2-114">L’application ajoute et supprime les guillemets doubles à l’aide d’une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="b71b2-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="b71b2-115">Comme ces opérations se produisent, des messages de journal sont générées à l’aide de la `LoggerMessage` modèle.</span><span class="sxs-lookup"><span data-stu-id="b71b2-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="b71b2-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="b71b2-116">LoggerMessage.Define</span></span>

<span data-ttu-id="b71b2-117">[Définir (LogLevel, ID d’événement, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) crée un `Action` délégué pour la journalisation d’un message.</span><span class="sxs-lookup"><span data-stu-id="b71b2-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="b71b2-118">`Define`surcharges autorisent le passage de paramètres de type jusqu'à six à une chaîne de format nommée (modèle).</span><span class="sxs-lookup"><span data-stu-id="b71b2-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="b71b2-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="b71b2-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="b71b2-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) crée un `Func` delegate pour définir un [connecter étendue](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="b71b2-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="b71b2-121">`DefineScope`surcharges autorisent le passage de jusqu'à trois paramètres de type à une chaîne de format nommée (modèle).</span><span class="sxs-lookup"><span data-stu-id="b71b2-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="b71b2-122">Modèle de message (nommé la chaîne de format)</span><span class="sxs-lookup"><span data-stu-id="b71b2-122">Message template (named format string)</span></span>

<span data-ttu-id="b71b2-123">La chaîne fournie pour le `Define` et `DefineScope` méthodes est un modèle et pas une chaîne interpolée.</span><span class="sxs-lookup"><span data-stu-id="b71b2-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="b71b2-124">Les espaces réservés sont remplis dans l’ordre que les types sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="b71b2-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="b71b2-125">Noms d’espace réservé dans le modèle doivent être descriptif et cohérent entre les modèles.</span><span class="sxs-lookup"><span data-stu-id="b71b2-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="b71b2-126">Ils servent de noms de propriété dans les données structurées de journal.</span><span class="sxs-lookup"><span data-stu-id="b71b2-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="b71b2-127">Nous vous recommandons de [Pascal casse](/dotnet/standard/design-guidelines/capitalization-conventions) pour les noms d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="b71b2-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="b71b2-128">Par exemple, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="b71b2-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="b71b2-129">Implémentation de LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="b71b2-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="b71b2-130">Chaque message du journal est une `Action` contenues dans un champ statique créé par `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="b71b2-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="b71b2-131">Par exemple, l’exemple d’application crée un champ pour décrire un message de journal pour une demande GET pour la page d’Index (*Internal/LoggerExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b71b2-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="b71b2-132">Pour le `Action`, spécifiez :</span><span class="sxs-lookup"><span data-stu-id="b71b2-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="b71b2-133">Le niveau de journal.</span><span class="sxs-lookup"><span data-stu-id="b71b2-133">The log level.</span></span>
* <span data-ttu-id="b71b2-134">Un identificateur d’événement unique ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) avec le nom de la méthode d’extension statique.</span><span class="sxs-lookup"><span data-stu-id="b71b2-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="b71b2-135">Le modèle de message (nommé la chaîne de format).</span><span class="sxs-lookup"><span data-stu-id="b71b2-135">The message template (named format string).</span></span> 

<span data-ttu-id="b71b2-136">Une demande de la page d’Index de l’application d’exemple définit le :</span><span class="sxs-lookup"><span data-stu-id="b71b2-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="b71b2-137">Ouvrez une session au niveau de `Information`.</span><span class="sxs-lookup"><span data-stu-id="b71b2-137">Log level to `Information`.</span></span>
* <span data-ttu-id="b71b2-138">Id d’événement de `1` avec le nom de la `IndexPageRequested` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b71b2-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="b71b2-139">Modèle de message (nommé la chaîne de format) en une chaîne.</span><span class="sxs-lookup"><span data-stu-id="b71b2-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="b71b2-140">Magasins de journalisation structuré peuvent utiliser le nom d’événement lorsqu’il est fourni avec l’id d’événement d’enrichir la journalisation.</span><span class="sxs-lookup"><span data-stu-id="b71b2-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="b71b2-141">Par exemple, [Serilog](https://github.com/serilog/serilog-extensions-logging) utilise le nom de l’événement.</span><span class="sxs-lookup"><span data-stu-id="b71b2-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="b71b2-142">Le `Action` est appelé via une méthode d’extension de fortement typée.</span><span class="sxs-lookup"><span data-stu-id="b71b2-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="b71b2-143">Le `IndexPageRequested` méthode consigne un message pour une demande d’obtention de page Index dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="b71b2-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="b71b2-144">`IndexPageRequested`est appelé sur l’enregistreur d’événements dans le `OnGetAsync` méthode dans *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b71b2-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="b71b2-145">Examinez la sortie de l’application console :</span><span class="sxs-lookup"><span data-stu-id="b71b2-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="b71b2-146">Pour passer des paramètres à un message de journal, définir jusqu'à six types lors de la création du champ statique.</span><span class="sxs-lookup"><span data-stu-id="b71b2-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="b71b2-147">L’exemple d’application enregistre une chaîne lors de l’ajout d’un guillemet en définissant un `string` de type pour le `Action` champ :</span><span class="sxs-lookup"><span data-stu-id="b71b2-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="b71b2-148">Modèle de message du journal du délégué reçoit ses valeurs d’espace réservé à partir des types fournis.</span><span class="sxs-lookup"><span data-stu-id="b71b2-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="b71b2-149">L’exemple d’application définit un délégué pour l’ajout d’un guillemet, où le paramètre de devis est un `string`:</span><span class="sxs-lookup"><span data-stu-id="b71b2-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="b71b2-150">La méthode d’extension statique pour l’ajout d’un guillemet, `QuoteAdded`, reçoit la valeur d’argument devis et passe à la `Action` délégué :</span><span class="sxs-lookup"><span data-stu-id="b71b2-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="b71b2-151">Dans le fichier de code-behind de la page d’Index (*Pages/Index.cshtml.cs*), `QuoteAdded` est appelée pour enregistrer le message :</span><span class="sxs-lookup"><span data-stu-id="b71b2-151">In the Index page's code-behind file (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="b71b2-152">Examinez la sortie de l’application console :</span><span class="sxs-lookup"><span data-stu-id="b71b2-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="b71b2-153">L’application exemple implémente un `try` &ndash; `catch` modèle pour la suppression de devis.</span><span class="sxs-lookup"><span data-stu-id="b71b2-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="b71b2-154">Un message d’information est consigné pour une opération de suppression réussit.</span><span class="sxs-lookup"><span data-stu-id="b71b2-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="b71b2-155">Un message d’erreur est enregistré pour une opération de suppression lorsqu’une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="b71b2-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="b71b2-156">Le message du journal pour l’échec de suppression opération inclut la trace de pile d’exception (*Internal/LoggerExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b71b2-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="b71b2-157">Notez la manière dont l’exception est passée au délégué dans `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="b71b2-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="b71b2-158">Dans le fichier Index page code-behind, une suppression réussie de devis appelle la `QuoteDeleted` méthode sur l’enregistreur d’événements.</span><span class="sxs-lookup"><span data-stu-id="b71b2-158">In the Index page code-behind, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="b71b2-159">Lorsqu’un guillemet n’est pas trouvé pour la suppression, un `ArgumentNullException` est levée.</span><span class="sxs-lookup"><span data-stu-id="b71b2-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="b71b2-160">L’exception est interceptée par le `try` &ndash; `catch` instruction et enregistrés en appelant le `QuoteDeleteFailed` méthode sur l’enregistreur d’événements dans le `catch` bloc (*Pages/Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b71b2-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="b71b2-161">Lorsqu’un devis est supprimé avec succès, examinez la sortie de la console de l’application :</span><span class="sxs-lookup"><span data-stu-id="b71b2-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="b71b2-162">En cas d’échec de la suppression de devis, inspecter la sortie de la console de l’application.</span><span class="sxs-lookup"><span data-stu-id="b71b2-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="b71b2-163">Notez que l’exception est incluse dans le message du journal :</span><span class="sxs-lookup"><span data-stu-id="b71b2-163">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="b71b2-164">Implémentation de LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="b71b2-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="b71b2-165">Définir un [connecter étendue](xref:fundamentals/logging/index#log-scopes) à appliquer à une série de messages du journal à l’aide de la [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) (méthode).</span><span class="sxs-lookup"><span data-stu-id="b71b2-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="b71b2-166">L’exemple d’application a un **Effacer tout** bouton pour supprimer tous les guillemets dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b71b2-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="b71b2-167">Les guillemets sont supprimés en supprimant une à la fois.</span><span class="sxs-lookup"><span data-stu-id="b71b2-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="b71b2-168">Chaque fois qu’un guillemet est supprimé, le `QuoteDeleted` méthode est appelée sur l’enregistreur d’événements.</span><span class="sxs-lookup"><span data-stu-id="b71b2-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="b71b2-169">Une étendue de journal est ajoutée à ces messages du journal.</span><span class="sxs-lookup"><span data-stu-id="b71b2-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="b71b2-170">Activer `IncludeScopes` dans les options de journalisation de console :</span><span class="sxs-lookup"><span data-stu-id="b71b2-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="b71b2-171">Paramètre `IncludeScopes` est requis dans les applications ASP.NET Core 2.0 pour activer des étendues de journal.</span><span class="sxs-lookup"><span data-stu-id="b71b2-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="b71b2-172">Paramètre `IncludeScopes` via *appsettings* les fichiers de configuration est une fonctionnalité qui a planifié pour la version de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b71b2-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="b71b2-173">L’exemple d’application efface les autres fournisseurs et ajoute les filtres afin de réduire la sortie de journalisation.</span><span class="sxs-lookup"><span data-stu-id="b71b2-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="b71b2-174">Cela rend plus facile de voir les messages du journal de l’exemple qui illustrent `LoggerMessage` fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="b71b2-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="b71b2-175">Pour créer une étendue de journal, ajoutez un champ pour contenir un `Func` delegate pour l’étendue.</span><span class="sxs-lookup"><span data-stu-id="b71b2-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="b71b2-176">L’exemple d’application crée un champ intitulé `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b71b2-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="b71b2-177">Utilisez `DefineScope` pour créer le délégué.</span><span class="sxs-lookup"><span data-stu-id="b71b2-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="b71b2-178">Jusqu'à trois types peuvent être spécifiés pour une utilisation en tant qu’arguments de modèle lorsque le délégué est appelé.</span><span class="sxs-lookup"><span data-stu-id="b71b2-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="b71b2-179">L’exemple d’application utilise un modèle de message qui inclut le nombre de devis supprimés (un `int` type) :</span><span class="sxs-lookup"><span data-stu-id="b71b2-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="b71b2-180">Fournir une méthode d’extension statique pour le message du journal.</span><span class="sxs-lookup"><span data-stu-id="b71b2-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="b71b2-181">Inclure tous les paramètres de type pour les propriétés nommées qui s’affichent dans le modèle de message.</span><span class="sxs-lookup"><span data-stu-id="b71b2-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="b71b2-182">L’exemple d’application utilise un `count` de devis à supprimer et renvoie `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="b71b2-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="b71b2-183">Le retour automatique à la portée l’extension de journalisation des appels dans un `using` bloc :</span><span class="sxs-lookup"><span data-stu-id="b71b2-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="b71b2-184">Examinez les messages du journal dans la sortie de la console de l’application.</span><span class="sxs-lookup"><span data-stu-id="b71b2-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="b71b2-185">Le résultat suivant montre les trois devis supprimés avec le message d’étendue de journal inclus :</span><span class="sxs-lookup"><span data-stu-id="b71b2-185">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="see-also"></a><span data-ttu-id="b71b2-186">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="b71b2-186">See also</span></span>

* [<span data-ttu-id="b71b2-187">Journalisation</span><span class="sxs-lookup"><span data-stu-id="b71b2-187">Logging</span></span>](xref:fundamentals/logging/index)
