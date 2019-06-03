---
title: Créer un serveur et un client gRPC .NET Core dans ASP.NET Core
author: juntaoluo
description: Ce tutoriel montre comment créer un service gRPC et un client gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de streaming.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 5/30/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 2b4325d2413e335a3061a7695def88a1b23ee52b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376368"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="73215-104">Tutoriel : Créer un serveur et un client gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73215-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="73215-105">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="73215-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="73215-106">Ce tutoriel montre comment créer un client [gRPC](https://grpc.io/docs/guides/) .NET Core et un serveur gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73215-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="73215-107">À la fin, vous disposerez d’un client gRPC qui communique avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="73215-108">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="73215-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="73215-109">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="73215-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="73215-110">Créer un serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="73215-111">Créez un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="73215-112">Tester le service du client gRPC avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="73215-113">Créer un service gRPC</span><span class="sxs-lookup"><span data-stu-id="73215-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73215-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73215-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="73215-115">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="73215-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="73215-116">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="73215-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="73215-117">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="73215-117">Select **Next**</span></span>
* <span data-ttu-id="73215-118">Nommez le projet **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="73215-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="73215-119">Il est important de nommer le projet *GrpcGreeter* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="73215-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="73215-120">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="73215-120">Select **Create**</span></span>
* <span data-ttu-id="73215-121">Dans la boîte de dialogue **Créer une application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="73215-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="73215-122">Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans les menus déroulants.</span><span class="sxs-lookup"><span data-stu-id="73215-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="73215-123">Sélectionnez le modèle **Service gRPC**.</span><span class="sxs-lookup"><span data-stu-id="73215-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="73215-124">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="73215-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73215-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73215-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="73215-126">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="73215-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="73215-127">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="73215-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="73215-128">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="73215-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="73215-129">La commande `dotnet new` crée un nouveau service gRPC dans le dossier *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="73215-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="73215-130">La commande `code` ouvre le dossier *GrpcGreeter* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="73215-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="73215-131">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « GrpcGreeter ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="73215-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="73215-132">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="73215-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73215-133">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="73215-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="73215-134">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="73215-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="73215-135">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="73215-136">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="73215-136">Open the project</span></span>

<span data-ttu-id="73215-137">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="73215-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="73215-138">Exécuter le service</span><span class="sxs-lookup"><span data-stu-id="73215-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73215-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73215-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="73215-140">Appuyez sur Ctrl+F5 pour exécuter le service gRPC sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="73215-140">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="73215-141">Visual Studio exécute le service dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="73215-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="73215-142">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="73215-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="73215-143">Exécutez le projet Greeter gRPC à partir de la ligne de commande, à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="73215-143">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="73215-144">Les journaux indiquent que le service écoute sur `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="73215-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="73215-145">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="73215-145">Examine the project files</span></span>

<span data-ttu-id="73215-146">Fichiers GrpcGreeter :</span><span class="sxs-lookup"><span data-stu-id="73215-146">GrpcGreeter files:</span></span>

* <span data-ttu-id="73215-147">*greet.proto* : le fichier *Protos/greet.proto* définit le service gRPC `Greeter` et est utilisé pour générer les ressources du serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="73215-148">Pour plus d’informations, consultez [Introduction à gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="73215-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="73215-149">Dossier *Services* : contient l’implémentation du service `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="73215-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="73215-150">*appSettings.json* : contient des données de configuration, telles que le protocole utilisé par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="73215-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="73215-151">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="73215-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="73215-152">*Program.cs* : contient le point d’entrée du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="73215-153">Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="73215-153">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="73215-154">*Startup.cs* : contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="73215-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="73215-155">Pour plus d’informations, consultez [Démarrage des applications](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="73215-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="73215-156">Créer le client gRPC dans une application console .NET</span><span class="sxs-lookup"><span data-stu-id="73215-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73215-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73215-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="73215-158">Sélectionnez **Fichier** > **Nouveau** > **Projet** dans la barre de menus.</span><span class="sxs-lookup"><span data-stu-id="73215-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="73215-159">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application console (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="73215-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="73215-160">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="73215-160">Select **Next**</span></span>
* <span data-ttu-id="73215-161">Dans la zone de texte **Nom**, entrez « GrpcGreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="73215-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="73215-162">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="73215-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73215-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73215-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="73215-164">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="73215-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="73215-165">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="73215-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="73215-166">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="73215-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73215-167">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="73215-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="73215-168">Suivez [ces instructions ](/dotnet/core/tutorials/using-on-mac-vs-full-solution) pour créer une application console portant le nom *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="73215-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="73215-169">Ajouter les packages nécessaires</span><span class="sxs-lookup"><span data-stu-id="73215-169">Add required packages</span></span>

<span data-ttu-id="73215-170">Ajoutez les packages suivants au projet de client gRPC :</span><span class="sxs-lookup"><span data-stu-id="73215-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="73215-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), qui contient l’API C# du client C-core.</span><span class="sxs-lookup"><span data-stu-id="73215-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="73215-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), qui contient des API de messages protobuf pour C#.</span><span class="sxs-lookup"><span data-stu-id="73215-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="73215-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), qui contient la prise en charge des outils C# pour les fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="73215-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="73215-174">Le package d’outils n’est pas nécessaire lors de l’exécution. La dépendance est donc marquée avec `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="73215-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73215-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73215-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="73215-176">Installer les packages à l’aide de la console du Gestionnaire de package (PMC) ou gérer le package NuGet</span><span class="sxs-lookup"><span data-stu-id="73215-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Package</span></span>

####  <a name="pmc-option-to-install-packages"></a><span data-ttu-id="73215-177">Option de la console du Gestionnaire de package pour installer des packages</span><span class="sxs-lookup"><span data-stu-id="73215-177">PMC option to install packages</span></span>

* <span data-ttu-id="73215-178">Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="73215-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="73215-179">Dans la fenêtre **Console du Gestionnaire de Package**, accédez au répertoire où se trouve le fichier *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="73215-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="73215-180">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="73215-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Core
Install-Package Grpc.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="73215-181">Option Gérer les packages NuGet pour installer les packages</span><span class="sxs-lookup"><span data-stu-id="73215-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="73215-182">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="73215-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="73215-183">Sélectionnez l’onglet **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="73215-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="73215-184">Entrez **Grpc.Core** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="73215-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="73215-185">Sélectionnez le package **Grpc.Core** sous l’onglet **Parcourir** et sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="73215-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="73215-186">Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="73215-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73215-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73215-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="73215-188">Exécutez les commandes suivantes à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="73215-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73215-189">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="73215-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="73215-190">Cliquez avec le bouton droit sur le dossier **Packages** dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="73215-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="73215-191">Entrez **Grpc.Core** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="73215-191">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="73215-192">Sélectionnez le package **Grpc.Core** dans le volet de résultats, puis **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="73215-192">Select the **Grpc.Core** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="73215-193">Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="73215-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="73215-194">Ajouter greet.proto</span><span class="sxs-lookup"><span data-stu-id="73215-194">Add greet.proto</span></span>

* <span data-ttu-id="73215-195">Créez un dossier **Protos** dans le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="73215-196">Copiez le fichier **Protos\greet.proto** du service Greeter gRPC vers le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="73215-197">Modifiez le fichier projet *GrpcGreeterClient.csproj* :</span><span class="sxs-lookup"><span data-stu-id="73215-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73215-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73215-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="73215-199">Cliquez avec le bouton droit sur le projet et sélectionnez **Modifier GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="73215-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="73215-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73215-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="73215-201">Sélectionnez le fichier *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="73215-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="73215-202">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="73215-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="73215-203">Cliquez avec le bouton droit sur le projet et sélectionnez **Outils > Modifier le fichier**.</span><span class="sxs-lookup"><span data-stu-id="73215-203">Right click the project and select **Tools > Edit File**.</span></span>

  ------

* <span data-ttu-id="73215-204">Ajoutez le fichier **greet.proto** au groupe d’éléments `<Protobuf>` du fichier projet GrpcGreeterClient :</span><span class="sxs-lookup"><span data-stu-id="73215-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="73215-205">Générez le projet client pour déclencher la génération des ressources du client C#.</span><span class="sxs-lookup"><span data-stu-id="73215-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greater-client"></a><span data-ttu-id="73215-206">Créer le client Greeter</span><span class="sxs-lookup"><span data-stu-id="73215-206">Create the greater client</span></span>

<span data-ttu-id="73215-207">Générez le projet pour créer les types dans l’espace de noms **Greeter**.</span><span class="sxs-lookup"><span data-stu-id="73215-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="73215-208">Les types `Greeter` sont générés automatiquement par le processus de génération.</span><span class="sxs-lookup"><span data-stu-id="73215-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="73215-209">Mettez à jour le fichier *Program.cs* du client gRPC par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="73215-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="73215-210">*Program.cs* contient le point d’entrée et la logique du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="73215-211">Le client Greeter est créé en :</span><span class="sxs-lookup"><span data-stu-id="73215-211">The greater client is created by:</span></span>

* <span data-ttu-id="73215-212">Instanciant un `Channel` contenant les informations de création de la connexion au service gRPC.</span><span class="sxs-lookup"><span data-stu-id="73215-212">Instantiating a `Channel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="73215-213">Utilisant le `Channel` pour créer le client Greeter :</span><span class="sxs-lookup"><span data-stu-id="73215-213">Using the `Channel` to construct the greater client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

<span data-ttu-id="73215-214">Le client Greeter appelle la méthode `SayHello` asynchrone.</span><span class="sxs-lookup"><span data-stu-id="73215-214">The greater client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="73215-215">Le résultat de l’appel `SayHello` s’affiche :</span><span class="sxs-lookup"><span data-stu-id="73215-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

<span data-ttu-id="73215-216">Fermez le `Channel` utilisé par le client lorsque les opérations ont fini de libérer toutes les ressources.</span><span class="sxs-lookup"><span data-stu-id="73215-216">Shut down the `Channel` used by the client when operations have finished to release all resources.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="73215-217">Tester le client gRPC avec le service Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="73215-217">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73215-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73215-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="73215-219">Dans le service Greeter, appuyez sur Ctrl+F5 pour démarrer le serveur sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="73215-219">In the Greeter service, press Ctrl+F5 to start the server without the debugger.</span></span>
* <span data-ttu-id="73215-220">Dans le projet GrpcGreeterClient, appuyez sur Ctrl+F5 pour démarrer le serveur sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="73215-220">In the GrpcGreeterClient project, press Ctrl+F5 to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="73215-221">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="73215-221">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="73215-222">Démarrez le service Greeter.</span><span class="sxs-lookup"><span data-stu-id="73215-222">Start the Greeter service.</span></span>
* <span data-ttu-id="73215-223">Démarrez le client.</span><span class="sxs-lookup"><span data-stu-id="73215-223">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="73215-224">Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="73215-224">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="73215-225">Le service envoie le message « Hello GreeterClient » comme réponse.</span><span class="sxs-lookup"><span data-stu-id="73215-225">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="73215-226">La réponse « Hello GreeterClient » s’affiche dans l’invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="73215-226">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="73215-227">Le service gRPC enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="73215-227">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a><span data-ttu-id="73215-228">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="73215-228">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
