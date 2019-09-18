---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/09/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 995fdd2bbba30ff983bc2055fcb97c14541e2ac6
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081482"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="e2d51-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="e2d51-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="e2d51-104">Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e2d51-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e2d51-105">Une application ASP.NET Core peut être hébergée sur Windows en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) sans utiliser IIS.</span><span class="sxs-lookup"><span data-stu-id="e2d51-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="e2d51-106">Lorsqu’elle est hébergée en tant que service Windows, l’application se lance automatiquement après le redémarrage du serveur.</span><span class="sxs-lookup"><span data-stu-id="e2d51-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="e2d51-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2d51-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2d51-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e2d51-108">Prerequisites</span></span>

* [<span data-ttu-id="e2d51-109">Kit de développement logiciel (SDK) ASP.NET Core 2.1 ou plus</span><span class="sxs-lookup"><span data-stu-id="e2d51-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="e2d51-110">PowerShell version 6.2 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="e2d51-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="e2d51-111">Modèle Service Worker</span><span class="sxs-lookup"><span data-stu-id="e2d51-111">Worker Service template</span></span>

<span data-ttu-id="e2d51-112">Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables.</span><span class="sxs-lookup"><span data-stu-id="e2d51-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="e2d51-113">Pour utiliser le modèle en tant que base d’une application de service Windows :</span><span class="sxs-lookup"><span data-stu-id="e2d51-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="e2d51-114">Créez une application Service Worker à partir du modèle .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2d51-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="e2d51-115">Suivez l’aide fournie dans la section [Configuration d’application](#app-configuration) pour mettre à jour l’application Service Worker afin qu’elle puisse s’exécuter en tant que service Windows.</span><span class="sxs-lookup"><span data-stu-id="e2d51-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2d51-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2d51-116">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e2d51-117">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="e2d51-117">Create a new project.</span></span>
1. <span data-ttu-id="e2d51-118">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e2d51-119">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-119">Select **Next**.</span></span>
1. <span data-ttu-id="e2d51-120">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="e2d51-120">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="e2d51-121">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-121">Select **Create**.</span></span>
1. <span data-ttu-id="e2d51-122">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="e2d51-122">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="e2d51-123">Sélectionnez le modèle **Service Worker**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-123">Select the **Worker Service** template.</span></span> <span data-ttu-id="e2d51-124">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-124">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2d51-125">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2d51-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e2d51-126">Utilisez le modèle Service Worker (`worker`) avec la commande [dotnet new](/dotnet/core/tools/dotnet-new) à partir d’un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="e2d51-126">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="e2d51-127">Dans l’exemple suivant, une application Service Worker est créée et se nomme `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-127">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="e2d51-128">Un dossier pour l’application `ContosoWorkerService` est créé automatiquement durant l’exécution de la commande.</span><span class="sxs-lookup"><span data-stu-id="e2d51-128">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="e2d51-129">Configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="e2d51-129">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e2d51-130">L’élément `IHostBuilder.UseWindowsService`, fourni par le package [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices), est appelé lors de la création de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="e2d51-130">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="e2d51-131">Si l’application s’exécute comme un service Windows, la méthode :</span><span class="sxs-lookup"><span data-stu-id="e2d51-131">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="e2d51-132">définit la durée de vie de l’hôte sur `WindowsServiceLifetime` ;</span><span class="sxs-lookup"><span data-stu-id="e2d51-132">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="e2d51-133">définit la racine du contenu.</span><span class="sxs-lookup"><span data-stu-id="e2d51-133">Sets the content root.</span></span>
* <span data-ttu-id="e2d51-134">Permet la journalisation dans le journal d’événements en utilisant le nom d’application en tant que nom de source par défaut.</span><span class="sxs-lookup"><span data-stu-id="e2d51-134">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="e2d51-135">Vous pouvez configurer le niveau de journalisation à l’aide de la clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="e2d51-135">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="e2d51-136">Seuls les administrateurs peuvent créer des sources d’événement.</span><span class="sxs-lookup"><span data-stu-id="e2d51-136">Only administrators can create new event sources.</span></span> <span data-ttu-id="e2d51-137">Si une source d’événement ne peut pas être créée en utilisant le nom de l’application, un avertissement est consigné dans la source *Application* source et les journaux d’événements sont désactivés.</span><span class="sxs-lookup"><span data-stu-id="e2d51-137">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e2d51-138">L’application nécessite des références de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) et [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="e2d51-138">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="e2d51-139">Pour effectuer des tests et un débogage lors de l’exécution en dehors d’un service, ajoutez un code pour déterminer si l’application s’exécute comme un service ou comme une application console.</span><span class="sxs-lookup"><span data-stu-id="e2d51-139">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="e2d51-140">Vérifiez si le débogueur est attaché ou si un switch `--console` est présent.</span><span class="sxs-lookup"><span data-stu-id="e2d51-140">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="e2d51-141">Si l’une de ces deux conditions est remplie (l’application n’est pas exécutée en tant que service), appelez <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="e2d51-141">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="e2d51-142">Si les conditions ne sont pas remplies (l’application est exécutée en tant que service) :</span><span class="sxs-lookup"><span data-stu-id="e2d51-142">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="e2d51-143">Appelez <xref:System.IO.Directory.SetCurrentDirectory*> et utilisez un chemin vers l’emplacement publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2d51-143">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="e2d51-144">N’appelez pas <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir le chemin d’accès car une application Windows Service retourne le dossier *C:\\WINDOWS\\system32* lorsque <xref:System.IO.Directory.GetCurrentDirectory*> est appelée.</span><span class="sxs-lookup"><span data-stu-id="e2d51-144">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="e2d51-145">Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="e2d51-145">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="e2d51-146">Cette étape est effectuée avant la configuration de l’application dans `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-146">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="e2d51-147">Appelez <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> pour exécuter l’application en tant que service.</span><span class="sxs-lookup"><span data-stu-id="e2d51-147">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="e2d51-148">Étant donné que le [fournisseur de configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider) nécessite des paires nom/valeur pour les arguments de ligne de commande, le switch `--console` est supprimé des arguments avant que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ne les reçoive.</span><span class="sxs-lookup"><span data-stu-id="e2d51-148">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="e2d51-149">Pour consigner des informations dans le journal des événements Windows, ajoutez le fournisseur EventLog à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="e2d51-149">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="e2d51-150">Définissez le niveau de journalisation à l’aide de clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="e2d51-150">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="e2d51-151">Dans l’exemple suivant, qui provient de l’exemple d’application, l’élément `RunAsCustomService` est appelé à la place de l’élément <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> afin de gérer les événements de durée de vie au sein de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2d51-151">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="e2d51-152">Pour plus d’informations, consultez la section [Gérer les événements de démarrage et d’arrêt](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="e2d51-152">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="e2d51-153">Type de déploiement</span><span class="sxs-lookup"><span data-stu-id="e2d51-153">Deployment type</span></span>

<span data-ttu-id="e2d51-154">Pour des informations et des conseils sur les scénarios de déploiement, consultez [Déploiement d’applications .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="e2d51-154">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="e2d51-155">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="e2d51-155">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="e2d51-156">Un déploiement dépendant du framework s’appuie sur la présence d’une version partagée à l’échelle du système de .NET Core sur le système cible.</span><span class="sxs-lookup"><span data-stu-id="e2d51-156">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="e2d51-157">Lorsque vous effectuez le scénario de déploiement dépendant du framework en suivant les conseils du présent article, le Kit de développement logiciel (SDK) produit un fichier exécutable ( *.exe*), appelé *fichier exécutable dépendant du framework*.</span><span class="sxs-lookup"><span data-stu-id="e2d51-157">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e2d51-158">Ajoutez les éléments de propriété suivants au fichier projet :</span><span class="sxs-lookup"><span data-stu-id="e2d51-158">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="e2d51-159">`<OutputType>` &ndash; Le type de sortie de l’application (`Exe` pour le fichier exécutable)</span><span class="sxs-lookup"><span data-stu-id="e2d51-159">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="e2d51-160">`<LangVersion>` &ndash; La version du langage C# (`latest` ou `preview`)</span><span class="sxs-lookup"><span data-stu-id="e2d51-160">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="e2d51-161">Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="e2d51-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="e2d51-162">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="e2d51-163">[L’identificateur relatif (RID)](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contient la version cible de .Net Framework.</span><span class="sxs-lookup"><span data-stu-id="e2d51-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="e2d51-164">Dans l’exemple suivant, le RID est défini sur `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="e2d51-165">La propriété `<SelfContained>` a la valeur `false`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="e2d51-166">Ces propriétés demandent au Kit de développement logiciel (SDK) de générer un fichier exécutable ( *.exe*) pour Windows et une application qui dépend du framework .NET Core partagé.</span><span class="sxs-lookup"><span data-stu-id="e2d51-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="e2d51-167">Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="e2d51-167">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="e2d51-168">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-168">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="e2d51-169">[L’identificateur relatif (RID)](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contient la version cible de .Net Framework.</span><span class="sxs-lookup"><span data-stu-id="e2d51-169">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="e2d51-170">Dans l’exemple suivant, le RID est défini sur `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-170">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="e2d51-171">La propriété `<SelfContained>` a la valeur `false`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-171">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="e2d51-172">Ces propriétés demandent au Kit de développement logiciel (SDK) de générer un fichier exécutable ( *.exe*) pour Windows et une application qui dépend du framework .NET Core partagé.</span><span class="sxs-lookup"><span data-stu-id="e2d51-172">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="e2d51-173">La propriété `<UseAppHost>` a la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-173">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="e2d51-174">Cette propriété fournit au service un chemin d’activation (un fichier exécutable *.exe*) pour un déploiement dépendant du framework (FDD).</span><span class="sxs-lookup"><span data-stu-id="e2d51-174">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="e2d51-175">Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="e2d51-175">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="e2d51-176">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-176">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="e2d51-177">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="e2d51-177">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="e2d51-178">Un déploiement autonome ne s’appuie sur la présence d’aucune infrastructure partagée sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="e2d51-178">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="e2d51-179">Le runtime et les dépendances de l’application sont déployés avec l’application.</span><span class="sxs-lookup"><span data-stu-id="e2d51-179">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="e2d51-180">Un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows est inclus dans l’élément `<PropertyGroup>` qui contient la version cible de .NET Framework :</span><span class="sxs-lookup"><span data-stu-id="e2d51-180">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="e2d51-181">Pour publier pour plusieurs RID :</span><span class="sxs-lookup"><span data-stu-id="e2d51-181">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="e2d51-182">Fournissez les RID dans une liste séparée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="e2d51-182">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="e2d51-183">Utilisez le nom de propriété [\<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (au pluriel).</span><span class="sxs-lookup"><span data-stu-id="e2d51-183">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="e2d51-184">Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="e2d51-184">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e2d51-185">Une propriété `<SelfContained>` est définie sur `true` :</span><span class="sxs-lookup"><span data-stu-id="e2d51-185">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="e2d51-186">Compte d’utilisateur du service</span><span class="sxs-lookup"><span data-stu-id="e2d51-186">Service user account</span></span>

<span data-ttu-id="e2d51-187">Pour créer un compte d’utilisateur du service, utilisez la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) depuis un interpréteur de commandes d’administration PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="e2d51-187">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="e2d51-188">Dans la Mise à jour de Windows 10 d’octobre 2018 (version 1809/build 10.0.17763) ou plus :</span><span class="sxs-lookup"><span data-stu-id="e2d51-188">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="e2d51-189">Dans un système d’exploitation Windows antérieur à la Mise à jour de Windows 10 d’octobre 2018 (version 1809/build 10.0.17763) :</span><span class="sxs-lookup"><span data-stu-id="e2d51-189">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="e2d51-190">Fournissez un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) à l’invite.</span><span class="sxs-lookup"><span data-stu-id="e2d51-190">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="e2d51-191">Le compte n’expire pas, sauf si le paramètre `-AccountExpires` est fourni à la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) avec un <xref:System.DateTime> d’expiration.</span><span class="sxs-lookup"><span data-stu-id="e2d51-191">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="e2d51-192">Pour plus d’informations, voir [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) et [Comptes d’utilisateurs de service](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="e2d51-192">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="e2d51-193">Une approche alternative à la gestion des utilisateurs lors de l’utilisation d’Active Directory consiste à utiliser des Comptes de service administrés.</span><span class="sxs-lookup"><span data-stu-id="e2d51-193">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="e2d51-194">Pour plus d’informations, consultez [Vue d’ensemble des comptes de service administrés de groupe](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="e2d51-194">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="e2d51-195">Droits d’ouverture de session en tant que service</span><span class="sxs-lookup"><span data-stu-id="e2d51-195">Log on as a service rights</span></span>

<span data-ttu-id="e2d51-196">Pour établir des droits *d’ouverture de session en tant que service* pour un compte d’utilisateur de service, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="e2d51-196">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="e2d51-197">Ouvrez l’éditeur de stratégie de sécurité locale en exécutant le fichier *secpool.msc*.</span><span class="sxs-lookup"><span data-stu-id="e2d51-197">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="e2d51-198">Développez le nœud **Stratégies locales** et sélectionnez **Attribution des droits utilisateur**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-198">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="e2d51-199">Ouvrez la stratégie **Ouvrir une session en tant que service**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-199">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="e2d51-200">Sélectionnez **Ajouter un utilisateur ou un groupe**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-200">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="e2d51-201">Fournissez le nom d’objet (compte d’utilisateur) avec l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2d51-201">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="e2d51-202">Tapez le compte d’utilisateur (`{DOMAIN OR COMPUTER NAME\USER}`) dans le champ du nom d’objet et sélectionnez **OK** pour ajouter l’utilisateur à la stratégie.</span><span class="sxs-lookup"><span data-stu-id="e2d51-202">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="e2d51-203">Sélectionnez **Avancé**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-203">Select **Advanced**.</span></span> <span data-ttu-id="e2d51-204">Sélectionnez **Rechercher maintenant**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-204">Select **Find Now**.</span></span> <span data-ttu-id="e2d51-205">Sélectionnez le compte d’utilisateur dans la liste.</span><span class="sxs-lookup"><span data-stu-id="e2d51-205">Select the user account from the list.</span></span> <span data-ttu-id="e2d51-206">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2d51-206">Select **OK**.</span></span> <span data-ttu-id="e2d51-207">Cliquez à nouveau sur **OK** pour ajouter l’utilisateur à la stratégie.</span><span class="sxs-lookup"><span data-stu-id="e2d51-207">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="e2d51-208">Sélectionnez **OK** ou **Appliquer** pour accepter les modifications.</span><span class="sxs-lookup"><span data-stu-id="e2d51-208">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="e2d51-209">Créer et gérer le service Windows</span><span class="sxs-lookup"><span data-stu-id="e2d51-209">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="e2d51-210">Créer un service</span><span class="sxs-lookup"><span data-stu-id="e2d51-210">Create a service</span></span>

<span data-ttu-id="e2d51-211">Utilisez les commandes PowerShell pour enregistrer un service.</span><span class="sxs-lookup"><span data-stu-id="e2d51-211">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="e2d51-212">À partir d’un interpréteur de commandes d’administration PowerShell 6, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2d51-212">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="e2d51-213">`{EXE PATH}` &ndash; Chemin d’accès au dossier de l’application sur l’hôte (par exemple, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="e2d51-213">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="e2d51-214">N’incluez pas le fichier exécutable de l’application dans le chemin.</span><span class="sxs-lookup"><span data-stu-id="e2d51-214">Don't include the app's executable in the path.</span></span> <span data-ttu-id="e2d51-215">Aucune barre oblique de fin n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e2d51-215">A trailing slash isn't required.</span></span>
* <span data-ttu-id="e2d51-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Compte d’utilisateur du service (par exemple, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="e2d51-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="e2d51-217">`{NAME}` &ndash; Nom du service (par exemple, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="e2d51-217">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="e2d51-218">`{EXE FILE PATH}` &ndash; Chemin du fichier exécutable de l’application (par exemple, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="e2d51-218">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="e2d51-219">Incluez le nom de fichier de l’exécutable avec l’extension.</span><span class="sxs-lookup"><span data-stu-id="e2d51-219">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="e2d51-220">`{DESCRIPTION}` &ndash; Description du service (par exemple, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="e2d51-220">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="e2d51-221">`{DISPLAY NAME}` &ndash; Nom d’affichage du service (par exemple, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="e2d51-221">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="e2d51-222">Démarrer un service</span><span class="sxs-lookup"><span data-stu-id="e2d51-222">Start a service</span></span>

<span data-ttu-id="e2d51-223">Démarrez un service avec la commande PowerShell 6 suivante :</span><span class="sxs-lookup"><span data-stu-id="e2d51-223">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="e2d51-224">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="e2d51-224">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="e2d51-225">Déterminer l’état d’un service</span><span class="sxs-lookup"><span data-stu-id="e2d51-225">Determine a service's status</span></span>

<span data-ttu-id="e2d51-226">Pour vérifier l’état d’un service, utilisez la commande PowerShell 6 suivante :</span><span class="sxs-lookup"><span data-stu-id="e2d51-226">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="e2d51-227">L’état est signalé comme étant l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2d51-227">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="e2d51-228">Arrêter un service</span><span class="sxs-lookup"><span data-stu-id="e2d51-228">Stop a service</span></span>

<span data-ttu-id="e2d51-229">Arrêtez un service avec la commande Powershell 6 suivante :</span><span class="sxs-lookup"><span data-stu-id="e2d51-229">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="e2d51-230">Supprimer un service</span><span class="sxs-lookup"><span data-stu-id="e2d51-230">Remove a service</span></span>

<span data-ttu-id="e2d51-231">Après un court délai pour arrêter un service, supprimez-le avec la commande PowerShell 6 suivante :</span><span class="sxs-lookup"><span data-stu-id="e2d51-231">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="e2d51-232">Gérer les événements de démarrage et d’arrêt</span><span class="sxs-lookup"><span data-stu-id="e2d51-232">Handle starting and stopping events</span></span>

<span data-ttu-id="e2d51-233">Pour gérer les événements <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> et <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="e2d51-233">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="e2d51-234">Créez une classe qui dérive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> à l’aide des méthodes `OnStarting`, `OnStarted` et `OnStopping` :</span><span class="sxs-lookup"><span data-stu-id="e2d51-234">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="e2d51-235">Créez une méthode d’extension pour <xref:Microsoft.AspNetCore.Hosting.IWebHost> qui transmet `CustomWebHostService` à <xref:System.ServiceProcess.ServiceBase.Run*> :</span><span class="sxs-lookup"><span data-stu-id="e2d51-235">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="e2d51-236">Dans `Program.Main`, appelez la méthode d’extension `RunAsCustomService` au lieu de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> :</span><span class="sxs-lookup"><span data-stu-id="e2d51-236">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="e2d51-237">Pour afficher l’emplacement de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> dans `Program.Main`, reportez-vous à l’exemple de code indiqué dans la section [Type de déploiement](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="e2d51-237">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="e2d51-238">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="e2d51-238">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="e2d51-239">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="e2d51-239">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="e2d51-240">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="e2d51-240">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="e2d51-241">Configuration des points de terminaison</span><span class="sxs-lookup"><span data-stu-id="e2d51-241">Configure endpoints</span></span>

<span data-ttu-id="e2d51-242">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e2d51-242">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="e2d51-243">Configurez l’URL et le port `ASPNETCORE_URLS` en définissant la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e2d51-243">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="e2d51-244">Pour obtenir d’autres approches de configuration des ports et des URL, notamment la prise en charge des points de terminaison HTTPs, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2d51-244">For additional URL and port configuration approaches, including support for HTTPS endpoints, see the following topics:</span></span>

* <span data-ttu-id="e2d51-245"><xref:fundamentals/servers/kestrel#endpoint-configuration>Kestrel</span><span class="sxs-lookup"><span data-stu-id="e2d51-245"><xref:fundamentals/servers/kestrel#endpoint-configuration> (Kestrel)</span></span>
* <span data-ttu-id="e2d51-246"><xref:fundamentals/servers/httpsys#configure-windows-server>(HTTP. sys)</span><span class="sxs-lookup"><span data-stu-id="e2d51-246"><xref:fundamentals/servers/httpsys#configure-windows-server> (HTTP.sys)</span></span>

> [!NOTE]
> <span data-ttu-id="e2d51-247">L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="e2d51-247">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="e2d51-248">Répertoire actif et racine du contenu</span><span class="sxs-lookup"><span data-stu-id="e2d51-248">Current directory and content root</span></span>

<span data-ttu-id="e2d51-249">Le répertoire de travail actif retourné par l’appel à <xref:System.IO.Directory.GetCurrentDirectory*> pour un service Windows est le dossier *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="e2d51-249">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="e2d51-250">Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres).</span><span class="sxs-lookup"><span data-stu-id="e2d51-250">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="e2d51-251">Utilisez une des approches suivantes pour gérer les ressources ainsi que les fichiers de paramètres d’un service, et y accéder.</span><span class="sxs-lookup"><span data-stu-id="e2d51-251">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="e2d51-252">Utiliser ContentRootPath ou ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="e2d51-252">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="e2d51-253">Utilisez les éléments [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) ou <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> pour localiser les ressources d’une application.</span><span class="sxs-lookup"><span data-stu-id="e2d51-253">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="e2d51-254">Définir le dossier de l’application comme chemin d’accès racine du contenu</span><span class="sxs-lookup"><span data-stu-id="e2d51-254">Set the content root path to the app's folder</span></span>

<span data-ttu-id="e2d51-255">La chaîne <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> correspond au même chemin que celui fourni à l’argument `binPath` lorsqu’un service est créé.</span><span class="sxs-lookup"><span data-stu-id="e2d51-255">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="e2d51-256">Au lieu d’appeler `GetCurrentDirectory` pour créer des chemins d’accès aux fichiers de paramètres, appelez <xref:System.IO.Directory.SetCurrentDirectory*> en utilisant le chemin d’accès à la racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2d51-256">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="e2d51-257">Dans `Program.Main`, définissez le chemin d’accès au dossier du fichier exécutable du service ainsi que le chemin d’accès pour établir la racine du contenu de l’application :</span><span class="sxs-lookup"><span data-stu-id="e2d51-257">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="e2d51-258">Stocker les fichiers d’un service dans un emplacement approprié sur le disque</span><span class="sxs-lookup"><span data-stu-id="e2d51-258">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="e2d51-259">Spécifiez un chemin absolu avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>, si vous utilisez <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, vers le dossier contenant les fichiers.</span><span class="sxs-lookup"><span data-stu-id="e2d51-259">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2d51-260">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e2d51-260">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="e2d51-261">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="e2d51-261">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="e2d51-262">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="e2d51-262">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
