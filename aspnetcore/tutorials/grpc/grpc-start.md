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
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Tutoriel : Bien démarrer avec le service gRPC dans ASP.NET Core

Par [John Luo](https://github.com/juntaoluo)

Ce tutoriel décrit les principes fondamentaux liés à la création d’un service gRPC sur ASP.NET Core.

À la fin, vous disposerez d’un service gRPC qui renvoie les salutations.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créez un service gRPC.
> * Exécutez le service.
> * Examiner les fichiers projet.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>Créer un service gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Créez une application web ASP.NET Core.
  ![Nouvelle application web ASP.NET Core](grpc-start/_static/np_3_0.1.png)
* Nommez le projet **GrpcGreeter**. Il est important de nommer le projet *GrpcGreeter* pour que les espaces de noms correspondent quand vous copiez et collez du code.
  ![Nouvelle application web ASP.NET Core](grpc-start/_static/np_3_0.2.png)
* Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans la liste déroulante. Choisissez le modèle **Service gRPC**.

  Le projet de démarrage suivant est créé :

  ![Explorateur de solutions](grpc-start/_static/se3.0.png)

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

### <a name="test-the-service"></a>Tester le service

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Vérifiez le paramètre **GrpcGreeter.Server** est défini comme projet de démarrage, et appuyez sur Ctrl + F5 pour exécuter le service gRPC sans le débogueur.

  Visual Studio exécute le service dans une invite de commandes. Les journaux indiquent que le service a démarré l’écoute sur `http://localhost:50051`.

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/server_start.png)

* Une fois le service en cours d’exécution, définissez le paramètre **GrpcGreeter.Client** est défini comme projet de démarrage, et appuyez sur Ctrl + F5 pour exécuter le client sans le débogueur.

  Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ». Le service envoie un message « Hello GreeterClient » en tant que réponse, qui s’affiche dans l’invite de commandes.

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/client.png)

  Le service enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.

  ![Nouvelle application web ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

* Exécutez le projet de serveur GrpcGreeter.Server à partir de la ligne de commande à l’aide de `dotnet run`. Les journaux indiquent que le service a démarré l’écoute sur `http://localhost:50051`.

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

* Exécutez le projet client GrpcGreeter.Server à partir d’une ligne de commande distincte à l’aide de `dotnet run`.

Le client envoie une salutation au service avec un message contenant son nom, « GreeterClient ». Le service envoie un message « Hello GreeterClient » en tant que réponse, qui s’affiche dans l’invite de commandes.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Le service enregistre les détails de l’appel réussi dans les journaux écrits dans l’invite de commandes.

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

### <a name="examine-the-project-files-of-the-grpc-project"></a>Examinez les fichiers du projet gRPC

Fichiers GrpcGreeter.Server :

* greet.proto : le fichier *Protos/greet.proto* définit le service gRPC `Greeter` et est utilisé pour générer les ressources du serveur gRPC. Pour plus d'informations, consultez <xref:grpc/index>.
* Dossier *Services* : contient l’implémentation du service `Greeter`.
* *appSettings.json* : contient des données de configuration, telles que le protocole utilisé par Kestrel. Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.
* *Program.cs* : contient le point d’entrée du service gRPC. Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.
* Startup.cs

contient le code qui configure le comportement de l’application. Pour plus d'informations, consultez <xref:fundamentals/startup>.

Fichier client gRPC GrpcGreeter.Client :

*Program.cs* contient le point d’entrée et la logique du client gRPC.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créez un service gRPC.
> * Exécutez le service et un client pour tester le service.
> * Examiner les fichiers projet.
