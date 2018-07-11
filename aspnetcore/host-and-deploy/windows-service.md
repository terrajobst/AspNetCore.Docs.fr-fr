---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: bce09a500160f0bf13926786d277f8b1e88c1bf8
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894255"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="04315-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="04315-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="04315-104">Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="04315-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="04315-105">Une application ASP.NET Core peut être hébergée sur Windows sans utiliser IIS en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="04315-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="04315-106">Quand elle est hébergée en tant que service Windows, l’application peut automatiquement démarrer après les redémarrages et les plantages sans nécessiter une intervention humaine.</span><span class="sxs-lookup"><span data-stu-id="04315-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="04315-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="04315-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="04315-108">Prise en main</span><span class="sxs-lookup"><span data-stu-id="04315-108">Get started</span></span>

<span data-ttu-id="04315-109">Les modifications minimales suivantes sont requises pour configurer un projet ASP.NET Core existant afin qu’il s’exécute dans un service :</span><span class="sxs-lookup"><span data-stu-id="04315-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="04315-110">Dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="04315-110">In the project file:</span></span>

   1. <span data-ttu-id="04315-111">Vérifiez la présence de l’identificateur de runtime ou ajoutez-le à la section **\<PropertyGroup>** qui contient la version cible du .Net Framework :</span><span class="sxs-lookup"><span data-stu-id="04315-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="04315-112">Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="04315-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="04315-113">Dans `Program.Main`, effectuez les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="04315-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="04315-114">Appelez [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) au lieu de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="04315-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="04315-115">Appelez [UseContentRoot](xref:fundamentals/host/web-host#content-root) et utilisez un chemin vers l’emplacement publié de l’application au lieu de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="04315-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="04315-116">Publiez l’application.</span><span class="sxs-lookup"><span data-stu-id="04315-116">Publish the app.</span></span> <span data-ttu-id="04315-117">Utilisez [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="04315-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="04315-118">Pour publier l’exemple d’application à partir de la ligne de commande, exécutez la commande suivante dans une fenêtre de console à partir du dossier de projet :</span><span class="sxs-lookup"><span data-stu-id="04315-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="04315-119">Utilisez l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995) pour créer le service.</span><span class="sxs-lookup"><span data-stu-id="04315-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="04315-120">La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable.</span><span class="sxs-lookup"><span data-stu-id="04315-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="04315-121">**L’espace entre le signe égal et le guillemet au début du chemin est obligatoire.**</span><span class="sxs-lookup"><span data-stu-id="04315-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="04315-122">Pour un service publié dans le dossier du projet, utilisez le chemin du dossier *publish* pour créer le service.</span><span class="sxs-lookup"><span data-stu-id="04315-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="04315-123">Dans l’exemple suivant, le service est :</span><span class="sxs-lookup"><span data-stu-id="04315-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="04315-124">S’appelle **MyService**.</span><span class="sxs-lookup"><span data-stu-id="04315-124">Named **MyService**.</span></span>
   * <span data-ttu-id="04315-125">Publié dans le dossier *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;FRAMEWORK_CIBLE&gt;\\publish*.</span><span class="sxs-lookup"><span data-stu-id="04315-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="04315-126">Représenté par un exécutable d’application nommé *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="04315-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="04315-127">Ouvrez une invite de commandes avec des privilèges administratifs et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="04315-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\aspnetcoreservice\bin\release\<TARGET_FRAMEWORK>\publish\aspnetcoreservice.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="04315-128">Vérifiez que l’espace est présent entre l’argument `binPath=` et sa valeur.</span><span class="sxs-lookup"><span data-stu-id="04315-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="04315-129">Pour publier et démarrer le service à partir d’un autre dossier :</span><span class="sxs-lookup"><span data-stu-id="04315-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="04315-130">Utilisez l’option [--output &lt;RÉPERTOIRE_SORTIE&gt;](/dotnet/core/tools/dotnet-publish#options) dans la commande `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="04315-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="04315-131">Créez le service avec la commande `sc.exe` en utilisant le chemin du dossier de sortie.</span><span class="sxs-lookup"><span data-stu-id="04315-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="04315-132">Ajoutez le nom de l’exécutable du service dans le chemin fourni dans `binPath`.</span><span class="sxs-lookup"><span data-stu-id="04315-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="04315-133">Démarrez le service avec la commande `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="04315-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="04315-134">Pour démarrer l’exemple de service d’application, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="04315-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="04315-135">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="04315-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="04315-136">La commande `sc query <SERVICE_NAME>` permet de déterminer l’état du service :</span><span class="sxs-lookup"><span data-stu-id="04315-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="04315-137">Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="04315-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="04315-138">Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).</span><span class="sxs-lookup"><span data-stu-id="04315-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="04315-139">Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="04315-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="04315-140">Arrêtez le service avec la commande `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="04315-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="04315-141">La commande suivante arrête l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="04315-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="04315-142">Après un court délai pour arrêter un service, désinstallez le service avec la commande `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="04315-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="04315-143">Vérifiez l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="04315-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="04315-144">Quand l’exemple de service d’application est dans l’état `STOPPED`, utilisez la commande suivante pour le désinstaller :</span><span class="sxs-lookup"><span data-stu-id="04315-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="04315-145">Fournir un moyen d’exécution en dehors d’un service</span><span class="sxs-lookup"><span data-stu-id="04315-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="04315-146">Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions.</span><span class="sxs-lookup"><span data-stu-id="04315-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="04315-147">Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :</span><span class="sxs-lookup"><span data-stu-id="04315-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="04315-148">Étant donné que la configuration d’ASP.NET Core nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé avant que les arguments ne soient passés à [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="04315-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="04315-149">Gérer les événements d’arrêt et de démarrage</span><span class="sxs-lookup"><span data-stu-id="04315-149">Handle stopping and starting events</span></span>

<span data-ttu-id="04315-150">Pour gérer les événements [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) et [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportez les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="04315-150">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="04315-151">Créez une classe qui dérive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) :</span><span class="sxs-lookup"><span data-stu-id="04315-151">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="04315-152">Créez une méthode d’extension pour [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) qui transmet le `WebHostService` personnalisé à [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) :</span><span class="sxs-lookup"><span data-stu-id="04315-152">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="04315-153">Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) :</span><span class="sxs-lookup"><span data-stu-id="04315-153">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="04315-154">Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) :</span><span class="sxs-lookup"><span data-stu-id="04315-154">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="04315-155">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="04315-155">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="04315-156">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="04315-156">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="04315-157">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="04315-157">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="04315-158">Configuration de point de terminaison Kestrel</span><span class="sxs-lookup"><span data-stu-id="04315-158">Kestrel endpoint configuration</span></span>

<span data-ttu-id="04315-159">Pour plus d’informations sur la configuration de point de terminaison Kestrel, notamment sur la configuration de HTTPS et la prise en charge de SNI, consultez [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="04315-159">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
