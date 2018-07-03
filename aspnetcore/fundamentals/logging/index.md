---
title: Journalisation dans ASP.NET Core
author: ardalis
description: Découvrez plus en détail le framework de journalisation dans ASP.NET Core. Apprenez à utiliser les fournisseurs de journalisation intégrés et découvrez-en plus sur les fournisseurs tiers les plus courants.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: 969ad303c3fee06aa40d43140153ffbf58b735db
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126285"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="0156d-104">Journalisation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0156d-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="0156d-105">Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0156d-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0156d-106">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec divers fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="0156d-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="0156d-107">Les fournisseurs intégrés vous permettent d’envoyer des journaux à une ou plusieurs destinations. Vous pouvez aussi incorporer un framework de journalisation tiers.</span><span class="sxs-lookup"><span data-stu-id="0156d-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="0156d-108">Cet article explique comment utiliser les API et fournisseurs de journalisation intégrés dans votre code.</span><span class="sxs-lookup"><span data-stu-id="0156d-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0156d-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0156d-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0156d-110">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0156d-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

## <a name="how-to-create-logs"></a><span data-ttu-id="0156d-111">Comment créer des journaux</span><span class="sxs-lookup"><span data-stu-id="0156d-111">How to create logs</span></span>

<span data-ttu-id="0156d-112">Pour créer des journaux, implémentez un objet [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) dans le conteneur d’[injection de dépendance](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="0156d-112">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="0156d-113">Appelez ensuite des méthodes de journalisation sur cet objet logger :</span><span class="sxs-lookup"><span data-stu-id="0156d-113">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="0156d-114">Cet exemple crée des journaux en utilisant la classe `TodoController` comme *catégorie*.</span><span class="sxs-lookup"><span data-stu-id="0156d-114">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="0156d-115">Les catégories sont décrites [plus loin dans cet article](#log-category).</span><span class="sxs-lookup"><span data-stu-id="0156d-115">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="0156d-116">ASP.NET Core ne fournit pas de méthodes logger asynchrones, car la journalisation étant normalement très rapide, l’utilisation de méthodes asynchrones est inutile.</span><span class="sxs-lookup"><span data-stu-id="0156d-116">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="0156d-117">Si ce n’est pas votre cas, envisagez plutôt de changer de processus de journalisation.</span><span class="sxs-lookup"><span data-stu-id="0156d-117">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="0156d-118">Si votre magasin de données est lent, écrivez d’abord les messages de log dans un magasin plus rapide, puis déplacez-les ensuite dans un magasin lent.</span><span class="sxs-lookup"><span data-stu-id="0156d-118">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="0156d-119">Par exemple, enregistrez les messages dans une file d’attente qui est lue et conservée dans le stockage lent par un autre processus.</span><span class="sxs-lookup"><span data-stu-id="0156d-119">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="0156d-120">Comment ajouter des fournisseurs</span><span class="sxs-lookup"><span data-stu-id="0156d-120">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0156d-121">Un fournisseur de journalisation prend les messages que vous créez avec un objet `ILogger`, puis les affiche ou les enregistre.</span><span class="sxs-lookup"><span data-stu-id="0156d-121">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="0156d-122">Par exemple, le fournisseur Console affiche les messages dans la console, tandis que le fournisseur Azure App Service les enregistre dans le stockage Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="0156d-122">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="0156d-123">Pour utiliser un fournisseur, appelez la méthode d’extension `Add<ProviderName>` du fournisseur dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="0156d-123">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="0156d-124">Le modèle de projet par défaut active la journalisation avec la méthode [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) :</span><span class="sxs-lookup"><span data-stu-id="0156d-124">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0156d-125">Un fournisseur de journalisation prend les messages que vous créez avec un objet `ILogger`, puis les affiche ou les enregistre.</span><span class="sxs-lookup"><span data-stu-id="0156d-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="0156d-126">Par exemple, le fournisseur Console affiche les messages dans la console, tandis que le fournisseur Azure App Service les enregistre dans le stockage Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="0156d-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="0156d-127">Pour utiliser un fournisseur, installez son package NuGet et appelez la méthode d’extension du fournisseur sur une instance de [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="0156d-127">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="0156d-128">Le conteneur [injection de dépendance](xref:fundamentals/dependency-injection) dans ASP.NET Core fournit l’instance `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="0156d-128">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="0156d-129">Les méthodes d’extension `AddConsole` et `AddDebug` sont définies dans les packages [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) et [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="0156d-129">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="0156d-130">Chaque méthode d’extension appelle la méthode `ILoggerFactory.AddProvider`, en passant une instance du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="0156d-130">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="0156d-131">L’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ajoute des fournisseurs de journalisation dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0156d-131">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="0156d-132">Si vous souhaitez obtenir la sortie de journal du code exécuté plus haut, ajoutez les fournisseurs de journalisation dans le constructeur de classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0156d-132">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="0156d-133">Vous trouverez des informations sur chaque [fournisseur de journalisation intégré](#built-in-logging-providers) et des liens vers des [fournisseurs de journalisation tiers](#third-party-logging-providers) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="0156d-133">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="settings-file-configuration"></a><span data-ttu-id="0156d-134">Configuration du fichier de paramètres</span><span class="sxs-lookup"><span data-stu-id="0156d-134">Settings file configuration</span></span>

<span data-ttu-id="0156d-135">Chacun des exemples précédents de la section [Comment ajouter des fournisseurs](#how-to-add-providers) charge une configuration de fournisseur de journalisation de la section `Logging` des fichiers de paramètres d’application.</span><span class="sxs-lookup"><span data-stu-id="0156d-135">Each of the preceding examples in the [How to add providers](#how-to-add-providers) section loads logging provider configuration from the `Logging` section of app settings files.</span></span> <span data-ttu-id="0156d-136">L’exemple suivant montre le contenu d’un fichier *appsettings.Development.json* standard :</span><span class="sxs-lookup"><span data-stu-id="0156d-136">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="0156d-137">Les clés `LogLevel` représentent des noms de journal.</span><span class="sxs-lookup"><span data-stu-id="0156d-137">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="0156d-138">La clé `Default` s’applique aux journaux qui ne sont pas explicitement répertoriés.</span><span class="sxs-lookup"><span data-stu-id="0156d-138">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="0156d-139">La valeur représente le [niveau de journal](#log-level) appliqué au journal donné.</span><span class="sxs-lookup"><span data-stu-id="0156d-139">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="0156d-140">Les clés de journal qui définissent `IncludeScopes` (`Console` dans l’exemple), spécifient si les [étendues de journal](#log-scopes) sont activées pour le journal indiqué.</span><span class="sxs-lookup"><span data-stu-id="0156d-140">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="0156d-141">Les clés `LogLevel` représentent des noms de journal.</span><span class="sxs-lookup"><span data-stu-id="0156d-141">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="0156d-142">La clé `Default` s’applique aux journaux qui ne sont pas explicitement répertoriés.</span><span class="sxs-lookup"><span data-stu-id="0156d-142">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="0156d-143">La valeur représente le [niveau de journal](#log-level) appliqué au journal donné.</span><span class="sxs-lookup"><span data-stu-id="0156d-143">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

## <a name="sample-logging-output"></a><span data-ttu-id="0156d-144">Exemple de sortie de la journalisation</span><span class="sxs-lookup"><span data-stu-id="0156d-144">Sample logging output</span></span>

<span data-ttu-id="0156d-145">Avec l’exemple de code présenté dans la section précédente, les journaux s’affichent dans la console quand vous exécutez l’application à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0156d-145">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="0156d-146">Voici un exemple de sortie de la console :</span><span class="sxs-lookup"><span data-stu-id="0156d-146">Here's an example of console output:</span></span>

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

<span data-ttu-id="0156d-147">Ces journaux ont été créés en accédant à `http://localhost:5000/api/todo/0`, qui déclenche l’exécution des deux appels `ILogger` montrés dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="0156d-147">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="0156d-148">Voici un exemple des mêmes journaux affichés dans la fenêtre Débogage quand vous exécutez l’exemple d’application dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="0156d-148">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="0156d-149">Les enregistrements de journal qui ont été créés par les appels de `ILogger` illustrés dans la section précédente commencent par « TodoApi.Controllers.TodoController ».</span><span class="sxs-lookup"><span data-stu-id="0156d-149">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="0156d-150">Ceux qui commencent par les catégories « Microsoft » proviennent d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0156d-150">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="0156d-151">ASP.NET Core et votre code d’application utilisent la même API de journalisation et les mêmes fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="0156d-151">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="0156d-152">Les autres sections de cet article détaillent certains points et présentent les options de journalisation.</span><span class="sxs-lookup"><span data-stu-id="0156d-152">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="0156d-153">Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="0156d-153">NuGet packages</span></span>

<span data-ttu-id="0156d-154">Les interfaces `ILogger` et `ILoggerFactory` se trouvent dans [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), et leurs implémentations par défaut se trouvent dans [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="0156d-154">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="0156d-155">Catégorie de journal</span><span class="sxs-lookup"><span data-stu-id="0156d-155">Log category</span></span>

<span data-ttu-id="0156d-156">Une *catégorie* est incluse dans chaque journal que vous créez.</span><span class="sxs-lookup"><span data-stu-id="0156d-156">A *category* is included with each log that you create.</span></span> <span data-ttu-id="0156d-157">Vous spécifiez la catégorie quand vous créez un objet `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="0156d-157">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="0156d-158">La catégorie peut être n’importe quelle chaîne, mais par convention, utilisez le nom complet de la classe à partir de laquelle les journaux sont écrits.</span><span class="sxs-lookup"><span data-stu-id="0156d-158">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="0156d-159">Exemple : « TodoApi.Controllers.TodoController ».</span><span class="sxs-lookup"><span data-stu-id="0156d-159">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="0156d-160">Vous pouvez spécifier la catégorie sous forme de chaîne ou utiliser une méthode d’extension qui dérive la catégorie du type.</span><span class="sxs-lookup"><span data-stu-id="0156d-160">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="0156d-161">Pour spécifier la catégorie sous forme de chaîne, appelez `CreateLogger` sur une instance `ILoggerFactory`, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0156d-161">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="0156d-162">L’utilisation de `ILogger<T>` est généralement plus simple, comme le montre l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="0156d-162">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="0156d-163">Cela équivaut à appeler `CreateLogger` avec le nom de type complet `T`.</span><span class="sxs-lookup"><span data-stu-id="0156d-163">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="0156d-164">Niveau du log</span><span class="sxs-lookup"><span data-stu-id="0156d-164">Log level</span></span>

<span data-ttu-id="0156d-165">Vous spécifiez un niveau [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel) pour chaque enregistrement écrit dans le log.</span><span class="sxs-lookup"><span data-stu-id="0156d-165">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="0156d-166">Le niveau de journalisation indique le degré de gravité ou d’importance.</span><span class="sxs-lookup"><span data-stu-id="0156d-166">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="0156d-167">Par exemple, vous pouvez écrire un enregistrement `Information` quand une méthode se termine normalement, un enregistrement `Warning` quand une méthode retourne un code d’erreur 404 et un enregistrement `Error` quand le code intercepte une exception inattendue.</span><span class="sxs-lookup"><span data-stu-id="0156d-167">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="0156d-168">Dans l’exemple de code suivant, les noms des méthodes (par exemple, `LogWarning`) spécifient le niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="0156d-168">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="0156d-169">Le premier paramètre est [l’ID de l’événement de journal](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="0156d-169">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="0156d-170">Le deuxième paramètre est un [modèle de message](#log-message-template) qui contient des espaces réservés pour les valeurs d’argument fournies par les autres paramètres de méthode.</span><span class="sxs-lookup"><span data-stu-id="0156d-170">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="0156d-171">Les paramètres de méthode sont expliqués en détail plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="0156d-171">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="0156d-172">Les méthodes de journal qui incluent le niveau dans leur nom sont des [méthodes d’extension pour ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="0156d-172">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="0156d-173">En arrière-plan, ces méthodes appellent une méthode `Log` qui a un paramètre `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="0156d-173">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="0156d-174">Vous pouvez appeler la méthode `Log` directement au lieu d’appeler ces méthodes d’extension, mais la syntaxe est relativement complexe.</span><span class="sxs-lookup"><span data-stu-id="0156d-174">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="0156d-175">Pour plus d’informations, consultez [l’interface ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) et le [code source des extensions logger](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="0156d-175">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="0156d-176">ASP.NET Core définit les [niveaux de journalisation](/dotnet/api/microsoft.extensions.logging.loglevel) suivants, classés selon leur degré de gravité (du plus faible au plus élevé).</span><span class="sxs-lookup"><span data-stu-id="0156d-176">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="0156d-177">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="0156d-177">Trace = 0</span></span>

  <span data-ttu-id="0156d-178">Fournit des informations qui sont uniquement destinées aux développeurs pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="0156d-178">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="0156d-179">Ces messages peuvent contenir des données d’application sensibles. Ils ne doivent donc pas être activés dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="0156d-179">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="0156d-180">*Niveau désactivé par défaut.*</span><span class="sxs-lookup"><span data-stu-id="0156d-180">*Disabled by default.*</span></span> <span data-ttu-id="0156d-181">Exemple : `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="0156d-181">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="0156d-182">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="0156d-182">Debug = 1</span></span>

  <span data-ttu-id="0156d-183">Fournit des informations ayant une utilité à court terme durant le développement et le débogage.</span><span class="sxs-lookup"><span data-stu-id="0156d-183">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="0156d-184">Exemple : `Entering method Configure with flag set to true.` En règle générale, vous n’activez pas les journaux de niveau `Debug` dans un environnement de production, sauf si vous en avez besoin pour résoudre des problèmes, en raison du grand nombre d’enregistrements générés.</span><span class="sxs-lookup"><span data-stu-id="0156d-184">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="0156d-185">Information = 2</span><span class="sxs-lookup"><span data-stu-id="0156d-185">Information = 2</span></span>

  <span data-ttu-id="0156d-186">Fournit des informations permettant d’effectuer le suivi du flux général de l’application.</span><span class="sxs-lookup"><span data-stu-id="0156d-186">For tracking the general flow of the application.</span></span> <span data-ttu-id="0156d-187">Ces journaux ont généralement une utilité à long terme.</span><span class="sxs-lookup"><span data-stu-id="0156d-187">These logs typically have some long-term value.</span></span> <span data-ttu-id="0156d-188">Exemple : `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="0156d-188">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="0156d-189">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="0156d-189">Warning = 3</span></span>

  <span data-ttu-id="0156d-190">Fournit des messages d’avertissement sur les événements anormaux ou inattendus dans le flux de l’application.</span><span class="sxs-lookup"><span data-stu-id="0156d-190">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="0156d-191">Il peut s’agir d’erreurs ou d’autres conditions qui ne provoquent pas l’arrêt de l’application, mais qui doivent être examinées.</span><span class="sxs-lookup"><span data-stu-id="0156d-191">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="0156d-192">Le niveau de journalisation `Warning` est généralement utilisé pour les exceptions gérées.</span><span class="sxs-lookup"><span data-stu-id="0156d-192">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="0156d-193">Exemple : `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="0156d-193">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="0156d-194">Error = 4</span><span class="sxs-lookup"><span data-stu-id="0156d-194">Error = 4</span></span>

  <span data-ttu-id="0156d-195">Fournit des informations sur des erreurs et des exceptions qui ne peuvent pas être gérées.</span><span class="sxs-lookup"><span data-stu-id="0156d-195">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="0156d-196">Ces messages indiquent un échec de l’activité ou de l’opération en cours (par exemple, la requête HTTP actuellement exécutée), pas un échec au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="0156d-196">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="0156d-197">Exemple de message de journal : `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="0156d-197">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="0156d-198">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="0156d-198">Critical = 5</span></span>

  <span data-ttu-id="0156d-199">Fournit des informations sur des échecs qui nécessitent un examen immédiat.</span><span class="sxs-lookup"><span data-stu-id="0156d-199">For failures that require immediate attention.</span></span> <span data-ttu-id="0156d-200">Exemples : perte de données, espace disque insuffisant.</span><span class="sxs-lookup"><span data-stu-id="0156d-200">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="0156d-201">Le niveau de journalisation vous permet de contrôler la taille de la sortie de journal qui est écrite sur un média de stockage ou dans une fenêtre d’affichage.</span><span class="sxs-lookup"><span data-stu-id="0156d-201">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="0156d-202">Par exemple, dans un environnement de production, vous allez peut-être choisir d’écrire tous les enregistrements de journal des niveaux `Information` et inférieurs dans un magasin de données de volume, mais d’écrire tous les enregistrements des niveaux `Warning` et supérieurs dans un magasin de données de valeur.</span><span class="sxs-lookup"><span data-stu-id="0156d-202">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="0156d-203">Durant le développement, vous écrivez en principe les enregistrements des niveaux `Warning` ou supérieurs dans la console.</span><span class="sxs-lookup"><span data-stu-id="0156d-203">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="0156d-204">Ensuite, pendant la phase de débogage, vous pouvez ajouter le niveau `Debug`.</span><span class="sxs-lookup"><span data-stu-id="0156d-204">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="0156d-205">La section [Filtrage de journal](#log-filtering) plus loin dans cet article explique comment déterminer les niveaux de journalisation gérés par un fournisseur.</span><span class="sxs-lookup"><span data-stu-id="0156d-205">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="0156d-206">Le framework ASP.NET Core écrit des enregistrements de niveau `Debug` pour les événements de framework.</span><span class="sxs-lookup"><span data-stu-id="0156d-206">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="0156d-207">Dans les exemples de journaux présentés plus haut dans cet article, les enregistrements du niveau `Debug` n’y figuraient pas puisque ceux en dessous du niveau `Information` étaient exclus.</span><span class="sxs-lookup"><span data-stu-id="0156d-207">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="0156d-208">Voici un exemple de journal dans la console si vous exécutez l’application configurée pour afficher les enregistrements de niveaux `Debug` et supérieurs pour le fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="0156d-208">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="0156d-209">ID d’événement de log</span><span class="sxs-lookup"><span data-stu-id="0156d-209">Log event ID</span></span>

<span data-ttu-id="0156d-210">Vous pouvez spécifier un *ID d’événement* pour chaque enregistrement écrit dans le journal.</span><span class="sxs-lookup"><span data-stu-id="0156d-210">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="0156d-211">L’exemple d’application effectue cette opération à l’aide d’une classe `LoggingEvents` définie localement :</span><span class="sxs-lookup"><span data-stu-id="0156d-211">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="0156d-212">Un ID d’événement est une valeur entière qui vous permet d’associer deux événements dans un ensemble d’événements enregistrés.</span><span class="sxs-lookup"><span data-stu-id="0156d-212">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="0156d-213">Par exemple, un enregistrement correspondant à l’ajout d’un article à un panier d’achat peut avoir l’ID d’événement 1000, et celui pour le paiement d’un achat avoir l’ID d’événement 1001.</span><span class="sxs-lookup"><span data-stu-id="0156d-213">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="0156d-214">Dans la sortie de journal, l’ID d’événement peut être stocké dans un champ ou être inclus dans le message texte, selon le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="0156d-214">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="0156d-215">Le fournisseur Debug n’affiche pas les ID d’événement, à l’inverse du fournisseur Console qui les affiche entre crochets à la suite de la catégorie :</span><span class="sxs-lookup"><span data-stu-id="0156d-215">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="0156d-216">Modèle de message de journal</span><span class="sxs-lookup"><span data-stu-id="0156d-216">Log message template</span></span>

<span data-ttu-id="0156d-217">Quand vous écrivez un message de journal, vous fournissez un modèle de message.</span><span class="sxs-lookup"><span data-stu-id="0156d-217">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="0156d-218">Le modèle de message peut être une chaîne, ou contenir des espaces réservés nommés dans lesquels les valeurs d’argument sont placées.</span><span class="sxs-lookup"><span data-stu-id="0156d-218">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="0156d-219">Le modèle n’est pas une chaîne de format. Les espaces réservés doivent être nommés, pas numérotés.</span><span class="sxs-lookup"><span data-stu-id="0156d-219">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="0156d-220">L’ordre des espaces réservés, pas leurs noms, détermine quels paramètres sont utilisés pour fournir leurs valeurs.</span><span class="sxs-lookup"><span data-stu-id="0156d-220">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="0156d-221">Si vous avez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0156d-221">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="0156d-222">Le message de log obtenu ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="0156d-222">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="0156d-223">Le framework de journalisation met en forme les messages de cette façon pour permettre aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="0156d-223">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="0156d-224">Étant donné que les arguments eux-mêmes sont passés au système de journalisation, et pas seulement le modèle de message mis en forme, les fournisseurs de journalisation peuvent stocker les valeurs de paramètre sous forme de champs en plus du modèle de message.</span><span class="sxs-lookup"><span data-stu-id="0156d-224">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="0156d-225">Si vous envoyez votre sortie de journal vers le Stockage Table Azure, votre appel de la méthode logger ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="0156d-225">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="0156d-226">Chaque entité Table Azure peut avoir les propriétés `ID` et `RequestTime`, ce qui simplifie les requêtes sur les données de log.</span><span class="sxs-lookup"><span data-stu-id="0156d-226">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="0156d-227">Vous pouvez rechercher tous les enregistrements de journal compris dans une plage `RequestTime` spécifique sans avoir besoin d’analyser le délai d’expiration du message texte.</span><span class="sxs-lookup"><span data-stu-id="0156d-227">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="0156d-228">Journalisation des exceptions</span><span class="sxs-lookup"><span data-stu-id="0156d-228">Logging exceptions</span></span>

<span data-ttu-id="0156d-229">Les méthodes logger ont des surcharges qui vous permettent de passer une exception, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="0156d-229">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="0156d-230">Tous les fournisseurs ne gèrent pas les informations sur les exceptions de la même façon.</span><span class="sxs-lookup"><span data-stu-id="0156d-230">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="0156d-231">Voici un exemple de sortie du fournisseur Debug extrait du code montré plus haut.</span><span class="sxs-lookup"><span data-stu-id="0156d-231">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="0156d-232">Filtrage de journal</span><span class="sxs-lookup"><span data-stu-id="0156d-232">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0156d-233">Vous pouvez spécifier un niveau de journalisation minimum pour une catégorie ou un fournisseur spécifique, ou pour l’ensemble des fournisseurs ou des catégories.</span><span class="sxs-lookup"><span data-stu-id="0156d-233">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="0156d-234">Les enregistrements de journal en dessous du niveau minimum ne sont pas passés à ce fournisseur, et ne sont donc pas affichés ou stockés.</span><span class="sxs-lookup"><span data-stu-id="0156d-234">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="0156d-235">Si vous souhaitez supprimer tous les enregistrements de journal, vous pouvez spécifier `LogLevel.None` comme niveau de journal minimum.</span><span class="sxs-lookup"><span data-stu-id="0156d-235">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="0156d-236">La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="0156d-236">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="0156d-237">**Créer des règles de filtre dans la configuration**</span><span class="sxs-lookup"><span data-stu-id="0156d-237">**Create filter rules in configuration**</span></span>

<span data-ttu-id="0156d-238">Les modèles de projet créent du code qui appelle `CreateDefaultBuilder` afin de configurer la journalisation pour les fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="0156d-238">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="0156d-239">La méthode `CreateDefaultBuilder` définit également la journalisation pour rechercher la configuration dans une section `Logging`, à l’aide de code similaire à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="0156d-239">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="0156d-240">Les données de configuration spécifient des niveaux de journalisation minimum par fournisseur et par catégorie, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="0156d-240">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="0156d-241">Ce code JSON crée six règles de filtre : une pour le fournisseur Debug, quatre pour le fournisseur Console et une commune à tous les fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="0156d-241">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="0156d-242">Vous verrez plus tard de quelle façon une seule de ces règles est choisie pour chaque fournisseur au moment de la création d’un objet `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="0156d-242">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="0156d-243">**Règles de filtre dans le code**</span><span class="sxs-lookup"><span data-stu-id="0156d-243">**Filter rules in code**</span></span>

<span data-ttu-id="0156d-244">Vous pouvez ajouter des règles de filtre dans le code, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="0156d-244">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="0156d-245">Le second `AddFilter` spécifie le fournisseur Debug par son nom de type.</span><span class="sxs-lookup"><span data-stu-id="0156d-245">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="0156d-246">Le premier `AddFilter` s’applique à tous les fournisseurs, car il ne spécifie aucun type de fournisseur.</span><span class="sxs-lookup"><span data-stu-id="0156d-246">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="0156d-247">**Mode d’application des règles de filtre**</span><span class="sxs-lookup"><span data-stu-id="0156d-247">**How filtering rules are applied**</span></span>

<span data-ttu-id="0156d-248">Les données de configuration et le code `AddFilter` contenus dans les exemples précédents créent les règles présentées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0156d-248">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="0156d-249">Les six premières proviennent de l’exemple de configuration et les deux dernières, de l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="0156d-249">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="0156d-250">nombre</span><span class="sxs-lookup"><span data-stu-id="0156d-250">Number</span></span> | <span data-ttu-id="0156d-251">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="0156d-251">Provider</span></span>      | <span data-ttu-id="0156d-252">Catégories commençant par...</span><span class="sxs-lookup"><span data-stu-id="0156d-252">Categories that begin with ...</span></span>          | <span data-ttu-id="0156d-253">Niveau de journalisation minimum</span><span class="sxs-lookup"><span data-stu-id="0156d-253">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="0156d-254">1</span><span class="sxs-lookup"><span data-stu-id="0156d-254">1</span></span>      | <span data-ttu-id="0156d-255">Débogage</span><span class="sxs-lookup"><span data-stu-id="0156d-255">Debug</span></span>         | <span data-ttu-id="0156d-256">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="0156d-256">All categories</span></span>                          | <span data-ttu-id="0156d-257">Information</span><span class="sxs-lookup"><span data-stu-id="0156d-257">Information</span></span>       |
| <span data-ttu-id="0156d-258">2</span><span class="sxs-lookup"><span data-stu-id="0156d-258">2</span></span>      | <span data-ttu-id="0156d-259">Console</span><span class="sxs-lookup"><span data-stu-id="0156d-259">Console</span></span>       | <span data-ttu-id="0156d-260">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="0156d-260">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="0156d-261">Warning</span><span class="sxs-lookup"><span data-stu-id="0156d-261">Warning</span></span>           |
| <span data-ttu-id="0156d-262">3</span><span class="sxs-lookup"><span data-stu-id="0156d-262">3</span></span>      | <span data-ttu-id="0156d-263">Console</span><span class="sxs-lookup"><span data-stu-id="0156d-263">Console</span></span>       | <span data-ttu-id="0156d-264">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="0156d-264">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="0156d-265">Débogage</span><span class="sxs-lookup"><span data-stu-id="0156d-265">Debug</span></span>             |
| <span data-ttu-id="0156d-266">4</span><span class="sxs-lookup"><span data-stu-id="0156d-266">4</span></span>      | <span data-ttu-id="0156d-267">Console</span><span class="sxs-lookup"><span data-stu-id="0156d-267">Console</span></span>       | <span data-ttu-id="0156d-268">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="0156d-268">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="0156d-269">Error</span><span class="sxs-lookup"><span data-stu-id="0156d-269">Error</span></span>             |
| <span data-ttu-id="0156d-270">5</span><span class="sxs-lookup"><span data-stu-id="0156d-270">5</span></span>      | <span data-ttu-id="0156d-271">Console</span><span class="sxs-lookup"><span data-stu-id="0156d-271">Console</span></span>       | <span data-ttu-id="0156d-272">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="0156d-272">All categories</span></span>                          | <span data-ttu-id="0156d-273">Information</span><span class="sxs-lookup"><span data-stu-id="0156d-273">Information</span></span>       |
| <span data-ttu-id="0156d-274">6</span><span class="sxs-lookup"><span data-stu-id="0156d-274">6</span></span>      | <span data-ttu-id="0156d-275">Tous les fournisseurs</span><span class="sxs-lookup"><span data-stu-id="0156d-275">All providers</span></span> | <span data-ttu-id="0156d-276">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="0156d-276">All categories</span></span>                          | <span data-ttu-id="0156d-277">Débogage</span><span class="sxs-lookup"><span data-stu-id="0156d-277">Debug</span></span>             |
| <span data-ttu-id="0156d-278">7</span><span class="sxs-lookup"><span data-stu-id="0156d-278">7</span></span>      | <span data-ttu-id="0156d-279">Tous les fournisseurs</span><span class="sxs-lookup"><span data-stu-id="0156d-279">All providers</span></span> | <span data-ttu-id="0156d-280">Système</span><span class="sxs-lookup"><span data-stu-id="0156d-280">System</span></span>                                  | <span data-ttu-id="0156d-281">Débogage</span><span class="sxs-lookup"><span data-stu-id="0156d-281">Debug</span></span>             |
| <span data-ttu-id="0156d-282">8</span><span class="sxs-lookup"><span data-stu-id="0156d-282">8</span></span>      | <span data-ttu-id="0156d-283">Débogage</span><span class="sxs-lookup"><span data-stu-id="0156d-283">Debug</span></span>         | <span data-ttu-id="0156d-284">Microsoft</span><span class="sxs-lookup"><span data-stu-id="0156d-284">Microsoft</span></span>                               | <span data-ttu-id="0156d-285">Suivi</span><span class="sxs-lookup"><span data-stu-id="0156d-285">Trace</span></span>             |

<span data-ttu-id="0156d-286">Quand vous créez un objet `ILogger` d’écriture d’enregistrements de log, l’objet `ILoggerFactory` sélectionne une seule règle par fournisseur à appliquer à cet objet.</span><span class="sxs-lookup"><span data-stu-id="0156d-286">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="0156d-287">Tous les messages écrits par cet objet `ILogger` sont filtrés selon les règles sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="0156d-287">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="0156d-288">La règle la plus spécifique pouvant être appliquée à chaque paire catégorie/fournisseur est sélectionnée parmi les règles disponibles.</span><span class="sxs-lookup"><span data-stu-id="0156d-288">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="0156d-289">L’algorithme suivant est utilisé pour chaque fournisseur quand un objet `ILogger` est créé pour une catégorie donnée :</span><span class="sxs-lookup"><span data-stu-id="0156d-289">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="0156d-290">Sélectionnez toutes les règles qui correspondent au fournisseur ou à son alias.</span><span class="sxs-lookup"><span data-stu-id="0156d-290">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="0156d-291">Si aucune règle n’est trouvée, sélectionnez toutes les règles avec un fournisseur vide.</span><span class="sxs-lookup"><span data-stu-id="0156d-291">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="0156d-292">À partir du résultat de l’étape précédente, sélectionnez les règles ayant le plus long préfixe de catégorie correspondant.</span><span class="sxs-lookup"><span data-stu-id="0156d-292">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="0156d-293">Si aucune règle n’est trouvée, sélectionnez toutes les règles qui ne spécifient aucune catégorie.</span><span class="sxs-lookup"><span data-stu-id="0156d-293">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="0156d-294">Si plusieurs règles sont sélectionnées, utilisez la **dernière**.</span><span class="sxs-lookup"><span data-stu-id="0156d-294">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="0156d-295">Si aucune règle n’est sélectionnée, utilisez `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="0156d-295">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="0156d-296">Par exemple, supposons que vous utilisez la liste de règles précédente et que vous créez un objet `ILogger` pour la catégorie « Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine » :</span><span class="sxs-lookup"><span data-stu-id="0156d-296">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="0156d-297">Les règles 1, 6 et 8 s’appliquent au fournisseur Debug.</span><span class="sxs-lookup"><span data-stu-id="0156d-297">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="0156d-298">La règle 8 est sélectionnée, car c’est la plus spécifique.</span><span class="sxs-lookup"><span data-stu-id="0156d-298">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="0156d-299">Les règles 3, 4, 5 et 6 s’appliquent au fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="0156d-299">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="0156d-300">La règle 3 est la plus spécifique.</span><span class="sxs-lookup"><span data-stu-id="0156d-300">Rule 3 is most specific.</span></span>

<span data-ttu-id="0156d-301">Quand vous créez des enregistrements de journal avec un objet `ILogger` pour la catégorie « Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine », les enregistrements des niveaux `Trace` et supérieurs sont envoyés au fournisseur Debug, et ceux des niveaux `Debug` et supérieurs sont envoyés au fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="0156d-301">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="0156d-302">**Alias de fournisseur**</span><span class="sxs-lookup"><span data-stu-id="0156d-302">**Provider aliases**</span></span>

<span data-ttu-id="0156d-303">Vous pouvez utiliser le nom de type pour spécifier un fournisseur dans une configuration, mais chaque fournisseur définit un *alias* plus court et plus simple d’emploi.</span><span class="sxs-lookup"><span data-stu-id="0156d-303">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="0156d-304">Pour les fournisseurs intégrés, utilisez les alias suivants :</span><span class="sxs-lookup"><span data-stu-id="0156d-304">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="0156d-305">Console</span><span class="sxs-lookup"><span data-stu-id="0156d-305">Console</span></span>
- <span data-ttu-id="0156d-306">Débogage</span><span class="sxs-lookup"><span data-stu-id="0156d-306">Debug</span></span>
- <span data-ttu-id="0156d-307">EventLog</span><span class="sxs-lookup"><span data-stu-id="0156d-307">EventLog</span></span>
- <span data-ttu-id="0156d-308">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="0156d-308">AzureAppServices</span></span>
- <span data-ttu-id="0156d-309">TraceSource</span><span class="sxs-lookup"><span data-stu-id="0156d-309">TraceSource</span></span>
- <span data-ttu-id="0156d-310">EventSource</span><span class="sxs-lookup"><span data-stu-id="0156d-310">EventSource</span></span>

<span data-ttu-id="0156d-311">**Niveau minimum par défaut**</span><span class="sxs-lookup"><span data-stu-id="0156d-311">**Default minimum level**</span></span>

<span data-ttu-id="0156d-312">Un paramètre de niveau minimum est utilisé uniquement si aucune règle de la configuration ou du code ne s’applique à une catégorie ou un fournisseur spécifique.</span><span class="sxs-lookup"><span data-stu-id="0156d-312">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="0156d-313">L’exemple suivant montre comment définir le niveau minimum :</span><span class="sxs-lookup"><span data-stu-id="0156d-313">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="0156d-314">Si vous ne définissez pas explicitement le niveau minimum, la valeur par défaut est `Information`, ce qui signifie que les niveaux `Trace` et `Debug` sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="0156d-314">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="0156d-315">**Fonctions de filtre**</span><span class="sxs-lookup"><span data-stu-id="0156d-315">**Filter functions**</span></span>

<span data-ttu-id="0156d-316">Vous pouvez écrire du code dans une fonction de filtre pour appliquer des règles de filtrage.</span><span class="sxs-lookup"><span data-stu-id="0156d-316">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="0156d-317">Une fonction de filtre est appelée pour tous les fournisseurs et toutes les catégories pour lesquels la configuration ou le code n’applique aucune règle.</span><span class="sxs-lookup"><span data-stu-id="0156d-317">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="0156d-318">Le code dans la fonction a accès au type de fournisseur, à la catégorie et au niveau de journalisation pour déterminer si un message doit ou non être enregistré.</span><span class="sxs-lookup"><span data-stu-id="0156d-318">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="0156d-319">Exemple :</span><span class="sxs-lookup"><span data-stu-id="0156d-319">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0156d-320">Certains fournisseurs de journalisation vous permettent de spécifier quand des enregistrements de log doivent être écrits sur un média de stockage, ou au contraire ignorés, en fonction de la catégorie et du niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="0156d-320">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="0156d-321">Les méthodes d’extension `AddConsole` et `AddDebug` fournissent des surcharges qui vous permettent de passer des critères de filtrage.</span><span class="sxs-lookup"><span data-stu-id="0156d-321">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="0156d-322">Dans l’exemple de code suivant, le fournisseur Console ignore les enregistrements en dessous du niveau `Warning`, et le fournisseur Debug ignore les enregistrements créés par le framework.</span><span class="sxs-lookup"><span data-stu-id="0156d-322">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="0156d-323">La méthode `AddEventLog` a une surcharge qui accepte une instance `EventLogSettings`, dont la propriété `Filter` peut contenir une fonction de filtre.</span><span class="sxs-lookup"><span data-stu-id="0156d-323">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="0156d-324">Le fournisseur TraceSource ne fournit aucune de ces surcharges, étant donné que son niveau de journalisation et d’autres paramètres dépendent des `SourceSwitch` et `TraceListener` qu’il utilise.</span><span class="sxs-lookup"><span data-stu-id="0156d-324">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="0156d-325">Vous pouvez définir des règles de filtrage pour tous les fournisseurs qui sont inscrits auprès d’une instance `ILoggerFactory` à l’aide de la méthode d’extension `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="0156d-325">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="0156d-326">L’exemple suivant limite les enregistrements du framework (ceux dont la catégorie commence par « Microsoft » ou « System ») aux avertissements, mais active ceux de l’application au niveau Debug.</span><span class="sxs-lookup"><span data-stu-id="0156d-326">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="0156d-327">Si vous souhaitez utiliser le filtrage pour empêcher l’écriture de tous les enregistrements correspondant à une catégorie particulière, spécifiez `LogLevel.None` comme niveau de journalisation minimum pour cette catégorie.</span><span class="sxs-lookup"><span data-stu-id="0156d-327">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="0156d-328">La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="0156d-328">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="0156d-329">La méthode d’extension `WithFilter` est fournie par le package NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="0156d-329">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="0156d-330">La méthode retourne une nouvelle instance `ILoggerFactory` qui filtre les messages de log passés à tous les fournisseurs de journalisation inscrits dans cette méthode.</span><span class="sxs-lookup"><span data-stu-id="0156d-330">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="0156d-331">Cela n’affecte pas les autres instances `ILoggerFactory`, y compris l’instance `ILoggerFactory` initiale.</span><span class="sxs-lookup"><span data-stu-id="0156d-331">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="0156d-332">Étendues de log</span><span class="sxs-lookup"><span data-stu-id="0156d-332">Log scopes</span></span>

<span data-ttu-id="0156d-333">Vous pouvez regrouper un ensemble d’opérations logiques dans une *étendue* pour joindre les mêmes données à chaque log qui est créé dans cet ensemble.</span><span class="sxs-lookup"><span data-stu-id="0156d-333">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="0156d-334">Par exemple, vous pouvez spécifier que chaque enregistrement créé pendant le traitement d’une transaction contienne l’ID de la transaction.</span><span class="sxs-lookup"><span data-stu-id="0156d-334">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="0156d-335">Une étendue est un type `IDisposable` qui est retourné par la méthode [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope). Elle s’applique tant qu’elle n’est pas supprimée.</span><span class="sxs-lookup"><span data-stu-id="0156d-335">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="0156d-336">Pour utiliser une étendue, incluez les appels de l’objet logger dans un bloc `using`, comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="0156d-336">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="0156d-337">Le code suivant active les étendues pour le fournisseur Console :</span><span class="sxs-lookup"><span data-stu-id="0156d-337">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="0156d-338">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="0156d-338">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="0156d-339">La configuration de l’option logger `IncludeScopes` pour la console est nécessaire pour activer la journalisation basée sur des étendues.</span><span class="sxs-lookup"><span data-stu-id="0156d-339">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="0156d-340">`IncludeScopes` peut être configuré au moyen des fichiers de configuration *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="0156d-340">`IncludeScopes` can be configured via *appsettings* configuration files.</span></span> <span data-ttu-id="0156d-341">Pour plus d’informations, consultez la section [Configuration du fichier de paramètres](#settings-file-configuration).</span><span class="sxs-lookup"><span data-stu-id="0156d-341">For more information, see the [Settings file configuration](#settings-file-configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0156d-342">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="0156d-342">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="0156d-343">La configuration de l’option logger `IncludeScopes` pour la console est nécessaire pour activer la journalisation basée sur des étendues.</span><span class="sxs-lookup"><span data-stu-id="0156d-343">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0156d-344">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="0156d-344">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="0156d-345">Chaque message de log fournit les informations incluses dans l’étendue :</span><span class="sxs-lookup"><span data-stu-id="0156d-345">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="0156d-346">Fournisseurs de journalisation intégrés</span><span class="sxs-lookup"><span data-stu-id="0156d-346">Built-in logging providers</span></span>

<span data-ttu-id="0156d-347">ASP.NET Core contient les fournisseurs suivants :</span><span class="sxs-lookup"><span data-stu-id="0156d-347">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="0156d-348">Console</span><span class="sxs-lookup"><span data-stu-id="0156d-348">Console</span></span>](#console-provider)
* [<span data-ttu-id="0156d-349">Déboguer</span><span class="sxs-lookup"><span data-stu-id="0156d-349">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="0156d-350">EventSource</span><span class="sxs-lookup"><span data-stu-id="0156d-350">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="0156d-351">EventLog</span><span class="sxs-lookup"><span data-stu-id="0156d-351">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="0156d-352">TraceSource</span><span class="sxs-lookup"><span data-stu-id="0156d-352">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="0156d-353">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0156d-353">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="0156d-354">Fournisseur Console</span><span class="sxs-lookup"><span data-stu-id="0156d-354">Console provider</span></span>

<span data-ttu-id="0156d-355">Le package de fournisseur [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envoie la sortie de log dans la console.</span><span class="sxs-lookup"><span data-stu-id="0156d-355">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="0156d-356">Les [surcharges AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) vous permettent de passer un niveau de journalisation minimum, une fonction de filtre et une valeur booléenne qui indique si les étendues sont prises en charge.</span><span class="sxs-lookup"><span data-stu-id="0156d-356">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="0156d-357">Une autre option consiste à passer un objet `IConfiguration`, qui permet de spécifier la prise en charge des étendues et les niveaux de journalisation.</span><span class="sxs-lookup"><span data-stu-id="0156d-357">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="0156d-358">Si vous envisagez d’utiliser le fournisseur Console dans un environnement de production, sachez qu’il impacte fortement les performances.</span><span class="sxs-lookup"><span data-stu-id="0156d-358">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="0156d-359">Quand vous créez un projet dans Visual Studio, la méthode `AddConsole` ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="0156d-359">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="0156d-360">Ce code fait référence à la section `Logging` du fichier *appSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="0156d-360">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="0156d-361">Les paramètres définis limitent les enregistrements du framework aux avertissements, mais active ceux de l’application au niveau Debug, comme cela est expliqué dans la section [Filtrage de log](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="0156d-361">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="0156d-362">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="0156d-362">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="0156d-363">Fournisseur Debug</span><span class="sxs-lookup"><span data-stu-id="0156d-363">Debug provider</span></span>

<span data-ttu-id="0156d-364">Le package de fournisseur [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) écrit la sortie de log à l’aide de la classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (appels de la méthode `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="0156d-364">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="0156d-365">Sur Linux, ce fournisseur écrit la sortie de log dans */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="0156d-365">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="0156d-366">Les [surcharges AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) vous permettent de passer un niveau de log minimum ou une fonction de filtre.</span><span class="sxs-lookup"><span data-stu-id="0156d-366">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="0156d-367">Fournisseur EventSource</span><span class="sxs-lookup"><span data-stu-id="0156d-367">EventSource provider</span></span>

<span data-ttu-id="0156d-368">Pour les applications qui ciblent ASP.NET Core 1.1.0 ou une version ultérieure, le package de fournisseur [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) peut implémenter le suivi des événements.</span><span class="sxs-lookup"><span data-stu-id="0156d-368">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="0156d-369">Sur Windows, il utilise [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="0156d-369">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="0156d-370">Le fournisseur est multiplateforme, mais il ne prend pas en charge la collection d’événements ni les outils d’affichage pour Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="0156d-370">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

<span data-ttu-id="0156d-371">[L’utilitaire PerfView](https://github.com/Microsoft/perfview) est très utile pour collecter et afficher les journaux.</span><span class="sxs-lookup"><span data-stu-id="0156d-371">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="0156d-372">Il existe d’autres outils d’affichage des journaux ETW, mais PerfView est l’outil recommandé pour gérer les événements ETW générés par ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0156d-372">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="0156d-373">Pour configurer PerfView afin qu’il collecte les événements enregistrés par ce fournisseur, ajoutez la chaîne `*Microsoft-Extensions-Logging` à la liste des **fournisseurs supplémentaires**.</span><span class="sxs-lookup"><span data-stu-id="0156d-373">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="0156d-374">(N’oubliez pas d’inclure l’astérisque au début de la chaîne.)</span><span class="sxs-lookup"><span data-stu-id="0156d-374">(Don't miss the asterisk at the start of the string.)</span></span>

![Fournisseurs supplémentaires dans PerfView](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="0156d-376">Fournisseur EventLog Windows</span><span class="sxs-lookup"><span data-stu-id="0156d-376">Windows EventLog provider</span></span>

<span data-ttu-id="0156d-377">Le package de fournisseur [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envoie la sortie de log dans le journal des événements Windows.</span><span class="sxs-lookup"><span data-stu-id="0156d-377">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="0156d-378">Les [surcharges AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) vous permettent de passer `EventLogSettings` ou un niveau de journalisation minimum.</span><span class="sxs-lookup"><span data-stu-id="0156d-378">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="0156d-379">Fournisseur TraceSource</span><span class="sxs-lookup"><span data-stu-id="0156d-379">TraceSource provider</span></span>

<span data-ttu-id="0156d-380">Le package de fournisseur [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) utilise les bibliothèques et fournisseurs [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="0156d-380">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="0156d-381">Les [surcharges AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) vous permettent de passer un commutateur de source et un écouteur de suivi.</span><span class="sxs-lookup"><span data-stu-id="0156d-381">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="0156d-382">Pour utiliser ce fournisseur, l’application doit s’exécuter sur .NET Framework (au lieu de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="0156d-382">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="0156d-383">Le fournisseur vous permet d’envoyer les messages vers différents [écouteurs](/dotnet/framework/debug-trace-profile/trace-listeners), tels que le [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) utilisé dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="0156d-383">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="0156d-384">L’exemple suivant configure un fournisseur `TraceSource` qui enregistre les messages `Warning` et de niveau supérieur dans la fenêtre de la console.</span><span class="sxs-lookup"><span data-stu-id="0156d-384">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="0156d-385">Fournisseur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0156d-385">Azure App Service provider</span></span>

<span data-ttu-id="0156d-386">Le package de fournisseur [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) écrit les enregistrements de log dans des fichiers texte dans le système de fichiers d’une application Azure App Service, et dans un [stockage Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) dans un compte de stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="0156d-386">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="0156d-387">Le fournisseur est disponible uniquement pour les applications qui ciblent ASP.NET Core 1.1 ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="0156d-387">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0156d-388">Si vous ciblez .NET Core, n’installez pas le package du fournisseur ni n’appelez explicitement [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="0156d-388">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="0156d-389">Le fournisseur est automatiquement disponible pour l’application qui est déployée sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0156d-389">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="0156d-390">Si vous ciblez le .NET Framework, ajoutez le package du fournisseur dans le projet et appelez `AddAzureWebAppDiagnostics` :</span><span class="sxs-lookup"><span data-stu-id="0156d-390">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="0156d-391">Une surcharge [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) vous permet de passer dans [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) avec lequel vous pouvez remplacer les paramètres par défaut, tels que le modèle de sortie de journalisation, le nom du blob et la limite de taille de fichier.</span><span class="sxs-lookup"><span data-stu-id="0156d-391">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="0156d-392">(Le *modèle de sortie* est un modèle de message qui est appliqué à tous les enregistrements de log au-dessus de celui fourni quand vous appelez une méthode `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="0156d-392">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

::: moniker-end

<span data-ttu-id="0156d-393">Quand vous effectuez un déploiement sur une application App Service, l’application honore les paramètres définis dans la section [Journaux de diagnostic](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) de la page **App Service** du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="0156d-393">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="0156d-394">Lorsque ces paramètres sont mis à jour, les changements prennent effet immédiatement sans avoir besoin de redémarrer ou redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="0156d-394">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Paramètres de journalisation Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="0156d-396">L’emplacement par défaut des fichiers journaux est le dossier *D:\\racine\\LogFiles\\Application*, et le nom de fichier par défaut est *diagnostics-aaaammjj.txt*.</span><span class="sxs-lookup"><span data-stu-id="0156d-396">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="0156d-397">La limite de taille de fichier par défaut est de 10 Mo, et le nombre maximal de fichiers conservés par défaut est de 2.</span><span class="sxs-lookup"><span data-stu-id="0156d-397">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="0156d-398">Le nom par défaut du blob est *{app-name}{timestamp}/aaaa/mm/jj/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="0156d-398">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="0156d-399">Pour plus d’informations sur le comportement par défaut, consultez [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="0156d-399">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="0156d-400">Le fournisseur fonctionne uniquement quand le projet s’exécute dans l’environnement Azure.</span><span class="sxs-lookup"><span data-stu-id="0156d-400">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="0156d-401">Il n’a aucun effet si le projet s’exécute localement &mdash; il n’écrit pas d’enregistrements dans les fichiers locaux ou dans le stockage de développement local pour les objets blob.</span><span class="sxs-lookup"><span data-stu-id="0156d-401">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="0156d-402">Fournisseurs de journalisation tiers</span><span class="sxs-lookup"><span data-stu-id="0156d-402">Third-party logging providers</span></span>

<span data-ttu-id="0156d-403">Frameworks de journalisation tiers qui sont pris en charge dans ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="0156d-403">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="0156d-404">[elmah.io](https://elmah.io/) ([dépôt GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0156d-404">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="0156d-405">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([Dépôt GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="0156d-405">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="0156d-406">[JSNLog](http://jsnlog.com/) ([dépôt GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="0156d-406">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="0156d-407">[Loggr](http://loggr.net/) ([dépôt GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0156d-407">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="0156d-408">[NLog](http://nlog-project.org/) ([dépôt GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0156d-408">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="0156d-409">[Serilog](https://serilog.net/) ([dépôt GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="0156d-409">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="0156d-410">Certains frameworks tiers prennent en charge la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="0156d-410">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="0156d-411">L’utilisation d’un framework tiers est semblable à l’utilisation des fournisseurs intégrés :</span><span class="sxs-lookup"><span data-stu-id="0156d-411">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="0156d-412">Ajoutez un package NuGet à votre projet.</span><span class="sxs-lookup"><span data-stu-id="0156d-412">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="0156d-413">Appelez une méthode d’extension sur `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="0156d-413">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="0156d-414">Pour plus d’informations, consultez la documentation du framework.</span><span class="sxs-lookup"><span data-stu-id="0156d-414">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="0156d-415">Streaming des journaux Azure</span><span class="sxs-lookup"><span data-stu-id="0156d-415">Azure log streaming</span></span>

<span data-ttu-id="0156d-416">Le streaming des journaux Azure vous permet d’afficher l’activité de journalisation en temps réel à partir des emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="0156d-416">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="0156d-417">Serveur d’applications</span><span class="sxs-lookup"><span data-stu-id="0156d-417">The application server</span></span>
* <span data-ttu-id="0156d-418">Serveur web</span><span class="sxs-lookup"><span data-stu-id="0156d-418">The web server</span></span>
* <span data-ttu-id="0156d-419">Suivi des demandes ayant échoué</span><span class="sxs-lookup"><span data-stu-id="0156d-419">Failed request tracing</span></span>

<span data-ttu-id="0156d-420">Pour configurer le streaming des journaux Azure :</span><span class="sxs-lookup"><span data-stu-id="0156d-420">To configure Azure log streaming:</span></span>

* <span data-ttu-id="0156d-421">Accédez à la page **Journaux de diagnostic** à partir du portail de votre application.</span><span class="sxs-lookup"><span data-stu-id="0156d-421">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="0156d-422">Définissez l’option **Journal des applications (Système de fichiers)** sur Activé.</span><span class="sxs-lookup"><span data-stu-id="0156d-422">Set **Application Logging (Filesystem)** to on.</span></span>

![Page Journaux de diagnostic du portail Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="0156d-424">Accédez à la page **Streaming des journaux** pour afficher les messages d’application.</span><span class="sxs-lookup"><span data-stu-id="0156d-424">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="0156d-425">Les messages sont enregistrés par application dans l’interface `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="0156d-425">They're logged by application through the `ILogger` interface.</span></span>

![Streaming des journaux d’application dans le portail Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="0156d-427">Journalisation des traces Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="0156d-427">Azure Application Insights trace logging</span></span>

<span data-ttu-id="0156d-428">Le kit SDK [Application Insights](https://azure.microsoft.com/services/application-insights/) est capable de collecter la télémétrie des traces à partir de journaux générés par l’infrastructure de journalisation ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0156d-428">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="0156d-429">Pour plus d’informations, consultez [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="0156d-429">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0156d-430">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0156d-430">Additional resources</span></span>

[<span data-ttu-id="0156d-431">Journalisation avancée avec LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="0156d-431">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
