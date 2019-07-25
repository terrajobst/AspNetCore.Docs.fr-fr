---
title: Créer un serveur et un client gRPC .NET Core dans ASP.NET Core
author: juntaoluo
description: Ce tutoriel montre comment créer un service gRPC et un client gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de streaming.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 3e90e3b17186757fe157fb6641888786bb7a0df2
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412524"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a>Tutoriel : Créer un serveur et un client gRPC dans ASP.NET Core

Par [John Luo](https://github.com/juntaoluo)

Ce tutoriel montre comment créer un client [gRPC](https://grpc.io/docs/guides/) .NET Core et un serveur gRPC ASP.NET Core.

À la fin, vous disposerez d’un client gRPC qui communique avec le service Greeter gRPC.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créer un serveur gRPC.
> * Créez un client gRPC.
> * Tester le service du client gRPC avec le service Greeter gRPC.

## <a name="prerequisites"></a>Prérequis

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a>Créer un service gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application web ASP.NET Core**.
* Sélectionnez **Suivant**.
* Nommez le projet **GrpcGreeter**. Il est important de nommer le projet *GrpcGreeter* pour que les espaces de noms correspondent quand vous copiez et collez du code.
* Sélectionnez **Créer**.
* Dans la boîte de dialogue **Créer une application web ASP.NET Core** :
  * Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans les menus déroulants. 
  * Sélectionnez le modèle **Service gRPC**.
  * Sélectionnez **Créer**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Accédez à un répertoire (`cd`) destiné à contenir le projet.
* Exécutez les commandes suivantes :

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * La commande `dotnet new` crée un nouveau service gRPC dans le dossier *GrpcGreeter*.
  * La commande `code` ouvre le dossier *GrpcGreeter* dans une nouvelle instance de Visual Studio Code.

  Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « GrpcGreeter ». Faut-il les ajouter ?**
* Sélectionnez **Oui**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

À partir d’un terminal, exécutez les commandes suivantes :

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un service gRPC.

### <a name="open-the-project"></a>Ouvrir le projet

Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *GrpcGreeter.sln*.

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a>Exécuter le service

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Appuyez sur `Ctrl+F5` pour exécuter le service gRPC sans le débogueur.

  Visual Studio exécute le service dans une invite de commandes.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

* Exécutez le projet gRPC Greeter *GrpcGreeter* à partir de la ligne de commande à l’aide de `dotnet run`.

<!-- End of combined VS/Mac tabs -->

---

Les journaux indiquent que le service écoute sur `https://localhost:5001`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a>Examiner les fichiers projet

Fichiers projet *GrpcGreeter* :

* *greet.proto* : le fichier *Protos/greet.proto* définit le service gRPC `Greeter` et est utilisé pour générer les ressources du serveur gRPC. Pour plus d’informations, consultez [Introduction à gRPC](xref:grpc/index).
* Dossier *Services* : contient l’implémentation du service `Greeter`.
* *appSettings.json* : contient des données de configuration, telles que le protocole utilisé par Kestrel. Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.
* *Program.cs* : contient le point d’entrée du service gRPC. Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.
* *Startup.cs* : contient le code qui configure le comportement de l’application. Pour plus d’informations, consultez [Démarrage des applications](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Créer le client gRPC dans une application console .NET

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Ouvrez une deuxième instance de Visual Studio.
* Sélectionnez **Fichier** > **Nouveau** > **Projet** dans la barre de menus.
* Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application console (.NET Core)** .
* Sélectionnez **Suivant**.
* Dans la zone de texte **Nom**, entrez « GrpcGreeterClient ».
* Sélectionnez **Créer**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Accédez à un répertoire (`cd`) destiné à contenir le projet.
* Exécutez les commandes suivantes :

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Suivez [ces instructions ](/dotnet/core/tutorials/using-on-mac-vs-full-solution) pour créer une application console portant le nom *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a>Ajouter les packages nécessaires

Le projet client gRPC requiert les packages suivants :

* [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), qui contient le client .NET Core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), qui contient des API de messages protobuf pour C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), qui contient la prise en charge des outils C# pour les fichiers protobuf. Le package d’outils n’est pas nécessaire lors de l’exécution. La dépendance est donc marquée avec `PrivateAssets="All"`.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Installez les packages à l’aide de la console PMC (console du Gestionnaire de package) ou à partir de Gérer les packages NuGet.

#### <a name="pmc-option-to-install-packages"></a>Option de la console du Gestionnaire de package pour installer des packages

* Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.
* Dans la fenêtre **Console du Gestionnaire de Package**, accédez au répertoire où se trouve le fichier *GrpcGreeterClient.csproj*.
* Exécutez les commandes suivantes :

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Option Gérer les packages NuGet pour installer les packages

* Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.
* Sélectionnez l’onglet **Parcourir**.
* Entrez **Grpc.Core** dans la zone de recherche.
* Sélectionnez le package **Grpc.Core** sous l’onglet **Parcourir** et sélectionnez **Installer**.
* Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Exécutez les commandes suivantes à partir du **Terminal intégré** :

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Cliquez avec le bouton droit sur le dossier **Packages** dans **Panneau Solutions** > **Ajouter des packages**.
* Entrez **Grpc.Net.Client** dans la zone de recherche.
* Sélectionnez le package **Grpc.Net.Client** dans le volet de résultats, puis sélectionnez **Ajouter un package**
* Répétez ces étapes pour `Google.Protobuf` et `Grpc.Tools`.

---

### <a name="add-greetproto"></a>Ajouter greet.proto

* Créez un dossier **Protos** dans le projet du client gRPC.
* Copiez le fichier **Protos\greet.proto** du service Greeter gRPC vers le projet du client gRPC.
* Modifiez le fichier projet *GrpcGreeterClient.csproj* :

  # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

  Cliquez avec le bouton droit sur le projet et sélectionnez **Modifier le fichier de projet**.

  # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

  Sélectionnez le fichier *GrpcGreeterClient.csproj*.

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

  Cliquez avec le bouton droit sur le projet, puis sélectionnez **Outils > Modifier le fichier**.

  ---

* Ajoutez un groupe d’éléments avec un élément `<Protobuf>` qui fait référence au fichier **greet.proto** :

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a>Créer le client Greeter

Générez le projet pour créer les types dans l’espace de noms `GrpcGreeter`. Les types `GrpcGreeter` sont générés automatiquement par le processus de génération.

Mettez à jour le fichier *Program.cs* du client gRPC par le code suivant :

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

*Program.cs* contient le point d’entrée et la logique du client gRPC.

Le client Greeter est créé en :

* Instanciation d’un `HttpClient` qui contient les informations permettant de créer la connexion au service gRPC.
* Utilisant le `HttpClient` pour construire le client Greeter :

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

Le client Greeter appelle la méthode `SayHello` asynchrone. Le résultat de l’appel `SayHello` s’affiche :

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Tester le client gRPC avec le service Greeter gRPC

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans le service Greeter, appuyez sur `Ctrl+F5` pour démarrer le serveur sans le débogueur.
* Dans le projet `GrpcGreeterClient`, appuyez sur `Ctrl+F5` pour démarrer le serveur sans le débogueur.

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

* Démarrez le service Greeter.
* Démarrez le client.

<!-- End of combined VS/Mac tabs -->

---

Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ». Le service envoie le message « Hello GreeterClient » comme réponse. La réponse « Hello GreeterClient » s’affiche dans l’invite de commandes :

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Le service gRPC enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.

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

### <a name="next-steps"></a>Étapes suivantes

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
