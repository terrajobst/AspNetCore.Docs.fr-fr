---
title: Démarrage d’une application dans ASP.NET Core
author: rick-anderson
description: Découvrez comment la classe Startup en ASP.NET Core configure les services et le pipeline de requête de l’application.
ms.author: riande
ms.custom: mvc
ms.date: 8/7/2019
uid: fundamentals/startup
ms.openlocfilehash: 9407de4ee91ba43b2c95fa98f0cf479bf8539cab
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310495"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="48f07-103">Démarrage d’une application dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48f07-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="48f07-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="48f07-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="48f07-105">La classe `Startup` configure les services et le pipeline de demande de l’application.</span><span class="sxs-lookup"><span data-stu-id="48f07-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="48f07-106">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="48f07-106">The Startup class</span></span>

<span data-ttu-id="48f07-107">Les applications ASP.NET Core utilisent une classe `Startup`, appelée `Startup` par convention.</span><span class="sxs-lookup"><span data-stu-id="48f07-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="48f07-108">La classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="48f07-108">The `Startup` class:</span></span>

* <span data-ttu-id="48f07-109">Inclut éventuellement une méthode <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> pour configurer les *services* de l’application.</span><span class="sxs-lookup"><span data-stu-id="48f07-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="48f07-110">Un service est un composant réutilisable qui fournit une fonctionnalité d’application.</span><span class="sxs-lookup"><span data-stu-id="48f07-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="48f07-111">Les services sont configurés &mdash;ou *inscrits*&mdash;dans `ConfigureServices` et consommés dans l’application via l’[injection de dépendances](xref:fundamentals/dependency-injection) ou <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span><span class="sxs-lookup"><span data-stu-id="48f07-111">Services are configured&mdash;also described as *registered*&mdash;in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="48f07-112">Inclut une méthode <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> pour créer le pipeline de traitement des requêtes de l’application.</span><span class="sxs-lookup"><span data-stu-id="48f07-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="48f07-113">`ConfigureServices` et `Configure` sont appelés par le runtime ASP.NET Core au lancement de l’application :</span><span class="sxs-lookup"><span data-stu-id="48f07-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="48f07-114">L’exemple précédent concerne [Razor Pages](xref:razor-pages/index), toutefois, la version MVC est similaire.</span><span class="sxs-lookup"><span data-stu-id="48f07-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

<span data-ttu-id="48f07-115">La classe `Startup` est spécifiée quand l’[hôte](xref:fundamentals/index#host) de l’application est créé.</span><span class="sxs-lookup"><span data-stu-id="48f07-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="48f07-116">La classe `Startup` est généralement spécifiée en appelant la méthode [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) dans le générateur d’hôte :</span><span class="sxs-lookup"><span data-stu-id="48f07-116">The `Startup` class is typically specified by calling the [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="48f07-117">L’hôte fournit des services accessibles au constructeur de classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="48f07-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="48f07-118">L’application ajoute des services supplémentaires à l’aide de `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48f07-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="48f07-119">Les services de l’hôte ainsi que ceux de l’application sont disponibles dans `Configure` et dans l’ensemble de l’application.</span><span class="sxs-lookup"><span data-stu-id="48f07-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="48f07-120">Seuls les types de service suivants peuvent être injectés dans le constructeur `Startup` lorsque vous utilisez <xref:Microsoft.Extensions.Hosting.IHostBuilder> :</span><span class="sxs-lookup"><span data-stu-id="48f07-120">Only the following service types can be injected into the `Startup` constructor when using <xref:Microsoft.Extensions.Hosting.IHostBuilder>:</span></span>

* `IWebHostEnvironment`
* `IHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="48f07-121">La plupart des services ne sont pas disponibles tant que la méthode `Configure` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="48f07-121">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="48f07-122">L’hôte fournit des services accessibles au constructeur de classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="48f07-122">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="48f07-123">L’application ajoute des services supplémentaires à l’aide de `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48f07-123">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="48f07-124">Les services de l’hôte ainsi que ceux de l’application sont alors disponibles dans `Configure` et dans l’ensemble de l’application.</span><span class="sxs-lookup"><span data-stu-id="48f07-124">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="48f07-125">Une utilisation courante de [l’injection de dépendances](xref:fundamentals/dependency-injection) dans la classe `Startup` consiste à injecter :</span><span class="sxs-lookup"><span data-stu-id="48f07-125">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="48f07-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> pour configurer les services en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="48f07-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="48f07-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> pour lire la configuration.</span><span class="sxs-lookup"><span data-stu-id="48f07-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="48f07-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> pour créer un enregistreur d’événements dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48f07-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="48f07-129">La plupart des services ne sont pas disponibles tant que la méthode `Configure` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="48f07-129">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

### <a name="multiple-startup"></a><span data-ttu-id="48f07-130">Démarrage multiple</span><span class="sxs-lookup"><span data-stu-id="48f07-130">Multiple StartUp</span></span>

<span data-ttu-id="48f07-131">Quand l’application définit différentes classes `Startup` pour différents environnements (par exemple, `StartupDevelopment`), la classe `Startup` appropriée est sélectionnée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="48f07-131">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="48f07-132">La classe dont le suffixe du nom correspond à l'environnement actuel est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="48f07-132">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="48f07-133">Si l’application est exécutée dans l’environnement de développement et comprend à la fois une classe `Startup` et une classe `StartupDevelopment`, la classe `StartupDevelopment` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="48f07-133">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="48f07-134">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="48f07-134">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="48f07-135">Pour plus d’informations sur l’hôte, consultez [L’hôte](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="48f07-135">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="48f07-136">Pour plus d’informations sur la gestion des erreurs lors du démarrage, consultez [Gestion des exceptions de démarrage](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="48f07-136">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="48f07-137">Méthode ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="48f07-137">The ConfigureServices method</span></span>

<span data-ttu-id="48f07-138">La méthode <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> est :</span><span class="sxs-lookup"><span data-stu-id="48f07-138">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="48f07-139">facultatif.</span><span class="sxs-lookup"><span data-stu-id="48f07-139">Optional.</span></span>
* <span data-ttu-id="48f07-140">Appelée par l’hôte avant la méthode `Configure` pour configurer les services de l’application</span><span class="sxs-lookup"><span data-stu-id="48f07-140">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="48f07-141">L’emplacement où les [options de configuration](xref:fundamentals/configuration/index) sont définies par convention.</span><span class="sxs-lookup"><span data-stu-id="48f07-141">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="48f07-142">L’hôte peut configurer certains services avant l’appel des méthodes `Startup`.</span><span class="sxs-lookup"><span data-stu-id="48f07-142">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="48f07-143">Pour plus d’informations, consultez [L’hôte](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="48f07-143">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="48f07-144">Pour les fonctionnalités qui nécessitent une configuration importante, il existe des méthodes d’extension `Add{Service}` pour <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span><span class="sxs-lookup"><span data-stu-id="48f07-144">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="48f07-145">Par exemple,**Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores et **Add**RazorPages :</span><span class="sxs-lookup"><span data-stu-id="48f07-145">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

<span data-ttu-id="48f07-146">L'ajout de services au conteneur de service les rend disponibles au sein de l’application et dans la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="48f07-146">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="48f07-147">Les services sont résolus via l’[injection de dépendances](xref:fundamentals/dependency-injection) ou à partir de <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span><span class="sxs-lookup"><span data-stu-id="48f07-147">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="48f07-148">Pour plus d’informations sur `SetCompatibilityVersion`, voir [SetCompatibilityVersion](xref:mvc/compatibility-version).</span><span class="sxs-lookup"><span data-stu-id="48f07-148">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

::: moniker-end

## <a name="the-configure-method"></a><span data-ttu-id="48f07-149">Méthode Configure</span><span class="sxs-lookup"><span data-stu-id="48f07-149">The Configure method</span></span>

<span data-ttu-id="48f07-150">La méthode <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> permet de spécifier le mode de réponse de l’application aux requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="48f07-150">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="48f07-151">Vous configurez le pipeline de requête en ajoutant des composants d’[intergiciel (middleware)](xref:fundamentals/middleware/index) à une instance de <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="48f07-151">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="48f07-152">`IApplicationBuilder` est disponible pour la méthode `Configure`, mais elle n’est pas inscrite dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="48f07-152">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="48f07-153">L’hébergement crée un `IApplicationBuilder` et le passe directement à `Configure`.</span><span class="sxs-lookup"><span data-stu-id="48f07-153">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="48f07-154">Les [modèles ASP.NET Core](/dotnet/core/tools/dotnet-new) configurent le pipeline pour permettre la prise en charge des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="48f07-154">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="48f07-155">Page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="48f07-155">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="48f07-156">Gestionnaire d’exceptions</span><span class="sxs-lookup"><span data-stu-id="48f07-156">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="48f07-157">HSTS (HTTP Strict Transport Security)</span><span class="sxs-lookup"><span data-stu-id="48f07-157">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="48f07-158">Redirection HTTPS</span><span class="sxs-lookup"><span data-stu-id="48f07-158">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="48f07-159">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="48f07-159">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="48f07-160">ASP.NET Core [MVC](xref:mvc/overview) et [Razor Pages](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="48f07-160">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

::: moniker range="< aspnetcore-3.0"

* [<span data-ttu-id="48f07-161">RGPD (règlement général sur la protection des données)</span><span class="sxs-lookup"><span data-stu-id="48f07-161">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="48f07-162">L’exemple précédent concerne [Razor Pages](xref:razor-pages/index), toutefois, la version MVC est similaire.</span><span class="sxs-lookup"><span data-stu-id="48f07-162">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

<span data-ttu-id="48f07-163">Chaque méthode d’extension `Use` ajoute un ou plusieurs composants de middleware au pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="48f07-163">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="48f07-164">Par exemple, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configure le [middleware](xref:fundamentals/middleware/index) pour qu’il traite des [fichiers statiques](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="48f07-164">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="48f07-165">Chaque composant de middleware du pipeline des requêtes est chargé d’appeler le composant suivant du pipeline ou de court-circuiter la chaîne, si c’est approprié.</span><span class="sxs-lookup"><span data-stu-id="48f07-165">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="48f07-166">Vous pouvez spécifier des services supplémentaires (comme `IWebHostEnvironment`, `ILoggerFactory` et tout service défini dans `ConfigureServices`) dans la signature de la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="48f07-166">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="48f07-167">Ces services sont injectés s’ils sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="48f07-167">These services are injected if they're available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="48f07-168">Vous pouvez spécifier des services supplémentaires (comme `IHostingEnvironment`, `ILoggerFactory` et tout service défini dans `ConfigureServices`) dans la signature de la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="48f07-168">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="48f07-169">Ces services sont injectés s’ils sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="48f07-169">These services are injected if they're available.</span></span>

::: moniker-end

<span data-ttu-id="48f07-170">Pour plus d’informations sur l’utilisation de `IApplicationBuilder` et sur l’ordre de traitement des middlewares, consultez <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="48f07-170">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="48f07-171">Configurer des services sans démarrage</span><span class="sxs-lookup"><span data-stu-id="48f07-171">Configure services without Startup</span></span>

<span data-ttu-id="48f07-172">Pour configurer les services et le pipeline de traitement de requête sans utiliser de classe `Startup`, appelez les méthodes pratiques `ConfigureServices` et `Configure` sur le générateur de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="48f07-172">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="48f07-173">Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="48f07-173">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="48f07-174">S’il existe plusieurs appels de méthode `Configure`, le dernier appel de `Configure` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="48f07-174">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="48f07-175">Étendre le démarrage avec les filtres de démarrage</span><span class="sxs-lookup"><span data-stu-id="48f07-175">Extend Startup with startup filters</span></span>

<span data-ttu-id="48f07-176">Utilisez <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> pour configurer le middleware au début ou à la fin du pipeline de middleware de la méthode [Configure](#the-configure-method) d’une application.</span><span class="sxs-lookup"><span data-stu-id="48f07-176">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="48f07-177">Utilisez `IStartupFilter` pour créer un pipeline de méthodes `Configure`.</span><span class="sxs-lookup"><span data-stu-id="48f07-177">`IStartupFilter` is used to create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="48f07-178">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) peut définir un middleware à exécuter avant ou après les middlewares qui sont ajoutés par les bibliothèques.</span><span class="sxs-lookup"><span data-stu-id="48f07-178">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="48f07-179">`IStartupFilter` implémente <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, qui reçoit et retourne un `Action<IApplicationBuilder>`.</span><span class="sxs-lookup"><span data-stu-id="48f07-179">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="48f07-180"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> définit une classe pour configurer le pipeline de requête d’une application.</span><span class="sxs-lookup"><span data-stu-id="48f07-180">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="48f07-181">Pour plus d’informations, consultez [Créer un pipeline de middlewares avec IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="48f07-181">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="48f07-182">Chaque `IStartupFilter` peut ajouter un ou plusieurs middlewares au pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="48f07-182">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="48f07-183">Les filtres sont appelés dans l’ordre dans lequel ils ont été ajoutés au conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="48f07-183">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="48f07-184">Les filtres peuvent ajouter l’intergiciel avant ou après la transmission du contrôle au filtre suivant. Par conséquent, ils s’ajoutent au début ou à la fin du pipeline de l’application.</span><span class="sxs-lookup"><span data-stu-id="48f07-184">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="48f07-185">L’exemple suivant montre comment inscrire un middleware auprès de `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="48f07-185">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="48f07-186">Le middleware `RequestSetOptionsMiddleware` définit une valeur d’options à partir d’un paramètre de chaîne de requête :</span><span class="sxs-lookup"><span data-stu-id="48f07-186">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="48f07-187">`RequestSetOptionsMiddleware` est configuré dans la classe `RequestSetOptionsStartupFilter` :</span><span class="sxs-lookup"><span data-stu-id="48f07-187">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="48f07-188">`RequestSetOptionsMiddleware` est configuré dans la classe `RequestSetOptionsStartupFilter` :</span><span class="sxs-lookup"><span data-stu-id="48f07-188">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

<span data-ttu-id="48f07-189">`IStartupFilter` est inscrit dans le conteneur de service de <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span><span class="sxs-lookup"><span data-stu-id="48f07-189">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

<span data-ttu-id="48f07-190">Quand un paramètre de chaîne de requête pour `option` est fourni, le middleware traite l’affectation de valeur avant que le middleware ASP.NET Core n’affiche la réponse.</span><span class="sxs-lookup"><span data-stu-id="48f07-190">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="48f07-191">L’ordre d’exécution de l’intergiciel est défini par l’ordre des inscriptions d’`IStartupFilter` :</span><span class="sxs-lookup"><span data-stu-id="48f07-191">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="48f07-192">Plusieurs implémentations d’`IStartupFilter` peuvent interagir avec les mêmes objets.</span><span class="sxs-lookup"><span data-stu-id="48f07-192">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="48f07-193">Si l’ordre est important, définissez l’ordre des inscriptions de leurs services `IStartupFilter` pour qu’il corresponde à l’ordre dans lequel leurs intergiciels doivent s’exécuter.</span><span class="sxs-lookup"><span data-stu-id="48f07-193">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="48f07-194">Les bibliothèques peuvent ajouter des intergiciels avec une ou plusieurs implémentations `IStartupFilter` qui s’exécutent avant ou après l’autre intergiciel d’application inscrit avec `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="48f07-194">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="48f07-195">Pour appeler un middleware `IStartupFilter` avant un middleware ajouté par le `IStartupFilter` d’une bibliothèque :</span><span class="sxs-lookup"><span data-stu-id="48f07-195">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="48f07-196">Placez l’inscription du service avant l’ajout de la bibliothèque au conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="48f07-196">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="48f07-197">Pour les appels suivants, placez l’inscription du service après l’ajout de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="48f07-197">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="48f07-198">Ajouter la configuration au démarrage à partir d’un assembly externe</span><span class="sxs-lookup"><span data-stu-id="48f07-198">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="48f07-199">Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application.</span><span class="sxs-lookup"><span data-stu-id="48f07-199">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="48f07-200">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="48f07-200">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48f07-201">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="48f07-201">Additional resources</span></span>

* [<span data-ttu-id="48f07-202">L’hôte</span><span class="sxs-lookup"><span data-stu-id="48f07-202">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
