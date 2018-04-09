---
title: Hôte ASP.NET Core dans un Service Windows
author: tdykstra
description: Découvrez comment héberger une application ASP.NET Core dans un Service Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: b0b27f274de1ca88b20bf582127132527b553ce0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="d704e-103">Hôte ASP.NET Core dans un Service Windows</span><span class="sxs-lookup"><span data-stu-id="d704e-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="d704e-104">Par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d704e-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d704e-105">La méthode recommandée pour héberger une application ASP.NET Core sur Windows sans l’aide d’IIS est exécuté un [Service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="d704e-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="d704e-106">Lorsqu’il est hébergé comme un Service Windows, l’application peut automatiquement démarrer après redémarrage et tombe en panne sans nécessiter une intervention humaine.</span><span class="sxs-lookup"><span data-stu-id="d704e-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="d704e-107">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d704e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d704e-108">Pour obtenir des instructions sur la façon d’exécuter l’exemple d’application, consultez l’exemple *README.md* fichier.</span><span class="sxs-lookup"><span data-stu-id="d704e-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d704e-109">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d704e-109">Prerequisites</span></span>

* <span data-ttu-id="d704e-110">L’application doit s’exécuter sur le runtime .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d704e-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="d704e-111">Dans le *.csproj* de fichiers, spécifiez les valeurs appropriées pour [TargetFramework](/nuget/schema/target-frameworks) et [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="d704e-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="d704e-112">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="d704e-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="d704e-113">Lorsque vous créez un projet dans Visual Studio, utilisez le **ASP.NET Core Application (.NET Framework)** modèle.</span><span class="sxs-lookup"><span data-stu-id="d704e-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="d704e-114">Si l’application reçoit des demandes à partir d’Internet (pas uniquement à partir d’un réseau interne), elle doit utiliser le [HTTP.sys](xref:fundamentals/servers/httpsys) serveur web (anciennement [WebListener](xref:fundamentals/servers/weblistener) pour les applications ASP.NET Core 1.x) au lieu de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d704e-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d704e-115">IIS est recommandé d’utiliser comme un serveur proxy inverse avec Kestrel pour les déploiements de bord.</span><span class="sxs-lookup"><span data-stu-id="d704e-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="d704e-116">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d704e-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="d704e-117">Prise en main</span><span class="sxs-lookup"><span data-stu-id="d704e-117">Get started</span></span>

<span data-ttu-id="d704e-118">Cette section explique les modifications minimales requises pour configurer un projet ASP.NET Core existant pour s’exécuter dans un service.</span><span class="sxs-lookup"><span data-stu-id="d704e-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="d704e-119">Installez le package NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="d704e-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="d704e-120">Apportez les modifications suivantes dans `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="d704e-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="d704e-121">Appelez `host.RunAsService` au lieu de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="d704e-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="d704e-122">Si le code appelle `UseContentRoot`, utilisez un chemin d’accès à l’emplacement de publication au lieu de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="d704e-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d704e-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d704e-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d704e-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d704e-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   * * *

3. <span data-ttu-id="d704e-125">Publier l’application dans un dossier.</span><span class="sxs-lookup"><span data-stu-id="d704e-125">Publish the app to a folder.</span></span> <span data-ttu-id="d704e-126">Utilisez [dotnet publier](/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) qui publie vers un dossier.</span><span class="sxs-lookup"><span data-stu-id="d704e-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="d704e-127">Testez en créant et en démarrant le service.</span><span class="sxs-lookup"><span data-stu-id="d704e-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="d704e-128">Ouvrez une invite de commandes avec des privilèges d’administrateur pour utiliser le [sc.exe](https://technet.microsoft.com/library/bb490995) outil de ligne de commande pour créer et démarrer un service.</span><span class="sxs-lookup"><span data-stu-id="d704e-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="d704e-129">Si le service se nomme MyService, publié sur `c:\svc`, et nommé AspNetCoreService, les commandes sont :</span><span class="sxs-lookup"><span data-stu-id="d704e-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="d704e-130">Le `binPath` valeur est le chemin d’accès au fichier exécutable de l’application, qui inclut le nom du fichier exécutable.</span><span class="sxs-lookup"><span data-stu-id="d704e-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Fenêtre de console créer et démarrer l’exemple](windows-service/_static/create-start.png)

   <span data-ttu-id="d704e-132">Lors de la fin de ces commandes, recherchez le même chemin que lors de l’exécution en tant qu’application console (par défaut, `http://localhost:5000`) :</span><span class="sxs-lookup"><span data-stu-id="d704e-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![En cours d’exécution dans un service](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="d704e-134">Fournir un moyen d’exécuter en dehors d’un service</span><span class="sxs-lookup"><span data-stu-id="d704e-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="d704e-135">Il est plus facile tester et déboguer lors de l’exécution en dehors d’un service, il est habituel pour ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions.</span><span class="sxs-lookup"><span data-stu-id="d704e-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="d704e-136">Par exemple, l’application peut s’exécuter comme une application de console avec une `--console` argument de ligne de commande ou si le débogueur est attaché :</span><span class="sxs-lookup"><span data-stu-id="d704e-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d704e-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d704e-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d704e-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d704e-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

* * *
## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="d704e-139">Gérer les événements de démarrage et arrêt</span><span class="sxs-lookup"><span data-stu-id="d704e-139">Handle stopping and starting events</span></span>

<span data-ttu-id="d704e-140">Pour gérer les `OnStarting`, `OnStarted`, et `OnStopping` événements, apporter les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="d704e-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="d704e-141">Créez une classe qui dérive de `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="d704e-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="d704e-142">Créer une méthode d’extension pour `IWebHost` qui transmet personnalisé `WebHostService` à `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="d704e-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="d704e-143">Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="d704e-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d704e-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d704e-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d704e-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d704e-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   * * *
<span data-ttu-id="d704e-146">Si personnalisé `WebHostService` code nécessite un service à partir de l’injection de dépendances (par exemple, un journal), l’obtenir à partir de la `Services` propriété du `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="d704e-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d704e-147">Serveur proxy et les scénarios d’équilibrage de charge</span><span class="sxs-lookup"><span data-stu-id="d704e-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d704e-148">Les services qui interagissent avec les demandes provenant d’Internet ou d’un réseau d’entreprise et qui sont trouvent derrière un proxy ou l’équilibrage de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="d704e-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="d704e-149">Pour plus d’informations, consultez [configurer ASP.NET Core pour travailler avec des serveurs proxy et des équilibrages de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="d704e-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="d704e-150">Remerciements</span><span class="sxs-lookup"><span data-stu-id="d704e-150">Acknowledgments</span></span>

<span data-ttu-id="d704e-151">Cet article a été écrite à l’aide de sources publiés :</span><span class="sxs-lookup"><span data-stu-id="d704e-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="d704e-152">Hébergement ASP.NET Core en tant que service Windows</span><span class="sxs-lookup"><span data-stu-id="d704e-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="d704e-153">L’hébergement de base ASP.NET dans un Service Windows</span><span class="sxs-lookup"><span data-stu-id="d704e-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
