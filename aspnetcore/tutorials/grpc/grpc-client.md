---
title: 'Tutoriel : Créer un client gRPC .NET Core'
author: juntaoluo
description: Cette série de tutoriels montre comment créer un service gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de streaming.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: ec6bf5072c76de640a78b2c3f13dd1fc552b9d04
ms.sourcegitcommit: b508b115107e0f8d7f62b25cfcc8ad45e1373459
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212638"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a><span data-ttu-id="ca57b-104">Tutoriel : Créer un client gRPC .NET Core</span><span class="sxs-lookup"><span data-stu-id="ca57b-104">Tutorial: Create a .NET Core gRPC client</span></span>

<span data-ttu-id="ca57b-105">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="ca57b-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="ca57b-106">Ce tutoriel explique comment créer un client [gRPC](https://grpc.io/docs/guides/) .NET Core qui peut communiquer avec les services gRPC.</span><span class="sxs-lookup"><span data-stu-id="ca57b-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client that can communicate with gRPC services.</span></span>

<span data-ttu-id="ca57b-107">À la fin, vous disposerez d’un client gRPC qui communique avec le service Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="ca57b-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

<span data-ttu-id="ca57b-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ca57b-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ca57b-109">Créez un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="ca57b-109">Create a gRPC client.</span></span>
> * <span data-ttu-id="ca57b-110">Exécutez le service sur le service Greeter gRPC créé dans le tutoriel précédent.</span><span class="sxs-lookup"><span data-stu-id="ca57b-110">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="ca57b-111">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="ca57b-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a><span data-ttu-id="ca57b-112">Créer une application console .NET</span><span class="sxs-lookup"><span data-stu-id="ca57b-112">Create a .NET console application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca57b-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca57b-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ca57b-114">Suivez [ces instructions ](/dotnet/core/tutorials/with-visual-studio) pour créer une application console portant le nom *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="ca57b-114">Follow the instructions [here](/dotnet/core/tutorials/with-visual-studio) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ca57b-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ca57b-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ca57b-116">Suivez [ces instructions ](/dotnet/core/tutorials/with-visual-studio-code) pour créer une application console portant le nom *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="ca57b-116">Follow the instructions [here](/dotnet/core/tutorials/with-visual-studio-code) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ca57b-117">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ca57b-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ca57b-118">Suivez [ces instructions ](/dotnet/core/tutorials/using-on-mac-vs-full-solution) pour créer une application console portant le nom *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="ca57b-118">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a><span data-ttu-id="ca57b-119">Ajouter les packages nécessaires</span><span class="sxs-lookup"><span data-stu-id="ca57b-119">Add required packages</span></span>

<span data-ttu-id="ca57b-120">Ajoutez les packages suivants au projet de client gRPC :</span><span class="sxs-lookup"><span data-stu-id="ca57b-120">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="ca57b-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), qui contient l’API C# du client C-core.</span><span class="sxs-lookup"><span data-stu-id="ca57b-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="ca57b-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), qui contient des API de messages protobuf pour C#.</span><span class="sxs-lookup"><span data-stu-id="ca57b-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="ca57b-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), qui contient la prise en charge des outils C# pour les fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="ca57b-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="ca57b-124">Le package d’outils n’est pas nécessaire lors de l’exécution. La dépendance est donc marquée avec `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="ca57b-124">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="ca57b-125">Les packages peuvent être ajoutés en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="ca57b-125">Packages can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca57b-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca57b-126">Visual Studio</span></span>](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="ca57b-127">Option de la console du Gestionnaire de package pour installer des packages</span><span class="sxs-lookup"><span data-stu-id="ca57b-127">PMC option to install packages</span></span>

* <span data-ttu-id="ca57b-128">Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="ca57b-128">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="ca57b-129">Dans la fenêtre **Console du Gestionnaire de Package**, accédez au répertoire où se trouve le fichier *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ca57b-129">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="ca57b-130">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ca57b-130">Run the following command:</span></span>

    ```powershell
    Install-Package Grpc.Core
    ```

* <span data-ttu-id="ca57b-131">Réexécutez la commande `Install-Package` pour Google.Protobuf et Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="ca57b-131">Repeat the `Install-Package` for Google.Protobuf and Grpc.Tools</span></span>

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="ca57b-132">Option Gérer les packages NuGet pour installer les packages</span><span class="sxs-lookup"><span data-stu-id="ca57b-132">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="ca57b-133">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ca57b-133">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="ca57b-134">Affectez la valeur « nuget.org » à **Source du package**.</span><span class="sxs-lookup"><span data-stu-id="ca57b-134">Set the **Package source** to "nuget.org"</span></span>
* <span data-ttu-id="ca57b-135">Entrez « Grpc.Core » dans la zone de recherche</span><span class="sxs-lookup"><span data-stu-id="ca57b-135">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="ca57b-136">Sélectionnez le package « Grpc.Core » sous l’onglet **Parcourir**, puis cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="ca57b-136">Select the "Grpc.Core" package from the **Browse** tab and click **Install**</span></span>
* <span data-ttu-id="ca57b-137">Répétez la procédure pour Google.Protobuf et Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="ca57b-137">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ca57b-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ca57b-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ca57b-139">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="ca57b-139">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
```

<span data-ttu-id="ca57b-140">Répétez la procédure pour Google.Protobuf et Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="ca57b-140">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ca57b-141">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ca57b-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ca57b-142">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="ca57b-142">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="ca57b-143">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="ca57b-143">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="ca57b-144">Entrez « Grpc.Core » dans la zone de recherche</span><span class="sxs-lookup"><span data-stu-id="ca57b-144">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="ca57b-145">Sélectionnez le package « Grpc.Core » dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="ca57b-145">Select the "Grpc.Core" package from the results pane and click **Add Package**</span></span>
* <span data-ttu-id="ca57b-146">Répétez la procédure pour Google.Protobuf et Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="ca57b-146">Repeat for Google.Protobuf and Grpc.Tools</span></span>

---

## <a name="add-the-greetproto-file"></a><span data-ttu-id="ca57b-147">Ajouter le fichier greet.proto</span><span class="sxs-lookup"><span data-stu-id="ca57b-147">Add the greet.proto file</span></span>

<span data-ttu-id="ca57b-148">Copiez le fichier **Protos\greet.proto** du service Greeter gRPC vers le projet du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="ca57b-148">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span> <span data-ttu-id="ca57b-149">Ajoutez le fichier **greet.proto** au groupe d’éléments `<Protobuf>` du fichier projet GrpcGreeterClient :</span><span class="sxs-lookup"><span data-stu-id="ca57b-149">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> <span data-ttu-id="ca57b-150">Vous pouvez ouvrir le fichier projet GrpcGreeterClient en double-cliquant dessus et en sélectionnant l’option **Modifier GrpcGreeterClient.csproj** dans le menu déroulant.</span><span class="sxs-lookup"><span data-stu-id="ca57b-150">You can open the project file of GrpcGreeterClient by right-clicking the project and selecting the **Edit GrpcGreeterClient.csproj** option from the dropdown menu.</span></span>
>
> ![Nouvelle application web ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> <span data-ttu-id="ca57b-152">Vous pouvez aussi accéder au répertoire GrpcGreeterClient et modifier `GrpcGreeterClient.csproj` dans votre éditeur favori.</span><span class="sxs-lookup"><span data-stu-id="ca57b-152">Alternatively, you can navigate to the GrpcGreeterClient directory and edit the `GrpcGreeterClient.csproj` with your favorite editor.</span></span>

<span data-ttu-id="ca57b-153">L’attribut `GrpcServices="Client"` est ajouté afin que seules les ressources du client C# soient générées pour le fichier protobuf inclus.</span><span class="sxs-lookup"><span data-stu-id="ca57b-153">The `GrpcServices="Client"` attribute is added so that only the C# client assets are generated for the included protobuf file.</span></span> <span data-ttu-id="ca57b-154">Générez le projet client pour déclencher la génération des ressources du client C#.</span><span class="sxs-lookup"><span data-stu-id="ca57b-154">Build the client project to trigger the generation of the C# client assets.</span></span>

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a><span data-ttu-id="ca57b-155">Créer un GreeterClient et invoquer l’appel d’unaire SayHello</span><span class="sxs-lookup"><span data-stu-id="ca57b-155">Create a GreeterClient and invoke the SayHello unary call</span></span>

<span data-ttu-id="ca57b-156">Ajoutez le code suivant à la méthode `Main` dans le fichier `Program.cs` du projet du client gRPC :</span><span class="sxs-lookup"><span data-stu-id="ca57b-156">Add the following code to `Main` method of the `Program.cs` file of the gRPC client project:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="ca57b-157">Pour accéder aux types nécessaires, vous devez utiliser les instructions using suivantes :</span><span class="sxs-lookup"><span data-stu-id="ca57b-157">To access the required types the following using statements are required:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

<span data-ttu-id="ca57b-158">GreeterClient est créé en instanciant un `Channel` contenant les informations nécessaires à la création de la connexion au service gRPC, et en l’utilisant pour construire le `GreeterClient` :</span><span class="sxs-lookup"><span data-stu-id="ca57b-158">The GreeterClient is created by instantiating a `Channel` containing the information for creating the connection to the gRPC service and using it to construct the `GreeterClient`:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

<span data-ttu-id="ca57b-159">GreeterClient contient l’appel d’unaire `SayHello` qui peut être invoqué de façon asynchrone :</span><span class="sxs-lookup"><span data-stu-id="ca57b-159">The GreeterClient contains the unary call `SayHello` which can be invoked asynchronously:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

<span data-ttu-id="ca57b-160">Les résultats de l’appel `SayHello` sont stockés dans `reply` qui peut ensuite être affiché :</span><span class="sxs-lookup"><span data-stu-id="ca57b-160">The results of the `SayHello` call is stored in `reply` which can then be displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

<span data-ttu-id="ca57b-161">Le `Channel` utilisé par le client doit s’arrêter lorsque les opérations ont fini de libérer toutes les ressources :</span><span class="sxs-lookup"><span data-stu-id="ca57b-161">The `Channel` used by the client should be shut down when operations have finished to release all resources:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> <span data-ttu-id="ca57b-162">Vous devez générer le projet avant que les types de l’espace de noms **Greeter** puissent être résolus.</span><span class="sxs-lookup"><span data-stu-id="ca57b-162">You will need to build the project before the types in the **Greeter** namespace can be resolved.</span></span> <span data-ttu-id="ca57b-163">Ces types sont générés automatiquement avec la build et ne sont pas disponibles tant qu’une build n’est pas exécutée.</span><span class="sxs-lookup"><span data-stu-id="ca57b-163">These types are generated automatically during build and are not be available before a build is run.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="ca57b-164">Tester le client gRPC avec le service Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="ca57b-164">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca57b-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca57b-165">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ca57b-166">Vérifiez que le service Greeter créé dans le tutoriel précédent est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="ca57b-166">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="ca57b-167">Une fois que le service est en cours d’exécution, revenez au projet **GrpcGreeterClient** pour le définir comme projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="ca57b-167">Once the service is running, return to the **GrpcGreeterClient** project set it as the Startup Project.</span></span> <span data-ttu-id="ca57b-168">Appuyez sur Ctrl+F5 pour exécuter le client sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="ca57b-168">Press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="ca57b-169">Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="ca57b-169">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="ca57b-170">Le service envoie un message « Hello GreeterClient » en tant que réponse, qui s’affiche dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ca57b-170">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="ca57b-172">Le service enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ca57b-172">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="ca57b-174">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ca57b-174">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="ca57b-175">Vérifiez que le service Greeter créé dans le tutoriel précédent est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="ca57b-175">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="ca57b-176">Exécutez le projet client GrpcGreeter.Server à partir d’une ligne de commande distincte à l’aide de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="ca57b-176">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="ca57b-177">Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ».</span><span class="sxs-lookup"><span data-stu-id="ca57b-177">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="ca57b-178">Le service envoie un message « Hello GreeterClient » en tant que réponse, qui s’affiche dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ca57b-178">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="ca57b-179">Le service enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ca57b-179">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="ca57b-180">Examinez les fichiers du projet gRPC</span><span class="sxs-lookup"><span data-stu-id="ca57b-180">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="ca57b-181">Fichier client gRPC GrpcGreeterClient :</span><span class="sxs-lookup"><span data-stu-id="ca57b-181">gRPC client GrpcGreeterClient file:</span></span>

<span data-ttu-id="ca57b-182">*Program.cs* contient le point d’entrée et la logique du client gRPC.</span><span class="sxs-lookup"><span data-stu-id="ca57b-182">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ca57b-183">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ca57b-183">Additional resources</span></span>

<span data-ttu-id="ca57b-184">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ca57b-184">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ca57b-185">Créez un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="ca57b-185">Create a gRPC client.</span></span>
> * <span data-ttu-id="ca57b-186">Exécutez le service sur le service Greeter gRPC créé dans le tutoriel précédent.</span><span class="sxs-lookup"><span data-stu-id="ca57b-186">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="ca57b-187">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="ca57b-187">Examine the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ca57b-188">Précédent : Créer un service gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="ca57b-188">Previous: Create a gRPC Greeter Service</span></span>](xref:tutorials/grpc/grpc-start)
