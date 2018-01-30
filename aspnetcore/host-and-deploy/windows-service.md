---
title: "Ordinateur hôte dans un Service Windows"
author: tdykstra
description: "Découvrez comment héberger une application ASP.NET Core dans un Service Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="6aa7e-103">Héberger une application ASP.NET Core dans un Service Windows</span><span class="sxs-lookup"><span data-stu-id="6aa7e-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="6aa7e-104">Par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6aa7e-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6aa7e-105">La méthode recommandée pour héberger une application ASP.NET Core sur Windows sans l’aide d’IIS est exécuté un [Service Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="6aa7e-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="6aa7e-106">De cette façon qu’il peut démarrer automatiquement après les redémarrages et les blocages, sans attendre d’une personne pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="6aa7e-107">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6aa7e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="6aa7e-108">Consultez le [étapes](#next-steps) section pour obtenir des instructions sur la façon de l’exécuter.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6aa7e-109">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6aa7e-109">Prerequisites</span></span>

* <span data-ttu-id="6aa7e-110">L’application doit s’exécuter sur le runtime .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="6aa7e-111">Dans le *.csproj* de fichiers, spécifiez les valeurs appropriées pour [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) et [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="6aa7e-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="6aa7e-112">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="6aa7e-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="6aa7e-113">Lorsque vous créez un projet dans Visual Studio, utilisez le **ASP.NET Core Application (.NET Framework)** modèle.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="6aa7e-114">Si l’application reçoit des demandes à partir d’Internet (pas uniquement à partir d’un réseau interne), elle doit utiliser le [WebListener](xref:fundamentals/servers/weblistener) serveur web au lieu de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="6aa7e-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="6aa7e-115">Kestrel doit être utilisé avec IIS pour les déploiements de bord.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="6aa7e-116">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="6aa7e-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="6aa7e-117">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="6aa7e-117">Getting started</span></span>

<span data-ttu-id="6aa7e-118">Cette section explique les modifications minimales requises pour configurer un projet ASP.NET Core existant pour s’exécuter dans un service.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="6aa7e-119">Installez le package NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="6aa7e-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="6aa7e-120">Apportez les modifications suivantes dans `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="6aa7e-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="6aa7e-121">Appelez `host.RunAsService` au lieu de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="6aa7e-122">Si le code appelle `UseContentRoot`, utilisez un chemin d’accès à l’emplacement de publication au lieu de`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="6aa7e-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="6aa7e-123">Publier l’application dans un dossier.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="6aa7e-124">Utilisez [dotnet publier](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) qui publie vers un dossier.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="6aa7e-125">Testez en créant et en démarrant le service.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="6aa7e-126">Ouvrez une fenêtre d’invite de commandes administrateur pour utiliser le [sc.exe](https://technet.microsoft.com/library/bb490995) outil de ligne de commande pour créer et démarrer un service.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="6aa7e-127">Si le service se nomme MyService, publier l’application à `c:\svc`et l’application proprement dite est nommée AspNetCoreService, les commandes ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="6aa7e-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="6aa7e-128">Le `binPath` valeur est le chemin d’accès au fichier exécutable de l’application, y compris le nom de fichier exécutable.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![Fenêtre de console créer et démarrer l’exemple](windows-service/_static/create-start.png)

  <span data-ttu-id="6aa7e-130">Lors de la fin de ces commandes, recherchez le même chemin que lors de l’exécution en tant qu’application console (par défaut, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="6aa7e-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![En cours d’exécution dans un service](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="6aa7e-132">Fournir un moyen d’exécuter en dehors d’un service</span><span class="sxs-lookup"><span data-stu-id="6aa7e-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="6aa7e-133">Il est plus facile tester et déboguer lors de l’exécution en dehors d’un service, il est habituel pour ajouter du code qui appelle `host.RunAsService` uniquement sous certaines conditions.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="6aa7e-134">Par exemple, l’application peut s’exécuter comme une application de console avec une `--console` argument de ligne de commande ou si le débogueur est attaché.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="6aa7e-135">Gérer les événements de démarrage et arrêt</span><span class="sxs-lookup"><span data-stu-id="6aa7e-135">Handle stopping and starting events</span></span>

<span data-ttu-id="6aa7e-136">Pour gérer les `OnStarting`, `OnStarted`, et `OnStopping` événements, apporter les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="6aa7e-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="6aa7e-137">Créez une classe qui dérive de `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="6aa7e-138">Créer une méthode d’extension pour `IWebHost` qui transmet personnalisé `WebHostService` à `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="6aa7e-139">Dans `Program.Main` modification appeler la nouvelle méthode d’extension à la place de `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="6aa7e-140">Si personnalisé `WebHostService` code a besoin obtenir un service à partir de l’injection de dépendances (par exemple, un journal), téléchargez-le à partir de la `Services` propriété du `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="6aa7e-141">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="6aa7e-141">Next steps</span></span>

<span data-ttu-id="6aa7e-142">Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) qui accompagne cette article est une application web MVC simple qui a été modifiée comme indiqué dans les précédents exemples de code.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="6aa7e-143">Pour l’exécuter dans un service, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="6aa7e-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="6aa7e-144">Publier sur *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="6aa7e-145">Ouvrez une fenêtre de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-145">Open an administrator window.</span></span>

* <span data-ttu-id="6aa7e-146">Entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="6aa7e-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="6aa7e-147">Dans un navigateur, accédez à http://localhost : 5000 pour vérifier qu’il s’exécute.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="6aa7e-148">Si l’application ne démarre pas comme prévu lors de l’exécution dans un service, un moyen rapide pour rendre les messages d’erreur accessible est pour ajouter un module fournisseur d’informations telles que la [fournisseur du journal des événements Windows](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="6aa7e-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="6aa7e-149">Accusés de réception</span><span class="sxs-lookup"><span data-stu-id="6aa7e-149">Acknowledgments</span></span>

<span data-ttu-id="6aa7e-150">Cet article a été écrit à l’aide de sources qui ont été déjà publiés.</span><span class="sxs-lookup"><span data-stu-id="6aa7e-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="6aa7e-151">Au plus tôt et plus utiles d'entre eux ont été ceux-ci :</span><span class="sxs-lookup"><span data-stu-id="6aa7e-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="6aa7e-152">Hébergement ASP.NET Core en tant que service Windows</span><span class="sxs-lookup"><span data-stu-id="6aa7e-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="6aa7e-153">L’hébergement de base ASP.NET dans un Service Windows</span><span class="sxs-lookup"><span data-stu-id="6aa7e-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
