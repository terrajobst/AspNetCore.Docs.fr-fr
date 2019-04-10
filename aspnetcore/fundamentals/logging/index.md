---
title: Journalisation dans ASP.NET Core
author: tdykstra
description: Découvrez plus en détail le framework de journalisation dans ASP.NET Core. Apprenez à utiliser les fournisseurs de journalisation intégrés et découvrez-en plus sur les fournisseurs tiers les plus courants.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 065b2016d3a2dcc2243ec6869e027c5fabe4dad8
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068402"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="182a5-104">Journalisation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="182a5-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="182a5-105">Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="182a5-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="182a5-106">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec différents fournisseurs de journalisation tiers et intégrés.</span><span class="sxs-lookup"><span data-stu-id="182a5-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="182a5-107">Cet article explique comment utiliser cette API de journalisation avec les fournisseurs de journalisation intégrés.</span><span class="sxs-lookup"><span data-stu-id="182a5-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="182a5-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="182a5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="182a5-109">Ajouter des fournisseurs</span><span class="sxs-lookup"><span data-stu-id="182a5-109">Add providers</span></span>

<span data-ttu-id="182a5-110">Un fournisseur de journalisation affiche ou stocke des journaux.</span><span class="sxs-lookup"><span data-stu-id="182a5-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="182a5-111">Par exemple, le fournisseur Console affiche les journaux dans la console, et le fournisseur Azure Application Insights les stocke dans Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="182a5-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="182a5-112">Il est possible d’envoyer les journaux à plusieurs endroits en ajoutant plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="182a5-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="182a5-113">Pour ajouter un fournisseur, appelez la méthode d’extension `Add{provider name}` du fournisseur dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="182a5-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="182a5-114">Le modèle de projet par défaut appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, qui ajoute les fournisseurs de journalisation suivants :</span><span class="sxs-lookup"><span data-stu-id="182a5-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="182a5-115">Console</span><span class="sxs-lookup"><span data-stu-id="182a5-115">Console</span></span>
* <span data-ttu-id="182a5-116">Débogage</span><span class="sxs-lookup"><span data-stu-id="182a5-116">Debug</span></span>
* <span data-ttu-id="182a5-117">EventSource (à partir d’ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="182a5-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="182a5-118">Si vous utilisez `CreateDefaultBuilder`, vous pouvez remplacer les fournisseurs par défaut par ceux de votre choix.</span><span class="sxs-lookup"><span data-stu-id="182a5-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="182a5-119">Appelez <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> et ajoutez les fournisseurs que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="182a5-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="182a5-120">Pour utiliser un fournisseur, installez son package NuGet et appelez sa méthode d’extension sur une instance de <xref:Microsoft.Extensions.Logging.ILoggerFactory> :</span><span class="sxs-lookup"><span data-stu-id="182a5-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="182a5-121">[L’injection de dépendances](xref:fundamentals/dependency-injection) ASP.NET Core fournit l’instance `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="182a5-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="182a5-122">Les méthodes d’extension `AddConsole` et `AddDebug` sont définies dans les packages [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) et [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="182a5-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="182a5-123">Chaque méthode d’extension appelle la méthode `ILoggerFactory.AddProvider`, en passant une instance du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="182a5-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="182a5-124">L’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) ajoute des fournisseurs de journalisation dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="182a5-124">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="182a5-125">Pour obtenir la sortie de journal du code exécuté plus haut, ajoutez les fournisseurs de journalisation au constructeur de classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="182a5-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="182a5-126">Vous trouverez des informations sur les [fournisseurs de journalisation intégrés](#built-in-logging-providers) et les [fournisseurs de journalisation tiers](#third-party-logging-providers) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="182a5-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="182a5-127">Créer des journaux</span><span class="sxs-lookup"><span data-stu-id="182a5-127">Create logs</span></span>

<span data-ttu-id="182a5-128">Récupérez un objet <xref:Microsoft.Extensions.Logging.ILogger%601> auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="182a5-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="182a5-129">L’exemple de contrôleur suivant crée des journaux `Information` et `Warning`.</span><span class="sxs-lookup"><span data-stu-id="182a5-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="182a5-130">La *catégorie* est `TodoApiSample.Controllers.TodoController` (le nom de classe complet de `TodoController` dans l’exemple d’application) :</span><span class="sxs-lookup"><span data-stu-id="182a5-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="182a5-131">L’exemple Razor Pages suivant crée des journaux de *niveau* `Information` et de *catégorie* `TodoApiSample.Pages.AboutModel` :</span><span class="sxs-lookup"><span data-stu-id="182a5-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="182a5-132">L’exemple précédent crée des journaux de *niveau* `Information` et `Warning` et de *catégorie* classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="182a5-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="182a5-133">Le *niveau* du journal indique la gravité de l’événement consigné.</span><span class="sxs-lookup"><span data-stu-id="182a5-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="182a5-134">La *catégorie* du journal est une chaîne associée à chaque journal.</span><span class="sxs-lookup"><span data-stu-id="182a5-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="182a5-135">L’instance `ILogger<T>` crée des journaux ayant comme catégorie un nom complet de type `T`.</span><span class="sxs-lookup"><span data-stu-id="182a5-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="182a5-136">Les [niveaux](#log-level) et les [catégories](#log-category) sont expliqués plus en détail plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="182a5-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="182a5-137">Créer des journaux au démarrage</span><span class="sxs-lookup"><span data-stu-id="182a5-137">Create logs in Startup</span></span>

<span data-ttu-id="182a5-138">Pour écrire des journaux dans la classe `Startup`, ajoutez un paramètre `ILogger` dans la signature de constructeur :</span><span class="sxs-lookup"><span data-stu-id="182a5-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="182a5-139">Créer des journaux dans le programme</span><span class="sxs-lookup"><span data-stu-id="182a5-139">Create logs in Program</span></span>

<span data-ttu-id="182a5-140">Pour écrire des journaux dans la classe `Program`, récupérez une instance `ILogger` auprès de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="182a5-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="182a5-141">Aucune méthode d’enregistreur d’événements asynchrone</span><span class="sxs-lookup"><span data-stu-id="182a5-141">No asynchronous logger methods</span></span>

<span data-ttu-id="182a5-142">La journalisation doit être suffisamment rapide par rapport au coût du code asynchrone en matière de performances.</span><span class="sxs-lookup"><span data-stu-id="182a5-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="182a5-143">Si votre magasin de données de journalisation est lent, n’écrivez pas directement dedans.</span><span class="sxs-lookup"><span data-stu-id="182a5-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="182a5-144">Écrivez au départ les messages de journal dans un magasin rapide, puis déplacez-les dans le magasin lent.</span><span class="sxs-lookup"><span data-stu-id="182a5-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="182a5-145">Par exemple, si vous vous connectez à SQL Server, évitez de le faire directement dans une méthode `Log`, car les méthodes `Log` sont synchrones.</span><span class="sxs-lookup"><span data-stu-id="182a5-145">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="182a5-146">Ajoutez plutôt de façon synchronisée des messages de journal à une file d’attente en mémoire, puis configurez un traitement en arrière-plan afin d’extraire les messages de la file d’attente et d’effectuer le travail asynchrone d’envoi des données vers SQL Server.</span><span class="sxs-lookup"><span data-stu-id="182a5-146">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="182a5-147">Configuration</span><span class="sxs-lookup"><span data-stu-id="182a5-147">Configuration</span></span>

<span data-ttu-id="182a5-148">La configuration de fournisseur de journalisation est fournie par un ou plusieurs fournisseurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="182a5-148">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="182a5-149">Formats de fichiers (INI, JSON et XML).</span><span class="sxs-lookup"><span data-stu-id="182a5-149">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="182a5-150">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="182a5-150">Command-line arguments.</span></span>
* <span data-ttu-id="182a5-151">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="182a5-151">Environment variables.</span></span>
* <span data-ttu-id="182a5-152">Objets .NET en mémoire.</span><span class="sxs-lookup"><span data-stu-id="182a5-152">In-memory .NET objects.</span></span>
* <span data-ttu-id="182a5-153">Stockage [Secret Manager](xref:security/app-secrets) non chiffré.</span><span class="sxs-lookup"><span data-stu-id="182a5-153">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="182a5-154">Magasin utilisateur chiffré comme [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="182a5-154">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="182a5-155">Fournisseurs personnalisés (installés ou créés).</span><span class="sxs-lookup"><span data-stu-id="182a5-155">Custom providers (installed or created).</span></span>

<span data-ttu-id="182a5-156">Par exemple, la configuration de journalisation est généralement fournie par la section `Logging` des fichiers de paramètres d’application.</span><span class="sxs-lookup"><span data-stu-id="182a5-156">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="182a5-157">L’exemple suivant montre le contenu d’un fichier *appsettings.Development.json* standard :</span><span class="sxs-lookup"><span data-stu-id="182a5-157">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

<span data-ttu-id="182a5-158">La propriété `Logging` peut avoir un niveau `LogLevel` et des propriétés de module fournisseur d'informations (Console ici).</span><span class="sxs-lookup"><span data-stu-id="182a5-158">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="182a5-159">La propriété `LogLevel` sous `Logging` spécifie le [niveau](#log-level) de journalisation minimal pour les catégories sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="182a5-159">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="182a5-160">Dans l’exemple, les catégories `System` et `Microsoft` sont consignées au niveau `Information`, et toutes les autres au niveau `Debug`.</span><span class="sxs-lookup"><span data-stu-id="182a5-160">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="182a5-161">Les autres propriétés sous `Logging` spécifient les fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="182a5-161">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="182a5-162">Cet exemple concerne le fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="182a5-162">The example is for the Console provider.</span></span> <span data-ttu-id="182a5-163">Lorsqu’un fournisseur prend en charge les [étendues de journal](#log-scopes), `IncludeScopes` indique si elles sont activées.</span><span class="sxs-lookup"><span data-stu-id="182a5-163">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="182a5-164">Une propriété de fournisseur (comme `Console` dans l’exemple) peut également spécifier une propriété `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="182a5-164">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> `LogLevel` <span data-ttu-id="182a5-165">sous un fournisseur spécifie les niveaux à consigner pour ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="182a5-165">under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="182a5-166">Si les niveaux sont spécifiés dans `Logging.{providername}.LogLevel`, ils remplacent ce qui est défini dans `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="182a5-166">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

`LogLevel` <span data-ttu-id="182a5-167">représentent des noms de journal.</span><span class="sxs-lookup"><span data-stu-id="182a5-167">keys represent log names.</span></span> <span data-ttu-id="182a5-168">La clé `Default` s’applique aux journaux qui ne sont pas explicitement répertoriés.</span><span class="sxs-lookup"><span data-stu-id="182a5-168">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="182a5-169">La valeur représente le [niveau de journal](#log-level) appliqué au journal donné.</span><span class="sxs-lookup"><span data-stu-id="182a5-169">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="182a5-170">Pour plus d’informations sur l’implémentation des fournisseurs de configuration, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="182a5-170">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="182a5-171">Exemple de sortie de la journalisation</span><span class="sxs-lookup"><span data-stu-id="182a5-171">Sample logging output</span></span>

<span data-ttu-id="182a5-172">Avec l’exemple de code présenté dans la section précédente, les journaux s’affichent dans la console lorsque l’application est exécutée en ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="182a5-172">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="182a5-173">Voici un exemple de sortie de la console :</span><span class="sxs-lookup"><span data-stu-id="182a5-173">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

<span data-ttu-id="182a5-174">Les journaux précédents ont été générés par une requête HTTP Get à l’exemple d’application à l’adresse `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="182a5-174">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="182a5-175">Voici un exemple des mêmes journaux tels qu’ils s’affichent dans la fenêtre Débogage quand vous exécutez l’exemple d’application dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="182a5-175">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="182a5-176">Les journaux créés par les appels de `ILogger` illustrés dans la section précédente commencent par « TodoApi.Controllers.TodoController ».</span><span class="sxs-lookup"><span data-stu-id="182a5-176">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="182a5-177">Ceux qui commencent par les catégories « Microsoft » proviennent du code du framework ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="182a5-177">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="182a5-178">ASP.NET Core et le code d’application utilisent la même API de journalisation et les mêmes fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="182a5-178">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="182a5-179">Les autres sections de cet article détaillent certains points et présentent les options de journalisation.</span><span class="sxs-lookup"><span data-stu-id="182a5-179">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="182a5-180">Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="182a5-180">NuGet packages</span></span>

<span data-ttu-id="182a5-181">Les interfaces `ILogger` et `ILoggerFactory` se trouvent dans [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), et leurs implémentations par défaut se trouvent dans [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="182a5-181">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="182a5-182">Catégorie de log</span><span class="sxs-lookup"><span data-stu-id="182a5-182">Log category</span></span>

<span data-ttu-id="182a5-183">Quand un objet `ILogger` est créé, une *catégorie* lui est spécifiée.</span><span class="sxs-lookup"><span data-stu-id="182a5-183">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="182a5-184">Cette catégorie est incluse dans tous les messages de journal créés par cette instance de `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="182a5-184">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="182a5-185">Si la catégorie peut être n’importe quelle chaîne, la convention est d’utiliser le nom de la classe, par exemple « TodoApi.Controllers.TodoController ».</span><span class="sxs-lookup"><span data-stu-id="182a5-185">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="182a5-186">Utilisez `ILogger<T>` pour obtenir une instance de `ILogger` qui utilise le nom de type complet `T` comme catégorie :</span><span class="sxs-lookup"><span data-stu-id="182a5-186">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="182a5-187">Pour spécifier explicitement la catégorie, appelez `ILoggerFactory.CreateLogger` :</span><span class="sxs-lookup"><span data-stu-id="182a5-187">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>` <span data-ttu-id="182a5-188">équivaut à appeler `CreateLogger` avec le nom de type complet `T`.</span><span class="sxs-lookup"><span data-stu-id="182a5-188">is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="182a5-189">Niveau du log</span><span class="sxs-lookup"><span data-stu-id="182a5-189">Log level</span></span>

<span data-ttu-id="182a5-190">Chaque journal spécifie une valeur <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="182a5-190">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="182a5-191">Le niveau de journalisation indique la gravité ou l’importance.</span><span class="sxs-lookup"><span data-stu-id="182a5-191">The log level indicates the severity or importance.</span></span> <span data-ttu-id="182a5-192">Vous pourriez par exemple écrire un journal `Information` lorsqu’une méthode se termine normalement et un journal `Warning` lorsqu’une méthode retourne un code de statut *404 Not Found*.</span><span class="sxs-lookup"><span data-stu-id="182a5-192">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="182a5-193">Le code suivant crée des journaux `Information` et `Warning` :</span><span class="sxs-lookup"><span data-stu-id="182a5-193">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="182a5-194">Dans le code précédent, le premier paramètre est [l’ID de l’événement de journal](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="182a5-194">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="182a5-195">Le deuxième paramètre est un modèle de message contenant des espaces réservés pour les valeurs d’argument fournies par les autres paramètres de méthode.</span><span class="sxs-lookup"><span data-stu-id="182a5-195">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="182a5-196">Les paramètres de méthode sont expliqués dans la section [Modèle de message](#log-message-template) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="182a5-196">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="182a5-197">Les méthodes de journal qui comportent le niveau dans leur nom (par exemple, `LogInformation` et `LogWarning`) sont des [méthodes d’extension pour ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="182a5-197">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="182a5-198">Elles appellent une méthode `Log` qui prend un paramètre `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="182a5-198">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="182a5-199">Vous pouvez appeler la méthode `Log` directement au lieu d’appeler ces méthodes d’extension, mais la syntaxe est relativement complexe.</span><span class="sxs-lookup"><span data-stu-id="182a5-199">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="182a5-200">Pour plus d’informations, voir <xref:Microsoft.Extensions.Logging.ILogger> et le [code source des extensions d’enregistreur d'événements](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="182a5-200">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="182a5-201">ASP.NET Core définit les niveaux de journalisation suivants, classés selon leur degré de gravité (du plus faible au plus élevé).</span><span class="sxs-lookup"><span data-stu-id="182a5-201">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="182a5-202">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="182a5-202">Trace = 0</span></span>

  <span data-ttu-id="182a5-203">Informations en général exclusivement utiles à des fins de débogage.</span><span class="sxs-lookup"><span data-stu-id="182a5-203">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="182a5-204">Ces messages peuvent contenir des données d’application sensibles. Ils ne doivent donc pas être activés dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="182a5-204">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> *<span data-ttu-id="182a5-205">Niveau désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="182a5-205">Disabled by default.</span></span>*

* <span data-ttu-id="182a5-206">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="182a5-206">Debug = 1</span></span>

  <span data-ttu-id="182a5-207">Informations qui peuvent être utiles dans le développement et le débogage.</span><span class="sxs-lookup"><span data-stu-id="182a5-207">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="182a5-208">Exemple : `Entering method Configure with flag set to true.` En raison de leur volume élevé, activez les journaux de niveau `Debug` en production seulement pour résoudre des problèmes.</span><span class="sxs-lookup"><span data-stu-id="182a5-208">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="182a5-209">Information = 2</span><span class="sxs-lookup"><span data-stu-id="182a5-209">Information = 2</span></span>

  <span data-ttu-id="182a5-210">Informations de suivi du flux général de l’application.</span><span class="sxs-lookup"><span data-stu-id="182a5-210">For tracking the general flow of the app.</span></span> <span data-ttu-id="182a5-211">Ces journaux ont généralement une utilité à long terme.</span><span class="sxs-lookup"><span data-stu-id="182a5-211">These logs typically have some long-term value.</span></span> <span data-ttu-id="182a5-212">Exemple :</span><span class="sxs-lookup"><span data-stu-id="182a5-212">Example:</span></span> `Request received for path /api/todo`

* <span data-ttu-id="182a5-213">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="182a5-213">Warning = 3</span></span>

  <span data-ttu-id="182a5-214">Informations sur les événements anormaux ou inattendus dans le flux de l’application.</span><span class="sxs-lookup"><span data-stu-id="182a5-214">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="182a5-215">Il peut s’agir d’erreurs ou d’autres situations qui ne provoquent pas l’arrêt de l’application, mais qu’il peut être intéressant d’examiner.</span><span class="sxs-lookup"><span data-stu-id="182a5-215">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="182a5-216">Le niveau de journalisation `Warning` est généralement utilisé pour les exceptions gérées.</span><span class="sxs-lookup"><span data-stu-id="182a5-216">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="182a5-217">Exemple :</span><span class="sxs-lookup"><span data-stu-id="182a5-217">Example:</span></span> `FileNotFoundException for file quotes.txt.`

* <span data-ttu-id="182a5-218">Error = 4</span><span class="sxs-lookup"><span data-stu-id="182a5-218">Error = 4</span></span>

  <span data-ttu-id="182a5-219">Fournit des informations sur des erreurs et des exceptions qui ne peuvent pas être gérées.</span><span class="sxs-lookup"><span data-stu-id="182a5-219">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="182a5-220">Ces messages indiquent un échec de l’activité ou de l’opération en cours (par exemple, la requête HTTP actuellement exécutée), pas un échec au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="182a5-220">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="182a5-221">Exemple de message de log :</span><span class="sxs-lookup"><span data-stu-id="182a5-221">Example log message:</span></span> `Cannot insert record due to duplicate key violation.`

* <span data-ttu-id="182a5-222">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="182a5-222">Critical = 5</span></span>

  <span data-ttu-id="182a5-223">Fournit des informations sur des échecs qui nécessitent un examen immédiat.</span><span class="sxs-lookup"><span data-stu-id="182a5-223">For failures that require immediate attention.</span></span> <span data-ttu-id="182a5-224">Exemples : perte de données, espace disque insuffisant.</span><span class="sxs-lookup"><span data-stu-id="182a5-224">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="182a5-225">Le niveau de journalisation permet de contrôler le volume de la sortie de journal écrite sur un support de stockage ou dans une fenêtre d’affichage.</span><span class="sxs-lookup"><span data-stu-id="182a5-225">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="182a5-226">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="182a5-226">For example:</span></span>

* <span data-ttu-id="182a5-227">En production, envoyez `Trace` au niveau `Information` dans un magasin de données de volume.</span><span class="sxs-lookup"><span data-stu-id="182a5-227">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="182a5-228">Envoyez `Warning` au niveau `Critical` à un magasin de données de valeurs.</span><span class="sxs-lookup"><span data-stu-id="182a5-228">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="182a5-229">Pendant le développement, envoyez `Warning` au niveau `Critical` à la console et ajoutez `Trace` au niveau `Information` lors de la résolution des problèmes.</span><span class="sxs-lookup"><span data-stu-id="182a5-229">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="182a5-230">La section [Filtrage de log](#log-filtering) plus loin dans cet article explique comment déterminer les niveaux de journalisation gérés par un fournisseur.</span><span class="sxs-lookup"><span data-stu-id="182a5-230">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="182a5-231">ASP.NET Core écrit des journaux pour les événements de framework.</span><span class="sxs-lookup"><span data-stu-id="182a5-231">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="182a5-232">Aucun journal du niveau `Debug` ou `Trace` n’était créé dans les exemples de journaux présentés plus haut dans cet article, puisque les journaux au-dessous du niveau `Information` étaient exclus.</span><span class="sxs-lookup"><span data-stu-id="182a5-232">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="182a5-233">Voici un exemple de journaux Console produits par l’exemple d’application configurée pour afficher les journaux `Debug` :</span><span class="sxs-lookup"><span data-stu-id="182a5-233">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="182a5-234">ID d’événement de log</span><span class="sxs-lookup"><span data-stu-id="182a5-234">Log event ID</span></span>

<span data-ttu-id="182a5-235">Chaque journal peut spécifier un *ID d’événement*.</span><span class="sxs-lookup"><span data-stu-id="182a5-235">Each log can specify an *event ID*.</span></span> <span data-ttu-id="182a5-236">Pour cela, l’exemple d’application utilise une classe `LoggingEvents` définie localement :</span><span class="sxs-lookup"><span data-stu-id="182a5-236">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="182a5-237">Un ID d’événement associe un jeu d’événements.</span><span class="sxs-lookup"><span data-stu-id="182a5-237">An event ID associates a set of events.</span></span> <span data-ttu-id="182a5-238">Par exemple, tous les journaux liés à l’affichage d’une liste d’éléments sur une page peuvent être 1001.</span><span class="sxs-lookup"><span data-stu-id="182a5-238">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="182a5-239">Le fournisseur de journalisation peut stocker l’ID d’événement dans un champ ID, dans le message de journalisation, ou pas du tout.</span><span class="sxs-lookup"><span data-stu-id="182a5-239">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="182a5-240">Le fournisseur Debug n’affiche pas les ID d’événements.</span><span class="sxs-lookup"><span data-stu-id="182a5-240">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="182a5-241">Le fournisseur Console affiche les ID d’événements entre crochets après la catégorie :</span><span class="sxs-lookup"><span data-stu-id="182a5-241">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="182a5-242">Modèle de message de log</span><span class="sxs-lookup"><span data-stu-id="182a5-242">Log message template</span></span>

<span data-ttu-id="182a5-243">Chaque journal spécifie un modèle de message.</span><span class="sxs-lookup"><span data-stu-id="182a5-243">Each log specifies a message template.</span></span> <span data-ttu-id="182a5-244">Ce dernier peut contenir des espaces réservés pour lesquels les arguments sont fournis.</span><span class="sxs-lookup"><span data-stu-id="182a5-244">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="182a5-245">Utilisez des noms et non des nombres pour les espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="182a5-245">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="182a5-246">L’ordre des espaces réservés, pas leurs noms, détermine quels paramètres sont utilisés pour fournir leurs valeurs.</span><span class="sxs-lookup"><span data-stu-id="182a5-246">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="182a5-247">Dans le code suivant, on voit que les noms de paramètres sont hors séquence dans le modèle de message :</span><span class="sxs-lookup"><span data-stu-id="182a5-247">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="182a5-248">Ce code crée un message de journal avec les valeurs des paramètres dans la séquence :</span><span class="sxs-lookup"><span data-stu-id="182a5-248">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="182a5-249">Le framework de journalisation fonctionne ainsi pour permettre aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="182a5-249">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="182a5-250">Les arguments proprement dits, et pas seulement le modèle de message mis en forme, sont transmis au système de journalisation.</span><span class="sxs-lookup"><span data-stu-id="182a5-250">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="182a5-251">C’est grâce à ces informations que les fournisseurs de journalisation peuvent stocker les valeurs des paramètres sous forme de champs.</span><span class="sxs-lookup"><span data-stu-id="182a5-251">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="182a5-252">Supposons par exemple que les appels de méthodes d’enregistreur d’événements se présentent ainsi :</span><span class="sxs-lookup"><span data-stu-id="182a5-252">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="182a5-253">Si vous envoyez les journaux au Stockage Table Azure, chaque entité Table Azure peut avoir les propriétés `ID` et `RequestTime`, ce qui simplifie les requêtes sur les données de journaux.</span><span class="sxs-lookup"><span data-stu-id="182a5-253">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="182a5-254">Une requête peut rechercher tous les journaux compris dans une plage `RequestTime` spécifique sans analyser le délai d’expiration du message texte.</span><span class="sxs-lookup"><span data-stu-id="182a5-254">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="182a5-255">Journalisation des exceptions</span><span class="sxs-lookup"><span data-stu-id="182a5-255">Logging exceptions</span></span>

<span data-ttu-id="182a5-256">Les méthodes logger ont des surcharges qui vous permettent de passer une exception, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="182a5-256">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="182a5-257">Tous les fournisseurs ne gèrent pas les informations sur les exceptions de la même façon.</span><span class="sxs-lookup"><span data-stu-id="182a5-257">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="182a5-258">Voici un exemple de sortie du fournisseur Debug extrait du code montré plus haut.</span><span class="sxs-lookup"><span data-stu-id="182a5-258">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="182a5-259">Filtrage de journal</span><span class="sxs-lookup"><span data-stu-id="182a5-259">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="182a5-260">Vous pouvez spécifier un niveau de journalisation minimum pour une catégorie ou un fournisseur spécifique, ou pour l’ensemble des fournisseurs ou des catégories.</span><span class="sxs-lookup"><span data-stu-id="182a5-260">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="182a5-261">Les enregistrements de log en dessous du niveau minimum ne sont pas passés à ce fournisseur, et ne sont donc pas affichés ou stockés.</span><span class="sxs-lookup"><span data-stu-id="182a5-261">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="182a5-262">Pour supprimer tous les journaux, choisissez `LogLevel.None` comme niveau de journalisation minimal.</span><span class="sxs-lookup"><span data-stu-id="182a5-262">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="182a5-263">La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="182a5-263">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="182a5-264">Créer des règles de filtre dans la configuration</span><span class="sxs-lookup"><span data-stu-id="182a5-264">Create filter rules in configuration</span></span>

<span data-ttu-id="182a5-265">Le code du modèle de projet appelle `CreateDefaultBuilder` afin de configurer la journalisation pour les fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="182a5-265">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="182a5-266">La méthode `CreateDefaultBuilder` définit également la journalisation pour rechercher la configuration dans une section `Logging`, à l’aide de code similaire à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="182a5-266">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="182a5-267">Les données de configuration spécifient des niveaux de journalisation minimum par fournisseur et par catégorie, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="182a5-267">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="182a5-268">Ce code JSON crée six règles de filtre : une pour le fournisseur Debug, quatre pour le fournisseur Console et une pour tous les fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="182a5-268">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="182a5-269">Une seule règle est choisie pour chaque fournisseur à la création d’un objet `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="182a5-269">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="182a5-270">Règles de filtre dans le code</span><span class="sxs-lookup"><span data-stu-id="182a5-270">Filter rules in code</span></span>

<span data-ttu-id="182a5-271">L'exemple suivant montre comment enregistrer des règles de filtre dans le code :</span><span class="sxs-lookup"><span data-stu-id="182a5-271">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="182a5-272">Le second `AddFilter` spécifie le fournisseur Debug par son nom de type.</span><span class="sxs-lookup"><span data-stu-id="182a5-272">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="182a5-273">Le premier `AddFilter` s’applique à tous les fournisseurs, car il ne spécifie aucun type de fournisseur.</span><span class="sxs-lookup"><span data-stu-id="182a5-273">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="182a5-274">Mode d’application des règles de filtre</span><span class="sxs-lookup"><span data-stu-id="182a5-274">How filtering rules are applied</span></span>

<span data-ttu-id="182a5-275">Les données de configuration et le code `AddFilter` contenus dans les exemples précédents créent les règles présentées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="182a5-275">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="182a5-276">Les six premières proviennent de l’exemple de configuration et les deux dernières, de l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="182a5-276">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="182a5-277">nombre</span><span class="sxs-lookup"><span data-stu-id="182a5-277">Number</span></span> | <span data-ttu-id="182a5-278">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="182a5-278">Provider</span></span>      | <span data-ttu-id="182a5-279">Catégories commençant par...</span><span class="sxs-lookup"><span data-stu-id="182a5-279">Categories that begin with ...</span></span>          | <span data-ttu-id="182a5-280">Niveau de journalisation minimum</span><span class="sxs-lookup"><span data-stu-id="182a5-280">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="182a5-281">1</span><span class="sxs-lookup"><span data-stu-id="182a5-281">1</span></span>      | <span data-ttu-id="182a5-282">Débogage</span><span class="sxs-lookup"><span data-stu-id="182a5-282">Debug</span></span>         | <span data-ttu-id="182a5-283">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="182a5-283">All categories</span></span>                          | <span data-ttu-id="182a5-284">Information</span><span class="sxs-lookup"><span data-stu-id="182a5-284">Information</span></span>       |
| <span data-ttu-id="182a5-285">2</span><span class="sxs-lookup"><span data-stu-id="182a5-285">2</span></span>      | <span data-ttu-id="182a5-286">Console</span><span class="sxs-lookup"><span data-stu-id="182a5-286">Console</span></span>       | <span data-ttu-id="182a5-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="182a5-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="182a5-288">Warning</span><span class="sxs-lookup"><span data-stu-id="182a5-288">Warning</span></span>           |
| <span data-ttu-id="182a5-289">3</span><span class="sxs-lookup"><span data-stu-id="182a5-289">3</span></span>      | <span data-ttu-id="182a5-290">Console</span><span class="sxs-lookup"><span data-stu-id="182a5-290">Console</span></span>       | <span data-ttu-id="182a5-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="182a5-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="182a5-292">Débogage</span><span class="sxs-lookup"><span data-stu-id="182a5-292">Debug</span></span>             |
| <span data-ttu-id="182a5-293">4</span><span class="sxs-lookup"><span data-stu-id="182a5-293">4</span></span>      | <span data-ttu-id="182a5-294">Console</span><span class="sxs-lookup"><span data-stu-id="182a5-294">Console</span></span>       | <span data-ttu-id="182a5-295">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="182a5-295">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="182a5-296">Error</span><span class="sxs-lookup"><span data-stu-id="182a5-296">Error</span></span>             |
| <span data-ttu-id="182a5-297">5</span><span class="sxs-lookup"><span data-stu-id="182a5-297">5</span></span>      | <span data-ttu-id="182a5-298">Console</span><span class="sxs-lookup"><span data-stu-id="182a5-298">Console</span></span>       | <span data-ttu-id="182a5-299">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="182a5-299">All categories</span></span>                          | <span data-ttu-id="182a5-300">Information</span><span class="sxs-lookup"><span data-stu-id="182a5-300">Information</span></span>       |
| <span data-ttu-id="182a5-301">6</span><span class="sxs-lookup"><span data-stu-id="182a5-301">6</span></span>      | <span data-ttu-id="182a5-302">Tous les fournisseurs</span><span class="sxs-lookup"><span data-stu-id="182a5-302">All providers</span></span> | <span data-ttu-id="182a5-303">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="182a5-303">All categories</span></span>                          | <span data-ttu-id="182a5-304">Débogage</span><span class="sxs-lookup"><span data-stu-id="182a5-304">Debug</span></span>             |
| <span data-ttu-id="182a5-305">7</span><span class="sxs-lookup"><span data-stu-id="182a5-305">7</span></span>      | <span data-ttu-id="182a5-306">Tous les fournisseurs</span><span class="sxs-lookup"><span data-stu-id="182a5-306">All providers</span></span> | <span data-ttu-id="182a5-307">Système</span><span class="sxs-lookup"><span data-stu-id="182a5-307">System</span></span>                                  | <span data-ttu-id="182a5-308">Débogage</span><span class="sxs-lookup"><span data-stu-id="182a5-308">Debug</span></span>             |
| <span data-ttu-id="182a5-309">8</span><span class="sxs-lookup"><span data-stu-id="182a5-309">8</span></span>      | <span data-ttu-id="182a5-310">Débogage</span><span class="sxs-lookup"><span data-stu-id="182a5-310">Debug</span></span>         | <span data-ttu-id="182a5-311">Microsoft</span><span class="sxs-lookup"><span data-stu-id="182a5-311">Microsoft</span></span>                               | <span data-ttu-id="182a5-312">Suivi</span><span class="sxs-lookup"><span data-stu-id="182a5-312">Trace</span></span>             |

<span data-ttu-id="182a5-313">À la création d’un objet `ILogger`, l’objet `ILoggerFactory` sélectionne une seule règle à appliquer à cet enregistrement d’événements par fournisseur.</span><span class="sxs-lookup"><span data-stu-id="182a5-313">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="182a5-314">Tous les messages écrits par une instance `ILogger` sont filtrés selon les règles sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="182a5-314">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="182a5-315">La règle la plus spécifique pouvant être appliquée à chaque paire catégorie/fournisseur est sélectionnée parmi les règles disponibles.</span><span class="sxs-lookup"><span data-stu-id="182a5-315">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="182a5-316">L’algorithme suivant est utilisé pour chaque fournisseur quand un objet `ILogger` est créé pour une catégorie donnée :</span><span class="sxs-lookup"><span data-stu-id="182a5-316">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="182a5-317">Sélectionnez toutes les règles qui correspondent au fournisseur ou à son alias.</span><span class="sxs-lookup"><span data-stu-id="182a5-317">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="182a5-318">Si aucune correspondance n’est trouvée, sélectionnez toutes les règles avec un fournisseur vide.</span><span class="sxs-lookup"><span data-stu-id="182a5-318">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="182a5-319">À partir du résultat de l’étape précédente, sélectionnez les règles ayant le plus long préfixe de catégorie correspondant.</span><span class="sxs-lookup"><span data-stu-id="182a5-319">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="182a5-320">Si aucune correspondance n’est trouvée, sélectionnez toutes les règles qui ne spécifient pas de catégorie.</span><span class="sxs-lookup"><span data-stu-id="182a5-320">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="182a5-321">Si plusieurs règles sont sélectionnées, prenez la **dernière**.</span><span class="sxs-lookup"><span data-stu-id="182a5-321">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="182a5-322">Si aucune règle n’est sélectionnée, utilisez `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="182a5-322">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="182a5-323">Avec la liste de règles précédente, supposons que vous créez un objet `ILogger` pour la catégorie « Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine » :</span><span class="sxs-lookup"><span data-stu-id="182a5-323">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="182a5-324">Les règles 1, 6 et 8 s’appliquent au fournisseur Debug.</span><span class="sxs-lookup"><span data-stu-id="182a5-324">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="182a5-325">La règle 8 est sélectionnée, car c’est la plus spécifique.</span><span class="sxs-lookup"><span data-stu-id="182a5-325">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="182a5-326">Les règles 3, 4, 5 et 6 s’appliquent au fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="182a5-326">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="182a5-327">La règle 3 est la plus spécifique.</span><span class="sxs-lookup"><span data-stu-id="182a5-327">Rule 3 is most specific.</span></span>

<span data-ttu-id="182a5-328">L’instance `ILogger` ainsi produite envoie des journaux de niveau `Trace` ou supérieur au fournisseur Debug.</span><span class="sxs-lookup"><span data-stu-id="182a5-328">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="182a5-329">Les journaux de niveau `Debug` et supérieurs sont envoyés au fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="182a5-329">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="182a5-330">Alias de fournisseur</span><span class="sxs-lookup"><span data-stu-id="182a5-330">Provider aliases</span></span>

<span data-ttu-id="182a5-331">Chaque fournisseur définit un *alias* qui peut être utilisé dans la configuration à la place du nom de type complet.</span><span class="sxs-lookup"><span data-stu-id="182a5-331">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="182a5-332">Pour les fournisseurs intégrés, utilisez les alias suivants :</span><span class="sxs-lookup"><span data-stu-id="182a5-332">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="182a5-333">Console</span><span class="sxs-lookup"><span data-stu-id="182a5-333">Console</span></span>
* <span data-ttu-id="182a5-334">Débogage</span><span class="sxs-lookup"><span data-stu-id="182a5-334">Debug</span></span>
* <span data-ttu-id="182a5-335">EventLog</span><span class="sxs-lookup"><span data-stu-id="182a5-335">EventLog</span></span>
* <span data-ttu-id="182a5-336">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="182a5-336">AzureAppServices</span></span>
* <span data-ttu-id="182a5-337">TraceSource</span><span class="sxs-lookup"><span data-stu-id="182a5-337">TraceSource</span></span>
* <span data-ttu-id="182a5-338">EventSource</span><span class="sxs-lookup"><span data-stu-id="182a5-338">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="182a5-339">Niveau minimum par défaut</span><span class="sxs-lookup"><span data-stu-id="182a5-339">Default minimum level</span></span>

<span data-ttu-id="182a5-340">Un paramètre de niveau minimum est utilisé uniquement si aucune règle de la configuration ou du code ne s’applique à une catégorie ou un fournisseur spécifique.</span><span class="sxs-lookup"><span data-stu-id="182a5-340">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="182a5-341">L’exemple suivant montre comment définir le niveau minimum :</span><span class="sxs-lookup"><span data-stu-id="182a5-341">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="182a5-342">Si vous ne définissez pas explicitement le niveau minimum, la valeur par défaut est `Information`, ce qui signifie que les niveaux `Trace` et `Debug` sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="182a5-342">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="182a5-343">Fonctions de filtre</span><span class="sxs-lookup"><span data-stu-id="182a5-343">Filter functions</span></span>

<span data-ttu-id="182a5-344">Une fonction de filtre est appelée pour tous les fournisseurs et toutes les catégories pour lesquels la configuration ou le code n’applique aucune règle.</span><span class="sxs-lookup"><span data-stu-id="182a5-344">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="182a5-345">Le code de la fonction a accès au type de fournisseur, à la catégorie et au niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="182a5-345">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="182a5-346">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="182a5-346">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="182a5-347">Certains fournisseurs de journalisation vous permettent de spécifier quand des enregistrements de log doivent être écrits sur un média de stockage, ou au contraire ignorés, en fonction de la catégorie et du niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="182a5-347">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="182a5-348">Les méthodes d’extension `AddConsole` et `AddDebug` offrent des surcharges qui acceptent des critères de filtrage.</span><span class="sxs-lookup"><span data-stu-id="182a5-348">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="182a5-349">Dans l’exemple de code suivant, le fournisseur Console ignore les enregistrements en dessous du niveau `Warning`, et le fournisseur Debug ignore les enregistrements créés par le framework.</span><span class="sxs-lookup"><span data-stu-id="182a5-349">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="182a5-350">La méthode `AddEventLog` a une surcharge qui accepte une instance `EventLogSettings`, dont la propriété `Filter` peut contenir une fonction de filtre.</span><span class="sxs-lookup"><span data-stu-id="182a5-350">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="182a5-351">Le fournisseur TraceSource ne fournit aucune de ces surcharges, étant donné que son niveau de journalisation et d’autres paramètres dépendent des `SourceSwitch` et `TraceListener` qu’il utilise.</span><span class="sxs-lookup"><span data-stu-id="182a5-351">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="182a5-352">Pour définir des règles de filtrage sur tous les fournisseurs inscrits auprès d’une instance `ILoggerFactory`, utilisez la méthode d’extension `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="182a5-352">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="182a5-353">L’exemple suivant limite les journaux du framework (dont la catégorie commence par « Microsoft » ou « System ») aux avertissements, tout en effectuant une journalisation au niveau Debug pour les journaux créés par le code de l’application.</span><span class="sxs-lookup"><span data-stu-id="182a5-353">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="182a5-354">Pour empêcher l’écriture de journaux, choisissez `LogLevel.None` comme niveau de journalisation minimal.</span><span class="sxs-lookup"><span data-stu-id="182a5-354">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="182a5-355">La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="182a5-355">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="182a5-356">La méthode d’extension `WithFilter` est fournie par le package NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="182a5-356">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="182a5-357">La méthode retourne une nouvelle instance `ILoggerFactory` qui filtre les messages de log passés à tous les fournisseurs de journalisation inscrits dans cette méthode.</span><span class="sxs-lookup"><span data-stu-id="182a5-357">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="182a5-358">Cela n’affecte pas les autres instances `ILoggerFactory`, y compris l’instance `ILoggerFactory` initiale.</span><span class="sxs-lookup"><span data-stu-id="182a5-358">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="182a5-359">Niveaux et les catégories de système</span><span class="sxs-lookup"><span data-stu-id="182a5-359">System categories and levels</span></span>

<span data-ttu-id="182a5-360">Voici quelques catégories utilisées par ASP.NET Core et Entity Framework Core, avec des notes sur les journaux associés :</span><span class="sxs-lookup"><span data-stu-id="182a5-360">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="182a5-361">Category</span><span class="sxs-lookup"><span data-stu-id="182a5-361">Category</span></span>                            | <span data-ttu-id="182a5-362">Notes</span><span class="sxs-lookup"><span data-stu-id="182a5-362">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="182a5-363">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="182a5-363">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="182a5-364">Diagnostics ASP.NET Core généraux.</span><span class="sxs-lookup"><span data-stu-id="182a5-364">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="182a5-365">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="182a5-365">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="182a5-366">Liste des clés considérées, trouvées et utilisées.</span><span class="sxs-lookup"><span data-stu-id="182a5-366">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="182a5-367">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="182a5-367">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="182a5-368">Hôtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="182a5-368">Hosts allowed.</span></span> |
| <span data-ttu-id="182a5-369">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="182a5-369">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="182a5-370">Temps de traitement des requêtes HTTP et heure de démarrage.</span><span class="sxs-lookup"><span data-stu-id="182a5-370">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="182a5-371">Liste des assemblys de démarrage d’hébergement chargés.</span><span class="sxs-lookup"><span data-stu-id="182a5-371">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="182a5-372">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="182a5-372">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="182a5-373">Diagnostics MVC et Razor.</span><span class="sxs-lookup"><span data-stu-id="182a5-373">MVC and Razor diagnostics.</span></span> <span data-ttu-id="182a5-374">Liaison de données, exécution de filtres, compilation de vues, sélection d’actions.</span><span class="sxs-lookup"><span data-stu-id="182a5-374">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="182a5-375">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="182a5-375">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="182a5-376">Informations de correspondance des itinéraires.</span><span class="sxs-lookup"><span data-stu-id="182a5-376">Route matching information.</span></span> |
| <span data-ttu-id="182a5-377">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="182a5-377">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="182a5-378">Démarrage et arrêt de la connexion, et réponses persistantes.</span><span class="sxs-lookup"><span data-stu-id="182a5-378">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="182a5-379">Informations du certificat HTTPS.</span><span class="sxs-lookup"><span data-stu-id="182a5-379">HTTPS certificate information.</span></span> |
| <span data-ttu-id="182a5-380">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="182a5-380">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="182a5-381">Fichiers pris en charge.</span><span class="sxs-lookup"><span data-stu-id="182a5-381">Files served.</span></span> |
| <span data-ttu-id="182a5-382">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="182a5-382">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="182a5-383">Diagnostics Entity Framework Core généraux.</span><span class="sxs-lookup"><span data-stu-id="182a5-383">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="182a5-384">Activité et configuration de la base de données, détection des modifications, migrations.</span><span class="sxs-lookup"><span data-stu-id="182a5-384">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="182a5-385">Étendues de log</span><span class="sxs-lookup"><span data-stu-id="182a5-385">Log scopes</span></span>

 <span data-ttu-id="182a5-386">Une *étendue* peut regrouper un ensemble d’opérations logiques.</span><span class="sxs-lookup"><span data-stu-id="182a5-386">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="182a5-387">Ce regroupement permet de joindre les mêmes données à tous les journaux créés au sein d’un ensemble.</span><span class="sxs-lookup"><span data-stu-id="182a5-387">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="182a5-388">Par exemple, chaque journal créé dans le cadre du traitement d’une transaction peut contenir l’ID de la transaction.</span><span class="sxs-lookup"><span data-stu-id="182a5-388">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="182a5-389">Une étendue est un type `IDisposable` qui est retourné par la méthode <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*>. Elle s’applique tant qu’elle n’est pas supprimée.</span><span class="sxs-lookup"><span data-stu-id="182a5-389">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="182a5-390">Utilisez une étendue en incluant les appels de l’enregistrement d’événements dans un bloc `using` :</span><span class="sxs-lookup"><span data-stu-id="182a5-390">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="182a5-391">Le code suivant active les étendues pour le fournisseur Console :</span><span class="sxs-lookup"><span data-stu-id="182a5-391">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="182a5-392">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="182a5-392">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="182a5-393">La configuration de l’option logger `IncludeScopes` pour la console est nécessaire pour activer la journalisation basée sur des étendues.</span><span class="sxs-lookup"><span data-stu-id="182a5-393">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="182a5-394">Pour plus d'informations sur la configuration, reportez-vous à la section [Configuration](#configuration).</span><span class="sxs-lookup"><span data-stu-id="182a5-394">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="182a5-395">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="182a5-395">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="182a5-396">La configuration de l’option logger `IncludeScopes` pour la console est nécessaire pour activer la journalisation basée sur des étendues.</span><span class="sxs-lookup"><span data-stu-id="182a5-396">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="182a5-397">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="182a5-397">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="182a5-398">Chaque message de log fournit les informations incluses dans l’étendue :</span><span class="sxs-lookup"><span data-stu-id="182a5-398">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="182a5-399">Fournisseurs de journalisation intégrés</span><span class="sxs-lookup"><span data-stu-id="182a5-399">Built-in logging providers</span></span>

<span data-ttu-id="182a5-400">ASP.NET Core contient les fournisseurs suivants :</span><span class="sxs-lookup"><span data-stu-id="182a5-400">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="182a5-401">Console</span><span class="sxs-lookup"><span data-stu-id="182a5-401">Console</span></span>](#console-provider)
* [<span data-ttu-id="182a5-402">Débogage</span><span class="sxs-lookup"><span data-stu-id="182a5-402">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="182a5-403">EventSource</span><span class="sxs-lookup"><span data-stu-id="182a5-403">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="182a5-404">EventLog</span><span class="sxs-lookup"><span data-stu-id="182a5-404">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="182a5-405">TraceSource</span><span class="sxs-lookup"><span data-stu-id="182a5-405">TraceSource</span></span>](#tracesource-provider)

<span data-ttu-id="182a5-406">Les options de [Journalisation dans Azure](#logging-in-azure) sont traitées plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="182a5-406">Options for [Logging in Azure](#logging-in-azure) are covered later in this article.</span></span>

<span data-ttu-id="182a5-407">Pour plus d'informations sur la journalisation stdout, voir <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> et <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="182a5-407">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="182a5-408">Fournisseur Console</span><span class="sxs-lookup"><span data-stu-id="182a5-408">Console provider</span></span>

<span data-ttu-id="182a5-409">Le package de fournisseur [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envoie la sortie de log dans la console.</span><span class="sxs-lookup"><span data-stu-id="182a5-409">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="182a5-410">Les [surcharges AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) permettent de transmettre un niveau de journalisation minimal, une fonction de filtre et une valeur booléenne qui indique si les étendues sont prises en charge.</span><span class="sxs-lookup"><span data-stu-id="182a5-410">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="182a5-411">Une autre option consiste à passer un objet `IConfiguration`, qui permet de spécifier la prise en charge des étendues et les niveaux de journalisation.</span><span class="sxs-lookup"><span data-stu-id="182a5-411">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="182a5-412">Le fournisseur Console a un impact significatif sur les performances et n’est généralement pas adapté à une utilisation en production.</span><span class="sxs-lookup"><span data-stu-id="182a5-412">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="182a5-413">Quand vous créez un projet dans Visual Studio, la méthode `AddConsole` ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="182a5-413">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="182a5-414">Ce code fait référence à la section `Logging` du fichier *appSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="182a5-414">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="182a5-415">Les paramètres définis limitent les enregistrements du framework aux avertissements, mais active ceux de l’application au niveau Debug, comme cela est expliqué dans la section [Filtrage de log](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="182a5-415">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="182a5-416">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="182a5-416">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="182a5-417">Pour voir la sortie de la journalisation Console, ouvrez une invite de commandes dans le dossier du projet et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="182a5-417">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="182a5-418">Fournisseur Debug</span><span class="sxs-lookup"><span data-stu-id="182a5-418">Debug provider</span></span>

<span data-ttu-id="182a5-419">Le package de fournisseur [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) écrit la sortie de log à l’aide de la classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (appels de la méthode `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="182a5-419">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="182a5-420">Sur Linux, ce fournisseur écrit la sortie de log dans */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="182a5-420">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="182a5-421">Les [surcharges AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) vous permettent de passer un niveau de log minimum ou une fonction de filtre.</span><span class="sxs-lookup"><span data-stu-id="182a5-421">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="182a5-422">Fournisseur EventSource</span><span class="sxs-lookup"><span data-stu-id="182a5-422">EventSource provider</span></span>

<span data-ttu-id="182a5-423">Pour les applications qui ciblent ASP.NET Core 1.1.0 ou ultérieur, le package de fournisseur [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) peut implémenter le suivi des événements.</span><span class="sxs-lookup"><span data-stu-id="182a5-423">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="182a5-424">Sur Windows, il utilise [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="182a5-424">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="182a5-425">Le fournisseur est multiplateforme, mais il ne prend pas en charge la collection d’événements ni les outils d’affichage pour Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="182a5-425">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

<span data-ttu-id="182a5-426">[L’utilitaire PerfView](https://github.com/Microsoft/perfview) est très utile pour collecter et afficher les journaux.</span><span class="sxs-lookup"><span data-stu-id="182a5-426">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="182a5-427">Il existe d’autres outils d’affichage des journaux ETW, mais PerfView est l’outil recommandé pour gérer les événements ETW générés par ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="182a5-427">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="182a5-428">Pour configurer PerfView afin qu’il collecte les événements enregistrés par ce fournisseur, ajoutez la chaîne `*Microsoft-Extensions-Logging` à la liste des **fournisseurs supplémentaires**.</span><span class="sxs-lookup"><span data-stu-id="182a5-428">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="182a5-429">(N’oubliez pas d’inclure l’astérisque au début de la chaîne.)</span><span class="sxs-lookup"><span data-stu-id="182a5-429">(Don't miss the asterisk at the start of the string.)</span></span>

![Fournisseurs supplémentaires dans PerfView](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="182a5-431">Fournisseur EventLog Windows</span><span class="sxs-lookup"><span data-stu-id="182a5-431">Windows EventLog provider</span></span>

<span data-ttu-id="182a5-432">Le package de fournisseur [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envoie la sortie de log dans le journal des événements Windows.</span><span class="sxs-lookup"><span data-stu-id="182a5-432">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="182a5-433">Les [surcharges AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) vous permettent de passer `EventLogSettings` ou un niveau de journalisation minimum.</span><span class="sxs-lookup"><span data-stu-id="182a5-433">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="182a5-434">Fournisseur TraceSource</span><span class="sxs-lookup"><span data-stu-id="182a5-434">TraceSource provider</span></span>

<span data-ttu-id="182a5-435">Le package de fournisseur [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) utilise les bibliothèques et fournisseurs <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="182a5-435">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="182a5-436">Les [surcharges AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) vous permettent de passer un commutateur de source et un écouteur de suivi.</span><span class="sxs-lookup"><span data-stu-id="182a5-436">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="182a5-437">Pour pouvoir utiliser ce fournisseur, il faut que l’application s’exécute sur .NET Framework (au lieu de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="182a5-437">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="182a5-438">Le fournisseur peut acheminer les messages vers différents [détecteurs d’événements](/dotnet/framework/debug-trace-profile/trace-listeners), comme <xref:System.Diagnostics.TextWriterTraceListener> (utilisé dans l’exemple d’application).</span><span class="sxs-lookup"><span data-stu-id="182a5-438">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="182a5-439">L’exemple suivant configure un fournisseur `TraceSource` qui enregistre les messages `Warning` et de niveau supérieur dans la fenêtre de la console.</span><span class="sxs-lookup"><span data-stu-id="182a5-439">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a><span data-ttu-id="182a5-440">Journalisation dans Azure</span><span class="sxs-lookup"><span data-stu-id="182a5-440">Logging in Azure</span></span>

<span data-ttu-id="182a5-441">Pour plus d'informations sur la journalisation dans Azure, voir les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="182a5-441">For information about logging in Azure, see the following sections:</span></span>

* [<span data-ttu-id="182a5-442">Fournisseur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="182a5-442">Azure App Service provider</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="182a5-443">Streaming des journaux Azure</span><span class="sxs-lookup"><span data-stu-id="182a5-443">Azure log streaming</span></span>](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [<span data-ttu-id="182a5-444">Journalisation des traces Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="182a5-444">Azure Application Insights trace logging</span></span>](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="182a5-445">Fournisseur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="182a5-445">Azure App Service provider</span></span>

<span data-ttu-id="182a5-446">Le package de fournisseur [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) écrit les enregistrements de log dans des fichiers texte dans le système de fichiers d’une application Azure App Service, et dans un [stockage Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) dans un compte de stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="182a5-446">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="182a5-447">Le package de fournisseur est disponible pour les applications ciblant .NET Core 1.1 ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="182a5-447">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="182a5-448">Si vous ciblez .NET Core, notez les points suivants :</span><span class="sxs-lookup"><span data-stu-id="182a5-448">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="182a5-449">Le package du fournisseur est inclus dans le [métapaquet Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="182a5-449">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="182a5-450">Le package du fournisseur n’est pas inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="182a5-450">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="182a5-451">Pour utiliser le fournisseur, installez le package.</span><span class="sxs-lookup"><span data-stu-id="182a5-451">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="182a5-452">N’appelez pas explicitement <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span><span class="sxs-lookup"><span data-stu-id="182a5-452">Don't explicitly call <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span></span> <span data-ttu-id="182a5-453">Le fournisseur est automatiquement mis à la disposition de l’application qui est déployée sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="182a5-453">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="182a5-454">Si vous ciblez le .NET Framework ou référencez le métapackage `Microsoft.AspNetCore.App`, ajoutez le package de fournisseur dans le projet.</span><span class="sxs-lookup"><span data-stu-id="182a5-454">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="182a5-455">Appelez `AddAzureWebAppDiagnostics` :</span><span class="sxs-lookup"><span data-stu-id="182a5-455">Invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="182a5-456">Une surcharge <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> permet de transmettre <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="182a5-456">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="182a5-457">L’objet de paramètres peut remplacer les paramètres par défaut, comme le modèle de sortie de journalisation, le nom d’objet blob et la limite de taille de fichier.</span><span class="sxs-lookup"><span data-stu-id="182a5-457">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="182a5-458">(*Modèle de sortie* est un modèle de message qui s’applique à tous les journaux en plus de ce qui est fourni avec un appel de méthode `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="182a5-458">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="182a5-459">Pour configurer les paramètres du fournisseur, utilisez <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> et <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="182a5-459">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="182a5-460">En cas de déploiement sur une application App Service, celle-ci applique les paramètres définis dans la section [Journaux de diagnostic](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) de la page **App Service** du Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="182a5-460">When you deploy to an App Service app, the application honors the settings in the [Diagnostic Logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="182a5-461">Lorsque ces paramètres sont mis à jour, les changements prennent effet immédiatement sans avoir besoin de redémarrer ou redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="182a5-461">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Paramètres de journalisation Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="182a5-463">L’emplacement par défaut des fichiers journaux est le dossier *D:\\racine\\LogFiles\\Application*, et le nom de fichier par défaut est *diagnostics-aaaammjj.txt*.</span><span class="sxs-lookup"><span data-stu-id="182a5-463">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="182a5-464">La limite de taille de fichier par défaut est de 10 Mo, et le nombre maximal de fichiers conservés par défaut est de 2.</span><span class="sxs-lookup"><span data-stu-id="182a5-464">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="182a5-465">Le nom par défaut du blob est *{app-name}{timestamp}/aaaa/mm/jj/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="182a5-465">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="182a5-466">Le fournisseur fonctionne uniquement quand le projet s’exécute dans l’environnement Azure.</span><span class="sxs-lookup"><span data-stu-id="182a5-466">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="182a5-467">Il n’a aucun effet si le projet s’exécute localement &mdash; il n’écrit pas d’enregistrements dans les fichiers locaux ou dans le stockage de développement local pour les objets blob.</span><span class="sxs-lookup"><span data-stu-id="182a5-467">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

### <a name="azure-log-streaming"></a><span data-ttu-id="182a5-468">Streaming des journaux Azure</span><span class="sxs-lookup"><span data-stu-id="182a5-468">Azure log streaming</span></span>

<span data-ttu-id="182a5-469">La diffusion en continu des journaux Azure permet d’afficher l’activité de journalisation en temps réel aux emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="182a5-469">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="182a5-470">Serveur d'applications</span><span class="sxs-lookup"><span data-stu-id="182a5-470">The app server</span></span>
* <span data-ttu-id="182a5-471">Serveur web</span><span class="sxs-lookup"><span data-stu-id="182a5-471">The web server</span></span>
* <span data-ttu-id="182a5-472">Suivi des demandes ayant échoué</span><span class="sxs-lookup"><span data-stu-id="182a5-472">Failed request tracing</span></span>

<span data-ttu-id="182a5-473">Pour configurer le streaming des journaux Azure :</span><span class="sxs-lookup"><span data-stu-id="182a5-473">To configure Azure log streaming:</span></span>

* <span data-ttu-id="182a5-474">Accédez à la page **Journaux de diagnostic** sur le portail de votre application.</span><span class="sxs-lookup"><span data-stu-id="182a5-474">Navigate to the **Diagnostics Logs** page from your app's portal page.</span></span>
* <span data-ttu-id="182a5-475">Définissez **Journalisation des applications (Système de fichiers)** sur **Activé**.</span><span class="sxs-lookup"><span data-stu-id="182a5-475">Set **Application Logging (Filesystem)** to **On**.</span></span>

![Page Journaux de diagnostic du portail Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="182a5-477">Accédez à la page **Diffusion en continu des journaux** pour voir les messages d’application.</span><span class="sxs-lookup"><span data-stu-id="182a5-477">Navigate to the **Log Streaming** page to view app messages.</span></span> <span data-ttu-id="182a5-478">Ils sont consignés par application par le biais de l’interface `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="182a5-478">They're logged by the app through the `ILogger` interface.</span></span>

![Streaming des journaux d’application dans le portail Azure](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="182a5-480">Journalisation des traces Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="182a5-480">Azure Application Insights trace logging</span></span>

<span data-ttu-id="182a5-481">Le Kit SDK Application Insights peut collecter et présenter des journaux générés par l’infrastructure de journalisation ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="182a5-481">The Application Insights SDK can collect and report logs generated by the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="182a5-482">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="182a5-482">For more information, see the following resources:</span></span>

* [<span data-ttu-id="182a5-483">Vue d'ensemble d'Application Insights</span><span class="sxs-lookup"><span data-stu-id="182a5-483">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* [<span data-ttu-id="182a5-484">Application Insights pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="182a5-484">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* <span data-ttu-id="182a5-485">[Adaptateurs de journalisation Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span><span class="sxs-lookup"><span data-stu-id="182a5-485">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* [<span data-ttu-id="182a5-486">Exemples d’implémentation d’Application Insights ILogger</span><span class="sxs-lookup"><span data-stu-id="182a5-486">Application Insights ILogger implementation samples</span></span>](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="182a5-487">Fournisseurs de journalisation tiers</span><span class="sxs-lookup"><span data-stu-id="182a5-487">Third-party logging providers</span></span>

<span data-ttu-id="182a5-488">Frameworks de journalisation tiers qui sont pris en charge dans ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="182a5-488">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="182a5-489">[elmah.io](https://elmah.io/) ([dépôt GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="182a5-489">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="182a5-490">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([Dépôt GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="182a5-490">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="182a5-491">[JSNLog](http://jsnlog.com/) ([dépôt GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="182a5-491">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="182a5-492">[KissLog.net](https://kisslog.net/) ([référentiel GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="182a5-492">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="182a5-493">[Loggr](http://loggr.net/) ([dépôt GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="182a5-493">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="182a5-494">[NLog](http://nlog-project.org/) ([dépôt GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="182a5-494">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="182a5-495">[Sentry](https://sentry.io/welcome/) ([dépôt GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="182a5-495">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="182a5-496">[Serilog](https://serilog.net/) ([dépôt GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="182a5-496">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>
* <span data-ttu-id="182a5-497">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="182a5-497">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="182a5-498">Certains frameworks tiers prennent en charge la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="182a5-498">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="182a5-499">L’utilisation d’un framework tiers est semblable à l’utilisation des fournisseurs intégrés :</span><span class="sxs-lookup"><span data-stu-id="182a5-499">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="182a5-500">Ajoutez un package NuGet à votre projet.</span><span class="sxs-lookup"><span data-stu-id="182a5-500">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="182a5-501">Appelez `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="182a5-501">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="182a5-502">Pour plus d’informations, consultez la documentation de chaque fournisseur.</span><span class="sxs-lookup"><span data-stu-id="182a5-502">For more information, see each provider's documentation.</span></span> <span data-ttu-id="182a5-503">Les fournisseurs de journalisation tiers ne sont pas pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="182a5-503">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="182a5-504">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="182a5-504">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
