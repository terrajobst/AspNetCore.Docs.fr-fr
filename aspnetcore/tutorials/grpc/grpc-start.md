---
title: 'Tutoriel : Bien démarrer avec le service gRPC dans ASP.NET Core'
author: juntaoluo
description: Cette série de tutoriels montre comment créer un service gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de diffusion en continu.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 5f7a2f6b57804b3295b23c322dcbac553b05528b
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320054"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="dee54-104">Tutoriel : Bien démarrer avec le service gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dee54-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="dee54-105">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="dee54-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="dee54-106">Ce tutoriel décrit les principes fondamentaux liés à la création d’un service gRPC sur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dee54-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="dee54-107">À la fin, vous disposerez d’un service gRPC qui renvoie les salutations.</span><span class="sxs-lookup"><span data-stu-id="dee54-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="dee54-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="dee54-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dee54-109">Créez un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="dee54-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="dee54-110">Exécutez le service.</span><span class="sxs-lookup"><span data-stu-id="dee54-110">Run the service.</span></span>
> * <span data-ttu-id="dee54-111">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="dee54-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="dee54-112">Créer un service gRPC</span><span class="sxs-lookup"><span data-stu-id="dee54-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dee54-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dee54-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dee54-114">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="dee54-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="dee54-115">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dee54-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="dee54-116">![Nouvelle application web ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="dee54-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="dee54-117">Nommez le projet **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="dee54-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="dee54-118">Il est important de nommer le projet *GrpcGreeter* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="dee54-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="dee54-119">![Nouvelle application web ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="dee54-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="dee54-120">Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="dee54-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="dee54-121">Choisissez le modèle **Service gRPC**.</span><span class="sxs-lookup"><span data-stu-id="dee54-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="dee54-122">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="dee54-122">The following starter project is created:</span></span>

  ![Explorateur de solutions](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dee54-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dee54-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dee54-125">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dee54-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="dee54-126">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="dee54-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="dee54-127">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dee54-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="dee54-128">La commande `dotnet new` crée un nouveau service gRPC dans le dossier *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="dee54-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="dee54-129">La commande `code` ouvre le dossier *GrpcGreeter* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dee54-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="dee54-130">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « GrpcGreeter ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="dee54-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="dee54-131">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="dee54-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dee54-132">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dee54-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dee54-133">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dee54-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="dee54-134">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="dee54-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="dee54-135">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="dee54-135">Open the project</span></span>

<span data-ttu-id="dee54-136">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="dee54-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a><span data-ttu-id="dee54-137">Tester le service</span><span class="sxs-lookup"><span data-stu-id="dee54-137">Test the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dee54-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dee54-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dee54-139">Vérifiez le paramètre **GrpcGreeter.Server** est défini comme projet de démarrage, et appuyez sur Ctrl + F5 pour exécuter le service gRPC sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="dee54-139">Ensure the **GrpcGreeter.Server** is set as the Startup Project and press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="dee54-140">Visual Studio exécute le service dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="dee54-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="dee54-141">Les journaux indiquent que le service a démarré l’écoute sur `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="dee54-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/server_start.png)

* <span data-ttu-id="dee54-143">Une fois le service en cours d’exécution, définissez le paramètre **GrpcGreeter.Client** est défini comme projet de démarrage, et appuyez sur Ctrl + F5 pour exécuter le client sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="dee54-143">Once the service is running, set the **GrpcGreeter.Client** is set as the Startup Project and press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="dee54-144">Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="dee54-144">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="dee54-145">Le service envoie un message « Hello GreeterClient » en tant que réponse, qui s’affiche dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="dee54-145">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="dee54-147">Le service enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="dee54-147">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="dee54-149">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dee54-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="dee54-150">Exécutez le projet de serveur GrpcGreeter.Server à partir de la ligne de commande à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="dee54-150">Run the Server project GrpcGreeter.Server from the command line using `dotnet run`.</span></span> <span data-ttu-id="dee54-151">Les journaux indiquent que le service a démarré l’écoute sur `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="dee54-151">The logs show that the service started listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* <span data-ttu-id="dee54-152">Exécutez le projet client GrpcGreeter.Server à partir d’une ligne de commande distincte à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="dee54-152">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="dee54-153">Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="dee54-153">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="dee54-154">Le service envoie un message « Hello GreeterClient » en tant que réponse, qui s’affiche dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="dee54-154">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="dee54-155">Le service enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="dee54-155">The service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="dee54-156">Examinez les fichiers du projet gRPC</span><span class="sxs-lookup"><span data-stu-id="dee54-156">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="dee54-157">Fichiers GrpcGreeter.Server :</span><span class="sxs-lookup"><span data-stu-id="dee54-157">GrpcGreeter.Server files:</span></span>

* <span data-ttu-id="dee54-158">greet.proto : le fichier *Protos/greet.proto* définit le service gRPC `Greeter` et est utilisé pour générer les ressources du serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="dee54-158">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="dee54-159">Pour plus d'informations, consultez <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="dee54-159">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="dee54-160">Dossier *Services* : contient l’implémentation du service `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="dee54-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="dee54-161">*appSettings.json* : contient des données de configuration, telles que le protocole utilisé par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="dee54-161">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="dee54-162">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="dee54-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="dee54-163">*Program.cs* : contient le point d’entrée du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="dee54-163">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="dee54-164">Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="dee54-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="dee54-165">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="dee54-165">Startup.cs</span></span>

<span data-ttu-id="dee54-166">contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="dee54-166">Contains code that configures app behavior.</span></span> <span data-ttu-id="dee54-167">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="dee54-167">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="dee54-168">Fichier client gRPC GrpcGreeter.Client :</span><span class="sxs-lookup"><span data-stu-id="dee54-168">gRPC client GrpcGreeter.Client file:</span></span>

<span data-ttu-id="dee54-169">*Program.cs* contient le point d’entrée et la logique du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="dee54-169">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="dee54-170">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="dee54-170">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dee54-171">Créez un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="dee54-171">Created a gRPC service.</span></span>
> * <span data-ttu-id="dee54-172">Exécutez le service et un client pour tester le service.</span><span class="sxs-lookup"><span data-stu-id="dee54-172">Ran the service and a client to test the service.</span></span>
> * <span data-ttu-id="dee54-173">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="dee54-173">Examined the project files.</span></span>
