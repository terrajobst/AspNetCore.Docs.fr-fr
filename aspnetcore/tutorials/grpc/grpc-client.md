---
title: 'Tutoriel : Créer un client gRPC .NET Core'
author: juntaoluo
description: Cette série de tutoriels montre comment créer un service gRPC sur ASP.NET Core. Découvrez comment créer un projet de service gRPC, modifier un fichier proto et ajouter un appel duplex de streaming.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: 031afbfaf097c518a85400b0b6abbc135c1bc611
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59674144"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a>Tutoriel : Créer un client gRPC .NET Core

Par [John Luo](https://github.com/juntaoluo)

Ce tutoriel explique comment créer un client [gRPC](https://grpc.io/docs/guides/) .NET Core qui peut communiquer avec les services gRPC.

À la fin, vous disposerez d’un client gRPC qui communique avec le service Greeter gRPC.

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créez un client gRPC.
> * Exécutez le service sur le service Greeter gRPC créé dans le tutoriel précédent.
> * Examiner les fichiers projet.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a>Créer une application console .NET

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Suivez [ces instructions ](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio) pour créer une application console portant le nom *GrpcGreeterClient*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Suivez [ces instructions ](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) pour créer une application console portant le nom *GrpcGreeterClient*.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Suivez [ces instructions ](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution) pour créer une application console portant le nom *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a>Ajouter les packages nécessaires

Ajoutez les packages suivants au projet de client gRPC :

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), qui contient l’API C# du client C-core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), qui contient des API de messages protobuf pour C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), qui contient la prise en charge des outils C# pour les fichiers protobuf. Le package d’outils n’est pas nécessaire lors de l’exécution. La dépendance est donc marquée avec `PrivateAssets="All"`.

Les packages peuvent être ajoutés en adoptant l’une des approches suivantes :

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a>Option de la console du Gestionnaire de package pour installer des packages

* Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.
* Dans la fenêtre **Console du Gestionnaire de Package**, accédez au répertoire où se trouve le fichier *GrpcGreeterClient.csproj*.
* Exécutez la commande suivante :

    ```powershell
    Install-Package Grpc.Core
    ```

* Réexécutez la commande `Install-Package` pour Google.Protobuf et Grpc.Tools.

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a>Option Gérer les packages NuGet pour installer les packages

* Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.
* Affectez la valeur « nuget.org » à **Source du package**.
* Entrez « Grpc.Core » dans la zone de recherche
* Sélectionnez le package « Grpc.Core » sous l’onglet **Parcourir**, puis cliquez sur **Installer**.
* Répétez la procédure pour Google.Protobuf et Grpc.Tools.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Exécutez la commande suivante à partir du **Terminal intégré** :

```console
dotnet add TodoApi.csproj package Grpc.Core
```

Répétez la procédure pour Google.Protobuf et Grpc.Tools.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.
* Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.
* Entrez « Grpc.Core » dans la zone de recherche
* Sélectionnez le package « Grpc.Core » dans le volet de résultats, puis cliquez sur **Ajouter un package**.
* Répétez la procédure pour Google.Protobuf et Grpc.Tools.

---

## <a name="add-the-greetproto-file"></a>Ajouter le fichier greet.proto

Copiez le fichier **Protos\greet.proto** du service Greeter gRPC vers le projet du client gRPC. Ajoutez le fichier **greet.proto** au groupe d’éléments `<Protobuf>` du fichier projet GrpcGreeterClient :

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> Vous pouvez ouvrir le fichier projet GrpcGreeterClient en double-cliquant dessus et en sélectionnant l’option **Modifier GrpcGreeterClient.csproj** dans le menu déroulant.
>
> ![Nouvelle application web ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> Vous pouvez aussi accéder au répertoire GrpcGreeterClient et modifier `GrpcGreeterClient.csproj` dans votre éditeur favori.

L’attribut `GrpcServices="Client"` est ajouté afin que seules les ressources du client C# soient générées pour le fichier protobuf inclus. Générez le projet client pour déclencher la génération des ressources du client C#.

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a>Créer un GreeterClient et invoquer l’appel d’unaire SayHello

Ajoutez le code suivant à la méthode `Main` dans le fichier `Program.cs` du projet du client gRPC :

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

Pour accéder aux types nécessaires, vous devez utiliser les instructions using suivantes :

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

GreeterClient est créé en instanciant un `Channel` contenant les informations nécessaires à la création de la connexion au service gRPC, et en l’utilisant pour construire le `GreeterClient` :

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

GreeterClient contient l’appel d’unaire `SayHello` qui peut être invoqué de façon asynchrone :

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

Les résultats de l’appel `SayHello` sont stockés dans `reply` qui peut ensuite être affiché :

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

Le `Channel` utilisé par le client doit s’arrêter lorsque les opérations ont fini de libérer toutes les ressources :

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> Vous devez générer le projet avant que les types de l’espace de noms **Greeter** puissent être résolus. Ces types sont générés automatiquement avec la build et ne sont pas disponibles tant qu’une build n’est pas exécutée.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Tester le client gRPC avec le service Greeter gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Vérifiez que le service Greeter créé dans le tutoriel précédent est en cours d’exécution.

* Une fois que le service est en cours d’exécution, revenez au projet **GrpcGreeterClient** pour le définir comme projet de démarrage. Appuyez sur Ctrl+F5 pour exécuter le client sans le débogueur.

  Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ». Le service envoie un message « Hello GreeterClient » en tant que réponse, qui s’affiche dans l’invite de commandes.

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/client.png)

  Le service enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

* Vérifiez que le service Greeter créé dans le tutoriel précédent est en cours d’exécution.

* Exécutez le projet client GrpcGreeter.Server à partir d’une ligne de commande distincte à l’aide de `dotnet run`.

Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ». Le service envoie un message « Hello GreeterClient » en tant que réponse, qui s’affiche dans l’invite de commandes.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Le service enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.

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

### <a name="examine-the-project-files-of-the-grpc-project"></a>Examinez les fichiers du projet gRPC

Fichier client gRPC GrpcGreeterClient :

*Program.cs* contient le point d’entrée et la logique du client gRPC.

## <a name="additional-resources"></a>Ressources supplémentaires

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créez un client gRPC.
> * Exécutez le service sur le service Greeter gRPC créé dans le tutoriel précédent.
> * Examiner les fichiers projet.

> [!div class="step-by-step"]
> [Précédent : Créer un service gRPC Greeter](xref:tutorials/grpc/grpc-start)
