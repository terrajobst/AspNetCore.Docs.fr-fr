---
title: Héberger ASP.NET Core dans un service Windows
author: rick-anderson
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153527"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="58e93-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="58e93-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="58e93-104">Par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="58e93-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="58e93-105">La méthode recommandée pour héberger une application ASP.NET Core sur Windows sans utiliser IIS consiste à l’exécuter dans un [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="58e93-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="58e93-106">Quand elle est hébergée en tant que service Windows, l’application peut automatiquement démarrer après les redémarrages et les plantages sans nécessiter une intervention humaine.</span><span class="sxs-lookup"><span data-stu-id="58e93-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="58e93-107">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="58e93-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="58e93-108">Pour obtenir des instructions sur la façon d’exécuter l’exemple d’application, consultez le fichier *README.md* de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="58e93-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58e93-109">Prérequis</span><span class="sxs-lookup"><span data-stu-id="58e93-109">Prerequisites</span></span>

* <span data-ttu-id="58e93-110">L’application doit s’exécuter sur le runtime .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="58e93-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="58e93-111">Dans le fichier *.csproj*, spécifiez des valeurs appropriées pour [TargetFramework](/nuget/schema/target-frameworks) et [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="58e93-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="58e93-112">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="58e93-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="58e93-113">Quand vous créez un projet dans Visual Studio, utilisez le modèle **Application ASP.NET Core (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="58e93-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="58e93-114">Si l’application reçoit des requêtes à partir d’Internet (pas uniquement à partir d’un réseau interne), elle doit utiliser le serveur web [HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener) pour les applications ASP.NET Core 1.x) au lieu de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="58e93-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="58e93-115">Nous vous recommandons d’utiliser IIS comme serveur proxy inverse avec Kestrel pour les déploiements de périphérie.</span><span class="sxs-lookup"><span data-stu-id="58e93-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="58e93-116">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="58e93-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="58e93-117">Prise en main</span><span class="sxs-lookup"><span data-stu-id="58e93-117">Get started</span></span>

<span data-ttu-id="58e93-118">Cette section explique les modifications minimales requises pour configurer un projet ASP.NET Core existant afin qu’il s’exécute dans un service.</span><span class="sxs-lookup"><span data-stu-id="58e93-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="58e93-119">Installez le package NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="58e93-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="58e93-120">Dans `Program.Main`, effectuez les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="58e93-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="58e93-121">Appelez `host.RunAsService` à la place de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="58e93-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="58e93-122">Si le code appelle `UseContentRoot`, utilisez un chemin de l’emplacement de publication au lieu de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="58e93-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58e93-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="58e93-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58e93-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="58e93-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. <span data-ttu-id="58e93-125">Publiez l’application sur un dossier.</span><span class="sxs-lookup"><span data-stu-id="58e93-125">Publish the app to a folder.</span></span> <span data-ttu-id="58e93-126">Utilisez [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) qui publie sur un dossier.</span><span class="sxs-lookup"><span data-stu-id="58e93-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="58e93-127">Effectuez un test en créant et en démarrant le service.</span><span class="sxs-lookup"><span data-stu-id="58e93-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="58e93-128">Ouvrez un interpréteur de commandes avec des privilèges administratifs afin de créer et de démarrer un service à l’aide de l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995).</span><span class="sxs-lookup"><span data-stu-id="58e93-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="58e93-129">Si le service est créé sous le nom MyService, qu’il est publié sur `c:\svc` et que son nom est AspNetCoreService, les commandes sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="58e93-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="58e93-130">La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable.</span><span class="sxs-lookup"><span data-stu-id="58e93-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Exemple de la fenêtre de la console de création et de démarrage](windows-service/_static/create-start.png)

   <span data-ttu-id="58e93-132">Une fois ces commandes exécutées, accédez au chemin correspondant à l’exécution en tant qu’application console (par défaut, `http://localhost:5000`) :</span><span class="sxs-lookup"><span data-stu-id="58e93-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Exécution dans un service](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="58e93-134">Fournir un moyen d’exécution en dehors d’un service</span><span class="sxs-lookup"><span data-stu-id="58e93-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="58e93-135">Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions.</span><span class="sxs-lookup"><span data-stu-id="58e93-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="58e93-136">Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :</span><span class="sxs-lookup"><span data-stu-id="58e93-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58e93-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="58e93-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58e93-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="58e93-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="58e93-139">Gérer les événements d’arrêt et de démarrage</span><span class="sxs-lookup"><span data-stu-id="58e93-139">Handle stopping and starting events</span></span>

<span data-ttu-id="58e93-140">Pour gérer les événements `OnStarting`, `OnStarted` et `OnStopping`, apportez les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="58e93-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="58e93-141">Créez une classe qui dérive de `WebHostService` :</span><span class="sxs-lookup"><span data-stu-id="58e93-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="58e93-142">Créez une méthode d’extension pour `IWebHost` qui transmet le `WebHostService` personnalisé à `ServiceBase.Run` :</span><span class="sxs-lookup"><span data-stu-id="58e93-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="58e93-143">Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de `RunAsService` :</span><span class="sxs-lookup"><span data-stu-id="58e93-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58e93-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="58e93-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58e93-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="58e93-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="58e93-146">Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété `Services` d’`IWebHost` :</span><span class="sxs-lookup"><span data-stu-id="58e93-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="58e93-147">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="58e93-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="58e93-148">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="58e93-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="58e93-149">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="58e93-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="58e93-150">Remerciements</span><span class="sxs-lookup"><span data-stu-id="58e93-150">Acknowledgments</span></span>

<span data-ttu-id="58e93-151">Cet article a été écrit à l’aide de sources publiées :</span><span class="sxs-lookup"><span data-stu-id="58e93-151">This article was written with the help of published sources:</span></span>

* <span data-ttu-id="58e93-152">[Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) (Hébergement d’ASP.NET Core en tant que service Windows)</span><span class="sxs-lookup"><span data-stu-id="58e93-152">[Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)</span></span>
* <span data-ttu-id="58e93-153">[How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) (Guide pratique pour héberger ASP.NET Core dans un service Windows)</span><span class="sxs-lookup"><span data-stu-id="58e93-153">[How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)</span></span>
