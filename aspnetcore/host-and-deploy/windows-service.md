---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 68afe77b05a717cffecc32188f18e9fde208b81f
ms.sourcegitcommit: 3ca20ed63bf1469f4365f0c1fbd00c98a3191c84
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2018
ms.locfileid: "41751657"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="40256-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="40256-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="40256-104">Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="40256-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="40256-105">Une application ASP.NET Core peut être hébergée sur Windows sans utiliser IIS en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="40256-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="40256-106">Quand elle est hébergée en tant que service Windows, l’application peut automatiquement démarrer après les redémarrages et les plantages sans nécessiter une intervention humaine.</span><span class="sxs-lookup"><span data-stu-id="40256-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="40256-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40256-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="40256-108">Prise en main</span><span class="sxs-lookup"><span data-stu-id="40256-108">Get started</span></span>

<span data-ttu-id="40256-109">Les modifications minimales suivantes sont requises pour configurer un projet ASP.NET Core existant afin qu’il s’exécute dans un service :</span><span class="sxs-lookup"><span data-stu-id="40256-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="40256-110">Dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="40256-110">In the project file:</span></span>

   1. <span data-ttu-id="40256-111">Vérifiez la présence de l’identificateur de runtime ou ajoutez-le à la section **\<PropertyGroup>** qui contient la version cible du .Net Framework :</span><span class="sxs-lookup"><span data-stu-id="40256-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. <span data-ttu-id="40256-112">Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="40256-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="40256-113">Dans `Program.Main`, effectuez les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="40256-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="40256-114">Appelez [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) au lieu de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="40256-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="40256-115">Appelez [UseContentRoot](xref:fundamentals/host/web-host#content-root) et utilisez un chemin vers l’emplacement publié de l’application au lieu de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="40256-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="40256-116">Publiez l’application.</span><span class="sxs-lookup"><span data-stu-id="40256-116">Publish the app.</span></span> <span data-ttu-id="40256-117">Utilisez [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="40256-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="40256-118">Si vous utilisez Visual Studio, sélectionnez **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="40256-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="40256-119">Pour publier l’exemple d’application à partir de la ligne de commande, exécutez la commande suivante dans une fenêtre de console à partir du dossier de projet :</span><span class="sxs-lookup"><span data-stu-id="40256-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="40256-120">Utilisez l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995) pour créer le service.</span><span class="sxs-lookup"><span data-stu-id="40256-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="40256-121">La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable.</span><span class="sxs-lookup"><span data-stu-id="40256-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="40256-122">**L’espace entre le signe égal et le guillemet au début du chemin est obligatoire.**</span><span class="sxs-lookup"><span data-stu-id="40256-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="40256-123">Pour un service publié dans le dossier du projet, utilisez le chemin du dossier *publish* pour créer le service.</span><span class="sxs-lookup"><span data-stu-id="40256-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="40256-124">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="40256-124">In the following example:</span></span>

   * <span data-ttu-id="40256-125">Le projet se trouve dans le dossier `c:\my_services\AspNetCoreService`.</span><span class="sxs-lookup"><span data-stu-id="40256-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="40256-126">Le projet est publié dans la configuration `Release`.</span><span class="sxs-lookup"><span data-stu-id="40256-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="40256-127">Le moniker de framework cible (TFM) est `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="40256-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="40256-128">L’identificateur du runtime (RID) est `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="40256-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="40256-129">L’exécutable de l’application s’appelle *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="40256-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="40256-130">Le service s’appelle **MyService**.</span><span class="sxs-lookup"><span data-stu-id="40256-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="40256-131">Exemple :</span><span class="sxs-lookup"><span data-stu-id="40256-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="40256-132">Vérifiez que l’espace est présent entre l’argument `binPath=` et sa valeur.</span><span class="sxs-lookup"><span data-stu-id="40256-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="40256-133">Pour publier et démarrer le service à partir d’un autre dossier :</span><span class="sxs-lookup"><span data-stu-id="40256-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="40256-134">Utilisez l’option [--output &lt;RÉPERTOIRE_SORTIE&gt;](/dotnet/core/tools/dotnet-publish#options) dans la commande `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="40256-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="40256-135">Si vous utilisez Visual Studio, configurez l’**Emplacement cible** dans la page de propriétés de publication **FolderProfile** avant de sélectionner le bouton **Publier**.</span><span class="sxs-lookup"><span data-stu-id="40256-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="40256-136">Créez le service avec la commande `sc.exe` en utilisant le chemin du dossier de sortie.</span><span class="sxs-lookup"><span data-stu-id="40256-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="40256-137">Ajoutez le nom de l’exécutable du service dans le chemin fourni dans `binPath`.</span><span class="sxs-lookup"><span data-stu-id="40256-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="40256-138">Démarrez le service avec la commande `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="40256-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="40256-139">Pour démarrer l’exemple de service d’application, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="40256-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="40256-140">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="40256-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="40256-141">La commande `sc query <SERVICE_NAME>` permet de déterminer l’état du service :</span><span class="sxs-lookup"><span data-stu-id="40256-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="40256-142">Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="40256-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="40256-143">Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).</span><span class="sxs-lookup"><span data-stu-id="40256-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="40256-144">Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="40256-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="40256-145">Arrêtez le service avec la commande `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="40256-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="40256-146">La commande suivante arrête l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="40256-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="40256-147">Après un court délai pour arrêter un service, désinstallez le service avec la commande `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="40256-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="40256-148">Vérifiez l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="40256-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="40256-149">Quand l’exemple de service d’application est dans l’état `STOPPED`, utilisez la commande suivante pour le désinstaller :</span><span class="sxs-lookup"><span data-stu-id="40256-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="40256-150">Fournir un moyen d’exécution en dehors d’un service</span><span class="sxs-lookup"><span data-stu-id="40256-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="40256-151">Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions.</span><span class="sxs-lookup"><span data-stu-id="40256-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="40256-152">Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :</span><span class="sxs-lookup"><span data-stu-id="40256-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="40256-153">Étant donné que la configuration d’ASP.NET Core nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé avant que les arguments ne soient passés à [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="40256-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="40256-154">`isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="40256-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="40256-155">Gérer les événements d’arrêt et de démarrage</span><span class="sxs-lookup"><span data-stu-id="40256-155">Handle stopping and starting events</span></span>

<span data-ttu-id="40256-156">Pour gérer les événements [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) et [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportez les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="40256-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="40256-157">Créez une classe qui dérive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) :</span><span class="sxs-lookup"><span data-stu-id="40256-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="40256-158">Créez une méthode d’extension pour [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) qui transmet le `WebHostService` personnalisé à [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) :</span><span class="sxs-lookup"><span data-stu-id="40256-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="40256-159">Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) :</span><span class="sxs-lookup"><span data-stu-id="40256-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="40256-160">`isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="40256-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="40256-161">Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) :</span><span class="sxs-lookup"><span data-stu-id="40256-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="40256-162">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="40256-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="40256-163">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="40256-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="40256-164">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="40256-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="40256-165">Configurer HTTPS</span><span class="sxs-lookup"><span data-stu-id="40256-165">Configure HTTPS</span></span>

<span data-ttu-id="40256-166">Spécifiez la [configuration de point de terminaison HTTPS d’un serveur Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="40256-166">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="40256-167">Répertoire actif et racine du contenu</span><span class="sxs-lookup"><span data-stu-id="40256-167">Current directory and content root</span></span>

<span data-ttu-id="40256-168">Le répertoire de travail actif retourné par l’appel à `Directory.GetCurrentDirectory()` pour un service Windows est le dossier *C:\WINDOWS\system32*.</span><span class="sxs-lookup"><span data-stu-id="40256-168">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\WINDOWS\system32* folder.</span></span> <span data-ttu-id="40256-169">Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres).</span><span class="sxs-lookup"><span data-stu-id="40256-169">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="40256-170">Adoptez l’une des approches suivantes pour tenir à jour et accéder aux ressources et aux fichiers de paramètres d’un service avec [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) quand vous utilisez un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) :</span><span class="sxs-lookup"><span data-stu-id="40256-170">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="40256-171">Utilisez le chemin racine du contenu.</span><span class="sxs-lookup"><span data-stu-id="40256-171">Use the content root path.</span></span> <span data-ttu-id="40256-172">`IHostingEnvironment.ContentRootPath` est le même chemin que celui fourni à l’argument `binPath` quand le service est créé.</span><span class="sxs-lookup"><span data-stu-id="40256-172">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="40256-173">Au lieu d’utiliser `Directory.GetCurrentDirectory()` pour créer des chemins aux fichiers de paramètres, utilisez le chemin racine du contenu et tenez à jour les fichiers dans la racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="40256-173">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="40256-174">Stockez les fichiers à un emplacement approprié sur le disque.</span><span class="sxs-lookup"><span data-stu-id="40256-174">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="40256-175">Spécifiez un chemin absolu avec `SetBasePath` au dossier contenant les fichiers.</span><span class="sxs-lookup"><span data-stu-id="40256-175">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40256-176">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="40256-176">Additional resources</span></span>

* <span data-ttu-id="40256-177">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="40256-177">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
