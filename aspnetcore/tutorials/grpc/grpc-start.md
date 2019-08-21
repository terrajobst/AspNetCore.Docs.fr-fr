---
title: Créer un serveur et un client gRPC .NET Core dans ASP.NET Core
author: juntaoluo
description: Ce tutoriel montre comment créer un service gRPC et un client gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de streaming.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 496f659bd51e2404a936bea8aad77e674e1a285d
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022490"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="ef6f2-104">Tutoriel : Créer un serveur et un client gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef6f2-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="ef6f2-105">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="ef6f2-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="ef6f2-106">Ce tutoriel montre comment créer un client [gRPC](https://grpc.io/docs/guides/) .NET Core et un serveur gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="ef6f2-107">À la fin, vous disposerez d’un client gRPC qui communique avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="ef6f2-108">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ef6f2-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="ef6f2-109">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ef6f2-110">Créer un serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="ef6f2-111">Créez un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="ef6f2-112">Tester le service du client gRPC avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef6f2-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ef6f2-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef6f2-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef6f2-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef6f2-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef6f2-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef6f2-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ef6f2-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="ef6f2-117">Créer un service gRPC</span><span class="sxs-lookup"><span data-stu-id="ef6f2-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef6f2-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef6f2-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ef6f2-119">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ef6f2-120">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="ef6f2-121">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-121">Select **Next**</span></span>
* <span data-ttu-id="ef6f2-122">Nommez le projet **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="ef6f2-123">Il est important de nommer le projet *GrpcGreeter* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="ef6f2-124">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-124">Select **Create**</span></span>
* <span data-ttu-id="ef6f2-125">Dans la boîte de dialogue **Créer une application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="ef6f2-126">Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans les menus déroulants.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="ef6f2-127">Sélectionnez le modèle **Service gRPC**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="ef6f2-128">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef6f2-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef6f2-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ef6f2-130">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ef6f2-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ef6f2-131">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ef6f2-132">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="ef6f2-133">La commande `dotnet new` crée un nouveau service gRPC dans le dossier *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="ef6f2-134">La commande `code` ouvre le dossier *GrpcGreeter* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="ef6f2-135">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « GrpcGreeter ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="ef6f2-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="ef6f2-136">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef6f2-137">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ef6f2-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ef6f2-138">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="ef6f2-139">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="ef6f2-140">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="ef6f2-140">Open the project</span></span>

<span data-ttu-id="ef6f2-141">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="ef6f2-142">Exécuter le service</span><span class="sxs-lookup"><span data-stu-id="ef6f2-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef6f2-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef6f2-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ef6f2-144">Appuyez sur `Ctrl+F5` pour exécuter le service gRPC sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="ef6f2-145">Visual Studio exécute le service dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="ef6f2-146">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ef6f2-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="ef6f2-147">Exécutez le projet gRPC Greeter *GrpcGreeter* à partir de la ligne de commande à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="ef6f2-148">Les journaux indiquent que le service écoute sur `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="ef6f2-149">Le modèle gRPC est configuré pour utiliser le protocole [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="ef6f2-149">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="ef6f2-150">Les clients gRPC doivent utiliser le protocole HTTPS pour appeler le serveur.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-150">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="ef6f2-151">MacOS ne prend pas en charge ASP.NET Core gRPC avec TLS.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-151">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="ef6f2-152">Une configuration supplémentaire est nécessaire pour exécuter correctement les services gRPC sur MacOS.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-152">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="ef6f2-153">Pour plus d’informations, consultez [Impossible de démarrer l’application ASP.NET Core gRPC sur MacOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="ef6f2-153">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="ef6f2-154">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="ef6f2-154">Examine the project files</span></span>

<span data-ttu-id="ef6f2-155">Fichiers projet *GrpcGreeter* :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-155">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="ef6f2-156">*greet.proto* : le fichier *Protos/greet.proto* définit le service gRPC `Greeter` et est utilisé pour générer les ressources du serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-156">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="ef6f2-157">Pour plus d’informations, consultez [Introduction à gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="ef6f2-157">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="ef6f2-158">Dossier *Services* : contient l’implémentation du service `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-158">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="ef6f2-159">*appSettings.json* : contient des données de configuration, telles que le protocole utilisé par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-159">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="ef6f2-160">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-160">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="ef6f2-161">*Program.cs* : contient le point d’entrée du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-161">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="ef6f2-162">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-162">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="ef6f2-163">*Startup.cs* : contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-163">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="ef6f2-164">Pour plus d’informations, consultez [Démarrage des applications](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="ef6f2-164">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="ef6f2-165">Créer le client gRPC dans une application console .NET</span><span class="sxs-lookup"><span data-stu-id="ef6f2-165">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef6f2-166">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef6f2-166">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ef6f2-167">Ouvrez une deuxième instance de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-167">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="ef6f2-168">Sélectionnez **Fichier** > **Nouveau** > **Projet** dans la barre de menus.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-168">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="ef6f2-169">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application console (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="ef6f2-169">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="ef6f2-170">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-170">Select **Next**</span></span>
* <span data-ttu-id="ef6f2-171">Dans la zone de texte **Nom**, entrez « GrpcGreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="ef6f2-171">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="ef6f2-172">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-172">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef6f2-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef6f2-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ef6f2-174">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ef6f2-174">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ef6f2-175">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-175">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ef6f2-176">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-176">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef6f2-177">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ef6f2-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ef6f2-178">Suivez [ces instructions ](/dotnet/core/tutorials/using-on-mac-vs-full-solution) pour créer une application console portant le nom *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-178">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="ef6f2-179">Ajouter les packages nécessaires</span><span class="sxs-lookup"><span data-stu-id="ef6f2-179">Add required packages</span></span>

<span data-ttu-id="ef6f2-180">Le projet client gRPC requiert les packages suivants :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-180">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="ef6f2-181">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), qui contient le client .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-181">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="ef6f2-182">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), qui contient des API de messages protobuf pour C#.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-182">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="ef6f2-183">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), qui contient la prise en charge des outils C# pour les fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-183">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="ef6f2-184">Le package d’outils n’est pas nécessaire lors de l’exécution. La dépendance est donc marquée avec `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-184">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef6f2-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef6f2-185">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ef6f2-186">Installez les packages à l’aide de la console PMC (console du Gestionnaire de package) ou à partir de Gérer les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-186">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="ef6f2-187">Option de la console du Gestionnaire de package pour installer des packages</span><span class="sxs-lookup"><span data-stu-id="ef6f2-187">PMC option to install packages</span></span>

* <span data-ttu-id="ef6f2-188">Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-188">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="ef6f2-189">Dans la fenêtre **Console du Gestionnaire de Package**, accédez au répertoire où se trouve le fichier *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-189">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="ef6f2-190">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-190">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="ef6f2-191">Option Gérer les packages NuGet pour installer les packages</span><span class="sxs-lookup"><span data-stu-id="ef6f2-191">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="ef6f2-192">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-192">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="ef6f2-193">Sélectionnez l’onglet **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-193">Select the **Browse** tab.</span></span>
* <span data-ttu-id="ef6f2-194">Entrez **Grpc.Net.Client** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-194">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="ef6f2-195">Sélectionnez le package **Grpc.Net.Client** sous l’onglet **Parcourir** et sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-195">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="ef6f2-196">Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-196">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef6f2-197">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef6f2-197">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ef6f2-198">Exécutez les commandes suivantes à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-198">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef6f2-199">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ef6f2-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ef6f2-200">Cliquez avec le bouton droit sur le dossier **Packages** dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-200">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="ef6f2-201">Entrez **Grpc.Net.Client** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-201">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="ef6f2-202">Sélectionnez le package **Grpc.Net.Client** dans le volet de résultats, puis sélectionnez **Ajouter un package**</span><span class="sxs-lookup"><span data-stu-id="ef6f2-202">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="ef6f2-203">Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-203">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="ef6f2-204">Ajouter greet.proto</span><span class="sxs-lookup"><span data-stu-id="ef6f2-204">Add greet.proto</span></span>

* <span data-ttu-id="ef6f2-205">Créez un dossier **Protos** dans le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-205">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="ef6f2-206">Copiez le fichier **Protos\greet.proto** du service Greeter gRPC vers le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-206">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="ef6f2-207">Modifiez le fichier projet *GrpcGreeterClient.csproj* :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-207">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef6f2-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef6f2-208">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="ef6f2-209">Cliquez avec le bouton droit sur le projet et sélectionnez **Modifier le fichier de projet**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-209">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef6f2-210">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef6f2-210">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="ef6f2-211">Sélectionnez le fichier *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-211">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef6f2-212">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ef6f2-212">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="ef6f2-213">Cliquez avec le bouton droit sur le projet, puis sélectionnez **Outils > Modifier le fichier**.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-213">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="ef6f2-214">Ajoutez un groupe d’éléments avec un élément `<Protobuf>` qui fait référence au fichier **greet.proto** :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-214">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="ef6f2-215">Créer le client Greeter</span><span class="sxs-lookup"><span data-stu-id="ef6f2-215">Create the Greeter client</span></span>

<span data-ttu-id="ef6f2-216">Générez le projet pour créer les types dans l’espace de noms `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-216">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="ef6f2-217">Les types `GrpcGreeter` sont générés automatiquement par le processus de génération.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-217">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="ef6f2-218">Mettez à jour le fichier *Program.cs* du client gRPC par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-218">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="ef6f2-219">*Program.cs* contient le point d’entrée et la logique du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-219">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="ef6f2-220">Le client Greeter est créé en :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-220">The Greeter client is created by:</span></span>

* <span data-ttu-id="ef6f2-221">Instanciation d’un `HttpClient` qui contient les informations permettant de créer la connexion au service gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-221">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="ef6f2-222">Utilisant le `HttpClient` pour construire le client Greeter :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-222">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="ef6f2-223">Le client Greeter appelle la méthode `SayHello` asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-223">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="ef6f2-224">Le résultat de l’appel `SayHello` s’affiche :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-224">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="ef6f2-225">Tester le client gRPC avec le service Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="ef6f2-225">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef6f2-226">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef6f2-226">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ef6f2-227">Dans le service Greeter, appuyez sur `Ctrl+F5` pour démarrer le serveur sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-227">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="ef6f2-228">Dans le projet `GrpcGreeterClient`, appuyez sur `Ctrl+F5` pour démarrer le client sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-228">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="ef6f2-229">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ef6f2-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="ef6f2-230">Démarrez le service Greeter.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-230">Start the Greeter service.</span></span>
* <span data-ttu-id="ef6f2-231">Démarrez le client.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-231">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="ef6f2-232">Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="ef6f2-232">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="ef6f2-233">Le service envoie le message « Hello GreeterClient » comme réponse.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-233">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="ef6f2-234">La réponse « Hello GreeterClient » s’affiche dans l’invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="ef6f2-234">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="ef6f2-235">Le service gRPC enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ef6f2-235">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="ef6f2-236">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ef6f2-236">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
