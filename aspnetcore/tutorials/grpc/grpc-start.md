---
title: Créer un serveur et un client gRPC .NET Core dans ASP.NET Core
author: juntaoluo
description: Ce tutoriel montre comment créer un service gRPC et un client gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de streaming.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 6a3a7446a488ef54d99d6c7605980c18890b9ad0
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776643"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="a501b-104">Tutoriel : Créer un serveur et un client gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a501b-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="a501b-105">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="a501b-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a501b-106">Ce tutoriel montre comment créer un client [gRPC](https://grpc.io/docs/guides/) .NET Core et un serveur gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a501b-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="a501b-107">À la fin, vous disposerez d’un client gRPC qui communique avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="a501b-108">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a501b-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a501b-109">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="a501b-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a501b-110">Créer un serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="a501b-111">Créez un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="a501b-112">Tester le service du client gRPC avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a501b-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a501b-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a501b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a501b-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a501b-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a501b-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a501b-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a501b-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="a501b-117">Créer un service gRPC</span><span class="sxs-lookup"><span data-stu-id="a501b-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a501b-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a501b-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a501b-119">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="a501b-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a501b-120">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a501b-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a501b-121">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="a501b-121">Select **Next**</span></span>
* <span data-ttu-id="a501b-122">Nommez le projet **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="a501b-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="a501b-123">Il est important de nommer le projet *GrpcGreeter* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="a501b-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="a501b-124">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a501b-124">Select **Create**</span></span>
* <span data-ttu-id="a501b-125">Dans la boîte de dialogue **Créer une application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="a501b-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="a501b-126">Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans les menus déroulants.</span><span class="sxs-lookup"><span data-stu-id="a501b-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="a501b-127">Sélectionnez le modèle **Service gRPC**.</span><span class="sxs-lookup"><span data-stu-id="a501b-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="a501b-128">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a501b-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a501b-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a501b-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a501b-130">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a501b-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a501b-131">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="a501b-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a501b-132">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a501b-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="a501b-133">La commande `dotnet new` crée un nouveau service gRPC dans le dossier *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="a501b-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="a501b-134">La commande `code` ouvre le dossier *GrpcGreeter* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a501b-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="a501b-135">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « GrpcGreeter ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="a501b-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="a501b-136">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="a501b-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a501b-137">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a501b-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a501b-138">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a501b-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="a501b-139">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="a501b-140">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="a501b-140">Open the project</span></span>

<span data-ttu-id="a501b-141">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="a501b-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="a501b-142">Exécuter le service</span><span class="sxs-lookup"><span data-stu-id="a501b-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a501b-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a501b-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a501b-144">Appuyez sur `Ctrl+F5` pour exécuter le service gRPC sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="a501b-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="a501b-145">Visual Studio exécute le service dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="a501b-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a501b-146">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a501b-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a501b-147">Exécutez le projet gRPC Greeter *GrpcGreeter* à partir de la ligne de commande à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a501b-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a501b-148">Les journaux indiquent que le service écoute sur `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="a501b-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="a501b-149">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="a501b-149">Examine the project files</span></span>

<span data-ttu-id="a501b-150">Fichiers projet *GrpcGreeter* :</span><span class="sxs-lookup"><span data-stu-id="a501b-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="a501b-151">*greet.proto* : le fichier *Protos/greet.proto* définit le service gRPC `Greeter` et est utilisé pour générer les ressources du serveur gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="a501b-152">Pour plus d’informations, consultez [Introduction à gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="a501b-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="a501b-153">Dossier *Services* : contient l’implémentation du service `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="a501b-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="a501b-154">*appSettings.json* : contient des données de configuration, telles que le protocole utilisé par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a501b-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="a501b-155">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a501b-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="a501b-156">*Program.cs* : contient le point d’entrée du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="a501b-157">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="a501b-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="a501b-158">*Startup.cs* : contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="a501b-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="a501b-159">Pour plus d’informations, consultez [Démarrage des applications](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a501b-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="a501b-160">Créer le client gRPC dans une application console .NET</span><span class="sxs-lookup"><span data-stu-id="a501b-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a501b-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a501b-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a501b-162">Ouvrez une deuxième instance de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a501b-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="a501b-163">Sélectionnez **Fichier** > **Nouveau** > **Projet** dans la barre de menus.</span><span class="sxs-lookup"><span data-stu-id="a501b-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="a501b-164">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application console (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="a501b-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="a501b-165">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="a501b-165">Select **Next**</span></span>
* <span data-ttu-id="a501b-166">Dans la zone de texte **Nom**, entrez « GrpcGreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="a501b-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="a501b-167">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a501b-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a501b-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a501b-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a501b-169">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a501b-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a501b-170">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="a501b-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a501b-171">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a501b-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a501b-172">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a501b-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a501b-173">Suivez [ces instructions ](/dotnet/core/tutorials/using-on-mac-vs-full-solution) pour créer une application console portant le nom *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="a501b-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="a501b-174">Ajouter les packages nécessaires</span><span class="sxs-lookup"><span data-stu-id="a501b-174">Add required packages</span></span>

<span data-ttu-id="a501b-175">Le projet client gRPC requiert les packages suivants :</span><span class="sxs-lookup"><span data-stu-id="a501b-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="a501b-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), qui contient le client .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a501b-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="a501b-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), qui contient des API de messages protobuf pour C#.</span><span class="sxs-lookup"><span data-stu-id="a501b-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="a501b-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), qui contient la prise en charge des outils C# pour les fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="a501b-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="a501b-179">Le package d’outils n’est pas nécessaire lors de l’exécution. La dépendance est donc marquée avec `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="a501b-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a501b-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a501b-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a501b-181">Installez les packages à l’aide de la console PMC (console du Gestionnaire de package) ou à partir de Gérer les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="a501b-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="a501b-182">Option de la console du Gestionnaire de package pour installer des packages</span><span class="sxs-lookup"><span data-stu-id="a501b-182">PMC option to install packages</span></span>

* <span data-ttu-id="a501b-183">Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="a501b-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="a501b-184">Dans la fenêtre **Console du Gestionnaire de Package**, accédez au répertoire où se trouve le fichier *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a501b-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="a501b-185">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a501b-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="a501b-186">Option Gérer les packages NuGet pour installer les packages</span><span class="sxs-lookup"><span data-stu-id="a501b-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="a501b-187">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a501b-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="a501b-188">Sélectionnez l’onglet **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="a501b-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="a501b-189">Entrez **Grpc.Net.Client** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="a501b-189">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="a501b-190">Sélectionnez le package **Grpc.Net.Client** sous l’onglet **Parcourir** et sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="a501b-190">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="a501b-191">Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a501b-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a501b-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a501b-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a501b-193">Exécutez les commandes suivantes à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="a501b-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a501b-194">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a501b-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a501b-195">Cliquez avec le bouton droit sur le dossier **Packages** dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="a501b-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="a501b-196">Entrez **Grpc.Net.Client** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="a501b-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="a501b-197">Sélectionnez le package **Grpc.Net.Client** dans le volet de résultats, puis sélectionnez **Ajouter un package**</span><span class="sxs-lookup"><span data-stu-id="a501b-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="a501b-198">Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a501b-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="a501b-199">Ajouter greet.proto</span><span class="sxs-lookup"><span data-stu-id="a501b-199">Add greet.proto</span></span>

* <span data-ttu-id="a501b-200">Créez un dossier **Protos** dans le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="a501b-201">Copiez le fichier **Protos\greet.proto** du service Greeter gRPC vers le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="a501b-202">Modifiez le fichier projet *GrpcGreeterClient.csproj* :</span><span class="sxs-lookup"><span data-stu-id="a501b-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a501b-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a501b-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="a501b-204">Cliquez avec le bouton droit sur le projet et sélectionnez **Modifier le fichier de projet**.</span><span class="sxs-lookup"><span data-stu-id="a501b-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a501b-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a501b-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="a501b-206">Sélectionnez le fichier *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a501b-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a501b-207">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a501b-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="a501b-208">Cliquez avec le bouton droit sur le projet, puis sélectionnez **Outils > Modifier le fichier**.</span><span class="sxs-lookup"><span data-stu-id="a501b-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="a501b-209">Ajoutez un groupe d’éléments avec un élément `<Protobuf>` qui fait référence au fichier **greet.proto** :</span><span class="sxs-lookup"><span data-stu-id="a501b-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="a501b-210">Créer le client Greeter</span><span class="sxs-lookup"><span data-stu-id="a501b-210">Create the Greeter client</span></span>

<span data-ttu-id="a501b-211">Générez le projet pour créer les types dans l’espace de noms `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="a501b-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="a501b-212">Les types `GrpcGreeter` sont générés automatiquement par le processus de génération.</span><span class="sxs-lookup"><span data-stu-id="a501b-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="a501b-213">Mettez à jour le fichier *Program.cs* du client gRPC par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a501b-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="a501b-214">*Program.cs* contient le point d’entrée et la logique du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="a501b-215">Le client Greeter est créé en :</span><span class="sxs-lookup"><span data-stu-id="a501b-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="a501b-216">Instanciation d’un `HttpClient` qui contient les informations permettant de créer la connexion au service gRPC.</span><span class="sxs-lookup"><span data-stu-id="a501b-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="a501b-217">Utilisant le `HttpClient` pour construire le client Greeter :</span><span class="sxs-lookup"><span data-stu-id="a501b-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="a501b-218">Le client Greeter appelle la méthode `SayHello` asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a501b-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="a501b-219">Le résultat de l’appel `SayHello` s’affiche :</span><span class="sxs-lookup"><span data-stu-id="a501b-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="a501b-220">Tester le client gRPC avec le service Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="a501b-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a501b-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a501b-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a501b-222">Dans le service Greeter, appuyez sur `Ctrl+F5` pour démarrer le serveur sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="a501b-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="a501b-223">Dans le projet `GrpcGreeterClient`, appuyez sur `Ctrl+F5` pour démarrer le client sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="a501b-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a501b-224">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a501b-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a501b-225">Démarrez le service Greeter.</span><span class="sxs-lookup"><span data-stu-id="a501b-225">Start the Greeter service.</span></span>
* <span data-ttu-id="a501b-226">Démarrez le client.</span><span class="sxs-lookup"><span data-stu-id="a501b-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a501b-227">Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="a501b-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="a501b-228">Le service envoie le message « Hello GreeterClient » comme réponse.</span><span class="sxs-lookup"><span data-stu-id="a501b-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="a501b-229">La réponse « Hello GreeterClient » s’affiche dans l’invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="a501b-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="a501b-230">Le service gRPC enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="a501b-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="a501b-231">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="a501b-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
