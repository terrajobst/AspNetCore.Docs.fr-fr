---
title: Bien démarrer avec ASP.NET Core SignalR
author: bradygaster
description: Dans ce tutoriel, vous créez une application de conversation qui utilise ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 10/03/2019
uid: tutorials/signalr
ms.openlocfilehash: 843416cf00c9241f8c05b1aba041ebbe7a05cf80
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037668"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Tutoriel : Bien démarrer avec ASP.NET Core SignalR

Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR. Vous apprenez à :

> [!div class="checklist"]
> * Créez un projet web.
> * Ajouter la bibliothèque de client SignalR.
> * Créer un hub SignalR.
> * Configurer le projet pour utiliser SignalR.
> * Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.

À la fin, vous disposerez d’une application de conversation opérationnelle :

![Exemple d’application SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a>Prérequis

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a>Créer un projet application web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Dans le menu, sélectionnez **Fichier > Nouveau projet**.

* Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application web ASP.NET Core**, puis **Suivant**.

* Dans la boîte de dialogue **Configurer votre nouveau projet**, sélectionnez *SignalRChat*, puis **Créer**.

* Dans la boîte de dialogue **créer une application web ASP.net Core** , sélectionnez **.net Core** et **ASP.net Core 3,0**. 

* Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages, puis **Créer**.

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.

* Exécutez les commandes suivantes :

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le menu, sélectionnez **Fichier > Nouvelle solution**.

* Sélectionnez **.NET Core > Application > Application web** (ne sélectionnez pas **Application web (modèle-vue-contrôleur)** ), puis **Suivant**.

* Veillez à sélectionner **.NET Core 3.0** comme **Framework cible**, puis sélectionnez **Suivant**.

* Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.

---

## <a name="add-the-signalr-client-library"></a>Ajouter la bibliothèque de client SignalR

La bibliothèque de serveur SignalR est incluse dans le framework partagé ASP.NET Core 3.0. La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet. Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*. unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.

* Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.

* Pour **Bibliothèque**, entrez `@microsoft/signalr@latest`.

* Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.

* Définissez **emplacement cible** sur *wwwroot/js/signalr/* , puis sélectionnez **installer**.

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  LibMan crée un dossier *wwwroot/js/signalr* et y copie les fichiers sélectionnés.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan. Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Les paramètres spécifient les options suivantes :
  * Utilisez le fournisseur unpkg.
  * Copiez les fichiers vers la destination *wwwroot/js/signalr* .
  * Copiez uniquement les fichiers spécifiés.

  La sortie se présente comme suit :

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).

* Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Les paramètres spécifient les options suivantes :
  * Utilisez le fournisseur unpkg.
  * Copiez les fichiers vers la destination *wwwroot/js/signalr* .
  * Copiez uniquement les fichiers spécifiés.

  La sortie se présente comme suit :

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Créer un hub SignalR

Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.

* Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.

* Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  La classe `ChatHub` hérite la classe SignalR `Hub`. La classe `Hub` gère les connexions, les groupes et la messagerie.

  La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients. Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel. Le code SignalR est asynchrone afin de fournir une scalabilité maximale.

## <a name="configure-signalr"></a>Configurer SignalR

Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.

* Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  Ces modifications ajoutent SignalR aux systèmes de routage et d’injection de dépendances ASP.NET Core.

## <a name="add-signalr-client-code"></a>Ajouter le code client SignalR

* Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  Le code précédent :

  * Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.
  * Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.
  * Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.

* Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  Le code précédent :

  * Crée et lance une connexion.
  * Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.
  * Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.

## <a name="run-the-app"></a>Exécuter l'application

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dans le terminal intégré, exécutez la commande suivante :

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.

---

* Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.

* Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.

  Le nom et le message sont affichés sur les deux pages instantanément.

  ![Exemple d’application SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console. Vous pouvez observer des erreurs liées à votre code HTML et JavaScript. Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé. Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.
>   ![Erreur de fichier SignalR.js introuvable](signalr/_static/3.x/f12-console.png)
> * Si vous recevez l’erreur ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY dans Chrome, exécutez les commandes suivantes pour mettre à jour votre certificat de développement :
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur SignalR, consultez :

> [!div class="nextstepaction"]
> [Introduction à ASP.NET Core SignalR](xref:signalr/introduction)
