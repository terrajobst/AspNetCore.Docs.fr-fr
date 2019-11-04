---
title: Créer un serveur et un client gRPC .NET Core dans ASP.NET Core
author: juntaoluo
description: Ce tutoriel montre comment créer un service gRPC et un client gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de streaming.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/10/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 0da5a4cf0d9cc15fee6417d143cfc9e9f1e4509c
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463065"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="71b6a-104">Didacticiel : créer un client et un serveur gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71b6a-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="71b6a-105">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="71b6a-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="71b6a-106">Ce tutoriel montre comment créer un client [gRPC](https://grpc.io/docs/guides/) .NET Core et un serveur gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="71b6a-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="71b6a-107">À la fin, vous disposerez d’un client gRPC qui communique avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="71b6a-108">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="71b6a-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="71b6a-109">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="71b6a-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="71b6a-110">Créer un serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="71b6a-111">Créez un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="71b6a-112">Tester le service du client gRPC avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71b6a-113">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="71b6a-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71b6a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71b6a-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71b6a-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71b6a-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71b6a-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="71b6a-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="71b6a-117">Créer un service gRPC</span><span class="sxs-lookup"><span data-stu-id="71b6a-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71b6a-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71b6a-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71b6a-119">Démarrez Visual Studio et sélectionnez **Créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="71b6a-120">Vous pouvez également, dans le menu **Fichier** de Visual Studio, sélectionner **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="71b6a-121">Dans la boîte de dialogue **créer un nouveau projet** , sélectionnez **gRPC service** , puis cliquez sur **suivant**:</span><span class="sxs-lookup"><span data-stu-id="71b6a-121">In the **Create a new project** dialog, select **gRPC Service** and select **Next**:</span></span>

  ![Boîte de dialogue **Créer un projet**](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="71b6a-123">Nommez le projet **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="71b6a-124">Il est important de nommer le projet *GrpcGreeter* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="71b6a-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="71b6a-125">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-125">Select **Create**.</span></span>
* <span data-ttu-id="71b6a-126">Dans la boîte de dialogue **créer un service gRPC** :</span><span class="sxs-lookup"><span data-stu-id="71b6a-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="71b6a-127">Le modèle de **service gRPC** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="71b6a-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="71b6a-128">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-128">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71b6a-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71b6a-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="71b6a-130">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="71b6a-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="71b6a-131">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="71b6a-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="71b6a-132">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="71b6a-132">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="71b6a-133">La commande `dotnet new` crée un nouveau service gRPC dans le dossier *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="71b6a-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="71b6a-134">La commande `code` ouvre le dossier *GrpcGreeter* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="71b6a-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="71b6a-135">Une boîte de dialogue s’affiche avec les **ressources requises pour la génération et le débogage dans’GrpcGreeter'. Ajoutez-les ?**</span><span class="sxs-lookup"><span data-stu-id="71b6a-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="71b6a-136">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71b6a-137">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="71b6a-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71b6a-138">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="71b6a-138">From a terminal, run the following commands:</span></span>

```dotnetcli
dotnet new grpc -o GrpcGreeter
cd GrpcGreeter
```

<span data-ttu-id="71b6a-139">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="71b6a-140">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="71b6a-140">Open the project</span></span>

<span data-ttu-id="71b6a-141">Dans Visual Studio, sélectionnez **fichier** > **ouvrir**, puis sélectionnez le fichier *GrpcGreeter. csproj* .</span><span class="sxs-lookup"><span data-stu-id="71b6a-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.csproj* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="71b6a-142">Exécuter le service</span><span class="sxs-lookup"><span data-stu-id="71b6a-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71b6a-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71b6a-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71b6a-144">Appuyez sur `Ctrl+F5` pour exécuter le service gRPC sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="71b6a-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="71b6a-145">Visual Studio exécute le service dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="71b6a-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71b6a-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71b6a-146">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="71b6a-147">Exécutez le projet gRPC Greeter *GrpcGreeter* à partir de la ligne de commande à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="71b6a-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71b6a-148">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="71b6a-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="71b6a-149">Exécutez le projet gRPC Greeter *GrpcGreeter* à partir de la ligne de commande à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="71b6a-149">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

---

<span data-ttu-id="71b6a-150">Les journaux indiquent que le service écoute sur `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="71b6a-150">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="71b6a-151">Le modèle gRPC est configuré pour utiliser le protocole [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="71b6a-151">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="71b6a-152">Les clients gRPC doivent utiliser le protocole HTTPS pour appeler le serveur.</span><span class="sxs-lookup"><span data-stu-id="71b6a-152">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="71b6a-153">MacOS ne prend pas en charge ASP.NET Core gRPC avec TLS.</span><span class="sxs-lookup"><span data-stu-id="71b6a-153">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="71b6a-154">Une configuration supplémentaire est nécessaire pour exécuter correctement les services gRPC sur MacOS.</span><span class="sxs-lookup"><span data-stu-id="71b6a-154">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="71b6a-155">Pour plus d’informations, consultez [Impossible de démarrer l’application ASP.NET Core gRPC sur MacOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="71b6a-155">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="71b6a-156">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="71b6a-156">Examine the project files</span></span>

<span data-ttu-id="71b6a-157">Fichiers projet *GrpcGreeter* :</span><span class="sxs-lookup"><span data-stu-id="71b6a-157">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="71b6a-158">*greet.proto* &ndash;Le fichier *Protos/greet.proto* définit gRPC `Greeter` et est utilisé pour générer les ressources du serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-158">*greet.proto* &ndash; The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="71b6a-159">Pour plus d’informations, consultez [Introduction à gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="71b6a-159">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="71b6a-160">Dossier *services* : contient l’implémentation du service `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="71b6a-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="71b6a-161">*appSettings.json* &ndash; contient des données de configuration, telles que le protocole utilisé par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="71b6a-161">*appSettings.json* &ndash; Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="71b6a-162">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="71b6a-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="71b6a-163">*Program.cs* &ndash; contient le point d’entrée du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-163">*Program.cs* &ndash; Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="71b6a-164">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="71b6a-164">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="71b6a-165">*Startup.cs* &ndash; contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="71b6a-165">*Startup.cs* &ndash; Contains code that configures app behavior.</span></span> <span data-ttu-id="71b6a-166">Pour plus d’informations, consultez [Démarrage des applications](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="71b6a-166">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="71b6a-167">Créer le client gRPC dans une application console .NET</span><span class="sxs-lookup"><span data-stu-id="71b6a-167">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71b6a-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71b6a-168">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71b6a-169">Ouvrez une deuxième instance de Visual Studio et sélectionnez **Créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-169">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="71b6a-170">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application console (.NET Core)** , puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-170">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="71b6a-171">Dans la zone de texte **Nom**, entrez **GrpcGreeterClient** et sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-171">In the **Name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71b6a-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71b6a-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="71b6a-173">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="71b6a-173">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="71b6a-174">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="71b6a-174">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="71b6a-175">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="71b6a-175">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71b6a-176">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="71b6a-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71b6a-177">Suivez les instructions de [Création d’une solution .NET Core complète sur macOS à l’aide de Visual Studio pour Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) pour créer une application console appelée *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="71b6a-177">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="71b6a-178">Ajouter les packages nécessaires</span><span class="sxs-lookup"><span data-stu-id="71b6a-178">Add required packages</span></span>

<span data-ttu-id="71b6a-179">Le projet client gRPC requiert les packages suivants :</span><span class="sxs-lookup"><span data-stu-id="71b6a-179">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="71b6a-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), qui contient le client .NET Core.</span><span class="sxs-lookup"><span data-stu-id="71b6a-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="71b6a-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), qui contient des API de messages protobuf pour C#.</span><span class="sxs-lookup"><span data-stu-id="71b6a-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="71b6a-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), qui contient la prise en charge des outils C# pour les fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="71b6a-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="71b6a-183">Le package d’outils n’est pas nécessaire lors de l’exécution. La dépendance est donc marquée avec `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="71b6a-183">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71b6a-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71b6a-184">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="71b6a-185">Installez les packages à l’aide de la console PMC (console du Gestionnaire de package) ou à partir de Gérer les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="71b6a-185">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="71b6a-186">Option de la console du Gestionnaire de package pour installer des packages</span><span class="sxs-lookup"><span data-stu-id="71b6a-186">PMC option to install packages</span></span>

* <span data-ttu-id="71b6a-187">Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-187">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="71b6a-188">Dans la fenêtre **Gestionnaire de package**, exécutez `cd GrpcGreeterClient` pour accéder au dossier contenant les fichiers *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="71b6a-188">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="71b6a-189">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="71b6a-189">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="71b6a-190">Option Gérer les packages NuGet pour installer les packages</span><span class="sxs-lookup"><span data-stu-id="71b6a-190">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="71b6a-191">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-191">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="71b6a-192">Sélectionnez l’onglet **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-192">Select the **Browse** tab.</span></span>
* <span data-ttu-id="71b6a-193">Entrez **Grpc.Net.Client** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="71b6a-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="71b6a-194">Sélectionnez le package **Grpc.Net.Client** sous l’onglet **Parcourir** et sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-194">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="71b6a-195">Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="71b6a-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71b6a-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71b6a-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="71b6a-197">Exécutez les commandes suivantes à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="71b6a-197">Run the following commands from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71b6a-198">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="71b6a-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="71b6a-199">Cliquez avec le bouton droit sur le dossier **Packages** dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-199">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="71b6a-200">Entrez **Grpc.Net.Client** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="71b6a-200">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="71b6a-201">Sélectionnez le package **Grpc.Net.Client** dans le volet de résultats, puis sélectionnez **Ajouter un package**</span><span class="sxs-lookup"><span data-stu-id="71b6a-201">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="71b6a-202">Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="71b6a-202">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="71b6a-203">Ajouter greet.proto</span><span class="sxs-lookup"><span data-stu-id="71b6a-203">Add greet.proto</span></span>

* <span data-ttu-id="71b6a-204">Créez un dossier *Protos* dans le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-204">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="71b6a-205">Copiez le fichier *Protos\greet.proto* du service Greeter gRPC vers le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-205">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="71b6a-206">Modifiez le fichier projet *GrpcGreeterClient.csproj* :</span><span class="sxs-lookup"><span data-stu-id="71b6a-206">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71b6a-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71b6a-207">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="71b6a-208">Cliquez avec le bouton droit sur le projet et sélectionnez **Modifier le fichier de projet**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-208">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71b6a-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71b6a-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="71b6a-210">Sélectionnez le fichier *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="71b6a-210">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71b6a-211">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="71b6a-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="71b6a-212">Cliquez avec le bouton droit sur le projet, puis sélectionnez **Outils** > **Modifier le fichier**.</span><span class="sxs-lookup"><span data-stu-id="71b6a-212">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="71b6a-213">Ajoutez un groupe d’éléments avec un élément `<Protobuf>` qui fait référence au fichier *greet.proto* :</span><span class="sxs-lookup"><span data-stu-id="71b6a-213">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="71b6a-214">Créer le client Greeter</span><span class="sxs-lookup"><span data-stu-id="71b6a-214">Create the Greeter client</span></span>

<span data-ttu-id="71b6a-215">Générez le projet pour créer les types dans l’espace de noms `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="71b6a-215">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="71b6a-216">Les types `GrpcGreeter` sont générés automatiquement par le processus de génération.</span><span class="sxs-lookup"><span data-stu-id="71b6a-216">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="71b6a-217">Mettez à jour le fichier *Program.cs* du client gRPC par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="71b6a-217">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="71b6a-218">*Program.cs* contient le point d’entrée et la logique du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-218">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="71b6a-219">Le client Greeter est créé en :</span><span class="sxs-lookup"><span data-stu-id="71b6a-219">The Greeter client is created by:</span></span>

* <span data-ttu-id="71b6a-220">Instanciant un `GrpcChannel` contenant les informations de création de la connexion au service gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-220">Instantiating a `GrpcChannel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="71b6a-221">Utilisant le `GrpcChannel` pour construire le client Greeter :</span><span class="sxs-lookup"><span data-stu-id="71b6a-221">Using the `GrpcChannel` to construct the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-5)]

<span data-ttu-id="71b6a-222">Le client Greeter appelle la méthode `SayHello` asynchrone.</span><span class="sxs-lookup"><span data-stu-id="71b6a-222">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="71b6a-223">Le résultat de l’appel `SayHello` s’affiche :</span><span class="sxs-lookup"><span data-stu-id="71b6a-223">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=6-8)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="71b6a-224">Tester le client gRPC avec le service Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="71b6a-224">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71b6a-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71b6a-225">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71b6a-226">Dans le service Greeter, appuyez sur `Ctrl+F5` pour démarrer le serveur sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="71b6a-226">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="71b6a-227">Dans le projet `GrpcGreeterClient`, appuyez sur `Ctrl+F5` pour démarrer le client sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="71b6a-227">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71b6a-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71b6a-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="71b6a-229">Démarrez le service Greeter.</span><span class="sxs-lookup"><span data-stu-id="71b6a-229">Start the Greeter service.</span></span>
* <span data-ttu-id="71b6a-230">Démarrez le client.</span><span class="sxs-lookup"><span data-stu-id="71b6a-230">Start the client.</span></span>


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71b6a-231">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="71b6a-231">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="71b6a-232">Démarrez le service Greeter.</span><span class="sxs-lookup"><span data-stu-id="71b6a-232">Start the Greeter service.</span></span>
* <span data-ttu-id="71b6a-233">Démarrez le client.</span><span class="sxs-lookup"><span data-stu-id="71b6a-233">Start the client.</span></span>

---

<span data-ttu-id="71b6a-234">Le client envoie une salutation au service avec un message contenant son nom, *GreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="71b6a-234">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="71b6a-235">Le service envoie le message « Hello GreeterClient » comme réponse.</span><span class="sxs-lookup"><span data-stu-id="71b6a-235">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="71b6a-236">La réponse « Hello GreeterClient » s’affiche dans l’invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="71b6a-236">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="71b6a-237">Le service gRPC enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="71b6a-237">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

> [!NOTE]
> <span data-ttu-id="71b6a-238">Le code de cet article requiert le certificat de développement ASP.NET Core HTTPS pour sécuriser le service gRPC.</span><span class="sxs-lookup"><span data-stu-id="71b6a-238">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="71b6a-239">Si le client échoue avec le message `The remote certificate is invalid according to the validation procedure.`, le certificat de développement n’est pas approuvé.</span><span class="sxs-lookup"><span data-stu-id="71b6a-239">If the client fails with the message `The remote certificate is invalid according to the validation procedure.`, the development certificate is not trusted.</span></span> <span data-ttu-id="71b6a-240">Pour obtenir des instructions afin de résoudre ce problème, consultez [Faire confiance au certificat de développement ASP.NET Core HTTPS sur Windows et macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="71b6a-240">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

### <a name="next-steps"></a><span data-ttu-id="71b6a-241">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="71b6a-241">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
