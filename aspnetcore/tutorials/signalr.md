---
title: Bien démarrer avec ASP.NET Core SignalR
author: bradygaster
description: Dans ce tutoriel, vous créez une application de conversation qui utilise ASP.NET Core SignalR.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: ba36f75dfa40012df3140cdcbf8a7732fb0709fe
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264539"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="ee6f5-103">Tutoriel : Bien démarrer avec ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ee6f5-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="ee6f5-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="ee6f5-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee6f5-106">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-106">Create a web project.</span></span>
> * <span data-ttu-id="ee6f5-107">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="ee6f5-108">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="ee6f5-109">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="ee6f5-110">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="ee6f5-111">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-111">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="ee6f5-113">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ee6f5-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="ee6f5-114">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="ee6f5-114">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee6f5-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee6f5-115">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="ee6f5-116">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-116">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="ee6f5-117">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-117">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ee6f5-118">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-118">Name the project *SignalRChat*.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="ee6f5-120">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-120">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="ee6f5-121">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.2**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-121">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee6f5-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ee6f5-123">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="ee6f5-124">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-124">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="ee6f5-125">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-125">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee6f5-126">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ee6f5-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ee6f5-127">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-127">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="ee6f5-128">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="ee6f5-128">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="ee6f5-129">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-129">Select **Next**.</span></span>

* <span data-ttu-id="ee6f5-130">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-130">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="ee6f5-131">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="ee6f5-131">Add the SignalR client library</span></span>

<span data-ttu-id="ee6f5-132">La bibliothèque de serveur SignalR est incluse dans le métapackage `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-132">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="ee6f5-133">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-133">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="ee6f5-134">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-134">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="ee6f5-135">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-135">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee6f5-136">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee6f5-136">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="ee6f5-137">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-137">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="ee6f5-138">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-138">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="ee6f5-139">Pour **Bibliothèque**, entrez `@aspnet/signalr@1`, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-139">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/libman1.png)

* <span data-ttu-id="ee6f5-141">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-141">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="ee6f5-142">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/*, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-142">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/libman2.png)

  <span data-ttu-id="ee6f5-144">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-144">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee6f5-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ee6f5-145">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="ee6f5-146">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-146">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="ee6f5-147">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-147">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="ee6f5-148">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-148">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="ee6f5-149">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-149">The parameters specify the following options:</span></span>
  * <span data-ttu-id="ee6f5-150">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-150">Use the unpkg provider.</span></span>
  * <span data-ttu-id="ee6f5-151">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-151">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="ee6f5-152">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-152">Copy only the specified files.</span></span>

  <span data-ttu-id="ee6f5-153">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-153">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee6f5-154">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ee6f5-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ee6f5-155">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-155">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="ee6f5-156">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="ee6f5-156">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="ee6f5-157">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-157">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="ee6f5-158">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-158">The parameters specify the following options:</span></span>
  * <span data-ttu-id="ee6f5-159">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-159">Use the unpkg provider.</span></span>
  * <span data-ttu-id="ee6f5-160">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-160">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="ee6f5-161">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-161">Copy only the specified files.</span></span>

  <span data-ttu-id="ee6f5-162">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-162">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="ee6f5-163">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="ee6f5-163">Create a SignalR hub</span></span>

<span data-ttu-id="ee6f5-164">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-164">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="ee6f5-165">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-165">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="ee6f5-166">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-166">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="ee6f5-167">La classe `ChatHub` hérite la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-167">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="ee6f5-168">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-168">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="ee6f5-169">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-169">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="ee6f5-170">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-170">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="ee6f5-171">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-171">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="ee6f5-172">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="ee6f5-172">Configure SignalR</span></span>

<span data-ttu-id="ee6f5-173">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-173">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="ee6f5-174">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-174">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="ee6f5-175">Ces modifications ajoutent SignalR au système d’injection de dépendances ASP.NET Core et au pipeline de middleware.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-175">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="ee6f5-176">Ajouter le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="ee6f5-176">Add SignalR client code</span></span>

* <span data-ttu-id="ee6f5-177">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-177">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="ee6f5-178">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-178">The preceding code:</span></span>

  * <span data-ttu-id="ee6f5-179">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-179">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="ee6f5-180">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-180">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="ee6f5-181">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-181">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="ee6f5-182">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-182">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="ee6f5-183">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-183">The preceding code:</span></span>

  * <span data-ttu-id="ee6f5-184">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-184">Creates and starts a connection.</span></span>
  * <span data-ttu-id="ee6f5-185">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-185">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="ee6f5-186">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-186">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="ee6f5-187">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="ee6f5-187">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee6f5-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee6f5-188">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ee6f5-189">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee6f5-190">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ee6f5-190">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ee6f5-191">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-191">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee6f5-192">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ee6f5-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ee6f5-193">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-193">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="ee6f5-194">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-194">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="ee6f5-195">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-195">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="ee6f5-196">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-196">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="ee6f5-198">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-198">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="ee6f5-199">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-199">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="ee6f5-200">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-200">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="ee6f5-201">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-201">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="ee6f5-202">![Erreur de fichier SignalR.js introuvable](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="ee6f5-202">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee6f5-203">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ee6f5-203">Next steps</span></span>

<span data-ttu-id="ee6f5-204">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee6f5-205">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-205">Create a web app project.</span></span>
> * <span data-ttu-id="ee6f5-206">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-206">Add the SignalR client library.</span></span>
> * <span data-ttu-id="ee6f5-207">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-207">Create a SignalR hub.</span></span>
> * <span data-ttu-id="ee6f5-208">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-208">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="ee6f5-209">Ajouter le code qui utilise le hub pour envoyer des messages de n’importe quel client à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="ee6f5-209">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="ee6f5-210">Pour en savoir plus sur SignalR, consultez :</span><span class="sxs-lookup"><span data-stu-id="ee6f5-210">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ee6f5-211">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ee6f5-211">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
