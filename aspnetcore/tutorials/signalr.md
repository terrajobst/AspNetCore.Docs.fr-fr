---
title: 'Tutoriel : Bien démarrer avec SignalR sur ASP.NET Core'
author: tdykstra
description: Dans ce tutoriel, vous allez créer une application de conversation qui utilise SignalR pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 8581628b3f0cf878b8cc1d0684046d22a8374af9
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523218"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Tutoriel : Bien démarrer avec SignalR sur ASP.NET Core

Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR. Vous apprenez à :

> [!div class="checklist"]
> * Créer une application web qui utilise SignalR sur ASP.NET Core.
> * Créer un hub SignalR sur le serveur.
> * Établir une connexion au hub SignalR à partir de clients JavaScript.
> * Utiliser le hub pour envoyer des messages de n’importe quel client vers tous les clients connectés.

À la fin, vous disposerez d’une application de conversation opérationnelle :

![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Prérequis

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 version 15.8 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **ASP.NET et développement web**
* [Kit SDK .NET Core 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [Kit SDK .NET Core 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [C# pour Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* [Visual Studio pour Mac version 7.5.4 ou ultérieure](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all) (inclus dans l’installation de Visual Studio)

---

## <a name="create-the-project"></a>Créer le projet

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Dans le menu, sélectionnez **Fichier > Nouveau projet**.

* Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**. Nommez le projet *SignalRChat*.

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.

* Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.1**, puis cliquez sur **OK**.

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Ouvrez un dossier que vous pouvez utiliser pour un nouveau projet.

* Dans le **Terminal intégré**, exécutez la commande suivante :

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le menu, sélectionnez **Fichier > Nouvelle solution**.

* Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).

* Sélectionnez **Suivant**.

* Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.

---

## <a name="add-the-signalr-client-library"></a>Ajouter la bibliothèque de client SignalR

La bibliothèque de serveur SignalR est comprise dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet. Pour ce tutoriel, vous utilisez le [Gestionnaire de bibliothèque (LibMan)](xref:client-side/libman/index) pour obtenir la bibliothèque cliente à partir de *unpkg*. [unpkg](https://unpkg.com/#/) est un [réseau de distribution de contenu](https://wikipedia.org/wiki/Content_delivery_network) qui peut fournir tout ce qui est trouvé dans [npm, le Gestionnaire de package Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.

* Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**. 

* Pour **Bibliothèque**, entrez _@aspnet/signalr@1_, puis sélectionnez la version la plus récente qui n’est pas en préversion.

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/libman1.png)

* Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.

* Définissez **Emplacement cible** sur *wwwroot/lib/signalr/*, puis sélectionnez **Installer**.

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/libman2.png)

  [LibMan](xref:client-side/libman/index) crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Dans le **Terminal intégré**, exécutez la commande suivante pour installer LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).

* Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan. Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Les paramètres spécifient les options suivantes :
  * Utilisez le fournisseur unpkg.
  * Copiez les fichiers vers la destination *wwwroot/lib/signalr*.
  * Copiez uniquement les fichiers spécifiés.

  La sortie se présente comme suit :

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).

* Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Les paramètres spécifient les options suivantes :
  * Utilisez le fournisseur unpkg.
  * Copiez les fichiers vers la destination *wwwroot/lib/signalr*.
  * Copiez uniquement les fichiers spécifiés.

  La sortie se présente comme suit :

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

## <a name="create-the-signalr-hub"></a>Créer le hub SignalR

Un [hub](xref:signalr/hubs) est une classe servant de pipeline global qui gère les communications client-serveur.

* Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.

* Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  La classe `ChatHub` hérite de la classe SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub). La classe `Hub` gère les connexions, les groupes et la messagerie.

  La méthode `SendMessage` peut être appelée par n’importe quel client connecté. Elle envoie le message reçu à tous les clients. Le code SignalR est asynchrone afin de fournir une scalabilité maximale.

## <a name="configure-the-project-to-use-signalr"></a>Configurer le projet pour utiliser SignalR

Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.

* Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Ces modifications ajoutent SignalR au système d’[injection de dépendances](xref:fundamentals/dependency-injection) au pipeline de [middleware](xref:fundamentals/middleware/index).

## <a name="create-the-signalr-client-code"></a>Créer le code client SignalR

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

* Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.

---

* Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.

* Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.

  Le nom et le message sont affichés instantanément dans les deux pages.

  ![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console. Vous pouvez observer des erreurs liées à votre code HTML et JavaScript. Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé. Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.
> ![Erreur de fichier SignalR.js introuvable](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Étapes suivantes

Si vous souhaitez que les clients se connectent à une application SignalR à partir de différents domaines, vous devez activer le Partage des ressources cross-origin (CORS). Pour plus d’informations, consultez [Partage des ressources cross-origin](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Pour en savoir plus sur SignalR, les hubs et les clients JavaScript, consultez ces ressources :

* [Introduction à SignalR pour ASP.NET Core](xref:signalr/introduction)
* [Utilisation des hubs dans SignalR pour ASP.NET Core](xref:signalr/hubs)
* [Client JavaScript SignalR ASP.NET Core](xref:signalr/javascript-client)
