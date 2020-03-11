---
title: Journalisation dans .NET Core et ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser le framework de journalisation fourni par le package NuGet Microsoft.Extensions.Logging.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/05/2020
uid: fundamentals/logging/index
ms.openlocfilehash: 58e236ad7f0863b87907d5585e1cb6bf61d46e99
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663302"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="32d30-103">Journalisation dans .NET Core et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32d30-103">Logging in .NET Core and ASP.NET Core</span></span>

<span data-ttu-id="32d30-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="32d30-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="32d30-105">.NET Core prend en charge une API de journalisation qui fonctionne avec différents fournisseurs de journalisation tiers et intégrés.</span><span class="sxs-lookup"><span data-stu-id="32d30-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="32d30-106">Cet article explique comment utiliser cette API de journalisation avec les fournisseurs de journalisation intégrés.</span><span class="sxs-lookup"><span data-stu-id="32d30-106">This article shows how to use the logging API with built-in providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32d30-107">La plupart des exemples de code présentés dans cet article proviennent d’applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d30-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="32d30-108">Les parties spécifiques à la journalisation de ces extraits de code s’appliquent à n’importe quelle application .NET Core qui utilise l' [hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="32d30-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="32d30-109">Pour obtenir un exemple d’utilisation de l’hôte générique dans une application de console non Web, consultez le fichier *Program.cs* de l' [exemple d’application tâches en arrière-plan](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span><span class="sxs-lookup"><span data-stu-id="32d30-109">For an example of how to use the Generic Host in a non-web console app, see the *Program.cs* file of the [Background Tasks sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span></span>

<span data-ttu-id="32d30-110">Le code de journalisation pour les applications sans hôte générique diffère dans la façon dont les [fournisseurs sont ajoutés](#add-providers) et les [enregistreurs d'événements créés](#create-logs).</span><span class="sxs-lookup"><span data-stu-id="32d30-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="32d30-111">Des exemples de code non hôte sont présentés dans ces sections de l’article.</span><span class="sxs-lookup"><span data-stu-id="32d30-111">Non-host code examples are shown in those sections of the article.</span></span>

::: moniker-end

<span data-ttu-id="32d30-112">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32d30-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="32d30-113">Ajouter des fournisseurs</span><span class="sxs-lookup"><span data-stu-id="32d30-113">Add providers</span></span>

<span data-ttu-id="32d30-114">Un fournisseur de journalisation affiche ou stocke des journaux.</span><span class="sxs-lookup"><span data-stu-id="32d30-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="32d30-115">Par exemple, le fournisseur Console affiche les journaux dans la console, et le fournisseur Azure Application Insights les stocke dans Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="32d30-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="32d30-116">Il est possible d’envoyer les journaux à plusieurs endroits en ajoutant plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="32d30-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32d30-117">Pour ajouter un fournisseur dans une application qui utilise un hôte générique, appelez la méthode d’extension `Add{provider name}` du fournisseur dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="32d30-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="32d30-118">Dans une application de console non hôte, appelez la méthode d’extension `Add{provider name}` du fournisseur lors de la création d’un `LoggerFactory` :</span><span class="sxs-lookup"><span data-stu-id="32d30-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="32d30-119">`LoggerFactory` et `AddConsole` requièrent une instruction `using` pour `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="32d30-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="32d30-120">Les modèles de projet ASP.NET Core par défaut appellent <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, qui ajoute les fournisseurs de journalisation suivants :</span><span class="sxs-lookup"><span data-stu-id="32d30-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* [<span data-ttu-id="32d30-121">Console</span><span class="sxs-lookup"><span data-stu-id="32d30-121">Console</span></span>](#console-provider)
* [<span data-ttu-id="32d30-122">Déboguer</span><span class="sxs-lookup"><span data-stu-id="32d30-122">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="32d30-123">EventSource</span><span class="sxs-lookup"><span data-stu-id="32d30-123">EventSource</span></span>](#event-source-provider)
* <span data-ttu-id="32d30-124">[EventLog](#windows-eventlog-provider) (uniquement en cas d’exécution sur Windows)</span><span class="sxs-lookup"><span data-stu-id="32d30-124">[EventLog](#windows-eventlog-provider) (only when running on Windows)</span></span>

<span data-ttu-id="32d30-125">Vous pouvez remplacer les fournisseurs par défaut par ceux de votre choix.</span><span class="sxs-lookup"><span data-stu-id="32d30-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="32d30-126">Appelez <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> et ajoutez les fournisseurs que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="32d30-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0 "

<span data-ttu-id="32d30-127">Pour ajouter un fournisseur, appelez la méthode d’extension `Add{provider name}` du fournisseur dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="32d30-127">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="32d30-128">Le code précédent nécessite des références à `Microsoft.Extensions.Logging` et `Microsoft.Extensions.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="32d30-128">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="32d30-129">Le modèle de projet par défaut appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, qui ajoute les fournisseurs de journalisation suivants :</span><span class="sxs-lookup"><span data-stu-id="32d30-129">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="32d30-130">Console</span><span class="sxs-lookup"><span data-stu-id="32d30-130">Console</span></span>
* <span data-ttu-id="32d30-131">Débogage</span><span class="sxs-lookup"><span data-stu-id="32d30-131">Debug</span></span>
* <span data-ttu-id="32d30-132">EventSource (à partir d’ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="32d30-132">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="32d30-133">Si vous utilisez `CreateDefaultBuilder`, vous pouvez remplacer les fournisseurs par défaut par ceux de votre choix.</span><span class="sxs-lookup"><span data-stu-id="32d30-133">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="32d30-134">Appelez <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> et ajoutez les fournisseurs que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="32d30-134">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

<span data-ttu-id="32d30-135">Vous trouverez des informations sur les [fournisseurs de journalisation intégrés](#built-in-logging-providers) et les [fournisseurs de journalisation tiers](#third-party-logging-providers) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="32d30-135">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="32d30-136">Créer des journaux</span><span class="sxs-lookup"><span data-stu-id="32d30-136">Create logs</span></span>

<span data-ttu-id="32d30-137">Pour créer des journaux, utilisez un objet <xref:Microsoft.Extensions.Logging.ILogger%601>.</span><span class="sxs-lookup"><span data-stu-id="32d30-137">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="32d30-138">Dans une application web ou un service hébergé, obtenez un `ILogger` à partir de l’injection de dépendances (DI).</span><span class="sxs-lookup"><span data-stu-id="32d30-138">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="32d30-139">Dans les applications de console non hôtes, utilisez le `LoggerFactory` pour créer un `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="32d30-139">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="32d30-140">L’exemple d’ASP.NET Core suivant crée un enregistreur d’événements de catégorie `TodoApiSample.Pages.AboutModel`.</span><span class="sxs-lookup"><span data-stu-id="32d30-140">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="32d30-141">La *catégorie* du journal est une chaîne associée à chaque journal.</span><span class="sxs-lookup"><span data-stu-id="32d30-141">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="32d30-142">L’instance `ILogger<T>` fournie par l’injection de dépendances crée des journaux ayant comme catégorie un nom complet de type `T`.</span><span class="sxs-lookup"><span data-stu-id="32d30-142">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="32d30-143">L’exemple d’application de console non hôte suivant crée un enregistreur d’événements de catégorie `LoggingConsoleApp.Program`.</span><span class="sxs-lookup"><span data-stu-id="32d30-143">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

::: moniker-end

<span data-ttu-id="32d30-144">Dans les exemples d’ASP.NET Core et d’application de console suivants, l’enregistreur d’événements est utilisé pour créer des journaux de niveau `Information`.</span><span class="sxs-lookup"><span data-stu-id="32d30-144">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="32d30-145">Le *niveau* du journal indique la gravité de l’événement consigné.</span><span class="sxs-lookup"><span data-stu-id="32d30-145">The Log *level* indicates the severity of the logged event.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

<span data-ttu-id="32d30-146">Les [niveaux](#log-level) et les [catégories](#log-category) sont expliqués plus en détail plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="32d30-146">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-3.0"

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="32d30-147">Créer des journaux dans la classe Programme</span><span class="sxs-lookup"><span data-stu-id="32d30-147">Create logs in the Program class</span></span>

<span data-ttu-id="32d30-148">Pour écrire des journaux dans la classe `Program` d’une application ASP.NET Core, récupérez une instance `ILogger` de l’injection de dépendances après la création de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="32d30-148">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/TodoApiSample/Program.cs?highlight=9,10)]

<span data-ttu-id="32d30-149">La journalisation pendant la construction de l’hôte n’est pas prise en charge directement.</span><span class="sxs-lookup"><span data-stu-id="32d30-149">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="32d30-150">Toutefois, un enregistreur d’événements distinct peut être utilisé.</span><span class="sxs-lookup"><span data-stu-id="32d30-150">However, a separate logger can be used.</span></span> <span data-ttu-id="32d30-151">Dans l’exemple suivant, un enregistreur d’événements [Serilog](https://serilog.net/) est utilisé pour se connecter `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="32d30-151">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateHostBuilder`.</span></span> <span data-ttu-id="32d30-152">`AddSerilog` utilise la configuration statique spécifiée dans `Log.Logger`:</span><span class="sxs-lookup"><span data-stu-id="32d30-152">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return Host.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddRazorPages();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {   
                    logging.AddSerilog();
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="32d30-153">Créer des journaux dans la classe de démarrage</span><span class="sxs-lookup"><span data-stu-id="32d30-153">Create logs in the Startup class</span></span>

<span data-ttu-id="32d30-154">Pour écrire des journaux dans la méthode `Startup.Configure` d’une application ASP.NET Core, incluez un paramètre `ILogger` dans la signature de la méthode :</span><span class="sxs-lookup"><span data-stu-id="32d30-154">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="32d30-155">L’écriture de journaux avant la fin de l’installation du conteneur d’injection de dépendances dans la méthode `Startup.ConfigureServices` n’est pas prise en charge :</span><span class="sxs-lookup"><span data-stu-id="32d30-155">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="32d30-156">L’injection d’un enregistreur d’événements dans le constructeur `Startup` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="32d30-156">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="32d30-157">L’injection d’un enregistreur d’événements dans la signature de méthode `Startup.ConfigureServices` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="32d30-157">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="32d30-158">La raison de cette restriction est que la journalisation dépend de l’injection de dépendances et de la configuration qui, à son tour, dépend de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="32d30-158">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="32d30-159">Le conteneur d’injection de dépendances n’est pas configuré avant que `ConfigureServices` soit terminé.</span><span class="sxs-lookup"><span data-stu-id="32d30-159">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="32d30-160">L’injection de constructeur d’un enregistreur d’événements dans `Startup` fonctionne dans les versions antérieures d’ASP.NET Core, car un conteneur d’injection de dépendances distinct est créé pour l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="32d30-160">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="32d30-161">Pour plus d’informations sur la raison de la création d’un seul conteneur pour l’hôte générique, consultez l’[annonce de changement cassant](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="32d30-161">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="32d30-162">Si vous devez configurer un service qui dépend de `ILogger<T>`, vous pouvez toujours le faire à l’aide de l’injection de constructeur, ou avec une méthode de fabrique.</span><span class="sxs-lookup"><span data-stu-id="32d30-162">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="32d30-163">L’approche de la méthode de fabrique est recommandée uniquement s’il n’y a aucune autre option.</span><span class="sxs-lookup"><span data-stu-id="32d30-163">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="32d30-164">Supposons, par exemple, que vous deviez remplir une propriété avec un service à partir de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="32d30-164">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="32d30-165">Le code en surbrillance précédent est une `Func` qui s’exécute la première fois que le conteneur d’injection de dépendances doit construire une instance de `MyService`.</span><span class="sxs-lookup"><span data-stu-id="32d30-165">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="32d30-166">Vous pouvez accéder à tous les services inscrits de cette manière.</span><span class="sxs-lookup"><span data-stu-id="32d30-166">You can access any of the registered services in this way.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="32d30-167">Créer des journaux au démarrage</span><span class="sxs-lookup"><span data-stu-id="32d30-167">Create logs in Startup</span></span>

<span data-ttu-id="32d30-168">Pour écrire des journaux dans la classe `Startup`, ajoutez un paramètre `ILogger` dans la signature de constructeur :</span><span class="sxs-lookup"><span data-stu-id="32d30-168">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="32d30-169">Créer des journaux dans la classe Programme</span><span class="sxs-lookup"><span data-stu-id="32d30-169">Create logs in the Program class</span></span>

<span data-ttu-id="32d30-170">Pour écrire des journaux dans la classe `Program`, récupérez une instance `ILogger` auprès de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="32d30-170">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="32d30-171">La journalisation pendant la construction de l’hôte n’est pas prise en charge directement.</span><span class="sxs-lookup"><span data-stu-id="32d30-171">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="32d30-172">Toutefois, un enregistreur d’événements distinct peut être utilisé.</span><span class="sxs-lookup"><span data-stu-id="32d30-172">However, a separate logger can be used.</span></span> <span data-ttu-id="32d30-173">Dans l’exemple suivant, un enregistreur d’événements [Serilog](https://serilog.net/) est utilisé pour se connecter `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="32d30-173">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateWebHostBuilder`.</span></span> <span data-ttu-id="32d30-174">`AddSerilog` utilise la configuration statique spécifiée dans `Log.Logger`:</span><span class="sxs-lookup"><span data-stu-id="32d30-174">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

```csharp
using System;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return WebHost.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddMvc();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {
                    logging.AddSerilog();
                })
                .UseStartup<Startup>();
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="32d30-175">Aucune méthode d’enregistreur d’événements asynchrone</span><span class="sxs-lookup"><span data-stu-id="32d30-175">No asynchronous logger methods</span></span>

<span data-ttu-id="32d30-176">La journalisation doit être suffisamment rapide par rapport au coût du code asynchrone en matière de performances.</span><span class="sxs-lookup"><span data-stu-id="32d30-176">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="32d30-177">Si votre magasin de données de journalisation est lent, n’écrivez pas directement dedans.</span><span class="sxs-lookup"><span data-stu-id="32d30-177">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="32d30-178">Écrivez au départ les messages de journal dans un magasin rapide, puis déplacez-les dans le magasin lent.</span><span class="sxs-lookup"><span data-stu-id="32d30-178">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="32d30-179">Par exemple, si vous vous connectez à SQL Server, évitez de le faire directement dans une méthode `Log`, car les méthodes `Log` sont synchrones.</span><span class="sxs-lookup"><span data-stu-id="32d30-179">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="32d30-180">Ajoutez plutôt de façon synchronisée des messages de journal à une file d’attente en mémoire, puis configurez un traitement en arrière-plan afin d’extraire les messages de la file d’attente et d’effectuer le travail asynchrone d’envoi des données vers SQL Server.</span><span class="sxs-lookup"><span data-stu-id="32d30-180">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="32d30-181">Pour plus d’informations, consultez [ce](https://github.com/dotnet/AspNetCore.Docs/issues/11801) problème github.</span><span class="sxs-lookup"><span data-stu-id="32d30-181">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

## <a name="configuration"></a><span data-ttu-id="32d30-182">Configuration</span><span class="sxs-lookup"><span data-stu-id="32d30-182">Configuration</span></span>

<span data-ttu-id="32d30-183">La configuration de fournisseur de journalisation est fournie par un ou plusieurs fournisseurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="32d30-183">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="32d30-184">Formats de fichiers (INI, JSON et XML).</span><span class="sxs-lookup"><span data-stu-id="32d30-184">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="32d30-185">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="32d30-185">Command-line arguments.</span></span>
* <span data-ttu-id="32d30-186">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="32d30-186">Environment variables.</span></span>
* <span data-ttu-id="32d30-187">Objets .NET en mémoire.</span><span class="sxs-lookup"><span data-stu-id="32d30-187">In-memory .NET objects.</span></span>
* <span data-ttu-id="32d30-188">Stockage [Secret Manager](xref:security/app-secrets) non chiffré.</span><span class="sxs-lookup"><span data-stu-id="32d30-188">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="32d30-189">Magasin utilisateur chiffré comme [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="32d30-189">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="32d30-190">Fournisseurs personnalisés (installés ou créés).</span><span class="sxs-lookup"><span data-stu-id="32d30-190">Custom providers (installed or created).</span></span>

<span data-ttu-id="32d30-191">Par exemple, la configuration de journalisation est généralement fournie par la section `Logging` des fichiers de paramètres d’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-191">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="32d30-192">L’exemple suivant montre le contenu d’un fichier *appsettings.Development.json* standard :</span><span class="sxs-lookup"><span data-stu-id="32d30-192">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="32d30-193">La propriété `Logging` peut avoir un niveau `LogLevel` et des propriétés de module fournisseur d'informations (Console ici).</span><span class="sxs-lookup"><span data-stu-id="32d30-193">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="32d30-194">La propriété `LogLevel` sous `Logging` spécifie le [niveau](#log-level) de journalisation minimal pour les catégories sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="32d30-194">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="32d30-195">Dans l’exemple, les catégories `System` et `Microsoft` sont consignées au niveau `Information`, et toutes les autres au niveau `Debug`.</span><span class="sxs-lookup"><span data-stu-id="32d30-195">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="32d30-196">Les autres propriétés sous `Logging` spécifient les fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="32d30-196">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="32d30-197">Cet exemple concerne le fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="32d30-197">The example is for the Console provider.</span></span> <span data-ttu-id="32d30-198">Lorsqu’un fournisseur prend en charge les [étendues de journal](#log-scopes), `IncludeScopes` indique si elles sont activées.</span><span class="sxs-lookup"><span data-stu-id="32d30-198">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="32d30-199">Une propriété de fournisseur (comme `Console` dans l’exemple) peut également spécifier une propriété `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="32d30-199">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="32d30-200">`LogLevel` sous un fournisseur spécifie les niveaux à consigner pour ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="32d30-200">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="32d30-201">Si les niveaux sont spécifiés dans `Logging.{providername}.LogLevel`, ils remplacent ce qui est défini dans `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="32d30-201">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

<span data-ttu-id="32d30-202">L’API de journalisation n’inclut pas de scénario pour modifier les niveaux de journal lorsqu’une application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="32d30-202">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="32d30-203">Toutefois, certains fournisseurs de configuration sont en charge du rechargement de la configuration, ce qui prend immédiatement effet sur la configuration de la journalisation.</span><span class="sxs-lookup"><span data-stu-id="32d30-203">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="32d30-204">Par exemple, le [fournisseur de configuration de fichier](xref:fundamentals/configuration/index#file-configuration-provider), qui est ajouté par `CreateDefaultBuilder` pour lire les fichiers de paramètres, recharge la configuration de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="32d30-204">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), which is added by `CreateDefaultBuilder` to read settings files, reloads logging configuration by default.</span></span> <span data-ttu-id="32d30-205">Si la configuration est modifiée dans le code pendant qu’une application est en cours d’exécution, l’application peut appeler [IConfigurationRoot. Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) pour mettre à jour la configuration de journalisation de l’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-205">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

<span data-ttu-id="32d30-206">Pour plus d’informations sur l’implémentation des fournisseurs de configuration, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="32d30-206">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="32d30-207">Exemple de sortie de la journalisation</span><span class="sxs-lookup"><span data-stu-id="32d30-207">Sample logging output</span></span>

<span data-ttu-id="32d30-208">Avec l’exemple de code présenté dans la section précédente, les journaux s’affichent dans la console lorsque l’application est exécutée en ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="32d30-208">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="32d30-209">Voici un exemple de sortie de la console :</span><span class="sxs-lookup"><span data-stu-id="32d30-209">Here's an example of console output:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 84.26180000000001ms 307
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/api/todo/0
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

<span data-ttu-id="32d30-210">Les journaux précédents ont été générés par une requête HTTP Get à l’exemple d’application à l’adresse `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="32d30-210">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="32d30-211">Voici un exemple des mêmes journaux tels qu’ils s’affichent dans la fenêtre Débogage quand vous exécutez l’exemple d’application dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="32d30-211">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request starting HTTP/2.0 GET https://localhost:44328/api/todo/0  
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
TodoApiSample.Controllers.TodoController: Information: Getting item 0
TodoApiSample.Controllers.TodoController: Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult: Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 34.167ms
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request finished in 98.41300000000001ms 404
```

<span data-ttu-id="32d30-212">Les journaux créés par les appels de `ILogger` illustrés dans la section précédente commencent par « TodoApiSample ».</span><span class="sxs-lookup"><span data-stu-id="32d30-212">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="32d30-213">Ceux qui commencent par les catégories « Microsoft » proviennent du code du framework ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d30-213">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="32d30-214">ASP.NET Core et le code d’application utilisent la même API de journalisation et les mêmes fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="32d30-214">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="32d30-215">Les journaux créés par les appels de `ILogger` illustrés dans la section précédente commencent par « TodoApi ».</span><span class="sxs-lookup"><span data-stu-id="32d30-215">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="32d30-216">Ceux qui commencent par les catégories « Microsoft » proviennent du code du framework ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d30-216">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="32d30-217">ASP.NET Core et le code d’application utilisent la même API de journalisation et les mêmes fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="32d30-217">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

<span data-ttu-id="32d30-218">Les autres sections de cet article détaillent certains points et présentent les options de journalisation.</span><span class="sxs-lookup"><span data-stu-id="32d30-218">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="32d30-219">Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="32d30-219">NuGet packages</span></span>

<span data-ttu-id="32d30-220">Les interfaces `ILogger` et `ILoggerFactory` se trouvent dans [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), et leurs implémentations par défaut se trouvent dans [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="32d30-220">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="32d30-221">Catégorie de journal</span><span class="sxs-lookup"><span data-stu-id="32d30-221">Log category</span></span>

<span data-ttu-id="32d30-222">Quand un objet `ILogger` est créé, une *catégorie* lui est spécifiée.</span><span class="sxs-lookup"><span data-stu-id="32d30-222">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="32d30-223">Cette catégorie est incluse dans tous les messages de journal créés par cette instance de `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="32d30-223">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="32d30-224">Si la catégorie peut être n’importe quelle chaîne, la convention est d’utiliser le nom de la classe, par exemple « TodoApi.Controllers.TodoController ».</span><span class="sxs-lookup"><span data-stu-id="32d30-224">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="32d30-225">Utilisez `ILogger<T>` pour obtenir une instance de `ILogger` qui utilise le nom de type complet `T` comme catégorie :</span><span class="sxs-lookup"><span data-stu-id="32d30-225">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="32d30-226">Pour spécifier explicitement la catégorie, appelez `ILoggerFactory.CreateLogger` :</span><span class="sxs-lookup"><span data-stu-id="32d30-226">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="32d30-227">`ILogger<T>` équivaut à appeler `CreateLogger` avec le nom de type complet `T`.</span><span class="sxs-lookup"><span data-stu-id="32d30-227">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="32d30-228">Log level</span><span class="sxs-lookup"><span data-stu-id="32d30-228">Log level</span></span>

<span data-ttu-id="32d30-229">Chaque journal spécifie une valeur <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="32d30-229">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="32d30-230">Le niveau de journalisation indique la gravité ou l’importance.</span><span class="sxs-lookup"><span data-stu-id="32d30-230">The log level indicates the severity or importance.</span></span> <span data-ttu-id="32d30-231">Vous pourriez par exemple écrire un journal `Information` lorsqu’une méthode se termine normalement et un journal `Warning` lorsqu’une méthode retourne un code de statut *404 Not Found*.</span><span class="sxs-lookup"><span data-stu-id="32d30-231">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="32d30-232">Le code suivant crée des journaux `Information` et `Warning` :</span><span class="sxs-lookup"><span data-stu-id="32d30-232">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="32d30-233">Dans le code précédent, le premier paramètre est [l’ID de l’événement de journal](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="32d30-233">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="32d30-234">Le deuxième paramètre est un modèle de message contenant des espaces réservés pour les valeurs d’argument fournies par les autres paramètres de méthode.</span><span class="sxs-lookup"><span data-stu-id="32d30-234">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="32d30-235">Les paramètres de méthode sont expliqués dans la section [Modèle de message](#log-message-template) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="32d30-235">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="32d30-236">Les méthodes de journal qui comportent le niveau dans leur nom (par exemple, `LogInformation` et `LogWarning`) sont des [méthodes d’extension pour ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="32d30-236">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="32d30-237">Elles appellent une méthode `Log` qui prend un paramètre `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="32d30-237">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="32d30-238">Vous pouvez appeler la méthode `Log` directement au lieu d’appeler ces méthodes d’extension, mais la syntaxe est relativement complexe.</span><span class="sxs-lookup"><span data-stu-id="32d30-238">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="32d30-239">Pour plus d’informations, voir <xref:Microsoft.Extensions.Logging.ILogger> et le [code source des extensions d’enregistreur d'événements](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="32d30-239">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="32d30-240">ASP.NET Core définit les niveaux de journalisation suivants, classés selon leur degré de gravité (du plus faible au plus élevé).</span><span class="sxs-lookup"><span data-stu-id="32d30-240">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="32d30-241">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="32d30-241">Trace = 0</span></span>

  <span data-ttu-id="32d30-242">Informations en général exclusivement utiles à des fins de débogage.</span><span class="sxs-lookup"><span data-stu-id="32d30-242">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="32d30-243">Ces messages peuvent contenir des données d’application sensibles. Ils ne doivent donc pas être activés dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="32d30-243">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="32d30-244">*Niveau désactivé par défaut.*</span><span class="sxs-lookup"><span data-stu-id="32d30-244">*Disabled by default.*</span></span>

* <span data-ttu-id="32d30-245">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="32d30-245">Debug = 1</span></span>

  <span data-ttu-id="32d30-246">Informations qui peuvent être utiles dans le développement et le débogage.</span><span class="sxs-lookup"><span data-stu-id="32d30-246">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="32d30-247">Exemple : `Entering method Configure with flag set to true.` En raison de leur volume élevé, n’activez les journaux de niveau `Debug` en production que pour résoudre des problèmes.</span><span class="sxs-lookup"><span data-stu-id="32d30-247">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="32d30-248">Information = 2</span><span class="sxs-lookup"><span data-stu-id="32d30-248">Information = 2</span></span>

  <span data-ttu-id="32d30-249">Informations de suivi du flux général de l’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-249">For tracking the general flow of the app.</span></span> <span data-ttu-id="32d30-250">Ces journaux ont généralement une utilité à long terme.</span><span class="sxs-lookup"><span data-stu-id="32d30-250">These logs typically have some long-term value.</span></span> <span data-ttu-id="32d30-251">Exemple : `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="32d30-251">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="32d30-252">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="32d30-252">Warning = 3</span></span>

  <span data-ttu-id="32d30-253">Informations sur les événements anormaux ou inattendus dans le flux de l’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-253">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="32d30-254">Il peut s’agir d’erreurs ou d’autres situations qui ne provoquent pas l’arrêt de l’application, mais qu’il peut être intéressant d’examiner.</span><span class="sxs-lookup"><span data-stu-id="32d30-254">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="32d30-255">Le niveau de journalisation `Warning` est généralement utilisé pour les exceptions gérées.</span><span class="sxs-lookup"><span data-stu-id="32d30-255">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="32d30-256">Exemple : `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="32d30-256">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="32d30-257">Error = 4</span><span class="sxs-lookup"><span data-stu-id="32d30-257">Error = 4</span></span>

  <span data-ttu-id="32d30-258">Fournit des informations sur des erreurs et des exceptions qui ne peuvent pas être gérées.</span><span class="sxs-lookup"><span data-stu-id="32d30-258">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="32d30-259">Ces messages indiquent un échec de l’activité ou de l’opération en cours (par exemple, la requête HTTP actuellement exécutée), pas un échec au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-259">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="32d30-260">Exemple de message de log : `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="32d30-260">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="32d30-261">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="32d30-261">Critical = 5</span></span>

  <span data-ttu-id="32d30-262">Fournit des informations sur des échecs qui nécessitent un examen immédiat.</span><span class="sxs-lookup"><span data-stu-id="32d30-262">For failures that require immediate attention.</span></span> <span data-ttu-id="32d30-263">Exemples : perte de données, espace disque insuffisant.</span><span class="sxs-lookup"><span data-stu-id="32d30-263">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="32d30-264">Le niveau de journalisation permet de contrôler le volume de la sortie de journal écrite sur un support de stockage ou dans une fenêtre d’affichage.</span><span class="sxs-lookup"><span data-stu-id="32d30-264">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="32d30-265">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="32d30-265">For example:</span></span>

* <span data-ttu-id="32d30-266">En production :</span><span class="sxs-lookup"><span data-stu-id="32d30-266">In production:</span></span>
  * <span data-ttu-id="32d30-267">La journalisation au `Trace` via des niveaux de `Information` génère un volume important de messages de journal détaillés.</span><span class="sxs-lookup"><span data-stu-id="32d30-267">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="32d30-268">Pour contrôler les coûts et ne pas dépasser les limites de stockage des données, consignez les `Trace` des messages de niveau `Information` dans un magasin de données à faible coût.</span><span class="sxs-lookup"><span data-stu-id="32d30-268">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="32d30-269">La journalisation au `Warning` via des niveaux de `Critical` produit généralement moins de messages journaux plus petits.</span><span class="sxs-lookup"><span data-stu-id="32d30-269">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="32d30-270">Par conséquent, les coûts et les limites de stockage ne sont généralement pas un problème, ce qui se traduit par une plus grande flexibilité du choix des magasins de données.</span><span class="sxs-lookup"><span data-stu-id="32d30-270">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="32d30-271">Lors du développement :</span><span class="sxs-lookup"><span data-stu-id="32d30-271">During development:</span></span>
  * <span data-ttu-id="32d30-272">Consignez les `Warning` via des messages `Critical` sur la console.</span><span class="sxs-lookup"><span data-stu-id="32d30-272">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="32d30-273">Ajoutez `Trace` par le biais de messages de `Information` lors de la résolution des problèmes.</span><span class="sxs-lookup"><span data-stu-id="32d30-273">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="32d30-274">La section [Filtrage de log](#log-filtering) plus loin dans cet article explique comment déterminer les niveaux de journalisation gérés par un fournisseur.</span><span class="sxs-lookup"><span data-stu-id="32d30-274">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="32d30-275">ASP.NET Core écrit des journaux pour les événements de framework.</span><span class="sxs-lookup"><span data-stu-id="32d30-275">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="32d30-276">Aucun journal du niveau `Information` ou `Debug` n’était créé dans les exemples de journaux présentés plus haut dans cet article, puisque les journaux au-dessous du niveau `Trace` étaient exclus.</span><span class="sxs-lookup"><span data-stu-id="32d30-276">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="32d30-277">Voici un exemple de journaux Console produits par l’exemple d’application configurée pour afficher les journaux `Debug` :</span><span class="sxs-lookup"><span data-stu-id="32d30-277">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of authorization filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of resource filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of action filters (in the following order): Microsoft.AspNetCore.Mvc.Filters.ControllerActionFilter (Order: -2147483648), Microsoft.AspNetCore.Mvc.ModelBinding.UnsupportedContentTypeFilter (Order: -3000)
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of exception filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of result filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[22]
      Attempting to bind parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[44]
      Attempting to bind parameter 'id' of type 'System.String' using the name 'id' in request data ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[45]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[23]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[26]
      Attempting to validate the bound parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[27]
      Done attempting to validate the bound parameter 'id' of type 'System.String'.
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[2]
      Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 32.690400000000004ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 176.9103ms 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

## <a name="log-event-id"></a><span data-ttu-id="32d30-278">ID d’événement de log</span><span class="sxs-lookup"><span data-stu-id="32d30-278">Log event ID</span></span>

<span data-ttu-id="32d30-279">Chaque journal peut spécifier un *ID d’événement*.</span><span class="sxs-lookup"><span data-stu-id="32d30-279">Each log can specify an *event ID*.</span></span> <span data-ttu-id="32d30-280">Pour cela, l’exemple d’application utilise une classe `LoggingEvents` définie localement :</span><span class="sxs-lookup"><span data-stu-id="32d30-280">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="32d30-281">Un ID d’événement associe un jeu d’événements.</span><span class="sxs-lookup"><span data-stu-id="32d30-281">An event ID associates a set of events.</span></span> <span data-ttu-id="32d30-282">Par exemple, tous les journaux liés à l’affichage d’une liste d’éléments sur une page peuvent être 1001.</span><span class="sxs-lookup"><span data-stu-id="32d30-282">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="32d30-283">Le fournisseur de journalisation peut stocker l’ID d’événement dans un champ ID, dans le message de journalisation, ou pas du tout.</span><span class="sxs-lookup"><span data-stu-id="32d30-283">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="32d30-284">Le fournisseur Debug n’affiche pas les ID d’événements.</span><span class="sxs-lookup"><span data-stu-id="32d30-284">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="32d30-285">Le fournisseur Console affiche les ID d’événements entre crochets après la catégorie :</span><span class="sxs-lookup"><span data-stu-id="32d30-285">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="32d30-286">Modèle de message de log</span><span class="sxs-lookup"><span data-stu-id="32d30-286">Log message template</span></span>

<span data-ttu-id="32d30-287">Chaque journal spécifie un modèle de message.</span><span class="sxs-lookup"><span data-stu-id="32d30-287">Each log specifies a message template.</span></span> <span data-ttu-id="32d30-288">Ce dernier peut contenir des espaces réservés pour lesquels les arguments sont fournis.</span><span class="sxs-lookup"><span data-stu-id="32d30-288">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="32d30-289">Utilisez des noms et non des nombres pour les espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="32d30-289">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="32d30-290">L’ordre des espaces réservés, pas leurs noms, détermine quels paramètres sont utilisés pour fournir leurs valeurs.</span><span class="sxs-lookup"><span data-stu-id="32d30-290">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="32d30-291">Dans le code suivant, on voit que les noms de paramètres sont hors séquence dans le modèle de message :</span><span class="sxs-lookup"><span data-stu-id="32d30-291">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="32d30-292">Ce code crée un message de journal avec les valeurs des paramètres dans la séquence :</span><span class="sxs-lookup"><span data-stu-id="32d30-292">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="32d30-293">Le framework de journalisation fonctionne ainsi pour permettre aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="32d30-293">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="32d30-294">Les arguments proprement dits, et pas seulement le modèle de message mis en forme, sont transmis au système de journalisation.</span><span class="sxs-lookup"><span data-stu-id="32d30-294">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="32d30-295">C’est grâce à ces informations que les fournisseurs de journalisation peuvent stocker les valeurs des paramètres sous forme de champs.</span><span class="sxs-lookup"><span data-stu-id="32d30-295">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="32d30-296">Supposons par exemple que les appels de méthodes d’enregistreur d’événements se présentent ainsi :</span><span class="sxs-lookup"><span data-stu-id="32d30-296">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="32d30-297">Si vous envoyez les journaux au Stockage Table Azure, chaque entité Table Azure peut avoir les propriétés `ID` et `RequestTime`, ce qui simplifie les requêtes sur les données de journaux.</span><span class="sxs-lookup"><span data-stu-id="32d30-297">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="32d30-298">Une requête peut rechercher tous les journaux compris dans une plage `RequestTime` spécifique sans analyser le délai d’expiration du message texte.</span><span class="sxs-lookup"><span data-stu-id="32d30-298">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="32d30-299">Journalisation des exceptions</span><span class="sxs-lookup"><span data-stu-id="32d30-299">Logging exceptions</span></span>

<span data-ttu-id="32d30-300">Les méthodes logger ont des surcharges qui vous permettent de passer une exception, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="32d30-300">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="32d30-301">Tous les fournisseurs ne gèrent pas les informations sur les exceptions de la même façon.</span><span class="sxs-lookup"><span data-stu-id="32d30-301">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="32d30-302">Voici un exemple de sortie du fournisseur Debug extrait du code montré plus haut.</span><span class="sxs-lookup"><span data-stu-id="32d30-302">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="32d30-303">Filtrage de journal</span><span class="sxs-lookup"><span data-stu-id="32d30-303">Log filtering</span></span>

<span data-ttu-id="32d30-304">Vous pouvez spécifier un niveau de journalisation minimum pour une catégorie ou un fournisseur spécifique, ou pour l’ensemble des fournisseurs ou des catégories.</span><span class="sxs-lookup"><span data-stu-id="32d30-304">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="32d30-305">Les enregistrements de log en dessous du niveau minimum ne sont pas passés à ce fournisseur, et ne sont donc pas affichés ou stockés.</span><span class="sxs-lookup"><span data-stu-id="32d30-305">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="32d30-306">Pour supprimer tous les journaux, choisissez `LogLevel.None` comme niveau de journalisation minimal.</span><span class="sxs-lookup"><span data-stu-id="32d30-306">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="32d30-307">La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="32d30-307">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="32d30-308">Créer des règles de filtre dans la configuration</span><span class="sxs-lookup"><span data-stu-id="32d30-308">Create filter rules in configuration</span></span>

<span data-ttu-id="32d30-309">Le code du modèle de projet appelle `CreateDefaultBuilder` pour configurer la journalisation des fournisseurs de console, de débogage et EventSource (ASP.NET Core 2,2 ou version ultérieure).</span><span class="sxs-lookup"><span data-stu-id="32d30-309">The project template code calls `CreateDefaultBuilder` to set up logging for the Console, Debug, and EventSource (ASP.NET Core 2.2 or later) providers.</span></span> <span data-ttu-id="32d30-310">La méthode `CreateDefaultBuilder` définit également la journalisation pour rechercher la configuration dans une section `Logging`, conformément à ce qui a été expliqué [plus haut dans cet article](#configuration).</span><span class="sxs-lookup"><span data-stu-id="32d30-310">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="32d30-311">Les données de configuration spécifient des niveaux de journalisation minimum par fournisseur et par catégorie, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="32d30-311">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

::: moniker-end

<span data-ttu-id="32d30-312">Ce code JSON crée six règles de filtre : une pour le fournisseur Debug, quatre pour le fournisseur Console et une pour tous les fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="32d30-312">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="32d30-313">Une seule règle est choisie pour chaque fournisseur à la création d’un objet `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="32d30-313">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="32d30-314">Règles de filtre dans le code</span><span class="sxs-lookup"><span data-stu-id="32d30-314">Filter rules in code</span></span>

<span data-ttu-id="32d30-315">L'exemple suivant montre comment enregistrer des règles de filtre dans le code :</span><span class="sxs-lookup"><span data-stu-id="32d30-315">The following example shows how to register filter rules in code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=2-3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

<span data-ttu-id="32d30-316">Le second `AddFilter` spécifie le fournisseur Debug par son nom de type.</span><span class="sxs-lookup"><span data-stu-id="32d30-316">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="32d30-317">Le premier `AddFilter` s’applique à tous les fournisseurs, car il ne spécifie aucun type de fournisseur.</span><span class="sxs-lookup"><span data-stu-id="32d30-317">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="32d30-318">Mode d’application des règles de filtre</span><span class="sxs-lookup"><span data-stu-id="32d30-318">How filtering rules are applied</span></span>

<span data-ttu-id="32d30-319">Les données de configuration et le code `AddFilter` contenus dans les exemples précédents créent les règles présentées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="32d30-319">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="32d30-320">Les six premières proviennent de l’exemple de configuration et les deux dernières, de l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="32d30-320">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="32d30-321">Number</span><span class="sxs-lookup"><span data-stu-id="32d30-321">Number</span></span> | <span data-ttu-id="32d30-322">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="32d30-322">Provider</span></span>      | <span data-ttu-id="32d30-323">Catégories commençant par...</span><span class="sxs-lookup"><span data-stu-id="32d30-323">Categories that begin with ...</span></span>          | <span data-ttu-id="32d30-324">Niveau de journalisation minimum</span><span class="sxs-lookup"><span data-stu-id="32d30-324">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="32d30-325">1</span><span class="sxs-lookup"><span data-stu-id="32d30-325">1</span></span>      | <span data-ttu-id="32d30-326">Débogage</span><span class="sxs-lookup"><span data-stu-id="32d30-326">Debug</span></span>         | <span data-ttu-id="32d30-327">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="32d30-327">All categories</span></span>                          | <span data-ttu-id="32d30-328">Information</span><span class="sxs-lookup"><span data-stu-id="32d30-328">Information</span></span>       |
| <span data-ttu-id="32d30-329">2</span><span class="sxs-lookup"><span data-stu-id="32d30-329">2</span></span>      | <span data-ttu-id="32d30-330">Console</span><span class="sxs-lookup"><span data-stu-id="32d30-330">Console</span></span>       | <span data-ttu-id="32d30-331">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="32d30-331">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="32d30-332">Avertissement</span><span class="sxs-lookup"><span data-stu-id="32d30-332">Warning</span></span>           |
| <span data-ttu-id="32d30-333">3</span><span class="sxs-lookup"><span data-stu-id="32d30-333">3</span></span>      | <span data-ttu-id="32d30-334">Console</span><span class="sxs-lookup"><span data-stu-id="32d30-334">Console</span></span>       | <span data-ttu-id="32d30-335">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="32d30-335">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="32d30-336">Débogage</span><span class="sxs-lookup"><span data-stu-id="32d30-336">Debug</span></span>             |
| <span data-ttu-id="32d30-337">4</span><span class="sxs-lookup"><span data-stu-id="32d30-337">4</span></span>      | <span data-ttu-id="32d30-338">Console</span><span class="sxs-lookup"><span data-stu-id="32d30-338">Console</span></span>       | <span data-ttu-id="32d30-339">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="32d30-339">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="32d30-340">Error</span><span class="sxs-lookup"><span data-stu-id="32d30-340">Error</span></span>             |
| <span data-ttu-id="32d30-341">5</span><span class="sxs-lookup"><span data-stu-id="32d30-341">5</span></span>      | <span data-ttu-id="32d30-342">Console</span><span class="sxs-lookup"><span data-stu-id="32d30-342">Console</span></span>       | <span data-ttu-id="32d30-343">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="32d30-343">All categories</span></span>                          | <span data-ttu-id="32d30-344">Information</span><span class="sxs-lookup"><span data-stu-id="32d30-344">Information</span></span>       |
| <span data-ttu-id="32d30-345">6</span><span class="sxs-lookup"><span data-stu-id="32d30-345">6</span></span>      | <span data-ttu-id="32d30-346">Tous les fournisseurs</span><span class="sxs-lookup"><span data-stu-id="32d30-346">All providers</span></span> | <span data-ttu-id="32d30-347">Toutes les catégories</span><span class="sxs-lookup"><span data-stu-id="32d30-347">All categories</span></span>                          | <span data-ttu-id="32d30-348">Débogage</span><span class="sxs-lookup"><span data-stu-id="32d30-348">Debug</span></span>             |
| <span data-ttu-id="32d30-349">7</span><span class="sxs-lookup"><span data-stu-id="32d30-349">7</span></span>      | <span data-ttu-id="32d30-350">Tous les fournisseurs</span><span class="sxs-lookup"><span data-stu-id="32d30-350">All providers</span></span> | <span data-ttu-id="32d30-351">Système</span><span class="sxs-lookup"><span data-stu-id="32d30-351">System</span></span>                                  | <span data-ttu-id="32d30-352">Débogage</span><span class="sxs-lookup"><span data-stu-id="32d30-352">Debug</span></span>             |
| <span data-ttu-id="32d30-353">8</span><span class="sxs-lookup"><span data-stu-id="32d30-353">8</span></span>      | <span data-ttu-id="32d30-354">Débogage</span><span class="sxs-lookup"><span data-stu-id="32d30-354">Debug</span></span>         | <span data-ttu-id="32d30-355">Microsoft</span><span class="sxs-lookup"><span data-stu-id="32d30-355">Microsoft</span></span>                               | <span data-ttu-id="32d30-356">Trace</span><span class="sxs-lookup"><span data-stu-id="32d30-356">Trace</span></span>             |

<span data-ttu-id="32d30-357">À la création d’un objet `ILogger`, l’objet `ILoggerFactory` sélectionne une seule règle à appliquer à cet enregistrement d’événements par fournisseur.</span><span class="sxs-lookup"><span data-stu-id="32d30-357">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="32d30-358">Tous les messages écrits par une instance `ILogger` sont filtrés selon les règles sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="32d30-358">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="32d30-359">La règle la plus spécifique pouvant être appliquée à chaque paire catégorie/fournisseur est sélectionnée parmi les règles disponibles.</span><span class="sxs-lookup"><span data-stu-id="32d30-359">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="32d30-360">L’algorithme suivant est utilisé pour chaque fournisseur quand un objet `ILogger` est créé pour une catégorie donnée :</span><span class="sxs-lookup"><span data-stu-id="32d30-360">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="32d30-361">Sélectionnez toutes les règles qui correspondent au fournisseur ou à son alias.</span><span class="sxs-lookup"><span data-stu-id="32d30-361">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="32d30-362">Si aucune correspondance n’est trouvée, sélectionnez toutes les règles avec un fournisseur vide.</span><span class="sxs-lookup"><span data-stu-id="32d30-362">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="32d30-363">À partir du résultat de l’étape précédente, sélectionnez les règles ayant le plus long préfixe de catégorie correspondant.</span><span class="sxs-lookup"><span data-stu-id="32d30-363">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="32d30-364">Si aucune correspondance n’est trouvée, sélectionnez toutes les règles qui ne spécifient pas de catégorie.</span><span class="sxs-lookup"><span data-stu-id="32d30-364">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="32d30-365">Si plusieurs règles sont sélectionnées, prenez la **dernière**.</span><span class="sxs-lookup"><span data-stu-id="32d30-365">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="32d30-366">Si aucune règle n’est sélectionnée, utilisez `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="32d30-366">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="32d30-367">Avec la liste de règles précédente, supposons que vous créez un objet `ILogger` pour la catégorie « Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine » :</span><span class="sxs-lookup"><span data-stu-id="32d30-367">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="32d30-368">Les règles 1, 6 et 8 s’appliquent au fournisseur Debug.</span><span class="sxs-lookup"><span data-stu-id="32d30-368">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="32d30-369">La règle 8 est sélectionnée, car c’est la plus spécifique.</span><span class="sxs-lookup"><span data-stu-id="32d30-369">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="32d30-370">Les règles 3, 4, 5 et 6 s’appliquent au fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="32d30-370">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="32d30-371">La règle 3 est la plus spécifique.</span><span class="sxs-lookup"><span data-stu-id="32d30-371">Rule 3 is most specific.</span></span>

<span data-ttu-id="32d30-372">L’instance `ILogger` ainsi produite envoie des journaux de niveau `Trace` ou supérieur au fournisseur Debug.</span><span class="sxs-lookup"><span data-stu-id="32d30-372">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="32d30-373">Les journaux de niveau `Debug` et supérieurs sont envoyés au fournisseur Console.</span><span class="sxs-lookup"><span data-stu-id="32d30-373">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="32d30-374">Alias de fournisseur</span><span class="sxs-lookup"><span data-stu-id="32d30-374">Provider aliases</span></span>

<span data-ttu-id="32d30-375">Chaque fournisseur définit un *alias* qui peut être utilisé dans la configuration à la place du nom de type complet.</span><span class="sxs-lookup"><span data-stu-id="32d30-375">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="32d30-376">Pour les fournisseurs intégrés, utilisez les alias suivants :</span><span class="sxs-lookup"><span data-stu-id="32d30-376">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="32d30-377">Console</span><span class="sxs-lookup"><span data-stu-id="32d30-377">Console</span></span>
* <span data-ttu-id="32d30-378">Débogage</span><span class="sxs-lookup"><span data-stu-id="32d30-378">Debug</span></span>
* <span data-ttu-id="32d30-379">EventSource</span><span class="sxs-lookup"><span data-stu-id="32d30-379">EventSource</span></span>
* <span data-ttu-id="32d30-380">EventLog</span><span class="sxs-lookup"><span data-stu-id="32d30-380">EventLog</span></span>
* <span data-ttu-id="32d30-381">TraceSource</span><span class="sxs-lookup"><span data-stu-id="32d30-381">TraceSource</span></span>
* <span data-ttu-id="32d30-382">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="32d30-382">AzureAppServicesFile</span></span>
* <span data-ttu-id="32d30-383">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="32d30-383">AzureAppServicesBlob</span></span>
* <span data-ttu-id="32d30-384">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="32d30-384">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="32d30-385">Niveau minimum par défaut</span><span class="sxs-lookup"><span data-stu-id="32d30-385">Default minimum level</span></span>

<span data-ttu-id="32d30-386">Un paramètre de niveau minimum est utilisé uniquement si aucune règle de la configuration ou du code ne s’applique à une catégorie ou un fournisseur spécifique.</span><span class="sxs-lookup"><span data-stu-id="32d30-386">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="32d30-387">L’exemple suivant montre comment définir le niveau minimum :</span><span class="sxs-lookup"><span data-stu-id="32d30-387">The following example shows how to set the minimum level:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

<span data-ttu-id="32d30-388">Si vous ne définissez pas explicitement le niveau minimum, la valeur par défaut est `Information`, ce qui signifie que les niveaux `Trace` et `Debug` sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="32d30-388">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="32d30-389">Fonctions de filtrage</span><span class="sxs-lookup"><span data-stu-id="32d30-389">Filter functions</span></span>

<span data-ttu-id="32d30-390">Une fonction de filtre est appelée pour tous les fournisseurs et toutes les catégories pour lesquels la configuration ou le code n’applique aucune règle.</span><span class="sxs-lookup"><span data-stu-id="32d30-390">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="32d30-391">Le code de la fonction a accès au type de fournisseur, à la catégorie et au niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="32d30-391">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="32d30-392">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="32d30-392">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="32d30-393">Niveaux et les catégories de système</span><span class="sxs-lookup"><span data-stu-id="32d30-393">System categories and levels</span></span>

<span data-ttu-id="32d30-394">Voici quelques catégories utilisées par ASP.NET Core et Entity Framework Core, avec des notes sur les journaux associés :</span><span class="sxs-lookup"><span data-stu-id="32d30-394">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="32d30-395">Category</span><span class="sxs-lookup"><span data-stu-id="32d30-395">Category</span></span>                            | <span data-ttu-id="32d30-396">Notes</span><span class="sxs-lookup"><span data-stu-id="32d30-396">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="32d30-397">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="32d30-397">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="32d30-398">Diagnostics ASP.NET Core généraux.</span><span class="sxs-lookup"><span data-stu-id="32d30-398">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="32d30-399">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="32d30-399">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="32d30-400">Liste des clés considérées, trouvées et utilisées.</span><span class="sxs-lookup"><span data-stu-id="32d30-400">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="32d30-401">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="32d30-401">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="32d30-402">Hôtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="32d30-402">Hosts allowed.</span></span> |
| <span data-ttu-id="32d30-403">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="32d30-403">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="32d30-404">Temps de traitement des requêtes HTTP et heure de démarrage.</span><span class="sxs-lookup"><span data-stu-id="32d30-404">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="32d30-405">Liste des assemblys de démarrage d’hébergement chargés.</span><span class="sxs-lookup"><span data-stu-id="32d30-405">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="32d30-406">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="32d30-406">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="32d30-407">Diagnostics MVC et Razor.</span><span class="sxs-lookup"><span data-stu-id="32d30-407">MVC and Razor diagnostics.</span></span> <span data-ttu-id="32d30-408">Liaison de données, exécution de filtres, compilation de vues, sélection d’actions.</span><span class="sxs-lookup"><span data-stu-id="32d30-408">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="32d30-409">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="32d30-409">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="32d30-410">Informations de correspondance des itinéraires.</span><span class="sxs-lookup"><span data-stu-id="32d30-410">Route matching information.</span></span> |
| <span data-ttu-id="32d30-411">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="32d30-411">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="32d30-412">Démarrage et arrêt de la connexion, et réponses persistantes.</span><span class="sxs-lookup"><span data-stu-id="32d30-412">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="32d30-413">Informations du certificat HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32d30-413">HTTPS certificate information.</span></span> |
| <span data-ttu-id="32d30-414">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="32d30-414">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="32d30-415">Fichiers pris en charge.</span><span class="sxs-lookup"><span data-stu-id="32d30-415">Files served.</span></span> |
| <span data-ttu-id="32d30-416">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="32d30-416">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="32d30-417">Diagnostics Entity Framework Core généraux.</span><span class="sxs-lookup"><span data-stu-id="32d30-417">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="32d30-418">Activité et configuration de la base de données, détection des modifications, migrations.</span><span class="sxs-lookup"><span data-stu-id="32d30-418">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="32d30-419">Étendues de log</span><span class="sxs-lookup"><span data-stu-id="32d30-419">Log scopes</span></span>

 <span data-ttu-id="32d30-420">Une *étendue* peut regrouper un ensemble d’opérations logiques.</span><span class="sxs-lookup"><span data-stu-id="32d30-420">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="32d30-421">Ce regroupement permet de joindre les mêmes données à tous les journaux créés au sein d’un ensemble.</span><span class="sxs-lookup"><span data-stu-id="32d30-421">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="32d30-422">Par exemple, chaque journal créé dans le cadre du traitement d’une transaction peut contenir l’ID de la transaction.</span><span class="sxs-lookup"><span data-stu-id="32d30-422">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="32d30-423">Une étendue est un type `IDisposable` qui est retourné par la méthode <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*>. Elle s’applique tant qu’elle n’est pas supprimée.</span><span class="sxs-lookup"><span data-stu-id="32d30-423">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="32d30-424">Utilisez une étendue en incluant les appels de l’enregistrement d’événements dans un bloc `using` :</span><span class="sxs-lookup"><span data-stu-id="32d30-424">Use a scope by wrapping logger calls in a `using` block:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

<span data-ttu-id="32d30-425">Le code suivant active les étendues pour le fournisseur Console :</span><span class="sxs-lookup"><span data-stu-id="32d30-425">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="32d30-426">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="32d30-426">*Program.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="32d30-427">La configuration de l’option logger `IncludeScopes` pour la console est nécessaire pour activer la journalisation basée sur des étendues.</span><span class="sxs-lookup"><span data-stu-id="32d30-427">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="32d30-428">Pour plus d'informations sur la configuration, reportez-vous à la section [Configuration](#configuration).</span><span class="sxs-lookup"><span data-stu-id="32d30-428">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="32d30-429">Chaque message de log fournit les informations incluses dans l’étendue :</span><span class="sxs-lookup"><span data-stu-id="32d30-429">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="32d30-430">Fournisseurs de journalisation intégrés</span><span class="sxs-lookup"><span data-stu-id="32d30-430">Built-in logging providers</span></span>

<span data-ttu-id="32d30-431">ASP.NET Core contient les fournisseurs suivants :</span><span class="sxs-lookup"><span data-stu-id="32d30-431">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="32d30-432">Console</span><span class="sxs-lookup"><span data-stu-id="32d30-432">Console</span></span>](#console-provider)
* [<span data-ttu-id="32d30-433">Déboguer</span><span class="sxs-lookup"><span data-stu-id="32d30-433">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="32d30-434">EventSource</span><span class="sxs-lookup"><span data-stu-id="32d30-434">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="32d30-435">EventLog</span><span class="sxs-lookup"><span data-stu-id="32d30-435">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="32d30-436">TraceSource</span><span class="sxs-lookup"><span data-stu-id="32d30-436">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="32d30-437">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="32d30-437">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="32d30-438">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="32d30-438">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="32d30-439">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="32d30-439">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="32d30-440">Pour plus d’informations sur stdout et sur la journalisation du débogage avec le module ASP.NET Core, consultez <xref:test/troubleshoot-azure-iis> et <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="32d30-440">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="32d30-441">Fournisseur Console</span><span class="sxs-lookup"><span data-stu-id="32d30-441">Console provider</span></span>

<span data-ttu-id="32d30-442">Le package de fournisseur [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envoie la sortie de log dans la console.</span><span class="sxs-lookup"><span data-stu-id="32d30-442">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="32d30-443">Pour voir la sortie de la journalisation Console, ouvrez une invite de commandes dans le dossier du projet et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="32d30-443">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="32d30-444">Fournisseur Debug</span><span class="sxs-lookup"><span data-stu-id="32d30-444">Debug provider</span></span>

<span data-ttu-id="32d30-445">Le package de fournisseur [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) écrit la sortie de log à l’aide de la classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (appels de la méthode `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="32d30-445">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="32d30-446">Sur Linux, ce fournisseur écrit la sortie de log dans */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="32d30-446">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="32d30-447">Fournisseur de source d’événements</span><span class="sxs-lookup"><span data-stu-id="32d30-447">Event Source provider</span></span>

<span data-ttu-id="32d30-448">Le package du fournisseur [Microsoft. extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) écrit dans une source d’événement multiplateforme portant le nom `Microsoft-Extensions-Logging`.</span><span class="sxs-lookup"><span data-stu-id="32d30-448">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="32d30-449">Sur Windows, le fournisseur utilise [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="32d30-449">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="32d30-450">Le fournisseur de la source d’événements est ajouté automatiquement lorsque `CreateDefaultBuilder` est appelé pour générer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="32d30-450">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

::: moniker range=">= aspnetcore-3.0"

#### <a name="dotnet-trace-tooling"></a><span data-ttu-id="32d30-451">outils de trace dotnet</span><span class="sxs-lookup"><span data-stu-id="32d30-451">dotnet trace tooling</span></span>

<span data-ttu-id="32d30-452">L’outil [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) est un outil global de l’interface CLI multiplateforme qui permet la collecte des traces .net core d’un processus en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="32d30-452">The [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) tool is a cross-platform CLI global tool that enables the collection of .NET Core traces of a running process.</span></span> <span data-ttu-id="32d30-453">L’outil collecte <xref:Microsoft.Extensions.Logging.EventSource> données du fournisseur à l’aide d’un <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span><span class="sxs-lookup"><span data-stu-id="32d30-453">The tool collects <xref:Microsoft.Extensions.Logging.EventSource> provider data using a <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span></span>

<span data-ttu-id="32d30-454">Installez les outils de trace dotnet avec la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="32d30-454">Install the dotnet trace tooling with the following command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-trace
```

<span data-ttu-id="32d30-455">Utilisez les outils de trace dotnet pour collecter une trace à partir d’une application :</span><span class="sxs-lookup"><span data-stu-id="32d30-455">Use the dotnet trace tooling to collect a trace from an app:</span></span>

1. <span data-ttu-id="32d30-456">Si l’application ne crée pas l’hôte avec `CreateDefaultBuilder`, ajoutez le fournisseur de la [source d’événements](#event-source-provider) à la configuration de journalisation de l’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-456">If the app doesn't build the host with `CreateDefaultBuilder`, add the [Event Source provider](#event-source-provider) to the app's logging configuration.</span></span>

1. <span data-ttu-id="32d30-457">Exécutez l’application avec la commande `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="32d30-457">Run the app with the `dotnet run` command.</span></span>

1. <span data-ttu-id="32d30-458">Déterminez l’identificateur de processus (PID) de l’application .NET Core :</span><span class="sxs-lookup"><span data-stu-id="32d30-458">Determine the process identifier (PID) of the .NET Core app:</span></span>

   * <span data-ttu-id="32d30-459">Sur Windows, utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="32d30-459">On Windows, use one of the following approaches:</span></span>
     * <span data-ttu-id="32d30-460">Gestionnaire des tâches (Ctrl + Alt + Suppr)</span><span class="sxs-lookup"><span data-stu-id="32d30-460">Task Manager (Ctrl+Alt+Del)</span></span>
     * [<span data-ttu-id="32d30-461">TaskList, commande</span><span class="sxs-lookup"><span data-stu-id="32d30-461">tasklist command</span></span>](/windows-server/administration/windows-commands/tasklist)
     * [<span data-ttu-id="32d30-462">Commande PowerShell d’extraction de processus</span><span class="sxs-lookup"><span data-stu-id="32d30-462">Get-Process Powershell command</span></span>](/powershell/module/microsoft.powershell.management/get-process)
   * <span data-ttu-id="32d30-463">Sur Linux, utilisez la [commande pidof](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span><span class="sxs-lookup"><span data-stu-id="32d30-463">On Linux, use the [pidof command](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span></span>

   <span data-ttu-id="32d30-464">Recherchez le PID pour le processus qui porte le même nom que l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-464">Find the PID for the process that has the same name as the app's assembly.</span></span>

1. <span data-ttu-id="32d30-465">Exécutez la commande `dotnet trace`.</span><span class="sxs-lookup"><span data-stu-id="32d30-465">Execute the `dotnet trace` command.</span></span>

   <span data-ttu-id="32d30-466">Syntaxe de la commande générale :</span><span class="sxs-lookup"><span data-stu-id="32d30-466">General command syntax:</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   <span data-ttu-id="32d30-467">Quand vous utilisez une interface de commande PowerShell, mettez la `--providers` valeur entre guillemets simples (`'`) :</span><span class="sxs-lookup"><span data-stu-id="32d30-467">When using a PowerShell command shell, enclose the `--providers` value in single quotes (`'`):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   <span data-ttu-id="32d30-468">Sur les plateformes non-Windows, ajoutez l’option `-f speedscope` pour modifier le format du fichier de trace de sortie en `speedscope`.</span><span class="sxs-lookup"><span data-stu-id="32d30-468">On non-Windows platforms, add the `-f speedscope` option to change the format of the output trace file to `speedscope`.</span></span>

   | <span data-ttu-id="32d30-469">Mot clé</span><span class="sxs-lookup"><span data-stu-id="32d30-469">Keyword</span></span> | <span data-ttu-id="32d30-470">Description</span><span class="sxs-lookup"><span data-stu-id="32d30-470">Description</span></span> |
   | :-----: | ----------- |
   | <span data-ttu-id="32d30-471">1</span><span class="sxs-lookup"><span data-stu-id="32d30-471">1</span></span>       | <span data-ttu-id="32d30-472">Consigne les événements Meta relatifs au `LoggingEventSource`.</span><span class="sxs-lookup"><span data-stu-id="32d30-472">Log meta events about the `LoggingEventSource`.</span></span> <span data-ttu-id="32d30-473">N’enregistre pas les événements à partir de `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="32d30-473">Doesn't log events from `ILogger`).</span></span> |
   | <span data-ttu-id="32d30-474">2</span><span class="sxs-lookup"><span data-stu-id="32d30-474">2</span></span>       | <span data-ttu-id="32d30-475">Active l’événement `Message` lors de l’appel de `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="32d30-475">Turns on the `Message` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="32d30-476">Fournit des informations d’une façon de programmation (non mise en forme).</span><span class="sxs-lookup"><span data-stu-id="32d30-476">Provides information in a programmatic (not formatted) way.</span></span> |
   | <span data-ttu-id="32d30-477">4</span><span class="sxs-lookup"><span data-stu-id="32d30-477">4</span></span>       | <span data-ttu-id="32d30-478">Active l’événement `FormatMessage` lors de l’appel de `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="32d30-478">Turns on the `FormatMessage` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="32d30-479">Fournit la version de chaîne mise en forme des informations.</span><span class="sxs-lookup"><span data-stu-id="32d30-479">Provides the formatted string version of the information.</span></span> |
   | <span data-ttu-id="32d30-480">8</span><span class="sxs-lookup"><span data-stu-id="32d30-480">8</span></span>       | <span data-ttu-id="32d30-481">Active l’événement `MessageJson` lors de l’appel de `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="32d30-481">Turns on the `MessageJson` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="32d30-482">Fournit une représentation JSON des arguments.</span><span class="sxs-lookup"><span data-stu-id="32d30-482">Provides a JSON representation of the arguments.</span></span> |

   | <span data-ttu-id="32d30-483">Niveau de l'événement</span><span class="sxs-lookup"><span data-stu-id="32d30-483">Event Level</span></span> | <span data-ttu-id="32d30-484">Description</span><span class="sxs-lookup"><span data-stu-id="32d30-484">Description</span></span>     |
   | :---------: | --------------- |
   | <span data-ttu-id="32d30-485">0</span><span class="sxs-lookup"><span data-stu-id="32d30-485">0</span></span>           | `LogAlways`     |
   | <span data-ttu-id="32d30-486">1</span><span class="sxs-lookup"><span data-stu-id="32d30-486">1</span></span>           | `Critical`      |
   | <span data-ttu-id="32d30-487">2</span><span class="sxs-lookup"><span data-stu-id="32d30-487">2</span></span>           | `Error`         |
   | <span data-ttu-id="32d30-488">3</span><span class="sxs-lookup"><span data-stu-id="32d30-488">3</span></span>           | `Warning`       |
   | <span data-ttu-id="32d30-489">4</span><span class="sxs-lookup"><span data-stu-id="32d30-489">4</span></span>           | `Informational` |
   | <span data-ttu-id="32d30-490">5</span><span class="sxs-lookup"><span data-stu-id="32d30-490">5</span></span>           | `Verbose`       |

   <span data-ttu-id="32d30-491">`FilterSpecs` entrées pour `{Logger Category}` et `{Event Level}` représentent des conditions de filtrage de journal supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="32d30-491">`FilterSpecs` entries for `{Logger Category}` and `{Event Level}` represent additional log filtering conditions.</span></span> <span data-ttu-id="32d30-492">Séparez les entrées `FilterSpecs` par un point-virgule (`;`).</span><span class="sxs-lookup"><span data-stu-id="32d30-492">Separate `FilterSpecs` entries with a semicolon (`;`).</span></span>

   <span data-ttu-id="32d30-493">Exemple utilisant un interpréteur de commandes Windows (**sans** guillemets simples autour de la valeur `--providers`) :</span><span class="sxs-lookup"><span data-stu-id="32d30-493">Example using a Windows command shell (**no** single quotes around the `--providers` value):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   <span data-ttu-id="32d30-494">La commande précédente active :</span><span class="sxs-lookup"><span data-stu-id="32d30-494">The preceding command activates:</span></span>

   * <span data-ttu-id="32d30-495">Enregistreur d’événements de la source d’événements pour produire des chaînes mises en forme (`4`) pour les erreurs (`2`).</span><span class="sxs-lookup"><span data-stu-id="32d30-495">The Event Source logger to produce formatted strings (`4`) for errors (`2`).</span></span>
   * <span data-ttu-id="32d30-496">`Microsoft.AspNetCore.Hosting` la journalisation au niveau de journalisation `Informational` (`4`).</span><span class="sxs-lookup"><span data-stu-id="32d30-496">`Microsoft.AspNetCore.Hosting` logging at the `Informational` logging level (`4`).</span></span>

1. <span data-ttu-id="32d30-497">Arrêtez les outils de trace dotnet en appuyant sur la touche entrée ou Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="32d30-497">Stop the dotnet trace tooling by pressing the Enter key or Ctrl+C.</span></span>

   <span data-ttu-id="32d30-498">La trace est enregistrée avec le nom *trace. NetTrace* dans le dossier où la commande `dotnet trace` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="32d30-498">The trace is saved with the name *trace.nettrace* in the folder where the `dotnet trace` command is executed.</span></span>

1. <span data-ttu-id="32d30-499">Ouvrez la trace avec [Perfview](#perfview).</span><span class="sxs-lookup"><span data-stu-id="32d30-499">Open the trace with [Perfview](#perfview).</span></span> <span data-ttu-id="32d30-500">Ouvrez le fichier *trace. NetTrace* et explorez les événements de trace.</span><span class="sxs-lookup"><span data-stu-id="32d30-500">Open the *trace.nettrace* file and explore the trace events.</span></span>

<span data-ttu-id="32d30-501">Pour plus d'informations, consultez les pages suivantes :</span><span class="sxs-lookup"><span data-stu-id="32d30-501">For more information, see:</span></span>

* <span data-ttu-id="32d30-502">[Utilitaire trace for Performance Analysis (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (documentation .net Core)</span><span class="sxs-lookup"><span data-stu-id="32d30-502">[Trace for performance analysis utility (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core documentation)</span></span>
* <span data-ttu-id="32d30-503">[Utilitaire trace for Performance Analysis (dotnet-trace) (documentation sur le](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) référentiel GitHub dotnet/Diagnostics)</span><span class="sxs-lookup"><span data-stu-id="32d30-503">[Trace for performance analysis utility (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (dotnet/diagnostics GitHub repository documentation)</span></span>
* <span data-ttu-id="32d30-504">[LoggingEventSource, classe](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (Explorateur d’API .net)</span><span class="sxs-lookup"><span data-stu-id="32d30-504">[LoggingEventSource Class](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API Browser)</span></span>
* <xref:System.Diagnostics.Tracing.EventLevel>
* <span data-ttu-id="32d30-505">[Source de référence LoggingEventSource (3,0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash; pour obtenir la source de référence pour une version différente, remplacez la branche par `release/{Version}`, où `{Version}` est la version de ASP.net Core souhaitée.</span><span class="sxs-lookup"><span data-stu-id="32d30-505">[LoggingEventSource reference source (3.0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash; To obtain reference source for a different version, change the branch to `release/{Version}`, where `{Version}` is the version of ASP.NET Core desired.</span></span>
* <span data-ttu-id="32d30-506">[Perfview](#perfview) &ndash; utile pour l’affichage des traces de la source d’événements.</span><span class="sxs-lookup"><span data-stu-id="32d30-506">[Perfview](#perfview) &ndash; Useful for viewing Event Source traces.</span></span>

#### <a name="perfview"></a><span data-ttu-id="32d30-507">Perfview</span><span class="sxs-lookup"><span data-stu-id="32d30-507">Perfview</span></span>

::: moniker-end

<span data-ttu-id="32d30-508">Utilisez l' [utilitaire PerfView](https://github.com/Microsoft/perfview) pour collecter et afficher les journaux.</span><span class="sxs-lookup"><span data-stu-id="32d30-508">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="32d30-509">Il existe d’autres outils d’affichage des journaux ETW, mais PerfView est l’outil recommandé pour gérer les événements ETW générés par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d30-509">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="32d30-510">Pour configurer PerfView afin qu’il collecte les événements enregistrés par ce fournisseur, ajoutez la chaîne `*Microsoft-Extensions-Logging` à la liste des **fournisseurs supplémentaires**.</span><span class="sxs-lookup"><span data-stu-id="32d30-510">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="32d30-511">(N’oubliez pas d’inclure l’astérisque au début de la chaîne.)</span><span class="sxs-lookup"><span data-stu-id="32d30-511">(Don't miss the asterisk at the start of the string.)</span></span>

![Fournisseurs supplémentaires dans PerfView](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="32d30-513">Fournisseur EventLog Windows</span><span class="sxs-lookup"><span data-stu-id="32d30-513">Windows EventLog provider</span></span>

<span data-ttu-id="32d30-514">Le package de fournisseur [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envoie la sortie de log dans le journal des événements Windows.</span><span class="sxs-lookup"><span data-stu-id="32d30-514">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="32d30-515">Les [surcharges AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) vous permettent de passer à <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span><span class="sxs-lookup"><span data-stu-id="32d30-515">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="32d30-516">Si `null` ou n’est pas spécifié, les paramètres par défaut suivants sont utilisés :</span><span class="sxs-lookup"><span data-stu-id="32d30-516">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="32d30-517">`LogName` &ndash; « application »</span><span class="sxs-lookup"><span data-stu-id="32d30-517">`LogName` &ndash; "Application"</span></span>
* <span data-ttu-id="32d30-518">`SourceName` &ndash; « Runtime .NET »</span><span class="sxs-lookup"><span data-stu-id="32d30-518">`SourceName` &ndash; ".NET Runtime"</span></span>
* <span data-ttu-id="32d30-519">`MachineName` &ndash; ordinateur local</span><span class="sxs-lookup"><span data-stu-id="32d30-519">`MachineName` &ndash; local machine</span></span>

<span data-ttu-id="32d30-520">Les événements sont consignés pour le [niveau d’avertissement et supérieur](#log-level).</span><span class="sxs-lookup"><span data-stu-id="32d30-520">Events are logged for [Warning level and higher](#log-level).</span></span> <span data-ttu-id="32d30-521">Pour consigner les événements inférieurs à `Warning`, définissez explicitement le niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="32d30-521">To log events lower than `Warning`, explicitly set the log level.</span></span> <span data-ttu-id="32d30-522">Par exemple, ajoutez le code suivant au fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="32d30-522">For example, add the following to the *appsettings.json* file:</span></span>

```json
"EventLog": {
  "LogLevel": {
    "Default": "Information"
  }
}
```

### <a name="tracesource-provider"></a><span data-ttu-id="32d30-523">Fournisseur TraceSource</span><span class="sxs-lookup"><span data-stu-id="32d30-523">TraceSource provider</span></span>

<span data-ttu-id="32d30-524">Le package de fournisseur [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) utilise les bibliothèques et fournisseurs <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="32d30-524">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="32d30-525">Les [surcharges AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) vous permettent de passer un commutateur de source et un écouteur de suivi.</span><span class="sxs-lookup"><span data-stu-id="32d30-525">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="32d30-526">Pour pouvoir utiliser ce fournisseur, il faut que l’application s’exécute sur .NET Framework (au lieu de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="32d30-526">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="32d30-527">Le fournisseur peut acheminer les messages vers différents [détecteurs d’événements](/dotnet/framework/debug-trace-profile/trace-listeners), comme <xref:System.Diagnostics.TextWriterTraceListener> (utilisé dans l’exemple d’application).</span><span class="sxs-lookup"><span data-stu-id="32d30-527">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="32d30-528">Fournisseur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="32d30-528">Azure App Service provider</span></span>

<span data-ttu-id="32d30-529">Le package de fournisseur [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) écrit les enregistrements de log dans des fichiers texte dans le système de fichiers d’une application Azure App Service, et dans un [stockage Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) dans un compte de stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="32d30-529">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32d30-530">Le fournisseur de package n’est pas inclus dans le framework partagé.</span><span class="sxs-lookup"><span data-stu-id="32d30-530">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="32d30-531">Pour utiliser le fournisseur, ajoutez le package du fournisseur au projet.</span><span class="sxs-lookup"><span data-stu-id="32d30-531">To use the provider, add the provider package to the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="32d30-532">Le package du fournisseur n’est pas inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="32d30-532">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="32d30-533">Lorsque vous ciblez .NET framework ou référencez le métapackage `Microsoft.AspNetCore.App`, ajoutez le package de fournisseur dans le projet.</span><span class="sxs-lookup"><span data-stu-id="32d30-533">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32d30-534">Pour configurer les paramètres du fournisseur, utilisez <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> et <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="32d30-534">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="32d30-535">Pour configurer les paramètres du fournisseur, utilisez <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> et <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="32d30-535">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="32d30-536">Une surcharge <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> permet de transmettre <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="32d30-536">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="32d30-537">L’objet de paramètres peut remplacer les paramètres par défaut, comme le modèle de sortie de journalisation, le nom d’objet blob et la limite de taille de fichier.</span><span class="sxs-lookup"><span data-stu-id="32d30-537">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="32d30-538">(*Modèle de sortie* est un modèle de message qui s’applique à tous les journaux en plus de ce qui est fourni avec un appel de méthode `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="32d30-538">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

<span data-ttu-id="32d30-539">En cas de déploiement sur une application App Service, celle-ci applique les paramètres définis dans la section [Journaux App Service](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) de la page **App Service** du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="32d30-539">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="32d30-540">Quand les paramètres suivants sont mis à jour, les changements prennent effet immédiatement sans avoir besoin de redémarrer ou redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-540">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="32d30-541">**Journalisation des applications (système de fichiers)**</span><span class="sxs-lookup"><span data-stu-id="32d30-541">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="32d30-542">**Journalisation des applications (objet blob)**</span><span class="sxs-lookup"><span data-stu-id="32d30-542">**Application Logging (Blob)**</span></span>

<span data-ttu-id="32d30-543">L’emplacement par défaut des fichiers journaux est le dossier *D:\\racine\\LogFiles\\Application*, et le nom de fichier par défaut est *diagnostics-aaaammjj.txt*.</span><span class="sxs-lookup"><span data-stu-id="32d30-543">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="32d30-544">La limite de taille de fichier par défaut est de 10 Mo, et le nombre maximal de fichiers conservés par défaut est de 2.</span><span class="sxs-lookup"><span data-stu-id="32d30-544">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="32d30-545">Le nom par défaut du blob est *{app-name}{timestamp}/aaaa/mm/jj/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="32d30-545">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="32d30-546">Le fournisseur fonctionne uniquement quand le projet s’exécute dans l’environnement Azure.</span><span class="sxs-lookup"><span data-stu-id="32d30-546">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="32d30-547">Il n’a aucun effet si le projet s’exécute localement &mdash; il n’écrit pas d’enregistrements dans les fichiers locaux ou dans le stockage de développement local pour les objets blob.</span><span class="sxs-lookup"><span data-stu-id="32d30-547">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="32d30-548">Streaming des journaux Azure</span><span class="sxs-lookup"><span data-stu-id="32d30-548">Azure log streaming</span></span>

<span data-ttu-id="32d30-549">La diffusion en continu des journaux Azure permet d’afficher l’activité de journalisation en temps réel aux emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="32d30-549">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="32d30-550">Serveur d'applications</span><span class="sxs-lookup"><span data-stu-id="32d30-550">The app server</span></span>
* <span data-ttu-id="32d30-551">Serveur web</span><span class="sxs-lookup"><span data-stu-id="32d30-551">The web server</span></span>
* <span data-ttu-id="32d30-552">Suivi des demandes ayant échoué</span><span class="sxs-lookup"><span data-stu-id="32d30-552">Failed request tracing</span></span>

<span data-ttu-id="32d30-553">Pour configurer le streaming des journaux Azure :</span><span class="sxs-lookup"><span data-stu-id="32d30-553">To configure Azure log streaming:</span></span>

* <span data-ttu-id="32d30-554">Accédez à la page **Journaux App Service** dans le portail de votre application.</span><span class="sxs-lookup"><span data-stu-id="32d30-554">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="32d30-555">Définissez **Journal des applications (Système de fichiers)** sur **Activé**.</span><span class="sxs-lookup"><span data-stu-id="32d30-555">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="32d30-556">Choisissez le **niveau** du journal.</span><span class="sxs-lookup"><span data-stu-id="32d30-556">Choose the log **Level**.</span></span> <span data-ttu-id="32d30-557">Ce paramètre s’applique uniquement à la diffusion en continu de journaux Azure, pas aux autres fournisseurs de journalisation de l’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-557">This setting only applies to Azure log streaming, not other logging providers in the app.</span></span>

<span data-ttu-id="32d30-558">Accédez à la page **Streaming des journaux** pour voir les messages d’application.</span><span class="sxs-lookup"><span data-stu-id="32d30-558">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="32d30-559">Ils sont consignés par application par le biais de l’interface `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="32d30-559">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="32d30-560">Journalisation des traces Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="32d30-560">Azure Application Insights trace logging</span></span>

<span data-ttu-id="32d30-561">Le package de fournisseur [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) écrit des journaux dans Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="32d30-561">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="32d30-562">Application Insights est un service qui surveille une application web et fournit des outils pour interroger et analyser les données de télémétrie.</span><span class="sxs-lookup"><span data-stu-id="32d30-562">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="32d30-563">Si vous utilisez ce fournisseur, vous pouvez interroger et analyser vos journaux à l’aide des outils Application Insights.</span><span class="sxs-lookup"><span data-stu-id="32d30-563">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="32d30-564">Le fournisseur de journalisation est inclus en tant que dépendance de [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), le package qui fournit toutes les données de télémétrie disponibles pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d30-564">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="32d30-565">Si vous utilisez ce package, vous n’avez pas besoin d’installer le package du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="32d30-565">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="32d30-566">N’utilisez pas le [package Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;destiné à ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="32d30-566">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="32d30-567">Pour plus d’informations, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="32d30-567">For more information, see the following resources:</span></span>

* [<span data-ttu-id="32d30-568">Vue d'ensemble d'Application Insights</span><span class="sxs-lookup"><span data-stu-id="32d30-568">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="32d30-569">[Application Insights pour les applications ASP.NET Core](/azure/azure-monitor/app/asp-net-core) : commencez ici si vous souhaitez implémenter la gamme complète des données de télémétrie d’Application Insights en même temps que la journalisation.</span><span class="sxs-lookup"><span data-stu-id="32d30-569">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="32d30-570">[Journaux ApplicationInsightsLoggerProvider pour .NET Core ILogger](/azure/azure-monitor/app/ilogger) : commencez ici si vous souhaitez implémenter le fournisseur de journalisation sans le reste des données de télémétrie Application Insights.</span><span class="sxs-lookup"><span data-stu-id="32d30-570">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="32d30-571">[Adaptateurs de journalisation Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="32d30-571">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="32d30-572">[Installer, configurer et initialiser le kit de développement logiciel (SDK) Application Insights](/learn/modules/instrument-web-app-code-with-application-insights) : tutoriel interactif sur le site Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="32d30-572">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="32d30-573">Fournisseurs de journalisation tiers</span><span class="sxs-lookup"><span data-stu-id="32d30-573">Third-party logging providers</span></span>

<span data-ttu-id="32d30-574">Frameworks de journalisation tiers qui sont pris en charge dans ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="32d30-574">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="32d30-575">[elmah.io](https://elmah.io/) ([dépôt GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="32d30-575">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="32d30-576">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([Dépôt GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="32d30-576">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="32d30-577">[JSNLog](https://jsnlog.com/) ([dépôt GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="32d30-577">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="32d30-578">[KissLog.net](https://kisslog.net/) ([référentiel GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="32d30-578">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="32d30-579">[Log4net](https://logging.apache.org/log4net/) ([GitHub référentiel](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="32d30-579">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="32d30-580">[Loggr](https://loggr.net/) ([dépôt GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="32d30-580">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="32d30-581">[NLog](https://nlog-project.org/) ([dépôt GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="32d30-581">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="32d30-582">[Sentry](https://sentry.io/welcome/) ([dépôt GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="32d30-582">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="32d30-583">[Serilog](https://serilog.net/) ([dépôt GitHub](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="32d30-583">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="32d30-584">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="32d30-584">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="32d30-585">Certains frameworks tiers prennent en charge la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="32d30-585">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="32d30-586">L’utilisation d’un framework tiers est semblable à l’utilisation des fournisseurs intégrés :</span><span class="sxs-lookup"><span data-stu-id="32d30-586">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="32d30-587">Ajoutez un package NuGet à votre projet.</span><span class="sxs-lookup"><span data-stu-id="32d30-587">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="32d30-588">Appeler une méthode d’extension `ILoggerFactory` fournie par le Framework de journalisation.</span><span class="sxs-lookup"><span data-stu-id="32d30-588">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="32d30-589">Pour plus d’informations, consultez la documentation de chaque fournisseur.</span><span class="sxs-lookup"><span data-stu-id="32d30-589">For more information, see each provider's documentation.</span></span> <span data-ttu-id="32d30-590">Les fournisseurs de journalisation tiers ne sont pas pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32d30-590">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="32d30-591">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="32d30-591">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
