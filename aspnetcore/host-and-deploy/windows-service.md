---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 0149039f69539b7c69d7ba45efcf09d80ffcba79
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275096"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="75b9c-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="75b9c-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="75b9c-104">Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="75b9c-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="75b9c-105">Une application ASP.NET Core peut être hébergée sur Windows sans utiliser IIS en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="75b9c-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="75b9c-106">Quand elle est hébergée en tant que service Windows, l’application peut automatiquement démarrer après les redémarrages et les plantages sans nécessiter une intervention humaine.</span><span class="sxs-lookup"><span data-stu-id="75b9c-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="75b9c-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75b9c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="75b9c-108">Prise en main</span><span class="sxs-lookup"><span data-stu-id="75b9c-108">Get started</span></span>

<span data-ttu-id="75b9c-109">Les modifications minimales suivantes sont requises pour configurer un projet ASP.NET Core existant afin qu’il s’exécute dans un service :</span><span class="sxs-lookup"><span data-stu-id="75b9c-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="75b9c-110">Dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="75b9c-110">In the project file:</span></span>

   1. <span data-ttu-id="75b9c-111">Vérifiez la présence de l’identificateur de runtime ou ajoutez-le à la section **\<PropertyGroup>** qui contient la version cible du .Net Framework :</span><span class="sxs-lookup"><span data-stu-id="75b9c-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="75b9c-112">Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="75b9c-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="75b9c-113">Dans `Program.Main`, effectuez les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="75b9c-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="75b9c-114">Appelez [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) au lieu de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="75b9c-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="75b9c-115">Si le code appelle `UseContentRoot`, utilisez un chemin de l’emplacement publié de l’application au lieu de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="75b9c-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     <span data-ttu-id="75b9c-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span><span class="sxs-lookup"><span data-stu-id="75b9c-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span></span>

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     <span data-ttu-id="75b9c-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span><span class="sxs-lookup"><span data-stu-id="75b9c-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span></span>

     ::: moniker-end

1. <span data-ttu-id="75b9c-118">Publiez l’application sur un dossier.</span><span class="sxs-lookup"><span data-stu-id="75b9c-118">Publish the app to a folder.</span></span> <span data-ttu-id="75b9c-119">Utilisez [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) qui publie sur un dossier.</span><span class="sxs-lookup"><span data-stu-id="75b9c-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

   <span data-ttu-id="75b9c-120">Pour publier l’exemple d’application à partir de la ligne de commande, exécutez la commande suivante dans une fenêtre de console à partir du dossier de projet :</span><span class="sxs-lookup"><span data-stu-id="75b9c-120">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. <span data-ttu-id="75b9c-121">Utilisez l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995) pour créer le service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span><span class="sxs-lookup"><span data-stu-id="75b9c-121">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span></span> <span data-ttu-id="75b9c-122">La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable.</span><span class="sxs-lookup"><span data-stu-id="75b9c-122">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="75b9c-123">**L’espace entre le signe égal et le caractère guillemet qui commence le chemin est requis.**</span><span class="sxs-lookup"><span data-stu-id="75b9c-123">**The space between the equal sign and the quote character that starts the path is required.**</span></span>

   <span data-ttu-id="75b9c-124">Pour l’exemple d’application et de commande qui suit, le service :</span><span class="sxs-lookup"><span data-stu-id="75b9c-124">For the sample app and command that follows, the service is:</span></span>

   * <span data-ttu-id="75b9c-125">S’appelle **MyService**.</span><span class="sxs-lookup"><span data-stu-id="75b9c-125">Named **MyService**.</span></span>
   * <span data-ttu-id="75b9c-126">Est publié dans le dossier *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="75b9c-126">Published to *c:\\svc* folder.</span></span>
   * <span data-ttu-id="75b9c-127">A un exécutable d’application nommé *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="75b9c-127">Has an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="75b9c-128">Ouvrez une invite de commandes avec des privilèges administratifs et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="75b9c-128">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   <span data-ttu-id="75b9c-129">**Vérifiez que l’espace est présent entre l’argument `binPath=` et sa valeur.**</span><span class="sxs-lookup"><span data-stu-id="75b9c-129">**Make sure the space is present between the `binPath=` argument and its value.**</span></span>

1. <span data-ttu-id="75b9c-130">Démarrez le service avec la commande `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="75b9c-130">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="75b9c-131">Pour démarrer l’exemple de service d’application, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="75b9c-131">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="75b9c-132">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="75b9c-132">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="75b9c-133">La commande `sc query <SERVICE_NAME>` permet de déterminer l’état du service :</span><span class="sxs-lookup"><span data-stu-id="75b9c-133">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="75b9c-134">Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="75b9c-134">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="75b9c-135">Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).</span><span class="sxs-lookup"><span data-stu-id="75b9c-135">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="75b9c-136">Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="75b9c-136">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="75b9c-137">Arrêtez le service avec la commande `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="75b9c-137">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="75b9c-138">La commande suivante arrête l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="75b9c-138">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="75b9c-139">Après un court délai pour arrêter un service, désinstallez le service avec la commande `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="75b9c-139">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="75b9c-140">Vérifiez l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="75b9c-140">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="75b9c-141">Quand l’exemple de service d’application est dans l’état `STOPPED`, utilisez la commande suivante pour le désinstaller :</span><span class="sxs-lookup"><span data-stu-id="75b9c-141">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="75b9c-142">Fournir un moyen d’exécution en dehors d’un service</span><span class="sxs-lookup"><span data-stu-id="75b9c-142">Provide a way to run outside of a service</span></span>

<span data-ttu-id="75b9c-143">Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions.</span><span class="sxs-lookup"><span data-stu-id="75b9c-143">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="75b9c-144">Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :</span><span class="sxs-lookup"><span data-stu-id="75b9c-144">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75b9c-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="75b9c-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span></span>

<span data-ttu-id="75b9c-146">Étant donné que la configuration d’ASP.NET Core nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé avant que les arguments ne soient passés à [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="75b9c-146">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75b9c-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="75b9c-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span></span>

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="75b9c-148">Gérer les événements d’arrêt et de démarrage</span><span class="sxs-lookup"><span data-stu-id="75b9c-148">Handle stopping and starting events</span></span>

<span data-ttu-id="75b9c-149">Pour gérer les événements [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) et [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportez les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="75b9c-149">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="75b9c-150">Créez une classe qui dérive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) :</span><span class="sxs-lookup"><span data-stu-id="75b9c-150">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="75b9c-151">Créez une méthode d’extension pour [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) qui transmet le `WebHostService` personnalisé à [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) :</span><span class="sxs-lookup"><span data-stu-id="75b9c-151">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="75b9c-152">Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) :</span><span class="sxs-lookup"><span data-stu-id="75b9c-152">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   <span data-ttu-id="75b9c-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="75b9c-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   <span data-ttu-id="75b9c-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="75b9c-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

<span data-ttu-id="75b9c-155">Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) :</span><span class="sxs-lookup"><span data-stu-id="75b9c-155">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="75b9c-156">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="75b9c-156">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="75b9c-157">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="75b9c-157">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="75b9c-158">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="75b9c-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="75b9c-159">Configuration de point de terminaison Kestrel</span><span class="sxs-lookup"><span data-stu-id="75b9c-159">Kestrel endpoint configuration</span></span>

<span data-ttu-id="75b9c-160">Pour plus d’informations sur la configuration de point de terminaison Kestrel, notamment sur la configuration de HTTPS et la prise en charge de SNI, consultez [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="75b9c-160">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
