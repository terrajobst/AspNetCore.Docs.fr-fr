---
title: Journalisation dans ASP.NET Core
author: ardalis
description: "Découvrez plus en détail le framework de journalisation dans ASP.NET Core. Apprenez à utiliser les fournisseurs de journalisation intégrés et découvrez-en plus sur les fournisseurs tiers les plus courants."
keywords: "ASP.NET Core,journalisation,fournisseurs de journalisation,Microsoft.Extensions.Logging,ILogger,ILoggerFactory,LogLevel,WithFilter,TraceSource,EventLog,EventSource,étendues"
ms.author: tdykstra
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/index
ms.openlocfilehash: 3eb167c961b8d089d508ef5622db6ae1cdd99088
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="2ac8a-105">Introduction à la journalisation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ac8a-105">Introduction to logging in ASP.NET Core</span></span>

<span data-ttu-id="2ac8a-106">Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2ac8a-106">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2ac8a-107">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec divers fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="2ac8a-108">Les fournisseurs intégrés vous permettent d’envoyer des journaux à une ou plusieurs destinations. Vous pouvez aussi incorporer un framework de journalisation tiers.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="2ac8a-109">Cet article explique comment utiliser les API et fournisseurs de journalisation intégrés dans votre code.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2ac8a-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ac8a-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2ac8a-113">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ac8a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="2ac8a-114">Comment créer des journaux</span><span class="sxs-lookup"><span data-stu-id="2ac8a-114">How to create logs</span></span>

<span data-ttu-id="2ac8a-115">Pour créer des journaux, prenez un objet `ILogger` dans le conteneur [injection de dépendance](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-115">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="2ac8a-116">Appelez ensuite des méthodes de journalisation sur cet objet logger :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="2ac8a-117">Cet exemple crée des journaux en utilisant la classe `TodoController` comme *catégorie*.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-117">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="2ac8a-118">Les catégories sont décrites [plus loin dans cet article](#log-category).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="2ac8a-119">ASP.NET Core ne fournit pas de méthodes logger asynchrones, car la journalisation étant normalement très rapide, l’utilisation de méthodes asynchrones est inutile.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="2ac8a-120">Si ce n’est pas votre cas, envisagez plutôt de changer de processus de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-120">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="2ac8a-121">Si votre magasin de données est lent, écrivez d’abord les messages de journal dans un magasin plus rapide, puis déplacez-les ensuite dans un magasin lent.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="2ac8a-122">Par exemple, enregistrez les messages dans une file d’attente qui est lue et conservée dans le stockage lent par un autre processus.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="2ac8a-123">Comment ajouter des fournisseurs</span><span class="sxs-lookup"><span data-stu-id="2ac8a-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2ac8a-125">Un fournisseur de journalisation prend les messages que vous créez avec un objet `ILogger`, puis les affiche ou les enregistre.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="2ac8a-126">Par exemple, le fournisseur Console affiche les messages dans la console, tandis que le fournisseur Azure App Service les enregistre dans le stockage Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="2ac8a-127">Pour utiliser un fournisseur, appelez la méthode d’extension `Add<ProviderName>` du fournisseur dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="2ac8a-128">Le modèle de projet par défaut active la journalisation avec la méthode [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-128">The default project template enables logging with the [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2ac8a-130">Un fournisseur de journalisation prend les messages que vous créez avec un objet `ILogger`, puis les affiche ou les enregistre.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-130">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="2ac8a-131">Par exemple, le fournisseur Console affiche les messages dans la console, tandis que le fournisseur Azure App Service les enregistre dans le stockage Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-131">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="2ac8a-132">Pour utiliser un fournisseur, installez son package NuGet et appelez la méthode d’extension du fournisseur sur une instance de `ILoggerFactory`, comme dans l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-132">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="2ac8a-133">Le conteneur [injection de dépendance](xref:fundamentals/dependency-injection) dans ASP.NET Core fournit l’instance `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-133">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="2ac8a-134">Les méthodes d’extension `AddConsole` et `AddDebug` sont définies dans les packages [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) et [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-134">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="2ac8a-135">Chaque méthode d’extension appelle la méthode `ILoggerFactory.AddProvider`, en passant une instance du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-135">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="2ac8a-136">L’exemple d’application utilisé dans cet article ajoute des fournisseurs de journalisation dans la méthode `Configure` de la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-136">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="2ac8a-137">Si vous souhaitez obtenir la sortie de journal du code exécuté plus haut, ajoutez les fournisseurs de journalisation dans le constructeur de classe `Startup` à la place.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-137">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="2ac8a-138">Vous trouverez des informations sur chaque [fournisseur de journalisation intégré](#built-in-logging-providers) et des liens vers des [fournisseurs de journalisation tiers](#third-party-logging-providers) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-138">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="2ac8a-139">Exemple de sortie de la journalisation</span><span class="sxs-lookup"><span data-stu-id="2ac8a-139">Sample logging output</span></span>

<span data-ttu-id="2ac8a-140">Avec l’exemple de code présenté dans la section précédente, les journaux s’affichent dans la console quand vous exécutez l’application à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-140">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="2ac8a-141">Voici un exemple de sortie de la console :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-141">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="2ac8a-142">Ces journaux ont été créés en accédant à `http://localhost:5000/api/todo/0`, qui déclenche l’exécution des deux appels `ILogger` montrés dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-142">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="2ac8a-143">Voici un exemple des mêmes journaux affichés dans la fenêtre Débogage quand vous exécutez l’exemple d’application dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-143">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="2ac8a-144">Les enregistrements de journal qui ont été créés par les appels de `ILogger` illustrés dans la section précédente commencent par « TodoApi.Controllers.TodoController ».</span><span class="sxs-lookup"><span data-stu-id="2ac8a-144">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="2ac8a-145">Ceux qui commencent par les catégories « Microsoft » proviennent d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-145">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="2ac8a-146">ASP.NET Core et votre code d’application utilisent la même API de journalisation et les mêmes fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-146">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="2ac8a-147">Les autres sections de cet article détaillent certains points et présentent les options de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-147">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="2ac8a-148">Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="2ac8a-148">NuGet packages</span></span>

<span data-ttu-id="2ac8a-149">Les interfaces `ILogger` et `ILoggerFactory` se trouvent dans [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), et leurs implémentations par défaut se trouvent dans [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-149">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="2ac8a-150">Catégorie de journal</span><span class="sxs-lookup"><span data-stu-id="2ac8a-150">Log category</span></span>

<span data-ttu-id="2ac8a-151">Une *catégorie* est incluse dans chaque journal que vous créez.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-151">A *category* is included with each log that you create.</span></span> <span data-ttu-id="2ac8a-152">Vous spécifiez la catégorie quand vous créez un objet `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-152">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="2ac8a-153">La catégorie peut être n’importe quelle chaîne, mais par convention, utilisez le nom complet de la classe à partir de laquelle les journaux sont écrits.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-153">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="2ac8a-154">Exemple : « TodoApi.Controllers.TodoController ».</span><span class="sxs-lookup"><span data-stu-id="2ac8a-154">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="2ac8a-155">Vous pouvez spécifier la catégorie sous forme de chaîne ou utiliser une méthode d’extension qui dérive la catégorie du type.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-155">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="2ac8a-156">Pour spécifier la catégorie sous forme de chaîne, appelez `CreateLogger` sur une instance `ILoggerFactory`, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-156">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="2ac8a-157">L’utilisation de `ILogger<T>` est généralement plus simple, comme le montre l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-157">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="2ac8a-158">Cela équivaut à appeler `CreateLogger` avec le nom de type complet `T`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-158">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="2ac8a-159">Niveau du journal</span><span class="sxs-lookup"><span data-stu-id="2ac8a-159">Log level</span></span>

<span data-ttu-id="2ac8a-160">Vous spécifiez un niveau [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel) pour chaque enregistrement écrit dans le journal.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-160">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="2ac8a-161">Le niveau de journalisation indique le degré de gravité ou d’importance.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-161">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="2ac8a-162">Par exemple, vous pouvez écrire un enregistrement `Information` quand une méthode se termine normalement, un enregistrement `Warning` quand une méthode retourne un code d’erreur 404 et un enregistrement `Error` quand le code intercepte une exception inattendue.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-162">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="2ac8a-163">Dans l’exemple de code suivant, les noms des méthodes (par exemple, `LogWarning`) spécifient le niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-163">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="2ac8a-164">Le premier paramètre est [l’ID de l’événement de journal](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-164">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="2ac8a-165">Le deuxième paramètre est un [modèle de message](#log-message-template) qui contient des espaces réservés pour les valeurs d’argument fournies par les autres paramètres de méthode.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-165">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="2ac8a-166">Les paramètres de méthode sont expliqués en détail plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-166">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="2ac8a-167">Les méthodes de journal qui incluent le niveau dans leur nom sont des [méthodes d’extension pour ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="2ac8a-168">En arrière-plan, ces méthodes appellent une méthode `Log` qui a un paramètre `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="2ac8a-169">Vous pouvez appeler la méthode `Log` directement au lieu d’appeler ces méthodes d’extension, mais la syntaxe est relativement complexe.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="2ac8a-170">Pour plus d’informations, consultez [l’interface ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) et le [code source des extensions logger](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="2ac8a-171">ASP.NET Core définit les [niveaux de journalisation](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel) suivants, classés selon leur degré de gravité (du plus faible au plus élevé).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="2ac8a-172">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="2ac8a-172">Trace = 0</span></span>

  <span data-ttu-id="2ac8a-173">Fournit des informations qui sont uniquement destinées aux développeurs pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="2ac8a-174">Ces messages peuvent contenir des données d’application sensibles. Ils ne doivent donc pas être activés dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="2ac8a-175">*Niveau désactivé par défaut.*</span><span class="sxs-lookup"><span data-stu-id="2ac8a-175">*Disabled by default.*</span></span> <span data-ttu-id="2ac8a-176">Exemple : `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="2ac8a-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="2ac8a-177">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="2ac8a-177">Debug = 1</span></span>

  <span data-ttu-id="2ac8a-178">Fournit des informations ayant une utilité à court terme durant le développement et le débogage.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="2ac8a-179">Exemple : `Entering method Configure with flag set to true.` En règle générale, vous n’activez pas les journaux de niveau `Debug` dans un environnement de production, sauf si vous en avez besoin pour résoudre des problèmes, en raison du grand nombre d’enregistrements générés.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="2ac8a-180">Information = 2</span><span class="sxs-lookup"><span data-stu-id="2ac8a-180">Information = 2</span></span>

  <span data-ttu-id="2ac8a-181">Fournit des informations permettant d’effectuer le suivi du flux général de l’application.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="2ac8a-182">Ces journaux ont généralement une utilité à long terme.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="2ac8a-183">Exemple : `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="2ac8a-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="2ac8a-184">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="2ac8a-184">Warning = 3</span></span>

  <span data-ttu-id="2ac8a-185">Fournit des messages d’avertissement sur les événements anormaux ou inattendus dans le flux de l’application.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="2ac8a-186">Il peut s’agir d’erreurs ou d’autres conditions qui ne provoquent pas l’arrêt de l’application, mais qui doivent être examinées.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="2ac8a-187">Le niveau de journalisation `Warning` est généralement utilisé pour les exceptions gérées.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="2ac8a-188">Exemple : `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="2ac8a-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="2ac8a-189">Error = 4</span><span class="sxs-lookup"><span data-stu-id="2ac8a-189">Error = 4</span></span>

  <span data-ttu-id="2ac8a-190">Fournit des informations sur des erreurs et des exceptions qui ne peuvent pas être gérées.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="2ac8a-191">Ces messages indiquent un échec de l’activité ou de l’opération en cours (par exemple, la requête HTTP actuellement exécutée), pas un échec au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="2ac8a-192">Exemple de message de journal : `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="2ac8a-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="2ac8a-193">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="2ac8a-193">Critical = 5</span></span>

  <span data-ttu-id="2ac8a-194">Fournit des informations sur des échecs qui nécessitent un examen immédiat.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-194">For failures that require immediate attention.</span></span> <span data-ttu-id="2ac8a-195">Exemples : perte de données, espace disque insuffisant.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="2ac8a-196">Le niveau de journalisation vous permet de contrôler la taille de la sortie de journal qui est écrite sur un média de stockage ou dans une fenêtre d’affichage.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="2ac8a-197">Par exemple, dans un environnement de production, vous allez peut-être choisir d’écrire tous les enregistrements de journal des niveaux `Information` et inférieurs dans un magasin de données de volume, mais d’écrire tous les enregistrements des niveaux `Warning` et supérieurs dans un magasin de données de valeur.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="2ac8a-198">Durant le développement, vous écrivez en principe les enregistrements des niveaux `Warning` ou supérieurs dans la console.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="2ac8a-199">Ensuite, pendant la phase de débogage, vous pouvez ajouter le niveau `Debug`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="2ac8a-200">La section [Filtrage de journal](#log-filtering) plus loin dans cet article explique comment déterminer les niveaux de journalisation gérés par un fournisseur.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="2ac8a-201">Le framework ASP.NET Core écrit des enregistrements de niveau `Debug` pour les événements de framework.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="2ac8a-202">Dans les exemples de journaux présentés plus haut dans cet article, les enregistrements du niveau `Debug` n’y figuraient pas puisque ceux en dessous du niveau `Information` étaient exclus.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="2ac8a-203">Voici un exemple de journal dans la console si vous exécutez l’application configurée pour afficher les enregistrements de niveaux `Debug` et supérieurs pour le fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="2ac8a-204">ID d’événement de journal</span><span class="sxs-lookup"><span data-stu-id="2ac8a-204">Log event ID</span></span>

<span data-ttu-id="2ac8a-205">Vous pouvez spécifier un *ID d’événement* pour chaque enregistrement écrit dans le journal.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="2ac8a-206">L’exemple d’application effectue cette opération à l’aide d’une classe `LoggingEvents` définie localement :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="2ac8a-207">Un ID d’événement est une valeur entière qui vous permet d’associer deux événements dans un ensemble d’événements enregistrés.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="2ac8a-208">Par exemple, un enregistrement correspondant à l’ajout d’un article à un panier d’achat peut avoir l’ID d’événement 1000, et celui pour le paiement d’un achat avoir l’ID d’événement 1001.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="2ac8a-209">Dans la sortie de journal, l’ID d’événement peut être stocké dans un champ ou être inclus dans le message texte, selon le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="2ac8a-210">Le fournisseur Debug n’affiche pas les ID d’événement, à l’inverse du fournisseur Console qui les affiche entre crochets à la suite de la catégorie :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="2ac8a-211">Modèle de message de journal</span><span class="sxs-lookup"><span data-stu-id="2ac8a-211">Log message template</span></span>

<span data-ttu-id="2ac8a-212">Quand vous écrivez un message de journal, vous fournissez un modèle de message.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-212">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="2ac8a-213">Le modèle de message peut être une chaîne, ou contenir des espaces réservés nommés dans lesquels les valeurs d’argument sont placées.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-213">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="2ac8a-214">Le modèle n’est pas une chaîne de format. Les espaces réservés doivent être nommés, pas numérotés.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-214">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="2ac8a-215">L’ordre des espaces réservés, pas leurs noms, détermine quels paramètres sont utilisés pour fournir leurs valeurs.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-215">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="2ac8a-216">Si vous avez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-216">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="2ac8a-217">Le message de journal obtenu ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-217">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="2ac8a-218">Le framework de journalisation met en forme les messages de cette façon pour permettre aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-218">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="2ac8a-219">Étant donné que les arguments eux-mêmes sont passés au système de journalisation, et pas seulement le modèle de message mis en forme, les fournisseurs de journalisation peuvent stocker les valeurs de paramètre sous forme de champs en plus du modèle de message.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-219">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="2ac8a-220">Si vous envoyez votre sortie de journal vers le Stockage Table Azure, votre appel de la méthode logger ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-220">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="2ac8a-221">Chaque entité Table Azure peut avoir les propriétés `ID` et `RequestTime`, ce qui simplifie les requêtes sur les données de journal.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-221">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="2ac8a-222">Vous pouvez rechercher tous les enregistrements de journal compris dans une plage `RequestTime` spécifique sans avoir besoin d’analyser le délai d’expiration du message texte.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-222">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="2ac8a-223">Journalisation des exceptions</span><span class="sxs-lookup"><span data-stu-id="2ac8a-223">Logging exceptions</span></span>

<span data-ttu-id="2ac8a-224">Les méthodes logger ont des surcharges qui vous permettent de passer une exception, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-224">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="2ac8a-225">Tous les fournisseurs ne gèrent pas les informations sur les exceptions de la même façon.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-225">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="2ac8a-226">Voici un exemple de sortie du fournisseur Debug extrait du code montré plus haut.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-226">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="2ac8a-227">Filtrage de journal</span><span class="sxs-lookup"><span data-stu-id="2ac8a-227">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2ac8a-229">Vous pouvez spécifier un niveau de journalisation minimum pour une catégorie ou un fournisseur spécifique, ou pour l’ensemble des fournisseurs ou des catégories.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-229">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="2ac8a-230">Les enregistrements de journal en dessous du niveau minimum ne sont pas passés à ce fournisseur, et ne sont donc pas affichés ou stockés.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-230">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="2ac8a-231">Si vous souhaitez supprimer tous les enregistrements de journal, vous pouvez spécifier `LogLevel.None` comme niveau de journal minimum.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-231">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="2ac8a-232">La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-232">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="2ac8a-233">**Créer des règles de filtre dans la configuration**</span><span class="sxs-lookup"><span data-stu-id="2ac8a-233">**Create filter rules in configuration**</span></span>

<span data-ttu-id="2ac8a-234">Les modèles de projet créent du code qui appelle `CreateDefaultBuilder` afin de configurer la journalisation pour les fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-234">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="2ac8a-235">La méthode `CreateDefaultBuilder` définit également la journalisation pour rechercher la configuration dans une section `Logging`, à l’aide de code similaire à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-235">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="2ac8a-236">Les données de configuration spécifient des niveaux de journalisation minimum par fournisseur et par catégorie, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-236">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="2ac8a-237">Ce code JSON crée six règles de filtre : une pour le fournisseur Debug, quatre pour le fournisseur Console et une commune à tous les fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-237">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="2ac8a-238">Vous verrez plus tard de quelle façon une seule de ces règles est choisie pour chaque fournisseur au moment de la création d’un objet `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-238">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="2ac8a-239">**Règles de filtre dans le code**</span><span class="sxs-lookup"><span data-stu-id="2ac8a-239">**Filter rules in code**</span></span>

<span data-ttu-id="2ac8a-240">Vous pouvez ajouter des règles de filtre dans le code, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-240">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="2ac8a-241">Le second `AddFilter` spécifie le fournisseur Debug par son nom de type.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-241">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="2ac8a-242">Le premier `AddFilter` s’applique à tous les fournisseurs, car il ne spécifie aucun type de fournisseur.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-242">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="2ac8a-243">**Mode d’application des règles de filtre**</span><span class="sxs-lookup"><span data-stu-id="2ac8a-243">**How filtering rules are applied**</span></span>

<span data-ttu-id="2ac8a-244">Les données de configuration et le code `AddFilter` contenus dans les exemples précédents créent les règles présentées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-244">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="2ac8a-245">Les six premières proviennent de l’exemple de configuration et les deux dernières, de l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-245">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="2ac8a-246">nombre</span><span class="sxs-lookup"><span data-stu-id="2ac8a-246">Number</span></span> | <span data-ttu-id="2ac8a-247">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="2ac8a-247">Provider</span></span>      | <span data-ttu-id="2ac8a-248">Catégories commençant par...</span><span class="sxs-lookup"><span data-stu-id="2ac8a-248">Categories that begin with ...</span></span>          | <span data-ttu-id="2ac8a-249">Niveau de journalisation minimum</span><span class="sxs-lookup"><span data-stu-id="2ac8a-249">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="2ac8a-250">1</span><span class="sxs-lookup"><span data-stu-id="2ac8a-250">1</span></span>      | <span data-ttu-id="2ac8a-251">Débogage</span><span class="sxs-lookup"><span data-stu-id="2ac8a-251">Debug</span></span>         | <span data-ttu-id="2ac8a-252">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="2ac8a-252">All categories</span></span>                          | <span data-ttu-id="2ac8a-253">Information</span><span class="sxs-lookup"><span data-stu-id="2ac8a-253">Information</span></span>       |
| <span data-ttu-id="2ac8a-254">2</span><span class="sxs-lookup"><span data-stu-id="2ac8a-254">2</span></span>      | <span data-ttu-id="2ac8a-255">Console</span><span class="sxs-lookup"><span data-stu-id="2ac8a-255">Console</span></span>       | <span data-ttu-id="2ac8a-256">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="2ac8a-256">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="2ac8a-257">Warning</span><span class="sxs-lookup"><span data-stu-id="2ac8a-257">Warning</span></span>           |
| <span data-ttu-id="2ac8a-258">3</span><span class="sxs-lookup"><span data-stu-id="2ac8a-258">3</span></span>      | <span data-ttu-id="2ac8a-259">Console</span><span class="sxs-lookup"><span data-stu-id="2ac8a-259">Console</span></span>       | <span data-ttu-id="2ac8a-260">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="2ac8a-260">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="2ac8a-261">Débogage</span><span class="sxs-lookup"><span data-stu-id="2ac8a-261">Debug</span></span>             |
| <span data-ttu-id="2ac8a-262">4</span><span class="sxs-lookup"><span data-stu-id="2ac8a-262">4</span></span>      | <span data-ttu-id="2ac8a-263">Console</span><span class="sxs-lookup"><span data-stu-id="2ac8a-263">Console</span></span>       | <span data-ttu-id="2ac8a-264">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="2ac8a-264">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="2ac8a-265">Error</span><span class="sxs-lookup"><span data-stu-id="2ac8a-265">Error</span></span>             |
| <span data-ttu-id="2ac8a-266">5</span><span class="sxs-lookup"><span data-stu-id="2ac8a-266">5</span></span>      | <span data-ttu-id="2ac8a-267">Console</span><span class="sxs-lookup"><span data-stu-id="2ac8a-267">Console</span></span>       | <span data-ttu-id="2ac8a-268">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="2ac8a-268">All categories</span></span>                          | <span data-ttu-id="2ac8a-269">Information</span><span class="sxs-lookup"><span data-stu-id="2ac8a-269">Information</span></span>       |
| <span data-ttu-id="2ac8a-270">6</span><span class="sxs-lookup"><span data-stu-id="2ac8a-270">6</span></span>      | <span data-ttu-id="2ac8a-271">Tous les fournisseurs</span><span class="sxs-lookup"><span data-stu-id="2ac8a-271">All providers</span></span> | <span data-ttu-id="2ac8a-272">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="2ac8a-272">All categories</span></span>                          | <span data-ttu-id="2ac8a-273">Débogage</span><span class="sxs-lookup"><span data-stu-id="2ac8a-273">Debug</span></span>             |
| <span data-ttu-id="2ac8a-274">7</span><span class="sxs-lookup"><span data-stu-id="2ac8a-274">7</span></span>      | <span data-ttu-id="2ac8a-275">Tous les fournisseurs</span><span class="sxs-lookup"><span data-stu-id="2ac8a-275">All providers</span></span> | <span data-ttu-id="2ac8a-276">Système</span><span class="sxs-lookup"><span data-stu-id="2ac8a-276">System</span></span>                                  | <span data-ttu-id="2ac8a-277">Débogage</span><span class="sxs-lookup"><span data-stu-id="2ac8a-277">Debug</span></span>             |
| <span data-ttu-id="2ac8a-278">8</span><span class="sxs-lookup"><span data-stu-id="2ac8a-278">8</span></span>      | <span data-ttu-id="2ac8a-279">Débogage</span><span class="sxs-lookup"><span data-stu-id="2ac8a-279">Debug</span></span>         | <span data-ttu-id="2ac8a-280">Microsoft</span><span class="sxs-lookup"><span data-stu-id="2ac8a-280">Microsoft</span></span>                               | <span data-ttu-id="2ac8a-281">Suivi</span><span class="sxs-lookup"><span data-stu-id="2ac8a-281">Trace</span></span>             |

<span data-ttu-id="2ac8a-282">Quand vous créez un objet `ILogger` d’écriture d’enregistrements de journal, l’objet `ILoggerFactory` sélectionne une seule règle par fournisseur à appliquer à cet objet.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-282">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="2ac8a-283">Tous les messages écrits par cet objet `ILogger` sont filtrés selon les règles sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-283">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="2ac8a-284">La règle la plus spécifique pouvant être appliquée à chaque paire catégorie/fournisseur est sélectionnée parmi les règles disponibles.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-284">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="2ac8a-285">L’algorithme suivant est utilisé pour chaque fournisseur quand un objet `ILogger` est créé pour une catégorie donnée :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-285">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="2ac8a-286">Sélectionnez toutes les règles qui correspondent au fournisseur ou à son alias.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-286">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="2ac8a-287">Si aucune règle n’est trouvée, sélectionnez toutes les règles avec un fournisseur vide.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-287">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="2ac8a-288">À partir du résultat de l’étape précédente, sélectionnez les règles ayant le plus long préfixe de catégorie correspondant.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-288">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="2ac8a-289">Si aucune règle n’est trouvée, sélectionnez toutes les règles qui ne spécifient aucune catégorie.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-289">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="2ac8a-290">Si plusieurs règles sont sélectionnées, utilisez la **dernière**.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-290">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="2ac8a-291">Si aucune règle n’est sélectionnée, utilisez `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-291">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="2ac8a-292">Par exemple, supposons que vous utilisez la liste de règles précédente et que vous créez un objet `ILogger` pour la catégorie « Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine » :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-292">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="2ac8a-293">Les règles 1, 6 et 8 s’appliquent au fournisseur Debug.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-293">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="2ac8a-294">La règle 8 est sélectionnée, car c’est la plus spécifique.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-294">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="2ac8a-295">Les règles 3, 4, 5 et 6 s’appliquent au fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-295">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="2ac8a-296">La règle 3 est la plus spécifique.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-296">Rule 3 is most specific.</span></span>

<span data-ttu-id="2ac8a-297">Quand vous créez des enregistrements de journal avec un objet `ILogger` pour la catégorie « Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine », les enregistrements des niveaux `Trace` et supérieurs sont envoyés au fournisseur Debug, et ceux des niveaux `Debug` et supérieurs sont envoyés au fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-297">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="2ac8a-298">**Alias de fournisseur**</span><span class="sxs-lookup"><span data-stu-id="2ac8a-298">**Provider aliases**</span></span>

<span data-ttu-id="2ac8a-299">Vous pouvez utiliser le nom de type pour spécifier un fournisseur dans une configuration, mais chaque fournisseur définit un *alias* plus court et plus simple d’emploi.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-299">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="2ac8a-300">Pour les fournisseurs intégrés, utilisez les alias suivants :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-300">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="2ac8a-301">Console</span><span class="sxs-lookup"><span data-stu-id="2ac8a-301">Console</span></span>
- <span data-ttu-id="2ac8a-302">Débogage</span><span class="sxs-lookup"><span data-stu-id="2ac8a-302">Debug</span></span>
- <span data-ttu-id="2ac8a-303">EventLog</span><span class="sxs-lookup"><span data-stu-id="2ac8a-303">EventLog</span></span>
- <span data-ttu-id="2ac8a-304">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="2ac8a-304">AzureAppServices</span></span>
- <span data-ttu-id="2ac8a-305">TraceSource</span><span class="sxs-lookup"><span data-stu-id="2ac8a-305">TraceSource</span></span>
- <span data-ttu-id="2ac8a-306">EventSource</span><span class="sxs-lookup"><span data-stu-id="2ac8a-306">EventSource</span></span>

<span data-ttu-id="2ac8a-307">**Niveau minimum par défaut**</span><span class="sxs-lookup"><span data-stu-id="2ac8a-307">**Default minimum level**</span></span>

<span data-ttu-id="2ac8a-308">Un paramètre de niveau minimum est utilisé uniquement si aucune règle de la configuration ou du code ne s’applique à une catégorie ou fournisseur spécifique.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-308">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="2ac8a-309">L’exemple suivant montre comment définir le niveau minimum :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-309">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="2ac8a-310">Si vous ne définissez pas explicitement le niveau minimum, la valeur par défaut est `Information`, ce qui signifie que les niveaux `Trace` et `Debug` sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-310">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="2ac8a-311">**Fonctions de filtre**</span><span class="sxs-lookup"><span data-stu-id="2ac8a-311">**Filter functions**</span></span>

<span data-ttu-id="2ac8a-312">Vous pouvez écrire du code dans une fonction de filtre pour appliquer des règles de filtrage.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-312">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="2ac8a-313">Une fonction de filtre est appelée pour tous les fournisseurs et toutes les catégories pour lesquels la configuration ou le code n’applique aucune règle.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-313">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="2ac8a-314">Le code dans la fonction a accès au type de fournisseur, à la catégorie et au niveau de journalisation pour déterminer si un message doit ou non être enregistré.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-314">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="2ac8a-315">Exemple :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-315">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2ac8a-317">Certains fournisseurs de journalisation vous permettent de spécifier quand des enregistrements de journal doivent être écrits sur un média de stockage, ou au contraire ignorés, en fonction de la catégorie et du niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-317">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="2ac8a-318">Les méthodes d’extension `AddConsole` et `AddDebug` fournissent des surcharges qui vous permettent de passer des critères de filtrage.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-318">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="2ac8a-319">Dans l’exemple de code suivant, le fournisseur Console ignore les enregistrements en dessous du niveau `Warning`, et le fournisseur Debug ignore les enregistrements créés par le framework.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-319">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="2ac8a-320">La méthode `AddEventLog` a une surcharge qui accepte une instance `EventLogSettings`, dont la propriété `Filter` peut contenir une fonction de filtre.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-320">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="2ac8a-321">Le fournisseur TraceSource ne fournit aucune de ces surcharges, étant donné que son niveau de journalisation et d’autres paramètres dépendent des `SourceSwitch` et `TraceListener` qu’il utilise.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-321">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="2ac8a-322">Vous pouvez définir des règles de filtrage pour tous les fournisseurs qui sont inscrits auprès d’une instance `ILoggerFactory` à l’aide de la méthode d’extension `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-322">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="2ac8a-323">L’exemple suivant limite les enregistrements du framework (ceux dont la catégorie commence par « Microsoft » ou « System ») aux avertissements, mais active ceux de l’application au niveau Debug.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-323">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="2ac8a-324">Si vous souhaitez utiliser le filtrage pour empêcher l’écriture de tous les enregistrements correspondant à une catégorie particulière, spécifiez `LogLevel.None` comme niveau de journalisation minimum pour cette catégorie.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-324">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="2ac8a-325">La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-325">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="2ac8a-326">La méthode d’extension `WithFilter` est fournie par le package NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-326">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="2ac8a-327">La méthode retourne une nouvelle instance `ILoggerFactory` qui filtre les messages de journal passés à tous les fournisseurs de journalisation inscrits dans cette méthode.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-327">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="2ac8a-328">Cela n’affecte pas les autres instances `ILoggerFactory`, y compris l’instance `ILoggerFactory` initiale.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-328">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="2ac8a-329">Étendues de journal</span><span class="sxs-lookup"><span data-stu-id="2ac8a-329">Log scopes</span></span>

<span data-ttu-id="2ac8a-330">Vous pouvez regrouper un ensemble d’opérations logiques dans une *étendue* pour joindre les mêmes données à chaque journal qui est créé dans cet ensemble.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-330">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span> <span data-ttu-id="2ac8a-331">Par exemple, vous pouvez spécifier que chaque enregistrement créé pendant le traitement d’une transaction contienne l’ID de la transaction.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-331">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="2ac8a-332">Une étendue est un type `IDisposable` qui est retourné par la méthode `ILogger.BeginScope<TState>`. Elle s’applique tant qu’elle n’est pas supprimée.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-332">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="2ac8a-333">Pour utiliser une étendue, incluez les appels de l’objet logger dans un bloc `using`, comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-333">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="2ac8a-334">Le code suivant active les étendues pour le fournisseur Console :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-334">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-335">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2ac8a-336">Dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-336">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="2ac8a-337">La configuration de l’option logger `IncludeScopes` pour la console est nécessaire pour activer la journalisation basée sur des étendues.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-337">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="2ac8a-338">La configuration de `IncludeScopes` à l’aide des fichiers de configuration *appsettings* sera possible dans la version ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-338">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-339">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2ac8a-340">Dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-340">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="2ac8a-341">Chaque message de journal fournit les informations incluses dans l’étendue :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-341">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="2ac8a-342">Fournisseurs de journalisation intégrés</span><span class="sxs-lookup"><span data-stu-id="2ac8a-342">Built-in logging providers</span></span>

<span data-ttu-id="2ac8a-343">ASP.NET Core contient les fournisseurs suivants :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-343">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="2ac8a-344">Console</span><span class="sxs-lookup"><span data-stu-id="2ac8a-344">Console</span></span>](#console)
* [<span data-ttu-id="2ac8a-345">Déboguer</span><span class="sxs-lookup"><span data-stu-id="2ac8a-345">Debug</span></span>](#debug)
* [<span data-ttu-id="2ac8a-346">EventSource</span><span class="sxs-lookup"><span data-stu-id="2ac8a-346">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="2ac8a-347">EventLog</span><span class="sxs-lookup"><span data-stu-id="2ac8a-347">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="2ac8a-348">TraceSource</span><span class="sxs-lookup"><span data-stu-id="2ac8a-348">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="2ac8a-349">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2ac8a-349">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="2ac8a-350">Fournisseur Console</span><span class="sxs-lookup"><span data-stu-id="2ac8a-350">The console provider</span></span>

<span data-ttu-id="2ac8a-351">Le package de fournisseur [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envoie la sortie de journal dans la console.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-351">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-352">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-352">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-353">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="2ac8a-354">Les [surcharges AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) vous permettent de passer un niveau de journalisation minimum, une fonction de filtre et une valeur booléenne qui indique si les étendues sont prises en charge.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-354">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="2ac8a-355">Une autre option consiste à passer un objet `IConfiguration`, qui permet de spécifier la prise en charge des étendues et les niveaux de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-355">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="2ac8a-356">Si vous envisagez d’utiliser le fournisseur Console dans un environnement de production, sachez qu’il impacte fortement les performances.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-356">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="2ac8a-357">Quand vous créez un projet dans Visual Studio, la méthode `AddConsole` ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-357">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="2ac8a-358">Ce code fait référence à la section `Logging` du fichier *appSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-358">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="2ac8a-359">Les paramètres définis limitent les enregistrements du framework aux avertissements, mais active ceux de l’application au niveau Debug, comme cela est expliqué dans la section [Filtrage de journal](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-359">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="2ac8a-360">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-360">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="2ac8a-361">Fournisseur Debug</span><span class="sxs-lookup"><span data-stu-id="2ac8a-361">The Debug provider</span></span>

<span data-ttu-id="2ac8a-362">Le package de fournisseur [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) écrit la sortie de journal à l’aide de la classe [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) (appels de la méthode `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-362">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="2ac8a-363">Sur Linux, ce fournisseur écrit la sortie de journal dans */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-363">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-364">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-365">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="2ac8a-366">Les [surcharges AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) vous permettent de passer un niveau de journal minimum ou une fonction de filtre.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-366">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="2ac8a-367">Fournisseur EventSource</span><span class="sxs-lookup"><span data-stu-id="2ac8a-367">The EventSource provider</span></span>

<span data-ttu-id="2ac8a-368">Pour les applications qui ciblent ASP.NET Core 1.1.0 ou une version ultérieure, le package de fournisseur [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) peut implémenter le suivi des événements.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-368">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="2ac8a-369">Sur Windows, il utilise [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-369">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="2ac8a-370">Le fournisseur est multiplateforme, mais il ne prend pas en charge la collection d’événements ni les outils d’affichage pour Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-370">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="2ac8a-373">[L’utilitaire PerfView](https://www.microsoft.com/download/details.aspx?id=28567) est très utile pour collecter et afficher les journaux.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-373">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="2ac8a-374">Il existe d’autres outils d’affichage des journaux ETW, mais PerfView est l’outil recommandé pour gérer les événements ETW générés par ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-374">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="2ac8a-375">Pour configurer PerfView afin qu’il collecte les événements enregistrés par ce fournisseur, ajoutez la chaîne `*Microsoft-Extensions-Logging` à la liste des **fournisseurs supplémentaires**.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-375">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="2ac8a-376">(N’oubliez pas d’inclure l’astérisque au début de la chaîne.)</span><span class="sxs-lookup"><span data-stu-id="2ac8a-376">(Don't miss the asterisk at the start of the string.)</span></span>

![Fournisseurs supplémentaires dans PerfView](index/_static/perfview-additional-providers.png)

<span data-ttu-id="2ac8a-378">La collecte d’événements sur Nano Server nécessite une configuration supplémentaire :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-378">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="2ac8a-379">Établissez une communication à distance PowerShell sur Nano Server :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-379">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="2ac8a-380">Créez une session ETW :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-380">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="2ac8a-381">Ajoutez des fournisseurs ETW pour [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core et autres types en fonction de vos besoins.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-381">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="2ac8a-382">Le GUID du fournisseur ASP.NET Core est `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-382">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="2ac8a-383">Exécutez le site et effectuez toutes les actions pour lesquelles vous voulez collecter des informations de suivi.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-383">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="2ac8a-384">Arrêtez la session de suivi quand vous avez terminé :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-384">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="2ac8a-385">Vous pouvez analyser le fichier *C:\trace.etl* obtenu avec PerfView de la même manière que sur les autres éditions de Windows.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-385">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="2ac8a-386">Fournisseur EventLog Windows</span><span class="sxs-lookup"><span data-stu-id="2ac8a-386">The Windows EventLog provider</span></span>

<span data-ttu-id="2ac8a-387">Le package de fournisseur [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envoie la sortie de journal dans le journal des événements Windows.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-387">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-388">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-388">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="2ac8a-390">Les [surcharges AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) vous permettent de passer `EventLogSettings` ou un niveau de journalisation minimum.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-390">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="2ac8a-391">Fournisseur TraceSource</span><span class="sxs-lookup"><span data-stu-id="2ac8a-391">The TraceSource provider</span></span>

<span data-ttu-id="2ac8a-392">Le package de fournisseur [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) utilise les bibliothèques et fournisseurs [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-392">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-393">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-394">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-394">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="2ac8a-395">Les [surcharges AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) vous permettent de passer un commutateur de source et un écouteur de suivi.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-395">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="2ac8a-396">Pour utiliser ce fournisseur, l’application doit s’exécuter sur .NET Framework (au lieu de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-396">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="2ac8a-397">Le fournisseur vous permet d’envoyer les messages vers différents [écouteurs](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), tels que le [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) utilisé dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-397">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="2ac8a-398">L’exemple suivant configure un fournisseur `TraceSource` qui enregistre les messages `Warning` et de niveau supérieur dans la fenêtre de la console.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-398">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="2ac8a-399">Fournisseur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2ac8a-399">The Azure App Service provider</span></span>

<span data-ttu-id="2ac8a-400">Le package de fournisseur [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) écrit les enregistrements de journal dans des fichiers texte dans le système de fichiers d’une application Azure App Service, et dans un [stockage Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) dans un compte de stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-400">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="2ac8a-401">Le fournisseur est disponible uniquement pour les applications qui ciblent ASP.NET Core 1.1.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-401">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ac8a-402">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-402">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2ac8a-403">Si vous ciblez .NET Core, vous n’êtes pas obligé d’installer le package du fournisseur ou d’appeler explicitement `AddAzureWebAppDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-403">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="2ac8a-404">Le fournisseur est automatiquement disponible pour une application qui est déployée sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="2ac8a-405">Si vous ciblez le .NET Framework, ajoutez le package du fournisseur dans votre projet et appelez `AddAzureWebAppDiagnostics` :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-405">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ac8a-406">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ac8a-406">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="2ac8a-407">Une surcharge `AddAzureWebAppDiagnostics` vous permet de passer [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) pour substituer des paramètres par défaut, tels que le modèle de sortie de journalisation, le nom du blob et la limite de taille de fichier.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-407">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="2ac8a-408">(Le *modèle de sortie* est un modèle de message qui est appliqué à tous les enregistrements de journal au-dessus de celui fourni quand vous appelez une méthode `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="2ac8a-408">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="2ac8a-409">Quand vous effectuez un déploiement sur une application App Service, votre application applique les paramètres définis dans la section [Journaux de diagnostic](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) de la page **App Service** du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-409">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="2ac8a-410">Quand vous modifiez ces paramètres, les modifications prennent effet immédiatement. Vous n’avez pas à redémarrer l’application ni à redéployer le code sur cette dernière.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-410">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Paramètres de journalisation Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="2ac8a-412">L’emplacement par défaut des fichiers journaux est le dossier *D:\\racine\\LogFiles\\Application*, et le nom de fichier par défaut est *diagnostics-aaaammjj.txt*.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-412">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="2ac8a-413">La limite de taille de fichier par défaut est de 10 Mo, et le nombre maximal de fichiers conservés par défaut est de 2.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-413">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="2ac8a-414">Le nom par défaut du blob est *{app-name}{timestamp}/aaaa/mm/jj/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-414">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="2ac8a-415">Pour plus d’informations sur le comportement par défaut, consultez [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-415">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="2ac8a-416">Le fournisseur est activé uniquement si votre projet est exécuté dans l’environnement Azure.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-416">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="2ac8a-417">Il n’a aucun effet si vous l’exécutez localement &mdash; il n’écrit pas d’enregistrements dans les fichiers locaux ou dans le stockage de développement local pour les objets blob.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-417">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="2ac8a-418">Fournisseurs de journalisation tiers</span><span class="sxs-lookup"><span data-stu-id="2ac8a-418">Third-party logging providers</span></span>

<span data-ttu-id="2ac8a-419">Voici des frameworks de journalisation tiers qui sont pris en charge dans ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-419">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="2ac8a-420">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) : fournisseur pour le service Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="2ac8a-420">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="2ac8a-421">[JSNLog](http://jsnlog.com) : enregistre les exceptions JavaScript et d’autres événements côté client dans votre journal côté serveur.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-421">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="2ac8a-422">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) : fournisseur pour le service Loggr</span><span class="sxs-lookup"><span data-stu-id="2ac8a-422">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="2ac8a-423">[NLog](https://github.com/NLog/NLog.Extensions.Logging) : fournisseur pour la bibliothèque NLog</span><span class="sxs-lookup"><span data-stu-id="2ac8a-423">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="2ac8a-424">[Serilog](https://github.com/serilog/serilog-extensions-logging) : fournisseur pour la bibliothèque Serilog</span><span class="sxs-lookup"><span data-stu-id="2ac8a-424">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="2ac8a-425">Certains frameworks tiers prennent en charge la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="2ac8a-425">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="2ac8a-426">L’utilisation d’un framework tiers est semblable à celle des fournisseurs intégrés : vous ajoutez un package NuGet à votre projet, puis vous appelez une méthode d’extension sur `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-426">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="2ac8a-427">Pour plus d’informations, consultez la documentation du framework.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-427">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="2ac8a-428">Vous pouvez également créer vos propres fournisseurs personnalisés pour prendre en charge d’autres frameworks de journalisation ou répondre à des besoins de journalisation particuliers.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-428">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="2ac8a-429">Streaming des journaux Azure</span><span class="sxs-lookup"><span data-stu-id="2ac8a-429">Azure log streaming</span></span>

<span data-ttu-id="2ac8a-430">Le streaming des journaux Azure vous permet d’afficher l’activité de journalisation en temps réel à partir des emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-430">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="2ac8a-431">Serveur d’applications</span><span class="sxs-lookup"><span data-stu-id="2ac8a-431">The application server</span></span> 
* <span data-ttu-id="2ac8a-432">Serveur web</span><span class="sxs-lookup"><span data-stu-id="2ac8a-432">The web server</span></span>
* <span data-ttu-id="2ac8a-433">Suivi des demandes ayant échoué</span><span class="sxs-lookup"><span data-stu-id="2ac8a-433">Failed request tracing</span></span> 

<span data-ttu-id="2ac8a-434">Pour configurer le streaming des journaux Azure :</span><span class="sxs-lookup"><span data-stu-id="2ac8a-434">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="2ac8a-435">Accédez à la page **Journaux de diagnostic** à partir du portail de votre application.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-435">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="2ac8a-436">Définissez l’option **Journal des applications (Système de fichiers)** sur Activé.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-436">Set **Application Logging (Filesystem)** to on.</span></span> 

![Page Journaux de diagnostic du portail Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="2ac8a-438">Accédez à la page **Streaming des journaux** pour afficher les messages d’application.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-438">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="2ac8a-439">Les messages sont enregistrés par application dans l’interface `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="2ac8a-439">They are logged by application through the `ILogger` interface.</span></span> 

![Streaming des journaux d’application dans le portail Azure](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="2ac8a-441">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="2ac8a-441">See also</span></span>

[<span data-ttu-id="2ac8a-442">Journalisation avancée avec LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="2ac8a-442">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
