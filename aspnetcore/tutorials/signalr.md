---
title: Bien démarrer avec ASP.NET Core SignalR
author: tdykstra
description: Dans ce tutoriel, vous créez une application de conversation qui utilise ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: c52041b34d6c9d1d8f06f980c900b805a0933293
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861979"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Tutoriel : Bien démarrer avec ASP.NET Core SignalR

Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR. Vous apprenez à :

> [!div class="checklist"]
> * Créez un projet web.
> * Ajouter la bibliothèque de client SignalR.
> * Créer un hub SignalR.
> * Configurer le projet pour utiliser SignalR.
> * Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.

À la fin, vous disposerez d’une application de conversation opérationnelle :

![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).

> [!NOTE]
> Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.  Si vous avez quelques minutes pour essayer un exercice de recherche de 7 différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).


[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a>Créer un projet web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Dans le menu, sélectionnez **Fichier > Nouveau projet**.

* Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**. Nommez le projet *SignalRChat*.

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.

* Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.2**, puis cliquez sur **OK**.

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.

* Exécutez les commandes suivantes :

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le menu, sélectionnez **Fichier > Nouvelle solution**.

* Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).

* Sélectionnez **Suivant**.

* Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.

---

## <a name="add-the-signalr-client-library"></a>Ajouter la bibliothèque de client SignalR

La bibliothèque de serveur SignalR est incluse dans le métapackage `Microsoft.AspNetCore.App`. La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet. Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*. unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.

* Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**. 

* Pour **Bibliothèque**, entrez `@aspnet/signalr@1`, puis sélectionnez la version la plus récente qui n’est pas en préversion.

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/libman1.png)

* Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.

* Définissez **Emplacement cible** sur *wwwroot/lib/signalr/*, puis sélectionnez **Installer**.

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/libman2.png)

  LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan. Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Les paramètres spécifient les options suivantes :
  * Utilisez le fournisseur unpkg.
  * Copiez les fichiers vers la destination *wwwroot/lib/signalr*.
  * Copiez uniquement les fichiers spécifiés.

  La sortie se présente comme suit :

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).

* Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Les paramètres spécifient les options suivantes :
  * Utilisez le fournisseur unpkg.
  * Copiez les fichiers vers la destination *wwwroot/lib/signalr*.
  * Copiez uniquement les fichiers spécifiés.

  La sortie se présente comme suit :

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Créer un hub SignalR

Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.

* Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.

* Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  La classe `ChatHub` hérite la classe SignalR `Hub`. La classe `Hub` gère les connexions, les groupes et la messagerie.

  La méthode `SendMessage` peut être appelée par n’importe quel client connecté. Elle envoie le message reçu à tous les clients. Le code SignalR est asynchrone afin de fournir une scalabilité maximale.

## <a name="configure-signalr"></a>Configurer SignalR

Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.

* Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Ces modifications ajoutent SignalR au système d’injection de dépendances ASP.NET Core et au pipeline de middleware.

## <a name="add-signalr-client-code"></a>Ajouter le code client SignalR

* Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Le code précédent :

  * Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.
  * Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.
  * Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.

* Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Le code précédent :

  * Crée et lance une connexion.
  * Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.
  * Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.

## <a name="run-the-app"></a>Exécuter l'application

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dans le terminal intégré, exécutez la commande suivante :

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.

---

* Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.

* Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.

  Le nom et le message sont affichés instantanément dans les deux pages.

  ![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console. Vous pouvez observer des erreurs liées à votre code HTML et JavaScript. Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé. Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.
> ![Erreur de fichier SignalR.js introuvable](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Créer un projet application web.
> * Ajouter la bibliothèque de client SignalR.
> * Créer un hub SignalR.
> * Configurer le projet pour utiliser SignalR.
> * Ajouter le code qui utilise le hub pour envoyer des messages de n’importe quel client à tous les clients connectés.

Pour en savoir plus sur SignalR, consultez :

> [!div class="nextstepaction"]
> [Introduction à ASP.NET Core SignalR](xref:signalr/introduction)
