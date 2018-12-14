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
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="45f04-103">Tutoriel : Bien démarrer avec ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="45f04-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="45f04-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="45f04-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="45f04-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="45f04-106">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="45f04-106">Create a web project.</span></span>
> * <span data-ttu-id="45f04-107">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="45f04-108">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="45f04-109">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="45f04-110">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="45f04-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="45f04-111">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="45f04-111">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="45f04-113">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="45f04-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="45f04-114">Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="45f04-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="45f04-115">Si vous avez quelques minutes pour essayer un exercice de recherche de 7 différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="45f04-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>


[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="45f04-116">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="45f04-116">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="45f04-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45f04-117">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="45f04-118">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="45f04-118">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="45f04-119">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="45f04-119">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="45f04-120">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="45f04-120">Name the project *SignalRChat*.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="45f04-122">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="45f04-122">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="45f04-123">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.2**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="45f04-123">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="45f04-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="45f04-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="45f04-126">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="45f04-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="45f04-127">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="45f04-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="45f04-128">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="45f04-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="45f04-129">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="45f04-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="45f04-130">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="45f04-130">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="45f04-131">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="45f04-131">Select **Next**.</span></span>

* <span data-ttu-id="45f04-132">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="45f04-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="45f04-133">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="45f04-133">Add the SignalR client library</span></span>

<span data-ttu-id="45f04-134">La bibliothèque de serveur SignalR est incluse dans le métapackage `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="45f04-134">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="45f04-135">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="45f04-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="45f04-136">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="45f04-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="45f04-137">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="45f04-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="45f04-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45f04-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="45f04-139">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="45f04-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="45f04-140">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="45f04-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="45f04-141">Pour **Bibliothèque**, entrez `@aspnet/signalr@1`, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="45f04-141">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/libman1.png)

* <span data-ttu-id="45f04-143">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="45f04-143">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="45f04-144">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/*, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="45f04-144">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/libman2.png)

  <span data-ttu-id="45f04-146">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="45f04-146">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="45f04-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="45f04-147">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="45f04-148">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="45f04-148">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="45f04-149">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="45f04-149">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="45f04-150">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="45f04-150">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="45f04-151">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="45f04-151">The parameters specify the following options:</span></span>
  * <span data-ttu-id="45f04-152">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="45f04-152">Use the unpkg provider.</span></span>
  * <span data-ttu-id="45f04-153">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="45f04-153">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="45f04-154">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="45f04-154">Copy only the specified files.</span></span>

  <span data-ttu-id="45f04-155">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="45f04-155">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="45f04-156">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="45f04-156">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="45f04-157">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="45f04-157">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="45f04-158">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="45f04-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="45f04-159">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="45f04-159">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="45f04-160">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="45f04-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="45f04-161">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="45f04-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="45f04-162">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="45f04-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="45f04-163">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="45f04-163">Copy only the specified files.</span></span>

  <span data-ttu-id="45f04-164">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="45f04-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="45f04-165">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="45f04-165">Create a SignalR hub</span></span>

<span data-ttu-id="45f04-166">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="45f04-166">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="45f04-167">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="45f04-167">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="45f04-168">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="45f04-168">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="45f04-169">La classe `ChatHub` hérite la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="45f04-169">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="45f04-170">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="45f04-170">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="45f04-171">La méthode `SendMessage` peut être appelée par n’importe quel client connecté.</span><span class="sxs-lookup"><span data-stu-id="45f04-171">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="45f04-172">Elle envoie le message reçu à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="45f04-172">It sends the received message to all clients.</span></span> <span data-ttu-id="45f04-173">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="45f04-173">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="45f04-174">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="45f04-174">Configure SignalR</span></span>

<span data-ttu-id="45f04-175">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-175">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="45f04-176">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="45f04-176">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="45f04-177">Ces modifications ajoutent SignalR au système d’injection de dépendances ASP.NET Core et au pipeline de middleware.</span><span class="sxs-lookup"><span data-stu-id="45f04-177">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="45f04-178">Ajouter le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="45f04-178">Add SignalR client code</span></span>

* <span data-ttu-id="45f04-179">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="45f04-179">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="45f04-180">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="45f04-180">The preceding code:</span></span>

  * <span data-ttu-id="45f04-181">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="45f04-181">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="45f04-182">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-182">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="45f04-183">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="45f04-183">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="45f04-184">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="45f04-184">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="45f04-185">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="45f04-185">The preceding code:</span></span>

  * <span data-ttu-id="45f04-186">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="45f04-186">Creates and starts a connection.</span></span>
  * <span data-ttu-id="45f04-187">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="45f04-187">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="45f04-188">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="45f04-188">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="45f04-189">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="45f04-189">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="45f04-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45f04-190">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="45f04-191">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="45f04-191">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="45f04-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="45f04-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="45f04-193">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="45f04-193">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="45f04-194">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="45f04-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="45f04-195">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="45f04-195">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="45f04-196">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="45f04-196">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="45f04-197">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.</span><span class="sxs-lookup"><span data-stu-id="45f04-197">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="45f04-198">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="45f04-198">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="45f04-200">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="45f04-200">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="45f04-201">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="45f04-201">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="45f04-202">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="45f04-202">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="45f04-203">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="45f04-203">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="45f04-204">![Erreur de fichier SignalR.js introuvable](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="45f04-204">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="45f04-205">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="45f04-205">Next steps</span></span>

<span data-ttu-id="45f04-206">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="45f04-206">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="45f04-207">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="45f04-207">Create a web app project.</span></span>
> * <span data-ttu-id="45f04-208">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-208">Add the SignalR client library.</span></span>
> * <span data-ttu-id="45f04-209">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-209">Create a SignalR hub.</span></span>
> * <span data-ttu-id="45f04-210">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="45f04-210">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="45f04-211">Ajouter le code qui utilise le hub pour envoyer des messages de n’importe quel client à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="45f04-211">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="45f04-212">Pour en savoir plus sur SignalR, consultez :</span><span class="sxs-lookup"><span data-stu-id="45f04-212">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="45f04-213">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="45f04-213">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
