---
title: 'Tutoriel : Bien démarrer avec SignalR sur ASP.NET Core'
author: tdykstra
description: Dans ce tutoriel, vous allez créer une application de conversation qui utilise SignalR pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055830"
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

* [Visual Studio 2017 version 15.7.3 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **ASP.NET et développement web**
* [Kit SDK .NET Core 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (Gestionnaire de package pour Node.js, utilisé pour la bibliothèque de client JavaScript SignalR.)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [Kit SDK .NET Core 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [C# pour Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (Gestionnaire de package pour Node.js, utilisé pour la bibliothèque de client JavaScript SignalR.)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* [Visual Studio pour Mac version 7.5.4 ou ultérieure](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all) (inclus dans l’installation de Visual Studio)
* [npm](https://www.npmjs.com/get-npm) (Gestionnaire de package pour Node.js, utilisé pour la bibliothèque de client JavaScript SignalR.)

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

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le menu, sélectionnez **Fichier > Nouvelle solution**.

* Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).

* Sélectionnez **Suivant**.

* Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.

---

## <a name="add-the-signalr-client-library"></a>Ajouter la bibliothèque de client SignalR

La bibliothèque de serveur SignalR est comprise dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). En revanche, vous devez obtenir la bibliothèque de client JavaScript à partir de [npm, le gestionnaire de package Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Dans la **Console du Gestionnaire de package**, accédez au dossier de projet (celui qui contient le fichier *SignalRChat.csproj*).

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. Basculez vers le nouveau dossier de projet.

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Dans le **Terminal**, accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).

---

* Exécutez l’initialiseur npm pour créer un fichier *package.json* :

  ```console
  npm init -y
  ```

  La commande crée une sortie similaire à l’exemple suivant :

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* Installez le package de bibliothèque de client :

  ```console
  npm install @aspnet/signalr
  ```

  La commande crée une sortie similaire à l’exemple suivant :

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

La commande `npm install` a téléchargé la bibliothèque de client JavaScript dans un sous-dossier sous *node_modules*. Copiez-la depuis cet emplacement vers un dossier sous *wwwroot* que vous pouvez référencer à partir de la page web de l’application de conversation.

* Créez un dossier *signalr* dans *wwwroot/lib*.

* Copiez le fichier *signalr.js* de *node_modules/@aspnet/signalr/dist/browser* vers le nouveau dossier *wwwroot/lib/signalr*.

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

* Remplacez le contenu de *Pages\Index.cshtml* par ce qui suit :

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
