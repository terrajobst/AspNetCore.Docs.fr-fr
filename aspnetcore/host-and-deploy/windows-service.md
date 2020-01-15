---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: 37fc0b7862db3280f9ade8d563feba28153ab79b
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75951838"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="074d5-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="074d5-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="074d5-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="074d5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="074d5-105">Une application ASP.NET Core peut être hébergée sur Windows en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) sans utiliser IIS.</span><span class="sxs-lookup"><span data-stu-id="074d5-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="074d5-106">Lorsqu’elle est hébergée en tant que service Windows, l’application se lance automatiquement après le redémarrage du serveur.</span><span class="sxs-lookup"><span data-stu-id="074d5-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="074d5-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="074d5-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="074d5-108">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="074d5-108">Prerequisites</span></span>

* [<span data-ttu-id="074d5-109">Kit de développement logiciel (SDK) ASP.NET Core 2.1 ou plus</span><span class="sxs-lookup"><span data-stu-id="074d5-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="074d5-110">PowerShell version 6.2 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="074d5-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="074d5-111">Modèle Service Worker</span><span class="sxs-lookup"><span data-stu-id="074d5-111">Worker Service template</span></span>

<span data-ttu-id="074d5-112">Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables.</span><span class="sxs-lookup"><span data-stu-id="074d5-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="074d5-113">Pour utiliser le modèle en tant que base d’une application de service Windows :</span><span class="sxs-lookup"><span data-stu-id="074d5-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="074d5-114">Créez une application Service Worker à partir du modèle .NET Core.</span><span class="sxs-lookup"><span data-stu-id="074d5-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="074d5-115">Suivez l’aide fournie dans la section [Configuration d’application](#app-configuration) pour mettre à jour l’application Service Worker afin qu’elle puisse s’exécuter en tant que service Windows.</span><span class="sxs-lookup"><span data-stu-id="074d5-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="074d5-116">Configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="074d5-116">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="074d5-117">L’application requiert une référence de package pour [Microsoft. extensions. Hosting. services Windows,](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="074d5-117">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="074d5-118">`IHostBuilder.UseWindowsService` est appelée lors de la génération de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="074d5-118">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="074d5-119">Si l’application s’exécute comme un service Windows, la méthode :</span><span class="sxs-lookup"><span data-stu-id="074d5-119">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="074d5-120">définit la durée de vie de l’hôte sur `WindowsServiceLifetime` ;</span><span class="sxs-lookup"><span data-stu-id="074d5-120">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="074d5-121">Affecte à la [racine du contenu](xref:fundamentals/index#content-root) la valeur [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="074d5-121">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="074d5-122">Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="074d5-122">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="074d5-123">Permet la journalisation dans le journal d’événements en utilisant le nom d’application en tant que nom de source par défaut.</span><span class="sxs-lookup"><span data-stu-id="074d5-123">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="074d5-124">Vous pouvez configurer le niveau de journalisation à l’aide de la clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="074d5-124">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="074d5-125">Seuls les administrateurs peuvent créer des sources d’événement.</span><span class="sxs-lookup"><span data-stu-id="074d5-125">Only administrators can create new event sources.</span></span> <span data-ttu-id="074d5-126">Si une source d’événement ne peut pas être créée en utilisant le nom de l’application, un avertissement est consigné dans la source *Application* source et les journaux d’événements sont désactivés.</span><span class="sxs-lookup"><span data-stu-id="074d5-126">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="074d5-127">Dans `CreateHostBuilder` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="074d5-127">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="074d5-128">Les exemples d’applications suivants accompagnent cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="074d5-128">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="074d5-129">Exemple de service de travail en arrière-plan &ndash; un exemple d’application Web non basée sur le [modèle de service de travail](#worker-service-template) qui utilise des [services hébergés](xref:fundamentals/host/hosted-services) pour les tâches en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="074d5-129">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="074d5-130">L’exemple de App Service Web &ndash; un exemple d’application Web Razor Pages qui s’exécute en tant que service Windows avec les [services hébergés](xref:fundamentals/host/hosted-services) pour les tâches en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="074d5-130">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="074d5-131">Pour obtenir des conseils sur MVC, consultez les articles sous <xref:mvc/overview> et <xref:migration/22-to-30>.</span><span class="sxs-lookup"><span data-stu-id="074d5-131">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="074d5-132">L’application nécessite des références de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) et [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="074d5-132">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="074d5-133">Pour effectuer des tests et un débogage lors de l’exécution en dehors d’un service, ajoutez un code pour déterminer si l’application s’exécute comme un service ou comme une application console.</span><span class="sxs-lookup"><span data-stu-id="074d5-133">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="074d5-134">Vérifiez si le débogueur est attaché ou si un switch `--console` est présent.</span><span class="sxs-lookup"><span data-stu-id="074d5-134">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="074d5-135">Si l’une de ces deux conditions est remplie (l’application n’est pas exécutée en tant que service), appelez <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="074d5-135">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="074d5-136">Si les conditions ne sont pas remplies (l’application est exécutée en tant que service) :</span><span class="sxs-lookup"><span data-stu-id="074d5-136">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="074d5-137">Appelez <xref:System.IO.Directory.SetCurrentDirectory*> et utilisez un chemin vers l’emplacement publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="074d5-137">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="074d5-138">N’appelez pas <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir le chemin d’accès car une application Windows Service retourne le dossier *C:\\WINDOWS\\system32* lorsque <xref:System.IO.Directory.GetCurrentDirectory*> est appelée.</span><span class="sxs-lookup"><span data-stu-id="074d5-138">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="074d5-139">Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="074d5-139">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="074d5-140">Cette étape est effectuée avant la configuration de l’application dans `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="074d5-140">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="074d5-141">Appelez <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> pour exécuter l’application en tant que service.</span><span class="sxs-lookup"><span data-stu-id="074d5-141">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="074d5-142">Étant donné que le [fournisseur de configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider) nécessite des paires nom/valeur pour les arguments de ligne de commande, le switch `--console` est supprimé des arguments avant que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ne les reçoive.</span><span class="sxs-lookup"><span data-stu-id="074d5-142">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="074d5-143">Pour consigner des informations dans le journal des événements Windows, ajoutez le fournisseur EventLog à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="074d5-143">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="074d5-144">Définissez le niveau de journalisation à l’aide de clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="074d5-144">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="074d5-145">Dans l’exemple suivant, qui provient de l’exemple d’application, l’élément `RunAsCustomService` est appelé à la place de l’élément <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> afin de gérer les événements de durée de vie au sein de l’application.</span><span class="sxs-lookup"><span data-stu-id="074d5-145">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="074d5-146">Pour plus d’informations, consultez la section [Gérer les événements de démarrage et d’arrêt](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="074d5-146">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="074d5-147">Type de déploiement</span><span class="sxs-lookup"><span data-stu-id="074d5-147">Deployment type</span></span>

<span data-ttu-id="074d5-148">Pour des informations et des conseils sur les scénarios de déploiement, consultez [Déploiement d’applications .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="074d5-148">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="074d5-149">Kit de développement logiciel</span><span class="sxs-lookup"><span data-stu-id="074d5-149">SDK</span></span>

<span data-ttu-id="074d5-150">Pour un service basé sur une application Web qui utilise le Razor Pages ou les frameworks MVC, spécifiez le kit de développement logiciel (SDK) Web dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="074d5-150">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="074d5-151">Si le service exécute uniquement des tâches en arrière-plan (par exemple, les [services hébergés](xref:fundamentals/host/hosted-services)), spécifiez le kit de développement logiciel (SDK) Worker dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="074d5-151">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="074d5-152">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="074d5-152">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="074d5-153">Un déploiement dépendant du framework s’appuie sur la présence d’une version partagée à l’échelle du système de .NET Core sur le système cible.</span><span class="sxs-lookup"><span data-stu-id="074d5-153">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="074d5-154">Lorsque vous effectuez le scénario de déploiement dépendant du framework en suivant les conseils du présent article, le Kit de développement logiciel (SDK) produit un fichier exécutable ( *.exe*), appelé *fichier exécutable dépendant du framework*.</span><span class="sxs-lookup"><span data-stu-id="074d5-154">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="074d5-155">Si vous utilisez le [Kit de développement logiciel (SDK) Web](#sdk), un fichier *Web. config* , qui est normalement généré lors de la publication d’une application ASP.net Core, n’est pas nécessaire pour une application de services Windows.</span><span class="sxs-lookup"><span data-stu-id="074d5-155">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="074d5-156">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="074d5-156">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="074d5-157">[L’identificateur relatif (RID)](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contient la version cible de .Net Framework.</span><span class="sxs-lookup"><span data-stu-id="074d5-157">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="074d5-158">Dans l’exemple suivant, le RID est défini sur `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="074d5-158">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="074d5-159">La propriété `<SelfContained>` a la valeur `false`.</span><span class="sxs-lookup"><span data-stu-id="074d5-159">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="074d5-160">Ces propriétés demandent au Kit de développement logiciel (SDK) de générer un fichier exécutable ( *.exe*) pour Windows et une application qui dépend du framework .NET Core partagé.</span><span class="sxs-lookup"><span data-stu-id="074d5-160">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="074d5-161">Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="074d5-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="074d5-162">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="074d5-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="074d5-163">[L’identificateur relatif (RID)](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contient la version cible de .Net Framework.</span><span class="sxs-lookup"><span data-stu-id="074d5-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="074d5-164">Dans l’exemple suivant, le RID est défini sur `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="074d5-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="074d5-165">La propriété `<SelfContained>` a la valeur `false`.</span><span class="sxs-lookup"><span data-stu-id="074d5-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="074d5-166">Ces propriétés demandent au Kit de développement logiciel (SDK) de générer un fichier exécutable ( *.exe*) pour Windows et une application qui dépend du framework .NET Core partagé.</span><span class="sxs-lookup"><span data-stu-id="074d5-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="074d5-167">La propriété `<UseAppHost>` a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="074d5-167">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="074d5-168">Cette propriété fournit au service un chemin d’activation (un fichier exécutable *.exe*) pour un déploiement dépendant du framework (FDD).</span><span class="sxs-lookup"><span data-stu-id="074d5-168">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="074d5-169">Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="074d5-169">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="074d5-170">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="074d5-170">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="074d5-171">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="074d5-171">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="074d5-172">Un déploiement autonome ne s’appuie sur la présence d’aucune infrastructure partagée sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="074d5-172">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="074d5-173">Le runtime et les dépendances de l’application sont déployés avec l’application.</span><span class="sxs-lookup"><span data-stu-id="074d5-173">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="074d5-174">Un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows est inclus dans l’élément `<PropertyGroup>` qui contient la version cible de .NET Framework :</span><span class="sxs-lookup"><span data-stu-id="074d5-174">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="074d5-175">Pour publier pour plusieurs RID :</span><span class="sxs-lookup"><span data-stu-id="074d5-175">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="074d5-176">Fournissez les RID dans une liste séparée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="074d5-176">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="074d5-177">Utilisez le nom de propriété [\<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (au pluriel).</span><span class="sxs-lookup"><span data-stu-id="074d5-177">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="074d5-178">Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="074d5-178">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="074d5-179">Une propriété `<SelfContained>` est définie sur `true` :</span><span class="sxs-lookup"><span data-stu-id="074d5-179">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="074d5-180">Compte d’utilisateur du service</span><span class="sxs-lookup"><span data-stu-id="074d5-180">Service user account</span></span>

<span data-ttu-id="074d5-181">Pour créer un compte d’utilisateur du service, utilisez la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) depuis un interpréteur de commandes d’administration PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="074d5-181">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="074d5-182">Dans la Mise à jour de Windows 10 d’octobre 2018 (version 1809/build 10.0.17763) ou plus :</span><span class="sxs-lookup"><span data-stu-id="074d5-182">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="074d5-183">Dans un système d’exploitation Windows antérieur à la Mise à jour de Windows 10 d’octobre 2018 (version 1809/build 10.0.17763) :</span><span class="sxs-lookup"><span data-stu-id="074d5-183">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="074d5-184">Fournissez un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) à l’invite.</span><span class="sxs-lookup"><span data-stu-id="074d5-184">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="074d5-185">Le compte n’expire pas, sauf si le paramètre `-AccountExpires` est fourni à la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) avec un <xref:System.DateTime> d’expiration.</span><span class="sxs-lookup"><span data-stu-id="074d5-185">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="074d5-186">Pour plus d’informations, voir [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) et [Comptes d’utilisateurs de service](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="074d5-186">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="074d5-187">Une approche alternative à la gestion des utilisateurs lors de l’utilisation d’Active Directory consiste à utiliser des Comptes de service administrés.</span><span class="sxs-lookup"><span data-stu-id="074d5-187">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="074d5-188">Pour plus d’informations, consultez [Vue d’ensemble des comptes de service administrés de groupe](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="074d5-188">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="074d5-189">Droits d’ouverture de session en tant que service</span><span class="sxs-lookup"><span data-stu-id="074d5-189">Log on as a service rights</span></span>

<span data-ttu-id="074d5-190">Pour établir des droits *d’ouverture de session en tant que service* pour un compte d’utilisateur de service, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="074d5-190">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="074d5-191">Ouvrez l’éditeur de stratégie de sécurité locale en exécutant le fichier *secpool.msc*.</span><span class="sxs-lookup"><span data-stu-id="074d5-191">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="074d5-192">Développez le nœud **Stratégies locales** et sélectionnez **Attribution des droits utilisateur**.</span><span class="sxs-lookup"><span data-stu-id="074d5-192">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="074d5-193">Ouvrez la stratégie **Ouvrir une session en tant que service**.</span><span class="sxs-lookup"><span data-stu-id="074d5-193">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="074d5-194">Sélectionnez **Ajouter un utilisateur ou un groupe**.</span><span class="sxs-lookup"><span data-stu-id="074d5-194">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="074d5-195">Fournissez le nom d’objet (compte d’utilisateur) avec l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="074d5-195">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="074d5-196">Tapez le compte d’utilisateur (`{DOMAIN OR COMPUTER NAME\USER}`) dans le champ du nom d’objet et sélectionnez **OK** pour ajouter l’utilisateur à la stratégie.</span><span class="sxs-lookup"><span data-stu-id="074d5-196">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="074d5-197">Sélectionnez **Avancé**.</span><span class="sxs-lookup"><span data-stu-id="074d5-197">Select **Advanced**.</span></span> <span data-ttu-id="074d5-198">Sélectionnez **Rechercher maintenant**.</span><span class="sxs-lookup"><span data-stu-id="074d5-198">Select **Find Now**.</span></span> <span data-ttu-id="074d5-199">Sélectionnez le compte d’utilisateur dans la liste.</span><span class="sxs-lookup"><span data-stu-id="074d5-199">Select the user account from the list.</span></span> <span data-ttu-id="074d5-200">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="074d5-200">Select **OK**.</span></span> <span data-ttu-id="074d5-201">Cliquez à nouveau sur **OK** pour ajouter l’utilisateur à la stratégie.</span><span class="sxs-lookup"><span data-stu-id="074d5-201">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="074d5-202">Sélectionnez **OK** ou **Appliquer** pour accepter les modifications.</span><span class="sxs-lookup"><span data-stu-id="074d5-202">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="074d5-203">Créer et gérer le service Windows</span><span class="sxs-lookup"><span data-stu-id="074d5-203">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="074d5-204">Créer un service</span><span class="sxs-lookup"><span data-stu-id="074d5-204">Create a service</span></span>

<span data-ttu-id="074d5-205">Utilisez les commandes PowerShell pour enregistrer un service.</span><span class="sxs-lookup"><span data-stu-id="074d5-205">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="074d5-206">À partir d’un interpréteur de commandes d’administration PowerShell 6, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="074d5-206">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="074d5-207">`{EXE PATH}` &ndash; chemin d’accès au dossier de l’application sur l’hôte (par exemple, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="074d5-207">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="074d5-208">N’incluez pas le fichier exécutable de l’application dans le chemin.</span><span class="sxs-lookup"><span data-stu-id="074d5-208">Don't include the app's executable in the path.</span></span> <span data-ttu-id="074d5-209">Aucune barre oblique de fin n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="074d5-209">A trailing slash isn't required.</span></span>
* <span data-ttu-id="074d5-210">`{DOMAIN OR COMPUTER NAME\USER}` compte d’utilisateur du service &ndash; (par exemple, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="074d5-210">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="074d5-211">`{SERVICE NAME}` &ndash; nom du service (par exemple, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="074d5-211">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="074d5-212">`{EXE FILE PATH}` &ndash; le chemin d’accès de l’exécutable de l’application (par exemple, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="074d5-212">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="074d5-213">Incluez le nom de fichier de l’exécutable avec l’extension.</span><span class="sxs-lookup"><span data-stu-id="074d5-213">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="074d5-214">`{DESCRIPTION}` &ndash; Description du service (par exemple, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="074d5-214">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="074d5-215">`{DISPLAY NAME}` &ndash; nom complet du service (par exemple, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="074d5-215">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="074d5-216">Démarrer un service</span><span class="sxs-lookup"><span data-stu-id="074d5-216">Start a service</span></span>

<span data-ttu-id="074d5-217">Démarrez un service avec la commande PowerShell 6 suivante :</span><span class="sxs-lookup"><span data-stu-id="074d5-217">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="074d5-218">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="074d5-218">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="074d5-219">Déterminer l’état d’un service</span><span class="sxs-lookup"><span data-stu-id="074d5-219">Determine a service's status</span></span>

<span data-ttu-id="074d5-220">Pour vérifier l’état d’un service, utilisez la commande PowerShell 6 suivante :</span><span class="sxs-lookup"><span data-stu-id="074d5-220">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="074d5-221">L’état est signalé comme étant l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="074d5-221">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="074d5-222">Arrêter un service</span><span class="sxs-lookup"><span data-stu-id="074d5-222">Stop a service</span></span>

<span data-ttu-id="074d5-223">Arrêtez un service avec la commande Powershell 6 suivante :</span><span class="sxs-lookup"><span data-stu-id="074d5-223">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="074d5-224">Supprimer un service</span><span class="sxs-lookup"><span data-stu-id="074d5-224">Remove a service</span></span>

<span data-ttu-id="074d5-225">Après un court délai pour arrêter un service, supprimez-le avec la commande PowerShell 6 suivante :</span><span class="sxs-lookup"><span data-stu-id="074d5-225">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="074d5-226">Gérer les événements de démarrage et d’arrêt</span><span class="sxs-lookup"><span data-stu-id="074d5-226">Handle starting and stopping events</span></span>

<span data-ttu-id="074d5-227">Pour gérer les événements <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> et <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="074d5-227">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="074d5-228">Créez une classe qui dérive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> à l’aide des méthodes `OnStarting`, `OnStarted` et `OnStopping` :</span><span class="sxs-lookup"><span data-stu-id="074d5-228">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="074d5-229">Créez une méthode d’extension pour <xref:Microsoft.AspNetCore.Hosting.IWebHost> qui transmet `CustomWebHostService` à <xref:System.ServiceProcess.ServiceBase.Run*> :</span><span class="sxs-lookup"><span data-stu-id="074d5-229">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="074d5-230">Dans `Program.Main`, appelez la méthode d’extension `RunAsCustomService` au lieu de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> :</span><span class="sxs-lookup"><span data-stu-id="074d5-230">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="074d5-231">Pour afficher l’emplacement de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> dans `Program.Main`, reportez-vous à l’exemple de code indiqué dans la section [Type de déploiement](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="074d5-231">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="074d5-232">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="074d5-232">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="074d5-233">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="074d5-233">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="074d5-234">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="074d5-234">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="074d5-235">Configurer les points de terminaison</span><span class="sxs-lookup"><span data-stu-id="074d5-235">Configure endpoints</span></span>

<span data-ttu-id="074d5-236">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="074d5-236">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="074d5-237">Configurez l’URL et le port en définissant la variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="074d5-237">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="074d5-238">Pour obtenir d’autres approches de configuration des ports et des URL, consultez l’article approprié sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="074d5-238">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="074d5-239">L’aide précédente couvre la prise en charge des points de terminaison HTTPs.</span><span class="sxs-lookup"><span data-stu-id="074d5-239">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="074d5-240">Par exemple, configurez l’application pour HTTPs lorsque l’authentification est utilisée avec un service Windows.</span><span class="sxs-lookup"><span data-stu-id="074d5-240">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="074d5-241">L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="074d5-241">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="074d5-242">Répertoire actif et racine du contenu</span><span class="sxs-lookup"><span data-stu-id="074d5-242">Current directory and content root</span></span>

<span data-ttu-id="074d5-243">Le répertoire de travail actif retourné par l’appel à <xref:System.IO.Directory.GetCurrentDirectory*> pour un service Windows est le dossier *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="074d5-243">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="074d5-244">Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres).</span><span class="sxs-lookup"><span data-stu-id="074d5-244">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="074d5-245">Utilisez une des approches suivantes pour gérer les ressources ainsi que les fichiers de paramètres d’un service, et y accéder.</span><span class="sxs-lookup"><span data-stu-id="074d5-245">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="074d5-246">Utiliser ContentRootPath ou ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="074d5-246">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="074d5-247">Utilisez les éléments [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) ou <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> pour localiser les ressources d’une application.</span><span class="sxs-lookup"><span data-stu-id="074d5-247">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="074d5-248">Lorsque l’application s’exécute en tant que service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> définit le <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> sur [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="074d5-248">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="074d5-249">Les fichiers de paramètres par défaut de l’application, *appSettings. JSON* et *appSettings. { Environment}. JSON*, sont chargés à partir de la racine de contenu de l’application en appelant [CreateDefaultBuilder lors](xref:fundamentals/host/generic-host#set-up-a-host)de la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="074d5-249">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="074d5-250">Pour les autres fichiers de paramètres chargés par le code du développeur dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, il n’est pas nécessaire d’appeler <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="074d5-250">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="074d5-251">Dans l’exemple suivant, le fichier *custom_settings. JSON* existe dans la racine de contenu de l’application et est chargé sans définir explicitement un chemin d’accès de base :</span><span class="sxs-lookup"><span data-stu-id="074d5-251">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="074d5-252">N’essayez pas d’utiliser <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir un chemin d’accès de ressource, car une application de service Windows retourne le dossier *C :\\Windows\\system32* comme répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="074d5-252">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="074d5-253">Définir le dossier de l’application comme chemin d’accès racine du contenu</span><span class="sxs-lookup"><span data-stu-id="074d5-253">Set the content root path to the app's folder</span></span>

<span data-ttu-id="074d5-254">La chaîne <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> correspond au même chemin que celui fourni à l’argument `binPath` lorsqu’un service est créé.</span><span class="sxs-lookup"><span data-stu-id="074d5-254">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="074d5-255">Au lieu d’appeler `GetCurrentDirectory` pour créer des chemins d’accès aux fichiers de paramètres, appelez <xref:System.IO.Directory.SetCurrentDirectory*> avec le chemin d’accès à la [racine de contenu](xref:fundamentals/index#content-root)de l’application.</span><span class="sxs-lookup"><span data-stu-id="074d5-255">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="074d5-256">Dans `Program.Main`, définissez le chemin d’accès au dossier du fichier exécutable du service ainsi que le chemin d’accès pour établir la racine du contenu de l’application :</span><span class="sxs-lookup"><span data-stu-id="074d5-256">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="074d5-257">Stocker les fichiers d’un service dans un emplacement approprié sur le disque</span><span class="sxs-lookup"><span data-stu-id="074d5-257">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="074d5-258">Spécifiez un chemin absolu avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>, si vous utilisez <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, vers le dossier contenant les fichiers.</span><span class="sxs-lookup"><span data-stu-id="074d5-258">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="074d5-259">Dépannage</span><span class="sxs-lookup"><span data-stu-id="074d5-259">Troubleshoot</span></span>

<span data-ttu-id="074d5-260">Pour dépanner une application de service Windows, consultez <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="074d5-260">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="074d5-261">Erreurs courantes</span><span class="sxs-lookup"><span data-stu-id="074d5-261">Common errors</span></span>

* <span data-ttu-id="074d5-262">Une version ancienne ou préliminaire de PowerShell est en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="074d5-262">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="074d5-263">Le service inscrit n’utilise pas la sortie **publiée** de l’application à partir de la commande [dotnet Publish](/dotnet/core/tools/dotnet-publish) .</span><span class="sxs-lookup"><span data-stu-id="074d5-263">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="074d5-264">La sortie de la commande [dotnet Build](/dotnet/core/tools/dotnet-build) n’est pas prise en charge pour le déploiement d’applications.</span><span class="sxs-lookup"><span data-stu-id="074d5-264">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="074d5-265">Les ressources publiées se trouvent dans l’un des dossiers suivants, selon le type de déploiement :</span><span class="sxs-lookup"><span data-stu-id="074d5-265">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="074d5-266">*bin/Release/{Target Framework}/Publish* (FDD)</span><span class="sxs-lookup"><span data-stu-id="074d5-266">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="074d5-267">*bin/Release/{Target Framework}/{Runtime identificateur}/Publish* (SCD)</span><span class="sxs-lookup"><span data-stu-id="074d5-267">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="074d5-268">Le service n’est pas à l’État en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="074d5-268">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="074d5-269">Les chemins d’accès aux ressources que l’application utilise (par exemple, les certificats) sont incorrects.</span><span class="sxs-lookup"><span data-stu-id="074d5-269">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="074d5-270">Le chemin d’accès de base d’un service Windows est *c :\\Windows\\system32*.</span><span class="sxs-lookup"><span data-stu-id="074d5-270">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="074d5-271">L’utilisateur ne dispose pas de droits *d’ouverture de session en tant que service* .</span><span class="sxs-lookup"><span data-stu-id="074d5-271">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="074d5-272">Le mot de passe de l’utilisateur a expiré ou a été transmis de manière incorrecte lors de l’exécution de la commande `New-Service` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="074d5-272">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="074d5-273">L’application requiert l’authentification ASP.NET Core, mais elle n’est pas configurée pour les connexions sécurisées (HTTPs).</span><span class="sxs-lookup"><span data-stu-id="074d5-273">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="074d5-274">Le port de l’URL de requête est incorrect ou n’est pas correctement configuré dans l’application.</span><span class="sxs-lookup"><span data-stu-id="074d5-274">The the request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="074d5-275">Journaux des événements système et des applications</span><span class="sxs-lookup"><span data-stu-id="074d5-275">System and Application Event Logs</span></span>

<span data-ttu-id="074d5-276">Accédez aux journaux des événements système et des applications :</span><span class="sxs-lookup"><span data-stu-id="074d5-276">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="074d5-277">Ouvrez le menu Démarrer, recherchez *Observateur d’événements*, puis sélectionnez l’application **Observateur d’événements** .</span><span class="sxs-lookup"><span data-stu-id="074d5-277">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="074d5-278">Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.</span><span class="sxs-lookup"><span data-stu-id="074d5-278">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="074d5-279">Sélectionnez **système** pour ouvrir le journal des événements système.</span><span class="sxs-lookup"><span data-stu-id="074d5-279">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="074d5-280">Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="074d5-280">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="074d5-281">Recherchez les erreurs liées à l’application défectueuse.</span><span class="sxs-lookup"><span data-stu-id="074d5-281">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="074d5-282">Exécuter l’application depuis une invite de commandes</span><span class="sxs-lookup"><span data-stu-id="074d5-282">Run the app at a command prompt</span></span>

<span data-ttu-id="074d5-283">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans les journaux des événements.</span><span class="sxs-lookup"><span data-stu-id="074d5-283">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="074d5-284">Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="074d5-284">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="074d5-285">Pour consigner des détails supplémentaires à partir de l’application, diminuez le [niveau de journalisation](xref:fundamentals/logging/index#log-level) ou exécutez l’application dans l' [environnement de développement](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="074d5-285">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="074d5-286">Effacer les caches de package</span><span class="sxs-lookup"><span data-stu-id="074d5-286">Clear package caches</span></span>

<span data-ttu-id="074d5-287">Une application fonctionnelle peut échouer immédiatement après la mise à niveau de la kit SDK .NET Core sur l’ordinateur de développement ou la modification des versions de package dans l’application.</span><span class="sxs-lookup"><span data-stu-id="074d5-287">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="074d5-288">Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures.</span><span class="sxs-lookup"><span data-stu-id="074d5-288">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="074d5-289">Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :</span><span class="sxs-lookup"><span data-stu-id="074d5-289">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="074d5-290">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="074d5-290">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="074d5-291">Effacez les caches de package en exécutant [dotnet NuGet LOCALS tout--Clear](/dotnet/core/tools/dotnet-nuget-locals) dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="074d5-291">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="074d5-292">L’effacement des caches de package peut également être effectué à l’aide de l’outil [NuGet. exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="074d5-292">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="074d5-293">*NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="074d5-293">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="074d5-294">Restaurez et regénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="074d5-294">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="074d5-295">Supprimez tous les fichiers du dossier de déploiement sur le serveur avant de redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="074d5-295">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="074d5-296">Application lente ou bloquée</span><span class="sxs-lookup"><span data-stu-id="074d5-296">Slow or hanging app</span></span>

<span data-ttu-id="074d5-297">Un *vidage sur incident* est un instantané de la mémoire du système et peut aider à déterminer la cause d’un incident d’application, d’un échec de démarrage ou d’une application lente.</span><span class="sxs-lookup"><span data-stu-id="074d5-297">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="074d5-298">L’application cesse de fonctionner ou rencontre une exception</span><span class="sxs-lookup"><span data-stu-id="074d5-298">App crashes or encounters an exception</span></span>

<span data-ttu-id="074d5-299">Obtenez un fichier dump et analysez-le depuis le [Rapport d'erreurs Windows](/windows/desktop/wer/windows-error-reporting) :</span><span class="sxs-lookup"><span data-stu-id="074d5-299">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="074d5-300">Créez un dossier pour accueillir les fichiers d’image mémoire dans `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="074d5-300">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="074d5-301">Exécutez le [script PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) avec le nom de l’exécutable de l’application :</span><span class="sxs-lookup"><span data-stu-id="074d5-301">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="074d5-302">Exécutez l’application en reproduisant les conditions ayant entraîné l’incident.</span><span class="sxs-lookup"><span data-stu-id="074d5-302">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="074d5-303">Une fois l’incident survenu, exécutez le [script PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="074d5-303">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="074d5-304">Après l’arrêt de l’application et après avoir terminé la collection dump, l’application peut être fermée normalement.</span><span class="sxs-lookup"><span data-stu-id="074d5-304">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="074d5-305">Le script PowerShell configure le rapport d’erreurs Windows pour collecter jusqu'à cinq fichiers dump par application.</span><span class="sxs-lookup"><span data-stu-id="074d5-305">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="074d5-306">Les fichiers dump d’incident peuvent occuper plus d’espace disque (jusqu’à plusieurs gigaoctets par fichier).</span><span class="sxs-lookup"><span data-stu-id="074d5-306">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="074d5-307">L’application se bloque, ne démarre pas ou s’exécute normalement</span><span class="sxs-lookup"><span data-stu-id="074d5-307">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="074d5-308">Quand une application *se bloque* (cesse de répondre mais ne se bloque pas), échoue pendant le démarrage ou s’exécute normalement, consultez [fichiers de vidage en mode utilisateur : choix de l’outil le mieux](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) adapté pour sélectionner un outil approprié pour produire le vidage.</span><span class="sxs-lookup"><span data-stu-id="074d5-308">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="074d5-309">Analyser le fichier dump</span><span class="sxs-lookup"><span data-stu-id="074d5-309">Analyze the dump</span></span>

<span data-ttu-id="074d5-310">Un fichier dump peut être analysé à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="074d5-310">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="074d5-311">Pour plus d’informations, consultez [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analyser un fichier dump en mode utilisateur).</span><span class="sxs-lookup"><span data-stu-id="074d5-311">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="074d5-312">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="074d5-312">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="074d5-313">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="074d5-313">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="074d5-314">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="074d5-314">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
