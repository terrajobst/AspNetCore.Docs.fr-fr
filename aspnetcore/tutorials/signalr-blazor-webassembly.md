---
title: Utiliser ASP.NET Core SignalR avec Blazor webassembly
author: guardrex
description: Créez une application de conversation qui utilise ASP.NET Core SignalR avec Blazor webassembly.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: d3605c0823e9ec3ce34fb781da66a7470aa00622
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034177"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a>Utiliser ASP.NET Core Signalr avec le webassembly éblouissant

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Ce didacticiel enseigne les bases de la création d’une application en temps réel à l’aide de Signalr avec le webassembly éblouissant. Vous allez apprendre à effectuer les actions suivantes :

> [!div class="checklist"]
> * Créer un projet d’application hébergée webassembly éblouissant
> * Ajouter la bibliothèque de client SignalR
> * Ajouter un concentrateur Signalr
> * Ajouter des services Signalr et un point de terminaison pour le concentrateur Signalr
> * Ajouter du code de composant Razor pour la conversation

À la fin de ce didacticiel, vous disposerez d’une application de conversation de travail.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Conditions préalables requises

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a>Créer un projet d’application webassembly éblouissant hébergé

Installez le modèle de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) . Le package [Microsoft. AspNetCore. éblouissant. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) a une préversion alors que l’assembly éblouissant est en version préliminaire. Dans une interface de commande, exécutez la commande suivante :

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
```

Suivez les instructions de votre choix d’outils :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Créez un projet.

1. Sélectionnez l' **application éblouissant** , puis sélectionnez **suivant**.

1. Tapez « BlazorSignalRApp » dans le champ **nom du projet** . Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet. Sélectionnez **Create** (Créer).

1. Choisissez le modèle **application éblouissant Webassembly** .

1. Sous **avancé**, activez la case à cocher **ASP.net Core hébergé** .

1. Sélectionnez **Create** (Créer).

> [!NOTE]
> Si vous avez mis à niveau ou installé une nouvelle version de Visual Studio et que le modèle de webassembly éblouissant n’apparaît pas dans l’interface utilisateur VS, réinstallez le modèle à l’aide de la commande `dotnet new` présentée précédemment.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Dans une interface de commande, exécutez la commande suivante :

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. Dans Visual Studio Code, ouvrez le dossier du projet de l’application.

1. Quand la boîte de dialogue semble ajouter des éléments multimédias pour générer et déboguer l’application, sélectionnez **Oui**. Visual Studio Code ajoute automatiquement le dossier *. vscode* avec les fichiers *Launch. JSON* et *Tasks. JSON* générés.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

1. Dans une interface de commande, exécutez la commande suivante :

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. Dans Visual Studio pour Mac, ouvrez le projet en accédant au dossier du projet et en ouvrant le fichier solution du projet ( *. sln*).

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

Dans une interface de commande, exécutez la commande suivante :

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a>Ajouter la bibliothèque de client SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **BlazorSignalRApp. client** , puis sélectionnez **gérer les packages NuGet**.

1. Dans la boîte de dialogue **gérer les packages NuGet** , vérifiez que la **source du package** est définie sur *NuGet.org*.

1. Avec l’option **Parcourir** sélectionnée, tapez « Microsoft. AspNetCore. signalr. client » dans la zone de recherche.

1. Dans les résultats de la recherche, sélectionnez le package `Microsoft.AspNetCore.SignalR.Client`, puis sélectionnez **installer**.

1. Si la boîte de dialogue **aperçu des modifications** s’affiche, sélectionnez **OK**.

1. Si la boîte de dialogue acceptation de la **licence** s’affiche, sélectionnez **J’accepte** si vous acceptez les termes du contrat de licence.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

Dans le **Terminal intégré** (**Afficher** > **Terminal** à partir de la barre d’outils), exécutez les commandes suivantes :

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

1. Dans la barre latérale **solution** , cliquez avec le bouton droit sur le projet **BlazorSignalRApp. client** , puis sélectionnez **gérer les packages NuGet**.

1. Dans la boîte de dialogue **gérer les packages NuGet** , vérifiez que la liste déroulante source est définie sur *NuGet.org*.

1. Avec l’option **Parcourir** sélectionnée, tapez « Microsoft. AspNetCore. signalr. client » dans la zone de recherche.

1. Dans les résultats de la recherche, activez la case à cocher située en regard du package `Microsoft.AspNetCore.SignalR.Client`, puis sélectionnez **Ajouter un package**.

1. Si la boîte de dialogue acceptation de la **licence** s’affiche, sélectionnez **accepter** si vous acceptez les termes du contrat de licence.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

Dans une interface de commande, exécutez les commandes suivantes :

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a>Ajouter un concentrateur Signalr

Dans le projet **BlazorSignalRApp. Server** , créez un dossier *hubs* (plural) et ajoutez la classe de `ChatHub` suivante (*hubs/ChatHub. cs*) :

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a>Ajouter des services Signalr et un point de terminaison pour le concentrateur Signalr

1. Dans le projet **BlazorSignalRApp. Server** , ouvrez le fichier *Startup.cs* .

1. Ajoutez l’espace de noms de la classe `ChatHub` au début du fichier :

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. Ajoutez les services Signalr à `Startup.ConfigureServices`:

   ```csharp
   services.AddSignalR();
   ```

1. Dans `Startup.Configure` entre les points de terminaison pour l’itinéraire du contrôleur par défaut et le secours côté client, ajoutez un point de terminaison pour le Hub :

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a>Ajouter du code de composant Razor pour la conversation

1. Dans le projet **BlazorSignalRApp. client** , ouvrez le fichier *pages/index. Razor* .

1. Remplacez le balisage par le code suivant :

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a>Exécuter l’application

1. Suivez les instructions pour vos outils :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Dans **Explorateur de solutions**, sélectionnez le projet **BlazorSignalRApp. Server** . Appuyez sur **CTRL + F5** pour exécuter l’application sans débogage.

1. Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.

1. Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**. Le nom et le message s’affichent instantanément sur les deux pages :

   ![L’exemple d’application signalr du webassembly éblouissant est ouvert dans deux fenêtres de navigateur qui affichent les messages échangés.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Devis : *Star Trek VI : le pays indécouvert* [&copy;1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Sélectionnez **Déboguer** > **exécuter sans débogage** à partir de la barre d’outils.

1. Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.

1. Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**. Le nom et le message s’affichent instantanément sur les deux pages :

   ![L’exemple d’application signalr du webassembly éblouissant est ouvert dans deux fenêtres de navigateur qui affichent les messages échangés.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Devis : *Star Trek VI : le pays indécouvert* [&copy;1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

1. Dans la barre latérale **solution** , sélectionnez le projet **BlazorSignalRApp. Server** . Dans le menu, sélectionnez **exécuter** > **Démarrer sans débogage**.

1. Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.

1. Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**. Le nom et le message s’affichent instantanément sur les deux pages :

   ![L’exemple d’application signalr du webassembly éblouissant est ouvert dans deux fenêtres de navigateur qui affichent les messages échangés.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Devis : *Star Trek VI : le pays indécouvert* [&copy;1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

1. Dans une interface de commande, exécutez les commandes suivantes :

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.

1. Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**. Le nom et le message s’affichent instantanément sur les deux pages :

   ![L’exemple d’application signalr du webassembly éblouissant est ouvert dans deux fenêtres de navigateur qui affichent les messages échangés.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Devis : *Star Trek VI : le pays indécouvert* [&copy;1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

---

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Créer un projet d’application hébergée Blazor webassembly
> * Ajouter la bibliothèque cliente SignalR
> * Ajouter un hub SignalR
> * Ajouter des services de SignalR et un point de terminaison pour le Hub SignalR
> * Ajouter du code de composant Razor pour la conversation

Pour en savoir plus sur la création d’applications Blazor, consultez la documentation Blazor :

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:signalr/introduction>
