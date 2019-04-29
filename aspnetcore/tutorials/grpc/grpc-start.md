---
title: 'Tutoriel : Bien démarrer avec le service gRPC dans ASP.NET Core'
author: juntaoluo
description: Cette série de tutoriels montre comment créer un service gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de diffusion en continu.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672565"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="e80dd-104">Tutoriel : Bien démarrer avec le service gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e80dd-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="e80dd-105">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="e80dd-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="e80dd-106">Ce tutoriel décrit les principes fondamentaux liés à la création d’un service gRPC sur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e80dd-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="e80dd-107">À la fin, vous disposerez d’un service gRPC qui renvoie les salutations.</span><span class="sxs-lookup"><span data-stu-id="e80dd-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="e80dd-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e80dd-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e80dd-109">Créez un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="e80dd-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="e80dd-110">Exécutez un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="e80dd-110">Run the gRPC service.</span></span>
> * <span data-ttu-id="e80dd-111">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="e80dd-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="e80dd-112">Créer un service gRPC</span><span class="sxs-lookup"><span data-stu-id="e80dd-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e80dd-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e80dd-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e80dd-114">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="e80dd-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e80dd-115">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e80dd-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="e80dd-116">![Nouvelle application web ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="e80dd-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="e80dd-117">Nommez le projet **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="e80dd-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="e80dd-118">Il est important de nommer le projet *GrpcGreeter* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="e80dd-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="e80dd-119">![Nouvelle application web ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="e80dd-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="e80dd-120">Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="e80dd-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="e80dd-121">Choisissez le modèle **Service gRPC**.</span><span class="sxs-lookup"><span data-stu-id="e80dd-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="e80dd-122">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="e80dd-122">The following starter project is created:</span></span>

  ![Explorateur de solutions](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e80dd-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e80dd-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e80dd-125">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e80dd-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e80dd-126">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="e80dd-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="e80dd-127">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e80dd-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="e80dd-128">La commande `dotnet new` crée un nouveau service gRPC dans le dossier *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="e80dd-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="e80dd-129">La commande `code` ouvre le dossier *GrpcGreeter* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e80dd-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="e80dd-130">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « GrpcGreeter ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="e80dd-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="e80dd-131">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e80dd-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e80dd-132">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e80dd-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e80dd-133">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e80dd-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="e80dd-134">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="e80dd-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="e80dd-135">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="e80dd-135">Open the project</span></span>

<span data-ttu-id="e80dd-136">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="e80dd-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="e80dd-137">Exécuter le service</span><span class="sxs-lookup"><span data-stu-id="e80dd-137">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e80dd-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e80dd-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e80dd-139">Appuyez sur Ctrl+F5 pour exécuter le service gRPC sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="e80dd-139">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="e80dd-140">Visual Studio exécute le service dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="e80dd-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="e80dd-141">Les journaux indiquent que le service a démarré l’écoute sur `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="e80dd-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e80dd-143">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e80dd-143">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e80dd-144">Exécutez le projet Greeter gRPC à partir de la ligne de commande, à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e80dd-144">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span> <span data-ttu-id="e80dd-145">Les journaux indiquent que le service a démarré l’écoute sur `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="e80dd-145">The logs show that the service started listening on `http://localhost:50051`.</span></span>

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
```

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="e80dd-146">Le tutoriel suivant vous montrera comment créer un client gRPC, qui est nécessaire pour tester le service Greeter.</span><span class="sxs-lookup"><span data-stu-id="e80dd-146">The next tutorial will demonstrate how to build a gRPC client, which is required to test the Greeter service.</span></span>

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="e80dd-147">Examinez les fichiers du projet gRPC</span><span class="sxs-lookup"><span data-stu-id="e80dd-147">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="e80dd-148">Fichiers GrpcGreeter :</span><span class="sxs-lookup"><span data-stu-id="e80dd-148">GrpcGreeter files:</span></span>

* <span data-ttu-id="e80dd-149">greet.proto : le fichier *Protos/greet.proto* définit le service gRPC `Greeter` et est utilisé pour générer les ressources du serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="e80dd-149">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="e80dd-150">Pour plus d'informations, consultez <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="e80dd-150">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="e80dd-151">Dossier *Services* : contient l’implémentation du service `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="e80dd-151">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="e80dd-152">*appSettings.json* : contient des données de configuration, telles que le protocole utilisé par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e80dd-152">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="e80dd-153">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e80dd-153">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="e80dd-154">*Program.cs* : contient le point d’entrée du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="e80dd-154">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="e80dd-155">Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="e80dd-155">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="e80dd-156">Startup.cs : contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e80dd-156">Startup.cs: Contains code that configures app behavior.</span></span> <span data-ttu-id="e80dd-157">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e80dd-157">For more information, see <xref:fundamentals/startup>.</span></span>

### <a name="test-the-service"></a><span data-ttu-id="e80dd-158">Tester le service</span><span class="sxs-lookup"><span data-stu-id="e80dd-158">Test the service</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e80dd-159">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e80dd-159">Additional resources</span></span>

<span data-ttu-id="e80dd-160">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e80dd-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e80dd-161">Créez un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="e80dd-161">Created a gRPC service.</span></span>
> * <span data-ttu-id="e80dd-162">Exécuter le service gRPC</span><span class="sxs-lookup"><span data-stu-id="e80dd-162">Ran the gRPC service.</span></span>
> * <span data-ttu-id="e80dd-163">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="e80dd-163">Examined the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e80dd-164">Suivant : Créer un client gRPC .NET Core</span><span class="sxs-lookup"><span data-stu-id="e80dd-164">Next: Create a .NET Core gRPC client</span></span>](xref:tutorials/grpc/grpc-client)